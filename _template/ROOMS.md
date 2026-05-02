# Room catalog

Common room archetypes. Pick the minimum set that fits the project's goals. Each room has a one-line purpose and a CONTEXT template under `_template/skeleton/<room>/CONTEXT.md.template`.

## Always include

- **`planning/`** - Specs, ADRs, diagrams. The "thinking" room. No code here.

## Pick what fits

| Room | Use when the project... | Typical artifacts |
|------|--------------------------|-------------------|
| `automation/` | Drives infrastructure, ops, or pipelines via scripts + IaC | PowerShell, Bash, Python, Terraform/OpenTofu |
| `engineering/` | Builds an application or library (general-purpose code room) | source, tests, build config |
| `lab/` | Needs a sandbox, test fleet, hardware, or VM environment | provisioning scripts, fleet inventories, ISO/image manifests |
| `data/` | Ingests, transforms, or analyzes datasets | parsers, ETL, schemas, sample datasets |
| `reporting/` | Produces deliverable reports as the public output | report generators, templates, samples, rendered output |
| `portal/` or `app/` | Has a user-facing UI (web/mobile/desktop) | frontend, backend API, auth config, deploy pointers |
| `research/` | Reads, annotates, and synthesizes external sources | papers, notes, interviews, themes, drafts |
| `content/` | Produces written or media content as the deliverable | drafts, scripts, briefs, final pieces |
| `integrations/` | Wires together third-party systems / APIs | connectors, webhook handlers, mapping configs |

## Cross-cutting starter folders (already in repo root)

- **`runbooks/`** - Operational procedures, one file per procedure. Keep what's relevant, delete the rest.
- **`reference/`** - Examples, snippets, vendor docs. Examples-only; never copy wholesale into rooms.

These come with the template. They're project content now - prune or extend as needed.

## Naming the room

Pick a short, honest noun. If you find yourself stacking concerns in one room (e.g. "automation-and-reporting"), split them. If two rooms keep referencing each other constantly, consider whether they're really one room.

## Splitting and merging later

It is fine to start with fewer rooms and split later. Trigger to split: a room's `CONTEXT.md` grows beyond ~80 lines, or its file naming conventions start contradicting themselves. Trigger to merge: two rooms share most of their CONTEXT and you keep editing both together.
