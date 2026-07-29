# skyAgent Workspace — prototype

A front-end prototype of the **skyAgent Workspace** for Skypoint's skyCommand, reworked into a
Claude-app-style multi-mode experience (**Chat / Cowork / Code**) in the Radix design system.

**▶ Live preview (desktop):** https://awinashsumit.github.io/skyagent-workspace/

Open it, click **Workspace** in the left sidebar, then the **⤢ full-screen** icon (top-left of the
workspace rail) for the immersive experience.

**▶ Live preview (mobile):** https://awinashsumit.github.io/skyagent-workspace/mobile.html

A standalone, native-app-style phone build of the Workspace — top app bar + slide-in drawer, bottom-sheet
model / approval / project pickers, sticky composer, simulated streaming and Cowork runs. Best on a phone
(or a narrow browser window); on desktop it renders inside a phone frame.

## Try this (demo path)
- Switch modes: **Chat · Cowork · Code** — each has its own home
- **Chat** — send a message → watch it **stream** (Stop / Regenerate); type **/** for the skills palette
- **Cowork** — pick a suggested task → a **live run** executes step-by-step and produces a result artifact
- Rail → **Artifacts** — the run you just did appears here → open one → **Copy**
- Rail → **Projects** — open a project or create one
- **Code** — the usage-stats dashboard (Overview / Models, contribution heatmap)
- Keyboard: **⌘/Ctrl + \\** collapse rail · **⌘/Ctrl + K** search · **F** full-screen · **Esc** close

## Notes
This is a **front-end prototype** — streaming, runs, and data are **simulated**; state persists in your
browser via `localStorage`. There is no backend.

## Run locally
```bash
node serve.js   # http://localhost:4599
```
(It's fully static — you can also open `index.html` directly.)
