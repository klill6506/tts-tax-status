# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 136 (bespoke-screen sweep: FORM 8915-F as
unit 14 and the SS LUMP-SUM ELECTION as unit 15 — both live-proven end-to-end
and fully reverted; five real defects fixed between them. Also closes out
s135's unit 13 (Form 8606), which shipped `9e88fdd` but never got a session
close.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — the bespoke-screen sweep continues at **State Refund**

**s136 shipped units 14, 15 and 16 on `slate-ui` (no deploy): `3d57e5d` ·
`53dfd5b` · `e030b71`**
*(s135 shipped unit 13 — Form 8606 + the Roth basis tracker — as `9e88fdd`.)*

### Unit 16 — Form 1099-G, unemployment §85, `e030b71`
- **PayerTable slim grid** (many state agencies, one number each — the INT/DIV
  shape). The legacy draft card `__draft_1099g__` is **retired under the flag**
  by the s130 type-to-add ruling: the typed payer name IS the create payload.
- ⚠⚠ **TRAP AVOIDED — check `OWNER_CHOICES` before wiring `showOwner`.**
  `Form1099G` allows **taxpayer/spouse only**, while PayerTable's shared owner
  column offers a third **"Joint"** — correct for InterestIncome /
  DividendIncome / CapitalTransaction (`IncomeOwner` / `CapitalOwner` carry it)
  and a **400-and-silent-revert** here. This screen renders its own owner cell;
  Spouse still appears only on a joint return.
- **No ƒx cells** · blank map: five money columns → `"0"`, `payer_name` is
  `blank=True` → `""` and the blank guard is OFF (proven live).
- The row preview nets exactly as `compute_1099g.net_unemployment` (box 1 less
  the SAME-YEAR repayment, **floored at zero**), and three engine conditions
  the legacy screen never surfaced at the row now show where they are caused:
  **D_1099G_WH_ONLY** as an invalid overlay on the box-4 cell,
  **D_1099G_1341** and **D_1099G_OTHERBOXES** as alerts in the expansion. The
  legacy header text was checked against the engine and **is accurate** — it
  carries over unchanged (the unit-15 audit, run again and passing).
- **Live QA (reverted, ORM-verified):** "GA Dept of Labor", box 1 $6,000 with
  $600 withheld → Sch 1 line 7 6,000 → 1040 L8 6,000, AGI **94,560 → 100,560**,
  L15 84,810, L16 12,204 → 13,524, L25b **2,450 → 3,050**. A $1,500 same-year
  repayment nets the row to 4,500; box 1 cleared to 0 raises the box-4 overlay
  + alert and both clear when it comes back. Deleted through its own lane (204)
  → back at rest.

### Unit 15 — the SS Lump-Sum Election (Pub 915 Worksheets 2 + 4), `53dfd5b`

### Unit 15 — the SS Lump-Sum Election (Pub 915 Worksheets 2 + 4), `53dfd5b`
- **PayerTable, not document tabs.** An earlier-year row is not a separately
  filed form — it is one preparer-asserted fact set per year, keyed by row id
  and a handful of numbers wide, so it takes the slim grid + expansion. The
  legacy "Add Earlier Year" button + separate edit panel become **type-to-add**
  (the typed year IS the create payload).
- **No ƒx cells** — `compute_ss_lumpsum` re-derives every worksheet line at
  render and no serializer column is computed. The WS1-vs-WS4 comparison
  reaches the preparer through **D_RET_008**, not this screen.
- **Blank-commit map:** `earlier_year` is `null=True` → blank commits **null**
  (an unnumbered row is a real state), so PayerTable's payer-name blank guard
  is OFF; all six money columns are non-nullable `default=0` → blank commits
  **"0"**. Both proven live (`{"earlier_year_agi":"0"}`, `{"earlier_year":null}`).
