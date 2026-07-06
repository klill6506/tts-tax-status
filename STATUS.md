# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-06, twenty-second session. Two units this session: (1) **S-4 f1065 render recalibration
DONE → Form 1065 FULLY TICKS; S-4 complete end-to-end** (coordinate overlay → AcroForm backend, `2599621`/
`c0dbff8`); (2) **S-5 entity-boundary diagnostics leg 1 DONE, live on prod** (`50f2874`) — 6 `D_EB_*` REDs for
1065/1120-S + `EntityBoundaryAssertion` model (migs 0170/0171 applied to prod). Build state: **idle — Ken
directs;** the teed-up next is S-5 leg 2 (React input UI) or another spine item.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: pull it, read `BUILD_ORDER.md` (order) / `SEASON_PLAN.md`
  (gates) / `PRODUCT_MAP.md` (scope). BUILD_ORDER is the single source of sequence.

## Active gates (all green)
- **Flow-assertion gate (all entities)** — `cd server && pytest tests/test_flow_assertions.py -q` → **422
  passed** (incl. the 1065 gate: 22 active RS assertions + 2 guards). UNCHANGED by the render leg (no
  compute change). Fast (~1.5s, pure).
- **1065 render leg** — `pytest tests/test_1065_render_leg.py --reuse-db -q` → **3 passed** (valid 6-page PDF +
  page-1/Sch-K values in text layer + Sch-L contra-net/parens + M-1/M-2 tie). ~60s (pooler).
- **S-5 entity boundary (NEW)** — `pytest tests/test_entity_boundary_leg.py --create-db -q` → **13 passed**
  (fire/no-fire per diagnostic across 1065+1120-S + clean-return no-false-REDs). Migs 0170/0171 applied to
  prod; `seed_rules` reseeded (6 `D_EB_*` active, `D_L_M3` deactivated). ~5min with `--create-db`.
- `manage.py check` clean. **Test DB `test_postgres` exists** — use `--reuse-db`. New DB tests must seed the
  forms they use and filter `FormLine` by `section__form=` NOT `form_definition=`. Must CREATE the `TaxYear`
  (not `get` it) in the fixture.
- ⚠ DB runs on the shared Supabase pooler are SLOW/flaky ("terminating connection due to administrator
  command" = transient, retry; never `drop_test_db` a slow run). Run DB tests in the background.

## ▶ RESUME HERE — **S-4 1065 core COMPLETE (all legs + render). Build is idle; next unit = Ken directs.**
Full detail in `.claude` memory `f1065-render-acroform-leg.md` + `1065-core-s4-kickoff.md`.

**✅ S-4 f1065 render recalibration (this session)** — the last deferred S-4 leg. `f1065` now renders via the
**AcroForm-fill backend** (was the legacy, never-calibrated coordinate overlay on pre-2025 numbering):
- `field_maps/f1065_2025.py` — 191 mappings (header + page 1 2025-numbering + Schedule K + Schedule L +
  M-1 + M-2), keyed by the seed `line_number`. Registered `f1065` in `renderer.ACROFORM_FORM_IDS`;
  `coordinates/f1065.py` marked SUPERSEDED.
- Field→line map extracted from the PDF's own label text. ⚠ seed L-keys OFFSET from form line#s (seed
  L15=form line 14 total assets, L8=form line 7b); `render_tax_return` already does the Schedule-L 4-column
  translate + contra-net + parens (shared with 1120-S).
- Visually verified a fully-populated 6-page render; `test_1065_render_leg.py` (3 DB green).
- **✅ Follow-up done same session (`c0dbff8`): Schedule M-1 line-4/line-7 total boxes** now render via
  render-layer synthesis (`M1_4`=4a+4b+4c, `M1_7`=7a+7b) → `f6_132`/`f6_139`. **The f1065 render now has NO
  known display gaps.** (Compute-vs-spec nuance noted below: M1_5/M1_8 include 4c/7b, the RS spec lists only
  4a/4b + 6a/7a — a separate compute leg.)

