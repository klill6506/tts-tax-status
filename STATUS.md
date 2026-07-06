# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-05, nineteenth session: **S-4 1065 core COMPLETE** — leg 5 issuer-side
`PartnerK1Computed` + 1065→1040 import (migs 0168/0169, applied to prod); **leg 6 the 1065 flow-assertion
gate** (22 active RS assertions + 6 staged pending; the intra-1065 tie chain now gated). S-4 core done;
render recalibration remains deferred. **Next = Ken directs** among the unblocked SPINE items.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: pull it, read `BUILD_ORDER.md` (order) / `SEASON_PLAN.md`
  (gates) / `PRODUCT_MAP.md` (scope). BUILD_ORDER is the single source of sequence.

## Active gates (all green)
- **Flow-assertion gate (all entities)** — `cd server && pytest tests/test_flow_assertions.py -q` → **423
  passed** (was 398; leg 6 added the **1065 gate: 22 active RS assertions + 2 guard tests**). Fast (~1.4s, pure).
  The 1065 runners live in `_run_1065_assertion` / `_RUNNERS_1065`; unbuilt assertions staged in
  `specs/flow_assertions_1065_pending.json`.
- **1065 Schedule K leg 1b** — pure `pytest tests/test_1065_schk_leg.py` (**7** — spec-driven Analysis
  pin 215000 + renumber/allocator assertions) · DB `pytest tests/test_1065_schk_pipeline_leg.py`
  (**9** — pipeline Analysis persist + 8 diagnostics fire/quiet). `test_seed_1065` count now **290**.
- **1065 Schedule L leg 2** — DB `pytest tests/test_1065_l_diagnostics_leg.py` (**7** — balanced quiet ·
  OOB BOY/EOY fire · exempt suppression · M-3 threshold · M-2 tie equal/differ).
- **1065 M-1/M-2 leg 3** — pure `pytest tests/test_1065_m_leg.py` (**3** — spec pins M1_9=298000 / M2_9=728000
  + tie identity) · DB `pytest tests/test_1065_m_diagnostics_leg.py` (**4** — tie quiet · M1/M2_3 fire · exempt).
- **1065 K-1 leg 4** — pure `pytest tests/test_1065_k1_leg.py` (**3** — K_TO_BOX 13c/13e · alloc · 60/40 sums)
  · DB `pytest tests/test_1065_k1_diagnostics_leg.py` (**7** — recon holds/breaks · 9c · special alloc ·
  item-L ties/breaks · cappct). Existing `test_k1_allocator.py` green after the K_TO_BOX refactor.
- **1065 K-1 IMPORT leg 5** — pure `pytest tests/test_1065_k1_import_leg.py -k box_shares` (**2** — box→field
  map incl. box 14a→se_earnings, §199A=box 1) · DB same file (**6** — persist 60/40 split + box 14a · non-1065
  no-op · offer→import · source-update offer · dismiss/resurface · unknown-owner false). Shareholder pipeline
  `test_k1_import_stage3.py` (**6**, needs 1120-S seeded upstream) + `test_k1_allocator.py` re-verified green.
- **1065 SE** pure `pytest tests/test_1065_se_compute_leg.py` (**36**) unchanged.
- `manage.py check` clean; frontend `tsc --noEmit` → 0 errors. **Test DB `test_postgres` exists** — use
  `--reuse-db`. **★ Leg 5 ADDED migrations 0168 (PartnerK1Computed) + 0169 (RLS) — APPLIED TO PROD this
  session** (`migrate returns` OK). New DB tests must `seed_1065`+`seed_1040` (not migration-pre-seeded) and
  filter `FormLine` by `section__form=` NOT `form_definition=`.
- ⚠ DB runs on the shared Supabase pooler are SLOW (a fresh `--create-db` ~2min; a 4-file batch took ~38 min).
  Run 1065 DB tests in the background; never `drop_test_db` a slow run (kills the live session).

## ▶ RESUME HERE — **S-4 1065 core COMPLETE (all legs 1a–6). Next = Ken directs** among the unblocked SPINE items.
Full reconcile map + all-leg detail in the `.claude` memory `1065-core-s4-kickoff.md`; per-leg build history
in `STATUS_ARCHIVE.md`.

