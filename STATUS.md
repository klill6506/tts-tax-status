# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-08, thirtieth session (continued on "go"). Units: **1120-S ATS
Scenario-5 legs 6 (`ca7e956`) + statement-source revision (`bebd7db`) + leg 7 engine-driven
scenario build (`9754388`) — ALL COMPLETE.***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — S5 build is PARTIAL-COMPLETE; two Ken rulings decide the rest
The engine-driven Scenario-5 build is LIVE (`apps/efile/ats/scenario5_1120s.py` +
`mef_build_ats_1120s_s5` command, rollback-txn): **every EXPECTED_LINES value ties to the
IRS key to the dollar and the full submission XSD-validates (2025v6.2)** — 4 DB tests green
(lines · K-1 split · XSD · truck-refuse). Remaining for an UPLOADABLE scenario package:
1. **§179 pass-through ruling** (REVIEW_QUEUE) → then the RS 4797 spec amendment + the unit
   (compute exclusion / k1_allocator facts / K-1 print code K / XML statement / M-2 3a
   addition) → add the truck to the scenario module (facts in its docstring).
2. **K-1 rounding ruling** (REVIEW_QUEUE, new) — allocator penny-offset vs diagnostic;
   test re-pin noted inline.
3. **Binary-attachment leg** (8453-CORP signature + 8822-B) — unmodeled; own leg.
4. Business-family IFA access (Ken's e-help call) — upload lane.
Next CC lanes while those wait: S-11 1041 legs 6-8 · S-17b direct deposit · 1120-S
scenarios 6/7/8 fact extraction (same playbook; PDFs already local).

## Ken's e-help call — five asks (unchanged)
Full script WITH identifiers: `docs/mef/ATS_UPLOAD_RUNBOOK.md` (repo-internal, NOT mirrored).
Asks: (1) 1040 → PRODUCTION status; (2) A2A enrollment same call; (3) 1120-S business-family
access status; (4) SOR re-request (1041 v5.3 + 1040 v5.4 BR); (5) low-priority header enums.
**Plus two in-app rulings in REVIEW_QUEUE 2026-07-08: §179 pass-through · K-1 rounding.**

## What landed this session (`ca7e956` → `bebd7db` → `9754388`, all pushed)
- **Leg 6** (`ca7e956`): 11 Itemized*Schedule statement mappers, reconcile-or-refuse,
  refDocId anchors, XSD-sequence order; §179-disposal refuse seam in the 4797 mapper.
- **Statement-source revision** (`bebd7db`): line 19 + M-2 3a/5a statements decompose their
  COMPUTE FORMULAS (D_* components w/ seed labels + D_FREE descs; Schedule K items), with
  LineItemDetail as the M-2 override tier; SUBSCHEDULE_CONFIG drops the computed lines;
  Sch B "other/Hybrid" accounting method (`TaxReturn.accounting_method_other_desc`, mig
  **0177 applied to prod**, serializer exposed); `MEF_SOFTWARE_ID_1120` (25025303). Mapper
  suite **45** green incl. live-XSD.
- **Leg 7** (`9754388`): the scenario module + command + 4 DB tests (above). Engine-truth
  divergences + preparer overrides (L3a/L3d/L10d/L10e/L24d) documented in the module.
  Fact sheet: `docs/mef/scenarios/scenario5_1120s_analysis.md` (local-only, gitignored dir).
- **4797 DB verify**: discriminator GREEN (the stalling test passes alone, 327s) —
  pooler-load, NOT a code bug (see below).

## Active gates
- 1120-S mapper pure suite **45** green (live-XSD incl. statements).
- Scenario-5 DB suite **4/4** green (`test_mef_scenario5_1120s_compute.py`).
- Flow gate not re-run — compute.py/renderer.py untouched this session (last green 422, s29).
- ⚠ `test_4797_pipeline_leg.py` full file: 3 pooler timeouts, then the discriminating
  single test PASSED alone (327s with `--timeout=900`) → verdict pooler latency
  (run_diagnostics ≈5.5 min/call tonight). Remaining: one full-file green run,
  `pytest tests/test_4797_pipeline_leg.py --reuse-db --timeout=900 -q` in a healthy
  window (~30 min at tonight's latency). pg_terminate lingering test_postgres
  sessions after any kill.

## ▶ Waiting on Ken / external
1. **§179-disposition pass-through ruling** (REVIEW_QUEUE) — blocks the S5 truck + real
   §179-disposal e-filing; RS 4797 spec amendment follows the ruling.
2. **K-1 whole-dollar rounding ruling** (REVIEW_QUEUE) — penny-offset vs diagnostic.
3. ATS assistor / e-help call (866-255-0654) — the five asks above.
4. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request.
5. 1120-S business-family e-file access — upload lane.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
