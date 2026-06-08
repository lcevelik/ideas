# Lens Encoding Data — Utilizing /i Technology for AI Camera Intelligence

**Date:** 2026-06-07
**Status:** Research / Deep Dive
**Tags:** #lens-encoding #cooke-i #zeiss-extended-data #ai #camera-tracking #virtual-production

---

## What Lens Encoding Data Contains

Modern cinema lenses output per-frame metadata via Cooke /i Technology or Zeiss eXtended Data:

| Parameter | What It Is | What It Reveals |
|-----------|-----------|-----------------|
| **Focus Distance** | Distance from sensor plane to focus point (meters) | Where operator is LOOKING |
| **Iris (T-stop)** | Aperture setting | Depth of field intent, lighting conditions |
| **Focal Length** | Current focal length (mm) | Wide vs telephoto framing choice |
| **Lens Name** | Lens identity (e.g., "Cooke S4 50mm") | Lens characteristics, distortion profile |
| **Camera Serial** | Camera body ID | Which camera in multi-cam setup |

### Additional Zeiss eXtended Data

| Parameter | What It Reveals |
|-----------|-----------------|
| **Focus breathing** | How field of view changes with focus |
| **Distortion coefficients** | Real-time lens warping |
| **Vignetting** | Light falloff characteristics |
| **Color transmission** | Lens-specific color cast |

---

## The Insight: Focus Distance = Operator Intent

Camera position tells you **where the camera is**.
Camera rotation tells you **where the camera is pointing**.
Focus distance tells you **where the operator is paying attention**.

These are different things:

```
Camera at (5, 0, 2), pointing at (0, 0, 0)
Focus distance: 3.5m

→ The camera is 5m away, but the operator pulled focus to 3.5m
→ There's something at 3.5m that matters — maybe an actor, a prop, a detail
→ The operator's INTENT is at 3.5m, not at the origin
```

---

## How to Use It

### 1. Focus Distance as Attention Signal

**Problem:** AI doesn't know which part of the scene is important.

**Solution:** Use focus distance to weight the operator's attention.

```python
def compute_attention_weight(camera_pos, look_at_dir, focus_dist, scene_objects):
    """
    Weight scene objects by how likely they are to be the subject.
    
    Focus distance tells us WHERE the operator is paying attention.
    Objects near the focus plane get higher attention weight.
    """
    weights = {}
    
    for obj_name, obj_pos in scene_objects.items():
        # Project object onto camera's look direction
        to_obj = obj_pos - camera_pos
        dist_along_look = np.dot(to_obj, look_at_dir)
        
        # Distance from focus plane
        focus_error = abs(dist_along_look - focus_dist)
        
        # Gaussian weight — closer to focus plane = higher weight
        sigma = 0.5  # 0.5m tolerance
        weight = np.exp(-(focus_error ** 2) / (2 * sigma ** 2))
        
        weights[obj_name] = weight
    
    return weights
```

**Use cases:**
- **Auto-focus tracking** — AI predicts where focus should be based on operator's pattern
- **Subject detection** — Identify which actor/prop is the subject without face detection
- **Scene understanding** — Build a "importance map" of the 3D scene

### 2. Focus Pull Pattern Recognition

**Problem:** Focus pulls are complex — the operator shifts attention mid-shot.

**Solution:** Analyze focus distance over time to detect attention shifts.

```python
def detect_focus_pulls(focus_distances, times, threshold=0.5):
    """
    Detect focus pull events — where operator shifts attention.
    
    Returns list of (time, from_dist, to_dist, speed) tuples.
    """
    pulls = []
    
    # Compute focus velocity
    focus_vel = np.diff(focus_distances) / np.diff(times)
    
    # Find rapid changes (focus pulls)
    for i in range(1, len(focus_vel) - 1):
        if abs(focus_vel[i]) > threshold:  # meters per second
            # Find start and end of pull
            start = i
            while start > 0 and abs(focus_vel[start]) > threshold * 0.5:
                start -= 1
            
            end = i
            while end < len(focus_vel) - 1 and abs(focus_vel[end]) > threshold * 0.5:
                end += 1
            
            pulls.append({
                "time": times[i],
                "from_dist": focus_distances[start],
                "to_dist": focus_distances[end],
                "speed": abs(focus_vel[i]),
                "duration": times[end] - times[start],
            })
            
            # Skip past this pull
            i = end + 1
    
    return pulls
```

