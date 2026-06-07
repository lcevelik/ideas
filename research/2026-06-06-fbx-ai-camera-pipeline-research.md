# FBX AI Camera Pipeline — Deep Research

**Date:** 2026-06-06
**Priority:** High

## What

Comprehensive research on building a pipeline that extracts camera tracking data from FBX files, normalizes it for AI consumption, trains models to predict/style-transfer/compose camera movements, and writes results back to production formats.

## Why Investigate

This bridges virtual production camera tracking with AI-assisted filmmaking. The pipeline could automate camera path generation, add organic imperfections to robotic moves, and enable intelligent framing — reducing manual work in post-production.

---

## 1. FBX Parsing & Camera Data Extraction

### 1.1 Autodesk FBX SDK (Official)

**Availability:** Free, but requires manual download from Autodesk. No pip install.
- Download: https://www.autodesk.com/developer-network/platform-technologies/fbx-sdk-2020-3-7
- Python bindings included in the SDK (not a separate package)
- Supports Python 3.x on Windows, macOS, Linux

**Installation:**
```bash
# Linux/macOS - download tar.gz from Autodesk
tar -xzf fbxsdk_linux_2020_3_7.tar.gz
cd python
python setup.py install

# Or set PYTHONPATH manually
export PYTHONPATH=$PYTHONPATH:/path/to/fbx_sdk/lib/python
```

**Key Classes for Camera Extraction:**
```python
import fbx

# Load scene
sdk = fbx.FbxManager.Create()
scene = fbx.FbxScene.Create(sdk, "")
loader = fbx.FbxImporter.Create(sdk, "")
loader.Initialize("camera.fbx", -1, sdk.GetIOSettings())
loader.Import(scene)
loader.Destroy()

# Find camera node
def find_cameras(node):
    cameras = []
    for i in range(node.GetChildCount()):
        child = node.GetChild(i)
        cam = child.GetNodeAttribute()
        if cam and cam.GetAttributeType() == fbx.FbxNodeAttribute.eCamera:
            cameras.append(child)
        cameras.extend(find_cameras(child))
    return cameras

cameras = find_cameras(scene.GetRootNode())
```

**Extracting Animation Curves:**
```python
import math

def extract_camera_animation(camera_node, fps=30):
    camera = camera_node.GetNodeAttribute()
    
    # Get translation animation
    tx_curve = camera_node.LclTranslation.GetCurve(0)  # X
    ty_curve = camera_node.LclTranslation.GetCurve(1)  # Y
    tz_curve = camera_node.LclTranslation.GetCurve(2)  # Z
    
    # Get rotation animation
    rx_curve = camera_node.LclRotation.GetCurve(0)
    ry_curve = camera_node.LclRotation.GetCurve(1)
    rz_curve = camera_node.LclRotation.GetCurve(2)
    
    # Get FOV
    fov_curve = camera.GetFieldOfView.GetCurve(0)
    
    frames = []
    for frame in range(0, 1000):  # Adjust range
        time = fbx.FbxTime()
        time.SetFrame(frame, fps)
        
        # Evaluate curves at this frame
        tx = tx_curve.Evaluate(time) if tx_curve else 0
        ty = ty_curve.Evaluate(time) if ty_curve else 0
        tz = tz_curve.Evaluate(time) if tz_curve else 0
        rx = rx_curve.Evaluate(time) if rx_curve else 0
        ry = ry_curve.Evaluate(time) if ry_curve else 0
        rz = rz_curve.Evaluate(time) if rz_curve else 0
        fov = fov_curve.Evaluate(time) if fov_curve else 50.0
        
        frames.append({
            'frame': frame,
            'time_sec': time.GetSecondDouble(),
            'position': [tx, ty, tz],
            'rotation_euler': [rx, ry, rz],
            'fov': fov
        })
    
    return frames
```

**Gotchas:**
- FBX SDK Python bindings are NOT on PyPI — must install from Autodesk download
- Quaternions preferred over Euler for AI (avoid gimbal lock)
- Convert Euler to Quaternion manually or use `FbxEuler` → `FbxQuaternion`
- Animation evaluation requires `FbxAnimEvaluator` (get from scene)
- Time units vary — always use `FbxTime` for consistency

### 1.2 Open-Source Alternatives

