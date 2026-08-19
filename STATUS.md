# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-18 (s270). **▶ 1040 BATCH-296 IS OPEN — 42 items, 21
closed + the 297 addendum ANSWERED, cluster 4's small/mid defect list is
DONE, and ⛔ #11 IS FLAGGED FOR KEN (both halves conflict with authority —
Pub 560 and the GA reg; see KEN DECISIONS).** s270 shipped **#38** (Sch 2 line 14 source lane), **#36** (GA
residency attach), **#39** (K-1 collectibles → the 28% rate group), **#41**
(the SC contract — whose acceptance FLUSHED OUT A REAL SC TAX-TABLE DEFECT:
the published rows are $100 wide from $7,000 up; every SC return in [$7k,
$100k) moves ±$3), and **closed the 297 addendum to #19** (all three NC
retests were keyed COMPUTED lines — zero code, 8 acceptance tests, one RS
agenda item for the residency-date metadata).*

*s269 shipped **#34** (the W-2G payer identity, `b04a73f`) and appended the
**#35 annex s268 never wrote** (the build landed 4 minutes after the file's
last write).*

*✅ **DEPLOY `25a462b` WENT LIVE 2026-08-17 07:55 UTC**, carrying all of
s268 including #35. `b04a73f` (#34) pushed and deploying behind it.*

*⚠ **s269 ADDS A NEW ERROR THAT WILL LOOK LIKE A WAVE** — `D_W2G_PAYER_ID`
fires on essentially every already-committed W-2G return, because the payer
EIN was never importable until now. That is the true state of those returns
(they could not have been e-filed); it is not a regression. See the
classes-that-move section.*

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

### ✅ s269 — BATCH-296 #34: the W-2G payer identity (`b04a73f`)
**⚠⚠ VERIFY-FIRST CHANGED THE SIZE OF THE ITEM.** It reads as "expose and
persist the W-2G payer TIN and identity fields" — a model + schema + plumbing
build. It was **one allowlist**. `FormW2G.payer_ein` / `payer_street` /
`payer_city` / `payer_state` / `payer_zip` / `box14_state_winnings` have
existed since **migration 0196**; the serializer exposes them, the Slate
misc-income screen edits them, and `_extract_w2gs` already READS every one to
build `<IRSW2G>`. Only `backentry.W2G_FIELDS` omitted them — while the sibling
`R1099_FIELDS` carries the identical block. **No migration, no new columns, no
new UI.** ⚠ The generalizable trap: *a payer-identity block that drifts from
`R1099_FIELDS` is invisible until e-file runs.* A test now fails if any
`payer_*` key exists there and not on W-2G.
- **The payer ADDRESS travels with the EIN.** The IRSW2G payer US address is
  schema-required and `read_model` refuses on the very next line, so admitting
  the EIN alone would have moved the refusal one line down and left the
  document just as untransmittable. ⚠ **Open question put to Codex:** the
  three fixtures quote an EIN but no payer address — if the source does not
  print one, those packets will now stop on the address instead.
- **⚠⚠ MISSING vs MALFORMED IS THE DESIGN.** A malformed EIN is refused at
  **staging** (`_validate_w2g`; both `58-2025627` and `582025627` accepted). A
  **missing** one still stages — refusing it would fail a whole ten-return
  batch over a field the preparer keys in the editor, and a source that does
  not print an EIN is not a defective payload. The missing case is the new
  **`D_W2G_PAYER_ID`** (error, seeded), reported **per row at every status**,
  naming the row, its owner and which fields are absent.
- That is **s264's principle in the other direction**: `D_EFILE_001` was
  already right, it just speaks only from `in_review` and names ONE refusal
  for the whole return. ⚠ A test pins that **"row 2" means the same row in
  both messages** (both enumerate the queryset under `Meta.ordering`).
- **The recipient identity was already correct and is now FENCED** — derived
  from `owner` + the return identity, so nothing was added to the payload and
  a test FAILS if anyone adds `recipient_ssn`/`ssn`/`recipient_name` to the
  allowlist. A second copy of an SSN in a PII lane is a second thing to get
  wrong.
- **The compound refusal is split.** One refusal covered four facts, so a
  blank EIN and a mis-keyed 8-digit one gave the SAME sentence — and it named
  "recipient SSN" as though it were a W-2G field, sending the fix to the wrong
  screen. One message per field now; the EIN message quotes what it found.
