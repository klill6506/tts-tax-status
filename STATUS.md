# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-25 (s291 — two units: ⓪ the DG-4 unblock (the s290
push hold LIFTED — RS amendment + spec re-export + the two held commits
LIVE) and the BATCH-296 NIIT disposition-flag build. Both deploys
Render-verified LIVE.)*

*⚠⚠ RESUME POINT — **the queue is drained and no push is held.** The next
spine item is **① the TaxWise extractor build** (approved plan ② phase B,
2-3 sessions — START IN A FRESH SESSION; then the 50-return pilot on
John's book). Nothing is blocked on RS or the states lane.*

*s291 SHIPPED AND LIVE (both Render-API-confirmed):
**① `78b4e60` / dep-da70jq49v7es73evgn70 — the DG-4 unblock.** The RS tree
came CLEAN (states lane committed `f583fbc`), so this session took the
paired amendment per Ken's ruling: RS `2845b28` drops 1040_SPINE scenario
DG-4 via the explicit-retirement pattern (D008_RETIRE_SCENARIO_PREFIXES;
the D_1040_008 FormDiagnostic entry KEPT with retirement notes — the s266
spirit), seeded against the RS DB (32→31 scenarios, integrity clean),
cached `server/specs/1040_spine_spec.json` re-exported (only test-level
delta = DG-4 gone), trip-wire retitled `test_spec_has_all_4_dg_scenarios`.
112 spine tests green. The push took ALL FOUR held s290 commits live
(D_1040_008 retirement `0be704c`, the 8995 line-1 table `a47de3a`, both
close commits). Post-deploy verified: the live D_1040_008 DiagnosticRule
row is `is_active=False` (the build reseed deactivated it as designed).
**② `e5e6d64` / dep-da70pegae00c73chbcmg — the BATCH-296 NIIT disposition
flag** (client 4167, the awaited NIIT decomposition): the entry lane's
probe held exactly — `dispositions[].net_investment_income_tax` was stored
and consumed by NOTHING. Now `non_1411_disposition_section_1231` sums each
flagged (`"no"`) is_4797 row's §1231/capital component (short-term → 0;
part3 → l24 − ordinary recapture; part1 → whole l24) and joins the #18 K-1
feed in ONE combined 8960 line-5b auto back-out under the single
§1.1411-4(d)(2) clamp. Authority: 2025 i8960 Line 5b; §1411(c)(2)(A); the
participation determination stays the preparer's. Per-row kwargs extracted
to `_property_kwargs` — one derivation shared with the 4797 aggregate.
7 tests + injection proof; 579 green (flow assertions + all 8960 suites);
127 green (all 4797 suites). Boundary (DEFERRAL_AUDIT): 6252/8824-linked
flows don't consult the flag — a flagged installment sale is a NEW item.*

*▶ NEXT: **① the TaxWise extractor build** (fresh session). Then
Ken-directed: EIC derive unit (probe-first) · AOTC picker. Then defects by
cost: the [client] −487 residual + S1-13 double-landing observation (hold
b926; instrumented rolled-back dry-run) · item 84 (§469(i) $25,000
allowance) · the two Houston items (4952 line 4e/4f derivation overstates
the deduction; capital_transactions.owner IGNORED in the POSITIVE-line-7
GA RIE split — both probed byte-identical, annexed in BATCH-296) · #60 ·
#43-medium · #70 · #16.*

*Entry lane: **[client] (client 4167) can re-stage NOW** — the flag is
honored live; keying stays `net_investment_income_tax: "no"` (annexed in
BATCH-296 with the boundary notes). Lacerte sub-queue remains UNBLOCKED
(79 indexes; regenerate newer arrivals with the script). Two filed returns
still to re-key on the QBI cf sign (s290 annex). ⚠ The 16A→2a routing can
move §86 taxable SS on staged returns carrying S-corp K-1 tax-exempt
interest.*

