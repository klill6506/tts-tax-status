# TTS Tax App - STATUS (current state only)

*Last updated: 2026-08-01, session 176 (**QA BATCH 002, LEG 1 — the engine-claim
triage**. Ken relayed ChatGPT's 10-return Batch 002 (~18 defect groups). The
s175 method held: of the four engine claims examined, TWO did not reproduce —
the "W-2G counted twice" prescription blamed the 1040 income math and the
engine is provably clean (the +$2,440 is the add-row retry race minting hidden
duplicate cards); the code-6 1099-R blanking is the Ken-ruled RS spec behaving
as designed WITH its RED firing. Two were real and are FIXED: the GA-500 7c
derive and the direct-field-save GA resync. Plus the 2210 label, verified
against the 2025 i2210 fetched from irs.gov.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — QA Batch 002, remaining queue (in priority order)

⚠ **Re-verify every screen-layer item on the DEPLOYED Slate first** — Batch 002
was QA'd before the 2026-08-01 ~22:40Z deploy (the s175 lesson: 2 of 6 items
had already stopped existing).

1. **The row-creation transactional family** — the batch's most-repeated theme
   (INT/DIV payers, dependents, K-1, W-2G, 2210 dated payments): no pending
   state, delayed render, retries mint silent duplicates. This is ALSO what
   actually caused the "W-2G counted twice" report (three cards created by
   retries; the UI list showed one, the server summed them all). One design:
   optimistic row w/ stable ID, disable-while-pending, reconcile by ID,
   inline error. Server idempotency: note `@idempotent_create` already exists
   on schedule_c — audit which add-lanes lack it.
2. **Rental `+ Add property` silently inert** (blocker-class: forced a
   Schedule 1 direct-entry fallback that discards Sch E/4562/8995 identity).
3. **The diagnostics-staleness family** — stale GA rail amounts after
   recompute, `D_GA500_010` predicate stale after UET entry, 8867 cascade
   cleared only by an unrelated save, "no affirmative run" control. One cause
   family: findings published from a different calculation revision than the
   one on screen.
4. **K-1 material participation defaults to an unsupported YES** — needs an
   unanswered state + diagnostic gate (passive-loss/QBI consequences).
5. **Diagnostic gating**: `D_6251_008` fires on a bare LTCL carryover;
   `D_GA500_008` fires with zero depreciation facts (repeat from Batch 001).
6. **GA retirement-exclusion feeders**: materially-participated Sch C earned
   income (worksheet L2/L5, $5,000 cap) + Sch D transaction gains (L9) both
   unfed — correct-by-coincidence only at the $65k cap.
7. **Form 8995 prior-year QBI loss carryforward inputs** (repeat gap,
   re-confirmed on a rental-loss scenario).
8. **Schedule A Medicare feeder provenance** (double-count trap: SSA screen
   auto-feeds premiums; direct entry of the gross total double-counts).
9. **Form 8960 filed PDF omits pure-arithmetic lines** (5d/9d/11/14/15 print
   blank while the screen computes them).
