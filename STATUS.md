# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-26 (s293 — extractor session 2: the p1 income-
DECOMPOSITION GATE is built and the emitted set now ties 6/6 to the
dollar. Every refusal across the 267-packet Inbox is NAMED.)*

*⚠⚠ RESUME POINT — **extractor session 3** (`scripts/taxwise1040/`).
State of the pipeline: classifier → census → positional parsers (Main
Info · 1040 face p1 income block 1a-11a + p2 incl. 13a/13b/14 and the
12a/12b/12c checkbox cluster · W-2/1099-R/W-2G detail reports · income
worksheet USW10401 (per-owner SSA/RRB/medicare/WH) · Schedule B face
(per-payer rows + Part III boxes) · TP/SP allocation worksheet USWRNR$1
(attribution only) · GA-500 + Sch 1 + RIE wks) → emitter with the
DECOMPOSITION GATE: every nonzero face income/deduction line must be
explained by extracted documents or the packet refuses by name; the
coverage gate also treats a dash-RULER report page as value-bearing
(detail reports print comma-less integers — the s292 farm packet proved
the currency-token test blind). Full-Inbox r5: **6 emitted / 261 refused
all-named / 267 scanned; rolled-back tie probe 6/6 TIE** (s292 was 2 of
16 with 12 silent NO_TIEs). 57 extractor tests green.*

*s293 facts worth knowing cold: the two s292 "over-computations" were
FAITHFUL extractions — TaxWise packets do NOT always print every source
page (a −23,068 farm loss existed only as face line 8), so the face
decomposition is the guard, never page presence. The alloc worksheet's
mixed TP/SP split can hide a 50/50 JOINT payer (subset-sum proved it) —
whole-column categories attribute, mixed refuses. The income worksheet
prints taxable SSA on ONE of THREE bracket rows (MFS / 50% / 85%). Face
13b (Schedule 1-A) passes through only when a filer is 65+ (engine
derives the senior deduction; a 12,000 MFJ case tied); tips/OT/car-loan
inputs refuse. A blindness X refuses to the agent lane (one real case in
the corpus). MFS Sch-B rows default to the filer.*

*▶ NEXT — session-3 worklist by measured r5 yield: ③ dependents (p1
grid + 8867/Sch EIC; the presence gate already refuses them by name) ·
the USSTB int/div detail parsers (carry per-payer TSJ — also the exit
from the mixed-split refusal; zero solo yield, needed for the Sch
D-family combos) · the ~4-packet sub-threshold int/div class with NO
pages (needs a face-sourced-rows convention — payer unknown; likely a
Ken question) · then page-coverage units by blocked-count: f8995 128 ·
sch_d/8949 83/59 · sch_e 71 · sch_c 60 · asset_detail 60 · sch_a 45 ·
k1_detail 44 · f8889 30 · est_payments_wks 27. THEN the 50-of-John's
pilot (101 JOHN packets in the Inbox). Then Ken-directed: EIC derive
(probe-first) · AOTC picker. Then defects by cost: item-59 −487 residual
+ S1-13 double-landing (hold b926) · item 84 (§469(i)) · the two
4952/GA-RIE-owner items · #60 · #43-medium · #70 · #16.*

*Entry lane: b47268a6 CLOSED (NIIT flag tied, no keying change; filed).
Lane is on the Lacerte sub-queue. Two NEW BATCH-296 items incoming from
them: `state_returns` silently dropped by `merge=replace_documents`
(dry-run caught an AL40NR zeroing — either preserve like the income
sections or warn by name) and AL `ret-exempt` all-or-nothing (cannot
express a mixed exempt/non-exempt MFJ). ⚠ Lacerte prints defeat
`tools/face.py` (all dashes) — the lane wrote `lacerte_face.py`; Lacerte
exports carry NO DOBs and NO payer EINs (D_EFILE_001 will hit); DOB list
tool columns are SPOUSE-FIRST. PipelineOut r2-r5 + shells-index.json are
PII working files of THIS unit.*

*⛔ KEN remaining: #21, #48 (RS 404), #56, #63, #69, #10 — the tail
tier. Carried: entity second-state-face transport (#3);
`OVERRIDE_HONORED_STATE_LINES`; 146-packet re-export; NC/CA/SC
linked-state reopens; #6 1065X/AAR; #68 optimizer; s274 PII narrowings;
RS 8990 re-authoring gate; 6765 Sec G; client-4545 D_8606_BASIS_ONLY;
1065 BATCH-004 #4 (Codex Box-2); Analysis line-2 active/passive proxy;
the unfloored 8960 line5 §1211(b) question; the s292 PII-history scrub
question (REVIEW_QUEUE — needs Ken's go).*

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
holder — coordinate EXPLICITLY before every run. s293 coordinated cleanly:
states lane held the RS tree (A3 authority-ownership in flight,
export-byte-identical; RS suite now 243, `check_authority_owners --strict`
is the per-loader pre-flight); entry lane on the Lacerte sub-queue.
Peers stage; Ken decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s293)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s290; no client changes s291-s293).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): source-inspection flow assertions slice the NEW file with the
  OLD import's line numbers — five phantom failures. New files/markdown/
  scratchpad only during a run.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c` (bit AGAIN in s293 — one-liners only).
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
  LOSE leading spaces (s290) — the extractor parses POSITIONALLY (word
  bboxes) everywhere for exactly this reason.
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section. Local templates beat both.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292/s293 ran whole tie probes this way — staging INSIDE
  the rolled-back transaction too). ⚠ Scripts touching client-named returns
  live in SCRATCHPAD or tax-test-data, never the repo (PII).
  ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection and the reconnect can flake DNS — ping + retry loop.

## 🔎 Carried for triage — NOT claims
- (s293) The r5 refusal census is the session-3 worklist source
  (`D:\tax-test-data\1040\PipelineOut\r5\*.refused.json`); the yield
  analyzer script pattern lives in the s293 scratchpad.
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

## RS AGENDA — carried:
Everything from s277–s292 stands (R-K1-20N; R-GA700-PARTNERS; AL_FORM_40NR
amendments; R-AL-TAX; R-B1/B2-AUTO; the 4562 line-17 reversal;
R-8880-LINE1-COMPOSE; FORM_7203 Part I line-3 sub-map; R-K1-179-BASIS;
R-GA500-RIE loss-allocation clause; SCHEDULE_K1 box 16 A/B routing;
R-8995-QBI line-1 population widening; FORM_8960 R-8960-INCOME 5b
description; the unfloored line5 §1211(b) question). **s293: nothing new
queued from this side; the states lane's A3 delta is declaration-ownership
only (export byte-identical).**
