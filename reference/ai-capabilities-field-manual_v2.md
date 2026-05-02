---
title: AI Capabilities Field Manual (Copilot in VS Code)
description: Ten AI capabilities available in this workspace via GitHub Copilot in VS Code, with use/skip criteria and setup guidance. Use when deciding which capability fits a task — workspace map, skills, subagents, instructions/prompt files, MCP servers, memory, tools, terminal/edit, notebooks, or model selection.
tags: [reference, copilot, vscode, skills, subagents, mcp, memory, instructions, prompts, claude-code]
source: Adapted from Clief Notes Skills Field Manual v1.0 (March 2026); rewritten for this workspace's GitHub Copilot stack (May 2026)
audience: workspace-owner, skill-builders
last_reviewed: 2026-05-01
---

# AI Capabilities Field Manual

**Ten capabilities available in this workspace via GitHub Copilot in VS Code.**

What each one does. When to use it. When to skip it. How to set it up right.

## Read This First

This manual is for **GitHub Copilot in VS Code** with a Claude model selected in the Copilot model picker. It is *not* for Claude Code / Claude IDE — some details differ. Verified facts about this stack (April 2026 VS Code docs):

- **Custom agents work.** Copilot reads `.github/agents/`, `.claude/agents/`, and `.agents/agents/` interchangeably; Claude Code only reads `.claude/agents/`. This workspace standardizes on `.claude/agents/<name>.md` (Claude-compatible format) for portability across both tools.
- **Subagents run synchronously.** The main chat blocks while the subagent runs. There is no background/parallel execution.
- **Slash commands exist** in Copilot: `/init`, `/agents`, `/create-agent`, `/create-prompt`, `/create-instruction`, `/create-skill`, `/create-hook`.
- **Hooks** (shell commands at lifecycle events) and the **agent plugins marketplace** are real Copilot features (some preview).
- **Skills, memory, MCP, `.instructions.md` with `applyTo`, `.prompt.md`** all work — verified.

When suggesting a capability, check the [Stack section in `CLAUDE.md`](../CLAUDE.md) first. Don't assume a specific model version.

A prompt is a string of text you type into chat. A capability is a feature of the tool. The leverage is almost always in **picking the right capability**, not writing a better prompt. This manual is the picker.

Each capability follows the same five-field shape:

- **What it does.** One paragraph. No hype.
- **Use it when.** The situations where this saves real time.
- **Skip it when.** The situations where it adds friction or noise.
- **The approach.** How to set it up so it actually works.
- **Layer.** Where this sits in the 60/30/10 framework (60% deterministic code, 30% rules/templates, 10% AI judgment, plus infrastructure and foundation layers).

**The core principle.** The biggest lever in AI output quality is what context the model can see. Five of these capabilities control that context. The other five control output mode and execution. Pick the right pair for your task.

## Contents

