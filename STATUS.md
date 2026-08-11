# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-11 (s242k). **BATCH-004 #1 — LEG 2 SHIPPED**
(`31eef58`): the Georgia Form 500X amendment. **Verify-first corrected
leg 1's premise: GA did NOT retire the 500X** — the 2025 Form 500X
(Rev. 07/21/25) is current per the live DOR check, and the 2025 Form 500
face carries no amended checkbox. The 500X is a SINGLE-COLUMN re-statement
of Form 500 (lines 8-26 mirror the corrected 500; Sch 2B refundable shifts
27→28) + the reconciliation lines (27 paid-with-original, 30 previous
refunds — ASKED facts, no per-line Column A exists). Built: SHA-pinned
face, `Form500X` model (migs 0299+0300), `compute_ga500x`
(single-source result), the `ga_*` amendment facts (no-GA-500 refuses by
name; a GA-500 on an amended return ALWAYS gets its 500X; explanation
falls back to federal Part II), state-baseline capture at mark-filed, four
D_500X rules. 14 tests + 620 neighbors green.*

*Previous (s242j): LEG 1 (`5f455c5`) — the lane amendment lifecycle +
the closed double-filing hazard (`extract_return` had no amended gate; an
amended return composed as a SECOND ORIGINAL — now refused by name).*

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

### ⭐ NEXT UNIT — **BATCH-004 #1 LEG 2b: the Form 500X render**
The item stays OPEN (annexes in `CC_CODE_CHANGES_1040_BATCH-004.md`).
Render the 5-page 2025 Form 500X (flat, coordinate-overlay — the `fga500`
pattern is the worked example, same page geometry family): pages 1
(identity + residency + the "Amended due to IRS Audit" checkbox +
dependents), 2-4 (lines 8-26 FROM THE CORRECTED GA-500 FACE + the
reconciliation lines from `compute_500x_result` — one source, never
re-derive), 5 (EXPLANATION OF CHANGES + signature block; explanation text
wraps). Wire into `render_complete` when the GA-500 carries a `Form500X`.
⚠ Verify value-box positions via the fitz→PNG visual loop like fga500;
⚠ check every checkbox pair positionally (s241w). Leg 3 after: amended
federal MeF (AmendedReturnInd + IRS1040X) — the extract refusal holds the
line. Then BATCH-003's six remaining items (⚠ build #3 with the s239
Georgia work), Form 8853 A/B+C, the IRS1116 e-file gap.

### What leg 2 established (s242k — do not re-derive)
- **Form 500X is CURRENT** (2025 Rev. 07/21/25; leg 1's "retired" claim
  was wrong and is corrected everywhere). SINGLE-COLUMN re-statement; the
  one mirror shift is Sch 2B refundable: 500 line 27 → 500X line 28
  (`GA500_SOURCE_LINES` pins it).
- `Form500X` hangs on the GA-500 STATE return (not the federal). Lines
  35-37 (UET/late-penalty/interest) are preparer-supplied model fields
  with NO lane carrier yet. A GA-ONLY amendment is legitimate — nothing
  demands the federal be amended.
- `compute_500x_result(ga500)` is the single source (lines/form/sources);
  line 39 floors at 0; D_500X_002 surfaces an over-election.
- State returns now get an as-filed baseline at mark-filed (both the
  sweep's `_mark_return_filed` and the direct-capture path in tests).

### What leg 1 established (s242j — do not re-derive)
- The 1040-X CORE PREDATES the item: `Form1040X` (amend-in-place OneToOne),
  `AsFiledBaseline` (frozen Column A; capture is idempotent-safe — a
  re-capture without `force` returns the existing snapshot unchanged),
  `compute_1040x` (A ← baseline, C ← live return, B = C − A; RED-defers by
  name: carrybacks, superseding, cascades, missing baseline), `rules_1040x`,
  the render map, the UI `form-1040x` endpoint. The 1040-X face code is
  **`1040-X`** (not FORM_1040X); seed command `seed_1040x`.
- The lane lifecycle: `amendment` block (Part II explanation REQUIRED),
  resolver inversion (filed + baseline, refusals with remedies), commit
  order (Form1040X + flag BEFORE `compute_return`), the sweep's named skip.
- `extract_return` refuses EVERY amended return at the top; per-form seams
  (4547 IND-476, 8888) sit behind it. Tests:
  `test_1040x_amendment_lane_s242j.py` (12).

### ⭐ STILL UNBLOCKED, still passed over — now TWELVE sessions
- **Form 8853 Sections A/B + Section C.** Spec cached; all four legs pending.
  Read the s232 write-up in STATUS_ARCHIVE first — Schedule 1 line 8e is
  COMPOSED not owned, line 25 FLOORS AT ZERO. ⚠ s241 gave it a second reason:
  Form 5329 line 36 takes "Form 8853 line 8" and no `Form8853` model exists.

### ⛔⛔ THE E-FILE GAP LIST
- **`IRS1116`** — the oldest live e-file gap. s238's `IRS8379` is the worked
  example end to end. (`IRS4797` CLOSED s242c.)
- **Amended MeF (IRS1040X + AmendedReturnInd)** — new, named at s242j;
  refused by name at extract until built (leg 3 of BATCH-004 #1).

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — 6 open** (2, 4, 5, 6, 8, 10);
  **BATCH-002 — 9/10 open as to COMPUTE only** (NOL-spec-blocked);
  **BATCH-003 — 6 open** (1, 3, 6, 8, 9, 10 — ⚠ build #3 together with the
  s239 Georgia work); **BATCH-004 — ONE open (#1, leg 1 of 3 done)**;
  **BATCH-005 — ✅ DONE, moved.** Every worked file carries a result annex;
  read it first.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

---

## ⚠ Classes that MOVE existing returns or output on next recompute
- **s242k: NONE beyond new-row reach.** `Form500X` engages only when a row
  exists (none do until a payload/API creates one); the state-baseline
  capture at mark-filed is additive; the D_500X rules no-op without the
  row. Migrations 0299/0300 are additive CreateModel + RLS.
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
