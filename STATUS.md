# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 131 (bespoke-screen sweep: SCHEDULE E +
K-1 PAGE 2 converged as sweep unit 8; live-proven end-to-end and fully
reverted; no open bug reports at boot).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — the bespoke-screen sweep continues at **Schedule F**, then J / Depreciation / the remainder

**s131 shipped sweep unit 8 on `slate-ui` (`b5c47ef`, pushed, no deploy):
Schedule E (Part I rentals & royalties) + Schedule E page 2 (K-1 router) as
TWO screens on the Schedule C DocumentTabs paradigm** — view-over-container,
one state machine, two renderings:

- **SlateScheduleEScreen** — property tabs over the two-column worksheet
  (address/type/days · income · the 15 expense lines); line 18 renders a
  locked ƒx cell when the Depreciation worksheet feeds the property; line 21
  net + 8582 allowed/suspended locked computed; the Schedule E / 8582 / 461
  facts span rides the ONE useTaxpayerFacts lane (REP sub-panel gate
  verbatim). Money cells commit the raw CurrencyInput-lane value VERBATIM
  (blank stays "" — never coerced to "0"; day ints keep `parseInt(v) || 0`).
- **SlateScheduleK1Screen** — K-1 tabs; the box grid IS the legacy
  sortK1FieldsByBox (source_type drives box visibility: 1065 GP/SE vs 1041
  other-portfolio proven); `k1EinValid` lifted to module scope and shared by
  both renderings (invalid = overlay + error rownote); 8582 + §199A lanes;
  **the legacy Form 7203 panel mounts VERBATIM via a render prop** (one
  implementation — restyle queued as a cosmetic REVIEW_QUEUE item); computed
  page-2 totals span (32/37/41).

**Live QA (demo return `bc270846…`), full lifecycle proven:** add property →
exact PATCH bodies (`description`/`rents_received: "24000"`/`repairs`) → net
23,000 flowed back into the ƒx cell; add K-1 → box 1 5,000 → Part II 5,000 /
line 41 28,000 on-screen; **AGI 94,560 → 122,560 ORM-verified** (1040 L11 +
SCHEDULE_E L26/32/41 exact); both records then deleted THROUGH the Slate
delete buttons (204s, recompute fired); **return restored clean: props 0,
k1s 0, L11 = 94,560, Sch E lines blank.** ≥400 console noise identical
count+endpoint across legacy/slate (pre-existing baseline).

**Gates at s131 close:** vitest **765/765** (14 new) · tsc **46 = baseline**
(0 errors in the new screens) · 4 side-by-sides committed
(`Design/slate-phase2-screenshots/{legacy,slate}-{sche,k1p2}.png`) ·
`/bugs` sweep at boot: no open reports.

**Next action: continue the sweep at Schedule F** (ScheduleF1040Section —
the Sch C/E DocumentTabs template again), then J, Depreciation, 6252, 8824,
7217, 8606, 8915-F, SS lump-sum, 1099-G, State Refund, Misc Income, HSA
8889, EIC, 2441, 8962, education, 5695, estimates/extension/e-file cards.
Pattern settled: view-over-container; PayerTable for record lists,
DocumentTabs+worksheet for card stacks, InputRow worksheets for facts cards;
multi-section tabs share ONE `.slate-screen` at the call site; screenshots
per screen; live QA writes reverted.

**Dev QA recipe (proven again this session):** preview_start django-demo +
vite · demo QA return `bc270846-5800-4cbc-8f7f-573d0a5a953f` ·
`scripts/mint_magic_link.py` (SINGLE-USE — mint per run) ·
`scripts/slate_screen_screenshots.mjs <returnId> <tokenFile> "<rail label>"
<slug>` · NEW this session: `scripts/qa_sche_k1_entry.mjs` +
`qa_sche_k1_revert.mjs` (the trusted-typing entry/revert pair — reusable
shape for the remaining sweep screens). ⚠ Headless replace-typing: use
Ctrl+A before page.type — `click({clickCount: 3})` does NOT reliably select,
values APPEND (24000 became 240000 on the first run; caught by ORM before
any assertion trusted it). ⚠ FFV ORM path is
`form_line__section__form__code`. ⚠ PS 5.1 `Out-File -Encoding utf8` writes
a BOM — a `git commit -F` message file picks it up into the subject line;
use ASCII for commit-message files. ⚠ NEW_UI reads at module load — reload
after setting localStorage. ⚠ QA .mjs scripts live under the repo root.
⚠ Judge saves by settled PATCH responses, never flat sleeps.

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
  Fed balance due ~$8,020 at rest — expected, don't chase it (the s131
  committed shots show $14,438 because the QA rental/K-1 were IN PLACE for
  the capture). All s131 QA writes fully reverted after (ORM-verified:
  0 rental properties, 0 schedule K-1s, 1040 L11 = 94,560).
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
