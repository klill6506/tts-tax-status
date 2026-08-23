# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-23 very early (s275 — Saturday night into Sunday).*

*⚠⚠ RESUME POINT — **s275 BUILT BATCH-296 #78+#80 AS ONE UNIT, COMMITTED
LOCALLY `2e13d3c`, DEPLOY PENDING KEN'S GO (ask standing: "push?")**.
#78: `split_conserving()` in `compute_ga500.py` — whole-dollar conserving
joint split (taxpayer floor, odd dollar/remainder-tie to SPOUSE, the filed
TaxWise convention), applied at the aggregate of each RIE line's joint
portion, the three L9 capital-gain branches, and the U.S.-obligation
line-6 cut; the old cent-halving double-rounded at whole-dollar format
(+$1 per odd joint total). #80: GA-500 line 5 now DERIVES from
`taxpayer.filing_status` (A/B/C/D letters verified from the pinned 2025
template face; the 7a unconditional-write idiom; `is_overridden` wins so
the 436-payload keyed corpus is untouched) + a staging warning
(`ga500_fields.5`) on non-single + `ga500` expected block + no keyed
line 5. 24 new tests red-then-green with the reported signature
(3472.50); regression 66-file GA/backentry set 1,510 passed — only the
documented seed_builtin_rules quintet (14/14 on fresh DB, not implicated)
+ ONE deliberate supersession: the old
`test_joint_interest_splits_fifty_fifty` pin (232.50/232.50, odd cent to
taxpayer) corrected to 232/233. Flow assertions green. NO migrations.
Annex appended to BATCH-296 (`2e13d3c`, deploy-pending noted). **AFTER
DEPLOY VERIFY**: entry lane re-stages the client 4502 $1-off packet
(should tie clean); then NEXT BUILD = #79 (§402(l) PSO exclusion — VERIFY
statute/Pub 575/1040 line-5 instructions FIRST, incl. per-taxpayer-vs-
per-plan cap), #75 (Sch A line-16 MeF statement, design in the s273
scratchpad), #76 (Credit Limit Worksheet B), #77 (GA-500 line 19 — VERIFY
vs IT-511), #82 (source_defects for ATTACHED-form defects — the two 8829
fixtures, clients 4175 and XXXX9987), #83 (local validate should read the
generated schema), the CTC missing-DOB staging warning, the shell-lookup
disambiguation (city), the cleanup-API unknown-key 400, the
staging-guard family (1310 date-of-death; asset `flow_to` unresolvable
`link_key`; `mortgage_deductible` teaching refusal), item 37/67
(D_6251_005 zero-TI exemption — gates TWO packets), and the EIC opt-out
class (line 27c election has no return-level field).*

*▶ s275 side notes: delvio-states session answered (their next-priority
ask routed to Ken: AL 40NR spec, SC Sch NR proration, NC D-400 items are
the state-side specs that unblock this queue; LA/Wave-5/MA are Ken's
sequencing). Their `.first()`-on-per-form-emitted-rules test trap: a
sweep chip was spawned (task_b1fd177f). RS AGENDA gained: R-GA500-RIE
rounding convention (conserving whole-dollar, TP floor, odd dollar to
spouse, largest-remainder at line aggregate) to be written into the spec
beside the s274 DIS rule when GA RIE rules are authored.*

*⚠ PII follow-ups carried from s274 (both Ken's): (a) public-mirror git
history still holds two commits with client surnames — history rewrite is
your call; (b) sync-guard blocklist hardening (source from the full 1040
Inbox/Done rosters) is queued. STANDING RULE: grep every mirror-bound
file against the session's message names BEFORE sync.*

*⛔ KEN — carried from s274: **#8 GA-700 Schedule 4 partner rows +
nonresident 4% withholding** scope approval (then verify O.C.G.A.
§48-7-129 at build); **#6** 1065X/AAR needs an RS spec + a season-one
scope ruling. Also **#68** (SEP-IRA/saver's-credit optimizer — "best"
needs defining) and the items under KEN DECISIONS OUTSTANDING below.*

