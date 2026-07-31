# TTS Tax App - STATUS (current state only)

*Last updated: 2026-07-31, session 156 (**⚡ THE BUSINESS-ENTITY SCREEN LANE
IS OPEN — Ken directed it over resuming the backlog. Unit 36 (Client Info +
Admin, both entity editors) is SHIPPED** on `slate-ui`, no deploy,
client-only but for two additive a11y/bug fixes (StateSelect gains an
optional `ariaLabel`; InfoSection's entity autosave gains a dirty-ref
guard). The lane was SCOPED first (the s137 method, via an Explore agent
over FormEditor's tab→component map): ~13 units, listed under RESUME HERE —
the Slate chrome already wraps entity returns; Depreciation / 8824 / Sched
K's StandardSection body / the 8990 fallback are ALREADY Slate for
entities; the State tab's `NEW_UI && isIndividual` gate is the lane's
explicit last unit. **Unit-36 findings:** (1) ⚠⚠ the legacy InfoSection
PATCHed the ENTITY with unchanged data ~800ms after every tab open — the
initial GET's setEntity satisfied the autosave effect's guard (a
stale-clobber window against another session's concurrent edit); FIXED at
the container with a dirty-ref, pinned by a mocked-api container test,
revert-tested, live-proven (idle 2.5s → 0 PATCHes). (2) ⚠⚠ ENGINE, not
fixed (sweep units are presentation-only), REVIEW_QUEUE + LEG 2 lane: a
1065 officer's compensation is written to line 7 "Other Income (Loss)" by
`compute.aggregate_officer_compensation` (bare line "7") while
`views._rollup_officer_compensation` targets `1065_L9` (line 9 Salaries) —
the s144 two-implementations shape, worse: they DISAGREE on the
destination. Engine-proven $50,000 → line 7 = 50,000 → line 8 Total Income
= 50,000; plus the delete residue (line 7 stays stale forever after the
last officer dies — the s143 zero-residue shape). The Slate 1065 Officers
card STATES it. (3) officer delete was window.confirm → the s153 two-step
lane; "also add as shareholder" became a per-row + Shareholder action.
The rule/diagnostic backlog stays PAUSED at LEG 2 item 5.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — **THE BUSINESS-ENTITY SCREEN LANE, unit 37 (Shareholders, 1120-S).**

**Ken picked the entity lane over resuming the backlog (2026-07-31, s156).**
The lane is SCOPED — 13 units in entity-nav order, 1120-S first per the
mission. ✅ = shipped:

| Unit | Tab(s) | Notes |
|---|---|---|
| ✅ 36 | Client Info + Admin | s156 — both entity editors |
| 37 | Shareholders (1120-S) | record-table paradigm — **NEXT** |
| 38 | Partners + Allocations (1065) | |
| 39 | Form 7203 + shareholder loans (1120-S) | |
| 40 | Income & Deductions (page1) | pure props, both entities |
| 41 | Schedule L balance sheets + SubSchedulePanel | biggest schedules unit |
| 42 | Schedule B + Sched K verify | Sch K/8990 already Slate via StandardSection — QA-verify only |
| 43 | Rental (8825) | entity twin of Sch E |
| 44 | Dispositions (entity Sch D) | |
| 45 | Schedule F (entity arm) | pure props |
| 46 | Elections: 2553 / 2848 / 3115 | three files, ~1,670 ln |
| 47 | Boundary + 8941 | |
| 48 | Extensions + PY Compare + State (entity arm) | wrap-up; opens the FormEditor:14152 `NEW_UI && isIndividual` gate |

Scoping facts (s156 audit): the Slate CHROME already wraps entity returns
(bare `NEW_UI`); already-converted-for-entities: Depreciation (entity-aware),
Like-Kind 8824, Sched K's body (SlateStandardSection + Noncash), the 1065
f8990 fallback. Three 1040-twin pairs where Slate landed only on the 1040
twin: rental/ScheduleE, dispositions/ScheduleD, ScheduleF. Demo QA returns:
1120-S `bbe88483-0690-4049-86a1-b2fe00822bf3` (WorkNAllDay, 1 officer) ·
1120-S `7f485239-…` · 1065 `16c8f946-379a-45ab-82d8-d156ad4e0b01` (Blue
Ridge). ⚠ These belong to **Delvio Demo Firm — mint magic links for the
`demo` user, not `dev`** (scratchpad mint variant; `dev` 404s on them).

The **rule/diagnostic backlog stays PAUSED mid-LEG-2 at item 5** (the
Schedule 1-A tips owner filter). Items 8 and 16 (the two Ken-ruled derives)
are ready to build the day it resumes. **s156 added the 1065 officer-rollup
defect to the LEG 2 lane** (REVIEW_QUEUE, engine-proven).

**s155 added one item to REVIEW_QUEUE** (minor): `form_4868` PATCH/DELETE
should arm the fresh-return marker so the client's `?fresh_return=1` kills
the follow-up GET the new Slate lane issues (the client-side fix already
covers correctness).

**s154 added four items to REVIEW_QUEUE** (unit-34 findings, all with proofs):
the faceless 1040 proforma snapshot (extend `build_1040_snapshot` with face
dollars — the real PY-compare fix), the `/prior-year/` bare-`.get()` 500 risk,
the generic face table's violet-on-plain-entry flaw, and the line-38 seed
label. **Ken-flag at close-out: the manual-PY entry lane** (the 1040's first —
the endpoint existed since B2-3, no individual tab rendered it).

Ken redirected back to the redesign on 2026-07-30 (s146): *"Let's get back to the
redesign unless this is pressing."* **The rule/diagnostic backlog stays PAUSED
mid-LEG-2, at item 5** (the Schedule 1-A tips owner filter) — a real
overstatement, but not pressing: nothing is in front of clients until January
2027 and the unit-24 screen already flags it by employer name. Ken directs when
it resumes.

**s147 added four items to the backlog** (all written up in `REVIEW_QUEUE.md`
with their engine proofs): Form 8615's line-1 sourcing → **LEG 2**; the missing
`IRS8615` e-file builder and the blank parent header → **LEG 3**; and a
cross-cutting recommendation to stop fixing the unpinned-year-constant shape one
form at a time (third occurrence). They are listed in the legs below.

**s153 added one engine item** (REVIEW_QUEUE, live-proven on the demo return):
duplicate state returns — no uniqueness constraint on (federal_return,
form_definition) and `_auto_sync_ga500`'s find-or-create is not atomic; the
demo pair's 195 ms spacing IS the race, the printed package includes both
copies ($426 of GA tax apart), and `create_state_return`'s 409 check has the
same window → **LEG 3 item 18** (a conditional unique constraint + the
locked find-or-create + Ken's call on which demo copy dies through the new
Remove lane).

**s152 added two engine items** (REVIEW_QUEUE, both observed live on the demo
return): the 1040-X delete residue (~61 published FFV rows survive the
DELETE and `is_amended_return` stays true, so the F8888-020 amended-refund
cap keeps blocking a return whose amendment was deleted) → **LEG 2-lane item
13** (the s143 zero-residue family — fix at the write); the missing
amended-1040 transmission path (MeF accepts e-filed 1040-X; the app has no
AmendedReturnInd / amended manifest on the 1040 side) → **LEG 3 item 17,
scoped SEPARATELY from the missing-documents batch** (a submission-type
feature). Also noted: the staged 1040-X flow-assertion file carries 0
assertions — RS has never authored any.

**s151 added one compute item and one e-file item** (REVIEW_QUEUE, both
engine-verified — the compute one priced with the engine's pure functions in
a probe): the Form 5695 Credit Limit Worksheet divergence (§25C-first
ordering + the missing CTC / Sch 3 line-6 subtractions; $1,000 of §25D
carryforward destroyed on case A, $2,000 of Sch 3 5a overstated-and-
transmitted on the CTC case) → **LEG 2 item 12**; the missing `build_irs5695`
(5th missing-MeF-document occurrence) → **LEG 3 item 16**, scoped WITH items
9/11/13 as the one "missing MeF documents" work item. The tracker's all-green
5695 row is annotated (the THIRD form proving open item 31 — the row was
green while the e-file leg does not exist).

**s150 added one diagnostics item, one render/e-file item and one spec item**
(REVIEW_QUEUE, engine-verified with the pure-function price): the
`rules_8960._f8960` auto-feed blindness ($608 charged while D_8960_NII_LOSS
says no tax; total silence on a rental-only return) → **LEG 2 item 11**
(diagnostics-lane, the LEG 1 shape); the vanishing 8960 lines 3/5b + the
blank arithmetic lines 5d/9d/11/14/15 in print AND XML → **LEG 3 item 15**,
scoped with the other render gaps; the unmodeled 5c / line 10 / election
checkboxes + the 9d misnumbering → the standing RS spec-corrections agenda.

**s149 added two compute items and two notes** (REVIEW_QUEUE, engine-verified
with the pure-function price): the Form 8880 MFJ line-4 both-columns rule →
**LEG 2 item 9** ($1,000 overstatement on the max case; prints and e-files);
the Credit Limit Worksheet's missing Schedule 3 6d/6l subtraction → **LEG 2
item 10**; the FIFTH year-fallback occurrence (`SAVERS_AGI_TIERS`, latent) →
LEG 4 item 17 strengthened again; and a minor render/e-file note (an
engaged-but-zero 8880 prints and transmits against the face's own "stop").
FORM_8880 also has NO tracker row at all — open item 31's list grows.

**s148 added three more** (REVIEW_QUEUE, engine-verified): the missing
`build_irs1116` (full path only — the §904(j) paths legitimately transmit no
1116) → **LEG 3**, scoped WITH items 9/11 as one "missing MeF documents" work
item; the printed Part II render gaps (Paid/Accrued box, "1099 taxes" in (l),
(q)–(t), box h) → **LEG 3**; and the `ADJ_EXCEPTION_TI` year fallback — the
**FOURTH** occurrence of the shape, which strengthens LEG 4 item 17 (rule once,
engine-wide) rather than adding a new item.

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
7. **(s147) Form 8615 line 1 counts only interest + dividends + capital gain.**
   `_source_child_amounts` = 1040 2b + 3b + max(0, 7). i8615 (fetched live) counts
   ALL unearned income — rents, royalties, pensions/annuities, taxable SS, taxable
   scholarships, unemployment, alimony, **trust beneficiary income** — and directs
   a child with no earned income to enter **AGI**. Engine-proven **$1,767** and
   **$1,217** understated on two returns, and `child_unearned_income` is
   `read_only_fields`, so there is no override to escape with. A minor who is a
   trust beneficiary is exactly what §1(g) exists to reach. REVIEW_QUEUE has the
   recommendation (AGI shortcut + the Child's Unearned Income Worksheet;
   RED-defer the Alternate Worksheet cases).
8. **`scha_gambling_winnings`: DERIVE it — KEN RULED 2026-07-30 (s153
   close-out, DECISIONS.md).** The §165(d) cap becomes the W-2G box-1 sum
   plus `other_gambling_winnings`, with an `_overridden` companion;
   `D_W2G_LOSS_CAP` stays as the disagreement reporter for overrides.
   Ready to build when the backlog resumes.
9. **(s149) Form 8880 line 4 must carry BOTH spouses' distributions in BOTH
   columns on MFJ** (face + i8880 + §25B(d)(2)(C); the didn't-file-jointly
   exception is preparer-applied). `compute_8880_column` subtracts each
   column's own entry only — **$1,000 overstated on the priced max case**, and
   it prints (`render_8880`) and transmits (`build_irs8880`). The RS spec's
   `R-8880-CONTRIB` lacks the rule (4th spec-divergence form — one RS
   conversation). No existing test pins the wrong MFJ arithmetic by name. The
   Slate screen instructs combined entry meanwhile. REVIEW_QUEUE has the
   recommendation.
10. **(s149) Form 8880's Credit Limit Worksheet must also subtract Schedule 3
   lines 6d and 6l** (2025 i8880 verbatim: "lines 1 through 3, 6d, and 6l");
   `compute_8880_db` reads 1/2/3 only while the 8911/8936 sibling reads
   include 6d/6l. Both are seeded direct-entry lines, so it is reachable
   today. The Slate screen warns live when 6d/6l hold a value.
11. **(s150) `rules_8960._f8960` must resolve rental the way the engine does**
   (Schedule 1 line 5 fallback + the `schedule_e_non_1411_income` back-out),
   and `d_8960_rental` must key off the RESOLVED 4a amount, not the override
   fact. Diagnostics-lane, no compute dollars — but the wrong guidance is
   priced ($608 charged while D_8960_NII_LOSS says no tax applies) and a
   rental-only return silences all five rules. The Slate screen carries the
   classification guidance at the 4a cell meanwhile.
13. **(s152) The 1040-X delete residue** — reset `is_amended_return=False` in
   the DELETE branch of views.py `form_1040x` (mirroring the create side)
   and make `compute_1040x_db` blank its lines when the `Form1040X` row is
   gone (it early-returns today, leaving ~61 stale FFV rows). Proven live:
   after a 204 DELETE the demo return kept 61 rows + the stuck flag, and
   F8888-020 (the amended-refund cap) stays armed. The s143 zero-residue
   shape — fix at the write. The Slate screen keys engagement off the row
   and warns on ghost rows meanwhile.
12. **(s151) Form 5695's two Credit Limit Worksheets, against the 2025 i5695
   verbatim** — compute §25C FIRST (line 31 = 1040 line 18 − Sch 3
   [6l,1,2,6d,3,4]), then §25D (line 14 = line 18 − Sch 3 [1,2,6d,3,4] −
   Form 5695 line 32 − 1040 line 19 (CTC) − Sch 3 [6m,6f,6g,6c,6h]). The
   engine's §25D-first order + lines-1–4-only subtraction destroys up to
   $3,200 of §25D carryforward (priced $1,000) and overstates a
   CTC-return's Sch 3 5a by up to the CTC (priced $2,000; transmits; 1040
   line 22 clamps so current-year tax hides it). Every needed input is
   computed before `compute_5695_db` runs. The RS spec carries the same
   simplified limit → the standing spec-corrections agenda. The Slate screen
   warns at the l14/l31 cells only when the divergence is live (live-proven).
   The `qa_unit31` flip case (solar 60,000 + insulation 4,000, demo return)
   is a ready regression: engine l15 12,204 / l16 5,796 / l32 0; the form
   gives l32 1,200 first.

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

11. **(s147) `build_irs8615`** — there is NO `IRS8615` builder in `apps/efile/`
   and no IRS8615 element in the `ReturnData1040.xsd` sequence, while Form 8615
   line 18 overwrites 1040 line 16 and line 16 transmits as `TaxAmt`. **The s140
   Schedule 1-A finding's SECOND OCCURRENCE** — scope it WITH item 9, not
   separately. Until then a kiddie-tax return is paper-only.
12. **(s147) Form 8615's parent header** — boxes A (parent's name), B (parent's
   SSN) and C (parent's filing status) all print blank. **Box C is render-only,
   the data is already stored**; A and B need two model fields → a migration.
13. **(s148) `build_irs1116`** — no `IRS1116` builder while Schedule 3 line 1
   transmits as `ForeignTaxCreditAmt`. **Emit ONLY on path "full"** — the
   §904(j) election and auto de minimis paths legitimately file no Form 1116.
   The missing-document shape's **4th occurrence** — scope WITH items 9/11 as
   ONE "missing MeF documents" work item. Until then a full-path FTC return is
   paper-only.
14. **(s148) Form 1116's printed Part II** — check (j) Paid (cash-basis
   default; no accrual election exists in the app), print "1099 taxes" in
   column (l) when `additional_foreign_tax` is 0 (i1116 verbatim), fill the
   (q)–(t) withheld-at-source breakdown the engine already has per-document,
   and decide box h "Resident of" (constant "United States" or a small field —
   Ken's call). Render-only except a possible box-h field.
15. **(s150) Form 8960's filed copy — print/transmit lines 3 and 5b, and fill
   the arithmetic lines 5d/9d/11/14/15.** `render_8960`, the field map
   (which has NO entry for line 3), and `_extract_f8960` move in one pass —
   they are value-for-value by design. 5b prints NEGATIVE (the face's
   "Combine lines 5a through 5c"); 14/15 are pure functions of stored data.
   Until then a filed line 8 with annuities or excluded gain does not foot
   from its own rows, on paper AND in the XML. Render/extract only — no
   migration.
17. **(s152) The amended-1040 transmission path** — MeF accepts e-filed Form
   1040-X, and the app cannot transmit one: no AmendedReturnInd /
   superseding indicator on the 1040 side (the 1120-S mapper has one), no
   amended manifest handling. **A submission-type feature — scope SEPARATELY
   from the missing-documents batch** (items 9/11/13/16). Until then a
   1040-X prints (render complete, appends to the package) and paper-files
   per D_1040X_006; the Slate screen states it. When the compute is next
   touched, also author the first 1040-X flow assertions in RS (the staged
   file holds 0).
18. **(s153) Duplicate state returns — a conditional unique constraint on
   (federal_return, form_definition) + a locked find-or-create in
   `_auto_sync_ga500`** (and the same window in `create_state_return`). The
   demo pair (195 ms apart, $426 of GA tax apart, both in the printed
   package) was the live proof; **Ken ruled at the s153 close-out and the
   stale copy is deleted** — the constraint work remains. A migration; sweep
   prod for existing duplicates BEFORE adding the constraint (the demo had
   one — prod may too: `SELECT federal_return_id, form_definition_id,
   COUNT(*) ... HAVING COUNT(*) > 1`).
16. **(s151) `build_irs5695`** — no IRS5695 builder while Schedule 3 lines
   5a/5b transmit (`ResidentialCleanEnergyCrAmt` / `EgyEffcntHmImprvCrAmt`)
   and `ReturnData1040.xsd` line 1382 expects `IRS5695` (maxOccurs 2 — the
   two-home case the app deliberately does not model; one document
   suffices). **The 5th missing-document occurrence — scope WITH items
   9/11/13 as the ONE "missing MeF documents" work item.** §25C(h) makes the
   QM ID numbers a condition of the §25C credit and they live ON the form,
   so an e-filed §25C claim without it is structurally disallowable. The
   print leg is complete (v2 field map fills the whole face); until the
   builder lands a claiming return is paper-only. The Slate screen states
   the gap whenever the face is engaged.

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
17. **(s147, +s148) ONE ruling on the year-keyed-constant fallback, not a fifth
   discovery.** `TABLE.get(year, DEFAULT)` / `TABLE.get(year) or TABLE[FALLBACK]`
   over year-keyed constants has now been found **FOUR** times — s142's
   `PY_STD_DEDUCTION` (live, D_SR_UNPINNED_YEAR), s146's four Form 6251 AMT tables
   (live, $18,141), s147's Form 8615 threshold (latent), s148's Form 1116
   `ADJ_EXCEPTION_TI` (latent; D_1116_007 covers only year==2026, never an
   unpinned year). **Sweep the engine for the pattern and rule once — skip the
   unpinned year and diagnose it — instead of finding it a fifth time.**
16. **`eic_self_employed`: DERIVE it — KEN RULED 2026-07-30 (s153 close-out,
   DECISIONS.md).** Default True when the return carries a Schedule C / F /
   SE K-1, with an `_overridden` companion. Ready to build when the backlog
   resumes (moves to the LEG 2 lane with item 8, its twin shape).

### ✅ LEG 1 — COMPLETE (s142), 12 diagnostics across three commits
(`2ed5eff` new `rules_sch_1a.py` · `42eb851` the two Form 5329 fixes · `d991b50`
the s137 five + two severity corrections). **Full write-up moved to
`STATUS_ARCHIVE.md` (s147) — read it there when you touch these rules.** What
still matters here: four of the twelve sit on TOP of defects that are still open
in LEG 2, the Schedule 1-A spec needs four corrections (open item 1 below), and
**five registry trip-wires were re-baselined** — a trip-wire that pins an exact
code set fails on every new rule, so update it with a pointer, never by loosening
the assertion.

### ✅ THE SCREEN SWEEP — **COMPLETE (s155): all 39 of 39 1040 screens converted.**
Unit 35 (Payments & E-File — the `payments` tab) shipped this session and
closed the sweep; the NEW_UI gate lives INSIDE PaymentsSection (the s153
StateSection shape). The count was verified the s137 way — every
`activeTab` mapped to its section component file, each checked for a
`NEW_UI` gate; the payments tab was the last without one.
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
19. ⚠⚠ **A FORM'S TRACKER ROW SAYING "UNIT FULLY COMPLETE" IS A CLAIM, NOT A
   FACT.** Form 8615's row was tagged complete with all six legs ✅ — and it has
   NO e-file leg at all, an incomplete render leg (the printed form's whole parent
   header is blank), and a compute leg that misses most kinds of unearned income.
   That is the SECOND form to prove open item 31 (SCH_1A was the first). **Audit
   the legs the tracker has no column for — `ls apps/efile/`, grep the builder's
   document sequence, read the field map for the header rows — before believing a
   green line.**
20. ⚠⚠ **A "NOT YET SUPPORTED" NOTICE OUTLIVES THE FEATURE IT DESCRIBED.** The
   8615 screen told preparers that qualified dividends and net capital gain defer
   to manual preparation; the engine built that path on 2026-06-24 and retired
   `D_8615_008` the same day. Second occurrence (s136's SS lump-sum said the same
   about a feature computed months earlier). **When a screen names a limitation,
   check the limitation still exists** — a preparer following stale copy works by
   hand a form the software already figured.
23. ⚠⚠ **A SEEDED LINE'S `is_computed` FLAG DESCRIBES THE LINE, NOT THE VALUE'S
   AUTHOR.** SCH_3 line 1 is seeded `is_computed=False` because a preparer may
   key it directly (the manual-FTC escape hatch) — yet the ENGINE writes it on
   every 1116 path. A client screen asking "did the engine write this?" must
   key off **`!is_overridden`** (every manual save sets `is_overridden=True`,
   views.py:2678), never off `is_computed`. s148's live run caught the auto
   de minimis pill reading "Not engaged" with $250 sitting on the line — the
   vitest fixture had assumed `is_computed=True` and passed.
24. ⚠⚠ **A COLUMN INSTRUCTION ON THE FORM FACE IS ARITHMETIC, NOT COMMENTARY.**
   Form 8880 line 4's margin text — "If married filing jointly, include both
   spouses' amounts in both columns" — changes line 5's subtrahend, and the RS
   spec carried the per-column formula without it (the 4th spec that keeps the
   arithmetic and drops the statutory rule). Read the column headers and
   margin instructions as formula; price the difference with the engine's own
   pure function before writing it up ($1,000 here, in one probe).
26. ⚠⚠ **A "MIRRORS X" DOCSTRING IS A CLAIM WITH A DATE ON IT.**
   `rules_8960._f8960` says it mirrors `compute_8960_db`'s input resolution —
   it did, until the engine gained the Schedule 1 line-5 rental auto-feed and
   nobody re-read the mirror. When an engine grows a NEW input source, grep
   for every module that re-derives its inputs (diagnostics recomputes,
   render fns, e-file extracts) and re-verify each one; the render and
   extract had been updated, the diagnostics had not. The screen's live run
   did not catch this one — the pure-function probe did, because the audit
   diffed the two input resolutions side by side.
25. ⚠ **A WORKSHEET'S LINE LIST IS A SPEC — DIFF THE COMPUTE'S LIST AGAINST IT
   VERBATIM, THEN GREP THE SIBLINGS.** The 8880 CLW read subtracts Sch 3
   1/2/3 and its comment names 6c/6g/6h — lines the 2025 worksheet does not
   list — while missing the 6d/6l it does. `compute_8911` and `compute_8936`
   already carry the right list, which turned "is this right?" into "the
   outlier is wrong" in one grep.
21. ⚠ **THE LIVE RUN STILL EARNS ITS KEEP ON A SCREEN THAT FETCHES NOTHING.**
   Unit 27 is prop-based, so s146's two fetch traps could not occur — and the
   live run still found copy a prop test could not: with line 18 equal to line 17
   the note read "$17,425 against $17,425", which is true and useless, and that is
   the §1(g)-floor case where the preparer most needs telling that §1(g) added
   nothing. Run it, then test what the run found.
22. ⚠ **A LABEL INTERPOLATED INTO `new RegExp(...)` NEEDS ESCAPING.**
   `slate_screen_screenshots.mjs` took the rail label verbatim, so
   "Kiddie Tax (8615)" became a capture group and searched for "Kiddie Tax 8615"
   — a bare 30s TimeoutError with no hint. FIXED in the script (it escapes now),
   so pass the label VERBATIM from `IndividualNav.tsx`.
28. ⚠⚠ **THE ENVELOPE TRAP LIVES IN CONTAINERS TOO — AUDIT EVERY RAW `get()`
   CALL WHEN CONVERTING A FETCH-BACKED SECTION.** `api.get()` resolves to an
   `{ok, status, data}` envelope; `Form1040XSection.load()` stored the
   envelope itself, so the legacy screen NEVER loaded its persisted row
   (blank explanation, "Not captured" baseline, unticked flags) while every
   PATCH succeeded — invisible on the legacy screen precisely because that
   screen had no state that DEPENDED on the row reading true. Third
   occurrence (s146's two were in a Slate screen); the container's data
   path needs its own test (`form1040xEnvelope.test.tsx` — mock `get`,
   assert the persisted row SURFACES), because the prop-based Slate suite
   structurally cannot see it. The live run caught it; the vitest suite was
   green throughout.
27. ⚠⚠ **A STATED DEFERRAL IS NOT A PRICED ONE — RE-READ OLD DEFERRAL LISTS
   AGAINST THE CURRENT-YEAR INSTRUCTIONS.** s110 logged "the Credit Limit
   Worksheet ordering is still the v1 simplified available-tax limit" as a
   stated boundary and it sat there sounding cosmetic; reading the 2025
   i5695's two worksheets VERBATIM turned it into a $1,000/$2,000 defect
   with an inverted credit ordering. Related: **a worksheet that subtracts a
   SIBLING form's line is an ordering statement** (line 14 subtracts line 32
   → §25C computes first), and ordering is arithmetic wherever one credit
   carries forward and the other dies.
29. ⚠⚠ **A PANEL THAT KEYS RECORDS BY A DERIVED, NON-UNIQUE KEY HIDES ITS
   DUPLICATES.** The legacy State panel mapped `stateReturnMap[stateCode] =
   sr` — last-write-wins over a newest-first list — so the demo return's two
   GA-500s rendered as ONE row (the stale one), while the auto-sync fed the
   other and the print package included both. When converting any panel that
   maps records by a derived key, ask what happens when two records share the
   key — and note the ORM PROBE found this pair, not the browser: probing the
   data a launcher launches into is part of auditing the launcher. (Related:
   the derived key itself was wrong for 3 of 4 states — `split("-")[0]` only
   parses the GA-* family. A derivation that happens to work for the most
   common case survives for years.)
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
  no hint. Grep `IndividualNav.tsx` for the tab id first. ⚠ **s147 FIXED the
  metacharacter half of this trap**: the label is interpolated into
  `new RegExp(...)`, so `Kiddie Tax (8615)` was a capture group matching
  "Kiddie Tax 8615" — nothing. The script now escapes it, so **pass the label
  VERBATIM from `IndividualNav.tsx`**, parentheses and all.
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
no merge/deploy without Ken · ✅ **the migrate + seed deploy step is DONE
(2026-07-30, Ken-directed): diagnostics 0005 applied on PROD and `seed_rules`
run + verified on BOTH DBs** (s142's 12 new rules + the D_5329_003 /
D_8863_DUAL_STUDENT severity promotions, plus the earlier D_W2_ family +
MATH_BALANCE_SHEET description, plus s144's D_RET_007 description and s145's
D_8863_DUAL_STUDENT severity+description — nothing outstanding at the next
deploy). s143 added no migration and no seed
change. **s146, s147, s148 and s150 add neither a migration nor a seed change**
(s147 and s148 touch only their spec mirrors, refreshed from the live RS
export; **s150, s151, s152 and s153 make no server change at all** — the
8960, 5695 and 1040-X spec mirrors were already current, each diffed clean
against the live export; s152's fixes are client-side only, in the shared
Form1040XSection container; s153 is client-only — the launcher view, the
`stateReturnRows.ts` builder and StateSection's Slate branch + remove lane;
**s154 is client-only** — the Tax Summary screen, `priorYearFace.ts` and the
FormEditor pyLookup filter + gate; **s155 is client-only** — the Payments
screen module, the PaymentsSection NEW_UI branch + `slateBankField`, and
`scripts/qa_unit35.mjs`). **Neither s144 nor s145 adds a migration, but BOTH change seeded rule
rows** — `seed_rules` must run on both DBs. ⚠ s145's is a **severity** change
(error → warning), which the s109b lesson says lives in TWO places — seed it, do
not assume the code change is enough.

**s156 gates (entity unit 36 — Client Info + Admin):** NEW
`slateEntityInfoScreen.test.tsx` **15** (12 view contracts across BOTH new
screens + 2 mocked-api container tests + 1 a11y/variant; the s152 rule that
a container's data path needs its OWN test; revert-tested TWICE — restoring
the un-guarded autosave failed exactly the spurious-PATCH container pin;
restoring single-click remove failed exactly the two-step test) · vitest
**1354/1354** (1339 + 15) · tsc **45 = baseline** (0 in the unit's files) ·
no server change (flow assertions n/a; the engine finding is REPORTED, not
fixed). Client changes: `SlateEntityInfoScreen` + `SlateEntityAdminScreen`
(slate/screens/), NEW_UI branches inside InfoSection + AdminSection,
`InfoSection` exported for the container test, the dirty-ref autosave guard
(fixes legacy too), StateSelect `ariaLabel` (optional, additive), the
`.slate-link` / `.slate-ptable-actions` / `.slate-adminfee*` CSS.
**LIVE-PROVEN** on the demo 1120-S `bbe88483` (`scripts/qa_unit36.mjs`, one
fresh `demo`-user magic link per run): idle 2.5s after open → **0 entity
PATCHes** (finding 1 live); legal-name edit → PATCH /entities/ 200 →
blanked back → 200; officer type-to-add → POST 201 → arm → Keep (0
DELETEs) → arm → Confirm → DELETE 204. Console errors = the known
pre-existing set. Screenshots `Design/screens/unit36/` (legacy+slate pairs:
ent-info-1120s · ent-admin-1120s · ent-info-1065 — the 1065 warning and
partnership labels live). **Demo settle, stated precisely:** the 1065 is
byte-identical to baseline (363 rows, hash `5e847264…`; the 46 blank
API-GET backfill shells value-checked `{''}` and deleted — the s148 rule).
The 1120-S FFV count is unchanged (359) with entity/officers/info fields at
baseline, but 14 ENGINE-WRITTEN lines (4797/officer-comp/Sch L EOY/K) were
refreshed from stale values by the first recompute this return has had in
months — the QA's net data change was zero (a $0 officer added and removed);
the new state is the engine's proven fixpoint (a second `compute_return`
changes nothing, hash `7bd3e674…` stable). ⚠ Unit-36 gotchas: the demo
ENTITY returns belong to Delvio Demo Firm → mint for `demo`, not `dev`; a
puppeteer wait must never grep `innerText` for a value that renders inside
an `<input>` (values aren't innerText — key off aria-labels/testids); a
scratchpad .mjs can't resolve repo node_modules — put QA scripts in
`scripts/`.

**s155 gates (unit 35 — Payments & E-File / the payments tab):** NEW
`slatePaymentsScreen.test.tsx` **21** (13 view contracts + the container's
envelope/finding-1 tests — the s152 rule that a container's data path needs
its OWN mocked-api test; revert-tested TWICE: restoring single-click remove
+ the client line-26 re-sum failed exactly the 4 tests pinning findings 4/5;
dropping the 4868 `afterMutate` failed exactly the 2 container tests pinning
finding 1) · no server change (`git status`: server tree clean but for the
parallel session's pre-existing files; flow assertions not required) ·
vitest **1339/1339** (1318 + 21) · tsc **45 = baseline** (0 in the unit's
files). **LIVE-PROVEN** on the demo QA return in two phases
(`scripts/qa_unit35.mjs A|B`, one fresh magic link each): **A** — at rest
pill "Owe $10,151", line 26 renders the PUBLISHED row as a locked ƒx cell,
all six panels disengaged with Start lanes, the D_1040_003 warning live;
est_payment_q1 = 1,000 keyed through the facts lane → PATCH /taxpayer/ 200 →
line 26 ƒx = 1,000 and the pill moved to **"Owe $9,081"** (NOT 9,151 — the
§6654 penalty on line 38 recomputes DOWN with the extra quarter credit;
engine-derived, my hand-arithmetic was wrong); blanked → "0" → line 26 back
to 0, pill back to 10,151. **B** — Start 4868 → PATCH 200, the banner
rendered from res.data (L7 derived 9,754, Sch 3 L10 note) and **the pill
flipped to "Balanced" with NO reload — finding 1 live**; two-step remove
proven (arm → confirm visible, 0 DELETEs; Keep → disarmed, 0 DELETEs; arm →
Confirm → DELETE 204 → pill back to "Owe $10,151"); then the no-compute 8888
lifecycle (start → acct1 = 100 → the exact-tie error + row-problems +
F8888-002-03 blocker all fired from the analysis → two-step remove, DELETE
204). Console errors = the known pre-existing set (403 `/api/v1/me/`
pre-login + the `prior-year/` 404s). **Demo writes all through the screen's
own lanes and self-reverting — ORM-verified ZERO DRIFT: FFV 892 rows, hash
`1dc75fda…` IDENTICAL before/after; all six singleton row counts 0; every
payments fact and the bank cluster back at defaults.** Screenshots
`Design/screens/unit35/` (atrest · estq1 · 4868 · 8888). ⚠ Session gotcha:
PS 5.1 `Get-Content`/`Set-Content` round-trips a no-BOM UTF-8 source as
ANSI and mangles every multi-byte char — the screen file had to be fully
rewritten; never regex-edit a UTF-8 source through PowerShell here.

**s154 gates (unit 34 — the Tax Summary screen / tax_summary tab):** NEW
`slateTaxSummaryScreen.test.tsx` **15** (13 screen + 2 priorYearFace;
revert-tested TWICE — restoring the generic is_overridden→violet mapping
failed exactly the 2 field-state tests that pin findings 3/4; restoring the
verbatim pyLookup pass-through failed the faceLineValues test that pins
finding 1) · no server change (verified by `git status`: server tree clean
but for the parallel session's pre-existing files; flow assertions not
required) · vitest **1318/1318** (1303 + 15) · tsc **45 = baseline**.
**LIVE-PROVEN** on the demo QA return in three phases
(`scripts/qa_unit34.mjs A|B|C`, one fresh magic link each): **A (no PYR)** —
owe pill "Owe $10,151" (the server's line 37), the five real band titles,
the manual-PY hint, lines 38/7 render ƒx `is-computed` readOnly (engine-
authored 397/150), the 1b–1h group closed; then the 1040's FIRST manual-PY
entry: 88,000 keyed into line 9's PY cell → PATCH /py-manual/ 200 →
persisted after reload with Δ +7%. **B (ORM-created FACELESS proforma
snapshot, underscore keys only)** — the faceless note fires, the hint
suppressed, NO phantom PY values, the manual 88,000 still shadows.
**C (snapshot face-keyed 9=91,000/24=11,000/33=2,000)** — line 24 shows
imported 11,000 with the reference tint + Δ +11%, manual 88,000 still wins
on line 9; blanking the manual cell PATCHed null and revealed 91,000
(persisted, Δ +4%). Console errors = the known pre-existing set (403
`/api/v1/me/` pre-login; the `prior-year/` 404s disappear once a snapshot
exists). **Demo writes REVERTED and ORM-verified ZERO DRIFT: FFV 892 rows,
hash `5820bba2…` IDENTICAL before/after** (the QA's only server writes were
its own PYR row — deleted by source tag `sherpa_proforma_qa34` — and the
py_manual_values keys — cleared; py-manual writes touch no FFV and trigger
no compute). Screenshots `Design/screens/unit34/` (atrest · manualpy ·
faceless · imported).

**s153 gates (unit 33 — the State Returns launcher / state-GA tab):** NEW
`slateStateReturnsScreen.test.tsx` **23** (revert-tested TWICE — restoring
the legacy business-entity refresh tooltip failed the contract-3 copy test;
restoring the legacy map-by-state-code + `split("-")` row shape failed the
3 buildStateRows tests that pin findings 5/6) · flow assertions untouched
(no server change — verify by `git status`: server tree clean but for the
parallel session's pre-existing files) · vitest **1303/1303** (1280 + 23) ·
tsc **45 = baseline** (s152's level). **LIVE-PROVEN** on the demo QA return
(`scripts/qa_unit33.mjs`): at rest BOTH GA-500 duplicates render as their
own rows with the "Duplicate GA returns" pill, the duplicate error note,
the GA auto-sync note, the overwrite warning and the derived 4-state
footer (screenshot `unit33_duplicates.png`); an SC1040 created through the
picker (POST 201) landed on the SOUTH CAROLINA row with Open/Refresh/Remove
(finding 6's mapping, live), the picker dropped to AL/NC; **Open navigated
to the SC1040's own editor**; the remove lane proved TWO-STEP live (arm →
Keep disarmed with no DELETE; arm → Confirm issued DELETE 204) and the SC
row returned to the picker. Console errors = the known pre-existing set
(403 `/api/v1/me/` pre-login + the `prior-year/` 404s). **Demo writes
self-reverting and ORM-verified ZERO DRIFT: federal 892-row FFV hash
IDENTICAL (`c8edf9c4…`), both GA-500 FFV hashes IDENTICAL, state-return
count back to 2** — the only writes were the QA's own SC1040, created and
removed through the screen itself. The GA duplicate pair was deliberately
not touched during QA; **at the close-out Ken ruled and the stale copy was
deleted by ORM** (verified: only its 153 rows; all other hashes unchanged).

**s152 gates (unit 32 — Form 1040-X / amended return):** NEW
`slateForm1040XScreen.test.tsx` **20** (revert-tested TWICE — restoring the
legacy placeholder face failed the 2 contract-2/6 tests; keying engagement
off FFV rows failed the contract-4 ghost test) + NEW
`form1040xEnvelope.test.tsx` **3** (revert-tested — restoring the
envelope-stored row failed the persisted-row test) · the 1040-X server legs
**14** (untouched) · flow assertions **521** (baseline exactly) · vitest
**1280/1280** (1257 + 23) · tsc **45** (⚠ ONE BELOW the 46 baseline — the
typed envelope unwrap in `Form1040XSection.load()` removed a pre-existing
error; no new errors, none in the unit's files). **LIVE-PROVEN** on the demo
QA return through the screen's own lanes (`scripts/qa_unit32.mjs`): at rest
"Not engaged" + the GHOST warning live (the first run's DELETE had left the
residue — finding 3 observed in production shape); baseline captured through
the screen (POST 200); `overpayment_on_original` = 2,450 keyed IN the screen
CREATED the amendment (PATCH 201) — the A/B/C grid published (A = C =
94,560 AGI / 12,204 total tax, B = 0), outcome 17 = 2,450 → 19 = 0 → 20 =
12,204, pill "Owe $12,204", the paper-only + Part-II-blank warnings live;
`amount_paid_with_original` = 500 reflowed server-side (17 = 2,950 → 19 =
500 → 20 = 11,704); the explanation committed on blur (PATCH 200) and
cleared the warning; DELETE 204 → the ghost warning again. ⚠ **The first QA
run FAILED against the pre-fix container** (pill "Prepare manually" at rest,
`baseline_captured` never true) — that failure IS what exposed the envelope
bug; the suite had stayed green because the Slate view is prop-based. **Demo
writes REVERTED and ORM-verified ZERO DRIFT: 892 of 892 FormFieldValue rows
identical** (the 61 ghost 1040-X rows deleted — they CARRY values by the
residue defect, so scope-checked by FormDefinition, not value-checked; the
AsFiledBaseline row deleted; `is_amended_return` reset; `compute_return`
re-run).

**s143 / s144 / s145 / s146 / s147 / s148 / s149 / s150 / s151 gates — moved to `STATUS_ARCHIVE.md`
(s143–145 by s147; s146 by s149; s147 by s150; s148 by s151; s149 by s152; s150 by s153; s151 by s155).** Re-run them if you touch
Form 8863, Form 5329, Form 6251, Form 8615, the 8960 family, the retirement
modules or the MeF-1040 band; each block records the exact suites, the counts
and what each revert proved. s146's fetch-trap lesson survives as Method items 16/17 — a
fetch-backed screen needs a live pass.

**KEN CLARIFIED (2026-07-28): the tax app is TESTING until January 2027.** He
switches to Slate when the redesign is FINISHED; everything rides `slate-ui`;
the shared Supabase DB caution is the one true-production constraint.

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE — trimmed to live items)
0p. **(s156) ⚠⚠ A 1065 officer's compensation is recorded as INCOME** — the
   two officer rollups disagree on the destination (`views` → line 9
   Salaries; `compute` → bare line "7", which on the 1065 is **Other Income
   (Loss)**). Engine-proven $50,000 both ways + the delete residue (line 7
   stays stale forever). Fix = gate the aggregate on 1120-S + blank-not-skip
   at zero + drop the `1065_L9` mapping; also decide whether a 1065 should
   have an Officers card at all. LEG 2 lane. The Slate 1065 screen states it.
0o. **(s153) ✅ PARTLY RESOLVED at the close-out — Ken ruled:** the stale
   demo GA-500 is deleted (`e27ab39c`), the Remove lane STAYS. What remains
   open is the ENGINE half only: the uniqueness constraint + the locked
   find-or-create (LEG 3 item 18 — a migration, staged at Ken's trigger).
   Ken also ruled `eic_self_employed` DERIVE and `scha_gambling_winnings`
   DERIVE (both with `_overridden` companions) and RATIFIED the 5329 line-2
   pro-rata — all five in DECISIONS.md.
0m. **(s152) Deleting a 1040-X amendment leaves two ghosts** — ~61 published
   FFV rows survive the DELETE and `is_amended_return` stays true, so
   F8888-020 (the amended-refund cap) keeps blocking. Proven live. LEG
   2-lane item 13 (the zero-residue family).
0n. **(s152) An amended 1040 cannot be transmitted at all** — no
   AmendedReturnInd / amended manifest on the 1040 side while MeF accepts
   e-filed 1040-X. Paper-only, stated on screen. LEG 3 item 17, scoped
   SEPARATELY from the missing-documents batch. RS also has zero 1040-X
   flow assertions.
0k. **(s151) Both Form 5695 Credit Limit Worksheets diverge from the 2025
   i5695** — ordering inverted (the form runs §25C first; the engine runs
   §25D first and destroys up to $3,200 of carryforward — $1,000 priced) and
   the subtraction lists are short (no CTC, no Sch 3 6c/6d/6f/6g/6h/6l/6m —
   $2,000 of Sch 3 5a overstated-and-transmitted on the priced CTC case).
   LEG 2 item 12; the RS spec needs the same correction.
0l. **(s151) Form 5695 is never transmitted while its credits are** (no
   `build_irs5695`; `ReturnData1040.xsd` expects IRS5695). **5th occurrence
   of the missing-document shape — scope with 0b/0e/3 as ONE work item.**
   LEG 3 item 16. The §25C(h) QM ID numbers live on the form, so an e-filed
   §25C claim without it is structurally disallowable.
0i. **(s150) The Form 8960 diagnostics are blind to the engine's rental
   auto-feed** — $608 of NIIT charged while D_8960_NII_LOSS says no tax
   applies; a rental-only return silences all five rules. LEG 2 item 11
   (diagnostics-lane).
0j. **(s150) The filed Form 8960 omits lines 3/5b while line 8 includes
   them** (print AND XML — the line 8 does not foot), and the arithmetic
   lines 5d/9d/11/14/15 print blank. LEG 3 item 15. Plus the spec gaps
   (5c / line 10 / election checkboxes; 9d misnumbered) → RS agenda.
0g. **(s149) Form 8880 overstates the MFJ Saver's Credit** — line 4 must carry
   both spouses' distributions in both columns and the engine subtracts each
   column's own entry only. **$1,000 priced max case; prints AND e-files.**
   LEG 2 item 9; the RS spec needs the rule (4th spec-divergence form).
0h. **(s149) The 8880 Credit Limit Worksheet misses Schedule 3 6d/6l**
   (direct-entry-reachable; the 8911/8936 sibling reads include them). LEG 2
   item 10. Plus a minor: an engaged-but-zero 8880 prints and transmits
   against the face's own "stop".
0e. **(s148) Form 1116 is never transmitted but the FULL-path credit is** (no
   `IRS1116` builder; the §904(j) paths legitimately file none). **4th
   occurrence of the missing-document shape — scope with items 0b/3 as ONE
   work item.** LEG 3 item 13.
0f. **(s148) The printed Form 1116's Part II needs hand-finishing** — the
   mandatory Paid/Accrued box, "1099 taxes" in column (l) (i1116 verbatim),
   the (q)–(t) breakdown, box h "Resident of". Render-only except a possible
   box-h field. LEG 3 item 14.
0a. **(s147) Form 8615 line 1 counts only interest + dividends + capital gain**
   while i8615 counts all unearned income and tells a no-earned-income child to
   enter AGI. **$1,767 / $1,217 understated, engine-proven; the cell is
   read-only.** LEG 2 item 7.
0b. **(s147) Form 8615 is never transmitted but its tax is** (no `IRS8615`
   builder). **The s140 Schedule 1-A finding's second occurrence — scope the two
   together.** LEG 3 item 11.
0c. **(s147) The printed Form 8615's parent header is blank** (boxes A/B/C).
   Box C is render-only; A/B need a migration. LEG 3 item 12.
0d. **(s147) Rule the year-keyed-constant fallback ONCE**, across the engine —
   third occurrence of the shape. LEG 4 item 17.
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
    (the row is annotated); the e-file half is LEG 3 item 9. **STATE_REFUND**,
    **FORM 5329**, **FORM 8880** (s149) and **FORM 8960** (s150) have no row at
    all, though every leg exists for each.
    The table needs those two columns. ⚠ **s144 deliberately did NOT open a
    Form 5329 row** even though it worked the compute leg: without the two
    missing columns a new row could only be recorded as all-green, which is the
    exact SCH_1A mistake this item exists to stop. Add the columns first, then
    both rows. ⚠⚠ **s151: the FORM 5695 row is the THIRD live proof** — five
    green legs while the e-file leg does not exist (no builder) and the
    compute leg's CLWs diverge from the 2025 instructions; the row is now
    annotated in place.

## Active gates
- **Branch discipline:** `slate-ui` checked out; parallel session's uncommitted
  work UNSTAGED. Never stage/stash/`git add .`.
- ✅ **The demo GA-500 duplicate is RESOLVED (Ken ruled, s153 close-out):**
  the stale `e27ab39c` was deleted by ORM (153 rows, verified nothing else
  cascaded; federal + surviving hashes unchanged); `a129a7e2` (auto-synced,
  GA tax 3,332) is the sole GA-500. The race/constraint fix stays queued
  (LEG 3 item 18) — the probe data lives in REVIEW_QUEUE.
- ⚠ **s155 wrote to the demo QA return and self-reverted THROUGH the
  screen's own lanes** (est_payment_q1 1,000 → blanked-to-"0"; a Form 4868
  created and two-step-removed; a Form 8888 created, acct1 = 100, two-step-
  removed). **ORM-verified ZERO DRIFT: FFV 892 rows, hash `1dc75fda…`
  IDENTICAL before/after; all six payment-singleton row counts 0; every
  payments fact and the bank cluster at defaults.**
- ⚠ **s153 wrote to the demo DB and self-reverted through the screen:** one
  SC1040 created via the picker (POST 201) and removed via the new Remove
  lane (DELETE 204). ORM-verified zero drift — federal 892-row hash and both
  GA-500 hashes identical, state-return count back to 2.
- ⚠ **Demo DB drift:** diagnostics migration 0005 applied to the DEMO DB only —
  prod applies at Ken's deploy (additive, safe). **`seed_rules` has been run on
  the DEMO DB for all of s142's rules; PROD seeds at Ken's deploy.**
- ⚠ **s152 wrote to the demo QA return and REVERTED it.** Baseline POST +
  amendment create/PATCHes/DELETE all through the app lanes; the QA
  deliberately left the delete residue standing long enough to OBSERVE it
  (61 ghost 1040-X FFV rows + `is_amended_return` true), then the revert
  deleted the ghost rows (scope-checked by the 1040-X FormDefinition — they
  carry values by the residue defect, so no blank-check applies), deleted
  the AsFiledBaseline row, reset the flag and re-ran `compute_return`.
  **Zero drift, ORM-verified 892 of 892 FormFieldValue rows identical.**
- ⚠ **s151 wrote to the demo QA return and REVERTED it.** Every write went
  THROUGH the Slate screen's facts lane (solar 20,000 → +insulation 4,000 →
  solar 60,000 → both blanked to "0"); no ORM raise was needed. The 17 blank
  FORM_5695 FFV shells `_backfill_values` created were deleted after
  value-checking `{''}` and `compute_return` re-run. **Zero drift,
  ORM-verified 892 of 892 FormFieldValue rows identical** to the pre-write
  baseline. The at-rest figures below still hold.
- ⚠ **s150 wrote to the demo QA return and REVERTED it.** The 1099-INT was
  raised to 250,000 by ORM (+`compute_return`) to engage the NIIT, and
  `e8960_state_tax_allocable` was keyed to 120,000 THROUGH the Slate screen
  (the facts-lane live proof), then blanked through the screen (the
  blank→"0" lane proof). The 6 blank FORM_8960 FFV shells `_backfill_values`
  created were deleted after value-checking `{''}`, the 1099-INT restored to
  1,250.00 and `compute_return` re-run. **Zero drift, ORM-verified 892 of
  892 FormFieldValue rows identical** to the pre-write baseline. The at-rest
  figures below still hold.
- ⚠ **s149 wrote to the demo QA return and REVERTED it.** `f8880_you_ira` was
  set to 2,000 THROUGH the Slate screen (the facts-lane live proof), engaging
  the 18-row FORM_8880 face; then blanked through the screen (the blank→"0"
  lane proof), disengaging it. The 18 blank FFV shells `_backfill_values`
  created were deleted after value-checking `{''}` and `compute_return` re-run.
  **Zero drift, ORM-verified 892 of 892 FormFieldValue rows identical** to the
  pre-write baseline. The at-rest figures below still hold.
- ⚠ **s144 / s147 / s148 each wrote to the demo QA return and REVERTED it**
  (s144: two synthetic 1099-Rs + a Form5329 row; s147: a Form8615 row + the
  1099-INT raised to 25,000; s148: `foreign_tax_paid` 250/650 + a Form1116 row
  + 31 blank FFV shells deleted). Every one closed at **zero drift,
  ORM-verified 892 of 892 rows identical**; full detail in `STATUS_ARCHIVE.md`.
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