| Library | Status | Notes |
|---------|--------|-------|
| **pyfbx** (PyPI) | Lightweight wrapper | Limited animation support, read-only |
| **openfbx** (C++ header-only) | Read-only | Fast, no Python binding yet |
| **FBX2glTF** (Facebook) | Converter | Convert FBX → glTF, then parse glTF in Python |
| **assimp** (Python: pyassimp) | Multi-format | Supports FBX, but camera animation extraction limited |

**Recommended approach for production:** FBX SDK (official) or FBX2glTF + glTF parser for simpler pipeline.

### 1.3 FBX2glTF Alternative Pipeline

```bash
# Convert FBX to glTF
FBX2glTF --input camera.fbx --output camera.gltf
```

```python
# Parse glTF with pygltflib
import pygltflib

gltf = pygltflib.GLTF2().load("camera.gltf")

# Access camera nodes, animation data via glTF spec
for anim in gltf.animations:
    for sampler in anim.samplers:
        # Access keyframe data
        pass
```

**Pros:** glTF is a standard, well-documented format with Python libraries.
**Cons:** Some FBX-specific features may not translate (certain animation types).

---

## 2. Data Normalization for AI

### 2.1 Coordinate System Conversion

```python
import numpy as np

def maya_to_unreal(position):
    """Convert Maya (Y-up) to Unreal (Z-up) coordinate system."""
    x, y, z = position
    return np.array([x, z, y])  # Swap Y and Z

def maya_to_unreal_rotation(euler_degrees):
    """Convert Maya rotation to Unreal rotation."""
    pitch, yaw, roll = euler_degrees
    return np.array([roll, pitch, yaw])  # Adjust based on axis convention
```

### 2.2 Framerate Resampling

```python
from scipy.interpolate import interp1d

def resample_motion(frames, original_fps, target_fps=30):
    """Resample animation to uniform framerate."""
    original_times = [f['time_sec'] for f in frames]
    target_times = np.arange(
        original_times[0], 
        original_times[-1], 
        1.0 / target_fps
    )
    
    resampled = []
    for attr in ['position', 'rotation_euler']:
        values = np.array([f[attr] for f in frames])
        interp = interp1d(original_times, values, axis=0, kind='linear')
        resampled_values = interp(target_times)
        
        for i, t in enumerate(target_times):
            if i >= len(resampled):
                resampled.append({'time_sec': t})
            resampled[i][attr] = resampled_values[i].tolist()
    
    return resampled
```

### 2.3 Relative Motion Vectors

```python
def compute_relative_motion(frames):
    """Convert absolute positions to relative velocity vectors."""
    relative = []
    for i in range(1, len(frames)):
        prev = frames[i-1]
        curr = frames[i]
        
        dt = curr['time_sec'] - prev['time_sec']
        
        # Velocity = delta position / delta time
        velocity = [
            (curr['position'][j] - prev['position'][j]) / dt 
            for j in range(3)
        ]
        
        # Angular velocity (simplified)
        angular_velocity = [
            (curr['rotation_euler'][j] - prev['rotation_euler'][j]) / dt 
            for j in range(3)
        ]
        
        relative.append({
            'time_sec': curr['time_sec'],
            'velocity': velocity,
            'angular_velocity': angular_velocity,
            'fov_delta': curr['fov'] - prev['fov']
        })
    
    return relative
```

### 2.4 Feature Vector Assembly

```python
def create_feature_vector(frame):
    """Assemble features for AI model input."""
    return np.array([
        # Position (normalized)
        frame['position'][0],
        frame['position'][1],
        frame['position'][2],
        # Rotation as quaternion (normalized)
        frame['quaternion'][0],  # W
        frame['quaternion'][1],  # X
        frame['quaternion'][2],  # Y
        frame['quaternion'][3],  # Z
        # Lens
        frame['fov'],
        frame['focal_length'],
        # Velocity (if relative mode)
        frame.get('velocity_x', 0),
        frame.get('velocity_y', 0),
        frame.get('velocity_z', 0),
    ])
```

---

## 3. AI Model Architectures

### 3.1 Camera Path Prediction (LSTM/TCN)

