# TTS Tax App - STATUS (current state only)

*Last updated: 2026-08-01, session 177 (**IMPORT LANE LEG B SHIPPED — the
per-return atomic commit**, `aff0025` on `slate-ui`, pushed, NO deploy.
`POST /backentry/batches/{id}/returns/{return_key}/commit/` lands one staged
return whole or not at all; merge gate, idempotent replay, dry_run, exclude,
batch status transitions. 11 new tests, three behaviors revert-proven, FA 521
green. ⚠ deploy debt adds **migration 0230**.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — THE BACK-ENTRY IMPORT LANE, LEG C (the lane is the spine;
## Ken's go 2026-08-01)

**The lane**: industrialize the 420-packet back-entry backlog (Inbox 420 /
Done 40; ~45 min/return via the UI). Legs: **A staging ✅ (`4926fa4`)** →
**B commit ✅ (this session, `aff0025`)** → **C reconciliation (NEXT)** →
D batch Mark-Filed/QA report.

**Leg B is LIVE in code** — one transaction per return:
- Taxpayer allowlist fields + document rows (W-2 w/ state entries, INT, DIV,
  R, W-2G, dependents) as **direct model creates with ONLY payload fields**
  (no serializer, so no serializer defaults — the s175 line-C rule held).
- `ga500_fields` land as preparer entries (`is_overridden=True`,
  `updated_by` — the update_fields convention); unknown GA line = 400 +
  full rollback. GA-500 auto-attaches via the UI's own `_auto_sync_ga500`
  chokepoint; force-created when `ga500_fields` demand it w/o a GA doc.
- **Merge gate**: non-empty target → 409 unless `{"merge":
  "replace_documents"}`; the gate covers all SIX row families (wider than
  the staging warning's four — W-2G/dependents would double just as
  silently). Replace deletes ONLY sections the payload supplies; families
  left in place are reported as warnings.
- Idempotent replay (re-POST → stored `commit_result`, new model fields
  `committed_at`/`commit_result`, migration 0230); `dry_run=1` runs the
  FULL commit (writes + compute + summaries) inside a rolled-back
  transaction; `exclude` action; batch staged→partial→committed.
- Response echoes computed face lines (federal 11/15/16/22/24/25d/33/34/37;
  GA-500 8/16/23/29/30/45/46) — **these are Leg C's reconciliation inputs.**

**▶ NEXT — Leg C, the reconciliation workspace:**
1. The packet's TaxWise answer key (expected refund/due, withholding, AGI,
   GA figures) needs a home in the schema — likely an optional `expected`
   section per return (additive to `backentry.v1`; staging ignores unknown
   sections today, so decide version bump vs. additive allowlist).
2. Compare committed/computed face lines (already echoed by Leg B) against
   expected; **$5 tolerance on the Form 2210 penalty is KEN'S RULING —
   record it in DECISIONS.md when built.** Exact-match policy for the other
   lines is undecided — flag for Ken.
3. Surface per-return tie/no-tie verdicts on the batch summary; a no-tie
   feeds the QA report (Leg D), not an auto-fix.
4. Then Leg D: batch Mark-Filed + QA report (fits the s175 status ruling:
   the agent marks filed once it ties).

## QA Batch 002 — remaining queue (paused behind the import lane; the
## diagnostics-staleness family (#3) folds into Leg C/D when needed)

⚠ **Re-verify every screen-layer item on the DEPLOYED Slate first** — Batch 002
was QA'd before the 2026-08-01 ~22:40Z deploy (the s175 lesson: 2 of 6 items
had already stopped existing).

1. **The row-creation transactional family** — INT/DIV payers, dependents,
   K-1, W-2G, 2210 dated payments: no pending state, retries mint silent
   duplicates (also the real cause of the "W-2G counted twice" report).
   Design: optimistic row w/ stable ID, disable-while-pending, reconcile by
   ID; audit which add-lanes lack `@idempotent_create`.
