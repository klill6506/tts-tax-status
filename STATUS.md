# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-10, session 46 (Ken delivered the Lacerte method list → the
depreciation-methods unit shipped END-TO-END, spec-first). RS `d5e4386` / tts `5539168`:
**Ken's method/life/vehicle-classification dropdowns + the post-1998 AMT matrix + the
§280F/§179-SUV vehicle caps — and TWO LIVE ENGINE BUG CLASSES fixed against the verbatim
Pub 946 (2025) tables** (the entire 200DB mid-quarter dict was derived-wrong — its Q4
5-yr column summed to 99.00%; the 150DB 10-yr column switched to SL a year late; the
luxury-auto constants were $100/$200 wrong AND miscited Rev. Proc. 2025-13 — the real
source is Rev. Proc. 2025-16). Migration 0182 applied to the shared DB; diagnostics
D_4562_VCLASS/SUV179/METHOD seeded.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE
1. **§168(k)(7)/(k)(10) bonus opt-out election unit** — the stated follow-on to s46
   (DEFERRAL_AUDIT 2026-07-10 item 1): per-class election input + election statement doc +
   GA add-back kill (why Ken seldom uses bonus) + §280F Table-2 flip + AMT 150DB
   re-engagement. RS 4562 R007/R008 already quote the interplay verbatim (RP 2025-16
   §2.03) — spec amendment is a drop-in. **Ken RULED 2026-07-10: SKIP §168(f)(1)
   units-of-production and ACRS legacy** ("It's been years since I used ACRS and I've
   never used UOP") — the election unit ships without them; face lines 15/16 stay
   preparer-entered pass-throughs. Do not re-ask.
2. **Full-suite straggler triage** (s45 diagnosis stands): remaining classes = (a) s27
   stale CENTS pins — dominant (test_1040 brackets, schedule_j ×12, topic5/7/8/9, GA-500,
   8995a, + test_schedule_l K15a `-1155.95` confirmed s46); re-pin with a hand-check,
   never bend compute. (b) mechanical rot (test_apr01_fixes `seeded` fixture;
   test_tts_forms manifest trip-wire). (c) 8835/8936/3800/2441/8911 pipeline pins.
   (d) ⚠ test_flow_assertions failed ×2 IN-SUITE but passes standalone — ordering/
   pollution class. Rerun: `pytest tests/ -q --reuse-db` (~36 min local PG).
3. **Batch 2 remaining** (`USABILITY_QUEUE.md`): 8825 group (7 — structured address =
   model+MeF leg; PY column; multi other-expense lines; totals layout) · quick sweep
   (1 yellow-dot bug · 6 footer trim · 13/14 renames [Dispositions → "Schedule D
   (Capital Gains)" — verify all 4797 paths ride the worksheet FIRST] · 16 hide filing
   fields) · bigger singles (2 nav status dots · 3 PY columns · 5 meals one-liner ·
   15 density pass) · B2-17 form units → Spine (8283-entity/2553/2848/3115).
4. **RS renumber unit #2: SCH_K_1120S** (fabricated 13f; missing 17c AE&P; wrong L18)
   → K1 → SCHL → 6198 → M3 line_map → 3800. Ledger:
   `docs/rs_handoff/2026-07-09_early_era_face_audit.md`.
5. **RS FA-export reconciliation pass** (queued since s32).
6. Ken-gated: **D1 typing-feel check on GA-600S** · e-services answers · item 10 Lacerte
   reprint · PWA install check.

## ▶ Waiting on Ken / external
1. **D1 typing-feel check** (gates the 1120-S client mirror deletion).
2. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
3. IdenTrust cert (⚠ 30-day download clock). 4. File-1018 Lacerte reprint (item 10).
5. PWA install check. 6. TaxWise forms-usage report.

## Active gates
- **Flow-assertion gate 447** — green this session (engine tables + AMT changed).
- **MeF 1120-S suite 66** — green; S5+S6 scenario pair 8/8 (byte-stable through the
  table fixes — their assets are HY, AMT lines un-pinned).
- **4562 render suite 23** — green incl. the y-band row-placement pin; ADS Section C
  rows NEW (print); MeF ADS refuse seam pins.
- **Depreciation engine suite 55** (21 NEW published-table/AMT/vehicle-cap pins —
  every value transcribed from Pub 946 (2025)/RP 2025-16/RP 2024-40, never derived).
- **Client**: tsc 0 · vitest 278 (parity untouched — no formula-line changes).
- **Live probes** (isolated firm, cascade-deleted): ORM probe — SUV §179 clamp $41,040
  exact, ADS 40-yr March $7,916 (A-13a), 31.5 legacy past-recovery 0, all 3 diagnostics
  fire, render_4562 Section C green; browser probe — all 3 dropdowns verbatim,
  vehicle-classification autosave round-trip persisted, AMT "Auto (150% DB)" label.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue: SCHL/SCH_K/K1/6198/3800 still inherit fabrications — renumber
  before amending. (4562 ✅ s45+s46.)

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
