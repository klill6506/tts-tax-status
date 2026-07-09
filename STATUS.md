# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-08, thirty-fourth session ("go" — 1120-S Scenario 6 kickoff).
Units this session: **S6 buildable mapper legs COMPLETE** (IRS8949 + IRS1120SScheduleD
doc mappers + Disposition `form_8949_box` field/UI + 1120-S PIN-signature header).
**The S6 scenario BUILD is RS-BLOCKED on three Ken-lane items** (8941 greenfield spec ·
8824 entity extension · SCHD_1120S renumber) — handoff filed.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — Scenario 6 is RS-blocked; next APP lane = REVIEW_QUEUE pair or S-3
**S6 status:** the app-buildable legs are DONE (below). The scenario build needs, in order:
1. `[RS — Ken]` **Form 8941 spec** — does not exist (`lookup/8941/export/` 404); the K13g
   credit (12,753) + K-1 13P can't compute without it.
2. `[RS — Ken]` **8824 entity amendment** — spec entity_types = ['1040'] only; entity
   routing (Sch D (1120-S) L5/L12 · entity 4797 L16) unspecced. An extract refuse seam now
   blocks e-filing any 1120-S with LikeKindExchange rows (declared-form fidelity).
3. `[RS — Ken]` **SCHD_1120S line_map renumber** — draft spec uses pre-2025 numbering
   (R011 "Line 5→K7"; the 2025 face nets on line 7); diagnostics/tests export null ids.
Full scope: `docs/rs_handoff/2026-07-08_s6_rs_gaps.md` + REVIEW_QUEUE (s34 item).
Then `[APP]`: the 8941 unit (compute/input/render/mapper) + the entity-8824 unit (engine
flows + IRS8824 doc + drop the refuse seam) + `mef_build_ats_1120s_s6`.
**Meanwhile the top unblocked APP items:** the REVIEW_QUEUE allocator pair (1065/1041
residual-offset rounding, each spec-first — 1065 spec amendment is also Ken-lane) · the RS
FA-export reconciliation pass · S-3 brokerage front end (∥, not spec-gated).

## Session-34 state (all committed + pushed)
- **IRS8949 doc mapper** (`builder_1120s.build_irs8949`): per-box Part I/II groups (A-F;
  1099-DA boxes unmodeled), rows + line-2/4 totals; first 8949 document mapper anywhere
  (the 1040 lane's Sch D is aggregate-path-only).
- **IRS1120SScheduleD mapper** (`build_1120s_schedule_d`): box-total groups (1b/2/3 +
  8b/9/10) + line 7/15 nets; **reconcile-or-refuse vs the flowed K7/K8a**; K nonzero with
  no rows refuses; QOF question caller-supplied (`schd_qof_disposal`, input leg deferred).
  Links: Sch K 7/8a → Schedule D doc; Schedule D → IRS8949 (the "attached to" class).
- **Disposition `form_8949_box`** (mig 0178, prod-applied): A-F choices, blank → C/F
  derivation (`resolve_8949_box`, box/term contradictions refuse); serializer + a select on
  the entity Dispositions editor (live-UI-verified on an isolated probe; deleted after).
- **PIN signature** (`Signature1120SInfo`): PractitionerPINGrp + PINEnteredByCd +
  SignatureOptionCd + officer TaxpayerPIN/phone/email/SignatureDt/Discuss +
  IRSResponsiblePrtyInfoCurrInd/Form8822BAttachedInd; "PIN Number" without both 5-digit
  PINs refuses. S5's 8453 path unchanged (option string now emittable there too).
- **Extract refuse seams**: expenses-of-sale on a capital transaction (8949 code-E
  adjustment leg unbuilt) · 'various' sold date · entity LikeKindExchange rows (8824 gap).
- **Verified**: 1120-S mapper pure suite **55** (incl. live-XSD capgains+PIN return) ·
  flow gate 444 · S5 DB scenario suite green (background run, exit 0) · tsc 0 / vitest 275.

## Active gates
- **Flow-assertion gate 444** — green (mapper-only session; no compute changes).
- MeF 1120-S mapper pure 55 · MeF 1040 pure 51.
- Migration 0178 applied to prod; no RS spec changes (mapper legs ride the existing
  8949/SCHD_1120S coverage; the drift items are flagged, not silently coded around).

## ▶ Waiting on Ken / external
1. **NEW: the three S6 RS items** (8941 greenfield · 8824 entity · SCHD_1120S renumber) —
   `docs/rs_handoff/2026-07-08_s6_rs_gaps.md`.
2. ATS assistor / e-help call (866-255-0654) — the five asks (docs/mef/ATS_UPLOAD_RUNBOOK.md).
3. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request.
4. 1120-S business-family e-file access — the S5 IFA upload (package READY).
5. Scope call: Scenario 7 (M-3/5471/Sch N) in declared-forms ATS scope (Pub 1436)?

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
