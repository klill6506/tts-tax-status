# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-05, nineteenth session: **S-4 1065 core leg 5** — issuer-side `PartnerK1Computed`
model + 1065→1040 K-1 import (the 1120-S `ShareholderK1Computed`/`k1_import.py` mirror). NEW model +
migrations 0168/0169 (applied to prod). Next = leg 6, the 1065 flow-assertion gate, then close S-4.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: pull it, read `BUILD_ORDER.md` (order) / `SEASON_PLAN.md`
  (gates) / `PRODUCT_MAP.md` (scope). BUILD_ORDER is the single source of sequence.

## Active gates (all green)
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q` → **398 passed**
  (unchanged — the 1065 changes touch no 1040 flow). Fast (~1s, pure).
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

## ▶ RESUME HERE — **S-4 1065 core (IN PROGRESS). Legs 1a–5 DONE; next = leg 6 (1065 flow-assertion gate), then close S-4.**
6 RS core specs cached to `server/specs/` (all `approved`, `a4f3370`). Full reconcile map + all-leg detail in
the `.claude` memory `1065-core-s4-kickoff.md`; per-leg build history in `STATUS_ARCHIVE.md`.

**✅ Leg 5 DONE (`6b51c3e`) — issuer-side K-1 persistence + 1065→1040 import** (the 1120-S
`ShareholderK1Computed`/`k1_import.py` mirror):
- **NEW model `PartnerK1Computed`** (migs **0168** create + **0169** RLS default-deny — **APPLIED TO PROD**
  this session via `migrate returns`). `box_shares` keyed by K-1 BOX NUMBER ("1"/"4c"/"14a"), the
  `allocate_k1` output keys — NOT the entity Sch-K "K1" keys the S-corp model uses.
- **Issuer persist** (`k1_allocator.py`): `persist_k1_for_partner_db` / `persist_all_partner_k1s_db`, hooked
  in `compute.py` right after `compute_1065_se_db` (so each partner's box 14a is final). Compute already
  existed (`allocate_all_k1s`) — this leg only added persist.
- **Import side** (`k1_import.py`): `_PARTNER_BOX_TO_K1_FIELD` (★ box **14a→se_earnings**, §199A=box 1) +
  `available/import/dismiss_partner_k1_offer`. `available_k1_offers` now MERGES both owner types; every offer
  carries `owner_kind`/`owner_id`/`source_type`; new `import/dismiss_k1_offer_by_owner` dispatch by resolving
  the id as Shareholder-or-Partner. `ScheduleK1.imported_from_partner` FK; `K1ImportDismissal.shareholder`
  nullable + `partner` FK + 2nd unique constraint (NULLs distinct → no cross-owner collision). Views re-key
  URL param `shareholder_id`→`owner_id`; frontend `K1ImportOffer` type + banner use `owner_id`.

**▶ NEXT — leg 6 (1065 flow-assertion gate) then close S-4:** no 1065 flow-assertion gate exists (1065_se is
diagnostic-gated only). Add `server/specs/flow_assertions_1065.json` + a `test_flow_assertions_1065.py` gate
(mirror the 1040/1120-S gates), then tick S-4 complete in BUILD_ORDER + form_coverage_tracker.

**⚠ DEFERRED — STILL OPEN after leg 5** (leg 5 added a NEW model, not Partner item-M/N/L-$ fields, so these
are unchanged; each needs a separate Partner-field migration): `D_M2_1` (item-L roll-forward), `D_K1_704C`
(item M §704(c)), `D_K1_706D` (§706(d) mid-year). Plus (DEFERRAL_AUDIT): f1065 page-1 + Schedule K **render
recalibration** (coords stale for 2025 — one dedicated render leg); `D_1065P1_174A` (needs R&E input line).

**What exists (reconcile, don't rebuild):** FORMULAS_1065, k1_allocator.py, compute_1065_se.py, k1_import.py.
**RED-defers (per specs):** K-2/K-3, §704(c) math, §706(d), item-L roll-forward, M-3, OBBBA flags.

*(Other unblocked SPINE items if Ken redirects: S-3 brokerage front end ∥, S-11 1041 module, S-5/S-6.)*

## This session's commit (pushed to origin/main)
- `6b51c3e` — leg 5: `PartnerK1Computed` model (migs 0168/0169, applied to prod) + issuer persist hooked in
  compute.py + `k1_import` partner path (merged offers, owner-dispatch) + serializer/views/frontend wiring +
  `test_1065_k1_import_leg.py` (2 pure + 6 DB). Shareholder pipeline + allocator re-verified green.

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
