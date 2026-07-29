# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 128 (**overnight autonomous run, Ken
pre-authorized**: Phase 2 §9.3 entry convergence COMPLETE + all four queued
polish units + the §9.4 launcher port on its own branch).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — Ken's morning review of the s128 overnight run, then the bespoke-screen sweep

**Ken's pre-sleep rulings (2026-07-29, all four honored):** neutral registry
hint stays · additive read-only server work approved · legacy-feature
recommendations implemented (overrule in the morning) · launcher included.

**Branch `slate-ui` (pushed through `2611dc9`), five commits overnight:**

1. **Item 3b `48912f0` — INT/DIV payer grids.** New Slate `PayerTable`
   (slim-columns + expand KEPT — the ruling: the table IS the right shape for
   many-payers×few-numbers; the expansion is the archetype InputRow
   worksheet). Legacy clearValue/blank-name/source-summary rules verbatim,
   view-over-container. NEW with the convergence: payer-registry EIN lookup +
   ZIP autofill for INT/DIV via the shared `usePayerAutofill` hook (1099-R
   semantics verbatim: fill-blanks-only, einChanged guard, Box 15/16 pair
   fill, neutral hints). Live-proven demo DB end-to-end; side-by-sides
   committed ({legacy,slate}-{interest,dividend}.png).
2. **Item 3c `6116212` — Social Security screen.** View over the
   taxpayer-fact autosave lane; MFJ spouse column. Live-proven: box 5 21,600
   → ONE taxpayer PATCH → AGI +18,360 exactly (85% — the worksheet flows);
   GA correctly ignores it. Shots committed.
3. **Items 3d/3e `3e927dd` — SlateStandardSection.** ONE swap converges
   EVERY seeded face-line tab (Sch 1/2/3, GA-500/SC1040 state tables, and
   every other StandardSection tab): computed = locked ƒx (no override lane
   through FFV onChange), overridden = violet, booleans keep the s106
   tri-state, percentage/integer commit RAW (the s126g ratio lesson).
   Live-proven: Sch 1 alimony → AGI +1,200 and reverted; GA-500 renders
   50 rows/21 ƒx. Shots committed ({legacy,slate}-{sch1,ga500}.png).
