# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-12, session 67 ("go" — autonomous). **S-20b Form 2553 —
RS DRAFT COMPLETE, ⏳ AWAITING KEN'S GATE-1. No tts code this session.** The RS
2553 spec is authored + SQLite-validated (harness 82/0) but deliberately NOT
seeded: `load_2553.py` ships `READY_TO_SEED = False` and the WORK_ORDERS
two-human-gates rule holds — a greenfield draft never crosses Gate-1 unattended,
and the tts print unit doesn't start until the spec is APPROVED + exported.
Research verbatim (f2553_source_brief.md in the RS repo): Form 2553 Rev. 12-2017
+ i2553 Rev. 12-2020 both current; **the printed Q1 user fee $6,200 is
superseded → $5,750 (Rev. Proc. 2026-1 App. A, quoted from the IRB 2026-1 PDF;
year-keyed)**; §1362(b)(5) PLR fallback $14,500; KC/Ogden addresses
live-verified. Draft: 28 facts / 8 rules / 45 face lines / 19 diagnostics / 10
scenarios (the three published i2553 window examples pinned) / 3 FAs staged
DRAFT. Gate-1 walk W1-W4 → REVIEW_QUEUE s67 (recommend approve-all) + the RS
WORK_ORDERS WO-26 entry. 2848 (S-20c) gap-confirmed 404 — same draft-to-gate
recipe next. `/bugs` at boot: clean.*

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
2. **⟨GATE 1⟩ 2553 spec awaits Ken (REVIEW_QUEUE s67).** On "approve W1-W4": in
   the RS repo flip `READY_TO_SEED = True` in `load_2553.py` → `manage.py
   load_2553` on prod → verify `lookup/2553/export/` → cache the tts mirror
   `server/specs/2553_spec.json` → build the tts print unit (render leg +
   diagnostics; FA-2553 runners + activate + mirror refresh in one motion).
3. **S-20c: Form 2848 RS draft-to-gate** (same recipe as 2553: research verbatim
   f2848/i2848 → source brief → gated loader → harness → AWAITING KEN; gap
   confirmed 404 this session). Per-preparer CAF autofill is the app-side
   value-add to spec for.
4. **S-20d: 3115 tts app build** — RS spec DONE + exported (WO-23); buildable
   NOW without any gate; likely print-only v1 (§481(a) spread engine input +
   Schedule E depreciation catch-up print). Can run ahead of the 2553/2848
   approvals if Ken hasn't answered.
5. **Ken ratifications pending (REVIEW_QUEUE):** s67 2553 Gate-1 (HARD gate) ·
   s65 J-E1/E2/E3 (8283 entity, shipped-to-recommendations) · s59 M-2 NNA cap ·
   R007 AMT-matrix · 40% transitional election · s49 candidates · s53
   partner-percentage diagnostic · s57 K-1 health-insurance ZZ · s64 pair.

## ▶ Waiting on Ken / external
1. **⟨GATE 1⟩ 2553 spec approval (s67)** — blocks the whole S-20b tts leg.
2. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
3. Ratifications: 8283 J-E1/E2/E3 (s65) · M-2 NNA cap (s59) · R007 AMT + 40% election (s47).
4. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
5. IdenTrust cert (⚠ 30-day download clock). 6. File-1018 Lacerte reprint (item 10).
7. PWA install check. 8. TaxWise forms-usage report. 9. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN at 463** (unchanged — no tts code this session).
  Both mirrors current from `/api/flow-assertions/export/`; the 1065 pending
  file stages GATE-8990-163J · GATE-704C-706D-DEFER · RECON-M2-CAPITAL ·
  FA-ENT-8824-01 (test-pinned set). FA-2553-01/02/03 exist only as DRAFT rows
  in the un-seeded loader — nothing reaches the export until Gate-1.
- Last full-suite GREEN = s54 `cd9b186`. s66 suites unchanged (570 batch · MeF
  83 · tsc 0 · vitest 300).
- ⚠ Shared-DB deploy state: mig 0188 + seeds + seed_rules current (s63/s66).
  No new migrations. Render deploy just needs the code push.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
