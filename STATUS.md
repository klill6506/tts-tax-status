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
`1040\CC Changes\CC_CODE_CHANGES_BATCH-296.md` — **42 items, OPEN** (33 + Codex's 35-42), 14
closed.** The running annex in the file is the record; read it first.
Order for cluster 3:

1. ~~**#7**~~ ✅ **CLOSED s268 — DOES NOT REPRODUCE at HEAD.**
   `SCH2_L21_ADDENDS` excludes `1a`/`1z`/`3` (Part I cannot reach line 21);
   every Schedule 2 writer targets a distinct line and the ONLY writer of
   the repayment (1a) is `compute_8962`; and **the cited packet carries no
   `form_1095as` key at all**, so it cannot produce a $7,288 repayment (its
   resolved return is an empty draft). Proven by running the mixed
   Part-I/Part-II case the item asks for:
   `server/tests/test_batch296_item7_s268.py` (4). ⚠ **The obvious fix —
   editing the addend tuple — would have broken a correct line.**
   ⚠ Fixture traps pinned there: the 8962 suite seeds no Form 8959, and
   8959 computes on W-2 **box 5**, so a careless fixture silently degrades
   the mixed case into the Part-I-only case and "passes".
   ⚠⚠ **THIRD ITEM IN THIS BATCH THAT PREDATES THE DEPLOY** (#6 refuted,
   #19 flagged, now #7) — Codex has been asked to re-run the remaining open
   items against the current image before we build for them.
2. ~~#14, #3, #4, #9~~ ✅ **ALL FOUR BUILT s268.** Load-bearing:
   - **#14** — the GA RIE US-obligation subtraction **prorated across
     owners** when no interest row carried `treasury_interest`, shrinking a
     spouse figure the preparer supplied. Attribution is now tagged-row or
     single-owner only; ambiguous → **both bases untouched + new
     `D_GA500_020`** (seeded, 957→958 rules). ⚠ Deliberate trade-off pinned
     by test: an untagged joint return keeps the interest in the base
     (possible double exclusion, capped) rather than crossing owners.
   - **#3** — D_GA500_016 fired on EITHER owner's zero column; now judges
     the **combined** exclusion. Teeth pinned (a genuinely zero election
     still fires).
   - **#4** — nothing ever DERIVED `8862.part_ii`; compute only backfilled
     the row's existence, so D_8862_003 blocked returns the editor could
     not fix. One reader now (`f8862_eic_category_ticked`). ⚠ **CTC/AOTC
     boxes are still un-derived** — their claimed-tests live in the
     diagnostics ctx. ✅ **FINISHED 2026-08-17 on Ken's go (`1f1feac`)** —
     all three boxes derive from ONE reader
     (`compute_eic.f8862_category_claims`, called by compute AND by
     `rules_eic._f8862_categories`, with a test pinning they agree).
     ⚠ The derive MOVED out of compute_eic into the 1040 pipeline after
     `compute_sch_8812` + the 19/27/28 sync — the CTC category reads lines
     19/28 and Sch 8812 L_12, and compute_eic runs BEFORE 8812, so it would
     have ticked a pass late.
   - **#9** — ⚠⚠ **THE TITLE WAS WRONG.** Not rounding (`_round0` is
     already HALF_UP and correct) — `applicable_figure` used **float +
     Python's banker's `round()`**, and in the 300-400% band (step 0.00025)
     every ODD percentage is an exact half-way case: **29 of 100 came back
     0.0001 LOW**. Understates the required contribution → **overstates the
     credit, understates the repayment**. Now exact Decimal.
3. ~~**Code L (§72(p))**~~ ✅ **BUILT s268.** Admitted to
   `SUPPORTED_CODES` — the FOURTH time an unsupported box-7 code blanked
   the whole pension column (U s239, 6, M s267, L).
   ⚠⚠ **VERIFY-FIRST CORRECTION — three places carried one wrong fact.**
   BATCH-296, this file and the code comment all said code L is "used with
   1, 2, 4, 7, or B" and predicted an **"L7"** doc. The i1099-R Table 1
   (fetched 2026-08-17) says **"Used with code 1 or B"** — that wider list
   is code M's. **L7 IS NOT AN IRS COMBINATION**; the real pairings are L1
   and LB. The sentence had been copied from the code-M note, never read
   from the table.
   Box 2a governs (normal §72 rules), so admission was the whole fix.
   **The basis consequence:** the loan is NOT cancelled and the amount does
   NOT add to basis when deemed distributed — basis moves only on
   REPAYMENT, which is a PLAN-SIDE ledger the payer reports through box 5.
   The app holds no plan-loan basis, so there is nothing to accrue and
   nothing it may infer (recorded so nobody "fixes" the absence).
4. ~~**The NOL current-year vintage fence**~~ ✅ **BUILT s268.**
   `compute_form_172` now also requires `source_tax_year < current_year`
   (§172(b)(2): a loss carries forward only to years FOLLOWING the loss
   year), plus **`D_172_VINTAGE_FUTURE` (ERROR** — the amount is dropped
   from the deduction, so a real prior-year loss keyed with the wrong year
   must not lose it quietly; the message explains that a schedule headed
   "Carryovers from X to Y" names the year amounts carry TO, not the loss
   year). ⚠ The fence's TEETH are pinned: a genuine prior-year vintage
   must survive.
5. Then the mid-size units: #11, #12, #13, #16, #18, #20, #21, #22, #25,
   #28, #30.

### ⚠⚠ THE BATCH GREW: 33 → 42 ITEMS (Codex posted 35-42 + a 297 addendum)
Triaged s268 (2026-08-17), nothing built. **Two findings gate the work:**

1. **⛔ KEN — #35 and #42 are ONE defect with CONTRADICTORY prescriptions.**
   Same return, same $1,105 1099-MISC, same `RIE-TP-17` 5,041-vs-6,146, same
   $58. **#35 routes it to the UNEARNED base; #42 to the age-62-64 EARNED
   bucket.** ⚠ **The earned side is CAPPED at $5,000, the unearned side is
   not** — both reach 6,146 on THIS return, so the acceptance figures cannot
   distinguish them, but they diverge on any return already at the cap.
   **Our reading: #35 is right.** 1099-MISC **box 3 is "Other income"** —
   not compensation for services (box 1 rent / box 7 NEC), no SE tax — and
   s241o established for the analogous 1099-PATR case that such income is
   UNEARNED, and only when not tied to a business/farm (else double-counted
   AND moved to the capped side). Building #42 as written would put uncapped
   income under the cap. **Neither started until Ken confirms** — building
   the wrong one is worse than waiting.
2. **#37 DUPLICATES #2** (zero-tax installment sale blocked by unrecaptured
   §1250). 37 adds the Form 6251 framing + acceptance test; treat 37 as the
   live spec, don't work both.

