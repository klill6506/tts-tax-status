# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-25 (s289 full day — four BATCH-296 items + GA-500
quantize deployed; eleven Ken rulings; four EIN client-pair merges; the
import-at-scale PLAN approved; front-desk temporary-client SERVER side
live).*

*⚠⚠ THE APPROVED PLAN (Ken, 2026-08-25 —
`C:\Users\Ken\.claude\plans\1-i-assume-the-calm-shannon.md` is the full
text): **① Front-desk temporary clients** — SERVER SIDE SHIPPED s289
(commit `251d0f9` LIVE, migs clients.0012 + firms.0006 applied): FRONT_DESK
role; `desk-search` (name OR last-4, both identity stores); `desk-create`
(409-with-candidates until confirm_new; is_temporary + desk_ssn_last4);
`temporary` queue; PRIMARY-identity capture auto-clears the flag in
`identity.upsert_identity`. ⚠ REMAINING: the React desk screen, and the
FRONT_DESK endpoint-lockout decision (UI-only today — DEFERRAL_AUDIT s289;
build the lockout BEFORE any desk login is issued). **② The 1,700-return
import**: bulk path DOES NOT EXIST — build the TaxWise packet→payload
extractor (per-form extractors + summary-page `expected`; refuse-don't-
guess), pipeline extract→validate→stage→dryrun→TIE→**AUTO-COMMIT +
MARK-FILED (Ken-approved)**; NO_TIE → per-preparer exception inboxes;
pilot = 50 of John's book; QA trimmed to the tie record for pipeline
returns (Ken-approved). **③ Ken's side**: export the 282 Lacerte 1040s
(⚠ the two existing Lacerte-prepared packets were NOT exports — no print
setting is established; complete-return print preferred, the pre-indexer
handles bloat) + the 15+2 faceless TaxWise re-exports. Lacerte 282 go
through the CAREFUL QA lane, never the pipeline.*

*Also s289: the four same-EIN duplicate client pairs MERGED (Ken-ruled;
zero EIN dupes remain; snapshot in D:\tax-test-data; ⚠ four client numbers
died — cross-lane refs corrected additively by the entry lane). The SSN
sweep found NO true SSN duplicates (5 hits = 3 separate-filing couples);
2,528 of 3,793 clients carry NO identity number yet — the fuzzy-name
backstop matters until returns are keyed.*

