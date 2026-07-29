# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 138 (bespoke-screen sweep units 20 and 21:
FORM 2441 / Child and Dependent Care (§21) and FORM 8962 / Premium Tax Credit
(§36B). Both live-proven end-to-end and fully reverted. FIFTEEN real defects —
including the worst the sweep has found: a blank line 11(f) that swings the
federal balance due by $11,050 and turns a repayment into a refund.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — the bespoke-screen sweep continues at **education credits (Form 8863)**

**s138 shipped units 20 and 21 on `slate-ui` (no deploy): `c7fa893` · `29b2fc4`**

Remaining 1040 screens (~14 of ~39; the count was re-measured in s137 by
mapping every `activeTab` to its section component **file** and checking each
for a `NEW_UI` gate — do it that way, never by scanning FormEditor alone):
**Sch 1-A** ≈342 · **EIC** ≈280 · **5329** ≈278 ·
**6251** ≈264 · **8615** ≈259 · **1116** ≈273 · **8863** ≈170 · **8880** ≈156 ·
**8960** ≈119 · **5695** · **1040-X** · the **state/GA** tab · the
**prior-year / tax-summary** views · the estimates/extension/e-file cards.
Most are small worksheet singletons now that the paradigms are settled.
**The business-entity screens (1120-S / 1065 shareholders, partners, balance
sheets, allocations, 7203, page-1 income/deductions) are a SEPARATE unscoped
lane — ~12 more, none started. Ken's call when to take them.**

### Unit 21 — Form 8962, Premium Tax Credit (§36B), `29b2fc4`
**DocumentTabs** per Form 1095-A (a received information return with its own
policy identity and 36 monthly boxes — the W-2 archetype, not a PayerTable row),
with the 12 × 3 monthly grid and the Part IV allocation rows as nested
`.slate-asstable` blocks, an InputRow facts worksheet, and the computed face from
the server's own FORM_8962 rows as locked ƒx cells. Presentation only.

- ⚠⚠⚠ **DEFECT — A BLANK LINE 11(f) REPORTS THE WHOLE ADVANCE PTC AS ZERO. The
  worst defect the sweep has found.** The annual method (line 10 Yes) reads
  **only** the line 11 year totals; the monthly 1095-A is ignored entirely.
  `D_8962_ANNUAL_INCOMPLETE` checks only 11(a)/11(b) and **explicitly permits** a
  blank 11(f) ("may be blank when there were no advance payments");
  `D_8962_ANNUAL_CONFLICT` only says the monthly amounts are "ignored". So with
  11(a)+11(b) entered, 11(f) blank and a 1095-A carrying APTC, **nothing flags
  it.** Live-proven on the demo return (one 1095-A, 12 × 1,000 premium / 1,100
  SLCSP / 900 advance PTC), three states of the SAME return:
  | state | line 25 APTC | result | balance due |
  |---|---|---|---|
  | monthly (the truth) | 10,800 | Sch 2 1a **REPAY 5,940** | 16,340 |
  | 11(f) filled 10,800 | 10,800 | Sch 2 1a **REPAY 5,939** | 16,339 |
  | **11(f) BLANK** | **0** | Sch 3 9 **REFUND 4,861** | **5,290** |
  A blank 11(f) swings the federal balance due by **$11,050** and turns a $5,940
  repayment into a $4,861 refundable credit. Also proven through the UI: ticking
  line 10 moved advance PTC 10,800 → 0 and the repayment 5,940 → 0. The screen
  now names the discarded advance at the 11(f) cell and says why no diagnostic
  catches it.
- ⚠⚠ **DEFECT — clearing any of the 36 monthly cells was a silent revert.** The
  grid committed CurrencyInput's raw value and `parseCurrency("")` returns `""`.
  Serializer-proven: `{"m03_premium": ""}` → 400 *"A valid number is required"*;
  `null` → 400 *"This field may not be null"*; **only `"0"`** is accepted (all 36
  columns are `DecimalField(default=0)`, NOT NULL). CurrencyInput renders 0 as
  **blank**, so every zero cell looks empty and clearing one is the natural
  correction. Live-proven fixed: `{"m03_premium":"0"}`, credit 4,860 → 4,455.
