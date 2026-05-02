---
title: The Resource Index
description: Curated repos, MCP servers, SDKs, and tools for Microsoft / Azure / PowerShell / Terraform / IAM / SaaS / security work, plus Claude Code and skill resources. Use when looking up a tool, library, MCP server, or reference for any of those domains.
tags: [reference, azure, microsoft, powershell, terraform, opentofu, iam, entra, security, saas, web, mcp, skills, claude-code]
audience: infra-engineers, skill-builders, developers
last_reviewed: 2026-05-01
---

# The Resource Index

**Curated repos, MCP servers, SDKs, and tools — organized by what you'd actually use them for.**

Reorganized and extended for the stack used in this workspace: Microsoft, Azure, PowerShell, Terraform/OpenTofu, websites, IAM, SaaS, and security.

## Contents

1. [How This List Works](#how-this-list-works)
2. [By Domain](#by-domain)
3. [Official Anthropic Repos](#official-anthropic-repos)
4. [Community Skill Collections](#community-skill-collections)
5. [MCP Servers and Directories](#mcp-servers-and-directories)
6. [Azure & Microsoft 365](#azure--microsoft-365)
7. [PowerShell](#powershell)
8. [Terraform / OpenTofu](#terraform--opentofu)
9. [Identity & Access Management](#identity--access-management)
10. [Security](#security)
11. [Web & SaaS Frameworks](#web--saas-frameworks)
12. [Frontend, UI, and Design](#frontend-ui-and-design)
13. [Programmatic Video (Remotion)](#programmatic-video-remotion)
14. [Learning and Reference](#learning-and-reference)
15. [The 60/30/10 Lens](#the-603010-lens)

## How This List Works

Every link was chosen because it solves a real problem. Each entry includes a one-line description, the direct link, and a note on who it is for.

**A note on trust.** Community skills and MCP servers can execute code on your machine. Only install from repos you have reviewed. Official Microsoft and Anthropic repos are safe; everything else, read the source before you run it.

Links were verified May 2026. Repos move fast — if a link is dead, search the repo name on GitHub.

## By Domain

Quick routing — jump straight to the section that matches the task.

| Doing this... | Go to |
|---------------|-------|
| Spinning up Azure resources / IaC | [Azure & Microsoft 365](#azure--microsoft-365), [Terraform / OpenTofu](#terraform--opentofu) |
| Writing PowerShell modules or scripts | [PowerShell](#powershell) |
| Working with Entra ID, app regs, role assignments, conditional access | [Identity & Access Management](#identity--access-management) |
| Hardening / auditing a tenant or codebase | [Security](#security), [Identity & Access Management](#identity--access-management) |
| Building a portal / dashboard / SaaS UI | [Web & SaaS Frameworks](#web--saas-frameworks), [Frontend, UI, and Design](#frontend-ui-and-design) |
| Connecting Claude to real systems | [MCP Servers and Directories](#mcp-servers-and-directories) |
| Building or finding a Claude skill | [Official Anthropic Repos](#official-anthropic-repos), [Community Skill Collections](#community-skill-collections) |
| Generating video programmatically | [Programmatic Video (Remotion)](#programmatic-video-remotion) |
| Understanding the underlying concepts | [Learning and Reference](#learning-and-reference) |

## Official Anthropic Repos

Start here for anything Claude-adjacent. Maintained by Anthropic; safe to clone.

### anthropics/skills — Anthropic's Official Skills Repository
<https://github.com/anthropics/skills>

The production skills that power Claude's document creation (docx, pptx, xlsx, pdf), plus example skills for creative work, enterprise workflows, and development. Each skill is a self-contained folder with a `SKILL.md`. The reference implementation — read these before building your own.

**Who it is for:** Everyone. Start here to understand what a well-built skill looks like.

### anthropics/claude-code — Claude Code
<https://github.com/anthropics/claude-code>

The CLI agent. Includes the plugin system, subagent architecture, and example plugins. The `/plugins` directory shows how to structure commands, agents, and skills.

**Who it is for:** Anyone using Claude Code in the terminal. Essential reading for `CLAUDE.md` and subagent patterns.

### anthropics/claude-plugins-official — Official Plugin Marketplace
<https://github.com/anthropics/claude-plugins-official>

Curated directory of high-quality plugins (Anthropic-built and vetted third-party). Plugins bundle skills, agents, MCP configs, and slash commands.

**Who it is for:** Claude Code users looking for ready-made functionality.

### anthropics/anthropic-cookbook — Anthropic Cookbook
<https://github.com/anthropics/anthropic-cookbook>

Jupyter notebooks and recipes for the Claude API: tool use, structured output, prompt caching, embeddings, RAG.

**Who it is for:** Developers building on the API directly.

### anthropics/courses — Anthropic Courses
<https://github.com/anthropics/courses>

Official educational content: prompt engineering, tool use, RAG implementation. Interactive notebooks.

**Who it is for:** Anyone teaching or learning Claude fundamentals.

## Community Skill Collections

These repos curate and organize skills from across the community.

### travisvn/awesome-claude-skills
<https://github.com/travisvn/awesome-claude-skills>

The most comprehensive community list. Organized by category. Good starting point for browsing.

### obra/superpowers — Superpowers for Claude Code
<https://github.com/obra/superpowers>

20+ battle-tested skills including TDD workflows, debugging patterns, and `/brainstorm`, `/write-plan`, `/execute-plan` commands. By Jesse Vincent.

**Who it is for:** Claude Code power users who want structured development workflows.

### affaan-m/everything-claude-code
<https://github.com/affaan-m/everything-claude-code>

Anthropic hackathon winner. Complete agent harness optimization system: skills, memory management, security scanning, hooks. 1200+ tests. Works across Claude Code, Codex, and Cursor.

**Who it is for:** Advanced users who want a production-grade Claude Code setup out of the box.

### SkillsMP — Agent Skills Marketplace
<https://skillsmp.com>

Web-based directory with 400,000+ agent skills. Searchable by category. Works with Claude Code and Codex CLI.

**Who it is for:** Searching for a skill by use case rather than browsing repos.

### AgentSkills.io — Open Standard
<https://agentskills.io>

Open specification for agent skills. Cross-platform compatibility (Claude, Codex, others).

**Who it is for:** Skill builders targeting multiple platforms.

## MCP Servers and Directories

MCP connects Claude to external tools. The first three are the ones to enable for this stack.

### Azure/azure-mcp — Azure MCP Server (Microsoft)
<https://github.com/Azure/azure-mcp>

Microsoft's official Azure MCP server. Resource Graph queries, ARM/Bicep operations, Storage, Key Vault, AKS, and more. **The single highest-value MCP for this workspace.**

**Who it is for:** Anyone running Azure work through Claude.

### microsoftdocs/mcp — Microsoft Learn MCP
<https://github.com/microsoftdocs/mcp>

Pulls live answers from `learn.microsoft.com` instead of stale training data. Critical for Azure/PowerShell/Entra work because Microsoft docs change monthly.

**Who it is for:** Anyone whose questions touch Microsoft products.

### github/github-mcp-server — GitHub's Official MCP Server
<https://github.com/github/github-mcp-server>

Repo management, issue and PR automation, CI/CD intelligence, code analysis, Dependabot alerts. The deepest GitHub integration available.

### modelcontextprotocol/servers — Official MCP Reference Servers
<https://github.com/modelcontextprotocol/servers>

Reference implementations: Filesystem, Postgres, Slack, Google Drive, Puppeteer. Study these before building custom servers.

### stripe/agent-toolkit — Stripe Agent Toolkit
<https://github.com/stripe/agent-toolkit>

Official Stripe MCP server. Products, customers, payment links, invoices, subscriptions.

**Who it is for:** Anyone processing payments through Stripe (only relevant if a SaaS project actually bills through Stripe).

### punkpeye/awesome-mcp-servers
<https://github.com/punkpeye/awesome-mcp-servers>

The largest curated MCP server list. The place to search when you need "MCP server for X."

### tolkonepiu/best-of-mcp-servers — Ranked
<https://github.com/tolkonepiu/best-of-mcp-servers>

410+ servers ranked by a quality score (stars, contributors, commit frequency). Updated weekly.

**Who it is for:** Quality signals, not just a list.

## Azure & Microsoft 365

The Microsoft platform stack. Use the **Microsoft Learn MCP** alongside these so Claude pulls from current docs.

### Azure/azure-powershell — Az PowerShell
<https://github.com/Azure/azure-powershell>

The official Az.* PowerShell modules. The default for any Azure automation in PS.

### microsoftgraph/msgraph-sdk-powershell — Microsoft Graph PowerShell
<https://github.com/microsoftgraph/msgraph-sdk-powershell>

The Microsoft.Graph PowerShell SDK — the way to script Entra, Intune, Exchange, Teams, SharePoint.

### Azure/bicep — Bicep
<https://github.com/Azure/bicep>

Microsoft's domain-specific language for Azure ARM. Useful when you need an Azure-only IaC and want to track AzureRM resource shapes 1:1. For multi-cloud or anything portable, prefer Terraform/OpenTofu.

### Azure CLI — `az`
<https://github.com/Azure/azure-cli>

The cross-platform CLI. Often the fastest path for ad hoc tasks or quick scripts that don't justify a full Az PowerShell session.

### Azure/azure-sdk-for-net — .NET SDK
<https://github.com/Azure/azure-sdk-for-net>

For Functions, App Service backends, or any .NET service that needs to talk to Azure.

### Azure architecture center
<https://learn.microsoft.com/azure/architecture/>

Reference architectures, design patterns, and the Cloud Adoption Framework. Cite this before designing anything new.

## PowerShell

Linter, tests, and the modules you'll keep reaching for.

### PowerShell/PSScriptAnalyzer
<https://github.com/PowerShell/PSScriptAnalyzer>

The PowerShell linter. **Run under `powershell.exe` (PS 5.1) before pushing**, not just `pwsh` — PS 5.1 and PS 7 disagree on rules like `PSUseDeclaredVarsMoreThanAssignments`, and CI commonly runs PS 5.1.

### pester/Pester — Pester
<https://github.com/pester/Pester>

The test framework for PowerShell. Required for any module worth shipping.

### PowerShell/PowerShell — PS 7 itself
<https://github.com/PowerShell/PowerShell>

The cross-platform PowerShell. Track release notes — breaking changes between minor versions are real.

### Microsoft Learn: PowerShell scripting style guide
<https://learn.microsoft.com/powershell/scripting/dev-cross-plat/style-guide>

Microsoft's own style guide. Approved verbs, parameter conventions, comment-based help.

### PowerShell Gallery
<https://www.powershellgallery.com>

Where modules are published. Look up Azure, Microsoft.Graph, PSRule, Pester, Maester from here.

## Terraform / OpenTofu

Default IaC stack for this workspace is **OpenTofu** with the AzureRM and AzAPI providers.

### opentofu/opentofu — OpenTofu
<https://github.com/opentofu/opentofu>

Open-source Terraform fork. Drop-in compatible. The default `tofu` runtime in this workspace.

### hashicorp/terraform-provider-azurerm — AzureRM provider
<https://github.com/hashicorp/terraform-provider-azurerm>

The mainstream Azure provider. First choice for any Azure resource it supports.

### Azure/terraform-provider-azapi — AzAPI provider
<https://github.com/Azure/terraform-provider-azapi>

Microsoft's provider that wraps the raw Azure REST API. Use when AzureRM doesn't yet support a property or resource type, or for preview features.

### Azure/Azure-Verified-Modules — Azure Verified Modules (AVM)
<https://github.com/Azure/Azure-Verified-Modules>

Microsoft-curated, tested Terraform and Bicep modules. Big time-saver — start here before writing your own module.

### terraform-linters/tflint — TFLint
<https://github.com/terraform-linters/tflint>

The Terraform linter. AzureRM-specific ruleset available. Run in CI.

### bridgecrewio/checkov — Checkov
<https://github.com/bridgecrewio/checkov>

Static analysis for Terraform / IaC: misconfig + security checks. Pair with TFLint in CI.

### gruntwork-io/terragrunt — Terragrunt
<https://github.com/gruntwork-io/terragrunt>

DRY wrapper for many envs/regions. **Skip unless** you have at least 3+ environments or repeated module-instantiation pain — adds complexity for small setups.

### tfsec / Trivy
<https://github.com/aquasecurity/trivy>

Trivy now includes the former `tfsec` IaC scanner. Alternative or complement to Checkov.

## Identity & Access Management

Entra ID-focused. Maester is the most underrated tool in this list — it's PowerShell + IAM + security in one package.

### maester365/maester — Maester
<https://github.com/maester365/maester>

Pester-based Entra tenant security testing. Hundreds of pre-built tests for conditional access, app registrations, role assignments, and Entra ID security baselines. Runs as Pester, integrates into CI.

**Who it is for:** Anyone responsible for an Entra tenant's security posture. Directly in the IAM + PowerShell + security crossover.

### microsoft/EntraExporter
<https://github.com/microsoft/EntraExporter>

Exports your full Entra tenant configuration to JSON for diffing, backup, or DR planning.

### AzureAD/AzureADAssessment
<https://github.com/AzureAD/AzureADAssessment>

Microsoft's tenant assessment tool — pulls a snapshot of your tenant's identity posture for review.

### microsoftgraph/entra-powershell
<https://github.com/microsoftgraph/entra-powershell>

The newer Entra-specific PowerShell module. Replaces parts of the deprecated AzureAD module.

### MSAL libraries
- .NET: <https://github.com/AzureAD/microsoft-authentication-library-for-dotnet>
- JavaScript: <https://github.com/AzureAD/microsoft-authentication-library-for-js>
- Python: <https://github.com/AzureAD/microsoft-authentication-library-for-python>

The right way to do OAuth/OIDC against Entra. Never roll your own auth — use MSAL or `Microsoft.Identity.Web`.

### AzureAD/microsoft-identity-web — Microsoft.Identity.Web
<https://github.com/AzureAD/microsoft-identity-web>

ASP.NET Core / .NET integration for Entra ID. Use this when the portal backend is .NET.

### Conditional Access reference
<https://learn.microsoft.com/entra/identity/conditional-access/>

Microsoft's CA documentation. Zero-Trust deployment guides, common policies, troubleshooting.

## Security

OWASP, secret scanning, KQL, and Azure-specific rule packs.

### OWASP Cheat Sheet Series
<https://github.com/OWASP/CheatSheetSeries>

Concise, actionable guides for every common security topic: auth, access control, session management, secrets, XSS, CSRF. Right level of detail to feed into a skill or `CONTEXT.md`.

### Azure/PSRule.Rules.Azure — PSRule for Azure
<https://github.com/Azure/PSRule.Rules.Azure>

PowerShell-based scanning for Azure resources, ARM/Bicep templates, and Terraform plans against the Well-Architected Framework and CIS benchmarks.

### microsoft/PSRule — PSRule (engine)
<https://github.com/microsoft/PSRule>

The underlying rule engine — write your own rules in PowerShell or YAML.

### Azure/Azure-Sentinel
<https://github.com/Azure/Azure-Sentinel>

Microsoft Sentinel content hub: KQL detections, analytic rules, hunting queries, playbooks, workbooks. Reference for any KQL work.

### trufflesecurity/trufflehog
<https://github.com/trufflesecurity/trufflehog>

Secret scanner. Run pre-commit and in CI to enforce the "never commit credentials" rule.

### gitleaks/gitleaks
<https://github.com/gitleaks/gitleaks>

Alternative secret scanner. Lighter than trufflehog; works well as a pre-commit hook.

### MITRE ATT&CK — `mitre/cti`
<https://github.com/mitre/cti>

The reference taxonomy for threat behaviors. Use when writing detections, threat-modeling, or mapping incidents.

### Microsoft Security baselines
<https://learn.microsoft.com/security/benchmark/azure/>

Microsoft Cloud Security Benchmark. Links Azure controls to CIS and NIST. The defensible baseline to claim compliance against.

## Web & SaaS Frameworks

For the portal-style projects. The frontend libraries (shadcn, Tailwind, etc.) are in the next section.

### vercel/next.js — Next.js
<https://github.com/vercel/next.js>

The default React framework: SSR, file-based routing, server actions, edge runtime. Pairs well with Entra auth via NextAuth or `next-auth-azure-ad`.

### nextauthjs/next-auth — Auth.js / NextAuth
<https://github.com/nextauthjs/next-auth>

Authentication for Next.js (and other frameworks). Has a first-class Microsoft Entra ID provider. Use when the portal isn't all-in on `Microsoft.Identity.Web`.

### withastro/astro — Astro
<https://github.com/withastro/astro>

Content-first web framework. Better choice than Next.js for documentation portals or marketing pages where most content is static.

### Azure Static Web Apps
<https://learn.microsoft.com/azure/static-web-apps/>

Hosting + auth + Functions API in one Azure service. Easiest hosting story for an Entra-protected portal.

### Azure App Service (.NET / Node.js)
<https://learn.microsoft.com/azure/app-service/>

When the backend is heavier than Functions can comfortably handle.

## Frontend, UI, and Design

Component libraries and visual standards. If your project has a brand system, encode it as a skill under `.claude/skills/` so the agent applies it automatically.

### shadcn-ui/ui — shadcn/ui
<https://github.com/shadcn-ui/ui>

Copy-paste React components on Radix UI + Tailwind. You own the code. Claude knows shadcn/ui patterns natively.

### tailwindlabs/tailwindcss — Tailwind CSS
<https://github.com/tailwindlabs/tailwindcss>

Utility-first CSS. The CSS layer Claude thinks in.

### Aceternity UI / Magic UI
- <https://ui.aceternity.com>
- <https://magicui.design>

Animated copy-paste components for marketing-quality pages.

### lucide-icons/lucide — Lucide Icons
<https://github.com/lucide-icons/lucide>

The icon set Claude uses by default in React artifacts. 1500+ icons, MIT.

### recharts/recharts — Recharts
<https://github.com/recharts/recharts>

The default charting library for Claude artifacts. React + D3.

### v0 by Vercel
<https://v0.dev>

Vercel's AI UI generator. Generates production React in shadcn/Tailwind. Good for a fast starting point.

## Programmatic Video (Remotion)

Lower priority for this workspace's stack — keep this section short. Skip unless a project actually produces video.

### remotion-dev/remotion
<https://github.com/remotion-dev/remotion>

Render React components into MP4/WebM. Server-side and serverless rendering. Requires a company license for teams of 3+.

### GitHub topic: remotion
<https://github.com/topics/remotion>

Discovery surface for templates and examples.

## Learning and Reference

Repos and sites for understanding tools rather than just using them.

### Anthropic Documentation
- <https://docs.claude.com>
- <https://code.claude.com/docs>

API reference, Claude Code documentation, skill authoring guides.

### Anthropic Blog: Skills Explained
<https://claude.com/blog/skills-explained>

How Skills, Projects, Prompts, MCP, and Subagents relate. Includes decision matrices.

### anthropics/anthropic-quickstarts
<https://github.com/anthropics/anthropic-quickstarts>

Deployable starter apps using the Claude API.

### Model Context Protocol Specification
<https://modelcontextprotocol.io>

The MCP spec site. Protocol docs, example servers and clients.

### Microsoft Learn
<https://learn.microsoft.com>

The umbrella for Azure, PowerShell, Entra, Microsoft 365, and Security docs. Pair with the Microsoft Learn MCP server for in-context lookups.

### HashiCorp Learn
<https://developer.hashicorp.com/terraform/tutorials>

Official Terraform tutorials. Most translate cleanly to OpenTofu.

## The 60/30/10 Lens

Every entry maps to a layer. Quick reference:

| Category | What it helps you build | Layer |
|----------|--------------------------|-------|
| Azure SDKs / CLIs | Cloud control + automation | 60% Code |
| Terraform / OpenTofu / AVM | Reproducible infra | 30% Rules |
| PowerShell modules + Pester | Repeatable scripts and tests | 60% Code |
| Maester / PSRule / Checkov / OWASP | Security baselines | 30% Rules |
| MSAL / Microsoft.Identity.Web | Identity plumbing | Infrastructure |
| MCP servers | External tool integration | Infrastructure |
| Web frameworks (Next.js, Astro) | User-facing apps | 60% Code |
| Frontend / UI libraries | Components, visualizations | 30% Rules |
| Anthropic skills + community skills | Repeatable AI workflows | 30% Rules |
| Learning / Reference | Understanding the tools | Foundation |

Most of these give you **30% tools** (rules and templates) or **infrastructure**. The AI's job (the 10%) is choosing which tool to use and filling in judgment-required parts. The 60% layer is the deterministic code those rules and infra run on.

**Takeaway:** knowing what tools exist and when to reach for them is worth more than getting better at raw prompting. A mediocre prompt aimed at the right skill outperforms a brilliant prompt aimed at nothing.

---


