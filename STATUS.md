# TTS Tax App — STATUS (current state only)

*⭐ s303 (2026-08-27): **the estimated-payments staging warning catches up
to s302b** (`5d0c33c5` + `692d899d`, deploy `dep-da8377cs728c73cubirg`,
API-verified LIVE). A miss of my own from s302b: that deploy made 1040
line 26 follow the creditable dated rows and re-scoped the POST-COMMIT
diagnostic `D_2210_DATED` to match, but left its PRE-COMMIT twin
`backentry._warn_dated_vs_flat_payments` on the old contract — docstring
and all. It then fired on exactly the payload shape s302b made canonical
(dated rows, no flat buckets) and its text instructed the enterer to send
BOTH representations "in agreement", which is now the wrong payload. The
entry lane caught it on two packets that tie on every line, penalty
included. Staging now mirrors `d_2210_dated`'s predicate exactly: warn
only when creditable rows exist AND a nonzero legacy flat total DISAGREES.
Three shapes that warned incorrectly now stage silent — rows-only
(canonical), a NON-creditable `extension` row beside flat buckets (the old
guard gated on "any rows", not "any CREDITABLE rows"), and an EMPTY rows
array beside buckets (a real payload shape, previously untested). Stale
docstrings corrected (`FederalEstimatedPaymentSerializer`,
`line_status_1040.md` line 26). Gates: 5 new/flipped staging tests; 172
backentry staging+commit, 111 across 2210/estpay, 526 flow assertions.*

*⚠⚠ **s303's OWN LESSON, and it cost two wrong answers inside one hour:
I READ ONE TIMESTAMP AND GENERALISED FROM IT, TWICE.** Asked where the 49
redundant returns came from, I first asserted (unmeasured) that they were
the entry lane's and existed because the broken warning demanded them —
the entry lane refuted it for their queue. I then probed
`TaxReturn.created_at`, found all 49 seeded 2026-07-23 by `backfill-2025`,
and nearly sent a "chronologically impossible, the warning shipped 08-03"
refutation. **The SHELL's timestamp says nothing about when its PAYMENTS
were keyed.** The payment rows and taxpayer rows were both written
2026-08-01→08-25, so ~43 of the 49 were keyed while the broken warning was
live — the original story survives, but as an INFERENCE that should never
have been handed on as a fact. ⚠ Second trap in the same probe:
**`import_vendor` cannot answer "which lane keyed this"** — the column
landed in mig 0364 on 2026-08-26 (s298) and all 49 predate it, so blank is
an artifact of the migration date, not evidence. The states lane had
promoted my inference into S-18 as "the item"; corrected before Ken read
it, and S-18 now carries a three-tier MEASURED / INFERRED / NOT-ESTABLISHED
evidence ledger.*

*⭐ **The prod census that settled S-18** (read-only, all 51 returns
carrying §6654-creditable dated rows): **2** canonical (flat == 0), **49**
redundant-but-consistent (nonzero flat agreeing exactly), **0** stale
duplicates. So **the repaired warning fires on ZERO production returns** —
a false-positive removal plus a guard against a shape single-sided editing
can still create, explicitly NOT claimed as a live catch. The census also
withdrew the states lane's own recommendation: option (b) (staging refuses
the redundant payload) would have refused **49 of 51, the dominant shape**,
mid-queue on a live import surface. ⛔ **KEN — S-18** now recommends (d):
clean up the 49, leave the field alone. ⚠ Hard constraint found in the
probe: `prior_year_applied` appears as a dated kind on these returns, so
cleanup must be per-return and twin-checked — `py_overpayment_applied` may
be the only place a return states that figure. The one-week re-census is
the load-bearing follow-up: if the count holds at 49 after a week of
rows-only keying, the causal story is confirmed by observation.*

*⭐ **s303 depth probe — build-queue item ⑦ (general-category Form 1116) is
the LARGE unit, not a gate flip.** Prod cannot size it (5 Form1116 rows, 4
passive + 1 general — the packets that would populate it are the refused
ones), so the probe read the 635 Lacerte Inbox PDFs positionally: **254
carry a 1116 face; 94 passive, 18 general, and 10 carry BOTH passive AND
general on the same return** (8 general-only). Multi-category is real, so
the unit needs the OneToOne→FK model change (⚠ + all three registries and
`SINGLETON_SECTIONS`, the s256 lesson), a genuinely computed Part IV across
categories (lines 27/32 are hard-wired to the single passive result today),
`D_1116_003` narrowed to the still-unsupported categories, multi-face
render, multi-document MeF, and per-category §904(c) carryover. ⚠ **All
counts are LOWER BOUNDS — the mark detector found no category box on 152 of
the 254**, my heuristic's miss, not the corpus's. Excluded en route: the 4-
and 6-page multi-category packets are REGULAR + **AMT** copies of each
category — and the AMT copy is already refused by name (`D_6251_006`) with
`Taxpayer.amt_ftc` as the keyed escape hatch, so it is a correctly-labelled
declared limitation, not a new gap.*

