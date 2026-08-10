# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-10 (s241b). **BATCH-004 #6 BUILT — the `education_students`
lane.** Form 8863 was complete except its importer; the notable part is that the
uniqueness constraint s241 had just added to Form 5329 would have been a DEFECT
here, and checking rather than pattern-matching is what caught it. One deploy
(`d55ff15`), no migration.*

*Previous (s241): BATCH-004 #7 + BATCH-003 #2 built together as one `form_5329s`
section; a duplicate-owner row made $110.40 of tax vanish (`97ea4a5`, migration
0278). (s240): BATCH-004 opened + triaged 10/10, #4 built (`c6aab19`).*

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

### ⭐ NEXT UNIT — BATCH-004 #8 (Form 8862), then the rest of the queue
**#8 is the smallest remaining item.** `f8862_2025.py` (render) + the `IRS8862`
MeF document + `seed_8862.py` all exist — this is the SPRINT_SCOPE Topic-7
"data-mapped render, no compute" boundary. **What is missing is the input model
itself**, and then the lane family on top of it. So unlike #4/#6/#7 this is a
genuine build, not a lane-only gap: a model + migration + RLS (the new-table
rule) + compute/validation + the `backentry.v1` section.

⚠ Read the s241/s241b pair before starting — **the two sessions reached OPPOSITE
answers on the same question**, and that contrast is the reusable lesson:
- Form 5329's compute reduced its rows to `{r.owner: r for r in ...}`, so one
  row per owner was a real contract the DB did not enforce → constraint added.
- Form 8863's compute ITERATES a list and Parts I/II aggregate, so several rows
  are normal → **a constraint would have been a defect**, and a test now pins
  that compute still iterates so the reasoning cannot be silently outrun.
**Check the consumer before deciding uniqueness. Do not pattern-match.**

⚠ Also carried from both: refuse derived/aggregate columns **BY NAME**
(s238 doctrine, now applied three times), and use the `link_key` carrier shape
for any FK a packet cannot know as a UUID.

The rest, roughly by size: #3 pre-2019 alimony ≈ #9 Form 1099-PATR ≈ #10 Form
4547 + 8879-TA ≈ #2 GA education credit + IT-QEE-TP2 ≈ #5 Schedule H << #1
1040-X amended lifecycle (large).

⚠ **#10's source check is CLOSED — Form 4547 is REAL** (Rev. December 2025,
created by OBBBA; filed WITH the current-year e-filed return, so its MeF leg is
part of the build). ⚠ **#9 does NOT go through the 404-STOP gate** — per s222 no
RS spec exists for any information return.

### ⭐ STILL UNBLOCKED, still passed over — now NINE sessions
- **Form 8853 Section C.** Spec cached at `server/specs/8853_sec_c_spec.json`;
  `lookup/8853_SEC_C/export/` returns 200; all four legs pending. Read the s232
  write-up in `STATUS_ARCHIVE.md` first — Schedule 1 line 8e is COMPOSED not
  owned, and line 25 FLOORS AT ZERO though the printed face does not say so.
  ⚠ **s241 gave this a second reason to happen**: Form 5329 line 36 takes its
  value "from Form 8853, line 8" and **no `Form8853` model exists at all**, so
  the Archer arm of the 5329 excess chain cannot be reconciled the way the HSA
  arm now is (`D_5329_006`). Sections A/B and Section C are near neighbours —
  doing them in one pass is the cheap order.

### ⛔⛔ THE E-FILE GAP LIST — still TWO named documents, unchanged
- **`IRS4797` (s240)** — no 1040-side Form 4797 document builder; MeF rejects
  the return without it (`S1-F1040-118-01`, Reject, Active). s240 added a
  refusal so it fails loudly at composition. **The higher-volume of the two** —
  every disposition, installment sale, like-kind exchange AND K-1 §1231 return.
  The 1120-S `IRS4797` builder is the worked example.
