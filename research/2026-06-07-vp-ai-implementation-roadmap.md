# VP + AI Integration — Implementation Roadmap

**Date:** 2026-06-07
**Status:** Planning
**Tags:** #virtual-production #ai #implementation #roadmap

---

## Phase 0: Data Foundation (Week 1-2)

**Goal:** Extract and normalize real camera tracking data.

### Tasks
- [ ] Install Autodesk FBX SDK on Linux server
- [ ] Write FBX parser that extracts camera transforms (position, rotation, FOV)
- [ ] Write data normalization pipeline:
  - Coordinate system conversion (Maya → Unreal)
  - Framerate resampling (scipy interpolation)
  - SLERP for quaternion interpolation
  - Relative motion vector computation
- [ ] Export normalized data to JSON/CSV
- [ ] Test with 3-5 sample FBX files from existing shoots

### Deliverable
```
fbx_ai extract camera.fbx --output data.json
fbx_ai normalize camera.fbx --target-fps 30 --output normalized.json
```

### Data We Need
- 3-5 FBX files from VP shoots (Mo-Sys, Stype, or any tracking system)
- Each should have animated camera node with position + rotation
- Preferably different shot types: static, pan, tracking, crane

---

## Phase 1: Path Prediction (Week 3-4)

**Goal:** Predict camera path continuation from partial input.

### Tasks
- [ ] Build training dataset from extracted FBX data
  - Split long takes into input/target windows
  - Input: 50 frames → Target: 200 frames
- [ ] Train LSTM encoder-decoder model
  - Input: 13 features per frame (pos, rot, lens, velocity)
  - Output: 13 features per frame (predicted future)
  - Loss: MSE + quaternion consistency
- [ ] Train TCN alternative for comparison
- [ ] Evaluate on held-out test sequences
  - Metrics: position error, rotation error, perceptual quality
- [ ] Export trained model to ONNX for fast inference

### Deliverable
```
fbx_ai predict camera.fbx --frames 200 --model lstm --output predicted.json
```

### Success Criteria
- Position error < 5cm at 50 frames into prediction
- Rotation error < 2 degrees at 50 frames
- Perceptually smooth continuation (no jarring transitions)

---

## Phase 2: Style Transfer (Week 5-6)

**Goal:** Transform camera motion between styles.

### Tasks
- [ ] Label training data by motion type:
  - Handheld, tripod, dolly, crane, Steadicam, robotic
- [ ] Train 1D Conv Autoencoder
  - Encoder: compress motion to latent style vector
  - Decoder: reconstruct with target style
- [ ] Train DDPM alternative for complex style transformations
- [ ] Test style transfer:
  - Robotic → handheld (add organic imperfections)
  - Handheld → stabilized (remove jitter)
  - Cross-operator style matching

### Deliverable
```
fbx_ai style camera.fbx --target-style handheld --output stylized.json
```

### Success Criteria
- Style is visually distinguishable from input
- Overall path geometry preserved
- No artifacts or unnatural movements

---

## Phase 3: Auto-Framing Agent (Week 7-8)

**Goal:** AI agent that keeps subjects framed using composition rules.

### Tasks
- [ ] Build Gymnasium environment for camera framing
  - State: actor position + camera state
  - Action: camera movement delta (x, y, z, pitch, yaw, fov)
  - Reward: Rule of Thirds + headroom + smoothness
- [ ] Train PPO agent in simulation
  - Start with single actor, expand to multi-actor
- [ ] Test in Unreal Engine mock environment
- [ ] Tune reward function for natural-looking results

### Deliverable
```
fbx_ai train-actor-tracking --data training_data/ --output framing_agent.pt
```

### Success Criteria
- Actor stays in frame 95%+ of the time
- Composition matches cinematographic rules
- Movement is smooth, not robotic

---

## Phase 4: Real-Time Integration (Week 9-10)

**Goal:** Deploy AI models for real-time inference on set.

### Tasks
- [ ] WebSocket server for real-time camera data streaming
- [ ] ONNX Runtime inference pipeline (< 16ms latency)
- [ ] Integration with Unreal Engine Live Link
  - AI suggestions fused with operator input
  - Confidence-based blending
- [ ] Safety mechanisms:
  - Operator override always available
  - Emergency stop
  - Confidence thresholds
- [ ] Test on mock VP stage setup

### Deliverable
```
fbx_ai stream --model inference_model.onnx --port 8765
```

### Success Criteria
- End-to-end latency < 16ms (60fps capable)
- Operator can override AI at any time
- No dropped frames during AI inference

---

## Phase 5: Production Deployment (Week 11-12)

**Goal:** Deploy at a real VP stage and validate with operators.

### Tasks
- [ ] Package as UE plugin (sits between FreeD input and Live Link)
- [ ] Operator training documentation
- [ ] Performance optimization:
  - Model quantization (INT8)
  - Batch inference for multiple cameras
  - GPU memory optimization
- [ ] Collect feedback from operators
- [ ] Iterate on models based on real-world performance

### Deliverable
- UE Plugin: `fbx_ai_camera_assistant.uplugin`
- Documentation: setup guide, operator manual
- Trained models: predictor.onnx, style.onnx, framing.onnx

---

## Tech Stack Summary

```
Development:
  Python 3.10+
  ├── FBX SDK (data extraction)
  ├── NumPy/SciPy (normalization)
  ├── PyTorch (model training)
  ├── ONNX Runtime (fast inference)
  ├── WebSocket (real-time streaming)
  └── OpenCV/matplotlib (visualization)

Deployment:
  Unreal Engine 5.5+
  ├── Live Link (real-time camera input)
  ├── nDisplay (LED wall rendering)
  ├── Blueprint (integration logic)
  └── Python Editor Utility (scripting)
```

---

## Training Data Requirements

### Minimum Viable Dataset
- **10+ hours** of camera tracking recordings
- **Multiple operators** (at least 3-5 different styles)
- **Multiple shot types** (static, pan, track, crane, handheld)
- **Multiple scenes** (dialogue, action, establishing, close-up)
- **FBX format** with animated camera nodes

### Data Collection Strategy
1. Request FBX exports from existing VP shoots
2. Set up recording pipeline for new shoots
3. Label data by motion type and operator
4. Augment with synthetic data if needed

### Data Format
```json
{
  "clip_name": "scene_01_take_03",
  "operator": "operator_a",
  "motion_type": "handheld",
  "fps": 30,
  "frames": [
    {
      "frame": 0,
      "time": 0.0,
      "position": [x, y, z],
      "rotation_quat": [w, x, y, z],
      "fov": 50.0,
      "focus_dist": 3.5,
      "iris": 2.8,
      "focal_length": 50
    }
  ]
}
```

---

## Risk Mitigation

| Risk | Mitigation |
|------|------------|
| FBX SDK not available | Use FBX2glTF + pygltflib fallback |
| Insufficient training data | Start with synthetic data, augment |
| Inference too slow | ONNX optimization, model quantization |
| Operators reject AI | Start as "suggestion" mode, not control |
| Coordination issues | Always use quaternions, document transforms |

---

## Success Metrics

| Metric | Target |
|--------|--------|
| Path prediction accuracy | < 5cm error at 50 frames |
| Style transfer quality | Blind test 70%+ correct style identification |
| Auto-framing success | 95%+ actor in frame |
| Inference latency | < 16ms per frame |
| Operator satisfaction | 80%+ would use again |
