# skyAgent Workspace — Claude-style Rework Checklist

**Goal (client):** skyAgent Workspace should feel like the Claude app — Chat / Cowork / Code-style
modes, a shared shell, real-time (websocket) streaming — "a web app that's like a desktop app,"
matching Claude's UI ratio/spacing. Translate into the Skypoint radix design system, don't pixel-copy.

**Legend:** `[ ]` todo · `[~]` in progress · `[x]` done · `[?]` needs a decision (see §0)

**Design learnings to hold on every item** (from prior sessions):
- Translate, don't replicate — adopt Claude's *layout system*, render in *our* DS (amber, Inter, our components).
- Color contract — amber = brand/selection only; green/blue/orange/red = status only.
- Icon minimalism — an icon only if it adds state/action/launcher info.
- One pattern per role — mode switch = segmented; list-filter = underline tabs; don't mix.
- Proportional scale, real grid, WCAG AA contrast, 44px targets, keyboard/ARIA, focus traps.
- Design the *transitions between states*, not just the empty one (empty / loading / streaming / error / N-items).
- Run the pre-flight (Nielsen + Laws of UX) BEFORE showing work.

---

## 0. Decisions — LOCKED (2026-07-20)
- [x] **Architecture** — Workspace is reachable from the dashboard (embedded), PLUS a **full-screen toggle**:
      expand → immersive Claude-style layout (own rail with mode switcher + recents, dashboard nav hidden),
      restore button → back to the dashboard shell. Build the immersive layout as the target; embedded is the entry.
- [x] **Third surface** — **literal parity: Chat / Cowork / Code.** Client wants full Claude parity.
      Code = a sessions + usage-stats surface (skyAgent flavor: automations / data queries / agent configs), same
      layout as Claude's Code home (stats dashboard + sessions list + composer with scope chips & Accept-edits mode).
- [x] **Real-time** — **front-end prototype**: simulate streaming/websocket on a timer; mock runs; seed in localStorage.
- [x] **Greeting** — **Inter** (DS-consistent); no second typeface.
- [~] **Scope for pass 1** — P0 below; confirm before build.

---

