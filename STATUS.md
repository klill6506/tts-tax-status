# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-13, session 71. FOUR acts (act four, Ken-directed):
**THE DEMO/TEST ENVIRONMENT EXISTS** — Supabase project `gujcgvxnzwsaytpobdar`
(`tts-tax-demo`), selected by `TTS_ENV=demo` → `server/.env.demo` (DB_* only,
gitignored; `.env` still supplies secrets). Fully built: all migrations
(0001→0194) + `seed_all` + per-firm print packages + **`bootstrap_demo`**
(NEW guard-refused-outside-demo command): Delvio Demo Firm · login `demo` /
`delvio-demo` · NINE synthetic returns through the real engine (1040 ATS
S2/S3/S4/S5/S8/S12/S13 + 1120-S S5/S6). `GET /version` now reports
`environment`; `run_demo.ps1` + launch.json `django-demo` start it.
Browser-verified: demo login → nine clients → Jones 1040 opens computing
(AGI $35,492). **Multi-tenant-as-product + three-environment architecture
recorded in DECISIONS.md. Dev-session probes should now PREFER the demo DB.**
Ken-next: his Render-demo-service call (phase 2, optional). Earlier acts:* (1) **the 1040 FA-mirror export-verbatim rebase
SHIPPED** (`9594462`; flow 484 → 500; FA-1040-4835-06 staged — no 4562→4835
feeder). (2) **Ken's RATIFICATION SWEEP emptied REVIEW_QUEUE** (12/12,
recommendations adopted; Spine S-21a/b/c authorized). (3) **S-21a SHIPPED —
the 1120-S M-2 NNA-cap compute leg + the face column rotation (mig 0194)**:
line 7(a) now computes under the ratified R002 cap (§1368(e)(1)(C) — AAA
without the net negative adjustment, floor 0; the published i1120s pp.50-51
example + the divergence pin are test oracles), AND the M-2 grid keys
rotated to the 2025 face (b=PTEP, c=AE&P, d=OAA — **the legacy letters had
every b/c/d value PRINTING ONE COLUMN OFF; face-verified via pymupdf widget
headers**). K17c derive re-targeted M2_7d→M2_7c; MeF builder re-keyed
(semantics unchanged); client grid headers = face. Shared DB: mig 0194 +
seed 359 clean. Gates: affected suites 636 + 137 · tsc 0 · vitest 300 ·
live ORM probe 11/11 · browser probe (rotated grid + divergence paint) ·
PRINT probe GREEN (text-band assert on the rendered packet). Boundaries →
DEFERRAL_AUDIT s71 third-act note (cascade auto-split · Lacerte col-a ·
R005/R006 manual).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE
**Ken directives standing (s48 + s52 addendum): work AUTONOMOUSLY down this list;
full gates + live probes; Ken-decisions → REVIEW_QUEUE with a recommendation, then
move on; mandatory session close before context exhausts.**
1. **Start every session with `/bugs`** (s55).
2. **FIRST (Ken-directed s71): extend `bootstrap_demo` with a synthetic
   1065 partnership + 1041 trust demo return** (hand-authored through the
   real engine — two-partner LLC w/ 8825 + guaranteed payments + K-1s; a
   trust w/ interest/dividends + fiduciary fees + beneficiary K-1/DNI;
   reuse the test-fixture shapes for correct line keys; run against the
   demo DB + verify in the browser; 1120 C-corp WAITS for the module
   build). THEN **S-21b: the 1065 partner-percentage diagnostic** (small code-registered unit: WARNING when
   profit_pct or loss_pct over ACTIVE partners doesn't sum to 100;
   capital_pct informational — D_K1_CAPPCT covers it; own unit with DB
   tests). Then **S-21c** (1065 Sch B Q4 auto-answer, Q11 clone, (c)
   presumed true, (d) from the M-3 flag; spec-first — RS 1065 SCH_B).
   S-21a DONE this session. The 1120/709 authoring waves + the 1120-S ATS
   lane stay Ken-gated.
3. **Ken ratifications pending: NONE — the queue is EMPTY** (the s71 live
   sweep answered all 12; dispositions recorded in REVIEW_QUEUE's sweep
   block). Standing non-decisions only: the 3115 OMB nit rides the next RS
   3115 amendment; the 8824 RS amendment note rides the next 8824 touch.
   *(Note: STATUS previously listed "s49 candidates" — no such open item
   exists in REVIEW_QUEUE; if there's an s49 list elsewhere Ken wants
   triaged, say so.)*

## ▶ Waiting on Ken / external
1. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
2. ~~Ratifications~~ — CLEARED by the s71 live sweep (queue empty).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).
9. s69 (no blocker): enter the firm's real CAF number + fax on the Preparer
   records (Admin → Preparers) so the 2848 L2 autofill carries them.
10. ~~s71 demo Render service~~ — DONE by Ken same-day: **LIVE at
    https://tts-tax-demo.onrender.com (verified environment=demo)**.
    Remaining nit: GEMINI_API_KEY blank on the demo service (AI help dark
    in demos until Ken sets it). Demo login: demo / delvio-demo.

## Active gates
- **Flow-assertion gate GREEN at 500** (s71: 1040 mirror rebased
  export-verbatim, 382 → 397 active + the new staged-pin test). Mirrors:
  1120S 41 export-verbatim · 1065 39 (export 43 − the s64 4 staged-pending) ·
  **1040 397 export-verbatim (export 398 − 1 staged-pending:
  FA-1040-4835-06 in `flow_assertions_1040_pending.json` — activate when a
  4562-engine → 4835 line-12 feeder lands)**. The s69 additive-append era is
  over on all three mirrors.
- s70 suites unchanged: test_3115 39 · manifest/acroform 201 (trip-wire 87) ·
  pair 36 · tsc 0 · vitest 300. Last full-suite GREEN = s54 `cd9b186`.
- ⚠ Shared-DB deploy state: **mig 0194 (M-2 column rotation) APPLIED +
  seed_1120s rerun (359, zero stale)**. ⚠ The deployed Render code still
  carries the pre-rotation formula registry until the s71 push deploys —
  push promptly (the mismatch window mirrors the s60/s63 recipe).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
