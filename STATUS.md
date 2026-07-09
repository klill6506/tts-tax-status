# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-09, thirty-seventh session ("go" — the standing overnight work order).
Unit: **S6 unit 3 — the Scenario-6 build `mef_build_ats_1120s_s6` COMPLETE** (all 41 key
lines tie to the dollar; submission live-XSD-valid; PIN-signed). **THE OVERNIGHT WORK ORDER
(units 1→2→3) IS DONE.***

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — the 1120-S ATS completion lane (mission rule: full set, THEN test)
The S6 build lane (s34→s37) is closed. Next in BUILD_ORDER's ladder, all Ken-gated or
Ken-directed:
1. **S7/S8 declared-forms ruling** (Ken/e-help; recommendation: declare only forms the firm
   files — 5471/8975/8858 OUT → S7/S8 likely NOT required).
2. Any required S7/S8 builds → **full-set bundle** → the **live upload loop** (the 8941
   key-inversion + the s36 entity-8824 ratifications resolve in-loop).
3. Interleavable while waiting: the REVIEW_QUEUE set (RS FA-export reconciliation pass ·
   1065/1041 allocator residual-offset units) · the TaxWise forms-usage gap-analysis unit
   (report incoming) · A2A pull-forward planning.

## Session-37 state (S6 unit 3)
- **`apps/efile/ats/scenario6_1120s.py` + `mef_build_ats_1120s_s6`** (rollback-txn, seq 22):
  engine-driven build, ALL 41 pinned key lines match to the dollar, submission + manifest
  validate vs 2025v6.2. Artifacts → `docs/mef/ats_out/1120s_scenario6/` (built with the
  documented placeholder originator id — pass the real one via `--efin` on the real run;
  ids in the local README.txt).
- **PIN signature exercised end-to-end** (Signature1120SInfo; no binaries).
- **NEW: K-1 box-10 code channel** — XSD requires a code; caller-supplied
  `extract.k10_other_income_code` (scenario asserts the key's "A"); un-asserted nonzero K10
  still refuses. App input leg + print-side box-10 code → DEFERRAL_AUDIT s37.
- **Documented-quirk overrides at build (no engine changes)**: 8941 = LAW (51,014; key's
  inverted 12,753 documented) · truck 8824 `is_real_property=True` (face = key exact, zero
  feeds) · M1_6b=0 override · 1125-A 540-typo absorbed · Silverado 24,492 · details in the
  scenario module docstring + form_coverage_tracker s37.
- Tests: S6 scenario 4 DB green · MeF 1120-S mapper 64 green · flow gate 447 (unchanged —
  no compute changes). No migrations; no client changes.

## ⚠⚠ UPLOAD GATE (unchanged)
The S6 package must NOT upload until the 8941 key-inversion is resolved with the ATS
assistor/e-help (REVIEW_QUEUE s34) — and per the mission's no-piecemeal rule, nothing
uploads until the full 1120-S scenario set is complete.

## Active gates
- **Flow-assertion gate 447** — green (1.58s pure run this session).
- ⚠ Test-DB lingering-session class (new note): a fresh pytest run immediately after a
  completed one can fail create with "database is being accessed by other users" — rerun
  with `--reuse-db`; don't drop.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Recorded in SEASON_PLAN (scope-change note) + BUILD_ORDER (mission header + NEXT ladder).
**No piecemeal ATS testing** — full 1120-S scenario set FIRST, then the upload loop.

## ▶ Waiting on Ken / external
0. **TaxWise forms-usage report incoming** → the forms-gap-analysis unit (checklists /
   build queue / RS authoring queue). A2A pull-forward after the 1120-S set (BUILD_ORDER
   re-order pending).
1. **S7/S8 declared-forms ruling** (Pub 1436; e-help) — shapes "the full 1120-S set."
2. **8941 key-inversion** — law 51,014 vs key 12,753 (REVIEW_QUEUE s34) — e-help.
3. Ratify the two s36 entity-8824 spec-silent rulings (REVIEW_QUEUE s36).
4. ATS assistor / e-help call (866-255-0654) — the five asks (docs/mef/ATS_UPLOAD_RUNBOOK.md).
5. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR + **709 family schemas** — SOR re-request.
6. SEASON_PLAN month-by-month re-cut for 1120/709 — dedicated planning session with Ken.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
