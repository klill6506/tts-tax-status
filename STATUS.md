# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s308 close, 2026-08-28

**State: idle and CLEAN. Head `504a484f` (s308), deploy
`dep-da91l1rl550s739kus9g` — verify live at next boot if this file still
says so.** Post-account-switch: the peer lanes are whatever is running
now; their last durable checkpoints are the entry lane's
`D:\tax-test-data\1040\Lacerte Inbox\LACERTE-RESUME-2026-08-28.md`
(**48 filed / 5 held** after s307's client-2234 GA LIC verification) and
the states lane's `D:\dev\delvio-states\STATUS.md` top block (staged set
21 = 8 ruled+built · 3 D-037-deferred · 10 UNRULED). ⚠ Any "Ken ruled X"
arriving by relay gets checked with him directly (standing; three 8/25–26
relays were wrong or retracted).

**✅ s308 SHIPPED (`504a484f`): the extractor Schedule C leg — sch_c is
GONE as a refusal class.** r22 = **22 emitted / 245 refused / 267
scanned** (was 21/246); zero drift on the 21 prior payloads; the one new
emit **TIES 14/14 lines to the dollar in a rolled-back probe** — the SE
chain end-to-end (Sch C net → SE tax → ½-SE deduction → AGI → GA RIE
earned income, §6654 penalty at delta 0). Packet identities live in
`PipelineOut/r22`, never here. Three gates in one unit:
- **`sch_c.py`** parses both Schedule C faces positionally (geometry from
  the nine solo packets' 16 raw dumps + a 126-page corpus calibration:
  70/70 TaxWise p1 pages parse, zero identity failures; the single p2
  error is a Lacerte-printed stray that already refuses upstream).
  Right-edge value recognition from the start (the s307 lesson — line
  13's caption prints "179"). Refusals by name: statutory box, 32b,
  passive/unreadable line G, "Other" method/inventory, line-34 Yes,
  4/27b with no page 2, line 30 with no sqft, Part V note rows.
  `depreciation_filed` carries a printed line 13. Form 8829 pages now
  BLOCK by name.
- **`sch2.py`** — the Schedule 2 other-taxes decomposition: face line 23
  neither blanket-refuses nor blanket-allows; components engine-derive
  (4 SE / 11 8959 / 12 8960) or refuse by name; line 21 must equal the
  printed component sum. ⭐ Proven live on its first corpus pass: it
  exposed $678 of Form 5329 tax on a packet whose 5329 page TaxWise
  never printed — the s297 omission class, caught by the sum identity.
- **f8995 Part I rows** parse (name/TIN/qbi + wrapped-name continuation)
  when a Schedule C source exists; the emitter matches rows one-for-one
  by TIN and count and checks Σrows = Σ(line 31) − Sch 1 line 15.
  ⭐ Also proven live: one packet's 8995 carries a second business whose
  Schedule C page was never printed — three independent gates refuse it.
- Plus: Schedule 1 line 3 decomposes against Σ(31); line 15 allowed only
  with an SE source; **the SE wage-base guard** (the W-2 detail report
  does NOT print box 3, so Schedule SE line 8a is underivable — refuse
  when wages + 0.9235×net cross $176,100; marginal cases fall to the
  dry-run tie).
- 39 new tests (identity injections, checkbox variants, the
  caption-numeral and name-initial-in-gutter regressions, the Schedule 2
  header-"02"/"$150,000"/"965" traps); 161 extractor tests green.

**▶ NEXT unblocked build work — extractor, by the r22 ranking (all upper
bounds until a class opens):** student_loan_educator_wks **7** >
f5695 = f8889 = asset_detail = f8880 = est_payments_wks **5** each >
other_income_wks 3 > f8962 3. Notes: the est_payments_wks class should
emit dated `federal_estimated_payments` rows (s302b note, still
standing); the exempt-interest decomposition (3 named packets, s307)
remains the cheapest targeted leg. Follow-on noted, not built: face
line 25c still blanket-refuses 8959 withholding even though it is
engine-derivable from extracted W-2 rows once an 8959 page is present —
worth folding into whichever leg next touches a 25c packet. Then: item
⑦'s **multi-category Form 1116** half (multi-session, model change —
**wants Ken's go before it starts**).

**⛔ WAITING ON KEN — unchanged from s307 (he said he'd come back to
each):**
1. **Georgia SALT add-back** (*"let me think on it"*) — ⚠ the
   booklet-vs-Lacerte proration CONFLICT must reach him before the field
   is designed (states lane `R-GA500-DED` = IT-511 literal proration →
   0 on all 9 resident packets; Lacerte does `max(0, 5d − cap)`;
   nonresident path different again — BATCH-296 tail has the full spec
   input).
2. **Per-property nonpassive lever** (*"I need to look this up at the
   office"*) — the lane's §1.469-2T(f)(3) hypothesis does not separate
   the parcels; do not build to it.
3. **The 3 firm-EIN W-2 rows** — wrong data on real returns; his call.
4. **Client 1071** — a 2210 line 8 fitting two histories, both tying.
5. **Client 1141** — dependent DOB exists nowhere; REVIEW_QUEUE carries
   the do-not-build recommendation (measured cost 1,700).
6. **One Inbox packet carries "no need to fil" typed into the TaxWise
   name field** (s307; identity in `PipelineOut/r21` refusals) — genuine
   do-not-file?
7. The states lane's **10 unruled** staged items + S-18/S-19 + the
   REVIEW_QUEUE pair (D_GA500_009 error→warning; the MFS
   living-arrangement field pair).

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS (WHEN BUILDS RUN)
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax` (srv-d6geloa4d50c73el2trg); demo = `tts-tax-demo`. ⚠ **A push
is not a deploy — CHECK THE DEPLOY STATUS after pushing** (API key in
`D:\dev\Passwords & Secrets\render-api-key.txt`). **Standing push
authorization (Ken 2026-08-23): push at own judgment; verify every deploy;
hold only for a named reason.** ⚠⚠ **ORDERING (s279/s282): push → deploy
LIVE → seed → verify — and the deploy ITSELF seeds (`build.sh seed_all`
auto-discovers `seed_*` at BUILD time).**
- **s308 deploy: `504a484f` → `dep-da91l1rl550s739kus9g`** — pushed
  2026-08-28 23:32Z, was `build_in_progress` at session close (verify at
  next boot). Extractor scripts + tests only — no server runtime change,
  no migration, no seeder, no schema regeneration.
- s307 deploy `78d9fedb` API-confirmed LIVE 2026-08-28 (the f8995 leg —
  also scripts-only).
- s306t deploy `440aac92` API-confirmed LIVE (deploy-skew severity split
  + GA LIC gate + LIC-CHILD input + three message fixes).
- s306 deploy — API-CONFIRMED LIVE 2026-08-27: `64c4dc15` →
  `dep-da8ap4ia6suc73bdidtg` (finished 21:35Z). It
  carries all three s306 pushes: `1ae86753` (the D_8889_EXCESS widening + the
  4547 casing normalisation + the schema descriptions — its own deploy
  `dep-da8aocpt0dsc73bvgfu0` BUILT and went update_in_progress before being
  superseded, which is the normal deactivation), then `e1754760` and `64c4dc15`
  (markdown only). **No migration, no seeder, no schema-vocabulary change** — the
  published back-entry schema WAS regenerated, but only field `description`s
  moved, so no allowlist or enum shifted. ⚠ The diagnostic rule's name/description
  reach prod because `seed_builtin_rules` is `update_or_create` and the deploy
  seeds at BUILD time; severity is unchanged (`warning`), so no rule row changes
  behaviour beyond the widened condition.
- s303 deploys, BOTH API-confirmed LIVE 2026-08-27: `54c187ba` (head of
  `5d0c33c5` + the scratch-file removal) → `dep-da8377cs728c73cubirg` =
  the staging-predicate fix itself; then `0d7673c9` (carrying `692d899d`,
  the empty-array test, + the session-close docs) →
  `dep-da83q215efls7394k1vg`, **the currently live deploy**. No migration,
  no seeder, no schema regeneration in either (the change is a staging
  predicate + docstrings; no vocabulary or allowlist moved).
- s302 deploys: `9999f2c6` → `dep-da7q5rek1f9s73chfqe0` (⑥, mig 0367 +
  D_K1_PTP_469G seed) — **API-confirmed LIVE**; `a9f97025` →
  `dep-da7qalvlk1mc738cgd60` (s302b line 26, no migration) —
  **API-confirmed LIVE**; `f882e494` → `dep-da7qdacs728c73cml8i0` (s302c
  D_EFILE_004 seed, no migration) — **API-confirmed LIVE**; `ffedba4d` →
  `dep-da7qp749v7es73bl9psg` (s302d SE line 8a, no migration) —
  **API-confirmed LIVE**. All four verified 2026-08-26.
- s301 deploys: `d6ab0e42` → `dep-da7ovk6417fc738ffrq0` · `03c96795` →
  superseded by `2a2bb807` — LIVE (s301b e8960 surface confirmed by the
  entry lane's committed 1723).
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (s302 regenerated; current as of `9999f2c6` — s302b/c changed no schema.)

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. s302 coordinated at boot;
three annexes appended to BATCH-296 (every load-bearing exchange annexed).
Peers stage; Ken decides. ⚠ A peer's factual claim can be RETRACTED
minutes later (the s302 "second fixture" correction) — build only against
what a batch file or your own probe shows.

## ⚠ Known red / rotted — THE ONE LIST (post-s302)
- ~~12 of 47 RS `check_*_integrity.py` gates FAIL~~ — **THE 12 WAS WRONG AND
  S-10 IS CLOSED** (states lane, 2026-08-27, correcting their own s302
  number). Their triage: **3 were never failing** (two pass every check then
  die printing a non-ASCII success line to a cp1252 console; one exits
  `ImproperlyConfigured` importing a loader without configuring Django) and
  **2 were their own defects from hours earlier**. True pre-existing count
  **7**, of which **10 of 12 are resolved and 10 gates are now pinned in
  `tests/test_integrity_gates.py`**; RS suite **254 passed / 0 failed**.
  ⚠⚠ **The sweep that produced the 12 read EXIT CODES without reading
  OUTPUT** — the same one-signal-generalised error class as s303's own
  timestamp misreads. ⭐ Their lesson: **a red gate does not merely fail to
  catch new defects, it CAMOUFLAGES them** — two fresh defects hid inside
  pre-existing redness, in gates nothing runs. ⚠ S-10a survives separately:
  `R-1040X-SUPERSED` has no authority link, and "superseding" appears ZERO
  times in the 2025 i1040x (they pulled and committed it as evidence), which
  also casts doubt on the sibling rule's existing citation. S-10c
  (`D_8582_PTP`'s unwritten recompute) also stands.
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof. (s302: the two s258
  fails appeared again in the sweep and PASSED in isolation — unchanged.)
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ~~the GA700 answer-key coverage test~~ — REPAIRED s302 (was red since
  s274; greped only FORMULAS_GA700, blind to the pull-map writes).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s302).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): five phantom failures. New files/markdown/scratchpad only.
- 🌐 ⚠ **The teardown warning "database test_postgres is being accessed by
  other users / 1 other session" is NOT lock contention on this setup** —
  it is YOUR OWN pooled connection: Supavisor holds it open after pytest
  finishes, so Django's teardown finds it still attached. Verified against
  `pg_stat_activity` (states lane, 2026-08-28: one idle connection, started
  inside the run window, idled as the run ended). Recurs on every run; zero
  effect on results. Do not go hunting for a peer who isn't there.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c`; a script run by absolute path needs
  `sys.path.insert(0, server)` (s298). ⚠ `python -m taxwise1040`
  does NOT resolve from `server\` — run the package's `__main__.py` BY
  PATH (s297).
- 🔧 ⚠ **`BaseModelSerializer` partial updates save ONLY the patched
  columns** (s298) — a serializer-side derived write must be injected into
  `validated_data`, never `setattr` on the instance (it silently no-ops).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ (s302 re-confirmation: `-replace` +
  `Set-Content` mangled an em-dash annex — python io did it clean.)
  ⚠ Embedded double-quote in a here-string arg to a NATIVE exe SPLITS the
  argument. ⚠ `$1` in an unquoted PS arg EXPANDS to empty. ⚠ PS
  `Sort-Object -Unique` on a one-element pipeline UNWRAPS to scalar (s296).
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash. ⚠ A
  COMMITTED return refuses plain stage+dryrun with HTTP 409 (s298 —
  verification route = the in-place rolled-back recompute, s289 pattern;
  s302 ran the 1792 probe this way clean, incl. the commit reconciliation).
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ **`str(Decimal)` preserves the STORED SCALE** — a diagnostic detail
  written as `str(amount)` reads back `"429.00"`, not `"429"`. Compare
  `Decimal(details[k])`, never the raw string (s306; two test failures).
- 🌐 ⚠ **A BACKGROUNDED pytest piped through `Select-Object` buffers ALL
  output until exit** — no interim progress at all, so a long sweep looks
  identical to a hung one. (The standing rule already bans that pipe; this is
  the detached-run reason.) Check liveness with the process's CPU time.
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); synthetic `insert_text` PDFs may
  LOSE leading spaces (s290) — the extractor parses POSITIONALLY everywhere.
  ⚠ **The AcroForm FILLER flattens ALL widgets out of its output** (s296).
  ⚠ **TaxWise RE-TYPESETS whole schedules** (s297): pin from packets only.
  ⚠ s301: a fixture that encodes the WINDOW's geometry instead of the
  CORPUS's keeps a blind spot green.
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section. Local templates beat both.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292–s302 ran probes this way). ⚠ Scripts touching
  client-named returns live in SCRATCHPAD or tax-test-data, never the repo
  (PII). ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295/6/7/s301).
- 🔧 ⭐ **DRY-RUN THE CORRECTION PASS AND READ EVERY ROW before --commit**
  (s298). ⭐ s302 corollary: **CENSUS THE PROD BLAST RADIUS before changing
  a computed line's source** (51 returns read, zero moved — the do-no-harm
  half; the failing case rides the tests).
- 🌐 ⭐⭐ **THE THIRD QUESTION (s302, states lane): "does anything ACT on
  it?"** — after "does it exist?" and "does it run?". A green detector
  nobody reads is functionally no detector, and it is HARDER to spot
  because it announces itself as a pass (D_2210_DATED fired correctly for
  weeks while line 26 dropped the money it was flagging). ⚠ s302d: the
  same question applies to a TEST — mine ran, was green, and measured
  nothing.
- 🔧 ⚠⚠ **A FIXTURE THAT KEYS A COMPUTED COLUMN MEASURES NOTHING** (s302d):
  `ScheduleC.net_profit` is written by compute from gross receipts, so a
  test that keys it gets line 31 = 0 — and every rate/ratio assertion then
  passes as `0 == 0`. Key the INPUT (`gross_receipts`), and assert the
  intermediate is nonzero BEFORE asserting anything about a rate applied
  to it.

## 🔎 Carried for triage — NOT claims
- (s303) **The simplified home-office 300 sq ft cap is applied PER SCHEDULE C,
  but Rev. Proc. 2013-13 §4.08(6) caps ONE TAXPAYER'S AGGREGATE across all
  qualified business uses of the SAME home** ("A taxpayer who has more than
  one qualified business use of the same home for a taxable year is limited
  to a maximum of 300 square feet ... must allocate the square footage among
  the qualified business uses"). `compute_schedule_c` caps `biz.home_office_
  sqft` per business, and BOTH diagnostics (D_SC_004 / D_SC_005) iterate per
  business — so one proprietor with two home-based Schedule Cs could claim
  600 sq ft, overstating by up to $1,500. **Prod census: ZERO returns hit it
  today** (17 simplified home offices, no proprietor with two). Latent,
  overstating, unfiled work. ⚠ The sibling rule is the OPPOSITE and the
  engine is RIGHT about it: §4.08(5) lets spouses sharing a home EACH use up
  to 300 sq ft of *different portions* — so the entry lane's 300+250=550
  packet is correct as filed, and a naive per-return aggregate check would
  wrongly flag it. Group by `proprietor`, never by return.
  ⚠⚠ **THE THRESHOLD IS AGGREGATE > 300, NOT "TWO ROWS" — do NOT ship an
  unconditional refusal.** I proposed refusing on "same proprietor, two
  simplified rows" (reasoning from §4.08(7), *"may use the safe harbor method
  for only one home for that taxable year"*). **The entry lane refuted it from
  the FIRST SENTENCE of §4.08(6), which I had already printed and read past:**
  *"A taxpayer who has more than one qualified business use of the same home
  for a taxable year and who elects the safe harbor method **must use the safe
  harbor method for each qualified business use of the home**."* Two elected
  rows in the same home are **MANDATORY**, not a defect — a refusal there
  would push the enterer to un-elect one, which is exactly what (6) forbids.
  The three cases for one proprietor with two simplified offices:
  **same home + aggregate ≤ 300 = LEGITIMATE AND COMPELLED** · same home +
  aggregate > 300 = defect under (6) · different homes = defect under (7).
  So: **aggregate > 300 per proprietor → refusable** (the missing home id then
  changes only which clause the message cites); **aggregate ≤ 300 → WARNING
  ONLY**, because compelled-same-home is indistinguishable from a (7) breach
  without a home identifier. `proprietor` + aggregate sqft is the key;
  `proprietor` alone is not.
- (s303) **§4.08(4) monthly averaging is unrepresentable in our data** — a
  business starting, ending or resizing mid-year must use *"the average of the
  monthly allowable square footage"*, no more than 300 in any one month, and a
  month counts only with *"15 or more days of a qualified business use"* (the
  Rev. Proc.'s own example: 400 sq ft from July 20 → average 125, not 300).
  `schedule_cs` keys ONE `home_office_sqft` with no in-service date, so a
  mid-year start silently claims the full-year amount. Same gap as the missing
  home identifier — size them together.
- (s302d, entry lane) **D_EFILE_001 cannot distinguish "EIN not keyed"
  from "EIN not obtainable."** Most packets satisfy it from the GA-500
  income-statement grid, but a payer that withheld no Georgia tax never
  appears there — a third-party sick-pay payer on one packet has no EIN
  anywhere in the source. That packet is held on a gap the preparer
  cannot close from the documents. If the shape recurs, the refusal
  should split (and the s298 21-blank-row class is the same question
  from the other side, still on Ken's prefix-tier call).
- (s302, states lane S-10c) **`D_8582_PTP` remains unverified** — one of
  seven keys `check_schedule_e_8582_integrity` reports as "no independent
  recompute mapped". s302's §469(g) build is validated only by its own
  i8582 PDF reading, which is why that reading was done. ⚠⚠ Their wider
  finding: `check_topic8`-style gates compare a rule's DECLARED inputs
  against the facts and never compare the FORMULA against the
  declaration — so `inputs: []` is vacuously valid and **the emptier a
  rule's declaration, the safer it looks** (exactly how R-SE-L8D-L9's
  missing L8a input survived; found by a $5,717 error, not by a gate).
- (s302) `div_1099s.us_government_income` is attribution-only BY DESIGN
  (splits a keyed ga500_fields S1-10 by owner; derives nothing) — the
  entry-lane note asks whether it should auto-derive S1-10; needs an
  off-switch decision (s237 class). · D_SCHD_006 QOF answer has no import
  surface (warning-level, filed).
- (s301) One packet's 5b decomposition: extracted pension rows carry
  115,150 vs face 15,150 — probe the 1099-R parse before assuming a
  rollover/exclusion. · One packet's Sch D carries 1b grid totals with NO
  f8949 pages + its own h-identity break — suspected misparse. · A blank
  printed 8949 page (synthetic packet) refuses "no transaction rows" —
  over-refusal by design.
- (s298) 21 named-but-blank W-2/1099-R rows have no unambiguous EIN match
  — held on D_EFILE_001; movement waits on Ken's prefix-tier call.
- (s297) The X mark at (474.7, y≈389) in the 1040 p2 EIC row region —
  unidentified, parser-ignored. · `est_payments_wks` (27) and `f8863` (6)
  gate at the face. ⚠ s302b note: when the est_payments_wks face class
  opens, the extractor should emit dated `federal_estimated_payments`
  rows — they now drive line 26 directly.
- (s296) The 22 sch_d GEOMETRY-error packets refuse loudly by design.
- (s295) 7 auxiliary Inbox PDFs refuse as non-packets — correct. ·
  `_summary_lines` GA500_SUMMARY_LINES lacks S1-6.
- (s290) GA RIE interest row excludes K-1 16A tax-exempt interest —
  stated boundary. · The rendered 8995 TIN prints unformatted — cosmetic.
- (s289) `IndividualForm7203`: no §179/charitable carryover keying;
  the §179 cap doesn't extend to 1065. · K-1 capital gains reach Sch D
  but not the L9 gain/loss WEIGHTS (⚠ s298: also the #78 aggregate
  convention under BOTH vendors).
- (s288) `IndividualForm7203` no home for box 16 code E; 1065 box 18
  a/b/c none recipient-side. · `ctc_override`/`odc_override` +
  `Dependent.compute_qualifies_*` are DEAD — removal candidate (Ken).
- (s287) 8825 line-1 repaint covers the LINE-1 table only. · Suggested-
  field convention covers W-2 3/5 + 1099-R box 16 (CLAUDE.md note stale).
- (s285) Sch 4 nonresident arm still apportions the whole widened base.
- (s283) The stamp excludes 1040 packets (Ken). · (s282)
  `OVERRIDE_HONORED_STATE_LINES` hand-synced. · (s281) OOS-state line-18
  prompt diagnostic specified, not built; stage allowlists `schd_fields`
  keys, `ga500_fields` not at all. · (s268) 1,604 queries/run. ·
  (s241/s281) `Form8606` unique-constraint candidate · 🔴 `HSAAccount`
  half CLOSED. · (s275/s281) `.first()`-on-per-form-rules remainder. ·
  (s294) a state face left by an omitting correction batch is not
  recomputed against that batch's federal changes.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried:
Everything from s277–s301 stands (see STATUS_ARCHIVE s300/s301 entries).
NEW (s302): **S-15** (SCHEDULE_K1 §469(g) disposition trio + the
R-8582-PTP related-party CORRECTION — the spec as written frees losses on
a related-party sale, the exact case §469(g)(1)(B) bars; + rule-2/rule-4
branch naming + the fully-taxable definition) and **S-16** (R-PAY-04
dated-rows source, matching R-2210-REG's precedence) — both staged by the
states lane, both Ken's. Recorded lane-side, not staged: the RS 8990 spec
models NO Schedule A surface (the seam if it is ever specified is
`partner_excess_bie`).
NEW (s306): **S-19** — `R-8889-EXCEPTIONS` states the same narrow
`line 2 > line 13` condition the code carried (blind to an employer-only
HSA excess, which i8889 measures against line 8) and its diagnostic message
still says the excise is "not computed here", which is stale since Form 5329
Part VII computes it from line 47. Staged by the states lane, who pulled both
instruction PDFs and confirmed the quotes verbatim, and who DISSOLVED a third
suspected defect in the same rule (line 12 vs line 13 are arithmetically
equivalent given `line 13 = min(line 2, line 12)`) — recorded because a false
finding attached to a true one is how a good item gets argued down.
⚠ Also noted by them, no action from this lane: `R-8889-EXCEPTIONS` declares
`inputs: []` while its formula reads lines 2 and 13 — the THIRD confirmed
instance of the S-17 under-declaration class, after `R-SE-L8D-L9` and
`R-1116-SUMMARY`.
