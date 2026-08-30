# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s317 (the Schedule 1-A leg), 2026-08-30

**State: idle and CLEAN. The r40→r41 re-census unit is BUILT, probed,
committed and pushed (`3f67f3bb`; extractor+tests only — zero runtime
change). The Schedule 1-A page (role=ignore since s127, the s315
converse's FIFTH instance) is now an extract source: r41 = 36/217/253,
+9 emits at EXACTLY the depth-probe price, and 8 of the 9 dry-ran full
TIE on prod rolled back.** Deploy for `3f67f3bb` verified per the
close-out line.

**The s317 unit in brief:**
- r40 (27/226/253) re-census: the top single-wall class was 14 packets
  refusing for "tips/overtime/car-loan inputs outside the extract" —
  while every one PRINTS the two-page Schedule 1-A carrying those
  inputs. Class probe over all 16 candidates: OT 14 · tips 2 · car
  loan 1 · senior 2; all six joint packets have W-2s on BOTH owners.
- Built `scripts/taxwise1040/sch_1a.py` (positional parse + engine
  mirror of compute_sch_1a's four chains, RS-specced constants
  verbatim), classify role flip, emit wiring, 17 unit tests (327
  extractor tests green). Emits are INPUTS only: single-filer 14a →
  `qualified_overtime_w2_amount`; single-W-2 tips → box 7 + the
  attestation; Part IV rows → `car_loan_vehicles`. Joint 14a refuses
  by name (item 64); multi-W-2 tips refuses by name.
- r41 drift FULLY accounted: old 13b classes (18+3) cleared; new = 7
  item-64 joint-OT + 1 multi-W-2 tips + 2 omitted-page fallbacks + 9
  emits; every other class +0.
- Rolled-back prod tie probe (batch `02b06114`, nothing written):
  **8/9 full TIE**. The ninth no_ties by ±$5 on GA 23/30/46 = the GA
  Low Income Credit keyed gate (D_GA500_022; the s316 class) — TWO
  packets now wait only on the LIC-NODEP assertion from the entry
  side.
- Annex posted to BATCH-296 (2026-08-30 evening). ⚠⚠ PII near-miss
  caught pre-commit AGAIN: witness surnames + a real VIN were in the
  parser docstring/test file at write time — scrubbed to shape-named
  witnesses + a synthetic VIN before commit; repo git-grep clean.

**Corpus: r41 = 36/217/253** (extractor tests 327 green).

**▶ NEXT:** nothing priced is open at single-wall scale below the
deferred classes — the remaining big single-wall unlocks are
asset_detail (10 solos, DEFERRED on Ken's trigger) and est_payments_wks
(7 solos, unpriced — next depth-probe candidate). Deferred-with-
triggers otherwise unchanged (f8959 / 1116 multi-category — Ken's go;
⚠ the s306t multi-employer-tips trigger SHAPE has now ARRIVED — see
DEFERRAL_AUDIT 2026-08-30 note — decision needs i1040 + RS amendment).
The entry lane's Lacerte book and the 50-of-John's pilot remain the
standing big arcs.

**⛔ WAITING ON KEN (adds two items):** ① bless the EIGHT tie-verified
queue candidates (clients 3393 · 3425 · 3430 · 3615 · 3689 · 4177 ·
4371 · 4636 — the BATCH-296 annex has the names + batch id) · ② item 64
(joint overtime owner attribution) now holds SEVEN packets refusing by
name — owner-agnostic aggregate vs 50/50 ruling; for TY2025 nothing
owner-sensitive consumes the split (GA HB 463 pulls are TY2026+) ·
carried: the RS R-GA500-RIE amendment (his DIRECT word to the states
lane) · client 4059's W-2G payer street address · the one-number
Schedule D carryover question (32,002 vs 29,963) · reprints · standing
decisions 1–8 · the 2a ruling-scope flag · the AL Form 40 line-26/
Schedule CP app leg.

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
- **s317 deploys: see the close-out line at the bottom of this file.**
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (s317 changed no vocabulary — the OT/tips fields, box 7 and
  `car_loan_vehicles` all pre-existed in the allowlists.)

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
session is the suspect — coordinate before touching anything. (s317 boot
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
- **Client typecheck**: green (s302; s317 touched no client code).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286). New files/markdown/scratchpad only.
- 🌐 ⚠ **The test_postgres teardown "1 other session" warning is your OWN
  pooled connection** (Supavisor; re-confirmed 2026-08-30).
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ inline `-c` BANNED — script FILES (re-broken s317 once,
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
  sits LEFT of its own label (s316g); TaxWise RE-TYPESETS schedules
  (s297) and can OMIT a page it bills on the invoice (s297/s316/s317 —
  two packets print 13b with NO Schedule 1-A page). ⚠ s317: an IRS-form
  face's left-gutter anchors + right-edge value columns (the f8995
  idiom) transfer cleanly to new forms — but caption text can print
  line-number-shaped words INSIDE the gutter x-range ("4b"@x0=59), so
  the cutoff is load-bearing.
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase** (s288).
- 🔧 ⭐ **Rolled-back dry-runs reproduce production locally** (s289;
  s292–s317). ⚠ Client-named scripts in SCRATCHPAD/tax-test-data only.
  ⚠ `Firm.objects.get(name="The Tax Shelter")`, never `.first()`.
  ⚠ Pooler timeouts kill the connection — ping + retry.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295–s317;
  s317 landed exactly at its depth-probe price: 9 predicted, 9 emitted).
- 🔧 ⭐ **DRY-RUN a correction pass and READ EVERY ROW** (s298). ⭐
  **CENSUS THE BLAST RADIUS before changing a computed line's source**
  (s302/s316f).
- 🌐 ⭐⭐ **THE THIRD QUESTION + ITS CONVERSE** (s302/s315): "does anything
  ACT on it?" / "which printed page (or face fact) does NOBODY read?" —
  s317 cashed the converse a FIFTH time (the Schedule 1-A pair,
  role=ignore while 14 packets refused for its own contents).
- 🌐 ⚠⚠ **READ WHAT THE ERROR SAYS before pattern-matching it to a known
  class** (s316g; the s281 evidence rule).
- 🔧 ⚠⚠ **A FIXTURE THAT KEYS A COMPUTED COLUMN MEASURES NOTHING** (s302d).
- 🔧 ⚠⚠ **A NEW BOOLEAN FIELD RE-SCOPES EVERY COUNT THAT PREDATES IT** (s311).
- 🔧 ⚠ **The answer key is a CONTRACT** (s313/s314); **A TIE CANNOT SEE
  NON-RECONCILED FIELDS** (s315; identity-verifier still open,
  DEFERRAL item 7).
- 🔧 ⭐ **MIRROR THE ENGINE, and make shared derives ONE HELPER both the
  feed and the diagnostic read** (s312→s317: mirror_sch_1a re-derives
  compute_sch_1a's chains from printed inputs; divergence refuses).
- 🔧 ⚠⚠ **THE SURNAME REFLEX MUST FIRE AT WRITE TIME** (s307/s316/s317 —
  three sessions running, surnames + a real VIN reached draft repo files
  and were caught only at the pre-commit sweep. Grep EVERY draft for the
  working set's surnames before commit; VINs count as identifying).

## 🔎 Carried for triage — NOT claims
- **(s317) TWO packets wait only on the GA LIC keyed gate** (the s316
  witness + the new r41 witness, client 3815): entry side keys
  `LIC-NODEP` true (+`LIC-CHILD 0`) and both should TIE.
- **(s317) Item-64 joint-OT class: 7 packets refuse by name** (6
  single-wall; the s316 senior-residual pair now decompose to exactly
  their OT residuals 180 / 3,730). Emit arm is small once Ken rules.
- **(s317) The multi-W-2 tips witness** (2 same-owner W-2s, 4c=770) —
  ALSO the s306t deferral trigger shape arrived; needs i1040 + RS
  amendment, flagged.
- (s316f) Client 1382 (filed, manual entry): preparer-keyed RIE-TP-10
  override holds the whole joint refund — would double-count 513 on any
  future recompute; fix = clear the override (Ken's queue).
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
amendment is WITH THE STATES LANE pending Ken's direct word (their
staged write-up: statute-primary O.C.G.A. §48-7-27(a)(5)(E)(i)). Also
staged there: whether D_GA500_019 widens beyond patronage. AL Form 40
line-26/Schedule CP app leg queued on Ken's direct word. **NEW (s317
flag): the s306t multi-employer-tips 4c method will need an RS
amendment when Ken opens it** (trigger shape arrived; see
DEFERRAL_AUDIT).

---
**s317 deploy close-out:** `3f67f3bb` (extractor + tests + deferral
note ONLY — zero server/runtime change) → the auto-deploy
**build_failed on a corrupted Render venv cache** (pip metadata crash
uninstalling charset-normalizer; same lockfile built green hours
earlier — the commit was NOT the cause; prod kept serving `0b5da5b7`
throughout) → manual `{"clearCache":"clear"}` redeploy
`dep-daaau2ks728c73fskll0` **API-confirmed LIVE**. ⚠ New hazard for
the standing list: a build_failed on a docs/scripts-only commit =
suspect the CACHE first; read the build log before the commit. The
docs-only close commits after this line trigger one more deploy —
verify the LAST one at next boot.
