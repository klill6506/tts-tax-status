# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-09 (s239). **1040 BATCH-003 opened; the three live
computed defects (#4, #5, #7) BUILT.** Each of the three concealed a defect
bigger than the one reported. One deploy (`4f924ac`), no migrations. BATCH-003 stays OPEN
(7 of 10 are unbuilt multi-leg form builds); BATCH-004 arrived this session and
is untouched.*

*Previous (s238): Form 8379 across all seven legs (`4ca1019` · `21d5eee`).*

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

### ⭐ NEXT UNIT — 1040 BATCH-004, or finish BATCH-003
**BATCH-004 was posted this session (2026-08-09 05:58) and has not been
triaged at all.** Ten items; from the titles they are mostly new-form builds
(1040-X lifecycle, GA education credits + IT-QEE-TP2, pre-2019 alimony,
Schedule H, Form 8863, Form 5329 Part III, Form 8862, 1099-PATR, Form 4547 /
8879-TA), plus **#4 K-1 §1231 → Form 4797 → Schedule 1, which is a live
RED-defer with an exact repro** — the same "live defect, not a missing form"
class this session led with, and the natural first item.

⚠ **BATCH-004 #10 needs a source check before anything is built.** It cites
"Form 4547 Trump Account elections and Form 8879-TA". Neither form has ever
been touched here. Verify both exist for TY2025 and get the actual faces off
irs.gov before designing — the 404-STOP reflex applies to the FORM's existence,
not just to a Rule Studio spec.

The seven QUEUED BATCH-003 items are all confirmed-real multi-leg builds; the
annex in the batch file names each one and what was verified. **#3 (mixed
passive/nonpassive on one K-1) should be built together with the s239 Georgia
work, not beside it** — a partnership row already splits its ordinary income by
SE subjection for Georgia, so a component-level design wants to be shared.

### ⭐ STILL UNBLOCKED, still passed over — now SEVEN sessions
- **Form 8853 Section C.** Spec cached at `server/specs/8853_sec_c_spec.json`;
  `lookup/8853_SEC_C/export/` returns 200; all four legs pending. Read the s232
  write-up in `STATUS_ARCHIVE.md` first — Schedule 1 line 8e is COMPOSED not
  owned, and line 25 FLOORS AT ZERO though the printed face does not say so.
- **The `IRS1116` e-file document** — the oldest live e-file gap. A full-path
  Form 1116 return is paper-only in code. s238's `IRS8379` build is a worked
  example end to end.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — 6 open** (2, 4, 5, 6, 8, 10);
  **BATCH-002 — items 9 and 10 open as to their COMPUTE half only** (both
  RS-blocked on the missing NOL spec); **BATCH-003 — 7 open** (1, 2, 3, 6, 8,
  9, 10, all confirmed real builds); **BATCH-004 — 10, untriaged**. Every file
  carries a result annex naming what is done and what is blocked; read it
  before starting. ⚠ None of the four has moved to Done, deliberately.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

### ✅ s239 in one paragraph
Three items, and **every one of them concealed a defect larger than the one
reported**. Code U was reported as a missing code; the loss was that an
unsupported code blanks the WHOLE pension column, so a $7 ESOP dividend
suppressed an unrelated $671 row. Codes J/T were reported as a false-blocking
diagnostic; admitting them alone would have taxed a Roth distribution twice.
The Georgia RIE was reported as omitting three K-1 amounts; the omission was
real but the earned/unearned TEST was also wrong, and two further feeds were
missing entirely. No migrations — all four defects were in the rules, not the
schema.

