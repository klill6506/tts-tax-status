# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-08, thirty-third session ("go" — S-17b direct deposit, the last
non-e-help queue item). Unit this session: **1040 refund direct deposit end-to-end**
(mapper 35b/c/d + header RefundDisbursementGrp RTN/DAN + 1040 Payments-tab input card,
live-UI-verified on an isolated probe). **THE ENTIRE KEN-SET NON-E-HELP QUEUE IS NOW
COMPLETE** — everything remaining on the 1040/1120-S/1041 MeF lanes waits on e-help/external.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — 1120-S Scenario 6 build (next unblocked spine item)
S-17b is DONE (below). Next per BUILD_ORDER: **1120-S ATS Scenario 6** (next-buildable per
the s32 fact sheets in docs/mef/scenarios/ — entity-side 8824 LKE + 8941 + 8949/Sch D
mapper legs; PIN-signed, no binary attachments). After/parallel: the REVIEW_QUEUE pair
(1065 + 1041 allocator residual-offset rounding units, each spec-first; RS FA-export
reconciliation pass) · S-3 brokerage front end (∥, not spec-gated).

## Session-33 state (S-17b direct deposit — all legs closed)
- **Extract** (`read_model.py`): `DirectDepositSource` + `_extract_direct_deposit` —
  elected only when line 35a > 0 AND both TaxReturn bank fields present; partial/invalid
  bank data REFUSES (UnmappableValue — never a silent paper-check downgrade); no refund →
  bank fields ignored (they also serve the payment side). Type choice → "1"/"2" enum.
- **Builder** (`builder.py`): IRS1040 35b/c/d emit directly after RefundAmt (Form8888Ind
  never — single account); header `RefundDisbursementGrp` carries Cd + RTN + DAN per
  R0000-250/251. ⚠ `RefundDisbursementCd` enum still UNPUBLISHED (e-help Q#5): direct
  deposit emits the reasoned "1" via module constant `REFUND_DISBURSEMENT_CD_DIRECT_DEPOSIT`
  (one-line fix on the e-help answer); paper check keeps the live-accepted "0".
- **Validators** (`values.py`): `routing_transit_num` (9 digits, Fed-district prefix
  01-12/21-32) + `bank_account_num` (1-17 [A-Za-z0-9-]) pinned to efileTypes.xsd 2025v5.3.
- **Input** (FormEditor `PaymentsSection`): "Refund Direct Deposit (lines 35b–35d)" card on
  the 1040 Payments tab — debounced autosave via the `/info/` PATCH (the entity Extensions
  tab's endpoint); inline RTN-prefix validation. Was previously entity-editors-only.
- **Verified**: pure suite 51 (incl. live-XSD DD return) · flow gate 444 · 1120-S mapper 49 ·
  tsc 0 · vitest 275 · live UI on an isolated probe firm (card renders, invalid RTN flags,
  autosave persisted RTN/DAN/type to the DB exactly; probe deleted, 64-row cascade).
- DEFERRAL_AUDIT: the s28 "AdditionalFilerInformation is paper-check-only" item RESOLVED;
  RefundProductCIPCd stays unemitted (no bank product — rides the Sept TPG unit).

## Active gates
- **Flow-assertion gate 444** — green (unchanged; mapper-only session).
- MeF 1040 pure suite 51 · 1120-S mapper pure 49 · client tsc 0 / vitest 275.
- No DB migrations this session; no compute changes; no RS spec changes needed (MeF mapper
  leg — authoritative sources = IRS1040.xsd/ReturnHeader1040x.xsd 2025v5.3 + the 1040 BR CSV).

## ▶ Waiting on Ken / external (the e-help family — now the ONLY open lane)
1. ATS assistor / e-help call (866-255-0654) — the five asks (script: docs/mef/ATS_UPLOAD_RUNBOOK.md);
   Q#5 RefundDisbursementCd enum now has a live stake (DD emits "1").
2. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request.
3. 1120-S business-family e-file access — the S5 IFA upload (the package itself is READY).
4. Scope call: Scenario 7 (M-3/5471/Schedule N) inside Sherpa's declared-forms ATS scope
   (Pub 1436)? S6/S8 unbuilt-form lists in the fact sheets.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
