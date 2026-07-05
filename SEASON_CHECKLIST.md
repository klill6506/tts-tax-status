# SEASON_CHECKLIST.md — Runway to Jan 2027, at a glance
*Adopted 2026-07-03. The tick-list companion to `SEASON_PLAN.md` (which stays the
authoritative narrative — dates, gates, and rationale). Items here mirror the plan;
scope and dates change only on Ken's explicit direction.*

## ▶ NOW WORKING ON
**Form 8835 unit — IN FLIGHT (2026-07-04 ninth session: scope walk done — Ken ruled J1
multi-facility; J2/J3/J4 no-preference → recommended options adopted-by-default; NO code
yet — build the four legs per STATUS.md's "Build facts established" list), then the S4
scenario.** Last completed: **Form 3800 unit (all four legs, same-session RS round-trip) +
Form 8936 unit (all four legs) — both 2026-07-04** (combined gate 451; D_8911_004 retired,
D_8936_003 softened; the S4 all-carries ordering Ken-blessed + pinned end-to-end).

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
  - [x] S2 Jones — scenario + mapper leg — 2026-07-03 `9b5dcad` (⚠ artifacts UNSIGNED,
        placeholder EFIN — Ken signs + uploads)
    - [x] Form 8283 unit (all four legs) — 2026-07-03 `1040-form-8283-complete`
    - [x] Return-level identity arms (deceased spouse, NRA election, IP PINs, divorced-spouse SSN, BinaryAttachment) — 2026-07-03 `86d4e38`
    - [x] EIC opt-out 27c (Ken ruled skip-entirely) — 2026-07-03 `c53e942`
    - [x] Ken's QBI-patron question resolved — 2026-07-03 (ruled: build 8995-A Schedule D)
    - [x] Scenario pins + mapper + NRA statement PDF — 2026-07-03 `2a151b5`/`9b5dcad` (all pins
          ALL MATCH; 11-doc submission live-XSD-valid; four NEW mappers: Sch A / 8283 Section A /
          preparer header grp / Sch C vehicle grp; test file 29/29; enacted 13a = 2,658.20 +
          ODC 500 diverge from the key's fiats — documented in README + module)
  - [ ] S3 — Form 4835 unit first, then scenario:
    - [x] Form 4835 unit — ALL FOUR LEGS GREEN (form ticks) — compute/diagnostics/seed
          2026-07-04 `c7cae44` (mig 0161); RENDER leg 2026-07-04 (field map + manifest +
          render_4835 + SKIP_PAGES; bijective 63-widget map; 412-passed gate)
    - [x] S3 scenario + mapper leg (Heather) — 2026-07-04: 4 NEW mappers (IRS1040ScheduleD
          1a/8a-aggregate-path / IRS1040ScheduleE Part-V / IRS1040ScheduleF / IRS4835) +
          the Sch D 1a/8a compute wire (spec R-SCHD-L7-L15; "UNUSED v1" retired) + SE farm
          optional method verified; scenario3 + command + test file; 11-doc submission
          live-XSD-valid, every pin ALL MATCH; combined DB gate 440. (⚠ artifacts UNSIGNED,
          placeholder EFIN — Ken signs + uploads. Key-forensics corrections: Sch F 2,970 =
          SEEDS line 26; PEC You ✔; Sch E A/B Yes ✔; line-38 IRS-figures-penalty election.)
  - [ ] S4 — 3800/8835/8936 units first, then scenario:
    - [x] Form 8936 unit (+ Schedule A) — ALL FOUR LEGS GREEN (form ticks) — 2026-07-04
          (migs 0162/0163; CleanVehicle per-vehicle model + two-phase compute — Sch 2 1b/1c
          repayment before the credit chain, Sch 3 6f/6m after 5695 — + 7 diagnostics +
          LABEL-VERIFIED f8936/f8936sa field maps + render_8936 + PNG visual pass + UI tab +
          FA-1040-8936-01..05; combined gate 422. OBBBA gate face-verified: acquired ≤
          9/30/2025; a dealer-TRANSFERRED credit never re-lands on the return — FACE-verified
          stop, RS spec amendment flagged. D_8936_003 interim ERROR while 3800 unbuilt.)
    - [ ] Form 8835 unit (spec cached; lands via the compute_3800 wiring point —
          `form_8835_credit() -> (amount, "1f"|"4e")`)
    - [x] Form 3800 unit — ALL FOUR LEGS GREEN (form ticks) — 2026-07-04 (mig 0164; the
          FULL same-session RS round-trip: 1040-side amend-by-lookup authored `5407bb2` +
          Ken's J1-J4 scope + W1-W4 review walks → seeded → export verified → tts built.
          §38(c)(1) Section A verbatim + §38(c)(4)(A) TMT-zeroed Section C → Sch 3 6a,
          computed LAST; passive tri-state gates (unanswered = excluded + RED; manual
          8582-CR escape hatches); carryforward statement page; subset field map over the
          9-page face + PNG pass; D_8911_004 RETIRED + D_8936_003 softened; combined gate
          451. The W4 S4-shape pin: the whole $13,200 carries, 6a blank.)
    - [ ] S4 scenario + mapper leg (⚠ reconcile the Transfer Election Statement attachment
          vs the draft face's 4a=No — a transferred credit never lands on Sch 3)
- [x] Brokerage summary-extraction skeleton (8949 Exception 2) — 2026-07-04 `c25635f`
      (mig 0160; import boundary + code-M auto + D_8949_006 confirm gate; 8/8; flow 381 held.
      OCR/AI front-end + frontend YELLOW deferred to Aug "extraction to production")

## July 2026 — Ken
- [ ] Sherpa Tax LLC formed + EIN + its IRS e-file application filed
- [ ] Bank calls (TPG first — lead with ~600 funded products)
- [ ] GA/SC/AL/NC DOR software-developer approval calls
- [ ] TaxWise as-filed-XML export ticket
- [ ] Answer the four open 1065 questions (`1065_status_assessment.md` Q1/Q2/Q4/Q5)
- [ ] A2A strong-auth certificate paperwork started (7–14 day lead)
- [ ] Business-family schema packages + ATS scenarios + business rules pulled (watch IRS inbox — unblocks 1120-S/7004 mappers)
- [ ] IFA-upload scenarios 8 + 5 → bring back acks (**rebuild both artifact sets first**);
      **NEW: S2 ready too** — sign (`--efin` + PINs) via `mef_build_ats_scenario2` and upload
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

## RULE STUDIO AUTHORING TRACK (added 2026-07-04)
*Ken rulings: (D-1) 1065 core specs are authored FRESH from IRS instructions/primary
sources, then the existing 35 compute formulas are reconciled against each spec —
never spec-from-code. Every mismatch found = Ken adjudication, logged. (D-2) 1041
Schedule I (AMT) is RED-DEFERRED for season one — loud diagnostic when a trust
shows AMT indicators; build later if needed.*

### July
- [ ] K-1 box 9c pass-through verification (in flight — active RS task)
- [ ] RS DB reconstructability check: fresh DB + all loaders + diff vs. production;
      document result in RS STATUS (if anything lives only in Supabase → fix now)
- [ ] Mirror sync extended to RS STATUS.md + session_log.md (Ken flips repo private)
- [ ] Begin draft→approved workflow: flip Ken-approved specs from draft status
- [ ] 1065 core, fresh-authored in order (spec → Ken walk → seed → reconcile compute):
  - [ ] Schedule K (aggregation spine)
  - [ ] Schedule K-1 + allocation engine rules (profit/loss/capital %, special allocations)
  - [ ] Schedules M-1 / M-2
  - [ ] Schedules L / B (light)
  - [ ] Form 8825 (1065)

### August
- [ ] SC1040 spec (CC drafts, Ken walks — GA-500 pattern)
- [ ] AL Form 40 spec (⚠ federal income tax deduction quirk — budget Ken's longest walk)
- [ ] NC D-400 spec
- [ ] GA-700 + PTET spec
- [ ] 1120-S delta audit: MEASURE Feb–Mar specs vs. campaign standard (verbatim
      excerpts / scenario counts / FA coverage), report to Ken; upgrade only what he picks

### September
- [ ] 1041 spine (entity types, rate schedule, exemptions)
- [ ] DNI / IDD / Schedule B (the heart — Ken's specialty walk)
- [ ] Schedule G · K-1 (1041) distribution ordering + character pass-through · GA 501
- [ ] Schedule I: RED-defer diagnostic wired per D-2 (no compute build)

### Oct–Nov
- [ ] TY2026 constants wave: every _constants_for_year addition gets a spec-side pin
- [ ] CASELAW_SE_LP seasonal re-verify (Soroban / Denham appeal status)

---

## ⚠ Unplanned work log (added per the never-silent rule)
- ⚠ Season checklist + status-mirror protocol adopted — 2026-07-03
- ⚠ 4797 K-1 box 9c pass-through test (parallel session's `cca5e34`) — full-file green-run
  gate verified and cleared — 2026-07-03
- ⚠ **8995-A Schedule D patron reduction + capped DPAD unit** (Ken ruled build-it for the S2
  MeF leg) — 2026-07-03 `f10e864` + `2c6e8a1` (RS `9b4490c`, mig 0158). All PURE gates green
  (math 17/17, render 7/7, efile 31/31 w/ live XSD, flow 381).
  - [x] **DB legs GREEN + prod seeds done — gate CLEARED** — 2026-07-03 `c10e51c` (fourth
    session: 66→50→40 across three pooler-fought runs, every test green on current code;
    4 seeder/test-side fixes `89a4f88` — the SD FormLines were never in the seeder's SECTIONS
    list; `seed_rules` + `seed_8995a` (58 lines / 8 sections) applied to the shared DB).
