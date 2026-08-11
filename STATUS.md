# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-11 (s242v). **✅ FORM 8853 SECTION C LEG 2 SHIPS
(`3b33982`, migs 0310/0311)** — the 1099-LTC document + all 12 spec
diagnostics. `Form1099LTC` (one row per received doc; box3_basis with
an explicit "unchecked"; boxes 4/5 NULLABLE — absence is never an
answer on optional boxes, the brief's central trap); `rules_8853.py`
dict-registry (5 errors = the compute refusals gone loud + 7 warnings
incl. four 1099-LTC cross-checks matched by insured TIN/name); lane
`ltc_1099s`. **Two leg-1 gaps found and closed**: (1) two Section C
rows for ONE insured (both index 1) computed and BOTH contributed —
now all rows of a duplicated insured refuse (multi-period, the spec's
second arm); (2) `form_8853_ltcs` never joined SECTION_RELATED — fresh
imports worked, any RE-import would KeyError; fixed + a structural
test now pins every LIST_SECTIONS member to a SECTION_RELATED entry.
19 new tests (fire AND quiet per diagnostic); leg-1 24 + 526 flow +
160 lane neighbors green; manage.py check clean. Leg 1 was s242u
(`ab5ed8e`, migs 0308/0309): model + compute + the composed-8e
delta-adjustment with engagement memory, zero-movement pinned.*

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

### ⭐ NEXT UNIT — **Form 8853 Section C leg 3: RENDER**
Legs 1-2 ✅ (s242u `ab5ed8e`, s242v `3b33982`). Leg 3 = the printed
face: **f8853 is NOT in `forms_manifest.json`** — register the 2025
face with its SHA256 (`5582f813…`, recorded in the s232 STATUS_ARCHIVE
write-up), dump the AcroForm fields, build the Section C field map
(lines 14a-26), wire `render_8853` into render_complete. ⚠ THE PARTIAL
POPULATIONS ARE THE TRAP (s232): three of four filing populations
complete Section C only PARTIALLY — out-of-set lines render BLANK, not
zero (the ADB-only path skips 17-25 entirely; a refused row renders
its inputs but no computed lines). ⚠ One page per insured (the 8814
per-child precedent); the "more than one Section C" checkbox derives
from the row count. ⚠ Delete `test_form_manifest.py`'s pinned comment
*"Form 8853 — never generated"* — that pin is the build's acceptance
criterion. ⚠ The AcroForm filler STRIPS widgets — checkbox tests
assert drawn "X" glyphs positionally (the s242r lesson). Then leg 4
MeF (grep the business-rules CSV for `F8853-*` FIRST; the 8e element
is `TotArcherMSAMedcrLTCAmt`; R-8853C-ATTACH wants render + MeF).
After: IRS1116, amended MeF, BATCH-001's six, NOL-blocked (parked).

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

### ⛔⛔ THE E-FILE GAP LIST
- **`IRS1116`** — the oldest live e-file gap. s238's `IRS8379` is the worked
  example end to end. (`IRS4797` CLOSED s242c.)
- **Amended MeF (IRS1040X + AmendedReturnInd)** — named at s242j; refused
  by name at extract until built (BATCH-004 #1 closed WITHOUT it — e-file
  was not in the item's ask; this is now a standing e-file gap like
  IRS1116, with the refusal holding the line against double-filing).

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
