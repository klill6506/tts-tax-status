# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-08 (s235). **The attribute-preservation layer is BUILT**
— the five-item shape STATUS named as the next unit — plus 1040 BATCH-002 #7
(dependent relationships + the Georgia 7a derive). Two deploys. BATCH-002 has
4 items open, BATCH-001 has 6.*

*Previous (s234): BATCH-002 items 2 and 5 — dependent standard deduction and
the Form 8960 line-4b clamp (`8be7ba9` · `fdfeab0` · `f7de7e4`).*

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

### ⭐ NEXT UNIT — 1040 BATCH-002 #3, then #8
**#3** is a third GA RIE partnership-loss owner case, directly adjacent to the
s233 trio and the s235 GA-500 work — two partnership activities on the SAME
EIN, one a taxpayer nonpassive loss allowed on Schedule E and one a spouse
passive loss fully deferred by Form 8582, where the filed worksheet includes
the $2,948 on the spouse retirement line rather than netting it. $306 of
Georgia tax. **#8** is the S-corp K-1 charitable deduction flowing once to
Schedule A and to Form 7203 Part III — `schedule_k1s` has no charitable field,
so a flat Schedule A entry ties the face while overstating ending stock basis.

Then the two large ones: **#4 Form 1116 FTC** and **#6 Form 8379 injured
spouse**. Both are whole-form builds, and neither needs Ken.

### ⭐ ALSO STILL PENDING AND STILL UNBLOCKED — the Form 8853 Section C build
Unchanged from s232, untouched for three sessions. Spec cached at
`server/specs/8853_sec_c_spec.json`; deployed `lookup/8853_SEC_C/export/`
returns 200. All four legs pending, none needs Ken. **The two things the build
must not get wrong** (Schedule 1 line 8e is COMPOSED, not owned; line 25 FLOORS
AT ZERO though the face does not say so) are written up in full in
`STATUS_ARCHIVE.md` under s232 — read that before starting.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — 6 open** (2, 4, 5, 6, 8, 10; 4
  and 10 partly resolved by the preservation layer — see its addendum annex)
  and **BATCH-002 — 4 open** (3, 4, 6, 8). Both carry result annexes naming
  exactly what is done and what is blocked; read them before starting.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

### ✅ s235 in one paragraph
Built the unit STATUS named: **`CarryforwardAttribute`, one row per POOL** keyed
by kind + source year + limitation class + owner, with a back-entry section, a
browser CRUD surface, per-vintage roll-forward, five `D_CFWD_*` rules and a
worksheet statement page. It closes the *preservation* half of BATCH-001 #4/#10
and BATCH-002 #1/#9/#10 — a face tie can no longer discard an NOL vintage, a
§179 carryover, a charitable pool or a Coverdell basis. **BATCH-002 #1 was
verified first and found to be a LANE-ONLY gap**: the engine has owned Form
4562 lines 9-13 all along and proforma already rolled line 13 forward; only the
batch lane could not carry line 10. Then **#7**, where the reported cause was
half right — the missing `parent` relationship was real, but $4,000 × 5.19% =
$207.60 identified the actual defect, which is that **Georgia Form 500 line 7a
was never derived from anything**, so every imported GA return silently carried
zero dependents regardless of relationship.

### ⚠⚠ THE FINDING WORTH CARRYING — a blacklist is correct only until the next value
The CTC qualifying-child relationship test was `relationship not in ("",
"other")`. That was correct **by luck**: every value then in
`RELATIONSHIP_CHOICES` happened to satisfy IRC §152(c)(2). Adding the MeF
enum's missing codes — PARENT, GRANDPARENT, AUNT, UNCLE, all §152(d)(2)
qualifying *relatives* — would have let **a dependent aunt under 17 take a
$2,200 Child Tax Credit**, silently. The rule is now the statute enumerated as
a whitelist, and **all three copies** read it (`compute_8812`, `credit_gates`,
`rules_8812`). **The lesson: a negative test over an open enum is a bug with a
delayed trigger — it fails on a value that does not exist yet, so nothing can
catch it until someone adds one.** Corollary already logged twice (s215, s225):
when a guard exists, grep for its twins before assuming one copy is the rule.

### ⚠⚠ THE OTHER ONE — the arithmetic named the cause the report did not
$1,877 − $1,670 = $207, and $4,000 × 5.19% = $207.60. One exemption, not a
relationship-specific rule. **Doing the arithmetic before reading code** (the
s216 lesson) turned a narrow "add `parent`" fix into the discovery that line 7a
had never been fed on any return.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **Georgia returns with federal dependents and an untouched 7a** gain $4,000
  of exemption per dependent. ⚠ Sign check: this LOWERS Georgia tax —
  correctly. A typed or batch-sent 7a does not move (`is_overridden`).
- **No federal movement** from the relationship work: every relationship that
  could already be keyed satisfies §152(c)(2), so no existing return's CTC
  changes.
- **Nothing moves from the preservation layer** — the pools feed no tax line
  (pinned by a test), and the §179 lane fields are new inputs.
- Carried from s234: dependent-filer standard deduction; the proforma §111
  snapshot; Form 8960 engaging where it previously vanished.

### ⚠ Known red / rotted (NONE of it from this session)
- **`test_topic7_input_leg.py::TestEICFacts::test_non_engaged_return_leaves_27a_quiet`**
  — a childless EIC of $312 engages on a bare $15,000 W-2 where the test expects
  line 27 blank. **NEW this session, and verified identical at pristine
  `6e819b5` in a throwaway worktree** — pre-existing, not from s235. Not
  diagnosed: it is either a real engagement defect or a stale test.
