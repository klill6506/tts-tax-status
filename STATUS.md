# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-08, twenty-ninth session. Unit: **1120-S ATS Scenario-5 doc mappers,
leg 3 (Form 4562) — COMPLETE** (`ac927b5`). One IRS4562 XML document per activity, bridge-gated
to the print renderer's asset classification via the new shared `f4562_derivation.py`.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — 1120-S Scenario-5 doc mappers, leg 4: Form 4797
Build the IRS4797 doc mapper: XSD = `docs/mef/schemas/2025v6.2/2025v6.2/CorporateIncomeTax/
Common/IRS4797/IRS4797.xsd` (2,082 lines); follow the 4562-leg pattern (`ac927b5` —
read-model source rows → pure builder → refuse-don't-guess seams → live-XSD test).
**Scenario intel (already extracted this session):** the scenario PDF's 4797 fills ONLY
page-1 lines 13/17 = $5,179 (§1245 recapture ordinary gain → 1120-S page-1 line 4); its
Part III property columns are BLANK — the engine will fill Part III properly; engine-truth
wins (ATS-answer-keys-stale rule). Read the app's 4797 compute/render source rows
(`Disposition` model / `compute_4797`) the same bridge-gate way. Then legs: 8825 (consume
`plan_4562_documents`' `8825:<rental_property_id>` doc-id links for referenceDocumentId) →
Itemized*Schedule statements → engine-driven scenario build. Upload still waits on
business-family access (Ken's e-help call — asks below).

## Ken's e-help call — five asks (unchanged)
Full script WITH identifiers: `docs/mef/ATS_UPLOAD_RUNBOOK.md` (repo-internal, NOT mirrored).
Asks: (1) 1040 → PRODUCTION status; (2) A2A enrollment (ASID + X.509 + ID.me) same call;
(3) 1120-S business-family access status; (4) SOR re-request (1041 v5.3 + 1040 v5.4 BR);
(5) low-priority: the two unpublished header-enum semantics.

## What landed this session (committed `ac927b5`, pushed)
- **IRS4562 doc mapper, one document per activity** (page1 trade / `8825:<rental>` per
  property / entity farm — the aggregate_depreciation flow split, RS 4562 R013). Part I §179
  return-level on the FIRST doc only (the IRS's own scenario layout); OBBBA $2.5M/$4M (R010,
  never the PDF's stale 2023 figures); line-6 rows exclude listed property; per-activity
  line 22 EXCLUDES §179 (R004) so it ties exactly to the engine's flow (current − §179) into
  page-1 L14 / the 8825 property. Part V listed §179/col-h split (col h omitted when fully
  §179'd, col i + line 29); Part VI amortization 42/43 split by begin year.
- **Bridge-gate**: asset classification extracted from `render_4562` into shared
  `apps/tts_forms/f4562_derivation.py` (pure, duck-typed); renderer refactored to call it —
  print output unchanged (render 21 green). Print-side 4562 quality gaps found and logged to
  DEFERRAL_AUDIT (aggregate-not-per-activity, line 22 §179 inclusion, missing 7/29/43).
- **referenceDocumentId links** (round-7 "attached to = LINKED"): page-1 `DepreciationAmt` →
  trade 4562; Sch K L11 `Section179ExpenseDeductionAmt` → the Part-I doc.
  `plan_4562_documents` exposes rental doc ids for the 8825 leg.
- **Refuse seams**: current-year asset w/ no line-19 class (print silently skips — XML
  refuses), §179 on ≤50% listed, unmappable method/convention.
- **Tests**: 8 new pure + live-XSD validation of the two-doc return. DEFERRAL_AUDIT ×3
  (24a/24b evidence input; entity line-11 §179 income limit; print-side 4562 gaps).

## Active gates
- Flow gate **422 green** (renderer.py touched — gate run + passed).
- Pure suites green: 1120-S mapper 24 (incl. 8 new) · render 4562/4797 21 · 1040 mapper +
  MIME + BR + scaffold 75.
- No compute change; no DB migration; serialization + print-refactor only.

## ▶ Waiting on Ken / external (unchanged)
1. ATS assistor / e-help call (866-255-0654) — the five asks above.
2. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request.
3. 1120-S business-family e-file access notice — upload lane blocked; 4797/8825/statement
   mapper legs remain buildable meanwhile.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