*▶ ENTRY-LANE STATE (their last reports, 2026-08-22 evening): eleven
filed that day, Inbox 386 / Done 534; corrected backlog 132 genuinely
unheld. Open for Ken: client XXXX0303 SOURCE RE-EXPORT (the two clergy
worksheets; §107 least-of-three needs used+FRV), item 37/67's seven tied
returns, #18's $486 NIIT, packet 214's PDFs, the 1120-S pre-incorporation
trailer, groups C/D, SEHI↔PTC. **LA is seeded but NOT cleared to build**
(CIT-620 computes AS A C CORP — design conversation needed).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.
- *The s274 resume pointer and day-log moved to `STATUS_ARCHIVE.md` (s275).*

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS (WHEN BUILDS RUN)
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax` (srv-d6geloa4d50c73el2trg); demo = `tts-tax-demo`. ⚠ **A push
is not a deploy — CHECK THE DEPLOY STATUS after pushing** (API key in
`D:\dev\Passwords & Secrets\render-api-key.txt`).

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions** — Tax Return Entry
(1040 entry lane) and Delvio-states (state campaign). They route ALL
questions here; Ken answers through this session on their behalf. GATE
EXCEPTIONS stay human end-to-end: RS prod seeds, pushes/deploys, and
tax-treatment rulings need Ken's word DIRECTLY in the acting session —
proven twice (the LA seed; the s274 8829-tier ruling). Coordination:
ListAgents + SendMessage; ONE delvio-tax tree holder; ONE
pytest/test_postgres holder; explicit-path staging; NO stash on the shared
tree. *(s275: this session holds the tree + test_postgres; states session
told to stand off pytest until handed a window.)*

## ▶ LANE MAP (Ken-ruled 2026-08-22, s273; unchanged s275)
- **Tax Return Entry (Claude)**: 1040s only, in tandem with this session.
- **Codex**: owns the 1065 entry lane (six pilot packets + the gated
  multistate packet; must NOT re-enter the two FILED returns; re-derives
  payloads from source). Re-passes the 1065 Inbox against `e3e88a4`.
- **Delvio-states (Claude)**: MS/AL verticals merged and live; stands off
  pytest until this session hands a window. LA seeded, NOT cleared (above).
- **This session**: holds the main tree + test_postgres; builds, deploys on
  Ken's go, annexes.

---

## ⚠ Known red / rotted — THE ONE LIST (post-s275)
Everything below is the **test-isolation unit**'s scope; each is
pre-existing, documented, and passes on a fresh DB / in isolation:
- **The quintet**: `test_backentry_cleanup::TestBackEntryCleanup` ×3 (s225) +
  `test_backentry_oos_states_s258::TestCleanupDisposition` ×2 —
  seed_builtin_rules leakage between modules (proven s273 at `9689a9b`;
  s275 re-proven 14/14 green on `--create-db` after failing on a
  leakage-contaminated reuse-db).
- **`test_1040.py` — 6 pipeline tests** — unscoped `_fv` `.get()` (s234);
  reuse-db only.
- **`test_mappings.py` — 7 setup ERRORS** (s239 reuse-db cross-module class).
- `test_4868.py` (4) — ⛔ KEN (s217).
- **Client typecheck**: green under `npm run typecheck` (s265); untouched
  by s275 (no client changes).

### ⚠ Test-run hazards (standing — unchanged, s275-verified)
- Never run two pytest invocations concurrently (one shared `test_postgres`).
  ⚠⚠ RS-suite pytest = delvio-tax pytest (same `test_postgres`).
- **`--reuse-db` surfaces the cross-module leakage families** after a big
  combined run leaves seeded rules behind (s275: the quintet failed even
  per-file on a contaminated reuse-db; `--create-db` on the affected files
  is the reset). A KILLED mid-run pytest leaves a stale DB — launch long
  runs DETACHED / in background, not under a tool timeout.
- A full suite is ~1-2 h. Never pipe pytest through `Select-Object`;
  redirect to a file. `poetry run` only from `server\`. Windows `python`
  cannot read Bash's `/tmp` — use the scratchpad.
- ⚠ msys `tail -f` on a file BLOCKS PowerShell `Add-Content` to it (s274).
- ⚠ PS5.1 `Set-Content -Encoding utf8` writes a BOM — a `git commit -F`
  file written that way puts U+FEFF in the subject (s275; write via
  `[IO.File]::WriteAllText` with BOM-less UTF8, or the Write tool).
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; `filing_status` is `"mfj"`; return CRUD routes carry the
  trailing slash.

## 🔎 Carried for triage — NOT claims
- (s268) 1,604 queries/run across 957 rules remain post-memo; the ~9s/packet
  figure is arithmetic, not measurement.
- (s241) `Form8606`/`HSAAccount` allow duplicate owners and their computes
  iterate; browser POST unguarded.
- (s234) a materially-participating 1120-S K-1's $250k nonpassive ordinary
  income never reached Schedule 1 line 5 / AGI (repro in
  `test_8960_line4b_clamp.py`).
- (s274, entry session) the shared-policy pair XXXX0303/XXXX2827 (99%
  allocation) is a natural 8962 allocation regression fixture if wanted.
- (s275, states session) rules emitted once-per-form from a shared loader:
  any test inspecting them via `.first()`/`.get()` checks one row of a
  family — sweep chip spawned (task_b1fd177f).

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+, s230) · 1040 v5.4 business rules not in
  hand · 1120-S Inbox: 180 / 214 / pre-incorporation trailer · 170 is a
  BUILD ITEM (GA-600S §179 HB 1199) · 17a / 17d.

## RS AGENDA — carried from s274, one addition (s275):
FA-1040-SCHF-04 re-export · AL_FORM_40NR no spec (#52) · FORM_2441 three
amendments · Form 4136 no spec (#48) · collectibles_28 deferral notes ·
SC1040 scenario pins 2,360 (published table 2,361 — also the APP's pinned
truth, s274) · NC D400 part-year dates · the ten staged FA definitions
(s242x) · 8862 per-line re-author · SCHEDULE_H draft · GA QEE / 4547 /
8879_TA no spec · `500` spec silent on RIE feeds, on the DIS-opens-the-gate
rule (s274), **and on the joint-split rounding convention — conserving
whole-dollar, TP floor, odd dollar to spouse, largest-remainder at the
line aggregate (s275) — amend all three when the RIE rules are authored** ·
1065 K-1 box-15 letters (URGENT).