**Use cases:**
- **Reconstruct shot blocking** — "Actor A was talking at 3.5m, then attention shifted to Actor B at 7m"
- **Edit decision support** — "This focus pull marks a natural cut point"
- **AI style learning** — "Good operators pull focus 0.3s before the actor moves"

### 3. Focal Length as Framing Intent

**Problem:** Same subject can be framed many ways — wide, tight, medium.

**Solution:** Focal length reveals the operator's framing choice.

```python
def classify_framing_intent(focal_length, sensor_width=36.0):
    """
    Classify framing intent based on focal length.
    
    Returns framing type and field of view.
    """
    fov = 2 * np.degrees(np.arctan(sensor_width / (2 * focal_length)))
    
    if fov > 60:
        return "wide", "establishing, environment, multiple subjects"
    elif fov > 35:
        return "medium", "standard framing, dialogue, single subject"
    elif fov > 20:
        return "tight", "close-up, detail, emotion"
    else:
        return "telephoto", "extreme close-up, compression, surveillance"
```

**Use cases:**
- **Shot classification** — Automatically categorize footage by shot type
- **AI framing suggestions** — "Most operators use 50mm for this type of scene"
- **Lens recommendation** — "For this shot, a 35mm would give better coverage"

### 4. Iris as Lighting/DOF Intent

**Problem:** AI doesn't know the desired depth of field.

**Solution:** Iris setting reveals DOF intent and lighting conditions.

```python
def compute_dof_intent(iris_t_stop, focal_length, focus_dist, sensor_height=24.0):
    """
    Compute depth of field from iris and focal length.
    
    Returns near/far DOF limits and whether operator wants shallow/deep DOF.
    """
    # Circle of confusion (approximate)
    coc = 0.03  # mm for 35mm sensor
    
    # Hyperfocal distance
    hyperfocal = (focal_length ** 2) / (iris_t_stop * coc) + focal_length
    
    # Near limit
    near = (focus_dist * (hyperfocal - focal_length)) / (hyperfocal + focus_dist - 2 * focal_length)
    
    # Far limit
    far = (focus_dist * (hyperfocal - focal_length)) / (hyperfocal - focus_dist)
    
    dof = far - near
    
    # Classify intent
    if dof < 1.0:
        intent = "shallow_dof"  # Isolate subject, bokeh
    elif dof < 5.0:
        intent = "moderate_dof"  # Subject + immediate context
    else:
        intent = "deep_dof"  # Everything in focus
    
    return {
        "near": near,
        "far": far,
        "dof_meters": dof,
        "intent": intent,
    }
```

**Use cases:**
- **Depth-aware compositing** — Match virtual DOF to real DOF
- **Lighting prediction** — Low iris = dark scene or bright lights
- **Subject isolation** — Shallow DOF = subject is important, background is not

### 5. Focus + Rotation Correlation

**Problem:** Camera rotation and focus are correlated — the operator rotates TO the subject, then focuses ON it.

**Solution:** Correlate rotation changes with focus changes.

```python
def correlate_rotation_and_focus(rotations, focus_dists, times):
    """
    Find correlation between camera rotation and focus changes.
    
    Pattern: rotation changes → operator repositioning
             focus changes → operator re-acquiring subject
    """
    # Compute rotation velocity
    rot_vel = np.diff(rotations, axis=0) / np.diff(times)[:, np.newaxis]
    rot_magnitude = np.linalg.norm(rot_vel, axis=1)
    
    # Compute focus velocity
    focus_vel = np.diff(focus_dists) / np.diff(times)
    
    # Find events where both change (re-framing)
    rot_threshold = np.percentile(rot_magnitude, 75)
    focus_threshold = np.percentile(np.abs(focus_vel), 75)
    
    events = []
    for i in range(len(rot_magnitude)):
        if rot_magnitude[i] > rot_threshold and abs(focus_vel[i]) > focus_threshold:
            events.append({
                "time": times[i],
                "type": "reframe",
                "rotation_speed": float(rot_magnitude[i]),
                "focus_change": float(focus_vel[i]),
            })
    
    return events
```

