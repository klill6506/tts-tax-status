# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-12, session 59 ("go" — autonomous). **PAGE1+M1+M2 renumber
unit #5 — RS LEG COMPLETE (RS `bb42381`); the tts leg is NEXT.** RS: 1120S_PAGE1
rebuilt to the 2025 face (line 19 = Form 7205 deduction — the row that shifted
19/20/21 → 20/21/22 — plus the full Tax and Payments block 23a-28e, previously
absent); 1120S_M1 rebuilt from a FABRICATED layout (its "3a guaranteed payments
§707(c)" is a 1065 line — real face: 3a depreciation / 3b travel & entertainment,
4 = 1+2+3, 7 = 5+6, line 8 ties SCHEDULE K LINE 18); 1120S_M2 rows 2/4 → page-1
line 22, PTEP (col b) + AE&P (col c) added, and **R002's AAA distribution cap
corrected to the §1368(e)(1)(C) without-net-negative-adjustment base** (published
i1120s pp.50-51 example + divergence pin scenarios; **Ken ratification queued,
REVIEW_QUEUE s59**). Fabricated/composite excerpts replaced verbatim (f1120s.pdf +
i1120s 2025, pymupdf); in-loader stale self-heal (facts/rules/lines/diags/
scenarios/excerpts); SQLite harness 319 checks green (twice-run self-heal proof);
prod seeded + verified; deployed exports verified; tts mirrors refreshed
(1120s_page1/m1/m2_spec.json). tts side untouched — flow 447 + spec-mirror 45 +
face pins 18 all green on the refreshed mirrors. `/bugs` at boot: no open reports.*

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
2. **tts leg of renumber unit #5: the page-1/K FFV semantic re-key + data
   migration + the seven s56 deferrals.** The RS spec (mirrors refreshed s59) is
   the target. Current internal FFV keys in `apps/returns/compute.py`
   FORMULA_REGISTRY["1120-S"]: `"19"` = other-deductions sum → face **20**;
   `"20"` = total deductions (sums 7-19) → face **21** (and gains the NEW 7205
   line-19 input term); `"21"` = OBI → face **22** (K1 reads `_d(v,"21")` —
   re-point); `"22a/22b/22c"` → **23a/23b/23c**; `"23a/23b/23c/23d"` →
   **24a/24b/24c/24z** (+ NEW 24d elective payment input); `"25"` owed → **26**
   (formula must also add the line-25 penalty per the face); `"26"` overpayment
   → **27**; `"27"` credited (see `_set_field_value(tax_return, "27", ...)`
   estimated-payments sites — check 1040 collision scoping) → **28a** + NEW
   **28b refunded** FFV. Chain: FormLine seed re-key + FFV **data migration**
   for existing returns + compute re-key + the s56 print re-route shim in
   `field_maps/f1120s.py` REMOVED (map new keys directly) + MeF LINE_ORDER
   re-key + pins updated. Then the s56 deferral tail (DEFERRAL_AUDIT s56: 7205
   input row · 24d input · 28b · 12a/12b charitable split · K16e/16f + K18 16f
   term · K14a/b K-2/K-3 conversion · 17c → M-2 col (c)/1099-DIV) and the s59
   additions (M-2 NNA cap in compute — pending Ken ratification; 7205/4255
   units stay deferred).
3. **Then: 6198 → M3 line_map (needs the face template download) → 3800** (rides
   the 3800/GBC entity unit).
4. **RS FA-export reconciliation pass** (queued since s32).
5. **S-20 B2-17 form units**: 8283-entity → 2553 → 2848 → 3115 app build.
6. **Ken ratifications pending (REVIEW_QUEUE):** NEW s59 M-2 NNA distribution cap ·
   R007 AMT-matrix · 40% transitional election · s49 candidates · s53
   partner-percentage diagnostic · s57 K-1 health-insurance ZZ presentation.

## ▶ Waiting on Ken / external
1. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
2. R007 AMT-correction ratification + 40%-election ruling (s47) + M-2 NNA cap (s59).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN s59** (447). Spec-mirror suite 45 · face pins 18 —
  all green against the refreshed s59 mirrors. No tts code changed this session.
- Last full-suite GREEN = s54 `cd9b186`.
- ⚠ `test_k1_import_stage3.py` standalone-run fixture errors are PRE-EXISTING
  (s57-verified via stash; green in-suite — the FormDefinition-preseeded class).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue remaining: **unit #5 tts leg → 6198 → M3 line_map → 3800.**
- ⚠ Prod seeders run s59: RS `load_1120s_full` (PAGE1+M1+M2 rebuild; SQLite
  harness 319-checks first; idempotent rerun verified clean; prod state
  verified: PAGE1 40 lines/47 facts/15 rules/12 scenarios · M-1 12 lines w/
  guaranteed-payments fact+R006+D003 gone · M-2 21 facts w/ PTEP+AE&P; deployed
  exports verified).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
