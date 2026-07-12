# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-12, session 61 ("go" — autonomous). **Renumber unit #6
COMPLETE — Form 6198 rebuilt to the REAL face (RS lane).** The old RS block's
line_map was FABRICATED (invented "prior year unallowed losses" line 2,
deductible loss on 20 vs the face's 21, 13 of 21 face lines missing). Rebuilt
verbatim vs f6198.pdf **Rev. November 2025 — a NEW revision** (Created 9/9/25)
+ i6198 Rev. 11-2025 (fetched): 21 face-keyed facts · R001-R009 (§465 substance
kept) · 25 face lines · D001-D006 · 7 scenarios incl. the THREE published i6198
L21 examples + the p.3 L5 income-offset example. Paraphrase excerpt → verbatim
(6 labels, forms_supporting.py mirrored); RuleAuthorityLink refresh-delete; full
in-loader self-heal. SQLite harness 144/0 (twice-run, pre-polluted); RS prod
seeded (stale-deletes exact: 9 facts + lines 2/10 + 2 scenarios), idempotent
rerun clean, deployed export verified; NEW tts mirror
`server/specs/form_6198_spec.json`. **tts had NO 6198 render/compute/MeF leg —
no drift possible** (field map = unmapped stub; 4835's at-risk cap matches R005).
Housekeeping: f6198.pdf was an UNREGISTERED template — hash-verified vs a fresh
irs.gov download and manifest-registered (83 forms, trip-wire bumped 82→83).
Gates: manifest 3/3 · flow 447. `/bugs` at boot: no open reports.*

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
2. **RS renumber unit #7: M3 line_map** — needs the face template download FIRST
   (f1120sm3 is a separate PDF; add to forms_manifest + `scripts/update_irs_forms.py`)
   → then **3800** (rides the 3800/GBC entity unit — the LAST audit-queue item).
3. **SCH_K input sub-unit** (the s56 tail 4-7, DEFERRAL_AUDIT s60): 12a/12b
   charitable split · K16e/16f inputs + K18 16f term · K14a/b K-2/K-3 row
   conversion · 17c → M-2 col (c)/1099-DIV wiring. Can ride the M3 trip or run
   standalone.
4. **RS FA-export reconciliation pass** (queued since s32).
5. **S-20 B2-17 form units**: 8283-entity → 2553 → 2848 → 3115 app build.
6. **Ken ratifications pending (REVIEW_QUEUE):** s59 M-2 NNA distribution cap
   (tts M-2 compute implements it once ratified) · R007 AMT-matrix · 40%
   transitional election · s49 candidates · s53 partner-percentage diagnostic ·
   s57 K-1 health-insurance ZZ presentation.
7. *(Future form unit, unblocked by s61: the 6198 tts build — input/compute/render
   legs; the spec is now face-true and mirrored. Not scheduled — Ken directs.)*

## ▶ Waiting on Ken / external
1. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
2. Ratifications: M-2 NNA cap (s59) · R007 AMT + 40% election (s47).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN s61** (447; untriggered by compute — RS-only unit —
  but run at close per convention). Manifest tests 3/3 (trip-wire 83).
- s60 consolidated state stands: flow+MeF+spec+pins 583 · S5+S6 8/8 · affected
  suites 208+106+36 · tsc 0 · vitest 300 · live ORM probe PASS.
- Last full-suite GREEN = s54 `cd9b186` (pre-re-key; next full run picks up any
  straggler pins on the old page-1 keys — none surfaced in the s60/s61 sweeps).
- ⚠ Shared-DB deploy state: mig 0187 APPLIED + seed rerun DONE (s60). Render
  deploy just needs the code push.
- ⚠ `test_k1_import_stage3.py` standalone-run fixture errors are PRE-EXISTING.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue remaining: **M3 line_map (template download first) → 3800.**

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
