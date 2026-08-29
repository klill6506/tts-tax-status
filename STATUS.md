# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s311 close, 2026-08-29

**State: idle and CLEAN. Head = the s311 unit (extractor f8880 leg +
the GA-500 7a claimed-dependents fix — ONE server runtime change in
views.py; deploy verification at the bottom of this file).** Peer
lanes at s311: the entry lane (tax-test-data-dc) reported the Lacerte
queue EMPTY (49 filed / 5 held; client 2455 filed TIE) and client 3218's
twr24 commit queued behind an expired prod token — Ken mints, they
commit. The states lane delivered the IT-511 line-10 research
(REVIEW_QUEUE item updated — see below). ⚠ Any "Ken ruled X" arriving
by relay gets checked with him directly (standing).

**✅ s311 SHIPPED: the extractor Form 8880 (Saver's Credit) leg —
f8880 is GONE as a refusal class — plus the GA-500 line-7a
claimed-dependents-only fix the tie probe forced.** r25 = **28
emitted / 239 refused / 267 scanned** (was 26/241); zero drift on the
26 prior payloads; BOTH new emits tie-probed rolled back (**13/13 and
15/15** — the second exercising the s310 GA lines 31-44 machinery
live: line 30/46 amount-due 53) and are flagged clean-to-commit in
the annex (clients 3427 and 4666, batches twr25-001/-003 — commit
ONLY those two; twr25 re-bundles everything, and client 3218's twr24
commit stands as already instructed). 16 new tests (15 parser + 1
derive); 222 extractor tests, 37 dependent tests, 616 flow-assertion
+ GA suite tests all green. Packet identities in `PipelineOut/r25`,
never here.

Unit shape (detail in form_coverage_tracker + the commit message):
- The 8880 face parses into the s202 `f8880_*` taxpayer vocabulary
  (the engine recomputes the credit; line 2 emits as the deferral
  OVERRIDE — the W-2 detail report prints no box 12, so the box-12
  derive is structurally 0 and the override is the designed route).
  Face line 20 gates against printed line 12: TaxWise omits the
  Schedule 3 page (s297 omission class), so the 8880 face is line
  20's only witness; residuals refuse by name.
- **⚠ THE ENGINE FIX (one no_tie, decomposed to the dollar): the s235
  GA-500 line-7a derive counted `claimed_as_dependent=False` EIC-only
  rows** — a field that postdates it (s281) and whose own contract
  excludes such rows from "every dependency-derived count". 3 × 4,000
  × 5.19% = 622 = the exact GA line-16 no_tie; TaxWise and O.C.G.A.
  §48-7-26 grant no exemption for an unclaimed EIC-only child. The
  derive + its LIC-CHILD twin now filter claimed-only. Prod census:
  ONE filed return carries a false row, NO GA-500 attached — nothing
  committed moves.
- The s295 upper-bound rule again: 8 solos → 2 emits + deeper named
  refusals — a NEW class (the more-than-four-dependents box, 1
  packet), a shell-carries-documents -Merge route (agent lane), and
  four packets now held ONLY by the 13b Schedule 1-A class.

**▶ NEXT unblocked build work — extractor, by the r25 solo ranking
(all upper bounds):** f5695 = est_payments_wks **6** > asset_detail =
f2441 **5** > f8863 = f8962 = other_income_wks **3**. Standing notes:
est_payments_wks should emit dated `federal_estimated_payments` rows
when it opens (s302b); the exempt-interest decomposition (3 named
packets, s307) stays the cheapest targeted leg; the 25c/8959 fold-in
has two named witnesses but needs its own depth probe (box 5 is not
printed; s310). **NEW (s311, before any f8962 extractor leg):
`compute_8962.family_size` counts ALL Dependent rows where §36B
counts CLAIMED dependents — same class as the 7a fix; verify the RS
FORM_8962 spec's family-size fact, then fix + test (population today:
1 return, no 8962).** Then item ⑦'s multi-category Form 1116 half
(multi-session, model change — **wants Ken's go before it starts**).

**⛔ WAITING ON KEN — unchanged from s310 except item 7:**
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
9. **Prod token for the entry lane** — client 3218 (twr24) + clients
   3427/4666 (twr25) commit when minted; ⚠ the 3427 tie needs the s311
   deploy LIVE first (the 7a fix is server-side).

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
- **s311 deploy: see the close-out line at the very bottom of this file
  for the API-confirmed status.** ONE server runtime change (the
  views.py 7a/LIC-CHILD claimed-only filter); no migration, no seeder,
  no schema regeneration (`f8880_*` fields are s202 vocabulary, already
  in the published schema).
- s310 deploy `972cf50e` → `dep-da934fvlk1mc73fq7ng0` API-confirmed LIVE
  2026-08-29 (scripts-only). · s309 `11415881` LIVE. · s306t `440aac92`
  LIVE.
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
- (s311) **`compute_8962.family_size` counts ALL Dependent rows; §36B's
  tax family is taxpayer + spouse + CLAIMED dependents** — the 7a class.
  Population today: 1 return with a false row (no 8962 on it). Fix wants
  the RS FORM_8962 spec's family-size fact verified first. NOT built.
- (s311) **The more-than-four-dependents box** (1 packet): the marked box
  means dependents beyond the grid exist only outside the packet —
  refuses by name; no route until a source exists.
- (s310) **A Schedule D identity break on one refused packet** (line 15
  −19,103 vs the column (h) sum −21,142, delta 2,039 — identity in
  `PipelineOut/r25` refusals). Decompose before hand-entry (annex warned
  the entry lane; they hold it do-not-hand-enter).
- (s310) **A Form 8995 single-digit-row oddity on one refused packet**
  (face line 9 prints 0 while line 10 prints 9) — triage before
  hand-entry (entry lane holds it).
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
refund → R-GA500-RIE fact list (s309); **NEW s311: if the FORM_8962
spec's family-size fact says "dependents" without the claimed
qualifier, it inherits the s302 spec-omits-the-bar class — check when
the 8962 family_size fix runs.**

---
**s311 deploy close-out:** `67debb5f` → `dep-da94p1f10e5c73aqk93g` —
**API-confirmed LIVE 2026-08-29** (the ONE server runtime change: the
GA-500 7a/LIC-CHILD claimed-only filter; extractor scripts + tests
otherwise). ⚠ Client 3427's entry-lane commit is now unblocked — her
tie needs this deploy, which is live.
