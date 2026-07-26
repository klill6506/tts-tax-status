# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-26, session 116 (QA Batch-001 item 6 — depreciation
rebuild: proposal APPROVED (all 4 recommendations) + Leg 1 SHIPPED end-to-end
and deploy-verified; item 15 PARKED by Ken in favor of the depreciation work).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s115 is archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s116 (2026-07-26): the depreciation rebuild (item 6, P0) is APPROVED and
Leg 1 is SHIPPED.**

1. **Ken's picks:** item 15 parked ("Depreciation redesign" instead). The
   four-leg rebuild plan (`Design/item6_depreciation_rebuild_proposal.md`,
   `58ce774`) got GO + all three sub-ratifications: per-asset ROUND_HALF_UP
   rounding · single-farm auto-link migration · add-fields-only basis model.
2. **Leg 1 SHIPPED** (app `3dfb977`+`0961407`; RS `5e6ffa3`+`37f565d`; mig
   0217 + seed_rules BOTH DBs; deploy VERIFIED live, bundle
   `index-BZjYNARY.js`, markers ×1/×1 vs 0-hit baseline):
   - **The [client] bug fixed**: on a 1040 the Flow To dropdown offered the
     ENTITY farm arm (`sched_f`), which wrote a nonexistent "F14" line —
     silently swallowed; the module showed "$4,069 → Schedule F" while
     line 14 stayed 0. The 1040 dropdown is now Schedule C/E/F with
     business/farm/property pickers (serializer now exposes `schedule_f` —
     it never had); entity arms never route on a 1040 (incl. the page1
     transient-stamp of the 1040's own line 14).
   - **Per-asset whole-dollar rounding (RS R017, Ken-ratified)**: engine
     reports each asset whole-dollar; destinations sum rounded amounts
     (TaxWise parity: [client] 4,068, not ROUND(4,069.03)).
   - **Migration 0217** (audited before+after BOTH DBs): prod 50 `sched_f`
     assets → `schedule_f` all auto-linked (every one was single-farm);
     1 `page1` → `schedule_c` linked; 1 blank $0 asset left red. Demo: 0.
   - **New diagnostics**: D_4562_DEST (unroutable asset — error when
     dollars move, warning for $0 legacy) + D_4562_RECON (module vs
     destination mismatch = blocking). RS spec-homed, seed_rules BOTH DBs.
   - **[client] PROD recompute verified**: Sch F 14 = 4,068 · farm loss
     6,642 · AGI 20,729 — the QA report's expected column, exact.
   - **Live demo UI probe green** (add asset → auto-links single farm →
     143 whole-dollar → farm line 14 = 143 → delete clears it; demo
     restored). ALSO fixed the standing dev nit: the vite dev proxy now
     rewrites Origin, so autoPort coexistence clients can SAVE (`0961407`).

**Gates green:** new leg 7 · engine 92 · flow **521** · schF-orch/topic8/schL
86 · vitest 459 · tsc 52 baseline (0 new) · RS export verified (17 rules /
16 diags incl. the 2 canonical-code additions).

**▶ NEXT (cold-start pointer): depreciation Leg 2 — entry integrity** (the
s107 draft-row convention for Add Asset, per-asset in-flight save queue,
Saving/Saved/Error state, block row-switch mid-save, fix the defaultValue
remount clobber). Then Leg 3 (basis fields: `original_cost` +
`prior_bonus_depreciation`, §280F in AMT/GA arms) and Leg 4 (paste grid +
CSV import for legacy inventories). All scoped in the approved proposal.
After the legs: item 15 (parked, proposal drafted) · item 16 · GA residual
(still BLOCKED on the two REVIEW_QUEUE questions) · 2210 panel.

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
- **Deploy:** s116 push `3dfb977` VERIFIED live in-session (bundle
  `index-BZjYNARY.js`; "no farm selected" ×1 + "Schedule F Farm" ×1 vs the
  0-hit pre-push baseline). `0961407` (dev-only vite proxy) rides the next
  build harmlessly. Nothing pending.
- **DB state:** mig 0217 applied + audited BOTH DBs (prod: 50→schedule_f
  linked / 1→schedule_c / 1 red; demo: no-op). seed_rules BOTH DBs
  (D_4562_DEST + D_4562_RECON active). [client]'s prod return recomputed to
  the corrected figures (intended — the QA fix itself).
- **RS:** 4562 spec amended (R016 destination / R017 rounding / 2 canonical
  diagnostics / L22 face excerpt / 4 scenarios); deployed export verified;
  tts mirror refreshed verbatim. FA-4562-DEST-01/ROUND-01 staged in RS —
  NOT yet in the app's flow-assertion export (surgical-refresh rule; the
  new pytest leg pins the same flows meanwhile).
- ⚠ **FA-1040-4835-06 drift** (chip `task_0cf10eac`, unchanged from s113).
- ~~autoPort vite CSRF 403 nit~~ **FIXED s116** (`0961407` — proxy rewrites
  Origin to the canonical dev origin).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
