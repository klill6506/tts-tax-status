# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-06, twentieth session: **S-10 GA-700 (Georgia partnership + PTET)** — legs 1/2/4 DONE
(compute `1d7f102` / seed+federal-pull+frontend `6c26d72` / diagnostics `f70a9d4`). Entity-level PTET return
built (5.19%, single gross-receipts apportionment); **render leg 3 DEFERRED** (GA 700 is a fillable-forms
viewer, not a static PDF). Prior: S-4 1065 core COMPLETE (all legs 1a–6).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: pull it, read `BUILD_ORDER.md` (order) / `SEASON_PLAN.md`
  (gates) / `PRODUCT_MAP.md` (scope). BUILD_ORDER is the single source of sequence.

## Active gates (all green)
- **Flow-assertion gate (all entities)** — `cd server && pytest tests/test_flow_assertions.py -q` → **422
  passed** (incl. the 1065 gate: 22 active RS assertions + 2 guards; runners `_run_1065_assertion`/
  `_RUNNERS_1065`; unbuilt staged in `specs/flow_assertions_1065_pending.json`). Fast (~2s, pure).
- **GA-700 (this session)** — pure `pytest tests/test_ga700_compute_leg.py` (**9** — 5 entity RS pins + §179
  limit/phaseout + ratio truncation + single-state default) · DB `test_ga700_state_return_leg.py` (**3** —
  federal 1065 pull → Sch 8 → PTET 5.19%) · DB `test_ga700_diagnostics_leg.py` (**9** — 10 D_GA700_* registered
  + fire/quiet). Seeded to prod: `seed_ga700` (FormDefinition) + `seed_rules` (10 diagnostics).
- **1065 core S-4 (COMPLETE, all legs)** — the leg-by-leg suites (`test_1065_*`) are green and archived in
  STATUS_ARCHIVE; migrations 0168/0169 applied to prod.
- `manage.py check` clean; frontend `tsc --noEmit` → 0 errors. **Test DB `test_postgres` exists** — use
  `--reuse-db`. New DB tests must seed the forms they use (not migration-pre-seeded) and filter `FormLine` by
  `section__form=` NOT `form_definition=`.
- ⚠ DB runs on the shared Supabase pooler are SLOW (a fresh `--create-db` ~2min). Run DB tests in the
  background; never `drop_test_db` a slow run (kills the live session).

## ▶ RESUME HERE — **S-10 GA-700 legs 1/2/4 DONE; leg 3 (render) DEFERRED → the NEXT unit.** Full detail +
wiring checklist in the `.claude` memory `ga700-build-unit.md`.

GA Form 700 (Georgia partnership + PTET, form_code **"GA-700"**, attaches to a federal **1065**). Spec `GA700`
(RS **draft** — promote→active is an RS follow-up); cached `server/specs/ga700_spec.json`.
- **✅ Leg 1 compute (`1d7f102`)** — `FORMULAS_GA700` (`FORMULA_REGISTRY["GA-700"]`): Sch 8 income → additions/
  subtractions → Sch 7 single gross-receipts apportionment (ROUND_DOWN, defaults **1.0 / 100% GA** single-state)
  → Sch 2 apportioned → Sch 1 GA taxable → **PTET 5.19%** (§48-7-21) when `GA_PTET`, else blank (pass-through).
  GA §179 $1.05M/$2.62M separately-stated delta (K-1; NOT in the entity flow). 9 pure tests.
- **✅ Leg 2 input (`6c26d72`)** — `seed_ga700` (8 sec/30 lines, **prod-seeded**); views.py
  `PARTNERSHIP_STATE_FORM_MAP` (1065→GA-700) + `GA700_FEDERAL_PULL` (line 23→S8_1, K4c→S8_5) +
  `_populate_ga700_from_federal`; frontend `GA_700_SECTION_TABS`/`isGa700`. 3 DB tests.
- **✅ Leg 4 diagnostics (`f70a9d4`)** — `rules_ga700.py` (10 D_GA700_*, 2 warning/8 info), runner-registered +
  **prod-seeded** (`seed_rules`). 9 DB tests.

**▶ NEXT — leg 3 (render), a dedicated leg.** ⚠ The GA 700 is served via a **fillable-forms viewer**
(`apps.dor.ga.gov/FillableForms/PDFViewer/Index?form=2025GA700`), NOT a static PDF — acquisition path differs
from the flat-state recipe (try the DOR "Print Blank Forms" static PDF, or extract from the viewer). Then apply
the flat-state render recipe (NC_D400/SC1040 precedent). **Until render lands the GA-700 form does not fully
tick** in form_coverage_tracker.

**⚠⚠ FLAGGED FOR KEN — GA §179 cross-spec conflict:** the GA700 spec pins **$1,050,000 / $2,620,000** (TY2025,
matches CLAUDE.md) but the **GA600 C-corp spec used $1.25M/$3.13M**. Built GA-700 to its own spec per the
RS-integration rule; §179 is separately-stated (K-1), so this did NOT block the entity/PTET build. **Ken
(depreciation CPA) to rule which GA §179 figure is authoritative for TY2025, and RS to reconcile the two specs.**
Also flagged: GA-600S compute still uses the 0.0539 (TY2024) PTET rate — possibly stale for its own TY2025.

**v1 DEFERRED (Partner model has no residency/GA-source field — needs a migration):** partner-level GA-source
allocation (Sch 4), nonresident 4% withholding (Form G-2-A, page-1 T), composite return (IT-CR), PTET owner-side
Form 500 subtraction. Surfaced by info diagnostics (D_GA700_NRW/_COMPOSITE), never silently computed.

*(Other unblocked SPINE items if Ken redirects: S-11 1041 module, S-5/S-6, S-3 brokerage ∥, S-13/S-14; plus the
S-4 f1065 render recalibration follow-on.)*

## This session's commits (pushed to origin/main)
- `1d7f102` — GA-700 leg 1: `FORMULAS_GA700` compute + spec cache + 9 pure tests.
- `6c26d72` — GA-700 leg 2: `seed_ga700` + federal-pull wiring + frontend tabs + 3 DB tests.
- `f70a9d4` — GA-700 leg 4: `rules_ga700.py` (10 diagnostics) + 9 DB tests.
- (Prior session, S-4: `6b51c3e`/`0e2a4e5` leg 5, `f1c095b`/`b316d5b` leg 6 — S-4 1065 core COMPLETE.)

## ▶ RS follow-ups (rides a dedicated RS session)
- **NEW — GA-700 spec is `draft`** (promote→active); **⚠⚠ reconcile the GA §179 conflict** (GA700 $1.05M/$2.62M
  vs GA600 $1.25M/$3.13M — Ken to rule which is authoritative for TY2025); check GA-600S's 0.0539 PTET rate for TY2025.
- Carried — **NC_D400 / AL_FORM_40 specs are `draft`** (promote→active). SC1040 `D_SC1040_BRACKET` threshold;
  GA-500 `R-GA500-MIL` (RS spec correct; tts fix handed off `task_f550dfd2`). `8867_spec.json` stale notes;
  RS Schedule D `D_8949_006`; 8995 sibling loader D_8995_001 note.

## ▶ Waiting on Ken (carried)
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5). S2/S3/S4 ready (UNSIGNED).
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` (order) · `SEASON_PLAN.md` (gates) · `PRODUCT_MAP.md` (scope).
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
