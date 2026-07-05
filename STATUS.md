# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-05, sixteenth session (**SC1040 Schedule NR render leg**): rendered the SC
Schedule NR (part-year/nonresident) summary lines 31-48 as a coordinate overlay appended behind the
SC1040 face. **This was the last remaining S-7 leg — the SC1040 is now COMPLETE across all legs**
(compute / input / render-face+header / diagnostics / Schedule-NR render). Render-only; no migration.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: pull it, read `BUILD_ORDER.md` (order) / `SEASON_PLAN.md`
  (gates) / `PRODUCT_MAP.md` (scope). BUILD_ORDER is the single source of sequence.

## Active gates (all green)
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q` → **398 passed**
  (unchanged — SC1040 is a self-contained state form; this session was render-only). Fast (~1.3s, pure).
- **SC1040 pure compute** — `pytest tests/test_compute_sc1040.py -q` → **16 passed** (SC1040TT midpoint
  138/138; spec scenarios + Schedule NR + per-owner sub-boxes).
- **SC1040 DB tests** — state-return `pytest tests/test_sc1040_state_return.py` (**4**); render
  `pytest tests/test_sc1040_render_leg.py` (**10** = 8 pure + 2 DB, incl. the new part-year 5-page
  Schedule-NR attach); diagnostics `pytest tests/test_sc1040_diagnostics_leg.py` (**4**).
- Combined SC1040 suite + flow gate = **422 passed** (9:02, `--reuse-db`). `manage.py check` clean.
- **Test DB `test_postgres` exists** — use `--reuse-db` next session (fast). No migration this session
  (Schedule NR render is FormFieldValue-backed, uses the existing `seed_sc1040` rows).

## ▶ RESUME HERE — S-7 is DONE. Next SPINE item: **S-8 AL Form 40 app build**
BUILD_ORDER's NEXT after S-7 is the remaining state app-builds: **S-8 AL Form 40** (⚠ fed-tax-deduction
quirk — Alabama allows a federal income tax deduction), then **S-9 NC D-400**, ∥ **S-3 brokerage front
end**. The RS specs for S-8/S-9 are already authored (7/4). Also on deck: the RS authoring spine is clear
to the **S-11 1041 module** [RS/Ken]. Pick the top unblocked SPINE item (S-8) unless Ken directs otherwise.
- **Reusable render recipe (proven this + prior session)** for the next flat state form: render blank
  page → `get_text('words')` on the "00"/"%%" glyph anchors → derive column right-edges (anchor_x1 − ~12.5)
  → set RL baseline = 792 − fitz_y1 **+ ~3.5pt font-descent nudge** → render synthetic-value PNG → measure
  value-vs-anchor delta programmatically (target ≈0) → visually verify → append after the face, gated on a
  flag. See `coordinates/fsc_schedule_nr.py` + `render_sc_schedule_nr`.

## This session's commit (pushed to origin/main)
- `d9fa2b0` — SC1040 Schedule NR render leg (coordinates/fsc_schedule_nr.py + render_sc_schedule_nr +
  COORDINATE_REGISTRY + render_sc1040 append + 6 tests + RS SC_SCHEDULE_NR spec cached to server/specs).

## ▶ SC1040 v1 boundaries (stated, not silent — see DEFERRAL_AUDIT.md)
- **Schedule NR render:** only the summary lines 31-48 render; the income-detail lines 1-30 (page 1)
  attach blank — the embedded compute models aggregate federal AGI (L31 Col A) + SC-source AGI (L31 Col B)
  entered by the preparer, not the line-by-line breakdown. NR-45 proration is displayed ×100 as "NN.NN"
  (the compute rounds the fraction to 2 decimals → whole-percent precision).
- Additions b/c/d fold into the line-e lump; subtractions f/g/h/j-n/r/s/u fold into line-v. §179
  business-income limit not modeled (D_SC1040_179 warns). Refundable-credit detail not itemized (feeds L23).
  Identity suffix / county code / phone / DOB not rendered. SC4972 / I-335 / catastrophe / SC2210 =
  direct-entry (each has a D_SC1040_* info diagnostic).

## ▶ RS follow-ups (rides a dedicated RS session)
- SC1040 spec: still OPEN (genuine verify, not blocking) — `D_SC1040_BRACKET` notes the $3,560/$17,830
  thresholds are corroborated by SC1040TT but not independently confirmed vs **SC Code §12-6-510**.
  (The $50k pin + draft→active promotion were RESOLVED 2026-07-05, RS `6e22b70`.)
- **Carried — GA-500 `R-GA500-MIL` military exclusion**: RS spec is CORRECT/authoritative; the app was
  the one BEHIND (over-inclusive `min(mret,35000)`) → tts fix handed off (`task_f550dfd2`). SB 31 full
  military exemption starts TY2026 (re-verify the enrolled bill before the TY2026 build).
- Carried: `8867_spec.json` stale D_8867_002/AOTC notes; RS Schedule D `D_8949_006`; SCHA
  `scha_qualified_contributions_cash`; 8995 sibling loader D_8995_001 retirement note.

## ▶ Waiting on Ken (carried)
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5). S2/S3/S4 ready (UNSIGNED).
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
4. tts-tax-status may carry Ken's uncommitted WIP — the status-mirror sync uses explicit file adds; leave
   Ken's WIP untouched.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` (order) · `SEASON_PLAN.md` (gates) · `PRODUCT_MAP.md` (scope).
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
