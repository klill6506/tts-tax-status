# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-23 early morning (s275 — the overnight shift; Ken
authorized autonomous pushes at ~00:15).*

*⚠⚠ RESUME POINT — **s275 SHIPPED SEVEN QUEUE ITEMS ACROSS SIX VERIFIED
DEPLOYS + ONE TOOLING CHANGE, overnight on Ken's standing authorization**:
① **#78+#80** (`2e13d3c`, dep-da5716h5efls739gd6sg) — GA joint-split
conservation + line-5 derivation; ② **#79** (`61e2a11`, mig 0337,
dep-da57f9qd0e5s73bej5s0) — §402(l) PSO exclusion end-to-end (statute/
Pub 575 verified; $3,000 per-taxpayer cap; 5b reduction, GA RIE L12 net,
5c checkbox print+MeF, D_RET_014, both UI surfaces); ③ **#77**
(`4a33288`, dep-da57lcad0e5s73bemj3g) — GA-500 line 19 eligible-itemizer
credit derived for single/MFS/HOH full-year (O.C.G.A. §48-7-27.1 +
IT-511 pp.9/18 verbatim; min($300, line 16)); **⛔ MFJ NOT DERIVED — KEN'S
MORNING QUESTION** ("per taxpayer" reads $600 on a joint return, no
primary source states it, no MFJ fixture; entry lane asked to post any
MFJ itemizer's printed line 19); ④ **#76** (`6fceeb9`,
dep-da57u9ht0dsc73c7ekb0) — Credit Limit Worksheet B COMPUTES (B1-B14
verbatim from i1040s8; the i8839 footnote substitution; CLW-A line 4;
adoption credits now displace CTC into refundable ACTC — the fixtures'
3,400 signature reproduced; D_8812_009 quiet-where-computed, NAMED
exclusion = engaged 5695 Part I); ⑤ **#82** (`a4eb22b`,
dep-da583g0ae00c73b3mk60) — source_defects attached-form class
(form:"attached" + attached_form/filed/computed; tie verdict untouched;
notes join face-cascade authority groups; published schema regenerated);
⑥ **#83** (runner only, no deploy) — import-lane.ps1 local validate now
reads the generated schema per lane, all four regression shapes verified
live; ⑦ **#75** (`9c25cc4`, migs 0338+0339, deploy pending verify at
close) — Schedule A line 16 composes with OtherMiscDeductionsStmt
(GAMBLING LOSSES auto-derived via the same §165(d) helper; new
`scha_other_itemized_type` closed-enum field; named refusals replace the
blanket one). Annexes for all are in BATCH-296; entry lane notified per
deploy with re-stage asks (the client-4502 $1-split; the two #76
fixtures; the two D_EFILE_001 gambling fixtures — clients 2003/3731).*

*▶ NEXT: **awaiting the entry session's hold shapes** for the
staging-guard family (1310 date-of-death; asset `flow_to` unresolvable
`link_key`; `mortgage_deductible` teaching refusal) + the CTC missing-DOB
warning — asked by message, will not build refusals from one-line
summaries. Also queued: shell-lookup disambiguation (city), cleanup-API
unknown-key 400, item 37/67 (D_6251_005 zero-TI exemption, SEVEN tied
returns), the EIC opt-out class (line 27c has no return-level field).
**Deferred flagged: `scha_other_itemized_type` UI dropdown** (both client
screens; import+API carry it).*

*⛔ KEN — MORNING QUESTIONS (new tonight): (1) **#77 MFJ**: is the GA
eligible-itemizer credit $600 on a joint return where both spouses
itemize ("$300.00 per taxpayer", §48-7-27.1)? Engine derives nothing for
MFJ until you rule. (2) **#82's fixture ruling** still owed: is 2.564%
(i8829 39-year) right for the client-4175 permanent note? (3) Carried:
#8 GA-700 Sch 4 scope; #6 1065X/AAR spec + season-one scope; #68
optimizer "best"; the s274 PII items (mirror history rewrite; guard
blocklist hardening).*

