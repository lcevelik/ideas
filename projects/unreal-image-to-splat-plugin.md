# Unreal Engine Image-to-Splat Plugin

**Date:** 2026-06-26
**Status:** 💡 Idea
**Tags:** #unreal-engine #3dgs #gaussian-splatting #machine-learning #virtual-production

## Concept

An Unreal Engine plugin that takes a single image as input and generates a 3D Gaussian Splat directly inside UE — no external pipeline, no manual processing. Drop an image, get a splat in your scene.

## Why

Current 3DGS workflows require:
- Multi-view capture or NeRF training
- External Python pipelines (COLMAP →训练 → export)
- Manual .ply/.splat file handling

This plugin would collapse that into a single step inside UE, making Gaussian Splatting accessible to artists and producers who don't want to touch Python.

## Research Findings

### Existing Single-Image-to-3DGS Methods

| Method | Source | Status | Notes |
|--------|--------|--------|-------|
| **Apple SHARP** | Apple Research | Paper + code | "Single-image to 3D Gaussian Splat" — depth-guided approach |
| **LiTo** | Apple Research (ICLR 2026) | Code + ComfyUI | Single-image to 3DGS generation |
| **Open-DiffusionGS** | ICCV 2025 | 852★ | Bakes GS into diffusion denoiser for fast single-stage generation |
| **TripoSplat** | — | Code | Image → 3D Gaussian Splat pipeline |

### Existing UE5 Gaussian Splatting Renderers

| Plugin | Stars | What it does |
|--------|-------|-------------|
| **GaussianSplattingForUnrealEngine** | 205 | Converts primitives into 3D Gaussians |
| **MLSLabsGaussianSplattingRenderer-UE** | 225 | Real-time 3DGS/4DGS rendering |
| **SplatRenderer-UEPlugin** | 59 | 3D/4DGS renderer for UE 5.5+ |
| **unreal-splat** | 21 | 3DGS rendering plugin |

### "ML-Sharp" Clarification Needed

The user mentioned "ML-Sharp" — possibilities:
- **Apple SHARP** (single-image to 3DGS research) — most likely
- **ML.NET** (Microsoft's ML framework for .NET/C#)
- **MLSharp** (basic C# ML library on GitHub, 0 stars — unlikely)

**→ Need to clarify with user which "ML-Sharp" they mean.**

## Architecture Options

### Option A: Python Backend + UE Plugin (Most Feasible)

```
[User drops image in UE]
        ↓
[UE Plugin sends image to local Python server]
        ↓
[Python: SHARP/LiTo/Open-DiffusionGS inference]
        ↓
[Returns .ply/.splat data]
        ↓
[UE Plugin loads into 3DGS renderer]
```

**Pros:** Use state-of-the-art models, GPU inference via PyTorch
**Cons:** Requires Python runtime, latency (~2-10s per image)

### Option B: ONNX Inference in UE (Ideal but Hard)

```
[User drops image in UE]
        ↓
[UE Plugin runs ONNX model directly]
        ↓
[Splat data generated in-engine]
        ↓
[Rendered immediately]
```

**Pros:** No external dependencies, instant
**Cons:** Model must export to ONNX (not all do), UE ONNX runtime limited

### Option C: Hybrid — ONNX for Core + Python for Fallback

```
[Image → ONNX depth estimation in UE]
        ↓
[If ONNX splat model available → direct]
[Else → Python backend fallback]
```

## Open Questions

- [ ] Which "ML-Sharp" does the user mean? Apple SHARP? ML.NET? Something else?
- [ ] Can SHARP/LiTo models export to ONNX for in-UE inference?
- [ ] What's the target latency? (Real-time vs. batch processing)
- [ ] Which UE version? (5.4+ has better plugin support)
- [ ] Should it work with existing .ply/.splat renderers or build a new one?
- [ ] What's the use case — virtual production, game dev, or visualization?

## How (Rough Implementation Plan)

### Phase 1: Proof of Concept (2-4 weeks)
1. Set up Apple SHARP / LiTo locally
2. Get single-image → .splat working
3. Build minimal UE5 plugin that loads .splat files
4. Wire up: image drop → Python call → splat in scene

### Phase 2: Integration (4-8 weeks)
1. Package Python backend as Windows service or bundled Python
2. Build UE5 editor UI (drag-drop, preview, settings)
3. Optimize pipeline latency
4. Add .ply/.splat format support

### Phase 3: Polish (4-8 weeks)
1. Real-time preview while processing
2. Batch processing support
3. Integration with existing 3DGS renderers
4. Marketplace submission

## Related Work

- Sony XYN 3DGS Pipeline (already researched — see `research/2026-06-14-sony-xyn-3dgs-pipeline.md`)
- Existing UE5 3DGS plugins can handle the rendering side
