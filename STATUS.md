# TTS Tax App - STATUS (current state only)

*Last updated: 2026-07-30, session 142 (Ken's backlog redirect: **LEG 1 —
diagnostics only — is COMPLETE**. Three commits on `slate-ui`, no deploy:
`2ed5eff` Schedule 1-A diagnostics (7 rules where there were ZERO),
`42eb851` the two Form 5329 fixes, `d991b50` the s137 five + two severity
promotions. **12 new diagnostics, 2 promotions, 1 dual-awareness rebuild, 71
new tests, no compute change, no migration.** Next: **LEG 2** — the compute
fixes with real dollars, each needing an RS spec fetch, the flow-assertion
gate and a Ken deploy.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — **LEG 2 of Ken's 2026-07-30 backlog: the compute fixes.**

Ken's redirect stands: *"You can fix those items first and then go back to the
screens."* The screen sweep is **PAUSED at 29 of ~39** and resumes at **Form
6251 (AMT)** once the backlog is clear.

**LEG 1 is done.** Every item below is written up in full in `REVIEW_QUEUE.md`
— read the entry before touching code; each carries its engine proof and my
recommendation.

### ▶ LEG 2 — compute fixes with real dollars. **START HERE.**
Each one needs the RS spec fetched first (the CLAUDE.md gate), the
flow-assertion gate run after (`pytest tests/test_flow_assertions.py -v`), and
a **Ken deploy**. Diagnostics now EXIST for four of the five, so the wrong
number is at least loud while the compute fix is pending.
1. **The stale QBI deduction on 1040 line 13** (s139, chip `task_8000a11e`) —
   $859 understated on the proof return. Blank line 13 in `compute_8995_db`'s
   not-engaged branch (`write_line_13`'s `is_overridden` guard already protects
   a real direct entry). **Do this one first** — smallest change, clearest
   proof, and `D_8995_STALE` (s142) already reports it.
2. **Sweep the `disengage()`-guarded-on-`!= ZERO` family** while in there (s138)
   — item 1 is that family with money attached; a `disengage()` cannot clear a
   "0" it wrote itself.
3. **Form 5329 Part I: blend the SIMPLE rate** (s141) — split line 1 into its
   SIMPLE and non-SIMPLE components in `owner_early_distributions` (the 1099-R
   codes already distinguish them), apportion the line-2 exception across the
   two, blend 25%/10%. $7,500–$15,000 overstated today; `D_RET_007` (s142) now
   warns about it per owner.
   ⚠⚠ **The RS spec's R-5329-02 carries the same shortcut**, so implementing the
   correct law puts the app AHEAD of the spec. Flag it loudly in the commit and
   in `REVIEW_QUEUE.md` — do NOT quietly diverge, and do not wait either: Ken
   has authorised the fix.
4. **Form 8863: one student cannot take BOTH credits** (s138) — have compute
   drop the LLC expenses for any student whose AOTC is allowed. $800 on the
   proof return. `D_8863_DUAL_STUDENT` is now an **error** (s142) so it blocks
   in the meantime.
5. **Schedule 1-A tips: filter line 4a by `W2Income.owner`** against each
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

### ▶ THE PAUSED SCREEN SWEEP — resume at **Form 6251 (AMT)**
29 of ~39 1040 screens are converted. Remaining (the count was re-measured in
s137 by mapping every `activeTab` to its section component **file** and checking
each for a `NEW_UI` gate — do it that way, never by scanning FormEditor alone):
**6251** ≈264 · **8615** ≈259 · **1116** ≈273 · **8880** ≈156 ·
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
work still unstaged: `server/apps/returns/views.py`, `tb_import.py`,
`tests/test_tb_import.py`, `server/scripts/create_ar_cutover_clients.py`;
⚠ also never `git stash` here) · no merge/deploy without Ken · at deploy:
migrate (diagnostics 0005) + **`seed_rules` on BOTH DBs** (s142's 12 new rules +
the D_5329_003 / D_8863_DUAL_STUDENT severity promotions, plus the earlier
D_W2_ family + MATH_BALANCE_SHEET description).

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
2. **(s141) Form 5329 charges the 25% SIMPLE rate on the WHOLE of line 3.**
   $7,500–$15,000 overstated. R-5329-02 carries the same shortcut. LEG 2 item 3.
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
9. **(s139) A stale QBI deduction on 1040 line 13** — $859 understated. **Now
   DIAGNOSED** (D_8995_STALE, s142). LEG 2 item 1. Chip `task_8000a11e`.
10. **(s139) Should `eic_self_employed` be DERIVED rather than asked?** The
   unanswered default silently costs $4,328–$7,152 and no diagnostic covers it.
11. **(s139, minor) Three seeded `1040_EIC` rows are never written.**
12. **(s138) Form 8863 lets ONE student take BOTH education credits** — $800,
   live-proven. **Now an ERROR** (s142); the compute fix is LEG 2 item 4.
13. **(s138) The Form 8863 line-7 lockout is global but keyed per student.**
14. **(s138, minor) `compute_8863_db.disengage()` cannot clear a "0" it wrote
   itself** — same shape likely in the other `!= ZERO`-guarded disengage paths.
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
    The table needs those two columns.

## Active gates
- **Branch discipline:** `slate-ui` checked out; parallel session's uncommitted
  work UNSTAGED. Never stage/stash/`git add .`.
- ⚠ **Demo DB drift:** diagnostics migration 0005 applied to the DEMO DB only —
  prod applies at Ken's deploy (additive, safe). **`seed_rules` has been run on
  the DEMO DB for all of s142's rules; PROD seeds at Ken's deploy.**
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