*▶ s275 side effects: migrations 0337/0338/0339 applied to the shared DB
(all additive with db_default — the 0338 CharField briefly lacked it,
caught and fixed within a minute via 0339). Published batch-import
schema regenerated post-#82-deploy (advertises the attached class + the
r_1099s pso field; regenerate again after #75's deploy for
scha_other_itemized_type). Runner backup pre-#83 in the s275 scratchpad.
RS AGENDA additions: R-RET-5AB pso amendment; R-GA500-RIE rounding
convention; line-5/line-19 derivations; the #77 MFJ answer when ruled.*

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
`D:\dev\Passwords & Secrets\render-api-key.txt`). *(s275: every overnight
deploy was API-verified live before its annex.)*

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** Overnight
arrangement (Ken, ~00:15 8/23): both siblings route questions here;
anything needing Ken directly (RS prod seeds, tax-treatment rulings)
HOLDS for morning. GATE EXCEPTIONS stay human end-to-end. Coordination:
ListAgents + SendMessage; ONE delvio-tax tree holder; ONE
pytest/test_postgres holder; explicit-path staging; NO stash on the
shared tree. *(s275 holds tree + test_postgres.)*

## ▶ LANE MAP (Ken-ruled 2026-08-22, s273; unchanged s275)
- **Tax Return Entry (Claude)**: 1040s only. Owes s275: the re-stage
  proofs (#78 client 4502; the two #76 fixtures; #75 clients 2003/3731),
  the staging-guard hold shapes, the PSO E-flag sweep count, any MFJ
  itemizer's printed line 19.
- **Codex**: 1065 entry lane; re-passes the 1065 Inbox against `e3e88a4`.
- **Delvio-states (Claude)**: idle overnight; LA seeded NOT cleared;
  state-spec priorities routed to Ken (AL 40NR / SC Sch NR / NC D-400).
- **This session**: builds, deploys (Ken's overnight authorization),
  annexes.

---

## ⚠ Known red / rotted — THE ONE LIST (post-s275)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage; fails
  even per-file on a contaminated reuse-db; `--create-db` on the affected
  files = reset AND non-implication proof (s275).
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔
  KEN s217).
- **Client typecheck**: green (`npm run typecheck`); s275's client change
  (PSO field) typechecked + vitest 12/12.

### ⚠ Test-run hazards (standing)
- One shared `test_postgres` (RS suite included). Long runs DETACHED.
  Never pipe pytest through `Select-Object`; redirect to a file.
  `poetry run` only from `server\`. Windows python can't read Bash /tmp.
- ⚠⚠ PS5.1 encoding traps (s275, three hits in one night): `Set-Content
  -Encoding utf8` BOMs (one reached a commit subject via `-F`);
  `Get-Content -Raw` without -Encoding mojibakes UTF-8 before a rewrite
  (double-encoded a batch-file annex AND compute_8839.py — the latter
  restored from HEAD); regex-replace file rewrites are BANNED here — use
  the Edit tool or `[IO.File]` with explicit BOM-less UTF8.
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.

## 🔎 Carried for triage — NOT claims
- (s268) 1,604 queries/run across 957 rules; + s275 adds
  `credit_limit_worksheet_b` running twice per pass (8839 limit + 8812) —
  memoization candidate if the s268 unit lands.
- (s241) `Form8606`/`HSAAccount` duplicate owners · (s234) the $250k
  nonpassive K-1 AGI gap (repro in `test_8960_line4b_clamp.py`) ·
  (s274) the shared-policy pair 8962 fixture · (s275, states session)
  `.first()`-on-per-form-rules sweep chip (task_b1fd177f).

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 170 (GA-600S §179
  HB 1199) · 17a / 17d.

## RS AGENDA — carried + s275 additions:
FA-1040-SCHF-04 re-export · AL_FORM_40NR no spec (#52) · FORM_2441 three
amendments · Form 4136 no spec (#48) · collectibles_28 notes · SC1040
pins 2,360 · NC D400 part-year dates · ten staged FA definitions · 8862
re-author · SCHEDULE_H draft · GA QEE / 4547 / 8879_TA no spec · `500`
spec silent on RIE feeds, DIS gate, joint-split rounding, line-5 and
line-19 derivations (s275) · **R-RET-5AB lacks the §402(l) PSO fact
(s275)** · 1065 K-1 box-15 letters (URGENT).
