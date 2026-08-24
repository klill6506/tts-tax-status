# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-23 ~late evening (s279 — two 1040-lane batches worked
live during entry, the retired ledger's last three items resolved, and a
seeder-collision guard that caught a silently dead diagnostic).*

*⚠⚠ RESUME POINT — **s279 SHIPPED TWO VERIFIED DEPLOYS (4 commits).**
**1040 BATCH-009 CLOSED** (`9b83991`, deploy `dep-da5m7jqd0e5s73bq1isg`
LIVE; annexed; moved to Done): Form 8960 was eating passive K-1 income —
the non-§1411 back-out used the nonpassive activity's GROSS box 1 while
line 4a carries it net of its own §179/UPE; the overstated back-out hit
the 4b clamp and flattened the bucket. Fix at the ONE shared function
(compute/diagnostics/render/MeF together); filed-split rows now enter the
back-out by filed classification. The entry lane re-ran the fixture SAME
NIGHT: ties, committed, filed.
**1040 BATCH-010 CLOSED** (`dcca02f`, deploy `dep-da5mhlfavr4c73f7s0f0`;
annexed): the NON-iterative S-corp SEHI path (7206, no 8962) now transmits
IRS7206 via the form's line-11 Box-5 route, mirroring the exact
scorp_sehi pairings compute feeds Sch 1 L17 (`scorp_k1_sehi_forms`, new
one-source generator). Farm/Medicare-fold stay the narrowed named boundary.
**BATCH-296 (retired ledger) — zero OPEN items remain**: #50 BUILT
(`cbe370f` — AL40 line 12 preparer override stands + chains,
D_AL40_L12_OVERRIDE polices cap/part-year scope; $1 L17 residual named =
the DOR tax-table midpoint vs continuous-bracket, the PRE-EXISTING stated
boundary); #51 ALREADY BUILT (other_income_items route 8h, s241z); #52
PARKED at the RS-spec gate — and **KEN RULED (via delvio-states): AL_40NR
spec authoring is NEXT in the states lane**, ahead of MS/CO; app build
follows the seeded spec (campaign open item 0; not tonight — full
source-pull → Gate-1 process).
**The duplicate-code guard fired on first run** (`7b8f01b`): D_RET_012 was
declared twice; the s243b SS-worksheet 6b ENGINE INVARIANT had been
silently dead since s242 (seeder last-writer-wins). Renumbered to
D_RET_015 + revived; `seed_rules` run against the shared DB (012 =
disability-MRA unchanged, 015 = invariant, both active;
D_AL40_L12_OVERRIDE also seeded).*

*▶ s279 SECOND HALF (post-close, Ken present) — TWO MORE BATCHES CLOSED,
two more verified deploys:*

*• **1040 BATCH-011 CLOSED** (`b217801` + generator `3033988`, deploy
`dep-da5n7rnavr4c73f8dffg` LIVE; annexed; Done): NEW `sch3_fields` (v1 =
line 6b, the Form 8801 allowed credit as filed — the sch1_fields shape;
verified from code there was genuinely no Schedule 3 surface before
concluding "no route," the 8960 lesson applied). The client-4545 149
credit now reaches 1040 line 20; boundaries named: no 8801 form
(exclusion items / carryforwards unmodeled, D_SCH3_003 = the honest
attach warning) and **NO IRS8801 MeF builder — a 6b return ties/commits
but cannot transmit** (a named future unit). The runner
(`import-lane.ps1`) now SHOUTS on replayed=True stages (#2). Published
1040 schema regenerated (the generator needed its own sch3_fields entry
— the stricter-than-production class again).*

*• **1120-S FORM6765 REQUEST CLOSED** (`5d263d8`, deploy
`dep-da5nhamq1p3s73bdo810` LIVE; annexed; Done — the last 1120-S queue
item, per Ken): BOTH blocking claims in the request were STALE — the RS
6765 spec EXISTS (200; cached, draft-trap-passed) and the FORM has been
built since s230. The true gap was the entity lane: new `form_6765`
OBJECT section (24 input facts; computed lines refuse toward the K13g
pin; fixed_base_pct FRACTION guard — >0.16 refuses as the
percent-as-fraction slip), K13g joins the answer key (the K15f-omission
class), closeout gains declared/present/ENGAGED holds. The packet-227
chain reproduces with no override: QREs 53,704 → floor 26,852 binds →
×15.8% = 4,243 → K13g → K-1 code M. Entity schema regenerated —
including the PRE-EXISTING form_2553 advertisement omission, fixed same
stroke. Codex re-stages packet 227 per the annex.*

*▶ NEXT: **1065 BATCH-004 #4 remains the only open batch item (9/10),
BLOCKED on Codex's Box-2 statement arithmetic** (question in that batch's
annex; Ken has the exact re-ask prompt). ALL queues now clear (1120-S,
1040, legacy-parked). Then per BUILD_ORDER. Awaiting from entry:
packet-170 + investment-partnership re-stages; packet-227 re-stage
(form_6765); client-4545 re-dry-run (sch3_fields); staging-guard fixture
shapes. Awaiting from states: the AL_40NR spec (Gate-1 with Ken; MS/CO
C-corp specs also authored-and-gated tonight). E-FILE UNITS QUEUED from
tonight: IRS8801 composer; the 6765-behind-K13g e-file leg is already
built (MeF K-1 code M).*

