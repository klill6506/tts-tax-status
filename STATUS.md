# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 139 (bespoke-screen sweep unit 23: the
EARNED INCOME CREDIT, §32 → Schedule EIC → 1040 line 27a. Live-proven
end-to-end and fully reverted. TWELVE real defects — including a tri-state
whose unanswered default silently costs a low-income client $4,328–$7,152, and a
COMPUTE defect found by the revert itself: a stale QBI deduction that survives
the deletion of every QBI source and understates the tax by $859.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — the bespoke-screen sweep continues at **Schedule 1-A (OBBBA)**

**s139 shipped unit 23 on `slate-ui` (no deploy): `4c1e51c`**

Remaining 1040 screens (~12 of ~39; the count was re-measured in s137 by
mapping every `activeTab` to its section component **file** and checking each
for a `NEW_UI` gate — do it that way, never by scanning FormEditor alone):
**Sch 1-A** ≈342 · **5329** ≈278 · **6251** ≈264 · **8615** ≈259 ·
**1116** ≈273 · **8880** ≈156 · **8960** ≈119 · **5695** · **1040-X** ·
the **state/GA** tab · the **prior-year / tax-summary** views · the
estimates/extension/e-file cards.
Most are small worksheet singletons now that the paradigms are settled.
**The business-entity screens (1120-S / 1065 shareholders, partners, balance
sheets, allocations, 7203, page-1 income/deductions) are a SEPARATE unscoped
lane — ~12 more, none started. Ken's call when to take them.**

### Unit 23 — the Earned Income Credit (§32), `4c1e51c`
A **singleton screenbar** (the unit-17/unit-20 shape) hosting a bare
`.slate-asstable` over the EXISTING Dependent rows for the qualifying-child
determination (nothing is added or deleted here), InputRow worksheets for every
§32 eligibility fact and election, and the whole computed face from the server's
own **1040_EIC** rows as locked ƒx cells. The tab's three stacked views (EIC +
the due-diligence attestation + the 8867/8862 checklist) share **ONE**
`.slate-screen` at the call site (the s130 Schedule A ruling). Presentation only
— the server was untouched.

