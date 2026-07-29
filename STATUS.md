# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 127 (**item 3a SHIPPED + three
Ken-directed entry fixes: EIN-first creates · payer state-ID tracking ·
ZIP autofill wiring** on branch `slate-ui`).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — Slate Phase 2, item 3b: next entry screen (INT/DIV payer grids)

Ken ratified the Slate archetype 2026-07-28; Phase 2 proceeds in plan §9
order, each screen screenshot-reviewed. Flag stays prod-OFF; no
merge/deploy without Ken.

**State:** Branch **`slate-ui`** (pushed `4b67755`): Phase 1 complete ·
item 1 Return Manager (`0672a6c`) · item 2 Diagnostics workspace
(`e7a0f30`) · **item 3a SHIPPED `4b67755`: the 1099-R stack on the
InputRow grid** — `slate/screens/SlateR1099Screen.tsx` is a VIEW over
RetirementIncomeSection (the s107 draft-row single-create path, the payer
EIN lookup, the draft remount generation, delete and the 5329 fact lane
all stay in the container). The vertical card stack becomes DOCUMENT TABS
(one per payer) over the two-column worksheet; Simplified Method + the
return-level 5329 span; running gross/taxable totals in the tools bar.
Adaptations, loud: the legacy "autofilled" YELLOW has no Slate mapping
(Ken ruling open) → neutral "from payer registry" hint, no colour
invented; `sm_taxable` is serializer-read-only → renders computed and
stays LOCKED via a NEW additive `noOverride` on FieldStateInput (proven:
the same dispatched Ctrl+Enter unlocks the W-2's Box 6 and does not
unlock this cell). One live-found defect fixed: the draft row is SPARSE
until first commit, so the controlled cells were flipping
uncontrolled→controlled — one normalizer at the boundary (display only;
the commit handlers still restore null-vs-zero).
**Live-verified demo DB:** entering a 1099-R through the Slate view =
ONE POST + six PATCHes against the SAME record (the split-record contract
holds), box 2a clears to NULL while a model-default box clears to "0", a
second draft starts clean, flag OFF = legacy card stack intact with zero
slate nodes and zero console errors. Side-by-sides committed
(`Design/slate-phase2-screenshots/legacy-r1099 · slate-r1099`) via the
NEW reusable `scripts/slate_screen_screenshots.mjs` (takes any rail label
— serves every remaining screen of the convergence).
Gates: vitest **650/650** (12 new; the draft-contamination test was
proven to bite by dropping the draftSeq key and watching it fail) · tsc
**46 = baseline**.

