# PhysioAI

AI-powered home physical therapy app for disabled persons in Thailand.
On-device pose detection (MediaPipe BlazePose) + Thai TTS feedback + 5 accessibility modes.

---

## 📁 Project structure

```
PhysioAI/
├── App/mobile/                  ← React Native app (the runnable code)
├── Design - UI/                 ← HTML design canvas (browser preview)
├── Architecture/                ← C4 architecture diagrams (.drawio)
├── PhysioAI Flow/               ← System flow diagrams
├── PhysioAI Grant timeline/     ← Gantt chart
├── Ref Docs/                    ← Research references (PDFs)
├── PhysioAI_SeniorProject_Full.docx   ← Main senior project report
└── ClaudeHandoff.md             ← Session handoff notes
```

---

## 🚀 Quick start — run on your iPhone (recommended, < 2 min)

This is the fastest path. **Camera, TTS, and haptics all work** on a real device.

1. On iPhone: install **Expo Go** from the App Store
2. iPhone and Mac on the **same Wi-Fi network**
3. In Terminal:
   ```bash
   cd /Users/rattee/Desktop/PhysioAI/App/mobile
   npm install        # first time only
   npx expo start
   ```
4. Open iPhone **Camera app** (not Expo Go) → aim at the QR code in Terminal
5. Tap the yellow banner that appears → app loads in Expo Go

> 📝 The same QR works on Android — install Expo Go from Play Store, open it, tap "Scan QR code."

---

## 💻 Run on iOS Simulator

**One-time setup:**

```bash
# 1. Install Xcode from App Store, then:
xcode-select --install
sudo xcodebuild -license accept

# 2. Install iOS Simulator runtime
xcodebuild -downloadPlatform iOS

# 3. Create a simulator device (use the runtime ID from `xcrun simctl list runtimes`)
xcrun simctl create "iPhone 17 Pro" \
  com.apple.CoreSimulator.SimDeviceType.iPhone-17-Pro \
  com.apple.CoreSimulator.SimRuntime.iOS-26-3
```

**Every time you want to run:**

```bash
cd /Users/rattee/Desktop/PhysioAI/App/mobile
npx expo start
# Then press `i` in the Metro terminal
```

> ⚠️ Simulator has **no camera hardware** — the Exercise Session screen will be black. UI, TTS, haptics, and the mock pose loop still work. Use a real iPhone if you want to see camera.

---

## 🤖 Run on Android Emulator

**One-time setup:**

1. Install **Android Studio** from https://developer.android.com/studio
2. Open Android Studio → **More Actions → Virtual Device Manager** → **Create Device**
3. Pick **Pixel 7** → API **34 (Android 14)** → Finish
4. Boot the emulator (▶ in AVD Manager)
5. Add to `~/.zshrc`:
   ```bash
   export ANDROID_HOME=$HOME/Library/Android/sdk
   export PATH=$PATH:$ANDROID_HOME/emulator:$ANDROID_HOME/platform-tools
   ```
6. `source ~/.zshrc`

**Every time:**

```bash
cd /Users/rattee/Desktop/PhysioAI/App/mobile
npx expo start
# Then press `a` in the Metro terminal
```

> 💡 Android emulator **can** use your Mac's webcam as the phone's camera — in AVD Extended Controls (⋯ button) → **Camera → Webcam0**.

---

## 🔁 Hot reload while you code

Leave `npx expo start` running. Edit any `.tsx` file → save → app reloads in ~1 sec.

Useful Metro hotkeys (press in the Metro terminal):

| Key | Does |
|---|---|
| `r` | Reload app |
| `j` | Open debugger |
| `m` | Toggle dev menu on device |
| `shift+r` | Reload with cache clear |
| `?` | Show all commands |

---

## 🧰 Recommended IDE

**VS Code** (https://code.visualstudio.com)

```bash
cd /Users/rattee/Desktop/PhysioAI/App/mobile
code .
```

Install these extensions (Cmd+Shift+X → search):
- `Expo Tools`
- `ESLint`
- `Prettier`
- `Error Lens`
- `Pretty TypeScript Errors`

Integrated terminal: Ctrl+\` — run `npx expo start` right inside the editor.

---

## 🛠️ Troubleshooting

### `CommandError: No iOS devices available in Simulator.app`
You haven't created a simulator device yet. Run:
```bash
xcrun simctl list runtimes      # confirm a runtime is installed
xcrun simctl create "iPhone 17 Pro" com.apple.CoreSimulator.SimDeviceType.iPhone-17-Pro com.apple.CoreSimulator.SimRuntime.iOS-26-3
```

### Metro stuck / weird cache errors
```bash
npx expo start -c       # clear cache and restart
```

### `Module not found` after pulling new code
```bash
rm -rf node_modules package-lock.json
npm install
npx expo start -c
```

### Camera shows black in iOS Simulator
**Expected.** Simulator has no camera hardware. Use a real iPhone via Expo Go.

### Thai TTS doesn't speak
- **iOS Simulator**: install the Thai voice — Simulator → Settings → Accessibility → Spoken Content → Voices → Thai
- **Real device**: should work out of the box. Check Mac/phone volume.

### Port 8081 already in use
```bash
lsof -ti:8081 | xargs kill -9
npx expo start
```

---

## 🏗️ Tech stack

| Layer | Tech |
|---|---|
| Mobile | React Native + Expo SDK 54 + TypeScript |
| Navigation | @react-navigation/native-stack |
| Camera | expo-camera |
| Thai TTS | expo-speech (language `th-TH`) |
| Haptics | expo-haptics |
| Storage | @react-native-async-storage/async-storage |
| Networking | axios + socket.io-client |
| Icons / Charts | react-native-svg |
| Pose detection | MediaPipe BlazePose (currently mocked, see `src/ai/poseDetector.ts`) |

---

## 📂 Where things live

```
App/mobile/src/
├── ai/              ← AI / Processing layer
│   ├── angle.ts            joint angle math
│   ├── poseDetector.ts     pose detection (currently MockPoseDetector)
│   ├── poseComparator.ts   compares live pose vs reference
│   ├── tts.ts              Thai text-to-speech
│   ├── haptic.ts           haptic feedback
│   └── feedbackBus.ts      dispatches feedback based on accessibility mode
├── controllers/     ← Application orchestration
│   ├── AccessibilityModeController.tsx   5 modes (standard / simplified / caregiver / audio-only / visual-only)
│   ├── AuthManager.tsx                   sign in / out
│   └── SessionController.ts              runs the exercise session loop
├── data/            ← Data access
│   ├── localStorage.ts        AsyncStorage wrapper
│   ├── apiClient.ts           REST client to backend
│   ├── wsClient.ts            Socket.io client
│   └── sessionRecorder.ts     records + uploads session data
├── ui/              ← UI layer
│   ├── screens/   Login, ExerciseList, ExerciseSession, Progress, Settings
│   └── components/ Button, Ico, Pill, Ring, ScoreBadge, ModeCard
├── types/           ← TypeScript type definitions
└── config/          ← Theme, sample exercises
```

Entry point: `App/mobile/App.tsx`

---

## 🎯 Next milestones

- [ ] Wire real **MediaPipe BlazePose** (via `react-native-fast-tflite`) — currently a mock
- [ ] Backend (Node + Express + Socket.io + PostgreSQL)
- [ ] Therapist web dashboard (React)
- [ ] Firebase Auth integration

---

## 📖 Documentation

- Architecture (C4 L2 + L3): `Architecture/Readme.md`
- Senior project report: `PhysioAI_SeniorProject_Full.docx`
- Session handoff: `ClaudeHandoff.md`
