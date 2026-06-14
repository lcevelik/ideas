# Sony XYN — 3D Gaussian Splatting Pipeline Research

> Deep research into Sony XYN's spatial capture solution and underlying 3DGS technology.
> Compiled 2026-06-14 from patents, product pages, and published research.

---

## What Is XYN?

**XYN** (pronounced "Zin") is Sony's spatial content creation platform — an integrated hardware+software suite for turning real-world spaces into production-quality 3DCG assets. Announced January 2025, launched at **NAB Show 2026** (April 19-22, Las Vegas). US professional availability: Summer 2026.

Sony's website never says "3D Gaussian Splatting" — they call it *"Sony's proprietary generation algorithm."* No whitepapers are published. However, cross-referencing Sony's **45+ patents** and research papers reveals the full pipeline.

---

## The 3-Stage Pipeline

### Stage 1: XYN Spatial Scan Navi (Capture)

- iOS smartphone app paired with **Sony Alpha mirrorless camera**
- **AR-guided capture** — overlays visual cues showing required shooting positions/angles
- Real-time bird's-eye heatmap showing capture coverage/completeness
- "Smart Assist" — auto camera metadata recording, auto-exposure, auto-shutter
- Feeds metadata directly to cloud processing

### Stage 2: XYN Spatial Scan (Cloud Processing)

- Cloud-based — no local GPU needed for reconstruction
- Upload photos → "Sony's proprietary algorithms" generate high-quality 3DCG
- Handles: reflections, gloss, transparent objects, thin shapes, HDR, atmospheric quality
- Production-quality photorealistic output
- *"Just a few clicks without complex setup"*

### Stage 3: XYN Spatial Renderer Plugin (Real-time Rendering)

- Sony-developed renderer for **Unreal Engine** (5.4, 5.5, 5.6) + **Disguise** media servers
- LED wall-optimized — stable quality at any camera distance/angle
- HDR support (S-Log3 → ACES, OCIO color management)
- Depth-based depth of field from depth information
- Inner/outer frustum optimization (quality vs performance)
- Real-time CG compositing + lighting matching + localized color correction

---

## What's Under the Hood (Inferred from Patents)

### Reconstructed Pipeline

```
┌─────────────────────────────────────────────────────┐
│  STAGE 1: CAPTURE (Spatial Scan Navi)               │
│  • Sony Alpha camera + iPhone app                    │
│  • AR-guided optimal positions                       │
│  • Camera metadata (poses, EXIF) recorded            │
│  • Z-buffer depth estimation from multi-view          │
└──────────────────────┬──────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────┐
│  STAGE 2: RECONSTRUCTION (Spatial Scan — Cloud)      │
│  • Multi-view stereo → point cloud initialization    │
│  • Camera poses from Alpha metadata (NOT COLMAP)     │
│  • Surface normals → Gaussian scaling                │
│  • 3DGS optimization (proprietary, not vanilla)      │
│  • Scale-aware contrastive segmentation              │
│  • SH encoding for view-dependent appearance         │
│  • HDR-aware color pipeline (S-Log3 → ACES)          │
│  • Compression via G-PCC + SH decorrelation (50%)    │
└──────────────────────┬──────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────┐
│  STAGE 3: RENDERING (Spatial Renderer Plugin)        │
│  • Custom UE plugin (5.4/5.5/5.6)                   │
│  • Disguise integration (GX 3, RX III)               │
│  • CUDA-based tile rasterizer (patent description)   │
│  • Inner/outer frustum optimization                  │
│  • Depth-based DOF from Gaussian depth               │
│  • HDR display (ACES, OCIO color management)         │
│  • Real-time CG compositing + lighting matching      │
└─────────────────────────────────────────────────────┘
```

---

## Sony Patents — The Technical Evidence

### Core 3DGS Pipeline Patents (Michael Taylor, SIE — Filed July 23, 2024)

**4 patents filed in one day** — clearly the core R&D lead for Sony's Gaussian splatting tech.

#### US20260027471A1 — "Using Gaussian Representations in Computer Game"
- **Inventors:** Michael Taylor, Mihee Kang, Maito Omori, Xinyu Zhang, Shun Terasaki, Akihiro Takano
- **Assignee:** Sony Interactive Entertainment Inc.
- **Key claims:**
  - Generate 3D representation from game video using Gaussians
  - Metadata provides z-buffer → depth map per frame
  - Point cloud created from depth maps OR mesh extraction + downsampling
  - Initial Gaussians for each point, scaled by surface/vertex normals
  - Camera poses read from metadata
  - Gaussian splatting executed with CUDA tile rasterizer
  - Opacity set to zero for scene Gaussians, only inserted content visible
  - Frame-by-frame occlusion

