# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-09 (s240). **1040 BATCH-004 opened and triaged 10/10;
item #4 BUILT.** The report was stale and the neighborhood held TWO live
defects — a passive §1231 loss with no §469 limitation and no diagnostic, and
every 1040 with a Form 4797 being a rejected transmission. One deploy, no
migrations. BATCH-004 stays OPEN (9 queued builds).*

*Previous (s239): 1040 BATCH-003 #4/#5/#7 — three live defects (`4f924ac` · `e7803eb`).*

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

### ⭐ NEXT UNIT — pick from BATCH-004's queued nine, or finish BATCH-003
**BATCH-004 is now fully triaged** (annex in the batch file names each item,
what exists today, and its size). Nothing in it is un-scoped any more.

**The two cheapest real wins are #6 (Form 8863) and #7 (Form 5329 Part III) —
both are LANE-ONLY gaps on forms that are already fully built**, so the work is
a `backentry.v1` importer family plus an end-to-end verification, not a form.
#7 is the smaller of the two.

The rest are genuine builds, roughly by size: #8 Form 8862 (render + MeF exist;
input model missing) < #3 pre-2019 alimony ≈ #9 Form 1099-PATR ≈ #10 Form 4547
+ 8879-TA ≈ #2 GA education credit + IT-QEE-TP2 ≈ #5 Schedule H << #1 1040-X
amended lifecycle (large).

⚠ **#10's source check is CLOSED — the form is REAL.** Form 4547, *Trump
Account Election(s)*, Rev. December 2025, created by OBBBA; instructions same
revision. The IRS states the election is filed **with the current-year e-filed
return**, so its MeF leg is part of the build. STATUS's prior "verify both
exist before designing" flag is answered.

⚠ **#9 does NOT go through the 404-STOP gate** — per s222 no RS spec exists for
any information return; build from the IRS form + a `_source_brief.md`.

### ⭐ STILL UNBLOCKED, still passed over — now EIGHT sessions
- **Form 8853 Section C.** Spec cached at `server/specs/8853_sec_c_spec.json`;
  `lookup/8853_SEC_C/export/` returns 200; all four legs pending. Read the s232
  write-up in `STATUS_ARCHIVE.md` first — Schedule 1 line 8e is COMPOSED not
  owned, and line 25 FLOORS AT ZERO though the printed face does not say so.

### ⛔⛔ THE E-FILE GAP LIST GREW — it is now TWO named documents
- **`IRS1116`** — the oldest live e-file gap. A full-path Form 1116 return is
  paper-only in code. s238's `IRS8379` build is a worked example end to end.
- **`IRS4797` (NEW, s240)** — there is no 1040-side Form 4797 document builder
  at all, and **MeF rejects the return without it** (`S1-F1040-118-01`, Reject,
  Active). s240 added the refusal so it fails loudly at composition; the
  document itself is unbuilt. **This is the higher-volume of the two** — every
  disposition, installment sale, like-kind exchange AND K-1 §1231 return needs
  it. The 1120-S `IRS4797` builder is the worked example.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — 6 open** (2, 4, 5, 6, 8, 10);
  **BATCH-002 — items 9 and 10 open as to their COMPUTE half only** (both
  RS-blocked on the missing NOL spec); **BATCH-003 — 7 open** (1, 2, 3, 6, 8,
  9, 10, all confirmed real builds); **BATCH-004 — 9 open, all triaged**.
  Every file carries a result annex naming what is done and what is blocked;
  read it before starting. ⚠ None of the four has moved to Done, deliberately.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

