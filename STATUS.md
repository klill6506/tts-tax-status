# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-25 (s290 — five units: the Lacerte 1040 preindexer
rebuilt + 79 packet indexes landed (entry-lane hold LIFTED), the QBI
carryforward sign refusal, front-desk plan ① COMPLETE (Check In screen +
FRONT_DESK lockout), K-1 16A → 1040 line 2a, the 8995 line-1 table from a
shared population. Three deploys Render-verified LIVE; two commits HELD
locally on one named blocker.)*

*⚠⚠ RESUME POINT — **two commits sit LOCAL-ONLY on main, push HELD on one
blocker**: `0be704c` (D_1040_008 retired — inactive stub + kept entry, the
s266 way, Ken-ruled) and `a47de3a` (the 8995 line-1 table; both after the
LIVE `ad0dedc`). The blocker: `test_dg_scenario[DG-4]` is RED BY DESIGN until RS
1040_SPINE scenario DG-4 drops (Ken ruled the retirement + DG-4 drop as ONE
amendment). **The RS tree is the states lane's and is MID-WORK (uncommitted
`load_al_40nr.py`)** — asked twice (no reply); the amendment is theirs, or
next session takes it if their tree is clean by then. After the RS
amendment: re-export the cached `server/specs/1040_spine_spec.json`, run
`pytest tests/test_1040_spine_diagnostics.py`, then push (takes both held
commits + the 8995 unit live in one deploy — verify via Render API).*

