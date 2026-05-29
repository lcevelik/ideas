# Unreal GSplat Generator Plugin — Text/Image to 3D Gaussian Splat

**Date:** 2025-05-29
**Status:** 💡 Idea
**Tags:** #unreal #plugin #3dgs #ai #generative #virtual-production

## Concept

An Unreal Engine plugin that creates 3D Gaussian Splats from text or images, directly inside the editor:

**Text-to-Splat flow:**
1. User enters text prompt in Unreal
2. Plugin enhances the prompt (LLM/improves detail)
3. AI generates an image from the enhanced prompt
4. Image is converted to a 3D Gaussian Splat
5. Splat is displayed live in the Unreal viewport

**Image-to-Splat flow:**
1. User provides image(s) as input
2. Plugin converts images to 3D Gaussian Splat
3. Splat is displayed live in the Unreal viewport

## Why

- **Zero context switching** — artists stay in Unreal, no external tools
- **Democratizes 3DGS** — non-technical users can generate splats from text
- **VP workflow** — instantly populate virtual sets with AI-generated 3D content
- **No one has this** — existing 3DGS tools are all standalone, none are Unreal-native

## Technical Architecture

```
┌─────────────────────────────────────┐
│         Unreal Engine Plugin        │
├─────────────────────────────────────┤
│                                     │
│  [Text Input]  or  [Image Input]    │
│       │                 │           │
│       ▼                 │           │
│  Prompt Enhancement     │           │
│  (LLM rewrite)          │           │
│       │                 │           │
│       ▼                 ▼           │
│  Image Generation ──────┘           │
│  (SDXL / Flux / API)               │
│       │                             │
│       ▼                             │
│  Image → 3DGS Conversion            │
│  (instantSplat / GaussianAnything)  │
│       │                             │
│       ▼                             │
│  GSplat Renderer                    │
│  (Unreal viewport)                  │
│                                     │
└─────────────────────────────────────┘
```

## Key Components

### 1. Prompt Enhancement
- Take basic text input, expand with detail (style, lighting, material)
- Could use local LLM or API call
- Example: "red sports car" → "A sleek red sports car, studio lighting, metallic paint, 3/4 angle view, high detail, photorealistic"

### 2. Image Generation
- **Option A:** API-based — Stability AI, Replicate, OpenAI DALL-E
- **Option B:** Local — SDXL/Flux via ComfyUI or diffusers
- **Option C:** Hybrid — local for dev, API for production

### 3. Image → 3DGS Conversion
- instantSplat / GaussianAnything / LGM
- Single-image-to-3D models getting better fast
- Multi-image input = higher quality if available
- Run locally (GPU) or via API

### 4. Unreal Integration
- Custom asset type for GSplat in Content Browser
- Viewport renderer (custom mesh/shader or plugin existing viewer)
- Drag-and-drop placement in scenes
- Material/lighting interaction with Unreal lights

## Input Modes

| Mode | Input | Quality | Speed |
|------|-------|---------|-------|
| Text only | Prompt | Lower (AI guess) | ~30-60s |
| Text + reference image | Prompt + image | Medium | ~30-60s |
| Image only | Single image | Medium | ~15-30s |
| Multi-image | 2-10 images | High | ~1-3 min |

## Use Cases

- **Virtual Production** — "Add a marble statue stage left" → instant splat in scene
- **Pre-viz** — quickly populate sets from text descriptions
- **Art direction** — iterate on 3D assets from text prompts
- **Set dressing** — image of real object → 3DGS asset in Unreal

## Implementation Plan

### Phase 1 — Proof of Concept
- [ ] Unreal C++ plugin skeleton
- [ ] Text input UI (simple editor panel)
- [ ] API call to image generation (Replicate/Stability)
- [ ] Image → 3DGS conversion pipeline
- [ ] Basic GSplat display in viewport

### Phase 2 — Polish
- [ ] Prompt enhancement with LLM
- [ ] Image input support (drag-drop)
- [ ] Multi-image input
- [ ] Content Browser integration (GSplat asset type)
- [ ] Save/load generated splats

### Phase 3 — Production
- [ ] Local inference option (no API dependency)
- [ ] Batch generation
- [ ] Scene integration (lighting, shadows, collisions)
- [ ] Settings panel (quality presets, model selection)
- [ ] Marketplace listing

## Open Questions

- Which image-to-3DGS model to use? (instantSplat, GaussianAnything, LGM, others)
- Local inference vs API — which for MVP?
- Unreal version target? (5.4+ has Nanite, could complement)
- Rendering approach — custom GSplat renderer in Unreal or convert to mesh?
- Distribution — Fab marketplace, Gumroad, or private licensing?
- How does this fit with SplatCloud? Could be the "creator tier" product
- Performance — can real-time GSplat rendering hit 30fps in Unreal viewport?