## 1. App shell & layout (Claude proportions)
- [x] **Full-screen toggle** — expand Workspace to immersive (own rail, dashboard header+nav hidden) ↔ restore. Esc + `F` key support. *(fixed a latent ≤1200px grid bug where the right panel didn't hide → 2-row wrap.)*
- [x] Left rail ~260px, fixed; content column; composer pinned to bottom, full height.
- [x] Rail: brand/lockup + full-screen toggle. Search present (existing).
- [x] **Persist immersive state** — saved on toggle; restored when you enter the workspace (not on cold load, so other views stay normal).
- [x] **Sidebar collapse toggle** — button in the conversation header + `⌘/Ctrl+\` collapses/restores the rail (conversation reclaims the width, smooth transition).
- [ ] Header zone: dismissible banner slot + "What's new" affordance (optional).
- [x] **Profile + plan footer** in the rail (avatar, name, plan tier) — pinned to the bottom.
- [x] Spacing rhythm/ratios aligned to our `--space-*` grid (Claude proportions).

## 2. Mode system
- [x] Segmented mode switcher at top of rail (Chat / Cowork / Code) — reuses `.view-toggle` neutral-active treatment (one-pattern-per-role).
- [x] Each mode swaps greeting, primary-action label, and composer placeholder; shell stays put.
- [x] **Persist last-used mode** across reloads.
- [ ] Per-mode rail items (Scheduled/Dispatch on Cowork; sessions on Code) beyond the links.

## 3. Left rail (per mode)
- [x] Primary action per mode: New chat / New task / New session.
- [x] **Per-mode rail links** — Chat: Projects·Artifacts·Customize · Cowork: +Scheduled(2)·Dispatch(Beta) · Code: Artifacts·Sessions(9)·Routines·Customize. *(presentational; wiring is P1)*
- [x] Recents list with "Show more" (existing).
- [x] **Recents is live** — sending a chat or launching a run prepends an active entry under "Today".
- [x] All rail links open real destinations (Artifacts, Projects, Scheduled, Dispatch, Routines, Sessions, Customize) — no dead links.
- [x] **Search filters Recents** live (hides non-matching rows + empty group headers, "no matches" state, restores on clear).
- [ ] Extend search to projects/artifacts too.

## 4. Composer (shared)
- [x] Big input; greeting above it (empty state).
- [x] `+` attach menu (existing).
- [x] **Model + effort selector** (skyAgent Pro/Fast · High/Med/Low) — chip + popover, updates label + response model-tag.
- [x] Autonomy mode chip (Execute/Plan) — existing.
- [x] Send + Enter-to-send.
- [x] **`/` skills menu** — palette above composer, 10 skyAgent playbooks (/briefing, /occupancy, /staffing, /moveouts, /alert, …), filters as you type, ArrowUp/Down + Enter/Tab to select, Esc/outside-click to close.
- [x] **Mic / dictation** affordance (toggles a recording state).
- [ ] Cowork: project/folder scope selector (Code has scope chips — see §7).

## 5. Chat surface + streaming
- [x] Message thread (user / skyAgent turns) — existing markup reused.
- [x] **Streaming** token-by-token render (simulated websocket) with **Stop** + **Regenerate**; "thinking…" → "N tools · streaming" → feedback row + model-tag.
- [x] New chat/task/session clears the thread for a fresh conversation.
- [x] Empty → first-message → streaming → done states. Error(retry) state exists (existing callout).
- [x] **Collapsible "Thought process" block** on streamed agent turns (reasoning steps).
- [x] **Copy per response** (feedback row, green confirm) + **Edit-and-resend** on user messages (hover pencil → repopulates composer).
- [ ] Inline artifacts open in a side panel from within a message; citations.

## 6. Cowork surface
- [x] "What can I take off your plate?" home + **suggested task cards** (mode-aware empty state; 4 operational tasks).
- [x] **Delegate → live run view** — sequential steps (pending → spinner → check), progress bar, N/N + elapsed, "Running"→"Done" status. Suggested-card click OR typing a task + Send in Cowork both launch a run (Chat still streams).
- [x] Task result → **result summary + artifact card** (Open/Copy).
- [x] **Run → Artifacts loop closed** — completed runs register their work product in the Artifacts gallery; Open jumps to it.
- [x] **Stop/cancel a run** (Cancel button → "Cancelled" state, steps reset, elapsed "stopped"); run persists in Recents.
- [ ] Project/folder scope + autonomy wiring.

## 7. Code surface (literal parity) + usage stats
- [x] "Code" = sessions surface; New session primary; rail links Artifacts/Sessions/Routines/Customize; composer placeholder ("automation, query, or agent config").
- [x] Home = usage **stats dashboard** — 8 stats (Sessions/Messages/Tokens/Active days/Current & Longest streak/Peak hour/Top model) + 18×7 **contribution heatmap** (amber intensity = brand data-viz, not status) + fun note.
- [x] **Overview / Models** tabs (underline, per role) + **All/30d/7d** range (segmented, per role); Models tab shows model-usage bars.
- [x] **Composer scope chips** (Local / Website / + Add source) + **Accept-edits** autonomy (Accept edits → Suggest only → Auto-run) — shown only in Code mode, matching Claude's Code composer.
- [x] **Sessions** rail link opens a real panel (4 sessions, mono filenames + Saved/Draft status).
- [ ] Range actually re-computes stats.

## 8. Projects
- [x] **Project gallery** — opens from rail "Projects"; 4 seeded projects (community/initiative groupings) with name, description, chats/artifacts counts; New-project button.
- [x] **Create project** — name-required form (validation) + description + instructions; persists to localStorage, prepends to gallery.
- [x] **Project detail** — name, description, editable **custom instructions** (context), contents (chats/artifacts), "Start a chat in this project" CTA; back-to-gallery.
- [x] Panels are mutually exclusive (opening Projects closes Artifacts and vice versa); Esc closes whichever is open.
- [ ] Actually scope a chat/task to the project + apply instructions; project files.

## 9. Artifacts
- [x] **Artifact gallery** — opens from the rail "Artifacts" link; 6 seeded work products (Report/PPTX/Table/Analysis/Alert), type icon + neutral type pill + updated time; panel covers the conversation, rail stays.
- [x] **Artifact viewer** — title + type/updated meta + Copy/Download actions + rendered content (tables / code); back-to-gallery arrow; Esc closes the panel (before exiting full-screen), X closes.
- [x] **Copy works** (writes to clipboard + "Copied" confirm) on viewer and run-result; Download confirms. Runs land in the gallery.
- [ ] Version history.

## 10. Skills (`/`)
- [x] `/` opens a skills palette in the composer (recognition over recall) — keyboard-navigable.
- [x] Seeded 10 skyAgent playbooks.
- [ ] Wire skills to actually run (prefill + send / trigger flows); map to Alerts templates / Agents.

## 11. Model & effort selector
- [ ] skyAgent model options + effort; consistent control across all modes.

## 12. Scheduled / Dispatch / Routines
- [x] **Scheduled** runs list (4 seeded: cadence + On/Off status).
- [x] **Dispatch / Routines** open with proper titled panels + explainer empty states (no dead links).
- [ ] Real scheduling CRUD; wire Dispatch/Routines to actual runs.

## 13. Customize / settings
- [x] **Customize** panel — default mode + default model selects; **applies + persists** (mode switches now, model updates the chip and restores on reload), reflects current state on open.
- [ ] Appearance + autonomy defaults.

## 14. States (design every transition)
- [x] Empty, **loading/skeleton** (Artifacts + Projects galleries shimmer on open), streaming, success states designed. Error-with-retry exists in chat.
- [ ] Skeleton/error states for the remaining lists.

## 15. Real-time behaviors
- [ ] Streaming responses; live task/run status; presence/"working…" indicators.
- [ ] Reconnect / dropped-connection handling (if real WS).
- [x] Prototype simulates streaming + live run status deterministically.

## 16. Design-system fidelity & accessibility
- [x] Reuse existing components (composer, cards, tabs, dialogs) + DS tokens — no ad-hoc colors.
- [x] Color contract (amber=brand only; heatmap amber=brand data-viz; type/priority pills neutral/status), one-pattern-per-role (mode/range=segmented; section tabs=underline).
- [x] **WCAG AA contrast pass** — measured new surfaces; bumped 6 informative texts from `--fg-subtle` (3.79) → `--fg-low` (5.92); mode/skill/rail text all ≥5.
- [x] **focus-visible** added to all new interactive cards; `/` menu keyboard-navigable; reduced-motion honored (caret + spinner).
- [x] **A11y sweep** — measured all interactive targets ≥24px (pointer bar); panels are `role="region"` + `aria-labelledby`; thread is `role="log" aria-live="polite"`; focus moves into a panel on open and returns to the opener on close.

## 17. Responsive / desktop-app feel
- [x] **Rail collapse + keyboard shortcuts** — `⌘/Ctrl+\` collapse rail, `⌘/Ctrl+K` focus search, `F` full-screen, `Esc` close panel/exit; Enter/Shift+Enter send; content reflows.
- [x] Feels native: no page reloads, instant view swaps, persistent state.

---

## Priority pass 1 (proposed P0)
Shell + rail + mode switch · Chat surface with simulated streaming · Composer (input, model/effort,
`/` skills, +attach) · Recents · Empty/streaming/error states · DS + a11y pass.
**P1:** Cowork surface + suggested tasks · Artifacts panel · Projects.
**P2:** Studio + usage stats · Scheduled/Dispatch/Routines · Customize · real WS backend.
