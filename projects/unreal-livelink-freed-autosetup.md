# Unreal Engine Live Link FreeD/OpenTrack Auto-Setup Plugin

**Date:** 2026-05-30
**Status:** 💡 Idea
**Tags:** #unreal-engine #virtual-production #camera-tracking #freed #opentrack #live-link #plugin

## Concept

An Unreal Engine plugin that automates the entire Live Link camera tracking setup for FreeD and OpenTrack IO protocols. Instead of manually configuring dozens of settings across multiple panels, the plugin handles it all in one click or wizard flow.

## Why

Setting up Live Link camera tracking in UE is tedious and error-prone:
- Multiple panels to configure (Live Link, Component, Lens, Virtual Camera)
- Easy to miss a setting (wrong up vector, wrong port, missing anchor point)
- Beginners waste hours on what should be a 30-second setup
- Even experienced users have to repeat the same steps every project

## What It Should Auto-Configure

1. **Live Link Source** — Set up FreeD/OpenTrack IO protocol with correct port and settings
2. **Up Vector** — Set to the correct orientation (Z-up for FreeD)
3. **Camera Component** — Assign tracking to the camera component
4. **Anchor Point** — Create and attach the tracking anchor/origin point
5. **Lens File** — Generate or configure the lens calibration file
6. **Virtual Camera** — Wire up the lens and tracking into the Virtual Camera system

## Possible UX Flows

- **One-click button:** "Setup FreeD Camera" in the toolbar or right-click menu
- **Wizard panel:** Step-by-step with preview of each setting
- **Blueprint node:** Expose as a function for procedural setups
- **Presets:** Save/load configurations for different camera rigs

## Open Questions

- Should it support both FreeD and OpenTrack as separate presets?
- How to handle lens calibration — import from file or manual entry?
- Should it create a new camera actor or configure an existing one?
- Support for multi-camera setups?
- Marketplace vs. free GitHub release?
- UE5 only or also UE4 support?
- Should it auto-detect FreeD packets on the network to find the right port?

## References

- FreeD protocol: industry standard for camera tracking data (PTZ, encoded heads)
- OpenTrack IO: open protocol for head/camera tracking
- UE Live Link: built-in framework for real-time data streaming
- Virtual Production camera tracking workflows (LED volume, green screen)
