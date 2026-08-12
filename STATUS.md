# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-12 (s243b). **✅ KEN'S FOUR-RETURN UNBLOCK BUILT
AND DEPLOYED (one deploy, no migrations)** — PENDING items 6/7/8 +
Walton GA RIE. Walton/Tucker/Parsons now tie their frozen payloads;
Patel is REFUTED as an app defect (the filed Schedule D face itself
doesn't foot by exactly $2,229 — source-side gap, reported to Ken).
Root causes: a BASIS-ONLY Form 8606 superseding an owner's whole IRA
taxable to 0 (→ `owner_supersedes_4b` gate, per-owner, in compute_8606
+ the RIE L11 pull); the 2441 Part III exclusion ignoring qualified
expenses (→ `part3_dcb` line-17 limit + `compute_2441_dcb_early` so 1e
reaches AGI/the SS worksheet in one pass); GA under-62 disability RIE
lacking date/type/7c-routing (→ 4 seeded GA lines, render routing,
3 diagnostics). Plus D_8606_BASIS_ONLY (warning) and D_RET_012 (error,
fault=engine — the SS-worksheet invariant). 13 new regressions +
106 neighbors + 612 GA/flow/SS + 217 RIE/commit/efile + check green;
`seed_ga500`/`seed_form_2441` re-run against the shared DB. Earlier
today s243 shipped BATCH-001 #8 (IND-CR 202 feed, `b62cc09`).*

*The 1040 lane closed THREE full batches in three days (005 s242i, 004
s242l, 003 s242t). Open queues: BATCH-001 (6), BATCH-002 (NOL-blocked
computes), Form 8853 legs 2-4, IRS1116, amended MeF.*

*Previous (s242i): ✅✅ BATCH-005 COMPLETE — 10 of 10, moved to Done. Ten
items → eight builds + two verify-and-closes, ten deploys, migs 0289-0298.
(s241w): ✅✅ BATCH-004 #5 Schedule H complete, all six legs. Design records:
the `server/specs/_*_source_brief.md` files + the batch annexes — do not
re-triage closed items.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax`; demo = `tts-tax-demo`. **The orphan third service
`tts-tax-app` still fails every push — Ken should delete it.**

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

## ⚠ KEN IS AWAY 2026-08-09 → ~2026-08-19 (10 days)
**Availability MINIMAL BUT NOT ZERO.** Batch questions; keep them low-friction.
Nothing is on a clock in that window; the next hard deadline is 2026-09-15.

---

## ▶ RESUME HERE

### ✅ s243b COMPLETE — Ken's four-return unblock (superseded the queue)
`D:\tax-test-data\1040\tmp\PENDING_CC_CHANGES.md` items 6/7/8 + the GA
RIE case, built and deployed in ONE commit. **Codex can rerun the four
production dry-runs.** Per-return verdicts (frozen payloads replayed
through the real API):
- **Return P (item 6) — NO TIE EXPECTED; refuted as an app defect.**
  6b computes 5,602 correctly and stably. The AGI shortfall is Schedule
  D: the FILED face itself doesn't foot — its printed components sum
  2,229 short of its printed line 15 (TaxWise carried +2,229 appearing
  on NO printed line, nowhere in the packet). Source-side data gap;
  waiting on Ken/Codex to locate the missing item. The app must not
  manufacture the tie. The new D_RET_012 invariant covers the original
  "6b drifts" suspicion (it never reproduced — 4 ORM shapes + the exact
  payload all stable).
- **Return W — TIES** (4b, 6b, AGI, RIE-TP-11/17, GA S1-7 all verified).
- **Return T (item 7) — TIES** (1e = 34, AGI 13,567).
- **Return G (item 8) — TIES numerically** (S1 line 7c routing + amount);
  the frozen payload predates the new DIS-DATE/TYPE fields, so
  D_GA500_7C_DETAILS (error) correctly demands them — Codex supplies
  them via `ga500_fields` on rerun.
- Post-deploy commands ALREADY RUN against the shared DB:
  `seed_ga500 --year 2025` (157 lines) + `seed_form_2441 --year 2025`.
- Regression home: `server/tests/test_four_return_unblock_s243b.py`
  (13, synthetic identities). Items 1–5 of the PENDING file untouched.

### ⭐ NEXT UNIT — **BATCH-001 #6: the Form 8862 multi-category expansion**
#8 shipped s243. Next in the open set (#2/#5/#6): #6 is the
best-scoped — the s241c build covers EIC only (Part II:
`tax_year_disallowed`, `eic_income_report_only`, days-in-US); the
item's remaining ask is CTC/ACTC/ODC (Part III), AOTC (Part IV), HOH
(Part V), qualifying-person details, and diagnostics gating a
previously disallowed credit per category. ⚠ The RS `8862` spec is a
draft collapsing each part to one boolean (s241b — RS agenda); build
from the 2025 face + i8862 per the information-return discipline.
⚠ Check `build_irs8862` + the IRS8862.xsd for the category members
(the MeF leg must grow with the model — the s227 generator rule).
Then #2 (AMT passive carryovers: per-activity input + AMT Carryover
Detail render + roll-forward + the 6251 feed — pools exist since
s235), then #5 (nonpassive rental routing — ⚠ FIRST re-read Ken's
2026-07-06 "diagnostic-only" REP ruling's exact scope; if the ruling
covers routing, it's ⛔ KEN). #4 NOL-parked; #10 the 1099-Q form unit.
After: BATCH-002's NOL-blocked computes, the RS agenda.

### What the 1040-X unit established (s242j-l — do not re-derive)
- The amendment lifecycle: `amendment` payload block (Part II explanation
  REQUIRED); resolver INVERSION (filed + baseline intact, refusals with
  remedies); Form1040X + `is_amended_return` written BEFORE
  `compute_return` (one pass fills A/B/C); baseline pinned byte-identical;
  the mark-filed sweep skips amendments BY NAME; `extract_return` refuses
  EVERY amended return at the top (per-form seams sit behind it).
- Georgia: **Form 500X is CURRENT** (2025 Rev. 07/21/25 — the "retired"
  premise was recall and wrong). SINGLE-COLUMN re-statement; the one
  mirror shift 500-L27 → 500X-L28 is pinned (`GA500_SOURCE_LINES` + a
  render bleed test). `Form500X` hangs on the GA-500 STATE return; GA-only
  amendments are legitimate; `compute_500x_result` is the single source;
  state returns freeze baselines at mark-filed. The 500X render: combs
  derived from the template's own dividers CALIBRATED against the 500's
  known-good table; explanation overflow → Statement page.
- The 1040-X face code is `1040-X`; seed `seed_1040x`; the RS spec was
  fully seeded 2026-06-25 (not a draft-trap case).
- Tests: `test_1040x_amendment_lane_s242j.py` (12),
  `test_ga500x_amendment_s242k.py` (14), `test_ga500x_render_s242l.py` (6).

### ⚠ s241's Form 5329 cross-check — still waiting on Sections A/B
Form 5329 line 36 takes "Form 8853 line 8" (Archer MSA distributions —
Section A/B territory). The s242u model is Section C only; the
line-36 cross-check stays unbuilt until an A/B carrier exists (parked
with Ken's s224 keyed-only ruling; revisit only on his direction).

### ✅ THE E-FILE GAP LIST IS EMPTY (as of s242z)
`IRS4797` (s242c) · `IRS8853` (s242x) · `IRS1116` (s242y) · amended MeF
(s242z). What remains refused at composition is NAMED per-case (see
each extract's refusal set), never a missing builder.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — 6 open** (2, 4, 5, 6, 8, 10);
  **BATCH-002 — 9/10 open as to COMPUTE only** (NOL-spec-blocked);
  **BATCH-003 — ✅ DONE 10/10, moved (s242t)** (#3 ✅ s242t, #6 ✅ s242q-s, #1 ✅ s242o, #8 ✅ s242m,
  #9 ✅ s242n, #10 ✅ s242p; the re-triage annex is the design record); **BATCH-004 — ✅ DONE
  10/10, moved (s242l)**; **BATCH-005 — ✅ DONE, moved (s242i).** Every
  worked file carries a result annex; read it first.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

---

## ⚠ Classes that MOVE existing returns or output on next recompute
- **⚠⚠ s243b MOVES THREE CLASSES (each a correction):** (1) any return
  where a BASIS-ONLY Form 8606 (no distributions/conversions/Roth
  distributions) coexists with IRA-path 1099-Rs — 4b regains the box-2a
  taxable it was wrongly zeroing, moving AGI, the SS worksheet, and GA
  RIE L11; (2) any return with employer DCB whose qualified expenses are
  BELOW the plan cap — the 2441 Part III exclusion now stops at line 16
  expenses, so 1e/1z/AGI rise (zero expenses → all benefits taxable);
  (3) GA render: an under-62 disability RIE now prints on Schedule 1
  line 7c/7f (was 7a/7d) with date/type, and returns lacking date/type
  fire the new D_GA500_7C_DETAILS error.
- **⚠ s243 MOVES GEORGIA RETURNS carrying a federal 2441 credit:** the
  GA-500 sync now fills CC-FED from Schedule 3 line 2, so line 20 gains
  the IND-CR 202 credit (a correction — those returns overstated GA tax
  by 50% of the federal credit). A keyed CC-FED is overwritten on the
  next MANUAL refresh unless overridden (the documented pull semantics);
  the auto path respects overrides.
- **⚠ s242z MOVES E-FILE OUTPUT on the amended class:** a valid amended
  return (record + baseline + explanation) now composes as a 1040-X
  submission instead of refusing; the invalid shapes still refuse, now
  each by name. No compute/render movement.
- **⚠ s242y MOVES E-FILE OUTPUT on the full-1116 class:** a full-path
  FTC return with a resolvable country now composes IRS1116 (previously
  refused outright); without a country it still refuses, now naming the
  remedy. No compute/render movement.
- **⚠ s242x MOVES E-FILE OUTPUT on one narrow class:** a return with a
  nonzero keyed 8e now REFUSES composition by name (it previously
  transmitted with no Form 8853 behind it — an S1-F1040-022 reject
  waiting to happen; the refusal is a correction). Returns with a
  single computing Section C insured now transmit the IRS8853 document.
  No compute/render movement.
- **s242w: NONE beyond new-row reach, plus one WARNING-class change:**
  a return with a keyed 8e and NO Form8853LTC rows keeps its D_SCH1_004
  warning (unchanged); the requirement is merely satisfiable now.
- **s242v: NONE beyond new-row reach.** `Form1099LTC` feeds no compute
  (diagnostics only); the duplicate-insured refusal narrows a class that
  has no rows yet; the SECTION_RELATED fix changes re-import behavior
  from a crash to the correct 409. Migrations 0310/0311 additive.
- **s242u: NONE beyond new-row reach.** `compute_8853_db` touches 8e only
  when a `Form8853LTC` row exists (none do) or the FORM_8853 memory row
  holds a value (it can't yet) — the zero-movement guarantee is pinned by
  a test. Migrations 0308/0309 are additive CreateModel + RLS.
- **⚠ s242q MOVES one narrow class**: a return whose Schedule D
  previously DISENGAGED (last capital item removed) had a stale 1040
  line 7 that BLOCKED line 16 — the disengage now clears the stale 7 and
  the tax computes. Every such return was WRONG before (blank tax);
  the change is a correction. 8814 feeds are new-row reach only.
- **s242p: NONE beyond new-row reach.** UPE engages only when
  K1UnreimbursedExpense rows exist (none do); every seam (Sch E totals,
  face rows, SE, QBI) returns the pre-existing figure at zero UPE.
- **s242o: NONE beyond new-fact reach.** The aggregates are None on every
  existing Taxpayer; the shared 2a source returns the identical per-row
  sum when no aggregate exists.
- **s242n: MOVES Form 7203 ending basis** on any return whose worksheet
  row carries the new generic charitable amount (none exist until keyed —
  new-row reach only). The 500X-rules registry conversion changes no
  behavior (same functions, dict-wrapped).
- **s242m: NONE.** Allowlist-only (staging accepts six W-2 clergy fields +
  one Taxpayer flag); the engine those fields feed is unchanged and
  engages only on `is_minister` rows.
- **s242k/l: NONE beyond new-row reach.** `Form500X` engages only when a
  row exists (none do until a payload/API creates one); the state-baseline
  capture at mark-filed is additive; the D_500X rules and the 500X render
  no-op without the row. Migrations 0299/0300 are additive CreateModel +
  RLS.
- **s242j: e-file output only** — any return with `is_amended_return=True`
  now REFUSES at composition (previously would have transmitted as an
  original — every such transmission was a double-filing hazard; the
  refusal is a correction). No compute/render movement: the amendment lane
  only writes when a payload carries the new `amendment` block.
- **⚠ s241o MOVES GEORGIA RETURNS carrying a 1099-PATR** (RIE L10 feed;
  L10 became a derived line — browser writes set `is_overridden`).
- **⚠ s241j MOVES DIAGNOSTICS**: post-2018 alimony instruments fire
  `D_SCH1_007` (error) — those returns are genuinely wrong today.
- **⚠ s241d/s241c MOVE E-FILE OUTPUT** on any return transmitting Form 8862 —
  every change a correction toward the truthful, narrower claim.
- Carried from s240: passive/PTP K-1 §1231 losses fire RED; a non-zero
  Schedule 1 line 4 refuses at MeF composition.
- Carried from s239: Roth 1099-Rs move 5a/5b → 4a/4b; GA partnership K-1s move
  RIE L2↔L13; code-U un-blanks the pension taxable column (largest mover).
- Carried from s236/s235: GA RIE line 13 on suspended passive K-1 losses;
  GA dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted
- **`--reuse-db` cross-module contamination**: `test_backentry_cleanup.py`
  (3, s225) and `test_mappings.py::TestApplyMappingAmbiguousFederalReturn`
  (3, s239).
- `test_topic7_input_leg.py::TestEICFacts::test_non_engaged_return_leaves_27a_quiet`
  — pre-existing (s235), not diagnosed.
- **`test_1040.py` — 6 pipeline tests**, unscoped `_fv` `.get()` (s234).
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  (s219). `test_4868.py` (4) — ⛔ KEN (s217).
  `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  PTET-gate class (s212).
- **⚠ PRE-EXISTING 1120-S defect (s241o)**:
  `test_line_key_registry_sweep.py::test_formula_targets_resolve` —
  `FORMULA_REGISTRY["1120-S"]` targets `M2_DIST_EXCESS` / `L24_BOOK_BRIDGE`,
  neither seeded, so the formula pass computes both and silently never
  persists them. 1120-S only. Deserves its own unit.
- **Client typecheck**: 55 error lines standalone (unchanged); vitest
  1,680 passed / 140 files.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB,
  cross-repo (`test_postgres`). A stalled background sweep beside foreground
  runs is contention, not a hang (s241o).
- A broad `-k` sweep blows the 600s timeout — background it; keep `-k` tight.
- `--create-db` does not reliably drop here; prove a pre-existing red via
  `git worktree` at a pristine SHA with the main venv + copied `server/.env`.
- A timed-out `pytest | Select-Object` loses ALL output — redirect to a file.
- `poetry run python > file` BUFFERS (use `-u`); stdout redirects go through
  cp1252 (write UTF-8 from inside Python); **never rewrite a UTF-8 file via
  `Set-Content`** — use the Write/Edit tools.
- **`poetry run` only works from `server\`**; Windows `python` cannot read the
  Bash tool's `/tmp` — use the scratchpad; DB probes: a throwaway
  `tests/test_zz_*.py` with `-s`, deleted after.
- Cloudflare-403 law sites and `rules.sos.ga.gov`: the in-app browser gets the
  text where WebFetch and curl fail (s239/s241o).
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes are `/api/v1/tax-returns/…` with the
  trailing slash; `filing_status` is `"mfj"`.
- `_finding(...)` kwargs land under `["details"]`; `ScheduleF` has no
  `business_name` (use `principal_activity`); `order_by("owner")` puts
  "spouse" before "taxpayer" (s241w).

### 🔎 Carried for triage — NOT claims
- **From s241o**: RIE L8 alimony underived; a fuller L10 derive possible in
  principle (all 8a-8z are FormFieldValues) but per-owner attribution and the
  (4)(b)2 gambling carve-out make it a design pass.
- **From s241**: `Form8606`/`HSAAccount` allow duplicate owners and their
  computes ITERATE (double-count, not vanish); browser POST unguarded.
- **From s234, potentially large**: a materially-participating 1120-S K-1's
  $250k nonpassive ordinary income never reached Schedule 1 line 5 / AGI.
  Repro: `server/tests/test_8960_line4b_clamp.py`, `_build(k1_ordinary="250000")`.
- Carried (s229): exact-tie 1040 shows `1040_SCHD_WS` clc_1/clc_3 drift on a
  bare recompute (−5,491 each), face still a tie.

### ✅ KEN DECISIONS OUTSTANDING
- **⛔ KEN (s231)**: §38(c)(6)(A) MFS threshold — flat $25,000 vs statutory
  $12,500; OVER-allows. Buildable without Ken; the ruling is the gate.
- **⛔ KEN (s227)**: the out-of-scope-state packet-disposition marker.
- **⛔ KEN (s230)**: Form 6765 Section G required for TY2026+ — re-author
  before a TY2026 season.
- **1040 v5.4 business rules still not in hand** (v5.4 schemas ARE on disk).
  ⚠ s240/s241w read the **v5.3** rules — re-check `S1-F1040-118-01` and the
  `SH-F1040-*` family against v5.4 on arrival.

### RS AGENDA
- **(s242x) The TEN staged FA definitions**: FA-1040-8853C-01..05 (titles
  authored s232, runnable definitions never written) + FA-4562-DEST-01 /
  FA-4562-ROUND-01 / FA-4562-280F-01 / FA-1040-2210-08 / FA-1040-2210-09
  (same state, older units). Author each definition in RS, re-export,
  move from `flow_assertions_1040_pending.json` to the gate mirror.
- **⛔ BLOCKING two batch items: NO NOL SPEC** (`172`/`NOL`/`FORM_172`/`1045`
  all 404). BATCH-001 #4 + BATCH-002 #10 wait. Preservation is built; only
  the computation waits. Highest-value authoring order on this list.
- (s242j, note): the `1040-X` RS spec was FULLY seeded at its unit
  (`load_1040_form_1040x.py`, integrity gate ALL PASS 2026-06-25) — NOT a
  draft-trap case; no re-authoring needed.
- (s241w): `SCHEDULE_H` is a DRAFT covering 7 of ~27 lines — re-author
  per-line from the brief; record the two worksheets and annual constants.
- (s241s): the GA QEE credit has NO SPEC. Author from the brief; MUST carry
  the two carryforward regimes.
- (s241p): `4547` and `8879_TA` have NO SPEC. Author from the brief; record
  `IND-476`.
- (s241o): the `500` spec still has NO rule for what feeds RIE lines 1/2/6-13
  — five defects and counting.
- (s241b): the `8862` spec is a draft collapsing each PART to one boolean —
  re-author per-line.
- Carried: `5329` roll-forward/Part VIII silence (s241); `R-8582-MULTIFORM`'s
  stale no-silent-gap cite + `4797` K-1 §1231 silence (s240); `R-RET-CODE`
  outrun three times (s239); `8379` draft (s238); `R-SCHA-CHARITABLE` three
  buckets vs seven K-1 codes + RIE-13 missing (s236); SCHEDULE_A carryover
  aggregation + `500` line 7a typed `input` (s235); R-STD-04 + `R-8960-INCOME`
  (s234); s232/s231/s230 items; the 1065 K-1 box-15 letters (still URGENT).

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
