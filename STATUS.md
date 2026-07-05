# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-04, ninth session (**Form 8835 unit IN FLIGHT — scope walk ALL FOUR
RULED + model leg DONE (migs 0165/0166 APPLIED TO THE SHARED DB) + compute leg WRITTEN BUT
UNTESTED — session paused at Ken's limit right before the first pure-test run**). The
3800/8936 state from the eighth session is unchanged (combined gate 451).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gate
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q`
  (**HELD**: 369 assertions / 391 gate tests; combined with the 3800 + 8936 + 8911 leg
  files = **451 passed**, reuse-db 7:04 — unchanged from the eighth session).
- Test DB `test_postgres` CURRENT (migs through **0164**); shared (prod) DB seeded through
  seed_3800/seed_8936/seed_rules. RS: 3800 amendment seeded (`5407bb2`).

## ▶ RESUME HERE — Form 8835 unit, MID-BUILD (next action: run the pure compute tests)
**EXACT NEXT COMMAND** (paused right before it; venv python direct — poetry run hangs):
```
cd server
C:\Users\Ken\AppData\Local\Packages\Claude_pzs8sxrjxfjjc\LocalCache\Local\pypoetry\Cache\virtualenvs\tts-tax-server-oXVY_DSk-py3.13\Scripts\python.exe -m pytest tests/test_form8835_compute_leg.py -q
```

**DONE this session (committed `d2bf28e`):**
- **Scope walk — ALL FOUR RULED by Ken** (re-asked after the no-preference round):
  J1 multi-facility list model · J2 mixed 1f/4e → RED-defer (D_8835_005; escape hatch =
  the 3800 1zz/4z direct-entry facts) · J3 straddle (window END mid-year) → RED-defer
  (D_8835_006; rule: whole-year-inside → 4e, ended-before → 1f, PIS-in-year → 4e) ·
  J4 passthrough routing → nullable `Taxpayer.f8835_passthrough_pis_date` (unanswered +
  amount → RED D_8835_007, feed withheld). Also mine: missing PIS on a producing facility
  → D_8835_008 withheld; line 16 patron alloc UNMODELED (1040 never). REVIEW_QUEUE's
  open J2/J3/J4 item is now RESOLVED — move it on the next touch.
- **Model leg DONE**: `RenewableFacility` (models.py, after CleanVehicle; fields 1:1 spec
  fact keys) + Taxpayer `f8835_passthrough_credit`/`f8835_passthrough_pis_date`; migs
  **0165 + RLS 0166 — APPLIED TO THE SHARED (PROD) DB** ✅; serializers
  (RenewableFacilitySerializer + Taxpayer exposure + nested `renewable_facilities` on the
  return payload); views CRUD `renewable-facilities(/<id>)` with `_recompute_1040` (the
  chokepoint rule). `manage.py check` clean. ⚠ **test DB NOT yet migrated through
  0165/0166** (use the scratchpad migrate_test_db.py NAME-override trick).
- **Compute leg WRITTEN, ⚠ NOT YET RUN**: `apps/returns/compute_8835.py` — year-keyed
  RATE table (2025: .006/.003 tiers, .006 hydro/marine post-2022, 3.0¢/1.5¢ pre-2022;
  D_8835_RATE_YEAR when unpinned) · OBBBA gate `facility_obbba_ok` (blank construction
  date proven by PIS < cutoff — the 8936 convention) · `route_for_window` (4e/1f/straddle/
  unknown) · `facility_lines` Part II chain (2→13 per spec formulas, ×5, +10%/+10%, EPE
  90%, bond 15% cap) · `form_8835_state` (THE shared gatherer: components, mixed/straddle/
  unrouteable flags, feed_amount/feed_route) · `form_8835_credit` (the PINNED 3800 wiring
  tuple) · `form_8835_face_value` · `compute_8835_db` (FFV aggregate rows "2".."15",
  disengage rule) — wired in compute.py immediately BEFORE the compute_3800_db block.
- **Pure tests WRITTEN, ⚠ NOT YET RUN**: `tests/test_form8835_compute_leg.py` — constants
  pin, OBBBA gate, all rate tiers, routing edges (S4 4e / T4 1f / straddle / PIS-in-year),
  spec vectors S4($13,200/4e)/T2/T3/T5, reduction chain (bond cap binds), EPE 90%.

**REMAINING legs (tasks #3-#7 in the harness task list):**
1. Run the pure tests (above) → fix → then the DB pipeline test file
   (`test_form8835_pipeline.py` — the form3800_pipeline pattern: engage, FFV rows, the
   3800 1f/4e feed incl. the S4 all-carries shape, disengage; migrate the test DB first).
2. Diagnostics + seed: `rules_8835.py` (D_8835_001/002/003/004/RATE_YEAR per spec +
   NEW 005 mixed / 006 straddle / 007 passthrough-unrouted / 008 missing-PIS — all ≤20
   chars, bridge-gated on `form_8835_state`) + `seed_8835` (FORM_8835 FormDefinition +
   the "2".."15" lines) + seed_rules; run on SHARED + test DBs. ⚠ D_8835_005-008 need RS
   loader homes (amend-by-lookup; flag if not done this unit — the 8936 R-8936-TRANSFER
   precedent).
3. Render leg: **f8835.pdf NOT downloaded** — manifest entry + update_irs_forms.py +
   sha256 vs irs.gov + LABEL-VERIFY bijective map + render_8835 (ONE FACE PER FACILITY,
   model-driven via facility_lines — the render_4835 pattern; header = filer name/SSN) +
   PNG visual pass + packet emit + SKIP_PAGES if instruction pages.
4. UI: RenewableFacilitiesSection tab (CleanVehiclesSection pattern) + the two Taxpayer
   passthrough inputs + D_8835_ → form_8835 routing; tsc clean.
5. FA leg: FA-1040-8835-01.. in tts gate file + RS loader homes SAME unit; combined flow
   gate; tag `1040-form-8835-complete`; then the S4 scenario + mapper leg.

**Build facts established this session (verified in code/spec — trust these):**
1. Spec cached + rich: `server/specs/8835_spec.json` (7 rules / 38 lines / 5 diags / 5
   tests incl. the S4 vector: solar 440,000 kWh × $0.006 ×5 [8a/8b/8c all Yes] = $13,200,
   PIS 9/22/2023 → route **4e**) + `form_8835_authoring_notes.md`. Spec metadata status
   "draft" — same posture the 8936 unit built from.
2. Wiring point PINNED: `compute_3800.form_3800_inflows` try-imports
   `compute_8835.form_8835_credit(tax_return) -> (amount, "1f"|"4e")` (single tuple; the
   FA checks only that "compute_8835" appears in the inflow source). Passive assertion
   `Taxpayer.f3800_ps_8835` already exists (mig 0164).
3. Sequencing: `compute_8835_db` must run in compute.py BEFORE the `compute_3800_db` block
   (compute.py ~line 2640); 8835 itself lands NOTHING on Sch 3 (all via 3800).
4. Rate is DERIVED, not preparer-entered: year-keyed RATE table (2025: $0.006 tier-1
   wind/CLB/geo/solar/offshore-wind post-2021; $0.003 open-loop/landfill/trash/hydro/
   marine; $0.006 hydro/marine PIS post-2022; 3.0¢/1.5¢ pre-2022) + D_8835_RATE_YEAR
   error when tax_year unpinned (the Sch F 2026 precedent). The face PRE-PRINTS the rates.
5. Model fields = spec fact keys (f8835_registration_no, resource_type, owner name/TIN,
   address, lat/long, construction_begin_date [OBBBA gate — D_8835_001 if ≥ 2025-01-01],
   placed_in_service_date, expansion_existing, qf_req_lt_1mw/before_1_29_2023/pwa/none,
   domestic_content, energy_community, nameplate dc/ac, kwh_produced, phaseout_adjustment,
   taxexempt_bond_fraction, wind_phasedown_l7g, epe_2024_nonconforming) + Taxpayer
   `f8835_passthrough_credit` + `f8835_passthrough_pis_date` (J4). Chain: 2=kWh×rate;
   4=2−3; 5d=min(4×bond, 4×15%); 6=4−5d; 8=6−7g; 9=8×5 if (8a|8b|8c); 10/11=9×10% each;
   12=9+10+11; 13=12×90% if EPE-2024 else 12; 15=13+14. 8c=Yes → D_8835_003 (Form 7220
   attach, warning). D_8835_002 warning, D_8835_004 info per spec.
6. **f8835.pdf template NOT downloaded yet** (no `resources/irs_forms/2025/f8835*.pdf`) —
   render leg starts with manifest entry + `scripts/update_irs_forms.py` + sha256 vs a
   fresh irs.gov download, then LABEL-VERIFY bijective map (the 4835 recipe).
7. FA leg: author FA-1040-8835-* in the tts gate file AND give them RS loader homes same
   unit (the [[fa-needs-rs-loader-home]] rule; the 8936 FAs drifted last unit — don't repeat).
8. Task list (harness) #2-#7 carry the leg breakdown: model/mig → compute →
   diagnostics+seed → render → UI tab → FA+close-out.

**Then:** the S4 scenario + mapper leg (⚠ reconcile the Transfer Election Statement
attachment vs the draft face's 4a=No — a transferred credit never lands on Sch 3; the
blessed shape: 8936 personal absorbs ~$2,193 tax → 3800 line 38 = 0, Sch 3 6a blank, the
whole $13,200 carries + carryforward statement). New MeF mappers: IRS8835 / IRS8936
(+SchA doc) / IRS3800 — 1:1 vs the 2025v5.4 XSDs.

**RS follow-ups (carried from eighth session, small):**
- FA-1040-8911-04's RS-side description still says "Form 3800 unbuilt" — refresh the text.
- RS 8936 spec: add the FACE-verified transfer stop (R-8936-TRANSFER) + D_8936_004
  wrong-year-PIS note. Both in DEFERRAL_AUDIT (eighth session, parts 1-2).

**Waiting on Ken (carried):**
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5); **S2 + S3
   ready** — sign via `mef_build_ats_scenario2/3 --efin … --practitioner-pin … --taxpayer-pin …`.
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
4. Preparer Manager visual-review batch (carried): Sch A line-11 dotted literal + qualified-
   contributions input; Clean Vehicles (8936) + Business Credit (3800) tabs + rendered
   8936/SchA/3800 faces + the 3800 carryforward statement.

## ▶ Carryover follow-up (older, still open)
- Next RS Schedule D touch — (a) add `D_8949_006` to the 8949 spec; (b) consider an FA for
  the 1a/8a aggregate netting.
- RS SCHA spec fact for `scha_qualified_contributions_cash` (input-only) — amend by lookup.
- RS 8995 sibling loader's D_8995_001 retirement note (DEFERRAL_AUDIT third-session item 2).
- 1065 beyond the SE unit resumes only on Ken's explicit direction — `1065_status_assessment.md`.
- Minor parked sweep: 1065-section selects non-key uncontrolled; legacy `boxCheckbox` (pre-existing).
- EIN/ZIP re-import stays PARKED.

## Authoritative files read at boot
- `SPRINT_SCOPE.md` · `SEASON_PLAN.md` (dated Jan-2027 runway) · `SEASON_CHECKLIST.md` (tick-list) · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` · `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md` (when RS work on deck).
