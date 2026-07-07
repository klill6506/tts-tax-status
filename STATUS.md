# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-06, twenty-fourth session. Unit: **S-11 1041 fiduciary module — APP BUILD STARTED,
legs 1/2/4 DONE** (Ken-directed "start 1041"). Greenfield estates & trusts entity type. Leg 1 seed + plumbing
(`539b204`), leg 2 DNI/IDD/Sch-G compute spine (`539b204`), leg 4 the 11 `D_1041_*` diagnostics (`d1117d8`).
All green, pushed. **REMAINS: leg 3 K-1 issuance · 5 f1041 render · 6 GA 501 · 7 frontend verify · 8 flow
gate.** Full detail: `.claude` memory `s11-1041-fiduciary-kickoff.md`.*

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
- **S-11 1041 compute** — `pytest tests/test_1041_compute_leg.py --reuse-db -q` → **13 pure + 1 DB smoke**
  (all 8 numeric spec cases + rate-schedule/cap-gain pins; smoke persists L23=19400/L24=5165/L21=600). Pure
  subset (`-k "not smoke"`) is ~0.14s. `1041` FormDefinition seeded to prod (8 sections/72 lines).
- **S-11 1041 diagnostics** — `pytest tests/test_1041_diagnostics_leg.py --reuse-db -q` → **6 passed** (each
  `D_1041_*` fires for its trigger + clean-return no-op). `seed_rules` reseeded to prod (11 `D_1041_*` active).
- **Client suite** — `cd client && npx vitest run` → **275 passed**; `npx tsc --noEmit` → **0 errors**.
- `manage.py check` clean. **Test DB `test_postgres` exists** — use `--reuse-db`. New DB tests must seed the
  forms they use and filter `FormLine` by `section__form=` NOT `form_definition=`. Must CREATE the `TaxYear`
  (not `get` it) in the fixture.
- ⚠ DB runs on the shared Supabase pooler are SLOW/flaky ("terminating connection due to administrator
  command" = transient, retry; never `drop_test_db` a slow run). Run DB tests in the background.

## ▶ RESUME HERE — **S-11 1041 fiduciary module: legs 1/2/4 DONE. Next = leg 3 (K-1 issuance).**
Full detail in `.claude` memory `s11-1041-fiduciary-kickoff.md`. Ken-directed "start 1041" this session.

**Greenfield entity type (estates & trusts).** Entity type = `TaxReturn.form_definition.code` (NO `entity_type`
field). `EntityType.TRUST="trust"` + the frontend already existed; the only plumbing gap was `ENTITY_FORM_MAP`
(`"trust"→"1041"`). Compute uses a **dedicated `compute_1041_db`** (early-return in `compute_return` on
`form_code == "1041"`, before the entity aggregations) — the DNI/IDD/§662-tier machinery can't live in the
Decimal-only FORMULA_REGISTRY. Raw dict keyed by **line_number**.
- **Leg 1 (`539b204`)** — `seed_1041.py`: `1041` FormDefinition, 8 sections / 72 lines (seeded to prod).
  `ENTITY_FORM_MAP` + views.
- **Leg 2 (`539b204`)** — `compute_1041.py`: R-1041-TOTINC/TOTDED/ATI/DNI/IDD/TIERS/EXEMPT/TAXINC/TAX/ESBT/
  NIIT/TOTTAX. §1(e) rate schedule + 0/15/20 cap-gain worksheet + §642(b) exemptions (600/300/100/5100/0) +
  ESBT 37%. Year-guarded TY2025. `tier1_included`/`tier2_included` emitted for the leg-3 allocator.
- **Leg 4 (`d1117d8`)** — `rules_1041.py`: 11 `D_1041_*`; registered in `seed_builtin_rules`, reseeded to prod.
  RED-defer: D_1041_AMT (Sch I; fires on TE_INT>0 proxy — ⚠ Ken confirm) + D_1041_BANKR.

**▶ NEXT — leg 3: K-1 (1041) issuance.** `SCHEDULE_K1_1041` seed + allocator + persistence; consumes
`tier1_included`/`tier2_included` + character retention. Then leg 5 f1041 render (⚠ IRS PDF NOT downloaded — add
to `forms_manifest.json`), leg 6 GA 501 (resident-only v1), leg 7 frontend verify, leg 8 flow-assertion gate.

## This session's commits (pushed to origin/main)
- `539b204` — **S-11 legs 1-2** — fiduciary entity type + DNI/IDD/Sch-G compute spine (seed_1041 + compute_1041 +
  ENTITY_FORM_MAP + 13 pure + 1 DB smoke test).
- `d1117d8` — **S-11 leg 4** — 11 `D_1041_*` diagnostics (rules_1041 + runner registration + 6 DB tests).

## ▶ RS / compute follow-ups (all non-blocking)
- **S-11 1041 (NEW 2026-07-06).** Remaining legs: 3 K-1 (`SCHEDULE_K1_1041` — spec exists) · 5 f1041 render
  (⚠ IRS PDF NOT downloaded — add to `forms_manifest.json`, run `update_irs_forms.py`) · 6 GA 501 (spec `GA501`) ·
  7 frontend editor verify (create_return path) · 8 flow-assertion gate (`flow_assertions_1041.json` — RS export).
  ⚠ **Ken to confirm the D_1041_AMT trigger** (currently fires on tax-exempt interest as a PAB-preference proxy;
  the 1041 face carries no ISO/accel-depreciation input this season). RS spec `1041` is `draft` (promote→active).
  Also carried: WO-10 Form 5227 (§664 four-tier) is its own module; §1062 Form 1062 = structure+flag only.
- **S-6 (updated 2026-07-06 follow-up).** ✅ Form 461 Sch-1 line-8p add-back DONE (`296b9f3`) — returns compute
  correctly now. ✅ RS reconciliation DONE (`d5b76b2`): 461/8582/SchE promoted draft→active, RE_PRO message
  de-staled, cached mirrors refreshed. **Still deferred by design:** (a) the §172 NOL carryover to NEXT year (the
  current-year add-back is built; the forward NOL is not); (b) R3 REP compute bypass NOT built — a true REP's
  materially-participated rental loss is still 8582-limited on screen (`D_8582_RE_PRO` info; Ken diagnostic-only);
  (c) R4 §465 at-risk Form 6198 compute not built (`D_8582_ATRISK` routes the preparer).
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
