# Ocelos Virtual Production - LiveLink Support Call

> Transcribed from a support call on 2026-06-09 between the Ocelos support team (led by Lee Boer) and the VP stage team (Grant, Justin/Mike, Joel).

---

## 🎯 Issue

**Ocelos tracking data visible in Unreal Engine but NOT propagating to render nodes / LED volume.**

- Ocelos camera tracking was working normally in-engine (green check mark visible in LiveLink panel)
- Tracking data would NOT transfer to the stage nodes — nothing visible on the LED wall
- Problem was sporadic — worked fine previously, started failing ~1.5–2 weeks before the call
- Different behavior than what they experienced at the Fort Worth stage (Joel's team)

---

## 👥 Participants

| Name | Role |
|------|------|
| **Lee Boer** | Ocelos Support (lead troubleshooting) |
| **Grant** | Stage Operator (had mic issues initially) |
| **Justin / Mike** | Stage team (sent video of issue earlier) |
| **Joel** | Fort Worth team (had tried to solve previously) |
| **Unnamed host** | Facilitated the call |

---

## 🔍 Root Cause

**Multiple issues identified:**

1. **LiveLink Hub not being used** — Data was being sent to multiple destinations individually instead of through a central hub
2. **Ocelos sending 3D protocol** — The video showed OpenTruckIO, which Ocelos doesn't support yet (only 3D protocol currently)
3. **LiveLink plugin not enabled on all nodes** — The LiveLink plugin was disabled on some render nodes
4. **Missing plugins** — LiveLink Lens and LiveLink Camera plugins needed for lens/camera data were not enabled on all projects
5. **Ocelos sending to multiple destinations** — Destinations 2 and 3 were sending data elsewhere, potentially causing conflicts

---

## ✅ Resolution

### Immediate Fix
1. **Switch to LiveLink Hub** — Centralizes all tracking data distribution
2. **Set up via Switchboard** — Add LiveLink Hub as a device with proper command-line arguments
3. **Enable LiveLink plugin** on every render node (was disabled on some)
4. **Disable extra Ocelos destinations** — Only send to one place (the hub)
5. **Start order matters** — Hub starts first, then nodes and editor connect to it automatically

### Recommended Setup (LiveLink Hub via Switchboard)

```
1. Open Switchboard
2. Add Device → LiveLink Hub (3rd from top)
3. Name it (e.g., "LiveLink Hub" or "Tracking")
4. Set IP to the hub computer's IP
5. In device Settings → Command Line Arguments:
   - Add: -enable=LiveLink3DProtocol
   (Note: the little prefix before -enable is also required)
6. Start LiveLink Hub FIRST
7. Then start all nodes and editor — they auto-connect
```

### Required Plugins (per project)
- ✅ **LiveLink Hub** plugin
- ✅ **LiveLink Lens** plugin (for lens data)
- ✅ **LiveLink Camera** plugin (for camera data)
- ✅ **LiveLink** plugin (on all nodes)

### LiveLink Source Configuration
- Use `0.0.0.0` (zeros) for the source — checks on every IP and port
- Avoid specific IP addresses (e.g., `192.168.x.x`) — if IP/port changes, tracking breaks

---

## 🔮 Future Notes

- **OpenTrackIO support coming** (expected by end of 2026) — will replace 3D protocol
- When available: only need to change the protocol in the hub, and it distributes to all nodes automatically
- **Clean up old LiveLink profiles** — remove CSUS and 3D profiles, keep only hub-based setup
- Hub-based setup is more manageable and less prone to configuration drift

---

## 💡 Key Takeaways

1. **Always use LiveLink Hub** for multi-node VP stages
2. **Hub starts first**, everything else connects after
3. **0.0.0.0 source** is safest — catches any IP/port changes
4. **All plugins must be enabled on ALL nodes** (LiveLink, Lens, Camera)
5. **One Ocelos destination** — don't scatter tracking data to multiple endpoints
6. **Use Switchboard** to manage hub startup and configuration

---

*Source: Support call recording, 2026-06-09, ~29.5 minutes*
