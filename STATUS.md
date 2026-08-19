# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-19 (s271). **▶ 1040 BATCH-296 IS OPEN — 49 items, 23
closed.** s271 shipped **#49** (Form 4797 line 8 into the import lane) and
**#47** (the box-2b dividend aggregate, mig 0325), and TRIAGED the other five
newly-posted items (43-46, 48). ⛔ **#11 and #44 are the two things waiting on
Ken** — see KEN DECISIONS. ⛔ **#48 is blocked: Form 4136 has no Rule Studio
spec** (404 live, none cached).*

*⚠ **Both s271 deploys were VERIFIED, not assumed**: `36f9c70` (#49) went
LIVE 2026-08-19 07:43 UTC; `c889431` (#47, carrying migration 0325) was
deploying at session close — **confirm it reached `live` before trusting
#47 in production.***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.
- *s271 moved the s269 + s270 per-leg blocks into `STATUS_ARCHIVE.md` (275 lines, purely
  additive — verified by `git diff --stat`), continuing what s268 started.*

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

---

## ▶ RESUME HERE

### ✅ s271 — BATCH-296 #49: Form 4797 line 8 (`36f9c70`, LIVE)
**One allowlist entry.** `Taxpayer.f4797_nonrecaptured_1231_losses` has
existed since migration 0123 — compute (`recaptured = min(l7,l8)` → line 12
ordinary → Sch 1 line 4; `l9 = max(0, l7−l8)` → the Sch D long-term netting),
serializer, the `P4797_8` render map and `D_4797_001` were all already there.
Only `backentry.TAXPAYER_FIELDS` omitted it, so a filed line 8 could not be
transcribed and the lane overstated LTCG / understated ordinary income.
- **Staging refuses a NEGATIVE** (`_validate_f4797_lookback`) — not cosmetic:
  a negative would make `min(l7, −x)` a negative ordinary "gain" on line 12
  and ADD to the Sch D LTCG. Zero and absence both stage.
- Acceptance: l7 32,062 / l8 5,544 / l9 26,518 → Sch 1 L4 5,544; with ST/LT
  carryovers 32,755 / 33,735 the Sch D face nets (32,755) / (7,217) /
  (39,972) / (3,000) and carries ST 29,755 / LT 7,217. Driven stage → commit
  → reopen.
- Regression: `server/tests/test_batch296_item49_s271.py` (10). Gates: 614 +
  507 (every backentry suite). Teeth proven by injecting both defects.
  **No migration.**

### ✅ s271 — BATCH-296 #47: the box-2b dividend aggregate (`c889431`, mig 0325)
`div_unrecap_1250_agg` — the third member of the `div_qualified_agg` /
`div_capgain_dist_agg` family, same valve (used ONLY when no payer row carries
box 2b, so per-row always wins and it can never feed twice).
- **⚠⚠ THE AMOUNT CHANGES THE ROUTE, NOT JUST A LINE.** Line 19 = 0 → the
  QDCGT worksheet; nonzero → the Schedule D Tax Worksheet
  (`schedule_d_route`). The omission computed all of line 16 by a *different
  worksheet* — which is why the reported gap was a clean $82.
- **⚠ THE RATE IS NOT 25%.** Unrecaptured §1250 gain is taxed at the ORDINARY
  rate, CAPPED at 25% (§1(h)(1)(D)). The fixture is at 24%, so the $905 the
  worksheet can reach leaves the 15% group and is taxed at 24%: +217 ordinary
  − 136 fifteen-percent = **$81**. My first pin assumed a flat 25% and was
  WRONG; the suite now asserts the mechanism step by step.
- **⚠ THE SINGLE SOURCE IS THE POINT.** Box 2b has three consumers that must
  agree — Sch D ENGAGEMENT, the §1250 worksheet line 11 → Sch D line 19, and
  the Exception-1 block test. An aggregate wired only into the worksheet would
  compute a line 19 on a return Schedule D never engaged for. A test pins that
  the aggregate ALONE engages Schedule D.
- The item's "reject or diagnose a conflicting aggregate plus payer total" is
  a staging **warning**: the double count is structurally impossible, so the
  only question is which figure was mistranscribed (the #38 precedent —
  pin an impossible risk rather than guard against it).
- Regression: `server/tests/test_batch296_item47_s271.py` (10). Gates: 536 +
  267 (Sch D / int-div / SDTW / 8814 / 4952) + 68. Teeth proven by reverting
  the 2b wiring to the bare row sum (3 pins fell, engagement among them).

### ▶ THEN — 1040 BATCH-296, CLUSTER 4
`1040\CC Changes\CC_CODE_CHANGES_BATCH-296.md` — **49 items, OPEN, 23
closed.** The running annex in the file is the record; read it first.

**▶ NEXT:** **#46** (the smallest of the new ones — see the triage below),
then the mid-size 1040 units **#12, #13, #15, #16, #17, #18, #20, #21, #22,
#25, #28, #30**, then Ken's ruled next big unit **#23/#24** (the depreciation
asset ledgers).

⚠ **#37 duplicates #2** — treat 37 as the live spec; do not work both.
⛔ **#40 is a LARGE multi-session build** (AMT passive losses = an AMT shadow
of Form 8582) and belongs after the big units.