- ⚠⚠ **DEFECT — the SEHI checkbox said "flagged" for something fully computed.**
  Ticking it runs the Pub 974 iterative SEHI↔PTC loop, **rewrites Schedule 1
  line 17** and reflows AGI (D_8962_SEHI's own text says so). The s136
  SS-lump-sum shape — a screen understating its own engine.
- ⚠ **DEFECT — a month with no column A earns no credit but still repays its
  APTC** (`if prem <= 0: continue`). Engine-proven: premium 0 all year with
  900/mo advanced → PTC 0, APTC 10,800, **repayment 975**. Now an error on that
  month's column A, and the policy tab carries the error dot.
- ⚠ **DEFECT — no 100%-of-FPL eligibility floor.** §36B(c)(1)(A) is 100–400%;
  engine-proven, the full credit is granted at **66% of FPL**. Deliberately a
  CONFIRM, not an error — §1.36B-2(b)(6) covers the common
  advanced-on-a-higher-estimate case. → REVIEW_QUEUE.
- ⚠ **DEFECT — the 400% cliff was invisible.** `repayment_limit` returns None at
  ≥400% and line 28 is then written **0**, which reads as "no cap needed" when it
  means "no cap exists". Engine-proven: household 60,000 → cap 1,625; **60,241 →
  no cap at all**.
- ⚠ **DEFECT — a 2026 return silently computes nothing** (`if tax_year != 2025:
  disengage()`), leaving Sch 3 line 9 and Sch 2 line 1a **blank, not $0**.
  D_8962_2026 is a RED but the screen said nothing.
- ⚠ **DEFECT — there was no computed face** beyond line 11 c/d/e in annual mode:
  no family size, FPL, FPL%, applicable figure, contribution, PTC, advance, net
  PTC, excess, cap or repayment. On a reconciliation form.

### Unit 20 — Form 2441, Child & Dependent Care (§21), `c7fa893`
A **singleton screenbar** hosting three paradigms that NEST: a bare
`.slate-asstable` over the existing Dependent rows for Part II (nothing is
added or deleted here, so a PayerTable add row would be a lie), **PayerTable**
for the Part I providers with the address block in the expansion, an
**InputRow** facts worksheet, and the whole computed face from the server's own
FORM_2441 rows as locked ƒx cells. Presentation only — the server was untouched.

- ⚠⚠ **DEFECT — the credit's gate lived on ANOTHER TAB and this screen said
  "✓ qualifies" anyway.** `qualifying_dependents()` requires
  `Dependent.claims_dependent_care`; legacy's `qualifies()` tested only
  under-13/disabled, and that **claim-blind** count drove the displayed
  "N qualifying persons with expenses → expense cap $X". Engine-proven on
  identical facts (one under-13 dependent, $6,000 expenses, $50k/$50k earned,
  AGI $100k): **line 3 3,000 / CREDIT 600 claimed vs line 3 0 / CREDIT 0
  unclaimed.** Live-proven: at rest the form was **fully disengaged** (0
  FORM_2441 rows, Sch 3 line 2 blank) while the legacy screen printed
  "Quinn Sample ✓ qualifies · 1 qualifying person with expenses → expense cap
  $3,000" — see the committed `legacy-f2441.png`. D_2441_007 exists but is only
  `info`. The new screen shows each row's real claim state, computes the cap from
  the CLAIMED count, errors by name, and makes the flag **editable at the row**
  (the Dependent PATCH lane generalized from care_expenses-only).
- ⚠⚠ **DEFECT — the engine deems BOTH spouses for the same months.** i2441
  lines 4/5: *"only one of you can be treated as having earned income in that
  month."* Engine-proven: both flags, 12 months, two persons, AGI $20k →
  line 4 = line 5 = 6,000, **CREDIT 1,920**; only-the-spouse-deemed → **0**.
  The RS spec and source brief are **both silent** (the spec merely names "the
  MFJ both-deemed case"), so per the Authoritative-Source Rule this is FLAGGED,
  not filled → REVIEW_QUEUE. The screen errors at the two checkboxes.
- ⚠ **DEFECT — no ZIP input existed.** `CareProvider.zip_code` is on the model,
  the serializer and the client row type, and it is printed inside the column (b)
  address string **and** transmitted as the e-file `ZIPCd`. Added to the row
  expansion; live-proven committing `{"zip_code":"30601"}`.
- ⚠ **DEFECT — the tax-exempt checkbox produced an UNFILEABLE return.** It says
  "no TIN required" and D_2441_004 forgives it, but `render_2441` prints column
  (c) blank and the e-file extract **raises `UnmappableValue`** without a 9-digit
  TIN. i2441 wants the literal **"Tax-Exempt"** there — the screen now says so
  (live-proven: typing it cleared the error). The e-file leg still cannot map it
  → REVIEW_QUEUE.
- ⚠ **DEFECT — only THREE rows print** (`providers[:3]`,
  `qualifying_dependents(...)[:3]`; the overflow statement page is unbuilt).
  Unlimited rows could be entered with no notice.
- ⚠ **DEFECT — "Months (max 12)" disagreed with the engine in BOTH directions.**
  `min(12, int(stored or 12))`: a stored **0 is falsy → the engine uses 12**
  (deeming $3,000 for one person, not $0) while the cell displays 0; and 20 is
  silently clamped. Both now error.
- ⚠ **DEFECT — the screen showed NO computed result at all** — no line 3/6/8/9/11,
  no credit, no percentage. The face now renders lines 3–12/24/27/31, and each of
  the three ways the credit legitimately lands at $0 (no earned income / benefits
  consumed the limit / the nonrefundable clamp) explains itself rather than
  showing a bare zero.

**Gates at s138 close:** vitest **974/974** (+54 across the two units) · tsc
**46 = baseline** · side-by-sides committed
(`Design/slate-phase2-screenshots/{legacy,slate}-{f2441,f8962}.png`) · every QA
PATCH **200** · demo QA writes reverted with **ZERO drift** against the pre-seed
baseline, twice (unit 20 and unit 21 each verified independently).

**Next action: continue the sweep at the education credits (Form 8863)**, then
EIC, Sch 1-A, 8880, 8615, 5329, 6251, 8960, 1116, 5695, 1040-X, the state tab,
and the estimates/extension/e-file cards. Paradigms settled: view-over-container;
**PayerTable** for flat record lists keyed by row id, **DocumentTabs +
worksheet** for card stacks AND per-filed-form rows, **InputRow worksheets** for
facts cards (screenbar header for singletons), **the asset register** for
computed sub-schedule grids, a bare **`.slate-asstable`** for a grid with no add
row (a derived row set, or a nested replace-all list whose rows have no id);
paradigms may NEST; multi-section tabs share ONE `.slate-screen` at the call
site; screenshots per screen; live QA writes reverted.

## 🔑 Method that is finding these defects (keep doing this)
1. **Read the engine against the screen before writing any JSX.** All fifteen
   defects across units 20 and 21 came from that, not from the UI work.
2. **Probe the engine's PURE functions first** — no DB, no browser. One
   `compute_2441_lines` probe proved the claim-flag gap, the both-deemed
   overstatement, the DCB chain and the tax-liability clamp in a single run.
3. **Check the MODEL TYPE and nullability of EVERY field in a blank map** —
   nullable-sent-"0" and non-nullable-sent-"" are both silent-revert factories.
4. **Follow the field past compute into RENDER and E-FILE.** Unit 20's defects
   3, 4 and 5 are invisible from compute alone: a column with no input, a
   checkbox whose path raises, and a silent `[:3]` truncation.
4b. **Ask what the DIAGNOSTIC actually tests, not what its name suggests.** Unit
   21's worst defect hid behind two rules that both "cover" the annual method:
   one checks 11(a)/11(b) and *explicitly permits* the blank 11(f) that costs
   $11,050, the other only says data is "ignored". Read the rule body and the
   exact predicate before concluding a hole is covered.
4c. **Where two modes read disjoint inputs, the inactive side is a silent
   liability.** "The methods never mix" is a correct safeguard that also means a
   switch can discard a whole information return. Say what is being discarded,
   in dollars.
5. **When the RS spec is SILENT on a rule the IRS states, flag it — never fill
   it.** Verify the rule against the instructions first so the flag is right.
6. **Distinguish a missing-data zero from a legitimate zero** and explain both.

## Dev QA recipe (proven again this session)
preview_start django-demo + vite · demo QA return
`bc270846-5800-4cbc-8f7f-573d0a5a953f` · `scripts/mint_magic_link.py`
(SINGLE-USE — **mint per run**; defaults to the DEMO DB) ·
`scripts/slate_screen_screenshots.mjs <returnId> <tokenFile> "<rail label>"
<slug> [outDir]` · `scripts/qa_unit20.mjs` · `scripts/qa_unit21.mjs` · the unit-20
and unit-21 ORM seed/revert pairs lived in the scratchpad (a Dependent +
CareProvider, and a Form 1095-A, are needed before those screens show anything).
- ⚠⚠⚠ **`settle()` MUST WAIT FOR THE WRITE TO BE *ISSUED* BEFORE WAITING FOR IT
  TO FINISH.** The taxpayer-facts lane DEBOUNCES (dirty set + `scheduleFlush`),
  so at the moment a scripted action returns, `inflight` is still **0** — the
  request does not exist yet. A settle that only waits for `inflight → 0` returns
  instantly and every value read after it is stale: unit 21's first run reported
  "(no write issued)" for three fields and read pre-write numbers, which looked
  exactly like the screen being broken. Two phases: wait (bounded, ~12s) for a
  matching request to appear, THEN drain, THEN a commit pause. This is the
  s136 in-flight lesson's mirror image — that one was "don't settle on silence",
  this one is "silence can mean not-yet-started".
- ⚠⚠ **Consecutive scripted writes on the facts lane still coalesce** even with
  the two-phase settle (~11s per PATCH, ~300KB `fresh_return` each, and the
  repaint can land mid-type). For multi-field arithmetic proofs, set the facts by
  **ORM + `compute_return`** and read the rows back — the browser proves the
  screen, the ORM proves the numbers. Don't burn a session fighting the lane.
- ⚠⚠ **A WRITING pass adds one `400` per write to the console — that is the
  §6695(g) DUE-DILIGENCE PRINT GATE, not a defect.** FormViewPane re-POSTs
  `render-pdf/` after every save, and `_enforce_print_gate` 400s while
  `due_diligence_print_blockers` is non-empty. This session's seeded dependent
  made Form **8867** due diligence required, so 7 writes produced 6–7 400s; at
  rest the blocker list is empty and the noise returns to the read-only 403/404
  baseline. Verify by blocker list, don't chase the 400.
- ⚠⚠ **Judge a slow write by its REQUEST BODY + an in-flight settle**, never by
  awaiting the response (a per-record PATCH re-derives the whole return).
- ⚠⚠ **`Ctrl+A` only SELECTS** — clearing needs an explicit `Backspace`.
  ⚠⚠ **A checkbox click TOGGLES** — assert the target state.
- ⚠ **An ORM create OR delete MUST be followed by `compute_return(tr)`** before
  a baseline is recorded or trusted.
- ⚠ **Demo-DB ORM probes need `TTS_ENV=demo`**; run them from Bash with
  `manage.py shell -c "$(cat file)"` (PowerShell mangles multi-line `-c`).
- ⚠ **A `role="alert"` string split by `<strong>` is unmatchable by
  `getByText`** — assert on the alert node's own `textContent`.
  ⚠ `FieldStateInput`'s `invalid` renders the CSS class `is-invalid`, **not**
  `aria-invalid`.
- ⚠ **The screenshot driver's legacy pass needs ≥2 inputs/selects on screen**, so
  seed a record first. ⚠ A QA `.mjs` must live **under the repo root**
  (`puppeteer-core` is an ephemeral root install). ⚠ Run vitest **from
  `client/`**. ⚠ `tsc` needs `-p tsconfig.renderer.json`. ⚠ NEW_UI reads at
  module load — reload after setting localStorage. ⚠ There is NO
  `.slate-summarybar`. ⚠ FFV ORM path = `form_line__section__form__code`.
  ⚠ A rail label passed to the screenshot driver is a **REGEX**.
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
1. **(s138) Form 8962 has NO 100%-of-FPL eligibility floor** — engine-proven, a
   full premium tax credit at 66% of FPL, where §36B(c)(1)(A) is 100–400%. Genuinely
   nuanced: the §1.36B-2(b)(6) safe harbour makes it CORRECT for the common
   advanced-on-a-higher-estimate case and wrong only where no APTC was paid.
   Recommendation: a `warning` that distinguishes the two, not an error.
2. **(s138) Form 2441 deems BOTH spouses for the same months** — engine-proven
   $1,920 vs $0 against an explicit IRS instruction the RS spec never carries.
   Needs a spec amendment + a per-owner months allocation + a diagnostic.
3. **(s138) A tax-exempt care provider cannot be e-filed** (the extract raises)
   and printed a blank TIN column. The screen now instructs "Tax-Exempt";
   the IRS2441 mapping still needs a ruling.
4. **(s138, minor) Only three Part I / Part II rows print** — is the overflow
   statement page wanted before the season?
5. **(s137) FIVE tax-accuracy holes the screens now WARN about but that no
   DIAGNOSTIC covers** — each a silent wrong number on a filed return:
   (a) a blank prior-year Sch A **5e** untaxes a whole state refund;
   (b) an **age/blind box count above 4**; (c) a tax year with no
   `PY_STD_DEDUCTION` entry silently uses **2024** constants (2027+); (d) the
   **§165(d) cap** not following the W-2G documents; (e) an explicit **$0
   `family_allocation`** zeroing the HSA deduction. Recommendation: seed (a),
   (d), (e) as errors and (b), (c) as warnings.
6. **(s137) Should `scha_gambling_winnings` auto-populate** from the W-2G box-1
   sum + `other_gambling_winnings`?
7. **(s137) `compute_8889_db` stores the FORM_8889 face from accounts[0] only.**
   The PDF is correct per owner; confirm the stored rows are feeders-only.
8. **(s137, minor)** `worksheet_2` computes `w8` unclamped while
   `_worksheet_rows` displays `max(0, …)` — confirm which the statement shows.
9. **(s130, re-confirmed s136/s137/s138) PATCH `/taxpayer/` and the per-form
   PATCH lanes run tens of seconds in-process.** Profile in a session that owns
   views.py.
10. **(s131) Form 7203 panel legacy-styled** inside the Slate K-1 screen.
11. **(s129) The RS authoring session** — scheduled; agenda in REVIEW_QUEUE.
12. **(s129) Launcher menu extras** — no data source; rulings wanted.
13. s124's `D_4562_RECON` scoping pair.
14. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
15. *(cosmetic)* Legacy floating "Calculating…" chip needs a Slate home.
16. *(s126)* Sch B 3a/3b detail tables entry gap; 5 unbuilt Sch B runners.
17. **(s136)** Replace-all nested lists (8915-F item C) remain vulnerable to
    out-of-order server arrival of two overlapping PATCHes.
18. **(s137, bookkeeping)** Neither **STATE_REFUND** nor the sweep's other
    worksheet pseudo-forms have rows in `form_coverage_tracker.md`, though their
    spec/compute/render/diagnostic/test legs all exist. Worth a short
    reconciliation pass.

## Active gates
- **Branch discipline:** `slate-ui` checked out (pushed through `c7fa893`);
  parallel session's uncommitted work UNSTAGED. Never stage/stash/`git add .`.
- ⚠ **Demo DB drift:** diagnostics migration 0005 applied to the DEMO DB only —
  prod applies at Ken's deploy (additive, safe).
- ⚠ Demo QA return (`bc270846…`) carries synthetic review data ON PURPOSE —
  s127 1099-R (TRS $24,000) + s128 1099-INT ($1,250/$300/$50 W/H) + 1099-DIV
  ($800/$600/$150) + SS box 5 $21,600.
  **At rest after the s138 reverts (ORM-verified, ZERO drift): 0 dependents,
  0 care providers, 0 FORM_2441 rows, all six `f2441_*` facts at defaults,
  0 Forms 1095-A, 0 FORM_8962 rows, all nine `f8962_*` facts at defaults,
  0 W-2G, 0 HSA accounts, all 17 `sr_*` facts at defaults,
  `scha_gambling_losses`/`_winnings` 0, 0 Forms 8915-F / 8606 / Roth trackers /
  SS lump-sums / 1099-G. AGI 94,560, L15 78,810, L16/L18 12,204, L19 0, L20 0,
  L24 12,204, L33 2,450, L37 10,151, Sch 3 line 2 blank, 1040 1e blank.**
  ⚠ Due-diligence print blockers are EMPTY at rest — a non-empty list means a
  QA seed is still in place.
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
