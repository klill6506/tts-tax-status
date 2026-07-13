# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-13, session 71 ("go" — autonomous, then Ken live).
TWO things happened: (1) **the 1040 FA-mirror export-verbatim rebase SHIPPED**
(`9594462`; flow gate 484 → 500 GREEN; 397 active + FA-1040-4835-06
staged-pending — no 4562→4835 line-12 feeder; tests + spec mirrors only).
(2) **Ken ran the RATIFICATION SWEEP live — all 12 open REVIEW_QUEUE calls
answered, every recommendation adopted; the queue is EMPTY.** Three build
units authorized → **NEW Spine S-21**: a) the 1120-S M-2 NNA-cap compute leg
(+ the s63 off-face grid-column re-key folded in), b) the 1065
partner-percentage diagnostic, c) the 1065 Sch B Q4 auto-answer (Q11 recipe
clone, (c) presumed true). `/bugs` at boot: clean.*

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
2. **BUILD_ORDER's next unblocked Spine item = S-21a: the 1120-S M-2 NNA-cap
   compute leg** (spec-first — RS 1120S_M2, mirror current since s59; the
   ratified R002 cap = AAA without regard to the NNA, §1368(e)(1)(C); pin the
   i1120s pp.50-51 worksheet example + divergence scenarios; FOLD IN the s63
   off-face M-2 grid column re-key b/c/d). Then **S-21b** (1065 partner-pct
   WARNING) → **S-21c** (1065 Sch B Q4 auto-answer, Q11 clone, (c) presumed
   true, (d) from the M-3 flag). The 1120/709 authoring waves + the 1120-S
   ATS lane stay Ken-gated.
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
- ⚠ Shared-DB deploy state: migs 0191/0192/0193 applied; seed_rules run
  (s70). s71 touched no app code — Render deploy still just needs the push.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
