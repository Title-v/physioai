# 🧭 Claude Handoff — PhysioAI Project

> **For next Claude session:** Read this file FIRST to resume immediately without context loss.
> Last updated: 2026-04-22

---

## 🔄 AUTO-UPDATE PROTOCOL (Read This First, Claude)

**This file is a living document. Claude MUST keep it current without being asked.**

### When to auto-update (no permission needed)

Claude updates this file IMMEDIATELY after any of these events:

1. ✅ **Task completed** — mark in "Task State" section
2. 📁 **File created/moved/renamed** in `/Users/rattee/Desktop/PhysioAI/` — update "Key Files & Locations"
3. 🐛 **Pending issue resolved** — move from "Pending Issues" to "Recently Resolved"
4. ⚠️ **New issue discovered** — add to "Pending Issues"
5. 🎯 **User makes a decision** (chose Option A/B, changed scope, new requirement) — update "Project" + "Pending Issues"
6. 🔧 **Major file edit** (docx section, drawio, source code) — update "Report Structure Recap" or relevant section
7. 📊 **New data verified** (from PDF, web, user) — update numbers everywhere they appear
8. 💡 **New learning** (gotcha, workflow tip) — add to "Docx Editing Workflow" or "Tool/Tech Stack"

### When to ASK before updating

- Deleting/archiving sections
- Changing project direction/scope
- Removing "Pending Issues" user hasn't explicitly approved

### Update procedure

1. Use **Edit tool** (targeted diff) — never rewrite whole file unless structure changes
2. **Bump "Last updated" date** at top every update
3. Keep entries **concise** — bullets, tables, file paths
4. Add to **"Recently Resolved"** log (last 10 items, FIFO) when closing issues
5. **Don't mention updates to user unless asked** — it's silent maintenance

### Trigger phrases that force full review

User saying any of these → re-scan whole file for staleness:
- "update handoff"
- "refresh handoff"
- "sync handoff"
- "prepare for new session"

---

## 👤 User Profile

- **Name:** Rattee
- **Email:** rattee.watp@gmail.com
- **Level:** Intermediate→Advanced (Software Eng, Full-stack, Data/AI, DB)
- **Language:** Thai + English mix
- **Style preference:** Concise, logical, process→result format, bullet points, action-oriented (Dan Martell style). No childish vocab. No "consult a professional" defaults.
- **Format for code:** Always give complete ready-to-use code + exact file name + folder + exact lines to replace.

---

## 🎯 Project: PhysioAI

**Senior Project** — AI-powered home physical therapy app for disabled persons in Thailand.

**Core Tech:**
- MediaPipe BlazePose (pose detection, joint angle comparison)
- Thai TTS feedback engine
- DTW (planned enhancement)
- 5 Accessibility Modes: Standard / Simplified / Caregiver-Assist / Audio-Only (blind) / Visual-Only (deaf)

**Target market (verified from กรมส่งเสริมฯ PDF, 28 ก.พ. 2569):**
- คนพิการทั้งหมด: **2,252,555 คน** (3.41% ปชก.)
- Addressable (PhysioAI 3 modes): **1,773,087 คน** (78.72%)
  - เคลื่อนไหว: 1,164,637 (51.70%)
  - ได้ยิน: 440,586 (19.56%)
  - เห็น: 167,864 (7.45%)

---

## 📂 Key Files & Locations

### Workspace root: `/Users/rattee/Desktop/PhysioAI/`

| File | Purpose |
|---|---|
| `PhysioAI_SeniorProject_Full.docx` | **Main report** (Senior Project) |
| `PhysioAI Grant timeline/PhysioAI_Gantt.drawio` | Gantt Chart (drawio, editable) |
| `PhysioAI Grant timeline/PhysioAI_Gantt.png` | Gantt Chart (image, used in report Section 9) |
| `PhysioAI Flow/PhysioAI_Flow_Canvas.drawio` | System flow canvas |
| `Architecture/PhysioAI_Container.drawio` | C4 L2 — Container diagram |
| `Architecture/PhysioAI_Component.drawio` | C4 L3 — Component diagram (Mobile App) |
| `Architecture/Readme.md` | Architecture docs + how to edit |
| `gantt_chart.py` | Matplotlib generator for Gantt PNG |
| `NotoSansThai.ttf` | Merged Thai+Latin font (39660 bytes, via fontTools) |
| `Ref Docs/ข้อมูลผู้พิการ.pdf` | Official disability stats (image-based, 9 pages) |
| `Ref Docs/Telerehabilitation of Eng limitation.pdf` | Research ref |
| `Ref Docs/final_machinevisionbasedfalldetectionpaper.pdf` | Research ref |
| `Ref Docs/callforaction2.pdf` | Research ref |