- **THREE stale-or-wrong guidance defects fixed**, all found by reading the
  engine against the screen:
  1. the RED note fired on *(election && no rows)*; **D_RET_004 gates on the
     SSA-1099 indicator** (`Taxpayer.ssa_lump_sum_prior_year`, now passed down
     from FormEditor) **and on "no row carries a positive amount"** — a
     preparer with the election on, a row added and $0 in it saw the warning
     vanish while the diagnostic still fired RED;
  2. the note claimed Worksheet 4 supersedes 6b *"only if it is lower"* — it
     supersedes **whenever the toggle is on** (`taxable_6b = w4["l21"] if
     applied else ws1`); D_RET_008 warns rather than reverting;
  3. **the Social Security screen (Slate AND legacy) still said the election
     "is not yet supported — prepare manually"** — it has been computed since
     the retiree-hardening cluster. Both now point at this screen and keep the
     one caveat that is still true: **pre-1994 years take Worksheet 3, which is
     not built** — and since `earlier_year` is a LABEL the engine never reads,
     a 1993 row would run silently through Worksheet 2, so it carries the
     invalid overlay + an alert.
- **Live QA (reverted, ORM-verified)** — the classic disability back-payment:
  indicator on, 2023 carrying **$9,000** of the $21,600 box 5, that year's box
  5 $12,000 against AGI $8,000 → the earlier year stays under the §86 base so
  Worksheet 2 adds nothing, and this year re-runs on $12,600 → **1040 L6b
  18,360 → 10,710**, AGI **94,560 → 86,910**, L15 71,160, L16 12,204 → 10,521
  (`compute_lump_sum_db`: savings 7,650, beneficial, applied). Row deleted
  through its own lane (204), both flags cleared → back to at rest.
- ⚠⚠ **Driver lesson: a checkbox click TOGGLES.** An unconditional click flips
  the flag back OFF on a second run — which is exactly what "the indicator
  never saved" looked like for two runs before the cause was found. Both
  toggles in the unit-15 drivers are idempotent and assert their state.
- ⚠ The taxpayer-facts lane is `/tax-returns/<id>/taxpayer/`, **not** `/info/`.
- ⚠ The Social Security screen's checkboxes carry **no aria-label** (the
  wrapping `<label>` is their name) — drive them by that label's text.

### Unit 14 — Form 8915-F, `3d57e5d`

- **Form 8915-F takes the unit-12 document ruling.** "One form per disaster
  year (item B) per spouse — never combine spouses", so a row is a separately
  filed form → **one tab per owner/year over the archetype two-column
  worksheet**.
- **Item C is the exception inside the exception.** The FEMA disasters are a
  nested list with **replace-all** write semantics whose rows may not have an
  id yet, so item C renders as its own dense `.slate-asstable` — **not** a
  PayerTable, which is keyed by row id.
- **View over container:** the self-managing card keeps every lane (the
  monotonic seq guard, the replace-all disaster writes, create/patch/delete,
  the PDF); the Slate screen is a prop-fed rendering.

**TWO REAL DEFECTS FOUND AND FIXED — both surfaced by live QA:**
1. **Blank-commit.** Legacy sent `null` for EVERY blanked money field, but
   only the four derived-override columns (`alloc_other` /
   `alloc_traditional` / `alloc_roth` / `not_on_8606`) are `null=True`. On the
   other sixteen the serializer answers "This field may not be null" — a 400,
   so clearing a value **silently reverted**. Non-nullable now commits `"0"`;
   the four overrides commit `null` (blank genuinely means "derive it").
   Proven live: the clear PATCH body is `{"cost_p2":"0"}` → 200.
