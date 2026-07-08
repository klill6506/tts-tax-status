# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-08 ~00:45 EDT, twenty-eighth session CONTINUATION ("My priority is doing
the MeF testing"). Unit: **FIRST LIVE ATS SUBMISSIONS — 4 upload/ack rounds worked in one
evening.** The full transmission stack is now PROVEN through the envelope into per-return
content validation (MeF computed S5's refund to the dollar = engine's $7,640). Three
gateway-only defect classes found + fixed + committed: MIME multipart format (`4a067f6`),
transmission-envelope root namespaces (`30ea3b4`), FilingSecurityInformation telemetry
(`2c36799`). Plus 1125-A + 1125-E doc mappers (`e7bebb9`/`35e3b59`) and S2/S3 whole-dollar
re-pins (`53a229b`).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — ATS round 5 (the per-return content fixes)
The round-4 acks (`docs/mef/ats_receipts.md` — the canonical ATS loop log) left a clean queue:
1. **R0000-248/249 (both)**: emit `AdditionalFilerInformation/AtSubmissionFilingGrp`
   (RefundProductElectionInd=No + RefundDisbursementCd paper-check) in the 1040 header —
   same XSD-optional-but-BR-required class as FSI; read the XSD group at
   ReturnHeader1040x.xsd:584 first.
2. **IND-114 (S8)**: emit IRS1040 `CapitalDistributionInd` from `capital_gain_distributions_only`.
3. **IND-434 (S8)**: scenario-8 build lacks spouse name/SSN (MFS needs SpouseNameControlTxt) —
   pull from the scenario PDF (`docs/mef/scenarios/`).
4. **F1040-003-02/-111-02 (S5)**: emit `ChldWhoLivedWithYouCnt`/`OtherDependentsListedCnt` on
   IRS1040 (grid emitted, counts missing → HOH gate fails).
5. **⚠ KEN RULING — S1-F1040-120-01 (S5)**: $1,475 moving expense, IRS scenario form list has
   NO 3903 → assert `ClaimStorageFeesInd` (deployed-military storage fees) or build IRS3903.
Then rebuild S8/S5 (round 5) + REBUILD S2/S3/S4/S12/S13 with ALL fixes (their on-disk artifacts
are July-3/4-era) and **bundle the five into ONE transmission** (envelope now proven).
**Signing credentials + device env values are recorded in `docs/mef/ATS_UPLOAD_RUNBOOK.md` and
`docs/mef/ats_receipts.md` (repo-internal, NOT mirrored) — never in this public-mirrored file.**

## What landed this session-continuation (all committed, pushed at close)
- **ATS rounds 1-4** (receipts + full ack log in `docs/mef/ats_receipts.md`):
  R1 MIME reject → `4a067f6` builds the real two-part MIME multipart transmission file per
  Pub 5446 (all scenario writers emit `.mime`; artifacts assembled purely from on-disk parts).
  R2 X0000-008 persisted → the tell was all-placeholder ack identity fields = TRANSMISSION-
  level error → `30ea3b4` envelope root now mirrors the Pub 5446 sample verbatim (default ns +
  SOAP + efile + xsi); manifest double-declared. R3 = breakthrough (real identity, v5.3
  validated, CRC32 ✓, refund computed) rejecting only on FilingSecurityInformation →
  `2c36799` emits the telemetry group always (SoftwareInfo device fields; settings
  MEF_DEVICE_ID/MEF_DEVICE_IP; ⚠ DeviceTypeCd enum 0|1 semantics unpublished — "1" chosen,
  e-help question). R4 = per-return content rules (the round-5 queue above).
- **X0000-008 Return-root fix** (`676f3ca`) — necessary but not sufficient (see R2); regression
  pins in both mapper suites.
- **1120-S Scenario-5 doc mappers legs 1-2**: 1125-A COGS (`e7bebb9`— deterministic 9a method
  mapping, K-1-before-1125A sequence) + 1125-E officer comp (`35e3b59` — RatioType percents
  never whole-dollared, PersonNameType via name_line1). 16 tests incl. live XSD.
- **S2/S3 whole-dollar re-pins** (`53a229b`) — first run since the s27 sweep; 8995-A cents
  boundary superseded; hand-verified; execution-proven green.
- **ATS runbook** (`docs/mef/ATS_UPLOAD_RUNBOOK.md`) + receipts log (`docs/mef/ats_receipts.md`).

## Active gates
- Flow gate 422 (unchanged — serialization-only evening). Pure MeF suites 65 + 16 green.
- S2 artifact test + S3 re-pinned lines green (targeted run). S4 artifact test = pooler kill.
- ⚠ Pooler SEVERELY degraded all evening (4+ AdminShutdown connection kills; 46-min batch).
  Broad-batch discipline held; nothing outstanding blocked on it except the round-5 rebuilds.

## ▶ Waiting on Ken / external
1. **Round-5 upload** after the fixes above (+ the ClaimStorageFeesInd ruling).
2. 1041 ATS-active v5.3 schemas+BR (SOR drop gave v3.0) + 1040 v5.4 BR — re-request.
3. 1120-S business-family e-file access notice (e-help 866-255-0654 if overdue) — the
   1120-S mapper (v6.2, X0000-008-hardened) waits on it; 4562/4797/8825 doc mappers remain.
4. DeviceTypeCd enum semantics (e-help, low priority — both values validate).

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
