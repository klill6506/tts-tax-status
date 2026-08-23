# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-23 ~16:30 (s278 close — the two entity queues Ken
directed at s277 close are CLEAR; three verified deploys).*

*⚠⚠ RESUME POINT — **s278 SHIPPED THREE VERIFIED DEPLOYS.**
**1120-S BATCH-014 CLOSED** (`1634c4b`, deploy `dep-da5klprl550s738d5vu0`
LIVE; annex appended; file moved to Done): #1 a bulk-sale member's
business-use % business-adjusts its GROUP basis on all three arms
(the 66.70% pickup — keyed cost_basis/amt_cost_basis are already
business-adjusted and net of history: never gross, never multiplied
twice; K15b 2,838 → −22,318 tied to the dollar BEFORE building); #2 §179
cancels BEFORE the GA-600S depreciation pair is built in BOTH
presentation modes (the election rides K11 and was never in the entity
income base — component-net had manufactured 31,128 of GA income from a
stale pre-conformity register election; S6_11 mode invariant is
structural again). ⚠ Entry lane must re-stage packet 170 with
`return_info.ga_depreciation_presentation: "aggregate_gross"` (its face
is the gross pair) — said in the annex.
**1065 BATCH-004 STANDS 9/10**: #5 (`5335871`, migs 0349+0350 — the
lower-tier K-1 row family; leg 1 = the K11 COMPONENT REGISTRY, the s230
duty discharged, the #6 keyed-K11 blank-stomp adjacent defect fixed, the
mixed −4,512 tie pinned; §1231 → K10 with no fabricated 4797; new
D_LT_NONDED seed line → K18c; K6b/K13b/K18b/K18c join the answer key)
and #7 ledger-only v1 (`5decfa1`, migs 0351+0352 — Form 8990 Schedule A
EBIE carryforward ledger, prior 57 → ending 57 persists independent of
tax effect; nonzero current-year refuses by name, spec-first). All
migrations + the seed applied to the shared DB pre-push; published
schema regenerated post-deploy (lower_tier_k1s + form_8990_schedule_a
advertised). Annexes appended to the batch file.*

*▶ NEXT: **#4 is BATCH-004's only open item — BLOCKED on Codex's Box-2
statement arithmetic** (question + re-ask in the annex). Queues swept
this boot; nothing else posted. Then per BUILD_ORDER. Also queued: the
GA six-re-stage dry-runs (KEN'S CALL); the staged-field-with-no-consumer
tripwire design chip; the RS agenda additions (8990 spec re-authoring
now has a live consumer; 4797 spec silent on business-use member basis).
Awaiting from entry: the staging-guard fixture shapes; the packet-170 +
investment-partnership re-stages on the new surfaces.*

*⚠ s278 lesson worth the pointer: the PS5.1 regex-rewrite ban was
violated once (mojibake in backentry_entity.py) — caught immediately by
grepping the diff for `â€`, restored from HEAD, edits redone with the
Edit tool. The ban is load-bearing; see the s278 memory file.*

*⛔ KEN — parked with him (carried): (1) clients 4387 + 3199 W-2 box-3/5
rounding-band acknowledgments; 3199 also needs 2020-2024 prior-year
files for the §1231 lookback. (2) The 2665 Notice 2008-1 question.
(3) The six-re-stage dry-run decision (GA ordering-bug residual class).
(4) Carried: #8 GA-700 Sch 4 scope; #6 1065X/AAR; #68 optimizer "best";
s274 PII items; the RS 8990 re-authoring gate.*

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
authorization (Ken 2026-08-23, DECISIONS.md): push at own judgment; verify
every deploy; hold only for a named reason.**

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** Coordination:
ListAgents + SendMessage — ⚠ the channel DROPPED a whole morning of
messages both ways (s277): anything load-bearing goes in batch-file
annexes too. GATE EXCEPTIONS stay human end-to-end IN the acting session.
**Never relay tokens through the message channel.** ONE delvio-tax tree
holder; ONE pytest/test_postgres holder (s278 held both). delvio-states:
authoring VA_500 → AZ_120/AZ_120A in RS, gated, no prod writes, no
test_postgres (two courtesy notes received s278; AZ bonus = 0% every
vintage is on the board for any future AZ app build).

## ⚠ Known red / rotted — THE ONE LIST (post-s278)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` on the affected files = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔
  KEN s217).
- **Client typecheck**: green (`npm run typecheck`).

### ⚠ Test-run hazards (standing)
- One shared `test_postgres` (RS suite included). Long runs DETACHED.
  Never pipe pytest through `Select-Object`; redirect to a file.
  `poetry run` only from `server\` (the Bash tool silently prints
  NOTHING for poetry one-liners — use the PowerShell tool).
- ⚠⚠ PS5.1 encoding traps (s275, RE-PROVEN s278): regex-replace file
  rewrites BANNED — one slipped through and mojibaked
  backentry_entity.py (HEAD restore). Edit tool or `[IO.File]` BOM-less
  UTF8; never `Set-Content`/`Get-Content -Raw` for UTF-8. After ANY
  shell touch of a source file, grep the diff for `â€` markers.
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.

## 🔎 Carried for triage — NOT claims
- (s268) 1,604 queries/run; + `credit_limit_worksheet_b` ×2/pass (s275);
  + the Part III SDTW re-run per pass (s276); + `iterative_sehi_expected`
  re-runs the Pub 974 loop at composition (s277) — all memoization
  candidates if the s268 unit lands.
- (s241) `Form8606`/`HSAAccount` duplicate owners · (s234) the $250k
  nonpassive K-1 AGI gap · (s274) the shared-policy pair 8962 fixture ·
  (s275) `.first()`-on-per-form-rules sweep chip (task_b1fd177f — MINE,
  not the states session's).

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried + s278 additions:
Everything from s277 still stands (500 spec: line-18/20 override
convention + line-26 derive + RIE dividend netting; SCHEDULE_K1_1065
box-13 vocabulary; the 6251 D_6251_005 courtesy pass queued with states;
1065 K-1 box-15 letters URGENT), plus s278: **the 8990 spec re-authoring
gate now has a live consumer** (the #7 full unit waits on it — the
ledger v1 is its input surface); **the 4797 spec is silent on
business-use member basis** (BATCH-014 #1's convention — business
portion of gross, whole-dollar, before historical reductions — should be
seeded as a rule); SCHEDULE_K1_1065/1065 spec has no lower-tier K-1
row-family vocabulary (the #5 build is ahead of it).