2. **Item-C clobber (silent data loss).** Every item-C write ships the
   COMPLETE list, and that list was built from the **render closure**. A PATCH
   here costs seconds (the whole return re-derives), so keying the FEMA number
   and then tabbing straight into the declaration date sent the pre-FEMA list
   and **wiped the number that had just been saved**. The list now composes
   through a ref that carries local edits forward until the server's answer
   paints; **props stay authoritative** (a refreshed list always overwrites
   the ref). Reproduced live before the fix (`analysis` read *"an unnamed
   disaster: not a valid FEMA declaration number"*) and clean after.
   ⚠ **Legacy `Form8915FRow` still has this closure shape** — it is only
   reachable with `NEW_UI` off, so it is not on the Slate path; do not
   re-introduce the pattern in a future unit.

- **Every `analysis` figure is a locked ƒx cell** (`noOverride`) — all of them
  come from the SAME `compute_8915f` the print leg uses, and the legacy yellow
  prose was the only thing marking them computed. `problems` /
  `efile_blockers` / the opt-out mismatch are `role="alert"` errors and raise
  the tab's **error dot**.

**Live QA (demo QA return, fully reverted, every figure ORM-verified):**
taxpayer 2025, DR-4832-GA declared 10/01 begun 09/24, **$22,000** of the
$24,000 TRS pension designated a qualified disaster distribution → lines
1e/6 = 22,000, line 7 = 0, spread ÷3 = 7,333 **+ the 2,000 non-QDD remainder →
1040 L5b 9,333** (was 24,000) → AGI **94,560 → 79,893**, L15 64,143, L16
8,970. Deleted through the screen's own Remove lane (204) → back to **AGI
94,560 / L15 78,810 / L16 12,204**.

**Gates at s136 close:** vitest **885/885** (+28 new across the three units) ·
tsc **46 = baseline** · side-by-sides committed
(`Design/slate-phase2-screenshots/{legacy,slate}-{8915f,sslump,1099g}.png`) ·
console ≥400 noise identical in BOTH modes (same 403/404 baseline).

**Next action: continue the sweep at State Refund**, then
Misc Income, HSA 8889, EIC, 2441, 8962, education, 5695,
estimates/extension/e-file cards. Pattern settled: view-over-container;
**PayerTable** for flat record lists **keyed by row id**, **DocumentTabs +
worksheet** for card stacks AND for per-filed-form rows (the unit-12 ruling),
**InputRow worksheets** for facts cards (screenbar header for singletons),
**the asset register** for computed sub-schedule grids, and a **bare
`.slate-asstable`** for a nested replace-all list whose rows have no id yet
(the unit-14 addition); paradigms may NEST (7217, 8915-F); multi-section tabs
share ONE `.slate-screen` at the call site; screenshots per screen; live QA
writes reverted.

**Dev QA recipe (proven again this session):** preview_start django-demo +
vite · demo QA return `bc270846-5800-4cbc-8f7f-573d0a5a953f` ·
`scripts/mint_magic_link.py` (SINGLE-USE — **mint per run**) ·
`scripts/slate_screen_screenshots.mjs <returnId> <tokenFile> "<rail label>"
<slug> [outDir]` · new `scripts/qa_unit14_{entry,revert}.mjs` and
`scripts/qa_unit15_{entry,revert}.mjs` and
`scripts/qa_unit16_{entry,revert}.mjs <returnId> <tokenFile>`.
⚠ A QA `.mjs` must live **under the repo root** — `puppeteer-core` is an
ephemeral root install, so a script in a scratch folder fails to resolve it.
⚠⚠ **`Ctrl+A` only SELECTS** — clearing a cell needs an explicit `Backspace`,
or the value never changes, no commit fires, and a driver awaiting that write
**hangs forever** (no timeout, no output).
⚠⚠ **Settling on "no write ISSUED for N ms" is a NO-OP here** — these PATCHes
take tens of seconds, so the last issue timestamp is already ancient when the
response lands. Count **in-flight** writes (`request` ++ /
`requestfinished|requestfailed` --) and then allow a commit tick.
⚠ **A React `onChange` date lane needs `input` ONLY** — dispatching `change`
as well fires the handler twice, and the first response is then dropped by the
card's monotonic seq guard.
⚠ A delete lane behind `window.confirm` is a silent no-op under puppeteer —
`page.on("dialog", d => d.accept())`; judge the delete by the **tab count
settling**. ⚠ A rail label passed to the screenshot driver is a **REGEX**.
⚠ PowerShell mangles a multi-line `manage.py shell -c` — run ORM probes from
the Bash tool with `shell -c "$(cat file)"`. ⚠ NEW_UI reads at module load —
reload after setting localStorage. ⚠ There is NO `.slate-summarybar` — judge
AGI by ORM. ⚠ FFV ORM path = `form_line__section__form__code`; the 1040's form
code is `1040`.

**Build rules in force:** presentation-only (server untouched this session) ·
selective `git add` only — NEVER `git add .` (parallel tb_import work still
unstaged: `server/apps/returns/views.py`, `tb_import.py`,
`test_tb_import.py`; ⚠ also never `git stash` here) · no merge/deploy
without Ken · at deploy: migrate (diagnostics 0005) + seed_rules BOTH DBs
(D_W2_ family + MATH_BALANCE_SHEET description).

**KEN CLARIFIED (2026-07-28): the tax app is TESTING until January 2027.**
He switches to Slate when the redesign is FINISHED; everything rides
`slate-ui`; the shared Supabase DB caution is the one true-production
constraint (sherpa-1099 prod + ~700 real clients).

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE — trimmed to live items)
1. **(s130, re-confirmed s136) PATCH `/info/` and the per-form PATCH lanes run
   tens of seconds in-process** on the QA return — every 8915-F write did.
   Profile in a session that owns views.py.
