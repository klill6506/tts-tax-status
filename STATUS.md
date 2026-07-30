# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 140 (bespoke-screen sweep unit 24: SCHEDULE
1-A, the four temporary OBBBA additional deductions — tips §70201, overtime
§70202, car loan interest §70203/§163(h)(4), the enhanced senior deduction
§70103 → 1040 line 13b. Live-proven end-to-end and fully reverted. TWELVE real
defects — including a blank date of birth that silently discards a
$6,000–$12,000 senior deduction with NO diagnostic anywhere, and a schedule
whose total is e-filed while the schedule itself is never transmitted.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — the bespoke-screen sweep continues at **Form 5329**

**s140 shipped unit 24 on `slate-ui` (no deploy): `886d9ef`**

Remaining 1040 screens (~11 of ~39; the count was re-measured in s137 by
mapping every `activeTab` to its section component **file** and checking each
for a `NEW_UI` gate — do it that way, never by scanning FormEditor alone):
**5329** ≈278 · **6251** ≈264 · **8615** ≈259 · **1116** ≈273 · **8880** ≈156 ·
**8960** ≈119 · **5695** · **1040-X** · the **state/GA** tab · the
**prior-year / tax-summary** views · the estimates/extension/e-file cards.
Most are small worksheet singletons now that the paradigms are settled.
**The business-entity screens (1120-S / 1065 shareholders, partners, balance
sheets, allocations, 7203, page-1 income/deductions) are a SEPARATE unscoped
lane — ~12 more, none started. Ken's call when to take them.**

### Unit 24 — Schedule 1-A (OBBBA additional deductions)
A **singleton screenbar** hosting three paradigms that NEST: InputRow
worksheets for the Part II/III return-level facts, **PayerTable** for the Part
IV line-22 vehicle rows (identity = the serializer-validated VIN, the two
statutory attestations in the expansion), a bare **`.slate-asstable`** over the
two filers for Part V (which has NO inputs of its own — the birth dates live on
Taxpayer Info and the SSN flags on Credits & 8812, so it is annotated, not
edited, here), and the whole computed face from the server's own **SCH_1A** rows
as locked ƒx cells. Four different phaseout thresholds, three different caps and
two OPPOSITE rounding directions are never re-implemented on the client; a
threshold table shows all four parts at once. Presentation only — the server was
untouched.

- ⚠⚠⚠ **DEFECT — a BLANK date of birth silently discards the whole senior
  deduction, and nothing in the app warns.** `_born_before(None, cutoff)` is
  False, so an unentered DOB is indistinguishable from "under 65".
  Engine-proven on identical returns (single, 70 years old, AGI 80,000): line 35
  computes **5,700 either way**, but 36a / 37 / 38 and 1040 line 13b go
  **5,700 → 0** when the DOB is blank. MFJ with two seniors at AGI 140,000:
  **12,000 → 6,000** on one blank spouse DOB. The per-person amount on line 35
  still PRINTS, so the filed page shows the deduction as available and $0 as
  claimed. Direction: **UNDERSTATEMENT** — the client overpays.
  **Live-proven on the demo QA return with no seeding at all**: its taxpayer
  carries `date_of_birth = NULL`, so the screen opened showing MAGI 94,560,
  line 35 **4,826** available per person, line 37 **0**, line 38 **0** — the
  error fired by name on first paint (1 of the 8 demo-DB taxpayers is NULL).
  → REVIEW_QUEUE (a diagnostic, and consider deriving nothing — just ask).
- ⚠⚠ **DEFECT — MFJ tips over-inclusion: the W-2 OWNER is ignored.** Line 4a
  sums box 7 across **every** W-2 as soon as *either* filer attests a qualified
  occupation. `W2Income.owner` EXISTS on the model (it drives the per-owner
  box-12 derive) but `_compute_part_ii_tips` never reads it, and its comment
  claiming the field doesn't exist yet is **stale**. Engine-proven, MFJ AGI
  120,000 with a spouse W-2 (`owner="spouse"`) carrying 10,000 of box 7 tips:
  taxpayer-attests-only → line 4a 10,000, deduction **10,000** — identical to
  both attesting. Mirror case identical. Direction: **OVERSTATEMENT**.
