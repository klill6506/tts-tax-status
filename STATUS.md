# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-11, session 56 ("go" — autonomous). **SCH_K_1120S renumber
unit #2 COMPLETE (RS spec-first + tts print fixes) + RET-G5 amendment resolved.**
RS: SCH_K rebuilt verbatim to the 2025 face (52 facts / 19 rules / 47 lines; 13f
biofuel fix, 17a-d split incl. 17c AE&P, L18 = 1-10 − 11-12e − 16f ties to M-1 L8;
the full-loader's "K18 must equal Page 1 Line 21" tax-law ERROR corrected), seeded +
export-verified + tts mirror refreshed. tts: **FOUR live print-fix zones on the
1120-S** — page-1 rows from old-19 down were one row off / unprinted (the 2025 face
inserted line 19 Form 7205; OBI printed on the Total-deductions row, est. payments
on the ENPI tax row, the 22a-23d tax/payment FFVs mostly silent), the K13 credits
block was one field off (8941 K13g printed on the biofuel row), K12b/c/d amounts one
row off, FFV K3 never printed. Print map re-routed to face rows (FFV keys unchanged);
**K17c (AE&P dividends) removed from K-1 allocation/print/MeF per i1120s p.40**
(1099-DIV only; it was printing per-share as box 17 code C = rehab expenditures) and
relabeled in prod. 11 new y-band pins + 752 affected tests + flow gate all green.
`/bugs` sweep at boot: no open reports.*

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
1. **Start every session with `/bugs`** (s55) — reports are CANDIDATES; Ken decides;
   computation-touching fixes re-run regression before merge.
2. **RS renumber unit #3: K1_1120S** (next in the audit-ledger queue) — box 12/13
   code-letter claims vs the 2025 i1120s code tables (12 A=cash 60% / B=cash 30% /
   C=noncash...); tts K-1 print codes verify in the same unit (s37 believed-safe,
   NOT yet verified). **NEW queue insert after SCHL: PAGE1+M1+M2 blocks** (found s56:
   pre-Form-7205 numbering + a fabricated M-1 excerpt line — ledger updated); the tts
   side of that unit = the page-1/K FFV re-key + data migration + the seven s56
   deferrals (DEFERRAL_AUDIT s56 block).
3. **RS FA-export reconciliation pass** (queued since s32).
4. **S-20 B2-17 form units**: 8283-entity → 2553 → 2848 → 3115 app build.
5. **Ken ratifications pending (REVIEW_QUEUE):** R007 AMT-matrix · 40% transitional
   election · s49 candidates · s53 partner-percentage diagnostic.

## ▶ Waiting on Ken / external
1. If `WORK_ORDER_bug_reporting.md` exists somewhere with more than the s55 chat
   summary, point CC at it (reconciliation flag stands).
2. R007 AMT-correction ratification + 40%-election ruling (s47).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN s56** (447 via test_flow_assertions in the 752-test
  affected-files run — the K-1 allocation change was gated).
- **Full suite GREEN as of s54** (`cd9b186`); s56 touched print maps + k1_issuer +
  MeF K-1 items + seed labels; affected files all green, full-suite rerun not run
  this session (print-map changes are pin-covered; next full run picks them up).
- New pin file: `tests/test_1120s_face_renumber_pins.py` (11 pins — page-1 rows,
  K12/K13 rows, K17c page-4 row, K17c-never-on-K-1 trio).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue remaining: K1 → SCHL → **PAGE1+M1+M2 (new s56)** → 6198 →
  M3 line_map → 3800.
- ⚠ Prod seeders run this session: RS `load_1120s_specs` + `load_1120s_full` +
  `load_1040_retirement` (stale-scenario delete added); tts `seed_1120s` (K17c
  relabel). All idempotent reruns, verified.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
