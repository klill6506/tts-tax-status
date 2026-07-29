# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 127 (**SLATE REDESIGN — front of the line by
Ken's directive**: Phase 1 Legs 0-6 shipped on branch `slate-ui`).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — Slate Phase 1, Leg 7 (acceptance sweep + side-by-sides), on branch `slate-ui`

**The suite redesign ("Slate" v2.0) is the active project (Ken, 2026-07-28 —
supersedes queue order).** Read FIRST:
1. `Design/SLATE_IMPLEMENTATION_PLAN.md` — the ratified plan + Ken's rulings.
2. `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md` — the v2.0 spec (tag `v2.0`).
3. The behavioral contract: `D:\dev\delvio-design\cc-implementation-prompt.md`.

**State:** Branch **`slate-ui`** (pushed): Leg 0 `1c0fc0c` · Leg 1 `c5b213c`
(shell) · Leg 2 `ff0e571` (SlateW2Screen) · Leg 3 `973e0ad` (field states +
ghost + D_W2_ rules) · Leg 4 `b3162c4` (DiagnosticsPanel dock) · Leg 5
`0c05d28` (FormViewPane) · **Leg 6 SHIPPED `bb5929b`: the keyboard layer** —
`slate/keyboard.ts` registry (global **Ctrl/Cmd+J** JumpBox toggle + **F6**
form-pane toggle, both preventDefault; `advanceFromInput` extends
`lib/field-grid-nav`'s pure scan to the Slate DOM: next blank editable
`.slate-field`, skipping filled cells and locked ƒx cells, wrapping once),
**JumpBox** command palette (all rail sections kinded by nav group + the
current screen's `[data-field]` inputs, collected from the DOM at open;
↑↓/⏎/Esc; field select = `focusFieldInDom` focus+flash; the AppHeader
trigger is enabled), **FieldStateInput Enter = accept-and-advance** on ghost
cells / plain advance elsewhere (⚠ the advance blurs the cell mid-accept —
the DOM value is synced BEFORE the focus move, else the blur commit reads
the unflushed empty value and WIPES the acceptance; test-pinned), and the
ClientHeader **"Recomputed · ⟨ago⟩" stamp** off `saveScope.lastAckedAt`
(every save path recomputes before responding, so ack time = recompute
time). **Live-verified demo DB**: Ctrl+J → filter → Enter jumped to
Schedule A; field jump landed focus+flash on Box 2; F6 hid/showed the pane;
Enter-advance from Box 2 skipped filled Box 3 + overridden Box 4 and landed
on ghost Box 5; ghost Enter accepted 50,000, advanced to Box 7, the PATCH
survived the blur, and the Recomputed stamp appeared (Box 5 reverted to
ghost afterward — QA return left as found). Flag OFF = legacy intact AND
Ctrl+J NOT intercepted (the registry never mounts). Gates: vitest
**617/617** (11 new) · tsc **46 = baseline** · server untouched.

**Next action (Leg 7): acceptance sweep + side-by-side screenshots** (plan
§8.8): legacy vs Slate on the dev server, SAME return, screenshots posted →
**STOP at the Phase 1 gate for Ken's review.** Sweep the plan's testing list
(§8 end): flag-off assertion suite, two-cell suggest guard, deep-link
routing, JumpBox filtering, Enter-advance ordering — all have tests; the
sweep re-verifies against the ACCEPTANCE list + captures the screenshots.
⚠ **Screenshots need a VISIBLE Browser pane** — the hidden pane can't
composite frames (screenshot times out) and starves rAF (pdfjs paints
stall). Run Leg 7 when the pane is displayed, or ask Ken to pop it open.
Dev QA recipe: preview_start django-demo + vite → app root `#/` (NOT
`#/tax-returns`) → Open return → localStorage `delvio-new-ui`=1 → reload.

**Build rules in force:** presentation-only (the Leg 3 server exceptions are
done; anything further needs Ken) · every input a real DOM input · **selective
`git add` only — NEVER `git add .` on this branch** (parallel tb_import work
unstaged in the tree) · no merge/deploy without Ken.

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE)
1. RS D_8990_DISALLOW vs D_8990_EBIE conflicting guidance on a 1065 (s126e).
2. Retire MATH_BALANCE_SHEET's 1065 arm? (s126d).
3. RS R-M2-3-TIE adjudication (s126b).
4. K-1 box 13/11 type codes (s125).
5. RS 1065_B stale D_B2_B1 note + 5 unbuilt Sch B diagnostics (s126).
6. s124's `D_4562_RECON` scoping pair.
7. Real One Heart EIN in committed test fixtures (chip `task_f06ee3ed`).
8. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
9. *(minor)* Legacy "autofilled" yellow (EIN/ZIP pulls) has no Slate state —
   ruling wanted on its Slate treatment.
10. *(observed, pre-existing)* The QA skeleton return's diagnostics run shows
    4 rule-execution errors (D_8995_003 + D_8959_001/3/4 crash w/ NoneType on
    missing taxpayer facts) — noisy on skeleton returns; worth a defensive
    guard pass in those rule families.

## Active gates
- **Branch discipline:** repo checked out on `slate-ui`; parallel session's
  uncommitted work UNSTAGED in the tree (`server/apps/returns/views.py`,
  `tb_import.py`, `test_tb_import.py`). Never stage/stash/`git add .`.
- **Gates at s127 Leg 6:** vitest **617/617** · tsc **46 = baseline**.
- ⚠ Demo QA return state: has a preparer of record ("QA Test Preparer",
  synthetic PTIN) since Leg 5 QA — its print gate is clear and
  MISSING_PREPARER/D_PREPARER_001 no longer fire on it.
- ⚠ At deploy (Ken gates): **seed_rules BOTH DBs** (D_W2_ family, Leg 3).
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
