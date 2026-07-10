# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-10, session 45 ("go"). Shipped: **RS renumber unit #1 — 4562 to
the 2025 face** (RS `e695c1a` / tts `4951f41`, spec-first). The tts print bug it exposed
is FIXED: the 2025 face's NEW 19h 50-year row had shifted residential rental → 19i and
nonresidential → 19j, and the field map was printing residential in the 50-year row.
MeF XML was already semantic (unchanged). Next CC lane = **renumber unit #2 (SCH_K_1120S)**
interleaved with the **full-suite straggler triage** + the FA-export reconciliation pass.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE
1. **Full-suite straggler triage — RUN COMPLETE (36 min): 5,147 passed / 169 failed /
   6 errors.** Root cause #1 ALREADY FIXED (`1125252`): compute_w2g_db read
   `tax_return.taxpayer` unguarded → RelatedObjectDoesNotExist → 500 on EVERY recompute
   endpoint for a return with no Taxpayer row (~25 failures; test_dependents fully green;
   the compute_1116 getattr-the-relation convention). Remaining classes, diagnosed by
   sample: (a) **stale cents pins from the s27 whole-dollar sweep** — the dominant class
   (test_1040 brackets `216020.25`→`216020`, schedule_j rate schedules ×12, topic5/7/8/9,
   GA-500, 8995a…); re-pin each with a hand-check, never bend compute. (b) **mechanical
   test rot** — test_apr01_fixes wants a deleted `seeded` fixture; test_tts_forms
   manifest trip-wire. (c) **8835/8936/3800/2441/8911 pipeline pins** — likely engine
   evolution since their build; verify per file. (d) ⚠ **test_flow_assertions failed ×2
   IN-SUITE but passes standalone (447/465 green)** — test-pollution/ordering class,
   investigate before trusting full-suite flow results. Full list:
   `C:\Users\Ken\AppData\Local\Temp\claude\D--dev-tts-tax-app\43c44282-a319-4b8e-9e32-60b8ea9990b6\tasks\b46rjemkq.output`
   (if gone: `pytest tests/ -q --reuse-db`, ~36 min on local PG). ⚠ That run held PRE-s45
   modules — 4562/W2G failures in it are already fixed.
2. **RS renumber unit #2: SCH_K_1120S** (worst drift count — fabricated 13f "FTC" row
   [face: Biofuel producer credit], missing 17c AE&P, wrong L18 formula, 12d/12e/13c
   misassigned, missing 3b/3c·8b/8c·13b/13e·14a/b·15a-f·16e/16f·17a-d). Then K1 → SCHL →
   6198 → M3 line_map (face template download first) → 3800 (rides the GBC unit).
   The s45 recipe: ledger `docs/rs_handoff/2026-07-09_early_era_face_audit.md`; ⚠ the
   4562 lesson — AcroForm row-group names follow FACE letters, so re-letters silently
   shift print rows even when "names exist in PDF" validation passes; add position pins.
3. **RS FA-export reconciliation pass** (queued since s32; mirror PINNED 26 vs export 30).
4. Ken-gated: **D1 typing-feel check on GA-600S** (gates the 1120-S mirror deletion) ·
   batch 2 · the e-services answers · item 10 Lacerte reprint.

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
