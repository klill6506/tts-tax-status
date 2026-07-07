# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-06, twenty-third session. One unit: **S-5 leg 2 — entity-boundary preparer-assertion
input UI DONE (`d74b016`) → S-5 COMPLETE end-to-end.** React "Boundary" tab on the 1065 + 1120-S editors
driving the `EntityBoundaryAssertion` flags behind the leg-1 `D_EB_*` diagnostics; new `entity-boundary`
GET/PATCH API action + `EntityBoundaryAssertionSerializer`; 4 new endpoint DB tests. Build state: **idle — Ken
directs;** next spine items are S-11 1041, S-6 PAL/basis, S-3 brokerage front end (∥).*

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
- **S-5 entity boundary** — `pytest tests/test_entity_boundary_leg.py --reuse-db -q` → **17 passed** (13
  leg-1 diagnostics fire/no-fire across 1065+1120-S + clean-return no-false-REDs; **4 leg-2 endpoint tests**
  = GET-creates-defaults / PATCH-persists→diagnostic-fires / apportionment shield on a 1120-S / 1040 rejected).
  Migs 0170/0171 applied to prod; `seed_rules` reseeded (6 `D_EB_*` active, `D_L_M3` deactivated).
- **Client suite** — `cd client && npx vitest run` → **275 passed**; `npx tsc --noEmit` → **0 errors** (after
  the S-5 leg-2 `EntityBoundarySection.tsx` + FormEditor "Boundary" tab).
- `manage.py check` clean. **Test DB `test_postgres` exists** — use `--reuse-db`. New DB tests must seed the
  forms they use and filter `FormLine` by `section__form=` NOT `form_definition=`. Must CREATE the `TaxYear`
  (not `get` it) in the fixture.
- ⚠ DB runs on the shared Supabase pooler are SLOW/flaky ("terminating connection due to administrator
  command" = transient, retry; never `drop_test_db` a slow run). Run DB tests in the background.

## ▶ RESUME HERE — **S-5 COMPLETE (leg 1 diagnostics + leg 2 input UI). Build is idle; next unit = Ken directs.**
Full detail in `.claude` memory `s5-entity-boundary-diagnostics.md`.

**✅ S-5 leg 2 — entity-boundary preparer-assertion input UI (this session, `d74b016`)** — closes the
input→compute chain: a preparer flips a boundary flag in the UI → the matching `D_EB_*` RED fires so the
return is prepared manually. Three parts:
- **API** — `entity-boundary` GET/PATCH `@action` on `TaxReturnViewSet` (`views.py`), modeled on the existing
  `preparer` singleton action: `get_or_create` returns an all-defaults row (GET never trips a false RED),
  PATCH persists flags, non-1065/1120-S returns get a 400. `EntityBoundaryAssertionSerializer` exposes the 7
  preparer fields ONLY (auto-derived M-3 / K-2-K-3 facts excluded). Deliberately kept OFF `TaxReturnSerializer`
  (separate endpoint, like PreparerInfo) to avoid the `Meta.fields` full-serialization break.
- **UI** — `client/src/renderer/pages/EntityBoundarySection.tsx`, a new **"Boundary"** tab wired into
  `SECTION_TABS` (1120-S) + `PARTNERSHIP_TABS` (1065) in FormEditor.tsx. Self-contained (GET on mount, PATCH
  per change, optimistic local state — no full-return refresh; assertions feed only diagnostics). Apportionment
  (nexus + P.L. 86-272 shield) both entities; M-3 adj-assets (currency) / REP, K-2/K-3 DFE, §704(c), §754 are
  1065-only. Each checkbox has help text + a live "⚠ Fires a boundary diagnostic" hint (inverted for the two
  suppress-flags). Diagnostics still run from the separate Diagnostics tab.
- **Tests** — `TestEntityBoundaryEndpoint` (4 DB) in `test_entity_boundary_leg.py`.

**▶ NEXT — Ken directs.** Unblocked spine items: **S-11 1041 module** (RS done, greenfield) · **S-6** PAL/basis
(RS done) · **S-3 brokerage front end** (∥, skeleton done) · **S-13/S-14 1120 + state C-corp** (RS done, ⚠ 1120
C-corp NOT season-one scope per SEASON_PLAN). Non-blocking follow-ups unchanged (see the RS/compute section).

## This session's commits (pushed to origin/main)
- `d74b016` — **S-5 leg 2 entity-boundary input UI**: `EntityBoundarySection.tsx` + FormEditor "Boundary" tab
  (1065 + 1120-S) + `entity-boundary` API action + `EntityBoundaryAssertionSerializer` +
  `TestEntityBoundaryEndpoint` (4). No compute/migration change (leg-1 model + migs already on prod).

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
