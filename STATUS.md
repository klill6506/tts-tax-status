# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-12, session 68 (continued — Ken came live and APPROVED
BOTH Gate-1 walks). **2553 (WO-26) + 2848 (WO-27) specs are SEEDED to RS prod,
exports verified 200, and the tts mirrors are cached**
(`server/specs/2553_spec.json` / `2848_spec.json`, pulled from the deployed
endpoint per the s64 never-hand-splice rule). The FA export stays clean — the
6 new FAs are DRAFT (1120-S serves exactly 32). The Gate-1 plain-English
re-present + AskUserQuestion worked well as the approval format; recorded in
the RS session_log as the house pattern. **No tts code yet — the S-20b/c tts
print units are now DISPATCHED as the next build pair.** `/bugs` at boot:
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
2. **S-20b/c tts print units: Form 2553 + Form 2848 as a PAIR** (specs
   approved+seeded s68; mirrors cached). Per unit: manifest + template download
   (`f2553.pdf` 4pp / `f2848.pdf` 2pp — check AcroForm fields first, prefer the
   AcroForm path) · input model/serializer (BaseModelSerializer) + UI card ·
   the computed helpers (2553 window calculator / 2848 sign-window +
   future-clock) · diagnostics code-registered from the specs (D_2553_* 19 /
   D_2848_* 17) + prod seed_rules · render legs + packet placement · **the
   2848 L2 preparer autofill from the Preparer record** (the approved
   value-add) · FA-2553-*/FA-2848-* runners + RS activate + both mirror
   refreshes in ONE motion (the s66 recipe) · live ORM + browser probes ·
   full gates. No MeF legs (both forms are paper/fax/online-upload only).
3. **S-20d: 3115 tts app build** — after the pair (WO-23 spec exported, no gate).
4. **Ken ratifications pending (REVIEW_QUEUE):** s65 J-E1/E2/E3 (8283 entity) ·
   s59 M-2 NNA cap · R007 AMT-matrix · 40% transitional election · s49
   candidates · s53 partner-percentage diagnostic · s57 K-1 health-insurance
   ZZ · s64 pair. *(The s67/s68 Gate-1s: ✅ RESOLVED-APPROVED 2026-07-12.)*

## ▶ Waiting on Ken / external
1. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
2. Ratifications: 8283 J-E1/E2/E3 (s65) · M-2 NNA cap (s59) · R007 AMT + 40% election (s47).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN at 463** (unchanged — no tts code s67/s68).
  FA-2553-01..03 / FA-2848-01..03 are DRAFT on RS prod — they activate WITH
  their tts runners + mirror refreshes in one motion during the print-unit
  build (the s65/s66 rule). Deployed FA export verified: 1120-S = 32, no
  draft leakage.
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
