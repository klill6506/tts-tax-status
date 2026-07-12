# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-12, session 65 ("go" — autonomous). **S-20a 8283-ENTITY
RS LEG COMPLETE (RS `8b6faca`, prod-seeded, export verified, tts mirror
refreshed).** The shared 8283 spec gained the entity arm verbatim from i8283
Rev. 12-2025: R-8283-ENTFILE (PTE noncash > $500 files Section A/B WITH the
1065/1120-S) · **R-8283-ENTSECB (the $5,000 Section-B test reads the ENTITY
item/group amount — NEVER the per-member allocation)** · R-8283-ENTFEED
(1120-S: rows → K12b YELLOW default / typed GREEN override; **1065: NO FEED —
the 2025 face 13a is one combined cash+noncash line; print+diagnose**) ·
R-8283-ENTCOPY (completed copy to each allocated member). D_8283_010 re-scoped
in place to the member side; NEW D_8283_014 (entity > $500 with no rows —
replaces D_SCHK_8283) / D_8283_015 (copy-to-members info) / D_8283_016 (1065
13a coverage). Scenarios T14-T16 with oracles. **FA-ENT-8283-01/02 staged
DRAFT** — the export-verbatim s64 mirrors stay intact (1120S FA export still
exactly 30); the tts leg activates them + refreshes mirrors together. J-E1/E2/
E3 queued (REVIEW_QUEUE s65, recommendations attached). Harness 45 green;
flow 460 + test_compute_8283 green against the refreshed mirror. Earlier this
session (s64 block): the FA-export reconciliation pass closed (flow 447→460).*

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
2. **S-20a tts leg — the entity 8283 build** (the RS spec is ready; audit notes
   from s65, all verified in code):
   - `NoncashContribution` FKs `TaxReturn` generically; `compute_8283`
     helpers (`row_analysis`/`noncash_rows`/`noncash_summary`) are
     return-generic — REUSE, don't fork.
   - `render_8283` (renderer.py ~8604) is 1040-gated (`form_definition.code
     != "1040"` → None) and reads Taxpayer for the name/SSN header → open to
     1120-S/1065 with entity name/EIN; add to the ENTITY packet order.
   - Feeder: `_default_k12b_from_8283` pre-formula pass, 1120-S only — the
     B11/K16e override-respecting `_set_field_value` recipe (⚠⚠ NEVER a
     registry formula — the derived-write class). 1065: no feed (J-E2).
   - NoncashContribution CRUD (views.py ~5532) must ride the
     mutation-recompute chokepoint for entity returns too.
   - MeF: **IRS8283 is a DECLARED ReturnData1120S document (2025v6.2:
     include line 86, element ref line ~1228, maxOccurs unbounded)** —
     build in builder_1120s (Section A rows; Section B → the wet-ink
     PDF-attachment refuse seam, mirror the 1040 lane's UnmappableValue at
     read_model ~1694). 1065 MeF = future-mapper deferral.
   - Diagnostics: code-register D_8283_014/015/016 (entity arms in
     rules_8283.py or a sibling); **RETIRE D_SCHK_8283** (rules_1120s_schk.py
     ~100 — the runner-deactivation precedent: rules_1065_l.D_L_M3).
   - UI: `NoncashContributionsSection.tsx` exists (1040) — mount on the
     entity navs (Deductions-adjacent tab); s50 NavScope maps D_8283_* →
     the tab.
   - FAs: write runners for FA-ENT-8283-01/02, ACTIVATE them in RS
     (draft→active), THEN refresh both gate mirrors from the export (they're
     export-verbatim since s64 — activate + refresh + runners land TOGETHER).
   - Tests: entity engagement/feeder/override DB tests + T14-T16 oracles;
     live ORM probe + browser probe (isolated firm, cascade-delete).
3. **S-20 remainder**: 2553 → 2848 → 3115 app build.
4. **Ken ratifications pending (REVIEW_QUEUE):** NEW s65 J-E1/E2/E3 (8283
   entity conventions — build proceeds on the recommendations) · s59 M-2 NNA
   cap · R007 AMT-matrix · 40% transitional election · s49 candidates · s53
   partner-percentage diagnostic · s57 K-1 health-insurance ZZ · s64 pair.
5. *(Renumber queue: CLEARED except 3800 — rides GBC. FA-export
   reconciliation: DONE s64.)*

## ▶ Waiting on Ken / external
1. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
2. Ratifications: 8283 J-E1/E2/E3 (s65) · M-2 NNA cap (s59) · R007 AMT + 40% election (s47).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN at 460** (s64 reconciliation; s65 confirmed
  against the refreshed 8283 mirror + test_compute_8283).
- ⚠ FA-ENT-8283-01/02 are DRAFT in RS — the deployed FA exports still serve
  exactly 30 (1120S) / 36 (1065); the tts leg activates + writes runners +
  refreshes mirrors in ONE motion.
- Last full-suite GREEN = s54 `cd9b186`.
- ⚠ Shared-DB deploy state: mig 0188 + seeds current (s63); no tts DB work
  s64/s65. RS prod: the 8283 amendment SEEDED (s65). Render deploy just needs
  the code push.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
