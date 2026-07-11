# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-10, session 50 ("go" — autonomous continuation per the s48
Ken directive). Two batch-2 bigger singles shipped: **B2-2 entity nav form-status
markers** (`f7bb426` — scope-aware `tabStatus`: per-form_code rule→tab maps +
entity data predicates; entity Diagnostics findings became jump-to-tab links) and
**B2-5 meals one-line reshape** (`b42e49f` — one atomic Meals block, tier rows
appear only when used, tier switch moves the amount between the s41 FFV lines,
computed split = one-line hint; compute/print/MeF untouched). Both live-probed on
1120-S AND 1065 (isolated firms, cascade-deleted). ⚠ Found in passing: server
`missing_shareholders_check` counts Shareholder rows on a 1065 — fires even with
partners entered (spawned as a task chip + listed below).*

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
1. **B2-3 · Manual-entry PY columns on I&D** (COGS + deductions; Ken-ruled:
   manual NOW, TaxWise importer/proforma auto-fills later, keyed values stay
   overridable; **B2-7b 8825 PY column rides it**). Design note for the fresh
   session: decide where manual PY values live — the PY Compare tab reads
   `PriorYearReturn.line_values` (importer-owned); a manual-entry column needs a
   store the importer can later overwrite-or-respect. Migration-bearing —
   needs full-session context.
2. **B2-15 · Global density pass** (+ B2-11 remainder; the s39 sized-wrapper
   recipe; ⚠ Browser-pane screenshots time out this machine — verify by DOM
   metrics/`preview_resize`). Then B2-17 form units (8283-entity/2553/2848/3115)
   go on the Spine.
3. **Server fix queued (also a Cowork task chip): `missing_shareholders_check`
   counts the Shareholder model on a 1065** — a 1065 WITH partners still fires
   the "No partners entered" ERROR (verified live s50). Branch on form_code →
   count Partner; add both-entity DB tests.
4. **Ken ratifications pending (REVIEW_QUEUE s47):** R007 AMT-matrix correction ·
   40% transitional-election mechanics. **+s49 candidates:** stale
   is_overridden-on-blank flag class · entity is_4797 legacy-row diagnostic.
5. **Full-suite straggler triage** (s45 diagnosis stands): s27 stale CENTS pins ·
   test_apr01_fixes `seeded` fixture · pipeline pins · flow ×2 in-suite-only ·
   TestRenderK1. Rerun: `pytest tests/ -q --reuse-db` (~36 min local PG).
6. **RS renumber unit #2: SCH_K_1120S** → K1 → SCHL → 6198 → M3 line_map → 3800.
   Ledger: `docs/rs_handoff/2026-07-09_early_era_face_audit.md`.
7. **RS FA-export reconciliation pass** (queued since s32).
8. Ken-gated: **D1 typing-feel check on GA-600S** · e-services answers · item 10
   Lacerte reprint · PWA install check.

## ▶ Waiting on Ken / external
1. **R007 AMT-correction ratification + 40%-election ruling (s47).**
2. **D1 typing-feel check** (gates the 1120-S client mirror deletion).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report.

## Active gates
- **Flow-assertion gate 447 unchanged** (s50 touched NO compute/render/allocator
  code — client UI + nav-status only; per the DECISIONS gate rule no flow run
  required).
- **Client: tsc 0 · vitest 305** (+27 entity-scope nav tests; parity untouched —
  the meals reshape is display-shape only, FORMULAS unchanged).
- **Live probes (s50, isolated firm PROBE-B22NAV-UI, 700-obj cascade-deleted):**
  1120-S nav: Shareholders ● / Sch L ● / page1 ✓ / Rental ✓ (8825 WARNING did
  not downgrade) / Depreciation ✓; finding-click jumps to the Shareholders tab.
  1065 nav: Partners ● / Rental ✓. Meals block: one 50% row default → +tier →
  switch 100%→80% DOT moves the amount (DB: D_MEALS_DOT=2000, D_MEALS_100='',
  DED 6,600/NONDED 5,400, line 20 ties) → × remove reverts.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue: SCH_K/K1/SCHL/6198/3800 still inherit fabrications.
- ⚠ Browser-pane screenshots time out this machine/session (page healthy —
  JS/read_page fine); probe proofs are DOM assertions. Coordinate clicks are
  ALSO unavailable (they require a cached screenshot) — use JS focus + typed
  keys for inputs.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
