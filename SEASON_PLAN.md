# SEASON_PLAN.md — Runway to January 2027 (adopted 2026-07-02)

*The authoritative dated plan for running the Jan 2027 filing season (TY2026 returns) in-house
on Sherpa. Adopted by Ken 2026-07-02 in the planning project. **This file supersedes the
"RUNWAY TO JANUARY 2027" section of `SPRINT_SCOPE.md`** — update that section to point here.*

***CANONICAL HOME: the `tts-tax-status` repo** — the shared brain both CC contexts (tax-app
AND rule-studio) pull at boot. BUILD_ORDER = order; SEASON_PLAN = fixed points/gates/risk;
PRODUCT_MAP = scope. All three live here canonically.*

## How this file works
- **Durable plan, not current state.** Resume pointers and in-flight work stay in `STATUS.md`.
- CC sessions doing planning or sequencing read this file at boot alongside the other
  authoritative files. Add it to the boot list in `STATUS.md`.
- Dates and gates change only on Ken's explicit direction. When a gate is passed or missed,
  note it here with the date.

---

> **⚡ SCOPE CHANGE — Ken, 2026-07-09 (explicit direction, recorded per the rule above):**
> **Mission = finish 1040, 1120-S, 1120, 1065, 1041, AND 709 by the end of 2026.** This
> ADDS the 1120 C-corp (superseding locked decision #2 below, which deferred it to summer
> 2027) and ADDS Form 709 gift tax (never previously in scope; **verified 2026-07-09: 709
> is MeF-e-fileable — the IRS opened the 709 family on MeF July 14, 2025 for authorized
> e-file providers**, so it gets the full engine+MeF treatment, not print-only). Also
> Ken-ruled same day: **no piecemeal ATS testing** — finish ALL work required for the full
> 1120-S ATS scenario set first, then run the upload loop (the S5-only upload is OFF).
> IRS e-services approvals for the business families (1120-S/1065/1120/1041) were
> Ken-verified 2026-07-09. **The month-by-month runway below is now STALE for 1120/709 —
> re-cut it in a dedicated planning session with Ken** (open inputs: RS authoring lane for
> 1120/709 specs; whether the 1120-S ATS set requires S7/S8 under declared-forms scope).

## Definition of "finished" for season one
Sherpa prepares and e-files the returns The Tax Shelter actually produces, at TaxWise/Lacerte
quality, for the Jan 2027 season: **1040, 1120-S, 1065, 1041 — federal + GA, SC, AL, NC** —
prepared, transmitted via A2A, and **accepted**, with bank fee-collect products attached where
clients want them. *(2026-07-09: the mission above extends the build target to 1120 + 709 by
year-end; season-one filing scope vs build scope to be reconciled in the re-cut.)*

**Prime directive: tax law accuracy.** Correctness over speed, always. Spec-first (Rule Studio)
for all tax law. Four-leg verification (Input → Compute → Render → Flow-assertion) before merge.
No silent defaults. The regression bed (real TY2025 returns tied to the dollar) is the proof gate.

## Locked scope decisions
1. **States built:** GA, SC, AL, NC (~2,440 of ~2,490 state returns). The other ~23 states
   (~50 returns) go to pay-per-return software — state software-developer approvals cannot be
   obtained mid-season, so no mid-February state builds.
2. **C-corps (1120): not built for season one.** ~5 returns → PPR. Build summer 2027.
3. **1041: built** (Sept–Oct). ~70 returns, Ken's specialty, April 15 deadline gives slack.
4. **Brokerage statements: summary-level extraction only** (8949 Exception 2 — category totals
   + broker PDF attached to the e-file). Lot-level import is season two.
5. **Document import scope:** W-2, 1099-R, SSA-1099, 1099-INT/DIV, 1099-B summary → extracted
   into YELLOW (imported-provenance) fields with mandatory preparer confirmation.
6. **E-signatures (8879/GA-8453): back-burnered.** Wet signatures season one.
7. **Entity:** Sherpa Tax LLC formed July 2026; IP assigned in; its own IRS e-file application
   filed immediately. The Tax Shelter's existing credentials stay untouched as the parachute.
   Which entity's credentials run the season is decided at the Oct 1 gate.
8. **PPR parachute** (Drake or similar) covers: C-corps, odd states, and any module that fails
   its Jan 2 go/no-go.

## Hard external dates (not movable)
- **~Nov 2026** — TY2026 schemas/scenarios publish; TY2026 software IDs must be created
  (year-encoded; all current IDs are TY2025-only).
- **Jan 2027** — season opens. **Mar 15, 2027** — 1065/1120-S deadline. **Apr 15, 2027** — 1040/1041.
- Business ATS (PY2026, TY2025 scenarios) is **open now** — first-pass business testing happens
  Aug–Oct; November is a re-test, not a maiden voyage.