**Use cases:**
- **Shot segmentation** — "This is a pan from Actor A to Actor B"
- **Attention prediction** — "Operator is about to shift focus to new subject"
- **AI-assisted framing** — "When rotation slows, pre-focus on predicted subject"

---

## Feature Vector Enhancement

### Current Feature Vector (11 features)

```
[vel_x, vel_y, vel_z, ang_x, ang_y, ang_z, qw, qx, qy, qz, fov_delta]
```

### Enhanced Feature Vector (16 features)

```
[vel_x, vel_y, vel_z, ang_x, ang_y, ang_z, qw, qx, qy, qz, fov_delta,
 focus_dist, focus_vel, iris, focal_length, dof_intent]
```

| Feature | Source | Why It Matters |
|---------|--------|----------------|
| `focus_dist` | Lens encoder | Where operator is looking |
| `focus_vel` | Computed (delta focus / delta time) | Attention shift speed |
| `iris` | Lens encoder | DOF intent, lighting |
| `focal_length` | Lens encoder | Framing choice |
| `dof_intent` | Computed (shallow/moderate/deep) | Isolation level |

### Code

```python
def create_enhanced_feature_vector(
    velocities,          # (N, 3) position deltas
    angular_velocities,  # (N, 3) rotation deltas
    quaternions,         # (N, 4) rotation
    fov_deltas,          # (N,) FOV changes
    focus_distances,     # (N,) focus distance from lens encoder
    iris_values,         # (N,) T-stop from lens encoder
    focal_lengths,       # (N,) focal length from lens encoder
) -> np.ndarray:
    """
    Assemble enhanced feature vector with lens data.
    
    Returns: (N, 16) array
    """
    n = len(velocities)
    features = np.zeros((n, 16))
    
    # Motion features (11)
    features[:, 0:3] = velocities
    features[:, 3:6] = angular_velocities
    features[:, 6:10] = quaternions
    features[:, 10] = fov_deltas
    
    # Lens features (5)
    features[:, 11] = focus_distances
    features[:, 12] = np.gradient(focus_distances)  # focus velocity
    features[:, 13] = iris_values
    features[:, 14] = focal_lengths
    features[:, 15] = _compute_dof_intent(focal_lengths, iris_values)
    
    return features


def _compute_dof_intent(focal_lengths, iris_values):
    """Compute DOF intent as continuous value (0=deep, 1=shallow)."""
    # Simplified: shallow DOF = large aperture (small T-stop) + long focal length
    # Normalize to 0-1 range
    normalized = (focal_lengths / 200.0) * (1.0 / np.maximum(iris_values, 1.0))
    return np.clip(normalized, 0, 1)
```

---

## Practical Applications

### Application 1: AI Focus Pull Assist

**Scenario:** Focus puller is overwhelmed with fast action.

**How lens data helps:**
1. Track focus distance over time
2. Learn operator's focus pull patterns (timing, speed, anticipation)
3. Predict when focus pull is needed (based on actor positions + past patterns)
4. Pre-position focus motor to predicted distance
5. Focus puller fine-tunes, AI handles the heavy lifting

```
Actor A at 3.5m, Actor B at 7.2m
Operator typically pulls focus 0.3s before cut
Cut is coming (detected by editing pattern)
→ AI pre-focuses to 7.2m
→ Focus puller confirms or adjusts
```

### Application 2: Virtual DOF Matching

**Scenario:** Virtual background needs to match real lens DOF.

**How lens data helps:**
1. Read real iris and focal length per frame
2. Compute expected DOF from lens physics
3. Apply same DOF to virtual background in Unreal
4. Result: seamless depth of field between real foreground and virtual background

