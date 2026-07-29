# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 127 (**SLATE REDESIGN — front of the line by
Ken's directive**: Phase 1 Legs 0-5 shipped on branch `slate-ui`).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — Slate Phase 1, Leg 6 (keyboard registry), on branch `slate-ui`

**The suite redesign ("Slate" v2.0) is the active project (Ken, 2026-07-28 —
supersedes queue order).** Read FIRST:
1. `Design/SLATE_IMPLEMENTATION_PLAN.md` — the ratified plan + Ken's rulings.
2. `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md` — the v2.0 spec (tag `v2.0`).
3. The behavioral contract: `D:\dev\delvio-design\cc-implementation-prompt.md`.

**State:** Branch **`slate-ui`** (pushed): Leg 0 `1c0fc0c` · Leg 1 `c5b213c`
(shell) · Leg 2 `ff0e571` (SlateW2Screen) · Leg 3 `973e0ad` (field states +
ghost + D_W2_ rules) · Leg 4 `b3162c4` (DiagnosticsPanel dock + deep links +
F3) · **Leg 5 SHIPPED `0c05d28`: FormViewPane** — 452px read-only right pane
in the chrome: "Form W-2" tab = HTML facsimile of the W-2 face (archetype
export), data-driven off the ACTIVE employer copy via the new `activeW2`
module channel (focusField-style; pane falls back to the first copy on a
stale id), live on every `fresh_return`; "1040 p.1" tab = page 1 of the
server `render-pdf` drawn via pdfjs (lazy import, debounced refetch on
fresh_return; the 1040 due-diligence print gate's 400 displays as a message
in the pane — never an ambient override). FICA suggestion/computed derivation
extracted VERBATIM to `slate/w2Derived.ts` — ONE implementation shared by the
entry screen and the facsimile. ClientHeader "Form view" now toggles the PANE
(pane default-open like the dock); the full Forms view stays reachable via
the pane's "Full view →" (Leg 4 pattern). `SlateReturnLike` carries
`w2_incomes` + `taxpayer` (structural, additive).
**Live-verified demo DB**: facsimile follows employer-tab switches AND a live
employer-name edit (fresh_return repaint); PDF tab proven both ways — 400
print-gate → message shown, then (QA preparer "QA Test Preparer" created +
assigned to the QA return) 200 → pdfjs parsed p.1 and sized the canvas to
pane width; the final paint was blocked only by hidden-pane rAF starvation
(QA-env artifact — pdf.js renders on rAF, which the hidden Browser pane
starves; same dependency as the proven legacy PdfViewer). Flag OFF = legacy
intact; zero trapped console errors. Gates: vitest **606/606** (8 new) · tsc
**46 = baseline** · server untouched this leg.
⚠ QA-return state change: the demo QA return now HAS a preparer of record —
MISSING_PREPARER / D_PREPARER_001 will no longer fire on it, and the print
gate no longer blocks its renders.

**Next action (Leg 6): keyboard registry** (plan §8.7): one `keyboard.ts`
registry — Ctrl+J JumpBox overlay (screens + fields; preventDefault verified
in Chrome), **F6** = the FormViewPane toggle key (button already wired),
Enter-advance via extended `field-grid-nav`, "Recomputed · time" indicator
off saveScope's last acknowledged mutation. Then Leg 7 (acceptance sweep +
side-by-side screenshots) — **STOP at the Phase 1 gate**.
Dev QA recipe: preview_start django-demo + vite → app root `#/` (NOT
`#/tax-returns`) → Open return → localStorage `delvio-new-ui`=1 → reload.
⚠ Hidden Browser pane: timers throttle ~1s, rAF starves (pdfjs paints stall),
screenshots time out — verify via DOM probes; visible window behaves normally.

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
- **Gates at s127 Leg 5:** vitest **606/606** · tsc **46 = baseline**.
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
