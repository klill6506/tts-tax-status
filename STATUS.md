# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-05, eighteenth session: **S-4 1065 core legs 1b + 2 + 3** — leg 1b Schedule K
2025 renumber + Analysis line (+ a latent `aggregate_schedule_d` royalties-K7 fix); leg 2 Schedule L
balance-check diagnostics; leg 3 M-1/M-2 reconciliation tie-outs (K_ANALYSIS = M1_9 = M2_3).*

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
- **1065 SE** pure `pytest tests/test_1065_se_compute_leg.py` (**36**) unchanged.
- `manage.py check` clean. **Test DB `test_postgres` exists** — use `--reuse-db` (fast rebuild avoided).
  No migration this session (1065 lines are FormFieldValue-backed, re-seeded via `seed_1065` + `seed_rules`).
- ⚠ DB runs on the shared Supabase pooler are SLOW (a 4-file batch took ~38 min). Run 1065 DB tests in
  the background; never `drop_test_db` a slow run (kills the live session).

## ▶ RESUME HERE — **S-4 1065 core (IN PROGRESS). Legs 1a + 1b + 2 + 3 DONE; next = leg 4 (K-1 alloc reconcile).**
6 RS core specs cached to `server/specs/` (all `approved`, `a4f3370`). Full reconcile map in the `.claude`
memory `1065-core-s4-kickoff.md`.

**✅ Leg 1a DONE (`10f1fd2`) — page-1 2025 renumber** (ord business income now line 23; K1←line 23; NEW
line 20 §179D; seed→286 lines).

**✅ Leg 1b DONE (this session) — Schedule K 2025 renumber + Analysis line + diagnostics.** Verified vs
the 2025 f1065 Schedule K + the RS `SCH_K_1065` / `1065_PAGE1` specs:
- **Renumber (`seed_1065`, →290 lines):** foreign taxes `K16a` → **line 21 (`K21`)**; **line 16 → `K16`
  "Schedule K-3 is attached" checkbox** (international K-2/K-3 RED-defer, Decision A); investment interest
  `K13d` → **`K13b`**; added **`K13c`** (§59(e)(2)) + **`K13e`** (other deductions); new computed **`K_ANALYSIS`**.
- **Analysis line (`FORMULAS_1065`):** `K_ANALYSIS` = `(Σ K 1-11) − (Σ K 12-13e + 21)` (R-SCHK-ANALYSIS,
  i1065 verbatim). Ties M-1 L9 / M-2 L3 — **the M-1/M-2 tie-out itself is leg 3, not yet asserted**.
- **Allocator (`k1_allocator`):** carried the K13d→K13b / K16a→K21 renames through the category / pro-rata /
  K→box maps so existing allocation keeps working.
- **Diagnostics (new `rules_1065_schk.py`, registered in the runner):** `D_SCHK_HANDOFF` (error, K1 ≠ page-1
  L23), `D_SCHK_K3` (error, K-3 attached), `D_SCHK_9C` (info, unrecap §1250), `D_1065P1_4797` (warning,
  L6 recapture split), `D_1065P1_COGS` (warning, COGS w/o 1125-A).
- **★ Latent bug FIXED:** `aggregate_schedule_d` (the 1120-S SCHD_1120S aggregate, keys K7/K8a) ran UNGATED
  for all business returns and cleared `K7` to 0 every recompute — but on a **1065 K7 = royalties**, so 1065
  royalties silently vanished (and dropped out of the Analysis line). Added a `code == "1065"` early-return
  guard. 1120-S Schedule D path untouched.

