# AI Tools & GitHub Repos - June 2026

> Curated from transcripts covering trending GitHub repos, AI coding tools, and creative AI workflows.

---

## 🔥 Trending GitHub Repos (June 2026)

### 1. Taste Skill
- **Author:** Leon XLX
- **Stars:** 42,000+
- **What it does:** Collection of portable agent skills that stop AI from generating generic "bootstrap template" output. Focuses on layout, typography, and motion design.
- **Why it matters:** Bridges the gap between high-end design and raw AI generation. Works with Claude Code and Cursor to generate reference boards, brand kits, and mobile layouts. Basically a "taste transplant" for LLMs — inject design sensibility into vibe-coded projects.
- **Use case:** Generate design references and hand directly to Claude Code/Cursor for implementation.

### 2. MarkItDown (Microsoft)
- **Author:** Microsoft (same team behind AutoGen)
- **Stars:** 151,000+
- **What it does:** Python utility that converts messy office documents (PDFs, PowerPoint, Word, Excel) into clean Markdown. Includes OCR for images and audio transcription.
- **Why it matters:** Local LLMs and RAG systems need structured data. MarkItDown preserves tables and links — most converters turn them into chaotic nonsense. Essential for feeding enterprise documents into AI systems.
- **Use case:** Feed legacy enterprise docs into local RAG pipelines without losing structure.

### 3. CopilotKit
- **Stars:** 2,700+ (this week alone)
- **What it does:** Front-end stack for building generative UI. Works with React, Angular, and mobile. Includes the AGUI protocol for shared state and human-in-the-loop workflows.
- **Why it matters:** Lets AI actually interact with your app — toggling switches, moving elements, responding to natural language. Builds sidebars and canvas actions that feel native, not tacked-on chat windows.
- **Use case:** Build dashboards where AI controls UI elements via natural language.

### 4. Goose
- **Organization:** Linux Foundation (recently moved)
- **Stars:** ~50,000
- **What it does:** Open-source, extensible AI agent that runs locally. Installs packages, executes scripts, edits files, runs tests independently. Written in Rust for speed and safety.
- **Why it matters:** Not just for code — includes tools for research, data analysis, and general automation. A pair programmer with permission to fix tests and deploy while you grab coffee.
- **Use case:** Local, private autonomous agent for development and automation.

### 5. Career Ops
- **Author:** Santifer
- **Stars:** 53,000+
- **What it does:** AI-powered job search system built on Claude Code. 14 skill modes, Go dashboard for tracking, PDF generation, and batch application processing. Interview prep mode that studies job descriptions.
- **Why it matters:** Levels the playing field against automated hiring systems used by big tech. Fights AI filtering with AI.
- **Use case:** Automated job search, application tracking, and interview prep.

### 6. Code Graph
- **Author:** Colby McHenry
- **Stars:** 46,000+ (this month alone!)
- **What it does:** Creates a pre-indexed knowledge graph of your local codebase for AI agents (Claude Code, Cursor, Gemini, etc.). Version 1.0 just released.
- **Why it matters:** Makes AI tasks 16% cheaper and 58% fewer tool calls. Gives the AI a semantic map of your codebase before it starts working. 100% local.
- **Use case:** Perfect for legacy codebases where file structure is a nightmare. AI already knows function/class relationships.

### 7. Understand Anything
- **Stars:** 58,000+
- **What it does:** Turns codebases or documentation into interactive knowledge graphs you can talk to. Focus on graphs that *teach*, not just look impressive.
- **Why it matters:** Works with every major AI CLI tool (Gemini CLI, Claude Code). Focus on memory and business knowledge — helps people understand systems rather than replacing developers.
- **Use case:** Onboarding new team members — hand them the graph and let them ask where business logic is hidden.

### 8. Money Printer Turbo
- **Stars:** 86,000+ (~30,000 new this month)
- **What it does:** High-speed AI tool for generating short-form videos with one click. Provide a theme/keyword → handles script, footage, subtitles, and background music.
- **Why it matters:** Uses MoviePy under the hood with LLM coordination. Content factory in a box for TikTok/YouTube Shorts scaling.
- **Use case:** Scale niche content channels with automated video production.

---

## 🛠️ AI Coding Tools