*⚠⚠ RESUME POINT — **nothing is mid-build.** s289 shipped FOUR verified
deploys (`12d736f`, `4cbf5bd`, `18a1a87` — all Render-API-confirmed live):
**① #13** — Form 7203 §1366(d) basis cap now limits K-1 §179 (41c+41d →
Schedule E col (j), print/MeF/MAGI, and the 8960 line-4b back-out) and
charitable (42c+42d, pro-rata by §170(b) bucket → Schedule A) via the shared
`k1_7203_result()` access point; new warning `D_K1_7203_DEDUCTION_LIMITED`
(seeded, verified); no carryover keying fields exist yet (DEFERRAL_AUDIT).
**The Overstreet acceptance TIED first attempt in production and is FILED**
(entry lane, return 5fd7bc12). **② #72** — the seven Simplified Method
INPUTS admitted to `r_1099s` (start date = engage switch); sm_* outputs
refuse by name. ⚠ Worksheet base is BOX 1 GROSS (Pub 575 line 1) — the
Gregory packet may show a 554 = box1−box2a residual; if so it goes to Ken
as a CSA-1099-R base ruling, not a re-key. **③ #73** — Sch 1 line 24z +
24z_type join the lane (19a/b/c precedent; literal writes VERBATIM; stage
warning on amount-without-literal). **④ #57** — a NEGATIVE 1040 line 7 now
allocates the GA-RIE capital loss by LOSS ownership (loss rows + tagged
carryovers), never by gain weights; joint-loss 50/50 (#78) survives.
⭐ #57 WAS ALSO item 59's $17: an instrumented rolled-back dry-run of the
staged item-59 return tied at 4,860 exactly — the "taxpayer interest leaks"
theory was a numeric coincidence (the spouse's half-weight of joint
capital-gain distributions × the −3,000 loss = −17.27). **Rider:** GA-500
S3-9 quantizes to 4dp before multiplying L12 (RS `500` R-GA500-S3, live in
RS prod today; T8b scenario pins 1/7 → 0.1429 → L13 1,715).*

*⚠⚠ s289 LATE — KEN RAN THE DECISION QUEUE LIVE: ELEVEN RULINGS (full
ledger at the top of `REVIEW_QUEUE.md`). The directed-build list is now
the lane's spine. ▶ NEXT (plan work first, then Ken-directed, then defects
by cost): **⓪a the desk UI screen + FRONT_DESK lockout** (plan ①'s
remaining half) and **⓪b the extractor build** (plan ② phase B — 2-3
sessions, then the 50-return pilot on John's book).
**① K-1 box 16A → 1040 line 2a** (Ken: YES — verify i1040 2a text, then
the k1 addend into compute_intdiv; moves §86 taxable SS). **② retire
D_1040_008 + drop RS DG-4** (one amendment). **③ Form 8995 line-1
business table** (punchlist 18 = LOMAX #2970, DIAGNOSED: QBI computes,
the 1i–1v name/TIN/QBI table is never populated — fill from the QBI
sources, 5-row cap + overflow statement, check the MeF group). **④ the
[client] residual** — #57 CONFIRMED by the item-59 return (tied, filed)
but [client] still NO_TIE: SP-17 6,027 vs 6,514 (−487; pre-fix was 5,014,
so #57's −1,500 is gone and a second defect UNMASKED) + the S1-13 delta
of exactly the derived SP-17 (double-landing observation; hold b926).
**⑤ item 84** (§469(i) $25,000 allowance never applies —
active_participation flip changes nothing; batches 9e7c7eff-f556-49ac-
a9d4-1970a56bb551 / 5af50f2c-38ef-4e5a-8b92-1c594addf3f2). **⑥ the two
NEW Houston items** (posted to BATCH-296 by the entry lane): 4952 line
4e/4f — engine treats net capital gain as investment income without the
election (deducts 5,206 vs filed 3,783; scha_investment_interest not
consulted), and capital_transactions.owner IGNORED in the POSITIVE-line-7
GA RIE split (the positive-side mirror of #57 — owner flip byte-identical,
probed; also the div aggregates carry no owner → per-row attribution
needed on joint GA returns, docstring ask). **⑦ EIC derive unit**
(probe-first, Ken-ruled) and **⑧ AOTC picker**. Then #60, #43-medium,
#70, #16. ⛔ KEN remaining: #21, #48 (RS 404), #56, #63, #69, #10 — the
tail tier, none urgent. CLOSED by ruling: #18 (Roy $486 stands), #28
(advise, no amending), item 85 (engine right), 2961 (as filed 43.18% —
the AL 40NR scenario-G/H AWAITING item is RESOLVED; scenario re-base on
the RS agenda), Houston (shell seeded onto EXISTING client 2576, return
a4166a10), 4167 (decompose first — with the entry lane).*

*Entry lane (same-day): released ~60 stale holds off my annex answers;
BATCH-012 Ellington committed/filed via the `form:"attached"` source-defect
arm; Overstreet committed/FILED; [client] + [client] retests requested
post-#57. The 37-packet "filed 2210 penalty, no worksheet" class: ruled
usable via the `t2210_penalty_source_*` trio, but DRY-RUN FIRST (±$5 line-38
tolerance + the new §6654(e)(2) waiver may tie bare). Token: Ken minted via
the new `server\scripts\mint_entry_token.py` (dev account, 15-min TTL).*

*States lane (same-day, RS repo): authority-ownership assessment + guard
widening (A1) + invalid source_type re-seed (A2, Ken-approved) all landed;
delvio-tax verified NOT coupled to RS `source_type` (ours is ScheduleK1's
entity kind). ⚠ RS `seed_all` now REFUSES on enum-ratchet growth — Ken's
call to raise, never a baseline bump in passing.*

*⑥ Gates: every new assertion defect-injected (10 injections across the
four items — 10/10 caught red); 815+79+543+713+629 green across the
touched lanes incl. flow assertions each deploy; published back-entry
schema regenerated (advertises 24z/24z_type/sm_* inputs); `check_rule_paths`
clean post-#13; client typecheck untouched (no client changes).*

*▶ AWAITING: client-4167 NIIT decomposition (entry lane produces the
line-by-line 8960, then Ken rules). Everything else formerly here is
RESOLVED by the s289-late ruling run — ledger at the top of
REVIEW_QUEUE.md. Ken also REPRINTED the ~140 faceless returns; the entry
lane re-screens that inbox class.*

*⛔ KEN — outstanding (carried): entity second-state-face transport (#3);
`OVERRIDE_HONORED_STATE_LINES`; 146-packet re-export; NC/CA/SC linked-state
reopens; #6 1065X/AAR; #68 optimizer; s274 PII narrowings; RS 8990
re-authoring gate; 6765 Sec G; client-4545 D_8606_BASIS_ONLY; 1065
BATCH-004 #4 (Codex Box-2); Analysis line-2 active/passive proxy.*

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
hold only for a named reason.**
⚠⚠ **ORDERING (s279/s282): push → deploy LIVE → seed → verify — and the
deploy ITSELF seeds (`build.sh seed_all` auto-discovers `seed_*` at BUILD
time). Manual post-deploy seed = the idempotent VERIFY; `check_rule_paths`
is one command.**

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — **s289: coordinate EXPLICITLY before every run; one collision
happened anyway (states' two-file run vs my idle connection — harmless, but
the ask-first reflex is now bilateral).** Peers stage; Ken decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s289)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s288; no client changes s289).

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
  bash heredoc (used all session, s288/s289).
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); a field map guessed from SHORT
  widget names silently no-ops (s287).
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). A STRING-COUNT question is checkable; "quote it exactly" is not.
  Local templates beat both.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces a staged return's
  production behavior locally** (s289): `transaction.atomic()` +
  `commit_staged_return` + read the FFVs + raise — the same operation the
  entry lane's prod dry-runs perform; nothing lands. ⚠ Scripts touching
  client-named returns live in SCRATCHPAD, never the repo (PII).

