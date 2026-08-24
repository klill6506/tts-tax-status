# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-24 (s281 — an overnight run: FIVE verified deploys, both
sibling lanes managed and directed, four Ken questions staged).*

*⚠⚠ RESUME POINT — **s281 SHIPPED FIVE VERIFIED DEPLOYS** (`2759c22` ·
`9003c95` · `c59d2cc` · `e9af114` · `7ebe61d`), each Render-API confirmed LIVE,
never inferred from the push.*

*① **1040 BATCH-012 — ALL THREE ITEMS CLOSED AND LANE-CONFIRMED.** ⭐ The
finding that reframed item 1: **the Rule Studio spec had the answer all
along** — `sch_8812` R001 AND R002 both list `dep_is_claimed_as_dependent`, and
the app had deleted the input while recording a justification for it in the
model docstring. `Dependent.claimed_as_dependent` (default + db_default TRUE,
mig 0354) now gates CTC/ODC, the page-1 grid, MeF DependentDetail + the
F1040-111-02/IND-089-01 counts, 1040-X and 8862, while **Schedule EIC and the
EIC computation keep the row** (§32(c)(3): four tests, no support test).
`D_8812_015` flags claimed-AND-self-supporting. ⚠ My own s280 triage note was
WRONG and is corrected in the annex — R002 deliberately omits
`provided_over_half_own_support`. **Item 2**: new `state_filing_status.py`,
case-insensitive + four `D_*_FS` ERRORs; the sibling audit found a FOURTH case
worse than the three named — **SC1040 had NO normalizer at all**. **Item 3**:
two stage WARNINGS (tin_type; the `qualified_dividends` omission — ⚠ its
stand-down follows the aggregate VALVE, so the **mixed-payer** shape is the
dangerous one). **Entry lane confirmed 4569 ties**: 33/34/35a −500 → filed
5,068, two-child EIC intact, zero diagnostic misfires. Rules seeded AFTER live
(1,016 → 1,021) per the s279 ordering rule.*

*② ⭐ **AL FORM 40 LINE 17 IS THE TAX TABLE, NOT THE BRACKET FORMULA** —
R-AL-TAX was never a precision question and never a Ken gate. Line 17's
instruction is mandatory: "You **must** figure your tax from the Tax Tables."
All 1,001 published rows harvested to
`tests/fixtures/al_form40_tax_table_2025.json`; each band's tax is the §40-18-5
formula at the band MIDPOINT half-up (measured: **1,912 of 1,914 exact-half
cases round up**; the two exceptions are the $50-wide floor rows, both
columns). ⚠ **The bigger half: above $100,000 the booklet's worksheet bases on
the LAST BAND's tax (4,958 / 4,918), so the engine was $2 HIGH on EVERY Alabama
return over $100,000, permanently.** The client-2047 fixture that had recorded
a known $1 miss now ties the filed 708/214 — independent confirmation nobody
constructed. ⚠ My own test caught taxable income of exactly 100,000 falling in
a gap between the last band and a worksheet headed "Over $100,000".*

*③ **BATCH-296 item 24 — TRIAGED, NOT BUILT: its Aug-19 addendum is STALE.**
The 1040 `depreciation_assets` section, `"8825"` in `DEPRASSET_LINKED_FLOWS`,
and the ungated rental branch of `aggregate_depreciation` all already exist
(s272). The sibling e2e file covered the Schedule C and F links but **never the
RENTAL one**, so it was neither proven built nor proven missing —
`test_b296_item24_rental_asset_register.py` settles it, 5 green. **Remaining
work is fixture verification with the real packets (entry lane), not a build.**
⚠ Two corrections against myself in the writing: `property_type` is the IRS
one-character code (staging refused my word by field and value — the refusal
working), and "filed wins" means `_write_parent_depreciation` SKIPS a guarded
parent, never COPIES into `depreciation`; the silent zero I feared is caught by
`D_4562_RECON`.*

*④ **GUARDS SHIPPED**: `manage.py check_rule_paths` (Ken's s267 REVIEW_QUEUE
ask — resolves every seeded rule's dotted path against deployed code; that
failure has hit 3× and a preparer found it every time); and **the
FormLine-seeder half of the two-writers guard** (the s279 chip) — a duplicate
`line_number` is a SILENT overwrite, not an error. ⚠ Both sweeps **report their
own coverage** (61/70 seeders = 87%, the 9 gaps recorded BY NAME) and both had
their **teeth proven by defect injection**, not by a green run.*

