# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-09 (s238). **1040 BATCH-002 #6 BUILT** — Form 8379,
Injured Spouse Allocation, a new form gaining all seven legs at once. The RS
spec answered 200 but covered 2 of its 9 balance rules. One deploy (`4ca1019`),
two migrations. BATCH-002 now has 0 fully-open items (9 and 10 are open as to
their RS-blocked COMPUTE half only); BATCH-001 has 6.*

*Previous (s237): the individual foreign tax credit and the Form 1116 lane
section (`75c53a7` · `bece549`).*

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

### ⭐ NEXT UNIT — the Form 8853 Section C build
Unblocked, needs nothing from Ken, and now untouched for **six** sessions —
it has been the runner-up every session since s232 and keeps being passed over
for batch items. Spec cached at `server/specs/8853_sec_c_spec.json`; deployed
`lookup/8853_SEC_C/export/` returns 200. All four legs pending.

**The two things the build must not get wrong** are written up in full in
`STATUS_ARCHIVE.md` under s232 — read that first:
- Schedule 1 line 8e is **COMPOSED, not owned** (the s230 shared-line lesson —
  a second writer would silently overwrite the first).
- Line 25 **FLOORS AT ZERO** though the printed face does not say so; the
  statute's "excess (if any)" is the floor, and LII had DROPPED that phrase
  while uscode.house.gov carried it. Unfloored, line 26 exceeds line 20.

### ⭐ ALSO UNBLOCKED — build the `IRS1116` e-file document
Named as the follow-up in s237 and now the oldest live e-file gap. A FULL-path
Form 1116 return is **paper-only in code** (`extract_return` refuses it by
name) because no `IRS1116` builder exists — the s148 sweep's 4th
missing-document occurrence. The s238 `IRS8379` build is a fresh worked example
of the whole shape: source dataclass → builder in `xsd:sequence` order → the
ReturnData1040 slot read off the schema → a test that validates real XML
against the real XSD. That last one earns its keep; it caught a bad SSN format
in s238 within seconds.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — 6 open** (2, 4, 5, 6, 8, 10) and
  **BATCH-002 — items 9 and 10 open as to their COMPUTE half only**, both
  RS-blocked. Both files carry result annexes naming exactly what is done and
  what is blocked; read them before starting. ⚠ BATCH-002 deliberately did
  **not** move to Done — the README lets a batch move with an unresolved ⛔ KEN
  item, but an RS-blocked build is different: two computations are genuinely
  unbuilt, and filing the batch away would record them as finished.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

### ✅ s238 in one paragraph
One item, and it was a whole form. Form 8379 had **nothing** — no model,
compute, render, MeF, diagnostics or lane section; the only trace anywhere was
`Form8888.filed_form_8379`, a boolean firing `D_8888_8379`. All seven legs
built in one pass, plus a browser card, against a governing specification that
was **not** the Rule Studio spec.

### ⚠⚠ THE FINDING WORTH CARRYING — a 200 from Rule Studio is not a green light
The 404-STOP gate exists to stop CC improvising a form with no spec. `8379`
answers **200**, so the gate never fired — and the export turned out to be
`"status": "draft"`, version 1, with a **4-entry `line_map` for a nine-line
Part III grid**, whose one allocation rule states the column constraint for
**two** of the nine lines. Implementing it faithfully would have shipped a form
with **seven unguarded reject conditions**, plus no exactly-one-injured-spouse
rule, no MFJ requirement, no address bars and no barred-companion list — every
one of them an Active/Reject MeF rule. *The general shape: the 404 gate tests
for the EXISTENCE of a spec, and nothing tests its COVERAGE. Read the
`line_map` against the form's own face before trusting a 200 — a draft spec is
the most dangerous kind, because it looks like permission to proceed.*

