# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-29, session 137 (bespoke-screen sweep: THE STATE REFUND
WORKSHEET, §111 / Pub 525 Worksheet 2 + 2a, as unit 17 — live-proven end-to-end
and fully reverted; three real defects fixed, one of them a silent $1,200
understatement of income.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — the bespoke-screen sweep continues at **Misc Income**

**s137 shipped unit 17 on `slate-ui` (no deploy): `f984261`**

### Unit 17 — the State Refund Worksheet (§111), `f984261`
- **The s132 SINGLETON paradigm.** Every input is an `sr_*` fact on the
  `Taxpayer` — no record collection, nothing keyed by row id — so: screenbar +
  two-column InputRow worksheet, no DocumentTabs, no PayerTable. The ONE save
  lane (`useTaxpayerFacts` → `saveFacts`, i.e. the `/taxpayer/` PATCH) stays in
  FormEditor's `StateRefundSection`; the Slate screen is a prop-fed rendering.
- **The whole computed chain renders from the server's OWN `STATE_REFUND` rows**
  as locked ƒx cells (`noOverride`) — a3/a4/a5, 1a/1b, w3/w7/w8/w9/w11,
  sch1_1/sch1_8z. The client never re-implements the §111 math, so the
  prior-year standard-deduction table stays a single source of truth on the
  server. The legacy screen threw those twelve lines away and printed two
  numbers. Only genuinely computed lines get ƒx — a1/a2/w2/w4/w10 are the
  seeder's `input` lines and are the entry cells above.
- **Blank-commit map:** every `sr_*` field is non-nullable with a default, so
  blank commits **"0"** and **nothing on this screen ever commits null**
  (serializer-proven: `""` invalid, `"0"` valid, `null` invalid).
  `sr_py_filing_status` is a select and is never blank.
- ⚠ The age/blind **count** is deliberately `type="currency"`:
  `normalizeCurrency`'s `String(Math.round(n))` guarantees the IntegerField
  receives a whole integer or `""` (→ `"0"`), so no keystroke can 400. A plain
  text cell would let `"2.5"` or `"abc"` through.

**THREE REAL DEFECTS FOUND AND FIXED — all from reading the engine against the
screen:**
1. **Blank-commit (silent revert).** `sr_py_age_blind_boxes` was missing from
   `SR_DECIMAL_FIELDS`, so clearing it sent `""` — and it is an
   **IntegerField**, not a Decimal, which DRF rejects with *"A valid integer is
   required"*. A 400, so the cell silently reverted. Proven at the serializer
   and live: the clear PATCH body is now `{"sr_py_age_blind_boxes":"0"}` → 200.
   The legacy rendering is fixed by the same line. `SR_DECIMAL_FIELDS` is now
   **exported** so the regression test binds to the real list, not a copy.
2. **A Pub 525 exception read "$0 taxable".** With any of the three exception
   flags on, `compute_state_refund_db` **disengages** — it blanks the worksheet
   rows AND Schedule 1 line 1/8z — yet the legacy panel still printed *"Taxable
   refund (Schedule 1 line 1): $0.00"*. The preparer was being told zero when
   the correct answer is "refigure by hand". The ƒx cells now disappear
   entirely and the screen says **NOT COMPUTED**, names which exception fired,
   and says where to key the result.
3. ⚠⚠ **A blank prior-year Sch A line 5e silently zeroes the ENTIRE refund.**
   `worksheet_2a` takes the cap-limited branch whenever `5d > 5e`, so with 5e at
   0 the recapture is the whole of 5d, `a3 <= a4` always holds, and the
   worksheet returns `(0, 0)`. **No diagnostic covers it** — `D_SR_INCOMPLETE`
   checks only line 17 and 5d. Engine-proven: identical inputs, 5e blank turns
   **$1,200 of taxable income into $0**. The 5e cell now carries the invalid
   overlay + the alert that explains it. The **genuine** cap-limited zero (5e
   entered, recapture ≥ the recovery) gets its own explanatory note, so a real
   $0 is never mysterious and a real defect is never mistaken for one.

