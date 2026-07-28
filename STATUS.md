# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 127 (**SLATE REDESIGN — front of the line by
Ken's directive**: Phase 1 Legs 0-3 shipped on branch `slate-ui`).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — Slate Phase 1, Leg 4 (DiagnosticsPanel docked), on branch `slate-ui`

**The suite redesign ("Slate" v2.0) is the active project (Ken, 2026-07-28 —
supersedes queue order).** Read FIRST:
1. `Design/SLATE_IMPLEMENTATION_PLAN.md` — the ratified plan + Ken's rulings.
2. `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md` — the v2.0 spec (tag `v2.0`).
3. The behavioral contract: `D:\dev\delvio-design\cc-implementation-prompt.md`.

**State:** Branch **`slate-ui`** (pushed): Leg 0 `1c0fc0c` · Leg 1 `c5b213c`
(editor shell) · Leg 2 `ff0e571` (SlateW2Screen) · **Leg 3 SHIPPED `973e0ad`:
field states + ghost.** Server (the sole Ken-approved exceptions): read-only
`expected_box3/5` on the W-2 serializer (w2_fica_check is the source) + NEW
`rules_w2_fica.py` — D_W2_BOX3_VAR/BOX5_VAR/BOX4_VAR (warning, $1 tol; Box 4
honours an entered Box 3, applies to overrides per Q3) + BOX3/BOX5_SUGGEST
(info); every finding carries `details.field` + `details.record` (the Leg 4
deep-link channel). ⚠ **seed_rules must run on BOTH DBs at deploy** (demo
already seeded during QA). Client: `fieldState.ts` (derivation + SUGGEST_CELLS
guard: ghost = Boxes 3&5 ONLY, test-pinned), FieldStateInput five treatments
(ghost Enter-accept · computed ƒx read-only w/ Ctrl+Enter override, scripted
change ignored while locked · overridden violet + flag + ↺ revert · estimated
unwired), commit-equal-reverts RETIRED for 3/5, basis-aware 4/6 display math,
legend footer, `D_W2_` → w2_income in RULE_TAB_MAP. **Live-verified demo DB**:
server-fed ghosts, Enter-accept persisted, Ctrl+Enter override → violet/flag/
revert, live rule probe fired BOX4_VAR (on the override) + BOX5_SUGGEST w/
field refs; flag OFF = legacy intact. Gates: server 14 new + W-2 band 47 +
diagnostics band 41 · vitest **589/589** (16 new) · tsc **46 = baseline**.
QA data: the Slate QA Household W-2 now holds Box 1 = 50,000, Box 3 accepted
50,000 (entered), Box 4 overridden 3,000 (BOX4_VAR fires — useful Leg 4 data),
Box 5 still ghost.

**Next action (Leg 4): DiagnosticsPanel docked** (plan §8.5): ≤190px dock,
severity-ranked coded rows, click-to-focus. Field-level focus = `data-field`
names on Slate inputs (already emitted: ein/employer/box1..box20) + findings'
`details.field`/`details.record` (already emitted by the D_W2_ family) →
switch to the record's tab copy, focus + `--duration-flash` flash. rule_code →
tab via the existing RULE_TAB_MAP. F3/Shift+F3 cycling. ⚠ Blocker-adjacent:
the Run Diagnostics POST aborts client-side on slow demo sweeps (pre-existing
— chip `task_3ddb0a08`; consider fixing en route since Leg 4 depends on runs).
Then Legs 5-7 per plan §8; **STOP at side-by-side screenshots (Phase 1 gate)**.
Dev QA recipe: preview_start django-demo + vite → app root `#/` (NOT
`#/tax-returns`) → Open return → localStorage `delvio-new-ui`=1 → reload.

**Build rules in force:** presentation-only (server exceptions above are done;
anything further needs Ken) · every input a real DOM input · **selective
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
9. *(new, minor)* Legacy "autofilled" yellow (EIN/ZIP pulls) has no Slate
   state — dropped visually under the flag; ruling wanted on its Slate
   treatment (plain entered vs a distinct pulled cue).

## Active gates
- **Branch discipline:** repo is checked out on `slate-ui`. A parallel
  session's uncommitted work rides in the tree UNSTAGED:
  `server/apps/returns/views.py` (M), `server/apps/returns/tb_import.py`,
  `server/tests/test_tb_import.py`. Do not stage, do not stash, do not
  `git add .`.
- **Gates at s127 Leg 3:** vitest **589/589** · tsc **46 = baseline** ·
  server: 14 new diagnostics + W-2 band 47 + diagnostics band 41.
- ⚠ At deploy (Ken gates): **seed_rules BOTH DBs** (D_W2_ family).
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