- ⚠⚠ **DEFECT — Schedule 1-A has NO DIAGNOSTICS AT ALL.** All six specced ones
  (`D_SCH1A_001..006`, two of them **errors** — MFS claiming, and claiming with
  no valid SSN) are unimplemented; there is no `rules_sch_1a.py` among the
  80-odd rules modules. Georgia's `rules_ga500.py` DOES police tips and overtime
  (D_GA500_013/015) while the federal form that creates the deduction is
  unpoliced. → REVIEW_QUEUE.
- ⚠⚠ **DEFECT — the schedule is never transmitted, but its total is.** 1040 line
  13b maps to `TotalAdditionalDeductionsAmt` (`builder.py:93`) and is e-filed,
  yet no `IRS1040Schedule1A` document is built — a grep of `apps/efile` for
  sch_1a / 1040s1a / QPVLI / overtime / senior returns **nothing**, while
  Schedules 1, 2, 3, A, B, D, E, 8812 and EIC all have builders. Engine-proven
  case: a single filer with tips 4,000 + OT 5,000 + car 3,000 + age 75 e-files
  **17,700** on line 13b with no supporting schedule. → REVIEW_QUEUE; the screen
  says to file on paper.
- ⚠⚠ **DEFECT — tips from more than one employer produce a form that breaks its
  own instruction.** The line 4 face text: *"If you received tips as an employee
  with respect to employment with more than one employer, enter -0- on lines 4a
  and 4b and see the instructions to determine the amount to enter on line 4c."*
  Engine-proven with two W-2s (6,000 + 4,000): 4a **10,000** / 4b 0 / 4c 10,000.
  The spec's own R-TIPS-01 is annotated "(single employer)" and D_SCH1A_006
  exists for exactly this — unbuilt.
- ⚠ **DEFECT — the W-2 box 5 special case is ignored.** Line 4a: *"see the
  instructions if Form W-2, box 5 is more than $176,100."* `medicare_wages` is
  already on W2Income, so this is checkable from data we hold; D_SCH1A_005
  unbuilt. Engine-proven: box 5 200,000 with 9,000 of tips is taken at face value.
- ⚠ **DEFECT — both Part IV attestations default to TRUE.** Adding a row with
  nothing but a VIN and an amount claims the deduction on seven statutory
  conditions nobody affirmed. Live-proven: the POST body was
  `{"vin":"1HGCM82633A004352","total_interest_paid":"0"}` and the created row
  came back with **both boxes ticked**. ORM-proven cost: a $1,200 vehicle moves
  13b 0 → 1,200, taxable income 78,810 → 77,610, tax 12,204 → 11,940, amount
  owed 10,151 → **9,876** — **$275 of tax** on an unaffirmed attestation.
  Compare `tips_eligible_for_deduction`, which defaults **False**, and the RS
  spec, which gives both car attestations a **null** default.
- ⚠ **DEFECT — vehicles 3+ vanish from the printed form while line 23 still sums
  them.** `render_sch_1a` takes `CarLoanVehicle.objects.filter(...)[:2]`.
  Engine-proven with three rows (1,200 + 900 + 700): line 23 **2,800** against a
  printed 22a+22b of **2,100** — a filed page that contradicts itself. The form
  routes >2 VINs to a statement page that isn't built.
- ⚠ **DEFECT — MFS zeroes three of the four parts, and Part IV survives.**
  Identical facts, engine-proven: MFS total **3,000** vs single **17,700**.
  Legacy flagged MFS on tips only, mentioned it in the overtime prose, and said
  nothing for Parts IV or V. Verified against the form face: the **Part IV
  caution carries NO joint-return requirement** while II, III and V all read
  "If married, you must file jointly to claim this deduction."
- ⚠ **DEFECT — the screen showed NO computed result at all**, and Parts I, V and
  VI were absent from it entirely — no MAGI, no cap, no phaseout, no per-part
  deduction, no total, and no sign of a $6,000-per-person senior deduction whose
  only inputs live on two other screens.
- ⚠ **DEFECT — the car-loan phaseout rounds UP × $200** — the opposite direction
  and double the rate of tips/overtime. Engine-proven: AGI 100,050 (fifty dollars
  over) → line 28 = 1, line 29 = **200**. A $200 cliff at every $1,000 boundary.
