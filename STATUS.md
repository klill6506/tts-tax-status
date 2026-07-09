# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-08, thirty-second session ("go" — the Ken-set non-e-help queue).
Units this session: **§179 pass-through unit (RS `d9923e7`; tts `421d365`+`5b8fa25`) ·
K-1 residual-offset rounding (RS `05cbe72`; tts `100bd71`) · GA501 RS amendments (same RS
commit) · S5 binary attachments (8453-CORP/8822-B) · scenario 6/7/8 fact sheets — flow
gate 440 → 444; THE S5 PACKAGE IS COMPLETE AND UPLOADABLE (IFA access pending).***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — S-17b direct deposit (the last non-e-help queue item)
The s31-evening Ken queue is DONE except S-17b: wire the existing TaxReturn bank fields
(`bank_routing_number`/`bank_account_number`/`bank_account_type`, models.py ~line 203) into
the 1040 mapper — the refund direct-deposit group (1040 line 35b/c/d) + the
AdditionalFilerInformation header group (R0000-248/249/250/251 demand RTN/DAN inside
bank-type disbursement groups; `build_additional_filer_information()` currently hardcodes
paper-check RefundDisbursementCd=0). ⚠ The RefundDisbursementCd enum semantics (0-7) are
unpublished — e-help question #5; build the RTN/DAN wiring, keep the enum conservative.
After S-17b: **1120-S Scenario 6 build** (next-buildable per the s32 fact sheets — entity
8824 + 8941 + 8949/Sch D mapper legs) · the REVIEW_QUEUE pair (1065/1041 allocator
residual-offset units; RS FA-export reconciliation).

## Session-32 state (all committed + pushed)
- **§179 pass-through (R-4797-ENTPASS)**: entity 4797 excludes §179-passed-through
  disposals (elected + `sec_179_prior`); facts ride K-1 17K "STMT" + statement page +
  `DisposOfPropWithSect179DedStmt` XML; `ENT179_GAIN` → M-2 3a. The S5 truck is live —
  engine emits pro-rata law (700/500/500, gain once); key quirk documented. 1065 box-20-L
  print = deferred to the partner-K-1-PDF unit (none exists); owner-side 1040 = own unit.
- **K-1 rounding (R-K1-ROUND)**: `allocate_whole` residual-to-last; Σ K-1 == Sch K exactly.
  1065/1041 allocators carry the same drift → REVIEW_QUEUE.
- **S5 binary attachments**: 8453-CORP (= `f8453crp.pdf`; 8453-S superseded) + 8822-B
  filled via the renderer pipeline, in the submission ZIP /attachment/ +
  BinaryAttachment docs + binaryAttachmentCnt=2. Production swaps in the SIGNED 8453 scan.
- **GA501 RS amendments**: HB 1199 text + 9c/settle-block line_map (15→27) + R-GA501-SETTLE
  + GA501-T6; tts mirror + T6 pin.
- **Scenario fact sheets** (local-only docs/mef/scenarios/): S6 next-buildable; S7 =
  M-3/5471/Sch N (Ken scope call — declared-forms ATS scope); S8 = 8975 CbC (its PDF text
  layer drops regions — rasterize).

## Active gates
- **Flow-assertion gate 444** (440 + FA-ENT-4797-179 ×2 entity files + FA-1120S-M2-179 +
  FA-K1-ROUND) — green.
- MeF 1120-S mapper pure suite 49 · 1120s spec 39 · ga501 pure 17 · attachment field maps 2.
- S5 DB suite green (rounding re-pins DB-verified in a 34:55 pooler run; the final batch
  re-verified the package with attachments + the 4797 pipeline file).
- Prod DB seeded: 1120-S ENT179_GAIN line (339 lines) + D_4797_ENTPASS rule. RS Supabase
  seeded: 4797 entity leg (FA 541) + K1_1120S rounding + GA501 (27 lines).
- ⚠ known-flaky: test_tts_forms 1125A/7203 endpoint tests errored once in the 35-min run,
  clean in isolation (pooler transient).

## ▶ Waiting on Ken / external (unchanged — e-help family)
1. ATS assistor / e-help call (866-255-0654) — the five asks (script: docs/mef/ATS_UPLOAD_RUNBOOK.md).
2. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request.
3. 1120-S business-family e-file access — the S5 IFA upload (the package itself is READY).
4. NEW scope call: is Scenario 7 (M-3/5471/Schedule N) inside Sherpa's declared-forms ATS
   scope (Pub 1436)? S6 and S8 have their own unbuilt-form lists too (fact sheets).

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