- ⚠ **Deliberately NOT widened:** `int_1099s`/`div_1099s`/`r_1099s` also carry
  an unvalidated `payer_ein`. Tightening a PUBLISHED contract can reject
  packets that stage today — its own change, not a side effect of this one.
- Regression: `server/tests/test_batch296_item34_s269.py` (33). Gates: 850
  green. No migration.

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

### ✅ s270 — the 297 addendum to #19: three NC retests, ZERO code
**All three were the #41 class — keyed COMPUTED lines — and the engine was
already right.** The nonresident retest keyed `fields["13"]` (computed from
`pn-a`/`pn-b` — the $476); the resident retest keyed lines 9/11 as totals
(computed from `ded-other`/`sa-*`/`DED-ELECT` — the $2,676); the part-year
fixture keyed seven computed lines and NEVER keyed line 20 (the whole of the
"dropped" $429 withholding). All three filed fact patterns tie through the
component keys, pinned: NR 0.7774 → tax 1,662/due 16; resident itemized →
4,478/refund 4,747; part-year 0.5000 → 1,270/due 841 (+ commit/reopen
persistence). The #41 staging warnings already name their exact mistakes.
⚠ The full-year control doubles as the honest note: the "wrong" unallocated
figures ARE correct for a full-year resident. ⚠ DEFERRED with reason:
part-year residency DATES — the RS `NC_D400` spec (21 lines) defines none,
and unspecced face lines are the improvise path we refuse. **RS agenda.**
Tests: `test_batch296_item19_addendum_s270.py` (8); teeth by injecting the
reported symptom (ratio ignored → three pins fail).

