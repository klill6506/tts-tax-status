# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-08 (s236). **1040 BATCH-002 #3 and #8 BUILT** — the
Georgia RIE §469-suspended K-1 loss, and the S-corp K-1 charitable carry into
Schedule A + Form 7203 basis. One deploy. BATCH-002 now has 2 items open,
BATCH-001 has 6.*

*Previous (s235): the attribute-preservation layer + BATCH-002 #7 (`a3b34c9` ·
`834600b` · `38dc878`).*

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

### ⭐ NEXT UNIT — 1040 BATCH-002 #4, then #6
Both are **whole-form builds, both unblocked, and neither needs Ken** — which
makes them the right work for the away window. **#4 is Form 1116** (the
individual foreign tax credit path; `ScheduleK1.foreign_taxes` exists as a
RED-defer presence flag today, and the cached spec is `server/specs/1116_spec.json`).
**#6 is Form 8379** (injured-spouse allocation).

### ⭐ ALSO STILL PENDING AND STILL UNBLOCKED — the Form 8853 Section C build
Unchanged from s232, untouched for four sessions. Spec cached at
`server/specs/8853_sec_c_spec.json`; deployed `lookup/8853_SEC_C/export/`
returns 200. All four legs pending, none needs Ken. **The two things the build
must not get wrong** (Schedule 1 line 8e is COMPOSED, not owned; line 25 FLOORS
AT ZERO though the face does not say so) are written up in full in
`STATUS_ARCHIVE.md` under s232 — read that before starting.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — 6 open** (2, 4, 5, 6, 8, 10) and
  **BATCH-002 — 2 open** (4, 6; 9 and 10 are open only as to their COMPUTE
  half, both RS-blocked). Both carry result annexes naming exactly what is done
  and what is blocked; read them before starting.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

### ✅ s236 in one paragraph
Two items, both verified before being built. **#3** was a genuine ENGINE defect
(the payload was checked first and was correct — the first item in six that
wasn't a lane-only gap): the Georgia RIE worksheet's line-13 feeder read each
K-1's RAW box amounts, so a passive partnership loss that Form 8582 suspended
in FULL still shrank the exclusion. New `k1_sche_included` returns what a K-1
actually contributed to Schedule E line 32/37, and the feeder's passive branch
routes that one figure. **#8** turned out to be a missing INPUT, not missing
math: the shared Form 7203 engine has mapped Part III line 42 to K12a/K12b
since s205 and the MeF builder already emits `CharitableContributionAmt`, but
`ScheduleK1` had no charitable field, so a shareholder's contribution reduced
nothing and ending stock basis ran high by exactly that amount.

### ⚠⚠ THE FINDING WORTH CARRYING — the filed number was WRONG, in a way that is provable
Item #3's reported answer (spouse RIE $7,672) **cannot be reproduced by any
correct computation, and the packet's own federal forms prove it**: Form 8582
Part VIII shows the loss **allowed $0**, Schedule E line 41 is the taxpayer's
−$2,948 alone, and there is **no $2,948 of spouse partnership INCOME anywhere
on the return**. TaxWise put the loss on worksheet L2 as −2,948, where the L5
zero floor discards it, then +2,948 on L13. Across the two lines that nets to
zero — the right answer — but only ONE of the two lines has a floor, so +2,948
leaks into the base. The correct answer is **$4,724**; our old $1,776 was 2,948
too LOW and the filed $7,672 is 2,948 too HIGH. **We recover $153 of the
reported $306 and Georgia will NOT tie.** The lesson: *when a filed figure and
a computed figure differ, the filed one is a hypothesis too — reconcile it
against the source return's own forms before treating it as the answer key.*
Precedent: s194 (Lacerte TIED, so ours was wrong), s201 (REFUTED).

### ⚠⚠ THE OTHER ONE — a label nobody reads until they trust it
The Form 7203 field map's Part III comments were a row off from line 38 down
(`42: Section 179 deductions`, `43: Charitable contributions`), contradicting
the compute engine directly beside them. **Nothing ever rendered wrong** — read
positionally off `f7203.pdf`, every `LineNN[0]` widget sits on the row printed
NN, and the MeF builder agrees; the AcroForm NAMES were right all along. Only
the comments were wrong, a fossil of a pre-2022 face that split short- and
long-term capital losses. But it is exactly the label a maintainer adding a row
would have trusted, and item #8 needed line 42. **A comment that describes a
mapping is part of the mapping.** Corrected, and a test now anchors each row to
the PDF's own printed text rather than to the map entry it renders with (s231),
proven by injecting the shift.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **A Georgia return with a passive K-1 whose loss was partly or wholly
  suspended** gets a different RIE line 13. ⚠ Sign check: a suspended loss
  leaving the base RAISES the exclusion and LOWERS Georgia tax — correctly. A
  preparer-entered L13 does not move (`is_overridden`).
- Also moving: a materially-participating K-1 carrying box 2 or 3 amounts (its
  boxes now route through the same helper).
- **Nothing moves from #8** — all three K-1 fields are new and default to zero.
- Carried from s235: Georgia dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted (NONE of it from this session)
- **~24 test files** assert registry wiring via
  `inspect.getsource(seed_builtin_rules)`; s233 refactored that function to
  iterate `all_registries()`, so these are **false REDs** — the registries are
  still registered. **20 of them surfaced in one s236 sweep**, which is the
  whole of that sweep's failures. Spun off as its own task.
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
  red under `--reuse-db` (3 tests).
- ✅ **FIXED in s236** (`d2b78d5`): `test_returns.py::test_retrieve_form_definition_with_sections`
  pinned 11 sections against a seeder that defines 12 — red since before s216,
  found only because "ret**rie**ve" matched a `-k` filter. The count now reads
  from `seed_1120s.SECTIONS`, so it cannot go stale again.
- **Client typecheck**: 55 error lines standalone. s236 touched no .tsx.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). `--reuse-db`.
- **A broad `-k` sweep is SLOW and blows the 600s Bash timeout** — s236's
  ~1,100-test sweep took **21½ minutes**. Run it with `run_in_background: true`
  and collect it later; do NOT start a second pytest while it runs. ⚠ And keep
  the `-k` terms tight: `k1` matches hundreds of node ids, and `rie` matches
  "ret**rie**ve".