- ⚠ **DEFECT — the self-employment limit on tips (spec R-TIPS-10) is unbuilt**
  and its stated reason — "awaits Schedule C" — is stale, Schedule C having been
  fully modelled since sweep unit 3. A self-employed filer's tips deduction can
  exceed the business's net income. → REVIEW_QUEUE.
- ⚠ Caught by the LIVE run, not by reading: the threshold table printed
  **"phaseout starts 0"** for tips, because the tips/overtime short-circuits
  return a dict of zeros that includes the threshold row itself. A disengagement
  artifact shown as a number is wrong guidance (the s136 lesson) — the cell now
  shows an em dash and the constant is still never re-derived on the client.

**Gates at s140 close:** vitest **1089/1089** (+39) · tsc **46 = baseline** ·
side-by-sides committed
(`Design/slate-phase2-screenshots/{legacy,slate}-sch1a.png`) · every QA write
**200/201/204** (POST 201 · 2 vehicle PATCH 200 · 6 taxpayer PATCH 200 ·
DELETE 204) · demo QA writes reverted to the exact s139 baseline with **ZERO
drift**, ORM-verified line by line (1040 11/12/13/13b/15/16/18/19/20/24/27/33/37,
0 vehicles, all 8 Sch 1-A facts at defaults re-read from the DB, print blockers
empty, Sch 3 L2 and 1040 1e blank). The console `403 /api/v1/me/` and
`404 …/prior-year/` are the pre-login identity probe and a return with no prior
year — checked in the server log, not defects.

**Next action: continue the sweep at Form 5329**, then 6251, 8615, 1116, 8880,
8960, 5695, 1040-X, the state tab, and the estimates/extension/e-file cards.
Paradigms settled: view-over-container; **PayerTable** for flat record lists
keyed by row id, **DocumentTabs + worksheet** for card stacks AND per-filed-form
rows, **InputRow worksheets** for facts cards (screenbar header for singletons),
**the asset register** for computed sub-schedule grids, a bare
**`.slate-asstable`** for a grid with no add row (a derived row set, a nested
replace-all list, or an EXISTING record set this screen only annotates —
including a two-row taxpayer/spouse table); paradigms may NEST; multi-section
tabs share ONE `.slate-screen` at the call site; screenshots per screen; live QA
writes reverted.

## 🔑 Method that is finding these defects (keep doing this)
1. **Read the engine against the screen before writing any JSX.** Eleven of
   unit 24's twelve findings came from that; the twelfth came from the live run.
2. **Probe the engine's PURE functions / a throwaway pytest probe first** — no
   browser. One parametrised probe file proved all eight of unit 24's numeric
   claims in a single 30-second run, then was deleted.
3. **Read the FORM FACE, not only the spec.** Unit 24's multiple-employer defect,
   the box-5 special case and the MFS-survives-Part-IV ruling all came from
   `pymupdf`-extracting `resources/irs_forms/2025/f1040s1a.pdf` and reading the
   Part cautions. The spec was silent or annotated-but-unbuilt on all three.
4. **Check the MODEL TYPE, nullability AND DEFAULT of every field.** A
   `BooleanField(default=True)` on an attestation is an overstatement engine; the
   same field defaulting False (tips) is safe. Read the default, don't assume it.
5. **Follow the field past compute into RENDER and E-FILE, and count the
   documents.** Unit 24's worst structural gap is an absence: every sibling
   schedule has a `build_*` and this one has none. Grep the builder's
   `documentId` list — a missing name is the finding.
6. **Ask whether a DIAGNOSTIC exists at all, per form.** `ls apps/diagnostics/`
   and look for the module. Six specced diagnostics, two of them errors, had
   simply never been written.
7. ⚠⚠ **THE REVERT IS A TEST, NOT HOUSEKEEPING.** Compare every line. Unit 24's
   revert came back exactly (unlike s139's stale QBI deduction, which the same
   discipline caught) — that clean result is itself the evidence that the Part IV
   delete path has no residue.
8. **Distinguish a missing-data zero, a legitimate zero, and a DISENGAGEMENT
   zero.** The third is the trap: a threshold row that only gets written when its
   part is engaged reads as "the phaseout starts at $0".

