# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-09, session 41 (S-19 batch 1: item 9 — the M&E four-tier
unit — shipped spec-first end-to-end, `d8dbeba` / RS `96931bf`). The 1120-S ATS
lane stays Ken-gated; CC lane = **S-19 batch 1 — resume at item 12 (Sch B Q11
auto-answer, RS-first)**.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE — S-19 usability batch 1 (Ken's + Whit's 1120-S list)
**`USABILITY_QUEUE.md` is the working checklist.** Done: 1-9 · 11 · 13 (+ parity
gate) · 13b · 14 · 15 · G-a.
**NEXT: item 12 — Schedule B Q11 auto-answer from return context** (receipts/assets
test). **KEN RULED: auto-answer ONLY** — derived, YELLOW, overridable; Schedule L/M-1
behavior and diagnostics unchanged. **Spec-first**: RS 1120S Schedule-B amendment
BEFORE any tts compute (RS repo `D:\dev\sherpa-tax-rule-studio`; read `session_log.md`
at its boot). Then: 16 (CC retrospective, LAST). Item 10 waits on Ken's Lacerte
reprint (file 1018). Batch 2 arrives from Ken when batch 1 is done.

**Session-41 landing (detail in STATUS_ARCHIVE s41 + USABILITY_QUEUE item 9):**
- **9 · M&E four-tier worksheet** — RS-first (1120S_PAGE1 R009/R010 + 1065_PAGE1
  twins; NEW IRS_2025_PUB463 source; verbatim i1120s/i1065/Pub 463 2025). NEW
  `D_MEALS_100` (§274(n)(2)/(e) exceptions only — Ken ruling) on BOTH entities;
  worksheet folded inline into the deductions list (100/80/50/0 + computed math
  rows); 17 free-form Other Deduction rows; seeders re-run (352/313 lines);
  parity fixture regenerated. Live-verified end-to-end; probe deleted.

## ▶ Waiting on Ken / external (unchanged)
1. **E-services email reply** — S7/S8 ruling · 8941 key-inversion · 1040 production
   flip · SOR re-request. Unblocks the 1120-S upload loop.
2. **IdenTrust cert** (⚠ 30-day download clock from notarization). Runbook:
   `docs/mef/A2A_ENROLLMENT.md` (not mirrored).
3. **Ken reprints file-1018's 2024 Lacerte return** → re-import (item 10).
4. **Ken verifies the PWA install** (S-18c) — s39/s40/s41 fixes ride the next deploy.
5. TaxWise forms-usage report · client-numbering chat conclusion.

## Active gates
- **Flow-assertion gate 447** — green THIS session (the D_MEALS_DED compute change
  rode through clean; no FA pins meals).
- **Client parity gate** — fixture REGENERATED this session (352 lines, S6 engine
  dump); vitest 278 green. Regenerate again on any 1120-S compute change.
- MeF 1120-S 64 green (statement mirror now D_FREE1-17).
- ⚠ tsc project config = 0 errors; the 148-line set is the alternate renderer
  invocation baseline — maintenance item, unchanged.
- ⚠⚠ 1120-S upload gate unchanged: full scenario set + the e-help answers first.
- ⚠ Item 12 will touch 1120-S derived values → RS spec-first + parity-fixture
  discipline both apply (YELLOW derived, overridable — no compute of record).
- ⚠ TY2026 WATCH (encoded in RS R009 notes): §274(o) employer-convenience meals
  disallowance applies to amounts paid after 12/31/2025 — re-verify the meals
  tiers at the 2026 spec cut.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · **`USABILITY_QUEUE.md`** (the S-19 lane) ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
