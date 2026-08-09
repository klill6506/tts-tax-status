# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-08 (s237). **1040 BATCH-002 #4 BUILT** — the individual
foreign tax credit. Two thirds of its reported cause was already closed and is
now pinned; the real gap was the full Form 1116 lane section. One deploy.
BATCH-002 now has 1 item open, BATCH-001 has 6.*

*Previous (s236): the Georgia RIE suspended-K-1-loss feeder + the S-corp K-1
charitable carry (`9215797` · `7b96ab0` · `d2b78d5` · `b05e1b2`).*

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

### ⭐ NEXT UNIT — 1040 BATCH-002 #6, the Form 8379 build
Unblocked, needs nothing from Ken, and **already triaged in s237** — read the
scoping note in the batch file's s237 annex before starting. What that triage
found:

- **Nothing exists.** No `Form8379` model, no compute, no render, no MeF, no
  lane section. The only trace anywhere is `Form8888.filed_form_8379`, a
  boolean whose sole job is to fire `D_8888_8379` (F8379-019-01 bars Form 8888
  and Form 8379 from riding one return).
- **The 404-STOP gate does NOT block** — `lookup/8379/export/` returns 200 and
  the spec is cached at `server/specs/8379_spec.json`. ⚠ **But it is
  `"status": "draft"`, version 1, with a `line_map` of FOUR entries** (`P1_ELIG`,
  `L13a`, `L15`, `L19`) for a Part III allocation grid of roughly twenty lines.
  Its four rules are sound as far as they go (the Part I eligibility chain;
  `col a == col b + col c`; the one-half standard-deduction split; the
  community-property carve-out for AZ/CA/ID/LA/NV/NM/TX/WA/WI where the IRS
  divides the overpayment by state law, not by the Part III entries) — they
  just do not describe the whole face.
- Not a 404, so not a STOP: this is the s222/s223 shape, where **the IRS form
  face + `IRS8379.xsd` + the `F8379-*` business rules are the real
  specification** and the build carries a `_source_brief.md`.
- **Size it honestly — a full seven-leg form build** (model + migration, seed,
  compute, AcroForm render, diagnostics, MeF document, lane section, tests).
  s237 stopped rather than start it half-built.

### ⭐ ALSO STILL PENDING AND STILL UNBLOCKED — the Form 8853 Section C build
Unchanged from s232, untouched for five sessions. Spec cached at
`server/specs/8853_sec_c_spec.json`; deployed `lookup/8853_SEC_C/export/`
returns 200. All four legs pending, none needs Ken. **The two things the build
must not get wrong** (Schedule 1 line 8e is COMPOSED, not owned; line 25 FLOORS
AT ZERO though the face does not say so) are written up in full in
`STATUS_ARCHIVE.md` under s232 — read that before starting.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — 6 open** (2, 4, 5, 6, 8, 10) and
  **BATCH-002 — 1 open** (6; 9 and 10 are open only as to their COMPUTE half,
  both RS-blocked). Both carry result annexes naming exactly what is done and
  what is blocked; read them before starting.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

### ✅ s237 in one paragraph
One item, and verify-first paid for itself before a line was written. The batch
item's three claims about the lane were checked first: **two were already
false.** `foreign_tax_paid` has been in `INT_FIELDS` and `DIV_FIELDS` since the
lane's first commit, and the §904(j) de minimis AUTO-election has landed
`min(foreign tax, regular tax)` on Schedule 3 line 1 — no Form 1116 engaged, no
face — since 2026-07-01, which is exactly the reported shape (a $97 credit with
no printed 1116). **The reported packet imports today, unchanged**, and both
facts are now pinned so they cannot silently regress. The real gap was the
FULL §904 path: every fact that forces it lives on the `Form1116` row and the
lane could not create one, so those packets were un-importable outright. Built
`form_1116s`, the lane's first SINGLETON section, plus the importable
`ftc_deminimis_optout`.

### ⚠⚠ THE FINDING WORTH CARRYING — a helper that fires an auto-credit needs an off switch
`ftc_deminimis_optout` existed on the model and in the browser but **not in the
lane**, while the auto-election fires on ANY 1099 foreign tax at or under the
ceiling. So a packet whose preparer **deliberately did not claim the credit**
(deducted the foreign tax on Schedule A, or let it go) imported **with a credit
the filed return does not have**, and nothing in the lane could suppress it.
⚠ Sign: the lane OVERSTATED the refund on exactly those returns, and the
reconciliation would have named the engine as the culprit. *The general shape:
when a convenience auto-derivation is added, the fact that TURNS IT OFF becomes
a required input on every import surface, not an optional preference.*

