# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-24 ~00:50 (s281 — 1040 BATCH-012 built, deployed and
verified LIVE; the standing `test_topic7_compute_leg` red diagnosed and cleared).*

*⚠⚠ RESUME POINT — **s281 SHIPPED TWO VERIFIED DEPLOYS.**
**1040 BATCH-012 ALL THREE ITEMS CLOSED** (`2759c22`, deploy
`dep-da5sd5eq1p3s73bh4ck0` LIVE; annex appended to the batch file; rules
seeded AFTER live, 1,016 → 1,021 rows, `check_rule_paths` green).
⭐ **The finding that reframed item 1: the Rule Studio spec had the answer
all along.** `sch_8812` R001 AND R002 both list
`dep_is_claimed_as_dependent` among their inputs; the app had deleted the
input AND recorded a justification for it (the `Dependent` docstring
asserted every row was claimed; `classify_dependent_odc`'s docstring said
"iff they are claimed" for a condition the code never tested). Built:
`Dependent.claimed_as_dependent` (default+db_default TRUE, migration 0354)
honored in CTC/ODC, the page-1 grid, MeF DependentDetail + the
F1040-111-02/IND-089-01 counts, 1040-X, 8862, the import allowlist and the
serializer — while **Schedule EIC and the EIC computation keep the row
unchanged** (§32(c)(3) has four tests and no support test).
**`D_8812_015`** flags claimed-AND-self-supporting as the contradiction it is.
⚠ **My s280 triage note was WRONG and is corrected in the annex**: R002
deliberately omits `provided_over_half_own_support`, so the entry lane's
dry-run finding "the field changes nothing" found the DESIGN, not a defect.
**Item 2 (FS normalizer)**: new `apps/returns/state_filing_status.py`;
case-insensitive + a loud `D_*_FS` ERROR on an unrecognized value, across
AL/NC/SC/GA. ⚠ The sibling audit found a FOURTH case worse than the three
named — **SC1040 had NO normalizer at all**, so an unrecognized status
reached single-filer treatment BY OMISSION. **Item 3**: two stage WARNINGS
(never errors) — a `dependents` row with no `tin_type`, and a `div_1099s`
row with box 1a and no `qualified_dividends`. ⚠ The dividend guard's
stand-down follows the aggregate VALVE: the taxpayer aggregate rescues the
all-omitted shape and rescues NOTHING once any row carries box 1b, which
makes the **mixed-payer shape the dangerous one** (named payer by payer).
**Also shipped: `manage.py check_rule_paths`** — Ken's REVIEW_QUEUE
recommendation after s267; resolves every seeded rule's dotted path against
deployed code, exits 1 on a break (that failure has hit 3× and a preparer
found it every time).*

*▶ **SECOND DEPLOY** (`9003c95`): the standing red in
`test_topic7_compute_leg.py` was **a fixture that could not SUCCEED**. First
PROVED not mine (all three compute changes reverted → fails identically at
HEAD, restored byte-for-byte — the s179 rule). Cause: that file's own
module-scoped `seeded_forms` shadows conftest's `credit_forms` and never
seeded `sch_8812`, so line 28 (ACTC) was empty on EVERY scenario — and the
test asserts "the age-8 QC's ACTC stays," a value that could never have been
nonzero. One seed call; 26 green. ⭐ **The mirror of the states lane's "a
fixture that cannot fail is not a test" the same night.** ⚠ It nearly became
a false major-defect report (single parent + 1 child + $15k wages → zero
ACTC looks exactly like a refundable-credit defect; the engine was never
involved).*

*▶ NEXT UNIT — **BATCH-296 item 24** (1040 Schedule E rental depreciation
assets). SCOUTED, not started: `DepreciationAsset` already exists on
`TaxReturn` with a `rental_property` FK, `property_label`, `amt_cost_basis`
and AMT prior/current — but **`FLOW_CHOICES` has no `schedule_e`** (only
`8825` for the entity path, plus `schedule_c`/`schedule_f`), and the 1040
`backentry.v1` allowlist/schema have no `depreciation_assets` section. So
the build ≈ add the `schedule_e` flow + the 1040 import surface + per-property
routing to Sch E line 18, reusing the existing AMT/§179 machinery.
`flow_to` is `max_length=10` — "schedule_e" is exactly 10, it fits. Fixtures
named in the item: clients 4125, 1814, 2583 and one further packet (client
numbers only — this file mirrors PUBLIC).
Then: entity-lane second-state-face transport (#3), AL Form 40NR (#4).*

*▶ AWAITING: (1) entry lane — **re-staging client 4569** with
`claimed_as_dependent: false` on the daughter's row (deploy confirmed to
them; expect 33/34/35a −500 → filed 5,068, AL 892 vs filed 893 = the
already-named R-AL-TAX boundary). **BATCH-012 stays in `CC Changes` until
that lane-confirms** — deliberately not moved to Done on the deploy alone.
(2) entry lane is hunting a **mixed-payer dividend packet** to exercise the
warning arm nobody has seen fire.*

*▶ ⛔ KEN — **NEW, states lane, staged and ready**: (a) **GA-500 Schedule 3
line 9 — D-36 APPLIES, and Georgia published the proof itself.** IT-511
works a FILLED example: col A 49,500, col C 39,093, exact ratio 78.975758%,
booklet prints line 9 = **78.98%** and line 13 = **12,637**. Printed →
16,000 × 0.7898 = 12,636.80 → 12,637 (MATCHES); full precision → 12,636.12
→ 12,636 (misses by $1); line 14 closes it (39,093 − 12,637 = 26,456). ⚠
**My counter-evidence is STRUCK**: client-3184 tying under full precision was
NON-DISCRIMINATING — its two products are 267.86 vs 267.60 on a 12,000 base,
nowhere near a straddle, so it never spoke to the question. `R-GA500-S3` is
**silent** on precision (not wrong — incomplete) and S3-9 has no ratio
fixture at all. Ask: amend to percentage-at-2-decimals citing the face + the
worked example. **GA-500 S3-9 UNTOUCHED until Ken rules.** (b) **Gate-1: may
discriminating scenarios be added to the seeded specs in §4.1/§4.2 of
`delvio-states/research/fixture_discrimination_sweep.md`?** Recommend YES in
one word for the whole section — it changes no rule text, no rate, no
rounding, and cannot alter any return; it only makes the fixture capable of
failing. Both are also in REVIEW_QUEUE.md.*

*⛔ KEN — carried unchanged: state-face override-honor convention; client-2961
AL 40NR source-defect disposition; the 146-packet re-export (re-export ONE
first); NC/CA/SC linked-state reopens (BATCH-015 #2-4); #8 GA-700 Sch 4; #6
1065X/AAR; #68 optimizer; s274 PII items; RS 8990 re-authoring gate; Form
6765 Section G (TY2026+); client-4545 D_8606_BASIS_ONLY; per-rule cleanup
acknowledgment. **1065 BATCH-004 #4 still the only open batch item, BLOCKED
on Codex's Box-2 statement arithmetic.**

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
- (s241) `Form8606`/`HSAAccount` duplicate owners · (s234) the $250k
  nonpassive K-1 AGI gap · (s274) shared-policy pair 8962 fixture ·
  (s275) `.first()`-on-per-form-rules sweep chip.
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
