# TTS Tax App — STATUS (current state only)

*⭐⭐ **s306q (2026-08-28, build lane) — THE NOL STATUTORY PASS: BOTH 80% LIMITS
ARE CORRECT, AND THE DIFFERENCE BETWEEN THEM IS LAW, NOT A BUG** (`836717ed`,
deploy API-confirmed **LIVE**). Ken asked for this saying both Lacerte and
TaxWise might be wrong, so nothing was checked against a vendor return.
**Federal** IRC §172(a)(2)(B)(ii) verbatim: 80% of the **excess** of taxable
income *over the pre-2018 NOLs* — our `0.80 × (ti − pre)` is right, and the
naive `0.80 × ti` would over-deduct **$24,000** on the spec's own T7.
**Georgia** IT-511 line 15b worksheet, line 5 verbatim: *"NOL from Line 2
applied to current year (cannot exceed **80% of Line 3**)"*, and line 3 is the
**RAW** *"Income before GA NOL (Line 15a)"* — so Georgia measures against the
raw base, which is exactly what `compute_ga500` does. ⭐⭐ **The reconciling
fact: the excess-over-(A) structure is a CARES Act creation** (TCJA as enacted
applied 80% flat), **and IT-511 p. 30 says Georgia *"did not adopt the revised
net operating loss provisions in the CARES Act."*** Georgia is frozen
pre-CARES. ⚠⚠ **The Georgia REGULATION (560-7-4-.01(3)) is too terse to settle
it** — *"such limitations … shall be applied to Georgia taxable net income"*
reads both ways, and I nearly reported our correct GA code as wrong on it; **the
booklet's printed WORKSHEET is what settles the arithmetic.** Real money: base
50,000 / pre 30,000 → Georgia allows 20,000 where the federal shape allows
16,000. **Guarded, not just documented:** `test_ga500_nol_divergence_s306q.py`
(7 tests) carries the explanation in the load-bearing assertion's failure
message, verified by defect injection; + DECISIONS.md + a citation at the code
site. ⚠ **The pre-existing T9 scenario is post-2017-only and could NEVER have
caught this** — the divergence is observable only when BOTH pools exist.*

*⭐ **s306q also shipped: `D_GA500_021` (warning) — the federal NOL's Georgia
add-back.** Georgia starts from federal AGI, which is already net of the
Schedule 1 line 8a deduction, and IT-511's Additions item 6 requires it added
back because the Georgia NOL is computed separately. **`S1-4` is direct-entry
and NOTHING writes it** — survivable while 8a was hand-keyed, but since the Form
172 engine landed **8a computes itself**, so a return can acquire a federal NOL
deduction with no human touching an NOL field while Georgia income is
understated by the whole amount, silently. The rule names it and deliberately
does NOT derive the number (same open design question as the s302
`us_government_income` → S1-10 item). Also: **`D_GA500_009`'s message cited
"Schedule 4 Part III"**, which per IT-511 is the **CARRYBACK** schedule, left
blank for a carry-forward-only loss — rewritten to name what is actually
missing (two scalars, no vintages, no expiry, no write-back), **and the registry
description carried the same wrong citation** (the s302b twin lesson, again).
`D_172_80PCT_STATEMENT` declared `warning` in the registry while always emitting
`info` — the finding's severity wins at runtime so nothing gated wrongly, but
the catalogue lied; aligned. Gates: 296 + 526 flow assertions.*

*⚠⚠ **s306p, AGAINST MYSELF — I CAUSED THE SIXTH "THE ROUTE ALREADY EXISTS",
AFTER FIVE SESSIONS OF SAYING IT TO THE ENTRY LANE** (`0e2405b3`, LIVE). Ken
authorized "GA estimated payments should reach the payments line"; I started
building the route. **It existed, from s277**, in the GA-500 attach/resync path
in `views.py` — and my version read the **GA-500 return's** payment rows (there
are none; they live on the FEDERAL return), computed 0, and **overwrote the
correct value**. An existing s277 test caught it. ⭐ **Searching the file you
expect is not searching** — grep the LINE NUMBER (`"26"`), not the concept.
⭐ **The real defect, found only after I stopped building:** the s277 predicate
required `tax_year_for == <return year>` — **strict equality against a field the
model documents as OPTIONAL** — so a row carrying only state/kind/amount was
silently dropped, which is exactly the "accepted and ignored" the lane reported.
Now `tax_year_for` → else `date_paid`'s year → else the return's own year.
**Blast radius measured against prod: 27 GA estimate rows, exactly ONE newly
counted — the reported row itself.** Both gates verified by defect injection.
⚠ The lane has since keyed `tax_year_for: 2025` (the Form 500 line-26 heading
states the year), so their packet exercises the primary path, not the fallback.*

