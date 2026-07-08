# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-08, thirtieth session. Unit: **1120-S ATS Scenario-5 leg 6 —
Itemized*Schedule attachment statement mappers (`ca7e956`) — COMPLETE.***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — 1120-S Scenario-5, leg 7: engine-driven scenario build (partial)
Leg 6 statements DONE (`ca7e956`) — all 11 LineItemDetail-backed Itemized*Schedule documents
(page-1 5/19 · 1125-A A5 · Sch L 6/9/14/18 · M-1 2/6 · M-2 3a/5a), reconcile-or-refuse,
refDocId-anchored, XSD-sequence-ordered; suite 41 green incl. live-XSD.
**Leg 7 input fact sheet is READY**: `docs/mef/scenarios/scenario5_1120s_analysis.md`
(gitignored folder — local only; full per-form line values, checkbox resolutions, cross-check
mismatches). Build on the 1040-Scenario-8 playbook (`apps/efile/ats/scenario8.py` shape:
`build_scenario5_1120s_return(firm, user)` + rollback-txn command; NOTE
`mef_build_ats_scenario5.py` is the 1040 S5 — the new command needs a distinct name, e.g.
`mef_build_ats_1120s_s5`). Key fact-sheet findings: ordinary income 87,920; K-1s 50/50 with
penny-offsets (box 1 = 43,960 each; box 2 = 1,788/1,787); the shareholder↔TIN pairing is
REVERSED from the attachment-page guess (Mak = the …0001 TIN, Issa = …0005 — full synthetic
identifiers in the local fact sheet, not here); 156,855 = line 11 RENTS;
line 14 depreciation 1,019 (§179 excluded, ties trade 4562 L22); scenario 4562 face prints the
STALE TY2023 §179 threshold (engine-truth OBBBA $2.5M/$4M wins, per the ATS-keys-stale rule);
K-1 box 17 prints `K* 1,400`.

**⚠⚠ LEG-7 PARTIAL BLOCKER — §179-disposition pass-through (REVIEW_QUEUE 2026-07-08, needs
Ken).** The scenario's Dodge-truck sale (§179 1,000) must NOT be on the corp 4797 (i4797:
pass-through → K-1 box 17 code K + DispositionOfPropWithSect179DeductionsStatement + M-2 3a
gain). `aggregate_dispositions` currently routes §179 disposals through the corp 4797; RS 4797
spec is SILENT → the MeF mapper now REFUSES any disposed asset with §179 (`ca7e956`, print
unchanged). Everything else in leg 7 is buildable; the truck (and the two
DispositionOfPropWithSect179DedStmt docs + K17-K) waits on the ruling + RS spec amendment +
the compute/allocator/print unit.

## Ken's e-help call — five asks (unchanged)
Full script WITH identifiers: `docs/mef/ATS_UPLOAD_RUNBOOK.md` (repo-internal, NOT mirrored).
Asks: (1) 1040 → PRODUCTION status; (2) A2A enrollment same call; (3) 1120-S business-family
access status; (4) SOR re-request (1041 v5.3 + 1040 v5.4 BR); (5) low-priority header enums.
**NEW ask for Ken (in-app): the §179 pass-through ruling above — REVIEW_QUEUE 2026-07-08.**

## What landed this session (`ca7e956`, pushed)
- **Leg 6 statement mappers**: `build_statement_documents` in builder_1120s (11 docs; M-1
  line-6 statement synthesizes the 6a "Depreciation" row — scenario Attachment-8 shape);
  `LineDetailSource` + `line_details` on the extract; anchors set referenceDocumentId on the
  page-1/Sch L/M-1/M-2/1125-A elements; ReturnData order = the XSD sequence
  (AccumAdjAcct* first, ItemizedOtherDeductionSch2 LAST — ref line 3706, not alphabetical).
- **SUBSCHEDULE_CONFIG** (views.py) extended: 5/19/A5/M2_3a/M2_5a single-amount rollups
  (client sub-schedule UI = deferred input leg, DEFERRAL_AUDIT).
- **§179 refuse seam**: `Disposed4797Source.sec_179_elected` + build_irs4797 refusal.
- **Tests**: 7 new pure (shapes/order, M-1 synth row, reconcile-refuse, anchor links,
  absent-without-rows, §179 refusal, live-XSD full statement set) — mapper suite 41 green.
- **Leg-7 fact sheet** agent-extracted → `docs/mef/scenarios/scenario5_1120s_analysis.md`.

## Active gates
- Flow gate NOT rerun this session — no compute/renderer change (mapper + views rollup config
  + tests only). Last green 422 (session 29).
- Pure suites: 1120-S mapper **41** green (was 34).
- ⚠ `test_4797_pipeline_leg.py --reuse-db` (session-29 in-flight DB stamp): first re-run this
  session hit a pytest-timeout on the pooler mid-query (no summary line); second re-run
  in flight at close — **verify at next boot** (see "In flight" below).

## In flight at close → ⚠ OPEN: 4797 pipeline DB verify (3 pooler timeouts)
`pytest tests/test_4797_pipeline_leg.py --reuse-db` timed out THREE times this session (per-test
`--timeout=300` fired), every time ~15-16 quick passes in, always inside
`run_diagnostics(tax_year)` (twice pinned at `rules.int_8825_flow_check`'s RentalProperty
SELECT, stack in `select.select()` — the SERVER not answering, not a CPU loop). Lingering
test_postgres sessions were pg_terminate'd between runs and after the last. Most likely the
documented degraded-pooler class ([[pooler-slow-not-hung-17min]]); a real diagnostics-runner
stall under this test's data shape is NOT excluded. **Next boot, discriminate before anything
else**: `pytest "tests/test_4797_pipeline_leg.py::test_section_1245_exception_diagnostics_fire"
--reuse-db --timeout=900 -q` alone on a fresh morning connection — green ⇒ pooler load (run the
full file); stalls again ⇒ treat as a REAL bug in the diagnostics path for this shape and debug
`int_8825_flow_check`. The compute logic itself is refactor-identical (classify_disposal
bridge-gate); pure 4797 (49) + mapper (41) + flow gate 422 (s29) are green.

## ▶ Waiting on Ken / external
1. **§179-disposition pass-through ruling** (REVIEW_QUEUE 2026-07-08) — blocks the S5 truck
   piece of leg 7 + real S-corp §179-disposal e-filing.
2. ATS assistor / e-help call (866-255-0654) — the five asks above.
3. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request.
4. 1120-S business-family e-file access notice — upload lane blocked; leg 7 build remains
   buildable meanwhile.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
