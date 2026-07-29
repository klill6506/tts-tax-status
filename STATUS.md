# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 137 (bespoke-screen sweep: THREE units —
STATE REFUND (§111) as unit 17, MISCELLANEOUS INCOME / Form W-2G as unit 18, and
FORM 8889 / HSA as unit 19. All three live-proven end-to-end and fully reverted.
SEVEN real defects fixed, three of which silently changed tax by four figures.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — the bespoke-screen sweep continues at **Form 2441 (child care)**

**s137 shipped units 17, 18 and 19 on `slate-ui` (no deploy): `f984261` ·
`9c379e7` · `5f2ac27`**

### ⚠ THE REMAINING-SCREEN LIST IN PAST STATUS FILES WAS ABOUT HALF THE REAL COUNT
Measured this session by mapping every `activeTab` to its section component and
checking each for a `NEW_UI` gate (resolving components to their own FILES —
scanning FormEditor alone wrongly reports the extracted sections as unconverted).
**~23 of ~39 1040 screens are converted; ~16 remain.** Past lists named 8.
The ones they omitted, all genuinely bespoke: **Form 8880** (saver's credit),
**8615** (kiddie tax), **5329** (penalties), **6251** (AMT), **8960** (NIIT),
**1116** (foreign tax credit), **Sch 1-A** (OBBBA), **1040-X**, the **state/GA**
tab, and the **prior-year / tax-summary** views. Sizes are known:
2441 ≈ 347 lines · 8962 ≈ 323 · Sch 1-A ≈ 342 · EIC ≈ 280 · 5329 ≈ 278 ·
6251 ≈ 264 · 8615 ≈ 259 · 1116 ≈ 273 · 8863 ≈ 170 · 8880 ≈ 156 · 8960 ≈ 119.
Most are small worksheet singletons now that the paradigms are settled — the
large bespoke screens (Depreciation, Sch C/D/E, K-1, Sch A) are all done.
**The business-entity screens (1120-S / 1065 shareholders, partners, balance
sheets, allocations, 7203, page-1 income/deductions) are a SEPARATE unscoped
lane — ~12 more, none started. Ken's call when to take them.**

### Unit 19 — Form 8889 / HSA (§223), `5f2ac27`
- **Document tabs** (unit-12 ruling: one Form 8889 per owner). The face renders
  from the server's own FORM_8889 rows as locked ƒx cells, retiring legacy's
  client-side `HSA_LIMITS` copy AND its `estDeduction` re-implementation.
- ⚠⚠ **DEFECT — the HSA deduction collapsed to zero when an "optional" field was
  cleared.** `family_allocation` is the ONE nullable field on HSAAccount and the
  engine branches on `is not None`, so **blank means "take the whole prorated
  limit"**. Legacy's money helper committed `value || "0"` for every box, so
  clearing the field labelled "(line 6, optional)" wrote 0 — read as an explicit
  $0 share. Engine-proven: family coverage, $8,550 contributed → deduction
  **8,550 with None, 0.00 with 0** (1,000 if the catch-up applies). Live-proven
  through the server's own line 13: 8,550 → 0 on `{"family_allocation":"0"}`,
  back to 8,550 on `{"family_allocation":null}`. A second-order effect the live
  run exposed: the $0 allocation ALSO made D_8889_EXCESS fire ("contributions of
  8,550 exceed the deductible limit of 0") — a confusing cascade rather than a
  pointer at the cause.
- ⚠ **`compute_8889_db` writes the FORM_8889 face rows from the FIRST account
  only** (`first = f`). The per-owner faces exist solely in the PDF, which
  `render_8889` recomputes per owner (render leg verified correct). So the face
  shows for accounts[0]; a later owner's tab says its figures are on the printed
  form rather than repeating the first owner's numbers. The Schedule 1/2 feeders
  ARE aggregated, so those show on every tab. → REVIEW_QUEUE.
- Surfaced at the cause: D_8889_EXCESS (with the 6% excise caveat),
  D_8889_FUNDING (lines 10 **and** 19 are written ZERO — engine-verified, so the
  legacy note is accurate and carries over), D_8889_MEDICARE as a warning that
  "months eligible" is **not** reduced automatically, the last-month-rule
  testing period, D_8889_HDHP, TY2026 interim line numbers, and an
  eligible-months entry above 12 (the engine clamps it, so it was silent).

### Unit 18 — Miscellaneous Income / Form W-2G (§61), `9c379e7`
- **Document tabs** — a W-2G carries its own payer identity and eight boxes and
  e-files as its own IRSW2G document (the W-2 / 1099-R archetype, not
  PayerTable). Totals come from the server's FORM_W2G rows.
- ⚠⚠ **DEFECT — the gambling loss deduction silently vanished.** Schedule A line
  16 is `min(pct × scha_gambling_losses, scha_gambling_winnings)`, capped by a
  **separate** fact that nothing populates from the W-2G documents and that
  lived only on the Deductions tab. This screen said "Enter [losses] once here —
  it flows to Schedule A automatically"; it does flow, but with the cap at its 0
  default the deduction is **zero**. Engine-proven:
  `gambling_line16(0, 10000, 0, 2025) = 0` vs `(10000, 10000, 0, 2025) = 10000`.
  The screen's own red warning compared losses against the REAL winnings, so it
  actively reassured the preparer. The cap now sits beside the losses with the
  total winnings as a computed reference and an error whenever it is blank or
  short. Live-proven: cap set → Schedule A line 16 = 10,000.
- ⚠ **DEFECT — "box 15 state income tax withheld is informational" was false.**
  Box 15 is summed into `state_income_tax_withheld()` → the Schedule A line-5a
  deduction. Live-proven: a 500 box-15 gave Schedule A 5a = 1,400.
- ⚠ **DEFECT — the §165(d) note ignored the OBBBA haircut.**
  `GAMBLING_LOSS_PCT = {2025: 1.00, 2026: 0.90}`; the note is now year-aware.
- Also: D_W2G_WH_ONLY and the **FW2G-009-01** e-file rule (a nonzero box 14
  needs BOTH box-13 halves) flag only the half that is missing. Fixed an
  **addressability** defect the tests caught: the payer's State and box 13's
  State produced IDENTICAL aria labels, so neither was uniquely reachable by a
  diagnostics deep link or scripted entry.

### Unit 17 — the State Refund Worksheet (§111), `f984261`
- The **s132 singleton** (screenbar + two-column worksheet); the whole Pub 525
  Worksheet 2/2a chain renders from the server's own STATE_REFUND rows as locked
  ƒx cells, so the prior-year standard-deduction table stays server-only.
- ⚠⚠ **DEFECT — a blank prior-year Sch A line 5e silently zeroes the ENTIRE
  refund.** `worksheet_2a` takes the cap-limited branch whenever `5d > 5e`, so
  with 5e at 0 the recapture is all of 5d, `a3 <= a4` always holds, and it
  returns `(0, 0)`. **No diagnostic covers it.** Engine-proven: 5e blank turns
  **$1,200 of taxable income into $0**. Now an invalid overlay + alert, while the
  GENUINE cap-limited zero gets its own explanatory note.
- ⚠ **DEFECT — a Pub 525 exception read "$0 taxable".** With an exception flag on
  the engine *disengages* and blanks Sch 1 line 1/8z, yet the panel printed
  "Taxable refund: $0.00" — zero where the answer is "refigure by hand".
- ⚠ **DEFECT — blank-commit.** `sr_py_age_blind_boxes` was missing from
  `SR_DECIMAL_FIELDS`; being an **IntegerField**, `""` → 400 → silent revert.
  Serializer-proven and live-proven (`{"sr_py_age_blind_boxes":"0"}` → 200).

**Gates at s137 close:** vitest **920/920** (+35 across the three units) · tsc
**46 = baseline** · side-by-sides committed
(`Design/slate-phase2-screenshots/{legacy,slate}-{staterefund,miscincome,hsa8889}.png`)
· console ≥400 noise identical to baseline (same 403/404 set).

**Next action: continue the sweep at Form 2441 (child care)**, then 8962,
education (8863), EIC, Sch 1-A, 8880, 8615, 5329, 6251, 8960, 1116, 5695,
1040-X, the state tab, and the estimates/extension/e-file cards. Paradigms
settled: view-over-container; **PayerTable** for flat record lists keyed by row
id, **DocumentTabs + worksheet** for card stacks AND per-filed-form rows,
**InputRow worksheets** for facts cards (screenbar header for singletons — unit
17 is the cleanest example), **the asset register** for computed sub-schedule
grids, a bare **`.slate-asstable`** for a nested replace-all list whose rows have
no id yet; paradigms may NEST; multi-section tabs share ONE `.slate-screen` at
the call site; screenshots per screen; live QA writes reverted.

## 🔑 Method that is finding these defects (keep doing this)
1. **Read the engine against the screen before writing any JSX.** Every one of
   the seven defects came from this, not from the UI work.
2. **Probe the engine's PURE functions first** — no DB, no browser, one run
   proves every branch (`compute_state_refund`, `gambling_line16`,
   `hsa_deduction` each took one probe).
3. **Check the MODEL TYPE and nullability of every field in a blank map** —
   nullable-sent-"0" and non-nullable-sent-"" are both silent-revert factories,
   and they cut in *both* directions (unit 19 was the inverse of unit 14).
4. **Render the server's own rows** wherever the engine already writes them;
   never re-derive a table or a chain on the client.
5. **Distinguish a missing-data zero from a legitimate zero** and explain both,
   or the warning teaches preparers to ignore it.

## Dev QA recipe (proven again this session)
preview_start django-demo + vite · demo QA return
`bc270846-5800-4cbc-8f7f-573d0a5a953f` · `scripts/mint_magic_link.py`
(SINGLE-USE — **mint per run**; defaults to the DEMO DB) ·
`scripts/slate_screen_screenshots.mjs <returnId> <tokenFile> "<rail label>"
<slug> [outDir]` · `scripts/qa_unit17_{entry,revert}.mjs` ·
`qa_unit18_{entry,revert}.mjs` · `qa_unit19.mjs` (entry+revert in one — every
8889 fact lives on the child row, so the delete reverts exactly).
- ⚠⚠ **Judge a slow write by its REQUEST BODY + an in-flight settle, not by
  awaiting the response.** A per-record PATCH re-derives the whole return; unit
  18's entry driver stalled on a 120s response wait after ~12 writes even though
  every write landed. Count in-flight (`request` ++ /
  `requestfinished|requestfailed` --).
- ⚠⚠ **`Ctrl+A` only SELECTS** — clearing needs an explicit `Backspace`, or no
  commit fires and a driver awaiting that write hangs forever.
- ⚠⚠ **A checkbox click TOGGLES** — assert the target state.
- ⚠ **Demo-DB ORM probes need `TTS_ENV=demo`**; plain `manage.py shell` points at
  the shared prod DB, and `scripts/runserver_demo.py` *starts a server* rather
  than giving a shell (cost a 4-minute timeout).
- ⚠ **The Schedule A form code is `SCHEDULE_A`, not `SCH_A`** (SCH_1 / SCH_2 /
  SCH_B do use the short form) — a wrong code returns empty rows and looks
  exactly like a compute failure.
- ⚠ **The screenshot driver's legacy pass needs ≥2 inputs/selects on screen**, so
  an empty record screen (8889 with no HSA) times out — seed one row, screenshot,
  then remove it. ⚠ **An ORM delete MUST be followed by `compute_return(tr)`**
  before recording a baseline (s133).
- ⚠ **Verify the tsc count by diffing against a clean `git worktree` at HEAD**,
  not against a remembered number — this session that turned an apparent
  "+4 new errors" into a missing destructured prop in seconds.
- ⚠ A QA `.mjs` must live **under the repo root** (`puppeteer-core` is an
  ephemeral root install). ⚠ Run vitest **from `client/`** — from the repo root
  the setup file never loads and every test fails. ⚠ NEW_UI reads at module
  load — reload after setting localStorage. ⚠ There is NO `.slate-summarybar`.
  ⚠ FFV ORM path = `form_line__section__form__code`. ⚠ A rail label passed to
  the screenshot driver is a **REGEX**. ⚠ PowerShell mangles a multi-line
  `manage.py shell -c` — run ORM probes from Bash with `shell -c "$(cat file)"`.

**Build rules in force:** presentation-only (the server was untouched all three
units) · selective `git add` only — NEVER `git add .` (parallel tb_import work
still unstaged: `server/apps/returns/views.py`, `tb_import.py`,
`test_tb_import.py`; ⚠ also never `git stash` here) · no merge/deploy without
Ken · at deploy: migrate (diagnostics 0005) + seed_rules BOTH DBs (D_W2_ family
+ MATH_BALANCE_SHEET description).

**KEN CLARIFIED (2026-07-28): the tax app is TESTING until January 2027.** He
switches to Slate when the redesign is FINISHED; everything rides `slate-ui`;
the shared Supabase DB caution is the one true-production constraint.

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE — trimmed to live items)
1. **(s137) FIVE tax-accuracy holes the screens now WARN about but that no
   DIAGNOSTIC covers** — each is a silent wrong number on a filed return:
   (a) a blank prior-year Sch A **5e** untaxes a whole state refund;
   (b) an **age/blind box count above 4** (`py_standard_deduction` has no
   ceiling); (c) a tax year with no `PY_STD_DEDUCTION` entry silently uses
   **2024** constants (2027+); (d) the **§165(d) cap** not following the W-2G
   documents; (e) an explicit **$0 `family_allocation`** zeroing the HSA
   deduction. My recommendation: seed (a), (d), (e) as errors and (b), (c) as
   warnings. Full write-ups in REVIEW_QUEUE.
2. **(s137) Should `scha_gambling_winnings` auto-populate** from the W-2G box-1
   sum + `other_gambling_winnings`? §165(d) caps losses at *gambling winnings*,
   which is exactly Sch 1 line 8b — a separate hand-keyed cap may be deliberate
   (a preparer asserting winnings the app does not hold) or may be the bug.
   Needs a ruling; a compute change would want the RS Schedule A spec + flow
   assertions, so it is out of a presentation sweep's remit.
3. **(s137) `compute_8889_db` stores the FORM_8889 face from accounts[0] only.**
   The PDF is correct per owner. Confirm whether the stored rows are intended as
   feeders-only; if any statement page or UI reads them per owner it is wrong.
4. **(s137, minor)** `worksheet_2` computes `w8` unclamped while
   `_worksheet_rows` displays `max(0, …)` — same tax result, different printed
   WS2-8. Confirm which the statement page should show.
5. **(s130, re-confirmed s136/s137) PATCH `/taxpayer/` and the per-form PATCH
   lanes run tens of seconds in-process.** Profile in a session that owns
   views.py.
6. **(s131) Form 7203 panel legacy-styled** inside the Slate K-1 screen —
   cosmetic; restyle-as-hook unit after the sweep.
7. **(s129) The RS authoring session** — scheduled; agenda in REVIEW_QUEUE.
8. **(s129) Launcher menu extras** — no data source; rulings wanted.
9. s124's `D_4562_RECON` scoping pair.
10. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
11. *(cosmetic)* Legacy floating "Calculating…" chip needs a Slate home.
12. *(s126)* Sch B 3a/3b detail tables entry gap; 5 unbuilt Sch B runners.
13. **(s136)** Replace-all nested lists (8915-F item C) remain vulnerable to
    out-of-order server arrival of two overlapping PATCHes.
14. **(s137, bookkeeping)** Neither **STATE_REFUND** nor the sweep's other
    worksheet pseudo-forms have rows in `form_coverage_tracker.md`, though their
    spec/compute/render/diagnostic/test legs all exist. Not invented this
    session — worth a short reconciliation pass.

## Active gates
- **Branch discipline:** `slate-ui` checked out (pushed through `5f2ac27`);
  parallel session's uncommitted work UNSTAGED. Never stage/stash/`git add .`.
- ⚠ **Demo DB drift:** diagnostics migration 0005 applied to the DEMO DB only —
  prod applies at Ken's deploy (additive, safe).
- ⚠ Demo QA return (`bc270846…`) carries synthetic review data ON PURPOSE —
  s127 1099-R (TRS $24,000) + s128 1099-INT ($1,250/$300/$50 W/H) + 1099-DIV
  ($800/$600/$150) + SS box 5 $21,600.
  **At rest after the s137 reverts (ORM-verified): 0 W-2G, 0 HSA accounts, all
  17 `sr_*` facts at defaults, `scha_gambling_losses`/`_winnings` 0,
  0 Forms 8915-F / 8606 / Roth trackers / SS lump-sums / 1099-G, AGI 94,560,
  L4b 0, L5b 24,000, L6a 21,600, L6b 18,360, L8 0, L15 78,810, L16 12,204,
  L25b 2,450, L25c blank.** Fed balance due ≈$8,512 — expected.
  ⚠ STATE_REFUND / FORM_W2G / FORM_8889 rows now read `""` rather than `None`
  (backfilled then blanked by `disengage()`) — identical semantically, not drift.
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