### ✅ s270 — BATCH-296 #41: the SC contract + THE TABLE DEFECT UNDER IT
**Part 1 — verify-first: the components were importable ALL ALONG.** The SC
engine has carried `add-oth` (→ line 2) and `ltcg` (full gain; engine × 44%
→ line i → 4) since s262b, and `_apply_state_fields` accepts any seeded
line. What was missing was DISCOVERABILITY: the schema's prose said key "the
printed face lines" (→ Codex keyed the COMPUTED totals, silently recomputed
from empty components — the $85), unknown keys refused only at COMMIT, and
nothing documented computed-vs-input. Fixed at the contract level,
registry-first: `state_registry.line_vocabulary()` (from the seeders'
SECTIONS — s262c's control-lines rule), staging refuses unknown state line
keys PER KEY + WARNS on keyed computed lines naming the remedy, and the
schema now carries `$defs.state_line_vocabulary` + per-state propertyNames
enums + corrected prose naming add-oth/ltcg.
- **⚠⚠ Part 2 — THE ACCEPTANCE FAILED BY $1 AND EXPOSED A REAL TAX-TABLE
  DEFECT.** Engine $3,626 vs filed $3,627 on $71,136. Fetched the published
  SC1040TT_2025 from dor.sc.gov rather than argue: **rows are $50 wide only
  below $7,000; $100 wide from $7,000 to $100,000.** s262b assumed $50
  everywhere — its "verified 138/138" parse never reached the $100 region —
  so EVERY SC return in [$7,000, $100,000) taxed off the wrong midpoint
  (±$3). `_sc1040tt` corrected + verified against ALL 1,070 published rows
  (zero mismatches); 15 rows spanning both seams pinned in the suite.
- ⚠ **THREE STALE PINS FELL WITH IT**: our $50k scenario (2,360 → published
  **2,361**), $75k (3,860 → **3,861**) — and ⚠ **the RS spec's SC1040
  scenario still says 2,360** (RS agenda). ⚠ Codex's dry-run control
  ($3,542/$231) was itself computed BY the broken table — the published row
  says $3,543/$230, which the control now pins.
- **⚠⚠ MOVEMENT CLASS — REAL AND WIDE**: every SC1040 with taxable income
  in [$7,000, $100,000) may move up to ±$3 on next recompute (a correction
  toward the published table). AL/NC untouched.
- Regression: `server/tests/test_batch296_item41_s270.py` (9). Gates: **634
  green** (s262c state-lane suite, SC compute suite, flow assertions,
  backentry commit). Teeth twice: the acceptance itself failed pre-table-fix;
  disabling the staging check fails both contract pins. No migration.

### ✅ s270 — BATCH-296 #39: K-1 collectibles → the 28% rate group
**THE FIX WAS ONE FEED, AND THE PARAMETER HAD BEEN WAITING BY NAME.**
`compute_28pct_worksheet`'s line-4 argument is literally named
`div_2d_plus_k1` and the RS `w28_4` routes "K-1" there — but the call site
never fed the K-1 term, so `schedule_k1s.collectibles_28` persisted with no
consumer, line 18 stayed 0, the route stayed QDCGT, and line 16 ran $45 low
(the 28%-vs-QDCGT spread on $500). Now `k1_collectibles_28_total` → w28 L4 →
Schedule D line 18 → the SDTW 28% rate group. **A SUBSET, not income** — box 9b
re-labels part of box 9a (already in line 12); the subset control pins that
lines 15/16/7/AGI/taxable are IDENTICAL with and without it, and line 16 tax
moves +$45 exactly.
- **Acceptance through the REAL lane** (stage+commit, the dry-run's path),
  the filed face to the dollar: AGI 277,598 · taxable 239,208 · 16 = 43,009 ·
  24 = 48,765 · 37 = 14,016 · 38 = 407 · Sch D 16 = 652 · 18 = 500. The
  omitted control reproduces the report: 18 = 0, 16 = 42,964.
- **⚠ D_K1_SPECIAL_GAIN NARROWED to unrecap-§1250-only** (s225
  both-in-one-pass): firing prepare-manually on collectibles the engine now
  computes would be wrong. The §1250 half stays RED (its K-1 feed into the
  §1250 worksheet is still unbuilt). ⚠ Its docstring's "the SDTW is
  deferred" was STALE — compute_sdtw has been live since Topic 9. Re-seeded.
- **⚠ Latent double-count hazard, checked not guarded**:
  `schd_other_collectibles` (the old spec's K-1 path) feeds the same W[4]. A
  return carrying the gain in both places would double line 18. **Zero rows
  in the shared DB carry either field nonzero (read-only check 2026-08-18)**;
  recorded at the call site.
- **RS AGENDA — three spec items now trail the build**: `w28_4`'s line-map
  note ("the K-1/other fact"), `R-K1-RED-DEFER`, and `FA-1040-K1-07`'s
  `blockers` list all still name collectibles_28 as deferred. The
  flow-assertion gate carries an explicit `__COMPUTES__` sentinel until the
  RS amendment lands; the sentinel then retires.
- Regression: `server/tests/test_batch296_item39_s270.py` (5). Gates: **675
  green** (flow assertions, Sch D spec suite, SDTW §1250 suite, INT/DIV
  scenarios, K-1 diagnostics leg, backentry commit). Teeth proven by
  reverting the feed (acceptance + subset control fail = the $42,964 repro).
  No migration.

### ✅ s270 — BATCH-296 #36: the GA residency attach trigger
**⚠⚠ THE ITEM'S DIAGNOSIS WAS WRONG ON BOTH COUNTS — the death date and the
NOL rows were BOTH innocent.** Nothing in the GA path reads a death date
(grep: zero references), and the NOL vintages compute a $0 deduction on this
shape (MTI ≤ 0) and preserve untouched. **The real cause: the GA-500 was
NEVER ATTACHED.** `_auto_sync_ga500` attached only on a GA-*tagged* income
document (W-2 state row / 1099-R / INT / DIV state boxes — the s106b
widening), and this GA-resident decedent's INT/DIV rows carry NO state boxes
(zero state withholding in the whole packet). No tagged doc → no GA return →
every GA expectation reconciled against zeros. The QA Batch-001 retiree class
ONE LAYER DEEPER: s106b widened W-2s→tagged documents; a resident with
UNTAGGED documents still fell through.
- **Fix: `_has_ga_home_address`** — a GA home address now attaches, both
  lanes (commit runs the same auto-sync). ⚠ A SEPARATE predicate on purpose:
  `_has_ga_source_document`'s "mirrors line-24 exactly" contract is
  load-bearing — residency is an attach question, never a withholding
  source. A test pins that separation.
- **Acceptance ties the filed GA return line for line** (synthetic twin, real
  dollars): GA 8 = 16,803 · RIE-TP-17 = 15,981 (= 17,696 int + 1,285 div −
  3,000 cap loss — the s182 "Capital gains (LOSSES)" pull, sign preserved) ·
  S1-8 = 836 · S1-13 = 16,817 · GA AGI = −14 · tax 0. Controls: a LIVING
  twin is identical; NO-NOL twin identical; an SC address attaches nothing.
- **⚠ THE ANSWER KEY MIS-KEYED ONE LINE AND THE ECHO INVITED IT** —
  `expected.ga500["S1-7"]=16817` put the printed TOTAL subtractions on the
  RIE-only line (correctly 15,981); the echo had no total-subtractions key,
  so the total had nowhere else to go. **S1-13 joined `GA500_SUMMARY_LINES`**
  (schema regenerated); the annex tells Codex how to re-key. Expect the S1-7
  row to no-tie on an unchanged re-stage — every other GA line ties.
- Regression: `server/tests/test_batch296_item36_s270.py` (9). Gates: 633
  green incl. flow assertions, backentry commit, the GA-500 band, the s106
  auto-attach suite (document trigger intact), GA RIE retirement suite.
  Teeth proven by reverting the trigger (all four attach-dependent tests
  fail = the repro). No migration.

### ✅ s270 — BATCH-296 #38: the source-controlled Schedule 2 line 14
**⚠ VERIFY-FIRST CUT THE ITEM ROUGHLY IN HALF — TWO of the four acceptance
criteria were ALREADY TRUE at HEAD.** `"14"` has always been in
`SCH2_L21_ADDENDS` (so line 14 → 21 → 1040 23 → 24, exactly once) and
`f1040s2_2025.FIELD_MAP` already maps it — **the render leg needed nothing.**
The real gap was the one the item's third sentence named: no importable source
field. *(Third item running: #34 was one allowlist, #7 did not reproduce, #38
was half-built already. The batch's size estimates run high.)*
- **`Taxpayer.sch2_l14_source_amount` / `_label` / `_note`** (mig **0324**),
  the Form 2210 documented-source shape. **NULLABLE on purpose**: blank means
  "leave line 14 to the preparer", a different fact from a source asserting
  **$0**; a 0 default would make the two indistinguishable.
- **Lane AND browser refuse an undocumented amount** —
  `_validate_sch2_l14_source_trio` + `TaxpayerSerializer`, so the browser
  cannot save what the lane refuses. `batch-import.schema.json` regenerated.
- **⚠⚠ LINE 14 IS A TWO-WRITER LINE — the whole design turns on this.** Unlike
  2210 line 38, it is a DIRECT-ENTRY line a preparer can already key. The write
  is scoped to *"only while a source is recorded"* — the `compute_6252` line-15
  shape, **NOT** the Schedule 2 line-13 feeder shape, which writes
  unconditionally and here would have silently zeroed a keyed figure on the
  next recompute (a DISAPPEARED number — nobody reports what nobody sees go).
- The other half: `_write_line` honours an override, so an overridden line 14
  files the OVERRIDE and discards the source. Legitimate, but never silent —
  that is **`D_6252_010`** (warning). **`D_6252_009`** (warning) is the
  source-verified advisory: names the amount + label and says plainly that the
  app computes no §453(l)(3) figure and nothing checks it. **Both warnings,
  never errors** — the item asked for acknowledgeable, and severity is the
  whole of that promise.
- **⚠ The item's feared "silent double count" is STRUCTURALLY IMPOSSIBLE, so it
  is PINNED rather than guarded against** — the source WRITES, never adds, and
  there is no second writer (the live RS `SCH_2` export types line 14 `input`,
  no calculation; fetched live, agrees with the cached copy). Cite verified at
  LII: **§453(l)(3)**, on the §453(l)(2)(B) timeshare/residential-lot
  dispositions.
- **⚠⚠ `D_6252_009/010` DELIBERATELY BYPASS `rules_6252._state()`.** Every other
  rule there no-ops unless `form_6252_engaged` — correct for them, fatal here:
  the premise is a packet printing line 14 with NO Form 6252. Gating them "for
  consistency" would silence them on exactly the return they exist for. The
  module docstring now says so and a test pins it.
- Regression: `server/tests/test_batch296_item38_s270.py` (21). **Gates: 1,046
  green** (flow assertions, Sch 1/2/3 scenario + render + seed + diagnostics,
  6252 compute, the 2210 source suite, backentry schema-gen + commit, full
  diagnostics + 1040 spine diagnostics). Migration **0324**.
- ⚠ **The suite was green on its FIRST run, so two defects were injected to
  prove teeth** (unconditional line-14 write; gating the advisory on
  `form_6252_engaged`). Both targeted pins failed as designed, plus one
  collateral; all green again on restore.
- ⚠ **Mig 0324 also carries a PRE-EXISTING un-migrated `AlterField` on
  `scha_charitable_carryover_in`** that s266 left behind — **help_text only, no
  schema change** — which `makemigrations` swept in. Not this build's.
- ⚠ **s270 was interrupted mid-build and resumed.** Leg 1 (`74bcfb9`) was
  committed deliberately INERT — the field existed but nothing could set it.
  That is no longer true: the lane is open as of this session's second commit.

### ▶ THEN — 1040 BATCH-296, CLUSTER 4
`1040\CC Changes\CC_CODE_CHANGES_BATCH-296.md` — **42 items, OPEN, 21
closed.** The running annex in the file is the record; read it first.

**▶ NEXT:** the mid-size 1040 units **#12, #13,
#16, #18, #20, #21, #22, #25, #28, #30** (⛔ #11 flagged for Ken — see KEN DECISIONS) — then Ken's ruled next big unit
**#23/#24** (the depreciation asset ledgers).

⚠ **#37 duplicates #2** — treat 37 as the live spec; do not work both.
⛔ **#40 is a LARGE multi-session build** (AMT passive losses = an AMT shadow
of Form 8582) and belongs after the big units, not in this cluster.

*Cluster 3 (all closed, s268):*

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

### ⚠⚠ THE BATCH GREW: 33 → 42 ITEMS (Codex posted 34-42 + a 297 addendum)
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
   income under the cap. ✅ **KEN RULED 2026-08-17: UNEARNED** — *"It's why we
   put it on Box 3. If it were earned we would put it on Schedule C."*
   **BUILT (`ac2b289`)**: `compute_1099misc.ga_rie_other_income_by_owner`
   feeds RIE **L10** (other income), 8z-ROUTE ONLY — a row routed to a
   Schedule C/F is earned and already inside that schedule's net profit on
   L2, so feeding it here would double-count AND move it to the capped side
   (load-bearing negative test). ⚠ **Box 8 deliberately excluded** (a
   dividend substitute — different character, separate question).
   ⚠ A test pins the divergence the acceptance figures could not show:
   at the $5,000 cap the earned reading gives $5,000, unearned $6,105.
2. **#37 DUPLICATES #2** (zero-tax installment sale blocked by unrecaptured
   §1250). 37 adds the Form 6251 framing + acceptance test; treat 37 as the
   live spec, don't work both.

Classification of the rest: ~~**#36**~~ ✅ **BUILT s270** (⚠ the death date AND the NOL attributes were
both innocent — the GA-500 was never ATTACHED: no state-tagged document
anywhere in a GA-resident retiree packet; fixed with the residency-by-address
trigger); ~~**#38**~~ ✅ **BUILT s270**
(source-controlled Sch 2 line 14 — ⚠ the "follows the 1040 line-38 pattern"
framing was only half right: line 38 is engine-computed, line 14 is
DIRECT-ENTRY, which made it a two-writer line and changed the design);
~~**#39**~~ ✅ **BUILT s270** (one feed — the parameter was NAMED
`div_2d_plus_k1` all along; D_K1_SPECIAL_GAIN narrowed to §1250-only); **#40** LARGE
multi-session (AMT passive losses = an AMT shadow of Form 8582; ⚠ blocked by
`D_CFWD_001` and Ken's NOL ruling generalizes — the engine owns the number);
~~**#41**~~ ✅ **BUILT s270** (⚠ the components were importable all along —
the CONTRACT was undiscoverable; and the acceptance flushed out the SC
tax-table $50-vs-$100-row defect); **297 addendum to #19** — re-verify first, #19 was
gathered inside the deploy void. ~~**#34**~~ ✅ **BUILT s269** — a NEW-SURFACE
item (visible only because s264's readiness rules started running) that the
first triage pass MISSED because it sits before the s267 annex rather than
with 35-42. ⚠ Its classification as "small-to-medium build" was still an
over-estimate: verify-first found it was one allowlist. See the resume
section.

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
- **⚠⚠ s270 #41 MOVES EVERY SC RETURN with taxable income in [$7,000,
  $100,000)** — up to ±$3 of SC tax on next recompute, a CORRECTION toward
  the published SC1040TT (the $50-row assumption was wrong above $7,000).
