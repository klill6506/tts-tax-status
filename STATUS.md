# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-05, thirteenth session (**MeF/diagnostics follow-up sweep** — knocked
down two tracked latent bugs Ken picked, both e-file/gate hardening, no tax-law change):*
1. *Sch 3 lines **6z + 13z** MeF silent-drop (`5d055e7`).*
2. *Form 8867 **AOTC gate/cascade lock-step** (`8783487`).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gates (all green)
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q`
  → **398 passed** (unchanged this session — neither fix touches compute/flow). Fast (~1.3s, pure).
- **Pure MeF 1040 suite** — `pytest tests/test_efile_mef_1040.py -q` → **40 passed** (+7 this session:
  the Sch 3 6z/13z group tests incl. live 2025v5.4 XSD validation of both groups).
- **AOTC 8867 lock-step** — `pytest tests/test_8867_aotc_lockstep.py -q --reuse-db` → **7 passed**
  (~5 min DB). Regression bed: `test_leg_c_8867_cascade.py` + `test_print_gate.py` +
  `test_credit_gates.py` → **51 passed** (~11 min DB) — no regression.
- `manage.py check` clean. Test DB `test_postgres` still needs **mig 0167** on its next fresh build
  (help_text-only; applies on the next migrate).

## ▶ RESUME HERE — idle; Ken directs the next unit
This session (thirteenth) did the **MeF/diagnostics follow-up sweep** — two latent bugs, both fixed
spec-faithfully with no compute/tax-law change:

**(1) Sch 3 6z/13z silent-drop (`5d055e7`).** `SCH3_LINE_ORDER` omitted lines 6z + 13z on the FALSE
premise (in the builder comment + DEFERRAL_AUDIT) that "the app stores no 6z type text." It stores
BOTH `6z_type` + `13z_type` (seeded; `D_SCH3_001` requires them; `_form_scoped_lines` carries them).
line 7 (6z→7) / line 14 (13z→14) summed the amounts, so a nonzero 6z/13z dropped from the XML while
the total stayed right. `build_schedule3` now emits `OtherNonrefundableCreditsGrp` +
`TotOthNonrefundableCreditsAmt` (6z) and `OtherRefundableCreditsGrp` (13z `<choice>`: enumerated code
`960(c)`/`FORM 8689`/`1062NL` else free text) + `TotalOtherRefundableCreditsAmt` (13z). Amount with no
type literal → hard `UnmappableValue`. No ATS scenario populates 6z/13z → existing scenarios unaffected.

**(2) Form 8867 AOTC gate/cascade lock-step (`8783487`).** The DD print gate
(`rules_eic._covered_credits`) and the attestation cascade (`compute_eic._cascade_claims`) defined AOTC
differently: gate = "8867 line 13 already answered" (CIRCULAR — reads the cascade's own output, so a
real AOTC claim with a blank line 13 escaped DD); cascade = "1040 line 29 > 0 OR any EducationStudent"
(misses a kiddie-tax-lockout AOTC → line 29 = 0; over-counts LLC-only). Both now call one shared
predicate **`compute_8863.aotc_claimed` = Form 8863 line 7 > 0**. Matches RS 8867 spec intent
(R-8867-RENDER). ⚠️ RS spec text is STALE (`D_8867_002` "RED until 8863 built"; 8863 IS built, D_8867_002
retired) → notes-only RS refresh flagged in DEFERRAL_AUDIT for the next 8867 touch.

Per `SEASON_PLAN.md`, remaining runnable/near-term CC items (unchanged): **4868 mapper** BLOCKED (no
TY2025 4868 schema on disk — SOR pull); **1120-S/7004 mappers** BLOCKED (business-family schema pull —
Ken's IRS inbox); **TaxWise season-one 1040 importer** = October. Unblocked build candidate not yet
started: the **GA-500 OT/tips exclusion** unit (HB 463 §48-7-27(a)(16)/(17) — a TY2026 hourly-worker GA
return currently taxes income GA now excludes; REVIEW_QUEUE Open + DEFERRAL_AUDIT).

Ask Ken which to pick up. The whole 1040 ATS scenario set is built (S2/S3/S4/S5/S8/S12/S13; S1 dropped).

## ▶ Waiting on Ken (carried)
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5). **S2 + S3 + S4
   ready** (UNSIGNED, placeholder EFIN) — sign via
   `mef_build_ats_scenario2/3/4 --efin … --practitioner-pin … --taxpayer-pin …`.
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · **TY2025 4868 schema** (unblocks the
   4868 mapper — currently only TY2026 4868 is on disk).
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
4. Preparer Manager visual-review batch (carried): Sch A line-11 dotted literal + qualified-
   contributions input; Clean Vehicles (8936) + Business Credit (3800) + Renewable Electricity (8835)
   tabs + the rendered 8936/SchA/3800/8835 faces + the 3800 carryforward statement.

## ▶ RS follow-ups (rides a dedicated RS session — parallel RS work may be uncommitted)
- **NEW this session:** `8867_spec.json` — R-8867-RENDER + `f8867_claims_aotc` notes + `D_8867_002` say
  "Form 8863 not built → AOTC RED"; STALE (8863 built, D_8867_002 retired). Refresh text to "AOTC derived
  from Form 8863 (line 7 > 0)" + mark D_8867_002 retired. Notes-only, no tax-law change.
- Carried: RS Schedule D — add `D_8949_006` to the 8949 spec; consider an FA for the 1a/8a aggregate
  netting. RS SCHA spec fact for `scha_qualified_contributions_cash` (input-only; amend by lookup).
  RS 8995 sibling loader's D_8995_001 retirement note. (The six prior handoff items were applied by the
  RS session 2026-07-04 — see the archive.)

## ▶ Carryover follow-up (older, still open)
- 1065 beyond the SE unit resumes only on Ken's explicit direction — `1065_status_assessment.md`.
- Minor parked sweep: 1065-section selects non-key uncontrolled; legacy `boxCheckbox` (pre-existing).
- EIN/ZIP re-import stays PARKED.
- **Proforma v1 follow-ups** (flagged, non-blocking): per-activity PAL roll (v1 is aggregate);
  `_sr_py_prior_refunded`/`_sr_py_unused_credits` sourcing; the TaxWise season-one 1040 importer (Oct).

## Authoritative files read at boot
- `SPRINT_SCOPE.md` · `SEASON_PLAN.md` (dated Jan-2027 runway) · `SEASON_CHECKLIST.md` (tick-list) · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` · `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md` (when RS work on deck).
