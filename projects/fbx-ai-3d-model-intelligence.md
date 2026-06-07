# FBX + AI — FBX 3D Model Intelligence

**Date:** 2026-06-07
**Status:** 🚀 Active (repo created: lcevelik/FBX_AI)
**Tags:** #fbx #ai #3d-models #virtual-production #unreal-engine

## Concept

AI-powered toolkit for working with FBX 3D model files — the industry-standard interchange format used across Maya, 3ds Max, MotionBuilder, Blender, and Unreal Engine.

## Core Modules

### 1. FBX Analyzer — Model Intelligence
Parse FBX files, extract structured data (mesh, skeleton, animations, materials), compute quality metrics (triangle quality, manifold check, UV analysis), generate reports.

### 2. FBX Generator — Text/Image to FBX
Generate 3D models from text prompts or reference images, export as FBX. Integrates TripoSR, InstantMesh, Meshy API, Tripo3D API.

### 3. FBX Optimizer — LOD & Cleanup
AI-driven mesh decimation, LOD chain generation, mesh cleanup (doubles, normals, holes), material consolidation, batch processing.

### 4. FBX Rigger — Auto-Rigging
Predict skeleton placement, AI-painted skinning weights, animation retargeting, Mixamo API integration.

### 5. FBX QA — Quality Assessment
Score geometry/UV/material/animation quality, classify good/bad assets, build benchmark datasets.

## Why This Matters for VP

- FBX is the universal interchange format in virtual production
- Studios have massive FBX libraries that need auditing/optimization
- AI generation (text-to-3D) produces OBJ/GLB — need FBX conversion for UE pipeline
- Auto-rigging saves hours of manual work for character assets
- Quality gates prevent bad assets from reaching the engine

## Tech Stack

- Python: trimesh, open3d, pyfqmr, numpy, scipy
- 3D Generation: TripoSR, InstantMesh, Meshy API, Tripo3D
- ML: PyTorch, ONNX Runtime
- CLI: Click + Rich
- FBX: Autodesk SDK (optional), trimesh+assimp (open source)

## Open Questions

- Autodesk FBX SDK licensing for commercial use
- Which 3D generation model is best for FBX output quality?
- Integration with existing SplatCloud pipeline?
- Unreal Engine plugin for direct FBX_AI invocation?
