# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-06, session 216 (the CC-Changes loop). This session
CLOSED **BATCH-012** — all 10 items, ONE deploy `7c5ac63`, migration 0252.
The file is annexed and moved to `CC Changes Done`. **The CC-Changes queue is
EMPTY; the watcher re-arms.***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax`; demo = `tts-tax-demo`. **The orphan third service
`tts-tax-app` still fails every push — Ken should delete it.**

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

---

## ▶ RESUME HERE

### The queue right now
`D:\tax-test-data\1120S\CC Changes\` holds **only its README** — batch-012 is
closed and there is nothing queued. Next session: sweep the folder at boot;
if a batch-013 has landed, work it. Otherwise Ken directs.

**One item is deliberately QUEUED for batch-013**: the signed/explicit-zero
Schedule L replacement recurrence that batch-012 posted as supporting evidence
rather than an eleventh item (`L25a = -20,000` / `L25d = 0` transferring
20,000 between the beginning and ending totals). It is a different mechanism
from the ten built — direct-field application of signed and explicit-zero
values, not row replacement — and folding it into this deploy would have meant
shipping it unverified against the rest. The annex asks the entry agent to
re-post it first in batch-013, **and to re-run that packet after this deploy
before re-reporting**: items 5 and 6 both touch L24d/L27d, so the deltas may
have moved.

### ✅ s216 in one paragraph
BATCH-012, all ten items, one deploy. **Verify-first paid four times — three
reported causes were understated, and verifying a fourth item turned up a
defect nobody reported.** #3's "the wrong location for this GA presentation"
was far worse than that: `INT_GA_BONUS_ADDBACK` read `S1_3`, which on the
GA-600S the app actually seeds is **"Total Income (Add Lines 1 and 2)"** — a
Schedule 1 total unrelated to depreciation on any return, a leftover from the
RS stub spec whose 4-line `line_map` still calls S1_2/S1_3
additions/subtractions. So the rule was **both** a false error whenever
Schedule 1 total income was zero **and silently satisfied whenever it was
not** — the more dangerous half, since a genuinely missing Georgia add-back
would never have fired. #1's "book Schedule L vs tax register" is right in
substance but not in its evidence: the packet keys **BOY only** and the app
rolls EOY forward, and its answer key restricts no L10 line at all, so the
figure the finding called "source EOY" was ours. The real argument is
stronger — Schedule L is *"Balance Sheets per Books"* (i1120s), M-1 exists to
reconcile the difference, and the **RS 1120S_SCHL spec has no tie diagnostic**
— so error severity was never defensible. #6's ask needed three exclusions it
did not name (tax-exempt interest, nondeductible expenses and the §179
disposition gain are all already in the M-2 columns; including them would
double count). **And verifying #1 found four register rows whose prior bonus
is exactly 1.8× gross cost — 1 + 0.8 in an 80% bonus year, i.e. the cost added
into the bonus column. `D_4562_BASIS` had caught it as an acknowledgable
warning and it was acknowledged away; recovered basis above what was paid is
impossible, so it now errors.** Built as asked: #2 the fully-recovered
table exemption, #4 the full federal→GA pull for an already-linked GA-600S,
#5 the derived-line release on a nonempty replacement, #7 per-asset netting of
the GA gross pair, #8 the guarded bootstrap attach mode, #9 `amt_cost_basis`,
#10 the Form 4562 line 16 input.

### ⚠ Classes that MOVE existing returns or output on next recompute
1. **Diagnostics — holds become CLEARABLE**: `SCHED_L_DEPR_TIE` is now a
   warning on every return (no book register exists yet); `D_4562_METHOD` and
   `INT_GA_BONUS_ADDBACK` stop firing on the shapes above. Returns held on
   those three can close out.
2. **Diagnostics — a NEW hold**: `D_4562_BASIS` ERRORS when prior bonus +
   prior §179 exceeds original cost. Previously acknowledgable. Any committed
   return carrying that impossible shape will now hold. ⛔ intended, but Ken
   should know.
3. **Print — the Georgia face**: the S7_8/S8_5 pair moves on any return whose
   register mixes equal and unequal federal/Georgia assets. Net Georgia income
   unchanged; the printed pair is not. Returns pinned to the old aggregate
   figures will no-tie.
4. **Compute — L24d/L27d**: ending book retained earnings moves on any return
   with M-1 lines 2 / 3a / 5b-rows / 6a / 6b. Book=tax returns unmoved, and
   the M-2 face never changes.
5. **Compute — page-1 line 14 / 4562 line 22**: only where `D_4562_L16` is
   nonzero, i.e. only after someone keys it.
6. **Commit — derived-line release**: any `replace_documents` commit now
   releases its families' derived lines. A shell holding an aggregate that
   disagreed with its rows was already computing a wrong number.

### ⚠ Known red / rotted (not this session's changes)
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  the batch-005 #9 PTET-gate class, red on main since s212.
- `test_mef_scenario5_1120s_compute.py` (both tests) — `M2_3a` computes 4,975
  against an expected 5,461. **Re-verified this session on a clean stash: the
  failure is byte-identical without any s216 change.** s213 changed the
  K16a→OAA routing per i1120s and the ATS scenario-5 expectation was never
  re-derived. ⛔ KEN: an IRS ATS scenario with a published answer key needs a
  ruling, not a test edit.
- closeout↔cleanup SUITE-ORDER contamination: `test_backentry_cleanup.py`
  fails 3 tests on a `DiagnosticRule` unique-code collision when run after
  the closeout files. **Green alone (6 passed).** Pre-existing, unchanged.

### ⚠ A SIBLING SESSION IS EDITING THIS REPO
At commit time the working tree carried uncommitted changes in
`server/apps/clients/views.py` and `server/tests/test_clients.py` that this
session did not make — a client duplicate-gate + rename action, dated today.
**s216 committed only its own paths and left those files alone.** Whoever owns
that work still needs to commit it. (The s208c rule applies: never
`git checkout --` or blanket-stash in a repo a sibling session is using.)

### Artifacts left on the shared DB (deliberate, s216)
- Migration 0252 rides the deploy — one NULLABLE `DepreciationAsset` column,
  so no `db_default` is needed (the s190 deploy-skew rule covers NOT NULL).
  No new tables, no RLS pair.
- One new seeded 1120-S FormLine (`D_4562_L16`) lands via `seed_all` on
  deploy, the standard path.
- No probe rows. Every test ran against the test DB.

### ⛔ KEN DECISIONS OUTSTANDING
- **NEW (s216)**: `D_4562_BASIS` warning→error escalation (class 2 above).
- **NEW (s216)**: the L24d current-year book bridge is a DERIVATION from
  i1120s M-1/M-2 mechanics, not a quoted RS rule — needs ratification.
- Carried (s215): `D_4562_VCLASS` warning→error escalation.
- Carried (s215): the ATS scenario-5 `M2_3a` expectation vs the s213 K16a→OAA
  routing — which is right?
- Form 6765: RS spec authored s212, ⏳ awaiting Ken's seed approval.
- M2_3a auto-rollup question (s213): should shareholder capital contributions
  EVER auto-route into AAA? Built as the explicit `M2_3A_OTHER` input only.
- The A–M item-7 asset decisions; states + K-2/K-3 holds (unchanged).

### RS AGENDA (s216 additions — built per primary sources, need ratification)
- **Schedule L L24d**: the current-year book/tax bridge
  (`D_M1_5B_ROWS + M1_6a + M1_6b − M1_2 − M1_3a`) and the three exclusions
  that keep it from double counting what the M-2 columns already carry.
- **GA-600S**: the Schedule 7/8 pair is built per-asset; an asset whose two
  arms are equal contributes to neither side. Generalises the batch-004 #2
  whole-register suppression.
- **GA-600S**: `S1_3` is a Schedule 1 TOTAL, not a bonus addback — the RS
  spec's 4-line `line_map` (S1_2 "Georgia additions" / S1_3 "Georgia
  subtractions") does not describe the form the app seeds and should be
  re-exported against the real DOR layout.
- **Form 4562**: line 16 is "Other depreciation (including ACRS)" — an
  independent amount, not a subtotal; it joins line 22 and page-1 line 14.
- **Form 4562 / §168(k)**: the special allowance is a percentage of the
  depreciable basis and cannot exceed 100% of it, so prior bonus + prior §179
  above original cost is an error, not a warning.
- **Form 1120-S Schedule L**: no IRS tie is required between Schedule L and a
  TAX depreciation register (Schedule L is per books; M-1 reconciles).
- Carried s215 items (7203 line 3m · §280F · Schedule L fully-recovered
  subset), s214 (8949 columns (f)/(g) · K15a passthrough · 2553), s213 (4562
  R008/R019 §280F family · 1120S_M2 K16a→OAA and the FA005 assertion · Sch L
  L24d carry · K15b sign) and s212 (GA600S S1 PTET gate · 6765 · M1-6b ·
  8825 22a/22b) unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — unchanged.
