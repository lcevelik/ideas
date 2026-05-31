# YouTube AI Video Scraper

**Date:** 2026-05-31
**Status:** 💡 Idea
**Tags:** #scraper #youtube #ai #automation #content-curation

## Concept

A script that scrapes YouTube search results for AI-related keywords and collects video links, titles, channel names, view counts, and upload dates. Goal: automated discovery of relevant AI content without manually browsing YouTube.

## Why

- YouTube's algorithm buries niche AI content under mainstream recommendations
- Useful for tracking the fast-moving AI/LLM tooling space
- Could feed into a daily/weekly digest or RSS-like pipeline

## Search Keywords

**Core AI coding:**
- "vibe coding"
- "Claude Code"
- "Cursor AI"
- "AI coding assistant"
- "AI pair programming"

**Generative AI:**
- "Generative AI"
- "AI generated"
- "text to image"
- "AI art"
- "ComfyUI"
- "Stable Diffusion"
- "Midjourney"

**Local/Open AI:**
- "local AI"
- "run AI locally"
- "ollama"
- "llama.cpp"
- "open source LLM"
- "self-hosted AI"
- "AI on home server"

**AI Agents & Tools:**
- "AI agent"
- "autonomous AI"
- "AI workflow"
- "MCP protocol"
- "function calling"
- "agentic"

**AI News & Research:**
- "AI breakthrough"
- "LLM benchmark"
- "AI model release"
- "GPT-5"
- "Claude 4"
- "Gemini"

## How

1. **Search layer:** Use `yt-dlp --flat-playlist` or YouTube Data API v3 to search by keyword
2. **Filter:** Deduplicate by video ID, filter by recency (last 7/30 days), min view count
3. **Output:** Save results to JSON/Markdown with video URL, title, channel, views, date
4. **Scheduling:** Cron job to run daily/weekly
5. **Optional:** Push results to Telegram or RSS feed

### Stack options
- `yt-dlp` (no API key needed, but can get rate-limited)
- YouTube Data API v3 (needs API key, 10K units/day free)
- `scrapetube` Python library (lightweight, no API key)

## Open Questions

- How to handle rate limiting / YouTube blocking?
- Should it store history to only surface NEW videos?
- Output format: JSON, Markdown, or feed into another system?
- Do we want transcripts too, or just metadata?
