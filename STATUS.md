# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-07, twenty-sixth session (Ken-directed detour). Unit: **Lacerte regression
harness v1 BUILT + first real 2025 return recreated in-app and diffed against Lacerte** —
`scripts/lacerte_regression/` (parse → load → compare). The first return surfaced **two live engine
bugs** (fix chips spawned; see below). MeF entity track (Scenario-5 doc mappers) and S-11 1041 legs
6-8 both unchanged from session 25.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: pull it, read `BUILD_ORDER.md` (order) / `SEASON_PLAN.md`
  (gates) / `PRODUCT_MAP.md` (scope). BUILD_ORDER is the single source of sequence.
- **PII rule**: this file mirrors to the PUBLIC status repo — regression clients are referred to by
  number only. The mapping + PDFs + facts JSONs + diff reports live in `D:\tax-test-data\` (never a repo).

## 🔴 KNOWN ENGINE BUGS (found by Lacerte regression return #1, 2026-07-07)
1. **Sch E line 22 — released prior-year passive loss never flows back.** A rental with current-year
   net income + `prior_year_unallowed_passive` computes 8582 correctly (line 3 ≥ 0 → all losses
   allowed) but `compute_schedule_e.py` writes `line22 = -rental_allowed` from the per-activity
   allocation, which assigns 0 when the activity's own income absorbs its prior loss → Sch 1 L5 /
   AGI overstated by the carryover. Also the prior loss is folded into line 21 (IRS face: line 21 =
   line 3 − line 20; the PY-loss deduction belongs on line 22). **Fix chip spawned.**
2. **Form 8960 line 4a — passive rental net income missing from NII** (NIIT understated).
   `compute_8960` has a `rental` param; the caller doesn't feed Sch E net. **Fix chip spawned.**
3. *(Policy, → REVIEW_QUEUE)* App persists cents on computed lines; Lacerte/IRS round each line to
   whole dollars. Comparator rounds before diffing, but decide round-at-write vs round-at-render.

## ▶ RESUME HERE — three live threads
**(0) NEW: fix the two regression bugs above** (chips exist; RS specs 8582/Sch E/8960 first, per
CLAUDE.md), then re-run the comparator on regression return #1 — expected result: **0 differences**
(federal refund + GA refund match Lacerte to the dollar). Return UUIDs + full line-by-line diff:
return #1's `*_diff_report.md` in `D:\tax-test-data\` (local only).

**(A) Spine (BUILD_ORDER): S-11 1041 legs 6-8 unchanged** — leg 6 GA Form 501 (resident-only v1, RS
spec `GA501`), then leg 7 frontend verify, leg 8 flow gate. Leg-5 follow-on: f1041sk1 K-1 PDF.

**(B) MeF entity e-file (∥ track): 1120-S ATS Scenario-5 doc mappers unchanged** — next = 1125-A
(smallest), then 1125-E/4562/4797/8825 + itemized statements; then S5 through the real engine.
SOR mailbox watch continues (want-list email sent 2026-07-07).

## Lacerte regression harness (NEW this session)
- `scripts/lacerte_regression/`: `parse_lacerte_pdf.py` (client-copy PDF → facts JSON skeleton:
  expected federal/GA summary lines high-confidence + best-effort inputs) → human/Claude completes
  the facts JSON → `load_return.py` (ORM entry into an existing 1040 shell + compute + GA-500
  attach/compute) → `compare_return.py` (whole-dollar diff vs expected → `*_diff.md`, exit 1 on any
  diff). README has the full procedure. **Loader writes to the shared PROD DB** (that's the point —
  returns land in the app), so it hard-aborts if the return already has W-2s/dependents/rentals.
- Return #1 results: **30 lines matched exactly** (wages/int/div aggregation, OBBBA SALT phase-down
  → $10k floor, QDCGT incl. 20% bracket, the whole 8959 chain incl. withholding reconciliation →
  25c, 8889 family-HDHP $0-deduction, depreciation engine to-the-dollar incl. AMT + GA columns,
  8812 $0-CTC via AGI phaseout, QBI $0 with −5,113 carryforward, GA-500 std/exemptions/5.19%);
  29 lines differ, ALL traced to bugs 1+2 above. Harness validated: it reproduces the hand analysis.
- TaxWise: same spine, only the parser differs; October TaxWise importer can emit the same
  facts-JSON schema (the contract between stages).
- Dependent DOBs for return #1 not in the Lacerte print — **Ken to add in the UI** (CTC stays $0
  either way at this AGI).

## Active gates (all green, unchanged — no compute code touched this session)
- **Flow-assertion gate** — 422 passed (untouched; harness is scripts-only).
- MeF 1120-S mapper 10 passed · S-11 1041 suites green · client suite 275 passed (all from session 25).
- `manage.py check` clean. Test DB `test_postgres` — `--reuse-db`; ⚠ pooler runs SLOW — background them.

## ▶ RS / compute follow-ups (non-blocking, carried)
- **NEW**: 8582 "line 3 ≥ 0 → stop" — app fills Parts II/III where the form says stop (cosmetic,
  fold into bug-1 fix). Sch B prints at $597 interest (< $1,500 threshold — harmless).
- **S-11 1041**: legs 6/7/8; f1041sk1 K-1 PDF; Sch A charitable; Sch G Part II; D_1041_AMT trigger;
  specs draft→active; K-1 v1 class deferrals; seed payments-section reconciliation.
- **S-6**: §172 forward NOL; R3 REP compute bypass; R4 §465 Form 6198 compute (deferred by design).
- **f1065 M-1 nuance**: FORMULAS_1065 M1_5/M1_8 include 4c/7b, RS spec lists 4a/4b/6a/7a — reconcile.
- Carried GA-700 / NC_D400 / AL_FORM_40 / SC1040 / GA-500 / 8867 items (STATUS_ARCHIVE 2026-07-06).
- Carried S-4: 6 staged 1065 flow assertions + `D_M2_1`/`D_K1_704C`/`D_K1_706D`.

## ▶ Waiting on Ken / external
1. **SOR mailbox watch** — want-list email sent 2026-07-07 (1040 v5.3/v5.4, 1120x v6.2, 1065 v5.3,
   1041 v5.3). When they land: drop zips in `docs/mef/`, file/hash/extract per `README_schemas.md`.
2. IFA-upload 1040 scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5).
3. TY2025 4868 schema (carried).
4. **Regression return #1 dependent DOBs** (app UI) + decision on rounding policy (REVIEW_QUEUE).

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` (order) · `SEASON_PLAN.md` (gates) · `PRODUCT_MAP.md` (scope).
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
