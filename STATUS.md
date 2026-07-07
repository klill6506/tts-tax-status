# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-07, twenty-fifth session (Ken-directed MeF detour). Unit: **MeF entity e-file —
July SOR release filed + 1120-S mapper leg 1 BUILT & GREEN** (`c2edd0d`). The July BMF/IMF SOR drop was
filed to `docs/mef/` (hashes in `schema_versions.md`); the TY2025v6.3 1120x tree is extracted + live; a
synthetic 1120-S composes to MeF XML and validates against the real Return1120S.xsd. SOR want-list email
SENT (all four families' ATS-active packages). ATS scenario triage done: **Scenario 5 is the lightest
1120-S scenario** — needs 1125-A/1125-E/4562/4797/8825 doc mappers + itemized statements. S-11 1041
(legs 6-8) unchanged, still the top spine item.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: pull it, read `BUILD_ORDER.md` (order) / `SEASON_PLAN.md`
  (gates) / `PRODUCT_MAP.md` (scope). BUILD_ORDER is the single source of sequence.

## Active gates (all green)
- **Flow-assertion gate (all entities)** — `cd server && pytest tests/test_flow_assertions.py -q` → **422
  passed**. UNCHANGED this session (MeF mapper = serialization only, no compute touched).
- **MeF 1120-S mapper (NEW)** — `pytest tests/test_mef_1120s.py -q` → **10 passed** (8 pure structure +
  2 live-XSD: synthetic S corp validates GREEN vs 2025v6.3 Return1120S.xsd; manifest valid w/ type 1120S).
  Pure/fast (~2s). 1040 e-file suites regress clean (`test_efile_mef_1040.py` + `test_efile_scaffold.py`
  → 59 passed).
- **S-11 1041 compute** — `pytest tests/test_1041_compute_leg.py --reuse-db -q` → 13 pure + 1 DB smoke.
- **S-11 1041 diagnostics** — `pytest tests/test_1041_diagnostics_leg.py --reuse-db -q` → 6 passed.
- **S-11 1041 K-1 (leg 3)** — `pytest tests/test_1041_k1_leg.py --reuse-db -q` → 6 pure + 3 DB.
- **S-11 1041 render (leg 5)** — `pytest tests/test_1041_render_leg.py --reuse-db -q` → 1 passed.
- **S-5 entity boundary** — 17 passed · **S-6 PAL/basis** — 7+11+9 passed (see STATUS_ARCHIVE for detail).
- **Client suite** — `cd client && npx vitest run` → 275 passed; `npx tsc --noEmit` → 0 errors.
- `manage.py check` clean. Test DB `test_postgres` exists — `--reuse-db`; DB tests: seed forms, filter
  `FormLine` by `section__form=`, CREATE the TaxYear. ⚠ pooler runs are SLOW/flaky — background them.

## ▶ RESUME HERE — two live threads
**(A) Spine (BUILD_ORDER): S-11 1041 legs 6-8 unchanged** — leg 6 GA Form 501 (resident-only v1, RS spec
`GA501`; federal ATI → Sch 2/3 → $1,350/$2,700 exemption → 5.19%; dedicated `compute_ga501_db` +
`render_ga501` + `rules_ga501` via `state_returns` FK; RED-defer Sch 4 NR + §168(k)/§179 add-backs), then
leg 7 frontend verify, leg 8 flow gate. Leg-5 follow-on: per-beneficiary f1041sk1 K-1 PDF.
⚠ NEW for the eventual 1041 e-file: the v5.5 schemas in hand are **"Not valid for ATS"** — ATS-active is
2025v5.3 (requested from SOR).

**(B) MeF entity e-file (∥ parallel track, Ken-directed 2026-07-07): next = 1120-S ATS Scenario 5 build.**
Leg 1 DONE (`c2edd0d`): `schema_locator` corp-tree support; `read_model_1120s` (FFV seed vocabulary +
`ShareholderK1Computed`); `builder_1120s` 1:1 vs 2025v6.3 (app page-1 keys are PRE-2023 face → translated
per XSD LineNumber annotations, app 19-27 → face 20-28a; M-2 app cols a/b/c/d=AAA/OAA/STPI/AEP mapped by
meaning; K-1 codes mirror printed tables 12:A/H/I/L · 13:A-F · 15:A-F · 16:A-D · 17:A/B/C+AC; K10/K13g/
shaded-M2 refuse rather than guess). Registered (2025, "2025v6.3", "1120S").
**Scenario triage (PDFs in `docs/mef/scenarios/ty25-f1120s-ats-scenario0{5,6,7,8}-*.pdf`):**
S5 "Great Atomic Pyrotechnics" = LIGHTEST (1120S + K-1×2 + **1125-A, 1125-E, 4562×2, 4797, 8825** +
8453-CORP/8822-B + ~8 Itemized*Schedule attachment docs — all engine-computed forms, serialization-only
mappers). S6 adds SchD/8824/8941/8949; S7 adds M-3/SchN/5471-family/8916-A; S8 adds 8975 (skip).
**Next legs in order:** (1) Scenario-5 doc mappers vs v6.3 XSDs (start 1125-A — smallest), (2) itemized
statement/attachment documents, (3) enter S5 data through the REAL engine (`ats/` module + command,
engine-vs-key pins, same playbook as 1040 Scenario 8), (4) DB leg on a real client 1120-S, (5) re-stamp
v6.2 + revalidate when the SOR packages land (one-line registry add). **Data-model gaps:** signing
officer name/title (BusinessOfficerGrp — mapper takes params; add TaxReturn fields + UI later);
B4a/B4b ownership tables; K10/K13g code modeling. FilingSecurityInformation uses documented placeholders.

## This session's commits (pushed to origin/main)
- `9b77c7e` — docs(mef): July SOR release recorded — BMF 1041/1065/1120x TY2025 extracted + hashed.
- `e06d768` — docs(mef): live version check — 1040 ATS-active v5.3 (v5.4 activates 8/9); 1120x v6.3 =
  Fall-2026 ATS / Jan-2027 prod (v6.3 IS the season-one production version — right build target).
- `c2edd0d` — **feat(mef): 1120-S e-file leg 1** — TY2025v6.3 mapper (extract + builder + XSD-valid green).
- `64196ee` — docs(mef): README — 1120x tree relocated to `schemas/2025v6.3` (locator convention).
- `a0e23d0` — docs(mef): 1065/1041 version check — **ATS-active = 2025v5.3 across all families**;
  SOR want-list recorded.

## ▶ RS / compute follow-ups (all non-blocking, carried)
- **S-11 1041**: legs 6/7/8; f1041sk1 K-1 PDF; Sch A charitable; Sch G Part II; D_1041_AMT trigger (Ken
  confirm); specs `1041`+`SCHEDULE_K1_1041` draft→active; K-1 v1 class deferrals; seed payments-section
  reconciliation.
- **S-6**: §172 forward NOL; R3 REP compute bypass; R4 §465 Form 6198 compute (all deferred by design).
- **f1065 M-1 nuance**: FORMULAS_1065 M1_5/M1_8 include 4c/7b, RS spec lists only 4a/4b/6a/7a — reconcile.
- Carried GA-700 / NC_D400 / AL_FORM_40 / SC1040 / GA-500 / 8867 items (see STATUS_ARCHIVE 2026-07-06).
- Carried S-4: 6 staged 1065 flow assertions + `D_M2_1`/`D_K1_704C`/`D_K1_706D` (Partner field migration).

## ▶ Waiting on Ken / external
1. **SOR mailbox watch — want-list email SENT 2026-07-07** (e-help re-post request): 1040 **2025v5.3**
   schemas + BR, 1040 **2025v5.4** BR, 1120x **TY2025v6.2** schemas+BR, 1065 **2025v5.3** schemas + FULL
   BR base, 1041 **TY2025v5.3** schemas+BR. When they land: drop zips in `docs/mef/`, then file/hash/
   extract per `README_schemas.md` (1120-S = one-line v6.2 re-stamp + revalidate).
2. IFA-upload 1040 scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5). S2/S3/S4 UNSIGNED.
3. TY2025 4868 schema (carried).

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` (order) · `SEASON_PLAN.md` (gates) · `PRODUCT_MAP.md` (scope).
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
