# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s316 close (autonomous, "go"), 2026-08-30

**State: idle and CLEAN. THE ENTIRE s315 PRICED TAIL (①–⑤) IS BUILT —
four code pushes, every unit tie-probed rolled back before commit.
Corpus r33 38/229/267 → r38 41/226/267, zero unexplained drift at any
step.** Head `2fdb2ec8`; deploy verification in the close-out line.

**✅ s316 SHIPPED (five units, four code commits):**
- `1597f8f9` — **① the 2210 prior-year safe-harbor leg.** The figures
  print on the Three-Year Tax Summary (USSUMRY1) — NO federal 2210
  page prints at all (the invoice bills one; the s297 omitted-page
  pattern; the GA 500-UET carries only the GA prior-year tax). The
  2025 column self-calibrates against the packet's own 1040 face
  (AGI==11, tax-after-credits==24, EIC/ACTC==27a+28) before the 2024
  column is trusted; prior tax nets the refundable-credit row (the
  i2210 line-8 quantity); a zero/negative prior tax NEVER asserts the
  §6654(e)(2) waiver (witnessed live: a −1,641 EIC-exceeds-tax prior
  year, warned and not emitted). r34: 28 of 38 emits gained EXACTLY
  the two fields (1 first-year, 9 zero-prior-tax, all warned; 0
  unexplained). Probes: client 2947 → **full TIE** (166→0); client
  1828's federal fully ties (89→0); committed client 2968 verified
  UNMOVED in-place (harbor doesn't bind — the tie is safe).
- `b32e5632` — **② the f7206 SEHI leg.** Face parses by marker
  windows with its own derivation identities; premiums attribute to
  THEIR Schedule C business (line 4 == line 31; line 5 == the
  engine's per-owner positive-profit base, mirrored from
  `_line5_base`); only the INPUTS emit (sehi_amount/sehi_ltc_amount);
  Schedule 1 line 17 decomposes as Σ line 14. r35: EXACTLY the two
  priced transitions; one TIES in full; the other decomposes to **+5
  GA = the Low Income Credit keyed-gate class** (single, GA AGI
  15,517 = the $5/exemption IT-511 bracket — the s307 client-2234
  shape, entry-lane keyable via LIC-NODEP + LIC-CHILD).
- `40a47552` — **③④ the four `unknown` pages named + the 65+ 13b
  derivation gate.** MFJ/MFS comparison (USMFJVS1/2) ignores → its
  packet emitted; 6781 / GA-itemized-adj (GAW529$1) / GA-other-state-
  credit (GAWKSH$1) / VA-8453 refuse by name. Then DEFERRAL item 6's
  trigger tripped the same session (the new emit's probe no_tied +180
  = its own 13b residual): `derived_senior_deduction` mirrors
  compute_sch_1a R-SEN-01..09 (§70103) and any 13b residual refuses
  by name. r37: EXACTLY the two known residual carriers (+180; the
  s315 +3,730) converted emit→named refusal; all 21 other 13b
  packets re-verified against the engine arithmetic (the phase-out
  witness 1,496 = 6,000 − r0(6%×75,074) reproduces exactly).
- `2fdb2ec8` — **⑤ the nol_wks leg.** Per-vintage NOL carryovers →
  `carryforward_attributes` (kind=nol_regular, vintage + column D
  only — utilization is ENGINE-OWNED, s306j; F/H/Total are parser
  identities; nonzero C/G refuse by name). r38: both solos emit and
  **TIE in full** (a 2022 vintage 5,238 untouched; a 2020 vintage
  33,458 − 22,261 prior use).

**Extractor suite: 309 green** (was 264). No server runtime change all
session — scripts + tests only (the 13b gate and docs= are emit-side).

**▶ QUEUE CANDIDATES (Ken's blessing to stage/commit):** FOUR
tie-verified emits — client 4053 (s315, 20/20) + the three s316 TIEs
(the f7206 witness, and the two NOL-worksheet witnesses). A FIFTH
(the +5 GA LIC packet) ties as soon as the entry lane keys LIC-NODEP +
LIC-CHILD (the client-2234 recipe).

**▶ NEXT build (nothing unblocked without new pricing):** the priced
tail is EXHAUSTED again — remaining deferrals all carry triggers not
yet met: f8959 (every carrier holds other walls — re-verify the
cheapest), asset_detail (Ken's domain, daytime), item ⑦'s 1116
multi-category half (**Ken's go first**), the Sch-1A component
worksheet leg if a tips/OT/car-loan packet ever needs the 13b residual
imported (two named witnesses now refuse on it), f8606/detail_sheet
(zero immediate). Depth-probe candidates: none priced — next session
should re-census r38's residual classes.

**⛔ WAITING ON KEN (unchanged from s315 except the queue):** the
one-number Schedule D carryover question (32,002 vs 29,963) · reprints
· client 4059's W-2G payer street address · mark-filed/closeout for
the NINE committed · queue blessing for the four new TIEs · standing
decisions 1–8 · the 2a ruling-scope flag · **the GA RIE ruling now has
its cleanest tally: §111 refund ×2 (1,701 · 1,195 — the s316 probe
decomposed the fourth witness to the kind: her 1,195 prior-year GA
refund reappears as 2025 Other income) + unemployment ×1 (365); ONE
ruling clears three GA faces** (REVIEW_QUEUE updated) · the states
lane's items (their AL reseed ruling reached me as a RELAY — my Form
40 line-26/Schedule CP app leg waits on Ken's direct word; their L27
discriminating scenario recorded as the acceptance vector, and their
line-24 amended-payments gap noted so the four-term sum isn't read as
complete).

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
- **s316 deploys: see the close-out line at the bottom of this file.**
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (s316 changed no vocabulary — t2210_prior_year_*, sehi_*, and
  carryforward_attributes all pre-existed in the allowlists; every s316
  change is extractor-side.)

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. Peers stage; Ken decides.
*(s316 practiced it: the states lane borrowed test_postgres mid-session
and released it explicitly; their AL ruling arrived as a RELAY and was
NOT acted on — the app leg waits for Ken's direct word.)*

## ⚠ Known red / rotted — THE ONE LIST (post-s302)
- ~~12 of 47 RS `check_*_integrity.py` gates FAIL~~ — **THE 12 WAS WRONG AND
  S-10 IS CLOSED** (states lane, 2026-08-27). True pre-existing count **7**,
  10 of 12 resolved, 10 gates pinned in `tests/test_integrity_gates.py`;
  RS suite **254 passed / 0 failed** (re-confirmed by the states lane's
  2026-08-30 run on this machine). ⚠ S-10a survives (`R-1040X-SUPERSED`
  has no authority link). S-10c (`D_8582_PTP`'s unwritten recompute) also
  stands. **S-10b diagnosed (states lane, 2026-08-28, `cdd524f`): all 15
  schedule_a gate failures are GATE defects, none spec — never read that
  gate as authority for engine work.**
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s302; s316 touched no client code).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): five phantom failures. New files/markdown/scratchpad only.
- 🌐 ⚠ **The teardown warning "database test_postgres is being accessed by
  other users / 1 other session" is NOT lock contention on this setup** —
  it is YOUR OWN pooled connection (Supavisor; re-confirmed by the states
  lane 2026-08-30). Zero effect on results.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c` (⚠ re-broken ONCE s316, caught on the
  error, script-file rerun); a script run by absolute path needs
  `sys.path.insert(0, server)` (s298). ⚠ `python -m taxwise1040` does
  NOT resolve from `server\` — run `__main__.py` BY PATH (s297).
- 🔧 ⚠ **`BaseModelSerializer` partial updates save ONLY the patched
  columns** (s298) — a serializer-side derived write must be injected into
  `validated_data`, never `setattr` on the instance.
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ Embedded double-quote in a here-string
  arg to a NATIVE exe SPLITS the argument (s315 — ALWAYS `-F` files for
  commit messages; s316 used `-F` throughout, clean). ⚠ `$1` in an
  unquoted PS arg EXPANDS to empty. ⚠ PS `Sort-Object -Unique` on a
  one-element pipeline UNWRAPS to scalar (s296). ⚠ `[IO.File]` calls
  resolve against the PROCESS cwd (s310) — absolute paths.
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash. ⚠ A
  COMMITTED return refuses plain stage+dryrun with HTTP 409 (s298 —
  verification route = the in-place rolled-back recompute, s289 pattern;
  s316 used BOTH probe shapes: staged-rollback for fresh shells, in-place
  for the committed witness). ⚠ An EXCLUDED staged return 409s BEFORE the
  merge parameter is read (s315) — the designed stale-batch kill switch;
  per-return `/exclude`, preparer-run.
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
  ⚠ s316: dot LEADERS pad label rows rightward INTO value regions —
  skip pure-dot words, never raise on them; and a value prints on the
  LAST row of its label block, up to ~6pt ABOVE its own line marker
  (the f7206 window convention).
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292–s316 ran probes this way). ⚠ Scripts touching
  client-named returns live in SCRATCHPAD or tax-test-data, never the repo
  (PII). ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295–s316:
  the s316 units landed EXACTLY at their s315 depth-probe prices — the
  probe-first discipline is what made five units in one session safe).
- 🔧 ⭐ **DRY-RUN THE CORRECTION PASS AND READ EVERY ROW before --commit**
  (s298). ⭐ **CENSUS THE PROD BLAST RADIUS before changing a computed
  line's source** (s302; s316d censused all 23 wave-through carriers
  BEFORE the 13b gate landed — 21 predicted to pass, 21 passed).
- 🌐 ⭐⭐ **THE THIRD QUESTION (s302): "does anything ACT on it?"** — and
  s315's converse: grep for the page section nobody parses (s316 cashed
  it twice: the Three-Year Summary and the NOL worksheet were both
  printed-on-every/many-packets and read by nothing).
- 🔧 ⚠⚠ **A FIXTURE THAT KEYS A COMPUTED COLUMN MEASURES NOTHING** (s302d):
  key the INPUT, and assert the intermediate is nonzero first.
- 🔧 ⚠⚠ **A NEW BOOLEAN FIELD RE-SCOPES EVERY COUNT THAT PREDATES IT**
  (s311). When a field's contract says "every X", grep for every X.
- 🔧 ⚠ **The answer key is a CONTRACT** (s313/s314): new face reads that
  only feed gates go in as INPUT MATERIAL, never into `expected` (s316
  followed it: prior-year figures, SEHI premiums and NOL vintages all
  entered as inputs; zero `expected` keys changed all session). ⚠ **A TIE
  CANNOT SEE NON-RECONCILED FIELDS** (s315): identity fields need their
  own verifier (DEFERRAL item 7, still open).
- 🔧 ⭐ **MIRROR THE ENGINE'S DERIVATION, never re-derive law, in
  emit-side gates** (s312→s316: `derived_senior_deduction` mirrors
  compute_sch_1a; the f7206 line-5 check mirrors `_line5_base`; the
  three-year TLAC check pins to face 24 because that's what l4 reads).

## 🔎 Carried for triage — NOT claims
- (s313) the four 25c/8959 witnesses — birth years extract (s315) but
  each carrier holds other walls; re-verify the cheapest before building.
- (s313) **Client 4059's D_W2G_PAYER_ID error** — committed and tying;
  the W-2G payer address is ABSENT from the packet (document ask).
- (s313) The entry lane's `verify_expected.py` reader misses the 1040
  inner band (line 38) and reads the extractor's explicit-zero GA
  convention as MISSING — their tool, noted in the annex.
- (s312) **A $56 unextracted Schedule 3 line-1-3/6d/6l credit on one
  f5695 packet** — the 5695 line-31 CLW names it.
- (s311) **The more-than-four-dependents box** (1 packet) — refuses by
  name; no route until a source exists.
- (s310→s312c) **The Schedule D identity break is DECOMPOSED** — vendor
  self-inconsistency at line 14; waiting on Ken's 2024 LT-carryover
  number (morning list).
- (s309→s316) **The GA RIE no_ties: FOUR witnesses, now ALL decomposed
  to kind** (REVIEW_QUEUE: §111 refund ×2, unemployment ×1, plus the
  s309 original) — one ruling clears three GA faces. · The two
  fully-phased-out student-loan-interest packets emit when their other
  classes clear.
- (s316) **Two 13b-residual packets refuse by name** (+180, +3,730 —
  tips/OT/car-loan components); a Schedule 1-A component-worksheet leg
  would decompose them if the class grows.
- (s316) **The third f7206 solo** falls to an allocation-worksheet
  identity break (anchored rows sum 130,193 vs printed 127,952) —
  undecomposed, the s301 deeper-wall mechanism.
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
  unidentified, parser-ignored.
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
refund + unemployment → R-GA500-RIE fact list (s309/s315/s316 — the
tally is now kind-complete, one ruling). States lane s316: AL
passthrough reseed + AL_FORM_40 line-26/Schedule CP amendment are DONE
in RS prod (their verified report); the delvio-tax app leg is QUEUED on
Ken's direct word, with their discriminating scenario (L27=3500/L35=500
refund, flipping to 1500 owed if the CP term drops) as the acceptance
vector and their stated line-24 amended-payments gap noted.

---
**s316 deploy close-out:** four code pushes this session —
`1597f8f9`+`f5cdb6de` → `dep-daa3ukuq1p3s738u534g` LIVE ·
`b32e5632`+`181fdd6d` → superseded in-pipeline · `40a47552`+`f168ba88`
→ `dep-daa48g8ae00c73b3cncg` LIVE · head `2fdb2ec8` (s316e) →
`dep-daa4c6uk1f9s73cqj1m0` building at close — **verify at next boot**
(expect a clean scripts-only build; zero server runtime changes all
session). The docs-only close commit(s) after this line trigger one
more deploy — verify the LAST one.
