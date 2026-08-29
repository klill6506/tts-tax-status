# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s313 close (daytime, Ken said "go" then away; peer entry lane active), 2026-08-29

**State: idle and CLEAN. One extractor unit shipped (`e728a72b`,
deploy API-confirmed LIVE): THREE new tie-verified emits
join the queue — r30 = 34 / 233 / 267 — and EVERY payload now carries
the signing preparer's PTIN (an entry-lane-reported defect, fixed
same-day). The token queue is now SIX + THREE.** The entry lane is
running (token-blocked, verify-only): it hand-fixed the six queued
payloads' PTINs in its consolidated batch `twq6-20260829` (byte-equal
to the r28 originals except the preparer block — USE IT for staging,
my annex ratifies it), built an offline expected-vs-face verifier
(`1040\tmp\verify_expected.py`), and withdrew its 4609 itemizer-credit
no_tie prediction after reading the s312f probe (line 19 is
engine-derived, Ken-ruled 2026-08-23 — not the 2234 LIC shape).

**✅ s313 SHIPPED (`e728a72b`, extractor scripts + tests only):**
- **W-2G withholding routing** — the 25b/25c decomposition now mirrors
  the engine's rosters (FORM_W2G spec verbatim: "a W-2G is not a
  1099"; box 4 → the 25c roster). The blanket face:25c refusal is a
  RESIDUAL check; the surviving class is the genuine Form 8959
  residual (4 packets, all of which print an 8959 page — parser leg
  deferred at zero immediate yield, see DEFERRAL_AUDIT).
- **W-2G GA state attribution** — the filed GA-500 line 24 is the
  witness (`parse_ga500_line24`, input material only, NEVER the answer
  key — adding it there would have drifted every prior payload); an
  exact decomposition over W-2 GA + 1099-R GA + W-2G box 15 proves the
  state and stages `box13_state "GA"`; anything else refuses by name.
  The engine's line-24 roster already consumed box 15 gated on
  box13_state — the extractor simply never staged it (found as an
  exactly-2,696 no_tie split 1,695 due / 1,001 refund).
- **The 2a consolidated valve at AMOUNT GRAIN** — a face-2a amount is
  payer-less by construction even with a Schedule B present (the form
  never lists tax-exempt payers; no USSTB page in the corpus). Non-MFJ
  → consolidated row; MFJ refuses by name. **Ruling-scope flag for Ken
  in REVIEW_QUEUE (top item)** — recommendation: keep it.
- **GA Schedule 1 line 9 (Path2College) transcribes to S1-9** (seeded
  line the engine already sums into S1-13; the beneficiary count is
  not printed — D_GA500_006 governs the cap). Lines 11/12 refuse by
  name (12 may fold the HB 463 tips/overtime exclusions the engine
  computes separately as S1-12a/12b).
- **The preparer PTIN on every payload** — read off the 1040 p2 Paid
  Preparer block ONLY (DECISIONS s280; the GA-8453 ERO PTIN
  legitimately differs on two packets — trap avoided by page
  selection, pinned in tests). PTIN is the roster match's
  authoritative key; all three fresh probes assigned cleanly (zero
  preparer warnings).
- Verification: 256 extractor tests green (8 new); r30 drift vs r29 is
  EXACTLY `preparer.ptin` everywhere + the two named field additions;
  three rolled-back tie probes ALL TIE (16/16, 18/18, 15/15) —
  `tmp\tie_probe_s313.txt` / `s313b.txt`.

**▶ NEXT unblocked build work — the corpus's immediate-yield tail
remains EXHAUSTED after these three.** Remaining named legs: the
f8959 parser (deferred, trigger = birth years arriving), asset_detail
(multi-session, gated behind a sch_e leg, daytime design in Ken's
depreciation domain), item ⑦'s multi-category 1116 half (**Ken's go
first**). Every other refusal is data-blocked (DOB, tokens) or a
named single wall.

