# SEASON_CHECKLIST.md — Runway to Jan 2027, at a glance
*Adopted 2026-07-03. The tick-list companion to `SEASON_PLAN.md` (which stays the
authoritative narrative — dates, gates, and rationale). Items here mirror the plan;
scope and dates change only on Ken's explicit direction.*

## ▶ NOW WORKING ON
**S2 (Jones) scenario + mapper leg** — first action: Ken's QBI-patron question
(see STATUS.md ▶ RESUME HERE).

## How to update (every session close — MANDATORY, no exceptions)
- Tick every item completed this session: `- [x] item — YYYY-MM-DD `SHA``
- Update the ▶ NOW WORKING ON line (next unit, or "idle — Ken directs").
- Any work done that wasn't on the list gets **ADDED as a ⚠ item** — never silent.
- A **form** ticks only when ALL its legs are green in `form_coverage_tracker.md`.
- ⛔ **Gates** tick only on Ken's call; a missed gate is noted in `SEASON_PLAN.md` with the date.

> **Baseline note:** everything completed before 2026-07-03 adoption (1040 federal core,
> GA 500 + HB 463, ~40 form units, flow gate 381) lives in `form_coverage_tracker.md` —
> this checklist starts from the SEASON_PLAN runway, with July items backfilled.

---

## July 2026 — CC
- [ ] 1120-S XML mapper (**blocked**: business-family schema pull — Ken's IRS inbox)
- [ ] 7004 extension mapper (own SW ID + own 4 scenarios; **blocked** same as above)
- [ ] 4868 extension mapper (runnable now — TY2025 schema package needed from SOR)
- [ ] Proforma/rollover — snapshot PRODUCER (read/roll side already built, migs 0105/0106)
- 1040 ATS scenarios (smallest-first; **S1 DROPPED** 2026-07-02 — 0 Schedule H filed firm-wide):
  - [x] S5 refundable-credit stack — 2026-07-02
  - [x] S8 signed (EFIN on file) — 2026-07-02 — ⚠ rebuild artifacts (pre-§12.5 filer-name fix) before upload
  - [x] S12 Gardenia Sch C family (7217 + 6 doc mappers, 43 pins match) — 2026-07-02
  - [x] S13 §30C (8911) — 2026-07-02
  - [ ] S2 Jones — scenario + mapper leg
    - [x] Form 8283 unit (all four legs) — 2026-07-03 `1040-form-8283-complete`
    - [x] Return-level identity arms (deceased spouse, NRA election, IP PINs, divorced-spouse SSN, BinaryAttachment) — 2026-07-03 `86d4e38`
    - [x] EIC opt-out 27c (Ken ruled skip-entirely) — 2026-07-03 `c53e942`
    - [ ] Ken's QBI-patron question resolved
    - [ ] Scenario pins + mapper + NRA statement PDF
  - [ ] S3 — Form 4835 unit first, then scenario
  - [ ] S4 — 3800/8835/8936 units first, then scenario (⚠ OBBBA-sunset check at each spec leg)
- [ ] Brokerage summary-extraction skeleton (8949 Exception 2)

## July 2026 — Ken
- [ ] Sherpa Tax LLC formed + EIN + its IRS e-file application filed
- [ ] Bank calls (TPG first — lead with ~600 funded products)
- [ ] GA/SC/AL/NC DOR software-developer approval calls
- [ ] TaxWise as-filed-XML export ticket
- [ ] Answer the four open 1065 questions (`1065_status_assessment.md` Q1/Q2/Q4/Q5)
- [ ] A2A strong-auth certificate paperwork started (7–14 day lead)
- [ ] Business-family schema packages + ATS scenarios + business rules pulled (watch IRS inbox — unblocks 1120-S/7004 mappers)
- [ ] IFA-upload scenarios 8 + 5 → bring back acks (**rebuild both artifact sets first**)
- [ ] TY2025 1040 business rules from SOR (PDF + CSV)
- [x] TY2025 software IDs — all six products verified (`docs/mef/software_ids.md`) — 2026-07-02

## August 2026 — CC
- [ ] 1065 Stages 3–6 through four-leg verification
- [ ] GA-700 (form ticks per tracker)
- [ ] GA PTET
- [ ] Business ATS scenarios passing: 1120-S ×4 · 1065 ×4 · 7004 ×4 (via IFA until A2A)
- [ ] A2A build (ASID registration, cert, SOAP client)
- [ ] ⛔ **GATE Aug 31:** one-time A2A comm test passed
- [ ] SC individual return build
- [ ] SC entity return build
- [ ] AL individual return build
- [ ] AL entity return build
- [ ] NC individual return build
- [ ] NC entity return build
  *(entity-state tail may spill into Sept–Oct per plan)*
- [ ] Document extraction to production (W-2, 1099-R, SSA-1099, 1099-INT/DIV, 1099-B summary → YELLOW + preparer confirmation)

## August 2026 — Ken
- [ ] Regression bed assembled — 40–50 real TY2025 returns by archetype
- [ ] State authoring reviews

## September 2026 — CC
- [ ] 1041 build (bulk)
- [ ] Bank integration (fee-collect products; reopens `RefundProductCd` default, DECISIONS MeF 6(e))
- [ ] Diagnostics/review layer + workflow states (prep → diagnostics-clear → transmit)
- [ ] E-file PIN / signature-data capture (rides workflow states; wet signatures don't defer it)

## September 2026 — Ken
- [ ] 1041 Rule Studio authoring (DNI, IDD, Sch B, beneficiary K-1s, GA 501)
- [ ] ⛔ **GATE Sept 15:** bank agreement signed, or desk-collect fallback locked in

## October 2026 — CC
- [ ] 1041 finish + its 4 ATS scenarios
- [ ] TaxWise → Sherpa migration (~2,600 clients)
- [ ] Printing at volume
- [ ] 9-preparer concurrency testing

## October 2026 — Ken
- [ ] Regression adjudication — every Sherpa-vs-as-filed mismatch ruled on
- [ ] ⛔ **GATE Oct 1:** GA DOR approval path confirmed with dates AND entity decision made (LLC vs TTS credentials)

## November 2026
- [ ] TY2026 re-cut — schemas from SOR, form updates, `_constants_for_year()` + prior-year regression tests
- [ ] TY2026 software IDs created under the chosen entity
- [ ] Formal ATS re-test (1040 + business families)
- [ ] State testing
- [ ] Ken: ATS/state test submissions; dress-rehearsal return selection
- [ ] ⛔ **GATE Nov 1:** regression bed ≥95% tying to the dollar; A2A healthy

## December 2026
- [ ] ⛔ **FEATURE FREEZE Dec 1** (bug-fix only)
- [ ] Staff training
- [ ] Dress rehearsal — preparers re-do real TY2025 returns in Sherpa vs as-filed
- [ ] ⛔ **GATE Jan 2, 2027:** go/no-go, module by module

---

## ⚠ Unplanned work log (added per the never-silent rule)
- ⚠ Season checklist + status-mirror protocol adopted — 2026-07-03
