# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-08, twenty-ninth session (continued in-session on Ken's "new session not
needed"). Units: **1120-S ATS Scenario-5 doc mappers legs 3+4+5 — Form 4562 (`ac927b5`),
Form 4797 + Form 8825 (`f5724fa`) — ALL COMPLETE.***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — 1120-S Scenario-5, leg 6: Itemized*Schedule statements → leg 7: scenario build
All five Scenario-5 DOCUMENT mappers are done (1125-A `e7bebb9` · 1125-E `35e3b59` · 4562
`ac927b5` · 4797 + 8825 `f5724fa`). Next: (leg 6) the Itemized*Schedule attachment statements
the scenario references — start from the scenario PDF's Attachment pages (pdf pages 0-3:
"Attachment 4, Form 1120S, Schedule …") + the ItemizedOtherDeductionsSchedule2-style
referenceDocumentName fixed lists in IRS1120S.xsd; note 8825 line 17 wants a **Schedule A
(Form 8825)** when other-deductions detail exists. Then (leg 7) the engine-driven scenario-5
build (1040-Scenario-8 playbook: `apps/efile/ats/` module building inputs through
`compute_return`, rollback-txn command). Scenario facts already extracted this session:
4562 ×2 (trade Part I §179 11,463 incl. listed Computer 464 / line 17 = 1,019; rental line
16→22 = 800) · 4797 only L13/17 = 5,179 §1245 (Part III columns BLANK in the PDF — engine
fills them; engine-truth wins). Upload still waits on business-family access (e-help call).
⚠ One in-flight check: `tests/test_4797_pipeline_leg.py` (DB stamp for the aggregate_dispositions
refactor) was still running on the pooler at close — re-run/verify green at next boot:
`pytest tests/test_4797_pipeline_leg.py --reuse-db`.

## Ken's e-help call — five asks (unchanged)
Full script WITH identifiers: `docs/mef/ATS_UPLOAD_RUNBOOK.md` (repo-internal, NOT mirrored).
Asks: (1) 1040 → PRODUCTION status; (2) A2A enrollment same call; (3) 1120-S business-family
access status; (4) SOR re-request (1041 v5.3 + 1040 v5.4 BR); (5) low-priority header enums.

## What landed this session (`ac927b5` + `f5724fa`, both pushed)
- **IRS4562** (leg 3): one document per activity; shared `apps/tts_forms/f4562_derivation.py`
  bridge-gate (render_4562 refactored onto it, print unchanged); Part I return-level on doc 1
  (OBBBA $2.5M/$4M); per-activity line 22 = Σ(current − §179) ties to page-1 L14 / 8825 flow;
  refDocId links L14 + K11; refuse seams (unmapped line-19 life, ≤50% listed §179).
- **IRS4797** (leg 4): per-asset routing extracted from `aggregate_dispositions` into shared
  `apps/returns/dispositions_4797.classify_disposal` (compute refactored onto it, behavior
  identical, flow gate 422); Part I/II rows + Part III columns (§1245/§1250); S-corp face
  rules (skip 8/9/11/12; no 18a/b; no Part IV); **reconcile-or-refuse** face-vs-flowed (L17↔
  page-1 L4, L7↔K9, unrecap↔K8c); links L4 + K9.
- **IRS8825** (leg 5): parallel address/expense groups per property from the same
  RentalProperty rows aggregate_rental_income reads; by-meaning placement (interest
  mortgage+other → face line 8; mgmt fees + supplies + other → line 17) so line 18 ties to
  total_expenses exactly; summary 20a-23 with line 23 reconciled to flowed K2 (refuse on
  drift); per-property line-14 refDocId → that rental's IRS4562; K2 links IRS8825; royalties
  refuse seam.
- **Tests**: suite 34 green (17 new across the three legs) incl. live-XSD validation of
  4562/4797/8825 documents; flow gate 422 twice (renderer leg + compute leg); render 21;
  MeF pure 75. DEFERRAL_AUDIT ×6 across both commits — headline: ⚠ **render_4797 print
  diverges from compute** (group-label §1250 test + 26a=0 vs resolve_recapture_type +
  resolved additional depreciation) — pre-existing print bug; XML side correct; print fix =
  its own render leg with Ken visual verify.

## Active gates
- Flow gate **422 green** (compute.py + renderer.py both touched this session — gate run
  after each).
- Pure suites green: 1120-S mapper **34** · render 4562/4797 21 · 4797 spec 49 · 1040
  mapper + MIME + BR + scaffold 75.
- ⚠ `test_4797_pipeline_leg.py` DB stamp in-flight at close (pooler) — verify at next boot.

## ▶ Waiting on Ken / external (unchanged)
1. ATS assistor / e-help call (866-255-0654) — the five asks above.
2. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request.
3. 1120-S business-family e-file access notice — upload lane blocked; statements + scenario
   build remain buildable meanwhile.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