**✅ Leg 2 DONE (this session) — Schedule L balance check + diagnostics.** The R-L-14/R-L-22 total
formulas already existed (FORMULAS_1065: `L15a/d` assets [face L14], `L24a/d` liab+cap [face L22],
contra-netted); this leg is validation-only. New `rules_1065_l.py` (registered): `D_L_BALANCE_BOY/EOY`
(error, L15≠L24 per column), `D_L_21_M2_TIE` (warning, partners' cap EOY `L23d` ≠ M-2 line 9 `M2_9`), `D_L_M3`
(warning, assets EOY ≥ $10M → M-3 RED-defer), `D_L_EXEMPT` (info, Schedule B Q4 box `B6`). ★ B6 exemption
SUPPRESSES the balance errors + M-2 tie. 7 DB tests. (Build-gap #2 "tts has NO balance check" — closed.)

**✅ Leg 3 DONE (this session) — M-1/M-2 reconciliation tie-outs.** RECON-ANALYSIS chain (unblocked by
leg-1b's `K_ANALYSIS`): `K_ANALYSIS` = `M1_9` = `M2_3`. New `rules_1065_m.py` (registered): `D_M1_ANALYSIS`
(error, M1_9 ≠ K_ANALYSIS), `D_M2_3` (warning, M2_3 ≠ M1_9), `D_M1_EXEMPT`/`D_M2_EXEMPT` (info, B6). B6
exemption suppresses the tie checks. The M-1/M-2 sum formulas already existed → validation-only. 3 pure + 4 DB.
DEFERRED: `D_M2_1` (M-2 line 1 = Σ K-1 item-L beginning capital — needs the item-L roll-forward; K-1 leg).

**▶ NEXT — leg 4 (K-1 alloc reconcile, RECON-K1-K):** reconcile `k1_allocator.allocate_all_k1s` per-partner
box totals back to the entity Schedule K lines (Σ partner box = K line); wire the new **K13c/K13e** into the
allocator + the **2025 K-1 box codes** (box 13 sub-codes, box 21 foreign). Then leg 5 issuer-side
**`PartnerK1Computed`** + 1065→1040 import (mirror 1120-S `ShareholderK1Computed`/`k1_import.py`) · leg 6
1065 flow-assertion gate. Also the deferred `D_M2_1` item-L roll-forward rides the K-1 leg.

**⚠ DEFERRED (flagged in DEFERRAL_AUDIT):** f1065 page-1 + Schedule K **render recalibration** (coords stale
for 2025 — the whole face is one dedicated render leg); `D_1065P1_174A` (needs an R&E input line);
`D_SCHK_704C` (needs item-M/N boolean flags); K13c/K13e per-partner K-1 box allocation (leg 4).

**What exists (reconcile, don't rebuild):** FORMULAS_1065, k1_allocator.py, compute_1065_se.py.
**RED-defers (per specs):** K-2/K-3, §704(c) math, §706(d), item-L roll-forward, M-3, OBBBA flags.

*(Other unblocked SPINE items if Ken redirects: S-3 brokerage front end ∥, S-11 1041 module, S-5/S-6.)*
- **Reusable flat-state-form render recipe (GA-500 / SC1040 / AL40 / NC D-400):** download DOR PDF → verify
  flat → strip instructions cover (`delete_page(0)`) → anchors ("00" glyphs / grid baselines) → RL baseline
  = 792 − anchor_y1 (+~2–3.5pt descent) → render synthetic-value PNG → measure delta → verify → overlay.

## This session's commits (pushed to origin/main)
- `8ad96d8` — leg 1b: seed renumber (→290 lines) + Analysis compute + `rules_1065_schk` diagnostics +
  `k1_allocator` renames + the `aggregate_schedule_d` 1065-royalties guard + tests. Prod reseeded
  (`seed_1065` + `seed_rules`, Ken-greenlit): pruned 2 stale sched_k lines (K16a/K13d, 64 FFV rows).
- `f55a0c8` — leg 2: `rules_1065_l.py` (5 Schedule L diagnostics: balance BOY/EOY, M-2 tie, M-3, exempt)
  + 7 DB tests. Validation-only (L14/L22 totals already computed). Prod `seed_rules` registered the D_L_* rules.
- `01a9a95` — leg 3: `rules_1065_m.py` (M-1/M-2 tie-outs: D_M1_ANALYSIS, D_M2_3, D_M1/M2_EXEMPT) + 3 pure
  + 4 DB tests. Validation-only (M-1/M-2 sums already computed). Prod `seed_rules` registered the D_M* rules.

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
