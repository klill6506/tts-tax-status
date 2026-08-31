# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s321 (the IRA pair leg + the other-income leg), 2026-08-31

**State: idle and CLEAN. TWO extractor legs shipped this session
(`4db9ab3a` + parent; deploy status at the close-out line below).
r49 = 46/207/253 (+2 emits, zero drift on the 44 priors both runs).
One new FULL TIE — queue candidates pending Ken now number FIFTEEN.
One new §6654 sub-110%-harbor vendor divergence (witness #2 of the
s319 class, ~$61). One packet parses fully but has NO SEEDED SHELL
(a seeding ask). ONE new runtime surface: the `roth_ira_bases`
backentry section (schema + SUPPORTED-SECTIONS regenerated).**

**The s321 units in brief:**
- **① The IRA pair (f8606 + ira_wks)** — the r47 top solo siblings
  (3/13 + 3/14, overlap 1) opened as one leg. Depth probe: form_8606s
  + compute_8606 complete since mig 0079; the ONE gap was the
  RothIRABasis tracker's missing lane surface (two of three ira_wks
  solos print ONLY a Roth contribution — 7,000 and 350, witnesses
  named in the BATCH-296 annex). Section added (dup-owner error,
  all-zero + override-disagreement warns; 9 tests). f8606 parses per
  OWNER (pages group by printed SSN — multi-page packets are two
  owners' faces, NEVER a p1+p2 pair) with a full compute_8606 mirror;
  ira_wks parses the byte-uniform US8606W1 grid by right-edge column
  recognition. The 4b/5b decomposition now mirrors doc_is_ira_path
  (s239): J/T/Q Roth rows are IRA-path with 8606-determined taxable —
  the gate was blind to it while the coverage gate censored every
  witness (the s306 fourth question).
- **② other_income_wks + the line-36 route** — TWO page shapes under
  one class key: the 1099-MISC report's other-income section →
  misc_1099s rows routed sch1_8z; the USW10407 Describe rows →
  other_income_items 8z rows; the face-level single-8z import is
  SUPPRESSED when the class supplies detail (double-count fence);
  face 8z / line 31 / the report total / the line-1-restates-the-
  report seam are identity gates. Plus 1040 line 36 (the irrevocable
  applied-forward election) now emits on the batch-001 #3
  f1040_fields route — nothing had EVER fed it, and the first witness
  no_tied on line 36 alone before the fix.
- **Tie probes (rolled back, `s321-tie-probe-rb`, outputs
  `tmp\tie_probe_s321{,b}.txt`): one FULL TIE** (applied-forward 494
  + the whole GA retirement-exclusion chain exact — **queue candidate
  #15**); one no_tie decomposed to the §6654 sub-110% harbor
  (engine 160 statutory vs filed 99; prior AGI 153,941 > 150k printed
  on the packet's own Three-Year Summary; TaxWise's 99 does not
  reconstruct from printed facts — a 100% harbor gives ≈95; no 2210
  page prints). **Witness #2 of the s319 vendor-divergence class.**
- The censored-solo lesson at full strength: NINE solos across the
  two classes delivered TWO emits. Fallthroughs: one -Merge (agent
  lane), one alloc-wks identity break, one 2b-no-payer + 25c, one
  GA-S1-12 + an odd recap-vs-face 1000/4000 mismatch, one GA RIE L10
  (joins that Ken item), one Sch 1-A tips wall, one NO-SHELL, two MFJ
  ownerless-row named refusals (one a clergy-housing shape).
- Extractor tests 399 → 439 green; backentry suites 209; flow
  assertions 526. PII: write-time sweep caught a real surname in a
  new parser docstring pre-commit (seventh session the sweep is
  load-bearing); fixtures fully synthetic.

**Corpus: r49 = 46/207/253** (extractor tests 439 green).

**▶ NEXT:** nothing priced is open — remaining unlocks are
deferred-with-triggers (asset_detail 12 solos — Ken's trigger; f8959;
1116 multi-category — Ken's go) or Ken decisions (item 64: 11 packets
/ 8 single-wall). Next depth-probe candidates from r49: k1_detail
(2 solos) and the residual named-refusal classes. The GA RIE L10
spec-gap item stays STAGED (REVIEW_QUEUE s318). The entry lane's
Lacerte book and the 50-of-John's pilot remain the standing big arcs.

**⛔ WAITING ON KEN:** ① bless the FIFTEEN tie-verified queue
candidates (s317: clients 3393 · 3425 · 3430 · 3615 · 3689 · 4177 ·
4371 · 4636; s318: 1945 · 2162 · 2861 · 4583; s319: the Part III/DCB
witness; s320: the education-credit witness; s321: the
applied-forward/GA-RIE witness — BATCH-296 annexes have names + batch
ids) · ② item 64 (joint overtime owner attribution; 11 packets, 8
single-wall) · ③ the §6654 sub-110% safe-harbor vendor divergence —
NOW TWO WITNESSES (s319 $46 · s321 ~$61; both filed returns
understate; TaxWise appears to use the 100% harbor above $150k prior
AGI) · ④ the GA RIE L10 spec/engine set-size item (REVIEW_QUEUE
s318; +1 witness s321) · ⑤ client 4081's $169 RIE-interest vendor
divergence · ⑥ two packets miss printed 8863 Part III student pages
(cheap reprints convert one) · ⑦ NEW: one packet needs a SEEDED SHELL
(the foreign-pension 8z witness — it emits the moment the shell
exists; entry-lane ask in the s321 annex) · carried: the RS
R-GA500-RIE amendment (states lane; Ken's re-check) · client 4059's
W-2G payer street address · the one-number Schedule D carryover
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
- **s321 deploys: see the close-out line at the bottom of this file.**
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (s321 DID add vocabulary — `roth_ira_bases` — and both the schema and
  SUPPORTED-SECTIONS.md were regenerated the same session.)

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
session is the suspect — coordinate before touching anything. (s321 boot
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
- **Client typecheck**: green (s302; s317–s321 touched no client code).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286). New files/markdown/scratchpad only.
- 🌐 ⚠ **The test_postgres teardown "1 other session" warning is your OWN
  pooled connection** (Supavisor; re-confirmed 2026-08-30).
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ inline `-c` BANNED — script FILES. ⚠ scripts by absolute
  path need `sys.path.insert(0, server)` (s298). ⚠ `python -m taxwise1040`
  doesn't resolve from `server\` — run `__main__.py` BY PATH (s297).
- 🌐 ⚠⚠ **`close_old_connections()` INSIDE a `.iterator()` loop kills the
  server-side cursor's own connection** (s316f census) — materialize the
  ID list FIRST, then per-item close+get.
- 🔧 ⚠ **`BaseModelSerializer` partial updates save ONLY the patched
  columns** (s298).
- 🌐 ⚠⚠ PS5.1 traps: regex-replace file rewrites BANNED; Edit tool or
  `[IO.File]` BOM-less UTF8. ⚠ git commit messages ALWAYS `-F` files
  (s315). ⚠ a bash heredoc patch of a py file can SILENTLY no-op on
  escape sequences — Edit tool for file edits, always (s321 re-lesson).
  ⚠ `$1` in an unquoted PS arg EXPANDS empty. ⚠ `Sort-Object
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
  RIGHT edge (s321: right-edge alignment is the FENCE that excludes
  caption numerals inside a value x-window — the "1," hazard); marker/
  caption tokens can END inside value windows (s307–s311); dot LEADERS
  invade value regions — skip, never raise; an f7206-style value prints
  up to 6pt ABOVE its own marker (s321: the US8606W1/USW10407 grid
  family prints values exactly 1.7pt above the caption's LAST row); a
  checkbox X sits LEFT of its own label (s316g/s319–s321); TaxWise
  RE-TYPESETS schedules (s297) and can OMIT a page it bills on
  (s297/s316/s317/s320; s321: an 8606 page 2 can print Part II
  conversions whose line-17 basis computes on an unprinted page 1 —
  named refusal; a worksheet line 1 can restate an entire absent MISC
  report). ⚠ s321: a multi-page IRS form in one packet can be TWO
  OWNERS' faces, not one owner's p1+p2 — group by the printed SSN.
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase** (s288).
- 🔧 ⭐ **Rolled-back dry-runs reproduce production locally** (s289;
  s292–s321 — the s321 probe script is REUSABLE:
  `D:\tax-test-data\1040\tmp\tie_probe_s321.py`, edit PAYLOADS). ⚠
  Client-named scripts in SCRATCHPAD/tax-test-data only.
  ⚠ `Firm.objects.get(name="The Tax Shelter")`, never `.first()`.
  ⚠ Pooler timeouts kill the connection — ping + retry.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295–s321;
  s321: NINE solos → TWO emits — the coverage gate censors the walls
  behind, s318).
- 🔧 ⭐ **DRY-RUN a correction pass and READ EVERY ROW** (s298). ⭐
  **CENSUS THE BLAST RADIUS before changing a computed line's source**
  (s302/s316f).
- 🌐 ⭐⭐ **THE THIRD QUESTION + ITS CONVERSE + THE FOURTH** (s302/s315/
  s306): "does anything ACT on it?" / "which printed fact does NOBODY
  read?" / "can its condition ever be TRUE for the shape I care
  about?" (s321: the 4b/5b Roth blindness could never fire while the
  coverage gate censored every 8606 packet — opening the class made
  the latent mismatch observable the same hour).
- 🌐 ⚠⚠ **READ WHAT THE ERROR SAYS before pattern-matching it to a known
  class** (s316g; the s281 evidence rule).
- 🔧 ⚠⚠ **A FIXTURE THAT KEYS A COMPUTED COLUMN MEASURES NOTHING** (s302d).
- 🔧 ⚠⚠ **A NEW BOOLEAN FIELD RE-SCOPES EVERY COUNT THAT PREDATES IT** (s311).
- 🔧 ⚠ **The answer key is a CONTRACT** (s313/s314); **A TIE CANNOT SEE
  NON-RECONCILED FIELDS** (s315; identity-verifier still open,
  DEFERRAL item 7). (s321: line 36 was IN the answer key since
  batch-001 #4 and nothing ever FED its route — the first witness
  found it.)
- 🔧 ⭐ **MIRROR THE ENGINE, one helper both sides** (s312→s321:
  mirror_f8606 re-types part_i/ii/iii; the 4b/5b split now reads the
  engine's own doc_is_ira_path semantics).
- 🔧 ⚠⚠ **THE SURNAME REFLEX MUST FIRE AT WRITE TIME** (s307–s321 —
  SEVEN sessions running; s321's draft parser docstring carried a real
  client surname copied from a code comment — sweep even text you
  copied from ALREADY-COMMITTED files; the s318 server/tests triage
  still holds three legacy instances).

## 🔎 Carried for triage — NOT claims
- **(s321) The recap-vs-face refund mismatch witness** — one packet's
  Main-Info recap prints 1000 where the face shows 4000 (surfaced
  when its f8606 wall fell). Un-diagnosed; suspect a source anomaly.
- **(s321) The alloc-wks identity witness** — rows sum 159,918 vs
  printed total income 158,873 (Δ1,045); surfaced behind the f8606
  wall. Un-decomposed.
- **(s321) The 2b-no-payer + 25c-210 witness** (the Part-III Roth
  packet) — both pre-existing classes, now its only walls.
- **(s321) The Sch 1-A tips wall witness** (25,000 trade-or-business
  tips, lines 5/13/38) — unwitnessed shape, named refusal.
- **(s321) Two MFJ ownerless-8z witnesses** — 11,040 ministerial
  housing / 3,000 box-3; the worksheet CAN print T/S letters (column
  exists) but these rows don't. A TSJ-keyed reprint would convert.
- **(s320) TWO packets miss printed 8863 Part III student pages** —
  reprints convert the solo one (Ken ask ⑥).
- **(s319) The $46 + (s321) ~$61 §6654 safe-harbor divergences** —
  now ONE two-witness class (Ken item ③).
- **(s319) The agent-lane -Merge witnesses** (now two: the f2441 one
  + the s321 ira_wks one) — routed, not lost.
- **(s319) The Sch-C decomposition gap witness** — Schedule 1 line 3
  prints 12,223 vs extracted nets 2,760. Undiagnosed.
- **(s318) Client 4081's $169 RIE-L6 interest divergence** — outcomes
  tie either way.
- **(s318) The FTC-205-against-tax-0 witness** — refuses by name.
- **(s318) One packet prints 1040 line 26 with NO est-payments page**
  (the omitted-page shape; named blanket refusal).
- **(s317) TWO packets wait only on the GA LIC keyed gate** (client
  3815 + the s316 witness).
- **(s317→s320) Item-64 joint-OT class: 11 packets, 8 single-wall.**
- **(s317) The multi-W-2 tips witness** (2 same-owner W-2s, 4c=770) —
  needs i1040 + RS amendment, flagged.
- (s316f) Client 1382: preparer-keyed RIE-TP-10 override holds the
  joint refund — clearing it is on Ken's queue.
- (s316) The third f7206 solo: allocation-worksheet identity break
  (130,193 vs 127,952) — undecomposed.
- (s313) the four 25c/8959 witnesses — every carrier holds other walls.
- (s313) Client 4059's W-2G payer street address (document ask).
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
**s321 deploy close-out:** `4db9ab3a` (both legs + the roth_ira_bases
surface) — deploy `dep-daag1rgu01pc73dprjcg` status at session end
recorded below; the docs-only close commits after this line trigger
one more deploy — verify the LAST one at next boot.
