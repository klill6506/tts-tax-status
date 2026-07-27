# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-26, session 119 (autosave stabilization — SHIPPED,
deploy verified live; migration 0219 applied to BOTH DBs by the deploy).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s118 is archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s119 (2026-07-26): autosave stabilization SHIPPED (`2e44e31`, deploy
verified live in-session).**
The "stuck on Saving…/Calculating…, fields vanish on reload, duplicate blank
state rows, silent Add failures" family is closed end-to-end:

1. **Bounded timeouts** — every request aborts at 30s (uploads 120s); the UI
   can never sit on "Saving…" forever (`lib/api.ts`).
2. **saveScope** (`lib/saveScope.ts`) — ONE global save store; every mutation
   runs in a per-record FIFO lane (the s117 depreciation queue,
   generalized). A failure blocks its lane holding the payload; header
   Retry replays in order, Discard is confirm-gated.
3. **Server revision acks** — `TaxReturn.revision` bumps on every successful
   mutation (`saved_revision` on the response, `revision` in fresh_return);
   the client discards stale payloads and shows "All changes saved" only
   after a real acknowledgement.
4. **Idempotent creates** — `X-Idempotency-Key` per user intent on the 8
   record-create endpoints (W-2, 1099-R, Sch C, dependents, W-2
   state/locality/Box 12/Box 14); a retry after a timeout replays the
   stored response instead of duplicating the row. Add buttons disable +
   show pending while their create is in flight.
5. **Calc separation (persist-first)** — a compute crash after a save
   returns 200 + `compute_error` instead of a 500; the header shows an
   amber "Calculation failed — data saved" with Recalculate, never a
   failed save.
6. **Unsaved-work protection** — beforeunload guard + Return Manager link
   confirm + unmount flush (return-id pinned) + a save-details popover
   (last acked revision, pending op + elapsed, Retry/Discard).

**Gates green:** client vitest **501/501** (32 new across 5 files —
regression tests A–E: persistence, delayed create, duplicate child,
failure-and-retry, slow calc) · tsc **52 baseline (unchanged)** · server
`test_autosave_stabilization.py` **9/9** + mutation-recompute/8879/schD/
entry-layer subset (isolated test DB).

## ✅ 0219 gate RESOLVED (Ken said push, 2026-07-26)
- Pushed `2e44e31`; both Render services rolled `index-Bg17tHbm.js` →
  `index-Drjjuvdj.js`; marker `Calculation failed — data saved` ×1 vs the
  0-hit pre-push baseline; prod + demo both serving 200 ⇒ `migrate
  --noinput` applied **0219 on BOTH DBs** (it runs before gunicorn).
- Local `runserver` against either DB works again on this commit.

## ▶ NEXT (cold-start pointer)
Back to the BUILD_ORDER spine: **depreciation Leg 4 (conversion-scale
entry)** — paste grid · CSV import/export with a published template · bulk
assignment · filters · fully-depreciated legacy rows. Acceptance: the
46-asset [client] inventory imports without changing the current-year
result. (Optional first: a live demo probe of the new header pill / Retry /
pending Add buttons — the 32 vitest UI tests cover these states.)

## Known follow-ups from s119 (tracked in DEFERRAL_AUDIT)
- Sch D / dependents / interest / dividend / Sch F row saves get timeouts +
  revision acks (via api.ts) but are not yet on saveScope FIFO lanes.
- DepreciationSection still runs its own s117 queue + local pill (works,
  but doesn't feed the header).
- Nav guard covers reload/close + the Return Manager breadcrumb; other
  in-app exits (sidebar links) don't confirm yet (HashRouter — no useBlocker).
- Calc still runs inline in the mutation request (bounded by the timeout);
  true async recompute is future work.

## ▶ Waiting on Ken / external
1. s118 ratifications (REVIEW_QUEUE): §280F AMT-arm derivation · GA no-bump table.
2. s115 ratifications: 8962 Part IV blank-pct · line 34/4-row cap · line-9 marriage-alt.
3. s114 ratifications: the 8867 rebuild's three judgment calls.
4. s113 ratifications: D_GA500_002 realignment · 2210 flat-7% · 7206 partner-arm scope.
5. Item-6-P1 GA residual — BLOCKING questions: GA line 5 filing status from
   federal? · couple the GA deduction election to the federal election?
6. s112 ratification: manifest-aware RS amendment (mechanism only).
7. 86 backfill review rows (83 effective) · S-24 hub-ein blanking · auth env
   vars · A2A WSDL · WISP · SEC-5 · Resend · role assignments · e-services ·
   CAF · ERO EFIN/PIN · beta clauses · older ratifications (s110 · s106 ·
   s101(4) · s100(3) · s99a · s97 · s96(4) · s95..s72).

## Active gates
- **Deploy:** s119 push `2e44e31` **VERIFIED live in-session** — prod +
  demo bundles rolled `index-Bg17tHbm.js` → `index-Drjjuvdj.js`; marker
  `Calculation failed — data saved` ×1 vs the 0-hit pre-push baseline.
  Nothing pending.
- **DB state:** mig 0219 applied BOTH DBs (by the deploy's
  `migrate --noinput`).
- **RS:** 4562 spec at `51371ec`; FA-4562 staged entries unchanged (s118).
- ⚠ FA-1040-4835-06 drift (chip `task_0cf10eac`, unchanged from s113).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