*✅ KEN RULING PASS (s279 close, plain-language Q&A — full record in
DECISIONS.md): **(1) AL-12 = SOURCE DEFECT, never an override** (the
#50-vs-s272 conflict resolved as (b); D_AL40_L12_OVERRIDE raised to
ERROR in every arm, deployed; the lane keys no AL-12 overrides; client
2047's packet closes with the $41 defect). **(2) Client 4545's 8606 =
defect on the filed return — close to Done.** **(3) Client 2665's Notice
2008-1 = defect (Ken checked) — close to Done.** **(4) 4387/3199
rounding-band acks approved; NO 2020-2024 prior-year files** ("not going
back 5 years") — the 3199 lookback closes without them, stated as such.
**(5) The two retrospective dry-run sweeps (S1-10 + 8960 classes):
SKIPPED for now** — not refused; re-raise on evidence. All five relayed
to the entry lane for packet closure.*

*⛔ KEN — still parked ("ask me again later"): (1) the state-face
override-honor convention (keyed COMPUTED lines on AL/SC/NC faces beyond
AL40-12 remain accepted-but-ignored — the GA line-18 class). (2) The AL
40NR packet's (client 2961) suspected source defect — nonresident
pensions belong in Column B per the booklet + the Admin Code reg
(810-3-15-.21(2), read verbatim by the states lane); AL% overstated
43.18% vs ≈30.65%, filed tax understated; one payer's plan type still
needs the actual 1099-R; disposition/amendment call rides the AL_40NR
Gate-1 walk in the states session. (3) **Client 1801 identified** (name
with Ken + the lane; number-only here): the "leave it alone" instruction traces to BATCH-296 item
62 (NEVER WORKED, unmarked-open in the retired ledger): his seeded shell
carries browser residue — a doubled educator expense (engine AGI $300
low) and preparer_due_diligence_attested=True that would OVERWRITE
transcribed 8867 answers at commit. Entering him today would corrupt;
the unblock is building item 62's asks (sch1_fields replaces
shell-resident values; a stale-attestation cleanup path) — a real build
unit, queued. (4) The 146-packet re-export (Ken handles tomorrow;
re-export ONE first). (5) Carried: #8 GA-700 Sch 4; #6 1065X/AAR; #68
optimizer; s274 PII items; RS 8990 re-authoring gate; Form 6765 Section
G (TY2026+).*

*Post-close addendum: **BATCH-010 moved to Done** on the lane's verified
three-packet re-run (1657 zero errors + eligible + PDF in Done; 4006
clean; D_RET_015 quiet on all three — no engine-ordering finding).
**BATCH-296 item 24 (the 1040 Schedule E rental asset ledger) confirmed
NOT built** — `flow_to` has no 1040-rental value and compute_schedule_e
reads no asset rows; it is the open remainder of the #23/#24/#53 single
build (the Sch C + Sch F legs shipped) and a real feature unit for
BUILD_ORDER. The lane's affected packet stays held on it; the retired
ledger's annex footer corrected (older unmarked items are not all
closed).*

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
holder; ONE pytest/test_postgres holder (s279 held both; states session
authoring MO_1120 then AL_40NR in RS — gated, no prod writes, no
test_postgres).

