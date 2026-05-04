# ADR-001: Use OpenTofu instead of Terraform for IaC

**Status:** accepted
**Date:** 2026-05-04
**Deciders:** j.morgan

> This is a worked example to show what good looks like. Delete or replace it once the project has its own first ADR.

## Context
We need an IaC tool for the Azure resources this project provisions (resource groups, storage, function apps). HashiCorp relicensed Terraform under the BUSL in 2023, which raises licensing questions for any future commercial use of modules we publish or borrow. The OpenTofu fork is BSL-free, drop-in compatible with our existing `.tf` files, and has a stable release cadence. The team already has Terraform experience, so switching cost is low.

## Decision
We will use OpenTofu (`tofu`) as the IaC tool for all infrastructure in this project, because it preserves the Terraform workflow we know without the licensing ambiguity.

## Consequences

**Positive:**
- No BUSL concerns for modules we author or vendor.
- Existing `.tf` syntax, providers, and state files work unchanged.
- CI image is smaller — single binary, no license check step.

**Negative / trade-offs:**
- Some third-party modules pin a `terraform` binary by name in their docs; we'll occasionally need to translate examples.
- Provider releases for OpenTofu can lag a few days behind Terraform.
- Tooling (VS Code extensions, pre-commit hooks) sometimes assumes `terraform` is on PATH; we maintain a `tofu` → `terraform` shim where needed.

**Follow-ups required:**
- Add `tofu` install step to the automation runbook.
- Update `automation/CONTEXT.md` to reference `tofu` commands, not `terraform`.

## Alternatives considered
| Option | Why not |
|--------|---------|
| Terraform (HashiCorp) | BUSL licensing creates downstream uncertainty for any module we'd want to share or open-source. |
| Pulumi | Different mental model (general-purpose languages); the team's existing Terraform fluency wouldn't transfer. |
| Bicep | Azure-only; we want the option to manage non-Azure resources (DNS, GitHub) from the same tool. |