*s290 SHIPPED AND LIVE (all Render-API-confirmed):
**① `5429760` / dep-da6vjo0ae00c73cfurgg** — the Lacerte 1040 preindexer
(the entry lane's 3-defect item, verify-first confirmed): wrapped
forms-needed continuation lines (a wrapped line was DROPPING the whole
Georgia section), full 1040-family + GA-individual FACE_PATTERNS curated
from real corpus heads (2 rounds; "Form8889" prints with NO space; curated
patterns now try before greedy fallback literals; satisfied-not-absent for
sibling-pattern tokens), 1040 identity (name-above-SSN) + name-stamp
supplements. 79 indexes landed at `1040\Lacerte Inbox\PageIndex\`; the
entry lane's Lacerte sub-queue hold LIFTED (messaged + annexed). ⚠ The
remaining unmapped tokens are OUT-OF-STATE faces genuinely absent from the
exports — two NC packets and two SC packets are affected (INSTALLED
states, so those exports may be incomplete for entry; packet names in the
BATCH-296 annex). PLUS the QBI
carryforward SIGN REFUSAL (taxpayer.qbi_loss_carryforward_prior /
qbi_reit_ptp_carryforward_prior keyed >0 refuse by name — the s289-late
[client] guard candidate; Form 172 precedent). ⚠ The read-only population
probe found TWO FILED returns carrying the positive slip, zero current-tax
impact only by the line-11/12 cap (s220 cancels-by-luck), pools destroyed —
flagged to the entry lane for re-key (BATCH-296 annex has the return ids).
**② `9a4ef7c` / dep-da6vqo7lk1mc73aubaf0** — front-desk plan ① COMPLETE:
`FrontDeskLockoutMiddleware` (default-deny API allowlist at ONE chokepoint,
injection-proven, built BEFORE any desk login per the deferral) +
`CheckInScreen` (Ken's one-flow ruling: search-first, New-Client only after
a search ran, separate first/last composing "LAST, FIRST", 409-candidates →
confirm_new; FRONT_DESK logins route to it standalone; staff at /check-in).
Verified live against the dev pair. Client suite 146 files / 1,764 green.
Still open (DEFERRAL_AUDIT): the preparer-side temporary-queue VIEW; the
stage-two full-SSN duplicate diagnostic.
**③ `ad0dedc` / dep-da6vv4tg1s2s73bqqt00** — K-1 box 16A rides 1040 line 2a
(Ken ruling): `k1_tax_exempt_interest_total` (7203-keyed) joins
compute_intdiv's 2a the way k1_interest_total joins 2b; i1040 2a text
verified from the 2025 PDF; §86 provisional / 8962 MAGI / MeF move through
the composed FFV (all readers audited). Boundaries: 16B basis-only
(negative-control pinned); 1065 box 18A has no recipient field. Mig 0361
help_text-only.*

*s290 HELD LOCALLY (deploy with the DG-4 unblock): **④ D_1040_008 retired**
(stub + kept inactive entry; override-shape tests now pin it silent).
**⑤ the Form 8995 line-1 business table** (punchlist 18 = client 2970,
diagnosis REFINED: print/MeF already itemized Sch C + K-1 — the dead face
was the SCREEN (FFV rows Sch-C-only) and farms/QBI-rentals were itemized
NOWHERE — the s287 screen≠print class). One shared population
(`f8995_line1_sources`: Sch C → cash farms → QBI rentals → K-1 §199A) now
feeds compute FFVs + render name/TIN + MeF group; face caps at 5 with an
all-rows overflow STATEMENT page; the XSD group is unbounded so MeF
transmits every row. 5 tests + injection; 573 + 143 green incl. flow
assertions + MeF scenarios. Client 2970's screen heals on next open
(recompute chokepoint).*

*▶ NEXT: **⓪ the DG-4 unblock** (above — then push + one deploy + Render
verify). **① the TaxWise extractor build** (approved plan ② phase B, 2-3
sessions — START IN A FRESH SESSION; then the 50-return pilot on John's
book). Then Ken-directed: EIC derive unit (probe-first) · AOTC picker.
Then defects by cost: the [client] −487 residual + S1-13 double-landing
observation (hold b926; instrumented rolled-back dry-run) · item 84
(§469(i) $25,000 allowance) · the two NEW Houston items (4952 line 4e/4f
derivation overstates the deduction — net capital gain IS 14,036 under
§1222(11), engine derives 12,613; and capital_transactions.owner IGNORED in
the POSITIVE-line-7 GA RIE split — both probed byte-identical, annexed in
BATCH-296) · #60 · #43-medium · #70 · #16.*

*Entry lane: Lacerte sub-queue UNBLOCKED (79 indexes current; regenerate
newer arrivals with the script — Ken kept exporting all session, 57→79).
Two filed returns to re-key the QBI cf sign on (annex). The QBI sign
refusal is LIVE — positive re-stages refuse by name. ⚠ The 16A→2a routing
can move §86 taxable SS on staged returns carrying S-corp K-1 tax-exempt
interest.*

*⛔ KEN remaining: #21, #48 (RS 404), #56, #63, #69, #10 — the tail tier.
Carried: entity second-state-face transport (#3); `OVERRIDE_HONORED_STATE_
LINES`; 146-packet re-export; NC/CA/SC linked-state reopens; #6 1065X/AAR;
#68 optimizer; s274 PII narrowings; RS 8990 re-authoring gate; 6765 Sec G;
client-4545 D_8606_BASIS_ONLY; 1065 BATCH-004 #4 (Codex Box-2); Analysis
line-2 active/passive proxy. ▶ AWAITING: client-4167 NIIT decomposition
(entry lane).*

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
hold only for a named reason.** ⚠ Right now main is 2 commits ahead of
origin with a NAMED hold (the DG-4 amendment — see the resume point).
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
holder — coordinate EXPLICITLY before every run (asked twice s290, no reply
either time; proceeded after the hold window on small runs). ⚠ The RS tree
is the STATES lane's and was mid-work (uncommitted file) all s290 — the
DG-4 amendment waits on them. Peers stage; Ken decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s290)
- **`test_dg_scenario[DG-4]` — RED BY DESIGN** until the RS amendment lands
  (the D_1040_008 retirement's paired half; see the resume point). This is
  the push blocker, not a defect.
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s290 — CheckInScreen included; vitest 146
  files / 1,764 green).

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
  are now DEAD everywhere once the 008 retirement deploys — removal
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

## RS AGENDA — carried + s290 adds:
Everything from s277–s289 stands (R-K1-20N; R-GA700-PARTNERS; AL_FORM_40NR
amendments; R-AL-TAX; R-B1/B2-AUTO; the 4562 line-17 reversal;
R-8880-LINE1-COMPOSE; FORM_7203 Part I line-3 sub-map; R-K1-179-BASIS;
R-GA500-RIE loss-allocation clause). **s290 adds: (a) 1040_SPINE — DROP
scenario DG-4** (Ken-ruled with the D_1040_008 retirement; THE ACTIVE
BLOCKER — see resume point). **(b) SCHEDULE_K1 box 16 A/B routing** — 16A →
1040 line 2a is now LIVE in compute (Ken-ruled); the spec should record it
(the k1_interest/2b addend precedent). **(c) R-8995-QBI** — the line-1
population now spans Sch C + cash farms + QBI rentals + K-1 §199A entities
with a 5-row face cap + overflow statement; the spec models Sch C rows
only (the rentals divergence was already flagged; widen the rule).
