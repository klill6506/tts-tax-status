# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s316 close part 2 (Ken's four rulings executed), 2026-08-30

**State: idle and CLEAN. Ken ruled four ways this morning ("1. mark
filed 2. blessed 3. clear all 3 4. go to work") and ALL FOUR ARE
EXECUTED: FIFTEEN returns are now FILED, FOURTEEN packets are CLOSED
to Done, and the GA RIE ruling is built, deployed, and both §111
witnesses are committed+filed under it.** Head `be03f6c6`; deploys per
the close-out line.

**Ruling execution record:**
- **① Mark-filed:** the nine committed returns swept filed via the
  real Leg-D endpoint (7 twq9 + 2 twq10; the two stale twq9 DOB copies
  EXCLUDED per the designed per-return action). Fresh auth minted
  (dev magic link, one-process rule).
- **② The four blessed:** staged as `twq11-20260830`, dry-run all
  four TIE on the live server, committed, marked filed.
- **③ The GA RIE ruling ("clear all 3") — s316f, `3e459267`:** the
  §111 taxable state refund + unemployment JOIN the RIE L10 derive.
  **Authority re-pointed STATUTE-PRIMARY same day** (O.C.G.A.
  §48-7-27(a)(5)(E)(i) "shall include but not be limited to" — the
  states lane's verbatim pull found the reg's tail invites ejusdem
  generis AND the same reg sentence prints a stale $4,000 earned cap
  vs the statutory $5,000; reg demoted to secondary, its (3) used only
  as the amount filter). ONE shared helper
  (`compute_ga500.rie_l10_other_income_by_owner`) feeds both the views
  pull and D_GA500_017's mirror — which had ALREADY drifted (the MISC
  box-3 component never joined the old PATR-only mirror; closed).
  Blast-radius census (rolled-back recompute over all 12 candidates):
  2 filed manual-entry returns move — client 2538 printed-exclusion
  only (GA tax already 0), client 1382 a REAL triage finding (a
  preparer-keyed RIE-TP-10 override holding the whole joint refund
  would DOUBLE-COUNT the spouse half on any future recompute; filed
  returns don't auto-recompute — carried below). Both §111 witnesses
  (clients 4025, 1828) then dry-ran TIE on the deployed build →
  committed as `twq12-20260830` → filed. The unemployment witness
  (3754) stays extract-refused on his separate 13b residual; his arm
  is unit-tested. D_GA500_019 stays patronage-scoped (flagged, not
  silently widened). RS R-GA500-RIE amendment: relayed to the states
  lane, who correctly held their gate for Ken's DIRECT word — it's on
  his queue with their staged write-up (`ga_rie_line10_ruling_staged`).
- **④ "Go to work" — s316g, `be03f6c6`:** the closeout gate held ALL
  the filed returns on ONE cause: **the digital-asset question,
  printed on every packet's 1040 face and read by nothing** (the s315
  converse, third instance this session). ⚠ My first read
  pattern-matched D_EFILE_001 to the s298 EIN class — the error's own
  message said otherwise (the s281 rule). `parse_p1_digital_assets`
  ships (X left of its Yes/No label, both-marked/out-of-window raise,
  unmarked stays None); r39 = 41/226/267 with drift EXACTLY the one
  new key on all 41 emits (whole corpus answers No); the s298-style
  correction pass set all 15 filed returns' fields from their OWN
  faces (every row printed and read first). Cleanup then cleared 14
  of 15 (warnings acknowledged source-verified per the tie
  verification + Ken's authorization) → **14 packets moved
  Inbox→Done**. The one hold: client 4059's W-2G payer street address
  — the standing document ask, exactly as designed.

**Corpus: r39 = 41/226/267** (extractor tests 310 green). ⚠ The 14
closed packets leave the Inbox, so r40+ scans ~253 and emit counts
drop by 14 — expected, not a regression.

**▶ NEXT:** re-census the r39 residual classes (nothing priced is
open); deferred-with-triggers unchanged (f8959 / asset_detail / 1116
multi-category — Ken's go). The entry lane's Lacerte book and the
50-of-John's pilot remain the standing big arcs.

**⛔ WAITING ON KEN (the list SHRANK again):** the RS R-GA500-RIE
amendment needs his DIRECT word to the states lane (my relay
correctly refused at their gate; their write-up is staged) · client
4059's W-2G payer street address (the last Inbox hold of the filed
set) · the one-number Schedule D carryover question (32,002 vs
29,963) · reprints · standing decisions 1–8 · the 2a ruling-scope
flag · the AL Form 40 line-26/Schedule CP app leg (his direct word;
acceptance vector recorded).

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
change must be deploy-LIVE before lane commits ride it** (s316f practiced:
the RIE fix deployed and API-verified BEFORE twq12 staged).
- **s316 deploys: see the close-out line at the bottom of this file.**
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (s316f/g changed no vocabulary — digital_assets_answer and the RIE
  fields all pre-existed in the allowlists.)

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — anything load-bearing goes in batch-file annexes too.
**Never relay tokens through the message channel.** ONE delvio-tax tree
holder; ONE pytest/test_postgres holder. Peers stage; Ken decides.
⚠ **A SECOND delvio-tax session existed 2026-08-30 (`delvio-tax-1f`)** —
the states lane flagged it; this session asserted the one-writer
protocol to it directly and heard nothing back before close. If a future
boot finds uncommitted tree changes it didn't make, THAT session is the
suspect — coordinate before touching anything.
⚠ **A peer's authority-upgrade can be adoptable even when their gate
holds** (s316f: their statute-primary re-point was doc-only and
verifiable — adopted; their RS seed correctly waits for Ken).

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
- **Client typecheck**: green (s302; s316 touched no client code).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286). New files/markdown/scratchpad only.
- 🌐 ⚠ **The test_postgres teardown "1 other session" warning is your OWN
  pooled connection** (Supavisor; re-confirmed 2026-08-30).
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ inline `-c` BANNED — script FILES (re-broken s316, caught).
  ⚠ scripts by absolute path need `sys.path.insert(0, server)` (s298).
  ⚠ `python -m taxwise1040` doesn't resolve from `server\` — run
  `__main__.py` BY PATH (s297).