## Dev QA recipe (proven again this session)
preview_start django-demo + vite · demo QA return
`bc270846-5800-4cbc-8f7f-573d0a5a953f` · `scripts/mint_magic_link.py`
(SINGLE-USE — **mint per run**; defaults to the DEMO DB) ·
`scripts/slate_screen_screenshots.mjs <returnId> <tokenFile> "<rail label>"
<slug> [outDir]` · `scripts/qa_unit24.mjs` is the unit-24 driver (needs no seed
— the demo taxpayer's NULL date_of_birth makes the headline defect fire at rest;
the screenshot pass seeds a vehicle + tips + overtime by ORM and reverts).
- ⚠ **The NEW_UI localStorage key is `delvio-new-ui`** (`"1"`/`"0"`), NOT
  `ui.newUI`, and it is read at MODULE LOAD — reload after setting it.
- ⚠⚠⚠ **`settle()` MUST WAIT FOR THE WRITE TO BE *ISSUED* BEFORE WAITING FOR IT
  TO FINISH.** The taxpayer-facts lane DEBOUNCES, so at the moment a scripted
  action returns, `inflight` is still **0**. Two phases: wait (bounded, ~12s) for
  a matching request body to appear, THEN drain, THEN a commit pause. A
  `window.fetch`/`page.on("request")` recorder + an `inflight` counter is the
  cheapest way to judge these lanes — **request BODY, never the response**.
- ⚠⚠ **The per-RECORD PATCH lane's re-render can still lag a settled write.** In
  the unit-24 run the vehicle-lane reads were consistently one action behind
  (the interest PATCH landed 200 but line 23 still read 0; the untick's fresh
  payload had not reached the checkbox when the re-tick was attempted, so it
  reported "already in state"). The taxpayer-facts lane kept up in the same run.
  **Browser proves the screen, ORM + `compute_return` proves the numbers** — do
  not chase the lag, prove the arithmetic the other way.
- ⚠⚠ **A PayerTable add row keeps its text after a REJECTED add** (it only
  remounts when the row count changes) and headless typing **APPENDS** — clear
  with Ctrl+A then **Backspace** before typing the next value, or the second
  attempt silently submits a concatenation of both.
- ⚠⚠ **`Ctrl+A` only SELECTS** — clearing needs an explicit `Backspace`.
  ⚠⚠ **A checkbox click TOGGLES** — assert the target state.
- ⚠ **An ORM create OR delete MUST be followed by `compute_return(tr)`** before
  a baseline is recorded or trusted — and then **compare EVERY line**.
- ⚠ A fact re-read from the ORM after assigning `Decimal("0")` prints
  `Decimal('0.00')` — the DecimalField quantizes on save, so re-fetch from the DB
  before calling a string difference "drift".
- ⚠ **Demo-DB ORM probes need `TTS_ENV=demo`**; run them from Bash with
  `manage.py shell -c "$(cat file)"` (PowerShell mangles multi-line `-c`).
  ⚠ `FormDefinition`'s year field is **`tax_year_applicable` (an int)**.
- ⚠ **Extract a form's own caution text with pymupdf** —
  `fitz.open(path)[page].get_text()` — before writing any prose that quotes it.
- ⚠ **A throwaway pytest probe must live under `server/tests/`** or `django_db`
  silently skips. Delete it before committing.
- ⚠ **A `role="alert"` string split by `<strong>` is unmatchable by
  `getByText`** — collect `getAllByRole("alert")` and match each node's own
  `textContent`. ⚠ `wholeDollars()` returns **no `$`**. ⚠ Text inside a
  PayerTable row EXPANSION needs the expand click first. ⚠ `FieldStateInput`'s
  `invalid` renders the CSS class `is-invalid`, **not** `aria-invalid`.
- ⚠ **Slate primitives must stay OUT of FormEditor's static imports** (the s133
  lazy-chunk rule). A structurally-compatible prop type needs no `import type`
  at all — pass the object and let TS check it structurally.
- ⚠ **The screenshot driver's rail label is a REGEX** — `"^Sch 1-A"` (never
  include the `(OBBBA)` parens unescaped). ⚠ A QA `.mjs` must live **under the
  repo root** (`puppeteer-core` is an ephemeral root install). ⚠ Run vitest
  **from `client/`**. ⚠ `tsc` needs `-p tsconfig.renderer.json`. ⚠ There is NO
  `.slate-summarybar`. ⚠ FFV ORM path = `form_line__section__form__code`.
  ⚠ Commit-message files must be ASCII, passed with `-F`.

