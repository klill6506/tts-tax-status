# 1040 SPRINT SCOPE v2 — Fable 5 Window
*Paste into tts-tax-app root as `SPRINT_SCOPE.md`, replacing any prior version. CC: read this at every session start, immediately after STATUS.md. This file defines what is in and out of scope. Do not build anything outside it without asking Ken.*

---

## GOAL

A 1040 module The Tax Shelter can use **in-house for the 2027 filing season (TY2026 returns)**, at TaxWise quality: computations match IRS published tables and worksheets to the dollar, diagnostics catch problems before printing, and nothing is silently half-built.

The Fable 5 window (~11 days) is for the hardest, highest-leverage work. Trivial forms and field maps are deliberately deferred — any model can do those later.

---

## TARGET-YEAR POLICY

- All RS specs and compute are **year-parameterized** via `_constants_for_year()`.
- **TY2026 is the product target. TY2025 is the verification bed.** Build both years' constants for every topic; OBBBA provisions differ between 2025 and 2026 (several take first effect in 2026) — verify each year's constants independently against enacted law. Never assume a 2025 amount carries to 2026.
- Rationale: the firm has thousands of completed TY2025 TaxWise returns. End-state QA = re-key real returns and match to the dollar. Ken is assembling a regression set of real TY2025 returns across archetypes; every topic must fully compute a TY2025 return.

---

## QUALITY RULES (non-negotiable — these define "TaxWise quality")

1. **Table semantics, not formulas.** Tax liability under the table threshold uses IRS Tax Table lookup conventions (bracket midpoints), not the rate schedules. EIC uses the EIC Table's $50-bracket lookup convention. Encode the table convention in the RS spec. A $2 mismatch vs. TaxWise destroys preparer trust.
2. **No silent gaps.** Any situation the software cannot compute correctly throws a RED diagnostic: "Not supported — prepare manually: <specific item>." It never computes a wrong number quietly. Unsupported paths are enumerated in each topic's spec.
3. **Definition of done is a checklist.** A topic is not marked complete in form_coverage_tracker.md until every item in its DoD below is green (Input → Compute → Render → Flow assertions for each form, plus the topic-specific items).
4. **Line-status truth.** Maintain a per-line table for Form 1040 and Schedules 1/2/3 recording COMPUTED (feeder built) vs DIRECT-ENTRY (preparer keys it). Update it every time a feeder topic completes. This file is how Ken knows what the software actually does.
5. Every line on Schedules 1/2/3 accepts direct entry from day one. Computed sources replace direct entry as feeder topics are built (YELLOW vs GREEN per the color system).

---

## SPRINT ROSTER (in order — do not reorder without asking Ken)

### 0. f1040_2025.py FIELD_MAP audit
Already queued. Verify every line against extracted 2025 PDF widget names; round-trip smoke test. The 2026 form gets mapped when IRS releases it (note in tracker).

### 1. 1040 Spine — TOPIC
DoD:
- Filing status (all five) with MFS/HOH diagnostics
- Dependents: preparer-asserted entry with diagnostics (SSN format, age vs. birthdate consistency, CTC/ODC/EIC qualification flags) — NO eligibility adjudication engine this sprint
- All income lines (direct-entry capable; computed where feeders exist)
- Standard deduction including additional amounts for 65+/blind (born-before-Jan-2 convention) — verify amounts per year
- Tax computation via Tax Table lookup convention; rate schedules above threshold
- **Bridge diagnostic:** until Topic 3 lands, any value on line 3a or a capital gain distribution entry fires RED "qualified dividends / capital gain distributions present — tax computation not yet supported." The spine must never compute table-only tax on those returns.
- Credits, other taxes, payments, withholding aggregation; refund/amount-owed
- Estimated payments input (four quarters + prior-year applied)
- Flow assertions wired for every computed line

### 2. Schedules 1, 2, 3 — TOPIC
DoD: full structure, every line direct-entry capable, totals flow to 1040, line-status table created, flow assertions on all totals and every computed line.

