# Runbooks

## What happens here
Operational procedures - how to deploy, recover, restart, or troubleshoot real systems. One file per procedure.

## File naming
`<verb>-<thing>_runbook.md` — e.g. `restart-arc-agent_runbook.md`, `recover-failed-vm_runbook.md`, `rotate-portal-secret_runbook.md`.

## Required sections per runbook
1. **When to use** — symptoms or trigger conditions
2. **Prerequisites** — access, tools, who to notify
3. **Steps** — numbered, copy-pasteable commands, with expected output
4. **Verification** — how to confirm it worked
5. **Rollback** — what to do if it didn't
6. **Last verified** — date + initials of last person who ran it cleanly

## What good looks like
- A teammate at 2 a.m. can follow it without calling you.
- Commands are exact, with placeholders clearly marked (`<vm-name>`).
- Failure modes are listed, not glossed over.

## What to avoid
- Runbooks that say "check the dashboard" without naming the dashboard.
- Procedures that drift — if you find yourself saying "ignore step 3, it's outdated," fix the runbook then and there.
