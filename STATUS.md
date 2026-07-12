# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-12, session 60 ("go" — autonomous). **Renumber unit #5
COMPLETE — the tts FFV semantic re-key SHIPPED (`e4c4ac8`, mig 0187).** The
1120-S internal page-1 keys now EQUAL the 2025 face: 19 = Form 7205 (NEW input),
20 other, 21 total, 22 OBI (K1 reads it), 23a-c tax, 24a-d/z payments (NEW 24d
EPE input), 25 penalty, 26 owed / 27 overpaid, 28a credited + NEW computed 28b
refunded. Mig 0187 renamed FormLine keys IN PLACE on the shared DB — 278 FFVs
per key carried, verified pre/post; py_manual `L:` + baseline `1120-S:` JSON
keys re-keyed; imported PY line_values needed NO migration (the 2024 print
already used this numbering — the re-key FIXED the live CY-vs-PY compare
misalignment). **LIVE FIX**: owed/overpayment now include the line-25 penalty
term (RS R014 — the old formulas omitted it). The s56 print re-route shim is
RETIRED (map = face 1:1; new widgets f1_37/f1_47/f1_53 y/x-verified); MeF
PAGE1_LINE_ORDER re-keyed + 7205/EPE elements added (XSD-verified); GA-600S
S6_1 pull, 8879-S/8453-S line 3, diagnostics, client footer/hidden/PY-compare
lists all re-keyed. s56-tail items 1-3 (7205 input · 24d · 28b) DELIVERED;
items 4-7 (charitable split · K16e/16f+K18 term · K14a/b · 17c→M-2(c)) stay
deferred as the SCH_K input sub-unit. Pre-existing K3c duplicate-widget alias
removed (s56 validator trip). `/bugs` at boot: no open reports.*

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
1. **Start every session with `/bugs`** (s55).
2. **RS renumber unit #6: 6198** (at-risk) — then **M3 line_map** (needs the face
   template download — f1120s M-3 is a separate PDF, add to forms_manifest) →
   **3800** (rides the 3800/GBC entity unit).
3. **SCH_K input sub-unit** (the s56 tail 4-7, DEFERRAL_AUDIT s60): 12a/12b
   charitable split · K16e/16f inputs + K18 16f term · K14a/b K-2/K-3 row
   conversion · 17c → M-2 col (c)/1099-DIV wiring. Can ride the 6198/M3 trips
   or run standalone.
4. **RS FA-export reconciliation pass** (queued since s32).
5. **S-20 B2-17 form units**: 8283-entity → 2553 → 2848 → 3115 app build.
6. **Ken ratifications pending (REVIEW_QUEUE):** s59 M-2 NNA distribution cap
   (tts M-2 compute implements it once ratified) · R007 AMT-matrix · 40%
   transitional election · s49 candidates · s53 partner-percentage diagnostic ·
   s57 K-1 health-insurance ZZ presentation.

## ▶ Waiting on Ken / external
1. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
2. Ratifications: M-2 NNA cap (s59) · R007 AMT + 40% election (s47).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN s60** (447). Consolidated batch flow+MeF+spec+pins
  583 · S5+S6 8/8 · affected suites (returns/diagnostics/schedule_f/mef 208 ·
  adjacent 1120-S-era batch 106 · state_filing+ga600s 36) · tsc 0 · vitest 300 ·
  live ORM probe input→compute→print PASS (isolated firm, 360-obj cascade).
- Last full-suite GREEN = s54 `cd9b186` (pre-re-key; the next full run picks up
  any straggler pins on the old keys — none surfaced in the s60 sweeps).
- ⚠ **Shared-DB deploy state: mig 0187 APPLIED + seed_1120s rerun DONE (s60)**
  — 355 lines seeded, zero stale deletes, all 278-FFV keys verified carried.
  Render deploy just needs the code push (migration already applied).
- ⚠ `test_k1_import_stage3.py` standalone-run fixture errors are PRE-EXISTING.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue remaining: **6198 → M3 line_map → 3800.**

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
