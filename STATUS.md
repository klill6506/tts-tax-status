# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 132 (bespoke-screen sweep: SCHEDULE F +
SCHEDULE J converged as sweep units 9 and 10; both live-proven end-to-end and
fully reverted; no open bug reports at boot).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — the bespoke-screen sweep continues at **Depreciation** (the big one), then 6252 / 8824 / 7217 / the remainder

**s132 shipped sweep units 9 + 10 on `slate-ui` (no deploy):**

- **Schedule F `aa24213` — SlateScheduleFScreen** on the Sch C DocumentTabs
  paradigm (view-over-container over ScheduleF1040Section): farm tabs over
  the two-column worksheet (A–G identification w/ EIN overlay + the accrual
  hard warning · Part I income w/ computed 1c/9 · Part II expenses w/ locked
  line 14 from the 4562 engine + 32/33 · 8582 panel gated line E "No" AND
  cash · 32a-f nested other-expense rows · SE farm-optional span · computed
  farm total → Sch 1 line 6). **Money commits "0" on blank (the legacy Sch F
  lane = Sch C's, NOT Sch E's raw lane — carry per-section).** Live QA:
  raised 40,000/repairs 5,000/other 500 → net 34,500 ƒx flow-back →
  **SCH_1 L6 34,500 / AGI 94,560 → 126,622 ORM-verified** → Slate delete
  (204) → restored clean (0 farms, L11 94,560). The zeroed orphan SE row
  after farm delete = the KNOWN s129 residue class, zero-filtered everywhere.
- **Schedule J `f1b3f23` — SlateScheduleJScreen** on the Credits/8812
  singleton paradigm (screenbar + worksheet; no tabs — the three base years
  are FIXED, not records): election + line 2 + CY preferential detail + base
  year 1 left · base years 2–3 right · computed span (4/6/8/12/16/17/22/23)
  gated on the election. Election checkbox commits a PLAIN boolean (not
  tri-state); base-year fs selects keep the blank "(same: …)" option; legacy
  `<details>` collapsibles flattened (density-first); per-base-year
  aria-labels carry the calendar year. Live QA: singleton created on first
  edit → elect → 3 base years 50,000/6,000 → line 6 = 10,000 / line 23 =
  12,425 → **1040 L16 10,785 → 12,425 ORM-verified** → un-elect + zero
  through the same lane → restored exact (L16 10,785); the inert QA
  singleton row ORM-removed (no UI delete lane exists for it, matching
  legacy).

**Gates at s132 close:** vitest **782/782** (17 new: 11 Sch F + 6 Sch J) ·
tsc **46 = baseline** · 4 side-by-sides committed
(`Design/slate-phase2-screenshots/{legacy,slate}-{schf,schj}.png`) ·
`/bugs` sweep at boot: no open reports · console ≥400 noise identical to the
s131 baseline on every headless run.

**Next action: continue the sweep at Depreciation** (DepreciationSection —
the LARGEST bespoke screen: asset table + bulk assignment + conversion entry
+ bonus elect-out classes, built s116–s120; plan the Slate shape before
coding — likely PayerTable-style asset rows + a detail worksheet), then
6252, 8824, 7217, 8606, 8915-F, SS lump-sum, 1099-G, State Refund, Misc
Income, HSA 8889, EIC, 2441, 8962, education, 5695,
estimates/extension/e-file cards. Pattern settled: view-over-container;
PayerTable for record lists, DocumentTabs+worksheet for card stacks, InputRow
worksheets for facts cards (screenbar header for singletons); multi-section
tabs share ONE `.slate-screen` at the call site; screenshots per screen;
live QA writes reverted.

**Dev QA recipe (proven again this session):** preview_start django-demo +
vite · demo QA return `bc270846-5800-4cbc-8f7f-573d0a5a953f` ·
`scripts/mint_magic_link.py` (SINGLE-USE — mint per run) ·
`scripts/slate_screen_screenshots.mjs <returnId> <tokenFile> "<rail label>"
<slug>` · entry/revert pairs now exist for Sch E/K-1 (`qa_sche_k1_*`), Sch F
(`qa_schf_*`), Sch J (`qa_schj_*`) — the reusable trusted-typing shape.
⚠ There is NO `.slate-summarybar` class — judge AGI by ORM (the bottom bar
is the refund monitor). ⚠ FFV ORM path = `form_line__section__form__code`;
the 1040's form code is `1040` (not F1040). ⚠ Headless replace-typing: Ctrl+A
before page.type. ⚠ NEW_UI reads at module load — reload after setting
localStorage. ⚠ QA .mjs scripts live under the repo root. ⚠ Judge saves by
settled PATCH responses, never flat sleeps.

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
  Fed balance due ~$8,020 at rest — expected, don't chase it. All s132 QA
  writes fully reverted (ORM-verified: 0 farms, 0 Schedule J rows, L11 =
  94,560, L16 = 10,785). Note: the s132 farm delete left SCH_1 L6 = "0"
  (was blank) — cosmetic recompute residue, value-equal.
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