### Claude UltraCode
- **What it does:** Creates up to 10 parallel agents with a deterministic fan-out strategy. A "boss" script manages specialist agents instead of a managing LLM.
- **Key strategies:** Adversarial verify, judge panel, pipelines, loop-until-dry.
- **How to activate:** Use the word "ultra code" in your Claude Code prompt.
- **Why it matters:** Replaces the managing LLM with code for more reliable orchestration. Quality doesn't decay like pure agent-managed systems.

### Kimi 2.7 Code (Moonshot AI)
- **What it does:** Fast, efficient, cheap coding model with 260K context window. Slightly more expensive than Kimi 2.6 but significantly more capable.
- **Why it matters:** Strong contender as a budget alternative to Opus. Known for speed and efficiency.
- **Cost:** ~$15/month range, extremely cost-effective.

### Unsloth Studio
- **What it does:** Free, open-source tool for fine-tuning LLMs locally. Also supports chatting with models (competitor to Ollama/LM Studio). Built by former Nvidia engineer.
- **Key features:** One-line install, dataset creation tools, local fine-tuning, model chat interface.
- **Why it matters:** Makes fine-tuning accessible — run a small LLM that outperforms models 100x bigger. Cut API costs to near zero.

---

## 🎬 Creative AI Tools

### ComfyUI
- **What it does:** Free, open-source visual node-based interface for AI workflows — images, videos, audio, 3D models, and LLMs.
- **Key concepts:**
  - Nodes perform specific tasks; connect them to build workflows
  - Workflows are shareable and customizable
  - Built-in template library (text-to-image, image-to-video, image editing, text-to-song, image-to-3D)
  - Desktop (free, local) and cloud versions available
- **Setup:** Download from comfyui.com → install → select "local" → name installation → download required models
- **Why it matters:** Most flexible AI content creation tool. Community-driven with massive workflow library.

---

*Sources: YouTube transcripts, June 2026*

### Claude Code Just Made Fine-Tuning AI Images Stupid Easy!

- **Source:** https://youtu.be/wZYD-zTKe7Y?si=MhigZMwhMqzPsXH0
- **Duration:** 19:02 (1141.5s)

### Key Points

- **[00:01]** AI image models are incredible at giving you something new every time you prompt
- **[00:41]** this video, we are going to create our own custom Laura, which means Laura and
- **[01:21]** models, encode text prompts, set image dimensions, run the diffusion sampling
- **[02:01]** since you have full control over this process. And since I'll be showing you
- **[02:39]** you're a complete beginner. And if you follow along and actually watch until
- **[03:17]** another link in the description, and you can just scroll down here to install
- **[03:52]** Cloud Code installed. So, what we're going to do now is just open up ComfyUI,
- **[04:29]** can select that. And then {slash} effort, we can change it to whatever we
- **[05:03]** using throughout this video and paste them into your workflows. Okay, now we
- **[05:38]** and we're just going to send that off. Now, once that's sending, let me explain
- **[06:18]** node types, and now it's writing the adapted skill. And as you can see,
- **[06:54]** offer them this kind of a service. Again, there's lots of different use
- **[07:30]** off the prompt I just showed you a minute ago, you can type into the
- **[08:04]** but you can just click down here on the download button, which will install Koh
- **[08:41]** in this red box. If you're using the same exact model that I'm using, you
- **[09:19]** images with the trained LoRA locally on my Mac. Basically, handle the whole
- **[10:00]** lighting, style, clothes, etc. Because inside of a LoRA, there are billions of
- **[10:39]** product as possible. So then we can add a trigger word which helps the model
- **[11:19]** change the LoRA by a tiny step until eventually it basically has as little
- **[11:56]** you to connect to external services. So for this use case, we're going to use it
- **[12:32]** file, I'm going to say, "Open the file for me where I need to paste my API
- **[13:06]** have a readme file right here. This is basically instructions for you the human
- **[13:46]** detailed instructions and explain to it what you're struggling with. There's no
- **[14:26]** click queue. But there is no queue. So, I have a question, so I'm going to
- **[15:00]** the console on the left side bar near the bottom. Okay, so as you can see, we
- **[15:39]** top-up enabled, it's not going to just drain your wallet. It might drain your
- **[16:16]** don't know. But doing this on cloud for 10 to 20 minutes for $2 is definitely
- **[16:51]** workflows for you. And as you can see, I'm also just following what Cloud Code
- **[17:36]** machine, so we can see which node is actually being worked on in real time.
- **[18:12]** suit, I'll be honest, but this isn't too much of the fault of the Laura. This is
- **[18:49]** any of these nodes because cloud code just simply don't order that for us.

---