- ⚠⚠⚠ **DEFECT — "Self-employed at any time in the year" is the GATE that admits
  every dollar of self-employment income into the EIC, and UNANSWERED means NO.**
  `compute_earned_income` reaches Worksheet B only when `self_employed` is
  truthy, and `bool(None) is False`, so an unanswered tri-state and an explicit
  *No* are the same thing: earned income is computed from **wages only** and the
  Schedule C profit is discarded. Engine-proven (HOH, no wages, Sch 1 L3 20,000 /
  L15 1,413): unanswered → earned **0**, CREDIT **0**; *Yes* → earned **18,587**
  (plateau), CREDIT **4,328**. Two qualifying children: **7,152 vs 0**. **NO
  DIAGNOSTIC COVERS IT** — `rules_eic.py` carries no self-employment rule other
  than the clergy defer (D_EIC_015), and nothing derives the flag from a
  Schedule C. Live-proven on the demo QA return: answering *Yes* moved the
  server's own Worksheet B rows from 0 to **1e 18,587 / 4b 68,587** (earned
  income 50,000 → 68,587) — the mechanism proven on the pre-phaseout row,
  because this return's AGI puts the credit at $0 either way (the unit-22
  lesson). A refundable credit worth four to seven thousand dollars, lost to an
  unanswered neutral-looking question. → REVIEW_QUEUE (derive it, don't ask it).
- ⚠⚠ **DEFECT — the main-home exception label stated the OPPOSITE of the rule,
  and the other half of the disqualifier had no input at all.** The label read
  *"NOT in the U.S. (incl. Puerto Rico / territories)"* — telling the preparer
  PR/territories count as the U.S. Pub 596 Rule 14, the seeded source brief
  ("US = 50 states + DC, NOT PR/territories") and D_EIC_012's own message all
  say the reverse. And `eic_puerto_rico_territory_home` — the second arm of
  `eic_disqualifiers`' Rule 14 test — was typed on TaxpayerData and exposed by
  the serializer but **absent from `EIC_TAXPAYER_FIELDS`**, so no rendering
  could ever set it. Direction: **OVERSTATEMENT** — an ineligible return claims
  the credit. Both fixed in the shared const, so the legacy screen is corrected
  by the same lines. Live-proven: `{"eic_puerto_rico_territory_home":true}` →
  200, and the D_EIC_012 zero explanation appeared immediately.
- ⚠⚠ **DEFECT — Form 2555 zeroes the EIC and its only checkbox lives on another
  tab, labelled for a different credit.** `files_form_2555` → D_EIC_006 → line
  27a = 0 (Rule 5); the box is on the Credits (8812) screen as "…— ACTC becomes
  0" and the EIC screen never mentioned 2555. The unit-20 shape.
- ⚠⚠ **DEFECT — Rule 2 (valid SSN) is fail-safe, invisible and un-overridable.**
  `eic_valid_ssn_taxpayer is not True` disqualifies, so an **unanswered** fact
  zeroes the credit — and the fact is derived as a **serializer side effect**
  (`TaxpayerSerializer._derive_person`), only when an SSN is present and only on
  a create/update through the API, so a taxpayer written by an importer or a
  seeder keeps `null` forever. **DB-proven on the demo database: of the 7
  taxpayers that HAVE an SSN, 3 carry `eic_valid_ssn_taxpayer` = NULL.** The
  `_overridden` companion exists for exactly this; legacy offered no control for
  either half. Now answerable at the screen for taxpayer and spouse.
- ⚠ **DEFECT — an explicit 0 in the Worksheet B overrides is silently replaced.**
  `se_net = _dec(tp.eic_se_net_earnings) or _dec(sch1_l3)` — a stored 0 is
  FALSY, so the cell displays 0 while the engine uses Schedule 1 line 3. The
  unit-20 `or 12` shape; live-proven with the real 20,000 / 1,413.
- ⚠ **DEFECT — answering ANY tri-state engages the whole EIC machinery.**
  `eic_engaged` fires on `any(fact is not None)`, so answering "No" to the
  informational "Form 4797 present" question turns EIC on and can raise
  eligibility ERRORs on a return that is not claiming the credit.
- ⚠ **DEFECT — the screen showed NO computed result at all** — no earned income,
  no investment total against the §32(i) limit, no table lookup, no lower-of-AGI
  comparison, no credit. On a credit with a $50-bracket table and a hard
  investment-income cliff. The whole face now renders from 1040_EIC as locked ƒx
  cells and every legitimate $0 names its own disqualifier.
- ⚠ **DEFECT — three seeded 1040_EIC rows are never written** (`step5_1`,
  `step5_2`, `wsb_6`), so the audit trail's line 1 and line 2 are permanently
  blank while "line 3 = line 1 − line 2" carries a value. The screen shows the
  wages and the waiver from Form 1040 / Schedule 1 directly and says so.
- ⚠ **DEFECT — the investment-income note understated what counts.** Legacy said
  "capital-gain distributions flow in automatically"; the engine adds
  `max(0, 1040 line 7)` — the WHOLE Schedule D net gain. Engine-proven: 200
  interest + 800 dividends + a 12,000 stock gain = **13,000 > the 11,950 limit**
  → the entire EIC is disqualified.
- ⚠ **DEFECT — the combat-pay election governs the EIC only.**
  `nontaxable_combat_pay` is one shared field: Schedule 8812 adds it to ACTC
  earned income **unconditionally** (line 18b). Correct law, invisible coupling.
- ⚠ **DEFECT — a flagged qualifying child's determinable §32(c)(3) failures and
  its TIN type were invisible.** Both now show per row (relationship "Other",
  the 4a/4b age test, ≤6 months resided, a joint return → D_EIC_QC_CONFLICT /
  D_SEI_001, both `error`), and a flagged child without a **valid SSN** silently
  drops the return to the childless column (D_EIC_010).
- ⚠⚠ **DEFECT (COMPUTE — found by the REVERT, not by the screen) → REVIEW_QUEUE:
  deleting the last QBI source leaves a STALE QBI DEDUCTION on 1040 line 13.**
  `compute_8995_db`'s not-engaged branch blanks the 8995 sibling rows but leaves
  line 13 untouched ("direct entry survives" — which `write_line_13`'s
  `is_overridden` guard already handles). Reproduced cleanly on the demo return:

  | state | 11 (AGI) | 13 (QBI) | 15 | 16 | 37 (owed) |
  |---|---|---|---|---|---|
  | baseline | 94,560 | *(blank)* | 78,810 | 12,204 | 10,151 |
  | + a Schedule C, 20,000 | 113,147 | 3,717 | 93,680 | 15,471 | 16,500 |
  | **Schedule C DELETED** | 94,560 | **3,717** | **75,093** | **11,379** | **9,292** |

  AGI returns correctly while the deduction persists — taxable income understated
  by 3,717, the amount owed by **$859**, on a return with no qualified business
  income at all. Two further recomputes do not clear it; no diagnostic flags it.
  The s138 `disengage()`-guarded-on-`!= ZERO` family, with real money attached.
  Chip `task_8000a11e`.

**Gates at s139 close:** vitest **1050/1050** (+48) · tsc **46 = baseline** ·
side-by-sides committed
(`Design/slate-phase2-screenshots/{legacy,slate}-eic.png`) · every QA PATCH
**200** (the `render-pdf` 400s are the §6695(g) print gate — the seeded dependent
made Form 8867 due diligence required) · demo QA writes reverted to the exact
s138 baseline with **ZERO drift**, verified by ORM (1040 lines, 0 dependents,
0 Schedule C, 0 1040_EIC rows, all EIC facts at defaults, 8867/8862/8995 all
blank, print blockers empty).

**Next action: continue the sweep at Sch 1-A (OBBBA)**, then 5329, 6251, 8615,
1116, 8880, 8960, 5695, 1040-X, the state tab, and the
estimates/extension/e-file cards. Paradigms settled: view-over-container;
**PayerTable** for flat record lists keyed by row id, **DocumentTabs +
worksheet** for card stacks AND per-filed-form rows, **InputRow worksheets** for
facts cards (screenbar header for singletons), **the asset register** for
computed sub-schedule grids, a bare **`.slate-asstable`** for a grid with no add
row (a derived row set, a nested replace-all list, or an EXISTING record set
this screen only annotates); paradigms may NEST; multi-section tabs share ONE
`.slate-screen` at the call site; screenshots per screen; live QA writes
reverted.

## 🔑 Method that is finding these defects (keep doing this)
1. **Read the engine against the screen before writing any JSX.** All twelve of
   unit 23's findings came from that and from the revert — none from the UI work.
2. **Probe the engine's PURE functions first** — no DB, no browser. One
   `compute_earned_income` + `eic_credit_amount` probe proved the whole
   self-employment gate ($0 vs $4,328 vs $7,152) in a single run.
3. **Check the MODEL TYPE and nullability of EVERY field in a blank map** —
   nullable-sent-"0" and non-nullable-sent-"" are both silent-revert factories.
4. **Follow the field past compute into RENDER and E-FILE**, and ask **who WRITES
   a fact**. Unit 23's Rule-2 defect is a serializer side effect, not a compute
   rule: a fact that only gets derived on an API save is `null` forever on any
   record an importer or seeder created — and `is not True` made that a $0 credit.
5. **Ask what the DIAGNOSTIC actually tests, not what its name suggests** — and
   whether one exists at all. Grep the rules module for the field before
   assuming a hole is covered; unit 23's worst defect has no rule of any severity.
6. **When the RS spec is SILENT on a rule the IRS states, flag it — never fill
   it.** Verify the rule against the instructions first so the flag is right.
7. **Distinguish a missing-data zero from a legitimate zero** and explain both.
8. ⚠⚠ **THE REVERT IS A TEST, NOT HOUSEKEEPING.** Unit 22's disengage residue and
   unit 23's stale QBI deduction were both found *only* because the baseline was
   re-verified line by line. Compare every line, not just the ones you touched:
   AGI came back exactly right while the deduction below it did not.
9. **When a phaseout hides the dollar effect, prove the mechanism on the
   PRE-phaseout row** (earned income, not the credit) instead of perturbing
   income.

## Dev QA recipe (proven again this session)
preview_start django-demo + vite · demo QA return
`bc270846-5800-4cbc-8f7f-573d0a5a953f` · `scripts/mint_magic_link.py`
(SINGLE-USE — **mint per run**; defaults to the DEMO DB) ·
`scripts/slate_screen_screenshots.mjs <returnId> <tokenFile> "<rail label>"
<slug> [outDir]` · the unit-23 ORM seed/revert/repro trio lived in the
scratchpad (a Dependent flagged `eic_qualifying_child` **and** a Schedule C are
needed before the EIC screen computes anything).
- ⚠ **The NEW_UI localStorage key is `delvio-new-ui`** (`"1"`/`"0"`), NOT
  `ui.newUI`, and it is read at MODULE LOAD — reload after setting it.
- ⚠⚠⚠ **`settle()` MUST WAIT FOR THE WRITE TO BE *ISSUED* BEFORE WAITING FOR IT
  TO FINISH.** The taxpayer-facts lane DEBOUNCES, so at the moment a scripted
  action returns, `inflight` is still **0** — the request does not exist yet.
  Two phases: wait (bounded, ~25s) for a matching request body to appear, THEN
  drain, THEN a commit pause. A `window.fetch` wrapper recording
  `{method, url, body, status}` + an `inflight` counter is the cheapest way to
  judge these lanes — **request BODY, never the response**.
- ⚠ **The harness `javascript_tool` call has its own ~30s ceiling**, well under a
  slow PATCH lane. Fire the action in one call and read `window.__qa.log` in the
  next rather than awaiting the settle inside a single call.
- ⚠⚠ **Consecutive scripted writes on the facts lane still coalesce.** For
  multi-field arithmetic proofs, set the facts by **ORM + `compute_return`** and
  read the rows back — the browser proves the screen, the ORM proves the numbers.
- ⚠⚠ **A WRITING pass adds one `400` per write to the console — that is the
  §6695(g) DUE-DILIGENCE PRINT GATE, not a defect.** Verify by
  `apps.tts_forms.views.due_diligence_print_blockers(tr)`; at rest it is empty.
- ⚠⚠ **`Ctrl+A` only SELECTS** — clearing needs an explicit `Backspace`.
  ⚠⚠ **A checkbox click TOGGLES** — assert the target state.
- ⚠ **An ORM create OR delete MUST be followed by `compute_return(tr)`** before
  a baseline is recorded or trusted — and then **compare EVERY line**.
- ⚠ **Demo-DB ORM probes need `TTS_ENV=demo`**; run them from Bash with
  `manage.py shell -c "$(cat file)"` (PowerShell mangles multi-line `-c`).
  ⚠ `FormDefinition`'s year field is **`tax_year_applicable` (an int)**, not
  `tax_year`.
- ⚠ **A `role="alert"` string split by `<strong>` is unmatchable by
  `getByText`** — collect `getAllByRole("alert")` and match each node's own
  `textContent`; same for `.slate-rownote` prose. Two renders in ONE test leave
  both trees in the container — split the cases instead.
  ⚠ `wholeDollars()` returns **no `$`** — assert `"4,328"`, not `"$4,328"`.
  ⚠ An `aria-label` reused by an entry cell AND its computed twin makes
  `getByLabelText` ambiguous — qualify the computed one.
  ⚠ `FieldStateInput`'s `invalid` renders the CSS class `is-invalid`, **not**
  `aria-invalid`.
- ⚠ **Slate primitives must stay OUT of FormEditor's static imports** (the s133
  lazy-chunk rule) — inline the `.slate-secheader` markup instead of importing
  `SectionHeader`.
