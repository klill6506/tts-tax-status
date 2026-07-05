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

## ▶ RESUME HERE — next SPINE item: **S-3 brokerage front end** ∥ or **S-11 1041 module app build**
BUILD_ORDER after S-9: the remaining unblocked SPINE items are **S-3 brokerage extraction-to-production
front end** (OCR/parse + YELLOW render + preparer-confirm UI; skeleton already done, not spec-gated, ∥) and
the **S-11 1041 module** app build (fiduciary DNI/IDD engine + Sch G + K-1 issuance + GA 501; RS authoring
complete 2026-07-05). **S-10 GA-700** stays gated behind S-4 1065 core. **S-4 1065 core** (compute build of
the 35 formulas) and **S-5/S-6** (boundary + PAL/basis app builds) are also open app-build work — all RS
authoring done. Pick the top unblocked item unless Ken directs otherwise.
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
