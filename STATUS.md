# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-26, session 120 (Form 4562 Leg 4 — conversion-scale
entry SHIPPED `202559d`; deploy VERIFIED live in-session).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s119 and s120 detail are archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s120 (2026-07-26): Form 4562 Leg 4 — conversion-scale entry SHIPPED
(`202559d`, no migration).** The QA Batch-001 item-6 depreciation rebuild
is COMPLETE — all four legs (s116 routing/rounding · s117 entry integrity ·
s118 basis fidelity/§280F arms · s120 conversion-scale entry):

1. **Published CSV template** + parser (aliased headers, per-row errors);
   Template / Export CSV buttons on the Depreciation tab (round-trip:
   export → edit in Excel → re-import).
2. **Paste rows** — spreadsheet rows pasted straight in (header row
   required) preview through the same server parser.
3. **Preview projects the engine result per row** — projected current-year
   column, fully-depreciated badges, batch total, resolved Activity;
   commit is idempotent and runs the full recompute.
4. **Activity column** resolves to Sch C/E/F by name (C:/E:/F: prefixes;
   blank auto-links a single-activity return, else unassigned + warning).
5. **Bulk assignment** (`depreciation/bulk-update`): activity / method /
   convention / life / group over any selection; filter bar (search,
   activity, group, status incl. Fully depreciated).
6. **Fully-depreciated legacy inventory is first-class**: imports at $0,
   badged, filterable — ACCEPTANCE test-pinned: 7 active + 39 legacy rows
   import without changing the current-year result.
7. **Lacerte boundary fixes**: snake-case group labels normalized (a
   Lacerte land asset would previously DEPRECIATE); bonus % suggestion now
   year-1-only (continuing assets were getting 40% stamped on remaining
   basis).

**Gates green:** NEW server `test_4562_leg4_bulk_entry.py` **16** ·
depreciation regression subset **97** · lacerte parser **39** · NEW client
`depreciationLeg4Entry.test.tsx` **10** · client vitest **511/511** · tsc
**52 baseline** · live demo probe green end-to-end (demo DB restored).

## ▶ NEXT (cold-start pointer)
The spine: item 15 (source-summary proposal — awaiting Ken's A/B/C pick,
rec C) · item 16 · item-6-P1 GA residual (BLOCKED on the two GA
REVIEW_QUEUE questions) · 2210 reconciliation panel. Spine otherwise idle —
Ken directs.

## Known follow-ups from s120 (tracked in DEFERRAL_AUDIT)
- Import/bulk-update don't feed the s119 header saveScope pill yet
  (DepreciationSection still runs its own local queue/pill).
- Paste grid requires the header row (headerless pastes rejected with
  guidance — deliberate).
- TaxWise-native file parsing stays out (CSV template + paste covers
  conversions, per the approved proposal).

## ▶ Waiting on Ken / external
1. Item-15 pick (A / B / rec C) — proposal at `Design/item15_source_summary_proposal.md`.
2. s118 ratifications (REVIEW_QUEUE): §280F AMT-arm derivation · GA no-bump table.
3. s115 ratifications: 8962 Part IV blank-pct · line 34/4-row cap · line-9 marriage-alt.
4. s114 ratifications: the 8867 rebuild's three judgment calls.
5. s113 ratifications: D_GA500_002 realignment · 2210 flat-7% · 7206 partner-arm scope.
6. Item-6-P1 GA residual — BLOCKING questions: GA line 5 filing status from
   federal? · couple the GA deduction election to the federal election?
7. s112 ratification: manifest-aware RS amendment (mechanism only).
8. 86 backfill review rows (83 effective) · S-24 hub-ein blanking · auth env
   vars · A2A WSDL · WISP · SEC-5 · Resend · role assignments · e-services ·
   CAF · ERO EFIN/PIN · beta clauses · older ratifications (s110 · s106 ·
   s101(4) · s100(3) · s99a · s97 · s96(4) · s95..s72).

## Active gates
- **Deploy:** s120 push `202559d`+`84f2b8c` **VERIFIED live in-session** —
  prod + demo bundles rolled `index-Drjjuvdj.js` → `index-DQfYbJEX.js`;
  marker `Paste assets from a spreadsheet` ×1 vs the 0-hit pre-push
  baseline. Nothing pending.
- **DB state:** no new migrations (latest remains 0219, applied BOTH DBs
  in s119).
- **RS:** 4562 spec at `51371ec` (unchanged — Leg 4 is entry tooling, no
  tax-law change); FA-4562 staged entries unchanged (s118).
- ⚠ FA-1040-4835-06 drift (chip `task_0cf10eac`, unchanged from s113).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