**Build rules in force:** presentation-only (the server was untouched) ·
selective `git add` only — NEVER `git add .` (parallel work still unstaged:
`server/apps/returns/views.py`, `tb_import.py`, `tests/test_tb_import.py`,
`server/scripts/create_ar_cutover_clients.py`; ⚠ also never `git stash` here) ·
no merge/deploy without Ken · at deploy: migrate (diagnostics 0005) +
seed_rules BOTH DBs (D_W2_ family + MATH_BALANCE_SHEET description).

**KEN CLARIFIED (2026-07-28): the tax app is TESTING until January 2027.** He
switches to Slate when the redesign is FINISHED; everything rides `slate-ui`;
the shared Supabase DB caution is the one true-production constraint.

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE — trimmed to live items)
1. **(s140) Schedule 1-A has NO diagnostics — all six specced ones are
   unimplemented, two of them errors.** Recommendation: write `rules_sch_1a.py`
   with D_SCH1A_001 (MFS claiming) and 002 (no valid SSN) as errors, 003
   (occupation off the IRS list), 005 (box 5 > $176,100) and 006 (multiple
   employers) as warnings, and **a new one with no spec entry: "a senior-eligible
   filer has no date of birth"** — the highest-value check on the form.
2. **(s140) Schedule 1-A is never transmitted while 1040 line 13b is.** Needs a
   `build_schedule1a` in the y2025 builder against `IRS1040Schedule1A.xsd`.
   Until then a return claiming these deductions is paper-only.
3. **(s140) The tips deduction ignores the W-2 owner** — a non-attesting spouse's
   box 7 tips are deducted. Recommendation: filter line 4a by
   `W2Income.owner` against each filer's attestation (the field already exists).
4. **(s140) Both Part IV attestations default TRUE** — $275 of tax on the proof
   return with nothing affirmed. Recommendation: flip both model defaults to
   False (matching the RS spec's null default and the tips precedent) and add a
   diagnostic for an un-attested row carrying interest.
5. **(s140) Tips from more than one employer** need the form's "-0- on 4a/4b"
   treatment plus the instruction-derived line 4c; D_SCH1A_006 covers it on paper.
6. **(s140) The §224 self-employment gross-income limit on tips (R-TIPS-10) is
   unbuilt** and Schedule C now exists — the deferral reason is stale.
7. **(s140, minor) Only two line-22 vehicle rows print** while line 23 sums them
   all — the same overflow-statement gap as Form 2441's `[:3]`. Worth one shared
   statement-page mechanism before the season.
8. **(s139) Deleting the last QBI source leaves a stale QBI deduction on 1040
   line 13** — $859 understated on the proof return, no diagnostic, two
   recomputes don't clear it. Recommendation: blank line 13 in
   `compute_8995_db`'s not-engaged branch (the override guard already protects a
   real direct entry) and add a "deduction with no QBI source" diagnostic.
   Chip `task_8000a11e`.
9. **(s139) Should `eic_self_employed` be DERIVED rather than asked?** The
   unanswered default silently costs $4,328–$7,152 and no diagnostic covers it.
10. **(s139, minor) Three seeded `1040_EIC` rows are never written.**
11. **(s138) Form 8863 lets ONE student take BOTH education credits** — the same
   $4,000 earns an extra $800, live-proven. §25A(c)(2)(A) forbids it and
   D_8863_DUAL_STUDENT is only a warning.
12. **(s138) The Form 8863 line-7 lockout is global but keyed per student.**
13. **(s138, minor) `compute_8863_db.disengage()` cannot clear a "0" it wrote
   itself** — same shape likely in the other disengage paths guarded on
   `!= ZERO`; item 8 above is that family with real money.
14. **(s138) Form 8962 has NO 100%-of-FPL eligibility floor** — engine-proven, a
   full premium tax credit at 66% of FPL, where §36B(c)(1)(A) is 100–400%.
15. **(s138) Form 2441 deems BOTH spouses for the same months** — engine-proven
   $1,920 vs $0 against an explicit IRS instruction the RS spec never carries.