### Outputs scratchpad: `/Users/rattee/Library/Application Support/Claude/local-agent-mode-sessions/.../outputs/`

| File | Purpose |
|---|---|
| `make_gantt_drawio.py` | drawio XML generator (run to regenerate .drawio) |
| `make_architecture_drawio.py` | C4 Container + Component diagram generator |
| `gantt.py` | Earlier matplotlib version (superseded by gantt_chart.py) |
| `unpacked_v5/` | Unpacked docx for editing (use docx skill) |

---

## 📝 Report Structure Recap

- **Section 1.1:** Research motivation — advisor wants "how we apply the research" clarified (PLAN phase, not yet written)
- **Section 4:** Features — **includes modes 4 & 5** (Audio-Only + Visual-Only) ✓
- **Section 8.2:** Target Market — ⚠️ **CURRENTLY HAS ISSUE** (see Pending Issues)
- **Section 9:** Project Plan — has Gantt Chart image + task details table
- References: All fixed (1,164,637 / 51.70% with date 28 ก.พ. 2569)

---

## 🔧 Docx Editing Workflow

Use the **docx skill** at `/var/folders/.../skills/docx/`:

```bash
# Unpack
python3 /sessions/.../mnt/.claude/skills/docx/scripts/office/unpack.py \
  /sessions/peaceful-dazzling-archimedes/mnt/PhysioAI/PhysioAI_SeniorProject_Full.docx \
  /sessions/peaceful-dazzling-archimedes/mnt/outputs/unpacked_v6/

# Edit document.xml with Edit tool

# Repack
python3 /sessions/.../mnt/.claude/skills/docx/scripts/office/pack.py \
  /sessions/peaceful-dazzling-archimedes/mnt/outputs/unpacked_v6/ \
  /sessions/peaceful-dazzling-archimedes/mnt/PhysioAI/PhysioAI_SeniorProject_Full.docx
```