### ✅ s240 in one paragraph
One item worked, and it was refuted as reported — the §1231 → 4797 → Schedule 1
chain has been complete since 2026-06-30 and ties the filed return to the
dollar. Behind it sat two defects the report never mentioned, **both created by
that same feed**: a passive activity's §1231 loss reaches AGI with no §469
limitation and no diagnostic, and a non-zero Schedule 1 line 4 makes the whole
return un-transmittable. Both fixed. Nine remaining items triaged; three of the
ten (#4, #6, #7) turned out to be already-built forms with import-lane gaps.

### ⚠⚠ THE FINDING WORTH CARRYING — retiring a RED can silently void a DIFFERENT rule's safety argument
`R-8582-MULTIFORM` RED-defers Form 8582 Part IX and justifies itself in
writing: *"The common Part IX triggers (section 1231, 28%-rate) are already
RED-deferred upstream in the K-1 router, so no NEW silent gap."* That sentence
was true when written. On 2026-06-30 the §1231 → Form 4797 feed was built and
`D_K1_SEC1231` was **retired as newly-supported** — a correct, well-tested
change that nevertheless **knocked the leg out from under a rule in a different
module**, whose own code and tests were untouched and stayed green. The two
guards left standing both tested `k1_sche_net < 0` (K-1 boxes 1/2/3), so the
one case that mattered — a K-1 whose ONLY loss is its §1231 amount — was
invisible to both, and a $50,000 passive loss reduced AGI in full in silence.
*The general shape: when you RETIRE a diagnostic because its subject is now
supported, grep for every rule that CITED it as its own coverage. A spec's
"no new silent gap" clause is a dependency, not a comment — and nothing in the
test suite can fail when a dependency like that expires.* This is the fourth
time here that adding support made an existing rule wrong (s225, s233, s238).

### ⚠⚠ THE OTHER ONE — a correct new feed can make an OLD seam start rejecting
Schedule 1 line 4 is written by `compute_4797_db`. Adding the K-1 §1231 feed
gave that line a non-zero value on returns that carry **no disposition at all**
— and the 1040 MeF builder has no `IRS4797` document, so `S1-F1040-118-01`
(Reject, Active) bounces them. No e-file code changed; no test failed; the
failure is an IRS reject arriving days later, which no internal reconciliation
catches. *When a feed newly populates a line, check what the MeF business rules
require to accompany that line — the reject list is the spec for the seam.*

### ⚠ Classes that MOVE existing returns or output on next recompute
- **s240 (diagnostics only, no number moves):** a passive or PTP K-1 carrying a
  net §1231 LOSS now fires `D_8582_MULTIFORM` (RED) or `D_K1_PTP_LOSS`
  (warning) where it was silent. No computed value changes — the loss still
  flows; it is now *stated* that §469 was not applied to it.
- **s240 (e-file REFUSAL, this one blocks):** any 1040 with a non-zero Schedule
  1 line 4 now refuses at MeF composition with "no IRS4797". These returns were
  already un-transmittable — they now fail at our seam instead of the IRS's.
- Carried from s239: Roth 1099-Rs (codes J/T/Q) move from 5a/5b to 4a/4b; any
  Georgia return with a partnership K-1 moves income between RIE L2 and L13;
  an engaged Form 8606 changes RIE L11; a code-U 1099-R un-blanks the whole
  pension taxable column (the largest mover).
- Carried from s236: a Georgia return with a passive K-1 whose loss was partly
  or wholly suspended gets a different RIE line 13.
- Carried from s235: Georgia dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted (unchanged from s239 — s240 added none)
- **~24 test files** assert registry wiring via
  `inspect.getsource(seed_builtin_rules)`; s233 refactored that function to
  iterate `all_registries()`, so these are **false REDs**. Hit again this
  session in `test_schedule_e_8582_diagnostics_leg.py::test_runner_registers_sche`
  and `test_schedule_k1_diagnostics_leg.py::test_runner_registers_schedule_e_p2`.
  Still spun off as its own task. **This is now the single most frequent
  false-red in the repo — it has cost time in four consecutive sessions.**
- **`test_topic7_input_leg.py::TestEICFacts::test_non_engaged_return_leaves_27a_quiet`**
  — pre-existing, verified at pristine `6e819b5` in a worktree (s235). Not diagnosed.
- **`test_1040.py` — 6 pipeline tests**, `MultipleObjectsReturned`. Their `_fv`
  helper does an **unscoped** `FormFieldValue.objects.get()`. Pre-existing (s234).
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  `ValidationError`. Pre-existing (s219). Not diagnosed.
- `test_4868.py` — 4 tests on the Schedule 3 line-10 feeder (s217). ⛔ KEN.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  batch-005 #9 PTET-gate class, red since s212.
- **`--reuse-db` cross-module contamination**: `test_backentry_cleanup.py` (3,
  s225) and `test_mappings.py::TestApplyMappingAmbiguousFederalReturn` (3, s239).
- **Client typecheck**: 55 error lines standalone. s240 touched no .tsx.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). `--reuse-db`.
- **A broad `-k` sweep is SLOW and blows the 600s Bash timeout** — s236's
  ~1,100-test sweep took **21½ minutes**. Run it with `run_in_background: true`.
  ⚠ Keep the `-k` terms tight: `k1` matches hundreds of node ids, `rie` matches
  "ret**rie**ve". (s240's `-k "efile or mef"` sweep = 543 tests / 3m11s — fine.)
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
  (s240) — it gets fixtures, the DB and the app context for free.**
- ⚠ **`poetry run` must be invoked from `D:\dev\delvio-tax\server`** — from the
  repo root it fails with "could not find a pyproject.toml". The Bash tool's
  cwd persists across calls, so `cd` absolutely, every call.
- ⚠ **Windows `python` cannot read the Bash tool's `/tmp`** — they are different
  filesystems. Write shared files to the scratchpad path, not `/tmp` (s240).
- ⚠ A Cloudflare-protected law site (justia) 403s both WebFetch and curl.
  **The in-app browser (`preview_start` + `get_page_text`) got the full
  verbatim Georgia reg** where both failed (s239).

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
- The one live external item: **1040 v5.4 business rules still not in hand,
  active 2026-08-09** (v5.4 schemas ARE on disk; 1041 v5.5 closed). ⚠ s240 read
  the **v5.3** rules for `S1-F1040-118-01`; re-check it against v5.4 on arrival.

### RS AGENDA
- **⛔ BLOCKING two batch items: THERE IS NO NOL SPEC.** `lookup/172/`,
  `lookup/NOL/`, `lookup/FORM_172/` and `lookup/1045/` all return 404.
  BATCH-001 #4 and BATCH-002 #10 both ask for Form 172, the post-2017
  limitation and the utilization ordering. **The preservation half is built and
  the pools are safe — only the computation waits.** Still the single
  highest-value RS authoring order on this list.
- **NEW (s240): `R-8582-MULTIFORM`'s no-silent-gap clause is now FALSE as
  written** and should be re-authored. It cites an upstream RED (`D_K1_SEC1231`)
  that was retired 2026-06-30. The app-side coverage is restored, but the spec
  still tells the next reader that §1231 is RED-deferred upstream, which it is
  not. While it is open: `R-8582-WS-NET` describes the Parts IV/V per-activity
  gathering purely in Schedule-E terms and **says nothing about which OTHER
  forms an activity's losses can land on** — that silence is what let the §1231
  component fall outside the engine in the first place.
- **NEW (s240): the `4797` spec has no rule for the K-1 §1231 feed.** The feed
  exists in code (`k1_section_1231_total` → Part I line 2) and is correct per
  i4797, but no spec rule governs it, so nothing declares whether the amount is
  pre- or post-§469. That ordering is the whole of the defect above.
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
