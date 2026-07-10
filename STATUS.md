# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-09, session 39 (S-19 usability batch 1 continued: items 11 · 2 ·
3 · 5 shipped + the dead "+ Add Shareholder/Partner/Officer" fix). The 1120-S ATS lane
stays Ken-gated; CC lane = **S-19 batch 1 — resume at item 6 (input widths)**.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE — S-19 usability batch 1 (Ken's + Whit's 1120-S list)
**`USABILITY_QUEUE.md` is the working checklist** (tax-app root, NOT mirrored). Done:
1 · 2 · 3 · 4 · 5 (state pass) · 7 · 11 · 13 (+ parity gate) · 13b · 15. **NEXT: item 6 —
right-size input widths** (width should suggest expected content; State fields already
right-sized by the item-5 selects). Then: 8+14 (label capitalization — server
`seed_1120s.py` + siblings, needs reseed) · G-a (placeholder sweep, ~130 across 26
files; a few already fell out with items 5/11) · 9 (M&E 4-line — **RS spec amendment
first**, Ken ruled: add the 100% line) · 12 (Sch B Q11 auto-answer — **RS spec first**,
Ken ruled: auto-answer only) · 16 (CC retrospective, last). Batch 2 arrives from Ken
when batch 1 is done.

**Session-39 landings (detail in STATUS_ARCHIVE s39 + USABILITY_QUEUE):**
- **11 focus-steal FIXED** — SubSchedulePanel rows keyed by a stable client `_key`
  (the local→server id swap was remounting the row); plus a pending-create guard
  (concurrent-blur duplicate rows, the s35 race class). `7915fce`
- **2 entity header** — breadcrumb + window title show the ENTITY name only (the
  linked client account read as the wrong taxpayer). `91b6f3e`
- **3 infinite scroll** — Return Manager sentinel-append; filters reset to page 1;
  page buttons + `page` URL param gone. `8d5508b`
- **5 state dropdowns** — shared `StateSelect` (type-ahead-friendly labels); entity /
  shareholders / partners / 8825 / 1040 taxpayer. W-2/1099 state boxes ride the 1040
  batch. `bcf4097`
- **⚠ Drive-by: "+ Add Shareholder/Partner/Officer" was DEAD** (create POSTs blank
  name; models required it → silent 400). `blank=True` ×3, **mig 0181 applied to the
  shared prod DB**. `15b0468`

## ▶ Waiting on Ken / external (unchanged from s38)
1. **E-services email reply** (sent 2026-07-09) — S7/S8 declared-forms ruling · 8941
   key-inversion · 1040 production flip · SOR re-request. Unblocks the 1120-S upload loop.
2. **IdenTrust cert** (packet overnighted 2026-07-09; ~7-14 day vetting; ⚠ 30-day
   download clock from notarization). Runbook: `docs/mef/A2A_ENROLLMENT.md` (not mirrored).
3. **Ken reprints the file-1018 client's 2024 return from Lacerte** → re-import
   (usability item 10; importer verified faithful).
4. **Ken verifies the PWA install** (S-18c) — the s39 fixes ride the same next deploy.
5. TaxWise forms-usage report · client-numbering chat conclusion.

## Active gates
- **Flow-assertion gate 447** — no compute changes this session (mig 0181 is
  validation-only; L3a/parity gates untouched).
- Client vitest 278 — green (run after every item). ⚠ `tsc -p tsconfig.renderer.json`
  = 148 pre-existing error lines (unchanged all session) — maintenance item.
- ⚠⚠ 1120-S upload gate unchanged: full scenario set + the e-help answers first.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · **`USABILITY_QUEUE.md`** (the S-19 lane) ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