**✅ Leg 6 DONE (`f1c095b`) — 1065 flow-assertion gate** (the first for the partnership entity; 1065_se was
diagnostic-gated only). Fetched the RS export (`/api/flow-assertions/export/?entity_type=1065`, 28 assertions)
and split into **`specs/flow_assertions_1065.json` — 22 ACTIVE** (compute built, legs 1a-5) + **`_pending.json`
— 6 STAGED** (ENTITY_BOUNDARY×3, Form 8990, §704(c)/§706(d), item-L capital roll-forward — not silently
dropped). Runners in `test_flow_assertions.py` (`_run_1065_assertion`/`_RUNNERS_1065`, all PURE): RECON-* via
real FORMULAS_1065 / k1_allocator / compute_1065_se; INV-* via allocator + SE worksheet; GATE-*/diagnostic
ties via source inspection; TI/4562-179/RC004 reuse shared runners. New `gating_check` type registered.
Full gate **423 passed**.

**✅ Leg 5 DONE (`6b51c3e`) — issuer-side K-1 persistence + 1065→1040 import** (1120-S mirror): NEW
`PartnerK1Computed` model (migs **0168**/**0169** — APPLIED TO PROD); issuer persist
(`persist_all_partner_k1s_db` hooked post-SE in compute.py); import side in `k1_import.py` (box 14a→se_earnings;
merged `available_k1_offers` w/ `owner_kind`/`owner_id` dispatch; `ScheduleK1.imported_from_partner`;
`K1ImportDismissal` shareholder-nullable + partner FK). 2 pure + 6 DB green. (Legs 1a–4 → STATUS_ARCHIVE.)

**⚠ S-4 remaining / deferred** (S-4 CORE is done — compute + diagnostics + K-1 issue/import + flow gate — but
these ride separate future legs): **f1065 page-1 + Schedule K render recalibration** (coords stale for 2025 —
one dedicated render leg); the 6 STAGED flow assertions (activate as each compute lands); `D_M2_1` /
`D_K1_704C` / `D_K1_706D` (need a Partner item-M/N/L-$ field migration); `D_1065P1_174A` (R&E input line).
Because render is deferred, the **1065 form does not fully tick** in form_coverage_tracker yet.

**▶ NEXT — Ken directs.** Unblocked SPINE items (all `[APP]`): **S-10 GA-700** (was gated behind S-4 1065
core — now unblocked), **S-11 1041 module**, **S-5 boundary diagnostics** + **S-6 PAL/basis** (RS authoring
done), **S-3 brokerage front end** (∥), **S-13/S-14 1120 + state C-corp**. Also S-4 follow-on: the f1065
render recalibration leg.

**What exists (reconcile, don't rebuild):** FORMULAS_1065, k1_allocator.py, compute_1065_se.py, k1_import.py,
the 1065 flow-assertion gate. **RED-defers (per specs):** K-2/K-3, §704(c) math, §706(d), item-L roll-forward,
M-3, OBBBA flags.

## This session's commits (pushed to origin/main)
- `6b51c3e` — leg 5: `PartnerK1Computed` model (migs 0168/0169, applied to prod) + issuer persist + `k1_import`
  partner path (merged offers, owner-dispatch) + serializer/views/frontend + `test_1065_k1_import_leg.py`
  (2 pure + 6 DB). `0e2a4e5` — leg 5 docs.
- `f1c095b` — leg 6: 1065 flow-assertion gate — `flow_assertions_1065.json` (22 active) + `_pending.json`
  (6 staged) + runners in `test_flow_assertions.py`. Full gate 423 passed; `check` clean. No compute/migration.

## ▶ RS follow-ups (rides a dedicated RS session)
- Carried — **NC_D400 / AL_FORM_40 specs are `draft`** (promote→active). SC1040 `D_SC1040_BRACKET` threshold;
  GA-500 `R-GA500-MIL` (RS spec correct; tts fix handed off `task_f550dfd2`). `8867_spec.json` stale notes;
  RS Schedule D `D_8949_006`; 8995 sibling loader D_8995_001 note.

## ▶ Waiting on Ken (carried)
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5). S2/S3/S4 ready (UNSIGNED).
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` (order) · `SEASON_PLAN.md` (gates) · `PRODUCT_MAP.md` (scope).
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
