# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-08, thirty-first session (Ken: "go ahead with the 1041 FA authoring plan").
Units this session: **S-11 legs 6 (`370fdb0`) · 7+8b (`7c62cc0`) · 8a (`06c8946` + RS `adc4710`) —
🏁 THE 1041 FULLY TICKS (tag `1041-complete`; flow gate 422 → 440).***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — KEN RULED BOTH S5 ITEMS + set the work queue (2026-07-08 evening)
**Ken's direction for the next session(s): "do everything but whatever relies on the e-help
call."** Both S5 rulings landed on the recommended options (full sequences in REVIEW_QUEUE
Resolved-since, session 31 evening):
1. **§179-disposition pass-through unit** — BUILD IT, spec-first: RS 4797 amendment (entity
   exclusion + 17K statement fields, i4797 verbatim) → compute exclusion + M-2 3a + allocator
   disposition facts + K-1 print code K + XML statement → add the S5 truck.
2. **K-1 rounding** — RESIDUAL-OFFSET allocator, spec-first: RS 1120-S K-1 spec amendment →
   k1_issuer residual-to-last-shareholder → re-pin S5 tests (notes inline).
Then the rest of the non-e-help backlog, suggested order: **S5 binary-attachment leg**
(8453-CORP + 8822-B — completes the uploadable S5 package except IFA access) · **RS
load_ga501 amendments** (docs/rs_handoff/2026-07-08_ga501_spec_drift.md) · **4797 full-file
run** (healthy pooler) · **S-17b direct deposit** · **1120-S scenarios 6/7/8 fact
extraction**. EXCLUDED (e-help/external): 1040 production cutover, S5 IFA upload, 1041 ATS
scenarios (SOR schemas), business-family access.

## S-11 1041 — COMPLETE this session (tag `1041-complete`, gate 440)
The 1041 module is done end-to-end: compute/K-1 issuance/diagnostics/render/GA-501 state
return/beneficiary UI+API/f1041sk1 K-1 PDF/live frontend verify/flow-assertion gate. Session-31
detail: form_coverage_tracker head notes (three entries) + STATUS_ARCHIVE when next rotated.
- **Leg 8a resolution worth knowing:** RS had ALREADY staged 10 draft 1041 FAs (2026-07-05,
  never activated → the export served zero). Consolidated into RS `load_1041_flow_assertions`
  (17 active + 2 staged, supersessions disabled, spec-loader drafts tombstoned); tts runners
  in `_RUNNERS_1041`. Staged pair activates when trust-side 8960 / the K-1 box-14H import land.
- **1041 MeF (the 4 ATS scenarios)** stays externally blocked: 1041 v5.3 ATS schemas owed by
  the SOR re-request (e-help ask #4).

## S5 1120-S ATS — UNBLOCKED for building (rulings landed)
Engine-driven scenario build LIVE (`9754388`). With both rulings ruled (above), the remaining
S5 work is ALL buildable: the §179 truck unit + the K-1 rounding fix + the binary-attachment
leg (8453-CORP/8822-B). Only the IFA UPLOAD of the finished package waits on business-family
access (e-help).

## Ken's e-help call — five asks (unchanged; no in-app rulings pending anymore)
Full script WITH identifiers: `docs/mef/ATS_UPLOAD_RUNBOOK.md` (repo-internal, NOT mirrored).
Asks: (1) 1040 → PRODUCTION status; (2) A2A enrollment same call; (3) 1120-S business-family
access status; (4) SOR re-request (1041 v5.3 + 1040 v5.4 BR); (5) low-priority header enums.

## Active gates
- **Flow-assertion gate 440** (422 + the 18 new 1041 tests) — green.
- GA-501 pure 16 + DB 5/5 · f1041sk1 render DB 3/3 · beneficiary endpoints 3/3.
- Client tsc 0 errors; vitest 275 green.
- Prod DB seeded: GA-501 form + rules. RS Supabase seeded: the 1041 FA family (deployed
  export verified = 17 active).
- Tags: `1041-complete` (tts). RS handoff open: `docs/rs_handoff/2026-07-08_ga501_spec_drift.md`.

## ▶ Waiting on Ken / external (e-help-call family only — both rulings RESOLVED)
1. ATS assistor / e-help call (866-255-0654) — the five asks.
2. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request (blocks 1041 ATS scenarios).
3. 1120-S business-family e-file access — the S5 IFA upload lane.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