2. **(s131) Form 7203 panel legacy-styled inside the Slate K-1 screen** —
   cosmetic; restyle-as-hook unit after the sweep.
3. **(s129) The RS authoring session** — scheduled; agenda in REVIEW_QUEUE.
4. **(s129) Launcher menu extras** — no data source; rulings wanted.
5. s124's `D_4562_RECON` scoping pair.
6. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
7. *(cosmetic)* Legacy floating "Calculating…" chip needs a Slate home.
8. *(s126)* Sch B 3a/3b detail tables entry gap; 5 unbuilt Sch B diagnostic
   runners.
9. **(s136, NEW)** Replace-all nested lists (item C today; any future one)
   remain vulnerable to **out-of-order server arrival** of two overlapping
   PATCHes — the ref fix removes the stale-closure clobber but cannot order
   the requests. A per-lane single-flight queue would close it; scope it if a
   second replace-all list appears.

## Active gates
- **Branch discipline:** `slate-ui` checked out (pushed through `3d57e5d`);
  parallel session's uncommitted work UNSTAGED (`server/apps/returns/views.py`,
  `tb_import.py`, `test_tb_import.py`). Never stage/stash/`git add .`.
- ⚠ **Demo DB drift from prod schema:** diagnostics migration 0005 applied
  to the DEMO DB only — prod applies at Ken's deploy (additive, safe).
- ⚠ Demo QA return (Slate QA Household `bc270846…`): carries the synthetic
  review data ON PURPOSE — s127 1099-R (TRS $24,000) + s128 1099-INT
  ($1,250/$300/$50 W/H) + 1099-DIV ($800/$600/$150) + SS box 5 $21,600.
  **At rest after the s136 revert (ORM-verified): 0 Forms 8915-F, 0 Forms
  8606, 0 Roth trackers, 0 SS lump-sum rows, both SS lump-sum flags false,
  0 Forms 1099-G, AGI 94,560, L4b 0, L5b 24,000, L6a 21,600, L6b 18,360,
  L8 0, L15 78,810, L16 12,204, L25b 2,450.** Fed balance due ≈$8,512 at rest
  — expected.
- ⚠ **An ORM delete on the QA return must be followed by `compute_return(tr)`**
  before any figure is recorded as a baseline (the s133 correction).
- ⚠ D_8995/D_8959 NoneType errors fire on this QA return's diagnostics —
  known RS-session agenda item (REVIEW_QUEUE), not a sweep defect.
- ⚠ Demo employers registry: synthetic TRS of Georgia 58-1234567 + GA
  account 1234567-AB (harmless, kept).
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