Classification of the rest: **#36** real bug, small-med (a death date stops
the LINKED GA return recomputing — confirm whether the death date or the NOL
attributes on the same payload actually cause it); **#38** small build
(source-controlled Sch 2 line 14, follows the 1040 line-38 pattern);
**#39** medium (`collectibles_28` persists but the Sch D Tax Worksheet
ignores it, $45 — ⚠ it is a SUBSET, not extra capital income); **#40** LARGE
multi-session (AMT passive losses = an AMT shadow of Form 8582; ⚠ blocked by
`D_CFWD_001` and Ken's NOL ruling generalizes — the engine owns the number);
**#41** state-lane build on the s262b registry (SC additions/subtractions
not importable, $85 off); **297 addendum to #19** — re-verify first, #19 was
gathered inside the deploy void.

✅ **KEN RULED 2026-08-16: the next BIG unit after the small defects is
#23/#24 — the Schedule C / Schedule E depreciation asset ledgers** (AMT
basis + property linkage), ahead of #27 (amended-return lifecycle), #26
(Form 8829 detail) and #10 (Form 4835). Depreciation is the practice's
specialty and touches the largest share of returns, and #23/#24 also
unlock the OTISUPER `business_use_pct` 1120-S unit already waiting.

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

**Still held in the 1120-S Inbox — THREE NEED KEN** (was six; see
`SOURCE_DECISIONS_NEEDED.md`): 180 (Lacerte negative-AAA override), 214
(mixed-entity PDF), CATALANC (trailer contribution). *(227 needs a 6765
spec, not a source answer.)*

✅✅✅ **s268 FILED THREE: 129 + ACECOMM + MWELDING** — federal + GA,
reconciliation TIE, zero errors, no holds.
- ACECOMM/MWELDING closed under the **≤$1 SOURCE-defect rule** (the source's
  face vs its own register). ⚠ `SCHED_L_DEPR_TIE` has been a WARNING since
  batch-012 #1 (a tax register cannot error against a book balance sheet),
  so `source_verified` clears it — **the hold notes calling it
  "error-severity" were STALE, and no ≤$1 tolerance code was needed.**
