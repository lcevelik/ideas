# Image Pipeline: Sort → PLY → Gallery

**Date:** 2026-05-29
**Status:** 💡 Idea
**Tags:** #3dgs #photogrammetry #gallery #pipeline

## Concept

Unified pipeline that combines three existing workflows into one:

1. **Pic Sorting** — Select and curate source images from a larger set
2. **Image → PLY** — Convert sorted images to PLY point cloud format (photogrammetry / 3DGS)
3. **Web Gallery** — Browse results in a web-based gallery viewer

Instead of running these as separate tools, integrate them into a single cohesive system.

## Why

- Currently these are disconnected steps — sorting, converting, viewing
- Eliminates manual file shuffling between stages
- Gallery provides immediate visual feedback on the pipeline output
- Could serve as a quick validation loop for 3DGS quality

## How (Rough)

- **Sort layer:** Drag-and-drop UI or CLI to tag/select best images from a capture set
- **Conversion layer:** Feed sorted images into image-to-PLY pipeline (COLMAP, gsplat, etc.)
- **Gallery layer:** Web viewer showing source images alongside resulting PLY/splat renders
- Possible tech: Next.js or simple static gallery + Three.js/PlayCanvas for 3D preview

## Open Questions

- What's the current "pic sorting" workflow? Manual folder moves or something smarter?
- Is this for 3DGS specifically or general photogrammetry?
- Should gallery support live 3D splat rendering or just static previews?
- Single-user tool or multi-user/collaborative?
