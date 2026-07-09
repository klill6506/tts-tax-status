# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-08, thirty-fourth session (S6 kickoff + the Ken-delegated RS trio).
Units: **S6 buildable mapper legs** (IRS8949 + IRS1120SScheduleD + `form_8949_box` + PIN
signature; tts `ff30464`) then — Ken ruled four judgment calls in-session and delegated —
**ALL THREE S6 RS ITEMS AUTHORED + SEEDED** (RS `b4c71b8`: 8941 greenfield · 8824 entity
extension · SCHD_1120S 2025-face renumber; deployed exports verified; tts mirrors refreshed).
**Scenario 6 is now APP-lane only.***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — the Form 8941 app unit (spec live: `server/specs/8941_spec.json`)
S6 build sequence, now all APP-lane:
1. **8941 unit** — compute (`compute_8941`? — worksheet rollup inputs per the spec's v1
   facts; line 5 preparer-entered per the Ken ruling) + input UI + K13g flow + K-1 box 13 +
   render (f8941 PDF is in resources, field-map stub exists) + IRS8941 doc mapper +
   D_8941_* diagnostics (§280C reminder = D_8941_004, diagnostic-only per the Ken ruling).
   Activate the staged FA-8941-01/02 when green.
2. **Entity-8824 unit** — engine flows per R-8824-ENTROUTE (entity Sch D L5/L12 via the
   compute_8824 feeds · entity 4797 L16/L5) + IRS8824 doc mapper + DROP the extract refuse
   seam (`read_model_1120s`, LikeKindExchange check) + activate FA-ENT-8824-01. §1031
   real-property hard RED applies to entities (D_8824_009); the S6 truck rides the
   documented-quirk override only.
3. **`mef_build_ats_1120s_s6`** — engine-driven build (S5 playbook; PIN-signed via
   `Signature1120SInfo`, no binaries). ⚠ UPLOAD-GATED on the REVIEW_QUEUE key-inversion
   item: the key's 8941 line 8 (12,753) inverts §45R(d)(3)(A) — the law says 51,014; the
   spec pins the law (F8941-T1); law-vs-key at upload = Ken/e-help.

## Session-34 state (tts `ff30464` + RS `b4c71b8`; mirrors + docs in the closing commit)
- **Mapper legs** (details in form_coverage_tracker): first IRS8949 doc mapper · Sch D
  (1120-S) reconcile-or-refuse vs K7/K8a · `form_8949_box` (mig 0178 prod) + entity
  Dispositions-tab select (probe-verified) · PIN signature · refuse seams (expenses-of-sale,
  'various' sold date, entity LKE rows — the LKE seam now drops with unit 2 above).
- **RS trio**: `load_8941` NEW (verbatim f8941 2025 face + i8941 2025; 16 facts/8 rules/
  23 lines/6 diags/4 scenarios/2 staged FAs) · `load_8824` entity amendment
  (R-8824-ENTROUTE; F8824-E1; FA-ENT-8824-01 staged) · SCHD_1120S renumbered to the 2025
  face (stale line rows deleted in-loader; R011/R012 + Sch K R013/R014 corrected).
  Supabase TaxForms 121 / FA 550; deployed exports verified.
- **tts mirrors refreshed**: `8941_spec.json` (NEW) · `form_8824_spec.json` ·
  `sched_d_1120s_spec.json`.
- **⚠ Two new REVIEW_QUEUE flags from the verbatim sourcing**: (1) f8941 face
  $33,000/$67,000 vs i8941 WS6 $33,300 self-contradiction (D_8941_005 hand-verify band);
  (2) ⚠⚠ the ATS S6 key INVERTS the WS5 FTE phaseout — upload decision for Ken/e-help.
- Retraction: the "null diagnostic/test ids in the RS export" claim (s34 morning) was a
  wrong-key dump artifact — corrected in the handoff doc.

## Active gates
- **Flow-assertion gate 444** — green. MeF 1120-S mapper pure 55 · MeF 1040 pure 51 ·
  tsc 0 / vitest 275. Spec-driven suites (8824/SchD/1120s) re-running vs the refreshed
  mirrors at close (pooler-slow; sibling-drift check) — verify green before building on them.
- Migration 0178 applied to prod. RS: no reseed pending; the loaders self-heal stale rows.

## ▶ Waiting on Ken / external
1. **S6 upload decision** (after units 1-3): the 8941 key-inversion — law (51,014) vs key
   (12,753) — raise with the ATS assistor/e-help (REVIEW_QUEUE s34).
2. ATS assistor / e-help call (866-255-0654) — the five asks (docs/mef/ATS_UPLOAD_RUNBOOK.md).
3. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request.
4. 1120-S business-family e-file access — the S5 IFA upload (package READY).
5. Scope call: Scenario 7 (M-3/5471/Sch N) in declared-forms ATS scope (Pub 1436)?

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
