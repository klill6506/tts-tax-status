# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-16 (s268). **▶ 1040 BATCH-296 IS OPEN. Cluster 2
shipped item #31 (`c0b5f52`) — the multi-packet cleanup 500.** Cluster 1
(`ca078dd`, s267) closed 1/6/15/17/29/32/33. 25 items remain.*

*✅ **DEPLOY `c0b5f52` IS LIVE** — Render confirms it went live 2026-08-16
00:21:48 UTC; prep serves normally. All three repos pushed and the status
mirror is synced.*

*⚠ **A TLS OUTAGE ON THE WORKSTATION BLOCKED VERIFICATION FOR ~30 MINUTES**
mid-session: git, curl and PowerShell/.NET all failed with
`SEC_E_UNTRUSTED_ROOT` (no proxy env vars; git uses schannel, the Windows
store) — a VPN/proxy or trust-store change on the machine, not the app. It
was NOT worked around (disabling certificate verification is not an option)
and it cleared on its own. **If it recurs: deploys cannot be verified and
pushes will fail — say so rather than recording a push as a deploy.***

*⚠ **The ~9s/packet production figure is still ARITHMETIC, not a
measurement** — extrapolated from Codex's reported 25s. Time a real
production run when convenient.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.
- *s268 moved the s243b→s263b session blocks into `STATUS_ARCHIVE.md` (308 lines, purely
  additive — verified by `git diff --stat`) so this file stops being an append-log.*

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS (WHEN BUILDS RUN)
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax` (srv-d6geloa4d50c73el2trg); demo = `tts-tax-demo`. ⚠ **A push
is not a deploy — CHECK THE DEPLOY STATUS after pushing** (API key in
`D:\dev\Passwords & Secrets\render-api-key.txt`): 30 pushes "deployed"
into a canceled-build void over two days (08-13→08-15) before anyone
looked. Ken raised the build spend limit $50 → $200 on 2026-08-15, which
resolved that void.

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

## ⚠ KEN IS AWAY 2026-08-09 → ~2026-08-19 (10 days)
**Availability MINIMAL BUT NOT ZERO.** Batch questions; keep them low-friction.
Nothing is on a clock in that window; the next hard deadline is 2026-09-15.

---

## ▶ RESUME HERE

### ✅ s268 — BATCH-296 #31: the multi-packet cleanup 500 (`c0b5f52`)
`POST /api/v1/backentry/cleanup/` documents ten packets but 500'd after
~2 min for both ten and five, while one packet succeeded in ~25s.
**Two independent causes** — either alone would have left it broken:
- **Cumulative latency past gunicorn's `--timeout 120`** (checked against
  the LIVE service via the API, not just `render.yaml` — only the live
  value binds). That is why FIVE packets failed exactly like ten: the
  worker dies at the boundary regardless, taking the already-passed
  packets' results with it.
- **No per-packet isolation** — `run_cleanup_check` was a bare list
  comprehension, so one packet raising discarded every other result.

**⚠⚠ THE MEASUREMENT IS THE FINDING.** Executing all **957 active rules**
read-only against a real filed 2025 return: **4,395 queries per diagnostics
run**, and **2,910 of them (66%) were the same four reads repeated** — the
1040 TaxReturn 939×, its FormFieldValue set 812×, Taxpayer 605×, Dependents
554×. `_ctx_1040` has **413 call sites** and had no memo, so every rule
re-read a tax year that no rule modifies. Separately the three `D_EFILE_*`
rules each ran the **whole composition extract** (145 queries, ~17s apiece)
to ask three questions of one answer.
- Fix: **`apps/diagnostics/run_cache.py`** — a per-run memo table
  `run_diagnostics` installs around its rule loop and resets in a `finally`.
  `_ctx_1040`, `_readiness_check`, `_ctx_ga500`, `_ctx_8995a` consult it.
  Re-measured: **1,604 queries (−64%)**, wall −63%. Extrapolated to the
  production rate implied by the reported ~25s/packet: a packet ≈9s, ten
  packets ≈91s, inside the 120s timeout. **⚠ THAT IS ARITHMETIC, NOT A
  MEASUREMENT — time a real production run once TLS works.**
- **Safety was verified, not assumed**: rules are read-only (no rule writes
  return data; none mutates the ctx it is handed — checked across all 103
  `rules*.py`); no `ATOMIC_REQUESTS`, so a raised packet cannot poison the
  next one's transaction. The memo cannot outlive its run and one test
  exists purely to fail if that changes.
- **A 95s wall-clock budget** bounds the request: packets past it return as
  `not_evaluated` rows naming what to resubmit; the FIRST packet always
  runs. A slow DB now degrades into an honest partial 200, never a 500.
- **API note (additive)**: `counts` gained `not_evaluated`. Not-checked ≠
  failed-a-gate, and the closeout report must not conflate them.
- **NOT done, deliberately**: raising the gunicorn worker timeout — a
  production infra change and **Ken's call**. Named in the annex as the
  next lever if deferrals persist.
- ⚠ **The s268 suite EMPTIES `DiagnosticRule` per test (autouse).** It
  passed alone and failed in the full sweep two ways at once: earlier
  modules seed the built-ins from module-scoped fixtures writing OUTSIDE
  the per-test transaction, so real findings fired AND an already-seeded
  code violated the unique constraint. Same class as the s266 isolation
  chip. **The cleanup suite's docstring still claims "the test DB starts
  empty" — untrue in a sweep.**
- Regression home: `server/tests/test_batch296_s268.py` (13).

### ▶ RESUME HERE — 1040 BATCH-296, CLUSTER 3
`1040\CC Changes\CC_CODE_CHANGES_BATCH-296.md` — **33 items, OPEN, 8
closed.** The running annex in the file is the record; read it first.
Order for cluster 3:

1. **#7 — Schedule 2 double-counts a $7,288 8962 repayment.**
   ⚠ **s268 TRIAGE, DO NOT SKIP: the addend list is CORRECT.**
   `SCH2_L21_ADDENDS` is `("4","7","8","9","11","12","13","14","15","16",
   "18","19")` — it excludes `1a`/`1z`/`3`, so Part I cannot reach line 21
   through the subtotal; and `compute_8962` writes exactly ONE Schedule 2
   line, `1a`. The leak is a **third writer or a line-number collision**.
   Anyone "fixing" the tuple breaks a correct line and leaves the bug.
2. The other small compute defects: **#14** (GA spouse RIE loses exactly
   $17 — a taxpayer-owned interest row leaking across owners), **#3**
   (D_GA500_016 reports $0 excluded while D_GA500_003 in the SAME run
   reports $25,982 — check the SPOUSE column and the pre/post-recompute
   read), **#4** (8862 `part_ii` derived row never populated after import;
   the editor shows it disabled so no preparer can fix it), **#9** (8962
   monthly contribution rounding, $9).
3. **Code L (§72(p))** — Ken ruled BUILD IT PROPERLY: verify the statute
   and handle the basis consequence (taxed; loan NOT cancelled; no basis
   increase until repaid). It is the code-M/code-U trap a third time: "L7"
   will blank the whole pension column until admitted.
4. **The NOL current-year vintage fence** (s267 finding): the engine
   filters vintages for EXPIRY ONLY (`compute_form_172.py` ~238); nothing
   excludes a loss year equal to the current year. Safe today only by
   arithmetic accident (the s220 cancels-by-luck class). One fence + a
   diagnostic.
5. Then the mid-size units: #11, #12, #13, #16, #18, #20, #21, #22, #25,
   #28, #30.

**✅ KEN RULED ALL THREE OPEN QUESTIONS (2026-08-15, live).** Nothing in
BATCH-296 waits on him. (1) **#5/#8 NOL:** the engine ALWAYS computes; a
disagreeing source is a reconciliation FINDING, not a different number —
the preservation-only marker and the hard-error variant were both costed
and DECLINED. ⚠ Generalizes: where the Code makes a deduction mandatory,
the engine owns the number. (2) **Code L:** build it properly. (3) **#19
NC:** restage first, build only what genuinely fails (expect Schedule PN's
nonresident allocation alone).

⚠ **Codex's #5 symptom does NOT reproduce at HEAD** — `D_CFWD_001` cannot
fire for `nol_regular` (s254 made it engine-computed). Ask him to re-run
against the current image before building for it.

**Large units deferred to later clusters:** #10 (Form 4835), #23/#24
(Schedule C / E depreciation asset ledgers with AMT basis and property
linkage), #26 (detailed Form 8829), #27 (federal + Georgia amended-return
lifecycle). Each is a multi-session build.

### ✅ s266b — the 1120-S inbox: THREE HELD RETURNS FILED
114 / 201 / 212 each FILED federal + GA-600S via closeout on prep.
⚠⚠ **RETIRE A DIAGNOSTIC BY `is_active=False` + A KEPT STUB, NEVER BY
DELETING THE REGISTRY ENTRY** — deleting D_SCHA_007 left the seeded DB row
dangling and EVERY diagnostics run errored. ⚠ A REPLAYED batch key
silently reuses the OLD payload — bump the key when a payload changes.

**Still held in the 1120-S Inbox — SIX NEED KEN** (see
`SOURCE_DECISIONS_NEEDED.md`): 129 ($1 imbalance), 170 (GA §179 election),
180 (Lacerte negative-AAA override), 214 (mixed-entity PDF), ACECOMM +
MWELDING ($1 register diffs), CATALANC (trailer contribution).
**Two are NEXT BUILD UNITS:** 227 — the Form 6765 spec now EXISTS (only
the entity-lane section is missing); OTISUPER — the grouped bulk-sale path
needs a `business_use_pct` on DepreciationAsset.

⛔ 17a (TaxWise report) · ⛔ 17d (WO-33) unchanged.

### ✅ s266 — the seven-class charitable unit (`f8248dd`, mig 0323)
- **PROVISIONAL, on the season checklist (REVIEW_QUEUE):** the TY2026
  floor's C-before-B tiebreak and the floored-once relief are
  `requires_human_review` — **re-verify against Pub 526 (2026) + the 2026
  Schedule A instructions when they publish.** A correction is ONE
  constant (`FLOOR_ORDER`).
- REVIEW_QUEUE also holds the (G)/(A) coordination question: this build
  PRESERVED the pre-existing "own ceilings + overall 60% cap" treatment.
- ⚠ Number batches from **queue ∪ Done**; check the destination name
  before ANY move-to-archive (BATCH-001 was issued twice).

### ✅ s265 — the typecheck gate: DEAD → green (57 → 0)
- **⚠⚠ `npm run typecheck` is the ONLY valid command.** The bare `-p .`
  form is a no-op.
- **An INTERFACE is not assignable to `Record<string, unknown>`** — only a
  type ALIAS gets the implicit index signature (21 of the 57).
- ⚠ Commit messages with backticks must go through `git commit -F`.

### ✅ s264 — e-file readiness diagnostics (spine 17c)
- **The refusals ARE the spec** — the rule calls `extract_return` and
  reports its `UnmappableValue` verbatim. (s268 memoized this: all three
  `D_EFILE_*` rules now share one extract per run.)
- Status gate is `in_review` onward; a composition crash is isolated.

### ✅ THE E-FILE GAP LIST IS EMPTY (as of s242z)
What remains refused at composition is NAMED per-case, never a missing
builder.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **ONE open file — BATCH-296, 33 items, 8
  closed.** BATCH-001..007 all ✅ DONE and moved. Every worked file carries
  a result annex; read it first.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of
  10; #10 multi-state parked under the states-on-hold ruling). Unchanged.

---

## ⚠ Classes that MOVE existing returns or output on next recompute
- **s268: NO tax-output movement.** Diagnostics findings are identical —
  only the query count changes. Two BEHAVIOR changes: (1) the cleanup
  endpoint can now return `not_evaluated` rows instead of a 500, and its
  `counts` gained a fourth key; (2) an exception in one packet no longer
  fails the whole request.
- **⚠⚠ s267 MOVES FOUR CLASSES (every one a correction):** (1) a return
  whose owner has BOTH a Part-III-only Form 8606 and traditional-IRA
  1099-Rs regains the traditional taxable on 4b, in AGI, and in the GA RIE
  line-11 base; (2) any 1099-R carrying box-7 code M stops blanking the
  WHOLE pension column; (3) e-file composition now SUCCEEDS where it
  refused (every ordinary high-wage Form 8959; every 8949 row stored with
  an ISO date); (4) the published `batch-import.schema.json` accepts
  `expected.sc1040/al40/nc_d400`.
- **⚠ s257 MOVES MFS RETURNS with a GBC and line 12 > $12,500** (a
  correction) — line 13 rises to the §38(c)(6)(A) threshold unless the
  preparer answers spouse-has-no-credit.
- **⚠ s256 MOVES PRINT + E-FILE OUTPUT on NOL returns** (corrections): the
  1040 PDF gains the line-8a statement page; MeF flips 8a positive + emits
  the statement; a keyed-8a-no-detail return REFUSES e-file by name.
- **s250/s248/s246b/s255: NONE beyond new-fact reach.**
- **s249/s241j MOVE DIAGNOSTICS**: post-2018 alimony instruments fire
  `D_SCH1_007` (error) on BOTH sides.
- **⚠⚠ s243b MOVES THREE CLASSES (each a correction):** basis-only 8606 +
  IRA-path 1099-Rs; employer DCB below the plan cap; GA under-62
  disability RIE.
- **⚠ s243 / s244 / s242z-y-x / s241o** — GA 2441 IND-CR 202 feed; the
  8862 class print + e-file; amended / full-1116 / keyed-8e e-file; GA
  1099-PATR RIE L10.
- Carried from s240/s239/s236/s235: passive/PTP K-1 §1231 losses fire RED;
  Roth 1099-Rs move 5a/5b → 4a/4b; GA partnership K-1s move RIE L2↔L13;
  code-U un-blanks the pension taxable column; GA RIE line 13 on suspended
  passive K-1 losses; GA dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted
- **⚠ NEW (s268), BOTH PROVEN PRE-EXISTING at `9e5cc91` in a pristine
  worktree — neither was previously recorded:**
  - `test_1040_spine_diagnostics.py::test_gate_blanks_line_16_on_capital_loss`
    — a COMPUTE test that never touches diagnostics: `1475` where it
    expects a blank line 16. **Possible tax-output defect, FOR KEN.** My
    read: it asserts the old *bridge* behavior (a capital LOSS blanks line
    16 because the return "needs Schedule D"); Schedule D has since been
    built, so a real line 16 may now be correct and the TEST stale — the
    s240 class. It could equally be a genuine defect. Not touched.
  - `test_schedule_k1_diagnostics_leg.py::test_family_registration` — five
    K1 rules exist in the code registry but not in the test's `_FAMILY`
    constant (`D_K1_UPE_PASSIVE`, `D_K1_UPE_SE`, `D_K1_7203_GENCHAR`,
    `D_K1_SPLIT_ARITH`, `D_K1_SPLIT_8582`). Fails in 0.2s with no DB, so
    it has been red a while unnoticed. Mechanical to fix.
- **`--reuse-db` cross-module contamination**: `test_backentry_cleanup.py`
  (3, s225) and `test_mappings.py::TestApplyMappingAmbiguousFederalReturn`
  (3, s239).
- **`test_1040.py` — 6 pipeline tests**, unscoped `_fv` `.get()` (s234).
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  (s219). `test_4868.py` (4) — ⛔ KEN (s217).
  `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  PTET-gate class (s212).
