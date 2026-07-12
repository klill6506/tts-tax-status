# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-11, session 57 ("go" — autonomous). **K1_1120S renumber
unit #3 COMPLETE (RS spec-first + tts K-1 code-letter fixes).** RS: K1_1120S rebuilt
verbatim vs the 2025 f1120ssk face + the i1120s 2025 code tables (boxes 1-19, Part
I/II items, code-assignment rules R012-R016/R021, 7 verbatim excerpts, stale-row
self-heal guards), seeded + export-verified + tts mirror refreshed. tts: **the s37
"K-1 codes mirror the tables" belief was FALSE — the box 13 print/MeF codes were the
PRE-2023 alphabet** (LIH printed "A", which means zero-emission nuclear on the 2025
table → 13a-13f now C/D/E/F/G/I), K12c §59(e)(2) I→J, K12d other L→ZZ (+ typed
statement), health insurance AC→ZZ (+ statement; i1120s p.17: W-2 box 14 is the
channel; AC = §448(c) gross receipts) with key K17_AC→K17_HEALTH, and K13e (other
rental credits) now allocates (issuer gap). 8941 = BA was already correct. 5 new
code-letter pins. `/bugs` sweep at boot: no open reports.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE
**Ken directives standing (s48 + s52 addendum): work AUTONOMOUSLY down this list;
full gates + live probes; Ken-decisions → REVIEW_QUEUE with a recommendation, then
move on; mandatory session close before context exhausts.**
1. **Start every session with `/bugs`** (s55) — reports are CANDIDATES; Ken decides;
   computation-touching fixes re-run regression before merge.
2. **RS renumber unit #4: 1120S_SCHL** (next in the audit-ledger queue) — the s44
   audit found it internally contradictory (fabricated "L22 Total liabilities";
   facts on a second fabricated numbering; R001 sum omits four asset lines; face is
   assets 1-15 / liabilities 16-21 / equity 22-26 / total 27). Keep R005-R008
   substance (M-2 tie, BOY carry, $250K exception, L3a default). tts Sch L print
   verify rides the same unit.
3. **Then: PAGE1+M1+M2 blocks** (s56 queue insert — pre-Form-7205 numbering +
   fabricated M-1 excerpt; the tts side = page-1/K FFV re-key + data migration +
   the seven s56 deferrals) → 6198 → M3 line_map → 3800.
4. **RS FA-export reconciliation pass** (queued since s32).
5. **S-20 B2-17 form units**: 8283-entity → 2553 → 2848 → 3115 app build.
6. **Ken ratifications pending (REVIEW_QUEUE):** R007 AMT-matrix · 40% transitional
   election · s49 candidates · s53 partner-percentage diagnostic · **NEW s57: K-1
   health-insurance ZZ-statement presentation** (shipped + pinned; recommendation
   attached).

## ▶ Waiting on Ken / external
1. If `WORK_ORDER_bug_reporting.md` exists somewhere with more than the s55 chat
   summary, point CC at it (reconciliation flag stands).
2. R007 AMT-correction ratification + 40%-election ruling (s47).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN s57** (447).
- **Affected suites GREEN s57**: face-renumber pins 49 (incl. the NEW
  `TestK1CodeLetters2025` 5) · test_mef_1120s + test_tts_forms 234 · S5+S6 8/8.
  Full-suite rerun not run this session (code-letter changes are pin-covered; last
  full-suite GREEN = s54 `cd9b186`).
- ⚠ `test_k1_import_stage3.py` has 3 standalone-run fixture errors (FormDefinition
  1120-S not seeded when the file runs ALONE on a fresh test DB) — **pre-existing**
  (verified on a stashed clean tree s57); green in-suite. The
  `test-db-formdefinition-preseeded` memory class.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue remaining: **SCHL → PAGE1+M1+M2 → 6198 → M3 line_map → 3800.**
- ⚠ Prod seeders run this session: RS `load_1120s_specs` (K1 rebuild; SQLite
  validate + Supabase, both idempotent-rerun verified, deployed export verified).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
