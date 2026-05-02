# project_template

## What this is
A standalone project starter. To begin a new project: copy this whole repo to a new folder, paste `_template/INIT-PROMPT.md` into a chat, push to a new git remote.

If you are reading this in a freshly copied template (before INIT runs), help the user follow `_template/INIT-PROMPT.md`. After INIT runs, the filled `_template/skeleton/CLAUDE.md.template` overwrites this file.

## Folder structure
```
project_template/
├── CLAUDE.md                       # this file (replaced by the project's CLAUDE.md after INIT runs)
├── _template/                      # bootstrap files (deleted after INIT runs)
├── runbooks/                       # starter operational procedures (kept)
├── reference/                      # starter examples and vendor docs (kept)
└── .claude/
    ├── skills/                     # starter skills (kept)
    └── agents/                     # starter subagents (kept)
```

## Token rules for this template
- This `CLAUDE.md` is always loaded. Keep it short. Detail goes in `_template/README.md` or `_template/INIT-PROMPT.md`.
- `reference/` is examples-on-demand. Don't load files from it unless the user asks.
- `_template/skeleton/` is data, not instructions. Read it when running INIT, otherwise skip.

## Naming conventions
Pattern: `description_status.extension`. Markdown and HTML use `kebab-case`.

## Rules
- ASCII only in `.ps1` / `.psm1` / `.psd1`. PS 5.1 in CI mangles non-ASCII.
- Customization files always go in `.claude/`. Use Claude-compatible frontmatter (`name`, `description`, `tools`).
- When this repo is opened directly as the template, **never `git add` / `git commit` / `git push`** - the maintainer pushes template changes manually. Once copied into a new project, normal git rules apply.
- `reference/` is read-only examples. Never edit files there to fix a bug - rebuild cleanly in a project context.

## Skills (`.claude/skills/`)
| Skill | Activates on |
|-------|--------------|
| `planning-docs` | Authoring or reviewing ADRs and technical specs using the templates in `_template/skeleton/planning/templates/` |

## Subagents (`.claude/agents/`)
| Agent | Purpose |
|-------|---------|
| `powershell-reviewer` | Read-only review of PowerShell scripts/modules |
| `terraform-reviewer` | Read-only review of Terraform/OpenTofu code |
| `Explore` (built-in) | Read-only codebase Q&A; `quick` / `medium` / `thorough` |

## Bootstrapping a new project
1. `git clone <this-template-url> <new-project>`, then `Remove-Item .git -Recurse -Force` inside it.
2. Open the new folder in VS Code.
3. Paste `_template/INIT-PROMPT.md` into a chat. Answer the questions.
4. `git init`, add a remote, push.
