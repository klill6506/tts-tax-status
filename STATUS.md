# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-26 (s292 — the TaxWise extractor, session 1 of ~3:
the pipeline runs END TO END and produced its FIRST TWO TIES. Also: the
spine spec cache absorbed BOTH same-evening RS dependent-fact deltas.)*

*⚠⚠ RESUME POINT — **extractor session 2** (`scripts/taxwise1040/`). The
skeleton is COMPLETE: classifier (90+ page types) → census → positional
parsers (Main Info · 1040 face incl. 25a-c/36/38 inner bands · W-2/1099-R/
W-2G detail reports · GA-500 + Sch 1 + RIE wks) → emitter (refuse-don't-
guess gate, recap-vs-face misparse refusals, 25a/25b withholding
decomposition, NAME→client_number shell resolution) → 10-per-batch
payloads that pass `import-lane.ps1 validate`. Full-Inbox run: **16/267
emitted; rolled-back tie probe (s289 pattern): 2 TIE to the dollar, 12
NO_TIE, 1 pooler timeout.** Session-2 worklist, by measured yield:
① the p1 income-decomposition gate (face 1a–8 vs extracted documents —
turns sub-threshold interest/dividends, cap-gain distributions and
SSA-without-worksheet into REFUSALS instead of silent NO_TIEs) + SSA
extraction from face 6a + the 12a claimed-as-dependent checkbox (two
probed returns filed the LIMITED std deduction); ② Sch B/USSTB int-div
detail parsers; ③ dependents (p1 grid + 8867/Sch EIC); ④ ⚠ investigate
the probe's two OVER-computations (one +23k — possible extraction defect;
audit the overstating half FIRST). Pilot target: 50 of John's book —
101 JOHN packets sit in the Inbox now (census: KEN=140 JOHN=101 JACOB=19
non-held).*

*s292 mechanics worth knowing cold: TaxWise draws values as SEPARATE text
objects — text order is meaningless, everything parses off word bboxes
(`words.py`). Detail reports parse off the dash-RULER row (each dash-run
bbox = a column; printed totals re-summed as a tripwire). The GA face
prints each line number twice — the RIGHT instance (x 338-412) sits on
the value's row. Fed-WH can RUN INTO the state code ('4512GA' — split by
char width); state codes can print lowercase. The shells index
(`shells_index.py` → `D:\tax-test-data\1040\shells-index.json`, 2,965
shells) is REQUIRED for locators — {ssn,last_name} resolves nothing
because most seeded shells carry no taxpayer SSN (s289). Probe scripts +
all outputs live in tax-test-data / scratchpad (PII), never the repo.*

*RS spine deltas absorbed (both Render-verified in the same push):
`a396379` deleted `dep_ctc_flag`/`dep_odc_flag` (Ken direct: CTC/ODC are
DERIVED, never preparer-asserted) and `3fc79fd` aligned `dep_tin_type` to
TIN_TYPE_CHOICES verbatim, added `dep_citizenship_status`, and WITHDREW
the redundant §24(h)(7) boolean (tin_type='valid_ssn' already carries the
whole test). Cache re-exported twice, structural diff verified both
times; the runner's unknown-dependent-fact guard caught delta #2. 112
spine tests green. REVIEW_QUEUE: `dep_released_by_form_8332` is the ONE
remaining unmodeled spine fact (fold into the ctc_override/odc_override
dead-field cleanup when Ken approves). The delvio dependents-screen
CTC/ODC checkboxes are the s288 removal candidates — Ken's ruling implies
they go, but UI removal was NOT done this session.*

*▶ NEXT: **extractor session 2** (worklist above) → the 50-of-John's
pilot. Then Ken-directed: EIC derive unit (probe-first) · AOTC picker.
Then defects by cost: the item-59-return −487 residual + S1-13
double-landing (hold b926) · item 84 (§469(i) $25,000 allowance) · the
two 4952/GA-RIE-owner items (annexed in BATCH-296) · #60 · #43-medium ·
#70 · #16.*

