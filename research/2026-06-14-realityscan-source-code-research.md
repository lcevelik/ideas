# RealityScan Source Code Research

**Date:** 2026-06-14
**Priority:** Medium

## What

Investigation into whether Epic Games' RealityScan mobile photogrammetry app has any publicly available source code on GitHub or elsewhere.

## TL;DR

**RealityScan source code is NOT publicly available.** Both RealityScan (mobile) and RealityCapture (desktop) are closed-source proprietary products. Epic Games has no public repos for either.

---

## GitHub Search Results

**Total repos matching "realityscan":** 41

**Key finding:** ALL are third-party tools/utilities that INTERACT with RealityScan — none contain the actual app source code.

### Notable Third-Party Repos

| Repo | Stars | Description |
|------|-------|-------------|
| nicolasdiolez/360Extractor | 128 | 360° video preprocessing for photogrammetry |
| Mistral-Yu/360Cam-PGM-3DGS-Tools | 47 | 360° video → photogrammetry/GS datasets |
| TakashiYoshinaga/360-to-RealityScan | 30 | Converts SfM outputs for RealityScan |
| stechdrive/Insta360Convert-GUI | 31 | Extract perspective views from 360° video |
| azadbal/RealityScan-to-LichtfeldStudio | 7 | RS alignment → LFS training workflow |
| matteo-collina/RealityScan-Live-Alignment | 4 | Auto-aligns images for RS |
| gohokler/realityscanflow | — | Batch processor for RealityScan |

### Epic Games GitHub Orgs Checked

- **EpicGames** — no realityscan, realitycapture, or photogrammetry repos
- **EpicGamesExt** — same, nothing

---

## The Products

### RealityScan (Mobile)
- iOS/Android app
- Free tier available
- Uses phone camera for photogrammetry capture
- Acquired from Capturing Reality

### RealityCapture (Desktop)
- Professional photogrammetry software
- Paid license required
- Previously owned by Capturing Reality (Czech company)
- Epic Games acquired Capturing Reality in 2021

Both products remain **closed-source** with no announced open-source plans.

---

## Open-Source Alternatives

If open-source photogrammetry is needed, these are viable:

| Tool | Type | Notes |
|------|------|-------|
| COLMAP | SfM + MVS | State-of-the-art, research-grade |
| OpenSfM | SfM | From Mapillary, good for video sequences |
| Meshroom (AliceVision) | Full pipeline | GUI-based, node graph |
| Open3D | Library | Python/C++, includes SfM components |
| SuperGlue / SuperPoint | Feature matching | Neural feature detector/matcher |
| OpenMVS | MVS only | Dense reconstruction |

---

## Conclusion

RealityScan/RealityCapture are proprietary commercial products with no source code available. For open-source photogrammetry workflows, COLMAP + OpenMVS or Meshroom are the standard alternatives. RealityScan's value is in its mobile UX and cloud processing — the underlying algorithms (SfM, MVS, meshing) are well-covered by open-source tools.
