# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-06, twenty-first session: **S-10 GA-700 render leg 3 DONE (`0d59255`) → S-10 GA-700
COMPLETE, all 4 legs.** The DOR "Print Blank Forms" static PDF resolved the deferred render blocker (the form
was NOT viewer-only); AcroForm text-overlay on the DOR fillable PDF whose semantic field names map ~1:1 to the
compute keys. Prior this session: legs 1/2/4 (compute `1d7f102` / seed+federal-pull+frontend `6c26d72` /
diagnostics `f70a9d4`). S-4 1065 core COMPLETE (all legs 1a–6).*

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
- **GA-700 (all 4 legs)** — pure `test_ga700_compute_leg.py` (**9**) · DB `test_ga700_state_return_leg.py`
  (**3** — federal 1065 pull → Sch 8 → PTET 5.19%) · DB `test_ga700_diagnostics_leg.py` (**9**) · **DB
  `test_ga700_render_leg.py` (3 — valid 8-page PDF + values land in the text layer + PTET-off blanks)**.
  Seeded to prod: `seed_ga700` (FormDefinition) + `seed_rules` (10 diagnostics). Render template committed:
  `server/pdf_templates/ga700_2025.pdf` (DOR blank, sha256 8661856…).
- **1065 core S-4 (COMPLETE, all legs)** — the leg-by-leg suites (`test_1065_*`) are green and archived in
  STATUS_ARCHIVE; migrations 0168/0169 applied to prod.
- `manage.py check` clean; frontend `tsc --noEmit` → 0 errors. **Test DB `test_postgres` exists** — use
  `--reuse-db`. New DB tests must seed the forms they use (not migration-pre-seeded) and filter `FormLine` by
  `section__form=` NOT `form_definition=`.
- ⚠ DB runs on the shared Supabase pooler are SLOW (a fresh `--create-db` ~2min). Run DB tests in the
  background; never `drop_test_db` a slow run (kills the live session).

## ▶ RESUME HERE — **S-10 GA-700 COMPLETE (all 4 legs). Next unit = Ken directs.** Full detail in the `.claude`
memory `ga700-build-unit.md`.

GA Form 700 (Georgia partnership + PTET, form_code **"GA-700"**, attaches to a federal **1065**) is built
end-to-end: input → compute → render → diagnostics. Spec `GA700` (RS **draft** — promote→active is an RS
follow-up); cached `server/specs/ga700_spec.json`.
- **✅ Leg 1 compute (`1d7f102`)** — `FORMULAS_GA700`: Sch 8 income → Sch 7 single gross-receipts apportionment
  (ROUND_DOWN, defaults **1.0 / 100% GA**) → Sch 2 apportioned → Sch 1 GA taxable → **PTET 5.19%** (§48-7-21).
- **✅ Leg 2 input (`6c26d72`)** — `seed_ga700` + `PARTNERSHIP_STATE_FORM_MAP` + `GA700_FEDERAL_PULL` + frontend.
- **✅ Leg 3 render (`0d59255`)** — `render_ga700_overlay` (AcroForm text overlay on the DOR fillable PDF, whose
  semantic field names S1L1..S8L12 map ~1:1 to compute keys; template stored unmodified in `pdf_templates/`).
  Ratio pre-formatted to a 6-decimal factor; ratio→Sch2 L4 and Total Tax→Sch3 L1 transcriptions; PTET→
  `CBX_ELECT_TO_PAY` checkbox. Visually verified on all 4 schedule pages. 3 DB tests.
- **✅ Leg 4 diagnostics (`f70a9d4`)** — `rules_ga700.py` (10 D_GA700_*), runner-registered + **prod-seeded**.

**▶ NEXT — Ken directs** among unblocked SPINE items: **S-11 1041 module** (RS DONE) · **S-5/S-6** boundary +
PAL/basis (RS DONE) · **S-3 brokerage front end** (∥) · **S-13/S-14 1120 + state C-corp** (RS DONE) · the S-4
**f1065 render recalibration** follow-on. Small GA-700 follow-ups (non-blocking, see RS/compute follow-ups
below): the Schedule-8 spec line-numbering and the display-only subtotals.