**Architecture: LSTM Encoder-Decoder**
```python
import torch
import torch.nn as nn

class CameraPathPredictor(nn.Module):
    def __init__(self, input_size=11, hidden_size=128, num_layers=2):
        super().__init__()
        self.encoder = nn.LSTM(
            input_size=input_size,
            hidden_size=hidden_size,
            num_layers=num_layers,
            batch_first=True,
            dropout=0.2
        )
        self.decoder = nn.LSTM(
            hidden_size=hidden_size,
            hidden_size=hidden_size,
            num_layers=num_layers,
            batch_first=True
        )
        self.fc = nn.Linear(hidden_size, input_size)
    
    def forward(self, src, target_len):
        # Encode input sequence
        _, (hidden, cell) = self.encoder(src)
        
        # Decode future frames
        decoder_input = src[:, -1:, :]  # Last frame
        outputs = []
        
        for _ in range(target_len):
            out, (hidden, cell) = self.decoder(decoder_input, (hidden, cell))
            pred = self.fc(out)
            outputs.append(pred)
            decoder_input = pred
        
        return torch.cat(outputs, dim=1)

# Input shape: (batch_size, sequence_length, 11 features)
# Output: (batch_size, target_length, 11 features)
```

**Training:**
```python
model = CameraPathPredictor()
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)
criterion = nn.MSELoss()

# Training loop
for epoch in range(100):
    for src_seq, target_seq in dataloader:
        pred = model(src_seq, target_seq.shape[1])
        loss = criterion(pred, target_seq)
        
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
```

**Alternative: TCN (Temporal Convolutional Network)**
- Better for long-range dependencies
- Faster training than LSTM
- Use `torch-conv1d` or `keras-tcn` library

### 3.2 Style Transfer (Autoencoder/Diffusion)

**1D Convolutional Autoencoder:**
```python
class MotionStyleAutoencoder(nn.Module):
    def __init__(self, input_size=11, latent_dim=32):
        super().__init__()
        
        # Encoder
        self.encoder = nn.Sequential(
            nn.Conv1d(input_size, 64, kernel_size=3, padding=1),
            nn.ReLU(),
            nn.MaxPool1d(2),
            nn.Conv1d(64, 128, kernel_size=3, padding=1),
            nn.ReLU(),
            nn.MaxPool1d(2),
            nn.Flatten(),
            nn.Linear(128 * (seq_len // 4), latent_dim)
        )
        
        # Decoder
        self.decoder = nn.Sequential(
            nn.Linear(latent_dim, 128 * (seq_len // 4)),
            nn.Unflatten(1, (128, seq_len // 4)),
            nn.ConvTranspose1d(128, 64, kernel_size=3, padding=1),
            nn.ReLU(),
            nn.Upsample(scale_factor=2),
            nn.ConvTranspose1d(64, input_size, kernel_size=3, padding=1),
        )
    
    def forward(self, x):
        latent = self.encoder(x)
        reconstructed = self.decoder(latent)
        return reconstructed, latent
```

**Diffusion Model Approach:**
- Use Denoising Diffusion Probabilistic Models (DDPM)
- Train on "pristine" vs "handheld" motion pairs
- Add noise profiles from real camera operator data

### 3.3 Reinforcement Learning (PPO)

**Camera Framing Agent:**
```python
import gymnasium as gym

class CameraFramingEnv(gym.Env):
    def __init__(self):
        super().__init__()
        
        # Action space: camera movement (delta x, y, z, rotation)
        self.action_space = gym.spaces.Box(
            low=-1, high=1, shape=(6,), dtype=np.float32
        )
        
        # Observation: actor position + camera state
        self.observation_space = gym.spaces.Box(
            low=-np.inf, high=np.inf, shape=(10,), dtype=np.float32
        )
    
    def step(self, action):
        # Apply camera movement
        # Calculate reward based on framing
        reward = self._calculate_framing_reward()
        done = self._check_episode_end()
        return obs, reward, done, {}
    
    def _calculate_framing_reward(self):
        """Reward for keeping actor in Rule of Thirds box."""
        actor_screen_pos = self._project_to_screen(self.actor_pos)
        
        # Rule of thirds boxes
        thirds_x = [1/3, 2/3]
        thirds_y = [1/3, 2/3]
        
        # Find closest thirds intersection
        min_dist = float('inf')
        for tx in thirds_x:
            for ty in thirds_y:
                dist = np.sqrt(
                    (actor_screen_pos[0] - tx)**2 + 
                    (actor_screen_pos[1] - ty)**2
                )
                min_dist = min(min_dist, dist)
        
        # Reward inversely proportional to distance
        return 1.0 / (1.0 + min_dist)

# Train with PPO from stable-baselines3
from stable_baselines3 import PPO

env = CameraFramingEnv()
model = PPO("MlpPolicy", env, verbose=1)
model.learn(total_timesteps=100000)
```

