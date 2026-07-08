# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-08, twenty-eighth session CONTINUATION-2. Unit: **🏁 ALL SEVEN 1040 ATS
SCENARIOS ACCEPTED — rounds 5/6/7 worked live in one sitting (upload↔ack↔fix with Ken). The 1040
MeF e-file stack is PROVEN against the live IRS gateway, engine-computed end-to-end. Tagged
`mef-1040-ats-accepted`.***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — 🏁 1040 ATS COMPLETE; next lane is Ken's call
**All seven designated TY2025 1040 scenarios (2/3/4/5/8/12/13) are ACCEPTED** — acceptance
table + the seven-round loop log in `docs/mef/ats_receipts.md`; post-completion steps in
`docs/mef/ATS_UPLOAD_RUNBOOK.md` (contact the ATS assistor re: production status).
Nothing in-flight. Next per BUILD_ORDER (Ken directs): **S-11 1041 legs 6-8** (GA 501 →
frontend → flow gate) or **1120-S ATS Scenario-5 doc mappers** (4562 → 4797 → 8825) — the
1120-S lane still waits on the business-family e-file access notice.

## What landed this session (committed at close)
- **Rounds 5→7 in one sitting** (upload↔ack↔fix loop with Ken live): round-5 acks accepted all
  five content fixes (sole reject = the device-IP regression, fixed + guarded); round-6 acks =
  **S4/S8/S12 ACCEPTED**; round-7 fixes (decedent-header family, IND-089 exemption counts,
  referenceDocumentId links, spouse signature trio) → **round-7 acks: S2/S3/S5/S13 ACCEPTED —
  ALL SEVEN DONE.** Full ack-by-ack detail: `docs/mef/ats_receipts.md`.
- **All five round-4 content fixes** (mapper/serialization only — no compute change):
  1. R0000-248/249 — `AdditionalFilerInformation/AtSubmissionFilingGrp` ALWAYS emitted in the
     1040 header (RefundProductElectionInd=false + RefundDisbursementCd=0). ⚠ the Cd enum (0-7)
     is UNPUBLISHED — "0"=paper-check/none reasoned from sibling enums + R0000-250/251; e-help
     question #2 (with DeviceTypeCd). Ken delegated the call.
  2. IND-114 — `CapitalDistributionInd` (7b) from `capital_gain_distributions_only`, after line 7.
  3. IND-434 — scenario-8 synthetic MFS spouse (SPOUSE_IDENTITY_GAP): fabricated name + a
     Pub 1436 test-range SSN ("00" 4th-5th digits; values in scenario8.py, repo-internal)
     + IRS1040 `SpouseNm` face echo for MFS.
  4. F1040-003-02/-111-02 — `ChldWhoLivedWithYouCnt`/`OtherDependentsListedCnt` emit whenever the
     dependents grid has rows (child bucket = SON/DAUGHTER/STEPCHILD/FOSTER CHILD + lived-with).
  5. S1-F1040-120-01 — `ClaimStorageFeesInd` asserted on S5's Schedule 1 (Ken delegated: the IRS
     scenario form list has no 3903, so the storage-fees exception is the intent). Scenario-level
     `dataclasses.replace` — NO app input yet (DEFERRAL_AUDIT, with the direct-deposit RTN/DAN gap).
- **NEW `manage.py mef_build_ats_round5`** — builds all seven scenarios through their own module
  build paths and bundles 7 Submission ZIPs → ONE Attachment ZIP → ONE MIME (`ats_out/round5/`,
  seqs 11-17, signed — creds in the repo-internal runbook, never here). Dry-run + signed run both
  green, every fix verified in the XML.
- **Tests**: 3 new pure tests (round-4 elements structural + absent-when-facts-absent + XSD
  validation); pure suites green: mapper 47, MIME, 1120-S 40-batch. Caught a real bug in review:
  lxml ns-qualified tags need `QName().localname` for tag compares.
- Docs: `ats_receipts.md` round-5 section · runbook round-5 header · DEFERRAL_AUDIT ×2.

## Active gates
- Flow gate 422 (unchanged — serialization-only session, no compute touched).
- Pure MeF suites green (51 mapper + MIME; 40 1120-S/BR/scaffold). S5+S8 DB scenario batch
  42/42 green (14:55, pooler-typical). Live gate: **all seven scenarios IRS-ACCEPTED.**

## ▶ Waiting on Ken / external
1. **Contact the ATS assistor / e-help (866-255-0654):** confirm the TY2025 1040 scenario set
   is complete + ask how to move the software ID to PRODUCTION status for Jan-2027.
2. 1041 ATS-active v5.3 schemas+BR (SOR drop gave v3.0) + 1040 v5.4 BR — re-request.
3. 1120-S business-family e-file access notice — 1120-S ATS lane waits on it; 4562/4797/8825
   doc mappers remain buildable meanwhile.
4. e-help enum questions (DeviceTypeCd, RefundDisbursementCd) — now LOW priority: both chosen
   values survived live acceptance.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