### ⚠⚠ THE OTHER ONE — adding a form made an existing rule wrong, again
Before this build, "is a Form 8379 on this return?" had exactly one possible
answer: the `Form8888.filed_form_8379` checkbox. A real `Form8379` row is now a
**second source for the same fact**, and `d_8888_8379`, `analyze_8888` and
`_extract_f8888` all read only the checkbox. A return carrying a genuine Form
8379 with that box unticked would have kept a refund split the IRS rejects
outright (F8379-019-01). One shared `compute_8888.form_8379_present()` now
serves all three, and both directions are pinned — the row alone bars the
split, and the checkbox alone still bars it (a preparer stating that an 8379 is
being filed SEPARATELY on paper bars it just the same). ⚠ Sign: **a rejected
transmission, not a wrong number** — the return looks correct and comes back
rejected, which is why it would have survived every reconciliation.
*This is s225's lesson for the third time: adding a rule usually makes an
existing rule wrong. Grep for who else answers the question you just gave a
second answer to.*

### ⚠ THE DESIGN CALL TO KNOW ABOUT — column (a) derived, column (c) NOT
Part III column (a) ("Amount shown on joint return") is **derived** from the
return the form rides on; the lane refuses an `l*_joint` key by name. Column
(c) is arithmetically redundant (`c = a − b`) and is **deliberately still
keyed** — deriving it would satisfy all nine balance rules by construction and
throw away the filed packet's own redundancy. Keeping both makes `a != b + c` a
real check that catches a transcription slip *or* an app return that does not
match the filed one.

⚠ Two column-(a) sources are settled by the XSD, not the printed face:
**line 13a is 1040 line 1a, not 1z** (both are named `WagesAmt`; 1z also
carries tips and household wages, none of them "reported on Form(s) W-2"), and
**line 20 is `EstimatedTaxPaymentAmt` = line 26**, not total payments, which
would double-count line 19's withholding.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **NONE from s238.** No existing compute code changed. `Form8379` rows are new
  and can only exist where a payload or preparer creates one. The single
  behavior change is the 8888 bar, and it reaches only a return carrying BOTH a
  Form 8888 and a real Form 8379 — a combination the IRS rejects today.
- Carried from s237: none (the e-file refusal reaches only full-path 1116
  returns, which could not have been validly transmitted before either).
- Carried from s236: a Georgia return with a passive K-1 whose loss was partly
  or wholly suspended gets a different RIE line 13; a materially-participating
  K-1 carrying box 2 or 3 amounts.
- Carried from s235: Georgia dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted (NONE of it from this session)
- **~24 test files** assert registry wiring via
  `inspect.getsource(seed_builtin_rules)`; s233 refactored that function to
  iterate `all_registries()`, so these are **false REDs**. Spun off as its own task.
- **`test_topic7_input_leg.py::TestEICFacts::test_non_engaged_return_leaves_27a_quiet`**
  — pre-existing, verified at pristine `6e819b5` in a worktree (s235). Not diagnosed.
- **`test_1040.py` — 6 pipeline tests**, `MultipleObjectsReturned`. Their `_fv`
  helper does an **unscoped** `FormFieldValue.objects.get()`. Pre-existing (s234).
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  `ValidationError`. Pre-existing (s219). Not diagnosed.
- `test_4868.py` — 4 tests on the Schedule 3 line-10 feeder (s217). ⛔ KEN.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  batch-005 #9 PTET-gate class, red since s212.
- **DiagnosticRule unique-code contamination** (s225): `test_backentry_cleanup.py`
  red under `--reuse-db` (3 tests) — hit again in s238's regression run
  (`D_PREPARER_001`), unrelated to the 17 new `D_8379_*` codes.
- **Client typecheck**: 55 error lines standalone. s238 touched no .tsx.
- ✅ **FIXED in s238**: `test_tts_forms.py::TestManifest::test_manifest_is_valid_json`
  pinned a hand-counted `len(forms) == 100`. Retired rather than re-pinned —
  three prior sessions (s124, s219, s230) recorded MISSING that re-pin, so it
  was failing for bookkeeping, never for a real defect. It now asserts
  non-empty + no duplicate `form_id`.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). `--reuse-db`.