#### US20260027472A1 — "Extracting Game Metadata for Volumetric Representations"
- Same team
- Extracts z-buffer, depth maps, normals, camera poses from game engine
- Applies to volumetric/Gaussian representations
- Mesh texture/topology altered by metadata
- User content converted to match representation type (Gaussians or NeRFs)

#### US20260027473A1 — "Using Game Metadata to Animate User-Generated Objects"
- Same team
- Focus: animating user-generated content using game metadata
- Objects track, translate, rotate, scale according to scene changes
- Physical alterations: ice melts, objects dissolve, smoke billows

#### US20260027474A1 — "Unsupervised Extraction of Shared Group Correspondences"
- Same team
- Unsupervised extraction of correspondences from video game data
- For 3D content generation using volumetric/Gaussian representations

### 3DGS Compression Patents (Sony Group Corp)

#### WO2026047473A1 — "3D Gaussian Splatting Data Compression"
- **Inventors:** Shashank Sridhara, Alexandre Zaghetto, Danillo Graziosi, Ali Tabatabai
- Uses **G-PCC framework** for geometry compression
- Occupancy tree coding for positions
- Transform coding for scales/rotations
- **Block-based graph Fourier transform (GFT)** for SH coefficients, base colors, opacities
- **KL-divergence-based graph construction** for edge weights between Gaussian distributions
- Alternatively: maps Gaussian parameters to 2D frames for video coding

#### WO2026009078A1 — "Decorrelation of Spherical Harmonic Coefficients"
- Same team
- Transform on SH coefficients to exploit inter-channel redundancies
- **Reduces memory by 50%** (SH = 48 of 59 parameters per Gaussian)
- Leverages G-PCC and V-PCC standards

### 3DGS Media Gallery (Sony Group Corp)

#### US20250367563A1 — "Interactive 3D Content Generation and Sharing"
- **Inventors:** Alexandre Zaghetto, Danillo Graziosi, Ali Tabatabai
- Framework for capturing 3D content from gaming sessions using 3DGS
- Sharing interactive 3D models across devices (mobile, TV, VR)
- User can change view direction, zoom in/out

### NeRF Patents (Sony Interactive Entertainment)

#### US12469220B2 — "Shaping NeRF Using Multiple Polygonal Meshes" (GRANTED)
- **Inventors:** Joseph Logan Olson, Nasir Mohammad Khalid
- Generate NeRF from text using polygonal meshes as spatial constraints
- Score distillation sampling (SDS) loss
- Convert NeRF to mesh for rendering

#### Additional NeRF patents: US20250005860A1 (abandoned), WO2025006150A1, WO2024073247A1, WO2024076936A2

### Japanese Sony Group Patents (3DGS-related)

14+ Japanese patents filed 2024 covering:
- Learning methods for 3D Gaussian representations
- Information processing for 3DGS/NeRF systems
- Encoding/decoding for volumetric data
- Display processing for Gaussian-rendered content
- Training processing for image generation

### Sony Semiconductor Solutions Patents

- WO2025187250A1 — Encoding/decoding for 3DGS data
- WO2025177813A1 — Encoding/decoding for 3DGS data
- WO2025197491A1 — Proxy voxel grid for NeRF/3DGS processing
- WO2025201947A1 — Circuitry for NeRF/3DGS processing

---

## Sony Research Papers

### Sony Taiwan (Most Active in 3DGS)

| Paper | Venue | Year | Key |
|-------|-------|------|-----|
| **EventSplat** | CVPR 2025 | 2025 | 3DGS from event cameras, real-time rendering |
| **NeISF** | CVPR 2024 | 2024 | Neural Incident Stokes Field, material estimation |
| **NeISF++** | CVPR 2025 | 2025 | Polarized inverse rendering |
| **ToF-Splatting** | ICCV 2025 | 2025 | Dense SLAM with sparse ToF depth |

### Sony CSL (Jun Rekimoto's Lab)

