# TTS Tax App — STATUS (current state only)

*⭐ s302 (2026-08-26 night): **BUILD-QUEUE ITEM ⑥ SHIPPED AND CLOSED IN
PRODUCTION — the §469(g) PTP release + the Form 8990 1040 ledger surface**
(`9999f2c6`, deploy `dep-da7q5rek1f9s73chfqe0`, API-verified LIVE; annex in
BATCH-296). ScheduleK1 gains the complete-disposition trio (mig 0367,
mirrored from the RentalProperty model trio); the §469(k) branch releases
current + prior-unallowed losses IN FULL when the trio qualifies —
verified verbatim from the 2025 i8582 PDF (PTP rule 4), NOT the RS spec,
whose R-8582-PTP text turned out to OMIT the unrelated-person bar (states
lane staged the correction as S-15; the engine carries the bar). i8582
rule 2 makes the allowed amount branch-invariant, so no disposition-gain
input is needed. Staging refuses the trio by name wherever the branch
never reads it (non-PTP / materially-participated / participation
omitted); D_K1_PTP_469G (info) speaks for released AND partially-asserted
rows; D_K1_PTP_LOSS stands down on released rows. Both K-1 screens carry
the trio. The 8990 half: `form_8990_schedule_a` is now a 1040 backentry
section — ONE shared allowlist/validator with the entity lane (moved to
backentry.py) — and the item's own fixture forced a verified guard
CORRECTION: only the flowing Schedule A columns (f)/(g)/(h) reach Part I
(Rev. 12-2025 FORMULAS encoding), so col (c) current-year EBIE now stages
nonzero on BOTH lanes (the old guard would have refused L019's own row).
Pre-deploy proof: rolled-back commit of the entry lane's staged 1792 —
TIE on all 24 reconciliation lines including both RIE columns exactly
(the item-B ∓1 residual is GONE; the s298 per-vendor split covers it).
**The entry lane re-staged and COMMITTED 1792 FILED, full clean tie.**
Gates: 13+9 new tests; 54 + 542 + 568 green; typecheck; vitest 8/8; the
s274-era standing red in the GA700 coverage test found and repaired (it
greped only FORMULAS_GA700 and could not see the GA700_FEDERAL_PULL
engine writes — red since s274, in no routine run, the s268 class).*

*⭐ s302b (same night, second deploy): **the line-26 estimated-payments
blocker** (`a9f97025`, deploy `dep-da7qalvlk1mc738cgd60`, API-verified
LIVE — the entry lane's 2638 item, filed in BATCH-296). The s289
consumed-by-nothing class IN THE BAD DIRECTION (states lane's framing,
adopted): dated `federal_estimated_payments` rows staged clean and
contributed NOTHING to line 26 — the app silently dropped money the
taxpayer actually paid and overstated the balance due.
`estimated_payments_total` now follows compute_2210's own precedence
(creditable rows, when any exist, ARE the payments record; legacy quartet
+ py_overpayment_applied are the no-rows fallback), so line 26 and the
§6654 penalty can never read different payment records. Pre-build prod
census: of 51 returns carrying rows, ZERO move. D_2210_DATED re-scoped to
the residual stale-scalar class — the old detector HAD fired on the
motivating shape (test-proven, green the whole time): detection was never
the gap, CONSEQUENCE was. ⭐ The states lane's third question is now
standing method: alongside "does it exist?" and "does it run?" ask
**"does anything ACT on it?"** — a green detector nobody reads is
functionally no detector. RS: R-PAY-04 amendment staged as S-16. 7 new
tests + the 2210 contract-flip pair; 573 green incl. flow assertions.*

*s302c (same night, third deploy): **D_EFILE_004** (`f882e494`, deploy
`dep-da7qdacs728c73cml8i0`, API-verified LIVE). The entry lane's first EIN-complete packet advanced the
readiness check to the schema tree, which prod deliberately lacks
(docs/mef/schemas/ gitignored: "gated IRS distribution, redistribution
terms unconfirmed"). `SchemaNotAvailable` is now its own outcome — a
WARNING naming the environment limitation ("readiness unmeasured in this
deployment"), while every other exception keeps the D_EFILE_002
internal-fault error (injection twin: a plain FileNotFoundError still
faults). ⛔ NEW KEN: install the MeF schema trees on Render, or not
(REVIEW_QUEUE; 2025v5.3 = 21.1 MB / 769 files, exists locally).*

*⭐ s302d (same night, fourth deploy): **Schedule SE line 8a derives from
the W-2 wage boxes** (`ffedba4d`, deploy `dep-da7qp749v7es73bl9psg`,
API-verified LIVE; the entry lane's 1412 item, filed in BATCH-296). THE SAME CLASS A THIRD TIME
TONIGHT, again overstating: line 8a is what consumes the $176,100 social
security wage base, and the only way to fill it was the preparer-entered
`ScheduleSE.w2_ss_wages` — nothing carried `W2Income.social_security_wages`
into it, so a taxpayer whose W-2 already exhausted the base paid the FULL
15.3% on self-employment income instead of Medicare-only 2.9% ($5,717 on
the reporting packet, plus the ½-SE/AGI/QBI/Georgia cascade). The pure
chain was always correct; it always received 0. New shared helper
`se_line_8a_for` (Σ boxes 3+7 for THAT proprietor; a nonzero keyed value
wins as the override and is the only way to state RRTA tier-1), wired
into ALL FOUR consuming sites (compute / print / MeF / diagnostics — the
s295b lesson). Authority: the 2025 Schedule SE face, downloaded and read.
Census: 187 SE rows, 36 derive, ZERO change SE tax. RS: R-SE-L8D-L9
consumes L8a and declares NO inputs — staged as S-17. ⚠⚠ MY OWN FIRST
TEST PASS WAS VACUOUS (keyed `ScheduleC.net_profit`, which compute
overwrites from gross receipts → line 6 = 0 → "0 == 0" satisfied every
rate assertion); only the partial-base case failed loudly. Every
end-to-end test now asserts line 6 nonzero FIRST. 10 new tests; 659 green
incl. flow assertions.*

*▶ NEXT (build queue): ⑦ CANDIDATE — the general-category Form 1116
(D_1116_003 refuses 4 of the 12 1116-carrying Lacerte packets; client
2303's 9,893 credit is the ENTIRE liability; measured priority, sits
next unless Ken reorders). Then extractor follow-ons by residual:
line-20 Sch 3 face class (5) > MFJ ownerless int/div (4) > the 1099-R
5b-decomposition probe (1) > SchB payer-less exception (1). NEW BATCH-296
entry-lane items still open: the `div_1099s.us_government_income` →
GA S1-10 derive design (attribution-only today BY DESIGN — an
auto-derive needs an off-switch decision, s237 class) · the D_SCHD_006
QOF-answer import surface (schd_fields lacks it; warning-level).*

*⭐ POST-DEPLOY CONFIRMATION (entry lane, same night): **all four units
verified in production by an independent lane.** 1792 committed FILED on
the §469(g)+8990 deploy (full clean tie); 1412 re-ran on s302d → TIE, SE
tax 1,337 Medicare-only with the whole predicted cascade landing; 2638
re-ran on s302b → TIE, the four dated rows landing 12,000 with no scalar;
D_EFILE_004 confirmed on three real packets (1521 / 2638 / 1412 all
cleanup-clear, held 0 — the schema-tree issue no longer holds anything;
before tonight one packet had cleared cleanup, now four).*

*Peer state (s302): I held tree + test_postgres all session (both peers
confirmed at boot); RELEASED at close and the states lane then ran the RS
suite themselves on their own tree: **254 passed / 0 failed** (245 → 254
as their ten pinned integrity gates entered the suite); prod re-counted
at **169 forms / 690 authority rows / 4,663 facts**. Nothing outstanding
between the lanes. ENTRY lane: 1723 committed on the s301b e8960
surface; **1792 committed FILED (full tie)** on this session's deploy;
2638 HELD awaiting their re-run of fixture batch a0ffa7a8 against
s302b (told: no py_overpayment_applied scalar); six wrong-preparer
packets self-corrected via replace_documents (their own transcription,
no engine issue); continuing the 41-packet queue (1633 next). STATES
lane: S-15 (SCHEDULE_K1 disposition trio + R-8582-PTP related-party
correction) and S-16 (R-PAY-04 dated-rows source) both staged for Ken,
both independently verified on their side; RS 245/0.*

*⛔ KEN remaining: NEW — MeF schema trees on Render (D_EFILE_004 is the
correctly-labeled standing state until ruled) · S-15 + S-16 (RS, states
lane) · carried: the s298 truncated-name PREFIX-MATCH tier + organizer
enrichment · S-14 SEHI method (three facets) + s300b D_8962_NOCONVERGE
note · S-11/S-12 dependent-chain · the s295 int/div ruling (narrowed) ·
the 51-dependent DOB ask · the 1723 GA Eligible-Itemizer question ·
#21 (its full-allowance half largely superseded by s302's release; the
partial-allowance-against-PTP-income half stands) · #48 (RS 404), #56,
#63, #69, #10 tail. Carried: entity second-state-face transport (#3);
`OVERRIDE_HONORED_STATE_LINES`; 146-packet re-export; NC/CA/SC
linked-state reopens; #6 1065X/AAR; #68 optimizer; s274 PII narrowings;
RS 8990 re-authoring gate; 6765 Sec G; client-4545 D_8606_BASIS_ONLY;
1065 BATCH-004 #4; Analysis line-2 active/passive proxy; the unfloored
8960 line5 §1211(b) question; tier-3 PII scrub; the 47 RS-integrity-gate
sweep (12 FAIL, states S-10 — ⚠ includes 8582-PTP1: no independent
recompute maps the PTP rule, which is WHY s302 verified from the PDF).*

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
- s302 deploys: `9999f2c6` → `dep-da7q5rek1f9s73chfqe0` (⑥, mig 0367 +
  D_K1_PTP_469G seed) — **API-confirmed LIVE**; `a9f97025` →
  `dep-da7qalvlk1mc738cgd60` (s302b line 26, no migration) —
  **API-confirmed LIVE**; `f882e494` → `dep-da7qdacs728c73cml8i0` (s302c
  D_EFILE_004 seed, no migration) — **API-confirmed LIVE**; `ffedba4d` →
  `dep-da7qp749v7es73bl9psg` (s302d SE line 8a, no migration) —
  **API-confirmed LIVE**. All four verified 2026-08-26.
- s301 deploys: `d6ab0e42` → `dep-da7ovk6417fc738ffrq0` · `03c96795` →
  superseded by `2a2bb807` — LIVE (s301b e8960 surface confirmed by the
  entry lane's committed 1723).
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (s302 regenerated; current as of `9999f2c6` — s302b/c changed no schema.)

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. s302 coordinated at boot;
three annexes appended to BATCH-296 (every load-bearing exchange annexed).
Peers stage; Ken decides. ⚠ A peer's factual claim can be RETRACTED
minutes later (the s302 "second fixture" correction) — build only against
what a batch file or your own probe shows.

## ⚠ Known red / rotted — THE ONE LIST (post-s302)
- ⚠⚠ **12 of 47 RS `check_*_integrity.py` gates FAIL** (states lane sweep
  2026-08-26) — staged for Ken (their S-10). Re-diagnose before inheriting.
  ⚠ 8582-PTP1 among them ("no independent recompute mapped").
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof. (s302: the two s258
  fails appeared again in the sweep and PASSED in isolation — unchanged.)
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ~~the GA700 answer-key coverage test~~ — REPAIRED s302 (was red since
  s274; greped only FORMULAS_GA700, blind to the pull-map writes).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s302).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): five phantom failures. New files/markdown/scratchpad only.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c`; a script run by absolute path needs
  `sys.path.insert(0, server)` (s298). ⚠ `python -m taxwise1040`
  does NOT resolve from `server\` — run the package's `__main__.py` BY
  PATH (s297).
- 🔧 ⚠ **`BaseModelSerializer` partial updates save ONLY the patched
  columns** (s298) — a serializer-side derived write must be injected into
  `validated_data`, never `setattr` on the instance (it silently no-ops).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ (s302 re-confirmation: `-replace` +
  `Set-Content` mangled an em-dash annex — python io did it clean.)
  ⚠ Embedded double-quote in a here-string arg to a NATIVE exe SPLITS the
  argument. ⚠ `$1` in an unquoted PS arg EXPANDS to empty. ⚠ PS
  `Sort-Object -Unique` on a one-element pipeline UNWRAPS to scalar (s296).
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash. ⚠ A
  COMMITTED return refuses plain stage+dryrun with HTTP 409 (s298 —
  verification route = the in-place rolled-back recompute, s289 pattern;
  s302 ran the 1792 probe this way clean, incl. the commit reconciliation).
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); synthetic `insert_text` PDFs may
  LOSE leading spaces (s290) — the extractor parses POSITIONALLY everywhere.
  ⚠ **The AcroForm FILLER flattens ALL widgets out of its output** (s296).
  ⚠ **TaxWise RE-TYPESETS whole schedules** (s297): pin from packets only.
  ⚠ s301: a fixture that encodes the WINDOW's geometry instead of the
  CORPUS's keeps a blind spot green.
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section. Local templates beat both.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292–s302 ran probes this way). ⚠ Scripts touching
  client-named returns live in SCRATCHPAD or tax-test-data, never the repo
  (PII). ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295/6/7/s301).
- 🔧 ⭐ **DRY-RUN THE CORRECTION PASS AND READ EVERY ROW before --commit**
  (s298). ⭐ s302 corollary: **CENSUS THE PROD BLAST RADIUS before changing
  a computed line's source** (51 returns read, zero moved — the do-no-harm
  half; the failing case rides the tests).
- 🌐 ⭐⭐ **THE THIRD QUESTION (s302, states lane): "does anything ACT on
  it?"** — after "does it exist?" and "does it run?". A green detector
  nobody reads is functionally no detector, and it is HARDER to spot
  because it announces itself as a pass (D_2210_DATED fired correctly for
  weeks while line 26 dropped the money it was flagging). ⚠ s302d: the
  same question applies to a TEST — mine ran, was green, and measured
  nothing.
- 🔧 ⚠⚠ **A FIXTURE THAT KEYS A COMPUTED COLUMN MEASURES NOTHING** (s302d):
  `ScheduleC.net_profit` is written by compute from gross receipts, so a
  test that keys it gets line 31 = 0 — and every rate/ratio assertion then
  passes as `0 == 0`. Key the INPUT (`gross_receipts`), and assert the
  intermediate is nonzero BEFORE asserting anything about a rate applied
  to it.

## 🔎 Carried for triage — NOT claims
- (s302d, entry lane) **D_EFILE_001 cannot distinguish "EIN not keyed"
  from "EIN not obtainable."** Most packets satisfy it from the GA-500
  income-statement grid, but a payer that withheld no Georgia tax never
  appears there — a third-party sick-pay payer on one packet has no EIN
  anywhere in the source. That packet is held on a gap the preparer
  cannot close from the documents. If the shape recurs, the refusal
  should split (and the s298 21-blank-row class is the same question
  from the other side, still on Ken's prefix-tier call).
- (s302, states lane S-10c) **`D_8582_PTP` remains unverified** — one of
  seven keys `check_schedule_e_8582_integrity` reports as "no independent
  recompute mapped". s302's §469(g) build is validated only by its own
  i8582 PDF reading, which is why that reading was done. ⚠⚠ Their wider
  finding: `check_topic8`-style gates compare a rule's DECLARED inputs
  against the facts and never compare the FORMULA against the
  declaration — so `inputs: []` is vacuously valid and **the emptier a
  rule's declaration, the safer it looks** (exactly how R-SE-L8D-L9's
  missing L8a input survived; found by a $5,717 error, not by a gate).
- (s302) `div_1099s.us_government_income` is attribution-only BY DESIGN
  (splits a keyed ga500_fields S1-10 by owner; derives nothing) — the
  entry-lane note asks whether it should auto-derive S1-10; needs an
  off-switch decision (s237 class). · D_SCHD_006 QOF answer has no import
  surface (warning-level, filed).
- (s301) One packet's 5b decomposition: extracted pension rows carry
  115,150 vs face 15,150 — probe the 1099-R parse before assuming a
  rollover/exclusion. · One packet's Sch D carries 1b grid totals with NO
  f8949 pages + its own h-identity break — suspected misparse. · A blank
  printed 8949 page (synthetic packet) refuses "no transaction rows" —
  over-refusal by design.
- (s298) 21 named-but-blank W-2/1099-R rows have no unambiguous EIN match
  — held on D_EFILE_001; movement waits on Ken's prefix-tier call.
- (s297) The X mark at (474.7, y≈389) in the 1040 p2 EIC row region —
  unidentified, parser-ignored. · `est_payments_wks` (27) and `f8863` (6)
  gate at the face. ⚠ s302b note: when the est_payments_wks face class
  opens, the extractor should emit dated `federal_estimated_payments`
  rows — they now drive line 26 directly.
- (s296) The 22 sch_d GEOMETRY-error packets refuse loudly by design.
- (s295) 7 auxiliary Inbox PDFs refuse as non-packets — correct. ·
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
- (s285) Sch 4 nonresident arm still apportions the whole widened base.
- (s283) The stamp excludes 1040 packets (Ken). · (s282)
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
Everything from s277–s301 stands (see STATUS_ARCHIVE s300/s301 entries).
NEW (s302): **S-15** (SCHEDULE_K1 §469(g) disposition trio + the
R-8582-PTP related-party CORRECTION — the spec as written frees losses on
a related-party sale, the exact case §469(g)(1)(B) bars; + rule-2/rule-4
branch naming + the fully-taxable definition) and **S-16** (R-PAY-04
dated-rows source, matching R-2210-REG's precedence) — both staged by the
states lane, both Ken's. Recorded lane-side, not staged: the RS 8990 spec
models NO Schedule A surface (the seam if it is ever specified is
`partner_excess_bie`).
