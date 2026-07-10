# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-09, session 43 (S-19 batch 1: item 16 — the CC retrospective —
written to `docs/RETROSPECTIVE_SCORP_2026-07.md`). **S-19 BATCH 1 IS COMPLETE** except
item 10 (Ken-gated). The 1120-S ATS lane stays Ken-gated; CC lane = **the REVIEW_QUEUE
interleave — top item: the RS FA-export reconciliation pass (queued since s32)** unless
Ken directs (retrospective discussion · batch 2 · e-services answers).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE
**S-19 batch 1 CLOSED** (all items done; 10 waits on Ken's Lacerte reprint; batch 2
arrives from Ken). Next unblocked CC work, in order of standing priority:
1. **RS FA-export reconciliation pass** (REVIEW_QUEUE s35/s32 — write runners or
   RS-disable each drift id, then re-adopt the full export; the mirror is PINNED at 26
   vs the export's 30 actives until done).
2. **1065 + 1041 K-1 allocator residual-offset units** (REVIEW_QUEUE s32 — same drift
   class as the shipped 1120-S R-K1-ROUND; each spec-first).
3. Or Ken directs: **retrospective discussion** (`docs/RETROSPECTIVE_SCORP_2026-07.md`,
   decision table at the bottom — A/B/C are single-session green-lights) · batch 2 ·
   the e-services answers when they land.

**Session-43 landing:** item 16 retrospective (analysis only, no code) — 7 keeps ·
6 prioritized proposals (local test-DB Postgres · RS face-audit sweep · race+bare-key
maintenance pass · client-formula-mirror retirement · FormEditor split · provenance
color primitive) · 3 explicit rejects. → REVIEW_QUEUE for Ken.

## ▶ Waiting on Ken / external (unchanged from s42 + the retrospective)
1. **E-services email reply** — S7/S8 ruling · 8941 key-inversion · 1040 production
   flip · SOR re-request. Unblocks the 1120-S upload loop.
2. **IdenTrust cert** (⚠ 30-day download clock from notarization). Runbook:
   `docs/mef/A2A_ENROLLMENT.md` (not mirrored).
3. **Ken reprints file-1018's 2024 Lacerte return** → re-import (item 10).
4. **Ken verifies the PWA install** (S-18c).
5. **Retrospective discussion** (new, s43) + the three s42 REVIEW_QUEUE rulings
   (1065 Sch B Q4 sibling · Q11 interpretive readings · SCHB renumber ratification).
6. TaxWise forms-usage report · client-numbering chat conclusion.

## Active gates
- **Flow-assertion gate 447** — no compute changes this session (analysis only).
- **Client parity gate** — fixture current (s42, byte-identical); vitest 278.
- MeF 1120-S 64 · S5+S6 scenario suites 8/8 (as of s42; untouched).
- ⚠ tsc project config = 0 errors; the 148-line alternate-invocation set unchanged.
- ⚠⚠ 1120-S upload gate unchanged: full scenario set + the e-help answers first.
- ⚠ TY2026 WATCH: §274(o) meals (RS R009) · Q11 $250K figures (RS SCHB R006).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` (S-19, batch 1 closed) ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
