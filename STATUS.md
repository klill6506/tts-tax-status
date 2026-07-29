# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 129 (bespoke-screen sweep: Taxpayer Info +
Dependents + SCHEDULE C converged; every s128 review item Ken-ruled; launcher
live; MATH_BALANCE_SHEET 1065 arm retired).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — the bespoke-screen sweep continues at **Schedule D**

**s129 late add — SCHEDULE C SHIPPED (`5247ec5`, sweep screen 3).**
SlateScheduleCScreen = view over ScheduleCSection: the card stack becomes
DOCUMENT TABS (1099-R paradigm) over the two-column worksheet; SE /
8995-8959 facts / QBI aggregation cards render as spans; aggregation CRUD
lifted to shared handlers; Schedule C types + field lists exported from
FormEditor (one list, two renderings). Blank-commit semantics legacy
VERBATIM (money "0" · sqft 0 · ints null); computed columns locked ƒx
(noOverride, Ctrl+Enter-stays-locked test-pinned); statutory-linked line 1
locks to the W-2 Box 1. Live-proven: tab add → POST → gross receipts 50,000
→ **computed NET PROFIT 50,000 + SE tax 7,065 flowed back into locked ƒx
cells** + the SE span auto-appeared; missing list keys caught live + fixed;
delete → 0 businesses; the zeroed orphan SE row = legacy compute behavior
(cleaned via ORM, QA return fully restored). Gates: vitest **714/714**
(8 new) · tsc **46** · shots committed.

**s129 (2026-07-29, after Ken's "go"): sweep screens 1–2 SHIPPED on
`slate-ui` (no deploy):**

1. **Taxpayer Info `851d809`** — SlateTaxpayerInfoScreen = view over
   TaxpayerInfoSection's useTaxpayerFacts lane + its commitValidated SSN/
   IP-PIN path. Shared INTO the container for both renderings: `ssnBlur`
   (mask guard + formatSsn + commitValidated) and `zipAutofill`. SSNs masked
   at rest; s105 DOB year gate kept (shared lib/dob); ages = locked computed
   cells; line-12 override renders `overridden` when set, commits null on
   clear. Live-proven: name write + mid-string edit + Ctrl+A clear all
   persisted; digital-asset select proven both directions; QA writes
   reverted ('' / null). vitest 701 (7 new) · tsc 46.
2. **Dependents `f5c4026`** — SlateDependentsScreen on PayerTable, ALL
   columns inline (no expansion needed). **PayerTable grew three additive
   capabilities for every future record list:** per-column custom `render`
   (dates/selects/checkboxes/validated cells), optional `detail` (expand
   column only when passed), `requireFirstColumn` opt-out (dependents first
   name may blank; payer screens keep the guard). SSN keeps legacy
   format-on-blur + ITIN tag; age + CTC/ODC = locked computed. Live-proven
   full lifecycle: type-to-add POST → PATCHes (name/relationship/DOB) →
   server-computed age 10 flowed back into the frozen cell → Slate delete →
   0 rows (QA data fully reverted). vitest 706 (5 new) · legacy suite 35 ·
   tsc 46.

**Gates at s129 close:** vitest **706/706** · tsc **46 = baseline** · demo QA
return restored (dependents 0, taxpayer fields '' / null as found).

**KEN RULED LIVE (2026-07-29, after the s129 close — two question rounds):**
§13 dispositions **APPROVED as shipped** · **launcher MERGED + DEPLOYED**
(delvio-launcher `60d17e8` → main; live bundle `index-BgY9TTrp.js` verified
carrying the Slate login at app.delviotax.com) · **RS authoring session
SCHEDULED** (agenda in REVIEW_QUEUE: topic/authority values · approved
D_8990_DISALLOW non-partnership scoping · D_8995/D_8959 None-guards ·
D_B2_B1 note · R-M2-3-TIE · K-1 box 13/11 codes) · **MATH_BALANCE_SHEET
1065-arm retirement APPROVED** (small build unit — do it next session
before/with Schedule C) · One Heart EIN fixture stays (item closed).

**MATH_BALANCE_SHEET 1065-arm retirement: DONE (`51e6f0b`, same day).**
Silent on every 1065 shape, no-coverage-hole test pins D_L_BALANCE reporting
the imbalance once; Schedule L leg 9/9 · diagnostics+flow 562/562. Demo DB
re-seeded; **deploy checklist: seed_rules BOTH DBs now also carries the
MATH_BALANCE_SHEET description change.**

**Next action: continue the sweep — Schedule D, then Credits/8812 →
Schedule A → the rest** (Preparer, Sch E (+K-1 pg2), F, J, Depreciation,
6252, 8824, 7217, 8606, 8915-F, SS lump-sum, 1099-G, State Refund, Misc
Income, HSA 8889, EIC, 2441, 8962, education, 5695, estimates/extension/
e-file cards). Pattern is settled: view-over-container; PayerTable (custom
renders) for record lists, DocumentTabs+worksheet for card stacks (Sch C is
the template for Sch E/F), InputRow worksheets for cards; screenshots per
screen; live QA writes reverted.

**s128 morning review: CLOSED** — every item ruled (see KEN RULED LIVE
above). The screenshots stay in `Design/slate-phase2-screenshots/` for
reference; overrules can still come back any session.

**Dev QA recipe (proven again this session):** preview_start django-demo +
vite · demo QA return `bc270846-5800-4cbc-8f7f-573d0a5a953f` (Slate QA
Household) · `scripts/mint_magic_link.py` (token is SINGLE-USE — mint per
run) · `scripts/slate_screen_screenshots.mjs <returnId> <tokenFile>
"<rail label>" <slug>`. ⚠ Headless QA lessons (s129): judge saves by
counting settled PATCH responses, never a flat sleep (a queued save can die
with the browser); Chrome date inputs eat Tab BETWEEN SEGMENTS — commit
DOBs via native-setter + bubbled `focusout` (s117); `page.select()` does NOT
move focus (a focused cell's blur never fires). ⚠ `puppeteer-core` =
ephemeral root install (`npm install --no-save --no-package-lock
puppeteer-core`).

**Build rules in force:** presentation-only (server untouched this session) ·
selective `git add` only — NEVER `git add .` (parallel tb_import work
unstaged: `server/apps/returns/views.py`, `tb_import.py`,
`test_tb_import.py`; ⚠ also never `git stash` here — it grabs views.py) ·
no merge/deploy without Ken · at deploy: migrate (diagnostics 0005) +
seed_rules BOTH DBs (D_W2_ family).

**KEN CLARIFIED (2026-07-28): the tax app is TESTING until January 2027.**
He switches to Slate when the redesign is FINISHED; everything rides
`slate-ui`; the shared Supabase DB caution is the one true-production
constraint (sherpa-1099 prod + ~700 real clients).

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE — trimmed to live items)
1. **(s129) The RS authoring session** — scheduled, runs when Ken sits down
   for it; agenda in REVIEW_QUEUE (topic/authority · D_8990 fix · D_8995/
   D_8959 guards · D_B2_B1 note · R-M2-3-TIE · K-1 box codes).
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
  Fed balance due $10,151 — expected, don't chase it. s129 QA writes fully
  reverted (0 dependents; taxpayer name/digital-assets restored).
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