- **⚠ PRE-EXISTING 1120-S defect (s241o)**:
  `test_line_key_registry_sweep.py::test_formula_targets_resolve` —
  `FORMULA_REGISTRY["1120-S"]` targets `M2_DIST_EXCESS` / `L24_BOOK_BRIDGE`,
  neither seeded. 1120-S only. Deserves its own unit.
- **Client typecheck**: green under `npm run typecheck` (s265).

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB
  (`test_postgres`), cross-repo.
- **⚠⚠ A `-k` sweep over all of `tests/` is ~5 HOURS here** (s268 measured
  3% in ~10 min). Scope a sweep to the actual blast radius instead: for a
  diagnostics change that is the **76 files matching `run_diagnostics`**
  (~22 min, 1,938 tests) — the cache is inert outside a run, so tests that
  call rule functions directly are provably unaffected.
- **⚠ NEVER pipe pytest through `Select-Object`** — it buffers the whole
  stream and a timeout loses ALL output. Redirect to a file and tail it.
- `--create-db` does not reliably drop here; prove a pre-existing red via
  `git worktree` at a pristine SHA with the main venv + copied
  `server/.env` (worked twice in s268).
- `poetry run python > file` BUFFERS (use `-u`); stdout redirects go through
  cp1252 (write UTF-8 from inside Python); **never rewrite a UTF-8 file via
  `Set-Content`/`Add-Content`** — use the Write/Edit tools or Python io.
  ⚠ `Measure-Object -Line` miscounts here — verify file edits with
  `git diff --stat`, which is authoritative.