## 🔎 Carried for triage — NOT claims
- (s289) `IndividualForm7203` has no §179/charitable carryover keying
  fields — D_K1_7203_DEDUCTION_LIMITED warns; DEFERRAL_AUDIT has the build
  trigger. · The 7203/K-1 §179 cap does NOT extend to 1065 partners
  (stated boundary — §704(d) worksheet caps only the asserted Sch-E loss).
- (s288) `IndividualForm7203` still has no home for box 16 code E;
  1065 box 18 a/b/c have none on the recipient side.
- (s288) `ctc_override` / `odc_override` + `Dependent.compute_qualifies_*`
  are dead to compute — only `D_1040_008` still reads them.
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
  WEIGHTS (capital_transactions + div 2a only) — a K-1-gains-only MFJ
  return falls to the carryover/50-50 fallback; pre-existing, noted.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried + s289 adds:
Everything from s277–s288 stands (R-K1-20N; R-GA700-PARTNERS income base;
AL_FORM_40NR amendments; R-AL-TAX mechanism; R-B1-AUTO / R-B2-AUTO; the
4562 line-17 reversal; R-8880-LINE1-COMPOSE; FORM_7203 Part I line-3
sub-map; SCHEDULE_K1 box 16 A/B; 1040_SPINE DG-4). **s289 RESOLVES one:
GA-500 S3-9 now implements the live R-GA500-S3 (4dp ratio) — off the
agenda. s289 ADDS: (a) `R-K1-179-BASIS`** — the SCHEDULE_K1 recipient
spec routes §179 to Schedule E col (j) with no §1366(d) interception and
no charitable cap either; the 7203 spec's R004 carries the allocation but
nothing ties the two specs' flows together, which is how the consumers
drifted unwired. **(b) R-GA500-RIE loss-allocation clause** — the spec
says "jointly-owned income splits 50/50" but is silent on how a NEGATIVE
netted line 7 allocates; the s289 rule (by loss ownership incl. tagged
carryovers) should be written into the spec so it can police it.
