# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-26 (s294 — BATCH-296 tail pair shipped: the
state-face echo asymmetry fixed + the ret-exempt suppression claim
REFUTED with per-person record flags built. Commit `ce04ad1`, deploy
`dep-da74u6hsrm7s73bdht60` LIVE, API-confirmed, seed verified. s294b
followup same night: the entry lane's re-key REFUSED — `_apply_state_fields`
judged "unknown line" against the RETURN's own FFV rows (bulk-created at
return creation), so any state return predating a seeder amendment
refused the new line at commit while staging accepted it. Fixed: the
guard validates against the form's FormLine set and BACKFILLS the missing
FFV row. Commit `2ce1dfa`, deploy `dep-da754be7bikc73fgk6r0` LIVE,
API-confirmed. 567 green.)*

*⚠⚠ RESUME POINT — **extractor session 3** (`scripts/taxwise1040/`),
unchanged from s293: classifier → census → positional parsers (Main
Info · 1040 face p1 income block 1a-11a + p2 incl. 13a/13b/14 and the
12a/12b/12c checkbox cluster · W-2/1099-R/W-2G detail reports · income
worksheet USW10401 · Schedule B face · TP/SP allocation worksheet
USWRNR$1 (attribution only) · GA-500 + Sch 1 + RIE wks) → emitter with
the DECOMPOSITION GATE. Full-Inbox r5: **6 emitted / 261 refused
all-named / 267 scanned; tie probe 6/6 TIE**. 57 extractor tests green.
The s293 facts-worth-knowing-cold block is in STATUS_ARCHIVE (s293
entry) — read it before extractor work.*

*s294 (this session, batch-loop detour before the extractor): the two
BATCH-296 tail items are RESOLVED, annexed, deployed:*
- *`state_returns` × `merge=replace_documents`: NOTHING ever deleted an
  omitted state return — the "40NR zeroing" was `reconcile_expected`
  comparing state expectations against ZEROS because `state_faces` only
  echoed payload-supplied states. Now EVERY attached registry-state face
  echoes live (GA-500 already did) and the replace pass warns by name
  (`state_returns[FORM] left in place`). A correction batch no longer
  re-supplies `state_returns` verbatim; an omitted face reconciles
  against live values. NOT recomputed — refresh-from-federal stays the
  explicit gesture.*
- *`ret-exempt` "suppresses ret-t": REFUTED — compute_al40nr never reads
  any ret-exempt flag; ret-t/ret-s carry only what is taxable to an AL
  RESIDENT. The reporting session's probe moved TWO variables in one
  edit and it RETRACTED (durable copy in the BATCH-296 annex; the
  states lane's STATUS corrected, D-45 addendum). Built the surviving
  half: per-person `ret-exempt-t`/`ret-exempt-s` seeded + staged +
  documented; D_AL40NR_RETIREMENT_PLAN_TYPE names WHO; legacy
  `ret-exempt` honored as the single-filer shorthand. Mirrors the states
  lane's same-evening RS per-taxpayer amendment (cached
  `al_form_40nr_spec.json` re-exported: 33 facts / 17 spec tests).*
- *Gates: 11 new tests (`test_batch296_tail_s294.py`), injection-proven
  (6 red under 5 injected defects); 685 green in combination incl. flow
  assertions; the 2 combined-run s258 fails = the KNOWN quintet leakage
  (isolated 8/8 green — non-implication proven).*

*▶ NEXT — session-3 worklist by measured r5 yield (unchanged): ③
dependents (p1 grid + 8867/Sch EIC) · the USSTB int/div detail parsers
(per-payer TSJ — also the exit from the mixed-split refusal) · the
~4-packet sub-threshold int/div class with NO pages (likely a Ken
question) · then page-coverage units by blocked-count: f8995 128 ·
sch_d/8949 83/59 · sch_e 71 · sch_c 60 · asset_detail 60 · sch_a 45 ·
k1_detail 44 · f8889 30 · est_payments_wks 27. THEN the 50-of-John's
pilot (101 JOHN packets in the Inbox). Then Ken-directed: EIC derive
(probe-first) · AOTC picker. Then defects by cost: item-59 −487 residual
+ S1-13 double-landing (hold b926) · item 84 (§469(i)) · the two
4952/GA-RIE-owner items · #60 · #43-medium · #70 · #16.*

