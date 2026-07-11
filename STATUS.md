# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-11, session 53 ("go" — autonomous per the s48/s52 directives).
Shipped THREE units: **D2 · 1120-S client formula-mirror retirement** (`7913796`,
−2,545 lines — FORMULAS_1120S + computeFields + the parity gate DELETED; every form
now paints computed cells from the autosave's `?fresh_return=1` payload; live-probed
both directions) · **1065 owner-diagnostics fix** (`2e33d5f` — missing_shareholders +
SSN checks read Partner on a 1065; the s50 false "No partners entered" ERROR dead;
41 tests +5; live run_diagnostics probe) · **straggler triage openers** (`2cda054`
apr01 fixture rot → 11 pass · `cc849c3` TestRenderK1 R-K1-ROUND fixture fix → 4 pass).
B2-17 promoted to the Spine as S-20a–d → **BATCH 2 FULLY DISPOSITIONED**.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE
**Ken directives standing (s48 + s52 addendum): work AUTONOMOUSLY down this list;
full gates + live probes; Ken-decisions → REVIEW_QUEUE with a recommendation, then
move on; mandatory session close before context exhausts.**
1. **Full-suite straggler triage, continued (item 4).** The s53 run COMPLETED:
   **5051 passed · 168 failed · 167 errors · 21 skipped in 46:31** (local PG,
   post-s53-fixes tree except apr01/TestRenderK1 which were fixed mid-run and
   verified standalone). **Inventory (by file):**
   - **⚠ `test_sch123_render.py` = 164 of the 167 ERRORS** — one setup/fixture
     root cause for the whole file (the apr01 shape); fix FIRST, it's ~half the
     red count in one stroke. (The other 3 errors = apr01's stale in-memory
     module during this run — already fixed `2cda054`, 11 pass standalone.)
   - **168 F's across ~70 files, top:** schedule_j_compute 12 · 1040 10 ·
     8835_pipeline 8 · topic5_compute 8 · 8936_pipeline 7 · sch123_scenarios 6 ·
     mef_scenario12_compute 5 · 3800_pipeline 5 · ga500_compute 4 ·
     client_import 4 · schedule_k1_diagnostics 4 · the rest ≤3 each (full list:
     re-aggregate from a rerun, or STATUS_ARCHIVE s53). Expected dominant class
     = s27 cents pins (~227 `X.00` pin occurrences repo-wide); fix ONLY failing
     files, per-file write-path analysis (some pins are inputs/KEEP-list and
     correct). Client-import/clients/entity-link failures may be a separate
     fixture class — diagnose before touching.
   ⚠ TaskStop on a background pytest leaves the python child alive — kill the
   PID before starting a fresh run or it dies fighting over test_postgres.
   Scratch-DB recipe for parallel verification: `$env:TEST_DB_TEST_NAME="x"` +
   `--create-db` (then `--reuse-db`).
2. **RS renumber unit #2: SCH_K_1120S** (fresh session; fabricated 13f · missing
   17c AE&P · wrong L18 formula) → K1 → SCHL → 6198 → M3 line_map → 3800.
   Ledger: `docs/rs_handoff/2026-07-09_early_era_face_audit.md`.
3. **RS FA-export reconciliation pass** (queued since s32).
4. **S-20 B2-17 form units** (promoted s53): 8283-entity → 2553 → 2848 → 3115
   app build (BUILD_ORDER S-20a–d; after the renumber queue unless Ken reorders).
5. **Ken ratifications pending (REVIEW_QUEUE):** R007 AMT-matrix · 40% transitional
   election · +s49 candidates (stale is_overridden-on-blank · entity is_4797 rows) ·
   +s53 NEW: 1065 partner-percentage diagnostic (recommend a WARNING when
   profit/loss pct ≠ 100 over active partners).

## ▶ Waiting on Ken / external
1. R007 AMT-correction ratification + 40%-election ruling (s47).
2. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
3. IdenTrust cert (⚠ 30-day download clock). 4. File-1018 Lacerte reprint (item 10).
5. PWA install check. 6. TaxWise forms-usage report.
7. Density feel-check (s52 trim + depreciation squeeze) — D1/D2 both RESOLVED/SHIPPED.

## Active gates
- **Flow-assertion gate 447 unchanged** (s53 touched NO compute/render/allocator
  code — D2 is client-only deletion; the diagnostics fix is rule-function-only).
- **Client: tsc 0 · vitest 300** (305 − the 5 retired mirror/parity tests — the
  parity gate is RETIRED per D2, it policed a mirror that no longer exists).
- Server targeted: test_diagnostics 41 · test_apr01_fixes 11 · TestRenderK1 4.
- **Live probes (all isolated firms, cascade-deleted):** D2 — typed 1a/1b/A2/A7 →
  downstream blank until flush, then server-exact paint (1c 45,000 / A8 6,000 /
  21 39,000; salaries 9,000 → 20 9,000 / 21 30,000, DB ties); diagnostics — 1065
  run_diagnostics fires with no partners, silent with one.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue: SCH_K/K1/SCHL/6198/3800 still inherit fabrications.
- ⚠ Browser-pane screenshots/coordinate clicks unavailable this machine — DOM
  assertions + JS focus/native-setter events; DOM re-reads can lag the autosave
  round trip — re-query before declaring a paint failure.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
