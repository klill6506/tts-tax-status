# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-09, session 44 (Ken approved the retrospective — "implement your
proposals and I agree on D"). Shipped: **A** local pytest Postgres · **B** face audit +
the M-3 $10M tax-law fix (RS `c374e89`) · **C** BaseModelSerializer scoped partial saves
+ hardening · **D1** GA-600S client-mirror deletion. CC lane next = **the RS renumber
queue (4562 first) interleaved with the FA-export reconciliation pass**.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE
Retrospective implementation landed (detail: STATUS_ARCHIVE s44). Next unblocked CC work:
1. **RS renumber queue** (from the face audit — ledger:
   `docs/rs_handoff/2026-07-09_early_era_face_audit.md`): **4562 FIRST** (the 2025 face
   added 19h 50-year property → 19i/19j shift; include the tts engine/field-map 19-row
   verification) → SCH_K_1120S → K1_1120S → 1120S_SCHL → 6198 → M3 line_map (needs the
   face template download) → 3800 (defer to the GBC unit). Each = spec-first, the
   SCHD/SCHB recipe.
2. **RS FA-export reconciliation pass** (queued since s32; mirror PINNED 26 vs export 30).
3. Ken-gated: **D1 typing-feel check on GA-600S** (gates the 1120-S mirror deletion) ·
   batch 2 · the e-services answers · item 10 Lacerte reprint.

**⚠ TESTING CHANGED (read before running tests):** pytest now uses LOCAL PostgreSQL 17
(`config/settings/test.py` → localhost; DECISIONS.md records it). The pooler batch caps /
teardown gotchas no longer apply to tests. Full-suite runs are feasible — the first one
immediately caught 6 stale test_returns pins (fixed: 352/313 counts; trust→1041).

## ▶ Waiting on Ken / external
1. **D1 typing-feel check** — open any GA-600S, type into Sched 3/6; computed cells now
   update on the autosave round-trip (~1s) instead of instantly. If the feel holds, the
   1120-S client mirror (the big drift source) goes the same way.
2. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
3. IdenTrust cert (⚠ 30-day download clock). 4. File-1018 Lacerte reprint (item 10).
5. PWA install check. 6. TaxWise forms-usage report.

## Active gates
- **Flow-assertion gate 447** — green this session (serializer/scoping changes rode through).
- **Full server suite** — first-ever complete run in flight at session close (targeted
  batches green: flow 447 · scoping 2+1 · returns 75 · MeF 64 · S5+S6 8/8 · B11 5).
  If the background run surfaced stragglers, they're next session's first action.
- **Client parity gate** — 1120-S fixture current; vitest 278 · tsc 0 (GA-600S formulas
  deleted — no GA fixture existed; the 1120-S mirror + gate stay until Ken's D1 verdict).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue: any RS amendment touching SCHL/SCH_K/K1/4562/6198/3800 BEFORE its
  renumber inherits the fabrications — renumber first.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` (batch 1 closed) ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