## ⚠ Known red / rotted — THE ONE LIST (post-s279)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` on the affected files = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔
  KEN s217).
- **Client typecheck**: green (`npm run typecheck`). No client changes s279.

### ⚠ Test-run hazards (standing)
- One shared `test_postgres` (RS suite included). Long runs DETACHED.
  Never pipe pytest through `Select-Object`; redirect to a file.
  `poetry run` only from `server\` (the Bash tool silently prints
  NOTHING for poetry one-liners — use the PowerShell tool).
- ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED. Edit tool
  or `[IO.File]` BOM-less UTF8; never `Set-Content`/`Get-Content -Raw`
  for UTF-8. After ANY shell touch of a source file, grep the diff for
  `â€` markers (s279: one Bash-heredoc append — checked clean).
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.

## 🔎 Carried for triage — NOT claims
- (s279 late, entry lane ×2 in one evening) **cleanup `source_verified`
  is ALL-OR-NOTHING per packet** — one open Ken-question warning blocks
  acking the settled ones (clients 4545 + 2665 both held whole on it).
  Per-rule acknowledgment is the ask; chip spawned (task in this
  session). Related NEW ⛔ KEN: **client 4545's D_8606_BASIS_ONLY** — the
  spouse's 8606 carries 6,000 basis forward whole (Part I blank) beside
  an IRA-flagged 89,000 distribution taxed in full; §408(d) pro-rata
  ordinarily applies — a filed-return-right question, the 2665 shape.
  BATCH-011 live-verified by the lane same night: 4545 NO_TIE → TIE at
  the predicted 46,473/47,751/6,917, committed, FILED (not cleanup-
  eligible until the 8606 ruling).
- (s268) 1,604 queries/run + the memoization candidates (s275/276/277).
- (s241) `Form8606`/`HSAAccount` duplicate owners · (s234) the $250k
  nonpassive K-1 AGI gap · (s274) the shared-policy pair 8962 fixture ·
  (s275) `.first()`-on-per-form-rules sweep chip (also `_ctx_al40` /
  the s279 `_l12_overridden` follow the same `.first()` pattern).
- (s279, from delvio-states) the static two-writers guard generalization:
  the diagnostic-code half SHIPPED (`test_diagnostics_code_uniqueness_s279`,
  caught D_RET_012); the FormLine-seeder half (two seeders update_or_create
  the same keyed row) is still unguarded.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried + s279 additions:
Everything from s277/s278 stands (500 spec override/derive conventions;
SCHEDULE_K1_1065 box-13 + box-15 letters; 6251 courtesy pass; 8990
re-authoring gate with its live consumer; 4797 business-use member basis;
lower-tier K-1 vocabulary), plus s279: **AL_FORM_40 needs (a) the line-12
override convention + D_AL40_L12_OVERRIDE seeded upstream, (b) the R-AL-TAX
DOR tax-table midpoint rule** (the continuous-bracket approximation now has
a named $1 miss on a real return); **AL_40NR spec authoring is COMMITTED as
the states lane's next unit (Ken ruling, campaign item 0)**; FORM_8960's
spec is silent on the 4b activity-net attribution the s279 fix encodes
(back-out = activity's line-5 contribution net of its own §179/UPE — should
be a spec rule); FORM_7206 spec should name the S-corp line-11 MeF mapping.

**Seeded-and-exportable (s279 late, Ken's "seed all three"): RS prod 167
forms — `MO_1120`, `MS_83105`, `CO_DR0112` export 200.** Exportable ≠
cleared to build (Ken's scoping call; Dec 1 freeze governs). Two warnings
that TRAVEL WITH those specs into any app build, verbatim from the states
lane:
- **MS_83105**: `MS_FRANCHISE_NET_FLOOR` is the ONE constant that changes
  if the DOR answers its outstanding ticket for §27-13-5(1)(b) (the $25
  statutory franchise minimum) instead of its own line-4 zero-floor
  instruction — every Mississippi bank return moves by $25.
- **CO_DR0112**: do NOT roll forward to TY2026 — four rules change at
  once; it is a re-authoring event, not a rate bump.