```python
def match_virtual_dof(iris_t_stop, focal_length, focus_dist):
    """
    Compute DOF parameters for Unreal's post-process volume.
    """
    # Unreal uses different DOF model
    # Need to convert physical DOF to Unreal's near/far transition
    near = focus_dist * 0.8  # Simplified
    far = focus_dist * 1.2   # Simplified
    
    return {
        "DepthOfFieldMethod": 1,  # Gaussian DOF
        "FocalDistance": focus_dist,
        "DepthBlurRadius": _compute_blur_radius(iris_t_stop, focal_length),
        "NearTransitionRegion": focus_dist * 0.1,
        "FarTransitionRegion": focus_dist * 0.3,
    }
```

### Application 3: Shot Analytics Dashboard

**Scenario:** Director wants to understand how cameras were used across a shoot.

**How lens data helps:**
```python
def analyze_lens_usage(clips):
    """
    Generate analytics from lens encoding data across all clips.
    """
    stats = {
        "total_shots": len(clips),
        "focal_length_distribution": Counter(),
        "iris_distribution": Counter(),
        "average_focus_distance": [],
        "focus_pull_count": 0,
        "shot_types": Counter(),
    }
    
    for clip in clips:
        # Focal length histogram
        for fl in clip.focal_lengths:
            stats["focal_length_distribution"][round(fl)] += 1
        
        # Shot type classification
        avg_fl = np.mean(clip.focal_lengths)
        if avg_fl < 35:
            stats["shot_types"]["wide"] += 1
        elif avg_fl < 70:
            stats["shot_types"]["medium"] += 1
        else:
            stats["shot_types"]["tight"] += 1
        
        # Focus pull detection
        pulls = detect_focus_pulls(clip.focus_distances, clip.times)
        stats["focus_pull_count"] += len(pulls)
    
    return stats
```

### Application 4: Operator Style Profiling

**Scenario:** Understand individual operator's tendencies.

**How lens data helps:**
```python
def profile_operator_style(clips_by_operator):
    """
    Build style profiles from lens usage patterns.
    """
    profiles = {}
    
    for operator, clips in clips_by_operator.items():
        all_focal_lengths = []
        all_iris_values = []
        all_focus_pull_speeds = []
        
        for clip in clips:
            all_focal_lengths.extend(clip.focal_lengths)
            all_iris_values.extend(clip.iris_values)
            
            pulls = detect_focus_pulls(clip.focus_distances, clip.times)
            all_focus_pull_speeds.extend([p["speed"] for p in pulls])
        
        profiles[operator] = {
            "preferred_focal_length": np.median(all_focal_lengths),
            "focal_length_range": (np.percentile(all_focal_lengths, 10),
                                   np.percentile(all_focal_lengths, 90)),
            "average_iris": np.median(all_iris_values),
            "focus_pull_speed": np.median(all_focus_pull_speeds) if all_focus_pull_speeds else 0,
            "style": _classify_style(all_focal_lengths, all_iris_values, all_focus_pull_speeds),
        }
    
    return profiles


def _classify_style(focal_lengths, iris_values, pull_speeds):
    """
    Classify operator style based on lens usage.
    """
    median_fl = np.median(focal_lengths)
    median_iris = np.median(iris_values)
    median_speed = np.median(pull_speeds) if pull_speeds else 0
    
    if median_fl > 85 and median_iris < 4:
        return "intimate"  # Tight lens, shallow DOF — emotional, close
    elif median_fl < 35 and median_iris > 8:
        return "documentary"  # Wide lens, deep DOF — observational
    elif median_speed > 2.0:
        return "dynamic"  # Fast focus pulls — action-oriented
    else:
        return "traditional"  # Standard choices — 50mm, moderate DOF
```

### Application 5: AI Lens Recommendation

**Scenario:** AI suggests optimal lens for a shot based on scene analysis.