*▶ NEXT: **item ⑦ is sized but NOT started — it is a multi-session unit and
Ken has not ordered it** ("sits next unless Ken reorders"). Recommend he
confirms before it begins, given the model change. Then extractor
follow-ons by residual: line-20 Sch 3 face class (5) > MFJ ownerless
int/div (4) > the 1099-R 5b-decomposition probe (1) > SchB payer-less
exception (1). Open BATCH-296 entry-lane items: the
`div_1099s.us_government_income` → GA S1-10 auto-derive design
(attribution-only today BY DESIGN; an auto-derive needs an off-switch
decision, s237 class) · the D_SCHD_006 QOF import surface.*

*⛔ **BLOCKED, AND IT IS KEN'S CALL — the entry lane is stopped.**
`import-lane.ps1` returns HTTP 403 (the saved session from 08-26 11:15
expired). The lane commits against **prep**, which holds the real filed
returns, and its script gates minting on Ken's explicit go for anything but
demo — so it correctly stopped and asked rather than re-authenticating.
Nothing lost: the 403 hit the staging POST, which writes nothing. Client
1855's payload is built and waiting. Client **1633 filed** (first packet on
that lane to clear cleanup entirely) before the block.*

*Peer state (s303): I hold the tree + test_postgres (states lane confirmed
it released both after its 254/0 RS run; entry lane blocked on auth, not on
the tree). Three annexes appended to BATCH-296 — the fix, the provenance
CORRECTION, and the item-⑦ sizing. Cross-lane triage this session: the
entry lane's **GA IND-CR 202 note is CONFIRMED, no defect** — `compute_ga500`
has carried `CC-FED × 0.50` since s243, cited to IT-511 Rev. 07/09/25 p.9
and the p.57 face (O.C.G.A. §48-7-29.10); their caution is the valuable
half and is filed, since on that packet the expense-base route also rounds
to 173 and the packet cannot discriminate the two rules by arithmetic. Their
**2210 line-8 method** (read the prior-year tax off line 8 rather than
asking) is filed; client 1518 stays held — it files no 2210, so the figure
genuinely is not in the packet.*

*⚠ PII housekeeping (states-lane flag, actioned): three code comments
carried a client surname — `backentry.py:688`, `test_backentry_commit.py`,
and the staging test class — all scrubbed to a neutral shape reference.
**Code comments are a PII carrier that travels into every quote of them.**
Forward-only; history is not rewritten on main.*

*⛔ KEN remaining: NEW — **S-18** (the 49 redundant flat-scalar returns;
recommendation (d) cleanup) · **the entry lane's auth re-mint** (blocking
that lane now) · carried: MeF schema trees on Render (D_EFILE_004 is the
correctly-labeled standing state until ruled) · S-15 + S-16 + S-17 (RS,
states lane) · the s298 truncated-name PREFIX-MATCH tier + organizer
enrichment · S-14 SEHI method (three facets) + s300b D_8962_NOCONVERGE
note · S-11/S-12 dependent-chain · the s295 int/div ruling (narrowed) ·
the 51-dependent DOB ask · the 1723 GA Eligible-Itemizer question ·
#21 (partial-allowance-against-PTP-income half stands) · #48 (RS 404),
#56, #63, #69, #10 tail. Carried: entity second-state-face transport (#3);
`OVERRIDE_HONORED_STATE_LINES`; 146-packet re-export; NC/CA/SC
linked-state reopens; #6 1065X/AAR; #68 optimizer; s274 PII narrowings;
RS 8990 re-authoring gate; 6765 Sec G; client-4545 D_8606_BASIS_ONLY;
1065 BATCH-004 #4; Analysis line-2 active/passive proxy; the unfloored
8960 line5 §1211(b) question; tier-3 PII scrub; the 47 RS-integrity-gate
sweep (12 FAIL, states S-10).*

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
