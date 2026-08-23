# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-23 ~14:00 (s277 mid-day — Ken active; standing push
authorization ruled this morning, every push Render-API-verified).*

*⚠⚠ RESUME POINT — **s276+s277 SHIPPED TWELVE VERIFIED DEPLOYS TODAY.**
s276 overnight (③ units): 6251 Part III SDTW (`2210c69` — the EIGHT
D_6251_005 holds ALL cleared in production); shell-lookup city/state +
cleanup unknown-key 400 (`129daab`); Schedule A dropdown (`44e9511`).
s277 (Ken present): **MFJ itemizer $600** ruled+built (`dcd35f4`,
verified on a real return to the dollar); **1065 BATCH-004 #9+#10**
(`1e2b821` — K15f answer key + schema parity-by-declaration); **#1+#8
AJ/AN statement trios** (`316f0e3`, mig 0340); **Path2College
per-beneficiary cap** ruled+built (`ae3c794`, mig 0341,
`ga_529_beneficiary_count`; Ken: beneficiary need NOT be a dependent —
no per-return ceiling); **iterative-SEHI e-file** (`33514d9` — composer
omits IRS7206, proves line 17 vs re-derived Pub 974; 1917/2065 closed
with ledgers); **GA-500 lines 18/20 honor overrides** (`4099d75` — the
vanishing other-state credit; 1569 Done); **1065 #3 K13e coded
deductions** (`658cb9e`, migs 0342+0343 — entity rows compose, box13
partner categories, D_K1_13E, pro-rata suppression); **SEHI-BASE
persistence** (`e284532` — the S-corp K-1 SEHI boundary; 4006/2665
unblock on recompute); **the GA pair** (`b68c5bd`, mig 0344 — RIE
dividend-line US-gov netting via `div_1099s[].us_government_income`
(client-2552 class, additive, item-14 pins prove the old path
byte-identical) + line 26 derives from dated GA estimate rows). All
annexed (BATCH-296 entry-channel items A-E + the BATCH-004 annexes);
both published schemas regenerated post-deploy each time.*

*▶ ENTRY-LANE STATE (they report): SEVENTEEN packets to Done today;
2552/2761/2887 re-staging on the just-live RIE dividend fix;
4006/2665 recompute+cleanup on the SEHI-BASE fix. The lost-messages
gap: the session channel dropped a whole morning BOTH ways — anything
load-bearing now ALSO goes in batch-file annexes.*

*⚠⚠ ▶ FIRST AT NEXT BOOT (Ken-directed at s277 close): **WORK THE TWO
ENTITY QUEUES BEFORE ANYTHING ELSE.** (1) **`1120S\CC Changes\
CC_CODE_CHANGES_1120S_BATCH-014.md`** — posted 2026-08-23 13:08, NEVER
SWEPT (it landed after the s277 boot sweep; contents unread — full
verify-first triage from scratch). (2) The open **1065 BATCH-004
remainder** (7/10 done): #5 (lower-tier family; LEG 1 = the K11
component-registry conversion per DEFERRAL_AUDIT), #7 ledger-only v1,
and #4 if Codex has posted the Box-2 statement arithmetic answer in the
annex. Then the rest of NEXT below.*

*▶ NEXT (build queue): **#6 and #2 SHIPPED in the afternoon**
(`a6749e6` PTP→K11 routing + D_K1_11, migs 0345+0346; `d35004e`
Statement A per-activity rows, migs 0347+0348 — SEVENTEEN verified
deploys on the day; BATCH-004 stands 7/10). Remaining: **#5** (the
lower-tier K-1 family — its LEG 1 is the K11 component-registry
conversion, DEFERRAL_AUDIT; fresh-session-sized), **#7** (ledger-only
v1 queued; the FULL 8990 unit is spec-first — the RS spec is a
draft-fraction, the s238 trap class), **#4 BLOCKED on Codex** (the
Box-2 statement arithmetic — question in the BATCH-004 annex; sign
conventions are not guessable). Also queued: the GA commit-ordering
fix's residual class (six re-stage dry-runs — KEN'S CALL, flagged not
run); the staged-field-with-no-consumer tripwire design chip. Awaiting
from entry: the staging-guard fixture shapes (real fixtures only).*

*⛔ KEN — parked with him: (1) clients 4387 + 3199 W-2 box-3/5
rounding-band acknowledgments (re-export the W-2 faces or acknowledge
with the band); 3199 also needs 2020-2024 prior-year files for
the §1231 lookback. (2) The 2665 Notice 2008-1 question (box 1 does not
exceed box 5 → deduction may be unsupportable; entry session asked him
directly). (3) Carried: #8 GA-700 Sch 4 scope; #6 1065X/AAR; #68
optimizer "best"; s274 PII items.*

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
annexes too. GATE EXCEPTIONS stay human end-to-end IN the acting session
(the entry lane held the client-4175 write until Ken confirmed there — correct; and it
REFUSED a relayed credential — also correct: **never relay tokens
through the message channel**; the one I relayed was revoked at the DB).
ONE delvio-tax tree holder; ONE pytest/test_postgres holder (s277 holds
both). delvio-states: MD_500 seeded to RS prod (Ken-gated), VA LOI
package done (D-22: declare 502+502PTET only; TY2026 LOI not yet
published — weekly watch running).

## ⚠ Known red / rotted — THE ONE LIST (post-s277)
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
- ⚠⚠ PS5.1 encoding traps (s275): regex-replace file rewrites BANNED;
  Edit tool or `[IO.File]` BOM-less UTF8; never `Set-Content` for UTF-8.
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
  Inbox: 180 / 214 / pre-incorporation trailer · 170 (GA-600S §179
  HB 1199) · 17a / 17d.

## RS AGENDA — carried + s277 additions:
Everything from s276 still stands, plus: `500` spec gains the line-18/20
override convention + the line-26 derive + the RIE dividend-line US-gov
netting (the s233 rule's second application); SCHEDULE_K1_1065 spec has
no box-13 statement-category vocabulary (the K13e build is ahead of it);
the 6251 D_6251_005 courtesy pass is QUEUED WITH THE STATES SESSION
pending Ken's RS-prod gate; 1065 K-1 box-15 letters (URGENT).
