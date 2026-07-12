# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-12, session 62 ("go" — autonomous). **Renumber unit #7
COMPLETE — the 1120S_M3 line_map rebuilt to the REAL face (RS lane). The s44
audit's renumber queue is now CLEARED except 3800, which rides the future
GBC entity unit.** The M-3 face entered the repo: f1120ss3.pdf Rev. December
2019 (current; ⚠ irs.gov's f1120sm3.pdf is the C-corp 1120 M-3) + i1120ss3.
The old P1-*/P2-*/P3-* map was FABRICATED (Part III ends at 32, spec had
33-36; invented P1-FS/P1-RS/P2-DEP/P3-DEP rows). Rebuilt verbatim: 30 face
facts · R001-R005 · 87 face lines · D001-D007 · 5 scenarios (published
Example 1 Schedule-L-gate pin + P1 L11 combine + the P3 L32 sign-flip).
**NEW: R005 = the $50M complete-ENTIRELY tier + the through-Part-I + M-1
option (M-1 L1 = P1 L11) — what the old "$50M" error had conflated with the
$10M filing gate**; tie chain specced (P1 L11 = P2 L26(a); P2 L26(d) =
Schedule K L18, the M-1 L8 anchor). SCHB's stale "$50M" link note healed.
Harness 165/0; prod stale-deletes exact (13 facts + 20 lines); export
verified; NEW mirror `server/specs/1120s_m3_spec.json`; f1120ss3 registered
(manifest 84). tts = boundary-RED by design (no M-3 leg; $10M constants
verified). Gates: manifest 3/3 · flow 447. `/bugs` at boot: no open reports.*

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
1. **Start every session with `/bugs`** (s55).
2. **SCH_K input sub-unit** (the s56 tail 4-7, DEFERRAL_AUDIT s60): 12a/12b
   charitable split · K16e/16f inputs + K18 16f term · K14a/b K-2/K-3 row
   conversion · 17c → M-2 col (c)/1099-DIV wiring.
3. **RS FA-export reconciliation pass** (queued since s32).
4. **S-20 B2-17 form units**: 8283-entity → 2553 → 2848 → 3115 app build.
5. **Ken ratifications pending (REVIEW_QUEUE):** s59 M-2 NNA distribution cap
   (tts M-2 compute implements it once ratified) · R007 AMT-matrix · 40%
   transitional election · s49 candidates · s53 partner-percentage diagnostic ·
   s57 K-1 health-insurance ZZ presentation.
6. *(Renumber queue: CLEARED except **3800**, which rides the future 3800/GBC
   entity unit — the audit ledger is the record. The 6198 + M-3 tts build
   units are unblocked-but-unscheduled; both specs now face-true + mirrored.)*

## ▶ Waiting on Ken / external
1. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
2. Ratifications: M-2 NNA cap (s59) · R007 AMT + 40% election (s47).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).

## Active gates
- **Flow-assertion gate GREEN s62** (447) + manifest 3/3 (trip-wire 84).
- s60 consolidated state stands: flow+MeF+spec+pins 583 · S5+S6 8/8 · affected
  suites 208+106+36 · tsc 0 · vitest 300 · live ORM probe PASS.
- Last full-suite GREEN = s54 `cd9b186` (pre-re-key; next full run picks up any
  straggler pins on the old page-1 keys — none surfaced in s60-s62 sweeps).
- ⚠ Shared-DB deploy state: mig 0187 APPLIED + seed rerun DONE (s60). Render
  deploy just needs the code push.
- ⚠ `test_k1_import_stage3.py` standalone-run fixture errors are PRE-EXISTING.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