### ⚠ s271 TRIAGE of the seven new items (43-49) — full evidence in the batch annex
| Item | Verdict |
|---|---|
| 43 | **Confirmed at HEAD, both halves** (`schedule_e_p2_totals_from_rows` consumes `passive_8582_allowed` only in the `net<0` branch; the gather `continue`s past every material-participation K-1). Medium-LARGE — half of it is a **new former-passive §469(f) regime**. Sequence with the big units. |
| 44 | ⛔ **KEN — do not build yet.** Ken's 2026-08-16 **≤$1 source-defect rule** likely already answers it (the source's own columns cannot reproduce its own printed (h) after cents were rounded away). Building the override would add an input whose only job is to reproduce arithmetic the source cannot show. |
| 45 | **Confirmed, but two different kinds of work.** The **Form 8960 half is a plain defect** (`schedule_e_non_1411_income` keys on the single `material_participation` boolean, which cannot represent a mixed row — the **s267 class**). The **8582 half is a documented v1 DEFERRAL** (s242t) that `D_K1_SPLIT_8582` already warns about; lifting it is a design change and must retire/narrow that rule in the same pass (s225). |
| 46 | **HALF ALREADY BUILT — the #39 pattern.** `compute_2441_lines` already takes a separate `dcb_expenses`, the caller already passes it, and `_gather_2441_inputs` already computes it over a **different claim-blind population**. Missing: only the **Part III line-16 SOURCE fact**. Small. |
| 47 | ✅ **BUILT s271.** |
| 48 | ⛔ **BLOCKED — Form 4136 has NO Rule Studio spec** (`/api/forms/lookup/4136/export/` → 404 live 2026-08-19; nothing cached in `server/specs/`). Per the mandatory RS rule we do not improvise a form. **RS agenda.** |
| 49 | ✅ **BUILT s271.** |

---

## ⚠ Classes that MOVE existing returns or output on next recompute
- **✅ s271 #49 and #47 MOVE NOTHING that exists today.** Both new facts are
  absent/NULL on every existing row, and no compute path changed for a return
  that does not carry them. ⚠ But when a payload DOES carry
  `div_unrecap_1250_agg`, note it can change the tax **ROUTE** (QDCGT → the
  Schedule D Tax Worksheet), not just line 19.
- **⚠⚠ s270 #41 MOVES EVERY SC RETURN with taxable income in [$7,000,
  $100,000)** — up to ±$3 of SC tax on next recompute, a CORRECTION toward
  the published SC1040TT (the $50-row assumption was wrong above $7,000).
- **s270 #39 MOVES a class only a new import can create**: a return with a
  nonzero K-1 `collectibles_28` gains Schedule D line 18 and, where the 28%
  rate group binds, more line-16 tax. Zero such returns exist in the shared
  DB today.
- **⚠⚠ s270 #36 MOVES A CLASS: every GA-home-address 1040 with no GA-tagged
  document and no GA-500 gains an auto-attached Georgia return on its next
  save or import.** For GA-resident retirees that is the missing state filing
  — a correction, not a regression.
- **✅ s270 #38 MOVES NOTHING** — `sch2_l14_source_amount` is NULL everywhere.
- **⚠⚠ s269 MOVES DIAGNOSTICS ON EVERY W-2G RETURN, and it will look like a
  regression.** `D_W2G_PAYER_ID` (error) fires on any W-2G row missing the
  payer name, a nine-digit EIN, or the full US payer address — essentially
  every already-committed W-2G return, because none of those fields was
  importable until then. **Those returns genuinely could not have been
  e-filed.** It clears by keying the payer block or re-importing.
  **No tax-output movement.**
- **⚠⚠ s268 cluster 3 MOVES FOUR CLASSES (each a correction):**
  (1) **every Form 8962 return between 300% and 400% FPL on an ODD
  percentage** — the applicable figure was 0.0001 low, so the credit falls
  and the excess-APTC repayment RISES. The largest reach of the cluster;
  (2) a joint GA return with an untagged US-obligation subtraction stops
  shrinking the spouse's RIE (`D_GA500_020` explains); (3) D_GA500_016 goes
  quiet on joint returns where one owner's column is empty; (4) an EIC/CTC/
  AOTC recertification return gains its ticked 8862 part.
