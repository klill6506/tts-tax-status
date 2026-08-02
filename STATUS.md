# TTS Tax App - STATUS (current state only)

*Last updated: 2026-08-02, session 180 (**🏁 PILOT BATCH 001 RAN THE IMPORT
LANE END-TO-END ON PROD** — 10 real TaxWise packets staged → dry-run →
committed → reconciled → mark-filed → diagnostics → QA reports. **8 of 10
TIED TO THE DOLLAR (federal + GA) and are FILED on prod**; 2 held draft
with fully-diagnosed causes; 1 more return proved the refusal lane. One
real Leg B bug found+fixed+DEPLOYED same session (`facc4d6`). Deploy debt:
ZERO.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — THE PILOT PROVED THE LANE; NEXT IS INDUSTRIALIZING THE
## BACKLOG (blocked on 4 Ken rulings, s180 REVIEW_QUEUE)

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
1. **Ken's 4 rulings (REVIEW_QUEUE s180)**: ① 37/34 inherit the line-38
   tolerance when fully explained by it? ② schema gaps (preparer field /
   1099-R exclusion / claimed-as-dependent / Sch 1-A tips-overtime)?
   ③ identity backfill from the TaxWise roster xlsx (unblocks ssn
   locators + same-name disambiguation)? ④ the 174 invoice-only packets —
   re-export from TaxWise?
2. **Industrialize**: next batches from the 37-candidate pool (~26
   remain), 10/batch, the pilot workflow verbatim. Grow the pool as
   rulings land.
3. Behind the lane: Batch 002 row-creation family (also ChatGPT
   Batch-003 items 6/7), ChatGPT Batch-003 engine claims (ACTC cap ·
   15-yr depreciation · GA low-income credit $5 · 8812 suppression —
   reproduce-first, s175 rule), Ken's ② MeF batch (`build_irs5695`),
   ④ year-constant ruling.

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
