# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-09, thirty-fifth session (overnight autonomous; Ken's standing work
order). Unit: **S6 unit 1 — the Form 8941 unit COMPLETE end-to-end** (tts `9e65aff`; RS
`ab3b0ab` FA activation). Stopped CLEANLY at the unit boundary per the work order's 80%
context guardrail — units 2 (entity-8824) and 3 (S6 build) remain.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ⏰ OVERNIGHT WORK ORDER (Ken, 2026-07-08 night — autonomous, NO QUESTIONS) — STILL STANDING
Units 1→2→3 in order, asking nothing; every judgment call pre-ruled (the four S6 rulings +
standing policy). **Unit 1 ✅ done (s35).** Law-vs-key on the 8941: engine emits the LAW
(51,014); the scenario build documents the key's 12,753 chain; UPLOAD stays Ken/e-help.
The S6 truck 8824: personal property = hard RED (D_8824_009); the scenario reproduces the
key's 8824 face via a documented-quirk override only. Commit+push every green leg; stop
cleanly before 80% context (done here — unit boundary).

## ▶ RESUME HERE — S6 unit 2: the entity-8824 unit
1. **Entity-8824 unit** — engine flows per R-8824-ENTROUTE (spec mirror
   `server/specs/form_8824_spec.json`, entity_types now 1040/1120S/1065): L21+ordinary L22
   → the ENTITY 4797 line 16 · business §1231 L22 → entity 4797 line 5 · capital L22 →
   Schedule D (1120-S) line 5 (ST) / line 12 (LT), same numbers on the 1065. compute_8824's
   feeds (`form_8824_to_4797_line5/_line16`, `form_8824_sch_d_st/_lt`) are already pure per-
   return — wire them into the ENTITY block (compute_8824_db currently runs only in the 1040
   branch; order it BEFORE aggregate_dispositions/aggregate_schedule_d, mirroring the 1040
   sequencing) and roll the Sch D 5/12 rows into line 7/15 → K7/K8a. Then: IRS8824 doc
   mapper (ReturnData ref 1291 — after 4797 976, before 8825 1312; link Sch D
   STCapGainLossLikeKindExchAmt/LT twin + entity 4797 L16 via refDocId) · DROP the
   LikeKindExchange refuse seam in `extract_return_1120s` (replace with reconcile-or-refuse)
   · extend rules_8824 D_8824_009 to entity returns (hard RED, personal property) · entity
   render leg (render_8824 for the entity packet — the 1040 renderer's face compute
   `compute_8824_face` is entity-agnostic) · activate FA-ENT-8824-01 (RS loader flip →
   Supabase reseed → mirror + runner, the s35 FA-8941 recipe).
2. **`mef_build_ats_1120s_s6`** — engine-driven build (S5 playbook; PIN-signed via
   `Signature1120SInfo`, no binaries). ⚠ UPLOAD-GATED on the key-inversion e-help item.
   S6 quirks to honor (docs/mef/scenarios/scenario6_1120s_analysis.md + the s34 memory):
   K12d §59(e)(2) row carries the §212 502,369 · K-1 odd dollars to the FIRST owner in the
   key (verify vs allocate_whole residual-to-LAST at build) · stale 4562 Part I limits on
   the key face · the truck 8824 rides the documented-quirk override.

## Session-35 state (tts `9e65aff` + RS `ab3b0ab`)
- **Form 8941 unit complete** — all legs green; details in form_coverage_tracker (s35 entry).
  Statutory line 8 = 51,014 (F8941-T1); key divergence documented only.
- **Probe-caught endpoint bug fixed in-unit**: concurrent focusout autosaves + full-row DRF
  saves = lost update; `form-8941` PATCH now `select_for_update`-locked. ⚠ The same race
  class exists on `entity-boundary` PATCH (diagnostic-only fields, low stakes) — REVIEW_QUEUE.
- **FA mirror pinned**: deployed 1120S export = 29 actives incl. the s32-drift set
  (FA008-012 / RC001-variant / FA-ENT-BND / FA-4562-179) which have NO tts runners; mirror
  = prior 23 actives + FA-8941-01/02 (25) with `_mirror_note` inside the JSON. The drift
  reconciliation pass stays queued (BUILD_ORDER ▶ NEXT).
- Migration 0179+0180 applied to prod; D_8941_* seeded (`seed_rules` run).

## Active gates
- **Flow-assertion gate 446** — green (444 + FA-8941-01/02). MeF 1120-S suite 59 (live-XSD
  incl. IRS8941) · 8941 pure 14 · 8941 DB 10 · acroform-K1 + S5 scenario 16 · tsc 0 /
  vitest 275 · spec-driven suites (8824/SchD/1120s) 71 green vs the refreshed mirrors.

## ▶ Waiting on Ken / external
1. **S6 upload decision** (after units 2-3): the 8941 key-inversion — law (51,014) vs key
   (12,753) — raise with the ATS assistor/e-help (REVIEW_QUEUE s34).
2. ATS assistor / e-help call (866-255-0654) — the five asks (docs/mef/ATS_UPLOAD_RUNBOOK.md).
3. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request.
4. 1120-S business-family e-file access — the S5 IFA upload (package READY).
5. Scope call: Scenario 7 (M-3/5471/Sch N) in declared-forms ATS scope (Pub 1436)?

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
