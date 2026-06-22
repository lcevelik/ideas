# Hermes Agent — Distilled Insights

> Compiled from video transcripts on Hermes Agent usage, architecture, and best practices.

---

## What Hermes Agent Is (And Isn't)

Hermes Agent is an autonomous AI operator — **not** a direct replacement for Claude Code or Codex. It's slower and less token-efficient for building apps or tools. The sweet spot is using Hermes as an **AI employee** that operates systems built by Claude/Codex.

**The mental model:** Build with Claude/Codex → Operate with Hermes.

Source: *15 Hermes Agent use cases*

---

## 15 Real-World Use Cases

### Business Operations
1. **SEO Analyst** — Give access to Search Console, Ahrefs, Semrush. Agent crawls data, creates reports, and can take action on findings.
2. **Lead Scraper** — Uses Apify, Firecrawl, Serper, Clay to find and enrich leads. Outputs CSV files with enriched contact data (emails, LinkedIn, etc.).
3. **Sales CRM** — Connects to HighLevel or similar CRMs. Handles inbox triage, client research, report generation.
4. **Sales Call Analyst** — Takes transcripts after calls, generates next steps and tasks within the CRM.
5. **Customer Support** — Monitors tickets, answers/escalates tickets. Shopify has both API and MCP support.
6. **Operations Employee** — Recurring workflows, process checklists, monitoring. Uses cron jobs for scheduling.

### Content & Research
7. **Research Agent** — Daily summary of what's new on social media (X, YouTube). Self-improving: rating results leads to better results next day.
8. **Content Producer** — Send a news item → agent creates video idea, outline, script, thumbnails.
9. **Re-purposing** — After video is done, creates newsletters, blog posts, LinkedIn/X posts.

### Knowledge & Planning
10. **Second Brain** — Using NotebookLM, Obsidian, or Pinecone as memory layer.
11. **Executive Assistant** — Access to inbox, calendar, tasks, meetings, notes. Triage, scheduling, meeting notes.
12. **Business Analyst** — Point agent at Excel sheets, dashboards. Creates reports and monitoring.
13. **Advisory Council** — Populate NotebookLM with content from influencers/authors, then query their "knowledge" for context.
14. **Investment Analyst** — Give list of stocks, agent crawls preferred sources with Firecrawl, returns aggregated analysis and advice.

### Developer
15. **Portfolio Tracker** — Send screenshot of trade → agent logs everything into a portfolio tracker with options tracking.

---

## Multi-Agent Architecture (Study Companion System)

A 6-agent system built for academic use, demonstrating key Hermes patterns:

### Agent Roles
| Agent | Role | Function |
|-------|------|----------|
| **Bill** | Orchestrator | Primary contact, routes tasks to specialists, reports back |
| **Vault** | File Librarian | Stores uploaded files, logs inventory, tags documents |
| **Scholar** | Content Extractor | Reads PDFs, extracts structured markdown notes |
| **Quiz Master** | Assessment Generator | Creates practice quizzes from structured notes |
| **Planner** | Calendar Manager | Extracts deadlines, exam dates, lecture schedules |
| **Dev** | Builder | Builds mission control dashboard and infrastructure |

### Pipeline Flow
```
Upload PDF → Bill coordinates → Vault stores → Scholar extracts → Quiz Master generates → Planner schedules
```

### Key Architecture Decisions

**Persistent Profiles** — Agents must be created as *persistent* (always active), not on-demand. Without this keyword, they're created on-need basis like in OpenClaw.

**Dedicated Workspaces** — Each agent gets:
- Own memory space
- Own identity (so.md)
- Own workspace directory
- Boundary enforcement (declines tasks outside jurisdiction)

**Shared Team Awareness** — Every agent knows the other agents and their roles. If Scholar needs dev work, it knows to escalate to Dev.

**Agent Login System** — Lightweight SQLite database where every agent logs:
- Agent name
- Task description
- AI model used
- Status (completed/pending)
- Timestamp

This provides full transparency on agent activity.

**Router Cheat Sheet** — Natural language routing where orchestrator reads intent and dispatches to correct agent. Plus slash commands (`/dev`, `/quiz`, etc.) for direct agent access.

### Mission Control Dashboard
- Overview: subjects, notes, quizzes, task counts
- Agent breakdown: activity distribution, 7-day heatmap, 30-day log
- Activity tab: detailed logs, task distribution, AI models used
- Chat section: direct chat with any agent from the dashboard
- Upload tab: triggers full pipeline
- Library: structured notes organized by subject
- Research tab: triggers Scholar for deep research
- Practice tab: interactive quizzes from Quiz Master
- Planner tab: daily digest, timetable, deadlines, exams