4. **Polish `2611dc9` — all four queued units.**
   - RM refund/due column + season totals: NEW `apps/returns/rm_aggregates.py`
     (kept OUT of views.py — that file carries the parallel tb_import
     session's work) + `/rm/refund-due/` (batched per scroll page) and
     `/rm/season-totals/`; live-proven (the QA return's ($10,151) matches its
     editor monitor). NEW_UI-gated fetches; flag-off network identical.
   - Form pane context-follow: W-2 facsimile on the W-2 screen, primary-form
     p.1 everywhere else (closes REVIEW_QUEUE s127j item 1).
   - `DiagnosticRule.topic`/`.authority` (migration **0005** — applied to the
     DEMO DB; **prod applies at deploy**): schema + serializer + workspace
     display only; VALUES await Rule Studio authoring (no invented citations).
   - Legacy-feature homes: Slate Help menu is LIVE (Report a bug / Ken-Bot /
     About fire the legacy AppShell dialogs via `delvio:menu-action` — one
     implementation); full §13 dispositions table in
     `Design/SLATE_IMPLEMENTATION_PLAN.md` for morning overrule.
5. **Launcher §9.4 — delvio-launcher branch `slate-ui` (`60d17e8`), NOT
   merged** (a main push auto-deploys the LIVE login hub — merge is Ken's
   call). Split-card login + suite bar + accent app cards; 16/16 tests;
   preview shot `design/slate-login-preview.png`. Data-honest deferrals in
   that repo's STATUS.md.

**Gates at s128 close:** vitest **694/694** (38 new) · tsc **46 = baseline** ·
server: rm_aggregates 5 + rule-metadata 1 + diagnostics/ack regression slice
**52/52** · launcher 16/16 + build green.

**Next action (the "…n" of entry convergence): the bespoke-screen sweep.**
Every SEEDED tab is now Slate; what remains inside the shell are the BESPOKE
section components rendering their legacy interiors (plan §5's sanctioned
mid-migration state): Taxpayer Info, Preparer, Dependents, Schedule C, D,
E (+K-1 pg2), F, J, Depreciation, 6252, 8824, 7217, 8606, 8915-F, SS
lump-sum, 1099-G, State Refund, Misc Income, Schedule A, HSA 8889, EIC,
Credits/8812, 2441, 8962, education, 5695, estimates/extension/e-file cards.
Convert screen-by-screen on the established view-over-container pattern
(PayerTable for record lists, InputRow worksheets for cards), screenshots per
screen. Suggested order: highest-traffic first (Taxpayer Info → Dependents →
Schedule C → Schedule D → Credits/8812 → Schedule A).

**Dev QA recipe:** preview_start django-demo + vite → `#/` → localStorage
`delvio-new-ui`=1 → reload. Screenshots:
`scripts/slate_screen_screenshots.mjs <returnId> <tokenFile> "<rail label>"
<slug>` (+ `scripts/mint_magic_link.py`; script waits generalized for state
editors + table screens this session). ⚠ `puppeteer-core` is an ephemeral
root install (`npm install --no-save --no-package-lock puppeteer-core`) —
reinstall if node_modules was cleaned.

**Build rules in force:** presentation-only (server exceptions were
Ken-authorized this session and are shipped) · selective `git add` only —
NEVER `git add .` (parallel tb_import work unstaged: `server/apps/returns/
views.py`, `tb_import.py`, `test_tb_import.py`) · no merge/deploy without Ken
· at deploy: migrate (diagnostics 0005) + seed_rules BOTH DBs (D_W2_ family).

**KEN CLARIFIED (2026-07-28): the tax app is TESTING until January 2027.**
He switches to Slate when the redesign is FINISHED; everything rides
`slate-ui`; the shared Supabase DB caution is the one true-production
constraint (sherpa-1099 prod + ~700 real clients).

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE — trimmed to live items)
1. **(s128) Morning review of the overnight run** — 8 side-by-side shots in
   `Design/slate-phase2-screenshots/`, §13 dispositions table, launcher
   preview shot. Overrule anything; it comes back next session.
2. **(s128) delvio-launcher `slate-ui` merge** — your call (auto-deploys the
   live login hub).
3. **(s128) Rule topic/authority authoring** — columns live, values blank
   until authored in RS/seed_rules.
4. RS D_8990_DISALLOW vs D_8990_EBIE conflicting guidance on a 1065 (s126e).
5. Retire MATH_BALANCE_SHEET's 1065 arm? (s126d).
6. RS R-M2-3-TIE adjudication (s126b).
7. K-1 box 13/11 type codes (s125).
8. RS 1065_B stale D_B2_B1 note + 5 unbuilt Sch B diagnostics (s126).
9. s124's `D_4562_RECON` scoping pair.
10. Real One Heart EIN in committed test fixtures (chip `task_f06ee3ed`).
11. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
12. *(pre-existing)* D_8995/D_8959 NoneType crashes on skeleton returns —
    still visible in the s128 shots; unchanged behavior.
13. *(cosmetic)* Legacy floating "Calculating…" chip needs a Slate home.

## Active gates
- **Branch discipline:** `slate-ui` checked out; parallel session's
  uncommitted work UNSTAGED (`server/apps/returns/views.py`, `tb_import.py`,
  `test_tb_import.py`). Never stage/stash/`git add .`.
- ⚠ **Demo DB drift from prod schema:** diagnostics migration 0005 is applied
  to the DEMO DB only — prod applies at Ken's deploy (additive, safe).
- ⚠ Demo QA return (Slate QA Household): now carries synthetic review data
  kept ON PURPOSE for screen review — the s127 1099-R (TRS, $24,000) + NEW
  s128: 1099-INT "First AmeriBank of Athens" ($1,250 int / $300 exempt /
  $50 W/H / ZIP-filled Athens GA), 1099-DIV "Vanguard Brokerage"
  ($800/$600/$150), SS box 5 $21,600. Fed balance due now $10,151 —
  expected, don't chase it. All other QA writes reverted.
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