### ⚠⚠ THE FINDING WORTH CARRYING — two instructions can disagree on their face
A Roth IRA 1099-R arrives with the box-7 IRA/SEP/SIMPLE checkbox **FALSE**
(i1099-R Box 7: "Do not check the box for a distribution from a Roth IRA that
is not a Roth SIMPLE IRA"), and the 1040 nevertheless reports it on **lines
4a/4b** (i1040 "Lines 4a and 4b": "an IRA includes a traditional IRA ..., Roth
IRA ..., and a SIMPLE IRA"). The engine had taken the stored checkbox as the
answer to "IRA or pension?", so a Roth distribution's gross sat on 5a while its
taxable came from Form 8606 on 4b — two halves of one distribution on two
different line pairs. Because box 2a is blank on a Roth 1099-R **by
instruction**, the generic no-basis fallback taxes the gross, so admitting
codes J/T without fixing the routing would have taxed $3,600 in full on 5b
while the 8606 wrote $0 on 4b. *The general shape: a stored INPUT field that
mirrors a source document's checkbox is not the same fact as the DESTINATION
form's category, even when they share a name. `doc_is_ira_path` is now the one
place that answers it.*

### ⚠⚠ THE OTHER ONE — one sentence, two different tests
Ga. Comp. R. & Regs. r. 560-7-4-.02(4)(b)1 splits the retirement exclusion's
earned and unearned portions, and it uses **a different test for partnerships
than for S corporations in the same sentence**: partnership income is earned
when "subject to Federal FICA tax or Federal self employment tax";
S-corporation income is earned when the taxpayer "materially participates".
The engine applied the S-corp test to both. They routinely disagree — under
§1402(a)(13) a limited partner's distributive share is outside the SE base
however much they participate — and it costs real money because the earned
portion is capped at $5,000 while the unearned portion is not, so the income is
silently absorbed by the cap. ⚠ **Sign is not character**: the test is
`box 14A != guaranteed payments`, not `> 0`, because a general partner's SE
LOSS is equally SE-subject. That distinction is what reconciles two filed
worksheets that look like they contradict each other — s236's carries a
materially-participating partnership LOSS as earned, this one carries
partnership INCOME as unearned. Both are correct; they are different kinds of
partner. *A rule that reads correctly for one entity type is not thereby
verified for the others that share the code path.*

### ⚠ Classes that MOVE existing returns or output on next recompute
- **Roth IRA 1099-Rs (codes J/T/Q) move from lines 5a/5b to 4a/4b**, and the
  Georgia RIE worksheet moves them from L12 to L11. Both are unearned, so no
  Georgia dollar changes; the federal face and the MeF payload do.
- **Any Georgia return with a partnership K-1**: ordinary business income moves
  between RIE L2 and L13 unless box 14A was keyed, and guaranteed payments,
  box-5 interest and box-6a dividends now enter the base for the first time.
  Direction is case-by-case; where it moves L2 → L13 the exclusion GROWS.
- **Any Georgia return with an engaged Form 8606**: RIE L11 now takes the
  8606's taxable total instead of the box-2a sum, so a basis recovery no longer
  inflates the base.
- **Any return with a code-U 1099-R**: the whole pension taxable column
  un-blanks. This is the largest mover in the batch.
- Carried from s238: none (the 8888/8379 bar reaches only a combination the
  IRS rejects today).
- Carried from s236: a Georgia return with a passive K-1 whose loss was partly
  or wholly suspended gets a different RIE line 13.
- Carried from s235: Georgia dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted (one is NEW from this session, and it is not mine)
- **~24 test files** assert registry wiring via
  `inspect.getsource(seed_builtin_rules)`; s233 refactored that function to
  iterate `all_registries()`, so these are **false REDs**. Hit again this
  session in `test_form8606_diagnostics_leg.py::test_runner_registers_8606`.
  Still spun off as its own task.
- ✅ **FIXED in s239**: `test_ga500_diagnostics_leg.py` pinned a hand-counted
  `len(codes) == 17`. Retired rather than re-pinned (the s238 manifest
  precedent) — it now asserts that the DB set equals what `RULES_GA500`
  declares, so the count follows the module instead of a person.
- **`test_topic7_input_leg.py::TestEICFacts::test_non_engaged_return_leaves_27a_quiet`**
  — pre-existing, verified at pristine `6e819b5` in a worktree (s235). Not diagnosed.
- **`test_1040.py` — 6 pipeline tests**, `MultipleObjectsReturned`. Their `_fv`
  helper does an **unscoped** `FormFieldValue.objects.get()`. Pre-existing (s234);
  re-confirmed as all 6 in s239.
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  `ValidationError`. Pre-existing (s219). Not diagnosed.
- `test_4868.py` — 4 tests on the Schedule 3 line-10 feeder (s217). ⛔ KEN.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  batch-005 #9 PTET-gate class, red since s212.
- **`--reuse-db` cross-module contamination**: `test_backentry_cleanup.py` (3,
  DiagnosticRule unique code, s225) and — **newly observed in s239, not
  diagnosed** — `test_mappings.py::TestApplyMappingAmbiguousFederalReturn` (3
  errors, `returns_formdefinition_code_tax_year_applicable` unique violation on
  `(1120-S, 2025)`). Same shape: a fixture re-creating a row a prior module's
  seed already left behind. Nothing in s239 creates FormDefinitions.
- **Client typecheck**: 55 error lines standalone. s239 touched no .tsx.

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
  the Bash tool also silently produces no output. **A script file under the
  scratchpad also fails — `config` is not importable from there.** Copy the
  script INTO `server\`, run it, delete it (s239).
- ⚠ **`poetry run` must be invoked from `D:\dev\delvio-tax\server`** — from the
  repo root it fails with "could not find a pyproject.toml". The Bash tool's
  cwd persists across calls, so `cd` absolutely, every call.
- ⚠ A Cloudflare-protected law site (justia) 403s both WebFetch and curl.
  **The in-app browser (`preview_start` + `get_page_text`) got the full
  verbatim Georgia reg** where both failed (s239) — it is the fallback for any
  JS-rendered or bot-gated authoritative source.

### 🔎 Carried for triage — NOT claims
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
- ✅ **CLOSED s239 (was carried since s233)**: `GA_RIE_EARNED_CAP` $5,000 vs
  the reg's $4,000. **The statute controls and says $5,000** —
  §48-7-27(a)(5)(E)(i), verbatim, twice. The reg's $4,000 is the
  pre-amendment figure (last amended April 2011). The code value was always
  right; the conflict is now recorded in the comment.
- ✅ **CLOSED s239 (was carried since s233)**: the batch-numbering collision.
  Codex took the instruction — BATCH-003 and BATCH-004 are correctly numbered.
- The one live external item: **1040 v5.4 business rules still not in hand,
  active 2026-08-09** (v5.4 schemas ARE on disk; 1041 v5.5 closed).

### RS AGENDA
- **⛔ BLOCKING two batch items: THERE IS NO NOL SPEC.** `lookup/172/`,
  `lookup/NOL/`, `lookup/FORM_172/` and `lookup/1045/` all return 404.
  BATCH-001 #4 and BATCH-002 #10 both ask for Form 172, the post-2017
  limitation and the utilization ordering. **The preservation half is built and
  the pools are safe — only the computation waits.** Still the single
  highest-value RS authoring order on this list.
- **NEW (s239): `R-RET-CODE` has now been outrun three times.** Its v1
  supported set omits code 6 (admitted s176 on Ken's ruling), code W (s199) and
  now code U (s239), and it says nothing about the conditional J/T admission.
  Each was built from a verified i1099-R Table 1 reading. Worth re-authoring
  the rule from the current table in one pass rather than a fourth exception.
- **NEW (s239): the `500` spec still has NO rule governing what feeds RIE lines
  1/2/6-13**, which is where four defects have now been found (s199, s233,
  s236, s239). The earned/unearned split BY ENTITY TYPE — partnerships by SE
  subjection, S corporations by material participation — is the single most
  valuable thing that spec could gain, and `D_GA500_018` currently has no spec
  condition to diverge from. The line_map should also record that the $5,000
  earned cap is statutory and that the reg's $4,000 is superseded.
- Carried (s238): the `8379` spec is a DRAFT whose `line_map` covers 4 of ~20
  lines and returns 200, so the 404-STOP gate waved it through. The build is
  complete, so promoting it is documentation — but **the export's `status`
  field is still not checked anywhere**.
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