**Also surfaced at the cause** (the unit-16 ruling): `D_SR_INCOMPLETE` on the
5d/17 cells, the §111 standard-deduction / sales-tax gate, the server's own w8
*"itemized did not exceed standard"* outcome, the 8z routing note, and an
**over-4 age/blind count** (`py_standard_deduction` multiplies the count through
with **no ceiling** and the serializer does not cap it, so a stray 9 inflates
the prior-year standard deduction and shrinks the taxable refund). The prior
year is **named everywhere** (2024 on a TY2025 return) so the preparer knows
which return to pull.

**Live QA (demo QA return, fully reverted, ORM-verified):** 2024 itemized
$40,000 with SALT uncapped at $9,000 and a $1,200 income-tax refund → a3 1,200 /
a4 0 / a5 1,200 / 1a 1,200 / w3 1,200 / **w7 29,200** (the 2024 MFJ standard
deduction, server-computed) / w8 10,800 / w9 1,200 / w11 1,200 → Sch 1 line 1
**1,200** → 1040 L8 1,200 → **AGI 94,560 → 95,760**. Clearing 5e drops sch1_1 to
0 and raises the overlay + alert; restoring returns 1,200. The AMT exception
removes every ƒx cell and shows NOT COMPUTED. All 15 PATCHes 200. Reverted
through the screen's own lane → back to **AGI 94,560 / L15 78,810 / L16 12,204 /
L25b 2,450**.

**Gates at s137 close:** vitest **897/897** (+12 new) · tsc **46 = baseline** ·
side-by-sides committed
(`Design/slate-phase2-screenshots/{legacy,slate}-staterefund.png`) · console
≥400 noise identical to baseline (the same 403/404 set).

**Next action: continue the sweep at Misc Income**, then HSA 8889, EIC, 2441,
8962, education, 5695, estimates/extension/e-file cards. Pattern settled:
view-over-container; **PayerTable** for flat record lists **keyed by row id**,
**DocumentTabs + worksheet** for card stacks AND for per-filed-form rows (the
unit-12 ruling), **InputRow worksheets** for facts cards (screenbar header for
singletons — unit 17 is the cleanest example), **the asset register** for
computed sub-schedule grids, and a **bare `.slate-asstable`** for a nested
replace-all list whose rows have no id yet; paradigms may NEST (7217, 8915-F);
multi-section tabs share ONE `.slate-screen` at the call site; screenshots per
screen; live QA writes reverted.

**Dev QA recipe (proven again this session):** preview_start django-demo + vite ·
demo QA return `bc270846-5800-4cbc-8f7f-573d0a5a953f` ·
`scripts/mint_magic_link.py` (SINGLE-USE — **mint per run**; defaults to the
DEMO DB) · `scripts/slate_screen_screenshots.mjs <returnId> <tokenFile>
"<rail label>" <slug> [outDir]` · new `scripts/qa_unit17_{entry,revert}.mjs
<returnId> <tokenFile>`.
⚠ **Demo-DB ORM probes need `TTS_ENV=demo`** — `manage.py shell` alone points at
the shared prod DB, and `scripts/runserver_demo.py` *starts a server* rather
than giving a shell (a 4-minute timeout this session).
⚠ A QA `.mjs` must live **under the repo root** — `puppeteer-core` is an
ephemeral root install.
⚠⚠ **`Ctrl+A` only SELECTS** — clearing a cell needs an explicit `Backspace`,
or no commit fires and a driver awaiting that write **hangs forever**.
⚠⚠ **Settling on "no write ISSUED for N ms" is a NO-OP** — count **in-flight**
writes (`request` ++ / `requestfinished|requestfailed` --), then allow a commit
tick. A `/taxpayer/` PATCH re-derives the whole return and takes tens of seconds.
⚠⚠ **A checkbox click TOGGLES** — assert the target state; an unconditional
click flips the flag back off on a second run.
⚠ PowerShell mangles a multi-line `manage.py shell -c` — run ORM probes from the
Bash tool with `shell -c "$(cat file)"`. ⚠ NEW_UI reads at module load — reload
after setting localStorage. ⚠ There is NO `.slate-summarybar` — judge AGI by ORM.
⚠ FFV ORM path = `form_line__section__form__code`; the 1040's form code is `1040`.
⚠ A rail label passed to the screenshot driver is a **REGEX**.