*Entry lane: on the Lacerte sub-queue. After the s294 deploy is LIVE it
will re-key the AL-40NR client's plan-type basis as `ret-exempt-s: true`
(economics unchanged, tax ties at 174). Its two BATCH-296 tail items are
closed by this session's annex. ⚠ Lacerte prints defeat `tools/face.py`
(the lane wrote `lacerte_face.py`); Lacerte exports carry NO DOBs / NO
payer EINs; DOB list tool columns are SPOUSE-FIRST. PipelineOut r2-r5 +
shells-index.json are PII working files.*

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
- s294 deploys: `ce04ad1` → `dep-da74u6hsrm7s73bdht60` and (s294b)
  `2ce1dfa` → `dep-da754be7bikc73fgk6r0`, both **LIVE, API-confirmed
  2026-08-26**; post-deploy verify: all three ret-exempt* lines present
  in the prod DB (read-only probe).
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any session that touches
  lane vocabulary, allowlists, or a state seeder MUST regenerate the
  published schema as a close-out step (bit in s294: the entry lane's
  local validator couldn't see the new lines because the schema predated
  the deploy — a generator nobody runs is the s289b
  sweep-that-checked-nothing family).

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. s294 coordinated live: the
states lane was MID-RUN when this session claimed test_postgres (collision
= 8 contention errors on their side, no defect either side) — the claim
message crossed their run; ask-then-wait beats claim-then-run. The
peer-contradiction protocol worked end to end: states lane flagged two
sessions' opposing claims instead of silently taking the newer one; the
original claimant re-checked its own payload file and retracted.
Peers stage; Ken decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s294)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof. (Bit again in s294's
  combined gate run — 2 s258 cleanup fails, isolated 8/8 green.)
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s290; no client changes s291-s294).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): source-inspection flow assertions slice the NEW file with the
  OLD import's line numbers — five phantom failures. New files/markdown/
  scratchpad only during a run.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c` (one-liners only).
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
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294, the
  ret-exempt retraction): the reported suppression compared arms differing
  in ret-t AND the flag, and the no-move arm had its own legitimate cause
  (the $6,000 age-65 exclusion zeroing a 3,181 carry). "Probed, not
  inferred" on a two-variable experiment is MORE credible and LESS
  reliable than an inference — isolate the variable or don't write
  "probed".

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
- (s294) A state face left in place by an omitting correction batch is
  NOT recomputed against that batch's federal changes — visible via the
  live-face reconciliation + the named warning, but if a stale-face
  pattern shows up in the lane, an auto-refresh question goes to Ken.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried:
Everything from s277–s292 stands (R-K1-20N; R-GA700-PARTNERS; AL_FORM_40NR
amendments; R-AL-TAX; R-B1/B2-AUTO; the 4562 line-17 reversal;
R-8880-LINE1-COMPOSE; FORM_7203 Part I line-3 sub-map; R-K1-179-BASIS;
R-GA500-RIE loss-allocation clause; SCHEDULE_K1 box 16 A/B routing;
R-8995-QBI line-1 population widening; FORM_8960 R-8960-INCOME 5b
description; the unfloored line5 §1211(b) question). **s294: the states
lane's AL_FORM_40NR per-taxpayer retirement amendment is SEEDED in RS
prod (33 facts / 17 tests incl. AL40NR-H2) and the app's cached spec +
vocabulary now mirror it — nothing newly queued from this side. Late
s294: the states lane's Ken-directed 1040X derived-input amendment is
seeded (RS `c0dd903`: `x_baseline_captured` REMOVED — system state, the
app's resolver already proves the baseline; `x_is_superseding` → derived
via new `x_extension_filed` + `R-1040X-SUPERSED`); cached
`1040x_spec.json` re-exported (11 facts / 13 rules), neither removed key
was read by app code. ⚠ QUEUED app follow-up: if/when the amendment lane
models superseding returns, implement `x_is_superseding_derived` per the
spec's shape — preparer supplies only the extension fact, the engine
derives against the due date.**