- **`IRS1116`** — the oldest live e-file gap. A full-path Form 1116 return is
  paper-only in code. s238's `IRS8379` build is a worked example end to end.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — 6 open** (2, 4, 5, 6, 8, 10);
  **BATCH-002 — items 9 and 10 open as to their COMPUTE half only** (both
  RS-blocked on the missing NOL spec); **BATCH-003 — 6 open** (1, 3, 6, 8, 9,
  10 — ⚠ build #3, mixed passive/nonpassive on one K-1, TOGETHER with the s239
  Georgia work); **BATCH-004 — 8 open, all triaged**. Every file carries a
  result annex naming what is done and what is blocked; read it before
  starting. ⚠ None of the four has moved to Done, deliberately.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

### ✅ s241b in one paragraph
BATCH-004 #6 built: `education_students`, one row per Form 8863 Part III
student. Refuted as stated, real as a lane gap — the form has been complete
since migration 0071 and only the importer was missing. **The finding worth
carrying is a negative one**: the uniqueness constraint s241 had added to Form
5329 hours earlier would have been a *defect* here, because `compute_8863`
iterates its rows and Parts I/II aggregate across students. Checking the
consumer rather than pattern-matching the previous session is what caught it,
and a test now pins that compute still iterates. Also settled that
`student_name`/`student_ssn` live on the row rather than on the dependent link
(MeF refuses a student without them), and built the `dependent_key` carrier for
the student-to-dependent link. §25A answer key hand-derived and tied: $2,500 →
$1,000 refundable + $1,500 nonrefundable.

### ✅ s241 in one paragraph
Two batch items turned out to be one section. BATCH-004 #7 (Form 5329 Part III,
traditional-IRA excess) and BATCH-003 #2 (Part VII, HSA excess) are the same
`_excess_part()` helper inside one function, and both were lane-only gaps — the
engine has computed Parts I–IX and routed both owners' totals to Schedule 2 line
8 since migration 0119. Built `form_5329s` carrying the leaves of **all nine
parts**, with `owner` required and every derived line refused by name; built the
excess roll-forward the item asked for; added `D_5329_006` reconciling line 44
against the return's own Form 8889 line 16. Behind them sat a defect nothing
could report. Both answer keys tie to the filed returns ($110 and $15).

### ⚠⚠ THE FINDING WORTH CARRYING — a dict keyed by a row attribute makes duplicates VANISH, not double
`compute_5329_db` and the diagnostics' `_f5329_state` both do
`{r.owner: r for r in Form5329.objects.filter(...)}`. That is an ordinary,
readable line of Python, and it silently encodes a uniqueness contract the
database never enforced. A second row for the same owner does not double-count —
**the last one wins and every earlier row's additional tax is dropped**, with no
error, no diagnostic and no rendered form. Measured before the fix: a taxpayer
row with $1,840 of prior traditional-IRA excess plus a second taxpayer row with
$250 of prior HSA excess wrote Schedule 2 line 8 = **$15.00 instead of $125.40**.
*The general shape: when compute reduces a row SET to a dict keyed by one field,
that key is a uniqueness constraint — and if the DB does not have it, the
overflow is invisible rather than wrong.* The contrast is the tell: this form's
own siblings, `Form8606` and `HSAAccount`, ITERATE their rows, so a duplicate
there at least shows up as a double count someone would notice. ⚠ **Sign:
UNDERSTATES tax** — the direction nobody reports. Now closed at the DB
(migration 0278), the browser POST, the re-owner PATCH, and staging.
⚠ An independent confirmation nobody had connected: `ReturnData1040.xsd` caps
`IRS5329` at **maxOccurs 2** — the schema has said "one per owner" all along.

### ⚠ THE SECOND ONE — a form that names its own source is telling you what to reconcile
Form 5329 line 44 reads, on the printed face, *"2025 distributions from your
HSAs **from Form 8889, line 16**"*. Line 16 is the TAXABLE portion; a fully
qualified distribution puts -0- there. The reported packet has a fully qualified
$631 distribution, so keying the gross on line 44 wipes out the $250 carried
excess and its $15 tax — **and nothing visible changes on the face, because the
part just prints as zero**. Where a form line states its source in words, that
sentence is a reconciliation the app can run; `D_5329_006` runs it. ⚠ Line 36
says the same thing about Form 8853 line 8 and **cannot be reconciled — there is
no Form8853 model** (DEFERRAL_AUDIT).