16. **(s138) A tax-exempt care provider cannot be e-filed** (the extract raises).
17. **(s138, minor) Only three Form 2441 Part I / Part II rows print.**
18. **(s137) FIVE tax-accuracy holes the screens now WARN about but that no
   DIAGNOSTIC covers** — (a) a blank prior-year Sch A **5e** untaxes a whole
   state refund; (b) an **age/blind box count above 4**; (c) a tax year with no
   `PY_STD_DEDUCTION` entry silently uses **2024** constants (2027+); (d) the
   **§165(d) cap** not following the W-2G documents; (e) an explicit **$0
   `family_allocation`** zeroing the HSA deduction.
19. **(s137) Should `scha_gambling_winnings` auto-populate** from the W-2G box-1
   sum + `other_gambling_winnings`?
20. **(s137) `compute_8889_db` stores the FORM_8889 face from accounts[0] only.**
21. **(s137, minor)** `worksheet_2` computes `w8` unclamped while
   `_worksheet_rows` displays `max(0, …)`.
22. **(s130, re-confirmed s136–s140) PATCH `/taxpayer/` and the per-form PATCH
   lanes run tens of seconds in-process**, and the per-record lane's re-render
   can lag a settled write by a whole action. Profile in a session that owns
   views.py.
23. **(s131) Form 7203 panel legacy-styled** inside the Slate K-1 screen.
24. **(s129) The RS authoring session** — scheduled; agenda in REVIEW_QUEUE.
25. **(s129) Launcher menu extras** — no data source; rulings wanted.
26. s124's `D_4562_RECON` scoping pair.
27. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
28. *(cosmetic)* Legacy floating "Calculating…" chip needs a Slate home.
29. *(s126)* Sch B 3a/3b detail tables entry gap; 5 unbuilt Sch B runners.
30. **(s136)** Replace-all nested lists (8915-F item C) remain vulnerable to
    out-of-order server arrival of two overlapping PATCHes.
31. **(s137, bookkeeping)** Neither **STATE_REFUND** nor the sweep's other
    worksheet pseudo-forms have rows in `form_coverage_tracker.md`, though their
    spec/compute/render/diagnostic/test legs all exist. **(s140) The opposite
    problem on SCH_1A**: it HAS a row, tagged `1040-sch1a-complete` with every
    leg ✅ — because the table has no DIAGNOSTICS column and no E-FILE column,
    and both of those legs are empty for this form. The row is annotated and
    re-opened. The table itself needs those two columns before any form's
    all-green line can be trusted.

## Active gates
- **Branch discipline:** `slate-ui` checked out; parallel session's uncommitted
  work UNSTAGED. Never stage/stash/`git add .`.
- ⚠ **Demo DB drift:** diagnostics migration 0005 applied to the DEMO DB only —
  prod applies at Ken's deploy (additive, safe).
- ⚠ Demo QA return (`bc270846…`) carries synthetic review data ON PURPOSE —
  s127 1099-R (TRS $24,000) + s128 1099-INT ($1,250/$300/$50 W/H) + 1099-DIV
  ($800/$600/$150) + SS box 5 $21,600.
  **At rest after the s140 reverts (ORM-verified, ZERO drift): 0 car-loan
  vehicles, all 8 Sch 1-A facts at defaults, 0 dependents, 0 Schedule C,
  0 1040_EIC rows, all 17 `eic_*`/election facts at defaults, 8867 / 8862 / 8995
  rows all blank, 0 care providers, 0 FORM_2441 rows, 0 Forms 1095-A,
  0 FORM_8962 rows, 0 education students, 0 W-2G, 0 HSA accounts, all 17 `sr_*`
  facts at defaults, 0 Forms 8915-F / 8606 / Roth trackers / SS lump-sums /
  1099-G. AGI 94,560, L12 15,750, L13 blank, L13b 0, L15 78,810, L16/L18 12,204,
  L19 0, L20 0, L24 12,204, L27 0, L33 2,450, L37 10,151, Sch 3 line 2 blank,
  1040 1e blank.**
  ⚠ **The taxpayer's `date_of_birth` is NULL and that is the s140 defect-1 live
  proof — do not "fix" it.** SCH_1A rows at rest: 3 = 31 = 94,560, 32 = 75,000,
  33 = 19,560, 34 = 1,174, 35 = **4,826**, 36a/36b/37/38 = 0.
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
