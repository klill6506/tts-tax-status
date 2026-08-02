# TTS Tax App - STATUS (current state only)

*Last updated: 2026-08-01, session 179 (**IMPORT LANE LEG D SHIPPED + 🚀 THE
STACKED DEPLOY LANDED AND VERIFIED (Ken's go, same session)** — `main`
fast-forwarded to `2f3f1a4`, bundle `index-HC1WX6M_.js` live on prod AND
demo, migrations 0228/0229/0230 applied BOTH DBs, `seed_ga500` 7c flip +
`seed_rules` D_RET_010 confirmed BOTH DBs (773 active rules), backentry
endpoints LIVE on prod (auth probe: 404-not-500 on a random batch id).
**DEPLOY DEBT: CLEARED. The lane is deployable-done — next is the pilot
batch.**)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — THE LANE IS BUILT AND DEPLOYED; NEXT IS THE FIRST REAL
## PILOT BATCH

**The lane (Ken's go 2026-08-01): COMPLETE in code.** Legs: **A staging ✅
(`4926fa4`)** → **B commit ✅ (`aff0025`)** → **C reconciliation ✅
(`5303279`)** → **D Mark-Filed + QA report ✅ (this session)**.

**Leg D is LIVE in code:**
- `POST /backentry/batches/{id}/mark-filed/` — sweeps the batch: committed
  returns whose reconciliation verdict is **`tie` — and ONLY those** — are
  marked `filed` (the s175 status ruling: the agent marks a return filed
  once it ties to the return as actually filed). The flip goes through the
  **UI's own →filed side effects**: as-filed baseline (1040-X Column A),
  proforma prior-year snapshot, audit log, guarded ledgerlink call (inert
  on →filed by design + `LEDGER_AUTOPOST_ENABLED` default OFF). Attached
  state returns ride the same flip (the packet filed as one return).
  Everything else skips WITH A REASON (no_tie / no answer key / not
  committed / excluded / already filed); per-return atomic; idempotent
  re-POST. **Shape decision (mine, revisable): an explicit batch action,
  NOT auto-on-tie at commit** — flag for Ken if he wants auto.
- `GET /backentry/batches/{id}/report/` — the PII-safe batch QA report:
  per-return commit/verdict/filed state, **the mismatching lines of every
  no-tie**, commit warnings + replaced sections, validation errors, and a
  **diagnostics-staleness verdict** per committed return (latest
  `DiagnosticRun.started_at` vs `committed_at` — a run that predates the
  commit describes a return that no longer exists; the Batch 002
  staleness family, folded in as promised).
- No new model state — the live `TaxReturn.status` is the truth the report
  reads. NO migration.

**▶ NEXT:**
1. ~~Ken's stacked deploy~~ ✅ **DONE + VERIFIED this session** (see Active
   gates).
2. **Pilot batch (THE next unit)**: drive ONE real ≤10-packet batch
   end-to-end on prod (stage → dry-run → commit → reconcile → mark-filed →
   report) with the delvio-1040-entry skill's conventions; tune the payload
   authoring workflow from what the packets actually contain.
3. Then industrialize: the 420-packet backlog at ~10/batch.
4. Behind the lane: Batch 002's row-creation family (matters for January's
   human preparers), Ken's ② MeF batch (`build_irs5695` next) and ④
   year-constant ruling.

## QA Batch 002 — remaining queue (paused behind the import lane;
## family #3 diagnostics-staleness is now SERVED by the Leg D report)

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
   calculation revision than the one on screen. *(Leg D's report now flags
   run-predates-commit per return; the SCREEN-side staleness fix remains.)*
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

- **s179 — Leg D, batch Mark-Filed + the QA report.** Everything in the
  resume block. Files: `apps/returns/backentry.py` (`mark_filed_sweep` /
  `_mark_return_filed` / `build_batch_report` / `_diagnostics_summary`),
  `views_backentry.py` (the two endpoints). 7 tests in
  `test_backentry_markfiled.py` (tied-sweep + GA-500 flip + baseline +
  idempotent replay; no_tie stays draft; unreconciled skipped; uncommitted/
  excluded skipped; report verdicts/mismatch-lines/filed counts + planted
  −7 delta; diagnostics staleness all three states; cross-firm 404 + auth).
  **Revert-proven ×2**: tie gate disabled → a no-tie return got filed (pin
  tripped); staleness comparison flipped → pin tripped. Gates: backentry
  38/38, FA 521 (559 total). NO migration, NO client code.

## Active gates
- **🚀 DEPLOYED (2026-08-01, Ken's go, this session): `main` == `slate-ui`
  == `2f3f1a4`** (fast-forward, 14 commits s176–s179). **VERIFIED:**
  ① prod bundle `index-HC1WX6M_.js` carries the s176 2210 label verbatim
  ("per the i2210 Line 8 chart"), old label gone — the carried
  browser-verification gate is satisfied by the bundle grep; ② migrations
  0228/0229/0230 applied on BOTH DBs (recorder rows + the tables answer);
  ③ `seed_ga500` 7c `is_computed=True` BOTH DBs; ④ `seed_rules` D_RET_010
  active BOTH DBs, **773 active rules both** (772 + D_RET_010, exactly
  expected); ⑤ demo service same bundle, `environment: "demo"`;
  ⑥ **backentry endpoints LIVE on prod** — dev-account magic-link probe,
  GET random batch id → 404 (was a guaranteed 500 pre-migration), logout
  clean. **DEPLOY DEBT: NONE.**
- **RS agenda (REVIEW_QUEUE s176/s176b)**: ① R-RET-CODE spec edit — code 6
  SUPPORTED; ② rule-studio `check_ga500_integrity.py` 7c→7a scenario edit.
- **REVIEW_QUEUE (s178)**: GA-500 UET line number disputed between our own
  sources (brief 44 vs coordinates/diagnostics 42) — verify against the
  DOR PDF before GA UET ever computes/reconciles.
- ⚠ `LEDGER_AUTOPOST_ENABLED` stays unset until production cutover (Jan 2027).
- ⚠ One test DB — never overlap pytest runs.
- ⚠ Full server suite (~6,900) does NOT finish in a session — s179 gated
  on: backentry 38 + FA 521; the deploy's own vite production build ran
  clean locally as pre-flight before the push.

## 🔑 Method notes (carried; s179 confirmations)
1. **THE REVERT IS THE ONLY PROOF** — tie gate and staleness comparison
   both deliberately broken; both pins tripped.
2. **Delegate to the chokepoint, don't copy it** — mark-filed reuses
   `capture_as_filed_baseline` / `capture_prior_year_1040_snapshot` /
   `handle_status_transition` (the UI's own →filed block), not a re-derive.
3. **Reachability ≠ permission** (s175, honored): the ledgerlink call is
   inert on →filed AND enablement-gated before credentials.
4. **No new model state when an existing field is the truth** — filed
   state reads live `TaxReturn.status`; zero migration.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. **The app is TESTING until January 2027.**

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
- **Slate lane:** `Design/SLATE_IMPLEMENTATION_PLAN.md` ·
  `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md`.