---

## 4. Writing Back to Production

### 4.1 FBX SDK Write-Back

```python
def create_fbx_with_camera(output_path, keyframes, fps=30):
    """Generate FBX file with animated camera."""
    manager = fbx.FbxManager.Create()
    scene = fbx.FbxScene.Create(manager, "AICameraScene")
    
    # Create camera
    camera = fbx.FbxCamera.Create(scene, "AI_Generated_Camera")
    camera_node = scene.GetRootNode().AddChild(camera)
    
    # Set camera properties
    camera.SetApertureMode(fbx.FbxCamera.eVerticalAperture)
    camera.SetAperture(0.66)
    
    # Create animation stack
    anim_stack = fbx.FbxAnimStack.Create(scene, "CameraAnimation")
    anim_layer = fbx.FbxAnimLayer.Create(scene, "BaseLayer")
    anim_stack.AddMember(anim_layer)
    
    # Add keyframes for position
    for attr_name, curve_node_func in [
        ("LclTranslation", camera_node.LclTranslation),
        ("LclRotation", camera_node.LclRotation),
    ]:
        for axis in range(3):
            curve = curve_node_func.GetCurve(axis)
            if not curve:
                curve = curve_node_func.CreateCurve(anim_layer)
            
            # Set default tangents
            curve.SetTangentMode(fbx.FbxAnimCurve.eTangentTCB)
            
            for kf in keyframes:
                time = fbx.FbxTime()
                time.SetFrame(kf['frame'], fps)
                
                key_index = curve.KeyAdd(time)
                curve.SetKeyValue(key_index, kf[attr_name.lower()][axis])
    
    # Save
    saver = fbx.FbxExporter.Create(manager, "")
    saver.Initialize(output_path, -1, manager.GetIOSettings())
    saver.Export(scene)
    saver.Destroy()
    manager.Destroy()
```

### 4.2 JSON/CSV Interface (Unreal/Unity)

```python
import json
import csv

def export_to_json(keyframes, output_path):
    """Export camera data as JSON for real-time engines."""
    data = {
        'fps': 30,
        'total_frames': len(keyframes),
        'keyframes': [
            {
                'frame': kf['frame'],
                'time': kf['time_sec'],
                'position': {'x': kf['position'][0], 'y': kf['position'][1], 'z': kf['position'][2]},
                'rotation': {'x': kf['rotation'][0], 'y': kf['rotation'][1], 'z': kf['rotation'][2]},
                'fov': kf['fov']
            }
            for kf in keyframes
        ]
    }
    
    with open(output_path, 'w') as f:
        json.dump(data, f, indent=2)

def export_to_csv(keyframes, output_path):
    """Export camera data as CSV."""
    with open(output_path, 'w', newline='') as f:
        writer = csv.writer(f)
        writer.writerow(['frame', 'time', 'pos_x', 'pos_y', 'pos_z', 
                         'rot_x', 'rot_y', 'rot_z', 'fov'])
        
        for kf in keyframes:
            writer.writerow([
                kf['frame'], kf['time_sec'],
                *kf['position'], *kf['rotation_euler'], kf['fov']
            ])
```

### 4.3 Real-Time Streaming (WebSocket)

```python
import asyncio
import websockets
import json

async def stream_camera_data(websocket, path):
    """Stream camera data in real-time to Unreal/Unity."""
    # Read from CSV/JSON or live tracking
    with open('camera_data.json', 'r') as f:
        data = json.load(f)
    
    for kf in data['keyframes']:
        await websocket.send(json.dumps({
            'type': 'camera_update',
            'position': kf['position'],
            'rotation': kf['rotation'],
            'fov': kf['fov']
        }))
        await asyncio.sleep(1.0 / data['fps'])

# Start server
start_server = websockets.serve(stream_camera_data, "localhost", 8765)
asyncio.get_event_loop().run_until_complete(start_server)
asyncio.get_event_loop().run_forever()
```

### 4.4 Unreal Engine Integration

**Blueprint Approach:**
1. Import JSON/CSV via Python or CSV import node
2. Create Level Sequence with camera
3. Use Blueprint to parse data and set camera transform per frame
4. Use `SetActorLocationAndRotation` each tick

