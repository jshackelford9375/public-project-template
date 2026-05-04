# Reference

## What happens here
Working-but-rough snippets, vendor docs, cheat sheets, and how-to guides. Treat everything in this folder as **examples to learn from, not code to ship**. Artifacts here are kept around because they show one workable approach to a problem, not because they're the right approach.

## Current contents
_Empty by default._ Add project-specific reference material here as the project takes shape: vendor docs you depend on, cheat sheets, configuration examples, working snippets too rough to ship.

## Where to look first (external)
For things that change too fast to ship as a frozen file:
- **GitHub Copilot agent mode capabilities** — [code.visualstudio.com/docs/copilot](https://code.visualstudio.com/docs/copilot)
- **Claude Code CLI capabilities** — [code.claude.com/docs](https://code.claude.com/docs/)
- **Tool, library, or MCP server lookup** — official registries / vendor docs / [github.com/modelcontextprotocol](https://github.com/modelcontextprotocol)

If a project starts referring to the same external doc repeatedly, save the relevant excerpt here with a date stamp and a link to the source.

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