2. **Rental `+ Add property` silently inert** (blocker-class).
3. **The diagnostics-staleness family** — findings published from a different
   calculation revision than the one on screen.
4. **K-1 material participation defaults to an unsupported YES.**
5. **Diagnostic gating**: `D_6251_008` on bare LTCL carryover; `D_GA500_008`
   with zero depreciation facts.
6. **GA retirement-exclusion feeders**: Sch C earned income + Sch D gains
   unfed (correct-by-coincidence at the $65k cap).
7. **Form 8995 prior-year QBI loss carryforward inputs.**
8. **Schedule A Medicare feeder provenance** (double-count trap).
9. **Form 8960 filed PDF omits pure-arithmetic lines** (5d/9d/11/14/15).
10. **Source-summary INT box-1 400** — `FORBIDDEN_ON_SUMMARY`/
    `summaryConflicts` unwired in `SourceSummary.tsx`.
11. **PDF preview canvas race** — cancel/await prior render per revision.
12. *(carried, s175)* Stale totals strip on Slate INT/DIV screens.

## What shipped this session (`slate-ui`, pushed, NO deploy)

- **`aff0025` (s177) — Leg B, the per-return atomic commit.** Everything in
  the resume block above. 11 tests in `test_backentry_commit.py` (happy
  path incl. only-payload-fields pin — a shell Taxpayer field NOT in the
  payload survives; replay; merge refuse + section-granular replace;
  dry-run rollback; GA-500 preparer entries; unknown-GA-line atomicity;
  invalid/excluded refusals; batch transitions; cross-firm 404; auth).
  **Revert-proven**: merge gate, `is_overridden` convention, and dry-run
  rollback each break their pins when disabled. Gates: backentry 21/21,
  FA 521, migration 0230 generated (NOT applied locally — shared-prod DB).

## Active gates
- **Deployed prod state unchanged from s175b**: `main` == `fdbd7f2`, bundle
  `index-BrbsO-k6.js`, seed debt CLEAR (772 rules both DBs), 0227 applied.
  `slate-ui` is now 11 commits AHEAD of `main` (s176–s177, un-deployed).
- **⚠ DEPLOY DEBT at the next deploy, BOTH DBs: ① `seed_ga500 --year 2025`**
  (7c `is_computed` flip) **+ ② `seed_rules`** (NEW D_RET_010 + D_RET_003
  rewording) **+ ③ migrations 0228/0229/0230** (back-entry staging tables +
  RLS + the commit-replay fields — none applied locally; the backentry
  endpoints 500 on live until then, which is safe: nothing links to them).
- **RS agenda (REVIEW_QUEUE s176/s176b)**: ① R-RET-CODE spec edit — code 6
  SUPPORTED (Ken ruled 2026-08-01; app deliberately ahead of spec);
  ② rule-studio `check_ga500_integrity.py` needs the 7c→7a scenario edit.
- ⚠ `LEDGER_AUTOPOST_ENABLED` stays unset until production cutover (Jan 2027).
- ⚠ One test DB — never overlap pytest runs.
- ⚠ The full server suite (~6,900) does NOT finish in a session — this
  session gated on: backentry 21 + FA 521 (server-only change; no client
  code touched, tsc/vitest not re-run).
- ⚠ Browser verification of the 2210 label still owed on the next live pass
  (carried from s176).

## 🔑 Method notes (carried; s177 confirmations)
1. **REPRODUCE BEFORE BUILDING** / **THE REVERT IS THE ONLY PROOF** — Leg B's
   three key behaviors were deliberately broken and each pin tripped.
2. **A QA-PRESCRIBED CITATION IS ALSO A HYPOTHESIS.**
3. **CHECK THE SPEC BEFORE CALLING IT A BUG.**
4. **Only-payload-fields discipline** (s175→s177): the commit lane never
   touches a serializer; a field the payload doesn't carry keeps its value.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. **The app is TESTING until January 2027.**

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
- **Slate lane:** `Design/SLATE_IMPLEMENTATION_PLAN.md` ·
  `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md`.
