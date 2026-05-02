# Project workspace template

A standalone starter for a single project. Copy this whole repo to a new folder, run the INIT prompt, and it becomes the project. Push it to a new git remote and you're done.

## Model

One repo per project. Everything the project needs - planning, rooms, runbooks, reference, skills, agents - lives in the same repo. No multi-root, no sibling-folder setup, no shared knowledge base to mount. If you build a skill or agent worth reusing, copy it into the next project by hand when you start it.

## How to use it

### Option A - AI-assisted (preferred)
1. Copy this whole repo to a new folder (e.g. `C:\repos\<new-project>`). Easiest way: `git clone` this template, then `Remove-Item .git -Recurse -Force` so you start clean.
2. Open the new folder in VS Code.
3. Paste the contents of `_template/INIT-PROMPT.md` into a chat.
4. Answer the questions. The agent fills placeholders, prunes unused rooms, moves the skeleton to the repo root, and deletes `_template/`.
5. `git init`, add a remote, push.

### Option B - Manual
1. Copy this repo to a new folder.
2. Open every `*.template` under `_template/skeleton/` (recurse), replace placeholders, save without `.template`.
3. Pick rooms from `_template/ROOMS.md`. Keep what you need, delete the rest.
4. Move the contents of `_template/skeleton/` up to the repo root, overwriting `CLAUDE.md`, `README.md`, and `.gitignore`. `runbooks/`, `reference/`, `.claude/`, and `.gitattributes` stay as-is.
5. Rename `workspace.code-workspace` to `<slug>.code-workspace`.
6. Append stack-specific ignores to the new `.gitignore` below the `=== STACK-SPECIFIC ===` marker.
7. Delete `_template/`.
8. `git init` and `git remote add origin <url>`.

## What's in this folder

| File | Purpose |
|------|---------|
| `README.md` | This file. |
| `INIT-PROMPT.md` | Paste-into-chat prompt to bootstrap a new workspace. |
| `ROOMS.md` | Catalog of room archetypes. |
| `skeleton/` | The full project workspace skeleton. Gets moved to repo root by INIT. |

## What survives outside `_template/`

The starter `runbooks/`, `reference/`, and `.claude/` folders at the repo root come along for the ride. They become part of the new project. Edit them, delete what you don't need, add project-specific skills and agents in `.claude/`.

## Rules carried into every new project

- One screen max for the project's `CLAUDE.md`. Push detail into room `CONTEXT.md` files.
- Each room has a "Promotion candidates" table. When an item appears twice, formalize it as a skill, agent, or shared module within this project.
- File naming: `description_status.extension`. Code uses language idiom. Docs use `kebab-case`.
- Scripts/IaC: parameters only - no hard-coded creds, IDs, or paths.
- Customization files (skills, agents) go in `.claude/`, never `.github/` or `.agents/`.