- ⚠ **The screenshot driver's rail label is a REGEX** — `"^EIC$"` for a
  three-letter label that prefixes nothing else. ⚠ A QA `.mjs` must live **under
  the repo root** (`puppeteer-core` is an ephemeral root install). ⚠ Run vitest
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
1. **(s139) Deleting the last QBI source leaves a stale QBI deduction on 1040
   line 13** — $859 understated on the proof return, no diagnostic, two
   recomputes don't clear it. Recommendation: blank line 13 in
   `compute_8995_db`'s not-engaged branch (the override guard already protects a
   real direct entry) and add a "deduction with no QBI source" diagnostic.
   Chip `task_8000a11e`.
2. **(s139) Should `eic_self_employed` be DERIVED rather than asked?** The
   unanswered default silently costs $4,328–$7,152 and no diagnostic covers it.
   Recommendation: default it True when the return carries a Schedule C /
   Schedule F / SE K-1 (the `_derive_person` precedent, with an `_overridden`
   companion), plus a residual warning.
3. **(s139, minor) Three seeded `1040_EIC` rows are never written** — write them
   before any EIC worksheet statement page is built.
4. **(s138) Form 8863 lets ONE student take BOTH education credits** — the same
   $4,000 earns an extra $800, live-proven. §25A(c)(2)(A) forbids it and
   D_8863_DUAL_STUDENT is only a warning. Recommendation: make it an error, and
   have compute drop the LLC expenses for any student whose AOTC is allowed.
