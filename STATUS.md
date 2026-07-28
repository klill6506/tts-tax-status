# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 127 (**SLATE REDESIGN — front of the line by
Ken's directive**: Slate v2.0 landed in delvio-design, plan ratified, Phase 1
Leg 0 shipped on branch `slate-ui`).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — Slate Phase 1, Leg 1 (the editor shell), on branch `slate-ui`

**The suite redesign ("Slate" v2.0) is the active project (Ken, 2026-07-28 —
supersedes queue order).** Read FIRST:
1. `Design/SLATE_IMPLEMENTATION_PLAN.md` — the ratified plan + Ken's rulings
   (all 12 questions RESOLVED; Boxes 4 & 6 stay computed w/ Ctrl+Enter override).
2. `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md` — the v2.0 spec (tag `v2.0`,
   pushed). Contrast gate: `python contrast-check.py` (22 must-pass PASS).
3. The behavioral contract: `D:\dev\delvio-design\cc-implementation-prompt.md`.

**State:** delvio-design v2.0 SHIPPED (`4e97342`, tagged, pushed). Root
`D:\dev\CLAUDE.md` Color System → Field-State System (red/yellow/green RETIRED,
Ken 2026-07-28). delvio-tax branch **`slate-ui`** cut from main; **Leg 0
shipped** (`1c0fc0c`, pushed to origin/slate-ui — branch push, NO deploy):
`lib/featureFlags.ts` (NEW_UI: VITE_NEW_UI env, default OFF; dev localStorage
`delvio-new-ui` override) · vendored `slate/slate-tokens.css` · `SlateRoot.tsx`
+ `slate.css` · `vite-env.d.ts` shim (tsc 52 → **46 pre-existing**, new files
clean) · README design section → Slate. Flag tests 4/4 green.

**Next action (Leg 1):** Slate editor shell inside `FormEditor` behind the flag
— `AppHeader` (44px accent bar) · `ClientHeader` (38px toolbar) · `LeftRail`
(272px; sections from `INDIVIDUAL_TABS` + `tabStatus()` → Slate 10px dots) ·
`SummaryBar` (46px; reuse RefundMonitor sources: 1040 lines 11/34/37 +
GA-500 fetch) — legacy tabs render inside the shell until converged. Then Legs
2-7 per plan §8; STOP at side-by-side screenshots for Ken (Phase 1 gate).
Component geometry: plan §5 layout + the survey report values; W-2 archetype
export is the visual truth (`delvio-design/screens/w2-entry.html.html`).

**Build rules in force:** presentation-only (sole exceptions Ken-approved:
read-only `expected_box3/5` serializer fields + additive `details.field` on
W-2-family diagnostics + new Box 3/5 variance REVIEW rules) · every input a
real DOM input · **selective `git add` only — NEVER `git add .` on this
branch** (a parallel session's uncommitted tb_import work sits in the tree) ·
no merge/deploy without Ken.

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE)
1. RS D_8990_DISALLOW vs D_8990_EBIE conflicting guidance on a 1065 (s126e).
2. Retire MATH_BALANCE_SHEET's 1065 arm? (s126d).
3. RS R-M2-3-TIE adjudication (s126b).
4. K-1 box 13/11 type codes (s125).
5. RS 1065_B stale D_B2_B1 note + 5 unbuilt Sch B diagnostics (s126).
6. s124's `D_4562_RECON` scoping pair.
7. Real One Heart EIN in committed test fixtures (chip `task_f06ee3ed`).
8. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).

## Active gates
- **Branch discipline:** repo is checked out on `slate-ui`. A parallel
  session's uncommitted work rides in the tree UNSTAGED:
  `server/apps/returns/views.py` (M), `server/apps/returns/tb_import.py`,
  `server/tests/test_tb_import.py`. Do not stage, do not stash, do not
  `git add .`. If that session resumes, it may switch back to main — Slate
  work always recommits on `slate-ui`.
- **Gates at s127:** client vitest 557 + 4 new flag tests · tsc **46 =
  new baseline** (was 52; vite-env.d.ts shim fixed 6 pre-existing) · server
  suites untouched this session.
- ⚠ FA-1040-4835-06 drift (chip `task_0cf10eac`).
- ⚠ One test DB — never overlap pytest runs.
- ⚠ `server/.venv` repaired s124; use `.venv\Scripts\python.exe` directly.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged — Slate redesign runs in front of it by Ken's 2026-07-28 directive.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
- **Slate lane:** `Design/SLATE_IMPLEMENTATION_PLAN.md` ·
  `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md`.
