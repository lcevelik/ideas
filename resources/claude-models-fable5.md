# Claude Fable 5 (Mythos) — Key Insights from YouTube Transcripts

## What Is Fable 5?

- **Claude Fable 5** (aka Claude Mythos 5) is Anthropic's most powerful model — the "safe" public version of the previously locked-down Mythos model.
- Same brain as Mythos 5, with safety rails added (may silently downgrade to Opus 4.8 if safeguards trip).
- Wins/dominates all major AI benchmarks.
- **1 million token context window**, built-in vision.
- Can run autonomously for days on long-running tasks.

## Availability & Pricing

- Included in Claude paid subscriptions **until June 22–23, 2026**, then moves to API-only (usage credits).
- **$50 per million output tokens** — double the cost of Opus 4.8.
- First model from a frontier lab not permanently available in subscriptions — signals the shift toward pay-per-use for top-tier models.
- Anthropic added **mandatory 30-day data retention** for Fable 5 (even API usage), which may deter enterprise clients.

## Effort Levels & Usage

- Use **`/model`** in Claude Code CLI to switch to Fable 5 (even if not visible in picker, run `/model claw-fable-5`).
- **Effort levels**: Start on **high** (recommended). Extra high and max available but burn significantly more tokens.
- Use **auto mode** (Shift+Tab) — Fable is smart enough to not need constant permission prompts.
- **Opus 4.8 fast** costs the same as Fable but is much faster — use it when speed matters and Fable's extra intelligence isn't needed.

## Paradigm Shift: How to Think About Fable 5

### From "Is Claude doing the work right?" → "Is Claude doing the right work?"
- Previous models needed micro-management and step-by-step guidance.
- Fable 5 can be trusted to build things correctly — now focus on ensuring it's solving the right problem.

### Anthropic's Three Tips
1. **Treat Claude like a thought partner** — not a subordinate. Ask "what are your thoughts?" before building.
2. **Give Claude goals, not steps** — provide high-level objectives and let it find the path. Use loops for autonomous operation.
3. **Be more ambitious** — push limits. Things previously impossible with AI are now achievable.

### Prompting Philosophy
- **Don't micromanage.** Give goals, constraints, and success criteria, then let it run.
- **Delete verbose scaffolding prompts** from previous models — they now degrade output.
- **Be more vague about implementation, but clear about the desired outcome.** The model is likely smarter than you at code architecture.
- **Add a scope guard**: "Simplest solution that works. No extra refactoring."
- Fable **pushes back on bad ideas** and identifies patterns you miss — first model that feels close to human-level intelligence to many users.

## Best Practices & Workflows

### Where to Use Fable 5
- **Best**: **Cursor** (agent view) and **Claude Code** (CLI) — both have built-in fallback to Opus 4.8 when safeguards trigger.
- **Avoid**: Raw API (no fallback mechanism), Claude.ai app (most guardrails/limitations).
- **Avoid using for trivial tasks** — don't burn Fable tokens on renaming files or simple edits. Use Opus 4.8 or Sonnet for those.

### Loops & Autonomous Operation
- **`/goal`** command: Paste a comprehensive prompt with success criteria → agent loops until goal is met.
- **`/loop`** command: e.g., `/loop 1 hour check for new Linear tasks` — scheduled autonomous work.
- Fable can work autonomously for hours/days — let it work overnight for complex projects.

### Advanced Plan Mode (Recommended Pre-Build Workflow)
1. Describe what you want to build.
2. Prompt: *"Before building anything, please ask me any questions you have on this idea so we can build a fully fleshed out app."*
3. Answer Fable's interview questions (it asks detailed clarifying questions).
4. Use the resulting comprehensive spec as a `/goal` for autonomous building.
5. This ensures Fable has full understanding before starting the build loop.

### Multi-Agent Workflows
- Run multiple Fable agents in parallel across different projects (Cursor agent view, multiple Claude Code terminals).
- Tools like **Tank** (open-source Claude Code orchestrator using Tmux) enable running many agents simultaneously with model selection per task.
- **Dynamic workflows**: Fable can spin up sub-agents (scope, search, fetch, verify, synth) for deep research before building.

## Guard Rails & Safeguards

