# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-24 (s282 — AL Form 40NR built end to end, ONE verified
deploy).*

*⚠⚠ RESUME POINT — **s282 SHIPPED AL FORM 40NR** (`d5fbea1`, deploy
`dep-da64e249v7es73fmt23g` Render-API-confirmed LIVE 09:32; form def + 17
`D_AL40NR_*` rules seeded — ⚠ the deploy's own `build.sh seed_all`
auto-discovered the new seeder and seeded at BUILD time, my manual run was the
idempotent verify; `check_rule_paths` clean at 1,038 rows; published schema
regenerated with the variant).*

*① **BUILD_ORDER #4 CLOSED — the first NON-DEFAULT REGISTRY VARIANT.** The
40NR is a physically different form from the resident Form 40 (SC/NC do
nonresidency as a schedule), so `StateForm` gained `is_default`; a bare
`state_returns` AL row still resolves to AL40, and the lane names the variant:
`{"state": "AL", "form": "AL_40NR", fields: {...}}` (staged + committed +
schema'd; `expected.al_40nr` control lines 19/21/29/33; create-state-return
takes the same optional `form`). The unblocking event: **Ken's morning
"approve all three" — RS `b978ef7` seeded the 40NR tax-table floor rows**
($0–$49 prints $0; staged question (c) RESOLVED), so spec and Form 40 table
agree and the engine reuses Form 40's published-table code verbatim.*

