# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 133 (bespoke-screen sweep: DEPRECIATION
converged as sweep unit 11 — the largest screen in the app; live-proven
end-to-end and fully reverted; no open bug reports at boot).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — the bespoke-screen sweep continues at **6252 / 8824 / 7217**, then the remainder

**s133 shipped sweep unit 11 on `slate-ui` (no deploy): `51e489b`**

- **Depreciation `51e489b` — SlateDepreciationScreen**, the largest bespoke
  screen (~1,900 lines incl. the edit form and the Part I card). It converges
  on a **THIRD paradigm: the ASSET REGISTER**. A conversion return carries 40+
  assets whose columns are almost entirely server-computed and whose rows are
  a sub-schedule **per destination activity**, sub-grouped by asset class with
  its own subtotal and a "→ where it lands" band — neither PayerTable (flat
  one-line payers) nor DocumentTabs (a stack of per-document cards) can
  express that, so the register is its own renderer: screenbar action cluster
  · §168(k)(7) election span · paste/import panels · filter + bulk toolbar ·
  per-activity `.slate-asstable` blocks · the archetype two-column InputRow
  worksheet for the selected asset · summary · Form 4562 Part I.
- **View over container, verified non-drifting:** every lane (s107 draft row,
  per-asset FIFO save queue, create-once serialisation, import preview/commit,
  CSV template/export, bulk assignment, the election PATCH, delete) stays in
  `DepreciationSection`; the derived values (displayAssets / filteredAssets /
  activityGroups / totals) are computed ONCE and consumed by BOTH renderings.
- **Blank-commit map verified field by field against `models.py`** (a wrong
  blank 400s and the row silently reverts): non-nullable money → `"0"` ·
  nullable money / lives / dates → `null` · description → `""` ·
  **`business_pct` / `bonus_pct` → REFUSED** (non-nullable with meaningful
  defaults 100/0; legacy sent `""`, which 400s and reverts, so refusing
  client-side is legacy's EFFECT without the failed request — typing 0 still
  sets zero).
- Every serializer `read_only_fields` value is a **locked ƒx cell**
  (`noOverride`). AMT collapsible FLATTENED (density-first); vehicle and
  amortization spans keep their GROUP conditions; disposal keeps its legacy
  gate. **Form 4562 Part I §179 got a Slate span inside the lazy chunk** and
  its taxpayer PATCH was lifted to the container, so ONE lane serves both
  renderings and no retired GREEN/YELLOW markup survives on a converged
  screen. Group/method/convention/life lists + the group-defaults table are
  now shared consts (two duplicated inline group lists collapsed).

**Live QA (demo QA return, fully reverted):** Schedule C fixture → + Add asset
→ draft "unsaved" → description created the row in **exactly ONE POST** (201)
→ basis 50,000, PIS 2025-03-03, 200DB/HY/7 → **current depreciation 7,145**
(Pub 946 Table A-1 year-one 14.29%) as a locked ƒx cell → activity band
"$7,145 → Schedule C, Line 13" → **Sch C line 13 → SCH_1 L3 → 1040 L8 → AGI
94,560 − 7,145 = 87,415, all ORM-verified** → deleted through the screen's own
Del lane (204) → empty register → fixture removed → **AGI back to 94,560**.

**Gates at s133 close:** vitest **813/813** (+31 new) · tsc **46 = baseline** ·
2 side-by-sides committed
(`Design/slate-phase2-screenshots/{legacy,slate}-depreciation.png`) ·
`/bugs` sweep at boot: no open reports · console ≥400 noise identical in BOTH
modes on every headless run (same 403/404 baseline).

**Next action: continue the sweep at 6252 / 8824 / 7217**, then 8606, 8915-F,
SS lump-sum, 1099-G, State Refund, Misc Income, HSA 8889, EIC, 2441, 8962,
education, 5695, estimates/extension/e-file cards. Pattern settled:
view-over-container; **PayerTable** for flat record lists, **DocumentTabs +
worksheet** for card stacks, **InputRow worksheets** for facts cards
(screenbar header for singletons), **the asset register** for computed
sub-schedule grids; multi-section tabs share ONE `.slate-screen` at the call
site; screenshots per screen; live QA writes reverted.

**Dev QA recipe (proven again this session):** preview_start django-demo +
vite · demo QA return `bc270846-5800-4cbc-8f7f-573d0a5a953f` ·
`scripts/mint_magic_link.py` (SINGLE-USE — **mint per run**, a reused token
silently fails the login wait) · `scripts/slate_screen_screenshots.mjs
<returnId> <tokenFile> "<rail label>" <slug>` for rail-reachable screens,
**`scripts/slate_depr_screenshots.mjs` for Depreciation** (see below) ·
entry/revert pairs now exist for Sch E/K-1, Sch F, Sch J and **Depreciation
(`qa_depr_entry.mjs` / `qa_depr_revert.mjs`)**.
⚠ **On a 1040 Depreciation is NOT a rail item** — it is an activity sub-screen
reached from "Open depreciation worksheet →" on Schedule C / E / F (the tab id
exists only in `SECTION_TABS`/`PARTNERSHIP_TABS`). Any driver must navigate
through the activity. ⚠ **Chrome date inputs: commit via native setter +
`focusout`** — React's `onBlur` listens at the root for the BUBBLING
`focusout`, never the non-bubbling `blur`. ⚠ There is NO `.slate-summarybar`
class — judge AGI by ORM. ⚠ FFV ORM path = `form_line__section__form__code`;
the 1040's form code is `1040`. ⚠ Headless replace-typing: Ctrl+A before
page.type. ⚠ NEW_UI reads at module load — reload after setting localStorage.
⚠ QA .mjs scripts must live under the repo root (or `scripts/`) so
`puppeteer-core` resolves. ⚠ Heredocs mangle `\` — write Windows paths with
forward slashes or use the Write tool. ⚠ `manage.py shell < file` runs
line-by-line and breaks multi-line blocks — use `shell -c "$(cat file)"`.
⚠ Judge saves by settled responses, never flat sleeps.

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
   cosmetic; restyle-as-hook unit after the sweep. *(The §179 Part I card,
   the other legacy-styled panel, was converged this session.)*
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
- ⚠ **CORRECTION to the s132 close figure:** STATUS recorded the QA return's
  at-rest 1040 line 16 as **10,785**; the true value for the baseline inputs
  is **12,204**, confirmed this session (stable, idempotent, and independently
  hand-checked: ordinary 78,060 by the IRS tax-table method + 750 preferential
  at 15% ≈ 12,203–12,204). s132's 10,785 was a **stale stored value** — its
  final ORM deletion of the inert Schedule J row ran without a recompute.
  Ruled out as a defect: creating and removing an inert (un-elected, zeroed)
  Schedule J row leaves line 16 at 12,204 both ways, so no Schedule J path
  bug. **Lesson: an ORM delete on the QA return must be followed by
  `compute_return(tr)` before any figure is recorded as a baseline.**
- ⚠ Demo QA return (Slate QA Household `bc270846…`): carries the synthetic
  review data ON PURPOSE — s127 1099-R (TRS $24,000) + s128 1099-INT
  ($1,250/$300/$50 W/H) + 1099-DIV ($800/$600/$150) + SS box 5 $21,600.
  **At rest after the s133 revert (ORM-verified): 0 depreciation assets, 0
  Schedule C, 0 Schedule J, AGI 94,560, L15 78,810, L16 12,204.** Fed balance
  due ≈$8,512 at rest — expected, don't chase it.
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