- **`poetry run` only works from `server\`**; a script run from outside
  needs `sys.path.insert(0, r"D:\dev\delvio-tax\server")`. Windows `python`
  cannot read the Bash tool's `/tmp` — use the scratchpad.
- `manage.py seed_rules` against the pooler takes >5 min — background it.
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes are `/api/v1/tax-returns/…` with the
  trailing slash; `filing_status` is `"mfj"`.
- `_finding(...)` kwargs land under `["details"]`; `ScheduleF` has no
  `business_name` (use `principal_activity`); `order_by("owner")` puts
  "spouse" before "taxpayer" (s241w).

### 🔎 Carried for triage — NOT claims
- **From s268**: after the memo cache, **1,604 queries per run remain
  across 957 rules** — a long tail of per-rule reads, no single hot spot
  left. If throughput matters again, the levers are (a) more loaders on
  `run_cache`, (b) the gunicorn worker timeout (Ken's call).
- **From s241o**: RIE L8 alimony underived; a fuller L10 derive possible in
  principle but per-owner attribution + the (4)(b)2 gambling carve-out make
  it a design pass.
- **From s241**: `Form8606`/`HSAAccount` allow duplicate owners and their
  computes ITERATE (double-count, not vanish); browser POST unguarded.
- **From s234, potentially large**: a materially-participating 1120-S K-1's
  $250k nonpassive ordinary income never reached Schedule 1 line 5 / AGI.
  Repro: `server/tests/test_8960_line4b_clamp.py`, `_build(k1_ordinary="250000")`.
- Carried (s229): exact-tie 1040 shows `1040_SCHD_WS` clc_1/clc_3 drift on a
  bare recompute (−5,491 each), face still a tie.

### ✅ KEN DECISIONS OUTSTANDING
- **⛔ KEN (s230)**: Form 6765 Section G required for TY2026+ — re-author
  before a TY2026 season.
- **⛔ NEW (s268)**: the capital-loss line-16 red above — stale test or real
  defect? (One-line answer; see Known red.)
- **1040 v5.4 business rules still not in hand** (v5.4 schemas ARE on disk).
  ⚠ s240/s241w read the **v5.3** rules — re-check `S1-F1040-118-01` and the
  `SH-F1040-*` family against v5.4 on arrival.

### RS AGENDA
- **(s242x) The TEN staged FA definitions**: FA-1040-8853C-01..05 + FA-4562-
  DEST/ROUND/280F + FA-1040-2210-08/09 — author in RS, re-export, move from
  `flow_assertions_1040_pending.json` to the gate mirror.
- (s241b, reaffirmed s244): the `8862` spec is a draft collapsing each PART
  to one boolean — re-author per-line from the Rev. 12-2025 face. ⚠ The
  seeded app face still carries a `part_v` pseudo-line the Dec-2025
  revision DROPPED; D_EIC_016 keys on it — retire or rename in the
  re-authoring.
- (s241w): `SCHEDULE_H` is a DRAFT covering 7 of ~27 lines — re-author.
- (s241s): the GA QEE credit has NO SPEC (two carryforward regimes).
- (s241p): `4547` and `8879_TA` have NO SPEC; record `IND-476`.
- (s241o): the `500` spec has NO rule for what feeds RIE lines 1/2/6-13.
- Carried: `5329` roll-forward silence (s241); `R-8582-MULTIFORM` stale cite
  + `4797` K-1 §1231 silence (s240); `R-RET-CODE` outrun ×3 (s239); `8379`
  draft (s238); `R-SCHA-CHARITABLE` buckets + RIE-13 (s236); SCHEDULE_A
  carryover aggregation + `500` line 7a typed `input` (s235); s232/s231/s230
  items; the 1065 K-1 box-15 letters (still URGENT).

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
