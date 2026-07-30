# TTS Tax App - STATUS (current state only)

*Last updated: 2026-07-30, session 146 (**THE SCREEN SWEEP IS BACK ON, and unit
26 — Form 6251 / AMT — is SHIPPED** (`1436cf1` on `slate-ui`, no deploy). Ken:
*"Let's get back to the redesign unless this is pressing."* The remaining backlog
item 5 is NOT pressing (nothing is in front of clients until Jan 2027 and the
defect is already visible on the Sch 1-A screen), so it waits. **30 of ~39
screens converted; the sweep resumes at Form 8615.** Earlier the same day: LEG 2
item 3 (`0800455`, the Form 5329 25%/10% blend, 13,000 → 5,500) and item 4
(`3850e2d`, Ken's education-credit election ruling, $800). ⚠⚠ **The app is now
knowingly AHEAD of THREE specs** — `R-5329-02`, `R-8863-LLC` and Schedule 1-A —
all flagged in REVIEW_QUEUE as ONE pattern for the RS session. ⚠ **At deploy:
`seed_rules` on BOTH DBs** (s144's D_RET_007 description + s145's
D_8863_DUAL_STUDENT severity). **No migration anywhere in s144/145/146.**)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — **the SCREEN SWEEP, at Form 8615 (unit 27).**

Ken redirected back to the redesign on 2026-07-30 (s146) after item 4 closed:
*"Let's get back to the redesign unless this is pressing."* **The rule/diagnostic
backlog is PAUSED mid-LEG-2, at item 5** (the Schedule 1-A tips owner filter) —
it is a real overstatement but not pressing: nothing is in front of clients until
January 2027 and the unit-24 screen already flags it by employer name. Ken
directs when it resumes.

**LEG 1 is done.** Every item below is written up in full in `REVIEW_QUEUE.md`
— read the entry before touching code; each carries its engine proof and my
recommendation.

### ▶ LEG 2 — compute fixes with real dollars. **PAUSED at item 5** (s146, Ken's redirect).
Each one needs the RS spec fetched first (the CLAUDE.md gate), the
flow-assertion gate run after (`pytest tests/test_flow_assertions.py -v`), and
a **Ken deploy**. Diagnostics now EXIST for four of the five, so the wrong
number is at least loud while the compute fix is pending.
1. ✅ **DONE (s143, `4c76624`) — the stale QBI deduction on 1040 line 13**
   (s139, chip `task_8000a11e`), $859 understated. `compute_8995_db`'s
   not-engaged branch now blanks line 13 via `write_line_13("")`, which pops
   `values["13"]` so the 14/15 reflow recovers the taxable income; the
   `is_overridden` guard still protects a real direct entry, because line 13 is
   a **computed** line (`seed_1040`: `is_computed=True`) and a typed figure IS
   an override. RS `R-8995-L15` makes line 13a the Form 8995 line-15 figure, so
   with no line 15 the line is blank.
   ⚠⚠ **NEW FINDING — it was an E-FILE defect too, not previously logged.**
   1040 line 13 maps to `QualifiedBusinessIncomeDedAmt` (`builder.py`
   `LINE_ORDER`), and `builder.py` OMITS a blank line from the XML while
   emitting a stored one. The stale figure was therefore **transmitted** as a
   real QBI deduction with the Form 8995 rows blanked — a deduction claimed
   with no supporting form behind it.
2. ✅ **DONE (s143, `4c76624`) — the `disengage()`-guarded-on-`!= ZERO` family**
   (s138). **The root cause was narrower than the backlog assumed and is now
   fixed at BOTH ends.** `compute_8863_db` wrote the nonrefundable credit to
   Schedule 3 line 3 UNCONDITIONALLY while the refundable half five lines up
   already wrote `""` at zero — so a student whose credit came to nothing
   (MFS-barred / phased out / no qualifying expenses) stored a literal `"0"`,
   and the `!= ZERO` guard then refused to clear the one value the engaged path
   could write. Now: blank at zero on the way IN, and "is anything stored" on
   the way OUT.
   ⚠ The same guard was hardened in `compute_1116` / `compute_8960` /
   `compute_2210`, but there it was **LATENT ONLY** — each already writes `""`
   or disengages outright when its amount is `<= 0`, and
   `test_sibling_modules_never_write_a_zero_feeder` **pins that as a fact**
   rather than assuming it, so a future edit that starts writing a zero is
   caught. Every guard now detects the change through the write itself, so an
   already-blank row costs nothing and does not fire a pointless
   `compute_sch_2/3` reflow. No dollar moves — a blank and a "0" sum
   identically.
3. ✅ **DONE (s144, `0800455`) — Form 5329 Part I blends the SIMPLE rate.**
   `part_i_line4` is the ONE helper; `compute_5329_full` AND
   `compute_retirement.compute_5329_part_i` (the pre-dual helper the
   FA-1040-5329 flow assertions exercise) both delegate to it, so the two
   engines cannot drift. `owner_early_distributions` now returns the code-S
   SUBTOTAL and `owner_inputs` passes it as `f5329_line1_simple_portion`.
   **`or has_s` is GONE** — a code-S document rates its OWN slice and the
   `simple_25pct` checkbox is the preparer's whole-line assertion, so both
   controls finally mean something (this also closed s141 defect 5).
   The s141 proof case: **13,000 → 5,500**; the worse one 25,125 → 10,125.
   ⚠⚠ **The app is now AHEAD of `R-5329-02`, deliberately.** The spec was
   fetched live and diffed against `server/specs/5329_spec.json` — identical
   but for the export timestamp, so the shortcut is still in the spec. Flagged
   in `REVIEW_QUEUE.md`, in the module docstring and in the commit.
   ⚠ **THE ONE JUDGMENT CALL — line 2 is apportioned PRO RATA**, and it is
   Ken-flaggable (worth up to $750 on a 50k return against the alternatives).
   The form carries one un-attributed exception figure and i5329 is silent on
   the allocation. Written up in REVIEW_QUEUE; pinned by
   `test_line_2_is_apportioned_pro_rata` so a change would be deliberate. The
   Form 8915-F QDD waiver rides the same pro rata (i5329 exempts a QDD from
   BOTH rates and 8915-F never says which distribution it came from).
   ⚠ **`part_i_split` is a NEW read-only serializer field, NOT a `computed_lines`
   key** — the renderer and the MeF extract iterate that dict by LINE NUMBER and
   the IRS form has no line for the split. Pinned by a test.
   ⚠ **D_RET_007's seeded DESCRIPTION changed** → `seed_rules` at deploy. Its
   message no longer says "refigure by hand"; it reports the two bases.
4. ✅ **DONE (s145) — Form 8863: one student cannot take BOTH credits.**
   **KEN RULED 2026-07-30: treat the AOTC entry as the §25A(c)(2)(A) election.**
   A nonzero `aotc_expenses` on an AOTC-eligible student IS the election
   (`compute_8863.aotc_elected`), and that student is dropped from the line-10
   LLC base — the two sums are now COMPLEMENTARY, so the same $4,000 can never
   earn both credits again. Entering $0 AOTC is how a preparer chooses LLC-only;
   a student who is not AOTC-eligible cannot elect at all, so their LLC expenses
   stand. The full ruling, and the two alternatives Ken rejected (an explicit
   per-student election field; leaving compute alone behind a blocking error),
   are recorded in **DECISIONS.md → Verified Rules — 2025 Tax Year**.
   ⚠⚠ **`D_8863_DUAL_STUDENT` DEMOTED error → warning.** s142 promoted it only
   because compute did not enforce the rule; compute enforces it now, so there is
   nothing left to block — and `warning` is the severity the RS spec carries.
   The message names the student and the dollars on BOTH sides and says which
   credit the student was put on, so the election is never silent.
   → **`seed_rules` at deploy** (severity + description changed).
   ⚠⚠ **The app is deliberately AHEAD of `R-8863-LLC`**, which still sums every
   student's LLC expenses. Spec fetched live and diffed against the cached copy
   — identical but for the export timestamp. Flagged in REVIEW_QUEUE.
   ⚠ **The RS key for this form is `FORM_8863`, not `8863`** — the form-number
   lookup 404s. Use the code the app gives its `FormDefinition`.
5. **PAUSED HERE. Schedule 1-A tips: filter line 4a by `W2Income.owner`** against each
   filer's attestation (the field already exists; treat `joint` as the
   taxpayer's) and warn when a W-2's tips are excluded.
6. **Form 8863 line-7 lockout** — currently `any()`, so one student's box makes
   the WHOLE return's AOTC nonrefundable. Key it per student.
7. **`scha_gambling_winnings`: derive it or keep asking?** — `D_W2G_LOSS_CAP`
   (s142) now reports the disagreement in both directions, but the underlying
   question (should the §165(d) cap simply BE the W-2G box-1 sum plus
   `other_gambling_winnings`?) is still Ken's call. Recommendation: derive it,
   with an `_overridden` companion.

### ▶ LEG 3 — needs a migration or an e-file builder. Stage; Ken pulls the trigger.
8. **`CarLoanVehicle.vehicle_qualifies` / `.loan_qualifies` → `default=False`**
   (s140) — $275 of tax on the proof return from seven conditions nobody
   affirmed. **A migration, and existing rows keep their stored True** — decide
   with Ken whether to backfill or leave history alone, and add a diagnostic for
   a row carrying interest with either attestation unticked.
9. **`build_schedule1a`** against the 2025 `IRS1040Schedule1A.xsd` (s140) —
   1040 line 13b transmits as `TotalAdditionalDeductionsAmt` with **no
   supporting schedule**; every sibling has a builder. Until it lands, a return
   claiming these deductions is paper-only. Note the Part IV line-22 rows come
   from `CarLoanVehicle`, not FormFieldValues. **This is now the ONLY thing
   still holding the Schedule 1-A tracker row open** — the diagnostics half
   closed in s142.
10. **Form 2441 tax-exempt provider e-file mapping** (s138) — the extract raises
   without a 9-digit TIN; i2441 wants the literal "Tax-Exempt" in column (c).

### ▶ LEG 4 — bigger, Ken-scoped. Confirm before starting.
11. **ONE shared overflow-statement mechanism** — three forms silently truncate
   printed rows: Schedule 1-A line 22 `[:2]` (line 23 still sums all) and Form
   2441's `[:3]` twice. Worth one mechanism, not three.
12. **Form 5329 Part IX waiver** — attach a statement and pay nothing. The
   common outcome, and the model cannot express it at all.
13. **R-TIPS-10, the §224 SE gross-income limit** — derive from the Schedule C
   the tips came from rather than adding two preparer facts.
14. **Form 8962: a 100%-of-FPL floor** as a **warning** that distinguishes the
   §1.36B-2(b)(6) safe-harbour case from the no-APTC case (not an error).
15. **Form 2441 both-spouses deeming** — $1,920 vs $0, against an explicit IRS
   instruction the RS spec never carries. Needs an RS decision first.
16. **`eic_self_employed`: derive or keep asking?** (s139) — the unanswered
   default costs $4,328–$7,152. My recommendation was to default it True when
   the return carries a Schedule C / F / SE K-1 with an `_overridden` companion.
   **Ken's ruling still needed** before building.

### ✅ LEG 1 — COMPLETE (s142). What shipped, and what to know about it.

**`2ed5eff` — NEW `apps/diagnostics/rules_sch_1a.py`.** Schedule 1-A had NO
diagnostics at all; `compute_sch_1a`'s own docstring deferred D_SCH1A_001..006
to "when the 1040 diagnostics framework exists", and that framework has existed
for a long time. All six specced rules plus **`D_SCH1A_NO_DOB`**, the one with
the money on it and the one the spec has no entry for: a valid-SSN filer with
no `date_of_birth` while line 35 > 0. **Live-proven READ-ONLY on the demo QA
return at rest** (its NULL date_of_birth is deliberate) — line 35 = 4,826,
line 37 = 0, the rule reports $4,826 forfeited, and the other six correctly
stay silent. Zero writes.
- ⚠⚠ **THE SPEC'S CONDITIONS CANNOT FIRE AS WRITTEN, AND THAT IS THE FINDING.**
  D_SCH1A_001/002 are specced as `filing_status == 'mfs' AND tips_deduction > 0`
  (and the SSN equivalent), but compute already short-circuits Parts II/III/V to
  zeros in exactly those states — so the deduction is structurally incapable of
  exceeding zero and, read literally, both rules are **dead code**. Every rule
  is therefore written against the **INPUTS**: what the preparer needs to be
  told is that the entries are being silently discarded. Severities unchanged.
- ⚠ **Two spec facts have no model field.** `tips_occupation_on_irs_list` is
  conflated with the non-SSTB test in the one boolean
  `tips_eligible_for_deduction` (so D_SCH1A_003 is a *superset* — correct either
  way). `tips_multiple_employers_or_occupations` is absent entirely, so
  D_SCH1A_006 **derives** the employer half from the tipped W-2s; the
  multiple-OCCUPATION half is not derivable and is not covered.
- ⚠ **D_SCH1A_004 is deliberately narrowed** to the 12-month near-miss band.
  Nothing here is an affirmative senior *claim* — Part V is derived from
  `date_of_birth` — so the literal condition would fire on every filer under 65
  whose MAGI leaves line 35 above zero. All four points are in `REVIEW_QUEUE.md`
  for the next Rule Studio session.
- ⚠ The line-4a **$176,100** threshold is the year's social security wage base
  and is **pinned per year (2025 only)**. Years absent from the map SKIP
  D_SCH1A_005 rather than reuse a stale base — the s137 PY_STD_DEDUCTION lesson,
  applied prospectively.

**`42eb851` — the two Form 5329 fixes.**
- **D_5329_003 warning → ERROR.** The conservative blank-value default STAYS
  (the engine must not guess an account value); what changes is that the
  preparer can no longer file past it. The message now names the tax per account
  and states outright that **an explicit 0 is a DIFFERENT answer from a blank**
  — the distinction nothing on screen made. `_EXCESS_PARTS` grew a fourth
  element, the 6%-tax line, so the quantification reads the engine's own number.
- **D_RET_007 rebuilt DUAL-AWARE** through `_f5329_state`. It read the
  deprecated `Taxpayer.f5329_simple_25pct` scalar plus a return-wide code-S
  scan, so a preparer who ticked the per-owner box with no code-S document got
  the 25% rate **silently, from the one check built to catch exactly that**. It
  now reports per owner and names WHICH of the two independent causes engaged
  the rate — they are kept apart in the state dict *because compute ORs them*,
  which is precisely why an unticked box can sit on a return taxed at 25%.
  Severity stays `info` (the backlog authorised the dual fix, not a promotion).

**`d991b50` — the s137 five + two severity corrections.** Each was proven
against the engine's pure functions first (a throwaway probe, no browser), and
each names its dollars in the message.
- **`D_SR_5E_BLANK` (error)** — a blank prior-year Schedule A line 5e untaxes
  the WHOLE state refund. `worksheet_2a` reads the entire 5d as capped away, so
  any refund at or below that returns (0, 0). Engine-proven: a $3,000 refund
  with 5d = $12,000 / 5e = $10,000 puts **$1,000** on Schedule 1 line 1; blank
  the 5e and it is **$0**. D_SR_INCOMPLETE checks 5d and line 17 but not 5e.
- **`D_W2G_LOSS_CAP` (error)** — the §165(d) cap reads
  `scha_gambling_winnings`, a preparer field, not the winnings the return
  reports (W-2G box 1 + `other_gambling_winnings`, which are what reach
  Schedule 1 line 8b). Both directions are wrong and nothing reconciles them:
  a blank/low cap makes `min(losses, 0)` = 0 and the whole deduction vanishes;
  a high cap allows losses above the ceiling. The rule reports which direction
  and how much.
- **`D_8889_FAMILY_ZERO` (error)** — Form 8889 line 6 is a genuine THREE-state
  cap: blank takes the full line 5 limit, a number is this filer's share, and an
  explicit **0** asserts the whole family limit went to the spouse, driving lines
  8/12/13 and the deduction to $0 (the s137 $8,550 → $0 swing).
- **`D_SR_AGE_BLIND_COUNT` (warning)** — `py_standard_deduction` multiplies the
  per-box amount by the count with **no ceiling** while Form 1040 line 12d has
  only four boxes. Engine-proven (2024, single): 4 = $22,400, 5 = $24,350,
  12 = $38,000. An inflated prior-year standard deduction makes worksheet line 8
  too low, so LESS of the refund is taxable — an **understatement**.
- **`D_SR_UNPINNED_YEAR` (warning)** — `PY_STD_DEDUCTION.get(refund_year,
  PY_STD_DEDUCTION[2024])` is a FALLBACK, not an error, and only 2024/2025 are
  pinned. A TY2027 return quietly figures the §111 benefit test against the 2024
  amounts. Engine-proven: the same inputs gave **$500** taxable on 2025
  constants and **$1,000** on the 2024 fallback. No overlap with
  D_SR_TY2026_INTERIM.
- **`D_8863_DUAL_STUDENT` warning → ERROR** — §25A(c)(2)(A) does not allow one
  student to take both credits and compute does not enforce it, so the same
  $4,000 earned an extra $800 on the s138 proof return. The compute fix is LEG 2
  item 4 and is still open, which is exactly why this has to block.
- **`D_8995_STALE` (error, NEW, no spec entry)** — the pair to LEG 2 item 1.
  Fires only when line 13 > 0, is **not** overridden (`write_line_13` already
  protects a genuine direct entry) and `qbi_engaged` is False. What is left can
  only be residue.

**Live read-only check on the demo QA return at rest: all nine of s142's new or
changed rules report ZERO findings** (0 W-2G, 0 HSA accounts, 0 education
students, all `sr_*` facts at defaults, blank 8995 rows) — no false positives.
**Zero writes to the demo data this session.**

⚠ **Five existing registry trip-wires were updated** to the new sets, each with
a comment pointing at the behaviour tests: `test_form_w2g_diagnostics_leg` and
`test_form8889_diagnostics_leg` (both gained an `_ERROR` set),
`test_topic8_diagnostics_leg` + `test_topic8_compute_leg` +
`test_w2_unit2_diagnostics` (Schedule C family 21 → 22), and
`test_form8863_diagnostics_leg` (severity). **A registry trip-wire that pins an
exact code set will fail on every new rule — expect it and update it with a
pointer, never by loosening the assertion.**

### ▶ THE SCREEN SWEEP — **ACTIVE AGAIN (s146). Resume at Form 8615.**
**30 of ~39** 1040 screens are converted (unit 26, Form 6251 / AMT, shipped
`1436cf1`). Remaining (the count was re-measured in
s137 by mapping every `activeTab` to its section component **file** and checking
each for a `NEW_UI` gate — do it that way, never by scanning FormEditor alone):
**8615** ≈259 · **1116** ≈273 · **8880** ≈156 ·
**8960** ≈119 · **5695** · **1040-X** · the **state/GA** tab · the
**prior-year / tax-summary** views · the estimates/extension/e-file cards.
Paradigms settled: view-over-container; **PayerTable** for flat record lists
keyed by row id, **DocumentTabs + worksheet** for card stacks, per-filed-form
rows AND per-owner forms, **InputRow worksheets** for facts cards (screenbar
header for singletons), **the asset register** for computed sub-schedule grids, a
bare **`.slate-asstable`** for a grid with no add row; paradigms may NEST;
multi-section tabs share ONE `.slate-screen` at the call site; screenshots per
screen; live QA writes reverted.
**The business-entity screens (1120-S / 1065 shareholders, partners, balance
sheets, allocations, 7203, page-1 income/deductions) are a SEPARATE unscoped
lane — ~12 more, none started. Ken's call when to take them.**

## 🔑 Method that is finding these defects (keep doing this)
1. **Read the engine against the spec and the form face before writing any
   rule.** Every one of s142's twelve rules came from that, not from a browser.
2. **Probe the engine's PURE functions in a throwaway probe first.** One probe
   file proved all three state-refund claims — the 5e untaxing, the unbounded
   box count, the 2024 fallback — in a single 20-second run, then was deleted.
3. **A spec condition that cannot fire IS a finding.** Four of s142's rules were
   specced against a computed value the engine structurally zeroes in exactly
   the state being policed. Read the compute path before trusting a condition;
   then write the rule against the INPUTS and flag the spec.
4. **Read the FORM FACE, not only the spec** — `pymupdf`
   (`fitz.open(path)[page].get_text()`). Schedule 1-A's line-4 multiple-employer
   "-0- on 4a/4b" instruction, the box-5 > $176,100 caution and the line-36a
   "born before January 2, 1961" cutoff all came from the PDF.
5. **Check the MODEL TYPE, nullability AND DEFAULT of every field.** A nullable
   cap has THREE reachable states (a value / an explicit 0 / NULL) and they mean
   three different things — Form 8889 line 6 and the Form 5329 `*_value` caps are
   both that shape, and both cost real money.
6. **A year-keyed constant dict with a `.get(year, DEFAULT)` fallback is a silent
   wrong answer**, not a safe default. Pin per year and SKIP the unpinned years.
7. **Ask whether a DIAGNOSTIC exists at all, per form** — `ls apps/diagnostics/`
   and look for the module. Schedule 1-A's was simply never written.
8. ⚠⚠ **A DIAGNOSTIC IS NOT A COMPUTE FIX.** Every LEG 1 rule makes a wrong
   number loud; four of them sit on top of defects that are still live in LEG 2.
   Say so in the message so the preparer knows to refigure by hand.
9. ⚠⚠ **FOLLOW THE DEFECTIVE LINE INTO E-FILE, not just print.** Item 1 looked
   like a wrong printed number; `builder.py`'s `LINE_ORDER` made it a wrong
   TRANSMITTED number, because the builder omits a blank line from the XML but
   emits a stored one. Grep `LINE_ORDER` / the mappers for any line you fix.
10. ⚠⚠ **FIX A ZERO-RESIDUE DEFECT AT THE WRITE, NOT ONLY AT THE CLEAR-UP.**
   Item 2's clear-up guard was the symptom; the cause was a sibling write that
   stored `"0"` where every other module stored `""`. When two writes in the SAME
   function disagree (8863 lines 290 vs 295), that is the bug.
11. ⚠ **A "sweep the family" instruction still needs the family AUDITED.** Three
   of item 2's four modules turned out to be latent, not defective — reporting
   them as four live bugs would have been false. Pin the reason each sibling is
   safe in a test so the claim survives.
12. ⚠ **THE REVERT IS THE TEST.** A passing test proves nothing until you have
   watched it fail. Each of s143's three fixes was reverted and the right tests
   failed; one revert also proved the trip-wire, not just the behaviour test.
   s144 did the same for all three of its fixes (10 / 4 / 1 failures).
13. ⚠⚠ **WHEN ONE RULE HAS TWO IMPLEMENTATIONS, MAKE THE SECOND DELEGATE.**
   s144 found `compute_retirement.compute_5329_part_i` carrying its own copy of
   the line-4 rate — dead in production but alive in the flow assertions, so a
   fix to one would have left the gate asserting the old law. It now calls the
   shared `part_i_line4`. (s143's lesson was the same shape one level down: two
   writes in the SAME function disagreeing.)
14. ⚠⚠ **A NEW COMPUTED FIGURE NEEDS A HOME THAT ITS CONSUMERS DO NOT ITERATE.**
   `compute_5329_form_lines` is keyed by LINE NUMBER and the renderer and the
   MeF extract loop over every key, so adding `simple_base` to it would have
   sent a non-line to an AcroForm map and an IRS5329 document. Check how a dict
   is CONSUMED before extending it; s144's split rides its own serializer field
   and a test pins that it stays out.
16. ⚠⚠ **A SCREEN THAT FETCHES ITS OWN DATA HAS TWO STATES A PROP-BASED TEST
   CANNOT SEE**: the envelope it did not unwrap, and the moment before the data
   arrives. s146 shipped both bugs into the live app and the vitest suite stayed
   green through each. Run the screen; then test what the run found.
17. ⚠ **`undefined` (loading) AND `{ready:false}` (answered: not applicable) ARE
   DIFFERENT STATES.** Collapsing them made a computable return say "Form 6251
   does not apply" for the length of the fetch — long enough to screenshot, and
   it read as a verdict. Waiting for INPUTS to exist is not waiting for the DATA
   behind them (the screenshot tool's settle went 900ms → 1900ms for this).
18. ⚠ **WHEN A DEFECT CANNOT BE DETECTED, SAY SO INSTEAD OF PRETENDING TO
   DETECT IT.** Unit 26's zero-cannot-override trap fires on every return by
   construction — a `default=0` non-nullable field has no untouched state — so
   the screen STATES the trap. My first draft flagged it per-cell and was wrong
   on every return.
15. ⚠ **A DEFECT THE SCREEN WARNS ABOUT IS A TEST THAT MUST BE REWRITTEN, NOT
   DELETED.** Fixing the engine broke exactly the tests that pinned the wrong
   behaviour (2 vitest, 3 pytest). Each was rewritten to pin the FIX with a note
   saying what it used to assert — that is the audit trail.

## Dev QA recipe (proven again this session)
preview_start django-demo + vite · demo QA return
`bc270846-5800-4cbc-8f7f-573d0a5a953f` · `scripts/mint_magic_link.py`
(SINGLE-USE — **mint per run**; defaults to the DEMO DB) ·
`scripts/slate_screen_screenshots.mjs <returnId> <tokenFile> "<rail label>"
<slug> [outDir]`
- ⚠ **Demo-DB ORM probes need `TTS_ENV=demo`**; run them from Bash with
  `manage.py shell -c "$(cat file)"` (PowerShell mangles multi-line `-c`).
  A pure-function probe needs no `TTS_ENV` at all.
- ⚠ **A throwaway pytest probe must live under `server/tests/`** or `django_db`
  silently skips. Delete it before committing.
- ⚠ **`$SP`/`/tmp` do NOT exist for `curl -o` on this box** — write scratch files
  into the scratchpad path. ⚠ `git` pathspecs must be **repo-root-relative**;
  running `git add server/...` from inside `server/` fails.
- ⚠ **Reading a huge single-line markdown file**: `grep -n` returns the whole
  line. Use `grep -n -o <literal>` for line numbers, then slice with Python and
  `PYTHONIOENCODING=utf-8` (cp1252 chokes on ✅).
- ⚠ **The Rule Studio key for Schedule 1-A is `SCH_1A`** — `1040_SCH1A`,
  `SCH1A`, `1040S1A` and `1040_SCH_1A` all 404. A guessed-key 404 is NOT "no
  spec"; try the code the app uses for its `FormDefinition`.
  **SECOND OCCURRENCE (s143): Form 8863's key is `FORM_8863`, not `8863`** —
  the bare form number 404s with `{"error":"No spec found for form '8863'"}`
  while `FORM_8863` returns the full spec. Form 8995's key IS the bare `8995`,
  so there is no single convention — **always try the `FormDefinition.code`
  the app itself uses.**
- ⚠ **`slate_screen_screenshots.mjs` takes the RAIL LABEL, not the form name** —
  Form 5329's is **`Add'l Taxes`** (`IndividualNav.tsx` line ~142), and a wrong
  label fails as a bare 30s `TimeoutError` at the second `waitForFunction` with
  no hint. Grep `IndividualNav.tsx` for the tab id first.
- ⚠ **The Rule Studio key for Form 5329 IS the bare `5329`** (unlike `SCH_1A`
  and `FORM_8863`) — third data point that there is no convention. Fetching it
  live and diffing against the cached `specs/*.json` takes one command and is
  worth doing every time: s144's diff was clean but for the export timestamp,
  which is what made "the spec still carries the shortcut" a fact, not a memory.
- ⚠ **Finding `details` money should be quantized** — `compute_5329`'s line dict
  holds un-quantized rate products, so `0.06 × 7000.00` serialises as
  `'420.0000'`. `DecimalField` reads come back at 2 dp, so a test asserting
  `"4000"` fails against `"4000.00"`.
- ⚠ The NEW_UI localStorage key is `delvio-new-ui` (`"1"`/`"0"`), read at MODULE
  LOAD — reload after setting it.
- ⚠ **An ORM create OR delete MUST be followed by `compute_return(tr)`** before
  a baseline is recorded or trusted — and then **compare EVERY line**.
- ⚠ **One test DB — never overlap pytest runs.** ⚠ `FormDefinition`'s year field
  is **`tax_year_applicable` (an int)**. ⚠ FFV ORM path =
  `form_line__section__form__code`. ⚠ Commit-message files must be ASCII, passed
  with `-F`. ⚠ Run vitest **from `client/`**; `tsc` needs
  `-p tsconfig.renderer.json`.

**Build rules in force:** selective `git add` only — NEVER `git add .` (parallel
work STILL unstaged and untouched by s143/s144: `server/apps/returns/views.py`,
`tb_import.py`, `tests/test_tb_import.py`,
`server/scripts/create_ar_cutover_clients.py`; ⚠ also never `git stash` here) ·
no merge/deploy without Ken · at deploy: migrate (diagnostics 0005) +
**`seed_rules` on BOTH DBs** (s142's 12 new rules + the D_5329_003 /
D_8863_DUAL_STUDENT severity promotions, plus the earlier D_W2_ family +
MATH_BALANCE_SHEET description, **plus s144's D_RET_007 description and s145's
D_8863_DUAL_STUDENT severity+description**). s143 added no migration and no seed
change. **Neither s144 nor s145 adds a migration, but BOTH change seeded rule
rows** — `seed_rules` must run on both DBs. ⚠ s145's is a **severity** change
(error → warning), which the s109b lesson says lives in TWO places — seed it, do
not assume the code change is enough.

**s146 gates (unit 26 — Form 6251 / AMT):** NEW `tests/test_amt_face_s146.py`
**7** (revert-tested — collapsing the null line 11 into "0.00" failed 2) · NEW
`slateForm6251Screen.test.tsx` **13** · flow assertions **521** (baseline
exactly) · the 6251 / amt_face bands **587** · vitest **1129/1129** · tsc **46 =
baseline**. **LIVE-PROVEN** on the demo QA return (AMTI 94,560 → exemption 88,100
→ excess 6,460 → TMT 1,485 against a regular tax of 12,204 → no AMT) with the
demo data **verified UNCHANGED, 892/892 rows — this unit wrote NOTHING**.
⚠⚠ **TWO OF THE THREE BUGS IN THIS UNIT WERE MINE AND ONLY THE LIVE RUN FOUND
THEM** — the unit tests pass the face in as a prop, so neither the missing
`{"data": ...}` unwrap nor the collapsed loading state was reachable from a test.
Both now have tests. **A fetch-backed screen needs a live pass; a vitest suite
over its props is not a substitute.**

**s145 gates (all green, re-run them if you touch these modules):** new
`tests/test_backlog_leg2_item4_8863_election.py` **12** · flow assertions **521**
(baseline exactly) · the 8863 / Topic 8 / Schedule 3 / MeF-1040 bands **298**.
**Both fixes revert-tested** — restoring the all-students LLC sum failed 4
(3 in the new suite + the severity trip-wire's line-10 assertion), and reverting
`aotc_elected` to the NAIVE reading Ken rejected ("drop the LLC whenever the AOTC
is allowed") failed 3, including the escape-hatch test.
⚠⚠ **THE FIRST DRAFT OF THE NEW SUITE COULD NOT SEE ITS OWN REGRESSION.** Its
`_sums()` helper re-derived the two bases with `aotc_elected` instead of reading
what `compute_8863_db` wrote, so reverting the compute fix left every test in the
file passing — only the older trip-wire caught it. It now runs a real
`compute_return` and reads FORM_8863 lines 1 and 10. **A test that
re-implements the thing it tests is blind by construction** (the s143
"a delete-the-source test can hide its own effect" lesson, second occurrence).

**s144 gates (all green, re-run them if you touch these modules):** new
`tests/test_backlog_leg2_item3_5329_blend.py` **17** · flow assertions **521**
(baseline exactly) · Topic 5 / 5329 / diagnostics bands **87** · retirement +
render + e-file bands **220** (1 skipped) · vitest **1116/1116** (+2) · tsc
**46 = baseline**. All three fixes **revert-tested** — reverting the blend
failed 10, the `or has_s` removal 4, the shared-helper delegation 1.
**LIVE-PROVEN** on the demo QA return (line 4 = 5,500, the blend note, D_RET_007
naming both bases) and the seed **REVERTED with ZERO drift**, ORM-verified
892/892 FormFieldValue rows. Side-by-sides in `Design/screenshots/`.

**s143 gates (unchanged):** `tests/test_backlog_leg2_compute.py` **8** · Topic 8
/ 8995 **166** · 8863 / 1116 **86** · 8960 / 2210 **119**.
⚠ **The source trip-wires strip comment lines** (`_code_only`) because those
sessions' own comments QUOTE the retired snippets to explain them — a raw
`inspect.getsource` match fires on the explanation, not on a regression.

**KEN CLARIFIED (2026-07-28): the tax app is TESTING until January 2027.** He
switches to Slate when the redesign is FINISHED; everything rides `slate-ui`;
the shared Supabase DB caution is the one true-production constraint.

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE — trimmed to live items)
1. **(s142) The Schedule 1-A RS spec needs four corrections** — 001/002 specced
   against a value compute structurally zeroes (dead code as written);
   `tips_occupation_on_irs_list` has no model field (conflated into one
   boolean); `tips_multiple_employers_or_occupations` has no field at all (the
   employer half is derived, the occupation half uncovered); D_SCH1A_004
   narrowed to the near-miss band because Part V is derived, not claimed.
   Recommendations in REVIEW_QUEUE. **Blocks nothing — the leg is shipped.**
2. ✅ **CLOSED (s144, `0800455`) — Form 5329 charged the 25% SIMPLE rate on the
   WHOLE of line 3.** Fixed as a real blend (13,000 → 5,500 on the proof case).
   **Two things replace it in the queue:** (a) 🔴 **Rule Studio must correct
   `R-5329-02`** — the app is now ahead of its spec, deliberately, and the spec
   needs the blend plus a fact for the code-S slice of line 1; (b) 🟡 **the
   pro-rata apportionment of line 2 is my judgment call** and Ken may want a
   second preparer field instead. Both written up in REVIEW_QUEUE.
3. **(s140) Schedule 1-A is never transmitted while 1040 line 13b is.** Needs a
   `build_schedule1a` against `IRS1040Schedule1A.xsd`. Until then a return
   claiming these deductions is paper-only. **The only thing still holding the
   Schedule 1-A tracker row open.** LEG 3 item 9.
4. **(s140) The tips deduction ignores the W-2 owner** — a non-attesting
   spouse's box 7 tips are deducted. LEG 2 item 5.
5. **(s140) Both Part IV attestations default TRUE** — $275 of tax on the proof
   return with nothing affirmed. A migration; LEG 3 item 8.
6. **(s140) Tips from more than one employer** need the form's "-0- on 4a/4b"
   treatment plus the instruction-derived line 4c. **Now DIAGNOSED**
   (D_SCH1A_006, s142) but not computed.
7. **(s140) The §224 self-employment gross-income limit on tips (R-TIPS-10) is
   unbuilt** and Schedule C now exists — the deferral reason is stale.
8. **(s140, minor) Only two line-22 vehicle rows print** while line 23 sums them
   all — the same overflow gap as Form 2441's `[:3]`. One shared mechanism.
9. ✅ **CLOSED (s143, `4c76624`) — the stale QBI deduction on 1040 line 13.**
   Diagnosed s142 (`D_8995_STALE`), FIXED s143. Also found to be an e-file
   defect (transmitted as `QualifiedBusinessIncomeDedAmt` with no Form 8995).
   Chip `task_8000a11e` can close.
10. **(s139) Should `eic_self_employed` be DERIVED rather than asked?** The
   unanswered default silently costs $4,328–$7,152 and no diagnostic covers it.
11. **(s139, minor) Three seeded `1040_EIC` rows are never written.**
12. ✅ **CLOSED (s145) — Form 8863 let ONE student take BOTH education credits.**
   **Ken ruled: the AOTC entry IS the §25A(c)(2)(A) election** (DECISIONS.md).
   Compute now drops an elected student from the line-10 LLC base; the
   diagnostic is back to **warning** and names the dollars on both sides.
   **What replaces it:** 🔴 **Rule Studio must correct `R-8863-LLC`** and give
   the election a spec concept — see the s145 REVIEW_QUEUE item, and note the
   PATTERN there: three forms (5329, 8863, Schedule 1-A) now diverge in the
   same direction, their specs carrying the arithmetic but not the statutory
   exception. Worth one conversation, not three tickets.
13. **(s138) The Form 8863 line-7 lockout is global but keyed per student.**
14. ✅ **CLOSED (s143, `4c76624`) — `compute_8863_db.disengage()` cannot clear a
   "0" it wrote itself.** Fixed at the root (the unconditional write) as well as
   at the guard. The other three `!= ZERO`-guarded paths (`compute_1116` /
   `compute_8960` / `compute_2210`) were **audited and were latent only** — each
   already writes `""` or disengages when its amount is `<= 0`; hardened anyway
   and that fact is now pinned by a test.
15. **(s138) Form 8962 has NO 100%-of-FPL eligibility floor** — engine-proven, a
   full premium tax credit at 66% of FPL, where §36B(c)(1)(A) is 100–400%.
16. **(s138) Form 2441 deems BOTH spouses for the same months** — engine-proven
   $1,920 vs $0 against an explicit IRS instruction the RS spec never carries.
17. **(s138) A tax-exempt care provider cannot be e-filed** (the extract raises).
18. **(s138, minor) Only three Form 2441 Part I / Part II rows print.**
19. **(s137) Should `scha_gambling_winnings` auto-populate** from the W-2G box-1
   sum + `other_gambling_winnings`? **Now DIAGNOSED in both directions**
   (D_W2G_LOSS_CAP, s142); the derive-or-ask question is still Ken's. LEG 2
   item 7.
20. **(s137) `compute_8889_db` stores the FORM_8889 face from accounts[0] only.**
21. **(s137, minor)** `worksheet_2` computes `w8` unclamped while
   `_worksheet_rows` displays `max(0, …)`.
22. **(s130, re-confirmed s136–s140) PATCH `/taxpayer/` and the per-form PATCH
   lanes run tens of seconds in-process**, and the per-record lane's re-render
   can lag a settled write by a whole action. Profile in a session that owns
   views.py.
23. **(s131) Form 7203 panel legacy-styled** inside the Slate K-1 screen.
24. **(s129) The RS authoring session** — scheduled; agenda in REVIEW_QUEUE, now
   headed by item 1 above.
25. **(s129) Launcher menu extras** — no data source; rulings wanted.
26. s124's `D_4562_RECON` scoping pair.
27. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
28. *(cosmetic)* Legacy floating "Calculating…" chip needs a Slate home.
29. *(s126)* Sch B 3a/3b detail tables entry gap; 5 unbuilt Sch B runners.
30. **(s136)** Replace-all nested lists (8915-F item C) remain vulnerable to
    out-of-order server arrival of two overlapping PATCHes.
31. **(s137, bookkeeping)** `form_coverage_tracker.md` has **no DIAGNOSTICS
    column and no E-FILE column**, so no form's all-green line can be trusted.
    The SCH_1A row is the live proof: it was tagged complete with every leg ✅
    while both of those legs were empty. **s142 closed the diagnostics half**
    (the row is annotated); the e-file half is LEG 3 item 9. Neither
    **STATE_REFUND** nor **FORM 5329** has a row at all, though every leg exists.
    The table needs those two columns. ⚠ **s144 deliberately did NOT open a
    Form 5329 row** even though it worked the compute leg: without the two
    missing columns a new row could only be recorded as all-green, which is the
    exact SCH_1A mistake this item exists to stop. Add the columns first, then
    both rows.

## Active gates
- **Branch discipline:** `slate-ui` checked out; parallel session's uncommitted
  work UNSTAGED. Never stage/stash/`git add .`.
- ⚠ **Demo DB drift:** diagnostics migration 0005 applied to the DEMO DB only —
  prod applies at Ken's deploy (additive, safe). **`seed_rules` has been run on
  the DEMO DB for all of s142's rules; PROD seeds at Ken's deploy.**
- ⚠ **s144 wrote to the demo QA return and REVERTED it.** Two synthetic 1099-Rs
  (`QA144 Fidelity 401(k)` code 1 / 50,000 and `QA144 SIMPLE IRA` code S /
  2,000) plus one `Form5329` row were created to live-prove the blend, then
  deleted with `compute_return(tr)` re-run. **Zero drift, ORM-verified: 892 of
  892 FormFieldValue rows identical to the pre-write baseline**, the original
  TRS 1099-R intact, no Form5329 rows left. The at-rest figures below still hold.
- ⚠ Demo QA return (`bc270846…`) carries synthetic review data ON PURPOSE —
  s127 1099-R (TRS $24,000) + s128 1099-INT ($1,250/$300/$50 W/H) + 1099-DIV
  ($800/$600/$150) + SS box 5 $21,600.
  **At rest (unchanged by s142 — this session wrote NOTHING to the demo data):
  0 car-loan vehicles, all 8 Sch 1-A facts at defaults, 0 dependents,
  0 Schedule C, 0 1040_EIC rows, all 17 `eic_*`/election facts at defaults,
  8867 / 8862 / 8995 rows all blank, 0 care providers, 0 FORM_2441 rows,
  0 Forms 1095-A, 0 FORM_8962 rows, 0 education students, 0 W-2G, 0 HSA
  accounts, all 17 `sr_*` facts at defaults, 0 Forms 8915-F / 8606 / Roth
  trackers / SS lump-sums / 1099-G. AGI 94,560, L12 15,750, L13 blank, L13b 0,
  L15 78,810, L16/L18 12,204, L19 0, L20 0, L24 12,204, L27 0, L33 2,450,
  L37 10,151, Sch 3 line 2 blank, 1040 1e blank.**
  ⚠ **The taxpayer's `date_of_birth` is NULL and that is now the LIVE PROOF for
  BOTH the s140 defect-1 and s142's `D_SCH1A_NO_DOB` ($4,826 forfeited, verified
  read-only this session) — do not "fix" it.** SCH_1A rows at rest:
  3 = 31 = 94,560, 32 = 75,000, 33 = 19,560, 34 = 1,174, 35 = **4,826**,
  36a/36b/37/38 = 0.
  ⚠ Due-diligence print blockers are EMPTY at rest.
- ⚠ D_8995/D_8959 NoneType errors fire on this return's diagnostics — known
  RS-session agenda item, not a sweep defect.
- ⚠ Demo employers registry: synthetic TRS of Georgia 58-1234567 + GA account
  1234567-AB (harmless, kept).
- ⚠ FA-1040-4835-06 drift (chip `task_0cf10eac`).
- ⚠ One test DB — never overlap pytest runs.
- ⚠ `server/.venv` repaired s124; use `.venv\Scripts\python.exe` directly.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged — Slate runs in front by Ken's ratification; the form queue
interleaves on Ken's direction.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
- **Slate lane:** `Design/SLATE_IMPLEMENTATION_PLAN.md` ·
  `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md`.