```python
def recommend_lens(scene_type, subject_distance, desired_dof):
    """
    Recommend lens settings based on scene requirements.
    
    Args:
        scene_type: "dialogue", "action", "establishing", "close-up"
        subject_distance: Distance to main subject (meters)
        desired_dof: "shallow", "moderate", "deep"
    """
    recommendations = {
        "dialogue": {
            "focal_length": 50,  # Standard, natural perspective
            "iris": 2.8 if desired_dof == "shallow" else 5.6,
        },
        "action": {
            "focal_length": 35,  # Wide, captures movement
            "iris": 8.0,  # Deep DOF, keep action sharp
        },
        "establishing": {
            "focal_length": 24,  # Wide, shows environment
            "iris": 11.0,  # Deep DOF, everything sharp
        },
        "close-up": {
            "focal_length": 85,  # Tight, isolates subject
            "iris": 2.0,  # Very shallow DOF, bokeh
        },
    }
    
    base = recommendations.get(scene_type, recommendations["dialogue"])
    
    # Adjust iris for desired DOF
    if desired_dof == "shallow":
        base["iris"] = max(base["iris"] - 1.5, 1.4)
    elif desired_dof == "deep":
        base["iris"] = min(base["iris"] + 2, 16.0)
    
    return base
```

---

## Data Collection Strategy

### What to Record

For every shoot, capture and log:

```json
{
  "frame": 0,
  "time": 0.0,
  "position": {"x": 0, "y": 0, "z": 0},
  "rotation_quat": {"w": 1, "x": 0, "y": 0, "z": 0},
  "lens": {
    "name": "Cooke S4 50mm",
    "focal_length": 50.0,
    "focus_distance": 3.5,
    "iris_t_stop": 2.8,
    "focus_ring_position": 0.45,
    "iris_ring_position": 0.32
  },
  "camera": {
    "serial": "ARRI_001",
    "sensor_width": 36.0,
    "sensor_height": 24.0,
    "shutter_angle": 180,
    "iso": 800
  }
}
```

### Where to Get It

| Source | How |
|--------|-----|
| **Cooke /i lenses** | Data embedded in lens metadata, extract via camera or post tool |
| **Zeiss eXtended Data** | Similar to Cooke, available on CP.3, Supreme Prime lenses |
| **ARRI cameras** | Built-in lens data recording in ARRIRAW/ProRes |
| **Post tools** | DaVinci Resolve, Basque can extract lens metadata |
| **Custom logger** | UDP listener that captures lens data per frame |

### Simple Lens Data Logger

```python
import socket
import json
import time

def listen_for_lens_data(port=7400):
    """
    Listen for lens data via UDP (common in VP setups).
    
    Many tracking systems forward lens data on a separate port.
    """
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    sock.bind(("0.0.0.0", port))
    
    print(f"Listening for lens data on port {port}...")
    
    while True:
        data, addr = sock.recvfrom(1024)
        
        try:
            lens_data = json.loads(data.decode())
            
            record = {
                "time": time.time(),
                "frame": lens_data.get("frame", 0),
                "focus_distance": lens_data.get("focus", 0),
                "iris_t_stop": lens_data.get("iris", 0),
                "focal_length": lens_data.get("zoom", 0),
                "lens_name": lens_data.get("lens_name", "unknown"),
            }
            
            print(f"Focus: {record['focus_distance']:.2f}m | "
                  f"Iris: T{record['iris_t_stop']:.1f} | "
                  f"FL: {record['focal_length']}mm")
            
            yield record
            
        except json.JSONDecodeError:
            continue
```

---

## Summary: Why Lens Data Matters

| Without Lens Data | With Lens Data |
|-------------------|----------------|
| AI knows where camera IS | AI knows where operator is LOOKING |
| AI predicts camera position | AI predicts attention + position |
| AI can't detect subject changes | AI detects focus pulls = subject changes |
| AI doesn't know DOF intent | AI matches virtual DOF to real DOF |
| AI can't classify shot types | AI classifies wide/medium/tight automatically |
| AI has 11 features | AI has 16 features (45% more information) |

**The lens is the operator's eye. Focus distance is where they're looking. Iris is how much context they want. Focal length is how they want the audience to feel. That's intelligence AI can use.**