1. [Context capabilities](#context-capabilities)
   - [1. Workspace map (CLAUDE.md / copilot-instructions.md)](#1-workspace-map-claudemd--copilot-instructionsmd)
   - [2. Skills (`.claude/skills/`)](#2-skills-claudeskills)
   - [3. Instructions files (`.instructions.md` with `applyTo`)](#3-instructions-files-instructionsmd-with-applyto)
   - [4. MCP servers](#4-mcp-servers)
   - [5. Memory (`/memories/`)](#5-memory-memories)
2. [Action capabilities](#action-capabilities)
   - [6. Subagents](#6-subagents)
   - [7. Prompt files (`.prompt.md`)](#7-prompt-files-promptmd)
   - [8. Terminal + file editing](#8-terminal--file-editing)
   - [9. Notebooks](#9-notebooks)
   - [10. Model and thinking-effort selection](#10-model-and-thinking-effort-selection)
3. [Putting it together](#putting-it-together)
4. [Quick reference](#quick-reference)

## Context capabilities

These five control what the agent knows when it processes your request. Highest-leverage tools in the stack.

### 1. Workspace map (CLAUDE.md / copilot-instructions.md)

**What it does.** Markdown files at the workspace root that auto-load into every chat. `CLAUDE.md` is the convention this workspace uses. `.github/copilot-instructions.md` is the VS Code Copilot equivalent (loaded automatically when present). Both act as a persistent system prompt that orients the agent: identity, rules, folder map, naming conventions, routing table, project list.

**Use it when:**

- You return to the same workspace repeatedly and want consistent behavior across sessions.
- You have rules that apply everywhere (encoding, secrets policy, push behavior, "no code without a spec," etc.).
- New projects or rooms are added and need to be discoverable without re-explaining.

**Skip it when:**

- The task is a one-off and the workspace has no shared structure to describe.
- You're scratch-padding in a folder that won't survive the day.

**The approach.**

Keep it to **one screen**. Detail belongs in per-room `CONTEXT.md` files, not the workspace map. The map's job is to point at the right room and the right skill, not to teach. The current `CLAUDE.md` in this workspace already follows this pattern: identity, rules, folder structure, projects, routing table, naming conventions, skills/subagents tables.

For the routing table, write each row like a trigger: "doing X → go to Y → read Z → invoke skill W." The agent uses this to pick the right files without scanning the whole tree.

**Layer.** Infrastructure. The map calibrates everything else.

### 2. Skills (`.claude/skills/`)

**What it does.** Folders containing a `SKILL.md` plus optional supporting files (templates, scripts, reference docs). The agent loads a skill on demand when its description matches the task. Each skill is self-contained and re-usable across projects. They work in Copilot in VS Code via the workspace's `.claude/skills/` directory.

The difference between a skill and a workspace map: the map is *who you are*; a skill is *how to do this specific thing*.

**Use it when:**

- You catch yourself typing the same instructions into multiple chats.
- A repeatable process should produce the same quality every time (brand styling, specific report formats, runbook templates, security review checklists).
- A standard or contract needs to be enforced (brand tokens, ASCII-only PowerShell, AVM module patterns).

**Skip it when:**

- The task happens once.
- The instructions fit in one prompt.
- You're still figuring out the right process — get it working manually first, then crystallize.

**The approach.**

Notice repetition first, then package. The structure:

```
.claude/skills/<skill-name>/
  SKILL.md          # Required. YAML frontmatter + markdown.
  REFERENCE.md      # Optional supplemental info.
  templates/        # Optional output templates.
  scripts/          # Optional code the agent can run.
```

The `description` field in YAML frontmatter is the trigger. Phrase it as a *use-when* sentence: "Author or review ADRs and technical specs using the project's planning templates." That's how the agent decides whether to activate.

This template ships one starter skill, `planning-docs`. Add a new skill when you've done the same thing twice and seen it deserve formalization (the "Promotion candidates" tables in each room are the hand-off point).

**Layer.** 30%. Skills are rules and templates; the agent's job is filling in the parts that need judgment.

### 3. Instructions files (`.instructions.md` with `applyTo`)

**What it does.** VS Code Copilot–specific. Markdown files (anywhere in the workspace, but typically `.github/instructions/`) with YAML frontmatter that includes an `applyTo` glob. The instructions auto-load into chat **only when the user is editing files matching the glob**. Lighter than a skill, scoped tighter than a workspace map.

**Use it when:**

- You have rules that apply to a specific file type or directory but not the whole workspace (e.g. "all .ps1 files use ASCII only," "files under `terraform/` follow the AVM module structure").
- You want context to load automatically based on what's open, not based on what was asked.

**Skip it when:**

- The rule is universal — put it in `CLAUDE.md` instead.
- The rule is a full repeatable process — make it a skill instead.

**The approach.**

```yaml
---
applyTo: "**/*.ps1,**/*.psm1,**/*.psd1"
---
# PowerShell rules
- ASCII only. PS 5.1 in CI mangles em-dashes and smart quotes.
- Use approved verbs. Run `Get-Verb` if unsure.
- Pester tests live next to the code they test.
- Verify with `powershell.exe -NoProfile -File <runner.ps1>` (PS 5.1) before pushing.
```

Keep these short. Long rules drift; one-screen rules survive.

**Layer.** 30%. Same as skills, but auto-targeted by file context instead of task description.

### 4. MCP servers

**What it does.** Model Context Protocol servers expose external systems (Azure, GitHub, Microsoft Graph, Microsoft Learn docs, filesystem, databases) as tools the agent can call. Configured in VS Code via `.vscode/mcp.json` or workspace settings. When connected, the agent reads real data and takes real actions instead of guessing.

**Use it when:**

- The agent needs your real Azure subscription state, not a description of it.
- You want it to read live Microsoft Learn docs (the `microsoftdocs/mcp` server) instead of training-data approximations.
- You're tired of being the human glue between the agent and external tools.

**Skip it when:**

- The data is small enough to paste in.
- The action is destructive and you haven't reviewed the server's permissions yet (start read-only; promote to write later).
- The external service has rate limits that will bottleneck the workflow.

**The approach.**

For this workspace's stack, the highest-value MCP servers are:

1. `Azure/azure-mcp` — Resource Graph, ARM, AKS, Storage, Key Vault.
2. `microsoftdocs/mcp` — live Microsoft Learn lookups. Critical because Azure/PowerShell docs change monthly.
3. `github/github-mcp-server` — issues, PRs, repo state.

Configure them in `.vscode/mcp.json` or the user-level VS Code MCP settings. Start each new connection with read-only credentials, validate behavior, then enable write access if needed.

For internal-only systems, you can build a custom MCP server. Keep secrets in a vault or env var, never in source.

**Layer.** Infrastructure. The action through a connector might be 60% (pull a report) or 10% (draft a remediation), but the connector itself is plumbing.

### 5. Memory (`/memories/`)

**What it does.** A persistent note system the agent reads and writes across sessions. In this workspace it's organized into three scopes:

- `/memories/` — user memory, loaded automatically into every conversation. Persists across all workspaces.
- `/memories/session/` — current-conversation only. Cleared when the session ends.
- `/memories/repo/` — repository-scoped facts. Created via Copilot, persists with the workspace.

**Use it when:**

- You hit the same gotcha twice and want to record it (PowerShell encoding traps, PSScriptAnalyzer differences between PS 5.1 and PS 7, Hyper-V vs Win32_Processor properties).
- You have stable preferences (push to origin/main automatically, never use em-dashes in PS files).
- A long task generates intermediate findings worth keeping for the rest of the session.

**Skip it when:**

- The fact is project-specific and durable — that belongs in a `CONTEXT.md` instead, where it lives with the project.
- The note is a one-off that won't apply again.
- You're tempted to dump a long doc into memory — use a skill or reference file.

**The approach.**

Keep entries **short**. User memory loads into every conversation, so brevity matters. Use bullet points, group by topic in separate files (`git-workflow.md`, `powershell-gotchas.md`, etc.). When a memory turns out wrong, edit or delete it — stale memory is worse than missing memory.

The right mental model: memory is for things that are true about *you* regardless of what you're working on. Project specifics go in project `CONTEXT.md`. Tenant/subscription IDs and other secrets never go in memory.

**Layer.** Infrastructure. Calibrates how the agent talks to you and which gotchas it pre-empts.

## Action capabilities

These five control how the agent does the work. Pick the right one and you avoid the most common mistake — asking for prose when you needed a file edit, or vice versa.

### 6. Subagents

**What it does.** Specialized agents with their own focused system prompt that you delegate bounded work to. The subagent runs, returns one report, and is done — stateless and not interactive.

**Important: subagents in Copilot run synchronously.** When you invoke one, the main chat waits for it to finish. There is no "kick it off and keep working" mode. Plan accordingly: subagents are good for bounded research/exploration where you'd block waiting for the answer anyway, not for long-running parallel work.

The built-in `Explore` subagent is fast, read-only, and good for codebase Q&A. Custom agents are defined as markdown files; Copilot supports two formats:

- **`.github/agents/<name>.agent.md`** — VS Code native format. Richer frontmatter: `tools`, `model`, `handoffs`, `agents` (allowed nested subagents), `user-invocable`, `disable-model-invocation`, `mcp-servers`, scoped `hooks`.
- **`.claude/agents/<name>.md`** — Claude-compatible format. Simpler frontmatter (`name`, `description`, `tools` as comma-separated string, `disallowedTools`). VS Code maps Claude tool names automatically. Use this if you want the same file to work in both Copilot and Claude Code.

User-scope agents go in `~/.copilot/agents/`. Run `Chat: New Custom Agent` or `/create-agent` to scaffold one with AI assistance.

**Use it when:**

- You'd otherwise chain many `grep` / `read_file` calls just to answer "where is X?" — use `Explore`.
- You want a dedicated reviewer with locked-down tools (read-only, no edits) — e.g. `powershell-reviewer`, `terraform-reviewer`.
- The task is bounded and stateless — you describe it fully in one prompt and want one report back.

**Skip it when:**

- The work is interactive or iterative — subagents return one message and can't be replied to.
- The task is small enough to handle in the main conversation.
- You want "start it and keep working." Copilot can't do that; the main chat blocks.
- The pattern is best as auto-activated rules — use a skill or `.instructions.md` instead.

**The approach.**

For exploration: invoke `Explore` with a clear question and a thoroughness level (`quick` / `medium` / `thorough`). Tell it exactly what you want returned.

For a custom reviewer (Claude-compatible format, works in both Copilot and Claude Code):

```yaml
---
name: powershell-reviewer
description: Read-only review of PowerShell scripts and modules. Reports findings; never edits.
tools: Read, Grep, Glob
---

# PowerShell reviewer

Review the supplied PowerShell file(s) and report:
- ASCII-only check (no em-dashes or smart quotes)
- Approved verbs (`Get-Verb`)
- ...
```

Keep each agent doing one thing. Don't write a "general reviewer" — write `powershell-reviewer` and `terraform-reviewer` separately so each can be opinionated. Save under `.claude/agents/<name>.md`.

**Layer.** Infrastructure. The subagent is plumbing; what it produces depends on the task.

### 7. Prompt files (`.prompt.md`)

**What it does.** VS Code Copilot–specific. Reusable prompt scripts stored as markdown files. Invokable on demand. Different from skills (skills are auto-activated by description match; prompts are explicitly run). Useful for parameterized one-shot operations.

**Use it when:**

- You have a multi-step prompt you re-run with small variations (e.g. "scaffold a new ADR with this number and topic," "generate a runbook from this incident summary").
- You want a teammate to be able to invoke a workflow without reading the prompt yourself.

**Skip it when:**

- The workflow is large enough to justify a skill (skills win for anything with templates or scripts).
- The prompt only runs once.

**The approach.**

Prompt files live in the user prompts folder (`%APPDATA%\Code\User\prompts` on Windows) or the workspace. Keep them short, parameterize with placeholders, and give them descriptive filenames so they show up in the picker.

For this workspace, the `_template/INIT-PROMPT.md` is a good example pattern — paste it into chat and it scaffolds a new project after asking the right questions.

**Layer.** 30%. Same family as skills, but invoked manually rather than auto-matched.

### 8. Terminal + file editing

**What it does.** The agent runs commands in your real terminal (`run_in_terminal`) and edits files in your real workspace (the `replace_string_in_file` / `create_file` family). Output and side effects are real. Errors are visible. Failed commands can be diagnosed and retried.

**Use it when:**

- The task is "do this" not "tell me about this." Refactor, run a script, build, deploy, install a module, scaffold a folder.
- You want the agent to verify its own work by running tests after a change.
- You need to perform reversible operations confidently.

**Skip it when:**

- The action is destructive and irreversible (drop tables, force-push, hard reset, `rm -rf`). Confirm first; don't let an agent guess.
- You only want a recommendation, not an execution.

**The approach.**

Tell the agent **what you want changed**, not what command to run. "Run PSScriptAnalyzer against `Build-LabVm.ps1` under PS 5.1 and show me any errors" is better than "execute `powershell.exe -File run-pssa.ps1`." The agent picks the right command and adapts when the first attempt fails.

For long-running commands (builds, servers, watchers), use background/async mode and let the agent come back when output is ready. Don't `Start-Sleep` to wait.

For file edits: prefer multi-edit operations when several changes go together. Read before you write. Verify after you write.

**Layer.** 60%. The terminal and file system are deterministic. The agent's contribution is choosing what to run; the result is as reliable as the command.

### 9. Notebooks

**What it does.** The agent can create, edit, and run Jupyter notebooks (`.ipynb`) directly in VS Code. Different from terminal because notebook state persists across cells and outputs are inline (charts, dataframes, error tracebacks).

**Use it when:**

- You're exploring data — Az PowerShell output, Microsoft Graph audit logs, KQL exports — and want to iterate on analysis.
- You want to capture the analysis itself as the deliverable, not just the answer.
- The task involves Python pandas / matplotlib / azure-sdk-for-python work.

**Skip it when:**

- A `.ps1` or `.py` script is the right artifact (notebooks are messy under version control).
- The work is one-off and a quick chat answer suffices.

**The approach.**

Tell the agent the dataset and the question. Let it pick whether to write a script or a notebook. For Azure / IAM data, it will often want to use `azure-identity` + `azure-mgmt-*` or `msgraph-sdk` from Python; that's fine. Confirm secrets come from env vars or DefaultAzureCredential, never inline.

**Layer.** 60%. Same as terminal — deterministic execution; the value is in choosing the right cells.

### 10. Model and thinking-effort selection

**What it does.** GitHub Copilot lets you pick the underlying model (Claude Opus, GPT-5, Gemini, etc.) and, on agents that support it, the reasoning effort. Different models have different strengths: Opus excels at long-context analysis and code, GPT-5 at structured outputs, Gemini at large context windows.

**Use it when:**

- The task is unusually complex (large refactor, multi-file design, deep code review). Use a more capable model and higher thinking effort.
- The task is bulk and repetitive (rename, regex sweep, simple edits). Use a lower-effort model — faster and cheaper.
- You hit a wrong-shaped answer and want a second opinion from a different model.

**Skip it when:**

- The default is already working well. Don't churn the model picker for habit's sake.

**The approach.**

Default to whatever the workspace agent is configured with. Bump effort when the agent visibly hesitates or produces shallow analysis. Drop effort for trivial work. Switch models when one repeatedly gets the same problem wrong; sometimes the answer is "this model has a blind spot here."

**Layer.** 10%. Model choice is the judgment knob. Most tasks don't need to touch it.

## Putting it together

The ten capabilities combine. A well-built workflow stacks several.

**Example: applying a Patch Tuesday-style automation runbook to Azure.**

1. **Workspace map (`CLAUDE.md`)** orients the agent: rules, push policy, ASCII-only PowerShell, the routing table.
2. **Instructions file** (`.github/instructions/powershell.instructions.md` with `applyTo: "**/*.ps1"`) loads automatically when the agent edits any `.ps1`, applying the PS-specific gotchas.
3. **MCP servers** (`Azure/azure-mcp`, `microsoftdocs/mcp`) give it real tenant state and current docs.
4. **Skill** (e.g. a future `azure-arc-onboarding` skill) defines the standard approach when that pattern repeats.
5. **Subagent** (`Explore`) answers "where is the existing onboarding script?" without cluttering the main conversation.
6. **Memory** records that PSScriptAnalyzer behavior differs across PS versions so the agent doesn't burn cycles re-discovering it.
7. **Terminal + file editing** does the actual work: writes the script, runs Pester, runs PSSA under both PS versions, commits, pushes.
8. **Model/effort selection** stays at the default unless the task gets gnarly.

Each capability does the thing it's best at. The map holds rules. Instructions scope rules to file types. MCP supplies real data. Skills capture repeatable processes. Subagents handle bounded explorations. Memory keeps gotchas. Terminal/edits do the work. Model selection is the rarely-touched judgment dial.

### The decision sequence

When starting a task in this workspace, run through these in order:

1. **Is the rule universal?** Yes → `CLAUDE.md`. No → next.
2. **Is the rule scoped to a file type?** Yes → `.instructions.md` with `applyTo`. No → next.
3. **Is this a repeatable process?** Yes → build or use a skill. No → next.
4. **Do I need real data from Azure / Graph / GitHub / docs?** Yes → enable the MCP server. No → next.
5. **Is this exploration or a bounded review?** Yes → invoke a subagent. No → main conversation.
6. **Output mode:** file edit → terminal/edit. Data exploration → notebook. Reusable workflow → prompt file.
7. **Is the answer shallow or wrong?** Bump model/effort. Otherwise stay default.

## Quick reference

| Capability | Best for | Layer | Auto-loads? |
|------------|----------|-------|--------------|
| Workspace map (`CLAUDE.md`) | Universal rules, routing | Infra | Yes |
| Skills (`.claude/skills/`) | Repeatable processes and standards | 30% | Yes (by description match) |
| Instructions (`.instructions.md`) | File-type-scoped rules | 30% | Yes (by `applyTo` glob) |
| MCP servers | External tool integration | Infra | After config |
| Memory (`/memories/`) | Personal/cross-session notes | Infra | Yes (user scope) |
| Subagents | Bounded research / review | Infra | On invoke |
| Prompt files (`.prompt.md`) | Reusable on-demand workflows | 30% | On invoke |
| Terminal + file editing | Real changes to real files/systems | 60% | Always available |
| Notebooks | Data exploration with persisted state | 60% | On request |
| Model / effort selection | The judgment dial | 10% | Manual |

---


