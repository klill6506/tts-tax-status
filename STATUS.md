# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 127 (**PHASE 1 RATIFIED BY KEN — Phase 2
underway: Return Manager SHIPPED** on branch `slate-ui`).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — Slate Phase 2, item 2: Diagnostics full view (three-pane workspace)

**KEN RATIFIED THE SLATE ARCHETYPE 2026-07-28** ("Yes I like it very much.
Please continue.") — recorded in DECISIONS.md. Phase 2 proceeds in plan §9
order, each screen screenshot-reviewed (shots posted to Ken as each lands;
repo copies in `Design/slate-phase2-screenshots/` — DEMO-DB shots only,
never prod data in committed files). Flag stays prod-OFF; the approval
covers the DESIGN, not the cutover — no merge/deploy without Ken.

**State:** Branch **`slate-ui`** (pushed): Phase 1 complete (Legs 0-7,
`1c0fc0c`…`513b92b`) · **Phase 2 item 1 SHIPPED `0672a6c`: Slate Return
Manager** — `slate/screens/SlateReturnManager.tsx` is a VIEW over the
legacy container (ALL state/behavior stays in `pages/ReturnManager.tsx`
and is passed as props — one state machine, two renderings, the FormEditor
pattern). Rail (244px): Views (All/My returns) + Status + Preparer + Tax
year as SINGLE-SELECT radios over the existing URL params (server contract
today — multi-select would need server work); entity-type strip = the
server's real per-tab counts (the legacy tabs' replacement, saved-tab
contract kept); search/chips/"N of M"/selection cluster/sort headers/
infinite-scroll sentinel/audited SSN reveal/Open/Delete/bulk assign all
delegate verbatim. AppShell takeover extended to `/` + `/returns` — incl.
the MENU BAR, which leaked above the Slate header on non-editor routes
(the `!isInEditor` gate didn't cover RM). Compact/Comfortable density
toggle (30/38px rows). **Deferred, in REVIEW_QUEUE:** the mock's
refund/due column + aggregate/average season totals need a server
aggregate (list payload has no amounts) — nothing was faked; the season
bar shows real counts + back-entry only.
**Live-verified demo DB:** `?status=` round-trips through the server (0
rows → chip → clear restores), search debounces to `?search=`, selection
cluster, row link lands in the Slate editor, flag OFF = legacy RM + legacy
chrome intact, zero trapped console errors. Review shots delivered to Ken
(rm-legacy vs rm-slate; demo DB has one return — sparse but faithful).
Gates: vitest **627/627** (10 new) · tsc **46 = baseline** · server
untouched.

**Next action (Phase 2 item 2): Diagnostics full view** (plan §9.2) —
three-pane workspace per the design's diagnostics.html: rail filters /
ranked table / detail pane (field-in-context mini-worksheet + ack
timeline — the ack system already exists server-side). Same pattern:
view over the existing DiagnosticsTab data machinery; screenshot review
at the end. Then §9.3 entry-paradigm convergence (screen list + rulings
as needed). Dev QA recipe: preview_start django-demo + vite → `#/` →
localStorage `delvio-new-ui`=1 → reload. Screenshot recipe:
`scripts/slate_screenshots.mjs` (+ `scripts/mint_magic_link.py`).

**Build rules in force:** presentation-only (server exceptions need Ken —
the RM aggregates item is QUEUED, not built) · selective `git add` only —
NEVER `git add .` (parallel tb_import work unstaged) · no merge/deploy
without Ken · at deploy: seed_rules BOTH DBs (D_W2_ family).

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE)
1. **NEW (s127h): RM refund/due column + aggregate season totals** — needs a
   read-only server aggregate; recommended as a Phase 2 polish unit.
2. RS D_8990_DISALLOW vs D_8990_EBIE conflicting guidance on a 1065 (s126e).
3. Retire MATH_BALANCE_SHEET's 1065 arm? (s126d).
4. RS R-M2-3-TIE adjudication (s126b).
5. K-1 box 13/11 type codes (s125).
6. RS 1065_B stale D_B2_B1 note + 5 unbuilt Sch B diagnostics (s126).
7. s124's `D_4562_RECON` scoping pair.
8. Real One Heart EIN in committed test fixtures (chip `task_f06ee3ed`).
9. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
10. *(minor)* Legacy "autofilled" yellow → Slate treatment ruling.
11. *(pre-existing)* D_8995/D_8959 NoneType crashes on skeleton returns.
12. *(cosmetic, Phase 2)* Legacy floating "Calculating…" chip needs a Slate home.

## Active gates
- **Branch discipline:** `slate-ui` checked out; parallel session's
  uncommitted work UNSTAGED (`server/apps/returns/views.py`, `tb_import.py`,
  `test_tb_import.py`). Never stage/stash/`git add .`.
- **Gates at s127 RM:** vitest **627/627** · tsc **46 = baseline**.
- ⚠ Demo QA return: has preparer "QA Test Preparer" (synthetic PTIN) since
  Leg 5 QA — print gate clear, D_PREPARER_001 silent on it.
- ⚠ FA-1040-4835-06 drift (chip `task_0cf10eac`).
- ⚠ One test DB — never overlap pytest runs.
- ⚠ `server/.venv` repaired s124; use `.venv\Scripts\python.exe` directly.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged — Slate Phase 2 runs in front by Ken's ratification; the form
queue interleaves on Ken's direction.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
- **Slate lane:** `Design/SLATE_IMPLEMENTATION_PLAN.md` ·
  `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md`.
