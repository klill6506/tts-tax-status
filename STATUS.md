# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-11, session 54 ("go" — autonomous per the s48/s52 directives).
**Full-suite straggler triage COMPLETE**: the whole s53 inventory (168 F + 167 E)
worked to ZERO across ten commits (`169401d`…`ca0a710`, ~45 test files). Found and
fixed **two live rate-line print bugs** in passing — Form 8880 line 9 (the applicable
decimal 0.50/0.20/0.10 stored/printed as "1"/"0" through `_write_row`'s whole-dollar
quantize) and SC Schedule NR line 45 (proration 0.60 printed "1" via
`compute_sc1040._format`'s PERCENTAGE branch) — plus a lacerte-importer
FormDefinition chain bug (latest-year picked over `--tax-year`). The s53
"possibly-real Sch 2/3 packet bug" was a STALE TEST (compute-owned lines seeded
without override), not a print bug. Mid-session full rerun: 84 F / 5,298 P / 0 E in
36 min (collected pre-sweeps-7-10; every one of its 84 subsequently fixed and
verified per-file).*

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
1. ✅ **CONFIRMED: the full suite is GREEN — 5,382 passed · 21 skipped · 0 F ·
   0 E in 33:41** (post-fix `--create-db` run, recorded 2026-07-11 at close —
   the FIRST fully-green full-suite run in the project's history). The suite is
   now a usable regression gate; future drift triages with the s54 taxonomy
   (auto-memory `s54-fullsuite-triage-complete`).
2. **RS renumber unit #2: SCH_K_1120S** (fresh session; fabricated 13f · missing
   17c AE&P · wrong L18 formula) → K1 → SCHL → 6198 → M3 line_map → 3800.
   Ledger: `docs/rs_handoff/2026-07-09_early_era_face_audit.md`.
   **Fold in the s54 RS amendment**: retirement-spec RET-G5 "unsupported
   exception 13" scenario is STALE (the 5329 full unit validates the whole
   i5329 01-23+99 table — 13 is valid; engine right; REVIEW_QUEUE s54).
3. **RS FA-export reconciliation pass** (queued since s32).
4. **S-20 B2-17 form units**: 8283-entity → 2553 → 2848 → 3115 app build.
5. **Ken ratifications pending (REVIEW_QUEUE):** R007 AMT-matrix · 40% transitional
   election · s49 candidates · s53 partner-percentage diagnostic.

## ▶ Waiting on Ken / external
1. R007 AMT-correction ratification + 40%-election ruling (s47).
2. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
3. IdenTrust cert (⚠ 30-day download clock). 4. File-1018 Lacerte reprint (item 10).
5. PWA install check. 6. TaxWise forms-usage report. 7. Density feel-check (s52).

## Active gates
- **Flow-assertion gate 447 GREEN** — re-run twice this session (the 8880 rate fix
  and the sc1040 `_format` fix are compute-code changes; both gated).
- **Server: the s53 full-suite inventory is CLEARED.** Ten commits, all pushed.
  Every touched file verified green per-file on scratch DBs
  (`TEST_DB_TEST_NAME=s54tri`/`s54trib`); confirmation full run pending (item 1).
- Client untouched this session (tsc/vitest state = s53: 0 / 300).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue: SCH_K/K1/SCHL/6198/3800 still inherit fabrications.
- ⚠ NEW ENGINE-BUG CLASS to watch: DECIMAL/RATIO face lines fed through
  whole-dollar writers (`_write_row` `_q` / state `_format`) — 8880 L9 and SC
  NR-45 were live-wrong; check the write path whenever a rate-bearing line lands.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