| Paper | Venue | Year | Key |
|-------|-------|------|-----|
| **Incremental Gaussian Splatting** | UIST 2024 | 2024 | Monocular gradual 3D reconstruction |
| **Gaussians in the City** | UIST 2024 | 2024 | Urban 3D reconstruction under distractors |

### Sony Corporation US (MPEG Standardization)

| Paper | Venue | Year | Key |
|-------|-------|------|-----|
| **MPEG Explorations Toward 3D Gaussian Splat Coding** | DCC 2026 | 2026 | 3DGS compression standardization |
| **SqueezeNeRF** | CVPRW 2022 | 2022 | Memory-efficient NeRF inference |

---

## Key Differentiators vs Open-Source 3DGS

| Aspect | Standard 3DGS | Sony XYN (from patents) |
|---|---|---|
| **Init source** | COLMAP (SfM) | Camera metadata + z-buffer from Alpha |
| **Scale init** | KNN distances × 0.3 | Surface/vertex normals from depth |
| **Segmentation** | Manual | Automatic (scale-aware contrastive training) |
| **Compression** | PLY (none) | G-PCC + SH decorrelation (50% reduction) |
| **Color pipeline** | RGB/SH (LDR) | HDR-native (S-Log3 → ACES → OCIO) |
| **Rendering** | SuperSplat (CPU/WebGL) | CUDA tile rasterizer (UE/Disguise) |
| **Occlusion** | None | Frame-by-frame Gaussian opacity masking |

---

## The Inventor Teams

| Team | People | Focus | Entity |
|------|--------|-------|--------|
| **Core 3DGS R&D** | Michael Taylor, Mihee Kang, Maito Omori, Xinyu Zhang, Shun Terasaki, Akihiro Takano | Gaussian representations, game integration, user-generated content | Sony Interactive Entertainment |
| **3DGS Compression** | Shashank Sridhara, Alexandre Zaghetto, Danillo Graziosi, Ali Tabatabai | G-PCC, SH decorrelation, MPEG standardization | Sony Group Corp |
| **NeRF/3D Generation** | Joseph Logan Olson, Nasir Mohammad Khalid | NeRF shaping, text-to-3D, NeRF-to-mesh | Sony Interactive Entertainment |
| **Event/Neural Rendering** | Toshiya Yura, Ashkan Mirzaei, Igor Gilitschenski | Event cameras, inverse rendering | Sony Taiwan |

---

## Market Context

- **Target:** Virtual production LED walls (typically several meters)
- **Competitors:** Luma AI, Polycam, RealityScan (all offer 3DGS capture)
- **Advantage:** End-to-end vertical integration (Alpha camera → cloud → UE/Disguise)
- **Standardization:** Sony writing MPEG standard for 3DGS compression
- **Roadmap:** Gaming, animation, architecture, manufacturing, digital twins, cultural heritage

---

## Sources

- radiancefields.com — "Sony XYN Launches Spatial Capture Solution with Gaussian Splatting" (Apr 16, 2026)
- xyn.sony.net — Official product pages
- Google Patents — 45+ Sony patents on 3DGS/NeRF/volumetric rendering
- arXiv — EventSplat (2412.07293), related papers
- OpenAlex — Sony research publication database

---

## Relevance to Our Work

### How This Compares to images_to_play Pipeline

| Aspect | images_to_play | Sony XYN |
|--------|---------------|----------|
| Camera source | Any phone/camera | Sony Alpha + iPhone |
| Reconstruction | COLMAP → Brush/gsplat | Proprietary cloud |
| Training | Local GPU (RTX 8000) | Cloud-based |
| Output | PLY → splat.steadiczech.com | Compressed 3DGS → UE/Disguise |
| Rendering | SuperSplat (web) | Custom CUDA UE plugin |
| HDR | No | Yes (S-Log3/ACES) |
| LED wall | No | Yes (primary target) |

### Potential Overlap / Learning Points

1. **Camera metadata assist** — We could use Alpha camera metadata to bootstrap COLMAP instead of relying on EXIF-only
2. **Surface normal scaling** — Our KNN-based init could be improved with normal-based scaling from depth
3. **SH decorrelation compression** — 50% reduction is significant for our PLY/SPZ uploads to splat.steadiczech.com
4. **Scale-aware segmentation** — Automatic scene segmentation would enable content insertion workflows
5. **HDR pipeline** — Our pipeline is LDR-only; HDR-aware training could improve quality for LED wall use cases
