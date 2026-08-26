# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-26 (s295 — extractor session 3: Schedule A unit +
dependents unit (grid/Sch EIC/8867) + GA Sch 1 additions + Form 1310
registry. Commit `c8f4ac1`, deploy `dep-da76i38u01pc73bp61a0` (verify
LIVE at next boot if this line is not updated). Full-Inbox r8: **8
emitted / 259 refused all-named; rolled-back tie probe 8/8 TIE**. 79
extractor tests green.)*

*⚠⚠ RESUME POINT — **extractor session 4** (`scripts/taxwise1040/`).
s295 shipped: ① `sch_a.py` (67/67 corpus faces parse clean; scha_*
engine-faithful mapping — line 1 MINUS the payload's own SSA medicare
premiums (compute re-adds them); 5a + sales-tax election; closed MeF
line-16 enum, GAMBLING → scha_gambling_losses; refuse on 8b seller
identity / 8d reserved / AGI>500k SALT phase-down / forced-standard).
② `dependents.py` — p1 columnar grid (geometry pinned on 84 packets),
Sch EIC (YOB/months/student/disabled; DOB stored as YOB-01-01 with a
named warning — every engine age test is year-based), full 8867
per-question transcription → f8867_fields (blank omitted), and the
**col-(7) CTC/ODC boxes treated as the source's OUTPUT**: a
compute_8812-mirror prediction must agree or the packet refuses
(matches the RS R-DEP-03 amendment that removed the input flags the
same night — states lane relayed it live). tin_type = the serializer's
SSN-shape derivation (a blank fails EVERY child credit at commit —
backentry's own staging warning). ③ GA Sch 1 additions block: a filed
line-5 "other addition" transcribes to the app's own preparer line
S1-5 (found via a $2 GA no-tie that decomposed to a filed +49
OTHER-ADDITIONS row; lines 1-4 nonzero refuse). ④ `f1310` PageType +
ALWAYS_BLOCK_KEYS (identity-bearing pages block on PRESENCE — the
currency-token census is blind to a sub-$1,000-refund Form 1310).*

*⚠ s295 method fact: an r5 "solo-refusal" count is an UPPER bound, not
a yield — the coverage gate returns EARLY, so page-blocked packets hide
their deeper refusals (one "sch_a-solo" packet fell straight into the
intdiv class once sch_a shipped). Yield claims need the unit BUILT and
re-run.*

*▶ NEXT — session-4 worklist by measured r8 yield: ① **⛔ KEN gates the
top item** — the 18-packet no-source-page int/div class (6 solo;
REVIEW_QUEUE s295 entry has the evidence + the consolidated-row
recommendation). ② sch_d solo=4 (but schd_fields lane surface needed —
check vocabulary first) · sch_c/asset_detail 3 each · f8880 3 · f5695 3
· sch1_p1 2 (+sch1-line8 2 solo pairs with it) · f8889 2 ·
student_loan_educator_wks 2 · g1099_detail 2 · ownerless-joint 2. The 7
"not-a-packet" files are auxiliary docs (wage statements/worksheets in
the Inbox), not returns — no build. Blocked-count giants (f8995 128 ·
sch_d 83 · sch_e 71) still need multi-unit combos; re-measure after
every unit. THEN the 50-of-John's pilot. Then Ken-directed: EIC derive
(probe-first) · AOTC picker. Then defects by cost: item-59 −487
residual + S1-13 double-landing (hold b926) · item 84 (§469(i)) · #60 ·
#43-medium · #70 · #16.*

*Entry lane: the AL-40NR client re-keyed as ret-exempt-s CLOSED (their
2026-08-25 message: tie_with_exception / filed / eligible). Lacerte
sub-queue continues; their Lacerte W-2 SUMMARY finding (boxes 3/5
derivable when no elective deferral — keyed as DERIVED) noted, no code
item. ⚠ Lacerte prints defeat `tools/face.py`; Lacerte exports carry NO
DOBs / NO payer EINs; DOB list tool columns are SPOUSE-FIRST.
PipelineOut r2-r8 + shells-index.json are PII working files.*

*⛔ KEN remaining: **the s295 int/div no-detail ruling (top extractor
yield)** · #21, #48 (RS 404), #56, #63, #69, #10 — the tail tier.
Carried: entity second-state-face transport (#3);
`OVERRIDE_HONORED_STATE_LINES`; 146-packet re-export; NC/CA/SC
linked-state reopens; #6 1065X/AAR; #68 optimizer; s274 PII narrowings;
RS 8990 re-authoring gate; 6765 Sec G; client-4545 D_8606_BASIS_ONLY;
1065 BATCH-004 #4 (Codex Box-2); Analysis line-2 active/passive proxy;
the unfloored 8960 line5 §1211(b) question; the s292 PII-history scrub
question; dep_released_by_form_8332 build (REVIEW_QUEUE).*

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
- s295 deploy: `c8f4ac1` → `dep-da76i38u01pc73bp61a0` (extractor scripts
  + tests only — no server code changed).
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any session that touches
  lane vocabulary, allowlists, or a state seeder MUST regenerate the
  published schema as a close-out step. (s295 touched none.)

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. s295 coordinated at boot:
both peers confirmed the tree and test_postgres free before any run; the
states lane relayed the R-DEP-03 dependents amendment (CTC/ODC now
DERIVED, dep_tin_type/dep_citizenship_status verbatim choices,
dep_released_by_form_8332 the one app-side gap) — it shaped the
dependents unit the same night. Peers stage; Ken decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s295)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s290; no client changes s291-s295).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): five phantom failures. New files/markdown/scratchpad only.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c` (one-liners only).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ Embedded double-quote in a here-string
  arg to a NATIVE exe SPLITS the argument — use `git commit -F -` with a
  bash heredoc. ⚠ `$1` in an unquoted PS arg EXPANDS to empty —
  single-quote TaxWise code tokens.
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); synthetic `insert_text` PDFs may
  LOSE leading spaces (s290) — the extractor parses POSITIONALLY everywhere.
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section. Local templates beat both.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292-s295 ran whole tie probes this way — staging INSIDE
  the rolled-back transaction too). ⚠ Scripts touching client-named returns
  live in SCRATCHPAD or tax-test-data, never the repo (PII).
  ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection and the reconnect can flake DNS — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294) —
  isolate the variable or don't write "probed".
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295): the
  coverage gate's early return hides deeper refusals until the unit ships.

## 🔎 Carried for triage — NOT claims
- (s295) The Inbox holds 7 auxiliary PDFs (wage statements/worksheets)
  that refuse as "not a TaxWise 1040 packet" — correct behavior; consider
  moving them to a sub-folder so the census denominator is returns-only.
- (s295) `_summary_lines` GA500_SUMMARY_LINES lacks S1-6 — GA Sch 1
  additions tie only indirectly (via 16/23/30/46). Add if an additions
  no-tie ever needs direct visibility.
- (s293) The r5→r8 refusal census normalizer pattern lives in the session
  scratchpads (rewrite is ~60 lines; refused.json is the corpus).
- (s290) The GA RIE interest row does NOT include K-1 16A tax-exempt
  interest — stated boundary, not a gap. · The rendered 8995 TIN prints
  UNFORMATTED digits — cosmetic.
- (s289) `IndividualForm7203` has no §179/charitable carryover keying
  fields — D_K1_7203_DEDUCTION_LIMITED warns. · The 7203/K-1 §179 cap does
  NOT extend to 1065 partners.
- (s288) `IndividualForm7203` still has no home for box 16 code E;
  1065 box 18 a/b/c have none on the recipient side.
- (s288) `ctc_override` / `odc_override` + `Dependent.compute_qualifies_*`
  are DEAD — removal candidate (Ken's call; fold with
  `dep_released_by_form_8332`, see REVIEW_QUEUE).
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
  live-face reconciliation + the named warning.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried:
Everything from s277–s294 stands (R-K1-20N; R-GA700-PARTNERS; AL_FORM_40NR
amendments; R-AL-TAX; R-B1/B2-AUTO; the 4562 line-17 reversal;
R-8880-LINE1-COMPOSE; FORM_7203 Part I line-3 sub-map; R-K1-179-BASIS;
R-GA500-RIE loss-allocation clause; SCHEDULE_K1 box 16 A/B routing;
R-8995-QBI line-1 population widening; FORM_8960 R-8960-INCOME 5b
description; the unfloored line5 §1211(b) question; the 1040X
derived-input amendment + queued `x_is_superseding_derived` app
follow-up). **s295: nothing newly queued — the dependents unit consumed
the R-DEP-03 amendment as shipped (col-7 = derived output; verbatim
tin_type/citizenship choices; no dep_ssn_valid boolean).**