- **`test_1040.py` — 6 pipeline tests**, `MultipleObjectsReturned`. Their `_fv`
  helper does an **unscoped** `FormFieldValue.objects.get()` that collides with
  sibling-form rows. Pre-existing (s234, verified at `8500744`).
- **24 test files** assert registry wiring via
  `inspect.getsource(seed_builtin_rules)`; s233 refactored that function to
  iterate `all_registries()`, so these are **false REDs** — the registries are
  still registered. Spun off as its own task.
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  `ValidationError`. Pre-existing (s219). Not diagnosed.
- `test_4868.py` — 4 tests on the Schedule 3 line-10 feeder (s217). ⛔ KEN.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  batch-005 #9 PTET-gate class, red since s212.
- **DiagnosticRule unique-code contamination** (s225): `test_backentry_cleanup.py`
  red under `--reuse-db`.
- **Client typecheck**: 55 error lines standalone. s235 touched one .tsx (the
  relationship option list); its own vitest suite is green (35 tests).

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). `--reuse-db`.
- **New (s235)**: a `-k` sweep over ~900 tests takes ~11 minutes and will blow
  the 600s Bash timeout — run it with `run_in_background: true` and collect it
  with `TaskOutput`, and do NOT start a second pytest while it runs.
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout
  loses ALL output — redirect to a file and tail it.
- ⚠ A `poetry run python script > file` redirect BUFFERS. Use `-u`.
- ⚠ A stdout **redirect goes through cp1252** on this box and dies on ligatures
  and the U+2212 minus. Write UTF-8 from inside Python.
- ⚠ `manage.py shell -c "..."` prints nothing; a multi-line `python -c` through
  the Bash tool also silently produces no output. Write a script file.
- ⚠ The Bash tool's cwd PERSISTS across calls — use absolute `cd` each call.
  `/tmp` in the Bash tool and the Windows Python interpreter do NOT resolve to
  the same place — write scratch files to the scratchpad path.
- To test whether a red suite is pre-existing, `git worktree add` a detached
  HEAD and run it with the MAIN venv's interpreter
  (`.venv/Scripts/python.exe -m pytest`). **New (s235): the worktree also needs
  `server/.env` copied in** — `base.py` reads `SECRET_KEY` from the environment
  and dies at import without it. Remove the worktree afterwards; that deletes
  the copied `.env` with it.

### 🔎 Carried for triage — NOT claims
- **From s234, potentially large and still unchased**: a materially-participating
  1120-S K-1 carrying **$250,000 of nonpassive ordinary business income never
  reached Schedule 1 line 5 or 1040 AGI** — line 9 was identical with the K-1 at
  $0 and at $250,000 — with `SCHEDULE_E` AND `FORM_8582` both seeded, while
  `schedule_e_non_1411_income` *did* see the same row. Either the fixture omits
  a required field, or a K-1's nonpassive net is not reaching AGI. The second
  reading would dwarf anything in the queue. Repro:
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
- **Carried (s233), low-friction**: the CC-Changes **batch numbering collided**
  — a second, unrelated `BATCH-001` was posted while the s227 `BATCH-001` sits
  in Done. Worth telling Codex to skip to `BATCH-003`.
- The one live external item: **1040 v5.4 business rules still not in hand,
  active 2026-08-09** (v5.4 schemas ARE on disk; 1041 v5.5 closed).

### RS AGENDA
- **⛔ NEW (s235), BLOCKING two batch items: THERE IS NO NOL SPEC.**
  `lookup/172/`, `lookup/NOL/`, `lookup/FORM_172/` and `lookup/1045/` all return
  404. BATCH-001 #4 and BATCH-002 #10 both ask for Form 172, the post-2017
  limitation and the utilization ordering; the 404-STOP gate forbids
  improvising them. **The preservation half is built and the pools are safe —
  only the computation waits.** A `FORM_172` spec is the single highest-value
  RS authoring order on this list.
- **NEW (s235)**: the `SCHEDULE_A` spec's `R-SCHA-CHARITABLE` models the
  carryover as ONE aggregate (`scha_charitable_carryover_in` /
  `..._carryover_out`). BATCH-002 #9 needs pools by source year AND §170(b)(1)
  limitation class, with §170(d)(1)(A) five-year expiry and the higher-%-first
  ordering. The storage exists; the rule needs amending before the compute can
  be built.
- **NEW (s235)**: the `500` spec types line 7a as `line_type: "input"`
  (`g_num_dependents`). The app now DERIVES it from the federal Dependent rows,
  preparer-overridable — the same amendment `g_lic_children` already carries.
  Record it, with the O.C.G.A. §48-7-26(a)/(b)(3) basis.
- Carried (s234): R-STD-04 is silent on where the dependent worksheet's line 1
  comes from; the FORM_8960 `R-8960-INCOME` rule says nothing about line 4b
  being bounded by line 4a.
- Carried (s233): `compute_ga500.GA_RIE_EARNED_CAP` $5,000 vs the reg's
  $4,000 — one re-verification against current §48-7-27(a)(5); and the GA-500
  spec's missing RIE rules belong in `R-GA500-RIE`.
- Carried (s232): the `[WO-SOURCETYPE-RECON]` additions; the export serializer
  omitting `requires_human_review`.
- Carried: the s231 Form 3800 five spec defects; the s230 Form 6765 items
  (a)-(e); the **1065 Schedule K-1 box-15 letters** for §41 and §45R (still
  URGENT — `D_3800_008` excludes those credits until both are verified);
  s228 `D_K1B_FULLY_ALLOWED`; s226/s227/s224/s223 unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
