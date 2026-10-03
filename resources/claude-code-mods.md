# Claude Code Mods — Important Improvements for Claude

> **Source:** https://claude.com/blog/claude-code-mods
> **Title:** Customize Claude Code with mods in TypeScript
> **Date:** October 1, 2026 (Anthropic official)
> **Category:** Product announcements · Claude Code
> **Saved:** 2026-10-02

---

## What Are Mods

**Mods are small TypeScript functions that change how Claude Code works.** A mod can:
- Rewrite a prompt
- Add new UI
- Replace a built-in feature
- Add entirely new functionality

You can write a mod yourself, or **ask Claude Code to write one for you**. Mods ship inside plugins — install and share like any plugin. They work in the Claude Code **CLI and desktop app**.

**⚠️ Security note:** Mods run with the same access to your machine as Claude Code itself. They are **not sandboxed** — only install mods from sources you trust, the same way you'd install any code on your computer.

---

## Why Mods Exist (vs Hooks)

Hooks gave users some control, but hooks **cannot**:
- Rewrite events
- Draw new UI
- Replace features

**Mods can.** The design was shared on GitHub before launch for developer feedback.

---

## How Mods Work

Every Claude Code action emits an event (tool call, permission request, screen draw). A mod hooks into one of these events and can:
- Run **before** the event
- Run **after** the event
- Run **instead of** the event
- **Wrap** the event (before + after)

### Single-Mod Capabilities
| Capability | Example |
|---|---|
| Rewrite prompts | Intercept before model sees it |
| Control tool calls | Block, rewrite, or retry |
| Permission management | Approve or deny requests |
| Security | Redact secrets from tool output before Claude reads it |
| UI manipulation | Edit/replace parts of the interface Claude Code draws |
| Interactive UI | Add buttons and inputs; other mods respond when pressed |
| Multi-target | Terminal, desktop app, or both |

### Mod Stacking
When several mods hook the same event, they run in **load order**:
- First mod to load sees the event **first** and the result **last**
- Lets you stack mods from different authors

### Self-Modding
**Claude Code can mod Claude Code.** Ask Claude to create a mod — it writes the TypeScript, installs it, and **hot reloads it in your session**.

---

## Replacing Built-In Features

Some built-in features now ship as mods:
- **`/diff`** is now a mod — turn it off in `/plugin` or replace with your own version
- **Roadmap:** More built-ins will move to mods over time → **pare Claude Code down to a small core and add back only what you want**

---

## Team & Enterprise Features

### Plugin Controls
- Mods ship inside plugins → existing plugin controls apply
- Admins can allow or block plugin marketplaces
- **Team/Enterprise plans:** Owner sets this in admin console
- **API/third-party API plans:** Admins push managed settings to users' machines

### Security Default Mod
On Team/Enterprise plans and machines with managed settings:
- Built-in **`sec-default`** ("security default") mod loads **first**
- Stops mods from doing risky things (e.g., overriding permission deny rules)
- View source: `github.com/anthropics/claude-code/tree/main/mods`
- Admins can load their own mods first instead — if you do, add `sec-default` to your list to keep its restrictions

### Team Use Cases
| Use Case | What the Mod Does |
|---|---|
| **CI/CD status** | Shows pipeline status in a pane beside the conversation; updates as builds pass/fail |
| **Production safeguards** | Requires confirmation before any command touches production config |
| **Audit logging** | Mod that loads first records every call that every other mod makes |

---

## Getting Started

- **Available today** in Claude Code CLI and desktop app
- **Install:** From the [Claude directory](https://claude.ai/directory) or run `/plugin` in the CLI
- **Share:** Package in a plugin and submit to the directory
- **Guide:** [Getting started with Claude Code mods](https://claude.dev/blog/getting-started-with-claude-code-mods/)
- **Docs:** [code.claude.com/docs/en/plugins/mods/overview](https://code.claude.com/docs/en/plugins/mods/overview)

---

## Related Anthropic Posts (Sep 2026)

| Date | Post |
|---|---|
| Sep 25, 2026 | [Build plugins for Claude](https://claude.com/blog/build-plugins-for-claude) |
| Sep 23, 2026 | [Claude Marketplace: plugins, agents, services from partners](https://claude.com/blog/claude-marketplace) |

---

*Distilled from official Anthropic blog. For personal reference — Claude Code improvements overview.*
