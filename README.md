# Kinetix-OS
Kinetix-OS is a webcam-based hand gesture control system for desktop/browser interactions.
It detects hand landmarks in real time, classifies gesture postures, and routes recognized gestures to system actions (cursor movement, click, scroll, tab switching, app switching, and shortcuts).

## Highlights
- Real-time one-hand tracking using MediaPipe Tasks
- Smoothed, bounded cursor control through index-finger navigation posture
- Gesture-based click, scroll, tab switch, app switch, and shortcut triggers
- Configurable gesture-to-action mapping by mode (`BROWSER`, `MEDIA`)
- Cooldown and hold logic to reduce accidental repeated triggers

## Repository layout
```text
Kinetix-OS/
├── README.md
├── kinetix_mvp_dashboard.html
└── backend/
    ├── main.py
    ├── config.py
    ├── cursor.py
    ├── gestures.py
    ├── workflow_router.py
    ├── mappings.json
    └── hand_landmarker.task
```

## How it works
1. `backend/main.py` opens webcam input and runs hand landmark detection.
2. Landmark points are converted to pixel coordinates and rendered in an overlay.
3. `backend/gestures.py` evaluates posture/gesture state:
   - navigation posture
   - scroll posture
   - pinch detection
   - two-finger swipe
   - whole-hand swipe
   - static finger-combo gestures
4. `backend/cursor.py` smooths and maps finger movement to screen coordinates.
5. `backend/workflow_router.py` applies cooldown/hold checks and executes mapped actions from `backend/mappings.json`.

## Requirements
- Python 3.10+ (recommended)
- Webcam
- OS input permissions for Python/Terminal (mouse + keyboard control)
- `backend/hand_landmarker.task` model file present