5. **(s138) The Form 8863 line-7 lockout is global but keyed per student** —
   ticking it on one student makes the WHOLE return's AOTC nonrefundable
   ($2,000 in the proof).
6. **(s138, minor) `compute_8863_db.disengage()` cannot clear a "0" it wrote
   itself** — same shape likely in the other disengage paths guarded on
   `!= ZERO`; item 1 above is that family with real money, so sweep them together.
7. **(s138) Form 8962 has NO 100%-of-FPL eligibility floor** — engine-proven, a
   full premium tax credit at 66% of FPL, where §36B(c)(1)(A) is 100–400%.
   Recommendation: a `warning` that distinguishes the §1.36B-2(b)(6) safe-harbour
   case from the no-APTC case, not an error.
8. **(s138) Form 2441 deems BOTH spouses for the same months** — engine-proven
   $1,920 vs $0 against an explicit IRS instruction the RS spec never carries.
9. **(s138) A tax-exempt care provider cannot be e-filed** (the extract raises)
   and printed a blank TIN column. The screen now instructs "Tax-Exempt".
10. **(s138, minor) Only three Form 2441 Part I / Part II rows print** — is the
   overflow statement page wanted before the season?
11. **(s137) FIVE tax-accuracy holes the screens now WARN about but that no
   DIAGNOSTIC covers** — (a) a blank prior-year Sch A **5e** untaxes a whole
   state refund; (b) an **age/blind box count above 4**; (c) a tax year with no
   `PY_STD_DEDUCTION` entry silently uses **2024** constants (2027+); (d) the
   **§165(d) cap** not following the W-2G documents; (e) an explicit **$0
   `family_allocation`** zeroing the HSA deduction. Recommendation: seed (a),
   (d), (e) as errors and (b), (c) as warnings.