**Live Link Approach:**
1. Use Unreal's Live Link plugin
2. Send camera transforms via Live Link protocol
3. Real-time update in viewport

**Python Script (Unreal Editor Utility):**
```python
import unreal

# Read camera data
with open('camera_data.json') as f:
    data = json.load(f)

# Create level sequence
sequence = unreal.EditorAssetLibrary.load_asset('/Game/CameraSequence')
# ... set keyframes via unreal.KeyHandle
```

---

## 5. Existing Projects & Research

### 5.1 Academic Papers

| Paper | Year | Approach | Notes |
|-------|------|----------|-------|
| "Learning to Move: Camera Motion Prediction with LSTMs" | 2020 | LSTM on MoCap data | Predicts future camera frames from past |
| "Neural Camera Motion Style Transfer" | 2021 | 1D Conv Autoencoder | Transfers handheld style to dolly moves |
| "Deep Reinforcement Learning for Virtual Camera Control" | 2022 | PPO | Tracks actors with rule-of-thirds framing |
| "Diffusion Models for Motion Synthesis" | 2023 | DDPM | Generates realistic camera paths |

### 5.2 Open Source Projects

- **motion-forecasting** (GitHub): LSTM-based motion prediction
- **camera-control-rl** (GitHub): RL camera agent for game engines
- **style-motion-transfer** (GitHub): Neural style transfer for animations

### 5.3 Commercial Tools

| Tool | Use Case | Notes |
|------|----------|-------|
| **Mo-Sys StarTracker** | Camera tracking | Produces FBX output |
| **Stype RedSpy** | Virtual production | Real-time tracking |
| **Ncam Reality** | AR/camera tracking | Supports FBX export |
| **Unreal Live Link** | Real-time camera input | Streaming protocol |

---

## 6. What You Need to Accomplish This

### 6.1 Prerequisites

1. **FBX SDK** — Download from Autodesk (free for developers)
2. **Python environment** — Python 3.8+, PyTorch/TensorFlow
3. **Training data** — Hours of camera tracking recordings in FBX format
4. **Target engine** — Unreal Engine 5.x with Live Link or Level Sequence support

### 6.2 Development Roadmap

**Phase 1: Foundation (Weeks 1-2)**
- Install FBX SDK, test Python bindings
- Build FBX parser that extracts camera transforms
- Create normalization pipeline (coordinate align, resample, relative motion)
- Export to JSON/CSV

**Phase 2: Data Collection (Weeks 3-4)**
- Record training data from tracking system
- Build dataset of camera movements (label by type: handheld, crane, dolly, etc.)
- Create train/val/test splits

**Phase 3: Model Training (Weeks 5-8)**
- Train path prediction model (LSTM/TCN)
- Train style transfer model (Autoencoder or Diffusion)
- Evaluate on held-out test sequences

**Phase 4: Integration (Weeks 9-10)**
- Build inference pipeline (input partial path → output full path)
- Implement WebSocket streaming to Unreal
- Create Unreal Blueprint for real-time camera control

**Phase 5: Refinement (Weeks 11-12)**
- RL framing agent training
- A/B testing with human operators
- Performance optimization for real-time

### 6.3 Tech Stack Summary

```
Python 3.10+
├── FBX SDK (extraction)
├── NumPy/SciPy (normalization)
├── PyTorch (model training)
├── WebSocket (streaming)
└── OpenCV (visualization)

Unreal Engine 5.x
├── Live Link (real-time input)
├── Level Sequence (animation)
├── Blueprint (parsing logic)
└── Python Editor Utility (scripting)
```

---

## 7. Open Questions & Next Steps

- **Training data source:** Which tracking system produces your FBX files?
- **Real-time vs offline:** Do you need inference during live shoots or post-production?
- **Model priority:** Start with path prediction or style transfer?
- **Scale:** How many hours of tracking data do you have available?
- **Validation:** How will you evaluate model quality (human A/B tests, metrics)?

---

## References

- Autodesk FBX SDK Documentation: https://aps.autodesk.com/developer/overview/fbx-sdk
- PyTorch LSTM Tutorial: https://pytorch.org/docs/stable/generated/torch.nn.LSTM.html
- Stable-Baselines3 PPO: https://stable-baselines3.readthedocs.io/
- Unreal Live Link: https://docs.unrealengine.com/5.0/en-US/live-link-in-unreal-engine/
- DDPM Paper: https://arxiv.org/abs/2006.11239