- Fable **triggers safety rejections more aggressively** than Opus — even benign prompts can get flagged.
- **Topics that trigger safeguards**: cybersecurity analysis, model distillation queries, anything resembling "tell me everything about this model."
- **Workaround for security reviews**: Run the analysis on Opus first, then pass Opus's findings to Fable in a new window with XML tags: *"What do you think of this? Would you implement this in our codebase?"*
- **Don't use via OpenRouter/API** for critical work — no graceful fallback, just errors on rejection.

## Cost Awareness

- Fable burns tokens at **~2x the rate of Opus 4.8** (some users report even higher).
- One user's Claude Max plan usage: **30% of 5-hour limit in a 44-minute live stream**, 11% of weekly balance.
- Estimated cost if paying full API rates: **~$93,000 for 1.86 billion tokens** at $50/M output tokens.
- Real-world monthly AI spend for power users: **$8,000–10,000/month** across subscriptions and API.
- **Use Fable strategically** for complex, high-value work. Use Opus 4.8 for everything else.

## What Fable 5 Excels At

- **One-shot complex applications**: Full productivity apps, 3D games, physics simulations, city traffic simulators.
- **3D graphics & modeling**: Best model for Blender/Maya workflows, 3D robot generation, mechanical assemblies.
- **Software cloning**: Can replicate any software from description in 1–2 prompts.
- **3D fluid dynamics & simulations**: Significantly more realistic than Opus 4.8, Gemini 3.1, or GPT 5.5.
- **Startup idea analysis**: Less agreeable, more objective — will push back on bad ideas, review through the lens of specific experts/founders.
- **Building app builders**: One-shot a Lovable clone (full app builder) in 2 prompts.
- **Deep research with dynamic workflows**: Spins up multi-agent research pipelines before building.
- **Self-verification**: Spins up Playwright browsers to visually verify what it built. Reads entire codebases before making changes.

## Limitations

- **AI/ML research**: Intentionally limited by Anthropic.
- **Safety triggers**: More aggressive than Opus — can downgrade mid-task.
- **Speed**: Much slower/more thorough than Opus — reads entire codebases, takes time to think.
- **Cost**: Prohibitive for casual use at API pricing.
- **30-day data retention**: Enterprise concern.
- **Not for trivial tasks**: Overkill for simple edits, file renames, basic code changes.

## Notable Quotes & Observations

> "This is perhaps the biggest improvement, the biggest model jump since the release of GPT-4." — power user testing Fable for 14 hours straight

> "Fable legit doesn't understand how powerful it is." — it suggests timelines of weeks/months for work it can do in hours.

> "The model is probably smarter than you. Just describe what type of app you want, how it should work, how it's going to be used, and let it figure out the details."

> "If you aren't prepared to spend multiple thousands of dollars per month for AI, you're not going to have an advantage." — on the emerging cost barrier

> "My job is just writing loops now and letting the agents loop." — Boris Cheney (creator of Claude Code)

## Game Development Showcase (Fable 5 + Opus 4.8)

- Built a complete **point-and-click adventure game** (Monkey Island style) using AI in roughly one day.
- **Workflow**: Concept art → AI-generated puzzle design → extract props/characters → sprite sheet generation → JSON-driven game engine → voice acting via ElevenLabs.
- **Key tools**: Image generation for concept art/state changes, video generation for sprite animations, Spriterific for automated sprite sheet creation, ElevenLabs for voice acting.
- **JSON-driven architecture**: Entire game state in a single JSON file — acts as both content definition and save file. Enables AI to modify game content without touching engine code.
- Game included: dialogue trees, inventory system, hotspot interactions, animated props, nav mesh, cutscenes, and full voice acting.

## Sources

1. [Claude Fable 5 just dropped and I'm speechless](https://youtu.be/RwsUnvHYmyw) — 12:57
2. [Claude Fable 5 — Mythos AI Goes Public (Mike Russell)](https://youtu.be/BxR-r4F4Pbw) — 1:12:00+ live stream
3. [Don't use Fable 5 in Claude… do this instead](https://youtu.be/BxR-r4F4Pbw) — 32:29
4. [Create a Monkey Island Game with AI (Claude Opus 4.8, Fable 5, Cursor)](https://youtu.be/3sg2aHYAruw) — 27:07
