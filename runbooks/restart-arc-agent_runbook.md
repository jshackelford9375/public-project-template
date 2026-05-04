# Restart the Arc agent on a managed Windows VM

> This is a worked example showing the runbook format. Delete or replace it once the project has its own first runbook.

## When to use
- An Arc-managed Windows VM shows `Disconnected` in the Azure Portal for more than 10 minutes.
- `Get-Service himds` reports `Stopped` on the VM and `Start-Service himds` fails or hangs.
- A scheduled extension run did not fire and the VM is otherwise reachable via RDP or Bastion.

Do **not** use if:
- The VM itself is unreachable. Use a VM-recovery runbook for that.
- The Arc agent is healthy but a specific extension is failing. Open the extension blade in the portal first.

## Prerequisites
- RBAC: `Azure Connected Machine Onboarding` and `Azure Connected Machine Resource Administrator` on the VM's resource group.
- Tools on the operator's machine: `pwsh` 7+, the `Az.ConnectedMachine` module, an AAD account scoped to the tenant.
- VM access: RDP via Bastion, or Run-Command from the portal / CLI.
- Notify: post in the `#ops-on-call` channel before running. This restart drops any in-flight extension runs.

## Steps

1. **Confirm the disconnection** from the operator's machine:
   ```powershell
   az connectedmachine show `
     --resource-group <rg-name> `
     --name <vm-name> `
     --query "status"
   ```
   Expected output: `"Disconnected"`. If `"Connected"`, stop here - the agent is healthy.

2. **Inspect the agent service on the VM** via Run-Command:
   ```powershell
   az connectedmachine run-command create `
     --resource-group <rg-name> `
     --machine-name <vm-name> `
     --run-command-name diagnose-himds `
     --location <region> `
     --script "Get-Service himds | Select-Object Status, StartType | ConvertTo-Json"
   ```
   Expected output: a JSON object with `"Status": "Stopped"` or `"Status": "StartPending"`. Capture this in the incident record.

3. **Restart the agent service**:
   ```powershell
   az connectedmachine run-command create `
     --resource-group <rg-name> `
     --machine-name <vm-name> `
     --run-command-name restart-himds `
     --location <region> `
     --script "Restart-Service himds -Force; Start-Sleep -Seconds 30; Get-Service himds | Select-Object Status | ConvertTo-Json"
   ```
   Expected output: `"Status": "Running"`.

4. **Wait 5 minutes** for the agent to re-establish the connection. Don't skip - the heartbeat runs on a 60-second cycle and the portal state lags by a couple of cycles.

## Verification
Re-run the query from step 1. Expected: `"Connected"`.

Then check that no extensions are stuck in `Failed`:
```powershell
az connectedmachine extension list `
  --resource-group <rg-name> `
  --machine-name <vm-name> `
  --query "[?provisioningState=='Failed'].name"
```
Expected: an empty array `[]`. If any extensions still report `Failed`, retry them individually using each extension's own runbook.

## Rollback
There is no real rollback for a service restart. If the agent is now `Connected` but a specific workload is broken (Defender extension, MMA collector, etc.), open the extension blade in the portal and trigger a reinstall.

If the agent stays `Disconnected` after 15 minutes:
- Check the VM's outbound network path to `*.guestconfiguration.azure.com` and `*.his.arc.azure.com`.
- If outbound is blocked, this is a network problem, not an agent problem - escalate to the network on-call.
- If outbound is fine, file a support ticket with the diagnostic output captured in step 2.

## Last verified
2026-04-22 - j.morgan
