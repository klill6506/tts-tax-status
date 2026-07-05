# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-05, sixteenth session (**two forms shipped**): (1) completed **SC1040** —
the Schedule NR render was the last leg → SC1040 fully done (S-7). (2) built **AL Form 40** (Alabama
individual) end-to-end across ALL legs — compute, input, render (face + identity header), diagnostics
→ **S-8 complete**. Both are self-contained state forms (FormFieldValue-backed, no migration).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: pull it, read `BUILD_ORDER.md` (order) / `SEASON_PLAN.md`
  (gates) / `PRODUCT_MAP.md` (scope). BUILD_ORDER is the single source of sequence.

## Active gates (all green)
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q` → **398 passed**
  (unchanged — AL40/SC1040 are self-contained state forms, touch no 1040 flow). Fast (~1.1s, pure).
- **AL Form 40** — pure compute `pytest tests/test_compute_al40.py` (**15**, all 5 RS spec pins) · state-return
  `pytest tests/test_al40_state_return.py` (**3**) · render `pytest tests/test_al40_render_leg.py` (**5** =
  4 pure + 1 DB with the identity header) · diagnostics `pytest tests/test_al40_diagnostics_leg.py` (**4**).
- **SC1040** — compute 16 · state-return 4 · render 10 · diagnostics 4 (all green from part 1).
- `manage.py check` clean. **Test DB `test_postgres` exists** — use `--reuse-db` (fast). No migration this
  session (both forms are FormFieldValue-backed, seeded via `seed_al40` / `seed_sc1040`).
- ⚠ The diagnostics-leg DB test (`test_al40_diagnostics_leg`) takes ~9 min (full diagnostics runner over a
  return set) — run it alone / in the background, not bundled, or it blows the 10-min cap.

## ▶ RESUME HERE — next SPINE item: **S-9 NC D-400 app build**
BUILD_ORDER's NEXT after S-8 is **S-9 NC D-400** (North Carolina individual; RS spec authored 7/4),
∥ **S-3 brokerage front end**. Also on deck: **S-10 GA-700** (gated behind S-4 1065 core) and the
**S-11 1041 module** app build (RS authoring complete 2026-07-05). Pick the top unblocked SPINE item
(S-9) unless Ken directs otherwise.
- **Reusable flat-state-form recipe (proven twice now — SC1040 + AL Form 40):** download the DOR PDF →
  verify flat (no AcroForm) → find value anchors: pre-printed "00" glyphs (SC) OR grid row-lines /
  label baselines (AL, which has neither "00" nor boxes) → RL baseline = 792 − anchor_y1 (+~2–3.5pt
  font-descent nudge) → render synthetic-value PNG → **measure value-vs-anchor delta programmatically** →
  visually verify → append/overlay + identity-header overlay (name/SSN/address/filing-X). See
  `coordinates/fal40.py` + `render_al40` / `_al40_header_overlay`.

## This session's commits (all pushed to origin/main)
- SC1040 Schedule NR: `d9fa2b0` (render leg) · `4a43e41` (close docs).
- AL Form 40: `28ceeab` compute · `3edce72` input (seed + wiring + frontend) · `938846c` face render ·
  `941cd46` diagnostics · `c81bcf2` identity header.

## ▶ AL Form 40 v1 boundaries (stated, not silent — see DEFERRAL_AUDIT.md)
- Form 40NR (true nonresidents), Schedule ATP additional taxes (L19) + penalties (L31), Form NOL-85A NOL
  alternative, Schedule OC nonrefundable/refundable credits — direct-entry / not computed (each has a
  D_AL40_* info diagnostic). §414(j)/SS/govt-pension/state-refund income is excluded AT SOURCE (no line).
- The federal FIT-worksheet pull feeds only fed-l22 (1040 L22), fed-agi (L11), and EIC/ACTC/AOC (27/28/29),
  scoped to the 1040 form; NIIT (8960), refundable adoption (L30), Form 2439 stay preparer-entered.
- The DOR tax-table midpoint rounding for small taxable incomes is approximated by the continuous
  2/4/5% bracket formula (no RS pin exercises L17).

## ▶ RS follow-ups (rides a dedicated RS session)
- **AL_FORM_40 spec is status `draft`** — promote to `active` (the SC1040 precedent; SC's draft→active +
  pin fix landed RS `6e22b70`). The spec's 5 test scenarios have `expected_outputs` (all verified in tts).
- SC1040: `D_SC1040_BRACKET` — the $3,560/$17,830 thresholds corroborated by SC1040TT but not confirmed
  vs SC Code §12-6-510 (non-blocking).
- Carried — GA-500 `R-GA500-MIL` (RS spec correct/authoritative; tts fix handed off `task_f550dfd2`);
  `8867_spec.json` stale notes; RS Schedule D `D_8949_006`; 8995 sibling loader D_8995_001 note.

## ▶ Waiting on Ken (carried)
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5). S2/S3/S4 ready (UNSIGNED).
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` (order) · `SEASON_PLAN.md` (gates) · `PRODUCT_MAP.md` (scope).
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
