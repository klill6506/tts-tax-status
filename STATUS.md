# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-12, session 68 ("go" — autonomous). **S-20c Form 2848 —
RS DRAFT COMPLETE, ⏳ AWAITING KEN'S GATE-1 (WO-27). No tts code this session.**
The s67 draft-to-gate recipe re-run on 2848: research verbatim (Form 2848 Rev.
1-2021 + i2848 Rev. 9-2021, both current) → `f2848_source_brief.md` →
`load_2848.py` DRAFT (34 facts / 9 rules / 30 lines / 17 diagnostics / 9
scenarios / 3 FAs staged DRAFT; entity_types all six suite types; print-first,
no MeF; the L2 preparer-autofill value-add specced) → harness 73/0 →
WO-27 staged AWAITING KEN. **⚠⚠ Research catch: an IRS Recent Development
posted 08-Jul-2026 (four days old)** — 5a-other/any-5b entries record the POA
as "MODIFIED" on the CAF (the rep loses TDS transcript access + Tax Pro
Account installment agreements); "never check line 4 unless truly
specific-use" — encoded as D_2848_MODCAF / D_2848_L4CAF warnings. **Ken now
holds TWO Gate-1 walks (2553 s67 + 2848 s68) — one "approve both" clears the
S-20b/c RS lane and the tts print units build as a pair.** `/bugs` at boot:
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
2. **S-20d: 3115 tts app build** — RS spec DONE + exported (WO-23); **buildable
   NOW, no gate**; likely print-only v1 (§481(a) spread engine input + Schedule
   E depreciation catch-up print; fetch `lookup/3115/export/` first per the RS
   rule — it's 200). This is the next autonomous unit.
3. **⟨GATE 1⟩ ×2 await Ken (REVIEW_QUEUE s67 + s68):** 2553 (WO-26) and 2848
   (WO-27). On "approve": in the RS repo flip `READY_TO_SEED = True` in the
   loader(s) → `manage.py load_2553` / `load_2848` on prod → verify
   `lookup/<form>/export/` → cache the tts mirrors
   (`server/specs/2553_spec.json` / `2848_spec.json`) → build the tts print
   units (render + diagnostics + FA runners/activate/mirror-refresh in one
   motion; they pair well as one session).
4. **Ken ratifications pending (REVIEW_QUEUE):** s67+s68 Gate-1s (HARD gates) ·
   s65 J-E1/E2/E3 (8283 entity) · s59 M-2 NNA cap · R007 AMT-matrix · 40%
   transitional election · s49 candidates · s53 partner-percentage diagnostic ·
   s57 K-1 health-insurance ZZ · s64 pair.

## ▶ Waiting on Ken / external
1. **⟨GATE 1⟩ 2553 (s67) + 2848 (s68) spec approvals** — block the S-20b/c tts legs.
2. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
3. Ratifications: 8283 J-E1/E2/E3 (s65) · M-2 NNA cap (s59) · R007 AMT + 40% election (s47).
4. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
5. IdenTrust cert (⚠ 30-day download clock). 6. File-1018 Lacerte reprint (item 10).
7. PWA install check. 8. TaxWise forms-usage report. 9. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN at 463** (unchanged — no tts code s67/s68).
  FA-2553-* and FA-2848-* exist only as DRAFT rows in un-seeded loaders —
  nothing reaches the export until the Gate-1s clear.
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
