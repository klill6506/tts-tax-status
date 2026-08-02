# TTS Tax App - STATUS (current state only)

*Last updated: 2026-08-01, session 176 + 176b (**QA BATCH 002 LEG 1 (the
engine-claim triage) + KEN'S TWO SAME-DAY ASKS**. s176: of four engine claims,
two did not reproduce (W-2G "double count" = the add-row race; code-6 blanking
= the Ken-ruled spec with its RED firing) and two were real and FIXED (GA-500
7c derive · direct-save GA resync), plus the 2210 label verified against the
2025 i2210. s176b, Ken live: ① the "green 1040" print — the IRS's published
2025 f1040.pdf IS the green rendition (irs.gov + the archive serve identical
bytes; all 100 other templates are B/W; the GA-500 comb form is the official
DOR web2 version) → NEW render-time de-tint (`decolorize.py`), output verified
0.00% colored, both pages; ② **KEN RULED: code 6 (§1035) IS SUPPORTED** —
built same session (taxable = box 2a; NEW D_RET_010 blocks a blank 2a instead
of the gross fallback; RS spec edit queued).)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — THE BACK-ENTRY IMPORT LANE (Ken's go, 2026-08-01 — jumped
## ahead of the Batch 002 screen queue below)

**The lane** (from ChatGPT's proposal, Ken-approved order): industrialize the
420-packet back-entry backlog (Inbox 420 / Done 40; ~45 min/return via the UI
today). Legs: **A staging ✅ (this session, `4926fa4`)** → **B commit (NEXT)**
→ C reconciliation workspace → D batch status/Mark-Filed/QA report.

**Leg A is LIVE in code** — `POST /api/v1/backentry/batches/` stages+validates
up to 10 returns (schema `backentry.v1`), model-driven field-level errors,
locator→seeded-shell resolution (never creates), (firm,batch_key) idempotent
replay, RLS default-deny on both new tables. **Staging writes NOTHING live —
the commit endpoint deliberately does not exist yet.**

**▶ NEXT — Leg B, the per-return atomic commit:**
1. `POST /backentry/batches/{id}/returns/{return_key}/commit/` — one
   transaction per return: apply taxpayer allowlist fields → create document
   rows (W-2 + state entries / INT / DIV / R / W-2G / dependents) → apply
   `ga500_fields` as preparer entries (`is_overridden=True`, the
   update_fields convention) → `compute_return` + `_auto_sync_ga500` →
   return computed federal/state summaries in the response.
2. Merge policy for a non-empty target (the staged warning): v1 = REFUSE
   unless `{"merge": "replace_documents"}` is passed explicitly; replace
   deletes the return's existing W2/INT/DIV/R/W2G rows in the same
   transaction (the ORM-delete-then-compute rule).
3. Batch status transitions (staged→partial→committed) + `exclude` action.
4. Idempotent commit (re-POST = replay), `dry_run=1` echo of the diff.
5. ⚠ Rule: commits go through the SAME model paths as the UI (serializer
   defaults like the s175 "New Business" line-C bug are the cautionary tale
   — set ONLY payload fields, never placeholder defaults).

Then Leg C (reconciliation + the $5 2210 acceptance policy — record the $5
ruling in DECISIONS when built) and Leg D (batch Mark-Filed + QA report;
fits the s175 status ruling: the agent marks filed once it ties).

## QA Batch 002 — remaining queue (paused behind the import lane; the
## diagnostics-staleness family (#3) gets folded into Leg C/D when needed)

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

- **`3e55635` (s176b) — the 1040 prints classic black-and-white.** The green
  came from the TEMPLATE: the IRS's published 2025 f1040.pdf ("Created
  9/5/25" face) paints ~45% of each page pale mint, and irs.gov AND the
  irs-prior archive serve the SAME bytes — there is no B/W official file to
  download. All 100 other 2025 templates scanned clean (a colored-pixel sweep);
  the GA-500 comb/barcode form is the official DOR "Approved web2" version
  (what GA requires for processing — TaxWise prints the same shape), left
  unchanged. NEW `apps/tts_forms/decolorize.py` rewrites the colored RGB
  fill/stroke operators to white/black at render time; the resources template
  stays the untouched official PDF (never-redraw rule); opt-in per form via
  `DECOLORIZE_FORM_IDS = {"f1040"}`. Output verified by rasterizing filled
  pages: 0.00% colored, layout/text intact. 4 new tests incl. a trip-wire
  that says to REMOVE the pass if a future template refresh goes B/W.
- **`5d4d81f` (s176b) — 1099-R code 6 (§1035 exchange) is SUPPORTED, Ken's
  ruling.** Taxable follows box 2a exactly (explicit 0 → 0; boot → taxable);
  the Sharon-shaped scenario heals (5b = 27,679, gross on 5a, no RED). NEW
  **D_RET_010** (error) blocks a code-6 doc with BLANK/'not determined' box 2a
  — the generic fallback would have taxed the whole $80,051 gross; §1035 docs
  are excluded from the Simplified-Method blank-2a trigger (box 5 carries
  contract premiums). D_RET_003 drops code 6 from its text. Deliberately AHEAD
  of RS spec R-RET-CODE — spec edit queued (REVIEW_QUEUE). Revert-proven.
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
- **⚠ DEPLOY DEBT (s176/b/c), at the next deploy: ① `seed_ga500 --year 2025`
  on BOTH DBs** (7c `is_computed` flip) **+ ② `seed_rules` on BOTH DBs** (NEW
  D_RET_010 + the D_RET_003 rewording) **+ ③ migrations 0228/0229 on BOTH
  DBs** (the back-entry staging tables + their RLS — neither DB migrated
  locally this session; the staging endpoint 500s on live until then, which
  is safe because Leg B doesn't exist).
- **RS agenda (REVIEW_QUEUE s176/s176b)**: ① R-RET-CODE spec edit — code 6 is
  now SUPPORTED (Ken ruled 2026-08-01; app deliberately ahead of spec);
  ② rule-studio `check_ga500_integrity.py` needs the 7c→7a scenario-input
  edit to match the new derive.
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
