# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Workspace Overview

This workspace contains two projects:

### marketingskills/
A content-only repository of 36 AI agent skills for marketing tasks (CRO, copywriting, SEO, analytics, growth) plus custom consulting framework skills. No executable code — skills are markdown files following the [Agent Skills specification](https://agentskills.io/specification.md). Also functions as a Claude Code plugin via `.claude-plugin/marketplace.json`.

**Custom framework skills added (not in upstream repo):**
- `mbb-strategic-frameworks` — McKinsey/BCG/Bain strategy, MECE, experience curves, NPS, M&A
- `big4-operational-frameworks` — Deloitte/PwC/KPMG/EY execution, AI transformation, risk, TOM
- `marketing-execution-persuasion` — Breakthrough Advertising, DASH Method, Hormozi offers, 48 Laws, Google PMax, Meta OCM

**WORKSPACE_CLAUDE.md** — GitHub backup mirror of `~/.claude/CLAUDE.md`. Keep in sync when the global file changes.

### my-first-claude-project/
A minimal Node.js project (CommonJS) with a single `index.js` entry point.

### tools/
MCP server implementations:
- `nvidia-nim-mcp/` — Kimi K2 Agent Swarm MCP server via NVIDIA NIM (Node.js). Start: `node tools/nvidia-nim-mcp/index.js`. Test: `node tools/nvidia-nim-mcp/test-server.js`
- `google-ads-mcp/` — Google Ads MCP server
- `mysql-mcp/` — MySQL MCP server
- `REGISTRY.md` — index of 29 tool integrations (same as `~/.claude/tools/REGISTRY.md`)

Note: `marketingskills/tools/` also contains integrations/ and MCP servers — separate from root `tools/`.

### .agents/skills/ and .claude/skills/
Custom skills not in the marketingskills upstream repo: `find-skills`, `skill-creator`, `vercel-composition-patterns`, `vercel-react-best-practices`, `vercel-react-native-skills`, `web-design-guidelines`. These mirror each other and load into Claude Code's skill system.

---

## nvidia-nim-mcp Architecture

The MCP server exposes **3 tools** over stdio transport:

| Tool | Purpose |
|---|---|
| `kimi_chat` | Single Kimi call with custom system prompt |
| `kimi_swarm` | N parallel agents via `Promise.all()` — max 40 (RPM limit) |
| `kimi_agent_task` | Route to pre-built role from the 15-agent marketing team |

**Model routing (3 variants):**
- `moonshotai/kimi-k2.5` — primary, all generation tasks
- `moonshotai/kimi-k2-thinking` — auto-selected for reasoning-heavy roles: STRATEGIST, QUANT, CONSULTANT, PRODUCT
- `moonshotai/kimi-k2-instruct` — fallback

**Rate limiting:** Sliding 60s window, hard cap at 40 RPM, warning at 35 RPM. Tracked in-process via `rpmTracker`. `kimi_swarm` counts each agent as 1 request.

**Requires:** `NVIDIA_API_KEY` env var. Server exits on startup if missing.

**`kimi_swarm` optional `aggregate_prompt`:** If provided, fires a final synthesis call combining all agent outputs after parallel execution completes.

---

## marketingskills: Key Conventions

### Skill Structure
Each skill lives in `skills/<skill-name>/SKILL.md` with required YAML frontmatter:
```yaml
---
name: skill-name        # must match directory name exactly
description: When to use this skill. Include trigger phrases.
---
```

### Naming Rules
- Lowercase letters, numbers, and hyphens only
- Cannot start/end with hyphen, no consecutive hyphens (`--`)
- `name` field must match parent directory name exactly

### Constraints
- `SKILL.md` must be under 500 lines — move details to `references/` subdirectory
- `description` must be 1-1024 characters and include trigger phrases for skill discovery
- `name` must be 1-64 characters

### Git Workflow
- Branches: `feature/skill-name`, `fix/skill-name-description`, `docs/description`
- Commits: Conventional Commits (`feat:`, `fix:`, `docs:`)

### Validation (no automated tests)
Manually verify: valid YAML frontmatter, `name` matches directory, naming rules followed, no sensitive data.

---

## my-first-claude-project: Commands
- Run: `node my-first-claude-project/index.js`
- No test suite configured yet

---

## Agent Hierarchy
Full 3-tier AI agent hierarchy is defined in `marketingskills/CLAUDE.md`. Read it when executing marketing tasks or managing agent orchestration.

**Key registries in `marketingskills/.sentinel/`:**
- `intel-gaps.md` — RECON-confirmed intel gaps
- `upgrade-log.md` — SENTINEL skill upgrade history
- `skill-scores.md` — MONITOR per-agent skill scores (1-5)
- `black-flash-log.md` — MONITOR Black Flash events + gap diagnoses
- `employee-log.md` — MONITOR employee agent spawn tracking

---

## Marketing Skills: Tools & Context

### Tool Registry
When a marketing skill mentions a specific tool (e.g., GA4, Stripe, Rewardful), reference the integration guides:
- **Tool index**: `~/.claude/tools/REGISTRY.md` — lists all 29 tools with capabilities and categories
- **Integration details**: `~/.claude/tools/integrations/{tool}.md` — API endpoints, auth setup, and common operations
- **MCP-enabled tools**: ga4, stripe, mailchimp, google-ads, resend, zapier (can be configured as MCP servers for direct interaction)

### Product Marketing Context
Before running any marketing skill, check for `~/.claude/product-marketing-context.md`. This shared context document contains foundational product/audience/positioning info that all marketing skills reference. If it doesn't exist yet, suggest running `/product-marketing-context` to create it.

### Version Checking
On first use of any marketing skill in a session, check for updates:
1. Fetch `https://raw.githubusercontent.com/coreyhaines31/marketingskills/main/VERSIONS.md`
2. Compare against local skill versions
3. Only notify if 2+ skills have updates or any skill has a major version bump
4. Non-blocking notification at end of response: "Skills update available: X marketing skills have updates. Say 'update skills' to update."
5. If user says "update skills", run `git pull` in `marketingskills/`

### Plugin Marketplace
The marketingskills repo is registered as a plugin marketplace (`coreyhaines31/marketingskills`). Skills can be updated via the plugin system.
