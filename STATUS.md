# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-26, session 118 (QA Batch-001 item 6 — depreciation
rebuild Leg 3 SHIPPED: basis fidelity + §280F caps in the AMT/GA parallel
arms).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s117 is archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s118 (2026-07-26): depreciation Leg 3 (basis fidelity + §280F parallels)
SHIPPED** (RS `51371ec` seeded + export-verified, mirror refreshed; app
`bb2935e`, mig 0218 BOTH DBs; seed_rules BOTH DBs):

1. **Split basis history (RS R018):** `original_cost` (null ⇒ equals
   `cost_basis`; pre-Leg-3 fleet untouched) + `prior_bonus_depreciation`.
   `cost_basis` stays the engine input — no recompute change (Ken's
   add-fields-only pick). Barn pin: 9,010 / 4,505 / 4,505 → still 266;
   accumulated 5,971 / adjusted 3,039 derived on the card.
2. **§280F caps now bind the AMT refigure and the GA arm (RS R019, closes
   s46 boundary #3).** AMT = same table as federal (§280F(a)(1)(A)
   statutory derivation — i6251 SILENT, flagged); GA = the NO-bonus table
   (the $8,000 bump IS §168(k)(2)(F)(i), never conformed). Plus the cap
   now runs AFTER the ≤50% SL recompute (it previously escaped the cap).
   **Both judgment calls await Ken's ratification (REVIEW_QUEUE).**
3. **Disposal + §1250-additional math on the split fields at every site**
   (compute / views / rules_4797 / renderer / MeF read_model — bridge
   parity), via the ONE model property `disposal_cost_basis`.
4. **D_4562_BASIS** effect-scaled (error impossible basis / warning recon
   gap; fires only when original_cost keyed). seed_rules BOTH DBs.
5. Stale-pin repair: `test_schedule_e_depreciation_flow` cents pin
   6,812.59 → 6,813 (the one assertion the s116 whole-dollar repin missed;
   fails on unmodified HEAD — not a Leg 3 regression).

**Gates green:** NEW `test_4562_leg3_basis_fidelity.py` **12** (Barn ·
AMT/GA caps · ≤50% cap ordering · disposal split · D_4562_BASIS ·
serializer no-double-count) · depr/4797/render **148** · flow **521** ·
mef_1120s **75** · schF **20** · vitest **469** · tsc **52 baseline**.
Live demo probe green (card fields render → 9,010 save → Saved ✓ →
adjusted 8,460 → cleared/restored; demo DB clean).

**▶ NEXT (cold-start pointer): depreciation Leg 4 — conversion-scale entry**
(spreadsheet-style paste grid · CSV import/export with a published template ·
bulk assignment of activity/method/life/convention · filters · first-class
fully-depreciated legacy inventory rows). Acceptance: [client]'s full
46-asset inventory imports without changing the current-year result. After
Leg 4: item 15 (parked, proposal drafted) · item 16 · item-6-P1 GA residual
(still BLOCKED on the two REVIEW_QUEUE GA questions) · 2210 panel.

## ▶ Waiting on Ken / external
1. **s118 ratifications (REVIEW_QUEUE):** §280F AMT-arm statutory
   derivation · GA no-bump table.
2. **s115 ratifications:** 8962 Part IV blank-pct · line 34/4-row cap ·
   line-9 marriage-alt.
3. **s114 ratifications:** the 8867 rebuild's three judgment calls.
4. **s113 ratifications:** D_GA500_002 realignment · 2210 flat-7% · 7206
   partner-arm scope.
5. **Item-6-P1 GA residual — BLOCKING questions:** GA line 5 filing status
   from federal? · couple the GA deduction election to the federal election?
6. **s112 ratification:** manifest-aware RS amendment (mechanism only).
7. **86 backfill review rows** (now 83 effective) · S-24 hub-ein blanking ·
   auth env vars · A2A WSDL · WISP · SEC-5 · Resend · role assignments ·
   e-services · CAF · ERO EFIN/PIN · beta clauses · older ratifications
   (s110 · s106 · s101(4) · s100(3) · s99a · s97 · s96(4) · s95..s72).

## Active gates
- **Deploy:** s118 push `bb2935e` **VERIFIED live in-session** — prod bundle
  rolled `index-Bc1mC_ho.js` → `index-Bg17tHbm.js`; marker `Accum.
  Depreciation (EOY)` ×1 vs the 0-hit pre-push baseline. Nothing pending.
- **DB state:** mig 0218 applied BOTH DBs; seed_rules re-run BOTH DBs
  (D_4562_BASIS catalogued).
- **RS:** 4562 spec at `51371ec` (R018/R019 + D_4562_BASIS); mirror
  refreshed verbatim. FA-4562-DEST-01/ROUND-01/280F-01 staged in RS only
  (surgical-refresh rule).
- ⚠ **FA-1040-4835-06 drift** (chip `task_0cf10eac`, unchanged from s113).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