### 3. Interest, Dividends & QDCGT — TOPIC (promoted; touches nearly every retiree return)
DoD:
- 1099-INT document input: all boxes (taxable interest, US savings bond/Treasury, tax-exempt, federal withholding, etc.), aggregating to lines 2a/2b
- 1099-DIV document input: all boxes — ordinary (1a), qualified (1b), capital gain distributions (2a), §1202/collectibles sub-boxes as stored fields with RED diagnostic if present, nondividend distributions, federal withholding, §199A dividends (box 5 — stored now, consumed by QBI later), exempt-interest dividends — aggregating to lines 3a/3b
- Line 7 checkbox path: capital gain distributions direct to 1040 line 7 when no Schedule D required
- **Qualified Dividends & Capital Gain Tax Worksheet** — full implementation; verify 0%/15%/20% breakpoints per filing status per year; replaces the spine's bridge diagnostic
- Schedule B render (payer listing from the 1099 documents, Part III foreign account questions) — nearly free once document input exists
- Flow assertions: 2a/2b, 3a/3b, line 7, line 16 tax

### 4. Schedule 1-A (OBBBA) — finish
DoD: senior age disqualification proven computational via L36a/b with a targeted test; Ken's statute review walked through in-session; on approval flip READY_TO_SEED → seed → verify → all 21 scenarios pass → flow assertions wired. Confirm 2026 constants alongside 2025.

### 5. Retirement Income — TOPIC (highest client volume; do not split)
DoD:
- 1099-R input: all boxes, distribution codes driving treatment; rollover handling; QCD handling (line 4b reduction + literal)
- **Simplified Method: OUT this sprint.** RED diagnostic when box 2a is blank or "taxable amount not determined" is checked with basis indicated (the OPM CSA-1099-R pattern): "Taxable amount not determined — compute manually (Simplified Method not yet supported)." TRS and most payers report 2a correctly and flow normally.
- Minimal Form 5329: 10% additional tax + common exception codes to Schedule 2; uncovered codes fire RED
- Taxable Social Security worksheet (provisional income, 50%/85% tiers); lump-sum election = RED unsupported
- Edge: simultaneous IRA deduction + taxable SS interaction worksheet = RED unsupported this sprint
- Flow assertions: 4a/4b, 5a/5b, 6a/6b, Schedule 2 additions

### 6. CTC/ACTC (Form 8812) — complete the build
DoD: full 8812 per current spec (verify TY2025 vs TY2026 amounts independently), existing 13 flow assertions stay green, phaseout boundary tests, render verified.

### 7. EIC — TOPIC (do not split) + Form 8867
DoD:
- Eligibility rules as RS spec: qualifying child tests as diagnostics + preparer assertion, investment income limit (verify per year), MFS exception rules, SSN requirements
- Earned income worksheet incl. SE earnings (from Schedule 1 flowed-or-direct values; Schedule C compute NOT required for this topic)
- Credit lookup with lower-of AGI/earned-income rule and EIC Table $50-bracket convention
- Schedule EIC render; Form 8862 as data-mapped render (no compute)
- Form 8867 due diligence checklist (data mapping; covers EIC, CTC/ODC, HOH — required on every such return; AOTC section renders but flags RED until 8863 exists)
- Flow assertions wired

### 8. Schedule D + Form 8949 — TOPIC (reuse 1120-S machinery)
DoD:
- 8949 input/render reusing the 192-field map and capital gains engine
- Short/long netting, $3,000 capital loss limitation, Capital Loss Carryover Worksheet
- Plugs into the QDCGT worksheet built in Topic 3 (Schedule D line 16 path replaces/joins the line 7 checkbox path)
- Schedule D Tax Worksheet (unrecaptured 1250 / 28% gain) = RED unsupported this sprint; first post-sprint item alongside the Schedule C topic
- Flow assertions wired

