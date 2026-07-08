# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-08, thirty-first session ("go"). Unit: **S-11 leg 6 — GA Form 501
fiduciary state return, all four legs (`370fdb0`) — COMPLETE.***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — S-11 legs 7-8 are the next spine work
GA-501 (leg 6) landed complete in one unit (`370fdb0`): compute (4/4 RS pins to the dollar,
16 pure) · input (seed 42 lines prod-seeded, trust→GA-501 map, 1041-L17 pull, FID_TYPE
derivation, client option, tsc 0) · render (DOR fillable, GA-700 recipe, visually verified)
· diagnostics (8 D_GA501_*, D_GA501_NR RED-defer) · DB 5/5 (50s — pooler healthy tonight)
· flow gate **422** unchanged. **REMAINS for the 1041 to tick:**
1. **Leg 7 — frontend editor verify** (trust return end-to-end in the SPA; the state-return
   panel now offers Form 501 on 1041s).
2. **Leg 8 — 1041 flow-assertion gate** (RS export → gate file, the 1065 leg-6 pattern) +
   **the per-beneficiary f1041sk1 K-1 PDF** (template already downloaded, in the manifest).
Also queued from this session: RS `load_ga501` amendments (HB 1199 diagnostic text + extend
line_map to 9c/11c-20) — `docs/rs_handoff/2026-07-08_ga501_spec_drift.md`.

## S5 1120-S ATS (parked on Ken/external — unchanged from session 30)
Engine-driven scenario build LIVE (`9754388`); uploadable package waits on: **§179
pass-through ruling** + **K-1 rounding ruling** (both REVIEW_QUEUE 2026-07-08) + the
binary-attachment leg (8453-CORP/8822-B) + business-family IFA access (e-help call).
⚠ `test_4797_pipeline_leg.py` full-file green run still owed in a healthy-pooler window
(discriminator GREEN — pooler-load, not a bug); tonight's pooler was healthy (GA-501 DB
5/5 in 50s) — a good window if the next session comes soon.

## Ken's e-help call — five asks (unchanged)
Full script WITH identifiers: `docs/mef/ATS_UPLOAD_RUNBOOK.md` (repo-internal, NOT mirrored).
Asks: (1) 1040 → PRODUCTION status; (2) A2A enrollment same call; (3) 1120-S business-family
access status; (4) SOR re-request (1041 v5.3 + 1040 v5.4 BR); (5) low-priority header enums.
**Plus two in-app rulings in REVIEW_QUEUE 2026-07-08: §179 pass-through · K-1 rounding.**

## What landed this session (`370fdb0`, pushed)
- **S-11 leg 6 — GA Form 501, all four legs in one unit** (detail: form_coverage_tracker
  head note). Spec-first from the live RS GA501 export; face-verbatim 9c cap + 11c-20
  settle block added as face-arithmetic-beyond-spec (seed_1041 precedent; RS handoff filed);
  conformity diagnostics carry the HB 1199 ruling (spec text drift flagged, not silently
  re-encoded). Drive-by fix: GA-700 refresh-from-federal fall-through in views.py.

## Active gates
- Flow-assertion gate **422** green this session (compute.py touched — GA-501 early-return).
- GA-501 pure suite 16 green; DB leg 5/5 green; client tsc 0 errors.
- Prod DB seeded: `GA-501` FormDefinition (3 sections/42 lines) + `seed_rules` (8 D_GA501_*).

## ▶ Waiting on Ken / external (unchanged)
1. **§179-disposition pass-through ruling** (REVIEW_QUEUE) — blocks the S5 truck + real
   §179-disposal e-filing; RS 4797 spec amendment follows the ruling.
2. **K-1 whole-dollar rounding ruling** (REVIEW_QUEUE) — penny-offset vs diagnostic.
3. ATS assistor / e-help call (866-255-0654) — the five asks above.
4. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request.
5. 1120-S business-family e-file access — upload lane.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
