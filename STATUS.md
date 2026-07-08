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

## ▶ RESUME HERE — S-11 COMPLETE; next lane is Ken's call
The 1041 module is done end-to-end: compute/K-1 issuance/diagnostics/render/GA-501 state
return/beneficiary UI+API/f1041sk1 K-1 PDF/live frontend verify/flow-assertion gate. Session-31
detail: form_coverage_tracker head notes (three entries) + STATUS_ARCHIVE when next rotated.
- **Leg 8a resolution worth knowing:** RS had ALREADY staged 10 draft 1041 FAs (2026-07-05,
  never activated → the export served zero). Consolidated into RS `load_1041_flow_assertions`
  (17 active + 2 staged, supersessions disabled, spec-loader drafts tombstoned); tts runners
  in `_RUNNERS_1041`. Staged pair activates when trust-side 8960 / the K-1 box-14H import land.
- **1041 MeF (the 4 ATS scenarios)** stays externally blocked: 1041 v5.3 ATS schemas owed by
  the SOR re-request (e-help ask #4).
**Candidate next CC lanes (Ken directs):** S-17b direct deposit · 1120-S scenarios 6/7/8 fact
extraction · `test_4797_pipeline_leg.py` full-file run (pooler healthy all session: DB legs
50-62s) · RS `load_ga501` amendments (HB 1199 text + line_map 9c/11c-20 — handoff filed).

## S5 1120-S ATS (parked on Ken/external — unchanged)
Engine-driven scenario build LIVE (`9754388`); uploadable package waits on: **§179 pass-through
ruling** + **K-1 rounding ruling** (both REVIEW_QUEUE 2026-07-08) + the binary-attachment leg
(8453-CORP/8822-B) + business-family IFA access (e-help call).

## Ken's e-help call — five asks (unchanged)
Full script WITH identifiers: `docs/mef/ATS_UPLOAD_RUNBOOK.md` (repo-internal, NOT mirrored).
Asks: (1) 1040 → PRODUCTION status; (2) A2A enrollment same call; (3) 1120-S business-family
access status; (4) SOR re-request (1041 v5.3 + 1040 v5.4 BR); (5) low-priority header enums.
**Plus two in-app rulings in REVIEW_QUEUE: §179 pass-through · K-1 rounding.**

## Active gates
- **Flow-assertion gate 440** (422 + the 18 new 1041 tests) — green.
- GA-501 pure 16 + DB 5/5 · f1041sk1 render DB 3/3 · beneficiary endpoints 3/3.
- Client tsc 0 errors; vitest 275 green.
- Prod DB seeded: GA-501 form + rules. RS Supabase seeded: the 1041 FA family (deployed
  export verified = 17 active).
- Tags: `1041-complete` (tts). RS handoff open: `docs/rs_handoff/2026-07-08_ga501_spec_drift.md`.

## ▶ Waiting on Ken / external
1. **§179-disposition pass-through ruling** (REVIEW_QUEUE) — blocks the S5 truck.
2. **K-1 whole-dollar rounding ruling** (REVIEW_QUEUE).
3. ATS assistor / e-help call (866-255-0654) — the five asks.
4. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request (blocks 1041 ATS scenarios).
5. 1120-S business-family e-file access — upload lane.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