### ⚠⚠ THE s241 / s241b PAIR — the same question, OPPOSITE right answers
s241 found that `compute_5329_db` reduced its rows to `{r.owner: r for r in …}`,
making a duplicate row VANISH, and closed it with a DB constraint. Hours later
s241b faced the identical question on `EducationStudent` — and the right answer
was the reverse: `compute_8863` **iterates** a list and Parts I/II aggregate
across students, so several rows per return is the normal case and a constraint
would have destroyed the multi-student return. *The transferable rule is not
"add the constraint" — it is **read what the consumer does with the row set,
every time**. A dict-by-attribute is a uniqueness contract; a list is not, and
the two are one character apart at the call site.* s241b pins the premise with a
test asserting `compute_8863_db` still iterates, so a future refactor to a dict
fails loudly instead of silently dropping a student's credit.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **s241b: NONE.** No compute changed; `education_students` rows only exist
  where a payload or preparer creates one.
- **s241: NONE.** No compute changed. `form_5329s` rows only exist where a
  payload or preparer creates one; `D_5329_006` fires only where a Form 5329 row
  and an HSA for the same owner already disagree; the roll-forward touches only
  a NEW year's return being seeded. The one blocking change (the duplicate-owner
  refusal) reaches only a state that was already producing a wrong number.
- Carried from s240: a passive or PTP K-1 carrying a net §1231 LOSS now fires
  `D_8582_MULTIFORM` (RED) or `D_K1_PTP_LOSS` (warning) where it was silent; any
  1040 with a non-zero Schedule 1 line 4 now refuses at MeF composition.
- Carried from s239: Roth 1099-Rs (codes J/T/Q) move from 5a/5b to 4a/4b; any
  Georgia return with a partnership K-1 moves income between RIE L2 and L13;
  an engaged Form 8606 changes RIE L11; a code-U 1099-R un-blanks the whole
  pension taxable column (the largest mover).
- Carried from s236: a Georgia return with a passive K-1 whose loss was partly
  or wholly suspended gets a different RIE line 13.
- Carried from s235: Georgia dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted
- **~24 test files** assert registry wiring via
  `inspect.getsource(seed_builtin_rules)`; s233 refactored that function to
  iterate `all_registries()`, so these are **false REDs**. Hit again this
  session in `test_form8889_diagnostics_leg.py::test_runner_registers_8889`.
  **This is now its FIFTH consecutive session and it is the single most frequent
  false-red in the repo.** Still spun off as its own task — the fix is
  mechanical (assert against `all_registered_rules()` instead of source text)
  but touches ~24 files, which is why it keeps being deferred. It should stop
  being deferred.
- **`--reuse-db` cross-module contamination**: `test_backentry_cleanup.py` (3,
  s225 — a `D_PREPARER_001` duplicate-key on rule seeding; fails alone too) and
  `test_mappings.py::TestApplyMappingAmbiguousFederalReturn` (3, s239).
- **`test_topic7_input_leg.py::TestEICFacts::test_non_engaged_return_leaves_27a_quiet`**
  — pre-existing, verified at pristine `6e819b5` in a worktree (s235). Not diagnosed.
- **`test_1040.py` — 6 pipeline tests**, `MultipleObjectsReturned`. Their `_fv`
  helper does an **unscoped** `FormFieldValue.objects.get()`. Pre-existing (s234).
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  `ValidationError`. Pre-existing (s219). Not diagnosed.
- `test_4868.py` — 4 tests on the Schedule 3 line-10 feeder (s217). ⛔ KEN.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  batch-005 #9 PTET-gate class, red since s212.
- ✅ **FIXED in s241 (they had been red and unlisted):** the two `D_RET_`
  registration counts in `test_topic5_compute_leg.py` and
  `test_topic5_diagnostics_leg.py` — hand-counted `== 10`, RED since s239 added
  D_RET_011 without re-pinning them. Both now read the roster from
  `RULES_RETIREMENT`. Also the proforma producer's key-contract guard, which had
  never gained `_k1_basis_704d` (s228) or `_carryforward_attributes` (s235) and
  was passing only because its fixture emits neither.