### 9. Ride-along (only if ahead of pace; otherwise post-sprint filler)
- Form 8880 (saver's credit — verify AGI tiers per year)

**Mid-sprint swap rule:** if pace is ahead at the midpoint, Ken may swap the Schedule C topic in ahead of item 8 or 9. Ask him; don't decide alone.

---

## NEXT-UP (immediately post-sprint — in this order)

1. **Schedule C + SE + 8995 (simple QBI) + 4562 reuse** — one topic, do not split. SEHI deduction included; SEHI↔PTC circular deferred until 8962 exists.
2. **Simplified Method worksheet** (OPM annuitants — replaces the Topic 5 RED diagnostic)
3. ~~Schedule D Tax Worksheet (1250/28%)~~ **DONE — built post-sprint as part of Topic 9
   (Schedule D + 8949 + the four i1040sd worksheets).** Verified 2026-06-24: `compute_sdtw`
   + the 28%-rate / unrecaptured-§1250 / carryover-out worksheets in `compute_schedule_d.py`,
   wired live into `compute_return` via `schedule_d_line_16` → 1040 line 16; 5 test legs
   (`test_topic9_*`) + 12 flow assertions (`FA-1040-SCHD-01..12`, all green). RED-defers
   Form 4952 / §1202 DIV 2c / Form 2555 / 8814 (intended v1 boundaries, own REDs). The
   prior "RED unsupported / first post-sprint item" notes were stale.
4. Schedule A (OBBBA-modified SALT and charitable rules — verify enacted amounts per year; 2026 introduces changes 2025 doesn't have)
5. Form 2441
6. **Schedule E page 1 + Form 8582** — one topic, never separately ($25k special allowance lives on 8582; E without it is the half-done trap)
7. Form 8962 (mandatory before live use for marketplace clients)
8. Form 8863
9. State refund taxability worksheet (RED diagnostic until built)

## RUNWAY TO JANUARY 2027 — SUPERSEDED by `SEASON_PLAN.md` (2026-07-02)

> This section is superseded by **`SEASON_PLAN.md`** (repo root) — the authoritative dated
> plan for the Jan 2027 season (locked scope, month-by-month, gates, risk register), adopted
> by Ken 2026-07-02. The old bullets here were already overtaken by events (GA-500 completed
> June 2026; MeF XML + ATS scenarios began 2026-06-27, months ahead of the Sept–Oct slot).
> Read SEASON_PLAN.md instead; dates and gates change only on Ken's explicit direction.

## DEFERRED (Ken decides timing)

> **2026-06-25 STALE-LIST CORRECTION:** most of this list was built and tagged after it was
> written. Verify against git tags before treating anything here as unbuilt (the
> "federal-backlog-already-built" lesson). **DONE + tagged:** Form 2210
> (`1040-form-2210-complete`, 2026-06-15 — full §6654: Part I + regular quarterly method +
> Schedule AI, 6 FA in the gate), AMT 6251 (`1040-6251-complete`), kiddie tax 8615
> (`1040-8615-complete`), Form 8606 (`1040-form-8606-complete`), 8995-A
> (`1040-8995a-complete`), Schedule F (promoted 2026-06-21, built) + Schedule J.

- ~~Form 2210~~ **DONE 2026-06-15** — tag `1040-form-2210-complete`. v1 intentional boundaries
  (own diagnostics, not silent gaps): Part II waiver = preparer note; per-period actual-WH-date
  election spread evenly; Schedule AI uses annualized *ordinary* tax (full QDCGT/AMT interplay
  flagged); farmers/fishermen 66⅔% rule out. §6621 rates are 2025-interim — re-pin for TY2026.
- ~~AMT (6251)~~ **DONE** · ~~kiddie tax (8615)~~ **DONE** · ~~8606~~ **DONE** ·
  ~~8995-A~~ **DONE**. Still genuinely deferred: **5329 full exception-code catalog** (only the
  minimal 5329 from Topic 5 retirement income is built), **multi-state** framework (GA-500 is
  the only state).
- ~~Schedule F~~ **PROMOTED 2026-06-21 → built** — Ken directed Schedule F (1040 farm) as the
  next unit after the Effort #5 K-1 router (cash-method v1). Schedule J farm income averaging
  rode as its fast-follow (built).

---

## STANDING RULES (unchanged from the master prompt)

Spec-first; Ken authors tax law; constants verified per year against IRS/statute, never training memory; ask Ken questions live, one at a time, with a recommendation; main-only git with per-leg commits and per-topic tags; shared-Supabase migration rules; targeted tests only; memory files and line-status table updated at every topic completion.
