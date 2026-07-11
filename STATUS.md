# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-11, session 52 ("go" — autonomous continuation per the s48
Ken directive; same day as s51's B2-3/B2-7b `baed26c`). Shipped **B2-15 global
density pass + B2-11 remainder** (`8c8409d`): every shared FFV input trimmed
py-1→py-0.5 (30→26px) + FieldRow rows py-1.5→py-1 (seeded rows 42→35px, ~17%
denser — entities AND 1040, shared components); Admin tab sprawl capped
(max-w-3xl cards; invoice descriptions 839→320, memo 887→448, quarterly DATE
boxes 767→128); 1065 partner classification note 746→576. DOM-metric probes on
both entities; autosave+compute round-trip verified post-trim. Whit/Ken
feel-check decides any further squeeze.*

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
**Ken directive addendum (2026-07-11, s52 close — Ken away from the computer
today): keep going until context runs out. Finish a topic → move straight to
the next. If a Ken decision is needed, log it to REVIEW_QUEUE with a
recommendation and MOVE ON to the next item instead of stopping. Full gates +
live probes on everything ("don't mess anything up"); run the MANDATORY
session-close protocol before context exhausts — Ken can't /clear mid-day.**
1. **D2 · 1120-S client formula-mirror retirement — KEN-UNBLOCKED 2026-07-11**
   ("feels fine"; the 1065 editor's always-server-compute precedent is the real
   feel evidence). Delete FORMULAS_1120S from computeFields (GA-600S recipe,
   s44 `D1`): 1120-S rides the `?fresh_return=1` payload; retire the parity
   fixture gate (`computeFields.parity.test.ts` + fixture — its only job was
   policing the mirror); computeFields becomes pass-through everywhere. Live
   probe: type through I&D → downstream computed lines paint from the server.
   Fresh full-context session — the mirror is ~249 tuples in FormEditor.tsx.
2. **Server fix queued (also a Cowork task chip): `missing_shareholders_check`
   counts the Shareholder model on a 1065** — a 1065 WITH partners still fires
   the "No partners entered" ERROR (verified live s50). Branch on form_code →
   count Partner; add both-entity DB tests. **Batch 2 is otherwise COMPLETE**
   (B2-17 form units are the remaining promotion: 8283-entity/2553/2848/3115
   go on the Spine as form units).
3. **Ken ratifications pending (REVIEW_QUEUE s47):** R007 AMT-matrix correction ·
   40% transitional-election mechanics. **+s49 candidates:** stale
   is_overridden-on-blank flag class · entity is_4797 legacy-row diagnostic.
4. **Full-suite straggler triage** (s45 diagnosis stands): s27 stale CENTS pins ·
   test_apr01_fixes `seeded` fixture · pipeline pins · flow ×2 in-suite-only ·
   TestRenderK1. Rerun: `pytest tests/ -q --reuse-db` (~36 min local PG).
5. **RS renumber unit #2: SCH_K_1120S** → K1 → SCHL → 6198 → M3 line_map → 3800.
   Ledger: `docs/rs_handoff/2026-07-09_early_era_face_audit.md`.
6. **RS FA-export reconciliation pass** (queued since s32).
7. Ken-gated: e-services answers · item 10 Lacerte reprint · PWA install check ·
   **density feel-check (s52 trim + any further depreciation-form squeeze)**.
   *(D1 feel-check RESOLVED 2026-07-11 — see item 1.)*

## ▶ Waiting on Ken / external
1. **R007 AMT-correction ratification + 40%-election ruling (s47).**
2. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
3. IdenTrust cert (⚠ 30-day download clock). 4. File-1018 Lacerte reprint (item 10).
5. PWA install check. 6. TaxWise forms-usage report.

## Active gates
- **Flow-assertion gate 447 unchanged** (s51+s52 touched NO compute/render/
  allocator code — s51 = display-only reference store + UI; s52 = CSS-only
  density; per the DECISIONS gate rule no flow run required).
- **Client: tsc 0 · vitest 305.** Server: `tests/test_py_manual.py` 7/7 (s51) +
  adjacent prior-year files green (local PG, `--reuse-db`).
- **Mig 0186 applied to the shared DB** (additive JSONField, deploy-safe).
- **Live probes (s51 PY columns + s52 density, isolated firm, cascade-deleted):**
  s51 — 1120-S I&D 80 PY cells; imported values YELLOW, keyed GREEN shadows,
  clear reveals; `L:`/`R:` keys merge; PY Compare merge + reroute with importer
  line_values byte-identical; 1065 79 cells. s52 — DOM metrics at 1280px:
  inputs 26px, Schedule K rows 35px, Admin max input 448 (was 887), partner
  note 576 (was 746); K2 keyed → autosave → K_ANALYSIS flowed (compute intact).
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
