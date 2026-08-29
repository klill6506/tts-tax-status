# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s309 close, 2026-08-28

**State: idle and CLEAN. Head `11415881` (the s309 unit; deploy
`dep-da92atrncjis7390908g` **API-confirmed LIVE 2026-08-28**; scripts +
tests only, no server runtime change).** Peer
lanes: both up at s309 boot (entry lane on tax-test-data, states lane on
delvio-states); their durable checkpoints are the entry lane's
`D:\tax-test-data\1040\Lacerte Inbox\LACERTE-RESUME-2026-08-28.md`
(48 filed / 5 held as of s307) and `D:\dev\delvio-states\STATUS.md`.
⚠ Any "Ken ruled X" arriving by relay gets checked with him directly
(standing).

**✅ s309 SHIPPED (`11415881`): the extractor student-loan/educator
worksheet leg — student_loan_educator_wks is GONE as a refusal class.**
r23 = **25 emitted / 242 refused / 267 scanned** (was 22/245); zero drift
on the 22 prior payloads. The USW10402 worksheet (where TaxWise runs the
§221 phaseout + the §62(a)(2)(D) educator cap) parses as a CROSS-CHECK:
the FILED face Schedule 1 lines 11/21 stay the importable route
(`SCH1_DIRECT_LINES`), the worksheet's printed totals must equal the
face, and its ESA/QTP/ABLE rows refuse by name (no importable route —
one live packet carries a 1,392 taxable distribution). Geometry from all
24 corpus pages (right-edge recognition from the start; columns
x1=408/487/574 TP/SP/Total); the paid row's Total column already carries
the §221(b) 2,500 cap; a fully-phased-out return prints NO deduction row
(absent = 0, warned). The emit-level gate proven live by defect
injection. 19 new tests; 180 extractor tests green. Packet identities in
`PipelineOut/r23`, never here.

