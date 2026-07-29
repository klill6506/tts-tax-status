# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 127 (**Phase 2 item 2 SHIPPED: Slate
Diagnostics workspace** on branch `slate-ui`).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — Slate Phase 2, item 3: entry-paradigm convergence (plan §9.3)

Ken ratified the Slate archetype 2026-07-28; Phase 2 proceeds in plan §9
order, each screen screenshot-reviewed. Flag stays prod-OFF; no
merge/deploy without Ken.

**State:** Branch **`slate-ui`** (pushed `e7a0f30`): Phase 1 complete ·
item 1 Return Manager (`0672a6c`) · **item 2 SHIPPED `e7a0f30`: Slate
Diagnostics workspace** — `slate/screens/SlateDiagnosticsWorkspace.tsx`
is a VIEW over the legacy DiagnosticsTab container (runs/auto-run/ack
machinery untouched — one state machine, two renderings): rail filters
(severity · by-screen via RULE_TAB_MAP · Active/Acknowledged · persisted
internal toggle), ranked table (search, F3/Shift+F3 selection, Enter
jump, dbl-click jump), detail pane (severity pill · meta table · READ-ONLY
field-in-context worksheet off the SAME w2Derived as the entry screen ·
ack-with-note + unack · run history from real runs). Data-honest
adaptations (mock Category taxonomy / Authority row / editable ctx
inputs have no data source) → REVIEW_QUEUE s127i w/ recommendation.
**Two live-found fixes rode along (shared machinery, both renderings):**
(1) latest-run selection prefers the newest COMPLETED run — overlapping
mount auto-runs left an in-progress `runs[0]` that read as "no issues"
(dock, status dots, both diagnostics views affected); (2) **diagnostic
deep-link focus was a one-shot 80 ms race** — a jump from the diagnostics
view mounts the target screen through a LAZY chunk, so the single
dispatch went unclaimed and focus never landed (reproduced in headless
Chrome — NOT just the hidden-pane env); `requestFieldFocus` now retries
to a 3 s deadline and SlateW2Screen's claim path uses the shared
`focusFieldWhenReady`.
**Live-verified demo DB:** filters/search/F3/selection, ack → note
round-trip → unack re-alarm (server-proven via API), Go-to-field landing
proven with TRUSTED input in headless Chrome (`focused: box4`), flag OFF
= legacy diagnostics intact w/ zero slate classes + zero trapped console
errors, ≥400s attributed (login `/me/` 403 + `/prior-year/` 404,
same-count-same-endpoint across modes). Review shots delivered to Ken +
committed: `Design/slate-phase2-screenshots/` (legacy-diagnostics ·
slate-diagnostics · slate-diag-filtered · slate-goto-field; demo DB
only). New capture recipe: `scripts/slate_diag_screenshots.mjs`.
Gates: vitest **638/638** (11 new) · tsc **46 = baseline**.

**Next action (Phase 2 item 3): entry-paradigm convergence** (plan §9.3)
— one screen at a time onto the InputRow grid: 1099-R stack, INT/DIV
payer grids, Social Security page, Schedule 1/2/3 tables, state line
tables; anything that can't fit the grid gets listed and asked, not
forced. Then §9.4 launcher/login (separate delvio-launcher item). Dev QA
recipe: preview_start django-demo + vite → `#/` → localStorage
`delvio-new-ui`=1 → reload. Screenshots: `scripts/slate_screenshots.mjs`
/ `slate_diag_screenshots.mjs` (+ `scripts/mint_magic_link.py`).

**Build rules in force:** presentation-only (server exceptions need Ken —
RM aggregates + rule kind/authority columns are QUEUED, not built) ·
selective `git add` only — NEVER `git add .` (parallel tb_import work
unstaged) · no merge/deploy without Ken · at deploy: seed_rules BOTH DBs
(D_W2_ family).

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE)
1. **NEW (s127i): Diagnostics workspace adaptations** — mock's Category
   taxonomy / Authority row / editable field-in-context need small server
   columns if wanted; read-only ctx recommended as permanent.
2. RM refund/due column + aggregate season totals (s127h) — server aggregate.
3. RS D_8990_DISALLOW vs D_8990_EBIE conflicting guidance on a 1065 (s126e).
4. Retire MATH_BALANCE_SHEET's 1065 arm? (s126d).
5. RS R-M2-3-TIE adjudication (s126b).
6. K-1 box 13/11 type codes (s125).
7. RS 1065_B stale D_B2_B1 note + 5 unbuilt Sch B diagnostics (s126).
8. s124's `D_4562_RECON` scoping pair.
9. Real One Heart EIN in committed test fixtures (chip `task_f06ee3ed`).
10. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
11. *(minor)* Legacy "autofilled" yellow → Slate treatment ruling.
12. *(pre-existing)* D_8995/D_8959 NoneType crashes on skeleton returns —
    visible as 4 errors in the new workspace shots; unchanged behavior.
13. *(cosmetic, Phase 2)* Legacy floating "Calculating…" chip needs a Slate
    home (overlaps the workspace's bottom-right in the review shots).

## Active gates
- **Branch discipline:** `slate-ui` checked out; parallel session's
  uncommitted work UNSTAGED (`server/apps/returns/views.py`, `tb_import.py`,
  `test_tb_import.py`). Never stage/stash/`git add .`.
- **Gates at s127 item 2:** vitest **638/638** · tsc **46 = baseline**.
- ⚠ Demo QA return: has preparer "QA Test Preparer" (synthetic PTIN) since
  Leg 5 QA — print gate clear, D_PREPARER_001 silent on it. A QA
  ack/unack cycle on D_W2_BOX5_SUGGEST was fully reverted.
- ⚠ FA-1040-4835-06 drift (chip `task_0cf10eac`).
- ⚠ One test DB — never overlap pytest runs.
- ⚠ `server/.venv` repaired s124; use `.venv\Scripts\python.exe` directly.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged — Slate Phase 2 runs in front by Ken's ratification; the form
queue interleaves on Ken's direction.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
- **Slate lane:** `Design/SLATE_IMPLEMENTATION_PLAN.md` ·
  `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md`.
