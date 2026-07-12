# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-12, session 64 (account-switch boot, "pick up the queue").
**RS FA-EXPORT RECONCILIATION PASS COMPLETE (queued since s32).** The deployed
RS flow-assertion exports are re-adopted as the tts gate mirrors: **1120S =
export VERBATIM (30 actives)** — the s35 hand-splice pin era is over; **1065 =
export minus 4 staged-pending build-gaps (32 actives)**. Every s32-drift id now
has an id-routed runner: FA008-012 (the Mar-30 8825/Sch-L regression set, RS's
declarative definitions adopted as-is) → NEW `_run_8825_schl_assertion`
(source pins on the shared `aggregate_rental_income` + the renderer SCHED_L_4COL
contra translation; FA012 re-derives L15a/L15d from BOTH FORMULAS_1120S and
FORMULAS_1065); FA-ENT-BND-01/02/03 → NEW `_run_entbnd_assertion` (the S-5
`rules_entity_boundary` module is built — threshold/severity/registry pins;
ACTIVATED on both entity gates); FA-4562-179-02/03 (runners pre-existed, now
loaded); RC001's export variant (no `expected_value`, inputs key `K1`) rides a
NEW `formula_equals` fallback that evaluates `expected_formula` over the
resolved values (probe: L24d 40,000 == ΣM2_8a-d 40,000 — non-vacuous). Still
staged in `flow_assertions_1065_pending.json` (test-pinned set): GATE-8990-163J
(no §163(j)/$31M logic) · GATE-704C-706D-DEFER (no Partner item-M fields) ·
RECON-M2-CAPITAL (item L data-entry) · **FA-ENT-8824-01's 1065 arm** (capital-
character 8824 refuses to manual via D_8824_011 until the Schedule D (1065)
unit — s36 ruling). **Flow gate 447 → 460, all green**; adjacent pin suites 493
total. Gate files written ensure_ascii (the FA-JSON cp1252 rule). REVIEW_QUEUE:
s35 item RESOLVED; 2 new notes ($300 DFE proxy recommendation = keep; RC001
variant shape = leave RS as-is). `/bugs` at boot: no open reports.*

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
2. **S-20 B2-17 form units**: 8283-entity → 2553 → 2848 → 3115 app build.
   (8283-entity has a live D_SCHK_8283 warning pointing at it — s63. Spec-first:
   fetch/author the RS 8283-entity spec before building.)
3. **Ken ratifications pending (REVIEW_QUEUE):** s59 M-2 NNA distribution cap
   (the tts M-2 compute leg implements it once ratified — fold the M-2 grid
   column-letter re-key in with it, DEFERRAL_AUDIT s63 item 5) · R007
   AMT-matrix · 40% transitional election · s49 candidates · s53
   partner-percentage diagnostic · s57 K-1 health-insurance ZZ presentation ·
   NEW s64 pair ($300 DFE proxy · RC001 variant — both "leave as-is"
   recommendations, no build blocked).
4. *(Renumber queue: CLEARED except **3800** — rides the future GBC entity
   unit. The 6198 + M-3 tts build units stay unblocked-but-unscheduled.
   FA-export reconciliation: DONE s64.)*

## ▶ Waiting on Ken / external
1. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
2. Ratifications: M-2 NNA cap (s59) · R007 AMT + 40% election (s47).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN s64 at 460** (was 447; +13 = the reconciled
  activations: FA-4562-179-02/03 + FA-ENT-BND-01/03 on 1120S; FA008-012 +
  FA-8941-01 + FA-ENT-BND-01/02/03 on 1065). Both mirrors now refresh straight
  from `/api/flow-assertions/export/` — no hand-splicing.
- s64 suites: flow 460 + 1120s face-renumber pins + schk input sub-unit = 493
  green. No compute/render/client code touched (test runners + spec mirrors +
  docs only) — the wider s63 suite state stands.
- Last full-suite GREEN = s54 `cd9b186`.
- ⚠ Shared-DB deploy state: mig 0188 APPLIED + seed rerun (359) + seed_rules
  DONE (s63). No DB work this session. Render deploy just needs the code push.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
