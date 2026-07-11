# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-10, session 49 ("go" — autonomous continuation per the s48
Ken directive). The **batch-2 QUICK SWEEP shipped: B2-1 · B2-6 · B2-13 · B2-14 ·
B2-16 all closed in one commit** (`11d711a`). Highlights: B2-1 became the retro
item-F provenance primitive (`fieldProvenanceClass` — GREEN typed / YELLOW
system-supplied / neutral blank on every FFV input; the inconsistent, stale amber
dot removed); B2-14's entry-path audit proved the entity Dispositions tab's
"Form 4797" option was an ORPHANED path feeding nothing (entity 4797 rides only
the depreciation worksheet) — tab is now Schedule D (Capital Gains) with a legacy
warning + one-click convert; B2-6 also fixed the 1065 footer mislabeling (manual
11/12/19/20 were in the computed footer, real totals 22/23 alphabetized into the
list); B2-16 flipped letter.py to blank-means-e-file so the default holds on
existing returns.*

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
1. **Batch-2 bigger singles** (`USABILITY_QUEUE.md`): **B2-2** form-status
   indicators in the entity nav (port the 1040 used/error markers) · **B2-3**
   manual-entry PY columns on I&D (Ken-ruled; B2-7b 8825 PY column rides it) ·
   **B2-5** meals one-line reshape (Ken-ruled: one row + 100/80/50 mark, "+"
   adds a tier; the s41 compute stands) · **B2-15** global density pass
   (preview_resize first; Browser-pane viewport ~351px). B2-11 remainder rides
   B2-15. Then B2-17 form units (8283-entity/2553/2848/3115) go on the Spine.
2. **Ken ratifications pending (REVIEW_QUEUE s47):** R007 AMT-matrix correction ·
   40% transitional-election mechanics. **+NEW s49 candidates:** the stale
   is_overridden-on-blank flag class (DEFERRAL_AUDIT s49 item 1) · entity
   is_4797 legacy-row diagnostic (item 2).
3. **Full-suite straggler triage** (s45 diagnosis stands): s27 stale CENTS pins
   dominant · test_apr01_fixes `seeded` fixture · pipeline pins · ⚠ flow ×2
   in-suite-only · test_tts_forms TestRenderK1 (pre-s48, bisect-verified).
   Rerun: `pytest tests/ -q --reuse-db` (~36 min local PG).
4. **RS renumber unit #2: SCH_K_1120S** (fabricated 13f; missing 17c AE&P;
   wrong L18) → K1 → SCHL → 6198 → M3 line_map → 3800. Ledger:
   `docs/rs_handoff/2026-07-09_early_era_face_audit.md`.
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
- **Flow-assertion gate 447 green** (s49 run: flow + print packages = 470 passed;
  no compute changes this session — letter.py blank-defaults-e-file only).
- **Client: tsc 0 · vitest 278** (parity untouched — no formula-line changes;
  the hidden I&D rollups are display-only).
- **Live probe (s49, isolated firm PROBE-B2SWEEP-UI, 685-obj cascade-deleted):**
  nav labels painted (Schedule K/L/B · Schedule D (Capital Gains) · Like-Kind
  Exchange) · I&D footer ties with hidden rollups (20=14,080 · 21=35,920; 1065
  footer = 22/23 only) · provenance paint GREEN/YELLOW/neutral incl. the
  blanked-stale-flag case · legacy-4797 convert round-trip · paper checkbox →
  BOTH lines "Paper" in DB.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue: SCH_K/K1/SCHL/6198/3800 still inherit fabrications.
- ⚠ Browser-pane screenshots time out this machine/session (page healthy —
  JS/read_page fine); probe proofs are DOM assertions.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
