# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-09, session 40 (S-19 batch 1 continued: items 6 · 8+14 · G-a
shipped — the batch's mechanical lane is DONE). The 1120-S ATS lane stays Ken-gated;
CC lane = **S-19 batch 1 — resume at item 9 (M&E four-line, RS SPEC FIRST)**.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE — S-19 usability batch 1 (Ken's + Whit's 1120-S list)
**`USABILITY_QUEUE.md` is the working checklist.** Done: 1 · 2 · 3 · 4 · 5 · 6 · 7 ·
8+14 · 11 · 13 (+ parity gate) · 13b · 15 · **G-a (global placeholder sweep)**.
**NEXT: item 9 — Meals & Entertainment four-line unit (M&E 100% / 80% / 50% / 0%
back in the deductions list, in-worksheet math visible).** Ken RULED: add the NEW
100% fully-deductible-meals line — **compute change → RS 1120S spec amendment FIRST**
(RS repo `D:\dev\sherpa-tax-rule-studio`; read its `session_log.md` at boot), then
seed + compute + UI + flow gate + **regenerate the client parity fixture**
(`manage.py dump_client_parity_fixture` — 1120-S compute changes require it).
Then: 12 (Sch B Q11 auto-answer — also RS-first, derived/YELLOW/overridable only) ·
16 (CC retrospective, LAST). Item 10 waits on Ken's Lacerte reprint (file 1018).
Batch 2 arrives from Ken when batch 1 is done.

**Session-40 landings (detail in STATUS_ARCHIVE s40 + USABILITY_QUEUE):**
- **6 input widths** — General-tab Entity/Return Info cards right-sized (EIN/Phone/
  method/counts/activity code; dates paired). `1b63438`
- **8+14 title-case labels** — 435 seed labels title-cased (agent-swept, structure
  byte-verified) + **both seeders re-run on the shared DB** (339+290 lines updated
  in place); client label surfaces matched. `522ea9f` + `531592e`
- **G-a placeholder sweep** — 26 files triaged (~124): 82 deleted · 11 → `title`
  tooltips (V-various, VIN, blank-behaviors) · 32 justified keeps (app controls;
  unlabeled W-2/8863 grid cells where placeholder = the only id — real labels are
  the noted follow-up). `697bf2e`
- Three parallel subagents did the mechanical sweeps; all diffs reviewed, suites
  re-run on the combined tree (vitest 278 · tsc baseline 148 byte-identical ·
  MeF 1120-S 64 post-reseed).

## ▶ Waiting on Ken / external (unchanged)
1. **E-services email reply** — S7/S8 ruling · 8941 key-inversion · 1040 production
   flip · SOR re-request. Unblocks the 1120-S upload loop.
2. **IdenTrust cert** (⚠ 30-day download clock from notarization). Runbook:
   `docs/mef/A2A_ENROLLMENT.md` (not mirrored).
3. **Ken reprints file-1018's 2024 Lacerte return** → re-import (item 10).
4. **Ken verifies the PWA install** (S-18c) — all s39/s40 fixes ride the next deploy.
5. TaxWise forms-usage report · client-numbering chat conclusion.

## Active gates
- **Flow-assertion gate 447** — untouched (no compute changes s39/s40; mig 0181 =
  validation-only; the seed reseed changed labels only).
- Client vitest 278 — green. ⚠ tsc renderer = 148 pre-existing lines (byte-identical
  through every s40 change) — maintenance item.
- ⚠⚠ 1120-S upload gate unchanged: full scenario set + the e-help answers first.
- ⚠ Item 9 will change 1120-S compute → parity fixture + flow gate + RS spec-first
  discipline all apply.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · **`USABILITY_QUEUE.md`** (the S-19 lane) ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