- **⚠⚠ s267 MOVES FOUR CLASSES (every one a correction):** a Part-III-only
  8606 + traditional-IRA 1099-Rs regains the taxable on 4b; box-7 code M
  stops blanking the whole pension column; e-file composition now succeeds
  where it refused (8959 high-wage; ISO-dated 8949 rows); the published
  schema accepts `expected.sc1040/al40/nc_d400`.
- **⚠ s257 MOVES MFS RETURNS with a GBC and line 12 > $12,500** (a
  correction) — line 13 rises to the §38(c)(6)(A) threshold unless the
  preparer answers spouse-has-no-credit.
- **⚠ s256 MOVES PRINT + E-FILE OUTPUT on NOL returns** (corrections).
- **s249/s241j MOVE DIAGNOSTICS**: post-2018 alimony instruments fire
  `D_SCH1_007` (error) on BOTH sides.
- **⚠⚠ s243b MOVES THREE CLASSES (each a correction):** basis-only 8606 +
  IRA-path 1099-Rs; employer DCB below the plan cap; GA under-62
  disability RIE.
- Carried from s240/s239/s236/s235: passive/PTP K-1 §1231 losses fire RED;
  Roth 1099-Rs move 5a/5b → 4a/4b; GA partnership K-1s move RIE L2↔L13;
  code-U un-blanks the pension taxable column; GA RIE line 13 on suspended
  passive K-1 losses; GA dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted
- **`--reuse-db` cross-module contamination**: `test_backentry_cleanup.py`
  (3, s225) and `test_mappings.py::TestApplyMappingAmbiguousFederalReturn`
  (3, s239).
