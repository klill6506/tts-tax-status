# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-11, session 58 ("go" — autonomous). **1120S_SCHL renumber
unit #4 COMPLETE (RS spec-first; tts verified face-correct — pins only).** RS: the
SCHL block's TWO fabricated numbering systems replaced with the 2025 face verbatim
(assets 1-15 w/ contra pairs · liabilities 16-21, NO subtotal line · equity 22-26 ·
total 27); fabricated R002 total-liabilities rule deleted; NEW R009 L15(d) → page-1
item F (i1120s p.49 verbatim); the fabricated source "excerpts" replaced with real
p.49 text; prod stale-deletes fired (26 orphan facts + R002 + L11/L28); export
verified + tts mirror refreshed (RS `bfcb95a`). tts: seeds/compute/print/MeF all
verified face-correct (item F ← L15d both sides) — the drift was RS-ONLY, no app
fix; NEW `TestScheduleLRowPins` y-band pins lock the zone. `/bugs` sweep at boot:
no open reports.*

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
2. **RS renumber unit #5: 1120S_PAGE1 + M1 + M2 blocks** (the s56 queue insert) —
   `load_1120s_full` runs pre-Form-7205 page-1 numbering ("Line 21 = ordinary
   business income"; 2025 face: 19 = 7205 deduction, 20 other, 21 total, 22 OBI,
   tax 23a-c, payments 24a+) + a fabricated M-1 excerpt line ("3a guaranteed
   payments" is a 1065 line; the 1120-S 3a is depreciation). **The tts side of
   this unit = the page-1/K FFV re-key + data migration + the seven s56 deferrals**
   (DEFERRAL_AUDIT s56 block: 7205 input row · 24d elective payment · 28b refunded
   FFV · 12a/12b charitable split · K16e/16f inputs + K18 16f term · K14a/b K-2/K-3
   row conversion · 17c → M-2 col (c)/1099-DIV wiring).
3. **Then: 6198 → M3 line_map (needs the face template download) → 3800** (rides
   the 3800/GBC entity unit).
4. **RS FA-export reconciliation pass** (queued since s32).
5. **S-20 B2-17 form units**: 8283-entity → 2553 → 2848 → 3115 app build.
6. **Ken ratifications pending (REVIEW_QUEUE):** R007 AMT-matrix · 40% transitional
   election · s49 candidates · s53 partner-percentage diagnostic · s57 K-1
   health-insurance ZZ presentation.

## ▶ Waiting on Ken / external
1. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
2. R007 AMT-correction ratification + 40%-election ruling (s47).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN s58** (447; ran with the 18 renumber pins = 465).
- **Affected suites GREEN s58**: face-renumber pins 18 (incl. NEW
  `TestScheduleLRowPins`) · test_schl_boy_inventory 4/4. s57 suites unchanged.
  Last full-suite GREEN = s54 `cd9b186`.
- ⚠ `test_k1_import_stage3.py` standalone-run fixture errors are PRE-EXISTING
  (s57-verified via stash; green in-suite — the FormDefinition-preseeded class).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue remaining: **PAGE1+M1+M2 → 6198 → M3 line_map → 3800.**
- ⚠ Prod seeders run s58: RS `load_1120s_complete` (SCHL rebuild; SQLite validate
  first, idempotent rerun verified; stale-deletes logged above; deployed export
  verified). s57: RS `load_1120s_specs` (K1 rebuild).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