- **Client typecheck**: 55 error lines standalone. s241 touched no .tsx.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). `--reuse-db`.
- **A broad `-k` sweep is SLOW and blows the 600s Bash timeout** — s236's
  ~1,100-test sweep took **21½ minutes**. Run it with `run_in_background: true`.
  ⚠ Keep the `-k` terms tight: `k1` matches hundreds of node ids, `rie` matches
  "ret**rie**ve". (s241's `-k "retirement or 5329 or 8889 or backentry or
  schema"` = 685 tests / 7m38s — fine, and it ran in the background.)
- ⚠ `--create-db` does NOT reliably drop an existing test DB here. To prove a
  red is pre-existing, use a `git worktree` at a pristine SHA with the MAIN
  venv's interpreter, and copy `server/.env` in (s235).
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout
  loses ALL output — redirect to a file and tail it.
- ⚠ A `poetry run python script > file` redirect BUFFERS. Use `-u`.
- ⚠ A stdout **redirect goes through cp1252** on this box and dies on ligatures
  and the U+2212 minus. Write UTF-8 from inside Python.
- ⚠ `manage.py shell -c "..."` prints nothing; a multi-line `python -c` through
  the Bash tool also silently produces no output. **A script file under the
  scratchpad also fails — `config` is not importable from there.** Copy the
  script INTO `server\`, run it, delete it (s239). **Simplest reliable probe: a
  throwaway `tests/test_zz_*.py` with `-s` print statements, deleted after
  (s240/s241) — it gets fixtures, the DB and the app context for free. s241 used
  exactly this to MEASURE the duplicate-owner drop before fixing it.**
- ⚠ **`poetry run` must be invoked from `D:\dev\delvio-tax\server`** — from the
  repo root it fails with "could not find a pyproject.toml". This bites the
  schema generator too: run it as `poetry run python -u ..\scripts\gen_backentry_schema.py`
  FROM `server\`, never from the repo root.
- ⚠ **Windows `python` cannot read the Bash tool's `/tmp`** — they are different
  filesystems. Write shared files to the scratchpad path, not `/tmp` (s240).
- ⚠ The Bash tool produced NO output at all for a `poetry run python -c` this
  session where the identical PowerShell command worked. **Prefer PowerShell for
  `poetry run` on this box** (s241).
- ⚠ A Cloudflare-protected law site (justia) 403s both WebFetch and curl.
  **The in-app browser (`preview_start` + `get_page_text`) got the full
  verbatim Georgia reg** where both failed (s239).
- ⚠ Lane API shapes that cost time (s241): **staging answers 201 even for an
  invalid payload** — the verdict is `row["status"]`, not the HTTP code; the
  return CRUD routes are `/api/v1/tax-returns/…` (not `/returns/`) and the
  detail route **needs its trailing slash** or you get a 301; `filing_status`
  is `"mfj"`, not `"married_joint"` (varchar(10)).

### 🔎 Carried for triage — NOT claims
- **From s241**: `Form8606` and `HSAAccount` both allow duplicate owners and
  their compute ITERATES, so a duplicate DOUBLE-COUNTS rather than vanishing.
  `views.py:623` (the 8606 proforma roll) guards with `.exists()`, but the
  browser POST does not. Not measured; not a claim. The 5329 constraint is the
  worked example if it turns out to be real.
- **From s234, potentially large and still unchased**: a materially-participating
  1120-S K-1 carrying **$250,000 of nonpassive ordinary business income never
  reached Schedule 1 line 5 or 1040 AGI** — line 9 was identical with the K-1 at
  $0 and at $250,000 — with `SCHEDULE_E` AND `FORM_8582` both seeded, while
  `schedule_e_non_1411_income` *did* see the same row. Repro:
  `server/tests/test_8960_line4b_clamp.py`, `_build(k1_ordinary="250000")`.
- Carried (s229): a filed, exact-tie 1040 shows worksheet drift on a bare
  recompute (`1040_SCHD_WS` clc_1/clc_3, −5,491 each), face still an exact TIE.

### ✅ KEN DECISIONS OUTSTANDING
- **⛔ KEN (s231, carried): §38(c)(6)(A), the MFS threshold.**
  `compute_3800.SEC38C1_THRESHOLD` is a flat $25,000; the statute makes it
  **$12,500** for an MFS taxpayer whose spouse has any business credit.
  **⚠ The sign: this OVER-allows.** Requires nothing from Ken to BUILD.
- **⛔ KEN (s227)**: the out-of-scope-state packet-disposition marker.
- **⛔ KEN (s230)**: Form 6765 Section G becomes REQUIRED for tax years
  beginning after 2025; the RS spec must be re-authored before a TY2026 season.
- The one live external item: **1040 v5.4 business rules still not in hand,
  active 2026-08-10** (v5.4 schemas ARE on disk; 1041 v5.5 closed). ⚠ s240 read
  the **v5.3** rules for `S1-F1040-118-01`; re-check it against v5.4 on arrival.

### RS AGENDA
- **⛔ BLOCKING two batch items: THERE IS NO NOL SPEC.** `lookup/172/`,
  `lookup/NOL/`, `lookup/FORM_172/` and `lookup/1045/` all return 404.
  BATCH-001 #4 and BATCH-002 #10 both ask for Form 172, the post-2017
  limitation and the utilization ordering. **The preservation half is built and
  the pools are safe — only the computation waits.** Still the single
  highest-value RS authoring order on this list.
- **NEW (s241): the `5329` spec says nothing about the roll-forward or about
  Part VIII.** Two gaps. (a) The spec has no rule stating that each part's
  total-excess line becomes next year's prior-excess line — s241 built the roll
  from the printed FACE's own wording (line 9 = "line 16 of your 2024 Form
  5329"), which is sound but unspecced. (b) **Part VIII (ABLE) has line 50 and
  no prior-year line at all**, so the form provides no chain — but whether an
  uncorrected ABLE excess is in fact taxed again the following year is a §529A
  question the FORM does not answer. s241 deliberately did not guess; the roll
  omits Part VIII and a test pins the omission. **Author the answer.**
- **NEW (s241): the `5329` spec does not state where lines 36 and 44 come
  from.** Both name a source ON THE PRINTED FACE — line 44 "from Form 8889, line
  16", line 36 "from Form 8853, line 8" — and neither has a spec rule, so
  nothing declared that line 44 is the TAXABLE distribution rather than the
  gross. That silence is the whole of `D_5329_006`'s defect.
- Carried (s240): `R-8582-MULTIFORM`'s no-silent-gap clause is now FALSE as
  written (it cites `D_K1_SEC1231`, retired 2026-06-30) and should be
  re-authored; `R-8582-WS-NET` says nothing about which OTHER forms an
  activity's losses can land on; the `4797` spec has no rule for the K-1 §1231
  feed, so nothing declares whether the amount is pre- or post-§469.
- Carried (s239): `R-RET-CODE` has been outrun three times (codes 6, W, U) —
  re-author from the current i1099-R Table 1 in one pass. The `500` spec still
  has NO rule governing what feeds RIE lines 1/2/6-13, where four defects have
  now been found; record the earned/unearned split BY ENTITY TYPE and that the
  $5,000 earned cap is statutory.
- Carried (s238): the `8379` spec is a DRAFT whose `line_map` covers 4 of ~20
  lines and returns 200, so the 404-STOP gate waved it through. **The export's
  `status` field is still not checked anywhere.**
- Carried (s236): `R-SCHA-CHARITABLE` models only three buckets while the K-1
  states **seven** — 1120-S box 12 codes A–G — so **B, D, F and G have no home
  and are REFUSED at both write paths**. ⚠ Sign: a refused code is not deducted
  AT ALL — it overstates tax.
- Carried (s236): the `500` spec's RIE line_map has no **RIE-13**.
- Carried (s235): the `SCHEDULE_A` charitable carryover modelled as ONE
  aggregate (BATCH-002 #9 needs pools by source year AND limitation class); the
  `500` spec typing line 7a as `input` when the app now DERIVES it.
- Carried (s234): R-STD-04 silent on the dependent worksheet's line 1; the
  FORM_8960 `R-8960-INCOME` rule silent on line 4b being bounded by line 4a.
- Carried (s232): the `[WO-SOURCETYPE-RECON]` additions; the export serializer
  omitting `requires_human_review`.
- Carried: the s231 Form 3800 five spec defects; the s230 Form 6765 items
  (a)-(e); the **1065 Schedule K-1 box-15 letters** for §41 and §45R (still
  URGENT — `D_3800_008` excludes those credits until both are verified);
  s228 `D_K1B_FULLY_ALLOWED`; s226/s227/s224/s223 unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
