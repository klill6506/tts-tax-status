# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s312f close (one overnight session s311→s312f, Ken asleep — autonomous continuation), 2026-08-29

**State: idle and CLEAN. SIX units shipped tonight after s311: ① the
8962 family-size claimed-only fix (`93428e59`, LIVE), ② the extractor
f5695 leg (`2990fbe2`), ③ the s312c triage pass — two extractor
defects fixed, all three TRIAGE holds resolved (`9a84de63`), ④ the
morning-list digest (s312d), ⑤ the Schedule 2 page-1-alone relaxation
(`1f298125`), ⑥ the extractor f8962 leg (SHA in the close-out line).
Corpus now r28 = 31 / 236 / 267 — FIVE new tie-verified emits tonight
join client 3218: SIX commits wait on Ken's token.** Peer lanes: both
restarted overnight and then ended; BATCH-296's annexes are the
durable record (five appended tonight, incl. **KEN'S MORNING LIST**
at the tail). ⚠ Any "Ken ruled X" arriving by relay gets checked with
him directly (standing).

**✅ s312 SHIPPED:**
- **`compute_8962.family_size` counts CLAIMED dependents only** —
  the RS FORM_8962 spec's R-8962-FAMILY-SIZE is verbatim "taxpayer +
  spouse (if not MFS) + dependents claimed" (§36B(d)(1)); an EIC-only
  row inflated the tax family → lower FPL% → larger PTC. The s311
  GA-7a class, found by the same sweep. Census: the one false-row
  return has ZERO 8962 engagement — nothing committed moves.
- **The extractor Form 5695 (Residential Energy Credits) leg — f5695
  is GONE as a refusal class.** All four faces parse into the s212
  `e5695_*` vocabulary (engine recomputes both parts); §25C identity
  gates re-run every printed cap; §25D refuses by name (no corpus
  witness); the 1040 line-20 gate now COMPOSES (face 20 == 8880 l12 +
  5695 l15 + l32) and the 5695 line-31 CLW gates against face 18 less
  the 8880 credit — which caught a $56 unextracted Schedule 3 credit
  by name on its first pass. r26 = **30 emitted / 237 refused / 267
  scanned** (was 28/239); zero drift on the 28 prior payloads; BOTH
  new emits tie-probed rolled back (32 tie lines total; one carries a
  $553 GA balance due, vendor-matched). 16 new tests; 237 extractor
  tests + the 25-test 8962 suite green. Packet identities in
  `PipelineOut/r26`, never here.
- Two calibration catches before anything landed: a p1 caption's bare
  "1" inside a too-wide marker band, and a template caption leaking
  into QM PIN fields (both fixed + regression-pinned; the annex
  carries the detail).
- **The overnight triage pass (s312c) resolved all three carried
  TRIAGE holds** and r27 = 30/237/267 with ZERO drift:
  1. The "8995 line 9 = 0 vs line 10 = 9" oddity was OUR parser's
     defect, not the packet's — the right-gutter repeat was dropped by
     TEXT alone, so a value equal to its own line number ($9 on
     line 9) was structurally invisible (the third
     marker-inside-the-value-window instance). Fixed positionally
     (x0 485-510) + regression; **that packet's do-not-hand-enter
     hold LIFTS** (it still refuses extraction on real classes: DOB,
     Sch D line 19, 25c, Sch 2-13 box 12).
  2. The Schedule D identity break (delta 2,039) is a GENUINE vendor
     self-inconsistency, decomposed to one line: every component ties
     its printed 8949 exactly, TaxWise used −19,103 downstream
     (line 16 AND its own carryover worksheet, LT figure 19,103), but
     printed line 14 = −32,002 where its arithmetic used −29,963.
     **Hold stays; the question for Ken is one number: the 2024 LT
     carryover into 2025 — 32,002 or 29,963?** (2,039 of 2026
     carryover rides on it; current-year tax identical either way.)
  3. The 5b decomposition (115,150 vs 15,150) was OUR emit check
     ignoring a $100,000 R-marker rollover the parser had already
     captured — the check now mirrors the engine's doc_taxable
     (max(0, taxable − rollover)). That packet's remaining wall is a
     $21 Sch 2-13 box-12 item (the standing box-12 class).

