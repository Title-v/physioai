# 🏗️ PhysioAI — Architecture Diagrams

C4-style architecture diagrams for the PhysioAI senior project.

---

## 📁 Files

| File | What it shows | Level |
|---|---|---|
| `PhysioAI_Container.drawio` | All applications + data stores + users | C4 Level 2 — Container |
| `PhysioAI_Component.drawio` | Internal components of the Mobile App | C4 Level 3 — Component |

---

## 🖼️ Diagram 1 — Container (C4 L2)

**Purpose:** Show how the system is structured into runnable things (apps, services, databases) and how they talk.

**Actors:**
- **Patient** (ผู้ป่วย / ผู้พิการ) — uses Mobile App
- **Therapist** (นักกายภาพบำบัด) — uses Web Dashboard

**Containers:**
| Container | Tech | Role |
|---|---|---|
| Mobile App | React Native + Expo | On-device AI, TTS, accessibility modes |
| Web Dashboard | React | Therapist monitoring + analytics |
| Backend API | Node.js + Express + Socket.io | REST + WebSocket, business logic |
| PostgreSQL | Relational DB | Users, sessions, exercise library, reference poses |
| Redis | In-memory cache | Session cache, hot exercise data |
| Firebase Auth | Google SaaS (external) | Token issuance + verification |

**Key flows:**
- Patient → Mobile → Backend (REST + WSS) → PostgreSQL / Redis
- Therapist → Web → Backend (REST + WSS) → same data
- Backend → Firebase Auth (verify JWT)

---

## 🖼️ Diagram 2 — Component (C4 L3) · Mobile App

**Purpose:** Zoom into the Mobile App and show its internal components, organized in 5 layers.

### Layers

**UI Layer** (blue)
- Login Screen
- Exercise List
- **Exercise Session** (main — camera + realtime AI)
- Progress Screen
- Settings

**Controller Layer** (green)
- Auth Manager
- **Session Controller** (orchestrator)
- Accessibility Mode Controller (5 modes)

**AI / Processing Layer** (amber)
- Camera Module (Expo Camera)
- Pose Detector (MediaPipe BlazePose + TFLite)
- Angle Comparator (vs reference pose)
- Thai TTS Engine (expo-speech)
- Visual / Haptic Feedback (deaf-friendly)

**Data Access Layer** (pink)
- Local Storage (AsyncStorage / offline)
- **Session Recorder** (keypoints + score) ← moved here from Controller layer
- API Client (REST / axios)
- WebSocket Client (socket.io-client)

**External** (purple, dashed)
- Backend API
- Firebase Auth

### Key component flows

1. **Live exercise loop:**
   `Exercise Session UI → Session Controller → Camera → Pose Detector → Angle Comparator → (score back up) → Session Controller → Mode Controller → TTS / Visual feedback`

2. **Save session:**
   `Angle Comparator → Session Recorder → API Client → Backend API`

3. **Offline fallback:**
   `Session Recorder → Local Storage` (sync later when online)

4. **Real-time notifications:**
   `Session Controller → WebSocket Client → Backend API` (subscribe for therapist messages, live monitoring)

---

## 🎨 Color Legend

| Color | Meaning |
|---|---|
| 🟦 Blue | User / UI Layer |
| 🟩 Green | Controller / Application Layer |
| 🟧 Amber | AI / Processing Layer |
| 🩷 Pink | Data Access Layer |
| 🟪 Purple (dashed) | External system (outside our control) |
| 🟨 Yellow cylinder | Database |

---

## 🛠️ How to Open / Edit

**Option 1 — diagrams.net (recommended):**
1. Go to [app.diagrams.net](https://app.diagrams.net)
2. **Open Existing Diagram** → choose `.drawio` file
3. Edit / style / add shapes → Save

**Option 2 — VS Code:**
- Install extension: **Draw.io Integration** (by Henning Dieterichs)
- Open `.drawio` file directly in VS Code

**Option 3 — Desktop app:**
- Download from [drawio-desktop](https://github.com/jgraph/drawio-desktop/releases)

---

## 🔁 How to Regenerate (from Python source)

Source script: `/Users/rattee/Library/.../outputs/make_architecture_drawio.py`

```bash
cd /path/to/outputs
python3 make_architecture_drawio.py
```

Output path in script (line ~8):
```python
OUT_DIR = "/sessions/.../mnt/PhysioAI/Architecture"
```

**What to edit in the script:**
- **Add/remove container** → find `d1.add_vertex(...)` block in DIAGRAM 1 section
- **Add/remove component** → find `d2.add_vertex(...)` block in DIAGRAM 2 section
- **Change connection** → find `d1.add_edge(...)` or `d2.add_edge(...)`
- **Adjust layout** → change `x, y, width, height` on the vertex calls
- **Change colors** → edit the `*_style()` functions near top

---

## 📤 Export to PNG / PDF / SVG

In diagrams.net:
1. `File → Export as → PNG` (or PDF / SVG)
2. Recommended: **Border Width: 10**, **Zoom: 200%** for high-res image
3. For reports: export to PNG at 150 DPI minimum

---

## 📝 Notes / Decisions

- **AI runs on-device** (MediaPipe + TFLite) — no video uploaded to server → privacy + low latency + no GPU server cost
- **Backend stores only keypoints + score**, not raw video
- **Accessibility Mode Controller** is cross-cutting — it decides whether Session Controller sends events to TTS, Haptic, or Visual layer
- **WebSocket (Socket.io)** is used for: therapist notifications, real-time dashboard updates, live monitoring sessions
- **Firebase Auth** is the only external dependency beyond hosting — chosen over custom auth for speed + security

---

## 🔗 Related Docs

- Full senior project report: `../PhysioAI_SeniorProject_Full.docx`
- System flow canvas: `../PhysioAI Flow/PhysioAI_Flow_Canvas.drawio`
- Gantt timeline: `../PhysioAI Grant timeline/PhysioAI_Gantt.drawio`
- Session handoff for Claude: `../ClaudeHandoff.md`
