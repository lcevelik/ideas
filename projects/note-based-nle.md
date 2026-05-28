# Note-Based NLE

**Status:** Idea
**Tags:** #nle #video #editing #ai #notes

## What
A non-linear video editor built around a note-based interface instead of (or alongside) a traditional timeline. Think Obsidian/Notion meets Premiere — your edit lives as structured notes, not just clips on a timeline.

## Why
- Traditional NLEs force you into timeline-first thinking
- Notes capture *intent* and *story* — timelines capture *sequence*
- AI-assisted editing is easier when your edit is structured data, not GUI state
- Collaboration is simpler when edits are text/markdown
- Version control for edits (git-friendly)

## How
- Notes as primary interface: each scene/beat/segment is a note block
- Markdown-based edit decision list (EDL)
- Timeline view is a *render* of the notes, not the source of truth
- AI can suggest restructures, pacing changes, alt takes based on note metadata
- Media references embedded in notes (timecodes, file refs)
- Export to traditional EDL/XML/AAF for handoff

## Open Questions
- How to handle precise timing/trimming in a note-based model?
- Hybrid mode: notes for story structure, timeline for fine cuts?
- Integration with existing NLEs (Premiere, Resolve) vs standalone?
- How does AI fit in — auto-generate notes from footage? Suggest edits from notes?

## Next Steps
- [ ] Research existing note-based editing tools
- [ ] Define core data model (note schema, media refs, timecodes)
- [ ] Prototype: markdown → simple timeline render
- [ ] Explore AI integration points
