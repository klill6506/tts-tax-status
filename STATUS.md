# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s319 (the Form 2441 extractor leg), 2026-08-30 late night

**State: idle and CLEAN. The f2441 leg shipped (`c638e8d1`, extractor +
tests only, zero runtime change; deploy verified per the close-out line
at the bottom). r46 = 43/210/253 (+2 emits, zero drift on the prior
41). One new FULL TIE on the rolled-back prod probe joins the queue —
candidates pending Ken's blessing now number THIRTEEN. The other new
emit no_ties on the §6654 penalty ONLY, decomposed to one mechanism
verified to the dollar both ways (below). Item 64's price GREW: 10
packets now refuse by its name, SEVEN single-wall.**

**The s319 unit in brief:**
- The r44 re-census (identical to r43: 41/212/253, zero corpus drift)
  ranked f2441 the top UNBLOCKED solo class (7 solo / 11 touched;
  asset_detail's 10 stay behind Ken's trigger). Depth probe: all seven
  witnesses print the full input surface; the consumption route was
  ALREADY COMPLETE server-side (compute_2441 + care_providers + the
  f2441_* override facts — the staging warnings bless exactly the emit
  shapes the leg uses). Parser mirrors the engine line-for-line
  (CDCC bracket, caps, the Part III §129 chain); engine boundaries
  refuse by name (9b, Part III 13/14/22, sub-statutory plan cap,
  household-employee Yes, checkbox A/B, deeming, off-grid or 13+ or
  zero-expense qualifying persons, earned-income divergence with no
  printed SE source). Schedule 3 line 2 became a cross-check; the
  1040 line-1z decomposition now accepts the Part III line-26
  taxable-DCB component.
