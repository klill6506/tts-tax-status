# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-08 (s234). **1040 BATCH-002 opened: items 2 and 5 —
the two live computed-tax defects — BUILT.** Item 5's reported CAUSE was
refuted and a worse one found underneath. One deploy. BATCH-002 stays in the
queue with 8 items open; BATCH-001 still has 6.*

*Previous (s233): GA RIE trio + the engine-fault gate (`8500744`).*

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
**There is more unattended work than the away window can absorb** — 14 open
1040 queue items plus the 8853 build.

---

## ▶ RESUME HERE

### ⭐ NEXT UNIT — the attribute-preservation layer (design once, not five times)
**BATCH-001 #4 and #10, and BATCH-002 #1, #9 and #10 are all one shape**: a
future-year tax attribute — regular NOL by loss-year vintage, §179 carryover,
charitable carryover by source year and limitation class, 1099-Q basis — that
the current face ties *without*, and therefore silently discards. They want one
attribute-preservation layer: source-year-keyed rows, independent roll-forward,
and a cleanup diagnostic that refuses completion when a filed carryover
worksheet has no home. **Read all five items before designing.** This is the
single largest lever left in either file and needs nothing from Ken.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — 6 open** (items 2, 4, 5, 6, 8,
  10) and **BATCH-002 — 8 open** (items 1, 3, 4, 6, 7, 8, 9, 10). Both carry
  PARTIAL result annexes naming exactly what is done; read them before
  starting so nothing is re-triaged.
  - Cheapest remaining: **BATCH-002 #7** (add a `parent` dependent relationship
    and flow it to GA-500 lines 7/14 — a missing enum value costing $207 of
    Georgia tax) and **BATCH-002 #3** (another GA RIE partnership-loss owner
    case, adjacent to everything s233 did).
  - Largest: #4 Form 1116 FTC, #6 Form 8379 injured spouse, #10 Form 172 NOL.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

### ⭐ ALSO STILL PENDING AND STILL UNBLOCKED — the Form 8853 Section C build
Unchanged from s232 and untouched for two sessions. Spec cached at
`server/specs/8853_sec_c_spec.json`; deployed `lookup/8853_SEC_C/export/`
returns 200. All four legs pending, none needs Ken. **The two things the build
must not get wrong** (Schedule 1 line 8e is COMPOSED, not owned; line 25 FLOORS
AT ZERO though the face does not say so) are written up in full in
`STATUS_ARCHIVE.md` under s232 — read that before starting.

### ✅ s234 in one paragraph
Led with the two items STATUS flagged as live computed-tax defects rather than
missing features. **Item 2 confirmed exactly as reported**: the R-STD-04
dependent worksheet was correct but its INPUT was starved — the spec fact
`dependent_filer_earned_income` is a non-null Decimal defaulting to 0 and
**nothing anywhere populated it**, so every dependent filer with wages took the
bare $1,350 minimum. The derivation now comes from the Instructions for Form
1040 (2025) p.35 footnote (1z + Sch 1 lines 3/6/8r/8t/8u − Sch 1 line 15), and
it exposed an ordering bug — line 12 was written before Schedule 1 existed — plus
a second live defect in the proforma §111 snapshot, which decides `did_itemize`
as `line12 > std` and so read **every dependent filer with wages as having
itemized**. **Item 5's reported cause was REFUTED**: Form 8960 already engages
on interest-only income (a clean repro computes the filed $3,271) and K-1
portfolio interest already reaches line 2b. The real mechanism sat one line up
and is worse — see below. Both fixes proven by revert. 545+ tests green
including all 526 flow assertions.

### ⚠⚠ THE FINDING WORTH CARRYING — a back-out sourced from somewhere other than the line it adjusts
Form 8960 line 4b's auto adjustment was computed from the K-1 / rental
**models**, while line 4a is read from **Schedule 1 line 5**. Two sources for
one relationship. When they disagreed, the subtraction ran past 4a and ate
unrelated **portfolio interest** — net investment income went negative and
`compute_8960_db` took its `niit <= 0` branch, **disengaging the form
entirely**. No line 17, no Schedule 2 line 12, no NIIT. The Instructions for
Form 8960 (2025) say it twice — line 4b adjusts "the amounts included on line
4a" — so the auto back-out is now clamped to 4a in both directions.
**The lesson: when an adjustment is derived from a different source than the
line it adjusts, the two WILL disagree, and the error is unbounded — it does
not stop at the boundary of the thing it was meant to adjust.** And a
*disengaged* form is the worst failure shape available: it leaves no wrong
number to notice, only an absent one.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **Dependent filers**: any return with 12a checked and earned income now gets
  a LARGER standard deduction (up to the filing-status base). ⚠ Sign check:
  this LOWERS federal tax on affected returns — correctly.
- **Proforma / §111 snapshot**: `_sr_py_std_deduction` and the derived
  `did_itemize` change for dependent filers. A return finalized before this
  carries the wrong snapshot until re-finalized.
- **Form 8960**: a return with a nonpassive K-1 or self-rental whose net is NOT
  in Schedule 1 line 5 now ENGAGES where it previously vanished. ⚠ Sign check:
  this RAISES tax — correctly; the NIIT was being skipped entirely.
  A return whose line 4a already matched its back-out is unmoved (pinned).