## Installation
From project root:

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install --upgrade pip
python3 -m pip install opencv-python mediapipe pyautogui numpy keyboard
```

## Running the app
Run from the `backend/` directory because the code uses local relative paths (`mappings.json`, `hand_landmarker.task`):

```bash
cd backend
python3 main.py
```

Exit with `Esc` in the OpenCV window.

## Default gestures and behavior
The system relies on posture gating to avoid conflicts:
- **Navigation posture** (index up, others down):
  - Cursor moves with index tip
  - Pinch (thumb + index close) triggers `PINCH_CLICK`
- **Scroll posture** (index + middle up, ring + pinky down):
  - Vertical index movement triggers `SCROLL_UP` / `SCROLL_DOWN`
- **Open palm + lateral movement**:
  - Triggers whole-hand app switching (`WHOLE_HAND_SWIPE_LEFT/RIGHT`)
- **Two-finger swipe posture + lateral movement**:
  - Triggers `SWIPE_LEFT` / `SWIPE_RIGHT`
- **Static finger combos**:
  - `INDEX_PINKY`
  - `INDEX_RING`

## Action routing
`backend/mappings.json` maps gesture names to action keys per mode.

<<<<<<< Updated upstream
## Notes
- `backend/hand_landmarker.task` must exist before startup.
- Some key bindings in `workflow_router.py` are OS-sensitive; adjust them if your platform layout differs.
# ⚡ KINETIX

**A Zero-Touch, Intent-Driven Spatial Computing Interface.**

[![Status](https://img.shields.io/badge/Status-Active_Development-10B981?style=for-the-badge)]()
[![Ecosystem](https://img.shields.io/badge/Architecture-Unified_Monorepo-20B2AA?style=for-the-badge)]()
[![AI](https://img.shields.io/badge/AI_Engine-VLM_Orchestration-7FFFD4?style=for-the-badge)]()

Kinetix breaks artificial intelligence out of the text box and gives it a physical presence in the real world. By translating raw human physical movement into digital action, Kinetix removes the barrier of traditional peripherals (mice, keyboards) and creates a hands-free, context-aware automation pipeline.

This project is a core module within a broader unified monorepo ecosystem, engineered to bridge the gap between low-level system tracking (hardware/OS manipulation) and high-level AI/GenAI (vision-language reasoning).

---

## 🎯 The Vision

Right now, humans must translate their intentions into keystrokes. Kinetix reverses this paradigm. Whether it is a surgeon in a sterile operating room needing to scroll through X-rays, an industrial worker with grease-covered hands reviewing schematics, or a user with motor impairments navigating the web—Kinetix makes the computer understand *you*.

## ✨ Core Features

* **Real-Time Spatial Perception:** Utilizes edge-based computer vision (MediaPipe) to instantly map skeletal joints of the hands, face, or body through any standard webcam.
* **Dynamic Gesture Mapping (Modes):** A highly fluid Next.js frontend that allows users to instantly remap physical gestures to digital actions via context bundles (e.g., *Browser Mode*, *Media Mode*, *Doctor Mode*, *Terminal Mode*).
* **VLM Orchestration Engine:** Integrates Vision-Language Models (VLMs) to understand screen context. Point at a paragraph and trigger a gesture, and the AI agent will read the screen coordinates and execute complex workflows (like summarizing text).
* **Zero-Latency Execution:** A deterministic Python/FastAPI backend calculates physical vectors (velocity, pinch distance) and fires system-level execution commands (via `pyautogui`, `pymouse`, etc.) via WebSockets.

---

## 🏗️ Architecture Pipeline

The system operates on a continuous loop of Perception, Reasoning, and Execution:

1.  **👁️ Perception (The Eyes):** OpenCV + MediaPipe captures physical movement.
2.  **⚙️ Math Engine (The Backend):** Python calculates the physics of the movement (velocity for swipes, distance for pinches, vectors for pointing).
3.  **🧠 Orchestrator (The Brain):** LangChain + VLM (GPT-4o/Claude) interprets complex visual queries based on screen context.
4.  **🦾 Execution (The Hands):** System-level scripts automatically move the cursor, scroll pages, or type text.

---

## 💻 Technology Stack

* **Frontend Interface:** Next.js, React, Tailwind CSS, Framer Motion *(Bioluminescent / Hacker-Architect aesthetic)*
* **Backend API & WebSockets:** Python, FastAPI, Uvicorn
* **Computer Vision:** OpenCV, Google MediaPipe
* **System Automation:** PyAutoGUI, keyboard

---

## 🚀 Getting Started

### Prerequisites
* Node.js (v18+)
* Python (3.10+)
* Standard Webcam

### Installation (Monorepo Setup)

**1. Clone the repository:**
```bash
git clone [https://github.com/susovanchatterjee/kinetix.git](https://github.com/susovanchatterjee/kinetix.git)
cd kinetix
=======
Key points:
- `TRAEWorkflowRouter` starts in `BROWSER` mode.
- Supported action handlers are defined in `ACTION_HANDLERS` inside `backend/workflow_router.py`.
- Cooldowns are dynamic:
  - open palm uses longer cooldown
  - scroll uses short cooldown
  - swipe/app-swipe have separate cooldowns
- Some gestures (`INDEX_PINKY`, `INDEX_RING`) require a short hold duration before firing.

## Tuning and customization
Most tuning happens in `backend/config.py`:
- Camera: `CAM_WIDTH`, `CAM_HEIGHT`
- Gesture thresholds:
  - `CLICK_THRESHOLD`
  - `SCROLL_THRESHOLD`
  - `SWIPE_THRESHOLD`
  - `SWIPE_BUFFER_SIZE`
- Cursor feel:
  - `DEADZONE`
  - `CURSOR_SPEED_DIVISOR`
  - `CURSOR_MAX_FACTOR`
- Timing:
  - `SCROLL_HOLD_DURATION`
  - `SCROLL_COOLDOWN`
  - `PER_GESTURE_COOLDOWN`
  - `OPEN_PALM_COOLDOWN`
  - `CALIBRATION_DURATION`

## Common issues
### 1) `hand_landmarker.task` missing
If startup prints:
`CRITICAL: 'hand_landmarker.task' missing.`
Place the file in `backend/` and rerun.

### 2) Gestures detected but no system action happens
- Verify OS accessibility/input permissions for Python/Terminal.
- Confirm your gesture exists in `mappings.json` under the active mode.
- Confirm the mapped action key exists in `ACTION_HANDLERS`.

### 3) Cursor feels jittery or too slow
- Increase `DEADZONE` to suppress micro-movements.
- Tune `CURSOR_SPEED_DIVISOR` / `CURSOR_MAX_FACTOR`.
- Adjust lighting/camera angle for cleaner landmarks.

### 4) Swipe gestures misfire
- Increase `SWIPE_THRESHOLD`.
- Increase `SWIPE_BUFFER_SIZE` for stronger smoothing.

## Safety notes
- This project can control mouse/keyboard globally.
- Test in a non-critical environment first.
- Keep one hand ready on physical keyboard/mouse to override behavior quickly.

## Future improvements (suggested)
- Add CLI flags / config profiles
- Add explicit mode-switch gestures
- Add unit tests for gesture math and routing
- Add packaging (`requirements.txt` / lockfile)
- Add structured logs instead of print statements

## Contributing
1. Fork/clone the repo
2. Create a feature branch
3. Make and test changes
4. Open a pull request with clear before/after behavior notes
>>>>>>> Stashed changes
