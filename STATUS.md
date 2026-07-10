# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-10, session 47 ("go" — the s46 stated follow-on). The
**§168(k)(7) bonus opt-out ELECTION unit shipped END-TO-END, spec-first** (RS
`fdeadfb` / tts this session): return-level per-class election → engine forces
bonus to 0 for the class → §280F Table 2 → GA add-back eliminated → election
statement in the print packet + the DECLARED `ElectNotClaimSpclDeprecAllwnc`
MeF document → per-class checkboxes on the Depreciation tab. **⚠⚠ TAX-LAW
CORRECTION shipped with it: the s46 R007 AMT arm was WRONG — a (k)(7)
election-out does NOT re-engage the AMT 150DB recompute for post-2015 property**
(i6251 2025 2l + i4562 2025 p.7 Note, both verbatim; eligibility controls, not
claiming). NEW per-asset `bonus_eligible` flag (default true) now drives the
150DB arm (never-eligible property only). Migration 0183 applied to the shared
DB; D_4562_ELECTBONUS/ELECTGAP seeded. **Ken must ratify the R007 reversal —
REVIEW_QUEUE s47 top item.***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE
1. **Ken ratifications pending (REVIEW_QUEUE s47):** the R007 AMT-matrix
   correction (reverses the election-out arm of Ken's 2026-07-10 matrix on two
   agreeing verbatim 2025 instructions) · the 40% transitional-election
   mechanics (spec-silent, unimplemented).
2. **Full-suite straggler triage** (s45 diagnosis stands): (a) s27 stale CENTS
   pins — dominant (test_1040 brackets, schedule_j ×12, topic5/7/8/9, GA-500,
   8995a, test_schedule_l K15a); re-pin with a hand-check, never bend compute.
   (b) mechanical rot (test_apr01_fixes `seeded` fixture; test_tts_forms
   manifest trip-wire). (c) 8835/8936/3800/2441/8911 pipeline pins.
   (d) ⚠ test_flow_assertions failed ×2 IN-SUITE but passes standalone.
   Rerun: `pytest tests/ -q --reuse-db` (~36 min local PG).
3. **Batch 2 remaining** (`USABILITY_QUEUE.md`): 8825 group (7) · quick sweep
   (1 · 6 · 13/14 renames · 16) · bigger singles (2 · 3 · 5 · 15) · B2-17 form
   units → Spine (8283-entity/2553/2848/3115).
4. **RS renumber unit #2: SCH_K_1120S** (fabricated 13f; missing 17c AE&P;
   wrong L18) → K1 → SCHL → 6198 → M3 line_map → 3800. Ledger:
   `docs/rs_handoff/2026-07-09_early_era_face_audit.md`.
5. **RS FA-export reconciliation pass** (queued since s32).
6. Ken-gated: **D1 typing-feel check on GA-600S** · e-services answers · item 10
   Lacerte reprint · PWA install check.

## ▶ Waiting on Ken / external
1. **R007 AMT-correction ratification + 40%-election ruling (s47, above).**
2. **D1 typing-feel check** (gates the 1120-S client mirror deletion).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report.

## Active gates
- **Flow-assertion gate 447** — green this session (engine + AMT changed).
- **Depreciation engine suite 63** (+8: election quartet — bonus-0/full-basis
  MACRS · no-AMT-adjustment · GA add-back 0 · Table-2 12,200 — + PIS-year gate,
  other-class isolation, never-eligible 150DB, class-token guard).
- **MeF 1120-S suite 69** (+3: election-doc content + ReturnData ref-1921
  position pin · no-election-no-doc · live-XSD valid with the election).
- **4562/4797 render suite 25** (+2: the statement page carries the i4562
  declaration verbatim · no-election-no-page). S5+S6 scenario pair 8/8.
- **Client**: tsc 0 · vitest 278 (parity untouched — no formula-line changes).
- **Live probes** (isolated firms, cascade-deleted 358+360 objs): ORM — 7-step
  election lifecycle green (baseline 10,000 bonus → elect → 0/2,000/AMT=regular/
  GA 0 → D008 fires on conflict → clean → ELECTGAP on no-election →
  bonus_eligible=false 150DB 1,500 → statement page in packet). Browser —
  election card + 7 checkboxes render; toggling "5-year property" PATCHes
  /info/ (200) → server recompute → bonus $0 / MACRS $2,000 / AMT $2,000 /
  GA Disallowed $0 painted; "Bonus-Eligible / Qualified property" checkbox in
  the asset edit form.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue: SCHL/SCH_K/K1/6198/3800 still inherit fabrications —
  renumber before amending. (4562 ✅ s45+s46+s47.)

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
