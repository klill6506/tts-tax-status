# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-08, thirty-first session (continued on "continue"). Units: **S-11 leg 6
GA-501 (`370fdb0`) + legs 7/8b — beneficiary UI/API + f1041sk1 K-1 PDF + live frontend verify
(`7c62cc0`) — ALL BUILDABLE 1041 LEGS GREEN.***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — S-11 is done to its external blockers; next lane is Ken's call
Session 31 closed S-11 legs 6, 7, and 8b:
- **Leg 6 GA-501** (`370fdb0`): all four legs one unit — compute (4/4 RS pins), input (prod-seeded,
  trust→GA-501, L17 pull), render (DOR fillable, visually verified), diagnostics (8 D_GA501_*).
- **Legs 7+8b** (session-31 second commit): f1041sk1 per-beneficiary K-1 PDF (position-correlated
  map, single-source allocator print, k1s package + render_complete wiring) · Beneficiary
  serializer/CRUD API/`FIDUCIARY_TABS`/Beneficiaries tab (the 1041 editor no longer falls
  through to the 1120-S tabs) · **live UI verify** on dev servers via an isolated probe firm
  (deleted after, 134-row cascade): input→compute round-trips, Sch B IDD, 60/40 beneficiaries,
  GA-501 created in-app (L8 449 ✓), K-1 Package PDF served.
**1041 REMAINS (both externally blocked):** leg 8a flow-assertion gate — **RS has ZERO 1041
FAs** (new REVIEW_QUEUE item w/ proposed ~15-20-FA authoring plan) · the 4 ATS scenarios
(1041 v5.3 ATS schemas still owed by the SOR re-request).
**Candidate next CC lanes:** S-17b direct deposit · 1120-S scenarios 6/7/8 fact extraction ·
RS 1041-FA authoring (if Ken green-lights the REVIEW_QUEUE item) · `test_4797_pipeline_leg.py`
full-file run (pooler healthy this session — GA-501 DB 5/5 in 50s, K-1 leg 3/3 in 62s).

## S5 1120-S ATS (parked on Ken/external — unchanged)
Engine-driven scenario build LIVE (`9754388`); uploadable package waits on: **§179 pass-through
ruling** + **K-1 rounding ruling** (both REVIEW_QUEUE 2026-07-08) + the binary-attachment leg
(8453-CORP/8822-B) + business-family IFA access (e-help call).

## Ken's e-help call — five asks (unchanged)
Full script WITH identifiers: `docs/mef/ATS_UPLOAD_RUNBOOK.md` (repo-internal, NOT mirrored).
Asks: (1) 1040 → PRODUCTION status; (2) A2A enrollment same call; (3) 1120-S business-family
access status; (4) SOR re-request (1041 v5.3 + 1040 v5.4 BR); (5) low-priority header enums.
**Plus three in-app rulings in REVIEW_QUEUE: §179 pass-through · K-1 rounding · 1041 FA authoring.**

## Active gates
- Flow-assertion gate **422** green this session (after the GA-501 compute.py branch).
- GA-501: pure 16 + DB 5/5. 1041 K-1 PDF: DB 3/3. Beneficiary endpoints: DB 3/3.
- Client tsc 0 errors; vitest **275** green.
- Prod DB seeded: GA-501 form + rules. Probe fixture fully removed after the UI verify.
- RS handoffs filed: `docs/rs_handoff/2026-07-08_ga501_spec_drift.md` (HB 1199 diagnostic text +
  line_map 9c/11c-20 extension).

## ▶ Waiting on Ken / external
1. **§179-disposition pass-through ruling** (REVIEW_QUEUE) — blocks the S5 truck.
2. **K-1 whole-dollar rounding ruling** (REVIEW_QUEUE).
3. **1041 flow-assertion RS authoring** (REVIEW_QUEUE, new s31) — blocks S-11 leg 8a.
4. ATS assistor / e-help call (866-255-0654) — the five asks.
5. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request.
6. 1120-S business-family e-file access — upload lane.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