- **A broad `-k` sweep is SLOW and blows the 600s Bash timeout** — s236's
  ~1,100-test sweep took **21½ minutes**. Run it with `run_in_background: true`
  and collect it later. ⚠ Keep the `-k` terms tight: `k1` matches hundreds of
  node ids, and `rie` matches "ret**rie**ve".
- ⚠ `--create-db` does NOT reliably drop an existing test DB here. To prove a
  red is pre-existing, use a `git worktree` at a pristine SHA with the MAIN
  venv's interpreter, and copy `server/.env` in (s235).
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout
  loses ALL output — redirect to a file and tail it.
- ⚠ A `poetry run python script > file` redirect BUFFERS. Use `-u`.
- ⚠ A stdout **redirect goes through cp1252** on this box and dies on ligatures
  and the U+2212 minus. Write UTF-8 from inside Python.
- ⚠ `manage.py shell -c "..."` prints nothing; a multi-line `python -c` through
  the Bash tool also silently produces no output. A bash heredoc into
  `python -` DOES work — **but s238 lost a heredoc to apostrophes in the body
  when it was chained with a following command.** For any long prose payload,
  write the file with the Write tool and run a tiny script file against it.
- ⚠ **`poetry run` must be invoked from `D:\dev\delvio-tax\server`** — from the
  repo root it fails with "could not find a pyproject.toml". The Bash tool's
  cwd persists across calls, so `cd` absolutely, every call.

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
- **⛔ BLOCKING two batch items: THERE IS NO NOL SPEC.** `lookup/172/`,
  `lookup/NOL/`, `lookup/FORM_172/` and `lookup/1045/` all return 404.
  BATCH-001 #4 and BATCH-002 #10 both ask for Form 172, the post-2017
  limitation and the utilization ordering; the 404-STOP gate forbids
  improvising them. **The preservation half is built and the pools are safe —
  only the computation waits.** Still the single highest-value RS authoring
  order on this list, and it is now the ONLY thing keeping BATCH-002 open.
- **NEW (s238), and it generalizes**: the **`8379` spec is a DRAFT whose
  `line_map` covers 4 of ~20 lines**, and because it returns 200 the 404-STOP
  gate waved it through. The build proceeded off the face + `IRS8379.xsd` + the
  18 `F8379-*` rules (the s222/s223 shape) and is complete, so promoting the
  spec is now **documentation, not a blocker** — but the general point stands:
  **the export's `status` field is not checked anywhere.** A `draft` spec that
  answers 200 is indistinguishable from an approved one at the gate. Worth
  either promoting drafts promptly or having CC read `metadata.status` and say
  so out loud.
- Carried (s236): `R-SCHA-CHARITABLE` models only three buckets while the K-1
  states **seven** — 1120-S box 12 codes A–G — so **B, D, F and G have no home
  and are REFUSED at both write paths**. ⚠ Sign: a refused code is not deducted
  AT ALL — it overstates tax.
- Carried (s236): the `500` spec's RIE line_map has no **RIE-13**, and no rule
  governs what FEEDS lines 1/2/6-13 at all.
- Carried (s235): the `SCHEDULE_A` charitable carryover modelled as ONE
  aggregate (BATCH-002 #9 needs pools by source year AND limitation class); the
  `500` spec typing line 7a as `input` when the app now DERIVES it.
- Carried (s234): R-STD-04 silent on the dependent worksheet's line 1; the
  FORM_8960 `R-8960-INCOME` rule silent on line 4b being bounded by line 4a.
- Carried (s233): `compute_ga500.GA_RIE_EARNED_CAP` $5,000 vs the reg's
  $4,000 — one re-verification against current §48-7-27(a)(5).
- Carried (s232): the `[WO-SOURCETYPE-RECON]` additions; the export serializer
  omitting `requires_human_review`.
- Carried: the s231 Form 3800 five spec defects; the s230 Form 6765 items
  (a)-(e); the **1065 Schedule K-1 box-15 letters** for §41 and §45R (still
  URGENT — `D_3800_008` excludes those credits until both are verified);
  s228 `D_K1B_FULLY_ALLOWED`; s226/s227/s224/s223 unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