**▶ NEXT unblocked build work — s312f added the f8962 leg and the
remaining classes are all priced (see the s312c tracker block):
est_payments_wks 7 (ZERO immediate yield — infrastructure), f2441 5
(structurally DOB-data-blocked), asset_detail (multi-session, gated
behind a sch_e leg — 130 landscape pages, 45+ free-text Form headers,
link-key design in Ken's depreciation domain), f8863 3 /
other_income_wks 3 (unprobed).** The corpus's immediate-yield tail is
EXHAUSTED — every remaining refusal is data-blocked (DOB, tokens),
multi-session, or a named single wall. **s312g probed the last two
unprobed classes: f8863 and other_income_wks BOTH zero immediate
yield** (f8863's near-surface packets all carry the DOB class — more
weight on the birth-year ask; other_income_wks's four carriers all
have other walls). Cheapest next legs when building resumes: the
25c/8959 fold-in (two named witnesses, needs its own probe — box 5
is not printed; s310); the exempt-interest decomposition (3 named
packets, s307). Then item ⑦'s multi-category Form 1116 half
(multi-session, model change — **wants Ken's go before it starts**)
and the asset_detail register (after sch_e; a daytime design).

**⛔ WAITING ON KEN — s312d compiled the MORNING LIST (the BATCH-296
tail annex): every data ask in one sitting — the token mint (5 queued
commits), 19 dependent birth years across 14 packets (two packets
emit the same day the years arrive), the one-number Schedule D
carryover question, and the reprint asks. The standing decisions
below are unchanged:**
1. **Georgia SALT add-back** (*"let me think on it"*) — ⚠ the
   booklet-vs-Lacerte proration CONFLICT must reach him first; BATCH-296
   tail has the full spec input incl. the nonresident path.
2. **Per-property nonpassive lever** (*"I need to look this up at the
   office"*).
3. **The 3 firm-EIN W-2 rows** — wrong data on real returns.
4. **Client 1071** — a 2210 line 8 fitting two histories, both tying.
5. **Client 1141** — dependent DOB exists nowhere (REVIEW_QUEUE has the
   do-not-build recommendation; measured cost 1,700).
6. **The "no need to fil" name-field packet** (s307; identity in
   `PipelineOut/r21` refusals) — genuine do-not-file?
7. The two s309 GA RIE items (REVIEW_QUEUE top two) — **now carrying
   the states lane's IT-511 research (s311): the booklet is SILENT on
   RIE line 10, and its one "similar income" phrase is an EXCLUSION —
   quoting it in support of the reg's category would be backwards.**
8. The states lane's **10 unruled** staged items + S-18/S-19 + the
   REVIEW_QUEUE pair (D_GA500_009 error→warning; the MFS
   living-arrangement field pair).
9. **Prod token for the entry lane** — SIX tie-verified commits
   queue on it: clients 3218 (twr24), 3427/4666 (twr25), 4572/4609
   (twr26) and the s312f PTC emit (twr28 — identity in the annex).
   Every deploy any of them needs is LIVE; the lane reads an
   exactly-622 GA no_tie as "wrong deploy", by design.

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
- **s312 deploys, all API-confirmed LIVE 2026-08-29:** `93428e59`
  (8962 family-size, the ONE runtime change) → `dep-da952k67bikc73amret0`;
  `2990fbe2` (f5695) → `dep-da95h2u7bikc73an46p0`; `9a84de63` (s312c
  triage) → `dep-da95q43ncjis7392cntg`; `1f298125` (sch2 relaxation) →
  see close-out; the s312f f8962 leg's status in the close-out line.
  No migration, no seeder, no schema regeneration (all vocabulary is
  s203/s204/s212, already published).
- s311 deploys `67debb5f` → `dep-da94p1f10e5c73aqk93g` (the 7a fix) +
  two docs-only follow-ups, all API-confirmed LIVE 2026-08-29. · s310
  `972cf50e` LIVE. · s309 `11415881` LIVE. · s306t `440aac92` LIVE.
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (Current as of `9999f2c6`; s311 changed no schema.)

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. Peers stage; Ken decides.
⚠ A peer's factual claim can be RETRACTED minutes later — build only
against what a batch file or your own probe shows.

## ⚠ Known red / rotted — THE ONE LIST (post-s302)
- ~~12 of 47 RS `check_*_integrity.py` gates FAIL~~ — **THE 12 WAS WRONG AND
  S-10 IS CLOSED** (states lane, 2026-08-27). True pre-existing count **7**,
  10 of 12 resolved, 10 gates pinned in `tests/test_integrity_gates.py`;
  RS suite **254 passed / 0 failed**. ⚠ S-10a survives (`R-1040X-SUPERSED`
  has no authority link). S-10c (`D_8582_PTP`'s unwritten recompute) also
  stands. **S-10b diagnosed (states lane, 2026-08-28, `cdd524f`): all 15
  schedule_a gate failures are GATE defects, none spec — the gate reads
  three contribution buckets where the spec models seven §170(b)(1)
  classes + vintages, and one scenario DESTROYS a carryover. Never read
  that gate as authority for engine work.**
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s302; s311 touched no client code).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): five phantom failures. New files/markdown/scratchpad only.
- 🌐 ⚠ **The teardown warning "database test_postgres is being accessed by
  other users / 1 other session" is NOT lock contention on this setup** —
  it is YOUR OWN pooled connection (Supavisor holds it open; verified
  against `pg_stat_activity`, states lane 2026-08-28). Zero effect on
  results.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c` (re-confirmed s309); a script run by
  absolute path needs `sys.path.insert(0, server)` (s298). ⚠ `python -m
  taxwise1040` does NOT resolve from `server\` — run the package's
  `__main__.py` BY PATH (s297).
- 🔧 ⚠ **`BaseModelSerializer` partial updates save ONLY the patched
  columns** (s298) — a serializer-side derived write must be injected into
  `validated_data`, never `setattr` on the instance.
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ Embedded double-quote in a here-string
  arg to a NATIVE exe SPLITS the argument. ⚠ `$1` in an unquoted PS arg
  EXPANDS to empty. ⚠ PS `Sort-Object -Unique` on a one-element pipeline
  UNWRAPS to scalar (s296). ⚠ `[IO.File]` calls resolve against the
  PROCESS cwd, not PowerShell's `cd` (re-hit s310) — absolute paths.
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash. ⚠ A
  COMMITTED return refuses plain stage+dryrun with HTTP 409 (s298 —
  verification route = the in-place rolled-back recompute, s289 pattern;
  s311 probed both new r25 payloads this way — batch+staged rows created
  INSIDE the rolled-back transaction, `commit_staged_return`
  self-validates, nothing landed).
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ **`str(Decimal)` preserves the STORED SCALE** — compare
  `Decimal(details[k])`, never the raw string (s306).
- 🌐 ⚠ **A BACKGROUNDED pytest piped through `Select-Object` buffers ALL
  output until exit** — check liveness with the process's CPU time.
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); synthetic `insert_text` PDFs may
  LOSE leading spaces (s290) — the extractor parses POSITIONALLY everywhere.
  ⚠ **The AcroForm FILLER flattens ALL widgets out of its output** (s296).
  ⚠ **TaxWise RE-TYPESETS whole schedules** (s297): pin from packets only.
  ⚠ s301: a fixture that encodes the WINDOW's geometry instead of the
  CORPUS's keeps a blind spot green. ⚠ s307–s311: values are recognised
  by RIGHT edge — caption numerals live at the left of value regions,
  a caption FRAGMENT can END inside a value window (trailing punctuation
  excludes it), and **a MARKER token can END inside a value window too**
  (f8880: "10"-"12" reach x1≈492 into the You column — the marker/value
  split is x1 ≤ 493 vs ≥ 494, measured, s311).
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292–s311 ran probes this way). ⚠ Scripts touching
  client-named returns live in SCRATCHPAD or tax-test-data, never the repo
  (PII). ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295–s311:
  nine confirmations; s311's 8 solos yielded 2 emits + deeper).
- 🔧 ⭐ **DRY-RUN THE CORRECTION PASS AND READ EVERY ROW before --commit**
  (s298). ⭐ **CENSUS THE PROD BLAST RADIUS before changing a computed
  line's source** (s302; s311: the 7a fix censused to ONE unaffected
  return before landing).
- 🌐 ⭐⭐ **THE THIRD QUESTION (s302): "does anything ACT on it?"** — after
  "does it exist?" and "does it run?". s310: grep the enclosing scope
  before defining a nested helper (`az` shadowing).
- 🔧 ⚠⚠ **A FIXTURE THAT KEYS A COMPUTED COLUMN MEASURES NOTHING** (s302d):
  key the INPUT, and assert the intermediate is nonzero first.
- 🔧 ⚠⚠ **A NEW BOOLEAN FIELD RE-SCOPES EVERY COUNT THAT PREDATES IT**
  (s311, the 7a lesson): `claimed_as_dependent` (s281) promised exclusion
  from "every dependency-derived count", but the s235 GA 7a derive and
  compute_8962's family_size were never re-swept. When a field's contract
  says "every X", grep for every X.

## 🔎 Carried for triage — NOT claims
- ~~(s311) `compute_8962.family_size`~~ — **FIXED s312** (`93428e59`,
  deploy LIVE; the spec's own formula settled it).
- (s312) **A $56 unextracted Schedule 3 line-1-3/6d/6l credit on one
  f5695 packet** (probably foreign tax; identity in `PipelineOut/r26`
  refusals) — the 5695 line-31 CLW names it; the packet also carries
  ownerless-document refusals.
- (s311) **The more-than-four-dependents box** (1 packet): the marked box
  means dependents beyond the grid exist only outside the packet —
  refuses by name; no route until a source exists.
- (s310→s312c) **The Schedule D identity break is DECOMPOSED** (see the
  s312c triage block above): vendor self-inconsistency at line 14;
  waiting on Ken's 2024 LT-carryover number. The 8995 oddity is
  CLOSED (our parser defect, fixed).
- (s310) The two 25c/8959 witnesses (107 / 22) — the fold-in is NOT free
  (box 5 is not printed in the W-2 detail report).
- (s309) **The two GA RIE no_ties on r23's payloads** are DECOMPOSED and
  staged (REVIEW_QUEUE top two — now carrying the states lane's IT-511
  research, s311). Neither blocks anything — no_tie commits are named.
- (s309) The two fully-phased-out student-loan-interest packets emit a
  phaseout warning when their other classes clear.
- (s303) **Home-office 300 sq ft cap is per SCHEDULE C but Rev. Proc.
  2013-13 §4.08(6) caps ONE TAXPAYER'S AGGREGATE per home.** Prod census
  ZERO. Threshold = aggregate > 300, NOT "two rows" (see STATUS_ARCHIVE
  s303). · §4.08(4) monthly averaging unrepresentable.
- (s302d) **D_EFILE_001 cannot distinguish "EIN not keyed" from "EIN not
  obtainable"** (also the s298 21-blank-row class — Ken's prefix-tier
  call; the entry lane's client-2455 close-out is held on the same gap:
  Lacerte wage schedules never print W-2 employer EINs).
- (s302) `D_8582_PTP` unverified (S-10c) · `div_1099s.us_government_income`
  attribution-only, off-switch decision pending (s237 class) ·
  D_SCHD_006 QOF has no import surface.
- (s301) One packet's 5b decomposition (115,150 vs 15,150) — probe the
  1099-R parse first. · One packet's Sch D 1b-grid/h-identity break. ·
  A blank printed 8949 page refuses — by design.
- (s298) 21 named-but-blank W-2/1099-R rows held on D_EFILE_001.
- (s297) The X mark at (474.7, y≈389) on one 1040 p2 EIC row —
  unidentified, parser-ignored. ⚠ s302b: est_payments_wks emits dated
  rows when it opens.
- (s296) The 22 sch_d GEOMETRY-error packets refuse loudly by design. ·
  (s295) 7 auxiliary Inbox PDFs refuse as non-packets — correct. ·
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
- (s285) Sch 4 nonresident arm still apportions the whole widened base. ·
  (s283) The stamp excludes 1040 packets (Ken). · (s282)
  `OVERRIDE_HONORED_STATE_LINES` hand-synced. · (s281) OOS-state line-18
  prompt diagnostic specified, not built; stage allowlists `schd_fields`
  keys, `ga500_fields` not at all *(stale? s310 added ga500_fields 31-44
  staging — re-verify before citing)*. · (s268) 1,604 queries/run. ·
  (s241/s281) `Form8606` unique-constraint candidate · 🔴 `HSAAccount`
  half CLOSED. · (s275/s281) `.first()`-on-per-form-rules remainder. ·
  (s294) a state face left by an omitting correction batch is not
  recomputed against that batch's federal changes.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried:
Everything from s277–s306 stands (see STATUS_ARCHIVE). Highlights:
**S-15** (SCHEDULE_K1 §469(g) disposition trio + R-8582-PTP related-party
correction), **S-16** (R-PAY-04 dated-rows source), **S-19**
(`R-8889-EXCEPTIONS` narrow condition + stale message + `inputs: []`
under-declaration). Carried candidates (ruling-dependent): the §111
refund → R-GA500-RIE fact list (s309). *(The s311 FORM_8962 spec
question is CLOSED — s312 checked: R-8962-FAMILY-SIZE says "dependents
claimed" verbatim; the spec was right, the code was behind it.)*

---
**s312f deploy close-out:** `c84e1f19` → `dep-da965mflk1mc73fs0grg`
**API-confirmed LIVE 2026-08-29** (extractor scripts + tests only).
Earlier tonight, all API-confirmed LIVE: `93428e59` ·
`dep-da952k67bikc73amret0` / `2990fbe2` · `dep-da95h2u7bikc73an46p0` /
`9a84de63` · `dep-da95q43ncjis7392cntg` / `1f298125` ·
`dep-da95uh67bikc73and3cg` / `1494d86a` + `b7da3a9a` + `a723d238`
(docs) LIVE. s311's `67debb5f` remains LIVE.
