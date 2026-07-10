# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-09, session 42 (S-19 batch 1: item 12 — Schedule B Q11
auto-answer — shipped spec-first end-to-end, `94380af` / RS `b7907bc`). The 1120-S
ATS lane stays Ken-gated; CC lane = **S-19 batch 1 — resume at item 16 (CC
retrospective, the LAST batch-1 item)**.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE — S-19 usability batch 1 (Ken's + Whit's 1120-S list)
**`USABILITY_QUEUE.md` is the working checklist.** Done: 1-9 · 11 · 12 · 13 (+ parity
gate) · 13b · 14 · 15 · G-a.
**NEXT: item 16 — CC retrospective** (4 months since S-Corp work began; review the
module for things we'd now do differently, propose to Ken for discussion — the LAST
batch-1 item). Item 10 waits on Ken's Lacerte reprint (file 1018). **Batch 2 arrives
from Ken when batch 1 is done.**

**Session-42 landing (detail in STATUS_ARCHIVE s42 + USABILITY_QUEUE item 12):**
- **12 · Schedule B Q11 auto-answer** — RS-first: 1120S_SCHB **renumbered to the
  verified 2025 face** (the old block was a fabricated ~20-question numbering;
  stale rows deleted in-loader) + NEW **R006** (derived/YELLOW/overridable; Sch
  L/M-1 behavior unchanged — Ken ruling) + verbatim i1120s Q11 total-receipts
  excerpt. tts: `_auto_answer_b11_db` post-formula-pass (positive-only receipts
  sum + computed L15d); amber "Auto" pill + amber chips client-side (1120-S
  scoped); override wins permanently. 5 DB tests; live-verified; probe deleted.
  ⚠ 3 new REVIEW_QUEUE items: 1065 Sch B Q4 sibling (needs Ken ruling) · the two
  interpretive readings (net-K3 substitution, positive-only p1 4/5) · SCHB
  renumber ratification.

## ▶ Waiting on Ken / external (unchanged)
1. **E-services email reply** — S7/S8 ruling · 8941 key-inversion · 1040 production
   flip · SOR re-request. Unblocks the 1120-S upload loop.
2. **IdenTrust cert** (⚠ 30-day download clock from notarization). Runbook:
   `docs/mef/A2A_ENROLLMENT.md` (not mirrored).
3. **Ken reprints file-1018's 2024 Lacerte return** → re-import (item 10).
4. **Ken verifies the PWA install** (S-18c) — s39/s40/s41/s42 fixes ride the next deploy.
5. TaxWise forms-usage report · client-numbering chat conclusion.

## Active gates
- **Flow-assertion gate 447** — green THIS session (the B11 write is a new
  post-pass hook; no FA pins Schedule B).
- **Client parity gate** — fixture regenerated this session: BYTE-IDENTICAL (S6's
  B11 was already "false"; the derived answer agrees); vitest 278 green.
- MeF 1120-S 64 green · S5+S6 scenario DB suites 8/8 green (the auto-answer
  agrees with both scenarios' No).
- ⚠ tsc project config = 0 errors; the 148-line set is the alternate renderer
  invocation baseline — maintenance item, unchanged.
- ⚠⚠ 1120-S upload gate unchanged: full scenario set + the e-help answers first.
- ⚠ TY2026 WATCH (RS R009 + R006 notes): §274(o) meals disallowance (paid after
  12/31/2025) · re-verify the Q11 $250K figures on the 2026 face.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · **`USABILITY_QUEUE.md`** (the S-19 lane) ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
