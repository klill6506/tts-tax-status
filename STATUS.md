# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-12, session 69 ("go" — autonomous). **S-20b/c COMPLETE:
the Form 2553 + Form 2848 tts print-unit pair SHIPPED** (both RS specs were
Gate-1-approved + seeded s68). Migs 0189/0190 + firms 0005 applied to the
shared DB; seed_rules run (D_2553_* 19 / D_2848_* 17 live); the six FAs
ACTIVATED on RS prod with runners + all three gate mirrors refreshed in one
motion — **flow gate 463 → 475 GREEN**. Live ORM probe 21/21 + live browser
probe green (an out-of-order card paint was caught live and fixed with a
monotonic seq guard). `/bugs` at boot: clean.*

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
2. **S-20d: Form 3115 tts app build** — the RS spec is DONE (WO-23, exported;
   check `server/specs/` for the cached mirror, else fetch
   `lookup/3115/export/`), NO gate. Spec-first discipline: audit the mirror
   against the deployed export at kickoff (the s64 rule).
3. After 3115: BUILD_ORDER's next unblocked Spine item (1120/709 authoring
   waves and the 1120-S ATS completion lane stay Ken-gated).
4. **Ken ratifications pending (REVIEW_QUEUE):** s65 J-E1/E2/E3 (8283 entity) ·
   s59 M-2 NNA cap · R007 AMT-matrix · 40% transitional election · s49
   candidates · s53 partner-percentage diagnostic · s57 K-1 health-insurance
   ZZ · s64 pair.

## ▶ Waiting on Ken / external
1. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
2. Ratifications: 8283 J-E1/E2/E3 (s65) · M-2 NNA cap (s59) · R007 AMT + 40% election (s47).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).
9. NEW (s69, no blocker): enter the firm's real CAF number + fax on the
   Preparer records (Admin → Preparers — the new fields) so the 2848 L2
   autofill carries them.

## Active gates
- **Flow-assertion gate GREEN at 475** (s69: +12 — FA-2553-WINDOW/COUNT/8832
  ×1120S + FA-2848-FUTURE/SIGN45/CAFFILL ×1120S/1065/1040). Mirrors: 1120S 38
  export-verbatim · 1065 36 (export 40 − the s64 4 staged-pending) · 1040 379
  (376 pre-s64 pinned + the 3 FA-2848 appended ADDITIVELY — the full 1040
  rebase is DEFERRAL_AUDIT s69 item 1).
- s69 suites: NEW test_2553_2848_pair 36 · manifest/acroform/packet 206 ·
  tsc 0 · vitest 300. Last full-suite GREEN = s54 `cd9b186`.
- ⚠ Shared-DB deploy state: **migs 0189 + 0190 (returns) + firms 0005 applied;
  seed_rules run (s69)**. Render deploy just needs the code push.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