- **s270 #39 MOVES a class only a new import can create**: a return with a
  nonzero K-1 `collectibles_28` gains Schedule D line 18 and, where the 28%
  rate group binds, more line-16 tax (a correction). Zero such returns exist in
  the shared DB today, so nothing already committed moves.
- **⚠⚠ s270 #36 MOVES A CLASS: every GA-home-address 1040 with no GA-tagged
  document and no GA-500 gains an auto-attached Georgia return on its next
  save or import.** For GA-resident retirees that is the missing state filing
  the item reported — a correction, not a regression. Known caveat recorded
  in code: a mailing address is not proof of residency (in-care-of, a
  nonresident at a GA relative's address) — those rare shapes simply don't
  use the attached GA-500.
- **✅ s270 #38 MOVES NOTHING — the opposite of s269.** `sch2_l14_source_amount` is
  NULL on every existing row, so compute does not touch Schedule 2 line 14 on
  any return that exists today, and `D_6252_009/010` are both silent without a
  recorded source. A return only changes once a preparer or an import records
  one. No tax-output movement, no new reds, no wave.
- **⚠⚠ s269 MOVES DIAGNOSTICS ON EVERY W-2G RETURN, and it will look like a
  regression.** `D_W2G_PAYER_ID` (error) fires on any W-2G row missing the
  payer name, a nine-digit EIN, or the full US payer address — which is
  essentially every already-committed W-2G return, because none of those
  fields was importable until this commit. **Those returns genuinely could
  not have been e-filed**; the rule reports a true state that was previously
  only visible as a single `D_EFILE_001` refusal at `in_review`. It clears by
  keying the payer block (every field is editable on the Slate misc-income
  screen) or re-importing with the now-accepted keys.
  **No tax-output movement** — no compute path changed. The other s269 change
  is message text: `_extract_w2gs`'s four-fact refusal became four
  field-specific ones (same refusal set, different wording), so anything
  matching those strings loosely should be re-read.
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
- ✅ **RESOLVED (s268) — the K1 `test_family_registration` red.** Five rules
  (`D_K1_UPE_PASSIVE`, `D_K1_UPE_SE`, `D_K1_7203_GENCHAR`,
  `D_K1_SPLIT_ARITH`, `D_K1_SPLIT_8582`) were in the REGISTRY but never in
  the test's `RULE_FNS` dispatch map, and `_FAMILY` derives from that map —
  so the trip-wire was short. ⚠ **It needs no DB and fails in 0.2s, which is
  exactly why it stayed red unnoticed**: nothing in a targeted run touched
  it. Map completed + severities recorded; 26 green.
  **BOTH s268-discovered pre-existing reds are now cleared** (this and the
  capital-loss line 16, which Ken ruled a stale test).
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

### ⛔ KEN — BATCH-296 #11: BOTH HALVES CONFLICT WITH AUTHORITY (s270, not built)
Two decisions, one item. The fixture batch stays uncommitted; full evidence
in the batch annex. **One decision each:**
1. **Should an 1120-S K-1 `se_retirement_amount` flow to Schedule 1 line 16?**
   The filed return took $5,031 there. **Pub 560: a shareholder-employee is
   NOT self-employed — the CORPORATION deducts the contribution on the
   1120-S** (the 1120-S K-1 has no box for it; the 1065's box 13 R is the
   partner path, already built). **Recommendation: NO feed; add a diagnostic**
   ("this amount deducts on the 1120-S, not here — confirm the filed line 16
   against the source"). Cost if built as asked: encoding a Pub 560 error on
   every S-corp owner packet.
2. **GA RIE: does materially-participating S-corp income stay EARNED
   (capped $5,000)?** The filed $35,000 exclusion is only reachable with it
   UNEARNED (5,000 + 30,050 → capped 35,000) — contrary to Ga. Comp. R.
   560-7-4-.02(4)(b)1's verbatim text, which s239 litigated and you ratified.
   Our engine gives $5,000 — the reg's answer. **Recommendation: keep the reg
   routing; treat the filed figure as source-side error.** If overruled, the
   change is one branch of `k1_ordinary_is_earned` + the s239 tests.

### ✅ KEN DECISIONS OUTSTANDING### ✅ KEN DECISIONS OUTSTANDING
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
