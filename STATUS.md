# TTS Tax App - STATUS (current state only)

*Last updated: 2026-08-01, session 178 (**IMPORT LANE LEG C SHIPPED — the
answer-key reconciliation**, on `slate-ui`, pushed, NO deploy. A payload's
optional `expected` section (the TaxWise answer key) reconciles inside the
Leg B commit: **$5 tolerance on the 2210 penalty ONLY (Ken's ruling, now in
DECISIONS.md); every other line ties exactly.** 10 new tests, both policy
directions revert-proven, FA 521 green. NO new migration — deploy debt
unchanged.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — THE BACK-ENTRY IMPORT LANE, LEG D (the lane is the spine;
## Ken's go 2026-08-01)

**The lane**: industrialize the 420-packet back-entry backlog (Inbox 420 /
Done 40; ~45 min/return via the UI). Legs: **A staging ✅ (`4926fa4`)** →
**B commit ✅ (`aff0025`)** → **C reconciliation ✅ (this session)** →
**D batch Mark-Filed + QA report (NEXT)**.

**Leg C is LIVE in code** — reconciliation inside the commit:
- `backentry.v1` gains an optional **`expected`** section per return —
  `{"federal": {line: $}, "ga500": {line: $}}`, line numbers restricted to
  the reconcilable face lines (typo = loud staging error, never a silently
  skipped comparison). Additive: staging always REJECTED unknown sections,
  so pre-Leg-C payloads are unchanged-valid (no version bump).
- Federal echo/reconcile lines now include **38** (the 2210 penalty):
  11/15/16/22/24/25d/33/34/37/38; GA-500 unchanged (8/16/23/29/30/45/46).
- **Policy (Ken's ruling → DECISIONS.md s178 entry): only penalty
  CALCULATIONS tolerate a difference — federal 38 ties within $5; every
  other line ties exactly.** GA UET is direct-entry (not computed), so no
  GA tolerance line exists in v1.
- `reconcile_expected()` runs inside `commit_staged_return` → the commit
  result carries `reconciliation` (per-line expected/actual/delta/tie +
  verdict); **dry_run previews the verdict without landing anything**.
  A no-tie is RECORDED, never a block — it feeds Leg D's QA report.
- Batch summary rows carry `reconciliation_verdict`; counts add
  `tied` / `not_tied`.

**▶ NEXT — Leg D, batch Mark-Filed + the QA report:**
1. Mark-Filed: per the s175 status ruling (status is a preparer control),
   the lane marks a committed return **filed** once its reconciliation
   verdict is `tie` — decide trigger shape (auto-on-tie at commit vs. an
   explicit batch action `POST .../mark-filed/` that sweeps tied returns).
   Explicit batch action is the safer default — flag for Ken only if he
   wants auto.
2. QA report: per-batch PII-safe report — committed/excluded counts,
   no-tie returns with their per-line deltas, warnings (families left in
   place, non-draft shells). Where it lands: response JSON + printable
   statement page? Decide at build.
3. Fold-ins from the Batch 002 queue: the diagnostics-staleness family
   (#3) — Leg D should report diagnostics from the post-commit compute
   revision, not a stale one.

## QA Batch 002 — remaining queue (paused behind the import lane; the
## diagnostics-staleness family (#3) folds into Leg D)

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

- **s178 — Leg C, the answer-key reconciliation.** Everything in the resume
  block above. Files: `apps/returns/backentry.py` (expected validation +
  `_parse_amount` + `reconcile_expected` + `PENALTY_TOLERANCE_LINES` +
  line 38 in the federal echo), `views_backentry.py` (verdict on staged
  rows, tied/not_tied counts), `tests/test_backentry_reconcile.py` (10
  tests: engine boundaries $3/$5/$6 on line 38, exact-rule $1 no-tie that
  still commits, staging validation, tie rollup, null-when-no-key).
  **Revert-proven BOTH directions**: tolerance disabled → penalty pins
  trip; tolerance-everywhere → exact-rule pins trip. One old staging test
  pin widened for the new counts keys. Gates: backentry 31/31, FA 521.
  NO migration, NO client code.

## Active gates
- **Deployed prod state unchanged from s175b**: `main` == `fdbd7f2`, bundle
  `index-BrbsO-k6.js`, seed debt CLEAR (772 rules both DBs), 0227 applied.
  `slate-ui` is ahead of `main` (s176-s178, un-deployed).
- **⚠ DEPLOY DEBT at the next deploy, BOTH DBs: ① `seed_ga500 --year 2025`**
  (7c `is_computed` flip) **+ ② `seed_rules`** (NEW D_RET_010 + D_RET_003
  rewording) **+ ③ migrations 0228/0229/0230** (back-entry staging tables +
  RLS + the commit-replay fields — none applied locally; the backentry
  endpoints 500 on live until then, which is safe: nothing links to them).
  s178 adds NO new debt.
- **RS agenda (REVIEW_QUEUE s176/s176b)**: ① R-RET-CODE spec edit — code 6
  SUPPORTED (Ken ruled 2026-08-01; app deliberately ahead of spec);
  ② rule-studio `check_ga500_integrity.py` needs the 7c→7a scenario edit.
- ⚠ `LEDGER_AUTOPOST_ENABLED` stays unset until production cutover (Jan 2027).
- ⚠ One test DB — never overlap pytest runs.
- ⚠ The full server suite (~6,900) does NOT finish in a session — this
  session gated on: backentry 31 + FA 521 (server-only change; no client
  code touched, tsc/vitest not re-run).
- ⚠ Browser verification of the 2210 label still owed on the next live pass
  (carried from s176).

## 🔑 Method notes (carried; s178 confirmations)
1. **THE REVERT IS THE ONLY PROOF** — both reconciliation policy directions
   were deliberately broken and each pin tripped (tolerance-off AND
   tolerance-everywhere).
2. **A STATUS claim is a hypothesis** — s177's "staging ignores unknown
   sections" was wrong (staging REJECTS them), which made the `expected`
   section addition cleanly additive with no version bump.
3. **Only-payload-fields discipline** (s175→s177) unchanged in the commit
   lane; Leg C touches no write path.
4. **Authoritative-source rule applied**: GA UET line number is disputed
   between the source brief (44) and the coordinate map/diagnostics (42) —
   GA penalty deliberately left OUT of `expected` v1 rather than guessed.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. **The app is TESTING until January 2027.**

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
- **Slate lane:** `Design/SLATE_IMPLEMENTATION_PLAN.md` ·
  `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md`.