10. **Source-summary INT box-1 400** — carried from s175 (#6), re-confirmed
    on two more returns: `FORBIDDEN_ON_SUMMARY`/`summaryConflicts` exist
    unwired in `SourceSummary.tsx`; disable the cells, surface the rejection.
11. **PDF preview canvas race** ("Cannot use the same canvas during multiple
    render() operations") — cancel/await prior render per revision.
12. *(carried, s175)* The stale totals strip on Slate INT/DIV screens.

## What shipped this session (all on `slate-ui`, pushed, NO deploy)

- **`6239d5e` — GA-500 line 7c derives from 7a + 7b** (2025 Form 500 face:
  "Total Number of Dependents"; verified against the PDF template). A
  preparer-saved 7c (`is_overridden`) still wins — safe for every existing
  return because `update_fields` marks ALL manual edits overridden. Was: 7a=2
  alone left 7c blank → line 14 = $0 (an $8,000 exemption missed, ~$416 GA
  tax). SAME COMMIT: LIC exemption count now EXCLUDES unborn dependents
  (IT-511 p35 / source-brief §8 — it was counting 7c whole; second latent
  defect in the same block). seed_ga500: 7c flipped to computed.
  **Revert-proven (6 tests fail on the pre-fix code).**
- **`9f9322e` — the direct field-save lane now resyncs the attached GA-500.**
  `update_fields` (and bare `/compute/`) called `compute_return` directly and
  skipped `_auto_sync_ga500` — every document-CRUD lane routes through
  `_recompute_1040`, which runs it. Exactly the s175 rental-mutation shape,
  one lane over. Was: a Schedule 1 direct entry moved federal AGI, Form 500
  line 8 stayed stale until manual Refresh. Revert-proven via the new API test.
- **`8169369` — 2210 prior-year safe-harbor label** now states the 2025 i2210
  Line 8 chart definition (line 22 + LISTED Sch 2 taxes − refundable credits).
  The old label (line 22 alone) dropped SE tax from the safe harbor; the QA
  prescription (line 24) was also imprecise — verified verbatim against
  i2210 (2025) fetched from irs.gov, cited in the JSX comment.
- **`4ff4b0a` — the two no-repro pins** (`test_qa_b002_w2g_double_count.py`,
  `test_qa_b002_code6_1099r.py`): W-2G card path == other-winnings path and
  line 9 internally consistent; code-6 blanks 5b LOUDLY (D_RET_003 error,
  payer named, gross still sums).

## Active gates
- **Deployed prod state unchanged from s175b**: `main` == `fdbd7f2`, bundle
  `index-BrbsO-k6.js`, seed debt CLEAR (772 rules both DBs), 0227 applied.
  `slate-ui` is now 5 commits AHEAD of `main` (this session, un-deployed).
- **⚠ NEW DEPLOY DEBT: `seed_ga500 --year 2025` must re-run on BOTH DBs at the
  next deploy** (7c `is_computed` flip — display metadata; values compute
  correctly regardless). No new migrations this session.
- **Ken decisions queued in REVIEW_QUEUE (s176)**: ① support code 6 as a
  tax-free exchange (RS spec R-RET-CODE change FIRST — Ken-ruled v1 set) vs.
  keep RED; ② rule-studio `check_ga500_integrity.py` needs the 7c→7a
  scenario-input edit to match the new derive.
- ⚠ `LEDGER_AUTOPOST_ENABLED` stays unset until production cutover (Jan 2027).
- ⚠ One test DB — never overlap pytest runs.
- ⚠ The full server suite (~6,900) does NOT finish in a session — this session
  gated on: GA-500 family 87 + FA 521 + render leg + the new tests (all
  green), tsc at the 46-error baseline, 2210 client tests 7/7.
- ⚠ Browser verification of the 2210 label SKIPPED deliberately: another
  session's dev server owns this folder; a static-string change is pinned by
  tsc + component tests. Eyeball it on the next live pass.

## 🔑 Method notes (s175's rules held; one addition)
1. **REPRODUCE BEFORE BUILDING** — 2 of 4 engine claims this batch were
   misattributed by QA. The W-2G "compute bug" was the add-row race wearing a
   compute costume: the UI showed one card, the server had three.
2. **THE REVERT IS THE ONLY PROOF** — both real fixes were revert-proven.
3. **A QA-PRESCRIBED CITATION IS ALSO A HYPOTHESIS** — QA said "use line 24";
   the i2210 Line 8 chart says line 22 + listed Sch 2 taxes − refundable
   credits, which is NOT line 24. Verify the prescription's law, not just
   its bug.
4. **CHECK THE SPEC BEFORE CALLING IT A BUG** — code-6 blanking is JUDGMENT 1
   in R-RET-CODE (Ken-confirmed). An engine behaving per a Ken ruling needs a
   Ken decision, not a fix.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. **The app is TESTING until January 2027.**

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
- **Slate lane:** `Design/SLATE_IMPLEMENTATION_PLAN.md` ·
  `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md`.
