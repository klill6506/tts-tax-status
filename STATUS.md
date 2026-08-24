# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-23 ~23:20 (s280 — two BATCH-296 items built, deployed,
and confirmed; the SC spec amendment Ken-ruled and re-exported same evening).*

*⚠⚠ RESUME POINT — **s280 SHIPPED TWO VERIFIED DEPLOYS.**
**BATCH-296 item 71 CLOSED AND LANE-CONFIRMED** (`457d05c`, deploy
`dep-da5qkhvavr4c73faqqg0` LIVE; annexed): SC Schedule NR line 45 carries
the printed two-decimal PERCENT (4dp ratio) per SchNRInst_2025 lines 45+47
— the whole-percent quantize was $2 of tax on every non-whole ratio. The
filed face refutes BOTH the old rule and full precision (15,435 / 15,398
vs the filed 15,399 — only 0.9777 reproduces it). The entry lane re-staged
the client-3184 part-year GA+SC packet SAME EVENING: **full tie, federal +
GA + SC, first attempt** (commit held only on the lane-wide
preparer-of-record question below). **KEN RULED the RS amendment** (via
delvio-states): `R-NR-PRORATION` now round-4 citing both authorities;
cache re-exported (rides `9c700cb`); **D-36 adopted campaign-wide: where
a state form prints a percentage, the PRINTED percentage is what
multiplies — never the full-precision ratio.**
**BATCH-296 item 62 CLOSED** (`9c700cb`, deploy `dep-da5r5q7avr4c73fb7ifg`
LIVE; annexed): the client-1801 doubling TRACED by a read-only shell probe
— browser residue on **SCH_1 line 21 = 300, OVERRIDDEN** (not line 11);
payload 11:300 + shell 21:300 → line 26 = 600, AGI 300 low. Built: (1)
`_warn_shell_direct_entry_residue` — every commit warns by name on nonzero
direct-entry/OVERRIDDEN lines the payload doesn't re-key, four scopes
(SCH_1 / SCH_3 / SCH_D / 1040-L36); deliberately warn-not-replace because
correction batches are INCREMENTAL. (2) `preparer_due_diligence_attested`
importable **CLEAR-ONLY** (false clears residue, true refuses at staging —
the s237 off-switch rule); published schema regenerated with enum+teaching
description. 77 backentry-commit tests green; 668 across adjacent suites +
flow gate. **The client-1801 unblock recipe is in the annex + sent to the
lane**: `sch1_fields {"11":300, "21":0}` + attestation false + transcribed
f8867_fields; browser entry stays forbidden until that batch commits.*

*▶ NEXT (all remaining units are LARGE — fresh session): per Ken's
priority ruling (1040+1065 first, states in demand-ranked gaps) the
candidates are **BATCH-296 item 24** (1040 Schedule E rental asset ledger
— the #23/#24/#53 remainder, a real feature unit), the **entity-lane
second-state-face transport** (ranked #3 — CO DR-0106 built but
unreachable; the gate every future entity-state build passes), and the
**AL Form 40NR app build** (ranked #4 — spec live + cached, 12 rules / 14
lines / 15 spec tests; encode the s279 build-critical conventions in
STATUS_ARCHIVE + the RS agenda). **1065 BATCH-004 #4 still the only open
batch item, BLOCKED on Codex's Box-2 statement arithmetic** (no addendum
arrived tonight; Ken has the re-ask prompt).*

*▶ AWAITING: (1) entry lane — client-1801 correction batch (recipe above);
client-3184 commit (ties; held on preparer-of-record). (2) **⛔ KEN — the
lane-wide preparer-of-record question** (entry lane raised it: recent
packets print Ken's roster entry on 1040 page 2; one return, client
3543, already FILED carrying the other roster entry off a mis-stated
question — correction put to Ken; rule recorded: **read the paid-preparer
block on 1040 PAGE 2, never the 8879**). (3) states lane — TN_FAE170 and
SC1120 apportionment precision need their own instruction reads under
D-36 (their side); **GA-500 S3-9 stays UNTOUCHED**: engine multiplies at
full precision, and the client-3184 packet's GA side TIES under that —
real evidence Georgia's printed convention may differ; needs IT-511 + the
500 face read before anyone touches it (open, no owner assigned).*

