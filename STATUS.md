# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 134 (bespoke-screen sweep: FORMS 6252 /
8824 / 7217 converged as sweep unit 12 — three screens on one paradigm
ruling; live-proven end-to-end and fully reverted; no open bug reports at
boot).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — the bespoke-screen sweep continues at **8606**, then the remainder

**s134 shipped sweep unit 12 on `slate-ui` (no deploy): `8ff8de4`**

- **Forms 6252 · 8824 · 7217 — three screens, ONE paradigm ruling.** A row
  that is itself a separately FILED form (i6252 "one Form 6252 per sale",
  i8824 "one per exchange", i7217 "a separate Form 7217 for each date") is a
  **DOCUMENT**, so it takes **document tabs over the archetype two-column
  worksheet** — never PayerTable, which is shaped for one-number-wide payer
  rows. The legacy summary tables fold into the tab labels + tools-bar totals.
- **Form 7217 NESTS the two paradigms for the first time**: the tabs carry the
  form (Part I facts as the worksheet), and its Part II distributed-property
  grid IS a flat record list, so it renders as a **nested PayerTable with
  type-to-add** (the typed column (a) description is the create payload —
  `addProperty` took an optional `description`).
- **View over container:** every lane (create, per-field PATCH, delete, and
  the two-level 7217 property CRUD) stays in the legacy section; the Slate
  screen is a prop-fed rendering. `useActiveRecord` (new, shared by the three)
  keeps the settled rows[0]-when-stale rule and adds the legacy "Add → the
  edit form opens" contract: a record id that wasn't there last render becomes
  the active tab (rows append, so rows[0] would strand a new one).
- **Blank-commit map verified field by field against `models.py`:**
  non-nullable money → `"0"` · **6252 `prior_gross_profit_pct` /
  `section_6621_rate` → `null`** (fractions, not currency — a 0% ratio is not
  "unanswered") · **8824 `days_to_identify` / `days_to_receive` → `null`**
  (null = not a deferred exchange; 0 days is a different, real answer) ·
  **7217 `outside_basis` → `null`** (unanswered = D_7217_004 RED; an entered 0
  is valid, fully depleted basis) · `holding_period_months` → `0` ·
  description / `rp_exception` / `category` → `""`.
- **No ƒx cells on any of the three** — none of the serializers exposes a
  computed column (compute_6252 / _8824 / _7217 re-derive every line at
  render), so every cell is preparer-keyed. The 7217 tri-state assertions keep
  **unanswered as a real third state** with the invalid overlay; a red-defer
  flag (6252 ×2, 8824 ×3) or an unanswered required assertion raises the tab's
  **error dot**.
- **Two primitive fixes carried by the unit:** a new `.slate-select.is-invalid`
  overlay, and **`.slate-inputrow-value > .slate-select` now pins to the 160px
  value column** — a native select sizes to its longest option and a flex item
  won't shrink below content, so long option lists were bleeding over the next
  worksheet column on EVERY screen (verified fixed on Taxpayer Info too).

**Live QA (demo QA return, fully reverted, every figure ORM-verified):**
6252 QA Duplex 300,000 / basis 180,000 / payments 60,000 → gross profit
120,000 ÷ contract price 300,000 = **ratio 0.40 → installment income 24,000**
→ Sch D → 1040 L7 150 → 24,150, total income 94,560 → **118,560**. 8824 QA
Warehouse cash 25,000 / FMV lk 500,000 / basis lk 320,000 → realized 205,000,
**recognized = boot 25,000** → 4797 L5 §1231 → Sch D → L7 **49,150**, total
income → **143,560**. 7217 QA Elm Street Partners + one property row, cash
5,000 < outside basis 40,000 → **no §731(a)(1) gain, no income change**
(correct). Each deleted through the screen's own Delete lane (204) → back to
**AGI 94,560 / L15 78,810 / L16 12,204**.

**Gates at s134 close:** vitest **844/844** (+31 new) · tsc **46 = baseline** ·
3 side-by-sides committed
(`Design/slate-phase2-screenshots/{legacy,slate}-{6252,8824,7217}.png`) ·
`/bugs` sweep at boot: no open reports · console ≥400 noise identical in BOTH
modes (same 403/404 baseline).

**Next action: continue the sweep at 8606**, then 8915-F, SS lump-sum,
1099-G, State Refund, Misc Income, HSA 8889, EIC, 2441, 8962, education,
5695, estimates/extension/e-file cards. Pattern settled: view-over-container;
**PayerTable** for flat record lists, **DocumentTabs + worksheet** for card
stacks AND for per-filed-form rows (the unit-12 ruling), **InputRow
worksheets** for facts cards (screenbar header for singletons), **the asset
register** for computed sub-schedule grids; the two may NEST (7217);
multi-section tabs share ONE `.slate-screen` at the call site; screenshots per
screen; live QA writes reverted.