**✅ S-5 entity-boundary diagnostics — leg 1 (this session, `50f2874`)** — the season-one "no silent gap" net
for entity returns (RS `ENTITY_BOUNDARY`; cached `server/specs/entity_boundary_spec.json`). Live on prod.
- `apps/diagnostics/rules_entity_boundary.py` — 6 diagnostics for 1065 + 1120-S: `D_EB_M3` (err),
  `D_EB_K2K3` (err), `D_EB_DFE_OK` (info), `D_EB_704C` (err), `D_EB_754` (err), `D_EB_APPORT` (err).
- **M-3 + the K-2/K-3 foreign gate AUTO-DERIVE** from return data (Sch L `L15d`, gross receipts `1a`,
  `K21`/`K16`); the rest read `EntityBoundaryAssertion` (OneToOne→TaxReturn, migs **0170/0171 applied to
  prod**) — 7 flags, ALL defaulting to the supported/non-firing state (no false RED without an explicit flag).
- Overlap resolved: the superseded 1065-only `D_L_M3` warning is **deactivated** (reseeded via `seed_rules`).
- `test_entity_boundary_leg.py` (13 DB). Simplifications documented (M-3 receipts=`1a` proxy; DFE criteria
  2-4 → `k2k3_dfe_confirmed_met` flag) — see the memory + DEFERRAL_AUDIT.

**▶ NEXT — Ken directs.** Teed up: **S-5 leg 2 — the React input UI** for the `EntityBoundaryAssertion` flags
(until it ships, §704(c)/§754/apportionment fire only via programmatic/admin flags; the auto-derived arms
fire from data today). Other unblocked spine items: **S-11 1041 module** · **S-6** PAL/basis · **S-3 brokerage
front end** (∥) · **S-13/S-14 1120 + state C-corp** (⚠ 1120 C-corp NOT season-one scope per SEASON_PLAN).

## This session's commits (pushed to origin/main)
- `2599621` — S-4 f1065 render recalibration → AcroForm backend (`field_maps/f1065_2025.py`, `ACROFORM_FORM_IDS`,
  `test_1065_render_leg.py`, `f1065.fields.json`, coordinate map SUPERSEDED).
- `c0dbff8` — Schedule M-1 line-4/line-7 total boxes (render-layer synthesis) → f1065 render has no known gaps.
- `b273fe7` — docs (M-1 gap resolved).
- `50f2874` — **S-5 entity-boundary diagnostics leg 1**: `rules_entity_boundary.py` (6 `D_EB_*`) +
  `EntityBoundaryAssertion` model (migs 0170/0171) + `seed_rules` reseed + `test_entity_boundary_leg.py` (13).

## ▶ RS / compute follow-ups (all non-blocking)
- **NEW (from the f1065 render leg) — compute-vs-spec M-1 nuance.** `FORMULAS_1065` M1_5 = 1+2+3+4a+4b+4c and
  M1_8 = 6+7a+7b, but the RS `1065_M1` spec formulas (R-M1-5 / R-M1-8) list only 4a/4b and 6a/7a (no 4c/7b).
  The form face names only 4a/4b and 7a (4c/7b are "other itemize" catch-alls). Reconcile compute↔spec (or
  confirm 4c/7b are intended extensions). Render ties to the computed totals either way (M-1 total boxes DONE
  `c0dbff8`).
- Carried GA-700: Schedule-8 spec line-numbering vs the form face; display-only subtotals (S1_3/S8_10/
  S5_8/S6_5); GA-700 spec is `draft` (promote→active); check GA-600S 0.0539 PTET rate for TY2025.
- Carried — **NC_D400 / AL_FORM_40 specs are `draft`** (promote→active). SC1040 `D_SC1040_BRACKET`;
  GA-500 `R-GA500-MIL` (tts fix handed off `task_f550dfd2`). `8867_spec.json` stale notes.
- Carried S-4: f1065 page-1 + Schedule K **render** is now DONE; the 6 staged 1065 flow assertions +
  `D_M2_1`/`D_K1_704C`/`D_K1_706D` (Partner field migration) remain deferred.

## ▶ Waiting on Ken (carried)
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5). S2/S3/S4 ready (UNSIGNED).
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` (order) · `SEASON_PLAN.md` (gates) · `PRODUCT_MAP.md` (scope).
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