- ⚠ **NEW (s271): `test_backentry_oos_states_s258.py::TestCleanupDisposition`
  (2) fails in a `-k backentry` sweep and PASSES alone** — the same
  cross-module isolation class. Not caused by s271 (both cleared in isolation
  and the suite is untouched by this session's changes).
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
- **⚠⚠ A `-k` sweep over all of `tests/` is ~5 HOURS here.** Scope a sweep to
  the actual blast radius (a diagnostics change = the 76 files matching
  `run_diagnostics`, ~22 min).
- **⚠ NEVER pipe pytest through `Select-Object`** — it buffers the whole
  stream and a timeout loses ALL output. Redirect to a file and tail it.
- ⚠ **NEW (s271): after restoring a file from a `.bak`, BUMP ITS MTIME and
  drop the stale `__pycache__` `.pyc`** — a restore that leaves an older
  mtime than the cached bytecode makes pytest keep running the INJECTED code
  and reads as "the restore failed".
- ⚠ **NEW (s271): a `poetry run python -c @"..."@` heredoc can silently NOT
  RUN.** Write the script to the scratchpad and run the FILE; verify the
  effect before trusting it.
- `--create-db` does not reliably drop here; prove a pre-existing red via
  `git worktree` at a pristine SHA with the main venv + copied `server/.env`.
- `poetry run python > file` BUFFERS (use `-u`); **never rewrite a UTF-8 file
  via `Set-Content`/`Add-Content`** — use the Write/Edit tools or Python io.
  ⚠ `Measure-Object -Line` miscounts here — verify with `git diff --stat`.
- **`poetry run` only works from `server\`**. Windows `python` cannot read the
  Bash tool's `/tmp` — use the scratchpad.
- `manage.py seed_rules` against the pooler takes >5 min — background it.
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes are `/api/v1/tax-returns/…` with the
  trailing slash; `filing_status` is `"mfj"`.
- `_finding(...)` kwargs land under `["details"]`; `ScheduleF` has no
  `business_name` (use `principal_activity`); `order_by("owner")` puts
  "spouse" before "taxpayer" (s241w).

### 🔎 Carried for triage — NOT claims
- **From s268**: after the memo cache, **1,604 queries per run remain across
  957 rules** — no single hot spot left. Levers: more loaders on `run_cache`,
  or the gunicorn worker timeout (Ken's call).
- ⚠ **The ~9s/packet production figure is still ARITHMETIC, not a
  measurement.** Time a real production run when convenient.
- **From s241o**: RIE L8 alimony underived; a fuller L10 derive needs a design
  pass (per-owner attribution + the (4)(b)2 gambling carve-out).
- **From s241**: `Form8606`/`HSAAccount` allow duplicate owners and their
  computes ITERATE (double-count, not vanish); browser POST unguarded.
- **From s234, potentially large**: a materially-participating 1120-S K-1's
  $250k nonpassive ordinary income never reached Schedule 1 line 5 / AGI.
  Repro: `server/tests/test_8960_line4b_clamp.py`, `_build(k1_ordinary="250000")`.

---

## ⛔ KEN DECISIONS OUTSTANDING

### BATCH-296 #11 — BOTH HALVES CONFLICT WITH AUTHORITY (s270, not built)
Two decisions, one item. The fixture batch stays uncommitted; full evidence
in the batch annex.
1. **Should an 1120-S K-1 `se_retirement_amount` flow to Schedule 1 line 16?**
   The filed return took $5,031 there. **Pub 560: a shareholder-employee is
   NOT self-employed — the CORPORATION deducts the contribution on the
   1120-S.** **Recommendation: NO feed; add a diagnostic.** Cost if built as
   asked: encoding a Pub 560 error on every S-corp owner packet.
2. **GA RIE: does materially-participating S-corp income stay EARNED
   (capped $5,000)?** The filed $35,000 exclusion is only reachable with it
   UNEARNED — contrary to Ga. Comp. R. 560-7-4-.02(4)(b)1, which s239
   litigated and you ratified. **Recommendation: keep the reg routing; treat
   the filed figure as source-side error.**

### BATCH-296 #44 — does the ≤$1 source rule already cover it? (s271, not built)
A filed Form 8949 summary whose printed columns (d)/(e)/(g) cannot reproduce
its own printed column (h), because the broker's cents were rounded away
($(34) printed, $(35) computed). **Your 2026-08-16 ≤$1 class rule appears to
answer this: a source packet contradicting ITSELF by ≤$1 is a SOURCE defect —
record it and close.** Codex asks instead for a new preparer-assertable
override. **Recommendation: apply the ≤$1 rule; do not build the override.**
One question: which?

### Carried
- **⛔ KEN (s230)**: Form 6765 Section G required for TY2026+ — re-author
  before a TY2026 season.
- **1040 v5.4 business rules still not in hand** (v5.4 schemas ARE on disk).
  ⚠ s240/s241w read the **v5.3** rules — re-check `S1-F1040-118-01` and the
  `SH-F1040-*` family against v5.4 on arrival.
- **1120-S Inbox still holds THREE for Ken** (see `SOURCE_DECISIONS_NEEDED.md`):
  180 (Lacerte negative-AAA override), 214 (mixed-entity PDF), CATALANC
  (trailer contribution). *(227 needs a 6765 spec, not a source answer.)*
  ⚠ **170 is a BUILD ITEM, not a held packet** — the GA-600S Schedule 7/8
  adjustment must not treat federal §179 as a Georgia difference for TY2025
  (HB 1199 conformity). Verify the mechanism first.
  ⛔ 17a (TaxWise report) · ⛔ 17d (WO-33) unchanged.

### RS AGENDA
- ⚠ **NEW (s271): Form 4136 has NO SPEC** — blocks BATCH-296 #48 entirely.
- (s270) `w28_4`'s line-map note, `R-K1-RED-DEFER`, and `FA-1040-K1-07`'s
  `blockers` all still name `collectibles_28` as deferred; the FA gate carries
  a `__COMPUTES__` sentinel until the amendment lands.
- (s270) the RS `SC1040` scenario still pins **2,360** — the published table
  says **2,361**.
- (s270) the NC `D400` spec defines no part-year residency DATES.
- **(s242x) The TEN staged FA definitions**: FA-1040-8853C-01..05 + FA-4562-
  DEST/ROUND/280F + FA-1040-2210-08/09.
- (s241b, reaffirmed s244): the `8862` spec collapses each PART to one
  boolean — re-author per-line from the Rev. 12-2025 face. ⚠ The seeded app
  face still carries a `part_v` pseudo-line the Dec-2025 revision DROPPED.
- (s241w): `SCHEDULE_H` is a DRAFT covering 7 of ~27 lines.
- (s241s): the GA QEE credit has NO SPEC. (s241p): `4547` / `8879_TA` none.
- (s241o): the `500` spec has no rule for what feeds RIE lines 1/2/6-13.
- Carried: `5329` roll-forward silence; `R-8582-MULTIFORM` stale cite +
  `4797` K-1 §1231 silence; `R-RET-CODE` outrun ×3; `8379` draft;
  `R-SCHA-CHARITABLE` buckets + RIE-13; SCHEDULE_A carryover aggregation +
  `500` line 7a typed `input`; the 1065 K-1 box-15 letters (still URGENT).

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