- 🌐 ⚠⚠ **`close_old_connections()` INSIDE a `.iterator()` loop kills the
  server-side cursor's own connection** (s316f census, self-inflicted) —
  materialize the ID list FIRST, then per-item close+get.
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
  sits LEFT of its own label and ~2.7pt ABOVE the label row (s316g);
  TaxWise RE-TYPESETS schedules (s297) and can OMIT a page it bills on
  the invoice (s297/s316 — no federal 2210 page prints, ever).
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase** (s288).
- 🔧 ⭐ **Rolled-back dry-runs reproduce production locally** (s289;
  s292–s316). ⚠ Client-named scripts in SCRATCHPAD/tax-test-data only.
  ⚠ `Firm.objects.get(name="The Tax Shelter")`, never `.first()`.
  ⚠ Pooler timeouts kill the connection — ping + retry.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295–s316;
  the s316 units all landed exactly at their depth-probe prices).
- 🔧 ⭐ **DRY-RUN a correction pass and READ EVERY ROW** (s298; s316g did
  — 15/15 uniform). ⭐ **CENSUS THE BLAST RADIUS before changing a
  computed line's source** (s302; the s316f census caught a filed-return
  double-count shape BEFORE deploy).
- 🌐 ⭐⭐ **THE THIRD QUESTION + ITS CONVERSE** (s302/s315): "does anything
  ACT on it?" / "which printed page (or face fact) does NOBODY read?" —
  s316 cashed the converse THREE times (Three-Year Summary, NOL
  worksheet, the digital-asset checkbox).
- 🌐 ⚠⚠ **READ WHAT THE ERROR SAYS before pattern-matching it to a known
  class** (s316g: D_EFILE_001 was the digital-asset question, not the
  s298 EIN class; the s281 evidence rule).
- 🔧 ⚠⚠ **A FIXTURE THAT KEYS A COMPUTED COLUMN MEASURES NOTHING** (s302d).
- 🔧 ⚠⚠ **A NEW BOOLEAN FIELD RE-SCOPES EVERY COUNT THAT PREDATES IT** (s311).
- 🔧 ⚠ **The answer key is a CONTRACT** (s313/s314); **A TIE CANNOT SEE
  NON-RECONCILED FIELDS** (s315; identity-verifier still open,
  DEFERRAL item 7).
- 🔧 ⭐ **MIRROR THE ENGINE, and make shared derives ONE HELPER both the
  feed and the diagnostic read** (s312→s316f: the 017 mirror had already
  drifted from its feeder when the components were separate functions).

## 🔎 Carried for triage — NOT claims
- **(s316f) Client 1382 (filed, manual entry): a preparer-keyed
  RIE-TP-10 override holds the whole joint refund; the new spouse-half
  derive would DOUBLE-COUNT 513 on any future recompute.** Fix = clear
  the override (preparer/Ken); filed returns don't auto-recompute, so
  nothing moves silently. Client 2538's shape is harmless (exclusion
  grows, GA tax already 0).
- (s316) **Two 13b-residual packets refuse by name** (+180, +3,730);
  a Schedule 1-A component-worksheet leg would decompose them.
- (s316) **The third f7206 solo**: allocation-worksheet identity break
  (rows sum 130,193 vs printed 127,952) — undecomposed.
- (s313) the four 25c/8959 witnesses — every carrier holds other walls.
- (s313) **Client 4059's W-2G payer street address** — the LAST Inbox
  hold of the filed set (D_W2G_PAYER_ID error; document ask).
- (s313) The entry lane's `verify_expected.py` reader gaps (their tool).
- (s312) The $56 unextracted Schedule 3 credit on one f5695 packet.
- (s311) The more-than-four-dependents box (1 packet).
- (s310→s312c) The Schedule D identity break — awaiting Ken's 2024
  LT-carryover number.
- (s309) The two fully-phased-out student-loan-interest packets emit
  when their other classes clear. · The unattributed-joint-capital-gain
  RIE question (client 3825) is STILL OPEN — it was NOT part of the
  2026-08-30 ruling.
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
S-15 / S-16 / S-19 stand (see STATUS_ARCHIVE). **NEW: the R-GA500-RIE
L10 amendment (the 2026-08-30 ruling) is WITH THE STATES LANE pending
Ken's direct word** — their staged write-up recommends citing O.C.G.A.
§48-7-27(a)(5)(E)(i) primary and recording the reg's stale $4,000.
Also staged there: whether D_GA500_019 widens beyond patronage (my
flag). AL Form 40 line-26/Schedule CP app leg queued on Ken's direct
word (acceptance vector recorded).

---
**s316 deploy close-out (part 2):** `3e459267` (s316f, the RIE ruling
— RUNTIME) → `dep-daa6lquq1p3s7390ncf0` **API-confirmed LIVE before
twq12 staged** · `be03f6c6` (s316g + citation re-point) → deploy
verified per the background check at close (expect live; scripts +
docstring only — zero runtime behavior change beyond s316f's). The
docs-only close commits after this line trigger one more deploy —
verify the LAST one at next boot.