*⭐ s306 (2026-08-27, build lane): **the HSA excess detector could not see an
employer-only over-contribution** — three BATCH-296 entry-lane items in one
pass. The headline is that `D_8889_EXCESS` **already existed and already ran**:
its condition was `line 2 (own contributions) > line 13 (the deduction)`, the
taxpayer's own excess and nothing else. Per **i8889** the employer's excess is a
different quantity against a different comparand — *"the excess, if any, of your
employer's contributions over your limitation on **line 8**"* — and **i5329 line
47** is the SUM of the two, *"Also include on line 47 any excess contributions
your employer made."* When the over-contribution is entirely the employer's,
lines 2, 12 and 13 are ALL zero, so the old condition was **structurally
incapable of firing**. Both quotes verified verbatim from the downloaded 2025
instruction PDFs (the states lane independently pulled both and confirmed them).
**Prod census: 70 HSA rows, 6 carry an excess, ALL 6 employer-only, old
condition fires on ZERO.** 3 keyed and agreeing (429 · 1,000 · 2,700 — three
independent witnesses for the formula), 3 blank at 1,000 each. ⚠ **The $180 of
missing excise is an UPPER BOUND, not a measured loss** — the excise does not
apply to a timely-withdrawn excess and the return does not record withdrawal.
Shipped: `excess_contributions()` in `compute_8889.py` (reads `owner_lines`, so
diagnostic and compute cannot drift) + the rule rewritten to name the total, its
composition, the missing excise, where to key it, and the i8889 *"report it as
Other income if it was not in W-2 wages"* consequence. **Two deliberate calls,
both evidence-based rather than taste: it stays a WARNING** (back-entry commit
gates on ZERO error-severity findings and errors are not acknowledgeable —
`backentry.py` Gate 4 — so an error would hard-block returns whose excess was
properly withdrawn, against Ken's "warn, never hard-block"), and **line 47 is NOT
auto-derived** because `Form5329.hsa_curr_excess` is `default=0` **non-nullable**,
so a keyed zero is indistinguishable from an unkeyed field and there is no
off-switch for the withdrawal case (s237 class). Silent when line 47 already
states the derived amount. Verified read-only over every prod HSA return: fires
on exactly the 3 blank, silent on the 3 keyed.*

*s306 items ② and ③: **the two opposite sign conventions** now carry
descriptions that each name the other —
`int_1099s[].accrued_interest_adjustment` is a POSITIVE MAGNITUDE the engine
subtracts (keying it negative overstates by exactly 2×; the entry lane's tell,
*"a delta that is exactly 2× an input means an inverted sign, not a missing
figure"*, is recorded in the field itself), while
`taxpayer.qbi_loss_carryforward_prior` carries its own sign;
`form_5329s.hsa_curr_excess` gained one too. **③ the 4547 enum casing was FIXED,
not documented** — staging upper-cases `form_4547s[].children[].relationship` in
place (the MeF `ChildRelationshipCd` enum is UPPERCASE while
`dependents[].relationship` on the SAME payload is lowercase), so `"son"` is
accepted and `"cousin"` still refuses. Narrow by design: normalising every enum
generically would change behaviour for lowercase sections. Published back-entry
schema regenerated.*

*⚠⚠ **s306's OWN LESSON — A COUNT TAKEN AGAINST A LIVE CORPUS IS A TIMESTAMP,
NOT A FACT.** My census read 68 rows / 5 excess / 2 keyed; a verification run 40
minutes later read 70 / 6 / 3 — same query, same firm. **Both wrong moves were
available**: quote whichever run I happened to have first, or treat the
difference as a defect and go hunting. Reconciling at ONE grain before quoting
either is what turned an apparent contradiction into a stronger item. The cause
is now MEASURED, not inferred, because the entry lane held the other half of the
evidence and supplied it on request: both new rows are theirs and both are NEW
RETURNS rather than rows changing state — one with 5,825 employer contributions
against a 9,550 limit (**no excess**, which is why rows moved 68→70 while excess
moved only 5→6) and one with 8,979 against 8,550 (**the 429**). The
decomposition is exact. ⭐ Splitting MEASURED from INFERRED *before* handing the
number on is the S-18 discipline applied before it cost anything.*

*⛔ **KEN — NEW: S-19** (states lane, staged): `R-8889-EXCEPTIONS` states the
same narrow `line 2 > line 13` condition the code had, and its diagnostic
message still says the 6% excise is *"not computed here"* — stale, since Form
5329 Part VII computes it from line 47. Spec and engine should tell one story.
⚠ The states lane checked and DISSOLVED a third suspected defect in the same
rule (i5329 names line 12 where the spec compares line 13, but `line 13 = min(
line 2, line 12)` makes those arithmetically equivalent) — recorded so a false
finding cannot be used to argue the true one down.*

*🔎 **The "not keyed vs not obtainable" class reaches its THIRD instance**
(entry lane, s306): `D_5329_003` holds a packet because the 12/31 HSA account
value is blank — and that value lives on the year-end HSA statement, which a
Lacerte partial-return print does not carry, so the hold cannot be cleared by
further reading. ⭐ It is the cleanest instance of the class because **the
direction of error is known and one-way: `hsa_value` can only ever REDUCE the
tax**, so a guess could silently under-report while its absence cannot make the
answer wrong (that return charged the full excise and ties). The other two are
`D_EFILE_001`'s two EIN holds, where a wrong guess is undetectable downstream.
That asymmetry is the argument for allowing a documented-absence designation on
the 5329 hold *before* the EIN ones.*

*🔴 **s306b (same session, LIVE REGRESSION found while Ken was blocked by it):
the Add W-2 button had been 400ing on EVERY return.** Reported through the entry
lane as "adding a W-2 throws a 404 / the route is missing". It was neither.
**s287 (Ken item 11, "no placeholder name") changed the button to POST
`{employer_name: "", wages: "0.00", federal_tax_withheld: "0.00"}`; nothing
relaxed the server, where `W2Income.employer_name` was `CharField(max_length=255)`
with no `blank=True`** — so DRF answered `400 {"employer_name":["This field may
not be blank."]}` to the exact payload the UI sends, for everyone, on every
return. **Fix: `blank=True, default=""` + mig 0370** (`ec00bcb3`). A nameless row
is a legitimate INTERMEDIATE state (add the row, then type the name) and cannot
reach a filed return — the MeF extract already refuses a W-2 with no employer
name BY NAME and the diagnostics already render it "employer not named", so
completeness stays at those gates rather than a create-time 400 that blocks data
entry outright. 5 new tests + 84 adjacent (W-2 / EIN-autofill / e-file extract).
**Deploy `dep-da8b65tbedkc73agspg0` (prep) + the demo twin, BOTH API-confirmed
LIVE on `ec00bcb3`. VERIFIED FUNCTIONALLY, not just deployed:** a POST of the
exact UI payload to the real route returned **201** with `employer_name=''`,
followed by a 204 cleanup DELETE. ⚠ That live POST was run against **DEMO, not
prep** — proving it needs a real write and every prep return is a live client
record (one mid-edit by another session). Demo serves the same commit and the fix
is a model-level constraint in code, so demo passing proves prep; prep is
API-confirmed live on that commit and its deploy runs `migrate --noinput`.*

*⚠⚠ **s306b's METHOD LESSON — A PROBE AND A SYMPTOM LOOK IDENTICAL IN A REQUEST
LOG.** The escalation named a 404, pointed at two of the day's deploys, and
offered a list of 404s as corroboration. **Every one of those 404s was the
reporting lane's OWN URL-guessing** — `/api/v1/w2s/`, `/api/v1/income/w2s/` etc.
carry a `WindowsPowerShell` user-agent, and `/api/tax-returns/...` simply lacks
the `/v1/`. The app generates exactly ONE 404 (`GET .../prior-year/`, benign, and
it fires 6 seconds before the failing POST, which is why the two got conflated).
**The real failure was a 400, and two of the three predate both deploys**, so
neither caused it. ⭐ What identified it was the RESPONSE SIZE: the body is 50
bytes, and enumerating every serializer field × every standard DRF message showed
only one 50-byte single-field error that the UI could provoke. ⭐ Second keeper:
**`OPTIONS` on that endpoint advertises twenty TAX-RETURN fields and no
`employer_name`**, which reads exactly like "the wrong serializer is bound" — it
is a DRF artifact (`TaxReturnViewSet.get_serializer_class()` returns the LIST
serializer for every action but `retrieve`, and metadata is built from it), so
**OPTIONS on any `@action` of that viewset describes a tax return.** It means
nothing; it is pinned in the test docstring so the next person does not lose the
same hour. The end-to-end test drives the real route with the real payload —
proof by behaviour, not by reading.*

*⭐ **s306c (same session, Ken's direct go): THE GENERAL-CATEGORY FORM 1116 IS
BUILT** (`6930e093`, deploy `dep-da8c5hv10e5c73bobam0`, API-confirmed LIVE and
PROD-VERIFIED — line 28 seeded with its right label, `general` defers on
nothing, `951a` still refuses, the mixed guard fires). ⭐⭐ **Sized as a
multi-session storage-model change; it was ONE session, and re-reading the
engine before writing is what found that**: the §904 limitation (Parts I+III) is
**category-AGNOSTIC** — the same computation applied separately per basket — so
general needed NO new arithmetic. Authority verbatim from the downloaded 2025
i1116 + the IRS face: *"a separate Form 1116 for each category of income"*, and
Part IV gives each basket its own line (**27 passive · 28 GENERAL**). Shipped:
the gate narrowed to {passive, general} (`non_passive_category` renamed
`unsupported_category`); the credit on ITS OWN Part IV line (was hard-coded 27,
so a general credit would have printed on the passive line); line 28 seeded +
both lines persisted; the print widget pinned **POSITIONALLY** against the
caption, never inferred from the field-name sequence (s285). ⚠⚠ **THE REAL TRAP:
the foreign-tax SOURCE.** `_foreign_tax_total` added the 1099-INT/DIV aggregate
to EVERY Form 1116 — that aggregate is **passive-basket tax by definition**
(payee-statement income), so a general form would have absorbed it and inflated
the general basket, a §904(d) violation producing a **plausible wrong number
rather than a failure**. A general form now takes only its keyed
`additional_foreign_tax`. ⚠⚠ **AND THE MeF HALF, the same class as a detector
that cannot fire:** the builder hard-coded `ForeignIncPassiveCategoryInd`, so a
general credit would have **TRANSMITTED UNDER THE PASSIVE INDICATOR** — a silent
wrong filing. Indicator + Part IV element now category-driven, verified against
`IRS1116.xsd` directly. **THE SAFETY PROPERTY** (what makes single-category
support safe without the one-row-per-category model change): two shapes the
singleton cannot hold refuse BY NAME — `D_1116_010` (a general form beside 1099
foreign tax = a genuine two-basket return) and `D_1116_011` (§904(j) is
passive-only by statute). ⚠ The `form_1116s` back-entry shape is UNCHANGED — no
new keys, no model change. Gates: 614 (all 1116 suites + 526 flow assertions) +
680 e-file/MeF. RS amendment staged with the states lane (spec, `R-1116-SUMMARY`,
`FA-1040-1116-07`, and the two new diagnostics).*

*⭐⭐ **s306d — THE CREDIT ORDERING IS CONFIRMED, AND THE TIE THAT "PROVED" IT WAS
HIDING A REAL $1** (`86db00c0`, deploy `dep-da8cs2m417fc73d99e5g`, LIVE). The
entry lane committed the general-category return TIE and then refused to let
that stand as proof — their reasoning exact: **if the ODC had wrongly stayed at
500, the FTC would simply be limited to the remainder, line 21 would still total
the tax, and 22/24 would still be zero — the return would TIE WITH BOTH CREDITS
WRONG.** Reading the committed return's stored lines settles the ordering: **19 =
70, not 500**; 20 = 9,892; 21 = 9,962; 22/24 = 0; the credit on **line 28
(general)**, zero on 27. ⚠ **But the filed return shows 9,893 / 69 to our 9,892 /
70 — identical total, which is exactly why it tied.** MEASURED cause: line 19's
§904 ratio was quantized to FOUR decimals (`L17 75,997 / L18 76,530 =
0.9930354…`; 4dp → 9,892, 5dp → 9,893, **exact → 9,893**). **i1116 rounds this
ratio to "at least four places" — FOUR IS A MINIMUM and we emitted the least
accurate value permitted.** FIVE is the ceiling, not a preference: MeF
`RatioType` is `fractionDigits=5`/`totalDigits=6`. Prod census BEFORE the change
(s302 rule): **8 full-path returns, 7 unchanged, 1 moves by exactly $1 — into
agreement with the filed return.** ⚠⚠ **LINE 3f STAYS AT FOUR DECIMALS, AND THAT IS NOW A MEASURED DECISION, NOT A
DEFERRAL.** The entry lane flagged a live return whose vendor-printed 3f carries
SIX decimals and where the ratio actually binds, predicting a $1 divergence.
**Both halves were checked and both say leave it alone:** (a) their arithmetic
does not reproduce — `0.6616 × 23,625 = 15,630.30 → 15,630`, which MATCHES the
printed line 6, as does the 6-decimal ratio, so the only real witness available
says our current 4dp answer is RIGHT; (b) the prod census says moving 3f to 5dp
would shift **5 of 8** full-path returns' line 3g by $1 (vs 1 of 8 for line 19),
with **no filed-return witness anywhere saying the new value is better**.
⭐ **The asymmetry is the point: line 19 changed because a filed return PROVED
4dp wrong; 3f must not change because the only evidence points the other way.**
`ForeignIncomePct` is also `RatioType` (5dp cap), so 4dp transmits fine. Gates
614 + 620 MeF.
⭐ **The keeper: a TIE is not evidence when two errors can offset.** Ask what the
green result was CAPABLE of catching — the same class as the vacuous SE fixture
and the s303 timestamp. It was caught only because the lane refused to bank an
unverified claim, and because the stored lines were readable after commit.*

*⭐ **s306e/f (same session): two Form 1116 gaps and the state-lane silent-value
class** (`0b6b6bb6` + `b2d0dd7c`, deploy API-confirmed LIVE). **① Line 3d was
NOT EXPRESSIBLE** — `compute_part1` always accepted `gross_foreign_source` and
NOTHING EVER SUPPLIED IT (no field, no allowlist key, no caller), so 3d fell
back to line 1a. Right only while 1a is UNADJUSTED; under §904(b)(2)(B) the two
diverge hard (1a 17,964 vs gross 3d 82,309 on the reporting packet → ratio
0.144389 instead of 0.661573, standard deduction under-apportioned by ~12,000).
Model field + mig 0371 + allowlist + a description saying plainly it is NOT 1a;
`deduction_apportion` also documented as the 3a INPUT, not the 3g result.
**② Line 18's adjustment exception was ASSUMED** — it is evaluated from two
`default=0` preparer fields, so unkeyed is indistinguishable from stated-zero.
i1116 is explicit the other way for the shape vendors print: *"You can't make
this election if you have any foreign qualified dividends or foreign capital
gains … and you made adjustments to those amounts when you completed lines 1a
and 5."* Prod census: **3 of 9 full-path returns sit in that blind spot** (one
with $75,796 of worldwide QD+gains). `D_1116_012` (warning, not RED — the
engine sees only WORLDWIDE amounts and some of those returns are right) names
the amounts and the two-part test. **③ s306f — state BOOLEAN control lines are
now TYPE-checked**: the vocabulary threw away the seeder's `FieldType`, so a
string in NC's `PYNR` staged clean, Schedule PN never engaged and NC taxed 100%
of a nonresident's income (**$4,780 overstated, and it read as an engine
defect**). Never a one-off — **30+ boolean control lines** across all eight
installed state forms had the same blind spot. Gates 616 + 175 + 6 new.*

*⚠⚠ **s306e/f, TWO errors of my own, both caught before they reached anyone.**
(a) `D_1116_012` used `_dec`, which does not exist in `rules_1116` — the runner
records the NameError AS the rule's finding, so the rule appeared to FIRE on
every return, **including the fixture written to prove it stays silent. The
positive test passed throughout; only the NEGATIVE test exposed it.** A rule
that fires everywhere looks like a working rule. (b) I grepped for
`"RIE-TP-DIS"`, found it only in the seeder, and was one message from telling
Ken the GA disability exclusion was "seeded but consumed by nothing" — **it is
consumed, via a dynamically built key** (`truthy(f"RIE-{p}-DIS")`), exactly the
s272b lesson (*the grep lied because the name was constructed*). s274 built that
gate deliberately, citing O.C.G.A. §48-7-27(a)(5). ⭐ **A literal grep cannot
prove an absence when the key is assembled at runtime.***

*⭐ **s306e/f AFTERMATH — both shipped items PROVEN on the live packet, and one
of my near-misses reversed by the lane.** ① **Georgia TIES**: the disability
retirement exclusion was keyable all along (`RIE-TP-DIS` + `-DIS-DATE` +
`-DIS-TYPE`) — S1-13 48,933 / RIE-TP-17 35,000, the 1,817 gone. **The lane had
reported "no way to state disability status" as a blocker and has retracted it**;
I nearly confirmed it from my own bad grep. ② **The 1116 3d fix is proven by a
PREDICTED-then-observed experiment**: the lane predicted before running that the
error would flip from 604 LOW to 259 HIGH, and it did to the dollar (credit 1,028
→ 165 against a correct 424, with 3f now 0.661573). **That isolates line 18 as
the SOLE remaining cause.**
⭐ **FEASIBILITY MEASURED (this changes the sizing of Ken's decision ① below):
the Worksheet for Line 18 is MUCH smaller than "build a worksheet" —
`compute_intdiv.compute_qdcgt_worksheet` already produces all 25 QD&CG lines,
so the 0%-rate slice is `ws_9`, the 15% slice falls straight out of it, and
`ws_5` IS the very line the adjustment exception's own threshold test names.**
The 28%/25% rows are unreachable anyway (a return with DIV 2b/2c/2d is already
ROUTE_BLOCKED). Answer key in the packet: worldwide 15% gains 24,499 × 0.5946 =
14,567 + 40,032 of 0% gains = 54,599 off 89,249 → 34,650.*

*🔎 (s306f) **A Schedule SE wage-base ATTRIBUTION warning was sized and NOT
built — deliberately.** A reported "SE blocker" re-run turned out not to be one:
the engine reproduces the FILED figures exactly when line 8a is populated
(1,786/893) and the reported wrong ones when it is 0 (9,422/4,711), and
`se_line_8a_for` zeroes 8a only when the W-2's `owner` differs from the
ScheduleSE `proprietor` — correct under §1402, which applies the base per
individual. Prod census for a would-be warning: **2 of 199 ScheduleSE rows** sit
in that shape, so it would be signal rather than noise. ⚠ **Not built because
the actual cause on that packet is UNCONFIRMED** (unkeyed box 3, an owner
mismatch, and a nonzero keyed `w2_ss_wages` override are three different
defects wanting three different detectors). Building now would be building to a
hypothesis — the same trap the lane correctly refused to hand me on the rental
recharacterisation.*

*⭐ **s306g — SC Schedule NR line 46 no longer defaults to zero in silence**
(`fdf3492f`). NR-46 ("standard or itemized deduction") is a preparer INPUT
consumed as `L47 = NR-46 × the proration %`; unkeyed it defaults to 0, the
allowable deduction becomes 0, and the SC brackets run across the ENTIRE
SC-source AGI — **$1,919 of tax the filed return does not have**, arrived at
silently (428 + 6% × 232,775 = 14,394 to the dollar). `D_SC1040_NR46` (warning)
names the ABSENCE and **explicitly says not to back-solve the value**: it is not
derivable from the federal return (neither the full federal itemized total
12,436 nor that less state income tax 13,268 reproduces the filed 12,475), and a
rule nudging anyone toward reverse-engineering 41,463 would be worse than
silence. 7 new tests — **four of them assert SILENCE**, deliberately, after the
`D_1116_012` NameError this session showed a positive-only test passes while a
rule fires on everything.
⭐ **THE SILENT-DEFAULT FAMILY IS THREE DIFFERENT BUGS WEARING ONE FACE**, and
only one was a type problem: a COMPUTED output keyed as an input (`ga500_fields`
"S1-7" — still open), a DISCARDED FieldType (the 30+ state boolean control
lines — fixed s306f), and an UNKEYED REQUIRED INPUT defaulting to zero (NR-46 —
now named).*

*⭐ **The reported "SE wage-base re-confirmation" was NOT a defect** — the entry
lane had keyed box 4 (social security TAX) and not box 3 (wages), so line 8a
derived nothing. s302d works: with box 3 keyed every federal line ties. ⚠ **The
lane's arithmetic was right to the cent and named the wrong component** — the
keeper is *the numbers you observe are evidence, the component you name is a
hypothesis*. ⭐ Holding the detector was vindicated: the owner-mismatch warning I
sized (2 of 199 rows) would have been DEAD CODE, because the owners matched.*

*⭐ **s306h/i (`add00472`, deploy verified): "the printed face shows RESULTS;
the importable vocabulary wants INPUTS."** ⚠⚠ **THREE reported "no route /
silently ignored" blockers were ROUTES THAT EXIST UNDER THE INPUT'S NAME** —
the federal NOL is `carryforward_attributes` kind `nol_regular` (feeding
`compute_form_172` → Sch 1 line 8a as a negative; `taxpayer.amt_nol` is the AMT
NOL, a different figure); the Georgia NOL is `S4-CF-PRE`/`S4-CF-POST` (line 15b
is COMPUTED: `pre_applied + post_applied` under the 80% limit); and the
retirement exclusion is the `RIE-*`/`MIL-*` lines (S1-7 is COMPUTED). **s306h:
`ga500_fields` had NO vocabulary check at all** — only "string key, scalar
value" — so a keyed computed line was accepted and recomputed over, while
`state_returns` has warned since BATCH-296 #41. It now warns and NAMES THE INPUT
to use instead. A warning not a refusal (`ga500_fields` commits
`is_overridden=True`); a null stays a CLEAR.
**s306i — `D_K1_7203_DIST`, and the finding is worse than "not implemented":**
the §1368(b)(2) excess-distribution gain **was already computed**
(`compute_7203`'s `_excess_distribution_gain`, which the 1040 side calls) and
**nothing ever read it**. It is deliberately not auto-entered (Ken s205: the
holding period is a preparer fact, never guessed) and the entity-side
disclosure rides the K-1 IMPORT OFFER, which never happens for a back-entered
return. ⚠⚠ **So the engine omitted the gain, a source return that also omitted
it TIED, and the tie carried no information** — the lane caught one at 12,583
and said so rather than banking it. The warning states outright that a return
omitting it will still reconcile.*

*⚠ **s306h/i process note against myself: A STASH-AND-COMPARE IS NOT A
CONTROLLED EXPERIMENT WHEN THE DATABASE CARRIES OVER.** One wide run threw 7
setup ERRORS; I stashed the change, the reverted run came back clean, and I
nearly recorded "the errors are mine" as proven. The reverted run had reused a
WARM DB. Two later runs WITH the change were clean (44 passed) and a
`--create-db` run of the whole neighbourhood is **318 passed** — the failing run
was the known reuse-DB seed-leakage class and does not reproduce. ⭐ The control
has to include the database, or the comparison measures the cache.*

*⭐ **s306j (`3f8948cc`) — engine-owned carryforward figures now announce that
they are OVERWRITTEN.** `CarryforwardAttribute` makes all three amounts inputs
on purpose (the lane transcribes the FILED worksheet, and a filed pool can
legitimately show a remaining balance that is not `original − used`). But for
`ENGINE_COMPUTED_KINDS` (§179, charitable, regular NOL) `compute_form_172`
recalculates `amount_used_current_year` / `remaining_amount` and writes them
back — so those keyed figures are **not ignored, they are overwritten, and the
value reads back CHANGED rather than absent**, which is why a keyed absorption
produced a byte-identical dry run. The warning names the doomed figures, says to
key `original_amount` + `source_tax_year`, and adds the clause that matters
most: **if the engine's absorption disagrees with the filed return that is a
FINDING to report, not a figure to transcribe over.** Silent for every other
kind, where transcription is the only source. **Fourth home for one confusion,
all now covered: `state_returns[].fields`, `ga500_fields`,
`carryforward_attributes`.** 8 new tests (four assert SILENCE) + 145 adjacent.*

*⭐⭐ **s306m/n (`22958b19`, deploy LIVE) — DRY RUNS NOW ECHO DIAGNOSTICS, and
that closes the worst-shaped gap on the lane.** A `DiagnosticRun` only exists
after a COMMIT, so **a HELD packet had no diagnostics at all** — and held
packets are precisely the ones where a diagnostic explains the gap. The entry
lane dumped a dry run and measured that there is no `diagnostics` key anywhere:
`D_SC_007` had fired at ERROR and was invisible to the only interface they have.
⚠ **My "you did not look" was the wrong half — they looked; the looking had
nowhere to land.** The dry run already performs the whole commit in a
rolled-back transaction, so the findings exist at that moment.
`commit_staged_return` gains `include_diagnostics` (default OFF so real commits
are unchanged); the view defaults it ON for dry runs. Shape: `rule_code` /
`severity` / `message`, from the SAME runner the app uses; wrapped so a
diagnostics failure can never break a commit.
**s306n — THE s225 DEFECT RECURRED**: staging has accepted the nested
`form_8829` since s272 and the generator never emitted it, so an author
validating OFFLINE got a **FALSE REFUSAL for a field the server takes** — which
is exactly why it was reported as "no route". Now generated from
`F8829_FIELDS` (30 properties, verified published). ⚠ SUPPORTED-SECTIONS' "neither
can drift from what staging accepts" is TOO STRONG — it holds only until a
feature ships after the last regeneration, and this is the second instance
(`schedule_fs` detail rows were the first, s225).*

*⚠⚠ **s306m, against myself — THE VIEW SERVED TWO LANES AND I CHANGED ONE
SIGNATURE.** Adding `include_diagnostics` to the 1040 `backentry` module alone
**500'd EVERY entity-lane commit** (`TypeError: unexpected keyword argument`) —
**136 failures**. The s295b lesson (one shared caller, two implementations)
arriving as a live 500 rather than a quiet divergence. ⭐ Caught only because the
sweep was run at all: the change itself was right and the BLAST RADIUS was not
where I looked. Both signatures are now pinned together by test, and the entity
lane imports the ONE helper rather than growing a copy. ⭐ Also re-confirmed the
quintet discipline: the 5 residual failures are the known files and pass **15/15
on `--create-db`** — re-diagnosed, not inherited.*

*⭐⭐ (s306l) **THE FIFTH REPORTED "NO ROUTE" THAT EXISTS, AND THE SECOND
"SILENT" THAT ISN'T — NOTHING BUILT.** The actual-expense **Form 8829 route
EXISTS**: not a top-level section (which is why it wasn't found) but **NESTED
under `schedule_cs` as `form_8829`**, because `Form8829`'s only FK is
`schedule_c` — no `tax_return` column, so it cannot be a top-level list and
nesting makes the activity link free (s272, BATCH-296 #26). `F8829_FIELDS`
carries the whole indirect column (`excess_mortgage`, `repairs_indirect`,
`utilities_indirect`, `other_indirect`, `insurance_indirect`, `rent_indirect`,
`casualty_indirect`) plus areas, the §280A cap inputs, the Part III basis facts
and both opening carryovers; every COMPUTED column is deliberately excluded.
⚠ And the reported "sqft present + no flag silently yields ZERO" is **NOT
silent — `D_SC_007` fires at ERROR severity** on exactly that predicate
(`sqft > 0 AND use_simplified is not True AND no Form 8829 row`), naming both
remedies.
⭐⭐ **THE REAL FINDING IS THE VISIBILITY GAP, AND IT IS BIGGER THAN THE
BLOCKER:** the lane measured an AGI delta, inferred silence, and did not see the
error the engine had already raised. **A lane reading only reconciliation deltas
misses every diagnostic the engine emits** — which would have short-circuited
this, the SC NR-46 one and the GA 15b one. Raised to them as the higher-leverage
fix. ⭐ Five reported "no route"s have now all existed (federal NOL, GA NOL, GA
disability, GA retirement total, actual-expense 8829); the measurements were
right every time and the INFERENCE from them was what missed.*

*🔎 (s306k) **A reported "the browser may default a W-2 EIN from the firm
record" trap CANNOT EXIST — the `Firm` model has NO EIN FIELD** (concrete fields
are id / name / is_active / created_at / updated_at), so there is nothing to
default from. Measured rather than only read: **3 of 845** prod W-2 rows carry
the firm's own EIN, one of them named `'New Employer'` (the OLD placeholder
retired in s287, so it predates the change and was hand-typed); zero 1099-R
payer rows; no EIN over-represented in a way suggesting a leaking default.
**Deliberately NOT built:** the only way to express "employer EIN == the firm's
EIN" today is to hardcode a real EIN in source — wrong on its own terms and
against the no-PII rule. If a firm EIN field ever lands for e-file, the check
becomes trivial and is worth having then. ⛔ **The 3 rows are wrong data on real
returns — cleaning them is Ken's call, not a unilateral write.***

*🔎 **3517's NOL absorption — ⛔ Ken's, as a SOURCE question. ⚠ MY OWN
POOL-SIZE HYPOTHESIS IS REFUTED** — the packet's Statement 1 prints it: 190,783
available, 16,131 absorbed, 174,652 carried to 2026, and the arithmetic closes,
so 190,783 is the pool ENTERING the year and `min(pool, 80% × base)` cannot be
the limiter. Cost ten minutes to eliminate and stops an unexamined assumption
sitting under Ken's ruling.*

*🔎 (original framing, now settled)* Engine absorbs 20,959 (= 80% of a 26,199 base — the
§172(a)(2)(B) cap, textbook, and the lane agrees the BASE is right because
Lacerte's own worksheet prints the same 26,199); Lacerte absorbs 16,131
(61.6%). The lane ruled out a QBI-net base (gives 16,786) and the SEHI/PTC
circularity (the whole SEHI deduction is 968). ⚠ **Open possibility raised back
to them: the deduction is `min(pool, 80% × base)`, so if the remaining POOL were
16,131 the cap never binds and there is no §172 disagreement at all** — the
190,783 "carryovers to 2026" figure is the pool AFTER absorption, not before.
Confirm which figure was keyed as `original_amount` before framing it as an
engine gap. ⭐ **s306q NARROWS THIS TO EXACTLY THAT QUESTION**: the federal 80%
formula is now verified verbatim against IRC §172(a)(2)(B)(ii) and pinned by
tests, so `min(pool, 0.80 × (base − pre2018))` is not in doubt. With no
pre-2018 component the engine's 20,959 is arithmetically forced from a 26,199
base — **so either the base or the POOL is the disagreement, and neither is a
§172 defect.** ⚠ Georgia's pools are genuinely NOT in the packet and must not be
derived from the federal ones — this return is the proof (16,131 federal
absorbed vs 23,759 Georgia).*

*⛔ **KEN — DECISIONS OPEN FROM THE ENTRY LANE'S PACKETS (none blocking, the
lane is holding correctly). ⚠ KEN ANSWERED SIX OF THESE 2026-08-28 — the status
below is post-ruling:**
· ⓪ **the Georgia SALT add-back — ⛔ STILL KEN'S** (*"Let me think on it and ask
me again later"*). ⚠⚠ **A CONFLICT MUST REACH HIM BEFORE THE FIELD IS
DESIGNED**: the states lane's spec `R-GA500-DED` implements IT-511's literal
**proration** formula while Lacerte does `max(0, 5d − cap)`, and on the entry
lane's 9 resident packets the booklet-literal rule gives **0**. The
resident/nonresident paths also differ. Do not build until that is resolved.
· ⓪b ~~`state_income_tax_payments` does not reach GA line 26~~ — **✅ DONE
(s306p)**, and the route had existed since s277; the bug was a predicate
demanding an OPTIONAL field.
· ① ~~the Worksheet for Line 18~~ — **✅ BUILT (s306, Ken's go)**.
· ② **a per-property NONPASSIVE lever — ⛔ STILL KEN'S** (*"I need to look this
up at the office. Ask me about this later"*). The lane deliberately withheld a
mechanism because their §1.469-2T(f)(3) hypothesis does NOT separate the two
parcels, so building to it would encode an unsupported rule.
· ③ ~~`ga500_fields` does not warn when a COMPUTED line is keyed~~ — **✅ DONE
(s306h)**.
· ④ **the 3 firm-EIN W-2 rows — ⛔ STILL KEN'S** (*"I'll be at the office in a
couple of hours and I will pull up the return and direct you then"*). Wrong data
on real returns; cleaning them is his call, not a unilateral write.
· ⑤ ~~the NOL rules~~ — **✅ VERIFIED (s306q)**: both jurisdictions correct, the
divergence is required by law and is now guarded. See the s306q entry above.
**NEW from s306q, staged in `REVIEW_QUEUE.md`:** whether `D_GA500_009` should
drop from **error to warning** (as an error it is unacknowledgable and
permanently blocks closeout on every Georgia return with an NOL — `backentry.py`
Gate 4 — even though the computation it warns about is correct), and whether to
build **Georgia Schedule 4 Parts I & II** (`S4-8` / `S4-NB-18` are seeded
`is_computed=True` with nothing writing them; the full IT-511 instructions were
extracted this session, including the Part II line 6 **nonbusiness RIE
proration**, which is substantial enough to want its own unit).*

*⚠ **s306c, against myself — I broke my own standing rule.** A PS5.1
`Get-Content | Set-Content -Encoding UTF8` rewrite of a test file double-encoded
it (every em-dash mangled, BOM added) — the exact rewrite the hazard list BANS.
Caught only because a following Edit would not match; repaired by reversing the
cp1252→UTF-8 round trip, and the whole working diff was scanned for mojibake and
BOM before commit. **The rule exists because the damage is invisible in a
terminal and survives into the commit.***

*🔎 Carried, NOT built: **multi-category (general AND passive on ONE return)**
remains the larger unit — the `Form1116` OneToOne→FK change + all three
registries + `SINGLETON_SECTIONS`, a real Part IV across baskets, multi-face
render, multi-document MeF, per-category §904(c) carryover. At least 10 clients
in the Lacerte set file that way; they refuse by name today.*

*▶ NEXT: **item ⑦'s SINGLE-CATEGORY half is DONE (s306c); the MULTI-CATEGORY half
is not started and not ordered** —
multi-session and it changes a model (OneToOne→FK + all three registries +
`SINGLETON_SECTIONS`), so it wants Ken's word before it starts. Then extractor
follow-ons by the **RE-MEASURED (r17) residual, which is NOT the ranking s303
left**: f8995 coverage (6 solo / 128 touched) > asset_detail (4) >
student_loan_educator_wks (4) > sch_c (3) = **line-20 Sch 3 face class (3, was
"5")** = f8880 (3) = f5695 (3) = f8889 (3). ⚠ Every one is an UPPER BOUND until
the corpus re-runs with that class open (fifth confirmation). Open BATCH-296
entry-lane items still unworked: the `div_1099s.us_government_income` → GA S1-10
auto-derive design (needs an off-switch decision, s237 class) · the D_SCHD_006
QOF import surface.*

*Peer state (s306): I hold the tree + test_postgres; both lanes confirmed no
collision at boot. **Entry lane is UNBLOCKED and running — 33 packets filed**
(auth good, Ken gave explicit `--prod-db` go in their session). Their open holds:
two on Ken's 2024 figures, one sanctioned Pub 974 method difference, one on the
general-category 1116 (item ⑦ — that packet is not enterable at all). States lane
holds neither the tree nor test_postgres; RS suite 254 passed / 0 failed.
⛔ **NOTHING of S-14…S-19 has been ruled** — the states lane's standing warning
applies: if any lane says Ken ruled one, that is a RELAY, check with him directly
(three relays on 8/25–26 differed from what he meant or were retracted).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS (WHEN BUILDS RUN)
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax` (srv-d6geloa4d50c73el2trg); demo = `tts-tax-demo`. ⚠ **A push
is not a deploy — CHECK THE DEPLOY STATUS after pushing** (API key in
`D:\dev\Passwords & Secrets\render-api-key.txt`). **Standing push
authorization (Ken 2026-08-23): push at own judgment; verify every deploy;
hold only for a named reason.** ⚠⚠ **ORDERING (s279/s282): push → deploy
LIVE → seed → verify — and the deploy ITSELF seeds (`build.sh seed_all`
auto-discovers `seed_*` at BUILD time).**
- **s306 deploy — API-CONFIRMED LIVE 2026-08-27: `64c4dc15` →
  `dep-da8ap4ia6suc73bdidtg`** (finished 21:35Z), the currently live deploy. It
  carries all three s306 pushes: `1ae86753` (the D_8889_EXCESS widening + the
  4547 casing normalisation + the schema descriptions — its own deploy
  `dep-da8aocpt0dsc73bvgfu0` BUILT and went update_in_progress before being
  superseded, which is the normal deactivation), then `e1754760` and `64c4dc15`
  (markdown only). **No migration, no seeder, no schema-vocabulary change** — the
  published back-entry schema WAS regenerated, but only field `description`s
  moved, so no allowlist or enum shifted. ⚠ The diagnostic rule's name/description
  reach prod because `seed_builtin_rules` is `update_or_create` and the deploy
  seeds at BUILD time; severity is unchanged (`warning`), so no rule row changes
  behaviour beyond the widened condition.
- s303 deploys, BOTH API-confirmed LIVE 2026-08-27: `54c187ba` (head of
  `5d0c33c5` + the scratch-file removal) → `dep-da8377cs728c73cubirg` =
  the staging-predicate fix itself; then `0d7673c9` (carrying `692d899d`,
  the empty-array test, + the session-close docs) →
  `dep-da83q215efls7394k1vg`, **the currently live deploy**. No migration,
  no seeder, no schema regeneration in either (the change is a staging
  predicate + docstrings; no vocabulary or allowlist moved).
- s302 deploys: `9999f2c6` → `dep-da7q5rek1f9s73chfqe0` (⑥, mig 0367 +
  D_K1_PTP_469G seed) — **API-confirmed LIVE**; `a9f97025` →
  `dep-da7qalvlk1mc738cgd60` (s302b line 26, no migration) —
  **API-confirmed LIVE**; `f882e494` → `dep-da7qdacs728c73cml8i0` (s302c
  D_EFILE_004 seed, no migration) — **API-confirmed LIVE**; `ffedba4d` →
  `dep-da7qp749v7es73bl9psg` (s302d SE line 8a, no migration) —
  **API-confirmed LIVE**. All four verified 2026-08-26.
- s301 deploys: `d6ab0e42` → `dep-da7ovk6417fc738ffrq0` · `03c96795` →
  superseded by `2a2bb807` — LIVE (s301b e8960 surface confirmed by the
  entry lane's committed 1723).
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (s302 regenerated; current as of `9999f2c6` — s302b/c changed no schema.)

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. s302 coordinated at boot;
three annexes appended to BATCH-296 (every load-bearing exchange annexed).
Peers stage; Ken decides. ⚠ A peer's factual claim can be RETRACTED
minutes later (the s302 "second fixture" correction) — build only against
what a batch file or your own probe shows.

## ⚠ Known red / rotted — THE ONE LIST (post-s302)
- ~~12 of 47 RS `check_*_integrity.py` gates FAIL~~ — **THE 12 WAS WRONG AND
  S-10 IS CLOSED** (states lane, 2026-08-27, correcting their own s302
  number). Their triage: **3 were never failing** (two pass every check then
  die printing a non-ASCII success line to a cp1252 console; one exits
  `ImproperlyConfigured` importing a loader without configuring Django) and
  **2 were their own defects from hours earlier**. True pre-existing count
  **7**, of which **10 of 12 are resolved and 10 gates are now pinned in
  `tests/test_integrity_gates.py`**; RS suite **254 passed / 0 failed**.
  ⚠⚠ **The sweep that produced the 12 read EXIT CODES without reading
  OUTPUT** — the same one-signal-generalised error class as s303's own
  timestamp misreads. ⭐ Their lesson: **a red gate does not merely fail to
  catch new defects, it CAMOUFLAGES them** — two fresh defects hid inside
  pre-existing redness, in gates nothing runs. ⚠ S-10a survives separately:
  `R-1040X-SUPERSED` has no authority link, and "superseding" appears ZERO
  times in the 2025 i1040x (they pulled and committed it as evidence), which
  also casts doubt on the sibling rule's existing citation. S-10c
  (`D_8582_PTP`'s unwritten recompute) also stands.
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof. (s302: the two s258
  fails appeared again in the sweep and PASSED in isolation — unchanged.)
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ~~the GA700 answer-key coverage test~~ — REPAIRED s302 (was red since
  s274; greped only FORMULAS_GA700, blind to the pull-map writes).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s302).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): five phantom failures. New files/markdown/scratchpad only.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c`; a script run by absolute path needs
  `sys.path.insert(0, server)` (s298). ⚠ `python -m taxwise1040`
  does NOT resolve from `server\` — run the package's `__main__.py` BY
  PATH (s297).
- 🔧 ⚠ **`BaseModelSerializer` partial updates save ONLY the patched
  columns** (s298) — a serializer-side derived write must be injected into
  `validated_data`, never `setattr` on the instance (it silently no-ops).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ (s302 re-confirmation: `-replace` +
  `Set-Content` mangled an em-dash annex — python io did it clean.)
  ⚠ Embedded double-quote in a here-string arg to a NATIVE exe SPLITS the
  argument. ⚠ `$1` in an unquoted PS arg EXPANDS to empty. ⚠ PS
  `Sort-Object -Unique` on a one-element pipeline UNWRAPS to scalar (s296).
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash. ⚠ A
  COMMITTED return refuses plain stage+dryrun with HTTP 409 (s298 —
  verification route = the in-place rolled-back recompute, s289 pattern;
  s302 ran the 1792 probe this way clean, incl. the commit reconciliation).
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ **`str(Decimal)` preserves the STORED SCALE** — a diagnostic detail
  written as `str(amount)` reads back `"429.00"`, not `"429"`. Compare
  `Decimal(details[k])`, never the raw string (s306; two test failures).
- 🌐 ⚠ **A BACKGROUNDED pytest piped through `Select-Object` buffers ALL
  output until exit** — no interim progress at all, so a long sweep looks
  identical to a hung one. (The standing rule already bans that pipe; this is
  the detached-run reason.) Check liveness with the process's CPU time.
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); synthetic `insert_text` PDFs may
  LOSE leading spaces (s290) — the extractor parses POSITIONALLY everywhere.
  ⚠ **The AcroForm FILLER flattens ALL widgets out of its output** (s296).
  ⚠ **TaxWise RE-TYPESETS whole schedules** (s297): pin from packets only.
  ⚠ s301: a fixture that encodes the WINDOW's geometry instead of the
  CORPUS's keeps a blind spot green.
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section. Local templates beat both.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292–s302 ran probes this way). ⚠ Scripts touching
  client-named returns live in SCRATCHPAD or tax-test-data, never the repo
  (PII). ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295/6/7/s301).
- 🔧 ⭐ **DRY-RUN THE CORRECTION PASS AND READ EVERY ROW before --commit**
  (s298). ⭐ s302 corollary: **CENSUS THE PROD BLAST RADIUS before changing
  a computed line's source** (51 returns read, zero moved — the do-no-harm
  half; the failing case rides the tests).
- 🌐 ⭐⭐ **THE THIRD QUESTION (s302, states lane): "does anything ACT on
  it?"** — after "does it exist?" and "does it run?". A green detector
  nobody reads is functionally no detector, and it is HARDER to spot
  because it announces itself as a pass (D_2210_DATED fired correctly for
  weeks while line 26 dropped the money it was flagging). ⚠ s302d: the
  same question applies to a TEST — mine ran, was green, and measured
  nothing.
- 🔧 ⚠⚠ **A FIXTURE THAT KEYS A COMPUTED COLUMN MEASURES NOTHING** (s302d):
  `ScheduleC.net_profit` is written by compute from gross receipts, so a
  test that keys it gets line 31 = 0 — and every rate/ratio assertion then
  passes as `0 == 0`. Key the INPUT (`gross_receipts`), and assert the
  intermediate is nonzero BEFORE asserting anything about a rate applied
  to it.

## 🔎 Carried for triage — NOT claims
- (s303) **The simplified home-office 300 sq ft cap is applied PER SCHEDULE C,
  but Rev. Proc. 2013-13 §4.08(6) caps ONE TAXPAYER'S AGGREGATE across all
  qualified business uses of the SAME home** ("A taxpayer who has more than
  one qualified business use of the same home for a taxable year is limited
  to a maximum of 300 square feet ... must allocate the square footage among
  the qualified business uses"). `compute_schedule_c` caps `biz.home_office_
  sqft` per business, and BOTH diagnostics (D_SC_004 / D_SC_005) iterate per
  business — so one proprietor with two home-based Schedule Cs could claim
  600 sq ft, overstating by up to $1,500. **Prod census: ZERO returns hit it
  today** (17 simplified home offices, no proprietor with two). Latent,
  overstating, unfiled work. ⚠ The sibling rule is the OPPOSITE and the
  engine is RIGHT about it: §4.08(5) lets spouses sharing a home EACH use up
  to 300 sq ft of *different portions* — so the entry lane's 300+250=550
  packet is correct as filed, and a naive per-return aggregate check would
  wrongly flag it. Group by `proprietor`, never by return.
  ⚠⚠ **THE THRESHOLD IS AGGREGATE > 300, NOT "TWO ROWS" — do NOT ship an
  unconditional refusal.** I proposed refusing on "same proprietor, two
  simplified rows" (reasoning from §4.08(7), *"may use the safe harbor method
  for only one home for that taxable year"*). **The entry lane refuted it from
  the FIRST SENTENCE of §4.08(6), which I had already printed and read past:**
  *"A taxpayer who has more than one qualified business use of the same home
  for a taxable year and who elects the safe harbor method **must use the safe
  harbor method for each qualified business use of the home**."* Two elected
  rows in the same home are **MANDATORY**, not a defect — a refusal there
  would push the enterer to un-elect one, which is exactly what (6) forbids.
  The three cases for one proprietor with two simplified offices:
  **same home + aggregate ≤ 300 = LEGITIMATE AND COMPELLED** · same home +
  aggregate > 300 = defect under (6) · different homes = defect under (7).
  So: **aggregate > 300 per proprietor → refusable** (the missing home id then
  changes only which clause the message cites); **aggregate ≤ 300 → WARNING
  ONLY**, because compelled-same-home is indistinguishable from a (7) breach
  without a home identifier. `proprietor` + aggregate sqft is the key;
  `proprietor` alone is not.
- (s303) **§4.08(4) monthly averaging is unrepresentable in our data** — a
  business starting, ending or resizing mid-year must use *"the average of the
  monthly allowable square footage"*, no more than 300 in any one month, and a
  month counts only with *"15 or more days of a qualified business use"* (the
  Rev. Proc.'s own example: 400 sq ft from July 20 → average 125, not 300).
  `schedule_cs` keys ONE `home_office_sqft` with no in-service date, so a
  mid-year start silently claims the full-year amount. Same gap as the missing
  home identifier — size them together.
- (s302d, entry lane) **D_EFILE_001 cannot distinguish "EIN not keyed"
  from "EIN not obtainable."** Most packets satisfy it from the GA-500
  income-statement grid, but a payer that withheld no Georgia tax never
  appears there — a third-party sick-pay payer on one packet has no EIN
  anywhere in the source. That packet is held on a gap the preparer
  cannot close from the documents. If the shape recurs, the refusal
  should split (and the s298 21-blank-row class is the same question
  from the other side, still on Ken's prefix-tier call).
- (s302, states lane S-10c) **`D_8582_PTP` remains unverified** — one of
  seven keys `check_schedule_e_8582_integrity` reports as "no independent
  recompute mapped". s302's §469(g) build is validated only by its own
  i8582 PDF reading, which is why that reading was done. ⚠⚠ Their wider
  finding: `check_topic8`-style gates compare a rule's DECLARED inputs
  against the facts and never compare the FORMULA against the
  declaration — so `inputs: []` is vacuously valid and **the emptier a
  rule's declaration, the safer it looks** (exactly how R-SE-L8D-L9's
  missing L8a input survived; found by a $5,717 error, not by a gate).
- (s302) `div_1099s.us_government_income` is attribution-only BY DESIGN
  (splits a keyed ga500_fields S1-10 by owner; derives nothing) — the
  entry-lane note asks whether it should auto-derive S1-10; needs an
  off-switch decision (s237 class). · D_SCHD_006 QOF answer has no import
  surface (warning-level, filed).
- (s301) One packet's 5b decomposition: extracted pension rows carry
  115,150 vs face 15,150 — probe the 1099-R parse before assuming a
  rollover/exclusion. · One packet's Sch D carries 1b grid totals with NO
  f8949 pages + its own h-identity break — suspected misparse. · A blank
  printed 8949 page (synthetic packet) refuses "no transaction rows" —
  over-refusal by design.
- (s298) 21 named-but-blank W-2/1099-R rows have no unambiguous EIN match
  — held on D_EFILE_001; movement waits on Ken's prefix-tier call.
- (s297) The X mark at (474.7, y≈389) in the 1040 p2 EIC row region —
  unidentified, parser-ignored. · `est_payments_wks` (27) and `f8863` (6)
  gate at the face. ⚠ s302b note: when the est_payments_wks face class
  opens, the extractor should emit dated `federal_estimated_payments`
  rows — they now drive line 26 directly.
- (s296) The 22 sch_d GEOMETRY-error packets refuse loudly by design.
- (s295) 7 auxiliary Inbox PDFs refuse as non-packets — correct. ·
  `_summary_lines` GA500_SUMMARY_LINES lacks S1-6.
- (s290) GA RIE interest row excludes K-1 16A tax-exempt interest —
  stated boundary. · The rendered 8995 TIN prints unformatted — cosmetic.
- (s289) `IndividualForm7203`: no §179/charitable carryover keying;
  the §179 cap doesn't extend to 1065. · K-1 capital gains reach Sch D
  but not the L9 gain/loss WEIGHTS (⚠ s298: also the #78 aggregate
  convention under BOTH vendors).
- (s288) `IndividualForm7203` no home for box 16 code E; 1065 box 18
  a/b/c none recipient-side. · `ctc_override`/`odc_override` +
  `Dependent.compute_qualifies_*` are DEAD — removal candidate (Ken).
- (s287) 8825 line-1 repaint covers the LINE-1 table only. · Suggested-
  field convention covers W-2 3/5 + 1099-R box 16 (CLAUDE.md note stale).
- (s285) Sch 4 nonresident arm still apportions the whole widened base.
- (s283) The stamp excludes 1040 packets (Ken). · (s282)
  `OVERRIDE_HONORED_STATE_LINES` hand-synced. · (s281) OOS-state line-18
  prompt diagnostic specified, not built; stage allowlists `schd_fields`
  keys, `ga500_fields` not at all. · (s268) 1,604 queries/run. ·
  (s241/s281) `Form8606` unique-constraint candidate · 🔴 `HSAAccount`
  half CLOSED. · (s275/s281) `.first()`-on-per-form-rules remainder. ·
  (s294) a state face left by an omitting correction batch is not
  recomputed against that batch's federal changes.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried:
Everything from s277–s301 stands (see STATUS_ARCHIVE s300/s301 entries).
NEW (s302): **S-15** (SCHEDULE_K1 §469(g) disposition trio + the
R-8582-PTP related-party CORRECTION — the spec as written frees losses on
a related-party sale, the exact case §469(g)(1)(B) bars; + rule-2/rule-4
branch naming + the fully-taxable definition) and **S-16** (R-PAY-04
dated-rows source, matching R-2210-REG's precedence) — both staged by the
states lane, both Ken's. Recorded lane-side, not staged: the RS 8990 spec
models NO Schedule A surface (the seam if it is ever specified is
`partner_excess_bie`).
NEW (s306): **S-19** — `R-8889-EXCEPTIONS` states the same narrow
`line 2 > line 13` condition the code carried (blind to an employer-only
HSA excess, which i8889 measures against line 8) and its diagnostic message
still says the excise is "not computed here", which is stale since Form 5329
Part VII computes it from line 47. Staged by the states lane, who pulled both
instruction PDFs and confirmed the quotes verbatim, and who DISSOLVED a third
suspected defect in the same rule (line 12 vs line 13 are arithmetically
equivalent given `line 13 = min(line 2, line 12)`) — recorded because a false
finding attached to a true one is how a good item gets argued down.
⚠ Also noted by them, no action from this lane: `R-8889-EXCEPTIONS` declares
`inputs: []` while its formula reads lines 2 and 13 — the THIRD confirmed
instance of the S-17 under-declaration class, after `R-SE-L8D-L9` and
`R-1116-SUMMARY`.
