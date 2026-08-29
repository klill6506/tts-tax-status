# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s315 close (daytime, all three peer lanes active), 2026-08-29

**State: idle and CLEAN. ALL NINE queue returns are COMMITTED (entry
lane, Ken's go): seven from twq9 + the two DOB-held from
`twq10-20260829` (batch 62745692-3fa0-443c-b5fb-380f9a56217a), every
one a TIE. None marked filed, no closeout run — both still awaiting
Ken.** The day's headline was the entry lane's commit-blocking find:
**every emitted dependent DOB carried a fabricated Jan-1 month/day**
(the emitter used the Schedule EIC birth YEAR while the Main Info
sheet printed the full date in a dependents section no parser read).
Fixed same-day; the entry lane re-verified digit-for-digit.

**✅ s315 SHIPPED (four commits, deploy per the close-out line):**
- `dd8afe65` — **dependent DOBs from the Main Info dependents section**
  (rows between Filing Status and the Preparer block, corpus-pinned
  bands). Full date keyed by SSN; EIC year cross-checks it (mismatch
  refuses); a bare-year-only dependent REFUSES by name — fabrication
  retired. **r32 = 36/231/267**: drift vs r31 EXACTLY dependent
  date_of_birth (6 corrected + 1 gained), zero regressions, and the
  **morning list's 19-birth-years ask DISSOLVED** — all 19 "carries no
  birth year" refusal lines vanished (the dates were in the packets),
  two packets newly emitted (clients 1828, 4053).
- `ddae39bc` — **D_CREDIT_ODC explains through ODC's OWN gates**
  (R002 spec-verified: claimed/citizenship/TIN — §24(h)(4) has no age
  test). The "is 35 … must be under 17" explainer class is gone;
  outcomes were always computed right.
- `998318c7` — **lookup/staging `docs=` runs the full ~30-family
  SECTION_RELATED census** (the commit gate's own census; the entity
  twin already did). The old four-family count read docs=0 on shells
  holding only a Schedule C / 1095-A / W-2G — the entry lane caught
  three commits off by exactly one uncounted type each.
- `63e25a00` — **the g1099_detail extractor leg** (1099-G unemployment
  detail; federal + GA variants reconcile 1:1, line 7 decomposes both
  directions, box 4 joins the 25b roster, GA WH joins the s313 line-24
  attribution sum). **r33 = 38/229/267**, zero drift, zero
  regressions. 264 extractor tests green.

**Rolled-back tie probes on the four new emits:** client 4053 TIES
20/20 (available whenever Ken blesses queue additions). Clients 1828,
2947, 3754 are DECOMPOSED no_ties, not queue candidates: 1828 + 2947
fail SOLELY on the §6654 penalty (deltas 89 / 166 — **the 2210
prior-year safe-harbor class, now THREE witnesses** with committed
client 2968's warning); 3754 adds the cleanest **GA RIE composition
witness yet** (delta EXACTLY his 365 of unemployment — REVIEW_QUEUE
s309 item updated: unemployment joins the §111 refund under the same
"other similar income" question) plus a 3,730 face-13b residual above
the statutory 12,000 senior deduction (DEFERRAL_AUDIT item 6).

**▶ NEXT unblocked build work (all priced in DEFERRAL_AUDIT s315):**
① the **2210 prior-year leg** (top candidate — converts two no_ties,
find where TaxWise prints the prior-year figures); ② the **f7206 leg**
(2 immediate emits, route exists); ③ the **nol_wks transcription leg**
(2 emits + carryforward preservation, the s235 rows); ④ the
classify-ignore for the MFJ/MFS comparison worksheet (1 emit) + named
refusals for 6781 / GA-itemized-wks / VA-multistate pages; ⑤ the 65+
13b senior-residual refinement (**before the next 65+ 13b emit is
staged**). Older deferred legs unchanged: f8959 (trigger now MET —
birth years exist — but all carriers still hold other walls;
re-verify), asset_detail (Ken's domain, daytime), item ⑦'s 1116 half
(**Ken's go first**).

**⛔ WAITING ON KEN — the morning list SHRANK:** ~~token ×9~~ **DONE
(all nine committed)** · ~~19 birth years~~ **DISSOLVED (packet data
all along)** · still standing: the one-number Schedule D carryover
question (32,002 vs 29,963) · the reprint asks · client 4059's W-2G
payer street address (document ask — committed and tying, cannot
e-file) · mark-filed / closeout authorization for the nine · standing
decisions items 1–8 (archived s312 block) · the 2a ruling-scope flag
(REVIEW_QUEUE top) · the states lane's three diagnosed items (+ their
AL D-16 prose application `5a1f9e0` is done in RS; prod reseed is
their direct ask to Ken).

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
- **s315 deploy: see the close-out line at the bottom of this file.**
- s314's terminating docs-only commit `22a2a597` →
  `dep-da9g7nou01pc73d1q9i0` was API-confirmed LIVE at s315 boot (the
  s314 close-out loop is fully settled).
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (s315 changed no vocabulary — G1099_FIELDS pre-existed from s224; the
  docs= change is response-shape only.)

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
against what a batch file or your own probe shows. *(s315 practiced it
again: the DOB claim was code-verified before building; their "~31
committed" class of error did not recur — their docs= qualification was
itself verified in code and answered with the section census.)*

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
- **Client typecheck**: green (s302; s315 touched no client code).

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
  arg to a NATIVE exe SPLITS the argument (re-hit s315 — a git commit -m
  here-string with an embedded quote split and MISASSEMBLED two commits;
  caught pre-push, reset --soft and redone with `-F` files. ALWAYS `-F`).
  ⚠ `$1` in an unquoted PS arg EXPANDS to empty. ⚠ PS `Sort-Object
  -Unique` on a one-element pipeline UNWRAPS to scalar (s296). ⚠
  `[IO.File]` calls resolve against the PROCESS cwd (s310) — absolute paths.
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash. ⚠ A
  COMMITTED return refuses plain stage+dryrun with HTTP 409 (s298 —
  verification route = the in-place rolled-back recompute, s289 pattern;
  s315 probed four candidates this way — batch+staged rows created INSIDE
  the rolled-back transaction, nothing landed). ⚠ An EXCLUDED staged
  return 409s BEFORE the merge parameter is read (s315) — the designed
  stale-batch kill switch; per-return `/exclude`, preparer-run.
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
  ⚠ s315: a single-value report's per-column mini-ruler is ONE dash word
  — below `_table`'s two-word ruler test — so the totals row arrives
  ownerless inside the body; classify in-loop, never assume the ruler.
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292–s315 ran probes this way). ⚠ Scripts touching
  client-named returns live in SCRATCHPAD or tax-test-data, never the repo
  (PII). ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295–s315:
  now also at TIE grain — both s315 g1099 emits parsed perfectly and
  no_tied on OTHER walls; a solo conversion is not a queue add until the
  rolled-back tie probe says so).
- 🔧 ⭐ **DRY-RUN THE CORRECTION PASS AND READ EVERY ROW before --commit**
  (s298). ⭐ **CENSUS THE PROD BLAST RADIUS before changing a computed
  line's source** (s302).
- 🌐 ⭐⭐ **THE THIRD QUESTION (s302): "does anything ACT on it?"** — and
  s315's converse: the Main Info dependents section was PRINTED on every
  packet and read by nothing; grep for the page section nobody parses.
- 🔧 ⚠⚠ **A FIXTURE THAT KEYS A COMPUTED COLUMN MEASURES NOTHING** (s302d):
  key the INPUT, and assert the intermediate is nonzero first.
- 🔧 ⚠⚠ **A NEW BOOLEAN FIELD RE-SCOPES EVERY COUNT THAT PREDATES IT**
  (s311). When a field's contract says "every X", grep for every X.
- 🔧 ⚠ **The answer key is a CONTRACT** (s313/s314): new face reads that
  only feed gates go in as INPUT MATERIAL, never into `expected`, unless
  a wholesale re-verification is intended. ⚠ **A TIE CANNOT SEE
  NON-RECONCILED FIELDS** (s315, the entry lane's ⭐): the DOB fix moved
  nothing a tie sees — identity fields need their own verifier
  (DEFERRAL_AUDIT item 7).

## 🔎 Carried for triage — NOT claims
- ~~(s313) the four 25c/8959 witnesses' DOB gates~~ — birth years now
  extract (s315); the f8959 deferral's trigger is MET but each carrier
  holds other walls — re-verify the cheapest carrier before building.
- (s313) **Client 4059's D_W2G_PAYER_ID error** — committed and tying;
  the W-2G payer address is ABSENT from the packet (document ask).
- (s313) The entry lane's `verify_expected.py` reader misses the 1040
  inner band (line 38) and reads the extractor's explicit-zero GA 16/23
  convention as MISSING — their tool, noted in the annex.
- (s312) **A $56 unextracted Schedule 3 line-1-3/6d/6l credit on one
  f5695 packet** — the 5695 line-31 CLW names it.
- (s311) **The more-than-four-dependents box** (1 packet) — refuses by
  name; no route until a source exists.
- (s310→s312c) **The Schedule D identity break is DECOMPOSED** — vendor
  self-inconsistency at line 14; waiting on Ken's 2024 LT-carryover
  number (morning list).
- (s309) **The GA RIE no_ties: now FOUR witnesses** (REVIEW_QUEUE — the
  s315 unemployment witness is exact-to-the-kind; two s309 originals;
  one undecomposed). · The two fully-phased-out student-loan-interest
  packets emit when their other classes clear.
- (s303) **Home-office 300 sq ft cap**: aggregate per home; prod census
  ZERO. · §4.08(4) monthly averaging unrepresentable.
- (s302d) **D_EFILE_001 cannot distinguish "EIN not keyed" from "EIN not
  obtainable"** (also the s298 21-blank-row class).
- (s302) `D_8582_PTP` unverified (S-10c) · `div_1099s.us_government_income`
  attribution-only, off-switch decision pending (s237 class) ·
  D_SCHD_006 QOF has no import surface.
- (s301) One packet's Sch D 1b-grid/h-identity break. · A blank printed
  8949 page refuses — by design.
- (s298) 21 named-but-blank W-2/1099-R rows held on D_EFILE_001.
- (s297) The X mark at (474.7, y≈389) on one 1040 p2 EIC row —
  unidentified, parser-ignored. ⚠ s302b: est_payments_wks emits dated
  rows when it opens — **and the s315 2210 finding repriced that leg**
  (DEFERRAL_AUDIT item 1).
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
  prompt diagnostic specified, not built. · (s268) 1,604 queries/run. ·
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
refund + unemployment → R-GA500-RIE fact list (s309/s315, one ruling).
NEW (states lane, s315): AL D-16 prose applied in RS (`5a1f9e0`);
their `[UNVERIFIED]` on the individual owner's Form 40 claim line —
this app models it as Schedule OC refundable → Form 40 line 25
(direct-entry, NOT a verbatim ALDOR citation; told them so).

---
**s315 deploy close-out:** the four-commit push (`dd8afe65` /
`ddae39bc` / `998318c7` / head `63e25a00`) → deploy
`dep-da9ktjnlk1mc7387dgb0` **API-confirmed LIVE**. Runtime changes:
credit_gates (ODC explainer) + backentry (docs= census) — no
migration, no seeder, no schema regeneration. ⚠ The docs-only close
commit(s) after this line trigger further deploys — verify the LAST
one at next boot (the s314 terminating pattern; expect a clean
docs-only build on top of `63e25a00`).
