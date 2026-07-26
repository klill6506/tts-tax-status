# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-26, session 117 (QA Batch-001 item 6 — depreciation
rebuild Leg 2 SHIPPED: entry integrity — draft-row Add Asset, per-asset save
queue, keyed edit card).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s116 is archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s117 (2026-07-26): depreciation Leg 2 (entry integrity) SHIPPED** (app
`73a9d50`, client-only — no migrations, no RS change, no compute change):

1. **Add Asset = the s107 draft-row convention.** No placeholder POST; the
   draft lives in client state and persists ONCE on the first non-blank
   description, carrying everything typed before it. Concurrent blurs
   serialise onto ONE create (in-flight promise ref); a failed create keeps
   the card + values with the DRF message inline + Retry.
2. **Per-asset FIFO save queue** — field saves on an asset chain behind each
   other, so a delayed response can never interleave with a later edit (the
   QA's "delayed autosave overwrote a different asset" class).
3. **Visible Saving…/Saved/Not-saved state** in the edit-card header;
   Add / Edit / Del / Import / Close are blocked while a save is unresolved
   (in flight or errored-unretried; Retry re-sends the exact payload).
4. **The cross-asset clobber is dead**: the edit card is keyed by asset id,
   so switching rows remounts fresh `defaultValue` inputs instead of
   inheriting the previous asset's DOM text (the s111 unkeyed-card defect).
5. Close keeps an unsaved-but-typed draft in the table (nothing typed is
   lost); an untouched empty draft is discarded; Add Asset returns to an
   existing unsaved draft instead of clobbering it.

**Gates green:** NEW `depreciationEntryIntegrity.test.tsx` **10** (incl. the
single-create race guard + the remount repro) · vitest **469** · tsc 52
baseline (0 new). Live demo probe green: Add → ZERO requests → draft card +
unsaved chip → description blur → exactly one POST 201 → Saved ✓ → delete
restored the demo DB.

**▶ NEXT (cold-start pointer): depreciation Leg 3 — basis fidelity** (add
`original_cost` + `prior_bonus_depreciation` alongside `cost_basis`;
computed accumulated-depreciation / adjusted-basis on the card; disposal
math on the split fields; fold §280F caps into the AMT/GA parallel arms —
the s46 boundary). Acceptance: the Barn keyed faithfully (9,010 / 4,505 /
historical split) still computes 266. Then Leg 4 (paste grid + CSV import,
legacy inventories). After the legs: item 15 (parked, proposal drafted) ·
item 16 · GA residual (still BLOCKED on the two REVIEW_QUEUE questions) ·
2210 panel.

## ▶ Waiting on Ken / external
1. **s115 ratifications (REVIEW_QUEUE):** 8962 Part IV blank-pct ·
   line 34/4-row cap · line-9 marriage-alt.
2. **s114 ratifications:** the 8867 rebuild's three judgment calls.
3. **s113 ratifications:** D_GA500_002 realignment · 2210 flat-7% · 7206
   partner-arm scope.
4. **Item-6-P1 GA residual — BLOCKING questions:** GA line 5 filing status
   from federal? · couple the GA deduction election to the federal election?
5. **s112 ratification:** manifest-aware RS amendment (mechanism only).
6. **86 backfill review rows** (now 83 effective) · S-24 hub-ein blanking ·
   auth env vars · A2A WSDL · WISP · SEC-5 · Resend · role assignments ·
   e-services · CAF · ERO EFIN/PIN · beta clauses · older ratifications
   (s110 · s106 · s101(4) · s100(3) · s99a · s97 · s96(4) · s95..s72).

## Active gates
- **Deploy:** s117 push `73a9d50` **VERIFIED live in-session** — prod bundle
  rolled `index-BZjYNARY.js` → `index-Bc1mC_ho.js`; marker `it saves as
  soon as you enter a description` ×1 vs the 0-hit pre-push baseline.
  Nothing pending.
- **DB state:** unchanged from s116 (mig 0217 applied+audited BOTH DBs;
  seed_rules current). No migrations in s117.
- **RS:** unchanged from s116 (4562 spec at `5e6ffa3`+`37f565d`).
  FA-4562-DEST-01/ROUND-01 still staged in RS only (surgical-refresh rule).
- ⚠ **FA-1040-4835-06 drift** (chip `task_0cf10eac`, unchanged from s113).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
