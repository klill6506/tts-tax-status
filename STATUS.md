# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-12, session 66 ("go" — autonomous). **S-20a ENTITY FORM
8283 — UNIT COMPLETE, BOTH LEGS** (RS leg s65 `8b6faca`; tts leg s66). The
worksheet (shared NoncashContribution model/compute) mounts on the entity
Schedule K tab; **1120-S K12b defaults from the rows total** (the override-
respecting `_derive_schk_inputs_db` pre-pass — typed GREEN wins, stale derives
self-clear, conservation withholds excluded); **1065 = print+diagnose only**
(combined 13a face line — D_8283_016 coverage warning, J-E2). render_8283
serves entity name/EIN headers and joins the entity packet; MeF: IRS8283 doc
in ReturnData1120S (declared, ref 1228) + K12b refDocId; Section B refuses
(the J7 wet-ink seam). Row-level D_8283_002-013 now sweep entity rows;
**D_SCHK_8283 RETIRED** → D_8283_014 error; D_8283_015 copy-to-members info.
FA-ENT-8283-01/02 ACTIVATED with runners + both export-verbatim mirrors
refreshed together — **flow gate 460 → 463**. Gates: 570 batch · MeF 83 ·
tsc 0 · vitest 300 · NEW test_8283_entity 11 · live ORM probe 15/15 · live
browser probe (typed 3,000 → K12b server-painted YELLOW). Shared-DB:
seed_rules run (D_SCHK_8283 inactive, D_8283_014/015/016 active). Boundaries
→ DEFERRAL_AUDIT s66 (5). Earlier this conversation: s64 FA-export
reconciliation (447→460) + s65 RS leg. `/bugs` at boot ×2: clean.*

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
2. **S-20b: Form 2553 app build** (spec-first — fetch/check the RS 2553 spec;
   404 → STOP per the RS rule) → then 2848 (S-20c) → 3115 (S-20d).
3. **Ken ratifications pending (REVIEW_QUEUE):** s65 J-E1/E2/E3 (8283 entity
   conventions — SHIPPED to the recommendations, a different ruling is a
   small re-cut) · s59 M-2 NNA cap (fold the M-2 grid column-letter re-key
   in, DEFERRAL_AUDIT s63 item 5) · R007 AMT-matrix · 40% transitional
   election · s49 candidates · s53 partner-percentage diagnostic · s57 K-1
   health-insurance ZZ · s64 pair ($300 DFE proxy · RC001 shape).
4. *(Renumber queue: CLEARED except 3800 — rides GBC. FA-export
   reconciliation: DONE s64. Entity 8283: DONE s65/s66.)*

## ▶ Waiting on Ken / external
1. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
2. Ratifications: 8283 J-E1/E2/E3 (s65) · M-2 NNA cap (s59) · R007 AMT + 40% election (s47).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN at 463** (s64 reconciled 460; +3 = the
  FA-ENT-8283 activations). Both mirrors refresh straight from
  `/api/flow-assertions/export/` — the 1065 pending file stages
  GATE-8990-163J · GATE-704C-706D-DEFER · RECON-M2-CAPITAL · FA-ENT-8824-01
  (test-pinned set).
- s66 suites: flow+8283+schk+pins+diagnostics 570 · MeF 1120-S + S5/S6 83 ·
  tsc 0 · vitest 300. Two s63 fixture updates (typed K12b now carries
  is_overridden — the derived-write convention; D_SCHK_8283 pin → D_8283_014).
- Last full-suite GREEN = s54 `cd9b186`.
- ⚠ Shared-DB deploy state: mig 0188 + seeds current (s63); **seed_rules
  rerun s66** (D_SCHK_8283 → inactive; D_8283_014/015/016 seeded). No new
  migrations. Render deploy just needs the code push.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