*⛔ KEN — carried unchanged from s279: state-face override-honor
convention; client-2961 AL 40NR source-defect disposition (rides the
40NR Gate-1); the 146-packet re-export (re-export ONE first); NC/CA/SC
linked-state reopens (BATCH-015 #2-4, source-required — Ken re-exports
the state faces); #8 GA-700 Sch 4; #6 1065X/AAR; #68 optimizer; s274 PII
items; RS 8990 re-authoring gate; Form 6765 Section G (TY2026+);
client-4545 D_8606_BASIS_ONLY question; per-rule cleanup acknowledgment
(chip open).*

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
annexes too (s280 held to it: both annexes carry the full record). GATE
EXCEPTIONS stay human end-to-end IN the acting session. **Never relay
tokens through the message channel.** ONE delvio-tax tree holder; ONE
pytest/test_postgres holder (s280 held both; states session was RS-side
only).

## ⚠ Known red / rotted — THE ONE LIST (post-s280)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` on the affected files = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔
  KEN s217).
- **Client typecheck**: green (`npm run typecheck`). No client changes s280.

### ⚠ Test-run hazards (standing)
- One shared `test_postgres` (RS suite included). Long runs DETACHED.
  Never pipe pytest through `Select-Object`; redirect to a file.
  `poetry run` only from `server\` (the Bash tool silently prints
  NOTHING for poetry one-liners — use the PowerShell tool).
- ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED. Edit tool
  or `[IO.File]` BOM-less UTF8; never `Set-Content`/`Get-Content -Raw`
  for UTF-8. After ANY shell touch of a source file, grep the diff for
  `â€` markers (s280: all appends + one line-surgery move checked clean).
- ⚠⚠ **`Measure-Object -Line` DOES NOT COUNT BLANK LINES** — never use it
  to find a file's end (s280: it said 2232 for a 2579-line file; the Edit
  anchored on a false tail and SPLIT a test class — pytest caught it,
  py_compile did not). Use `[IO.File]::ReadAllLines(...).Count`.
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.

## 🔎 Carried for triage — NOT claims
- (s280, states lane) **"A fixture that cannot fail is not a test"** —
  the SC spec's two scenarios (0.60 / 1.00) were exact at EITHER
  precision and could never catch the precision defect. Sweep candidate:
  regression fixtures whose inputs are insensitive to the rule they
  nominally cover.
- (s279 late) cleanup `source_verified` all-or-nothing per packet —
  per-rule acknowledgment chip open; client-4545 D_8606_BASIS_ONLY ⛔ KEN.
- (s268) 1,604 queries/run + memoization candidates (s275/276/277).
- (s241) `Form8606`/`HSAAccount` duplicate owners · (s234) the $250k
  nonpassive K-1 AGI gap · (s274) shared-policy pair 8962 fixture ·
  (s275) `.first()`-on-per-form-rules sweep chip.
- (s279) FormLine-seeder two-writers guard (diagnostic-code half shipped;
  seeder half unguarded).

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried + s280 changes:
Everything from s277/s278/s279 stands EXCEPT: ~~FORM_8960 4b attribution~~
(carried), **SC_SCHEDULE_NR R-NR-PRORATION ✅ AMENDED AND RULED (s280,
D-36)** — round-4, citing SchNRInst_2025 line 45 + the face's line 47;
cache re-exported at `9c700cb`. NEW: D-36 instruction-reads for TN_FAE170
R-TN-SCHN + SC1120 R-SC1120-APPORT (states lane holds); GA-500 S3-9
precision question (see AWAITING — evidence cuts BOTH ways, untouched).
AL_FORM_40NR build conventions (printed-percent ×, whole-dollar floors,
HOF column, two-column floors) recorded in s279's STATUS_ARCHIVE entry.