### Messaging Setup
- **Telegram** — Primary channel for orchestrator (Bill)
- **Discord** — Each agent gets own channel; one bot token for all agents (simpler than Telegram's per-agent tokens)
- Having two platforms provides redundancy — if one goes dead, use the other to debug

---

## Installation & Setup

### Infrastructure Options
1. **Home PC / Mini PC / Raspberry Pi** — Flash with Ubuntu, run locally
2. **VPS** (~$4-7/month) — Recommended for reliability and security isolation

### One-Line Installer
```bash
# From hermes.nousresearch.com (Linux tab)
curl -sSL https://... | bash  # handles all dependencies
```

### Model Configuration
- **Free tier:** Hermes 3.76 Flash (via Nous Research)
- **Best value:** OpenAI Codex through ChatGPT subscription ($20/mo Plus or $100/mo Pro) — use Codex models on Hermes at no extra cost
- This is a legitimate, sanctioned integration

### Gateway as System Service
Install messaging gateway as a system service that starts on boot — critical for VPS deployments so agents stay alive after reboots.

---

## Lessons from Production Agent Systems

### From Nick Nisi (WorkOS) — "Building AI Systems That Ship"

**The Harness Pattern** — A multi-agent system with gates between stages:
- Implementer → Verifier → Reviewer → Closer → Retrospective
- **The gates matter more than the agents.** Verification between stages prevents compounding errors.

**Agents Lie** — They will skip work and claim it's done. Solutions:
- Cryptographic proof: SHA-256 test output, save hash, verify later
- Make it easier to do the work than to fake it
- Don't ask nicely — make them prove it

**Retrospective Agent** — Analyzes all logs after completion, updates memory system so next run avoids known roadblocks. Self-improving loop.

**Context Drop** — As complexity grows, agents start forgetting or skipping tasks. Solution: TypeScript state machine to enforce step progression rather than relying on agent memory.

**Key Insight:** "I haven't written a line of code myself in probably eight months. I've gotten really good at just scaling that with agents and then reviewing what they do."

---

## Design Principles

1. **Hermes as operator, not builder** — Use Claude/Codex to create tools; use Hermes to run them
2. **Scope agent access carefully** — Don't give sensitive information; use read-only where possible
3. **Keep responses short** — Longer messages = more tokens. Instruct agents to be concise.
4. **Break complex tasks into steps** — Agents should report progress at each step for transparency
5. **Session continuity** — Agents should remember previous conversations and build on them
6. **Self-improving loops** — Rating results, retrospective analysis, memory updates
7. **Redundant messaging** — Use 2+ platforms (Telegram + Discord) for fault tolerance
8. **Agent logging** — Every action logged to database for accountability and debugging
9. **Verify, don't trust** — Gates, cryptographic proof, evidence requirements between agent stages
10. **Find the bottleneck** — Automate whatever takes your time, even social media scrolling

---

## Sources

- *15 Hermes Agent use cases I wish I tried sooner* — [YouTube](https://youtu.be/gpJNLgv3vdw)
- *Hermes AI Study Companion — 6 Agents + Mission Control* — Larry's tutorial
- *How I deleted 95% of my agent skills and got better results* — Nick Nisi, WorkOS

### Hermes Bible: You Can Copy REAL Agent Workflows

- **Source:** https://youtu.be/t_GxN2Gwqsk?si=jGr3fhkyGmQgpuJS
- **Duration:** 10:53 (652.5s)

### Key Points

- **[00:01]** Okay, folks, we've been preaching since day one that the best way to work with
- **[00:22]** whatever workflow you want to set up with your Hermes agent, they can always
- **[00:44]** that you can draw inspiration from or even copy. You can check on next, you
- **[01:06]** so that other people can copy or draw inspirations from. So, it's not
- **[01:26]** going to walk through the Hermes Bible. We're going to talk about some of the
- **[01:44]** Hermes agent workflow from day to day. But, what I noticed is
- **[02:06]** uh your you know, until you interact with your Hermes agent. So, what you can
- **[02:28]** basically, even if I close this, right? I go back
- **[02:54]** You can type tmux ls and you'll see the tmux sessions that are currently active.
- **[03:17]** session. So, this is very useful if you're running loops, if you have an
- **[03:39]** you're using the app, uh it's a full tour of the desktop app. It contains
- **[03:59]** backend, which we in fact did a video guide on how to do that. You can check
- **[04:18]** well. It's like a complete roadmap of your mastery of Hermes agent. So, level
- **[04:44]** using MCPs. Level six is the builder tier. So, scheduled and async tasks.
- **[05:07]** that you can make use of. And it goes all the way up to level 14. And I find a
- **[05:30]** but much more so for the agents.md and the user.md. Now, the slash goal is
- **[05:51]** measurable goals, and the recommended workflows. There's also a companion flow
- **[06:10]** before you go ahead and start a new session, make sure you start a new
- **[06:31]** already a loop system that you can run. So,
- **[06:53]** LLM Wiki as well, and then you'll get a full output every
- **[07:17]** You'll need another agent to verify that the work is in fact
- **[07:35]** easy mundane tasks that takes a lot of time if you were to do it yourself. But,
- **[07:56]** run those uh coding projects. Now, if you're an
- **[08:16]** tickets, then the coding agents writes the implementation, review agent audits
- **[08:37]** agent-to-agent coordination. So, the agents use the Kanban board to claim
- **[08:56]** on X and you don't want to pay with API key, but you are paying for X Premium,
- **[09:19]** loops, there's a community member that recommends eight loops that you can run
- **[09:43]** sub-agent loop for your Hermes agent. So, uh what you can do is you can in
- **[10:03]** note every 30 minutes or every hour, that's also another loop use case that
- **[10:26]** every time you need help for those workflows that you're running. So, if
- **[10:44]** an even more workflow examples that we can copy. All right? So, if you find

---
