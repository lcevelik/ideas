# Claude Code Workflows — Distilled from YouTube Transcripts

> Patterns, setups, and strategies for getting the most out of Claude Code.  
> Sources: Multiple YouTube tutorials and talks (June 2026 timeframe).

---

## Table of Contents

1. [Mission Control Dashboard (Multi-Agent Orchestration)](#1-mission-control-dashboard-multi-agent-orchestration)
2. [Git Worktrees for Parallel Agents](#2-git-worktrees-for-parallel-agents)
3. [UltraCode — Deterministic Fan-Out Patterns](#3-ultracode--deterministic-fan-out-patterns)
4. [Routines — Proactive / Scheduled Agents](#4-routines--proactive--scheduled-agents)
5. [LM Studio + Claude Code (Free Local AI)](#5-lm-studio--claude-code-free-local-ai)
6. [Local SEO Content Generation Workflow](#6-local-seo-content-generation-workflow)
7. [Cross-Cutting Best Practices](#7-cross-cutting-best-practices)

---

## 1. Mission Control Dashboard (Multi-Agent Orchestration)

**Source:** Mike Russell — *"My Exact Claude Code Setup Still Allowed After Ban"*

### Concept

A self-hosted web dashboard ("Tank") that acts as a command center for running multiple Claude Code sessions simultaneously. Each agent runs in its own sandbox (tmux window), and the dashboard provides real-time visibility, chat history, and Git integration.

### Key Architecture

- **Self-hosted** — runs on your own server; nothing leaves the building except calls to Anthropic
- **Forgjo** (local Gitea/Forgejo fork) — self-hosted Git repos instead of GitHub (avoids security concerns and keeps everything local)
- **tmux** — each Claude Code session runs in its own tmux window, enabling multiple concurrent agents on one machine
- **SQLite** — lightweight database for chat history, task tracking, and to-do lists
- **Hooks** — bidirectional communication between the dashboard and Claude Code terminals; responses are echoed back to the web UI
- **Local AI** — uses a local model for task naming/summarization (not for the main Claude Code work)

### Workflow Pattern

1. Create a new task from the dashboard → local AI names it → Claude Code spins up in a tmux session
2. Watch the terminal live in the browser, or let it run in the background
3. When done, review the output, open a PR on Forgejo, merge it
4. Forgejo runs CI/CD actions locally (install SSH keys, redeploy, run tests)
5. Sessions are **resumable** — chat context persists across restarts

### Anthropic TOS Compliance

The setup stays within Anthropic's terms because:
- It's **interactive** terminal use (not programmatic token hijacking)
- Hooks echo real Claude Code responses back to the UI
- It's controlled by one person, not farmed out to a team
- **Not** using third-party tools like OpenClaw or impersonating the API

> **"Your subscription will no longer cover third party tools like Open Claw. But they don't ban interactive terminal use."** — Head of Claude Code (April 2025)

### Features Demonstrated

- **Multi-tasking** — run a coding task and a research task simultaneously
- **Drag-and-drop images** — attach screenshots to prompts (uploaded to temp location, referenced by Claude Code)
- **Chat with Claude Code as a general LLM** — e.g., "find the best Mexican food in Limassol, Cyprus" using sub-agents for web search
- **Usage tracking** — real-time 5-hourly and weekly usage percentages in the dashboard
- **To-do management** — `todo.md` in the repo, mirrored in the dashboard

### Safety Warning

> Every agent runs with **permissions skipped** — it doesn't stop and ask, it just does.  
> **Run it isolated, lock it down for testing, never run as root.**

---

## 2. Git Worktrees for Parallel Agents

**Source:** *"Git Worktrees Explained — Run Multiple AI Agents in Parallel (Claude Code Tutorial)"*

### The Problem

Running two Claude Code sessions in the same directory causes chaos — both agents read/write the same files, and changes appear unexpectedly.

### The Solution: Git Worktrees

A Git repo has two parts:
- **Object store** — commits, history, branches (shared)
- **Working tree** — files on disk (normally coupled 1:1)

**Worktrees decouple them.** One repo, one history, but multiple independent working directories on different branches.

```
main-repo/              ← source of truth
├── project-feature-a/  ← worktree, branch: feature-a
├── project-feature-b/  ← worktree, branch: feature-b
└── project-hotfix/     ← worktree, branch: hotfix
```

### Commands

```bash
# List all worktrees
git worktree list

# Create a new worktree with a new branch
git worktree add -b <branch-name> <path> <base-branch>

# Example
git worktree add -b new-endpoint ../project-endpoint origin/staging

# Remove a worktree
git worktree remove <path>
```

### Naming Convention

Prefix all worktree directories with the project name so `ls` shows them grouped together. Suffix with the feature name for clarity in terminal tabs.

### Advanced Patterns

#### Agent-vs-Agent Comparison
Give two agents the **same task** in separate worktrees. Review both outputs, pick the better one. Cost: same time, half the opportunity cost. Great for architecture decisions.

#### Worktree-Specific CLAUDE.md
- Root `CLAUDE.md` (or `agents.md` for Codex) provides project-wide context — automatically available in all worktrees
- Add a **worktree-specific** `CLAUDE.md` in each worktree root for scoped context per feature

#### tmux Integration
```bash
# Create named session for a worktree
tmux new-session -s hubspot -c ../project-hubspot

# List active sessions
tmux ls

# Detach: Ctrl+B, D
# Reattach: tmux attach -t hubspot
```

Each tmux session = one persistent Claude Code instance per worktree. Close your laptop, reopen, reattach — context preserved.

### When to Use vs. Not Use

| Use Worktrees When | Don't Use When |
|---|---|
| 2+ independent tasks that don't share files | Single task spans the whole codebase |
| Want to compare two approaches to the same problem | Exploratory work where the agent needs to roam freely |

---

## 3. UltraCode — Deterministic Fan-Out Patterns

**Source:** *"Claude just dropped UltraCode... its Insane"*

### What Is UltraCode?

UltraCode replaces the **managing LLM** with **code** (JavaScript) to orchestrate parallel sub-agents. Instead of a master agent that can forget things or decay in performance, a deterministic script runs the show.

- Up to **10 concurrent agents** per task
- Up to **1,000 agents per run** (sequential batches)
- Activated by including "ultra code" in the prompt
- Separate axis from reasoning effort (low/medium/high/max)

### Key Strategies

| Strategy | Description |
|---|---|
| **Adversarial Verify** | Spawn independent skeptics to refute a claim; kill on majority vote. Plausible-but-wrong findings don't survive. |
| **Judge Panel** | Generate multiple independent attempts → judges score them → synthesize winner, grafting best ideas |
| **Perspective Verify** | Identical reviewers each given a different lens to evaluate from |
| **Pipeline vs. Parallel** | Items flow stage-to-stage with no barrier |
| **Completeness Critic** | Asks "what did we miss?" after initial output |
| **Loop Until Dry** | Keep iterating until no more improvements found |

### When to Use UltraCode (The 20/80 Rule)

**Use it (~20% of the time) for:**
- Research and audits
- Multi-perspective reviews and decisions
- Adversarial verification of claims
- Building separable, reusable systems
- When you don't know the shape of the answer

**Don't use it (~80% of the time) for:**
- Single-file refactors
- Normal feature builds
- Bug fixes
- Anything with strict step-by-step dependencies
- Quick sub-5-minute tasks

### Token Cost Warning

- **4–7x token burn** compared to normal mode
- Parallel fan-out burns tokens fast
- If you don't need a team to figure out the question, don't use it
- "It's like firing a bazooka when a hammer would have done the job"

### Build Pattern: Agentic Operating System

The video demonstrates building a **model comparison dashboard** using UltraCode:
1. **Recon** phase — multiple agents research current models, benchmarks, pricing
2. **Merge** phase — consolidate findings
3. **Verify** phase — cross-check facts
4. **Design** phase — plan the UI/UX
5. **Build** phase — implement the feature

Result: A searchable, filterable model database (by smartest, cheapest, fastest, most-used, arena rankings, open-source status).

---

## 4. Routines — Proactive / Scheduled Agents

**Source:** Anthropic Engineer Maya — *"Code with Claude" Workshop*

### What Are Routines?

A new feature in Claude Code that lets you kick off **remote Claude Code sessions** by defining:
- A **prompt** (what to do)
- **Repos** (what code to connect)
- **Connectors** (Slack, GitHub, Google Drive, etc.)
- A **trigger** (when to run)

Claude Code handles hosting, session state, and infrastructure.

### Three Design Principles

1. **Always Available** — runs on Claude Code's managed infrastructure; nothing depends on your laptop being open
2. **Proactive Triggers** — time-based (cron) or event-based (GitHub events, custom webhooks)
3. **Interactive & Steerable** — every routine is a Claude Code session you can open, watch, steer, and resume from web, CLI, or desktop

### Trigger Types

| Type | Example |
|---|---|
| **Time-based** | "Once a week, review all new changes merged to main against our docs repo and create a PR to update docs" |
| **Event-based** | "When a PR labeled `needs-docs` is merged, create documentation" |
| **Release-based** | "When a release is cut, diff the release branch against docs and create PRs for new features" |

### Three Decisions for Every Routine

1. **When** should it trigger? (schedule or event)
2. **What context** does Claude need? (repos, files, connectors)
3. **How do you steer it?** (agent-on-agent review, human-in-the-loop, output verification)

### Steerability Patterns

- **Agent-on-agent review** — one routine creates docs PRs; another routine reviews those PRs and leaves comments before a human sees them (generator-critiquer pattern)
- **Human-in-the-loop** — open the live session on the web, ask questions mid-session, nudge in a different direction
- **Resume sessions** — continue a past routine's conversation
- **Verify outputs** — render the changed documentation pages and confirm correctness

### Real Anthropic Internal Use Case

One engineer maintains docs for Claude Code + Agent SDK. She set up:
- A weekly routine to diff source code against docs repo
- Auto-create PRs when documentation is outdated
- Slack notifications when PRs are created

> Weekly PRs for Claude Code have gone up 100% since the beginning of the new year. Routines help the docs keep up.

---

## 5. LM Studio + Claude Code (Free Unlimited AI Agents)

**Source:** *"Claude Code + LM Studio: FREE Unlimited AI Agents (Don't Pay $200/month)"*

### The Setup: Co-Work on 3P

Use Claude Desktop's **gateway mode** to connect to local AI models via LM Studio — no Anthropic account or API key needed for the LLM backbone.

### Steps

1. **Download Claude Desktop** (Mac/Windows) — do NOT sign in
2. **Enable Developer Mode** → Help → Troubleshooting → Enable Developer Mode
3. **Configure Third-Party Inference** → Developer menu → Configure Third-Party Inference
   - Connection type: **Gateway**
   - Credential kind: **Static API Key**
   - Gateway base URL: from LM Studio's developer panel
   - API key: `lm-studio` (or any placeholder for local use)
4. **Disable built-in tools** — local models don't have web search; use MCP instead

### LM Studio Setup

1. **Download a model** — look for green tick (full GPU offload possible). Recommended: Gemma 4B or 2B for agentic tasks
2. **Enable Developer Mode** in LM Studio settings
3. **Rename the API identifier** — Claude looks for "Sonnet", "Opus", or "Haiku" in the model name. Rename your local model's API identifier to `claude-opus-4.8` (or similar) so Claude recognizes it
4. **Load the model** — wake it up from sleep into memory
5. **Copy the gateway URL** from LM Studio's developer panel

### Adding Web Search (MCP)

Local models don't have built-in web search. Add an MCP server:

1. Sign up for Brave Search API (free tier available)
2. Get the NPX install command from the Brave Search MCP docs
3. Edit Claude Desktop's configuration file
4. Paste in your existing config + the MCP settings → ask Claude to merge them
5. Save, restart

> You can add any MCP connector: ClickUp, Gmail, Firecrawl, etc. Just paste the config and ask Claude to combine.

### Dynamic Workflows

With Opus 4.8, Claude Code supports **dynamic workflows**:
- Up to **16 concurrent agents**
- Up to **1,000 agents per run**
- Use `/deep-research` slash command in Claude Code to auto-generate hundreds of agents for complex tasks

---

## 6. Local SEO Content Generation Workflow

**Source:** *"Claude Code Local SEO: How I Got 50,000 Google Clicks/Mo"*

### Overview

Use Claude Code to automate local SEO — optimizing Google Business profiles, generating localized blog posts, and creating service pages. The creator generated 50,000 clicks/month and $500K+ revenue from a local business.

### Google Business Profile Optimization

1. Create a `google-business-profile-setup.md` file with optimization checklist
2. Paste it into a Claude Code project folder
3. Give Claude your business website URL
4. Claude researches competitors and generates:
   - Primary + 9 secondary categories (stolen from top competitors)
   - 10–50 service offerings (validated against keyword search volume)
   - Service areas (municipalities within 2-hour drive)
   - Products (for click-through rate, not search impressions)

### Key SEO Concepts for the Workflow

- **Proximity** is the #1 ranking factor in 2026 (out of your control)
- **Top 3 Map Pack** gets 50% of all clicks
- **Service-area businesses** (plumber travels to you) rank across the whole city; storefront businesses (barber) rank locally
- Use **Semrush** to validate keyword volume before adding services (search "[service] [city]" — if 0 volume, skip it)

### Blog Post & Service Page Generation

- Claude Code can generate localized content targeting specific municipalities
- Template-based approach: define the structure, Claude fills in location-specific variations
- Aim for the organic listings below the map pack to capture additional traffic

---

## 7. Cross-Cutting Best Practices

### Project Organization

- Keep all Claude Code projects in a dedicated folder
- One folder per project; each project gets its own `CLAUDE.md` or `agents.md`
- Use `todo.md` files for task tracking — Claude can read and update them

### Context Management

- **Root `CLAUDE.md`** — project-wide context, automatically available everywhere
- **Worktree-specific `CLAUDE.md`** — scoped context per feature
- Feed flat repo files to other AIs (Gemini, etc.) for cross-validation before coding

### Multi-Agent Coordination

| Approach | Best For |
|---|---|
| tmux + worktrees | Parallel independent features |
| UltraCode | Research, audits, multi-perspective decisions |
| Routines | Scheduled/recurring tasks, documentation sync |
| Mission Control dashboard | Power users running 5+ concurrent sessions |

### Security & Safety

- **Never run as root**
- **Run agents in isolated environments** (containers, separate users, etc.)
- **Permissions skipped is powerful but dangerous** — review outputs carefully
- Use Forgejo/Gitea instead of GitHub for full local control
- Everything self-hosted = nothing leaves the building except API calls

### Model Selection Strategy

Be **model agnostic** — use the right model for the right task:
- Multimedia → Gemini
- Code review → GPT-5.5
- Deep research → Claude with UltraCode
- Routine tasks → cheaper/smaller models
- Build a knowledge base of current model capabilities (benchmarks, pricing, speed)

### Token Efficiency

- UltraCode: reserve for the ~20% of tasks that truly benefit from parallel agents
- Reasoning effort settings (low/medium/high/max) are a separate axis from UltraCode
- For simple tasks, keep effort low and UltraCode off
- "Firing a bazooka when a hammer would have done the job" — match tool to task

---

## Sources

| Video | Creator | Key Topic |
|---|---|---|
| My Exact Claude Code Setup Still Allowed After Ban | Mike Russell | Mission Control dashboard, Forgejo, multi-agent orchestration |
| Claude Code + LM Studio: FREE Unlimited AI Agents | Mike Russell | Gateway mode, local models, dynamic workflows |
| Git Worktrees Explained — Run Multiple AI Agents in Parallel | (Tutorial) | Worktrees for parallel Claude Code sessions |
| Claude just dropped UltraCode... its Insane | Jack | UltraCode strategies, judge panels, agentic OS |
| Anthropic Engineer: Routines Workshop | Maya (Anthropic) | Routines feature, proactive agents, scheduled workflows |
| Claude Code Local SEO: 50,000 Google Clicks/Mo | (Tutorial) | SEO content generation with Claude Code |
