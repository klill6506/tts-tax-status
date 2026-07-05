# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-05, seventeenth session: built **NC D-400** (North Carolina individual)
end-to-end across ALL 4 legs — compute, input, render (face + identity header), diagnostics
→ **S-9 complete**. Self-contained state form (FormFieldValue-backed, no migration), the
GA-500 / SC1040 / AL Form 40 pattern.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: pull it, read `BUILD_ORDER.md` (order) / `SEASON_PLAN.md`
  (gates) / `PRODUCT_MAP.md` (scope). BUILD_ORDER is the single source of sequence.

## Active gates (all green)
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q` → **398 passed**
  (unchanged — NC D-400 is a self-contained state form, touches no 1040 flow). Fast (~1s, pure).
- **NC D-400** — pure compute `pytest tests/test_compute_nc_d400.py` (**14**, all 8 RS spec pins) · state-return
  `pytest tests/test_nc_d400_state_return.py` (**4**) · render `pytest tests/test_nc_d400_render_leg.py` (**6** =
  5 pure + 1 DB with the identity header) · diagnostics `pytest tests/test_nc_d400_diagnostics_leg.py` (**8**).
- **SC1040** (S-7) + **AL Form 40** (S-8) complete from prior sessions.
- `manage.py check` clean. **Test DB `test_postgres` exists** — use `--reuse-db` (fast). No migration this
  session (NC D-400 is FormFieldValue-backed, seeded via `seed_nc_d400` + `seed_rules`).
- ⚠ The NC D-400 diagnostics-leg test calls the rule FUNCTIONS directly (fast, ~1:47) — it does NOT run the
  full diagnostics runner, so it avoids the ~9-min sweep the AL40 diagnostics test incurs.

## ▶ RESUME HERE — **S-4 1065 core (IN PROGRESS — Ken directed 2026-07-05; leg 1a DONE)**
All 6 RS core specs cached to `server/specs/` (all `approved`, `a4f3370`). Full reconcile map in the
`.claude` memory `1065-core-s4-kickoff.md`. Tasks #6-11 in the tracker. DB tests are SLOW (~17 min/run — the
pooler-slow-not-hung reality; run 1065 pipeline tests in the background).

**✅ Leg 1a DONE (`10f1fd2`) — page-1 2025 face renumber** (Ken greenlit the prod-data touch; near-zero-risk,
no stale FFV deletion occurred). Verified vs the actual 2025 f1065.pdf: NEW line 20 = §179D energy-efficient
buildings (Form 7205, direct-entry); line 21 = Other deductions (was 20); line 22 = Total deductions Σ9-21
(was 21); **line 23 = Ordinary business income = 8−22** (was 22); **K1 now pulls line 23** (the page1→K
handoff, R-SCHK-1). seed_1065 → 286 lines. 15 DB tests + flow 398 + SE pure 36 green.

**▶ NEXT — leg 1b (Schedule K renumber + Analysis line):** Schedule K foreign taxes `K16a` → **line 21**;
**line 16 → K-2/K-3-attached checkbox** (international RED-defer, Decision A); add **K13b** (invest interest —
app currently mislabels as K13d), **K13c** (§59(e)(2)), **K13e** (other deductions). Then build the
**Analysis-of-Net-Income line** = `(ΣK 1-11) − (K12 + K13a + K13b + K13c + K13e + K21)` (spec R-SCHK-ANALYSIS;
ties M-1 L9 = M-2 L3). Add diagnostic **D_SCHK_HANDOFF** (K1 ≠ page-1 line 23), D_1065P1_COGS/4797/174A.

**⚠ DEFERRED render (flagged):** the f1065 coordinate map still keys page-1 20/21/22 (now stale) with no 23,
AND those page-1 coords were already MISALIGNED for the 2025 template (coords at RL y≈222 → fitz ~570, but
2025 lines 20-23 are at fitz ~475-511). The f1065 page-1 + Schedule K render needs a dedicated recalibration
leg (the flat-state render recipe: "00"/label anchors → 792−fitz_y → PNG-verify).

**Remaining S-4 legs (tasks #7-11):** Sch L balance check (L14=L22, D_L_BALANCE_*) · M-1/M-2 tie-outs · K-1
alloc reconcile (RECON-K1-K) · issuer-side **`PartnerK1Computed`** + 1065→1040 import (mirror 1120-S) · 1065
flow-assertion gate. **What exists (reconcile, don't rebuild):** FORMULAS_1065, k1_allocator.py,
compute_1065_se.py. **RED-defers:** K-2/K-3, §704(c) math, §706(d), item-L roll-forward, M-3, OBBBA flags.

*(Other unblocked SPINE items if Ken redirects: S-3 brokerage front end ∥, S-11 1041 module, S-5/S-6.)*
- **Reusable flat-state-form recipe (proven THREE times — GA-500 / SC1040 / AL40 / NC D-400):** download the
  DOR PDF → verify flat (no AcroForm; if the "web-fill" version has widgets, grab the **handwritten**
  version) → **strip any leading instructions cover** (`delete_page(0)`) → find value anchors (pre-printed
  "00" glyphs, or grid/label baselines) → RL baseline = 792 − anchor_y1 (+~2–3.5pt descent nudge) → render
  synthetic-value PNG → measure value-vs-anchor delta → visually verify → append/overlay + identity-header
  overlay. ⚠ filing-status circles sit between the number and the label (x≈78 for NC), not at the far-left
  rotated section label. See `coordinates/fnc_d400.py` + `render_nc_d400`.

## This session's commits (all pushed to origin/main)
- NC D-400: `69cf82b` compute · `b31d5c7` input (seed + wiring + frontend) · `c704f21` render (face +
  identity header) · `8358e74` diagnostics. Also fixed a latent AL40 `refresh_from_federal` bug (it was
  missing the AL40 branch → fell through to the GA pull); added AL40 + NC branches.

## ▶ NC D-400 v1 boundaries (stated, not silent — see DEFERRAL_AUDIT.md)
- Amended return (L22/L24, Schedule AM), Form D-422 est-tax underpayment interest (L26e), NC NOL
  (Schedule S L39), Schedule PN-1 — direct-entry / not computed (each has a D_NCD400_* diagnostic).
- D-400TC credits (L16) + consumer use tax (L18) direct-entry. The 20% depreciation-recovery installments
  (L23f/L24f) are direct-entry (need 2020-2024 records; a TY2025 add-back first recovers TY2026).
- Render: line 13 (Sch PN %) placement is approximate (PYNR-only box); the withholding/other-payments
  totals print in the 20a/21a sub-boxes (the form has no single line-20/21 total box). Year-guarded to 2025.

## ▶ RS follow-ups (rides a dedicated RS session)
- **NC_D400 spec is status `draft`** — promote to `active` (the SC1040/AL40 precedent). All 8 spec test
  scenarios have `expected_outputs` (all verified in tts).
- Carried — **AL_FORM_40 spec is `draft`** (promote→active). SC1040 `D_SC1040_BRACKET` threshold vs SC Code
  §12-6-510 (non-blocking). GA-500 `R-GA500-MIL` (RS spec correct; tts fix handed off `task_f550dfd2`).
  `8867_spec.json` stale notes; RS Schedule D `D_8949_006`; 8995 sibling loader D_8995_001 note.

## ▶ Waiting on Ken (carried)
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5). S2/S3/S4 ready (UNSIGNED).
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` (order) · `SEASON_PLAN.md` (gates) · `PRODUCT_MAP.md` (scope).
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
