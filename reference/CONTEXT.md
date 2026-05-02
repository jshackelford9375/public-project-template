# Reference

## What happens here
Working-but-rough snippets, vendor docs, cheat sheets, and how-to guides. Treat everything in this folder as **examples to learn from, not code to ship**. Artifacts here are kept around because they show one workable approach to a problem, not because they're the right approach.

## Current contents
- `folder-organization-guide.md` — the three-layer (map / rooms / skills) workspace pattern this repo is built on. Read this before reorganizing folders or creating a new room archetype.
- `resource-index_v2.md` — curated repos, MCP servers, SDKs, and tools for the Microsoft / Azure / PowerShell / Terraform / IAM / SaaS / security stack. Tag-frontmattered for AI lookup.
- `ai-capabilities-field-manual_v2.md` — the ten AI capabilities available in this workspace via GitHub Copilot in VS Code (workspace map, skills, instructions files, MCP, memory, subagents, prompt files, terminal/edit, notebooks, model selection). Decision sequence and quick-reference table at the bottom.

## How to use these examples
1. Read the existing artifact to understand the pattern and the failure modes.
2. Extract the **data contract** (input parameters, output shape, file naming) and document it in the relevant project's `planning/` as a spec.
3. Build the new implementation in the appropriate project room from a clean spec, applying the rules in that room's `CONTEXT.md`.
4. Do **not** copy the artifact wholesale. Specifically: never carry over hard-coded credentials, hard-coded paths, or mixed-concern functions.

## What good looks like
- Each file has a one-line comment / header explaining what it demonstrates and what its known shortcomings are.
- When a clean replacement lands in a project room, the reference artifact gets a `_superseded` suffix and a pointer to the replacement.

## What to avoid
- Treating reference as production code.
- Editing reference artifacts to fix bugs. If it's worth fixing, it's worth rebuilding in the project.
- Promoting an artifact because "it works" — working is not the bar; the rules in the destination room's `CONTEXT.md` are.
