# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-05, fifteenth session (**SC1040 build — S-7**): built the South Carolina
SC1040 individual return through FOUR legs — compute, input, render (face + page-1 identity
header), and diagnostics. The **resident SC1040 is COMPLETE and filable end-to-end**; the ONLY
remaining S-7 item is the **Schedule NR render** (part-year/nonresident). Also migrated the three
planners (BUILD_ORDER/SEASON_PLAN/PRODUCT_MAP) canonical to `tts-tax-status`, retired
SEASON_CHECKLIST, and fixed a real latent serializer bug.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners now live in `tts-tax-status`**: pull it, read `BUILD_ORDER.md` (order) / `SEASON_PLAN.md`
  (gates) / `PRODUCT_MAP.md` (scope). BUILD_ORDER is the single source of sequence.

## Active gates (all green)
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q` → **398 passed**
  (unchanged — SC1040 is a self-contained state form, touches no 1040 flow). Fast (~1.3s, pure).
- **SC1040 pure compute** — `pytest tests/test_compute_sc1040.py -q` → **16 passed** (SC1040TT midpoint
  138/138 vs the published SCDOR table; spec scenarios + Schedule NR + the per-owner sub-boxes).
- **SC1040 DB tests** — state-return `pytest tests/test_sc1040_state_return.py` (**4**, create→map→pull→
  compute + duplicate 409 + SC-business 400); render `pytest tests/test_sc1040_render_leg.py` (**5** = 4
  pure + 1 DB, tax + identity visible); diagnostics `pytest tests/test_sc1040_diagnostics_leg.py` (**4**).
- `manage.py check` clean. **Test DB `test_postgres` is FRESHLY BUILT** (a mid-session kill-cascade forced a
  clean rebuild — ~11 min on the slow pooler); use `--reuse-db` next session (fast). One idle teardown
  session may linger (harmless; `scripts/drop_test_db.py` clears it if a `--create-db` run ever fails).
- No new migration this session (SC1040 is FormFieldValue-backed, seeded via `seed_sc1040`, GA-500 pattern).

## ▶ RESUME HERE — S-7 SC1040: build the **Schedule NR render** (the last leg)
The resident SC1040 is fully done (compute/input/render-face/render-header/diagnostics, all green +
committed). The ONE remaining item to tick S-7 complete: render the **SC Schedule NR** (2-page
part-year/nonresident schedule).
- **Groundwork already in place:** template `resources/state_forms/SC/2025/sc_schedule_nr.pdf` +
  manifest entry `fsc_schedule_nr` (`SC-SCHEDULE-NR`, 2 pages, sha `6301e35e…`). The embedded Schedule
  NR **compute already runs** inside `compute_sc1040` (part-year/nonresident branch) and persists
  `NR-43/NR-44/NR-45/NR-47/NR-48` → SC1040 line 5.
- **To build:** `coordinates/fsc_schedule_nr.py` (Col A federal / Col B SC two-money-column layout;
  auto-extract value boxes from the flat PDF like the face — the "00" cents-anchor recipe), a
  `render_sc_schedule_nr()` helper appended after the SC1040 face inside `render_sc1040` (attach only
  when `PYNR` is true, GA-500 Schedule-1 append precedent), register `fsc_schedule_nr` in
  `COORDINATE_REGISTRY`, visually verify (render→PNG→Read), add render tests.
- **Recipe (proven this session):** render blank page to PNG + `get_text`/`get_drawings` to find the
  "00" anchors → derive right-edge coords → render synthetic-value PNG → visually verify every cell.

## This session's commits (all pushed to origin/main)
- `307a810` compute engine · `a8cb291` seed + short-key refactor · `81e0809` state-return wiring +
  frontend picker · `4cb497a` **serializer bug fix** (renewable_facilities) + SC1040 section tabs ·
  `af2c6a7` face render · `13443d5` identity header render · `118613d` diagnostics leg.
- Planner migration: tax-app `65eb92d` (retire SEASON_CHECKLIST, boot pulls tts-tax-status);
  tts-tax-status `b54c111` (BUILD_ORDER/PRODUCT_MAP canonical + SEASON_PLAN re-cut).

## ▶ RS follow-ups (rides a dedicated RS session)
- **✅ RESOLVED 2026-07-05 (RS `6e22b70`) — SC1040 spec promoted `draft` → `active` + the wrong $50k
  test pin corrected** ($2,533 placeholder → the published SC1040TT $2,360, verified 138/138). Loader
  `load_sc1040.py` fixed + re-seeded RS Supabase; deployed export now `status: active` + `L6: 2360`
  (verified live). Still OPEN (genuine verify, not blocking): `D_SC1040_BRACKET` notes the
  $3,560/$17,830 thresholds are corroborated by SC1040TT but not independently confirmed vs **SC Code
  §12-6-510**.
- **Carried — GA-500 `R-GA500-MIL` military exclusion is WRONG in RS** (encodes `L3+L8`; tts fixed +
  canonical `min(mret,35000)`). Handoff `docs/rs_handoff/2026-07-05_ga500_military_exclusion_fix.md`.
- Carried: `8867_spec.json` stale D_8867_002/AOTC notes; RS Schedule D `D_8949_006`; SCHA
  `scha_qualified_contributions_cash`; 8995 sibling loader D_8995_001 retirement note.

## ▶ SC1040 v1 boundaries (stated, not silent — see DEFERRAL_AUDIT.md)
- Additions b/c/d fold into the line-e "Other additions" lump (`add-oth`); subtractions f/g/h/j-n/r/s/u
  fold into the line-v "Other subtractions" lump (`sub-oth`) with later-year SC depreciation. §179
  business-income limit not modeled (D_SC1040_179 warns). Refundable-credit detail (L19/L20/22a-e) not
  itemized — the aggregate feeds L23. Identity suffix / county code / phone / DOB not rendered. SC4972 /
  I-335 / catastrophe / SC2210 = direct-entry (each has a D_SC1040_* info diagnostic).

## ▶ Waiting on Ken (carried)
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5). S2/S3/S4 ready (UNSIGNED).
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
4. tts-tax-status has Ken's uncommitted WIP (`PRODUCT_MAP.md` edit + `CHANGE_REGISTER.md`) — left
   untouched this session; the status-mirror was synced with explicit file adds (not `git add -A`).

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` (order) · `SEASON_PLAN.md` (gates) · `PRODUCT_MAP.md` (scope).
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