*② **The engine enforces the form's traps STRUCTURALLY**: retirement enters
column B ONLY (`ret-t`/`ret-s` + per-taxpayer age-65 $6,000 RS exclusion; NO
input reaches column C — r. 810-3-14-.05 lists a nonresident's AL gross income
exhaustively and pensions appear nowhere); the line-10 percentage is the
PRINTED two-decimal figure (D-36) struck on line 9; Part II's asymmetric sums
ride the `adj-p2`/`pen-ew` split; Schedule A's three floors read their OWN
columns (medical 4% col B; casualty 10% + job 2% col C), proration ON the
schedule, 24c/29 unprorated; FIT built to the form (D-32 A1 two printed
multiplications; the regulation's single fraction lives only in
`D_AL40NR_MFS_FIT_DIVERGENCE`); line 14 override-honored — the NRA valve —
and exempted (with AL40's line 12) from the does-not-survive staging warning
via `OVERRIDE_HONORED_STATE_LINES`.*

*③ **Evidence**: 55 new tests green, 614-test regression green (incl. flow
assertions), teeth proven by TWO defect injections (full-precision pct → 5
red; retirement leaked to col C → scenario H red), both reverted. The filed
reconstruction ties end to end THROUGH THE LANE (commit → compute → echo:
tax 174, refund 426); the retirement-corrected twin ties at 343. The render
leg carries a POSITION AUDIT — 17 values each asserted inside its own cell +
row band on the ALDOR template (grid read off the template's own printed
in-grid marker digits), so a drift fails loudly.*

*④ **Two spec→app diagnostic adaptations, RECORDED (RS agenda)**:
`D_AL40NR_ATTACH_FEDERAL_RETURN` ships INFO (the spec's condition names
`federal_return_attached`, a fact its own facts table never declares);
`D_AL40NR_RETIREMENT_NOT_IN_COLB` discriminates on the ATTACHED federal
1040's 4b+5b vs an empty RS carry (the spec's `retirement_in_column_b` is the
engine's own output). Named v1 boundaries (DEFERRAL_AUDIT): Part II per-line
col-C limits (IRA AL-source cap / moving / SEHI's own ratio) not keyable —
flag such a packet; page-2 Parts I–VI not rendered; no client-UI variant
picker (the endpoint takes `form`, the UI doesn't offer it yet). No client
changes — typecheck not run, still green from s281.*

*▶ NEXT (fresh session): **⛔ BUILD_ORDER #3 (entity second-state-face
transport) remains STAGED FOR KEN** (architectural; options in REVIEW_QUEUE).
With #4 done the individual state-demand list is CLEARED — next unblocked
work is 1040/1065 batch items as the lanes post them, else the s281 carried
list.*

*▶ AWAITING: (1) **entry lane still token-blocked** (Ken mints in their
session). When unblocked: restage client 4569 (AL dollar fixed — should tie
OUTRIGHT) and key the client-2961 40NR packet through the new variant path
(SendMessage sent with the vocabulary; also durably in the regenerated
schema's `$defs.state_line_vocabulary.AL_40NR`). ⚠ client-2961's
corrected-position disposition (third 1099-R plan type) is a Ken gate.
(2) **GA-500 S3-9: Ken APPROVED the precision amendment but the seed is
BLOCKED on a live two-writers hazard** — `load_ga500_form_500.py` and
`load_ga700.py` both declare AuthoritySource `GA_OCGA_48_7` differently and a
seed would silently rewrite GA-700's version (campaign D-31's fourth member,
live in prod). States lane holds it `READY_TO_SEED=False`; needs Ken's
ownership call. (3) 1040 BATCH-012 + BATCH-296 stay in `CC Changes`
(lane-confirmed / entry-lane fixture work).*

*⛔ KEN — outstanding: (a) ~~AL 40NR floor~~ **RESOLVED** ("approve all
three", RS `b978ef7`). (b) fixture-discrimination scenarios — approved same
ruling; execution is the states lane's. (c) **NEW: the `GA_OCGA_48_7`
authority-row ownership** (above). (d) **the entity second-state-face
transport** (#3, REVIEW_QUEUE). Carried unchanged: state-face override-honor
convention (⚠ s282 grew the honored set by AL_40NR "14" — the convention
question is riper, see `OVERRIDE_HONORED_STATE_LINES`); client-2961 AL 40NR
source-defect disposition; the 146-packet re-export; NC/CA/SC linked-state
reopens; #8 GA-700 Sch 4; #6 1065X/AAR; #68 optimizer; s274 PII items; RS
8990 re-authoring gate; Form 6765 Section G; client-4545 D_8606_BASIS_ONLY;
per-rule cleanup acknowledgment. **1065 BATCH-004 #4 still blocked on
Codex's Box-2 arithmetic.***

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
⚠⚠ **ORDERING (s279): push → deploy LIVE → `seed_rules` → verify.** Never
seed a code-coupled rule row before its deploy is live. ⚠ s282 refinement:
**the deploy ITSELF seeds — `build.sh` runs `seed_all`, which auto-discovers
every `seed_*` command at BUILD time** (the AL_40NR def existed before my
manual seed ran). A new seeder therefore goes live WITH its deploy; the
manual post-deploy seed is the idempotent VERIFY, and `check_rule_paths` is
one command.

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** Coordination:
ListAgents + SendMessage — ⚠ the channel DROPPED a whole morning of messages
both ways (s277): anything load-bearing goes in batch-file annexes too. GATE
EXCEPTIONS stay human end-to-end IN the acting session. **Never relay tokens
through the message channel.** ONE delvio-tax tree holder; ONE
pytest/test_postgres holder.
⚠ **s281 (Ken, overnight): this session is authorized to ANSWER questions
from the states and entry lanes and to use its own judgment**, escalating to
Ken only where an answer is genuinely required. **The limit stands: a relayed
authorization is not a Ken ruling** — amending a seeded spec stays Ken's gate.
Peers stage; Ken decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s282)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` on the affected files = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting any of these (the s281 topic7 lesson: a
  carried red nobody re-derived turned out to be a fixture that could not
  succeed).
- **Client typecheck**: green (`npm run typecheck`, s281). No client changes s282.

### ⚠ Test-run hazards (standing)
⚠⚠ **SCOPE MARKING (s281).** This file is the hazards list for ONE of three
lanes. 🌐 = campaign-wide (belongs in EVERY lane's boot + session memory);
🔧 = this repo's test runs only.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED.
  Never pipe pytest through `Select-Object`; redirect to a file.
  `poetry run` only from `server\` (the Bash tool silently prints
  NOTHING for poetry one-liners — use the PowerShell tool).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED. Edit tool
  or `[IO.File]` BOM-less UTF8; never `Set-Content`/`Get-Content -Raw`
  for UTF-8. After ANY shell touch of a source file, grep the diff for
  mojibake markers (s282: checked clean; ⚠ grep the DIFF, not the file —
  THIS file contains the marker string literally). ⚠ s282: **an embedded
  double-quote in a PS 5.1 here-string argument to a NATIVE exe splits the
  argument** (a `git commit -m @'...'@` with a quoted phrase became stray
  pathspecs) — pass long native args via a FILE (`git commit -F`).
- 🌐 ⚠⚠ **`Measure-Object -Line` DOES NOT COUNT BLANK LINES** — use
  `[IO.File]::ReadAllLines(...).Count` (s280).
- 🔧 ⚠ **`pytest-randomly` is NOT installed here** (s281; verify with
  `find_spec`, not a lockfile grep). Test order is NOT randomized.
- 🌐 ⚠⚠ **A BARE HTTP 400 (no `error` body) MEANS THE BODY NEVER PARSED —
  suspect ENCODING, not the packet** (s281; fix: send
  `[Text.Encoding]::UTF8.GetBytes($json)` with `charset=utf-8`; the server
  was proven innocent by reproduction, `tests/test_cleanup_note_encoding.py`).
- 🌐 Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.

## 🔎 Carried for triage — NOT claims
- (s282) **The state-face override-honor convention now has a REGISTRY**:
  `backentry.OVERRIDE_HONORED_STATE_LINES` (AL40 "12", AL_40NR "14") exempts
  honored valves from the computed-line staging warning. ⚠ It must stay in
  sync with each compute's `overridden` handling BY HAND — nothing enforces
  the pairing. If a third line joins, consider deriving both sides from one
  declaration. The convention itself (which lines honor overrides,
  campaign-wide) is still Ken's carried question.
- (s281) **Out-of-scope-state line-18 candidate diagnostic — NOT BUILT; the
  shape is specified** (prompt-shaped, keyed on the OTHER state's tax paid —
  a fact the app never prepares; n=3: 90 / 28,072 / legitimately nothing).
  Line 18 is keyable and its override WINS (s277).
- (s281) **Stage allowlists `schd_fields` keys but `ga500_fields` keys not
  at all** — refusal happens atomically at COMMIT ("unknown GA-500 line"),
  not at stage. Low severity, real asymmetry.
- (s281) `D_8812_015` fires on 1 of 345 live Dependent rows — CORRECT (the
  §152 contradiction), not a deploy defect; no computed value moved.
- ⭐ (s281, both lanes) **A test/fixture/packet is evidence only about what
  it could have observed**; non-discriminating vs wrong-yardstick are
  different failure modes; a sweep must report its own coverage.
- (s281) The mirror pair for Ken: 8812 spec had an input the code dropped;
  GA-500 S3-9 code had a behavior the spec never spoke to.
- (s280) regression fixtures insensitive to their rule — sweep + Gate-1 ask
  (approved 2026-08-24; execution with the states lane).
- (s279 late) cleanup `source_verified` all-or-nothing per packet;
  client-4545 D_8606_BASIS_ONLY ⛔ KEN.
- (s268) 1,604 queries/run + memoization candidates.
- (s241/s281) `Form8606` unique-constraint candidate (DB clean; Ken's call).
  🔴 `HSAAccount` half CLOSED — multiple HSAs are legitimate.
- (s275/s281) `.first()`-on-per-form-rules sweep chip — narrowed by the
  seeder guard (now 62/71 seeders in reach after seed_al40nr); remaining:
  multi-instance forms + the 9 named out-of-reach seeders.
- (s279) FormLine-seeder two-writers guard: BOTH halves now shipped (s281).

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried + s282 changes:
Everything from s277–s281 stands, minus what Ken's "approve all three"
resolved (AL 40NR floor seeded `b978ef7`; §4.1/§4.2 scenarios approved).
**NEW (s282):** amend `AL_FORM_40NR` diagnostics —
`D_AL40NR_ATTACH_FEDERAL_RETURN` conditions on an undeclared fact
(`federal_return_attached`); `D_AL40NR_RETIREMENT_NOT_IN_COLB` conditions on
the engine's own output (`retirement_in_column_b`) — both shipped with
recorded adaptations (see rules_al40nr docstring). **STILL BLOCKED:** GA-500
S3-9 seed on the `GA_OCGA_48_7` two-writers ownership (D-31 member four,
live). R-AL-TAX mechanism amendment (s281) still staged. D-36 reads for
TN_FAE170 / SC1120 in progress on the states lane.
