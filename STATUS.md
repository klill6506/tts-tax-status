# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-06, twenty-third session. Two units: **(1) S-5 leg 2 input UI DONE (`d74b016`) → S-5
COMPLETE;** **(2) S-6 PAL/basis deepening app build COMPLETE, all 5 R-items** (R1 self-rental `c4cd928`; R2-R5
boundary diagnostics + Form 461 `07fb29f`). Build state: **idle — Ken directs;** next spine items are S-11
1041, S-3 brokerage front end (∥), S-13/S-14 1120 (⚠ NOT season-one scope).*

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
- **S-6 PAL/basis** — `pytest tests/test_schedule_e_selfrental_leg.py tests/test_pal_deepening_diagnostics.py
  tests/test_form_461.py --reuse-db -q` → **7 + 11 + 9 passed**. Existing 8582 leg
  (`test_schedule_e_8582_diagnostics_leg.py` + `_compute_leg.py`) → **31 passed / 1 skipped** after the
  RE_PRO error→info + registration-set updates. Migs 0172/0173/0174 applied to prod; diagnostics reseeded.
- **Client suite** — `cd client && npx vitest run` → **275 passed**; `npx tsc --noEmit` → **0 errors**.
- `manage.py check` clean. **Test DB `test_postgres` exists** — use `--reuse-db`. New DB tests must seed the
  forms they use and filter `FormLine` by `section__form=` NOT `form_definition=`. Must CREATE the `TaxYear`
  (not `get` it) in the fixture.
- ⚠ DB runs on the shared Supabase pooler are SLOW/flaky ("terminating connection due to administrator
  command" = transient, retry; never `drop_test_db` a slow run). Run DB tests in the background.

## ▶ RESUME HERE — **S-5 + S-6 both COMPLETE this session. Build is idle; next unit = Ken directs.**
Full detail in `.claude` memory `s6-pal-basis-deepening.md` + `s5-entity-boundary-diagnostics.md`.

**✅ S-6 PAL/basis deepening (this session) — app build COMPLETE, all 5 R-items.** Extends the existing 8582
per-activity engine + Schedule E. Ken ruled R3 diagnostic-only + R5 preparer-entered (2026-07-06).
- **R1 self-rental (§1.469-2(f)(6), `c4cd928`)** — the ONE real compute change. A Self-Rental (`RentalProperty.
  property_type=="7"`) with net income + material participation in the tenant → net income NON-passive: kept on
  Sch E lines 21/24/26 but EXCLUDED from the 8582 passive buckets (can't absorb passive losses); net loss stays
  passive. Field `selfrental_matl_part_tenant` (mig 0172); `D_SCHE_SELFRENTAL` (info); type-7 checkbox. 7 tests.
- **R2-R5 (`07fb29f`) — all diagnostic-only.** R2 PTP `D_8582_PTP` (info, auto-derived from `is_ptp`). R3 REP:
  `D_8582_RE_PRO` RED→info (message made honest — engine still limits; adjust manually) + `D_8582_REP_MATLPART` +
  `D_8582_REP_TESTS`; fields `f8582_rep_agg_election`/`_hours`/`_services_majority`. R4 at-risk `D_8582_ATRISK`
  (warning); field `f8582_at_risk_limited`. R5 §461(l): new `compute_461` (EBL, $313k/$626k 2025) + `rules_461`
  (`D_461_EBL` warning/`_NOL` info/`_ORDER` info); preparer fields `f461_agg_business_income`/`_deductions`.
  migs 0173/0174. 11 + 9 tests.

**▶ NEXT — Ken directs.** Unblocked spine items: **S-11 1041 module** (RS done, greenfield) · **S-3 brokerage
front end** (∥, skeleton done) · **S-13/S-14 1120 + state C-corp** (RS done, ⚠ 1120 C-corp NOT season-one scope
per SEASON_PLAN). Non-blocking follow-ups below.

## This session's commits (pushed to origin/main)
- `d74b016` — **S-5 leg 2 entity-boundary input UI** (EntityBoundarySection.tsx + `entity-boundary` API + 4 tests).
- `c6f431c` — docs (S-5 complete).
- `c4cd928` — **S-6 R1 self-rental** recharacterization (mig 0172; compute_schedule_e + D_SCHE_SELFRENTAL + 7 tests).
- `07fb29f` — **S-6 R2-R5** PAL boundary diagnostics + Form 461 (migs 0173/0174; rules_8582/rules_461/compute_461
  + 11 + 9 tests + existing-test severity/set updates).

## ▶ RS / compute follow-ups (all non-blocking)
- **NEW (S-6) — deferred by design.** (1) Form 461 §461(l): the disallowed EBL add-back to Schedule 1 (line 8p)
  + the §172 NOL carryover mechanic are NOT built — diagnostic-scope only (a return with an EBL over-deducts on
  screen; `D_461_EBL` warns the preparer to add it back). Consider wiring the Sch 1 add-back / hard-RED. (2) R3
  REP: compute does NOT bypass 8582 for materially-participated rentals (diagnostic-only per Ken) — the on-screen
  loss is still limited for a true REP; preparer adjusts. (3) R4 at-risk: Form 6198 compute not built (routes via
  `D_8582_ATRISK`). **RS reconciliation:** `FORM_8582`/`SCHEDULE_E`/`461` specs are all `status: draft` (promote→
  active); the RS `D_8582_RE_PRO` message assumes a compute bypass we don't do (tts message diverged, documented);
  the RS flow-assertion FA-04 severity should re-export to info.
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
