# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-12, session 63 ("go" — autonomous). **SCH_K INPUT
SUB-UNIT COMPLETE (the s56 tail items 4-7; mig 0188).** The 1120-S Schedule K
line-12 family re-keyed to the 2025 face (K12a cash / NEW K12b noncash / K12c
inv-interest / K12d 59(e)(2) / K12e other — FFVs rode the FK, prod renamed rows
were all blank; MappingRule targets + JSON stores re-keyed). NEW inputs: K16e
(derives bottom-up from ShareholderLoan.loan_repayments, override-respecting;
K-1 box 16 code E is PER-RECIPIENT with pro-rata fallback), K16f (enters K18
per the face formula "11 through 12e and 16f" AND M2_5a per i1120s p.50
adjustment 2; K-1 code F), K14a/14b (the K-2/K-3 CHECKBOXES — the old "Name of
Country" text row converted; MeF emits 14b, REFUSES on checked 14a — K-2 not
built), and M2_7d ← K17c (the AE&P column line-7 derive; M2_8d follows).
K-1 box 12 codes now A/C/H/J/ZZ; box 16 A-F. NEW diagnostics (prod-seeded):
D_SCHK_CHARCODE (D006 mirror) · D_SCHK_8283 (noncash>$500, form not built) ·
D_SCHK_17C_DIV (1099-DIV channel info) · D_EB_K2K3/DFE_OK extended to 1120-S.
Gates: flow 447 · MeF+pins+spec suites green · test_returns 359 · tsc 0 ·
vitest 300 · shared DB mig 0188 + seed rerun + seed_rules DONE · live ORM
probe 14/14 + live browser probe (type→autosave→server-paint verified; K14a
renders as boolean chips). Boundaries → DEFERRAL_AUDIT s63 (charitable
category input · entity 8283 = S-20a · 1099-DIV = sherpa-1099 lane · K-2
refuse · ⚠ M-2 grid column letters OFF-FACE b/c/d — fold into the M-2 NNA
leg). `/bugs` at boot: no open reports.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE
**Ken directives standing (s48 + s52 addendum): work AUTONOMOUSLY down this list;
full gates + live probes; Ken-decisions → REVIEW_QUEUE with a recommendation, then
move on; mandatory session close before context exhausts.**
1. **Start every session with `/bugs`** (s55).
2. **RS FA-export reconciliation pass** (queued since s32).
3. **S-20 B2-17 form units**: 8283-entity → 2553 → 2848 → 3115 app build.
   (8283-entity now has a live D_SCHK_8283 warning pointing at it — s63.)
4. **Ken ratifications pending (REVIEW_QUEUE):** s59 M-2 NNA distribution cap
   (the tts M-2 compute leg implements it once ratified — fold the M-2 grid
   column-letter re-key in with it, DEFERRAL_AUDIT s63 item 5) · R007
   AMT-matrix · 40% transitional election · s49 candidates · s53
   partner-percentage diagnostic · s57 K-1 health-insurance ZZ presentation.
5. *(Renumber queue: CLEARED except **3800** — rides the future GBC entity
   unit. The 6198 + M-3 tts build units stay unblocked-but-unscheduled.)*

## ▶ Waiting on Ken / external
1. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
2. Ratifications: M-2 NNA cap (s59) · R007 AMT + 40% election (s47).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN s63** (447 within the 530-test consolidated run).
- s63 suites: 1120-S spec+pins 80 · MeF+scenarios+flow 530 · returns/diag/K-1
  batch 347 (the 3 `test_k1_import_stage3` fixture errors are PRE-EXISTING,
  reconfirmed standalone) · tsc 0 · vitest 300.
- Last full-suite GREEN = s54 `cd9b186`; the s63 sweep of affected suites
  surfaced only the expected seed-count pins (355→359, updated).
- ⚠ Shared-DB deploy state: **mig 0188 APPLIED + seed_1120s rerun (359 lines) +
  seed_rules DONE (s63)**; mig 0187 state stands. Render deploy just needs the
  code push.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