**THEN (same session, Ken live-directed — `712e558` + `fddb639`, pushed):**
**(1) EIN-first creates the 1099-R** (Ken ruled after seeing the live
proof): the client gate is now payer NAME **or** EIN — the serializer had
allowed an EIN-first create since 2026-07-02 but the client demanded the
name, so the registry lookup (whose job is FILLING the name) could never
fire on a new 1099-R. Pinned regression contract #2 updated in the same
change; its fake serializer had also drifted (400'd blank names the real
one accepts). Rode along: draft-keyed lookup marks (hints never showed on
draft cards, either rendering). **(2) Payer state-ID tracking** ("TaxWise
would not save the state ID"): the registry's EmployerStateAccount table
existed (the W-2 loop has been saving them all along) — the 1099-R loop
now FEEDS it (Box 15 state + payer's state number, learned even onto
bulk-imported payers, never modifying existing rows) and the lookup READS
it (single account + both cells blank → the pair fills; state typed later
→ ID fills from the cached accounts). **(3) ZIP autofill wiring** ("the
zip database doesn't seem to work very often"): the table was NEVER the
problem (40,979 rows both DBs, all Athens/Conyers ZIPs hit) — the hook
was missing from the TAXPAYER address (the most-typed address in the app)
and the 1099-R payer block; both wired, fill-blanks-only; Slate 1099-R
payer block reordered ZIP-before-City so the fill is visible.
Live-proven end-to-end on the demo DB (EIN-first → name/GA/state-ID all
filled + "from payer registry" hints · taxpayer ZIP 30606 → Athens, GA
persisted; all QA data reverted; demo registry seeded w/ synthetic TRS
payer + GA account). Gates: server **35/35** across the three
employer-learning bands (5 new) · vitest **656/656** (4 new) · tsc 46.
⚠ **These are flag-independent entry fixes riding `slate-ui`** — they
reach the live app only when Ken approves a merge/deploy of the branch
(or asks for a cherry-pick to main). Flagged to Ken in-session.

*(Item 2 detail, for reference)* `slate/screens/SlateDiagnosticsWorkspace.tsx`
is a VIEW over the legacy DiagnosticsTab container (runs/auto-run/ack
machinery untouched — one state machine, two renderings): rail filters
(severity · by-screen via RULE_TAB_MAP · Active/Acknowledged · persisted
internal toggle), ranked table (search, F3/Shift+F3 selection, Enter
jump, dbl-click jump), detail pane (severity pill · meta table · READ-ONLY
field-in-context worksheet off the SAME w2Derived as the entry screen ·
ack-with-note + unack · run history from real runs). Data-honest
adaptations (mock Category taxonomy / Authority row / editable ctx
inputs have no data source) → REVIEW_QUEUE s127i w/ recommendation.
**Two live-found fixes rode along (shared machinery, both renderings):**
(1) latest-run selection prefers the newest COMPLETED run — overlapping
mount auto-runs left an in-progress `runs[0]` that read as "no issues"
(dock, status dots, both diagnostics views affected); (2) **diagnostic
deep-link focus was a one-shot 80 ms race** — a jump from the diagnostics
view mounts the target screen through a LAZY chunk, so the single
dispatch went unclaimed and focus never landed (reproduced in headless
Chrome — NOT just the hidden-pane env); `requestFieldFocus` now retries
to a 3 s deadline and SlateW2Screen's claim path uses the shared
`focusFieldWhenReady`.
**Live-verified demo DB:** filters/search/F3/selection, ack → note
round-trip → unack re-alarm (server-proven via API), Go-to-field landing
proven with TRUSTED input in headless Chrome (`focused: box4`), flag OFF
= legacy diagnostics intact w/ zero slate classes + zero trapped console
errors, ≥400s attributed (login `/me/` 403 + `/prior-year/` 404,
same-count-same-endpoint across modes). Review shots delivered to Ken +
committed: `Design/slate-phase2-screenshots/` (legacy-diagnostics ·
slate-diagnostics · slate-diag-filtered · slate-goto-field; demo DB
only). Capture recipe: `scripts/slate_diag_screenshots.mjs`.

**Next action (item 3b): the INT/DIV payer grids** — plan §9.3 names them
the most likely "can't fit the grid" candidate (inline-columns + expand);
audit first, convert if it fits, otherwise LIST IT AND ASK rather than
force it. Then Social Security, Schedule 1/2/3 tables, state line tables;
then §9.4 launcher/login (separate delvio-launcher item). Dev QA recipe:
preview_start django-demo + vite → `#/` → localStorage `delvio-new-ui`=1
→ reload. Screenshots: `scripts/slate_screen_screenshots.mjs <returnId>
<tokenFile> "<rail label>" <slug>` (+ `scripts/mint_magic_link.py`).

**Build rules in force:** presentation-only (server exceptions need Ken —
RM aggregates + rule kind/authority columns are QUEUED, not built) ·
selective `git add` only — NEVER `git add .` (parallel tb_import work
unstaged) · no merge/deploy without Ken · at deploy: seed_rules BOTH DBs
(D_W2_ family).

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE)
1. **NEW: merge/cherry-pick the flag-independent entry fixes?** The
   EIN-first + state-ID + ZIP work benefits the LIVE app (legacy UI
   included) but rides `slate-ui` — Ken decides merge vs cherry-pick vs
   wait for the Slate cutover.
2. **(s127j, minor): the form-view pane still shows the W-2 facsimile on
   non-W-2 screens** — recommend falling back to 1040 p.1 off-W-2.
   *(Also minor: remaining address blocks — 2441 provider, preparer/firm —
   not yet audited for the ZIP hook; wire as each screen converges.)*
3. **(s127i): Diagnostics workspace adaptations** — mock's Category
   taxonomy / Authority row / editable field-in-context need small server
   columns if wanted; read-only ctx recommended as permanent.
4. RM refund/due column + aggregate season totals (s127h) — server aggregate.
5. **Legacy "autofilled" yellow → Slate treatment** — now CONCRETE: the
   1099-R payer fields ship with a neutral "from payer registry" hint
   instead of the retired yellow. Ruling wanted before more screens land.
6. RS D_8990_DISALLOW vs D_8990_EBIE conflicting guidance on a 1065 (s126e).
7. Retire MATH_BALANCE_SHEET's 1065 arm? (s126d).
8. RS R-M2-3-TIE adjudication (s126b).
9. K-1 box 13/11 type codes (s125).
10. RS 1065_B stale D_B2_B1 note + 5 unbuilt Sch B diagnostics (s126).
11. s124's `D_4562_RECON` scoping pair.
12. Real One Heart EIN in committed test fixtures (chip `task_f06ee3ed`).
13. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
14. *(pre-existing)* D_8995/D_8959 NoneType crashes on skeleton returns —
    visible as 4 errors in the new workspace shots; unchanged behavior.
15. *(cosmetic, Phase 2)* Legacy floating "Calculating…" chip needs a Slate
    home (overlaps the bottom-right in the review shots).

## Active gates
- **Branch discipline:** `slate-ui` checked out; parallel session's
  uncommitted work UNSTAGED (`server/apps/returns/views.py`, `tb_import.py`,
  `test_tb_import.py`). Never stage/stash/`git add .`.
- **Gates at s127 close:** vitest **656/656** · tsc **46 = baseline** ·
  employer-learning server bands **35/35**.
- ⚠ Demo employers registry now holds a SYNTHETIC payer: TRS of Georgia
  58-1234567 + GA account 1234567-AB (seeded for the EIN-first QA proof;
  synthetic, demo DB only, harmless to keep).
- ⚠ Demo QA return: has preparer "QA Test Preparer" (synthetic PTIN) since
  Leg 5 QA — print gate clear, D_PREPARER_001 silent on it. A QA
  ack/unack cycle on D_W2_BOX5_SUGGEST was fully reverted. **Item 3a QA
  left ONE synthetic 1099-R on it** (Teachers Retirement System, $24,000
  gross / box 2a deliberately blank, GA withholding) — kept on purpose so
  the converged screen is reviewable; it moves the return to a balance
  due, which is why the refund monitor differs from earlier legs' shots.
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