**Build rules in force:** presentation-only (server untouched this session) ·
selective `git add` only — NEVER `git add .` (parallel tb_import work still
unstaged: `server/apps/returns/views.py`, `tb_import.py`, `test_tb_import.py`;
⚠ also never `git stash` here) · no merge/deploy without Ken · at deploy:
migrate (diagnostics 0005) + seed_rules BOTH DBs (D_W2_ family +
MATH_BALANCE_SHEET description).

**KEN CLARIFIED (2026-07-28): the tax app is TESTING until January 2027.**
He switches to Slate when the redesign is FINISHED; everything rides
`slate-ui`; the shared Supabase DB caution is the one true-production
constraint (sherpa-1099 prod + ~700 real clients).

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE — trimmed to live items)
1. **(s137, NEW — tax-accuracy gap, server-side)** Three holes the unit-17
   screen now *warns* about but that no **diagnostic** covers. Each is a silent
   wrong number on a filed return and belongs in `rules_state_refund.py`:
   (a) **a blank prior-year Sch A line 5e** while 5d > 0 → the whole refund
   silently untaxed (the $1,200 → $0 proof above); (b) **an age/blind box count
   above 4** → `py_standard_deduction` has no ceiling; (c) **a tax year with no
   `PY_STD_DEDUCTION` entry** (2027+) silently falls back to the **2024**
   constants — `D_SR_TY2026_INTERIM` fires for 2026 only. The sweep is
   presentation-only, so these were surfaced at the cells, not fixed in the
   engine. Wants an RS-session decision + a seeded rule.
2. **(s137, minor)** `worksheet_2` computes `w8 = w6 - std` unclamped while
   `_worksheet_rows` displays `max(0, w6 - std)`. The taxable result is
   identical (both gate on `w8 <= 0`); only the displayed WS2-8 differs, showing
   0 where the worksheet would show a negative. Cosmetic — confirm which the
   statement page should print.
3. **(s130, re-confirmed s136/s137) PATCH `/taxpayer/` and the per-form PATCH
   lanes run tens of seconds in-process** on the QA return — every unit-17
   write did. Profile in a session that owns views.py.
4. **(s131) Form 7203 panel legacy-styled inside the Slate K-1 screen** —
   cosmetic; restyle-as-hook unit after the sweep.
5. **(s129) The RS authoring session** — scheduled; agenda in REVIEW_QUEUE.
6. **(s129) Launcher menu extras** — no data source; rulings wanted.
7. s124's `D_4562_RECON` scoping pair.
8. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
9. *(cosmetic)* Legacy floating "Calculating…" chip needs a Slate home.
10. *(s126)* Sch B 3a/3b detail tables entry gap; 5 unbuilt Sch B diagnostic
    runners.
11. **(s136)** Replace-all nested lists (8915-F item C today) remain vulnerable
    to **out-of-order server arrival** of two overlapping PATCHes — the ref fix
    removes the stale-closure clobber but cannot order the requests. A per-lane
    single-flight queue would close it; scope it if a second one appears.

## Active gates
- **Branch discipline:** `slate-ui` checked out (pushed through `f984261`);
  parallel session's uncommitted work UNSTAGED (`server/apps/returns/views.py`,
  `tb_import.py`, `test_tb_import.py`). Never stage/stash/`git add .`.
- ⚠ **Demo DB drift from prod schema:** diagnostics migration 0005 applied
  to the DEMO DB only — prod applies at Ken's deploy (additive, safe).
- ⚠ Demo QA return (Slate QA Household `bc270846…`): carries the synthetic
  review data ON PURPOSE — s127 1099-R (TRS $24,000) + s128 1099-INT
  ($1,250/$300/$50 W/H) + 1099-DIV ($800/$600/$150) + SS box 5 $21,600.
  **At rest after the s137 revert (ORM-verified): all 17 `sr_*` facts at their
  defaults, 0 Forms 8915-F, 0 Forms 8606, 0 Roth trackers, 0 SS lump-sum rows,
  both SS lump-sum flags false, 0 Forms 1099-G, AGI 94,560, L4b 0, L5b 24,000,
  L6a 21,600, L6b 18,360, L8 0, L15 78,810, L16 12,204, L25b 2,450.** Fed
  balance due ≈$8,512 at rest — expected.
  ⚠ The `STATE_REFUND` rows now read `""` rather than `None` — `_backfill_values`
  created them and `disengage()` blanked them. Semantically identical (both
  parse to ZERO); do not treat it as drift.
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
