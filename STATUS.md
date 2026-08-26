# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-26 (s297 — extractor session 4, unit ① + the tie
probe's finding built same-session. ① **Schedule 1 face parser +
line-8/10 component routing** (`401ec6e`): both pages parse positionally
(⚠ TaxWise RE-TYPESETS Schedule 1 — packet marker geometry, never the
blank template's); feeds line 1 → the #30 `sr_filed_taxable_refund`
valve, 11/20/21 + 19a/19b/19c + 24z/24z_type → `sch1_fields`, 8h/8v/8z →
`other_income_items`; 8b = consistency vs extracted W-2Gs; every other
component refuses by letter/name. ② **the p2 TAX-BLOCK face gates** —
the r10 tie probe caught one packet tying AGI/TI but no_tie +263: the
filed face took a $263 Schedule 3 credit (line 20, de-minimis-FTC
shape) and TaxWise printed NO Schedule 3 page. Same class as another
packet's missing sch1_p2 behind a filed line 10. The face is the
guard (s293 principle): p2 now captures 17-21/23/25c/26/27a-31/32 with
refusal-grade identities; nonzero 17/20/23/25c/26/29/30/31 refuse by
name; 19/27a/28 stay engine-derived (an existing emit's CTC ties). **r11
census: 11 emitted / 256 refused** (was 9/258); emitted-set tie 11/11
(rolled back — payloads byte-identical to the probed set). 107 extractor
tests green. No vocabulary change → published schema deliberately
untouched.*

*⚠⚠ s297 measured re-rank (post-build census, the only current one):
**f8995 touches 128 packets (4 solo — the no-business REIT-dividend QBI
shape)** > sch_e 71 > sch_c 60 = asset_detail 60 > **f8949 59 touched, 9
SOLO — the top measured solo builder** > k1_detail 44 > f8889 30 > 7203
29. The s296 "13b deduction 9-solo" collapsed to 1 solo (upper-bound
lesson, third time). int/div face classes: 2b 14 touched/4 solo, 3b 9/3
— still Ken-gated (REVIEW_QUEUE s295, narrowed). The s297 finder packets
are identified by name in the r10/r11 run logs under PipelineOut (PII
stays there, never here).*

*▶ NEXT — extractor session 5: ① **f8949 summary rows →
`capital_transactions`** (9 solo, top measured class; pairs with sch_d).
② the f8995 4-solo REIT shape (likely cheap: 199A REIT dividends ride a
documented aggregate — verify vocabulary before building; marking the
page ignore-when-derivable needs the QBI decomposition guard). ③ probe
whether `ctc_ext_carryover_wks` (11 touched) is derivable → role ignore.
⛔ Ken: the narrowed int/div payer-less classes. THEN the 50-of-John's
pilot (11 tie-verified emits already banked, batches r11-001/002 in
PipelineOut). Then defects by cost: item-59 −487 residual + S1-13
double-landing (hold b926) · item 84 (§469(i)) · #60 · #43-medium · #70
· #16.*

*Entry lane (peer, confirmed in-session s297): still BLOCKED on Ken's
auth mint; THREE payloads queued — L012 (re-keyed subject_to_se), L013
(5329 waiver; schema re-pull ALREADY done, row passes validation), L014
(first itemizer + 2210). ⚠ Their line-37 open question was SETTLED
s297: R-REF-03 (compute.py "37") already includes the line-38 penalty
per the 2025 i1040 verbatim — Lacerte's printed 1,863 will tie; their
annex updated to CLOSED with citation. Item B (joint-interest $1) is
CLOSED BY RULING (tie_with_exception), not by code. Two of their packets
sit with Ken (one has no seeded shell; one needs a dependent birth year
that 41 packets share) — named in their BATCH-296 annex, not here.*

*States lane (peer, confirmed in-session s297): R-5329-11 **verified
against all three authorities** (their durable doc: delvio-states
`research/f5329_part9_waiver_verification.md`, commit `613264c`) —
staged for Ken, NOT authored (Gate-1 is his). ⚠ Their finding: i5329
p.10's line-54a sentence is STALE (describes a shortfall; the face
computes tax) — the shipped s296 app matches the face+XSD, no app
change. R-SE-L2 verification not started; the 1065_SE consistency check
queued behind it.*

*⛔ KEN remaining: **the s295 int/div ruling (narrowed)** · the auth mint
(blocks the entry lane) · the entry lane's shell + birth-year packets
(their annex) · #21,
#48 (RS 404), #56, #63, #69, #10 — the tail tier. Carried: entity
second-state-face transport (#3); `OVERRIDE_HONORED_STATE_LINES`;
146-packet re-export; NC/CA/SC linked-state reopens; #6 1065X/AAR; #68
optimizer; s274 PII narrowings; RS 8990 re-authoring gate; 6765 Sec G;
client-4545 D_8606_BASIS_ONLY; 1065 BATCH-004 #4 (Codex Box-2);
Analysis line-2 active/passive proxy; the unfloored 8960 line5 §1211(b)
question; the s292 PII-history scrub question; dep_released_by_form_8332
build (REVIEW_QUEUE).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.
  (The 2026-08-24 refinement covers COMMIT MESSAGES only — and even there BUSINESS names
  only; personal surnames are not permitted. ⚠ s296's and s297's pushed commit messages
  and two parser docstrings carry packet surnames — recorded in REVIEW_QUEUE with the
  s292 history-scrub question.)

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
- s297 deploy: `401ec6e` → `dep-da7fq86gekts73c3uc60` **LIVE,
  API-confirmed 2026-08-26** (extractor scripts + tests only; no server
  code, no migration, no seeder; the same-day scrub commit follows it).
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any session that touches
  lane vocabulary, allowlists, or a state seeder MUST regenerate the
  published schema as a close-out step. (s297 touched NONE — schema
  unchanged by design.)

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. s297 coordinated at boot:
both peers confirmed tree + test_postgres mine; the entry lane's queue
state + the line-37 settlement recorded in their annex. Peers stage; Ken
decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s297)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s296; s297 touched no client code).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): five phantom failures. New files/markdown/scratchpad only.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c`. ⚠ `python -m taxwise1040` does NOT
  resolve from `server\` — run the package's `__main__.py` BY PATH (s297).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ Embedded double-quote in a here-string
  arg to a NATIVE exe SPLITS the argument. ⚠ `$1` in an unquoted PS arg
  EXPANDS to empty. ⚠ PS `Sort-Object -Unique` on a one-element pipeline
  UNWRAPS to scalar (s296). ⚠ This machine's Git Bash lacks `cat` on PATH
  (s297) — heredoc-append via `cat >>` silently no-ops; use the Edit tool.
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); synthetic `insert_text` PDFs may
  LOSE leading spaces (s290) — the extractor parses POSITIONALLY everywhere.
  ⚠ **The AcroForm FILLER flattens ALL widgets out of its output** (s296).
  ⚠ **TaxWise RE-TYPESETS schedule templates** (s297): packet marker
  geometry ≠ the blank IRS template's — pin from packets only.
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section. Local templates beat both.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292-s297 ran whole tie probes this way). ⚠ Scripts
  touching client-named returns live in SCRATCHPAD or tax-test-data, never
  the repo (PII). ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295/s296/s297
  — THREE consecutive sessions): depth-probe AND check uncovered pages
  before building a unit; a page can be MISSING behind a filed face line
  (a missing sch1_p2, a missing Schedule 3) — the FACE is the guard, and the
  emitted-set tie probe is what finds the silent-no_tie class.

## 🔎 Carried for triage — NOT claims
- (s297) The X mark printed at (474.7, y≈389) in the 1040 p2 EIC row
  region on a no-EIC packet — some line-27 checkbox; unidentified,
  currently ignored by the parser; identify when an EIC face divergence
  ever appears.
- (s297) `est_payments_wks` (27 touched) and `f8863` (6) now ALSO gate at
  the face (26/29) — when their page-parsers are built, the face gates
  become consistency checks like 8b.
- (s296) The 22 sch_d GEOMETRY-error packets from the calibration sweep
  refuse loudly by design — diagnose when one is otherwise free.
- (s295) The Inbox holds 7 auxiliary PDFs that refuse as "not a TaxWise
  1040 packet" — correct; consider a sub-folder so the census denominator
  is returns-only.
- (s295) `_summary_lines` GA500_SUMMARY_LINES lacks S1-6 — GA Sch 1
  additions tie only indirectly.
- (s290) The GA RIE interest row does NOT include K-1 16A tax-exempt
  interest — stated boundary. · The rendered 8995 TIN prints UNFORMATTED
  digits — cosmetic.
- (s289) `IndividualForm7203` has no §179/charitable carryover keying
  fields — D_K1_7203_DEDUCTION_LIMITED warns. · The 7203/K-1 §179 cap does
  NOT extend to 1065 partners.
- (s288) `IndividualForm7203` still has no home for box 16 code E;
  1065 box 18 a/b/c have none on the recipient side.
- (s288) `ctc_override` / `odc_override` + `Dependent.compute_qualifies_*`
  are DEAD — removal candidate (Ken's call).
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
Everything from s277–s296 stands (R-K1-20N; R-GA700-PARTNERS; AL_FORM_40NR
amendments; R-AL-TAX; R-B1/B2-AUTO; the 4562 line-17 reversal;
R-8880-LINE1-COMPOSE; FORM_7203 Part I line-3 sub-map; R-K1-179-BASIS;
R-GA500-RIE loss-allocation clause; SCHEDULE_K1 box 16 A/B routing;
R-8995-QBI line-1 population widening; FORM_8960 R-8960-INCOME 5b
description; the unfloored line5 §1211(b) question; the 1040X
derived-input amendment + queued `x_is_superseding_derived` app
follow-up; R-SE-L2 clergy + SE-subject-other-income addends — with the
states lane, verification NOT started; **R-5329-11 waiver clause —
VERIFIED by the states lane s297 (their research doc is the citable
record), staged for Ken**).
