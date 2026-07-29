# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 138 (bespoke-screen sweep unit 20:
FORM 2441 / Child and Dependent Care, §21. Live-proven end-to-end and fully
reverted. SEVEN real defects, one of which silently zeroed the entire credit
and another of which overstates it against the IRS instructions.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — the bespoke-screen sweep continues at **Form 8962 (PTC)**

**s138 shipped unit 20 on `slate-ui` (no deploy): `c7fa893`**

Remaining 1040 screens (~15 of ~39; the count was re-measured in s137 by
mapping every `activeTab` to its section component **file** and checking each
for a `NEW_UI` gate — do it that way, never by scanning FormEditor alone):
**8962** ≈323 lines · **Sch 1-A** ≈342 · **EIC** ≈280 · **5329** ≈278 ·
**6251** ≈264 · **8615** ≈259 · **1116** ≈273 · **8863** ≈170 · **8880** ≈156 ·
**8960** ≈119 · **5695** · **1040-X** · the **state/GA** tab · the
**prior-year / tax-summary** views · the estimates/extension/e-file cards.
Most are small worksheet singletons now that the paradigms are settled.
**The business-entity screens (1120-S / 1065 shareholders, partners, balance
sheets, allocations, 7203, page-1 income/deductions) are a SEPARATE unscoped
lane — ~12 more, none started. Ken's call when to take them.**

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

**Gates at s138 close:** vitest **947/947** (+27) · tsc **46 = baseline** ·
side-by-sides committed
(`Design/slate-phase2-screenshots/{legacy,slate}-f2441.png`) · all 7 QA PATCHes
**200** · demo QA writes reverted with **ZERO drift** against the pre-seed
baseline.

**Next action: continue the sweep at Form 8962 (PTC)**, then education (8863),
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
1. **Read the engine against the screen before writing any JSX.** All seven of
   this unit's defects came from that, not from the UI work.
2. **Probe the engine's PURE functions first** — no DB, no browser. One
   `compute_2441_lines` probe proved the claim-flag gap, the both-deemed
   overstatement, the DCB chain and the tax-liability clamp in a single run.
3. **Check the MODEL TYPE and nullability of EVERY field in a blank map** —
   nullable-sent-"0" and non-nullable-sent-"" are both silent-revert factories.
4. **Follow the field past compute into RENDER and E-FILE.** Defects 3, 4 and 5
   are invisible from compute alone: a column with no input, a checkbox whose
   path raises, and a silent `[:3]` truncation.
5. **When the RS spec is SILENT on a rule the IRS states, flag it — never fill
   it.** Verify the rule against the instructions first so the flag is right.
6. **Distinguish a missing-data zero from a legitimate zero** and explain both.

## Dev QA recipe (proven again this session)
preview_start django-demo + vite · demo QA return
`bc270846-5800-4cbc-8f7f-573d0a5a953f` · `scripts/mint_magic_link.py`
(SINGLE-USE — **mint per run**; defaults to the DEMO DB) ·
`scripts/slate_screen_screenshots.mjs <returnId> <tokenFile> "<rail label>"
<slug> [outDir]` · `scripts/qa_unit20.mjs` (entry + revert in one) · the unit-20
ORM seed/revert pair lived in the scratchpad (a Dependent + CareProvider are
needed before the screen shows anything).
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
1. **(s138) Form 2441 deems BOTH spouses for the same months** — engine-proven
   $1,920 vs $0 against an explicit IRS instruction the RS spec never carries.
   Needs a spec amendment + a per-owner months allocation + a diagnostic.
2. **(s138) A tax-exempt care provider cannot be e-filed** (the extract raises)
   and printed a blank TIN column. The screen now instructs "Tax-Exempt";
   the IRS2441 mapping still needs a ruling.
3. **(s138, minor) Only three Part I / Part II rows print** — is the overflow
   statement page wanted before the season?
4. **(s137) FIVE tax-accuracy holes the screens now WARN about but that no
   DIAGNOSTIC covers** — each a silent wrong number on a filed return:
   (a) a blank prior-year Sch A **5e** untaxes a whole state refund;
   (b) an **age/blind box count above 4**; (c) a tax year with no
   `PY_STD_DEDUCTION` entry silently uses **2024** constants (2027+); (d) the
   **§165(d) cap** not following the W-2G documents; (e) an explicit **$0
   `family_allocation`** zeroing the HSA deduction. Recommendation: seed (a),
   (d), (e) as errors and (b), (c) as warnings.
5. **(s137) Should `scha_gambling_winnings` auto-populate** from the W-2G box-1
   sum + `other_gambling_winnings`?
6. **(s137) `compute_8889_db` stores the FORM_8889 face from accounts[0] only.**
   The PDF is correct per owner; confirm the stored rows are feeders-only.
7. **(s137, minor)** `worksheet_2` computes `w8` unclamped while
   `_worksheet_rows` displays `max(0, …)` — confirm which the statement shows.
8. **(s130, re-confirmed s136/s137/s138) PATCH `/taxpayer/` and the per-form
   PATCH lanes run tens of seconds in-process.** Profile in a session that owns
   views.py.
9. **(s131) Form 7203 panel legacy-styled** inside the Slate K-1 screen.
10. **(s129) The RS authoring session** — scheduled; agenda in REVIEW_QUEUE.
11. **(s129) Launcher menu extras** — no data source; rulings wanted.
12. s124's `D_4562_RECON` scoping pair.
13. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
14. *(cosmetic)* Legacy floating "Calculating…" chip needs a Slate home.
15. *(s126)* Sch B 3a/3b detail tables entry gap; 5 unbuilt Sch B runners.
16. **(s136)** Replace-all nested lists (8915-F item C) remain vulnerable to
    out-of-order server arrival of two overlapping PATCHes.
17. **(s137, bookkeeping)** Neither **STATE_REFUND** nor the sweep's other
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
  **At rest after the s138 revert (ORM-verified, ZERO drift): 0 dependents,
  0 care providers, 0 FORM_2441 rows, all six `f2441_*` facts at defaults,
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