- Strong-auth certificate for A2A: 7–14 day issuance lead time.

---

## Month-by-month

### July 2026
**CC:** 1120-S XML mapper (hardest-first); 7004 + 4868 extension mappers (own scenarios + own
software IDs — IDs already created; 7004 tests ONCE for all business families); proforma/rollover
engine; finish remaining 1040 ATS scenarios via IFA; brokerage summary-extraction skeleton.
**Ken:** LLC formed + EIN + e-file application filed; bank calls (TPG first — lead with ~600
funded products); GA/SC/AL/NC DOR software-developer approval calls; TaxWise as-filed-XML
export ticket; answer the four open 1065 questions (`1065_status_assessment.md` Q1/Q2/Q4/Q5);
start A2A strong-auth certificate paperwork.

### August 2026
**CC:** 1065 Stages 3–6 through four-leg; **begin passing business ATS scenarios** (1120-S ×4,
1065 ×4, 7004 ×4 — via IFA until A2A lands); A2A build (ASID registration, cert, SOAP client)
+ **one-time comm test by Aug 31**; SC/AL/NC state builds; doc extraction to production.
**Ken:** assemble regression bed — 40–50 real TY2025 returns by archetype (EIC families,
TRS/OPM retirees, SS+dividends, investors, Sch C); state authoring reviews.

### September 2026
**CC:** 1041 build; bank integration (whichever bank signed); diagnostics/review layer +
workflow states (prep → diagnostics-clear → transmit).
**Ken:** 1041 Rule Studio authoring (DNI, IDD, Sch B, beneficiary K-1s, GA 501).
**⛔ GATE Sept 15:** bank agreement signed, or the desk-collect fallback is locked in.

### October 2026
**CC:** 1041 finish + its 4 ATS scenarios; TaxWise→Sherpa migration (~2,600 clients); printing
at volume; 9-preparer concurrency testing.
**Ken:** regression adjudication — every Sherpa-vs-as-filed mismatch ruled on by Ken.
**⛔ GATE Oct 1:** GA DOR approval path confirmed with dates (single point of failure), and
the entity decision made (LLC vs. TTS credentials for the season).

### November 2026
**CC:** TY2026 re-cut — schemas from SOR, form updates, `_constants_for_year()` additions with
prior-year regression tests; TY2026 software IDs under the chosen entity; formal ATS re-test
(1040 + business families); state testing.
**Ken:** ATS/state test submissions; dress-rehearsal return selection.
**⛔ GATE Nov 1:** regression bed ≥95% tying to the dollar (prime-directive gate); A2A healthy.

### December 2026
**⛔ FEATURE FREEZE Dec 1.** Bug-fix only. Staff training; dress rehearsal — preparers re-do
real TY2025 returns in Sherpa vs. as-filed.
**⛔ GATE Jan 2, 2027:** go/no-go, module by module. Each module earns its own ticket.

---

## Risk register — season-killers

| Risk | Early warning | Fallback |
|---|---|---|
| GA DOR approval fails/stalls (2,400 returns — single point of failure) | No confirmed path Oct 1 | Full TaxWise season |
| Regression bed won't tie | <95% by Nov 1 | Prime-directive gate — whatever can't prove itself doesn't ship |
| A2A not passing | Comm test failing Aug 31; recheck Nov 1 | IFA can't carry 3,000 returns — gates volume |
| Business ATS not green | Not passed by Dec 1 | Entities to PPR; 1040s proceed |
| No bank signed | Nothing by Sept 15 | Desk-collect fees; ~50 advance clients to PPR |
| LLC e-file application stalls | Not approved by Oct 1 | Season runs on TTS credentials; migrate spring 2027 |

## Consciously deferred to season two
1120-C build · lot-level brokerage import · e-signatures · multi-state beyond GA/SC/AL/NC ·
sherpa-1099 expansion · AI-prepared returns as a production feature · React form viewer ·
990/706/709 (out of scope entirely).

## Notes for CC sessions
- Business ATS synthetic EINs must use the "00" prefix in per-family ranges (Pub 5078 Exhibit 4:
  1065 = 00-2000000–00-2999999, 1041 = 00-4000000–00-4999999, etc.) — Business Rule R0000-148
  rejects anything else. Wire into the synthetic-return generator from day one.
- Form 7004 is tested independently (own software ID, own 4 scenarios) — NOT covered by
  1120/1065/1041 family testing. Same for 4868 on the individual side.
- Comm testing is one-time per transmission method actually used in production. Production is
  A2A-only → one A2A comm test. IFA is scaffolding for ATS uploads until A2A lands, then drops.
- Ken is the bottleneck resource in Sept–Oct (1041 authoring + regression adjudication +
  extension season). Plan his tasks, not just CC's.