- ⚠ `--create-db` does NOT reliably drop a test DB that already exists here
  ("database test_postgres already exists" → it silently reuses). To prove a red
  is pre-existing, use a `git worktree` at a pristine SHA with the MAIN venv's
  interpreter, and copy `server/.env` in (s235).
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout
  loses ALL output — redirect to a file and tail it.
- ⚠ A `poetry run python script > file` redirect BUFFERS. Use `-u`.
- ⚠ A stdout **redirect goes through cp1252** on this box and dies on ligatures
  and the U+2212 minus. Write UTF-8 from inside Python.
- ⚠ `manage.py shell -c "..."` prints nothing; a multi-line `python -c` through
  the Bash tool also silently produces no output. Write a script file (a bash
  heredoc into `python -` DOES work).
- ⚠ The Bash tool's cwd PERSISTS across calls — use absolute `cd` each call.

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
  order on this list.
- **NEW (s236)**: `R-SCHA-CHARITABLE` models only three buckets (cash 60%,
  noncash FMV 50%, capital-gain-to-50%-org 30%). The K-1 states **seven** —
  1120-S box 12 codes A–G — so **B (cash 30%), D (noncash 30%), F (capital-gain
  20%) and G (100%) have no home and are REFUSED at both write paths.** That is
  the same gap `D_SCHA_007` already RED-defers on the taxpayer side; amending
  the rule to model all seven closes both at once. ⚠ Sign: a refused code means
  the contribution is not deducted AT ALL — it overstates tax.
- **NEW (s236)**: the `500` spec's RIE line_map has no **RIE-13**, and no rule
  governs what FEEDS lines 1/2/6-13 at all. Two sessions running (s233, s236)
  the app has had to settle a sourcing question from Ga. Comp. R. & Regs. r.
  560-7-4-.02 directly because `R-GA500-RIE` does not exist. Authoring it
  should carry the regulation's two operative sentences verbatim: *"Only
  retirement income that is included in Georgia taxable income shall be
  included…"* and the earned/unearned split keyed to FICA/SE-tax liability.
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