### ⚠⚠ THE OTHER ONE — a OneToOne cannot answer the questions a FK family answers
`form_1116s` is the lane's first singleton section, and the reverse accessor
**raises `RelatedObjectDoesNotExist` when absent** and returns a bare model
instance (no `.count()`, no set `.delete()`) when present. The merge gate, the
replace pass and the cleanup persistence gate all called `getattr(target, rel)`
— every one of them would have broken. They now go through one `_section_qs`
helper. **The next singleton section will hit this exact trap**; it is not
specific to Form 1116.

### ⚠⚠ AND ONE SEAM CLOSED BY REFUSAL, NOT BY BUILDING
Schedule 3 line 1 transmits as a bare `ForeignTaxCreditAmt` and **there is
still no `IRS1116` e-file document builder** (the s148 sweep already named this
as the 4th occurrence of the missing-document shape). Correct on the §904(j)
paths — no Form 1116 is due — but a FULL-path return would file a credit with
its required form MISSING, and **no MeF business rule catches it**: every
`F1116-*` rule in `1040_Business_Rules_2025v5.3.csv` presumes the form is
already present. `extract_return` now refuses a full-path return by name, with
the §904(j) path pinned to keep extracting so a future blanket guard cannot
ground every retiree with an international fund. **It is a refusal, not a fix —
the full path was already paper-only in fact and is now paper-only in code.
Building `IRS1116` is the follow-up.**

### ⚠ Classes that MOVE existing returns or output on next recompute
- **NONE from s237.** No compute code changed; `form_1116s` rows can only exist
  where a payload creates one, and `ftc_deminimis_optout` defaults false (the
  old behavior). The one behavior change is the **e-file refusal**, and it
  reaches only returns already computing a full Form 1116 — which could not
  have been validly transmitted before either.
- Carried from s236: a Georgia return with a passive K-1 whose loss was partly
  or wholly suspended gets a different RIE line 13 (a suspended loss leaving
  the base RAISES the exclusion and LOWERS Georgia tax — correctly); a
  materially-participating K-1 carrying box 2 or 3 amounts.
- Carried from s235: Georgia dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted (NONE of it from this session)
- **~24 test files** assert registry wiring via
  `inspect.getsource(seed_builtin_rules)`; s233 refactored that function to
  iterate `all_registries()`, so these are **false REDs** — the registries are
  still registered. s237 hit one of them
  (`test_1116_diagnostics_leg.py::test_rules_1116_registered_in_runner`) and
  **verified rather than assumed**: `RULES_1116` is registered at
  `runner.py:262`. Spun off as its own task.
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
  red under `--reuse-db` (3 tests) — hit again in s237's regression run.
- **Client typecheck**: 55 error lines standalone. s237 touched no .tsx.

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
  the Bash tool also silently produces no output — **s237 lost an append to a
  batch file to exactly this** and had to redo it as a script file. A bash
  heredoc into `python -` DOES work.
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
- **NEW (s237)**: the **`8379` spec is a DRAFT with a 4-entry `line_map`**
  (`P1_ELIG`, `L13a`, `L15`, `L19`) for a Part III grid of ~20 lines. Its four
  rules are correct as far as they go; they simply do not cover the face. The
  build can proceed off the IRS form + `IRS8379.xsd` + the `F8379-*` rules
  (the s222/s223 shape), but promoting the spec out of draft with the full
  allocation grid would make it a transcription instead.
- Carried (s236): `R-SCHA-CHARITABLE` models only three buckets while the K-1
  states **seven** — 1120-S box 12 codes A–G — so **B, D, F and G have no home
  and are REFUSED at both write paths**, the same gap `D_SCHA_007` RED-defers
  on the taxpayer side. ⚠ Sign: a refused code is not deducted AT ALL — it
  overstates tax.
- Carried (s236): the `500` spec's RIE line_map has no **RIE-13**, and no rule
  governs what FEEDS lines 1/2/6-13 at all. Two sessions running (s233, s236)
  the app settled a sourcing question from Ga. Comp. R. & Regs. r. 560-7-4-.02
  directly because `R-GA500-RIE` does not exist.
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
