# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-11, session 51 ("go" — autonomous continuation per the s48
Ken directive). Shipped **B2-3 manual PY columns on I&D + B2-7b 8825 PY column**
(`baed26c`, mig 0186): editable "PY 2024" comparison column on every entity I&D
row and per-8825-property, backed by NEW `TaxReturn.py_manual_values` written
through a row-locked single-key `/py-manual/` merge PATCH. Manual GREEN shadows
imported YELLOW; clearing reveals the import (Ken ruling: importer/proforma
auto-fill later, keyed stays overridable). PY Compare edits rerouted to the
manual store; the old `prior-year/update-line` action retired. Live-probed
1120-S AND 1065 (isolated firm, 685-obj cascade delete).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE
**Ken directive (2026-07-10, s48; still standing): work AUTONOMOUSLY down this
list — finish an item, close it properly, continue. Stop only for a Ken-gated
decision or when context runs low.**
1. **B2-15 · Global density pass** (+ B2-11 remainder; the s39 sized-wrapper
   recipe; ⚠ Browser-pane screenshots time out this machine — verify by DOM
   metrics/`preview_resize`). Then B2-17 form units (8283-entity/2553/2848/3115)
   go on the Spine.
2. **Server fix queued (also a Cowork task chip): `missing_shareholders_check`
   counts the Shareholder model on a 1065** — a 1065 WITH partners still fires
   the "No partners entered" ERROR (verified live s50). Branch on form_code →
   count Partner; add both-entity DB tests.
3. **Ken ratifications pending (REVIEW_QUEUE s47):** R007 AMT-matrix correction ·
   40% transitional-election mechanics. **+s49 candidates:** stale
   is_overridden-on-blank flag class · entity is_4797 legacy-row diagnostic.
4. **Full-suite straggler triage** (s45 diagnosis stands): s27 stale CENTS pins ·
   test_apr01_fixes `seeded` fixture · pipeline pins · flow ×2 in-suite-only ·
   TestRenderK1. Rerun: `pytest tests/ -q --reuse-db` (~36 min local PG).
5. **RS renumber unit #2: SCH_K_1120S** → K1 → SCHL → 6198 → M3 line_map → 3800.
   Ledger: `docs/rs_handoff/2026-07-09_early_era_face_audit.md`.
6. **RS FA-export reconciliation pass** (queued since s32).
7. Ken-gated: **D1 typing-feel check on GA-600S** · e-services answers · item 10
   Lacerte reprint · PWA install check.

## ▶ Waiting on Ken / external
1. **R007 AMT-correction ratification + 40%-election ruling (s47).**
2. **D1 typing-feel check** (gates the 1120-S client mirror deletion).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report.

## Active gates
- **Flow-assertion gate 447 unchanged** (s51 touched NO compute/render/allocator
  code — a new display-only reference store + UI; per the DECISIONS gate rule
  no flow run required).
- **Client: tsc 0 · vitest 305.** Server: NEW `tests/test_py_manual.py` 7/7 +
  adjacent prior-year files green (local PG, `--reuse-db`).
- **Mig 0186 applied to the shared DB** (additive JSONField, deploy-safe).
- **Live probes (s51, isolated firm PROBE-B23PY-UI, 685-obj cascade-deleted):**
  1120-S I&D: 80 PY cells + "PY 2024" headers; imported 1a=50,000/7=11,111 show
  YELLOW; keyed 22,222 over line 7 → GREEN + DB `L:7`; clear → YELLOW 11,111
  back + key deleted; deduction line 9 + meals 50% tier keys merge; 8825 card:
  15 PY cells/property, `R:<id>:rents_received/repairs` saved; PY Compare shows
  the merged overlay and its edit wrote `L:19` (importer line_values
  byte-identical). 1065: 79 PY cells, `L:10` saved (own per-return store).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue: SCH_K/K1/SCHL/6198/3800 still inherit fabrications.
- ⚠ Browser-pane screenshots/coordinate clicks unavailable this machine — DOM
  assertions + JS focus/native-setter events (s51 note: mid-typing remounts can
  eat computer-typed keys on the PY cells; the native value-setter + focusout
  dispatch is the reliable probe recipe).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
