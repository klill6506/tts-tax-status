# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 130 (bespoke-screen sweep: SCHEDULE D +
CREDITS/8812 + the SCHEDULE A TAB converged; all live-proven; no open bug
reports at boot).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — the bespoke-screen sweep continues at **Preparer**, then the remainder

**s130 shipped THREE sweep units on `slate-ui` (no deploy):**

1. **Schedule D `4126eb9` (sweep 4)** — SlateScheduleDScreen on the INT/DIV
   PayerTable paradigm: 8 slim columns (frozen computed (h); V/INH date
   shorthand kept), expansion = i8949 col-(f) code chips (NEW
   `.slate-codechip`, tokens only) + Exception-2 flags, facts span, ST/LT
   totals by box membership. **Type-to-add retires the s107 draft row under
   the flag** (the create always carries a valid column (a)). Live-proven:
   computed (h) 2,000 flow-back → box A→D ST→LT re-net → chips 'W'/'BW' →
   AGI 94,560→96,560→back; return ORM-verified clean.
2. **Credits & 8812 `75d7c45` (sweep 5)** — SlateCredits8812Screen on the
   archetype worksheet (the SS paradigm, one useTaxpayerFacts lane): SSN
   gates (spouse rows MFJ/MFS), ACTC opt-out, MAGI add-backs (2555 amount
   conditional), the 8 Sch 1/2/3 placeholder totals + Worksheet-B/RRTA
   flags. Live-proven: PATCH bodies exact (eitc_claimed 3000→0, actc_opt_out
   true→false), reverted.
3. **Schedule A tab `55bd4f3` (sweep 6)** — three stacked section views in
   ONE `.slate-screen` at the call site (⚠ `.slate-screen` roots must never
   stack — the negative margins overlap; caught on the first screenshot):
   SlateScheduleAScreen (facts; 5a auto/override/sales-tax trio as a neutral
   hint; the line-12→8283 notice ladder stays computed in the CONTAINER) ·
   SlateStateTaxPayments (slim table; out-of-year exclusion hint; 5a running
   total) · SlateNoncashContributions (8283 slim table + Section B/appraiser/
   Part V expansion; commit semantics legacy VERBATIM; 50%/30% buckets;
   `form-8283-items` jump target kept; BOTH mounts converge — the entity
   charitable tab keeps its R-8283-ENTFEED wording). Live-proven across all
   three lanes; ORM-verified clean after.

**Gates at s130 close:** vitest **746/746** (32 new this session) · tsc
**46 = baseline** · 8 shots committed (schd/credits8812/scha ×
legacy/slate/live) · `/bugs` sweep at boot: no open reports.

**Next action: continue the sweep** — remaining screens: Preparer, Sch E
(+K-1 pg2), F, J, Depreciation, 6252, 8824, 7217, 8606, 8915-F, SS lump-sum,
1099-G, State Refund, Misc Income, HSA 8889, EIC, 2441, 8962, education,
5695, estimates/extension/e-file cards. Pattern settled: view-over-container;
PayerTable (custom renders + optional detail) for record lists,
DocumentTabs+worksheet for card stacks (Sch C = the template for Sch E/F),
InputRow worksheets for facts cards; multi-section tabs share ONE
`.slate-screen` at the call site; screenshots per screen; live QA writes
reverted.

**Dev QA recipe (proven ×3 this session):** preview_start django-demo +
vite · demo QA return `bc270846-5800-4cbc-8f7f-573d0a5a953f` ·
`scripts/mint_magic_link.py` (SINGLE-USE — mint per run) ·
`scripts/slate_screen_screenshots.mjs <returnId> <tokenFile> "<rail label>"
<slug>`. ⚠ **NEW_UI reads at module load — after setting localStorage you
MUST reload; hash-nav alone renders LEGACY** (burned two tokens before
diagnosis). ⚠ QA .mjs scripts must live under the repo root (ESM resolves
puppeteer-core from the script's path). ⚠ Judge saves by counting settled
PATCH responses, never flat sleeps. ⚠ `puppeteer-core` = ephemeral root
install (`npm install --no-save --no-package-lock puppeteer-core`).

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
1. **(s129) The RS authoring session** — scheduled; agenda in REVIEW_QUEUE
   (topic/authority · D_8990 fix · D_8995/D_8959 guards · D_B2_B1 note ·
   R-M2-3-TIE · K-1 box codes).
2. **(s129) Launcher menu extras** — recents feed / firm stats / locked
   cards have no data source; rulings wanted before server work.
3. s124's `D_4562_RECON` scoping pair.
4. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
5. *(cosmetic)* Legacy floating "Calculating…" chip needs a Slate home.
6. *(s126)* Sch B 3a/3b detail tables entry gap; 5 unbuilt Sch B diagnostic
   runners (build leg, pairs with the RS session's D_B2_B1 fix).

## Active gates
- **Branch discipline:** `slate-ui` checked out; parallel session's
  uncommitted work UNSTAGED (`server/apps/returns/views.py`, `tb_import.py`,
  `test_tb_import.py`). Never stage/stash/`git add .`.
- ⚠ **Demo DB drift from prod schema:** diagnostics migration 0005 applied
  to the DEMO DB only — prod applies at Ken's deploy (additive, safe).
- ⚠ Demo QA return (Slate QA Household `bc270846…`): carries the synthetic
  review data ON PURPOSE — s127 1099-R (TRS $24,000) + s128 1099-INT
  ($1,250/$300/$50 W/H) + 1099-DIV ($800/$600/$150) + SS box 5 $21,600.
  Fed balance due ~$8,020 — expected, don't chase it. All s130 QA writes
  fully reverted (ORM-verified: 0 capital transactions, 0 payments, 0
  noncash items, facts at defaults).
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
