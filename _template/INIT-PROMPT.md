# INIT prompt - turn this template into a project workspace

Paste this whole file into a chat **after copying this repo to a new folder**. The agent will fill placeholders, prune rooms you don't need, promote the skeleton to the repo root, and delete `_template/`. Result: a standalone project ready for `git init` and a new remote.

---

I copied this template into a new folder and want to turn it into a real project workspace. Run the flow below.

## Step 1 - Ask the minimum

Ask me only these three questions and wait for answers:

1. **Project name** (e.g. "Acme Reporting Pipeline").
2. **One-liner** - what is this project, in one sentence?
3. **Goals** - what does "done" look like? 1-3 bullets is fine.

Don't ask for slug, destination path, or anything else yet.

## Step 2 - Propose the rest

Based on my answers, **propose** the following as a single message and ask me to confirm or edit. Make educated guesses; don't ask me to fill blanks.

- **Slug:** kebab-case of the project name (lowercase, `[a-z0-9-]`, collapsed dashes).
- **Tech stack:** infer from the one-liner and goals. Lean toward what this template assumes by default: Windows + PowerShell, Azure if cloud is implied, Terraform/OpenTofu for IaC, Python or Node for code, React + Tailwind for UI. State each choice with a one-line rationale. Mark anything genuinely unclear as "TBD - decide in first ADR."
- **Rooms:** pick the minimum set from `_template/ROOMS.md` that fits. `planning/` is always included. For each pick, give a one-line reason. Don't include rooms "in case."
- **Deliverables:** what the project produces (one or two items).
- **Commands:** if the stack implies obvious commands (`pwsh`, `terraform plan`, `npm test`, `pytest`), propose 3-6 rows for the Commands table. If none apply, write `_None yet._`
- **Avoid list:** 2-4 things this project should explicitly not do, derived from the stack and one-liner. Concrete examples beat abstractions: "No Moment.js - use date-fns" beats "avoid deprecated libraries." "No live database deps in reports" beats "keep reports portable."
- **Owner:** infer from `git config user.name` if available, otherwise leave as `TBD`.

Wait for me to accept or edit before doing anything else.

## Step 3 - Build it

Once I confirm the proposal, do all of this in order:

1. **Fill placeholders.** For every `*.template` file in `_template/skeleton/` (recurse into subfolders), substitute placeholders (see table below) and save without the `.template` suffix. Remove room folders I didn't pick. Update `{{ROOMS_TREE}}`, `{{ROUTING_ROWS}}`, and `{{ROOMS_TABLE}}` to only include kept rooms. Rename `workspace.code-workspace` to `<slug>.code-workspace`.
2. **Promote skeleton to repo root.** Move every filled file/folder from `_template/skeleton/` up to the repo root, overwriting any same-named files (the existing root `CLAUDE.md` and `.gitignore` get replaced this way - that is intentional). Leave `runbooks/`, `reference/`, `.claude/`, and `.gitattributes` untouched - they survive as project content. Then append stack-specific entries to the new `.gitignore` below the `=== STACK-SPECIFIC ===` marker (PowerShell module paths, `.terraform/`, `node_modules/`, `__pycache__/`, etc., based on the chosen stack). Don't regenerate `.gitignore` from scratch - only append below the marker. Finally, delete `_template/` entirely.
3. **Verify tooling.** Run `git --version` and `pwsh --version` plus stack-dependent checks (`az --version`, `terraform --version` / `tofu --version`, `node --version`, `python --version`). Report anything missing - don't silently continue.
4. **Print a checklist.** Show what was created, what was skipped, and the next 3 actions. End with a copy-pastable command:
   ```powershell
   code "<absolute-path-to-the-.code-workspace-file>"
   ```

## Placeholders

### Project-wide (always required)
| Placeholder | Meaning |
|-------------|---------|
| `{{PROJECT_NAME}}` | Human-readable name |
| `{{PROJECT_SLUG}}` | kebab-case folder/workspace name |
| `{{PROJECT_ONE_LINER}}` | One-sentence description |
| `{{PROJECT_LONG_DESCRIPTION}}` | Optional longer paragraph (omit if not provided) |
| `{{SUCCESS_CRITERIA}}` | Bullet list of measurable outcomes |
| `{{TECH_STACK}}` | Markdown bullet list |
| `{{COMMANDS_TABLE}}` | Markdown table: `\| Action \| Command \|`. If none, write `_None yet._` |
| `{{ROOMS_TABLE}}` | Markdown table of kept rooms with purpose |
| `{{ROOMS_TREE}}` | Tree fragment of kept rooms (folder structure block) |
| `{{ROUTING_ROWS}}` | Routing-table rows for kept rooms (task router block) |
| `{{PROJECT_SKILLS_TABLE}}` | Skill rows: `\| skill-name \| when invoked \|`. If none, write `\| _None._ \| - \|` |
| `{{AVOID}}` | Bullet list of project-specific things to avoid (concrete examples, not abstractions) |
| `{{OWNER}}` | Person responsible |
| `{{DATE}}` | Today's date (YYYY-MM-DD) |

### Room-specific (only required if that room is kept)
Fill these from the confirmed proposal. If the room is being pruned, the placeholder vanishes with the file.

| Placeholder | Room | Meaning |
|-------------|------|---------|
| `{{AUTOMATION_LANGUAGE}}` | `automation/` | Primary scripting language (e.g. `PowerShell 7`, `Python 3.12`, `Bash`) |
| `{{ENGINEERING_LANGUAGE}}` | `engineering/` | Primary application language (e.g. `Python 3.12`, `TypeScript`, `C#`) |
| `{{PORTAL_PURPOSE}}` | `portal/` | One-line user-facing purpose (e.g. "internal status dashboard for the lab fleet") |
| `{{PORTAL_HOSTING}}` | `portal/` | Where it runs (e.g. `Azure Static Web Apps`, `Azure App Service`) |
| `{{PORTAL_AUTH}}` | `portal/` | Identity provider + scope (e.g. `Entra ID, internal tenant only`) |
| `{{PORTAL_FRONTEND}}` | `portal/` | Frontend stack (e.g. `React + Vite + Tailwind`) |
| `{{PORTAL_BACKEND}}` | `portal/` | Backend stack (e.g. `Azure Functions (TypeScript)`) |
| `{{PORTAL_STORAGE}}` | `portal/` | Persistence (e.g. `Cosmos DB`, `Azure Table Storage`, `none`) |
| `{{PORTAL_IAC}}` | `portal/` | IaC tool + module location (e.g. `OpenTofu, automation/iac/portal/`) |
| `{{REPORT_REQUIRED_FIELDS}}` | `reporting/` | Bullet list of required fields per report record |

## Hard rules

- Don't add features, scripts, or code beyond the scaffold. Only `CLAUDE.md`, `README.md`, room `CONTEXT.md` files, `.code-workspace`, and `.gitignore` plus the folder structure.
- Don't invent placeholders. If you need one, ask first.
- Don't run `git init` or any git commands. I'll do that after I review.
- The starter `runbooks/`, `reference/`, and `.claude/` folders stay - they're project content now.