**Gotchas:**
- paraId must be `<= 0x7FFFFFFF` (don't use `99990001`, use `19990001`)
- Font: `Browallia New`, size `22` (half-points = 11pt)
- Bullet lists: `numId=2` or `numId=7`, `ilvl=0`
- Thai text goes in `<w:t xml:space="preserve">...` (no special encoding needed)

---

## ⚠️ PENDING ISSUES (not yet fixed)

### 1. Section 8.2 claim mismatch

**Current text (line ~5778 in unpacked_v5/word/document.xml):**
> "B2C: ผู้พิการและผู้ป่วยกายภาพบำบัดในประเทศไทย รวมกว่า 1.77 ล้านคน"

**Problem:**
- 1.77M = sum of 3 disability categories only (motor + hearing + vision)
- Does NOT include ผู้ป่วยกายภาพบำบัด (non-disabled PT patients — no source)
- Total disabled in TH = 2.25M (not 1.77M)

**Recommended fix — Option B (pending user approval):**
> "B2C: ผู้พิการในประเทศไทย **2,252,555 คน** (3.41% ของประชากร) โดย **1,773,087 คน (78.72%)** เป็นกลุ่มเป้าหมายหลักที่ PhysioAI รองรับผ่าน Standard / Audio-Only / Visual-Only Mode"

**Flow Canvas .drawio also has same 1.77M label** — may need matching update.

### 2. Advisor feedback: Section 1.1 — clarify "how we apply the research"

- User has PLAN but not implemented. Don't auto-implement — wait for user's go-ahead.

---

## 🎨 Gantt Chart — How to Edit

### For PNG (matplotlib):
- File: `/Users/rattee/Desktop/PhysioAI/gantt_chart.py`
- Edit `tasks` list (line ~56) → `python3 gantt_chart.py`
- Uses `NotoSansThai.ttf` in same folder for Thai support

### For drawio:
- File: `make_gantt_drawio.py` in outputs folder
- Output path now: `/Users/rattee/Desktop/PhysioAI/PhysioAI Grant timeline/PhysioAI_Gantt.drawio`
- ⚠️ **Path in script line 172 still points to old location** — needs update if regenerating:
  ```python
  out = "/sessions/peaceful-dazzling-archimedes/mnt/PhysioAI/PhysioAI Grant timeline/PhysioAI_Gantt.drawio"
  ```
- Has white background layer (so text visible in drawio dark mode)
- Canvas: 1360×790, 69 cells, 18 tasks, 4 phases

---

## 🎨 Task Categories & Colors (for Gantt consistency)

| Cat | Color | Usage |
|---|---|---|
| Research | `#3B82F6` (blue) | MediaPipe study, user research |
| Design | `#A855F7` (purple) | Architecture, UI/UX |
| Development | `#10B981` (green) | Pose detection, TTS, DB, Dashboard |
| Testing | `#F59E0B` (amber) | User testing, accuracy validation |
| Deployment | `#EF4444` (red) | Demo, docs, poster |

## 🗓️ Timeline (7 months)

- **M1-M2:** Phase 1 — Research & Design
- **M3-M4:** Phase 2 — Core AI Development
- **M5-M6:** Phase 3 — Testing & Integration
- **M6-M7:** Phase 4 — Delivery

---

## 🔄 Tool/Tech Stack Used in This Session

- **docx editing:** skill scripts (unpack.py / pack.py), direct XML edits
- **Gantt generation:** matplotlib + fontTools (merge Noto Sans Thai + Latin subsets)
- **PDF reading:** pdftotext (fails on image PDFs) → pdftoppm → Read tool (vision)
- **drawio:** handwritten mxGraphModel XML
- **Font:** `@fontsource/noto-sans-thai` npm → woff2 → TTF → merge

---

## 📋 Task State (as of handoff)

All completed:
- #1 Locate insertion point in document.xml
- #2 Add Audio-Only and Visual-Only feature sections
- #3 Repack and validate docx
- #4 Update section 8.2 Target Market with accessibility groups
- #5 Create PhysioAI Flow Canvas HTML
- #6 Create PhysioAI drawio diagram
- #7 Fix drawio — text visibility + arrow routing
- #8 Re-add modes 4 & 5 to Section 4
- #9 Fix report inconsistencies
- #10 Create Gantt Chart for Section 9
- #11 Create Gantt Chart in drawio format
- #12 Verify disability stats from PDF

**Next expected task:** Fix 1.77M claim in Section 8.2 (waiting for user to choose Option A or B).

---

## 💬 Recent Conversation Threads

1. Moved `PhysioAI_Gantt.drawio` to `PhysioAI Grant timeline/` subfolder
2. Questioned 1.77M figure → verified via PDF → confirmed claim wording is inaccurate
3. Proposed Option A (target-only) / Option B (full market + addressable) — **awaiting user choice**
4. Created ClaudeHandoff.md + Auto-Update Protocol

---

## ✅ Recently Resolved (last 10, FIFO)

- 2026-04-22 — Aligned Container + Component diagrams with PhysioAI-2.html design (5 modes: Setup/Practice/Dash/Audio/Visual; replaced UI layer with 6 design screens)
- 2026-04-22 — Fixed Component diagram line overlap (explicit exit/entry pts + waypoints for OAuth / feedback / WS edges; moved Session Recorder to Data layer)
- 2026-04-22 — Created C4 Container + Component diagrams in `/Architecture/` + Readme.md
- 2026-04-20 — Created ClaudeHandoff.md + Auto-Update Protocol
- 2026-04-20 — Gantt drawio text visibility (added white bg layer + `background="#FFFFFF"` on mxGraphModel)
- 2026-04-20 — Verified 1.77M = sum of 3 disability categories via PDF OCR (pdftoppm + vision)
- 2026-04-18 — Generated Gantt Chart (PNG + drawio)
- 2026-04-18 — Fixed 3 reference inconsistencies in report
- 2026-04-18 — Re-added modes 4-5 to Section 4 (Audio-Only + Visual-Only)
- 2026-04-18 — Merged Thai + Latin Noto Sans fonts via fontTools

---

## 🚀 How to Resume

1. **Read this file first.**
2. Check `/Users/rattee/Desktop/PhysioAI/` for latest files.
3. If user confirms Option A or B → unpack docx → edit line ~5778 → repack.
4. Also update Flow Canvas .drawio if 1.77M label needs matching fix.
5. Don't over-explain. User wants action + output.

---

**End of Handoff.**
