# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 127 (**SLATE REDESIGN — front of the line by
Ken's directive**: Phase 1 Legs 0-2 shipped on branch `slate-ui`).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — Slate Phase 1, Leg 3 (field states + ghost), on branch `slate-ui`

**The suite redesign ("Slate" v2.0) is the active project (Ken, 2026-07-28 —
supersedes queue order).** Read FIRST:
1. `Design/SLATE_IMPLEMENTATION_PLAN.md` — the ratified plan + Ken's rulings
   (all 12 questions RESOLVED; Boxes 4 & 6 stay computed w/ Ctrl+Enter override).
2. `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md` — the v2.0 spec (tag `v2.0`).
3. The behavioral contract: `D:\dev\delvio-design\cc-implementation-prompt.md`.

**State:** Branch **`slate-ui`** (pushed): Leg 0 `1c0fc0c` (flag/tokens/SlateRoot) ·
Leg 1 `c5b213c` (editor shell live behind flag) · **Leg 2 SHIPPED `ff0e571`:
SlateW2Screen** — DocumentTabs employer tabs (W2Screen:123 in-flight-add safety
verbatim: no worksheet while adding), InputRow grid (box badges, 32px rows,
160px value col, two-column layout), `FieldStateInput` = five-state primitive
base carrying FieldGrid's s106 semi-controlled mechanics VERBATIM, nested
Box 12/14 + state/locality on the SAME lanes (`useNestedRowSaves`/`rowErrorText`
now EXPORTED from W2Screen — one implementation, two renderings), FICA 3-6
keep legacy computed-default semantics this leg. FormEditor splice = a NEW_UI
ternary on the w2_income body; SlateW2Screen is lazy (nothing loads flag-off).
**Live-verified demo DB**: add W-2 → 201, Box 1 → PATCH → recompute → FICA
defaults (6.2%/1.45%) + refund-monitor AGI update, Box 12 nested add, flag
OFF = legacy pixel-intact (zero slate classes/CSS), zero console errors.
Note: the Slate QA Household return now holds one W-2 (Box 1 = 50,000, one
Box 12 D row) — useful test data for Leg 3.

**Next action (Leg 3): field states + ghost** (plan §6/§7, §12 rulings):
- Server (the sole approved exceptions): read-only `expected_box3`/`expected_box5`
  serializer fields on the W-2 endpoint · Box 3/5 variance REVIEW rules
  (W-2 family) · additive `details.field` on W-2-family diagnostics.
- Client: `fieldState.ts` derivation + FieldStateInput gains the five
  treatments — ghost suggestion on Boxes **3 & 5 ONLY** (Enter accepts through
  queueW2Patch; retire commit-equal-reverts for 3/5), Boxes **4 & 6 computed
  ƒx chip** w/ Ctrl+Enter override → violet OVERRIDDEN (Ken's Q3 override),
  EST component unwired (Q6). Two-cell suggest guard TEST required (§7.5).
- Open Leg 3 decision: legacy autofilled-yellow (EIN/ZIP pulls) has no Slate
  state mapping — dropped visually in Leg 2, decide its Slate treatment.
Then Legs 4-7 per plan §8; **STOP at side-by-side screenshots (Phase 1 gate)**.
Dev QA recipe: preview_start django-demo + vite → app root `#/` → Open the
return (list route is `#/`, NOT `#/tax-returns`) → localStorage
`delvio-new-ui`=1 → reload (flag reads at module load).

**Build rules in force:** presentation-only (sole exceptions above, Ken-approved) ·
every input a real DOM input · **selective `git add` only — NEVER `git add .`
on this branch** (a parallel session's uncommitted tb_import work sits in the
tree) · no merge/deploy without Ken.

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
- **Gates at s127 Leg 2:** client vitest **573/573** (12 new SlateW2Screen
  contract tests) · tsc **46 = baseline** · server suites untouched this leg.
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