12. **(s137) Should `scha_gambling_winnings` auto-populate** from the W-2G box-1
   sum + `other_gambling_winnings`?
13. **(s137) `compute_8889_db` stores the FORM_8889 face from accounts[0] only.**
14. **(s137, minor)** `worksheet_2` computes `w8` unclamped while
   `_worksheet_rows` displays `max(0, …)` — confirm which the statement shows.
15. **(s130, re-confirmed s136/s137/s138/s139) PATCH `/taxpayer/` and the
   per-form PATCH lanes run tens of seconds in-process.** Profile in a session
   that owns views.py.
16. **(s131) Form 7203 panel legacy-styled** inside the Slate K-1 screen.
17. **(s129) The RS authoring session** — scheduled; agenda in REVIEW_QUEUE.
18. **(s129) Launcher menu extras** — no data source; rulings wanted.
19. s124's `D_4562_RECON` scoping pair.
20. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
21. *(cosmetic)* Legacy floating "Calculating…" chip needs a Slate home.
22. *(s126)* Sch B 3a/3b detail tables entry gap; 5 unbuilt Sch B runners.
23. **(s136)** Replace-all nested lists (8915-F item C) remain vulnerable to
    out-of-order server arrival of two overlapping PATCHes.
24. **(s137, bookkeeping)** Neither **STATE_REFUND** nor the sweep's other
    worksheet pseudo-forms have rows in `form_coverage_tracker.md`, though their
    spec/compute/render/diagnostic/test legs all exist. Worth a short
    reconciliation pass.

## Active gates
- **Branch discipline:** `slate-ui` checked out (pushed through `4c1e51c`);
  parallel session's uncommitted work UNSTAGED. Never stage/stash/`git add .`.
- ⚠ **Demo DB drift:** diagnostics migration 0005 applied to the DEMO DB only —
  prod applies at Ken's deploy (additive, safe).
- ⚠ Demo QA return (`bc270846…`) carries synthetic review data ON PURPOSE —
  s127 1099-R (TRS $24,000) + s128 1099-INT ($1,250/$300/$50 W/H) + 1099-DIV
  ($800/$600/$150) + SS box 5 $21,600.
  **At rest after the s139 reverts (ORM-verified, ZERO drift): 0 dependents,
  0 Schedule C, 0 1040_EIC rows, all 17 `eic_*`/election facts at defaults,
  8867 / 8862 / 8995 rows all blank, 0 care providers, 0 FORM_2441 rows,
  0 Forms 1095-A, 0 FORM_8962 rows, 0 education students, 0 W-2G, 0 HSA
  accounts, all 17 `sr_*` facts at defaults, 0 Forms 8915-F / 8606 / Roth
  trackers / SS lump-sums / 1099-G. AGI 94,560, L12 15,750, L13 blank,
  L15 78,810, L16/L18 12,204, L19 0, L20 0, L24 12,204, L27 0, L33 2,450,
  L37 10,151, Sch 3 line 2 blank, 1040 1e blank.**
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