**✅ RESOLVED 2026-07-06 — GA §179 = $2,500,000 / $4,000,000 (Ken-ruled: HB 1199 retroactive).** Georgia
CONFORMS to federal/OBBBA §179 for TY2025 via HB 1199 (IRC conformity advanced to Jan 1, 2026, retroactive to
tax years beginning on/after Jan 1, 2025), superseding both HB 290 (Jan 1, 2025) and the Aug-2025 GA Form 4562
(which still printed the pre-OBBBA $1.25M). Sources: BDO + Tax Foundation quoting HB 1199. Both stale figures
($1.05M GA700/CLAUDE.md = 2021; $1.25M GA600 = pre-HB-1199 Form 4562) corrected. Because GA limit AND phaseout
now equal federal, the GA §179 delta is structurally $0 — the only remaining GA depreciation diff is §168(k)
bonus. Applied: `depreciation_engine.GA_179_*`, `compute.GA700_SEC179_*`, `rules_ga700`, RS `load_ga700`/
`load_ga600`, CLAUDE.md, DECISIONS.md (`b975300` tts / `09d55d9` RS). SC ($1.25M/$3.13M) + NC ($25k/$200k) are
their OWN correct rules — untouched. ⚠ Still flagged separately: GA-600S compute's 0.0539 (TY2024) PTET rate —
possibly stale for its own TY2025.

**v1 DEFERRED (Partner model has no residency/GA-source field — needs a migration):** partner-level GA-source
allocation (Sch 4), nonresident 4% withholding (Form G-2-A, page-1 T), composite return (IT-CR), PTET owner-side
Form 500 subtraction. Surfaced by info diagnostics (D_GA700_NRW/_COMPOSITE), never silently computed.

*(Other unblocked SPINE items if Ken redirects: S-11 1041 module, S-5/S-6, S-3 brokerage ∥, S-13/S-14; plus the
S-4 f1065 render recalibration follow-on.)*

## This session's commits (pushed to origin/main)
- `0d59255` — GA-700 leg 3: `render_ga700_overlay` (AcroForm overlay) + `ga700_2025.py` field map + DOR
  template + 3 DB tests → **S-10 GA-700 COMPLETE**.
- `b975300` — **GA §179 conformity fix ($2.5M/$4M, HB 1199 retroactive)**: depreciation_engine + compute +
  rules_ga700 + seed + tests + DECISIONS.md. RS twin: `09d55d9` (load_ga700 + load_ga600). CLAUDE.md (parent)
  corrected separately.
- (Prior this session — `1d7f102` leg 1 compute · `6c26d72` leg 2 input · `f70a9d4` leg 4 diagnostics.)

## ▶ RS / compute follow-ups (rides a dedicated session — all non-blocking)
- **NEW (from leg 3) — GA-700 Schedule-8 spec line-numbering vs the form face.** The spec/compute treats income
  as L1 (ordinary) + L5 (guaranteed pmts) + **L6 ("other income")**, but the actual GA 700 form has line 6 =
  "Net gain (loss) under §1231" and line 7 = "Other Income (loss)". Render correctly maps `S8_6` → the form's
  **line-7 box (S8L7)**; the L8 total still ties. RS Sch-8 line map should be reconciled to the form face.
- **NEW (from leg 3) — display-only subtotals not computed** (render leaves them blank; map entries pre-wired to
  auto-render once produced): `S1_3` (Sch1 Total = L1+L2), `S8_10` (Sch8 Total = L8+L9), `S5_8` (Sch5 total),
  `S6_5` (Sch6 total). A small `FORMULAS_GA700` follow-up (spec-check + flow gate) closes these for full face
  fidelity. Also deferred in render: Schedule 4 partner detail (needs Partner residency/GA-source migration),
  continuation-page name/FEIN (duplicate AcroForm field names across pages).
- **NEW — RS reseed + export for the GA §179 fix.** `load_ga700.py` + `load_ga600.py` are amended to $2.5M/$4M
  (`09d55d9`) but NOT yet reseeded/exported to the deployed RS DB — so the cached tts mirrors still show the old
  figure: `server/specs/ga700_spec.json` (~22 refs), `ga600s_spec.json`, `form_4562_spec.json`. These mirrors are
  NON-operative (compute uses the Python constants, already fixed), but reseed+export from an RS session to refresh
  them. ⚠ **GA-600S (S-corp) has NO RS loader** (only `load_ga600.py` = C-corp) — its `ga600s_spec.json` mirror is
  an orphan that won't auto-regenerate; its running §179 comes from `depreciation_engine.GA_179_*` (already fixed).
- **GA §179 conflict RESOLVED** (see the RESUME section) — GA conforms $2.5M/$4M via HB 1199. **GA-700 spec is
  `draft`** (promote→active); check GA-600S's 0.0539 PTET rate for TY2025.
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
