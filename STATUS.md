# TTS Tax App - STATUS (current state only)

*Last updated: 2026-08-02, session 180/b/c (**🏁 THE LANE IS INDUSTRIAL:
20 REAL RETURNS IMPORTED TONIGHT, 19 TIED TO THE DOLLAR AND FILED.**
s180: pilot 10/10 (after s180b's four Ken rulings). s180c: batch-002 ran
9/10 tied+filed SAME NIGHT and grew the schema four more times —
capgains family `d894ba4`, car-loan family `fbbf6af`, dependent
tin_type + dependent_filer_earned_income `50bcb69` (all deployed). The
ONE hold: BARROW, on the GA low-income-credit exemption-count question
(= ChatGPT Batch-003 item 3, REVIEW_QUEUE). Triage v3: 34 were
ready-now, +18 unlocked by capgains; 175 HARD (Sch C/E/A + depreciation
era). Deploy debt: ZERO.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — BATCH-003 IS NEXT: ~42 lane-eligible packets remain
## (triage_v3_buckets.csv: the rest of READY_NOW + the 18 capgains
## unlocks), 10/batch, workflow verbatim. The AUTHORING_GUIDE now carries
## EVERY convention batches 001-002 earned: preparer · GA line-5 letter ·
## GA 7a dependent count · dependent tin_type/citizenship on
## credit-claiming returns · dependent_filer_earned_income · 1099-R
## post-exclusion taxable · capital_transactions · car_loan_vehicles ·
## UNKNOWN-payer interest. Residuals: BARROW held on the GA LIC
## exemption-count ruling (REVIEW_QUEUE s180c) · FINLEY which-is-which ·
## RS agenda retirement-exclusion input · the 174 quarantined
## invoice-only re-exports · 175 HARD packets await the Sch C/E/A era.

**Pilot-001 results (batches `pilot-001`/`-001b`/`-001c` on prod, dev QA
account, 2026-08-01/02 night):**
- **8 FILED, tie 17/17 lines** (client numbers 1953/2095/2182/2223/2251/
  2595/2763/3295): W-2, INT (4-payer Sch B), DIV, 1099-R (codes 3/4/7/Q,
  IRA box), SSA, Sch 1-A senior deduction, age-additions std deduction,
  MFJ + GA filing status B, EIC-decline, full-refund and zero-tax shapes —
  all exact, federal AND GA-500.
- **2 committed, HELD draft** (the lane's designed outcome): #1995 —
  1099-R $5,400 flag-E exclusion not expressible in backentry.v1 (schema
  gap); #2743 — 2210 penalty 83 vs 82, INSIDE the $5 line-38 tolerance but
  the $1 flows into untolerated line 37 (policy question).
- **1 refused by design**: a same-name hub client pair (two identical
  name rows, #2129/#2130), ssn+last_name locator cannot resolve (no
  identity rows) — staged INVALID, excluded. Proves the never-guess rule.
- Mark-filed swept ONLY ties (7+1 via correction batch); post-commit
  diagnostics ran on all 10 (no error-severity findings; MISSING_PREPARER
  warns on all — schema gap); reports saved:
  `D:\tax-test-data\tmp\pilot-001\pilot001*_report.json`.

**Shipped this session:**
- **`facc4d6` (DEPLOYED prod+demo): Leg B commit coerces payload JSON
  scalars through each model field's clean()** — raw setattr left
  date_of_birth as a STRING on the in-memory Taxpayer; on a VIRGIN shell
  `Taxpayer(tax_return=target)` caches that instance, so same-transaction
  compute died str-vs-date → 500 on EVERY real senior packet. Regression
  pin on a virgin shell; revert-proven. Backentry 39/39, FA 521.
- **Workflow artifacts** (in `D:\tax-test-data\tmp\pilot-001\`):
  `AUTHORING_GUIDE.md` (the payload-authoring spec agents follow; now
  carries the GA line-5 rule), `pilot_driver.py` (auth/stage/dryrun/
  commit/markfiled/report CLI against prod), `triage_inbox.py` +
  results CSV (classifies all 420 packets by the invoice's billed-forms
  list), `RUNBOOK.md`, per-return payloads/notes. 10 entered PDFs moved
  to Done with notes.

**Key workflow facts (industrialization inputs):**
- **Locators must be client_number** — virgin shells have NO Taxpayer row
  (93 of 2,831 have one), so ssn+last_name resolves nothing until an
  identity backfill lands (REVIEW_QUEUE).
- **ga500_fields must always carry {"5": "<letter>"}** — GA-500 line 5 is
  preparer-entered by design; empty = "A" default = single $12,000 GA std
  deduction (an MFJ return silently overstates GA tax by 5.19% × $12,000 —
  caught by reconciliation, fixed via correction batch `pilot-001c`).
- **Inbox inventory: only 246 of 420 packets are real** — 174 are
  invoice-only PDFs (no return inside); 37 of the 246 are lane-eligible
  under the current schema (triage CSV). The rest need UI entry or schema
  extensions.
- Per-return authoring = 1 subagent reading the full packet (~4-5 min,
  parallelizes ×10); dry-run precommit preview works BOTH via prod
  endpoint and via local rollback script (same code, same DB).

**▶ NEXT:**
1. **Industrialize**: next batches from the candidate pool (~26 remain
   of the 37), 10/batch, the pilot workflow verbatim — subagent
   authoring per the updated AUTHORING_GUIDE (preparer section + GA
   line-5 letter + post-exclusion 1099-R taxable), stage → dry-run →
   commit → mark-filed → report. Re-run the triage on the cleaned Inbox
   to refresh the pool as needed.
2. Residuals: FINLEY which-is-which (Ken); RS agenda — retirement-spec
   exclusion input (then the engine field + payload field); the 174
   quarantined invoice-only re-exports (whenever convenient).
3. Behind the lane: Batch 002 row-creation family (also ChatGPT
   Batch-003 items 6/7), ChatGPT Batch-003 engine claims (ACTC cap ·
   15-yr depreciation · GA low-income credit $5 · 8812 suppression —
   reproduce-first, s175 rule), Ken's ② MeF batch (`build_irs5695`),
   ④ year-constant ruling.

**s180c (same night — batch-002 + four schema growths, all deployed):**
- **Batch-002: 10 packets (A–B names incl. ChatGPT's started shells —
  replace_documents overwrote partial UI entries cleanly), 9 tied+filed.**
  Every dry-run miss was diagnosable to a payload/allowlist cause and
  fixed the same hour: dependent `tin_type` blank zeroed EVERY child
  credit + EIC (CTC needs valid_ssn — on a completed return the TaxWise
  filing + 8867 IS the eligibility evidence, transcribed);
  `dependent_filer_earned_income` is the R-STD-04 worksheet's dedicated
  input (both BRAYs got the $1,350 floor without it — AGI−1,350 to the
  dollar, and with it both match their packets' printed limited std);
  GA-500 **7a** (dependent count) is preparer-entered like line 5 —
  missing = the $4,000/dependent exemption drops (BURROUGHS, exactly
  207 = 5.19%×4,000).
- **Schema families added + deployed**: `capital_transactions` (8949
  rows on CapitalTransaction; box A–L routing; broker summaries; Sch D
  carryovers via taxpayer fields — test pins 8949→Sch D→AGI exact) and
  `car_loan_vehicles` (Sch 1-A Part IV QPVLI; attestation booleans
  default true). Backentry suite now 44 tests.
- **Engine evidence BOTH ways**: BROWN CHANELL's EIC 6,651 + ACTC 4,119
  computed EXACT (counter-evidence to Batch-003's "ACTC capped at $21"
  as a general bug) · BARROW pins the GA LIC exemption-count divergence
  (Batch-003 item 3) to a precise $5 — HELD on that ruling.
- **Triage v3** (invoice billed-forms, benign/computed lines reclassified):
  34 ready-now · +18 capgains · 0 for a 1099-G family (not built) · 175
  HARD (Sch C 54 · Sch E 54 · 4562/depr 47/44 · SE 33 · Sch A 31 · K-1
  21 · 8889 19...). `D:\tax-test-data\tmp\pilot-001\triage_v3_buckets.csv`.

**s180b (same session, Ken ruled live one-at-a-time):**
- **Tolerance flow-through** `4e0558e`: 37/34 tie when their delta is
  EXACTLY the (within-$5-tolerance) line-38 delta; `tolerated_by: "38"`;
  DECISIONS.md s180; revert-proven. JONES's stored verdict recomputed
  from her stored commit data → tie → FILED.
- **Schema extensions** `1a0c3df` (DEPLOYED, live-proven via pilot-001d):
  `preparer` section roster-resolved PTIN-first (unknown → warning,
  never a guess) and assigned to federal + attached states;
  claimed-as-dependent boxes; Sch 1-A tips/overtime inputs. The 1099-R
  exclusion is an AUTHORING CONVENTION (post-exclusion taxable) because
  the RS retirement spec has no exclusion rule — spec-first.
- **DUNN corrected end-to-end on prod**: exclude-and-correct pattern ×2
  now (dorsey/001c, dunn/001d): re-staged with the exclusion convention
  + preparer, merge=replace_documents, 17/17 TIE, FILED, fresh
  diagnostics clean of MISSING_PREPARER. **Pilot final: 10/10 tied+filed.**
- **Identity backfill: 2,001 PRIMARY SSNs** into clients_tax_identity
  via `upsert_identity` (strict 3-way roster match; every ambiguity
  skipped + counted; all 11 pilot identities cross-checked exact).
- **Housekeeping**: 174 invoice-only PDFs → `Inbox\_invoice_only\`;
  public-mirror history force-scrubbed (Ken's go — the never-rewrite
  rule stands for code repos).

## QA Batch 002 — remaining queue (unchanged from s179; paused behind the
## lane; ChatGPT Batch-003 adds overlapping items 6/7 = family #1)
1. Row-creation transactional family (add-lanes minting silent duplicates).
2. Rental `+ Add property` silently inert (blocker-class).
3. Diagnostics-staleness SCREEN-side fix (report side served by Leg D).
4. K-1 material participation defaults to unsupported YES.
5. Diagnostic gating: D_6251_008 / D_GA500_008.
6. GA retirement-exclusion feeders (Sch C earned income + Sch D gains).
7. Form 8995 prior-year QBI loss carryforward inputs.
8. Schedule A Medicare feeder provenance.
9. Form 8960 filed PDF omits pure-arithmetic lines.
10. Source-summary INT box-1 400 unwired.
11. PDF preview canvas race.
12. Stale totals strip on Slate INT/DIV screens.

## Active gates
- **Deploy debt: ZERO** — `main` == `facc4d6`, deployed prod+demo this
  session (server-only; verified by behavior: the pre-fix dry-run 500
  became a 200-tie on prod). No pending migrations/seeds.
- **RS agenda (unchanged)**: R-RET-CODE code-6 edit; rule-studio
  check_ga500_integrity 7c→7a scenario edit.
- **REVIEW_QUEUE**: 4 NEW s180 items (above) + GA UET line dispute (s178).
- ⚠ `LEDGER_AUTOPOST_ENABLED` stays unset until production cutover (Jan 2027).
- ⚠ One test DB — never overlap pytest runs.
- ⚠ ChatGPT works the SAME Inbox alphabetically from the top (Batch-003 =
  ADKINS→[client]); import-lane batches pick from later letters. His
  Batch-003 CC list is in `D:\tax-test-data\QA Reports\Batch-003\`.

## 🔑 Method notes (s180 additions)
1. **The reconciliation IS the QA** — it caught the GA line-5 default
   (623 GA tax overstatement) that eight green federal lines hid.
2. **Pre-register expected outcomes before running** (RUNBOOK.md) — the
   two no-ties were predicted with their exact causes before any commit.
3. **A dry-run 500 reproduces locally against the same prod DB** with
   commit_staged_return inside a forced-rollback transaction — full
   traceback, zero writes, no Render log access needed.
4. **The staged copy is FROZEN at staging** — a payload fix needs
   exclude + a new correction batch (pilot-001c pattern), not a file edit.
5. ⚠ `poetry -C server run python` resolves ALL relative paths against
   server/ — absolute paths for scripts AND their file arguments.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. **The app is TESTING until January 2027.**

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
- **Slate lane:** `Design/SLATE_IMPLEMENTATION_PLAN.md` ·
  `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md`.