*⛔ KEN remaining: #21, #48 (RS 404), #56, #63, #69, #10 — the tail tier.
Carried: entity second-state-face transport (#3); `OVERRIDE_HONORED_STATE_
LINES`; 146-packet re-export; NC/CA/SC linked-state reopens; #6 1065X/AAR;
#68 optimizer; s274 PII narrowings; RS 8990 re-authoring gate; 6765 Sec G;
client-4545 D_8606_BASIS_ONLY; 1065 BATCH-004 #4 (Codex Box-2); Analysis
line-2 active/passive proxy; the unfloored 8960 line5 §1211(b) question
(s272 note, re-flagged s291).*

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
hold only for a named reason.** Main is in sync with origin; no held
commits. ⚠⚠ **ORDERING (s279/s282): push → deploy LIVE → seed → verify —
and the deploy ITSELF seeds (`build.sh seed_all` auto-discovers `seed_*`
at BUILD time). Manual post-deploy seed = the idempotent VERIFY;
`check_rule_paths` is one command.**

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. s291 claimed the RS tree
by message after it came clean (per the s290 ruling), committed `2845b28`,
and RELEASED it — the RS tree is the states lane's again. Peers stage;
Ken decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s291)
- ~~`test_dg_scenario[DG-4]`~~ **CLEARED s291** — the RS amendment landed;
  the spine suite is fully green (112).
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s290; no client changes s291).

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
  bash heredoc (used all session).
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); a field map guessed from SHORT
  widget names silently no-ops (s287). ⚠ pymupdf `insert_text` synthetic
  PDFs may LOSE leading spaces at extraction — parser tests needing indent
  signals also need a shape fallback (s290).
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section (s290: i1040gi.pdf →
  line-2a verbatim). Local templates beat both.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces a staged return's
  production behavior locally** (s289). ⚠ Scripts touching client-named
  returns live in SCRATCHPAD, never the repo (PII).

## 🔎 Carried for triage — NOT claims
- (s290) The GA RIE interest row does NOT include K-1 16A tax-exempt
  interest — CORRECT by the r. 560-7-4-.02 base (not in GA taxable
  income); stated boundary, not a gap. · The rendered 8995 TIN prints
  UNFORMATTED digits (no NN-NNNNNNN dash) — cosmetic, all consumers.
- (s289) `IndividualForm7203` has no §179/charitable carryover keying
  fields — D_K1_7203_DEDUCTION_LIMITED warns; DEFERRAL_AUDIT has the build
  trigger. · The 7203/K-1 §179 cap does NOT extend to 1065 partners.
- (s288) `IndividualForm7203` still has no home for box 16 code E;
  1065 box 18 a/b/c have none on the recipient side (16A now ROUTES — 18A
  still cannot be keyed, s290 boundary).
- (s288) `ctc_override` / `odc_override` + `Dependent.compute_qualifies_*`
  are DEAD everywhere now the 008 retirement is deployed — removal
  candidate for a cleanup pass (Ken's call; they still hold keyed data).
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
  WEIGHTS — a K-1-gains-only MFJ return falls to the carryover/50-50
  fallback; pre-existing, noted.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried + s291 adds:
Everything from s277–s290 stands (R-K1-20N; R-GA700-PARTNERS; AL_FORM_40NR
amendments; R-AL-TAX; R-B1/B2-AUTO; the 4562 line-17 reversal;
R-8880-LINE1-COMPOSE; FORM_7203 Part I line-3 sub-map; R-K1-179-BASIS;
R-GA500-RIE loss-allocation clause; SCHEDULE_K1 box 16 A/B routing;
R-8995-QBI line-1 population widening). ~~1040_SPINE DROP scenario DG-4~~
**DONE s291 (`2845b28`)**. **s291 adds: (a) FORM_8960 R-8960-INCOME** —
the 5b description should record BOTH auto back-out feeds (the #18 K-1
§1231 feed and the s291 flagged-disposition feed) the way the formula
records the 2b/3b/line-7 auto-pulls. **(b)** the unfloored line5 §1211(b)
question (s272) is still open for Ken — §1.1411-4(d)(2) arguably floors
the whole net-gain category, a behavior change for every capital-loss
return.
