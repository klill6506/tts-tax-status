# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-04, ninth session (**Form 8835 unit KICKOFF — boot + spec analysis +
scope walk only; NO code written, context filled early**). The 3800/8936 state from the
eighth session is unchanged (combined gate 451, migs through 0164).*

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

## ▶ RESUME HERE — Form 8835 unit (scope walk DONE, build the four legs)
**Scope walk result (AskUserQuestion, this session):**
- **J1 RULED (Ken): multi-facility list model** — one `RenewableFacility` row per facility
  (CleanVehicle/Form4835 pattern), one rendered Form 8835 face per facility, FORM_8835 FFV
  rows persist the return-level aggregate. Line 16 (patron/beneficiary alloc) UNMODELED —
  coops/estates/trusts only, never a 1040 (stated boundary).
- **J2/J3/J4 answered "[No preference]"** → proceed on the recommended options, but they are
  ADOPTED-BY-DEFAULT, not Ken-ruled — surface them in the review walk before seeding any RS
  amendment / at the first natural checkpoint:
  - J2 mixed 1f/4e routing → **RED-defer** (new D_8835_005 error; escape hatch = the 3800
    direct-entry facts `f3800_other_credits_1zz` / `f3800_other_specified_4z`).
  - J3 straddle year (4-yr window END falls inside the tax year) → **RED-defer** (same
    escape hatch). Clean rule: whole year inside [PIS, PIS+4yr) → 4e; window ended before
    the year → 1f; PIS in-year → 4e (no pre-PIS production possible); window end mid-year
    → straddle RED.
  - J4 passthrough-only routing → **nullable `Taxpayer.f8835_passthrough_pis_date`** (the
    K-1 facility's PIS; one routing rule everywhere = spec T4's own shape); passthrough > 0
    with the date unanswered → RED, feed withheld (tri-state convention).

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