**⛔ WAITING ON KEN — the s312d MORNING LIST stands (BATCH-296 tail,
two annexes above the s313 one): the token mint now unblocks NINE
commits (six + clients 2968 / 3852 / 4059 — stage
the six from `twq6-20260829`, the three from twr30), 19 dependent
birth years across 14 packets, the one-number Schedule D carryover
question (32,002 vs 29,963), the reprint asks.** Standing decisions
items 1–8 unchanged (see the archived s312 STATUS block in
STATUS_ARCHIVE for the enumerated list: GA SALT add-back conflict ·
per-property nonpassive · 3 firm-EIN W-2s · 1071 · 1141 · "no need to
fil" packet · the two GA RIE questions · the states lane's 10
unruled). NEW for Ken: the 2a ruling-scope flag (REVIEW_QUEUE top,
nothing blocked) · client 4059 needs ONE DOCUMENT to file: her own
W-2G (or the firm's copy) for the lottery payer's street address —
D_W2G_PAYER_ID; the address is ABSENT from the packet, and per
refuse-don't-guess nobody fills a filed record from general
knowledge. She ties and commits either way; the gate is the filed
sweep (the D_EFILE_001 shape).

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
- **s313 deploy: see the close-out line at the bottom of this file.**
- s312 deploys all API-confirmed LIVE 2026-08-29: `93428e59` (8962
  family-size, the ONE runtime change) / `2990fbe2` (f5695) /
  `9a84de63` (s312c triage) / `1f298125` (sch2 relaxation) /
  `c84e1f19` (f8962 leg). s311 `67debb5f` LIVE.
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (Current as of `9999f2c6`; s313 changed no schema/vocabulary — the
  fields staged, `box13_state` and `preparer.ptin`, were already in the
  published allowlists.)

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
against what a batch file or your own probe shows. *(s313 practiced both
directions: the peer's PTIN claim was code-verified before building; their
4609 no_tie prediction was refuted from the s312f probe file and they
withdrew it.)*

## ⚠ Known red / rotted — THE ONE LIST (post-s302)
- ~~12 of 47 RS `check_*_integrity.py` gates FAIL~~ — **THE 12 WAS WRONG AND
  S-10 IS CLOSED** (states lane, 2026-08-27). True pre-existing count **7**,
  10 of 12 resolved, 10 gates pinned in `tests/test_integrity_gates.py`;
  RS suite **254 passed / 0 failed**. ⚠ S-10a survives (`R-1040X-SUPERSED`
  has no authority link). S-10c (`D_8582_PTP`'s unwritten recompute) also
  stands. **S-10b diagnosed (states lane, 2026-08-28, `cdd524f`): all 15
  schedule_a gate failures are GATE defects, none spec — never read that
  gate as authority for engine work.**
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s302; s313 touched no client code).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): five phantom failures. New files/markdown/scratchpad only.
- 🌐 ⚠ **The teardown warning "database test_postgres is being accessed by
  other users / 1 other session" is NOT lock contention on this setup** —
  it is YOUR OWN pooled connection (Supavisor; verified, states lane
  2026-08-28). Zero effect on results.
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
  s313 probed all three r30 payloads this way — batch+staged rows created
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
  (f8880: the marker/value split is x1 ≤ 493 vs ≥ 494, measured, s311).
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292–s313 ran probes this way). ⚠ Scripts touching
  client-named returns live in SCRATCHPAD or tax-test-data, never the repo
  (PII). ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295–s311:
  nine confirmations).
- 🔧 ⭐ **DRY-RUN THE CORRECTION PASS AND READ EVERY ROW before --commit**
  (s298). ⭐ **CENSUS THE PROD BLAST RADIUS before changing a computed
  line's source** (s302).
- 🌐 ⭐⭐ **THE THIRD QUESTION (s302): "does anything ACT on it?"** — s313's
  S1-9 was its cleanest instance yet: parsed correctly, consumed by
  NOTHING, invisible until the tie probe. s310: grep the enclosing scope
  before defining a nested helper.
- 🔧 ⚠⚠ **A FIXTURE THAT KEYS A COMPUTED COLUMN MEASURES NOTHING** (s302d):
  key the INPUT, and assert the intermediate is nonzero first.
