# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-13, session 71 ("go" — autonomous). **The s69/s70 1040
FA-mirror rebase SHIPPED** (the one non-Ken-gated candidate in BUILD_ORDER's
NOW line; DEFERRAL_AUDIT s69 item 1 CLOSED). The 1040 gate mirror is now
EXPORT-VERBATIM from the deployed RS endpoint (the s64 recipe): 397 active +
1 staged-pending (FA-1040-4835-06 — no 4562-engine → 4835 line-12 feeder
exists). 16 new ids id-routed to runners (NEW _run_461_assertion +
_run_4835_assertion; SCHE-03 + 8582-08 id-first arms; FA-8941-01 rode the
existing prefix chains); 130 drifted shared definitions adopted as-is — one
runner fix (FA-1040-8911-04 re-kinded gating_check → flow_check by the RS
Form-3800 amendment; 6a landing pin added). **Flow gate 484 → 500 GREEN.**
No app/compute code touched — tests + spec mirrors only. `/bugs` at boot:
clean.*

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
2. **BUILD_ORDER's next unblocked Spine item** — S-20 fully done AND the 1040
   FA-mirror rebase done (s71); the 1120/709 authoring waves and the 1120-S
   ATS completion lane stay Ken-gated. **Nothing is unblocked: idle — Ken
   directs.**
3. **Ken ratifications pending (REVIEW_QUEUE):** s65 J-E1/E2/E3 (8283 entity) ·
   s59 M-2 NNA cap · R007 AMT-matrix · 40% transitional election · s49
   candidates · s53 partner-percentage diagnostic · s57 K-1 health-insurance
   ZZ · s64 pair · s70 RS-3115 OMB-citation nit (fold into the next RS
   3115 amendment).

## ▶ Waiting on Ken / external
1. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
2. Ratifications: 8283 J-E1/E2/E3 (s65) · M-2 NNA cap (s59) · R007 AMT + 40% election (s47).
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