*Entry lane: unchanged from s291 — client 4167 can re-stage (flag live);
Lacerte sub-queue unblocked; two filed returns to re-key on the QBI cf
sign. ⚠ The extractor's PipelineOut dir + shells-index.json under
`D:\tax-test-data\1040\` are PII working files of THIS unit.*

*⛔ KEN remaining: #21, #48 (RS 404), #56, #63, #69, #10 — the tail tier.
Carried: entity second-state-face transport (#3); `OVERRIDE_HONORED_STATE_
LINES`; 146-packet re-export; NC/CA/SC linked-state reopens; #6 1065X/AAR;
#68 optimizer; s274 PII narrowings; RS 8990 re-authoring gate; 6765 Sec G;
client-4545 D_8606_BASIS_ONLY; 1065 BATCH-004 #4 (Codex Box-2); Analysis
line-2 active/passive proxy; the unfloored 8960 line5 §1211(b) question.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.
  (The 2026-08-24 refinement covers COMMIT MESSAGES only — this file stays strict.)

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS (WHEN BUILDS RUN)
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax` (srv-d6geloa4d50c73el2trg); demo = `tts-tax-demo`. ⚠ **A push
is not a deploy — CHECK THE DEPLOY STATUS after pushing** (API key in
`D:\dev\Passwords & Secrets\render-api-key.txt`). **Standing push
authorization (Ken 2026-08-23): push at own judgment; verify every deploy;
hold only for a named reason.** ⚠⚠ **ORDERING (s279/s282): push → deploy
LIVE → seed → verify — and the deploy ITSELF seeds (`build.sh seed_all`
auto-discovers `seed_*` at BUILD time). Manual post-deploy seed = the
idempotent VERIFY; `check_rule_paths` is one command.**

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. s292 exchanged two RS
spine deltas with the states lane by message (both absorbed same-session);
the RS tree stayed the states lane's throughout. Peers stage; Ken decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s292)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s290; no client changes s291/s292).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): source-inspection flow assertions slice the NEW file with the
  OLD import's line numbers — five phantom failures. New files/markdown/
  scratchpad only during a run.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c`.
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ Embedded double-quote in a here-string
  arg to a NATIVE exe SPLITS the argument — use `git commit -F -` with a
  bash heredoc. ⚠ `$1` in an unquoted PS arg EXPANDS to empty (s292:
  'USWW2E$1' → 'USWW2E') — single-quote TaxWise code tokens.
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); a field map guessed from SHORT
  widget names silently no-ops (s287); synthetic `insert_text` PDFs may
  LOSE leading spaces (s290) — the s292 extractor parses POSITIONALLY
  (word bboxes) everywhere for exactly this reason.
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section. Local templates beat both.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292 ran the whole 16-return tie probe this way —
  staging INSIDE the rolled-back transaction too). ⚠ Scripts touching
  client-named returns live in SCRATCHPAD, never the repo (PII).
  ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection and the reconnect can flake DNS — ping + retry loop.

## 🔎 Carried for triage — NOT claims
- (s292) The extractor probe saw TWO over-computed returns (one +23k on
  line 11) — could be an extraction defect OR a filed-return exclusion the
  engine doesn't model; session-2 item ④, investigate before more parsers.
- (s290) The GA RIE interest row does NOT include K-1 16A tax-exempt
  interest — CORRECT by the r. 560-7-4-.02 base; stated boundary, not a
  gap. · The rendered 8995 TIN prints UNFORMATTED digits — cosmetic.
- (s289) `IndividualForm7203` has no §179/charitable carryover keying
  fields — D_K1_7203_DEDUCTION_LIMITED warns; DEFERRAL_AUDIT has the build
  trigger. · The 7203/K-1 §179 cap does NOT extend to 1065 partners.
- (s288) `IndividualForm7203` still has no home for box 16 code E;
  1065 box 18 a/b/c have none on the recipient side.
- (s288) `ctc_override` / `odc_override` + `Dependent.compute_qualifies_*`
  are DEAD — removal candidate (Ken's call; the RS a396379 ruling implies
  the UI checkboxes go too — fold with `dep_released_by_form_8332`, see
  REVIEW_QUEUE s292).
- (s287) The 8825 line-1 repaint covers the LINE-1 table only. ·
  The suggested-field convention covers W-2 3/5 + 1099-R box 16 —
  CLAUDE.md's W-2-only note is stale.
- (s285) Sch 4 nonresident arm still apportions the whole widened base.
- (s283) The stamp excludes 1040 packets (name+SSN privacy — Ken).
- (s282) `OVERRIDE_HONORED_STATE_LINES` hand-synced.
- (s281) OOS-state line-18 prompt diagnostic specified, not built. ·
  Stage allowlists `schd_fields` keys, `ga500_fields` not at all.
- (s268) 1,604 queries/run + memoization candidates.
- (s241/s281) `Form8606` unique-constraint candidate · 🔴 `HSAAccount`
  half CLOSED.
- (s275/s281) `.first()`-on-per-form-rules sweep remainder.
- (s289) K-1 capital gains reach Schedule D but not the L9 gain/loss
  WEIGHTS — pre-existing, noted.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried + s292 note:
Everything from s277–s291 stands (R-K1-20N; R-GA700-PARTNERS; AL_FORM_40NR
amendments; R-AL-TAX; R-B1/B2-AUTO; the 4562 line-17 reversal;
R-8880-LINE1-COMPOSE; FORM_7203 Part I line-3 sub-map; R-K1-179-BASIS;
R-GA500-RIE loss-allocation clause; SCHEDULE_K1 box 16 A/B routing;
R-8995-QBI line-1 population widening; FORM_8960 R-8960-INCOME 5b
description; the unfloored line5 §1211(b) question). **s292: the
dependent-fact rework was DONE BY THE STATES LANE (a396379 + 3fc79fd) and
absorbed here — nothing queued from this side.**