- **129 needed a SECOND, different ruling.** The first pass applied the
  ≤$1 source rule (key `L15a` 40,691→40,690): it TIED but closeout correctly
  refused on `MATH_BALANCE_SHEET` (error; never acknowledgeable) — because
  129 is a different shape: the RETURN's own balance sheet did not balance,
  not just the source's presentation. **Ken then ruled generally: "if ever
  the balance sheet is out of balance $1, balance to CASH"** (DECISIONS.md).
  Beginning cash 39,856 → 39,857; both sides $40,691; **the answer key stood
  exactly as authored** and it FILED. ⚠ The check was NOT relaxed, and the
  app was NOT changed to auto-plug — silently balancing to cash would mask
  the real keying errors `MATH_BALANCE_SHEET` exists to catch.

⚠⚠ **170 IS NO LONGER A HELD PACKET — IT IS A BUILD ITEM.** Ken's ruling
points at the APP, not the source face: the printed GA-600S reports regular
depreciation only ($2,175 / $5,541, GA income $102,800) — what Georgia
sharing the full federal §179 looks like — and the payload agrees (federal
override total **2,175**, state **5,541**, §179 **57,920 on five assets, no
state §179**). Production instead computes **S7_8 $31,435 / S8_5 $3,673 / GA
income $133,928**, introducing a federal-vs-GA §179 difference the size of
the §179 itself, which HB 1199 conformity says should cancel. → **Post a CC
change: the GA-600S Schedule 7/8 adjustment must not treat federal §179 as a
Georgia difference for TY2025.** ⚠ Verify the mechanism first — the above is
the dollar signature + payload evidence, not a read of the compute path.
✅ **RULED 2026-08-16 (s268), unblocking four:** the **≤$1 class rule** —
a mismatch between a source packet's own printed face and its own register
is a SOURCE defect; record the $1 and close the packet (129, ACECOMM,
MWELDING). ⚠ Narrow by design: ≤$1 and only where the source contradicts
ITSELF — an app-vs-consistent-source gap still holds, at any amount.
And **packet 170: Georgia SHARES the federal $57,920 §179 election** per
HB 1199 conformity; the source used the stale pre-conformity limit (§179
only — GA still does not conform to §168(k) bonus). Both in DECISIONS.md.
▶ **The three ruled $1 packets and 170 are now closeable — do that first
in cluster 3** (restage/closeout, no code).
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
- **⚠⚠ s268 cluster 3 MOVES FOUR CLASSES (each a correction):**
  (1) **every Form 8962 return between 300% and 400% FPL on an ODD
  percentage** — 29 of those 100 percentages had an applicable figure
  0.0001 low, so the credit falls and the excess-APTC repayment RISES
  (money the taxpayer owes back). The largest reach of anything in this
  cluster; (2) a joint GA return with an untagged US-obligation
  subtraction stops shrinking the spouse's RIE and may now exclude more
  (with `D_GA500_020` explaining); (3) D_GA500_016 goes quiet on joint
  returns where one owner's column is empty; (4) an EIC recertification
  return gains a ticked `8862.part_ii`, clearing D_8862_003 and changing
  what the face and MeF carry — and since 2026-08-17 the SAME applies to
  CTC and AOTC recertification returns (`part_iii` / `part_iv`).
  No migration.
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
- ✅ **RESOLVED (s268, `317ded5`) — the capital-loss line-16 red.** Ken
  ruled 2026-08-16 "treat it as a stale test", and verifying before
  rewriting showed *why* it was stale: **the fixture never had a capital
  loss.** It poked `-1500.00` into line 7, which **s242q made
  ENGINE-OWNED** — with no `CapitalTransaction` row, carryover, K-1 or 4797
  gain, `schedule_d_engaged()` is False and the compute CLEARS line 7
  (leaving a stale value there had caused a real defect: a stale 114
  survived removing the last Form 8814 and then BLOCKED line 16). So the
  return is correctly taxed on wages alone; a preparer holding a manual
  Schedule D uses an OVERRIDE, which is still respected. ⚠ The $1,475 was
  DERIVED, not blessed (IRS Tax Table single, 14,250-14,300 bracket,
  midpoint 14,275 → 1,474.50 → 1,475) and the test now asserts lines
  7/11/12/15/16 so every step is checkable. Spine suite 52/52.
- **⚠ STILL RED (s268), PROVEN PRE-EXISTING at `9e5cc91` in a pristine
  worktree and not previously recorded:**
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