**Dev QA recipe (proven again this session):** preview_start django-demo +
vite · demo QA return `bc270846-5800-4cbc-8f7f-573d0a5a953f` ·
`scripts/mint_magic_link.py` (SINGLE-USE — **mint per run**) ·
`scripts/slate_screen_screenshots.mjs <returnId> <tokenFile> "<rail label>"
<slug> [outDir]` (now also accepts a **table-only** legacy section as ready —
the 6252/8824 legacy screens render NO inputs until "Edit") · new
`scripts/qa_unit12_{entry,revert}.mjs <returnId> <tokenFile> <6252|8824|7217>`.
⚠ **A rail label passed to the screenshot driver is a REGEX** — `"Schedule F
(Farm)"` silently never matches; pass `"Schedule F"`. ⚠ **The legacy 6252/8824
delete lanes go through `window.confirm`** — puppeteer AUTO-DISMISSES an
unhandled dialog, so a revert driver must `page.on("dialog", d => d.accept())`
or it is a silent no-op. ⚠ **Judge a delete by the tab count settling, not a
flat sleep** — the container refetches the whole return and a
recompute-heavy delete outran a 3s pause (looked like a stale-tab defect; it
was the read). ⚠ **PowerShell mangles a multi-line `manage.py shell -c`
string** — run the ORM probe from the Bash tool with `shell -c "$(cat file)"`.
⚠ Chrome date inputs: `<input type="date">` on a React `onChange` lane commits
from a native setter + a bubbling `input` event. ⚠ NEW_UI reads at module load
— reload after setting localStorage. ⚠ Headless replace-typing: Ctrl+A before
page.type. ⚠ There is NO `.slate-summarybar` — judge AGI by ORM. ⚠ FFV ORM
path = `form_line__section__form__code`; the 1040's form code is `1040`.

**Build rules in force:** presentation-only (server untouched this session) ·
selective `git add` only — NEVER `git add .` (parallel tb_import work
unstaged: `server/apps/returns/views.py`, `tb_import.py`,
`test_tb_import.py`; ⚠ also never `git stash` here) · no merge/deploy
without Ken · at deploy: migrate (diagnostics 0005) + seed_rules BOTH DBs
(D_W2_ family + MATH_BALANCE_SHEET description).

**KEN CLARIFIED (2026-07-28): the tax app is TESTING until January 2027.**
He switches to Slate when the redesign is FINISHED; everything rides
`slate-ui`; the shared Supabase DB caution is the one true-production
constraint (sherpa-1099 prod + ~700 real clients).

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE — trimmed to live items)
1. **(s130) PATCH `/info/` ≈64s in-process** on the QA return — profile in a
   session that owns views.py.
2. **(s131) Form 7203 panel legacy-styled inside the Slate K-1 screen** —
   cosmetic; restyle-as-hook unit after the sweep.
3. **(s129) The RS authoring session** — scheduled; agenda in REVIEW_QUEUE.
4. **(s129) Launcher menu extras** — no data source; rulings wanted.
5. s124's `D_4562_RECON` scoping pair.
6. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
7. *(cosmetic)* Legacy floating "Calculating…" chip needs a Slate home.
8. *(s126)* Sch B 3a/3b detail tables entry gap; 5 unbuilt Sch B diagnostic
   runners.

## Active gates
- **Branch discipline:** `slate-ui` checked out; parallel session's
  uncommitted work UNSTAGED (`server/apps/returns/views.py`, `tb_import.py`,
  `test_tb_import.py`). Never stage/stash/`git add .`.
- ⚠ **Demo DB drift from prod schema:** diagnostics migration 0005 applied
  to the DEMO DB only — prod applies at Ken's deploy (additive, safe).
- ⚠ Demo QA return (Slate QA Household `bc270846…`): carries the synthetic
  review data ON PURPOSE — s127 1099-R (TRS $24,000) + s128 1099-INT
  ($1,250/$300/$50 W/H) + 1099-DIV ($800/$600/$150) + SS box 5 $21,600.
  **At rest after the s134 revert (ORM-verified): 0 installment sales, 0
  like-kind exchanges, 0 partnership distributions, AGI 94,560, L7 150,
  L15 78,810, L16 12,204.** Fed balance due ≈$8,512 at rest — expected.
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