*⑤ **The standing red in `test_topic7_compute_leg.py` is CLEARED** — it was **a
fixture that could not SUCCEED**: its own `seeded_forms` never seeded
`sch_8812`, so ACTC was empty on every scenario and the test asserted a value
that could never have been nonzero. First PROVED not mine by reverting all
three compute changes (fails identically at HEAD). ⚠ It nearly became a false
major-defect report.*

*▶ NEXT (fresh session): **⛔ BUILD_ORDER #3 (entity second-state-face
transport) is STAGED FOR KEN, NOT STARTED** — it is a major architectural
change (CLAUDE.md requires asking) and the recommendation + three options are
in REVIEW_QUEUE. Then **AL Form 40NR** (#4) — ⚠ **do NOT inherit the spec's
floor defect** (it computes $1 where Alabama prints $0 for taxable $0–$49;
implement the Form 40 path, which is correct there, and flag the divergence).
Otherwise: 1040/1065 batch items as the lanes post them.*

*▶ AWAITING: (1) **the entry lane's production session has EXPIRED** — staging
and commits blocked until Ken mints a token IN their session (they correctly
refused to route one through the message channel; I did not offer). Nine
returns closed tonight, every state face tying (GA / AL 40 / SC1040 incl. Sch
NR / NC D-400; out-of-scope proven for UT, VA, NJ). They are using the blocked
window to scan all 353 Inbox packets for **Schedule B Part II with 2+ dividend
payers** — the structural filter for the mixed-payer arm nobody has seen fire.
(2) **client 4569 should now tie OUTRIGHT** once restaged — the AL dollar is
fixed. (3) BATCH-012 stays in `CC Changes` (lane-confirmed, packet not yet
recommitted).*

*▶ ⛔ KEN — **FOUR QUESTIONS, all staged in REVIEW_QUEUE + here, none
actioned**: (a) **GA-500 Schedule 3 line 9 — D-36 APPLIES and Georgia published
the proof itself** (IT-511's worked example: printed 78.98% → 12,637 matches;
full precision misses by $1; line 14 closes it). ⚠ **My counter-evidence is
STRUCK** — client-3184 was NON-DISCRIMINATING (267.86 vs 267.60 on a 12,000
base). `R-GA500-S3` is SILENT on precision, not wrong. (b) **May discriminating
scenarios be added to the seeded specs in §4.1/§4.2 of the
fixture-discrimination sweep?** Recommend YES in one word for the section — it
changes no rule text and cannot alter a return. (c) **`AL_FORM_40NR` computes
$1 where Alabama prints $0** for taxable income $0–$49. (d) **The entity
second-state-face transport** (architectural — see REVIEW_QUEUE for the three
options and what I would want confirmed before building).*

*⛔ KEN — carried unchanged: state-face override-honor convention; client-2961
AL 40NR source-defect disposition; the 146-packet re-export (re-export ONE
first); NC/CA/SC linked-state reopens; #8 GA-700 Sch 4; #6 1065X/AAR; #68
optimizer; s274 PII items; RS 8990 re-authoring gate; Form 6765 Section G;
client-4545 D_8606_BASIS_ONLY; per-rule cleanup acknowledgment. **1065
BATCH-004 #4 still the only open batch item, BLOCKED on Codex's Box-2
arithmetic.**

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
seed a code-coupled rule row before its deploy is live. s281 held to it and
`check_rule_paths` now makes the "verify" step one command.

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
Ken only where an answer is genuinely required. ⚠ **The limit the states lane
drew and was RIGHT to draw: a relayed authorization is not a Ken ruling** —
amending a seeded spec stays Ken's gate (the s262 rule). Peers stage; Ken
decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s281)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` on the affected files = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ✅ **CLEARED s281**: `test_topic7_compute_leg.py` (was 1 red — the missing
  `sch_8812` seed, above). ⚠ **Its diagnosis is a reason to distrust the rest
  of this list**: it had been carried as a red without anyone asking what its
  assertion could observe. Re-diagnose before inheriting.
- **Client typecheck**: green (`npm run typecheck`). No client changes s281.

### ⚠ Test-run hazards (standing)
⚠⚠ **SCOPE MARKING (s281, prompted by the 1040 entry lane).** This file is
**the hazards list for ONE of three lanes**, not the campaign's. A note here
protects only whoever boots THIS repo — which is why the `Get-Content -Raw`
UTF-8 trap, recorded below for several sessions, still cost the entry lane a
debugging cycle from its own scripts. **A hazard note is worth what its
worst-case reader can act on, so its home must match its blast radius.**
Entries below are marked 🌐 (campaign-wide — belongs in EVERY lane's boot,
and in session memory, which boots regardless of which repo is open) or 🔧
(this repo's test runs only).
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED.
  Never pipe pytest through `Select-Object`; redirect to a file.
  `poetry run` only from `server\` (the Bash tool silently prints
  NOTHING for poetry one-liners — use the PowerShell tool).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED. Edit tool
  or `[IO.File]` BOM-less UTF8; never `Set-Content`/`Get-Content -Raw`
  for UTF-8. After ANY shell touch of a source file, grep the diff for
  mojibake markers (s281: every python-rewrite checked clean). ⚠ Grep the
  DIFF (`git diff | grep '^+'`), not the file — THIS file contains the
  marker string literally, right here, so a whole-file count of 1 on
  STATUS.md is expected and is not corruption.
- 🌐 ⚠⚠ **`Measure-Object -Line` DOES NOT COUNT BLANK LINES** — never use it
  to find a file's end (s280). Use `[IO.File]::ReadAllLines(...).Count`.
- 🔧 ⚠ **`pytest-randomly` is NOT installed here** (s281 asserted it was, from a
  transitive `poetry.lock` hit, and was wrong — verify with
  `find_spec('pytest_randomly')`, not a lockfile grep). Test order is NOT
  randomized; `-p no:randomly` is inert.
- 🌐 ⚠⚠ **A BARE HTTP 400 (no `error` body) MEANS THE BODY NEVER PARSED —
  suspect ENCODING, not the packet.** Every 400 this app raises carries a
  named message, so a bodyless one was produced before any application code
  ran. s281: the entry lane's cleanup POSTs failed on any non-ASCII note.
  Two causes, both client-side: `Get-Content -Raw` with no `-Encoding`
  (PS 5.1 reads a BOM-less UTF-8 file as the ANSI codepage), and a STRING
  body with `application/json` and no charset, so Content-Length counts
  CHARACTERS while the socket carries BYTES (1,626 vs 1,630 on the real
  note) and the server reads a truncated body. Fix: send
  `[Text.Encoding]::UTF8.GetBytes($json)` with `charset=utf-8`. ⚠ The
  tempting misreads are "wrong batch id" and "wrong state for cleanup";
  both are dead ends. **The server was verified innocent by reproduction
  before any fix was attempted** (`tests/test_cleanup_note_encoding.py`).
- 🌐 Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.

## 🔎 Carried for triage — NOT claims
- (s281, from the entry lane's third out-of-scope case) **Candidate
  diagnostic, DELIBERATELY NOT BUILT: warn when `out_of_scope_states` is
  declared on a GA-resident return with no line 18 keyed.** Tempting — the
  missed credit silently OVERSTATES Georgia tax — but an out-of-scope state
  does not always produce a Georgia credit (the other state may levy no tax;
  Georgia may not be the resident state), so it would misfire on legitimate
  packets. Needs the discrimination question answered first (which shapes
  genuinely carry a credit?). ⚠ The class is NOT blocked meanwhile: line 18
  is keyable and its override WINS (the s277 fix); the rule is recorded at
  the point of use in `payload_out_of_scope_states`' docstring.
- ⚠ **(s281, raised by the 1040 entry lane) STAGE ALLOWLISTS `schd_fields`
  KEYS AND `ga500_fields` KEYS NOT AT ALL** — two adjacent blocks in the same
  function, one doing `if ln not in SCHD_DIRECT_LINES: _err(...)` and the
  other validating only that keys are non-empty strings. **Not a data-loss
  risk**: `_apply_state_fields` refuses the whole commit atomically at 400 on
  an unknown line ("unknown GA-500 line"), so a bad key dies loudly — at
  COMMIT rather than at STAGE. Fail-fast would be better: a bad key should
  die before a batch is staged. Low severity, real asymmetry. ⚠ The lane's
  hypothesis that such a key lands SILENTLY is REFUTED — recorded because a
  refuted hypothesis is worth as much as a confirmed one here.
- ⚠ **(s281, POST-DEPLOY PROBE) ONE live return now shows a NEW ERROR that
  was not there yesterday, and it is CORRECT — not a regression.**
  `D_8812_015` was the one new rule not covered by the pre-deploy probe (the
  four `D_*_FS` rules were, across 785 rows, zero firing). Probed after:
  **1 of 345 live Dependent rows fires it** — a nephew born 2000 flagged
  `provided_over_half_own_support` AND claimed as a dependent, which is
  exactly the §152(c)(1)(D)/§152(d)(1)(C) contradiction the rule exists to
  name. One of the two facts on that row is wrong and a preparer has to say
  which. ⚠ **NO computed value moved**: `claimed_as_dependent` defaults TRUE,
  so every classification is byte-identical to before — the diagnostic is the
  only new thing. Recorded so the red is not mistaken for a deploy defect.
- ⭐ (s281, BOTH lanes, independently) **A test/fixture/packet is evidence
  only about what it could have observed.** Four instances in one night:
  the SC spec's 0.60/1.00 scenarios (exact at either precision); the states
  lane's own sweep v1 (0 findings from 0.8% coverage — it caught itself);
  `test_topic7_compute_leg`'s missing 8812 seed (a fixture that could not
  SUCCEED); and client-3184 offered as GA precision counter-evidence when its
  two methods differed by 26 cents. ⚠ **And the distinction the entry lane
  drew: non-discriminating evidence and correctly-measured-against-the-wrong-
  standard are DIFFERENT failure modes.** Their AL `dep-n` probe was sound and
  discriminating and still produced a wrong item, because the bracket it was
  measured against was recalled rather than fetched. A discriminating fixture
  buys a trustworthy measurement, never a correct yardstick.
- (s281) **The mirror pair worth showing Ken**: on 8812 the SPEC had an input
  the CODE dropped; on GA-500 S3-9 the CODE had a behavior the SPEC never
  spoke to. Neither was a disagreement — both were places nobody was forced
  to decide, and in both the fixtures could not have surfaced it.
- (s281) A live **SC1040 return carries `mfs` lowercase** — pre-fix it granted
  a childcare credit MFS is barred from; **no dollar impact only because that
  return had no childcare expenses** (a value that cancels by luck). Now
  case-normalized; nothing to repair.
- (s280, states lane) regression fixtures insensitive to the rule they cover
  — now systematically swept, see the Gate-1 ask above.
- (s279 late) cleanup `source_verified` all-or-nothing per packet;
  client-4545 D_8606_BASIS_ONLY ⛔ KEN.
- (s268) 1,604 queries/run + memoization candidates (s275/276/277).
- (s241) `Form8606`/`HSAAccount` duplicate owners — ⚠ **NARROWED s281 by a
  read-only probe, and the two halves have DIFFERENT answers.** Live DB:
  **zero duplicate (tax_return, owner) pairs** in either — 35 Form8606
  rows, 62 HSAAccount rows, neither carrying a constraint.
  🔴 **`HSAAccount` must NOT get the Form5329-style uniqueness
  constraint — a taxpayer may legitimately hold MORE THAN ONE HSA and
  Form 8889 aggregates them.** A constraint there would refuse a correct
  return; treat that half as CLOSED, not pending.
  `Form8606` (one per person per year) is a plausible candidate, but far
  less urgent than Form5329 was: **both siblings ITERATE their rows, so a
  duplicate double-counts VISIBLY rather than silently disappearing** —
  which was the whole reason Form5329's dict-keyed-by-owner needed the
  constraint. No action taken: a unique constraint on a shared
  production table is Ken's call when the DB is already clean. · (s234) the $250k
  nonpassive K-1 AGI gap · (s274) shared-policy pair 8962 fixture ·
  (s275) `.first()`-on-per-form-rules sweep chip — ⚠ **NARROWED, NOT
  CLOSED, by s281's seeder guard.** That guard proves no parsed seeder
  declares one `line_number` in two sections of the same form, so the
  SEEDER source of the ambiguity is currently nil and stays that way.
  ⚠ It does NOT cover the other source — several instances of one form on
  a return (two Schedule Cs, two rentals) — nor the 9 seeders outside the
  guard's 87% reach. So the remaining audit is smaller and better defined,
  not finished.
- (s279) FormLine-seeder two-writers guard (diagnostic-code half shipped;
  seeder half unguarded).

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried + s281 changes:
Everything from s277/s278/s279/s280 stands. **NEW (s281, states lane, both
STAGED not seeded — Ken's gate):** `R-GA500-S3` amend to
percentage-at-2-decimals + add a discriminating ratio scenario (it currently
has none); and the §4.1/§4.2 fixture-discrimination scenarios. D-36
instruction reads for TN_FAE170 `R-TN-SCHN` and SC1120 `R-SC1120-APPORT` are
IN PROGRESS on the states lane — ⚠ the sweep flags BOTH as having
non-discriminating fixtures, so the read and the fixture are one pass.
AL_FORM_40NR build conventions recorded in s279's STATUS_ARCHIVE entry.