- **Tie probe (rolled back, batch key `s319-tie-probe-rb`, output
  `tmp\tie_probe_s319.txt`): one FULL TIE incl. the Part III/DCB
  shape** (taxable benefits → 1e, gross line 16 keyed, refund exact;
  the earned-income override fired as the blessed SE shape) — **queue
  candidate #13**. The other emit no_ties by exactly the penalty:
  TaxWise §6654 penalty 47 = the 100% prior-year safe harbor to the
  dollar, where §6654(d)(1)(C) requires **110% above $150k prior AGI**
  (the packet's own Three-Year Summary prints 224,913) — at 110% the
  90%-of-current limb binds and the engine's 93 is statutory. Both
  endpoints hand-verified exactly. **The filed return understates the
  penalty by $46.** Recorded, not forced (s309).
- The five other "solos" hit walls behind (the upper-bound lesson,
  measured again): one routes to the agent lane (shell already
  carries 2 doc rows — the -Merge case), one is now SINGLE-wall on
  item 64, the rest carry payer-less int/div, an ownerless 8z, a
  Sch-C decomposition gap, a 5329 tax — all named.
- PII: the write-time reflex fired LATE again — real dependent SSNs,
  provider EINs, and surname-bearing helper names reached the test
  fixture draft; caught and scrubbed at the pre-commit sweep (fifth
  session running — the sweep is load-bearing, run it on EVERY draft).

**Corpus: r46 = 43/210/253** (extractor tests 373 green).

**▶ NEXT:** re-census the residual after Ken's rulings; nothing priced
is open — the remaining unlocks are deferred-with-triggers
(asset_detail 10 solos — Ken's trigger; f8959; 1116 multi-category —
Ken's go) or Ken decisions (item 64 converts SEVEN single-wall packets
now). Next depth-probe candidates from r46: f8863 (3/6) · f8606 (3/14)
· other_income_wks (3/4). The GA RIE L10 spec-gap item stays STAGED
(REVIEW_QUEUE s318). The entry lane's Lacerte book and the 50-of-
John's pilot remain the standing big arcs.

**⛔ WAITING ON KEN:** ① bless the THIRTEEN tie-verified queue
candidates (s317: clients 3393 · 3425 · 3430 · 3615 · 3689 · 4177 ·
4371 · 4636; s318: 1945 · 2162 · 2861 · 4583; s319: the new Part III
witness — BATCH-296 annexes have names + batch ids) · ② item 64
(joint overtime owner attribution; now 10 packets, 7 single-wall) ·
③ NEW: the $46 §6654 safe-harbor divergence on the s319 no_tie
witness (TaxWise 100% vs the statutory 110% above $150k prior AGI —
does TaxWise's 2210 worksheet carry a blank prior-AGI field?) ·
④ the GA RIE L10 spec/engine set-size item (REVIEW_QUEUE s318) ·
⑤ client 4081's $169 RIE-interest vendor divergence · carried: the RS
R-GA500-RIE amendment (states lane seeded; Ken's re-check) · client
4059's W-2G payer street address · the one-number Schedule D carryover
question (32,002 vs 29,963) · reprints · standing decisions 1–8 · the
2a ruling-scope flag · the AL Form 40 line-26/Schedule CP app leg.

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
LIVE → seed → verify — and the deploy ITSELF seeds.** ⚠⚠ **A runtime
change must be deploy-LIVE before lane commits ride it.**
- **s319 deploys: see the close-out line at the bottom of this file.**
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (s319 changed no vocabulary — care_providers, the dependents care
  fields and every f2441_* fact all pre-existed in the allowlists.)

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — anything load-bearing goes in batch-file annexes too.
**Never relay tokens through the message channel.** ONE delvio-tax tree
holder; ONE pytest/test_postgres holder. Peers stage; Ken decides.
⚠ **A SECOND delvio-tax session existed 2026-08-30 (`delvio-tax-1f`)** —
if a future boot finds uncommitted tree changes it didn't make, THAT
session is the suspect — coordinate before touching anything. (s319 boot
found the tree clean.)

## ⚠ Known red / rotted — THE ONE LIST (post-s302)
- **S-10a** (`R-1040X-SUPERSED` no authority link) · **S-10c**
  (`D_8582_PTP` unwritten recompute) survive; RS suite 254/0
  (states lane, 2026-08-30 run). **S-10b: all 15 schedule_a gate
  failures are GATE defects, none spec.**
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s302; s317–s319 touched no client code).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286). New files/markdown/scratchpad only.
- 🌐 ⚠ **The test_postgres teardown "1 other session" warning is your OWN
  pooled connection** (Supavisor; re-confirmed 2026-08-30).
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ inline `-c` BANNED — script FILES (re-broken s319 once,
  caught at the parse error). ⚠ scripts by absolute path need
  `sys.path.insert(0, server)` (s298). ⚠ `python -m taxwise1040` doesn't
  resolve from `server\` — run `__main__.py` BY PATH (s297).
- 🌐 ⚠⚠ **`close_old_connections()` INSIDE a `.iterator()` loop kills the
  server-side cursor's own connection** (s316f census) — materialize the
  ID list FIRST, then per-item close+get.
- 🔧 ⚠ **`BaseModelSerializer` partial updates save ONLY the patched
  columns** (s298).
- 🌐 ⚠⚠ PS5.1 traps: regex-replace file rewrites BANNED; Edit tool or
  `[IO.File]` BOM-less UTF8. ⚠ git commit messages ALWAYS `-F` files
  (s315). ⚠ `$1` in an unquoted PS arg EXPANDS empty. ⚠ `Sort-Object
  -Unique` on one element UNWRAPS (s296). ⚠ `[IO.File]` resolves against
  PROCESS cwd (s310). ⚠⚠ `Measure-Object -Line` skips blanks.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for invalid payloads — the verdict is
  `row["status"]`; CRUD routes carry the trailing slash. ⚠ A COMMITTED
  return refuses stage+dryrun with 409 (in-place rolled-back recompute is
  the verification route, s289). ⚠ An EXCLUDED staged return 409s BEFORE
  merge is read (s315).
- 🌐 ⚠ **`str(Decimal)` preserves the STORED SCALE** (s306).
- 🌐 ⚠ pymupdf/TaxWise geometry: parse POSITIONALLY; values recognised by
  RIGHT edge; marker/caption tokens can END inside value windows
  (s307–s311); dot LEADERS invade value regions — skip, never raise; an
  f7206-style value prints up to 6pt ABOVE its own marker; a checkbox X
  sits LEFT of its own label (s316g/s319 — the 2441 provider X sits left
  of its "No"); TaxWise RE-TYPESETS schedules (s297) and can OMIT a page
  it bills on the invoice (s297/s316/s317). ⚠ s317/s319: IRS-face
  left-gutter anchors + right-edge value columns transfer cleanly to new
  forms — but caption text plants line-number-shaped words inside naive
  bands (s319: the 2441 Part III line-25 caption prints a bare "20" at
  x0 360 next to the 347.3 inner-tag column — band ceilings are
  load-bearing).
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase** (s288).
- 🔧 ⭐ **Rolled-back dry-runs reproduce production locally** (s289;
  s292–s319). ⚠ Client-named scripts in SCRATCHPAD/tax-test-data only.
  ⚠ `Firm.objects.get(name="The Tax Shelter")`, never `.first()`.
  ⚠ Pooler timeouts kill the connection — ping + retry.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295–s319;
  s319: 7 solos → 2 emits + 1 agent-lane route + 4 deeper-walled).
- 🔧 ⭐ **DRY-RUN a correction pass and READ EVERY ROW** (s298). ⭐
  **CENSUS THE BLAST RADIUS before changing a computed line's source**
  (s302/s316f).
- 🌐 ⭐⭐ **THE THIRD QUESTION + ITS CONVERSE** (s302/s315): "does anything
  ACT on it?" / "which printed page (or face fact) does NOBODY read?"
- 🌐 ⚠⚠ **READ WHAT THE ERROR SAYS before pattern-matching it to a known
  class** (s316g; the s281 evidence rule).
- 🔧 ⚠⚠ **A FIXTURE THAT KEYS A COMPUTED COLUMN MEASURES NOTHING** (s302d).
- 🔧 ⚠⚠ **A NEW BOOLEAN FIELD RE-SCOPES EVERY COUNT THAT PREDATES IT** (s311).
- 🔧 ⚠ **The answer key is a CONTRACT** (s313/s314); **A TIE CANNOT SEE
  NON-RECONCILED FIELDS** (s315; identity-verifier still open,
  DEFERRAL item 7).
- 🔧 ⭐ **MIRROR THE ENGINE, and make shared derives ONE HELPER both the
  feed and the diagnostic read** (s312→s319: mirror_f2441 re-derives
  compute_2441's chains from printed inputs; divergence refuses).
- 🔧 ⚠⚠ **THE SURNAME REFLEX MUST FIRE AT WRITE TIME** (s307–s319 —
  FIVE sessions running; s319's draft fixture carried real dependent
  SSNs, provider EINs, and surname-bearing HELPER NAMES. Grep EVERY
  draft — including function names — for the working set's identifiers
  before commit; SSNs, EINs and VINs all count).

## 🔎 Carried for triage — NOT claims
- **(s319) The $46 §6654 safe-harbor divergence** — TaxWise penalty 47
  (100% prior-year harbor exact) vs engine 93 (110% above $150k prior
  AGI per §6654(d)(1)(C); the 90%-current limb then binds). Filed
  return understates by $46; vendor-divergence deliverable for Ken.
- **(s319) The agent-lane -Merge witness** — an f2441 emit whose shell
  already carries 2 document rows; routed, not lost.
- **(s319) The Sch-C decomposition gap witness** — Schedule 1 line 3
  prints 12,223 where the extracted Schedule C nets sum 2,760; a
  second business may not be in the extract. Undiagnosed.
- **(s318) Client 4081's $169 RIE-L6 interest divergence** — TaxWise
  251 vs engine 420, GA-taxed either way, outcomes tie.
- **(s318) The FTC-205-against-tax-0 witness** — refuses by name at
  the face-16 limitation gate.
- **(s318) One packet prints 1040 line 26 with NO est-payments
  worksheet page** (the omitted-page shape; named blanket refusal).
- **(s317) TWO packets wait only on the GA LIC keyed gate** (client
  3815 + the s316 witness): entry side keys `LIC-NODEP` true
  (+`LIC-CHILD 0`) and both should TIE.
- **(s317→s319) Item-64 joint-OT class: now 10 packets, 7 single-wall**
  — a ruling converts seven immediately; the emit arm is small.
- **(s317) The multi-W-2 tips witness** (2 same-owner W-2s, 4c=770) —
  the s306t deferral trigger shape arrived; needs i1040 + RS
  amendment, flagged.
- (s316f) Client 1382 (filed, manual entry): preparer-keyed RIE-TP-10
  override holds the whole joint refund — clearing it is on Ken's queue.
- (s316) The third f7206 solo: allocation-worksheet identity break
  (rows sum 130,193 vs printed 127,952) — undecomposed.
- (s313) the four 25c/8959 witnesses — every carrier holds other walls.
- (s313) Client 4059's W-2G payer street address — the LAST Inbox hold
  of the filed set (document ask).
- (s313) The entry lane's `verify_expected.py` reader gaps (their tool).
- (s312) The $56 unextracted Schedule 3 credit on one f5695 packet.
- (s311) The more-than-four-dependents box (1 packet).
- (s310→s312c) The Schedule D identity break — awaiting Ken's 2024
  LT-carryover number.
- (s309) The two fully-phased-out student-loan-interest packets emit
  when their other classes clear. · The unattributed-joint-capital-gain
  RIE question (client 3825) is STILL OPEN.
- (s303) Home-office 300 sq ft cap: prod census ZERO. · §4.08(4)
  monthly averaging unrepresentable.
- (s302d) D_EFILE_001 "not keyed vs not obtainable" (the s298 21-row
  class). · (s302) `D_8582_PTP` unverified (S-10c) ·
  `div_1099s.us_government_income` off-switch pending · D_SCHD_006 QOF
  no import surface.
- (s301) One packet's Sch D 1b-grid/h-identity break. · (s298) 21
  named-but-blank W-2/1099-R rows. · (s297) the X mark at (474.7,
  y≈389) on one 1040 p2 EIC row.
- (s296) 22 sch_d geometry packets refuse by design · (s295) 7
  auxiliary PDFs refuse as non-packets · `_summary_lines`
  GA500_SUMMARY_LINES lacks S1-6.
- (s290) GA RIE interest row excludes K-1 16A tax-exempt interest —
  stated boundary. · 8995 TIN prints unformatted — cosmetic.
- (s289) `IndividualForm7203` carryover keying gaps · K-1 gains vs L9
  weights (+ the #78 aggregate convention).
- (s288) 7203 box 16E / 1065 18a-c gaps · `ctc_override`/`odc_override`
  DEAD — removal candidate (Ken).
- (s287) 8825 line-1 repaint scope · suggested-field convention note
  stale. · (s285) Sch 4 nonresident arm · (s283) stamp excludes 1040
  packets (Ken) · (s282) `OVERRIDE_HONORED_STATE_LINES` hand-synced ·
  (s281) OOS-state line-18 diagnostic specified not built · (s268)
  1,604 queries/run · (s241/s281) `Form8606` unique-constraint ·
  🔴 `HSAAccount` half CLOSED · (s275/s281) `.first()` remainder ·
  (s294) omitting-correction state-face staleness.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried:
S-15 / S-16 / S-19 stand (see STATUS_ARCHIVE). The R-GA500-RIE L10
amendment is WITH THE STATES LANE pending Ken's direct word. Also
staged there: whether D_GA500_019 widens beyond patronage. AL Form 40
line-26/Schedule CP app leg queued on Ken's direct word. The s306t
multi-employer-tips 4c method will need an RS amendment when Ken opens
it (trigger shape arrived; see DEFERRAL_AUDIT).

---
**s319 deploy close-out:** `c638e8d1` (the f2441 leg — extractor +
tests only, zero server/runtime change) deploy `dep-daadfimk1f9s73d29kcg`
**API-confirmed LIVE**. The docs-only close commits after this line
trigger one more deploy — verify the LAST one at next boot.