- 🔧 ⚠⚠ **A NEW BOOLEAN FIELD RE-SCOPES EVERY COUNT THAT PREDATES IT**
  (s311). When a field's contract says "every X", grep for every X.
- 🔧 ⚠ **The answer key is a CONTRACT: adding an expected line drifts every
  prior payload** (s313) — new face reads that only feed gates go in as
  INPUT MATERIAL (`parse_ga500_line24` pattern), never into
  `expected.ga500`, unless a wholesale re-verification is intended.

## 🔎 Carried for triage — NOT claims
- ~~(s310) The two 25c/8959 witnesses~~ — **PROBED s313**: now FOUR
  witnesses, ALL print an in-packet Form 8959 page; the fold-in is a
  deferred parser leg (DEFERRAL_AUDIT), trigger = birth years arriving.
- (s313) **Client 4059's D_W2G_PAYER_ID error** — the W-2G payer
  address is ABSENT from the packet; ties/commits but cannot file
  until the client's own W-2G document supplies it (a document ask,
  not a keying task — refuse-don't-guess held).
- (s313) The entry lane's `verify_expected.py` reader misses the 1040
  inner band (line 38) and reads the extractor's explicit-zero GA 16/23
  convention as MISSING — their tool, noted in the annex.
- (s312) **A $56 unextracted Schedule 3 line-1-3/6d/6l credit on one
  f5695 packet** (identity in `PipelineOut/r26` refusals) — the 5695
  line-31 CLW names it; the packet also carries ownerless-document
  refusals.
- (s311) **The more-than-four-dependents box** (1 packet) — refuses by
  name; no route until a source exists.
- (s310→s312c) **The Schedule D identity break is DECOMPOSED** — vendor
  self-inconsistency at line 14; waiting on Ken's 2024 LT-carryover
  number (morning list item 3).
- (s309) **The two GA RIE no_ties** are DECOMPOSED and staged
  (REVIEW_QUEUE, with the states lane's IT-511 research). · The two
  fully-phased-out student-loan-interest packets emit when their other
  classes clear.
- (s303) **Home-office 300 sq ft cap**: aggregate per home (Rev. Proc.
  2013-13 §4.08(6)); prod census ZERO. · §4.08(4) monthly averaging
  unrepresentable.
- (s302d) **D_EFILE_001 cannot distinguish "EIN not keyed" from "EIN not
  obtainable"** (also the s298 21-blank-row class; Lacerte wage schedules
  never print W-2 employer EINs).
- (s302) `D_8582_PTP` unverified (S-10c) · `div_1099s.us_government_income`
  attribution-only, off-switch decision pending (s237 class) ·
  D_SCHD_006 QOF has no import surface.
- (s301) One packet's Sch D 1b-grid/h-identity break. · A blank printed
  8949 page refuses — by design.
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
  keys, `ga500_fields` not at all *(stale — s313 verified: ga500_fields
  accepts any line-shaped key by design; S1-9 staged through it)*. ·
  (s268) 1,604 queries/run. · (s241/s281) `Form8606` unique-constraint
  candidate · 🔴 `HSAAccount` half CLOSED. · (s275/s281)
  `.first()`-on-per-form-rules remainder. · (s294) a state face left by
  an omitting correction batch is not recomputed against that batch's
  federal changes.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried:
Everything from s277–s306 stands (see STATUS_ARCHIVE). Highlights:
**S-15** (SCHEDULE_K1 §469(g) disposition trio + R-8582-PTP related-party
correction), **S-16** (R-PAY-04 dated-rows source), **S-19**
(`R-8889-EXCEPTIONS` narrow condition + stale message + `inputs: []`
under-declaration). Carried candidates (ruling-dependent): the §111
refund → R-GA500-RIE fact list (s309).

---
**s313 deploy close-out:** `e728a72b` → `dep-da9f3r7lk1mc73829830`
**API-confirmed LIVE 2026-08-29** (extractor scripts + tests only; no
runtime change, no migration, no seeder, no schema regeneration). All
s312/s311 deploys remain LIVE per the archived close-outs.
