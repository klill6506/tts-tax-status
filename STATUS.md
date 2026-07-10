# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-10, session 48 ("go" — batch-2 8825 group). The **8825
Rev-12-2025 unit shipped END-TO-END, spec-first** (RS `c4c94bc` / tts `a4435f4`):
the RS 8825 spec was ANOTHER early-era drifted block (its numbering matched NO
published revision) — renumbered verbatim to the Dec-2025 face (f8825.pdf +
i8825.pdf Rev. 12-2025, both fetched). Closes usability items **B2-7a, B2-8,
B2-9**. tts: mig 0184/0185 (2b other income · line-13 wages · col (c) A-I code ·
NEW RentalPropertyOtherDeduction = **Schedule A (Form 8825)** category rows →
line 17) · **FOUR live print bugs fixed** (property type printed in the col-(c)
A-I column · interest_other dropped from line 8 · mgmt/supplies dropped from 17 ·
21/22a never printed + line 23 omitted them) · official f8825sa template + map ·
MeF 2b/2c/wages/alpha-code elements + GeneralDependencyMedium detail statements
(no declared Schedule A doc in 2025v6.2) · D_8825_001-005 seeded · UI address
block / income 2a+2b / face-ordered expenses / "+ Add" category rows / Calculated
totals rows (whole-dollar).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE
1. **Ken ratifications pending (REVIEW_QUEUE s47):** the R007 AMT-matrix
   correction · the 40% transitional-election mechanics (both unchanged from s47).
2. **Batch 2 remaining** (`USABILITY_QUEUE.md`): quick sweep (1 · 6 · 13/14
   renames · 16) · bigger singles (2 · 3 · 5 · 15) · B2-17 form units → Spine
   (8283-entity/2553/2848/3115). The 8825 group (7/8/9) is DONE this session;
   B2-7b PY column rides the B2-3 unit.
3. **Full-suite straggler triage** (s45 diagnosis stands): s27 stale CENTS pins
   dominant · mechanical rot (test_apr01_fixes `seeded` fixture; the
   test_tts_forms manifest pin is FIXED — re-baselined 82 this session) ·
   pipeline pins · ⚠ flow ×2 in-suite-only · **+NEW confirmed straggler:
   test_tts_forms TestRenderK1 (Alice 60% share comes back 100%) — fails on
   clean main, bisect-verified NOT from s48.** Rerun:
   `pytest tests/ -q --reuse-db` (~36 min local PG).
4. **RS renumber unit #2: SCH_K_1120S** (fabricated 13f; missing 17c AE&P;
   wrong L18) → K1 → SCHL → 6198 → M3 line_map → 3800. Ledger:
   `docs/rs_handoff/2026-07-09_early_era_face_audit.md`. (8825 ✅ s48 — found
   OUTSIDE the audit's queue; treat every pre-s34 block as suspect.)
5. **RS FA-export reconciliation pass** (queued since s32).
6. Ken-gated: **D1 typing-feel check on GA-600S** · e-services answers · item 10
   Lacerte reprint · PWA install check.

## ▶ Waiting on Ken / external
1. **R007 AMT-correction ratification + 40%-election ruling (s47).**
2. **D1 typing-feel check** (gates the 1120-S client mirror deletion).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report.

## Active gates
- **Flow-assertion gate 447** — green (FA008 healed honestly: the 20a sum now
  inlines rents_received + other_rental_income; SCHE-02's pure runner guarded
  via `_state.adding` on the new detail-rows property).
- **MeF 1120-S suite green** incl. 3 new 8825 tests (Rev-12-2025 elements ·
  Schedule A GeneralDependencyMedium content/link/position · live-XSD valid;
  ⚠ XSD facet lesson: MediumExplanationType forbids newlines).
- **8825 AcroForm/render suite green** (+f8825sa map coverage-both-ways pin +
  the NEW line-1 column x-order pin — the wrong-column print-bug class guard).
- S5+S6 scenario pair 8/8 (their extracts exercise the new read-model loop).
- test_tts_forms manifest trip-wire re-baselined **82** (was stale at 80 vs 81).
- **Client**: tsc 0 · vitest 278 (parity untouched — no formula-line changes).
- **Live probes** (isolated firms, cascade-deleted 361 objs each): ORM — K2
  12,800 exact · 3-page print (composed address, line 8 = 6,400 combined,
  Schedule A page) · D_8825_002/005 fire. Browser — card paints (address block,
  2b, wages, A-I select) · "+ Add" → POST 201 · category A13 + $1,500 →
  Total Expenses $8,300 painted · server K2 15,700 round-trip.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue: SCH_K/K1/SCHL/6198/3800 still inherit fabrications.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
