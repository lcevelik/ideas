# Gouging Splat — Image Sorting → Processing → Gallery Pipeline

**Date:** 2025-05-29
**Status:** 💡 Idea
**Tags:** #3dgs #gallery #pipeline #image-sorting

## Concept

Connect the image sorting algorithm to the Gouging Splat app to create a single coherent pipeline:

1. **Sort** — Image sorting algorithm selects/organizes input images
2. **Process** — Gouging Splat app processes the sorted images (3DGS training/splatting)
3. **Upload** — Automatically push results to a gallery on the website

One seamless flow from raw images to live gallery.

## Why

- Currently these are likely separate manual steps
- Reduces friction — no manual handoff between sorting, processing, and publishing
- Makes the whole pipeline reproducible and automatable
- Gallery gets updated without manual intervention

## How

- Wire image sorting algorithm as the input stage for the gouging splat app
- After processing completes, auto-upload results (renders, splats, thumbnails) to the website gallery
- Could be a CLI pipeline, a single app with stages, or orchestrated scripts
- Gallery endpoint needs an upload API or static file deployment

## Open Questions

- What format does the gallery expect? (images, splat files, both?)
- Is the image sorting algorithm a standalone script or integrated into something?
- Where does the website gallery live? (splat.steadiczech.com? separate site?)
- Should this be a single CLI tool or a web-triggered pipeline?
- Error handling — what happens if sorting or processing fails mid-pipeline?