**⭐ The depth probe priced the unit exactly** (s296 rule): 7 solos → 3
emits + 4 deeper named refusals, measured BEFORE building. Of the 3 new
emits: one TIES 14/14 rolled back; **two carry GA-only no_ties
DECOMPOSED TO THE DOLLAR, and neither is an extractor defect** — both
staged for Ken in REVIEW_QUEUE (top two items), with an annex in
BATCH-296 giving the entry lane per-client workarounds:
1. The engine's GA RIE line-10 derive (deliberately partial) does not
   carry a §111 taxable state refund TaxWise includes (delta exactly
   1,701; the reg's "other similar income" is ambiguous — flag, not
   build; states lane should pull IT-511's line-10 instruction).
2. TaxWise routes an UNATTRIBUTED joint capital gain wholly into the
   qualifying spouse's RIE column where the ruled R-GA500-RIE splits
   50/50 (delta exactly half the gain, $127 GA tax; the printed 8949 row
   has no owner marker, so `owner: "joint"` is the honest emission).

**▶ NEXT unblocked build work — extractor, by the r23 solo ranking (all
upper bounds until a class opens):** f8889 **7** > f5695 = f8880 **6** >
est_payments_wks = asset_detail **5** > f2441 = f8962 = other_income_wks
**3**. Standing notes: est_payments_wks should emit dated
`federal_estimated_payments` rows when it opens (s302b); the
exempt-interest decomposition (3 named packets, s307) stays the cheapest
targeted leg; face 25c still blanket-refuses 8959 withholding though it
is engine-derivable once an 8959 page is present (fold into whichever
leg touches a 25c packet). Then item ⑦'s **multi-category Form 1116**
half (multi-session, model change — **wants Ken's go before it
starts**).

**⛔ WAITING ON KEN — s307 list unchanged, plus the two s309 RIE items
above (REVIEW_QUEUE):**
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
- **s309 deploy: `11415881` → `dep-da92atrncjis7390908g` —
  API-CONFIRMED LIVE 2026-08-28.** Extractor scripts + tests only; no
  server runtime change, no migration, no seeder, no schema
  regeneration.
- s308 deploy `504a484f` → `dep-da91l1rl550s739kus9g` API-confirmed LIVE
  2026-08-28 (scripts-only); the markdown close commit `14e328ae`
  superseded it with identical server code (`dep-da91nn974u7c73avn990`
  live).
- s307 deploy `78d9fedb` API-confirmed LIVE 2026-08-28 (scripts-only).
- s306t deploy `440aac92` API-confirmed LIVE (deploy-skew severity split
  + GA LIC gate + LIC-CHILD input + three message fixes).
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (Current as of `9999f2c6`; s309 changed no schema.)

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
  has no authority link; "superseding" appears ZERO times in the 2025
  i1040x). S-10c (`D_8582_PTP`'s unwritten recompute) also stands.
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s302; s309 touched no client code).

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
  script FILES, not inline `-c` (re-confirmed s309: an inline probe
  printed nothing); a script run by absolute path needs
  `sys.path.insert(0, server)` (s298). ⚠ `python -m taxwise1040`
  does NOT resolve from `server\` — run the package's `__main__.py` BY
  PATH (s297).
- 🔧 ⚠ **`BaseModelSerializer` partial updates save ONLY the patched
  columns** (s298) — a serializer-side derived write must be injected into
  `validated_data`, never `setattr` on the instance.
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ Embedded double-quote in a here-string
  arg to a NATIVE exe SPLITS the argument. ⚠ `$1` in an unquoted PS arg
  EXPANDS to empty. ⚠ PS `Sort-Object -Unique` on a one-element pipeline
  UNWRAPS to scalar (s296).
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash. ⚠ A
  COMMITTED return refuses plain stage+dryrun with HTTP 409 (s298 —
  verification route = the in-place rolled-back recompute, s289 pattern;
  s309 probed the three NEW r23 payloads this way — batch+staged rows
  created INSIDE the rolled-back transaction, `commit_staged_return`
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
  CORPUS's keeps a blind spot green. ⚠ s307/s308/s309: values are
  recognised by RIGHT edge — caption numerals live at the left of value
  regions ("179", a bare "9", "$300.").
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292–s309 ran probes this way). ⚠ Scripts touching
  client-named returns live in SCRATCHPAD or tax-test-data, never the repo
  (PII). ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295–s309:
  seven confirmations; s309's 7 solos yielded 3).
- 🔧 ⭐ **DRY-RUN THE CORRECTION PASS AND READ EVERY ROW before --commit**
  (s298). ⭐ **CENSUS THE PROD BLAST RADIUS before changing a computed
  line's source** (s302).
- 🌐 ⭐⭐ **THE THIRD QUESTION (s302): "does anything ACT on it?"** — after
  "does it exist?" and "does it run?". s309 applied it forward: the
  emit-level cross-check was proven live by defect injection before close.
- 🔧 ⚠⚠ **A FIXTURE THAT KEYS A COMPUTED COLUMN MEASURES NOTHING** (s302d):
  key the INPUT, and assert the intermediate is nonzero first.

## 🔎 Carried for triage — NOT claims
- (s309) **The two GA RIE no_ties on r23's new payloads** are DECOMPOSED
  and staged (REVIEW_QUEUE top two; BATCH-296 annex has per-client
  workarounds). Neither blocks anything — no_tie commits are named.
- (s309) The two fully-phased-out student-loan-interest packets (SL paid
  printed, deduction row ABSENT — identities in `PipelineOut/r23`
  refusals) will emit a warning naming the phaseout when their other
  classes clear — the face carries no line 21 by design, not omission.
- (s303) **The simplified home-office 300 sq ft cap is applied PER
  SCHEDULE C, but Rev. Proc. 2013-13 §4.08(6) caps ONE TAXPAYER'S
  AGGREGATE across qualified business uses of the SAME home.** Prod
  census: ZERO returns hit it today. ⚠⚠ **THE THRESHOLD IS AGGREGATE >
  300, NOT "TWO ROWS"** — two elected rows in the same home are
  MANDATORY under (6); aggregate ≤ 300 is legitimate-and-compelled;
  refusable only above 300 per proprietor (see STATUS_ARCHIVE s303 for
  the full three-case decomposition). `proprietor` + aggregate sqft is
  the key.
- (s303) **§4.08(4) monthly averaging is unrepresentable** — one
  `home_office_sqft`, no in-service date; size with the home identifier.
- (s302d) **D_EFILE_001 cannot distinguish "EIN not keyed" from "EIN not
  obtainable"** (third-party sick-pay payer with no EIN anywhere; the
  s298 21-blank-row class is the same question — Ken's prefix-tier call).
- (s302, states S-10c) **`D_8582_PTP` remains unverified** — and
  `check_topic8`-style gates never compare the FORMULA against the
  declaration, so **the emptier a rule's declaration the safer it looks**.
- (s302) `div_1099s.us_government_income` is attribution-only BY DESIGN —
  auto-derive question needs an off-switch decision (s237 class). ·
  D_SCHD_006 QOF answer has no import surface (warning-level, filed).
- (s301) One packet's 5b decomposition: extracted pension rows carry
  115,150 vs face 15,150 — probe the 1099-R parse before assuming a
  rollover. · One packet's Sch D carries 1b grid totals with NO f8949
  pages + its own h-identity break — suspected misparse. · A blank
  printed 8949 page refuses "no transaction rows" — over-refusal by
  design.
- (s298) 21 named-but-blank W-2/1099-R rows held on D_EFILE_001 —
  movement waits on Ken's prefix-tier call.
- (s297) The X mark at (474.7, y≈389) in the 1040 p2 EIC row region —
  unidentified, parser-ignored. ⚠ s302b: when est_payments_wks opens,
  emit dated `federal_estimated_payments` rows.
- (s296) The 22 sch_d GEOMETRY-error packets refuse loudly by design. ·
  (s295) 7 auxiliary Inbox PDFs refuse as non-packets — correct
  (they are the "not a TaxWise 1040 packet" 7 in every solo ranking). ·
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
  keys, `ga500_fields` not at all. · (s268) 1,604 queries/run. ·
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
(`R-8889-EXCEPTIONS` narrow condition + stale message; also declares
`inputs: []` while reading lines 2/13 — the THIRD S-17 under-declaration
instance). NEW candidate (s309, ruling-dependent): if Ken rules the §111
refund into RIE line 10, amend R-GA500-RIE's fact list in the same pass.