- Carried from s233: diagnostic runs report FAILED on any engine fault; the
  three GA RIE line 6/9/13 movements.

### ⚠ Known red / rotted (NONE of it from this session)
- **`test_1040.py` — 6 pipeline tests**, `MultipleObjectsReturned`. Their `_fv`
  helper does an **unscoped** `FormFieldValue.objects.get()` that collides with
  sibling-form rows. **NEW this session, and verified identical at pristine
  `8500744` in a throwaway worktree** — pre-existing, not from s234.
- **24 test files** assert registry wiring via
  `inspect.getsource(seed_builtin_rules)`; s233 refactored that function to
  iterate `all_registries()`, so these are **false REDs** — the registries are
  still registered. Confirmed failing: `test_8615_diagnostics_leg`,
  `test_schedule_e_8582_diagnostics_leg`. Spun off as its own task.
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  `ValidationError`. Pre-existing (s219). Not diagnosed.
- `test_4868.py` — 4 tests on the Schedule 3 line-10 feeder (s217). ⛔ KEN.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  batch-005 #9 PTET-gate class, red since s212.
- **DiagnosticRule unique-code contamination** (s225): `test_backentry_cleanup.py`
  red under `--reuse-db`.
- **Client typecheck**: 55 error lines standalone (untouched by s234 — no client code).

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). `--reuse-db`.
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout
  loses ALL output — redirect to a file and tail it.
- ⚠ A `poetry run python script > file` redirect BUFFERS. Use `-u`.
- ⚠ A stdout **redirect goes through cp1252** on this box and dies on ligatures
  and the U+2212 minus. Write UTF-8 from inside Python.
- ⚠ `manage.py shell -c "..."` prints nothing; a multi-line `python -c` through
  the Bash tool also silently produces no output. Write a script file.
- ⚠ The Bash tool's cwd PERSISTS across calls — use absolute `cd` each call.
  **New (s234)**: `/tmp` in the Bash tool and the Windows Python interpreter do
  NOT resolve to the same place — write scratch files to the scratchpad path.
- **New (s234)**: to test whether a red suite is pre-existing, `git worktree add`
  a detached HEAD and run it with the MAIN venv's interpreter
  (`.venv/Scripts/python.exe -m pytest`) — the worktree has no venv of its own,
  and this needs no `git stash` and no `git checkout --`.

### 🔎 Carried for triage — NOT claims
- **NEW (s234), potentially large**: a materially-participating 1120-S K-1
  carrying **$250,000 of nonpassive ordinary business income never reached
  Schedule 1 line 5 or 1040 AGI** in the item-5 repro — line 9 was identical
  with the K-1 at $0 and at $250,000 — and this held with `SCHEDULE_E` AND
  `FORM_8582` both seeded, while `schedule_e_non_1411_income` *did* see the
  same row. Either the fixture omits a required field, or a K-1's nonpassive
  net is not reaching AGI. The second reading would dwarf anything in the
  queue. Repro: `server/tests/test_8960_line4b_clamp.py`,
  `_build(k1_ordinary="250000")`.
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
  in Done. Nothing was lost. Worth telling Codex to skip to `BATCH-003`.
- The one live external item: **1040 v5.4 business rules still not in hand,
  active 2026-08-09** (v5.4 schemas ARE on disk; 1041 v5.5 closed).

### RS AGENDA
- **NEW (s234)**: the 1040 SPINE spec's **R-STD-04 is silent on where worksheet
  line 1 comes from** — it lists `dependent_filer_earned_income` as a keyed
  fact with `default_value: "0"` and says nothing about deriving it, which is
  exactly why nothing did. The rule should carry the i1040 p.35 derivation
  (1z + Sch 1 3/6/8r/8t/8u − Sch 1 15) so the next build does not re-derive it,
  and should note that the fact is an OVERRIDE, not the primary source.
- **NEW (s234)**: the FORM_8960 spec's `R-8960-INCOME` describes line 4b only as
  "nonpassive_adj(4b)" with no statement that it is bounded by line 4a. Add the
  i8960 constraint — 4b adjusts "the amounts included on line 4a" — and the
  §1411(c)(1)(A)(i) point that portfolio interest is NII regardless of material
  participation.
- Carried (s233): `compute_ga500.GA_RIE_EARNED_CAP` $5,000 vs the reg's
  $4,000 — one re-verification against current §48-7-27(a)(5) to settle it;
  and the GA-500 spec's missing RIE rules (Georgia-taxable-income limit on the
  base; owner attribution of capital losses) belong in `R-GA500-RIE`.
- Carried (s232): the `[WO-SOURCETYPE-RECON]` additions; the export serializer
  omitting `requires_human_review`.
- Carried: the s231 Form 3800 five spec defects; the s230 Form 6765 items
  (a)-(e); the **1065 Schedule K-1 box-15 letters** for §41 and §45R (still
  URGENT — `D_3800_008` excludes those credits until both are verified);
  s228 `D_K1B_FULLY_ALLOWED`; s226/s227/s224/s223 unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
