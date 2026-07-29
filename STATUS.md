# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 127 (**SLATE REDESIGN — front of the line by
Ken's directive**: Phase 1 Legs 0-4 shipped on branch `slate-ui`).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — Slate Phase 1, Leg 5 (FormViewPane, F6), on branch `slate-ui`

**The suite redesign ("Slate" v2.0) is the active project (Ken, 2026-07-28 —
supersedes queue order).** Read FIRST:
1. `Design/SLATE_IMPLEMENTATION_PLAN.md` — the ratified plan + Ken's rulings.
2. `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md` — the v2.0 spec (tag `v2.0`).
3. The behavioral contract: `D:\dev\delvio-design\cc-implementation-prompt.md`.

**State:** Branch **`slate-ui`** (pushed): Leg 0 `1c0fc0c` · Leg 1 `c5b213c`
(shell) · Leg 2 `ff0e571` (SlateW2Screen) · Leg 3 `973e0ad` (field states +
ghost + D_W2_ rules) · **Leg 4 SHIPPED `b3162c4`: DiagnosticsPanel docked** —
≤190px dock in the content column (ACTIVE findings only, ranked
error>review>info, per-severity counts, Full-view link, Hide; the ClientHeader
Diagnostics button toggles the DOCK now), field deep links (`focusField.ts`
event channel: row click / F3 routes rule_code→tab via RULE_TAB_MAP then
`details.field`+`details.record` → SlateW2Screen switches the employer copy
and focuses+flashes the cell; unclaimed requests land on `[data-field]`
directly), F3/Shift+F3 cycling. TWO fixes riding along: the Run-Diagnostics
POST now carries `timeoutMs` 180s (the pre-existing silent client abort on
slow demo sweeps — chip `task_3ddb0a08` CLOSED; a fresh full sweep completed
live w/ 8 findings incl. both D_W2_ rules) · rail section clicks return to the
input view (Leg 1 latent gap — previously stuck on Forms/Diagnostics view).
**Live-verified demo DB**: dock renders the real run ranked, BOX4_VAR row
click lands focus+flash on the overridden Box 4, F3 cycles across tabs,
hide/show toggle, flag OFF = legacy intact. Gates: vitest **598/598** (9 new)
· tsc **46 = baseline** · server untouched this leg.
⚠ QA note: the hidden Browser pane throttles timers to ~1s — deep-link
probes race; the 80ms delays behave normally in a visible window.

**Next action (Leg 5): FormViewPane (F6)** (plan §8.6): 452px read-only right
pane — W-2 HTML facsimile (markup from `delvio-design/screens/w2-entry.html`
lines 375-449, data-driven off the active W-2) + a "1040 p.1" tab hosting the
existing server PDF render via pdfjs. Live update on `fresh_return`. The
ClientHeader "Form view" button (currently switches to the full Forms primary
tab) becomes the pane toggle — keep the full Forms view reachable (same
pattern as the Leg 4 dock/Full-view split). F6 key = Leg 6 (registry) but the
button toggle lands now. Then Leg 6 (keyboard registry: Ctrl+J JumpBox,
Enter-advance via field-grid-nav, Recomputed· indicator) and Leg 7
(acceptance sweep + side-by-side screenshots) — **STOP at the Phase 1 gate**.
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
- **Gates at s127 Leg 4:** vitest **598/598** · tsc **46 = baseline**.
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
