# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-10, session 45 (extended — Ken delivered S-Corp usability
BATCH 2, 17 items, and directed the depreciation UI cluster). Shipped this session:
**① RS renumber unit #1 — 4562 to the 2025 face** (RS `e695c1a` / tts `4951f41`; the
live print bug — residential rental in the new 19h 50-year row — FIXED) · **② the W2G
taxpayer-relation guard** (`1125252` — root cause #1 of the full-suite failures) ·
**③ the B2 depreciation cluster** (`8445ecc` — items 4/10/11/12: activity
sub-schedules, Calculated line 14/8825 display, denser worksheet; live-probe
verified, probe cascade-deleted). Batch-2 intake + 4 Ken rulings: `USABILITY_QUEUE.md`.
**Ken is pulling the Lacerte method list → the depreciation-methods RS spec session
(10-20 methods + the §168(k)(7) bonus opt-out election) is the next big unit.***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE
1. **Full-suite straggler triage — RUN COMPLETE (36 min): 5,147 passed / 169 failed /
   6 errors; root cause #1 FIXED (`1125252`,** the compute_w2g_db unguarded
   `tax_return.taxpayer` → 500-on-every-recompute class, ~25 failures; test_dependents
   fully green). Remaining classes, diagnosed by sample: (a) **stale cents pins from the
   s27 whole-dollar sweep** — dominant (test_1040 brackets `216020.25`→`216020`,
   schedule_j ×12, topic5/7/8/9, GA-500, 8995a…); re-pin with a hand-check, never bend
   compute. (b) **mechanical rot** — test_apr01_fixes wants a deleted `seeded` fixture;
   test_tts_forms manifest trip-wire. (c) **8835/8936/3800/2441/8911 pipeline pins** —
   verify per file. (d) ⚠ **test_flow_assertions failed ×2 IN-SUITE but passes standalone
   (447 green)** — ordering/pollution class, investigate before trusting full-suite flow
   results. Full list:
   `C:\Users\Ken\AppData\Local\Temp\claude\D--dev-tts-tax-app\43c44282-a319-4b8e-9e32-60b8ea9990b6\tasks\b46rjemkq.output`
   (if gone: `pytest tests/ -q --reuse-db`, ~36 min on local PG). ⚠ That run held pre-s45
   modules — 4562/W2G failures in it are already fixed.
2. **Depreciation-methods RS spec session** (Ken-directed, gated on his Lacerte method
   list): 10-20 methods (ADS SL, GDS SL/150DB elections, §168(f)(1) units-of-production,
   pre-MACRS/ACRS legacy, §280F caps…) + the **§168(k)(7) bonus opt-out election**
   (per-class, statement doc, kills the GA add-back) + sibling elections (SL/150DB/ADS/
   de-minimis). Spec-first on the freshly renumbered 4562; then the engine leg
   (published-table pins ONLY — never derived arithmetic); then batch-2 UI leftovers.
3. **Batch 2 remaining** (`USABILITY_QUEUE.md`): 8825 group (7 — structured address =
   model+MeF leg; PY column; multi other-expense lines; totals layout) · quick sweep
   (1 yellow-dot bug · 6 footer trim · 13/14 renames [Dispositions → "Schedule D
   (Capital Gains)" — verify all 4797 paths ride the worksheet FIRST, incl. business
   land] · 16 hide filing fields) · bigger singles (2 nav status dots · 3 PY columns
   [IncomeDeductionsSection already takes priorYear!] · 5 meals one-liner · 15 density
   pass) · B2-17 form units → Spine (8283-entity/2553/2848/3115).
4. **RS renumber unit #2: SCH_K_1120S** (fabricated 13f; missing 17c AE&P; wrong L18)
   → K1 → SCHL → 6198 → M3 line_map → 3800. Ledger:
   `docs/rs_handoff/2026-07-09_early_era_face_audit.md`; ⚠ the 4562 lesson — AcroForm
   row-group names follow FACE letters; add position pins.
5. **RS FA-export reconciliation pass** (queued since s32; mirror PINNED 26 vs export 30).
6. Ken-gated: **D1 typing-feel check on GA-600S** (gates the 1120-S mirror deletion) ·
   the Lacerte method list · e-services answers · item 10 Lacerte reprint.

**⚠ Parallel-test recipe (new, s45):** `TEST_DB_TEST_NAME=test_postgres_s45` env +
`--create-db` runs a second pytest session on its own scratch DB while a long run holds
`test_postgres` (`config/settings/test.py`).

## ▶ Waiting on Ken / external
1. **D1 typing-feel check** — open any GA-600S, type into Sched 3/6; if the ~1s autosave
   round-trip feel holds, the 1120-S client mirror goes the same way.
2. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
3. IdenTrust cert (⚠ 30-day download clock). 4. File-1018 Lacerte reprint (item 10).
5. PWA install check. 6. TaxWise forms-usage report.

## Active gates
- **Flow-assertion gate 447** — green this session (post-re-letter).
- **MeF 1120-S suite 66** (+2 s45 pins: GDS 2025 lettering/order · 50-yr mixed-month
  refuse) — green. S5+S6 scenario pair 8/8 green (XML byte-stable through the re-letter).
- **4562 render/field-map 35** — green, incl. the NEW y-band row-placement pin
  (`test_fill_4562_line19_2025_reletter`) that catches face re-letters the
  names-exist-in-PDF validation cannot.
- **Full server suite** — in flight (see RESUME 1); targeted batches all green.
- **Client parity gate** — untouched this session (no compute changes; client unchanged).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue: any RS amendment touching SCHL/SCH_K/K1/6198/3800 BEFORE its
  renumber inherits the fabrications — renumber first. (4562 ✅ s45.)

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` (batch 1 closed) ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
