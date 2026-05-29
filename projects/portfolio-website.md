# Portfolio Website — Client Showcase

**Date:** 2025-05-29
**Status:** 💡 Idea
**Tags:** #portfolio #website #clients #showcase

## Concept

A single portfolio website that showcases all projects in one place. One link to send clients so they can see the full body of work — FonixFlow, AI demos, 3DGS, virtual production, etc.

## Why

- Professional single-link sharing with clients
- No need to send multiple URLs for different projects
- Demonstrates range and capability in one view
- First impression matters — curated portfolio > scattered links

---

## Projects to Showcase

### 1. FonixFlow
- **Live:** fonixflow.com + ai.fonixflow.com
- **What:** AI-powered booking/flow platform
- **Show:** Live link, screenshots, feature highlights

### 2. 3DGS / Gouging Splat
- **Live:** splat.steadiczech.com
- **What:** 3D Gaussian Splatting — image processing, training, gallery
- **Show:** Embedded viewer, before/after renders, pipeline overview

### 3. AI/ML Work
- **What:** Hermes Agent, autonomous pipelines, LLM tooling
- **Show:** Demo videos, architecture diagrams, capability descriptions

### 4. Virtual Production
- **What:** Sony DMPC lab — LED volume, real-time rendering, camera tracking
- **Show:** Reel/clips, behind-the-scenes, technical breakdown

### 5. Open Design
- **What:** Open-source design tool / creative workflow
- **Show:** Live demo link, screenshots

---

## Proposed Stack

**Recommended: Next.js + Static Export (SSG)**

- Already have Next.js experience (Project Tracker)
- Static export = free hosting on Cloudflare Pages / Vercel / GitHub Pages
- Fast loads, good SEO, no server costs
- Easy to add dynamic stuff later if needed

**Structure:**
```
portfolio/
├── app/
│   ├── page.tsx              # Hero + project grid
│   ├── projects/
│   │   ├── page.tsx          # All projects overview
│   │   ├── fonixflow/
│   │   ├── 3dgs-splat/
│   │   ├── ai-ml/
│   │   ├── virtual-production/
│   │   └── open-design/
│   └── contact/
├── public/
│   ├── images/               # Project screenshots, hero images
│   └── videos/               # Demo clips, reels
└── components/
    ├── ProjectCard.tsx
    ├── VideoEmbed.tsx
    └── Gallery.tsx
```

## Pages

| Page | Content |
|------|---------|
| **Home** | Hero (name, title, one-liner), project grid (4-6 cards), CTA to contact |
| **Project Detail** | Hero image/video, description, tech stack, live link, screenshots gallery |
| **About** | Brief bio — VP Supervisor, 20+ yrs, Sony DMPC, AI/ML focus |
| **Contact** | Email, GitHub, LinkedIn — simple form or just links |

## Design Direction

- **Dark theme** — fits the tech/creative aesthetic
- **Minimal** — let the work speak, not the UI
- **Visual-first** — hero images, embedded 3D viewer, video clips
- **Fast** — no bloat, Lighthouse 95+
- Inspiration: linear.app, vercel.com, raycast.com (clean, dark, confident)

---

## Implementation Plan

### Phase 1 — Scaffold & Home (1-2 hrs)
- [ ] `npx create-next-app@latest portfolio` with App Router + Tailwind
- [ ] Dark theme setup, global styles, fonts
- [ ] Home page: hero section + project card grid
- [ ] Responsive layout (mobile-first)

### Phase 2 — Project Pages (2-3 hrs)
- [ ] Project detail page template
- [ ] Fill in content for all 5 projects
- [ ] Add screenshots/images for each
- [ ] External links to live demos

### Phase 3 — Polish (1-2 hrs)
- [ ] About page
- [ ] Contact section (email link + GitHub + LinkedIn)
- [ ] Smooth scroll, transitions, hover effects
- [ ] Meta tags, OG images for link previews
- [ ] Favicon

### Phase 4 — Deploy (30 min)
- [ ] Push to `lcevelik/portfolio` GitHub repo
- [ ] Deploy to Cloudflare Pages (already on Cloudflare for FonixFlow)
- [ ] Point domain (cevelik.com? libor.dev? or subdomain)

---

## Domain Options

| Domain | Pros | Cons |
|--------|------|------|
| `cevelik.com` | Professional, matches name | Need to register |
| `libor.dev` | Clean, dev-focused | Need to register |
| `portfolio.steadiczech.com` | Free, uses existing domain | Less clean for clients |
| `lcevelik.github.io` | Free, instant | Less professional |

**Recommendation:** Register `cevelik.com` — most professional for client-facing work.

---

## Open Questions

- Which domain to use?
- Any NDA projects that need to be excluded or gated?
- Want embedded 3DGS viewer or just screenshots?
- Video reel — have existing footage or need to create?
- Auto-update from repos or manual content management?
