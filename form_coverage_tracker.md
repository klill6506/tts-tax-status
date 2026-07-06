# Form Coverage Tracker — tts-tax-app

> **2026-07-05 (S-4 1065 core, leg 6) — 1065 flow-assertion gate → ✅ DONE. S-4 CORE COMPLETE.**
> The FIRST flow-assertion gate for the partnership entity (previously `1065_se` was diagnostic-gated only).
> Fetched the Rule Studio export (`/api/flow-assertions/export/?entity_type=1065`, 28 assertions) and split it
> (mirroring the 1040 `_pending` convention — nothing silently dropped): **`server/specs/flow_assertions_1065.json`
> — 22 ACTIVE** (every assertion whose 1065 compute is built in legs 1a-5) + **`flow_assertions_1065_pending.json`
> — 6 STAGED** (`FA-ENT-BND-01/02/03` = ENTITY_BOUNDARY/S-5 not built; `GATE-8990-163J` = Form 8990 not built;
> `GATE-704C-706D-DEFER` = Partner item-M/N §704(c)/§706(d) fields not built; `RECON-M2-CAPITAL` = item-L
> tax-basis capital roll-forward RED-deferred). All runners are in `tests/test_flow_assertions.py`
> (`_run_1065_assertion` / `_RUNNERS_1065`, PURE / no-DB): **RECON-P1-K1/ANALYSIS/L-BALANCE** re-derived through
> the real `FORMULAS_1065`; **RECON-K1-K/9C + INV-GP-DIRECT/CHAR** through the real `k1_allocator` (MagicMock
> partners, `PartnerAllocation` patched); **RECON-14A + INV-SE-BASE** through `compute_1065_se`
> (`entity_14a_bottom_up`, `worksheet_base`); **RECON-M1-ANALYSIS/L21-M2 + GATE-K2K3/SMALL-PTNR** via diagnostic
> source-inspection (the tie/suppression is diagnostic-enforced); **TI001-4** (MACRS), **FA-4562-179-02/03**,
> **RC004** reuse the existing shared runners. A new `gating_check` assertion type was registered; `RECON-*` /
> `INV-*` are routed into the reconciliation / table_invariant runners by id prefix. Gate result: **24 1065
> tests green** (22 assertions + loaded/pending-staged guards); **full flow-assertion suite 423 passed**;
> `manage.py check` clean; no compute/model/migration change. ⚠ **The 1065 FORM still does NOT fully tick** —
> `f1065` page-1 + Schedule K **render recalibration** (2025 coords) is a deferred follow-on leg; S-4 CORE
> (compute + diagnostics + K-1 issue/import + flow gate) is what completed here. **▶ Next: Ken directs** —
> S-10 GA-700 is now UNBLOCKED (depended on the federal 1065 flow), plus S-11 1041 / S-5 / S-6 / S-13/S-14.

> **2026-07-05 (S-4 1065 core, leg 5) — issuer-side K-1 persistence + 1065 → 1040 import → ✅ DONE.**
> The 1120-S `ShareholderK1Computed` / `k1_import.py` mirror for partnerships. **NEW model
> `PartnerK1Computed`** (migrations **0168** create + **0169** RLS default-deny — **applied to the shared prod
> DB** via `migrate returns`; leg 5 is the first S-4 leg with a real schema migration — legs 1–4 were
> FormFieldValue-backed). `box_shares` is keyed by K-1 **box number** ("1"/"4c"/"14a"…), the OUTPUT keys of
> `k1_allocator.allocate_k1`, NOT the entity Schedule K "K1" keys the S-corp twin uses. **Issuer COMPUTE
> already existed** (`allocate_all_k1s`) — this leg added **PERSIST** (`persist_k1_for_partner_db` /
> `persist_all_partner_k1s_db` in `k1_allocator.py`, hooked in `compute.py` immediately after
> `compute_1065_se_db` so each partner's box 14a is final and the persisted row always mirrors the entity's
> current Schedule K). **Import side** (`k1_import.py`): `_PARTNER_BOX_TO_K1_FIELD` (★ the 1065-specific map —
> box **14a → `se_earnings`**, no 1120-S analog since S-corp shareholders have no SE; §199A QBI = box 1,
> guaranteed payments excluded) + `available_partner_k1_offers` / `import_partner_k1_offer` /
> `dismiss_partner_k1_offer`. `available_k1_offers` now MERGES both owner types; every offer carries
> `owner_kind` / `owner_id` / `source_type`; new `import_k1_offer_by_owner` / `dismiss_k1_offer_by_owner`
> dispatch by resolving the id as a Shareholder or Partner. `ScheduleK1.imported_from_partner` FK added;
> `K1ImportDismissal.shareholder` made nullable + `partner` FK + a 2nd unique constraint (Postgres NULLs
> distinct → no cross-owner collision). Views re-key the URL param `shareholder_id`→`owner_id`; serializer
> exposes `imported_from_partner`; frontend `K1ImportOffer` type + import banner use `owner_id`. Tests:
> `test_1065_k1_import_leg.py` — 2 pure (box→field map incl. 14a→se_earnings) + 6 DB (persist 60/40 split +
> box 14a · non-1065 no-op · offer→import · source-update offer · dismiss/resurface · unknown-owner false).
> Shareholder pipeline (`test_k1_import_stage3.py`, 6) + `test_k1_allocator.py` re-verified green; frontend
> `tsc --noEmit` 0 errors. **DEFERRED — STILL OPEN** (leg 5 added a new model, not Partner item-M/N/L-dollar
> fields; each needs a separate Partner-field migration): `D_M2_1`, `D_K1_704C`, `D_K1_706D`. **▶ NEXT leg 6:
> the 1065 flow-assertion gate** (none exists — 1065_se is diagnostic-gated only), then S-4 closes.

> **2026-07-05 (S-4 1065 core, leg 4) — Schedule K-1 allocation reconcile (RECON-K1-K) → ✅ DONE.**
> Built spec-first from RS `SCHEDULE_K1_1065`. **Allocator wiring (`k1_allocator.py`):** promoted the local
> `k_to_box` to a module-level **`K_TO_BOX`** (single source of truth shared with the reconcile diagnostic)
> and wired the leg-1b **`K13c`→box 13c / `K13e`→box 13e** deductions into it + `K_LINES_PRO_RATA`. **New
> `rules_1065_k1.py` (registered):** **`D_K1_RECON`** (error — Σ per-partner K-1 box ≠ entity Schedule K
> line; profit/loss %s must total 100% per category; whole-dollar rounding tolerance = #partners),
> **`D_K1_9C`** (info — entity K9c allocates to box 9c; confirms the pass-through — closes the open box-9c
> verification), **`D_K1_SPECIAL_ALLOC`** (warning — a PartnerAllocation override is applied but §704(b) SEE
> is not tested), **`D_K1_ITEML`** (warning — item L tax-basis capital doesn't roll forward: ending ≠
> beginning + contributed + current-year − withdrawals), **`D_K1_CAPPCT`** (info — an item J capital % is
> reported; nothing allocates by capital_pct — expected). Tests: 3 pure (K_TO_BOX has 13c/13e; 13c/13e
> allocate; 60/40 sums to entity) + 7 DB (recon holds/breaks · 9c · special alloc · item-L ties/breaks ·
> cappct). Flow gate 398, existing allocator suite green. **DEFERRED** (need new Partner boolean fields +
> migration; RED-defer structure, see DEFERRAL_AUDIT): `D_K1_704C` (item M §704(c)) + `D_K1_706D` (§706(d)
> mid-year interest change).

> **2026-07-05 (S-4 1065 core, leg 3) — Schedule M-1 / M-2 reconciliation tie-outs → ✅ DONE
> (diagnostics; the M-1/M-2 sum compute already existed).** Built spec-first from RS `1065_M1` + `1065_M2`.
> This wires the RECON-ANALYSIS chain that leg-1b's `K_ANALYSIS` unblocked (R-M1-9's flagged "tts computes
> NO Analysis line" gap is now closed): **Analysis line (`K_ANALYSIS`) = M-1 line 9 (`M1_9`) = M-2 line 3
> (`M2_3`)**. New `rules_1065_m.py` (registered): **`D_M1_ANALYSIS`** (error — M1_9 ≠ K_ANALYSIS; the
> book→return reconciliation must land on the Schedule K return income), **`D_M2_3`** (warning — M2_3
> data-entry ≠ M1_9; both-zero quiet), **`D_M1_EXEMPT`** / **`D_M2_EXEMPT`** (info — Schedule B Q4 box `B6`).
> ★ B6 exemption **suppresses** the two tie checks (the EXEMPT infos own it). The R-M1-5/8/9 + R-M2-5/8/9 sum
> formulas were already in FORMULAS_1065 (M1_5/M1_8/M1_9, M2_5/M2_8/M2_9) — validation-only, no compute change.
> Tests: 3 pure (spec pins M1-1 M1_9=298000, M2-1 M2_9=728000, tie identity) + 4 DB (tie holds quiet · M1
> mismatch fires · M2_3 off fires · exempt suppression). `check` clean. **DEFERRED** (K-1 leg, see
> DEFERRAL_AUDIT): `D_M2_1` (M-2 line 1 = Σ K-1 item L beginning capital — needs the item-L roll-forward;
> Partner model has only capital %, not tax-basis capital $).

> **2026-07-05 (S-4 1065 core, leg 2) — Schedule L balance check + M-2 tie / M-3 / exempt diagnostics
> → ✅ DONE (diagnostics; the L14/L22 total compute already existed).** Built spec-first from RS `1065_L`.
> Build-gap #2 closed ("tts has NO balance check"). New `rules_1065_l.py` (registered): **`D_L_BALANCE_BOY`**
> / **`D_L_BALANCE_EOY`** (error — face line 14 total assets [app `L15a`/`L15d`] ≠ face line 22 total
> liab+capital [app `L24a`/`L24d`], per column), **`D_L_21_M2_TIE`** (warning — partners' capital EOY [`L23d`]
> ≠ Schedule M-2 line 9 [`M2_9`]; both-zero stays quiet), **`D_L_M3`** (warning — total assets EOY ≥ $10M →
> Schedule M-3 required, RED-defer Decision F), **`D_L_EXEMPT`** (info — Schedule B Q4 small-partnership box
> [app `B6`] set → Schedule L not required). ★ The small-partnership exemption (B6) **suppresses** the balance
> errors + the M-2 tie (D_L_EXEMPT owns it). The R-L-14/R-L-22 total formulas were already in FORMULAS_1065
> (`L15a/d`, `L24a/d`, contra-netted) — this leg is validation-only, no compute change. Tests: 7 DB
> (balanced quiet · OOB BOY/EOY fire · exempt suppression · M-3 threshold · M-2 tie equal/differ). `check`
> clean. RED-defers per spec: R-L-21-TIE math, Schedule M-3 build.

> **2026-07-05 (S-4 1065 core, leg 1b) — Schedule K 2025 renumber + Analysis-of-Net-Income line
> + handoff/page-1 diagnostics → ✅ DONE (compute + input + diagnostics; render still deferred).**
> Built spec-first from RS `SCH_K_1065` + `1065_PAGE1` (cached specs). **Renumber (seed_1065):** the
> app's Schedule K was on PRE-2025 numbering; reconciled to the 2025 face — foreign taxes `K16a` → **line
> 21 (`K21`)**; **line 16 → `K16` "Schedule K-3 is attached" checkbox** (international K-2/K-3 RED-defer,
> Decision A); investment interest `K13d` → **`K13b`**; added **`K13c`** (§59(e)(2)) + **`K13e`** (other
> deductions); new computed **`K_ANALYSIS`** line. **Compute (FORMULAS_1065):** `K_ANALYSIS` = (Σ K
> 1-11) − (Σ K 12-13e + 21) per R-SCHK-ANALYSIS (i1065 verbatim; ties M-1 L9 / M-2 L3 — tie-out enforced
> in the later M-1/M-2 leg). **Allocator (k1_allocator):** carried the K13d→K13b / K16a→K21 renames through
> `K_LINE_CATEGORY` / `K_LINES_PRO_RATA` / the K→box map so existing allocation keeps working. **Diagnostics
> (new `rules_1065_schk.py`, registered):** `D_SCHK_HANDOFF` (error — K1 ≠ page-1 L23), `D_SCHK_K3` (error —
> K-3 attached), `D_SCHK_9C` (info — unrecap §1250), `D_1065P1_4797` (warning — L6 recapture), `D_1065P1_COGS`
> (warning — COGS w/o 1125-A). **Tests:** 7 pure (spec-driven Analysis pin 215000 + renumber assertions) +
> 9 DB (pipeline Analysis persist + 8 diagnostics fire/quiet). Flow gate 398, SE pure 36, `check` clean.
> **DEFERRED (see DEFERRAL_AUDIT):** `D_1065P1_174A` (needs an R&E input line), `D_SCHK_704C` (needs
> item-M/N flags), the f1065 page-1+SchK **render recalibration** (coords stale for 2025), and K13c/K13e
> per-partner K-1 box allocation (the K-1 reconcile leg). ⚠ Prod reseed of `seed_1065` DELETES the old
> `K16a`/`K13d` FFV rows — flag for Ken before reseeding prod (leg-1a precedent).

> **2026-07-05 (seventeenth session) — NC D-400 (North Carolina individual) → ✅ FULLY COMPLETE
> across ALL 4 LEGS (S-9); no migration, FormFieldValue-backed (GA-500/SC1040/AL40 pattern),
> form_code "NC_D400".** Built spec-first from the RS `NC_D400` spec (status `draft`; TY2025).
> **Compute** ✅ `compute_nc_d400` (dispatched in `compute_return` for NC_D400) — federal-AGI start (L6),
> the ★ 85% bonus (L3) + 85% §179-excess (L4) add-back (NC decoupled, $25k/$200k limits, conformity FROZEN
> Jan-1-2023 → OBBBA N/A), AGI-banded child deduction (breakpoint→higher band), restricted NC-itemized
> subset (mort+prop capped $20k / property $10k-$5k MFS / medical less 7.5% AGI), std ded (MFS $0 if spouse
> itemizes), Schedule S Part B subtractions (Bailey/military/SS/US-obligation), Schedule PN proration, and
> the ★ FLAT 4.25% tax (§105-153.7, year-guarded to 2025 — steps to 3.99% after). All 8 RS scenario pins
> verified. **Input** ✅ `seed_nc_d400` (4 sections / 47 lines: face + Schedule S + Schedule A + Schedule
> PN) + NC→NC_D400 wiring (`INDIVIDUAL_STATE_FORM_MAP`, `NC_D400_FEDERAL_PULL` = federal AGI 1040 L11 → D-400
> L6, `_populate_nc_d400_from_federal`, create + refresh dispatch — ⚠ also FIXED a latent AL40
> `refresh_from_federal` bug that fell through to the GA pull) + frontend picker/section-tabs
> (`NC_D400_SECTION_TABS`, isNcD400, SUPPORTED_STATES, D_NCD400_→State tab). **Render** ✅ FACE (flat NCDOR
> "handwritten" template `fnc_d400` — the leading "Do Not Include This Page" instructions cover STRIPPED via
> `delete_page(0)` so the stored 2-page template = the form face; pre-printed "00" anchors → two value
> columns pg0 right-483 / pg1 right-555 + line-12a left box + 20a/21a payment sub-boxes + line-13 Sch PN %
> box, render→PNG→measure-delta + visually verified) + page-0 identity header (name/SSN/address + filing
> circle X at x≈78). **Diagnostics** ✅ 8 `D_NCD400_*` rules (179LIMIT+MFS_STDDED warning; the rest info;
> AMENDED dormant-but-registered for spec parity). Tests: 14 pure compute + 4 state-return DB + 6 render + 8
> diagnostics, all green; flow gate 398. Commits `69cf82b`/`b31d5c7`/`c704f21`/`8358e74`. Template
> `nc_d400.pdf` (NCDOR handwritten, cover-stripped) sha-pinned in the manifest (`fnc_d400`). RS follow-up:
> NC_D400 spec is `draft` → promote to `active`. **v1 boundaries** (RS D_NCD400_*): amended return (L22/L24,
> Sch AM), Form D-422 est-tax interest, NC NOL (Sch S L39), Schedule PN-1 — direct-entry / not computed.

> **2026-07-05 (sixteenth session, part 2) — AL FORM 40 (Alabama individual) → ✅ FULLY COMPLETE
> across ALL LEGS (S-8); no migration, FormFieldValue-backed (GA-500/SC1040 pattern), form_code
> "AL40".** Built spec-first from the RS `AL_FORM_40` spec (status `draft`; TY2025). **Compute** ✅
> `compute_al40` (dispatched in `compute_return` for AL40) — income-from-scratch, the standard-
> deduction sliding scale as a formula, personal + tiered dependent exemptions, 2/4/5% tax
> (§40-18-5), and ★ THE ALABAMA QUIRK: the LIABILITY-based federal-income-tax deduction (L12 =
> max(0, (1040 L22 + 8960 NIIT) − refundable credits), part-year apportioned by AL AGI ÷ fed AGI).
> All 5 RS scenario pins verified. **Input** ✅ `seed_al40` (2 sections / 36 lines) + AL→AL40 wiring
> (`INDIVIDUAL_STATE_FORM_MAP`, create/refresh-from-federal, `_populate_al40_from_federal` pulls the
> FIT-worksheet federal facts SCOPED to the 1040 form) + frontend picker/section-tabs (`AL40_SECTION_TABS`,
> isAl40, SUPPORTED_STATES, D_AL40_→State tab). **Render** ✅ FACE (flat 2-page ALDOR template `fal40`,
> single value column, coordinate overlay, lines 5b–35 + two fidelity mirrors 29/32, visually verified) +
> page-1 identity header (name/SSN/address/filing-status X). **Diagnostics** ✅ 6 `D_AL40_*` info rules
> (FIT/STDDED/EXCLUSIONS/ATP/40NR/NOL; registered + seeded). Tests: 15 pure compute + 3 state-return DB +
> 5 render + 4 diagnostics DB, all green; flow gate 398. Commits `28ceeab`/`3edce72`/`938846c`/`941cd46`/
> `c81bcf2`. Template `al_form_40.pdf` (25f40blk.pdf) downloaded + sha-pinned in the manifest (`fal40`). RS
> follow-up: AL_FORM_40 spec is `draft`. **v1 boundaries** (RS D_AL40_*): Form 40NR nonresident, Schedule
> ATP (L19), Form NOL-85A, Schedule OC credits — direct-entry / not computed.

> **2026-07-05 (sixteenth session) — SC1040 Schedule NR RENDER LEG → ✅ SC1040 FULLY COMPLETE
> (S-7 all legs green); no migration, render-only.** Rendered the SC Schedule NR (part-year/
> nonresident) summary lines 31-48 as a coordinate overlay on the flat 2-page SCDOR template,
> appended behind the SC1040 face when the return is part-year/nonresident (SC1040 "PYNR" set);
> resident returns attach no Schedule NR. **New** `coordinates/fsc_schedule_nr.py`: two-money-column
> map (Col A federal x-anchor 473.4, Col B SC 571.5, value right-edge = anchor − 12.5 = the fsc1040
> face gap) + the two inline proration-% blanks (lines 45/47, right of the pre-printed "%"). Anchors
> auto-extracted from the flat PDF "00"/"%%" glyphs; baselines pinned RL_y = 792 − fitz_y1 + **3.5pt
> font-descent nudge** (measured value-vs-"00" delta −0.04pt after nudge). **renderer.py**:
> `render_sc_schedule_nr()` (PYNR-gated → None for residents; NR-45 fraction ×100 as "NN.NN") +
> register `fsc_schedule_nr` in COORDINATE_REGISTRY + append after the face in `render_sc1040`
> (GA-500 Schedule-1 append precedent). **v1 boundary** (matches `compute_sc1040._schedule_nr`): only
> summary lines 31-48; income-detail lines 1-30 attach blank (preparer enters the AGI totals on line
> 31). RS `SC_SCHEDULE_NR` spec (status `active`) cached to `server/specs/`. Tests **+6** (5 pure
> map/gate + 1 DB part-year 5-page attach: L48=47,800 @ 60.00% proration): render leg **10/10**;
> SC1040 compute 16 + state-return 4 + diagnostics 4 + **flow 398** = **422 passed**. Commit `d9fa2b0`.

> **2026-07-05 (fifteenth session) — SC1040 (South Carolina individual) — RESIDENT RETURN COMPLETE
> across FOUR legs; Schedule NR render is the only remaining sub-leg → S-7 NOT yet fully ticked.**
> New state form on the GA-500 pattern (FormFieldValue-backed, `seed_sc1040`, no migration).
> **Compute** ✅ `compute_sc1040` (dispatched in `compute_return` for form_code SC1040) — SC1040TT tax
> verified 138/138 vs the published SCDOR table (midpoint convention; the RS spec's "$2,533" pin was
> wrong — real = $2,360); §168(k)+§179 add-backs (SC pre-OBBBA $1.25M), retirement/military/age-65
> stack, 44% cap gain, embedded Schedule NR compute. **Input** ✅ `seed_sc1040` (5 sections / 81 lines)
> + `create-state-return`/`refresh-from-federal` SC wiring + frontend picker/section-tabs + the
> RED/YELLOW/GREEN color system (generic editor). **Render** ✅ FACE (3 pages, coordinate overlay,
> visually verified) + page-1 identity header; ⬜ **Schedule NR render remains**. **Diagnostics** ✅ 6
> `D_SC1040_*` info rules (registered + seeded). Tests: 16 pure compute + 4 state-return DB + 5 render
> + 4 diagnostics DB, all green; flow gate 398. Commits `307a810`/`a8cb291`/`81e0809`/`4cb497a`/
> `af2c6a7`/`13443d5`/`118613d`. Also fixed a latent `renewable_facilities` serializer bug (broke ALL
> full-return serialization since the 7-04 Form 8835 unit). RS follow-up: SC1040 spec is `draft` + carries
> the wrong $50k pin.

> **2026-07-05 (thirteenth session) — e-file/diagnostics HARDENING, no coverage change.** Two latent
> bugs on already-complete forms, no leg status moved: (1) **Schedule 3 e-file leg** — `build_schedule3`
> now emits the lines 6z + 13z repeating groups (`OtherNonrefundableCreditsGrp` / `OtherRefundableCreditsGrp`
> + totals) that were silently dropped (`5d055e7`); (2) **Form 8867 diagnostics leg** — the AOTC covered-
> credit gate + attestation cascade unified onto one `compute_8863.aotc_claimed` signal (`8783487`). Both
> forms remain ✅ complete; these harden existing legs. No migration.

> **2026-07-04 (ninth session) -- FORM 8835 (Renewable Electricity Production Credit §45) →
> ✅ FORM 8835 COMPLETE (ALL FOUR LEGS TICK) -- migs 0165 (RenewableFacility + 2 Taxpayer
> passthrough facts) + 0166 (RLS), both applied to the SHARED DB.** Built from the cached RS
> `8835_spec.json` + Ken's 2026-07-04 J1-J4 scope rulings. **Model** (`RenewableFacility`,
> one row per §45 facility = one Form 8835 face; J1 multi-facility): field names = the spec
> fact keys; Taxpayer `f8835_passthrough_credit`/`f8835_passthrough_pis_date` (J4).
> **Compute** (`compute_8835.py`): year-keyed 2025 RATE table (Fed. Reg. 2025-09366 — $0.006
> tier-1 / $0.003 tier-2 / hydro-marine post-2022 $0.006 / pre-2022 3.0¢-1.5¢; 2026 unpinned
> → D_8835_RATE_YEAR), the OBBBA before-2025 begin-construction gate (blank date proven by a
> pre-cutoff PIS — the 8936 convention), the Part II chain (2-13 per facility: kWh×rate, bond
> 15% cap, ×5 increased, +10%/+10% bonuses, 90% EPE), `form_8835_state` (THE shared gatherer),
> `form_8835_credit() -> (amount, "1f"|"4e")` (the pinned 3800 wiring, LIVE), `compute_8835_db`
> (FORM_8835 face rows, disengage). Runs BEFORE `compute_3800_db`; lands NOTHING on Sch 3 (all
> via the 3800 §38 limitation). **Routing** (R-8835-ROUTE, J3 binary-per-year): whole year in
> the 4-yr PIS window → 4e; window ended → 1f; PIS-in-year → 4e; window ends mid-year →
> straddle RED-defer. **Diagnostics** (`rules_8835.py`, 9 rules): spec 001-004 + RATE_YEAR +
> the J2/J3/J4 RED-defers **005 mixed / 006 straddle / 007 passthrough-unrouted / 008
> missing-PIS** (all ≤20 chars, error, bridge-gated on `form_8835_state`; escape hatch = the
> 3800 1zz/4z facts). **Render**: f8835.pdf downloaded (seq 835, Cat 14954R, sha pinned) +
> `f8835_2025` LABEL-VERIFIED bijective field map (97 widgets; dead: line-7 reserved pair, 10
> table slivers, line-3 sub-entries) + `render_8835` (ONE FACE PER FACILITY via
> `facility_lines`; lines 14/15 first-face) + 3-page PNG visual pass + packet emit before 3800.
> **UI**: `RenewableFacilitiesSection` tab (Credits group, before 3800) + the two Taxpayer
> passthrough inputs + D_8835_ → form_8835 routing; tsc clean. **GATES**: pure compute
> **19/19** (first run) · DB pipeline **8/8** (S4 4e / T4 1f / all-carries W4 / mixed / straddle
> / passthrough / OBBBA / disengage) · diagnostics **10/10** · render **6/6** · **FA-1040-8835-
> 01..06** in the gate file (transcribed verbatim from the RS canonical set) → **flow gate 397**.
> RS Supabase reseeded (9 diags / 6 FAs); cached `8835_spec.json` refreshed from the live
> deployed export (5→9 diags). Shared DB migrated (0165/0166) + seeded (seed_8835 14 lines,
> seed_rules); test DB migrated through 0166. Tag `1040-form-8835-complete`. **NEXT: the S4
> scenario + mapper leg** (⚠ reconcile the Transfer Election Statement vs 4a=No — a transferred
> credit never lands on Sch 3; the blessed all-carries shape: 8936 personal absorbs the tax →
> 3800 line 38 = 0, Sch 3 6a blank, the whole $13,200 carries).

> **2026-07-04 (eighth session, part 2) -- FORM 3800 (General Business Credit §38) →
> ✅ FORM 3800 COMPLETE (ALL FOUR LEGS TICK) -- the FULL spec-first round-trip in one
> session (mig 0164).** Ken: "should you go ahead and build the 3800?" → the deployed RS
> spec was a thin 1120-S-era draft → **RS 1040-side AMENDMENT authored** (`load_1040_form_
> 3800.py`, AMEND-BY-LOOKUP — entity_types + R001-R003 kept, R004 refreshed, the stale
> 1a/1b/1c sketch deleted; RS commit `5407bb2`), Ken's **scope walk J1-J4** (built-feeder
> Part III rows 1f/1s/1y/1aa + 1zz/4z catch-alls · passive = per-inflow TRI-STATE assertion
> · carryforward = two in-buckets + computed out · §6417/§6418 out) + **review walk W1-W4**
> (incl. the blessed S4 all-carries outcome) → seeded → deployed export verified → canonical
> `3800_spec.json` cached. **tts unit**: Taxpayer `f3800_*` facts (mig 0164) ·
> `compute_3800.py` (Part III inflow gathering bridge-gated to `form_8911_business_credit` +
> the new `form_8936_business_parts`; the 8835 wiring point stubbed for the next unit;
> §38(c)(1) Section A verbatim — line 10b = 1040 L19 + Sch 3 2-4/5a/5b + 6x excl 6a/6b/6k;
> **§38(c)(4)(A) Section C with TMT ZEROED for specified credits**; line 38 = 28+37 → **Sch 3
> line 6a**, computed LAST among the nonrefundable credits) · 6 diagnostics (D_3800_001-006)
> · seed_3800 (42 lines) · **subset field map** over the 9-page face (semantic grid subforms
> Pg3Table.Line1f… verify the rows; all 9 pages print per the face mandate) + render_3800 +
> **carryforward STATEMENT page** + PNG visual pass · Business Credit (3800) UI tab (passive
> selects + catch-alls + CF-ins + escape hatches) · FA-1040-3800-01..05 (authored in the RS
> loader AND the tts gate — no drift). **RETIREMENTS**: D_8911_004 → is_active=False stub
> (8911 line 3 lands on row 1s); D_8936_003 → softened to the spec's info (8936 lines 8/21
> land on 1y/1aa); FA-8911-04/FA-8936-02 runners repinned to the new reality. **FA-DRIFT
> FIX**: the five FA-1040-8936-* tts entries replaced with the RS-canonical definitions (the
> RS loader had authored those ids first). **GATES**: pure compute **9/9** · DB pipeline
> **6/6** (incl. the end-to-end W4 ordering pin: 8936 personal absorbs the tax → the whole
> 13,200 specified credit carries, 6a blank) · render leg **5/5** · **combined flow gate +
> all 3800/8936/8911 legs = 451 passed** (reuse-db 7:04). Shared DB migrated (0164) + seeded
> (seed_3800, seed_rules — retirement/softening verified live); test DB migrated. NEXT: the
> **Form 8835 unit** (spec cached; implements `form_8835_credit() -> (amount, "1f"|"4e")`),
> then the S4 scenario.**

> **2026-07-04 (eighth session) -- FORM 8936 (Clean Vehicle Credits §30D/§25E/§45W) →
> ✅ FORM 8936 COMPLETE (ALL FOUR LEGS TICK) -- migs 0162 (CleanVehicle + 4 Taxpayer facts)
> + 0163 (RLS).** Spec-first from the cached RS `8936_spec.json` + `8936_scha_spec.json`
> (authored 2026-07-04; the S4 arc). **Model**: `CleanVehicle` one-Schedule-A-per-vehicle
> (type new/used/commercial, OBBBA `acquired_date`, transfer election, per-part inputs) +
> Taxpayer `clean_vehicle_magi_prior`/`_prior_filing_status`/`_new_k1_credit`/
> `_commercial_k1_credit`. **Compute** (`compute_8936.py`, TWO-PHASE): phase 1 REPAYMENT —
> a dealer-transferred credit DENIED (MAGI over the cap BOTH years, per-year status caps;
> or 30-day resale) repays on **Schedule 2 lines 1b/1c → 1040 line 17** BEFORE every
> credit-limit worksheet (the 8962 excess-APTC reflow precedent); phase 2 CREDITS after
> compute_5695 (lines 11/16 read Sch 3 5b) and before EIC/8812/8911 (whose CLWs read
> 6f/6m) — Part IV computes BEFORE Part III (line 11 INCLUDES the line-18 used credit,
> the i8936 WALK-ITEM-C asymmetry) → **Sch 3 6f (new personal, line 13) / 6m (used, line
> 18)**; unused personal portion LOST. **OBBBA gate face-verified** (i8936 What's New
> verbatim): no credit for a vehicle ACQUIRED after 9/30/2025 (absolute date, not
> year-keyed — the acquired-before/PIS-later tail claims in its PIS year); blank acquired
> date proven by PIS ≤ cutoff, else excluded + D_8936_001. **FACE-VERIFIED transfer stop**
> (f8936sa 8c/8d + 13b/13c verbatim): a TRANSFERRED credit was received at the point of
> sale — a qualifying transferred vehicle contributes NOTHING to the face (never
> double-claims); RS spec silent → amendment flagged (STATUS). Business (line 8 → 3800
> 1y) + commercial (21 → 1aa) PARK behind **D_8936_003 interim ERROR** (spec info; the
> D_8911_004 pattern — soften when 3800 lands). **Diagnostics** 7 (OBBBA/MAGI/3800/
> VIN-PIS/tax-limited-LOST/used-price/seller-report). **Render**: f8936 + f8936sa (3-page
> SchA) manifest-registered (sha256 vs fresh irs.gov downloads), LABEL-VERIFIED bijective
> maps (31 + 65 widgets; the $4,000 line-16 sliver dead), render_8936 (main + one SchA per
> vehicle, Yes/No chains follow the face's stop logic; 8b/8e/13f/18b/18c auto-Yes only
> when the vehicle proceeds — stated boundary), **rendered-PNG visual pass done** (the S3
> forensics rule). **Input**: CleanVehiclesSection tab (type-driven fields) + prior-year
> MAGI/status + K-1 cards; D_8936_ → form_8936 routing; tsc clean. **GATES**: pure compute
> **18/18** · DB pipeline **8/8** (incl. repayment→line-17, the 6m-squeezes-6f ordering
> pin, disengage, render smoke) · render leg **10/10** · **FA-1040-8936-01..05** in the
> gate file (⚠ need RS loader homes) · **combined flow gate + all four legs = 422 passed**
> (reuse-db 2:54). Shared DB migrated + seeded (seed_8936 25 lines, seed_rules); test DB
> migrated through 0163. NEXT: Form 8835 unit (spec cached) → Form 3800 (⛔ RS spec is a
> thin 1120-S draft — Ken authors) → the S4 scenario.**

> **2026-07-04 (seventh session) -- ATS SCENARIO 3 (Lynette Heather) SCENARIO + MAPPER LEG
> COMPLETE ✅ -- 4 NEW MeF mappers + the Schedule D 1a/8a aggregate compute wire.** No
> migration. **Mappers** (each 1:1 vs its 2025v5.4 XSD, ReturnData positions 934/941/955/1263):
> `IRS1040ScheduleD` (QOF ind + the 1a/8a `TotalST/LTCGL1099BssRptNoAdjGrp` groups + combine
> spine 7/15/16 + face-routing 17-22; **v1 = the aggregate path ONLY** -- 8949 box totals
> raise, IRS8949 doc mapper unbuilt), `IRS1040ScheduleE` (**Part V farm-rental leg only** --
> A/B booleans + 40/41/42/43; Parts I-IV raise), `IRS1040ScheduleF` (identity + cash-method
> income/expense groups; MateriallyParticipatedInd required; accrual/passive raise),
> `IRS4835` (max 4; income/expense + aggregate-"Other" 30a row + Section 263A negative row +
> Section263AIndicatorCd; 34c signed on loss; PAL literal attr). **Compute wire:** the
> SCHEDULE_D 1a/8a rows (seeded since Topic 9, labeled "UNUSED v1") now enter the netting per
> the spec's own R-SCHD-L7-L15 ("L7 = combine 1a..6; L15 = combine 8a..14") -- direct-entry
> (d)/(e), computed (h), engagement extended, field map Row1a f1_3/f1_4/f1_6 + Row8a
> f1_23/f1_24/f1_26 (widget-verified). **Scenario:** `scenario3.py` + `mef_build_ats_scenario3`
> + `test_mef_scenario3_compute.py`; the SE FARM OPTIONAL METHOD verified already emitted
> (4b/15). Key-forensics corrections (rendered-PNG visual pass -- the text decoders missed the
> Sch F fills): 2,970 = Sch F line 26 SEEDS AND PLANTS (not supplies); PEC You checked; Sch E
> A/B both Yes; line 38 blank -> the i1040 IRS-figures-the-penalty election (FFV override --
> compute_2210 has no 6654(e)(2) arm, stated in DEFERRAL_AUDIT). **GATES:** command run ALL
> MATCH (income 72,235 / AGI 71,821 / 13a 567.40 / TI 55,504 / L16 6,328 / SE 827 / owe
> 3,750, every pin engine-verified) / 11-doc submission + manifest live-XSD-valid / pure
> efile 50/50 / combined DB gate (flow assertions + acroform maps + the S3 file) **440
> passed** (16m reuse-db; 1 test-shape fix re-run green). Artifacts UNSIGNED
> (`docs/mef/ats_out/scenario3`) -- Ken signs + uploads. NEXT: S4 needs 8835 + 8936 built
> first (RS specs cached), then the S4 scenario.**

> **2026-07-04 (sixth session) -- FORM 4835 (Farm Rental) RENDER LEG -> ✅ FORM 4835 COMPLETE
> (ALL FOUR LEGS TICK) -- commit `cee838f`.** No migration / no compute change (render-only).
> **Field map** `apps/tts_forms/field_maps/f4835_2025.py`: 63 AcroForm widgets, all on PDF page
> 0, LABEL-VERIFIED bijective vs the official PDF (scratchpad `label_verify_4835.py` — same-row
> printed-text extraction; the map covers every widget exactly once, asserted). Two-column Part
> II decoded: left col (Part2_LeftCol subform) f1_17-30 = lines 8-20, right col f1_31-58 = lines
> 21-30g + totals; 30a-f = desc+amount pairs; 30g §263A = desc f1_53 + amount f1_54; Line A
> Yes/No = c1_1[0]/[1]; 34a/34b = c1_4[0]/[1]; EIN header = Comb[0].f1_03 (comb, 9). **Manifest**
> `f4835` entry added — **sha256 `8ecc6172…` VERIFIED against a fresh irs.gov download**
> (genuine 2025 template, Attachment Seq 37, Cat 13117W). **render_4835()** (renderer.py after
> render_schedule_f_1040): one copy per Form4835 activity, MODEL-DRIVEN through the pure
> `compute_4835_lines` chain (the bridge-gate). Line 7 gross + line 31 total always print; net
> line 32 signed (parens on loss); on a loss the 34a/34b at-risk box + line 34c print; §263A on
> 30g prints parenthesized; **matpart activity -> NO face** (belongs on Schedule F). Registered
> in `ACROFORM_FORM_IDS`; packet emit after Schedule F; `SKIP_PAGES["Form 4835"]={1,2}` strips
> the two instruction pages so each activity = one page. **v1 render boundary:** the model stores
> the six 30a-f "other expenses" as a single aggregate -> printed on line 30a labeled "Other"
> (does not itemize the six specify rows; DEFERRAL_AUDIT). **Header** = the 1040 filer name(s) +
> SSN (the render_8283 pattern). **GATES:** `test_4835_render_leg.py` **11/11** (bijective map +
> map-covers-every-line no-DB trip-wires; income face sweep incl. Line-A box; multi-line L7 sum
> vector; loss parens + 34c + at-risk box; §263A parenthetical; matpart no-copy; two-activity
> two-copy; packet one-page). Combined **flow-assertion gate + all four 4835 legs = 412 passed**
> (reuse-db 15m). NEXT: the S3 (Heather) scenario + mapper leg — all forms now built.**

> **2026-07-04 (fifth session) -- FORM 4835 (Farm Rental) COMPUTE/DIAGNOSTICS/SEED LEGS --
> commit `c7cae44`, mig 0161.** Parallel RS session authored all four blocking specs
> (4835/8835/8936/8936_SCHA export 200); this session built the 4835 unit spec-first.
> **compute_4835.py**: gross L7 = 1+2b+3b+4a+4c+5b+5d+6 (spec includes the L4a CCC-election +
> L5d, resolving my authoring-note [VERIFY]); L31 expenses with §263A on 30g reducing; net L32;
> **FULL loss path** — §465 at-risk (Form 6198) BEFORE §469 passive (Form 8582 $25k active-
> participation allowance with the MAGI>100k phaseout) → line 34c + suspended PAL. Multi-
> instance (T12): Schedule E line 40 = Σ each activity's net/loss. **Model Form4835** (mig 0161,
> fields 1:1 with spec fact keys). **Wiring**: compute_4835_db runs before compute_schedule_e_db;
> a Form4835 now ENGAGES Schedule E (was the S3-flow bug — 4835-only returns disengaged); line
> 42 (gross) added to _SCHE_P2_OUTPUT_LINES (seeded-but-never-written); line 41 folds in line 40
> → Sch 1 L5. **NOTHING to Schedule SE** (§1402(a)(1), the defining invariant). **rules_4835.py**
> 9 diagnostics (MATPART/SE_GUARD/LOSS_LIMITED/CCC_ELECT/CROPINS_DEFER/263A/REPRO/SCHJ/QBI);
> retired the stale `D_SCHE_4835` "not built" RED. **seed_4835** (FORM_4835, 52 lines). QBI is
> preparer-asserted (default NOT QBI — no auto 8995/8995-A feed). **GATES**: pure compute
> **13/13** (spec vectors T1-T12+S3) · integration **2/2** (S3: Sch E 40=11,061 / 42=17,035 /
> Sch 1 L5=11,061, none to SE) · diagnostics **5/5** · flow gate **HELD (401 passed)**.
> ⏳ REMAINING for the form to TICK: render leg (f4835 AcroForm field map — template downloaded,
> 63 fields, not yet committed) + the S3 scenario/mapper. Then S4 (8835/8936/SchA + scenario).**

> **2026-07-04 (fifth session) -- BROKERAGE 1099-B SUMMARY-EXTRACTION SKELETON (8949
> Exception 2) -- commit `c25635f`, mig 0160.** Pivot away from the blocked S3 (Form 4835
> has NO Rule Studio spec — 404, RS server confirmed up via 8283 200; STOPped per the
> mandatory RS rule rather than improvise. S4's 8835/8936 are also 404; only 3800 has a spec.
> Ken ruled "pivot to an unblocked unit"). Delivered the season-one brokerage import boundary
> (SEASON_PLAN item 4). **Leg A** — `compute_schedule_d` now auto-applies code M to every
> `is_summary` row (RS `R-8949-SUMMARY` "code M auto" was UNIMPLEMENTED; this is the single
> source consumed by `render_8949_1040` col (f), the D_8949 diagnostics, and the e-file).
> **Leg B** — new `apps/returns/brokerage_1099b.py::import_brokerage_summary()` maps per-box
> category totals → Exception-2 `CapitalTransaction` summary rows (description "broker — see
> attached statement", blank b/c, code M, statement_attached); idempotent per broker;
> `provenance=IMPORTED` (YELLOW) + `import_confirmed` mandatory-confirm gate (SEASON_PLAN
> item 5). Model + mig 0160 add `provenance`/`import_source`/`import_confirmed` (mirrors the
> existing `DataProvenance`; moved that enum above `CapitalTransaction`). New diagnostic
> **D_8949_006** (warning; trip-wire 16→17). The OCR/AI PDF front-end plugs in ABOVE this seam
> (August "extraction to production"); frontend YELLOW rendering is likewise deferred — the
> skeleton stops at the backend normalized-payload boundary by design. **GATES:** new
> `test_brokerage_summary_skeleton.py` **8/8** (create · idempotent+spares-manual · empty-skip ·
> bad-box/amount reject · flow to Sch D L16 / 1040 L7 · code-M auto · D_8949_006 gate · render
> smoke); flow **381 HELD**; topic9 compute + diagnostics leg GREEN. **Follow-up:** add
> D_8949_006 to the 8949 RS spec on the next Schedule D touch (app-level workflow gate, not
> silently written to the cached JSON).**

> **2026-07-03 (fourth session) -- 8995-A SCHEDULE D DB GATE CLEARED ✅ (unit TICKS) + S2
> (JONES) SCENARIO + MAPPER LEG BUILT -- commits `89a4f88`/`2a151b5`/`c10e51c`/`9b5dcad`,
> mig 0159.** **8995-A DB legs:** three pooler-fought runs (25/20/16 min; 21 connection-kill
> E/F's total) — every test in the five leg files GREEN on current code; 4 real failures were
> ALL seeder/test-side (the spec's 7 SD lines were never in `seed_8995a`'s hardcoded SECTIONS
> list → new f8995a_schd section, 58 lines / 8 sections, reseeded on the SHARED DB with
> `seed_rules`; the patron-delta test missed the §199A income-limit interaction — engine
> 48,013/53,863 hand-verified CORRECT; a title line-wrap; two stale 51-counts). Engine math
> untouched. **S2 scenario leg:** full key forensics (+29 char-shift fills; [l]-path vector
> checks + ✔ glyphs; the key fills INPUTS ONLY — computed lines blank; **Sch A line-18
> itemize-anyway CHECKED**, 27c checked, statutory box checked, dependent col (7) blank =
> fiat #2). **FOUR NEW MAPPERS** 1:1 vs 2025v5.4 XSDs: IRS1040ScheduleA (5a sales ind,
> line-11 `qualifiedContributionsAmt` attr = the dotted "$200" — new Taxpayer field mig 0159
> + abs_pos face literal + FormEditor input; line-12 → IRS8283 ref; `ItmzdDedLessThanStdDedInd`;
> 15/16 guards raise — 4684/misc-statement unbuilt), IRS8283 (Section A complete, one document,
> all rows; **Section B raises** = wet-ink J7 boundary; col (e) → YYYY-MM|VARIOUS), ReturnHeader
> `PaidPreparerInformationGrp` (from PreparerInfo, after Filer), Sch C `AdditionalVehicleInfoGrp`
> (43-47b — boundary retired; 47a/b = the EXISTING has_evidence/evidence_written fields).
> scenario2.py + mef_build_ats_scenario2 + NRA statement PDF (the first live BinaryAttachment
> consumer). **ENACTED PINS ALL MATCH the engine** (hand-computed first): 1a 8,513 (statutory
> wages off 1a) / Sch C 31 = 26,979 no-SE / 12 = 22,201 ELECTED / **13 = 2,658.20** (the
> 8995-A income-limit arm binds — the 1040 line-13 FFV carries CENTS; 15 = 10,632.80 → table
> 1,063) / 19 = 500 ODC / 24 = 563 / 26 = 300 divorcedSpouseSSN / refund 901 / 27 CLEARED.
> The 11-document submission (1040, Sch 1, 8812, Sch A, Sch C, 8283, 8995-A, 8995-A Sch D,
> W-2 ×2, BinaryAttachment) validates vs the LIVE 2025v5.4 XSDs; artifacts in
> docs/mef/ats_out/scenario2. 8995-A pins ride the XML (engine-result-driven, no FFV rows —
> the 7217 pattern). Test file **29/29 GREEN** (--timeout=1800 — the 13-seed fixture outlasts
> the 300s cap; 2 helper-case expectation fixes: name_line1/business_name_line keep entry
> case). Gates: flow 381 · efile 31/31 · acroform 33/33 · tsc 0 · vitest 275. Detail →
> STATUS.md + DEFERRAL_AUDIT (fourth session).**

> **2026-07-03 (third session) -- FORM 8995-A SCHEDULE D (patron reduction §199A(b)(7) + capped
> DPAD §199A(g)) -- ★★ BUILT, ALL PURE GATES GREEN, 🔴 DB LEGS POOLER-BLOCKED (no tick until
> green) -- SPEC-FIRST RS round-trip -- flow gate unchanged 381 (FA-08 amended in place).**
> **Ken RULED (AskUserQuestion): "Is this a MeF test scenario? If so build 8995-A Schedule D"**
> — retired the stated 8995-A patron/DPAD RED-defer for the S2 (Jones) leg. **SPEC** (RS
> `9b4490c`, amend-by-lookup): +`a_business_schd_qbi_alloc`/`_wages_alloc`/`a_dpad_199ag`,
> R-8995A-PATRON reauthored routing→calculation (SD2–SD6 → L14), SCOPE/P2-COMP/P4-LIMIT amended
> (patron → 8995-A at ANY income per i8995a 2025 verbatim; face line-3 skip at/below threshold;
> L15 floor; **L38 cap = L33 − L37 face verbatim**), +7 SD lines, **D_8995A_001/002 REPURPOSED
> error→info (Ken ruled)** + NEW 008/009 guards, T9 amended + T11–T15 HAND-COMPUTED (T13 = the
> S2 below-threshold zero-wage patron — proves the wage limit does NOT zero it; T14 = full
> allocation + zero wages → reduction still $0, the lesser arm); new source IRS_8995A_SCHD_FORM;
> integrity checker extended, 15 scenarios ALL PASS; export drift = exactly the amendment.
> **MODEL** mig 0158 (APPLIED). **COMPUTE** `f10e864`: engine per-column L14/SD chain + skip +
> cap; `form_8995a_patron_engaged` = THE single bridge-gate (router/render/extract/diags);
> `compute_8995a_db` returns the result dict; D_8995_001 retired-dormant. **INPUT** serializers
> `__all__` (auto), FormEditor conditional patron-alloc fields (Sch C + F). **RENDER** f8995ad
> Rev 12-2022 registered (manifest+SHA) — 23-widget map BIJECTIVE; render_8995a fills
> 1{col}_patron + L14/L38 and inserts Sch D copies at page 2 (3 patron columns/copy). **MeF**
> `2c6e8a1`: F8995ASource re-derived via the render gates; IRS8995A + IRS8995AScheduleD
> builders (full XSD-order maps; patron-anchored zeros STATED; SSTB/Agg = XSD CHOICE);
> ReturnData slots after IRS8995 (XSD 2224/2252); efile 31/31 incl. LIVE XSD of the pair.
> **⚠ S2 ENACTED-PIN DIVERGENCE (S13-class):** the key's blank 13a is scenario fiat — enacted
> law computes NONZERO 13a + attaches the QBI documents the key lacks. 🔴 CARRIED: the DB legs
> (3 pooler-sick attempts, zero real failures) + prod `seed_rules` + the RS 8995 sibling
> loader's D_8995_001 retirement note (DEFERRAL_AUDIT). Detail → STATUS.md + RS session_log.

> **2026-07-03 (second session) -- S2 RETURN-LEVEL ARMS (identity/header + EIC opt-out 27c) --
> ★★ BOTH UNITS COMPLETE -- commits `86d4e38` + the 27c unit -- flow gate 380→381.** The
> DEFERRAL_AUDIT item-10 S2 gap list, corrected and closed. **STALE-LIST FINDINGS:** the
> statutory-employee Sch C arm was ALREADY BUILT (W-2 Unit 2, 2026-06-15 — the
> federal-backlog-already-built lesson); the former-spouse SSN needed no statement (the XSD's
> `divorcedSpouseSSN` ATTRIBUTE on line 26). **UNIT A (identity arms, `86d4e38`):**
> deceased-spouse header — c1_3 + the MM/DD/YYYY entry spaces f1_05-10 render from the
> mig-0049 date fields (i1040 "Death of a Taxpayer" verbatim), Pub 4164 §12.5 DECD name line
> (all four cases; S2 = `JOHN A & JUDY DECD<JONES`), the i1040 "Filing as surviving spouse"
> literal via the NEW `AcroField.abs_pos` overlay mechanism (widget-less face text), IRS1040
> DeceasedInd/PrimaryDeathDt/SpouseDeathDt · NRA-spouse §6013(g)/(h) election (mig 0155;
> spine fact was ALREADY SPECCED sort-5) — c1_9 + f1_30 render + NRASpouseTreatedAsResidentGrp
> + TaxpayerInfo checkbox · ReturnHeader IdentityProtectionPIN/SpouseIdentityProtectionPIN ·
> BinaryAttachment document builder + binaryAttachmentCnt (package.py already carried
> /attachment) · line-26 divorcedSpouseSSN. Gates: efile pure 29/29 incl. LIVE XSD validation
> of the full arm set, header-render DB 5/5, map audit, flow 380, tsc, vitest 275. **UNIT B
> (EIC opt-out 27c, spec-first):** **Ken RULED "skip-entirely, ACTC-sibling"**
> (AskUserQuestion) — RS 1040_EIC amended (`e64dbea`: fact `eic_opt_out`, R-EIC-27A gate,
> D_EIC_017 info, scenario EIC-G6, FA-1040-EIC-10 loader-homed; seeded, deployed export
> verified +1/+1/+1 zero-removal drift, canonical eic_spec.json refreshed 16→17 tests) →
> tts mig 0156 + `eic_engaged`/`compute_eic` gates (the EXPLICIT line-27 clear kills the
> stale-value hazard; the legacy eitc_claimed spine write precedes it) + Schedule EIC render
> suppression + c2_13 box + DoNotClaimEICInd + EIC-tab checkbox + D_EIC_017 (seed_rules run).
> Gates: flow **381** (FA-1040-EIC-10 runner arm added) · topic7 compute+diagnostics DB green
> after one 42-min pooler-sick run (15 fixture ERRORs = the documented connection-kill class;
> all re-ran green) + 3 real test-shape fixes (family pin 16→17, the cached-taxpayer flip
> mutation, the `_fires` handler collision) · efile 29/29 · vitest 275 · tsc. NEXT: the S2
> (Jones) scenario + mapper leg — needs the Sch A/C fill forensics (the +29/−29 decode is in
> the scratchpad notes) and ONE Ken question: the ag-co-op-patron "no QBI" scenario note vs
> the engine computing QBI on a statutory-employee Sch C (§199A patron reduction = the 8995-A
> RED-defer). Detail → STATUS.md + DEFERRAL_AUDIT 2026-07-03 (second session) + RS session_log.

> **2026-07-03 -- FORM 8283 (noncash charitable contributions, §170(f)(11)) -- ★★ UNIT
> COMPLETE, ALL FOUR LEGS GREEN -- tag `1040-form-8283-complete` -- SPEC-FIRST RS round-trip --
> flow gate 375→380.** ATS Scenario 2's tax-law form, smallest-first per Ken (S2→S3→S4). The
> SEASON_PLAN appendix-4 OBBBA check ran AT the spec leg: §170(f)(11)/(f)(12) untouched by
> P.L. 119-21; the $500/$5,000/$500,000 tiers statutory + non-indexed; the Rev. 12-2025
> form/instructions current (fetched verbatim). **SPEC**: RS `load_1040_form_8283.py` (RS
> `0748f06`) AMENDS the shared 1120S/1065/1040 stub BY LOOKUP (entity_types preserved; the
> 1120s-era placeholder 10 facts/5 unnamed rules/SA-* lines/D001-D003/2 tests RETIRED) — 51
> facts / 5 rules / 49 lines / 13 diagnostics / 13 HAND-COMPUTED scenarios / 5 loader-homed FA;
> sources USC_26_170F11 (verbatim) + IRS_2025_8283_INSTR refreshed to Rev. 12-2025 verbatim;
> `check_8283_integrity.py` ALL PASS. **Ken's walk (AskUserQuestion): J6 RULED "warn only,
> feed anyway"** (substantiation REDs — no appraisal §170(f)(11)(C), vehicle CWA §170(f)(12),
> attach-tier — fire while the amount STILL FEEDS line 12; the withhold recommendation
> rejected); J2 default-50%-bucket + bind-gated D_8283_008 and J4 conservation RED-defer
> (the ONE feed withhold) as recommended; J1/J3/J5/J7 + stub retirement approved.
> **MODEL** `NoncashContribution` (one row per item or per similar-items GROUP — J5; migs
> 0153 + RLS 0154, APPLIED). **COMPUTE** `compute_8283.py` (`row_analysis`/`noncash_summary`
> = THE bridge-gate); Schedule A line 12 buckets DEFAULT to the row totals (50% ordinary /
> 30% capgain — YELLOW) with the flat facts as per-field GREEN overrides (R-8283-SCHA12,
> the 5a feeder pattern); `charitable_noncash_inputs` shared with the diagnostics;
> D_SCHA_004 repointed to the resolved inputs. **INPUT** serializer + Meta.fields (the
> 7217-hotfix lesson honored) + 2 CRUD actions (recompute-on-mutation) +
> `NoncashContributionsSection` (rows grid + expandable Section-B/appraiser/donee detail)
> on the schedule_a tab; tsc 0 / vitest 275. **RENDER** f8283 Rev-12-2025 registered
> (manifest+SHA256): 117-widget field map LABEL-VERIFIED bijective vs the PDF; `render_8283`
> — Section A 4-row grid w/ overflow copies + ONE Section B copy per item (per-donee-per-item
> rule); packet gated on engaged (total > $500; the feed flows regardless). **13 D_8283_***
> (rules_8283, seed_rules run — 13 registered). **GATES**: 16 pure (13 scenarios + round-trip
> smoke) + flow gate **380** + **DB pipeline 6/6 (101s)** incl. the S2 pin (line 11 250 /
> 12 700 / 14 950), GREEN-override 650, conservation-withhold, sub-$500 feeds-without-form,
> J6 warn-only, and the detail-payload serializer net + `manage.py check` clean. Boundaries →
> DEFERRAL_AUDIT 2026-07-03 (10 items incl. the S2 MAPPER-leg gaps: deceased spouse, NRA
> statement, IP PIN, former-spouse SSN, STATUTORY-EMPLOYEE Sch C, EIC opt-out 27c). NEXT:
> the S2 mapper needs those return-level arms first. Detail → STATUS.md + RS session_log.

> **2026-07-02 (eighth session) -- SCHEDULE SE R-SE-ROUND: per-line whole-dollar rounding --
> ★ UNIT COMPLETE, SPEC-FIRST RS round-trip -- flow gate unchanged 375 (FA-1040-SCHSE-02
> amended in place, zero id drift 353=353).** Resolved the S12 REVIEW_QUEUE item same day:
> **Ken ruled per-line whole-dollar** (the engine cents-chained SE — 12 = 3,437.44 — diverging
> $1 from the ATS key/TaxWise 2,786 + 652 = **3,438** and printing 10+11 ≠ 12). **SPEC**: i1040
> "Rounding Off to Whole Dollars" fetched + quoted VERBATIM (new excerpt on IRS_2025_1040_INSTR;
> the WebFetch paraphrase was wrong again); NEW rule **R-SE-ROUND** (multiplication lines
> 4a/5b/10/11/13 round their product half-up; sum lines add already-rounded entries; L2 sums
> cents → rounds the total); SE-T1..T5 re-pinned (T5 load-bearing: 4,238 vs cents 4,238.87);
> `check_topic8_integrity` per-line ALL PASS; seeded RS Supabase (rules 13→14), deployed export
> verified, canonical `schedule_se_spec.json` refreshed (RS `37f21b3`). **COMPUTE**:
> `compute_schedule_se_lines` per-line `_qd` (entered lines round at entry; the $400/$100 floors
> compare rounded values); minister + the MeF SE extractor ride the same helper automatically;
> Form 7206 consumes the now-whole ½-SE untouched (`_r0` already whole-dollar). **RE-PINS**:
> topic8 compute/input/diagnostics + Schedule F pure + FA runner (SCHSE-01 half-up dollar,
> SCHF-08 source pin) + **S12 re-pinned to the engine** (½-SE 1,719 → AGI 122,445.00 / QBI
> 4,321.80 / tax 17,436.10 / owe 6,430.10 — **every whole-dollar XML value unchanged**: tax
> 17,436 / total 20,874 / owe 6,430; SE line 12 now MATCHES the IRS key). **AUDIT (the ruling's
> companion)**: 7206 already per-line; Schedule C sums-only (no action); **8995 ×20% lines
> (5/9/14) still cents → flagged, and **Ken RULED "build it now" SAME SESSION: R-8995-ROUND
> BUILT as the identical unit** (RS `bfe1d4f`; ×20% lines round their product, entries at
> entry, 1i-1v face rows whole; 8995-T2 re-pinned 9,952; `compute_8995_lines` per-line `_qd`;
> S12 re-pinned AGAIN — QBI **4,322** whole → taxable 102,373.00 / tax 17,436.06 / owe
> 6,430.06, whole-dollar XML still unchanged; 8995-A + 8959 stay cents-chained = stated
> boundaries, DEFERRAL_AUDIT). **GATES**: flow 389 ×2 · efile pure 23/23 ×2 · spec-scenario 16
> · DB 96 passed (+3 pooler connection-kills re-run green) + the 8995-leg DB re-confirm ·
> final `mef_build_ats_scenario12` ALL MATCH + XSD-valid, artifacts rebuilt →
> `docs/mef/ats_out/scenario12/`. **🔧 HOTFIX riding the commit: `partnership_distributions`
> was declared on TaxReturnSerializer but missing from Meta.fields (7217 unit) → every
> return-detail GET 500'd since `b665e55`; caught by this session's DB sweep, one-line fix.**
> Detail → STATUS.md + RS session_log.

> **2026-07-02 (seventh session) -- MeF ATS Scenario 1040-12 (Sam Gardenia) through the REAL
> engine + SIX new e-file document mappers + the W-2 box-12/state groups -- ★★ UNIT COMPLETE --
> flow gate unchanged 375 (pure serialization; the one compute-file touch is the ADDITIVE
> reader `compute_schedule_c.form_7206_rows` — no math change).** The Schedule C family
> scenario (Single, KY; DESIGNER "ENERGY BUILD" sole prop; SEHI 1,000; a no-gain 7217
> distribution). **MAPPER**: read_model + builder grew **IRS1040ScheduleC** (header A-J +
> Parts I/II/III; the compute leg's PERSISTED output columns override the pure chain — carries
> the 8829 reflow; Part IV vehicle answers + line-E address = stated boundaries),
> **IRS1040ScheduleSE** (lines 7/14 are PRE-PRINTED — no XSD elements, never emitted; the
> extractor re-derives line 2 through the SAME helpers compute ran and RAISES on drift vs the
> persisted se_tax), **IRS1040Schedule2** (full SCH2_LINE_ORDER; the 1e/1f/1y/17a repeating
> typed groups map to their TOTAL elements only), **IRS7206** (via the NEW
> `form_7206_rows` bridge-gate reader; extractor RECONCILES Σ line 14 vs Sch 1 L17 — S-corp/
> farm/Medicare-fold SEHI raise UnmappableValue), **IRS8995** (line-1 identity rows from the
> ScheduleC/K-1 models mirroring render_8995; line 15 schema-REQUIRED; the 16/17 carryforward
> zeros emit), **IRS7217** (one per distribution via `distribution_results` — withheld §732(c)
> allocation or gain-with-unasserted-holding-period raise; line 7 emits its figured 0; the
> three Part II line-B totals close the element) — plus IRSW2 **EmployersUseGrp** (box 12 DD)
> + **W2StateLocalTaxGrp** (boxes 15-17 KY). ReturnData1040 positions VERIFIED: 1040 → Sch1 →
> **Sch2** → Sch3 → 8812 → **SchC*** → EIC → **SchSE*** → 1099R* → 2441 → 6251 → **7206*** →
> **7217*** → 8862 → 8863 → 8911* → 8911SchA* → **8995** → W2*. **SCENARIO**:
> `ats/scenario12.py` + `mef_build_ats_scenario12` (rollback default, seq 4) — engine matches
> the hand-computed ENACTED-LAW pins: **43 pins ALL MATCH** (1040 ×19 / Sch1 ×5 / Sch2 ×2 /
> 8995 ×11 / 7217 face ×8): AGI 122,445 / std **15,750** (key stale 15,000) / **QBI 4,322**
> (the key OMITS 8995 entirely) / tax 17,436 (key 18,634 on its stale inputs — reproduced) /
> owe **6,430** (key 7,628); 7217 col (e) **6,000** per §732(a)(2)+(c) (the key's own 4,000
> contradicts its line 10). §6654: scenario silent on prior year, key computes NO penalty →
> `t2210_prior_year_tax=14,444` models the exactly-met 100% harbor (line 38 blank, FORM_2210
> disengaged — documented input modeling). **⚠️ SURFACED — Schedule SE rounding convention
> (REVIEW_QUEUE, Ken to rule):** the engine cents-chains SE (12 = 3,437.44 → 3,437 on Sch 2
> 4/21 + 1040 23); the key/TaxWise round per line (2,786 + 652 = 3,438); the engine's own
> printed SE face shows 10+11 ≠ 12 at whole dollars. Total tax/owe agree either way.
> PRE-EXISTING, documented in the artifact README, never bent. (SE line 9: engine 70,222 is
> CORRECT; the key's 62,722 is a typo.) **GATES**: 42 pure (test_efile_mef_1040 incl. the
> nine-document structural + XSD tests, + scaffold) + **DB scenario suite ALL GREEN**
> (test_mef_scenario12_compute: 19 line pins + Sch1/2/8995 + 7217 face + SE/no-penalty + the
> artifact leg) + XSD-valid first try + `manage.py check` clean + flow gate **375 green**
> (9 getsource-pin FAs flaked when the PARALLEL 4797 session saved compute.py mid-run —
> re-run 9/9 green on the stable tree). Boundaries → DEFERRAL_AUDIT 2026-07-02 (8 items).
> Artifacts → `docs/mef/ats_out/scenario12/` (gitignored). NEXT: S2 (8283) / S3 (4835) / S4
> (3800+8835+8936) each need their tax-law unit first, smallest-first per Ken. Detail →
> STATUS.md.

> **2026-07-02 (sixth session) -- FORM 7217 (§§731/732 partnership property distributions) --
> ★★ UNIT COMPLETE, ALL FOUR LEGS GREEN -- tag `1040-form-7217-complete` -- SPEC-FIRST RS
> round-trip -- flow gate 370→375.** ATS Scenario 12's tax-law form, smallest-first per Ken.
> The SEASON_PLAN appendix-4 OBBBA check ran AT the spec leg: §732 is a basis provision, no
> sunset (amendments end at P.L. 106-170 (1999)); Dec-2024 form/instructions current; ONE
> post-publication development found + encoded (IRS 2026-04-27: TY2025+ K-1 box 19 codes B/C/G →
> line 3, A/D/F → 5a, A/F statement → 5b — sourcing hints, Dec-2026 instructions revision).
> **SPEC**: RS `load_1040_form_7217.py` (RS `de3b2f9`/`c824378`) 31 facts / 5 rules / 23 lines /
> 11 diagnostics / **18 HAND-COMPUTED scenarios** (T2/T3 pin the i7217's OWN Examples 1+2 verbatim
> incl. the 100/440/110 waterfall; every §732(c)(1)(A)/(B)+(c)(2)(A)/(B)+(c)(3)(A)/(B) branch) /
> 5 loader-homed FA; NEW independent gate `check_7217_integrity.py` (statute-direct waterfall,
> self-tested vs Example 2) ALL PASS. **Ken's walk (AskUserQuestion): J1 RULED "wire §731(a)(1)
> gain to Sch D now"** (landing verified verbatim vs the 2025 Partner's Inst. K-1 box 19: "Form
> 8949 and the Schedule D"; NEW source IRS_2025_K1P_INSTR) — encoded as a synthesized no-1099-B
> 8949 row (box C ST / box F LT per the NEW holding-period assertion; proceeds=5c basis=6;
> unasserted → feed WITHHELD + D_7217_011 RED); J2 §731(a)(2)-loss + J3 §737 defers KEPT; J4
> rounding (half-up, residue-to-largest), J5 classification-withhold, J6 securities 5b-feeder
> approved. Seeded RS Supabase; deployed export id-verified (lookup key `FORM_7217`); FA drift =
> EXACTLY the 5 new ids. **MODEL** `PartnershipDistribution` (one Form 7217 per distribution
> date) + `DistributedProperty` children (migs 0150 + RLS 0151, APPLIED) — MULTI-INSTANCE, NO
> FFV face rows. **COMPUTE** `compute_7217.py` pure helpers (Part I chain; the §732(c) waterfall;
> `distribution_results` = THE bridge-gate every consumer reads); the J1 wire injects virtual
> box-C/F rows into compute_schedule_d_db's BOX TOTALS (never the l4/l11 args — double-count
> guard FA-05) + a schedule_d_engaged arm; 11 D_7217_* diagnostics (rules_7217, seed_rules run).
> **INPUT** nested serializers + 4 CRUD actions (recompute-on-mutation) + the two-level
> `PartnershipDistributionsSection` on the form_7217 Income tab; tsc 0 / vitest 275. **RENDER**
> f7217 registered (manifest+SHA256), label-verified map incl. the generated 30-row Part II grid;
> `render_7217` one copy per distribution via distribution_results; the synthesized rows join
> render_8949_1040 (desc truncates at the face's 30 chars). **GATES**: 20 pure compute + 6 render
> + **DB pipeline 4/4** (Gardenia no-gain/render smoke; gain-LT → 8949 box F → Sch D 16 → 1040
> line 7 = 2,000; withheld-until-asserted incl. the assert-flip; money-only + unclassified REDs;
> one pooler connection-kill re-ran green) + flow gate **375** + check clean. ⚠️ S12 answer-key
> notes for the mapper leg: key omits QBI + uses pre-OBBBA 15,000 std ded; its own 7217 Part II
> col (e) 4,000 contradicts its line 10 = 6,000 (engine follows §732 → 6,000 = Example 1); SE
> line 9 key typo 62,722 (176,100−105,878 = 70,222, immaterial). Boundaries → DEFERRAL_AUDIT
> 2026-07-02 (9 items). NEXT: the Scenario-12 mapper needs SIX new document builders (ScheduleC/
> ScheduleSE/Schedule2/7206/8995/7217 — 8995 because ENACTED law computes QBI 4,322 the key
> omits). Detail → STATUS.md + RS session_log.

> **2026-07-02 (fifth session) -- MeF ATS Scenario 1040-13 (William & Nancy Birch, §30C) through
> the REAL engine + 3 new e-file document mappers + a found-and-fixed Schedule 3 mapper gap --
> ★★ UNIT COMPLETE -- flow gate unchanged 370 (pure serialization, no compute/render change).**
> The 8911 engine unit (same day, tag `1040-form-8911-complete`) made this mapper pure
> serialization — no RS spec needed. **MAPPER**: read_model + builder grew **IRS6251**
> (spine-always/add-backs-nonzero emit mirroring render_6251; the SAME `compute_6251_face`
> bridge-gate source; Part III = the render leg's anchors-12/40 v1 boundary; attached when 8911
> engages — the i8911 line-8 "figure the TMT even with no AMT owed" rule, exactly why the
> scenario's form list includes 6251 — or when AMT>0; a RED-deferred 6251 raises UnmappableValue),
> **IRS8911** (FORM_8911 face FFVs; line 8 figured-TMT emitted EVEN AT 0 like the IRS key; zeros
> elsewhere omitted; line 3 business/K-1 > 0 raises UnmappableValue — the Form 3800 gap, mirrors
> D_8911_004; referenceDocumentId links the Schedule A docs), **IRS8911ScheduleA** one per
> RefuelingProperty row (cells via compute's OWN `property_schedule_a` — bridge-gate; the FACE
> stop rules encoded: tract-No stops after 6a, 0% business skips 9-16, 100% business skips Part
> III, non-main-home stops at 17; schema-required desc/dates/6a-indicator validated at extract).
> ReturnData1040 positions VERIFIED: 2441 → 6251 → 8862 → 8863 → 8911* → 8911ScheduleA* → W2*
> (IRS8911 includes from CorporateIncomeTax/Common — the include path confirmed). **🔧 GAP
> FOUND+FIXED: `SCH3_LINE_ORDER` was missing 6i/6j/6k — Schedule 3 line 6j (the 8911 credit
> itself!) was silently DROPPED from every submission** (XSD-valid, totals masked it); added from
> the verified XSD; 6z = a repeating type-text group the app can't fill (stated boundary,
> DEFERRAL_AUDIT); 6e face-Reserved never emitted. **🔧 FILER NAME LINE = Pub 4164 §12.5**
> (fetched verbatim from the local pub): NEW `values.filer_name_line1` — "HENRY A<CARTER", MFJ
> "JOHN A & JANE B<SMITH" (Birch = the first MFJ scenario forced it), different-last
> "CARL A<JONES<& ANGIE MYER", the over-35 shortening ladder; header-ONLY (PersonNameType forbids
> '<'). ⚠️ scenarios 5+8 artifacts predate this — REBUILD before IFA upload. **SCENARIO**:
> `ats/scenario13.py` + `mef_build_ats_scenario13` (rollback default, seq 3) — engine matches the
> hand-computed ENACTED-LAW pins to the dollar (28 pins: 1040 ×18, 8911 face ×7, Sch 3 ×3, + the
> 6251 face ×10): tax 11 / credit 11 / **refund 609** (⚠️ the answer key's pre-OBBBA 30,000 std
> ded gives 162/162 — refund 609 IDENTICAL since the credit fully offsets; the earlier STATUS
> "598" figure was wrong — [[ats-answer-keys-pre-obbba-stale]]); README documents the divergence.
> **GATES**: 21 pure (test_efile_mef_1040 incl. the six-document XSD validation + name-line
> cases) + 19 scaffold + **DB 22/22** (17 line pins + 8911/Sch3 face + 6251 figured-TMT face +
> the artifact leg; two pooler connection-kills re-run green) + flow 370 + check clean.
> Artifacts → `docs/mef/ats_out/scenario13/` (gitignored). NEXT: S12 (7217) needs the Form 7217
> tax-law unit FIRST. Detail → STATUS.md + `.claude` [[mef-line-order-maps-silent-drop]] (new).

> **2026-07-02 (fourth session) -- FORM 8911 + Schedule A (§30C refueling credit) -- ★★ UNIT
> COMPLETE, ALL FOUR LEGS GREEN -- tag `1040-form-8911-complete` -- SPEC-FIRST RS round-trip --
> flow gate 366→370.** ATS Scenario 13's tax-law form, smallest-first per Ken's ruling. The
> SEASON_PLAN appendix-4 **OBBBA sunset check ran AT the spec leg**: §30C(i) verbatim (statute +
> i8911 Rev 12-2025) terminates the credit for property placed in service **after 6/30/2026** —
> TY2025 fully live, **TY2026 HALF-YEAR live (Jan–Jun installs)**, TY2027+ dead → S13 survives,
> no re-rule; the window is EXPLICIT in compute (never a latest-year fallback) + FA-03 pins it.
> **SPEC**: RS `load_1040_form_8911.py` (RS `bd739e1`) 16 facts / 5 rules / 35 lines ("A-"
> prefixed Sch A) / 6 diagnostics / 10 HAND-COMPUTED scenarios / 4 loader-homed FA;
> `check_8911_integrity.py` ALL PASS; Ken approved all four judgment items in-session (the
> line-6b ORDERING reading — its verbatim text is self-referential via Sch 3 line 7; census
> tract = PREPARER ASSERTION v1 with 11-digit GEOID format validation; business path RED-defers
> to the unbuilt Form 3800 D_8911_004; T1 pins ENACTED law — ⚠️ the IRS answer key uses the
> PRE-OBBBA 30,000 std ded → their 162 vs the engine's 11, see the new `.claude`
> [[ats-answer-keys-pre-obbba-stale]]); seeded RS Supabase, deployed export id-verified, FA
> drift-check delta = exactly the 4 new ids. **MODEL** `RefuelingProperty` (one Sch A per row)
> + `Taxpayer.refueling_k1_credit` (migs 0148 + RLS 0149, APPLIED). **COMPUTE** `compute_8911`
> (window/claim-year/tract gates; 30%/$1,000-per-item personal at the main home; 6%/30%-PWA/
> $100,000 business + §179 backout + the 1/29/2023 auto-Yes; L5 = 1040 L16 + Sch2 1z; 6b = L19
> + the pre-8911 Sch 3 credits; **L8 = TMT via compute_6251_result FIGURED ALWAYS** per i8911;
> L10 = min(L4, L9) → Sch 3 6j, excess PERMANENTLY LOST) wired after 8812/the 19-20 sync,
> before the second downstream pass; business line 3 never lands (D_8911_004 RED). **INPUT**
> serializer + `refueling-properties` CRUD (recompute-on-mutation) + `RefuelingPropertiesSection`
> on the new form_8911 tab (Credits nav group; D_8911_ routing) + the K-1 line-2 card; tsc 0 /
> vitest 275. **RENDER** f8911 + f8911sa registered (manifest + SHA256), every widget
> LABEL-VERIFIED; `render_8911` = the face from FFVs + ONE Schedule A per property from the
> model via the SAME `property_schedule_a` helper, merged into one packet doc; the pre-printed
> $100k/$1k cap slivers deliberately unmapped. **GATES**: 18 pure + 7 render + **DB pipeline
> 4/4 in 83s** (Birch 300→11→Sch3 6j→1040 L20; tract-No disengage; business-only face-not-6j;
> render smoke w/ GEOID comb); flow gate **370**; `manage.py check` clean. Boundaries →
> DEFERRAL_AUDIT 2026-07-02 (3800 defer; Appendix A/B not adjudicated; coords/other-owner/7220
> out; TMT-None corner; TY2026 re-pin). NEXT: the Scenario-13 mapper (IRS6251 + IRS8911/SchA
> docs + `scenario13.py`). Detail → STATUS.md + RS session_log.

> **2026-07-02 -- MeF ATS Scenario 1040-5 (Bobby Barker) through the REAL engine + 7 new e-file
> mappers + TWO spec-first engine fixes -- ★★ UNIT COMPLETE -- flow gate 365→366.**
> The refundable-credit-stack scenario (HOH + blind, 2 QC, EIC-after-disallowance/8862, 2441 ×2
> providers, 8863 AOTC-on-self, Sch 8812 with the 2025 line-28 ACTC OPT-OUT, Sch 1 moving
> expenses): `ats/scenario5.py` + `mef_build_ats_scenario5` (rollback default), **22 hand-computed
> 1040 lines match to the dollar** (EIC 5,494 lower-of lookup; line 28 = 0; refundable AOTC 392;
> 25d = 1,754 EXACT). **Fix 1 -- SCH_8812 ACTC opt-out** (RS `c2b778b`, tts `83071ea`): fact
> `actc_opt_out` + R017 `AND NOT actc_opt_out` (Part II-A skipped entirely, CTC/ODC line 19 never
> touched), D_8812_014/D014 info, TS19, FA-1040-CTC-13 (conditional_zero, loader-homed); Taxpayer
> mig 0147 also adds pecf_primary/pecf_spouse/us_home_more_than_half_year/
> mfs_hoh_lived_apart_last_6_months (spine facts; PECF new in RS, other two already specced);
> input = 8812-tab election card + Taxpayer-Info header block; render = five f1040 widgets
> label-verified (c1_5/c1_6/c1_7/c1_32/c2_14). **Fix 2 -- R-8959-ENGAGE = the i8959 Who-Must-File
> conditions** (RS `22f29be`, tts `e25e8d3`): the scenario's box-6 whole-dollar rounding (453 vs
> 452.86) put $0.14 on line 25c via the code's spec-less "line 22 > 0" arm -- REMOVED; the missed
> bullet-1 arm ADDED (any SINGLE W-2 box 5 > $200k FLAT -- one 210k W-2 on an MFJ return must
> file to recover the $90 withheld); `max_single_w2_box5` shared by compute + all three D_8959_*
> (bridge-gate); D_8959_003 engage-gated; scenarios 8959-T6/T7 HAND-COMPUTED; verbatim
> Who-Must-File excerpt on NEW source IRS_2025_8959_INSTR; integrity gate ALL PASS; zero
> sibling-spec drift. **MAPPER**: read_model + builder grew IRS1040Schedule1/Schedule3/
> Schedule8812 (Part II-A omitted under the election; required line-12 Ind emitted)/ScheduleEIC/
> IRS2441 (provider+person groups)/IRS8862 (per-child 365-day + per-student answers derived from
> the SAME model facts the credits computed from)/IRS8863 (per-student 27-30 via
> compute_8863.aotc_student_lines -- new shared helper); IRS1040 gained PECF/MainHome/
> DependentDetail grid/SepdSps/12a-12d block (via std_deduction_checkboxes -- bridge-gate)/
> DoNotClaimACTCInd; IRSW2 boxes 3-6. All NINE documents validate vs the 2025v5.4 XSDs in
> ReturnData1040 sequence order. Stale scaffold pin repointed (ack parser built 6-29).
> Boundaries -> DEFERRAL_AUDIT 2026-07-02 (EIC opt-out unbuilt, months-vs-days granularity,
> business rules still Ken-pull). Detail -> STATUS.md + RS `session_log.md` + `.claude`
> [[mef-efile-kickoff]].

> **2026-07-05 -- GA-500 Schedule 1 (Adjustments to Income) RENDER LEG -- ★★ COMPLETE (9 pure + 4 DB render tests passed in 2:47) -- flow gate 398 held.**
> Closes the DEFERRAL_AUDIT "Schedule 1 DETAIL page never coordinate-mapped" gap. The S1-*
> adjustment lines previously reached the printed return only via the Form-500 line-9 net; now
> the full Schedule 1 renders. Template WASN'T in the repo (`ga500.pdf` = the 5-page main return
> only); downloaded the GA-DOR blank-form-printing packet (27 pages), extracted Schedule 1 =
> source pages 5-7 (p1 Adjustments grid Additions 1-6 / Subtractions 7-14, p2 Retirement Income
> Exclusion worksheet lines 1-17 dual-column, p3 Military Retirement Income Exclusion worksheet)
> → `resources/state_forms/GA/2025/ga500_schedule1.pdf` (flat, registered manifest
> `fga500_schedule1` / `GA-500-SCH1`). Ken ruled (AskUserQuestion): **all 3 pages** + **tips/OT
> fold into printed line 12** (2025 form has no 12a/12b cell → render L12 = S1-12+S1-12a+S1-12b so
> the column foots to L13; exact on the TY2025 bed where tips/OT = 0). Build: NEW
> `coordinates/fga500_schedule1.py` (namespaced P1-*/P2-{TP,SP}-*/P3-{TP,SP}-* + SSN comb) +
> `render_ga500_schedule1()` (maps FFV → keys; 7a←RIE-TP-17, 7b←MIL-TP-3+8, 7d/7e spouse;
> attach only when adjustments exist; p2 only if RIE applies, p3 only if military). Pre-printed
> constants (p2 L4 $5k, p3 L2 $17.5k/L7 $35k) intentionally unmapped. Baseline recipe =
> box-bottom rule + 2pt (get_drawings "re"; the ".00" text anchors skew ~8-10pt low). No compute
> change in the render leg (flow 398 unchanged). ⚠ **Surfaced + then FIXED (Ken's go-ahead) a
> military over-exclusion COMPUTE bug:** `compute_ga500.mil()` was `l3+l8`, over-excluding military
> retirement of $17,501–$34,999; now `min(mret, 35000)` per IT-511 (renderer 7b/7e updated to match;
> T5 re-pinned + NEW `T5b-military-midrange`; 19 pure scenarios + flow 398 green). RS-side spec
> `R-GA500-MIL` + `check_ga500_integrity.py` reconciliation PENDING (`docs/rs_handoff/2026-07-05_…`).
> Boundaries → DEFERRAL_AUDIT 2026-07-05. Detail → `.claude` [[ga500-schedule1-render-leg]] +
> [[ga500-military-exclusion-overexclusion-bug]].

> **2026-07-02 -- GA-500 HB 463 tips/overtime exclusions (§48-7-27(a)(16)/(17)) -- ★★ UNIT COMPLETE (DB-verified, GA suite 48 passed in 9:20) -- tag `ga500-hb463-tips-ot-complete` -- SPEC-FIRST RS round-trip -- flow gate 363→365.**
> Found during the same-day HB 463/AP statute verification: the enacted Act (signed 2026-05-11,
> legis.ga.gov doc 249080) created TY2026-2028 exclusions for cash tips + qualified overtime
> (≤$1,750 each) with NO compute_ga500 handling — an hourly worker's TY2026 GA return overstated
> tax. Ken chose (AskUserQuestion) full unit / **PER-TAXPAYER cap** (MFJ = each spouse separately,
> the "received by" reading — re-verify at the 2026 IT-511, W7) / OT **assertion+auto-feed**
> (unasserted FT-hourly → NO exclusion, penalty-safe + D_GA500_013 nudge) / dedicated
> **S1-12a/12b** sub-lines. RS `load_ga500_form_500.py`: +6 facts, +R-GA500-TIPS/-OT (window-gated;
> bases = RAW federal qualified amounts — **GA has NO phaseout**, never the post-phaseout federal
> deduction; OT = W-2 employee comp ONLY, never 1099), R-GA500-L9-S1 amended (subs L7-L12b),
> +D_GA500_013/014/015, +scenarios T15-T18 (HAND-COMPUTED: per-taxpayer caps 3,500/2,750 → 4,179;
> unasserted-OT → 2,246; TY2025 year-gate → 2,491; under-cap passthrough → 1,672), +FA-GA500-13/14,
> and the **HB 463 authority excerpt CORRECTED to verbatim /AP text** (the prior summary excerpt
> wrongly dated the std-ded increase 2027; the seed deletes the stale row). `check_ga500_integrity`
> re-types the window/caps independently — ALL PASS (18 scenarios). Seeded RS Supabase
> (83f/23r/91L/15d/18t/14FA) → canonical `500_spec.json` re-exported (id-diff: only intended
> changes). tts: seed_ga500 +8 lines (bases + FT-hourly checkboxes + computed 12a/12b; 144→152,
> shared DB re-seeded); compute_ga500 `_tips_ot_cap` EXPLICIT window (TY2029+ = 0, never a
> latest-year fallback) + S1-13 sum; federal pull in `_populate_ga500_from_federal` (per-owner W-2
> box 7 gated on the federal tipped-occupation attestation + other-tips field; OT = the W-2 field
> only; window-gated so D_GA500_015 can't misfire on a pulled value); rules_ga500 +3 (seed_rules
> run); FA merged into the gate file (341→343 assertions). INPUT = the seeded lines on the existing
> Sch 1 tab (StandardSection — zero client change; tsc 0 / vitest 275 untouched). RENDER = correct
> via the line-9 net on the Form 500 face (the Schedule 1 DETAIL page has never been
> coordinate-mapped — pre-existing whole-page gap, stated). GATES: flow **365** (incl. FA-13/14),
> 18 pure scenarios, **DB 48 passed in 9:20** (seed 4 / compute 5 / diagnostics 17 / state-return 5
> incl. both pull tests / + the rest of the GA suite). Boundaries → DEFERRAL_AUDIT 2026-07-02.
> Detail → STATUS.md + RS `session_log.md` + `.claude` [[ga500-kickoff]].

> **2026-07-01 -- Form 2210 DATED federal payments (§6654 due-date→date-paid accrual) -- ★★ UNIT COMPLETE (DB-verified, 6 pipeline passed in 2:24) -- tag `1040-2210-dated-payments-complete` -- SPEC-FIRST RS round-trip -- flow gate 362→363.**
> Ken chose scope option 1 in-session ("build as designed", `server/specs/2210_dated_penalty_design.md`).
> RS FORM_2210 amended (RS `35413f5`/`9f94b7b`/`0b72b6f`): R-2210-REG reworded to the rule its own
> authority excerpt already stated — every payment applies to the EARLIEST still-underpaid installment
> and accrues **from the installment due date to min(date cured, 4/15/2026)** (7%→6% at 3/31/2026);
> withholding stays ¼-spread ON the due dates (§6654(g) default; actual-date election deferred). NEW
> fact `t2210_payments_dated`, diag `D_2210_DATED` (dated-vs-line-26 reconciliation warning), scenarios
> **P-T7 (mid-year lump = 217)** / **P-T8 (Q4 paid 1/25 = 5)** / P-G2 — all HAND-COMPUTED; P-T1..T6
> pinned unchanged (due-date payments reproduce the fixed-day numbers exactly); FA-02/03 reworded +
> **FA-1040-2210-07** (merged SURGICALLY into the gate file — the deployed RS FA export is missing 21
> accumulated assertions; drift → REVIEW_QUEUE). `check_2210_integrity` re-types the dated algorithm
> independently, ALL PASS. ⚠️ KEY LAW SHAPE: the form has TWO allocations — FACE line 25 nets payments
> per column DATE WINDOW (+overpay carry); the PENALTY worksheet applies earliest-first — never blend
> them (FA-03 caught exactly that). MODEL `FederalEstimatedPayment` (migs 0145 + RLS 0146, APPLIED;
> mirrors StateIncomeTaxPayment, no state_code; creditable kinds = estimate + prior_year_applied,
> PY-applied blank-dated → 4/15 per i2210; serializer requires date or quarter). COMPUTE
> `days_at_rates` + the unified `regular_penalty` (dated rows REPLACE the flat buckets; empty → the
> pre-amendment fallback); `federal_dated_payments`/`-reconciliation` = the single source for compute
> AND diagnostics (bridge-gate). INPUT serializer + `federal-estimated-payments` CRUD
> (recompute-on-mutation) + React `FederalEstimatedPaymentsSection` dated grid on the form_2210 tab.
> RENDER verified unchanged (face 4/5/7/9/19). GATES: flow **363** (incl. FA-07), 389 pure (incl. 16
> 2210 + memo 10); **DB 6 pipeline passed** (incl. dated-late-Q4 = 4.00 → line 38 + the divergence
> warning); tsc 0 / vitest 248; check clean. 1040 line 26 stays flat (spine scope) — boundaries →
> DEFERRAL_AUDIT 2026-07-01. Detail → STATUS_ARCHIVE + DECISIONS.md + `.claude`
> [[form-2210-penalty-due-date-to-date-paid]].

> **2026-07-01 -- 1065 Schedule K / K-1 line 14a SE classification (§1402(a)(13)) -- ★★ UNIT COMPLETE (RECOVERED from an uncommitted dead session, verified, finished) -- tag `1065-se-line14a-complete` -- SPEC-FIRST RS round-trip -- 1040 flow gate unchanged 362.**
> The FIRST RS-specced core-1065 piece. Ken locked the four tax-law calls 2026-07-01
> (`server/specs/1065_se_line14a_spec.md`): (1) undetermined → active/INCLUDE safety net; (2) LLC members
> ride the same active/passive functional-analysis axis as limited partners (Renkemeyer/Soroban — the
> state-law label is not the test; contra-Sirius binds 5th Cir. only, GA/11th unresolved); (3) passive
> capital GP excluded (§1402(a)(13) carve-back is services-only); (4) active capital GP included
> (Reg §1.1402(a)-1(b)). RS form `1065_SE`: 9 rules / 3 diagnostics / 10 tests / 7 authorities (CFR/USC
> quoted VERBATIM; case-law group requires_human_review, re-verify each season) / RS FA 338→341
> (RECON-14A + INV-CHAR active, FLOW-14A-SE disabled=future); canonical export
> `server/specs/1065_se_spec.json`. MODEL `Partner.se_classification` + `se_classification_basis`
> (mig 0144, APPLIED). COMPUTE `resolve_se_classification` + rewritten `compute_self_employment`
> (replaces the silent limited-exclude / LLC-full-include); NEW `compute_1065_se.py` — entity K14a
> derived BOTTOM-UP = Σ per-partner box 14a (replaces `K14a = K1 + K4c`; override kept, D_SE_RECON owns
> the break). DIAGNOSTICS D_SE_UNDET / D_SE_GPCHAR / D_SE_RECON (runner-registered, seeded + active in
> the shared DB; `seed_1065_se` validates spec shape + ensures the K14a line). INPUT PartnersSection
> SE-classification select (general = fixed-active read-only, undetermined hint) + basis note. TESTS
> spec-DRIVEN `test_1065_se_compute_leg.py` (T1–T10 read from the export + INV-CHAR noise + off-form
> no-op + shape pins) + `test_k1_allocator.py` rewritten per spec §11 = **47 passed**; tsc 0 /
> vitest 248. Boundaries (stated → DEFERRAL_AUDIT): SE-base sub-spec (4797 Part II backout + rental
> 1b/1c, coupled w/ 4797 §1245/§1250 verify); 14b/14c; non-individual guard; no 1065 FA gate file yet;
> 1065 K-1 PDF render still stub; no DB pipeline test. Detail → STATUS_ARCHIVE + DECISIONS.md +
> `.claude` [[1065-se-line14a-unit]].

> **2026-07-01 -- State withholding → Schedule A line 5a (auto-total state income tax) -- ★★ UNIT COMPLETE (DB-verified, 28 passed) -- tag `1040-schedule-a-state-5a-complete` -- SPEC-FIRST RS round-trip -- flow gate 360→362.**
> Ken's UX/bug queue ("state withholding → Schedule A"). Ken chose (AskUserQuestion) the FULL scope
> (withholding + estimates + prior-year taxes + prior-year extensions, WITH payment dates) + full RS
> round-trip; sequencing = state now, federal 2210 dates next. Line 5a was a single hand-keyed fact; it
> now AUTO-TOTALS the return's state/local income tax = WITHHELD (documents) + dated payments (YELLOW),
> preparer overrides GREEN, suppressed when general sales taxes are elected. §164 cash-basis date filter
> (a Q4 estimate paid in January lands on next year's 5a). RS `load_1040_schedule_a.py` +4 facts, +rule
> `R-SCHA-5A-STATE` (precedence 2, before SALT — feeds 5d), +2 diags (`D_SCHA_010` transparency /
> `D_SCHA_011` out-of-year), +5 scenarios (T12–T16), +2 FA, +1 authority (`IRS_2025_SCHA_INSTR`, line-5a
> text quoted VERBATIM); `check_schedule_a_integrity` extended + ALL PASS; seeded RS Supabase
> (FlowAssertions 336→338); re-exported `schedule_a_spec.json` (zero content drift). MODEL
> `StateIncomeTaxPayment` (per-return list; kind/amount/date_paid/quarter/tax_year_for/state_code/memo;
> reusable for GA-500 + federal 2210 UET; migs 0142 + RLS 0143, APPLIED). COMPUTE `state_income_tax_withheld`
> (W-2 box17 via `W2StateEntry` rows, else the legacy flat field when no state rows — mig-0084 backfill
> double-count guard; + 1099-R/INT/DIV/G/W-2G) + `state_tax_payments_summary` (dated, in-year) → 5a routing;
> engages on entered payments, NOT on withholding alone. DIAGNOSTICS `D_SCHA_010/011` (re-derive via the
> compute helpers). INPUT serializer + `state-income-tax-payments` CRUD (recompute-on-mutation) + React
> `StateTaxPaymentsSection` (dated grid, in-year total footer, out-of-year amber) + line 5a YELLOW auto-total
> display. Gates: flow **362**, client tsc 0 / vitest 248, 18 pure Sch A GREEN, `manage.py check` clean.
> ✅ **DB-VERIFIED 2026-07-01: `tests/test_schedule_a_compute_leg.py` 28 passed in 13:46** (6 `test_pipeline_5a_*`
> — autofill / override GREEN / sales-tax suppress / out-of-year + D_SCHA_011 / withholding-alone no-engage /
> W-2 no-double-count — + 4 pre-existing pipeline + 18 pure incl. T12–T16). Ran venv-python direct with
> `--timeout=1800 --reuse-db` after dropping a leftover `test_postgres`. **Tagged `1040-schedule-a-state-5a-complete`.**
> Detail → STATUS.md + DECISIONS.md + `.claude` [[schedule-a-5a-state-withholding]] / [[form-2210-penalty-due-date-to-date-paid]].

> **2026-07-01 -- Roth IRA basis tracker (year-over-year, feeds Form 8606 line 22/24) ★★ UNIT COMPLETE -- tag `1040-roth-basis-tracker-complete` -- SPEC-FIRST RS round-trip -- flow gate 359→360.**
> Ken's UX/bug queue ("Roth basis tracker — year-over-year Roth contribution basis; 8606 Part III only
> captures at distribution"). Ken chose (AskUserQuestion) contribution+conversion cumulative scope + the
> full RS round-trip. NEW per-owner `RothIRABasis` model (`contribution_basis`→line 22, `conversion_basis`
> →line 24, `current_year_contributions` for the roll; UniqueConstraint tax_return+owner; migs 0140 + RLS
> 0141, APPLIED). COMPUTE: `_resolve_roth_basis` sources line 22/24 from the tracker (YELLOW) unless the
> Form8606 field is a non-zero override (GREEN); `owner_lines` gained the overrides + emits l22/l24; both
> `compute_8606_db` AND `render_8606` call it (render previously read the doc field directly — would have
> hidden the tracker value on the PDF). DIAGNOSTICS: new `D_8606_ROTHNOBASIS` (warning, tracker-aware) +
> `d_8606_part3` made tracker-aware. INPUT: `RothIRABasisSerializer` + `roth-ira-bases` CRUD (recompute-on-
> mutation, one-per-owner guard) + `RothIRABasisSection` folded into the form_8606 tab + YELLOW carried-hint
> under line 22/24. PROFORMA: `_roth_ira_bases` read side (roll recovers contribution basis first). RS:
> `load_1040_8606.py` (RS `be680d4`) +fact `f8606_roth_cy_contributions` +rule `R-8606-ROTHTRACK` +diag
> +scenario F-G2 +FA-1040-8606-07; `check_8606_integrity` ALL PASS; §408A(d)(4) math unchanged; seeded RS
> Supabase → re-exported `8606_spec.json` (zero drift on existing). Gates: flow **360**, client tsc 0 /
> vitest 248, 14 pure 8606 tests GREEN; DB pipeline/diagnostics tests run separately. Detail → STATUS.md +
> DECISIONS.md + `.claude` [[roth-basis-tracker]].

> **2026-07-01 -- Form 1116 §904(j) de minimis FTC AUTO-path + opt-out -- SPEC-FIRST RS round-trip -- flow gate unchanged 359.**
> Ken's UX/bug queue, next item ("foreign-tax de minimis §904(j)"). The §904(j) election now applies
> AUTOMATICALLY: when the only foreign tax is from 1099-INT/1099-DIV (passive + payee-statement BY DEFINITION),
> total ≤ $300/$600, and NO full Form 1116 is engaged, the credit lands on Schedule 3 line 1 as `min(foreign
> tax, regular tax)` (YELLOW), no Form 1116 face, no carryover. Preparers no longer have to open the FTC tab
> and tick the box for the common retiree-with-an-international-fund case. Law re-verified verbatim vs i1116 +
> 26 USC §904(j) (matches the already-authored spec). Ken chose (AskUserQuestion) auto-apply + opt-out AND the
> full RS round-trip. **RS** `load_1040_form_1116.py`: +fact `f1116_deminimis_optout`, reworded `R-1116-ELECT`
> (auto) + `D_1116_001` (auto-applied), +`D_1116_009` (warning, over-ceiling-no-form nudge — closes a silent
> gap); `check_1116_integrity.py` ALL PASS (math unchanged, facts 19→20 / diags 8→9); seeded RS Supabase →
> re-exported canonical `1116_spec.json` (semantic diff = zero drift). **tts**: COMPUTE helper
> `_deminimis_election_credit` + auto-path in `compute_1116_db` (`form is None`); `compute_1116_result` returns
> the auto dict (render+diagnostics agree). INPUT `Taxpayer.ftc_deminimis_optout` (mig 0139, APPLIED) +
> serializer + opt-out card on `Form1116Section`. DIAGNOSTICS `rules_1116` D_1116_001 auto + D_1116_009 +
> registry synced (9 rules). RENDER unchanged (render_1116 already None for path != full). Opt-out lives on
> Taxpayer NOT Form1116 (the auto-path must work with no form engaged); an engaged Form 1116 stays in the manual
> flow untouched. Gates: flow **359**, client tsc 0 / vitest 248, 12 pure 1116 compute GREEN. +6 new DB tests
> (4 pipeline: auto/MFJ/opt-out/over-ceiling + 2 diagnostics). Detail → STATUS.md + DECISIONS.md + `.claude`
> [[ftc-904j-deminimis-auto-path]].

> **2026-06-30 -- Default-to-No due-diligence questions (digital assets + Sch B 7a/8) -- SPEC-FIRST RS round-trip -- flow gate unchanged 359.**
> Ken's UX/bug batch. The digital-asset header question + Schedule B Part III foreign questions (7a foreign
> account, 8 foreign trust) now DEFAULT to No: the UI pre-selects No shown YELLOW (unconfirmed default; the
> stored field stays null), the face PRINTS No, and the diagnostic drops to INFO ("confirm before filing").
> Explicit Yes/No → GREEN, INFO clears. Because these diagnostics are SPEC-GOVERNED (`D_1040_017` in
> `1040_spine_spec.json`, `D_SCHB_001` in `sch_b_spec.json`, both "implemented strictly from the spec"), CC
> flagged it and Ken chose the proper RS round-trip: amended `load_1040_spine.py` (D_1040_017 warning→info) +
> `load_1040_intdiv_qdcgt.py` (D_SCHB_001 error→info, condition widened to 7a OR line 8 — closing a
> pre-existing spec-vs-code gap where the code only checked 7a; R-SB-05 desc updated), both RS integrity gates
> GREEN, seeded RS Supabase, re-exported both specs to tts (semantic diff = zero drift). RS commit `5d3aac0`.
> tts: render (renderer.py null→No for both), UI (TaxpayerInfoSection select + InterestIncomeSection SchBAnswer,
> default-No YELLOW), rules_1040/rules_schb synced, 4 tests updated. Gates: flow 359, tsc 0 / vitest 248,
> **digital-assets DB 3-passed + Sch B DB 5-passed (confirmed)**. tts `b519377` + fix `cb4a7ad` (the D_SCHB_001
> RULES_SCHB registry severity was left stale at "error" while the function returned info — the clean re-run
> caught it). tts DiagnosticRule rows re-seed on deploy. NOTE: MeF still requires an explicit digital-asset
> answer (stricter e-file posture — intentionally left; e-file track).

> **2026-06-30 -- UX/bug batch: Sch D "V"-for-various + legacy itemizing box removal -- flow gate unchanged 359.**
> Two of Ken's queued UX items. (1) **Sch D "V" shorthand** (`d97d8b1`, frontend-only): Form 8949 date columns
> (b)/(c) now expand "V"→VARIOUS and "I"/"INH"→INHERITED on blur (TaxWise convention). The Sch D model is
> `CapitalTransaction` (free-text CharField dates; the *box* drives ST/LT) — already fully VARIOUS/INHERITED-aware
> across render + the D_8949 diagnostics; only the typing convenience was missing. New pure `lib/capital-tx-date.ts`
> + `dateText` helper. (Corrects the STATUS note that assumed the Disposition `_various` booleans — that's Form 4797,
> which already had its checkbox.) (2) **Legacy itemizing box — FULL CLEAN REMOVAL** (Ken's call): the manual
> Line-12 `elif tp.itemizing` path was redundant (override box + real Schedule A cover it) and a footgun (no
> larger-of-standard guard). Removed the compute.py branch + the `TaxpayerInfoSection` checkbox (→ Schedule A hint)
> + the dead `d_1040_011` branch; **redirected AMT** (`compute_6251`) to the real Line-12==SchA-17 signal, closing
> the `FLAGGED` heuristic (mirrors `compute_section_68_db`). Model columns + serializer kept (additive-only). Gates:
> flow **359**, manage.py check clean, client tsc 0 / vitest 248, **33 DB tests** confirmed (`test_legacy_itemizing_*`
> + `test_011_*` + 30 AMT/§68 no-regression). Detail → STATUS.md.

> **2026-06-30 -- Schedule D line 4/11 face-cell fix (render-only) -- flow gate unchanged 359.**
> Closed the REVIEW_QUEUE 2026-06-29 finding: Sch D lines 4/11 only surfaced the Form 4797 feed at
> render time, not Form 6252 / Form 8824 ST/LT gains, so those amounts printed blank (tax was always
> correct — line 16 → 1040 7a unaffected). RS `SCHEDULE_D` spec fetched first (line_map 4/11 confirmed
> the 6252/8824 feeds belong there). New `schedule_d_face_line()` in `compute_schedule_d.py` — single
> source of truth for the renderer + tests, reusing the existing `form_6252_sch_d_st/lt` /
> `form_8824_sch_d_st/lt` feed functions already used in the netting-input computation. Repointed the
> 4 `test_compute_8824.py` pipeline tests off the un-persisted FormFieldValue row. ⚠️ Those 4 DB tests
> remain UN-CONFIRMED this session — Supabase pooler hung on `test_postgres` creation 3x (cleared via
> `scripts/drop_test_db.py`); re-run next session. Detail → REVIEW_QUEUE.md (Resolved) / STATUS.md.

> **2026-06-30 -- SSA/RRB WITHHOLDING + MEDICARE (Sch A / SEHI) + RAILROAD ★★ UNIT COMPLETE -- tag `1040-ssa-medicare-railroad-complete` -- flow gate 357→359.**
> Ken's Bach-return item 1 (spec-first). RS `load_1040_retirement` amended + seeded (RS Supabase `ylqaejdqwuvwpglxnpgv`):
> +4 facts (`ssa_fed_withheld`→25b, `ssa_medicare_premiums`, `ssa_medicare_destination` schedule_a|sehi, `ssa_is_railroad`
> RRB-1099 SSEB metadata), +2 rules (`R-RET-25B-SS` extends the 25b roster; `R-RET-MEDICARE` → Sch A L1 (Pub 502) OR
> single-proprietor SEHI capped at net SE profit (Form 7206)), +`D_RET_009` RED (Medicare-as-SEHI w/ multiple businesses
> or Marketplace/PTC → manual), +2 scenarios, +`FA-1040-RET-09/10`, +2 authority sources (Pub 502 + Form 7206 instr,
> both verbatim-verified vs the live IRS text — a WebFetch summary had WRONGLY said Medicare isn't deductible).
> `check_retirement_integrity` ALL PASS; canonical `retirement_spec.json` re-exported (39/13/9). tts: Taxpayer +7 fields
> (mig 0131, applied) + serializer; COMPUTE 25b WH (`compute_retirement.py`) / Medicare→Sch A (`compute_schedule_a.py`)
> / Medicare→SEHI folded BEFORE the QBI loop so it also reduces QBI (`compute_schedule_c.py`); DIAGNOSTIC `D_RET_009`
> (routed to social_security tab); RENDER via existing 25b/Sch A maps (`ssa_is_railroad` = label metadata); UI card in
> `SocialSecuritySection` (taxpayer+spouse inputs, Medicare destination select, RRB checkboxes). TESTS 4 pure Sch A
> (`test_ssa_medicare_sehi.py`) + 2 FA. ⚠️ DB pipeline tests (25b/SEHI cap/D_RET_009 firing) deferred (pooler) — covered
> by FA source-checks + integrity. Client gate tsc 0 / vitest 223; manage.py check clean. Detail → STATUS.md + RS session_log.
>
> **2026-06-30 -- Bach UX batch (items 2,3,4,5,6,7,8)** — frontend: #8 pennies (`wholeDollars`), #2 1099-R EIN autofill
> (yellow), #4 Summary two-column + AGI strict-`form_code` fix, #5 Sch C/E/F depreciation links, #7 gambling losses
> single-entry→Sch A, #3 GA "Add a state return" picker, #6 draft-row pre-load (1099-R + 1099-G; W-2 deferred). tsc 0 /
> vitest 223. Detail → STATUS.md.

> **2026-06-29 -- SS LUMP-SUM ELECTION (Pub 915 Worksheets 2+4) ★★ UNIT COMPLETE -- tag `1040-ss-lumpsum-complete` -- all 6 legs + RS seed + DB tests; flow gate 357.**
> Retiree-hardening cluster item 1 (Ken-directed; D_RET_004 first + EXPLICIT-TOGGLE election). LAW verified verbatim
> vs Pub 915 (2025); Terry example reconciles to the dollar (election 2500 < regular 3000). Legs 1-5 prior session
> (SPEC/MODEL/COMPUTE/DIAGNOSTICS/FLOW; tts `3626f1e`..`be9c544`, RS `bcd6c40`). **This session (`bf3c60a`..`1f83aed`):**
> RS SEED (Ken go-ahead) — seeded RS's own Supabase via `load_1040_retirement`, re-exported canonical
> `retirement_spec.json` (lse_* present), re-seeded tts `1040_RETIREMENT` 24→26 lines (+lse_ws2_21/lse_ws4_21 in the
> hardcoded `seed_1040_retirement.py`). RENDER — box 6c = `c1_41[0]` (f1040 HEADER_MAP); `_build_header_data` checks
> it only when `compute_lump_sum_db().applied`; `render_ss_worksheet_statement` appends a WS2/WS4 section. INPUT —
> `SocialSecurityLumpSumSerializer` + `ss-lump-sums` CRUD (recompute-on-mutation) + React
> `SocialSecurityLumpSumSection.tsx` + new `ss_lump_sum` Income tab + RULE_TAB_MAP D_RET_004/008 → ss_lump_sum;
> removed the stale "compute manually" toggle. Client gate tsc 0 / vitest 219 / build clean. DB TESTS —
> `test_ss_lumpsum_pipeline.py` 7 django_db (light seed) on Terry, ALL GREEN (271s). Cluster items 2-3 (D_RET_005,
> D_8606_NO_YEAREND) still open. Detail → STATUS.md + memory.
>
> **2026-06-29 -- ⚠️ NEW CROSS-UNIT FINDING (flagged to Ken, NOT fixed): Schedule D line 11 face/test gap.** The
> long-pooler-blocked 8824 pipeline tests RAN this session; 4 of 5 FAIL because Sch D lines 4/5/11/12 FormFieldValue
> rows are never written by compute (netting INPUTS only) — the cross-form gains show only at RENDER (`dbeef2d`
> `_direct_plus_k1`) and only the 4797 feed (not 6252/8824 LT). Tax correct (line 16→7a); display/test-layer only.
> Detail → DEFERRAL_AUDIT.md + REVIEW_QUEUE.md.

> **2026-06-28 -- Form 8824 (Like-Kind Exchanges §1031 + §1043) ★★ UNIT COMPLETE -- tag `1040-form-8824-complete` -- all 6 legs green; flow gate 347→356.**
> Ken chose "Full incl. Part IV + computed recapture" 2026-06-28. **Closed the last Form 4797 cross-form
> RED-defer `D_4797_004`** (like-kind). SPEC + COMPUTE + RENDER + DIAGNOSTICS + ASSERTIONS (prior session) +
> **INPUT (this session)**. INPUT: `LikeKindExchangeSerializer` (GREEN inputs only, 34 fields) + `like_kind_exchanges`
> / `like_kind_exchange_detail` CRUD `@action`s + return-serializer/prefetch wiring — mirrors `installment_sales`
> exactly. React `LikeKindExchangeSection.tsx` (rows-CRUD, the `InstallmentSalesSection` precedent) +
> `like_kind_exchanges` tab in IndividualNav's **Income** group + `D_8824_`→tab in RULE_TAB_MAP + has-data
> predicate. Client gate: vitest 217/217, vite build clean, **tsc 0 errors** (the old ~84-err baseline is now
> clean; also fixed the nav test's `emptyRd` which was missing `installment_sales`). COMPUTE: `LikeKindExchange`
> model (1 row/exchange, migs 0126/0127) + `compute_8824.py` (no-cache) wired BEFORE `compute_4797_db`; §1231→4797
> L5→Sch D L11 / ordinary→4797 L16→Sch 1 L4 / capital→Sch D / §1043→4797 L10; §1.1031(d)-2 liability netting +
> §1245(b)(4)/§1250(d)(4) recapture + 25a/b/c basis. RENDER: manifest `f8824` + `field_maps/f8824_2025.py` (63
> fields) + `render_8824_1040` after Form 4797. DIAGNOSTICS: `rules_8824.py` 10 `D_8824_*` (RED-defers:
> multi-asset 007 / main-home 008 / personal-property 009). ASSERTIONS: 9 `FA-1040-8824-*` + `_run_8824_assertion`
> → gate 356; FA-09 verifies all 10 diag codes registered. VERIFIED (everything runnable GREEN): 12 pure compute
> + 8 render + flow gate 356 + `manage.py check` + both RS integrity gates + full client gate. ⚠️ The 6 DB
> pipeline tests (5 `test_compute_8824.py` + 4797 `test_diag_002_red_defers`) remain **blocked by the documented
> intermittent Supabase pooler outage** ("terminating connection due to administrator command" mid-seed) — re-run
> next session; logic proven independently. Commits `7d14f69`..`c1350c2`. RS hygiene: re-seed `load_4797` on next
> RS deploy (covers both 6252 + 8824). Detail → STATUS.md / 8824-kickoff memory.

> **2026-06-28 -- Form 6252 (Installment Sale Income) ★★ UNIT COMPLETE -- tag `1040-form-6252-complete` -- all 6 legs green; flow gate 339→347.**
> Ken chose Broad v1 (Parts I-III + §453A) 2026-06-28. SPEC + COMPUTE (prior session) + RENDER + DIAGNOSTICS +
> INPUT + ASSERTIONS (this session). COMPUTE: `InstallmentSale` model (1 row/sale, migs 0124/0125) +
> `compute_6252.py` (pure no-cache + `_db`→Sch 2 L15 §453A + `_face` + feeds) wired BEFORE `compute_4797_db`;
> **fully retired the 4797↔6252 RED-defer** (`D_4797_003` deactivated). RENDER: `render_6252_1040` (compute-driven
> face, one Form 6252/sale via fitz) + f6252 PDF trimmed-to-form + `f6252_2025.py` (49/49). DIAGNOSTICS:
> `rules_6252.py` 8 `D_6252_*` (v1 limit: no §1245-vs-§1250 flag → D_6252_007 heuristic). INPUT:
> `InstallmentSaleSerializer` + CRUD + `InstallmentSalesSection.tsx` + `installment_sales` tab (Income group) +
> removed vestigial `f4797_has_form_6252`; vite build + vitest 217/217. ASSERTIONS: 8 `FA-1040-6252-*` +
> `_run_6252_assertion` → gate 347. VERIFIED: 22/22 `test_compute_6252.py` + 9/9 diag + flow gate 347. Commits
> `0cec206`..`4c1b06b`. RESUME = next unit Form 8824 (§1031). Detail → STATUS.md.
> RENDER leg (2026-06-28): `render_6252_1040` (`renderer.py`) compute-driven face — reads `compute_6252_face`,
> ONE Form 6252 per non-deferred InstallmentSale row, concatenated via fitz (the `render_5329` multi-instance
> precedent; `render_4797_1040`/`render_1116` single-source discipline). f6252 PDF TRIMMED to the form-only page
> (irs.gov bundles 3 instruction pages) + manifest entry (SHA256 of trimmed) + field map `f6252_2025.py` (49/49
> AcroForm fields; line 19 GP% prints as a decimal). `f6252` in `ACROFORM_FORM_IDS`, wired after Form 4797.
> VERIFIED: 22/22 `test_compute_6252.py` (incl. 4 new render), flow gate 339 GREEN.
> Ken chose Broad v1 (Parts I-III + §453A) 2026-06-28. SPEC done prior session (RS, `form_6252_spec.json`).
> COMPUTE leg this session: NEW `InstallmentSale` model (one row/sale, proforma-rollable; migs 0124 + RLS 0125)
> + `compute_6252.py` (pure port of `load_6252.compute_6252`, NO cache, + `_db`/`_face` + cross-form feeds)
> wired into `compute_return` BEFORE `compute_4797_db`. **Closed the Form 4797↔6252 RED-defer** (Ken: "fully
> retire the flag"): `compute_4797` now consumes the 6252 feeds (line 4 §1231 / line 10 ordinary / line 15
> recapture) instead of deferring; `D_4797_003` retired (is_active=False stub); RS `load_4797.py` synced +
> `check_4797_integrity` ALL PASS (11 scenarios); F4797-G2 scenario removed; FA-1040-4797-07 dropped the 6252
> blocker. Routing: capital → Sch D (ST L4 / LT L11); business §1231 → 4797 L4 → Sch D L11; ordinary recapture
> → 4797 L15 → Sch 1 L4; unrecap §1250 → Sch D 25% WS; §453A interest → **Schedule 2 line 15**. Tests
> `test_compute_6252.py`. VERIFIED: 20/20 pure scenarios (6252+4797), RS integrity ALL PASS, **flow gate 339
> GREEN**, SCHD+4797 FAs 19/19, pipeline-t4 closure GREEN (328s under pooler latency). §453A: §6621 rate
> preparer-asserted/row; max ordinary/LTCG year-keyed (2025 37%/20%). Detail → STATUS.md.

> **2026-06-27 -- Form 4797 (Sales of Business Property) ★★ UNIT COMPLETE -- tag `1040-form-4797-complete` -- all legs green; flow gate 332→339.**
> Ken chose "Broad v1" (full Parts I-IV) 2026-06-26. SPEC (RS `29351c2`: `load_4797.py` modern-style +
> `check_4797_integrity.py`; seeded + `form_4797_spec.json`) + COMPUTE (`0e8c0b4`: migration 0123 — 9
> Disposition recapture fields + 7 Taxpayer `f4797_*`; `compute_4797.py` pure port + `compute_4797_result`/
> `_db`/`_face` no-cache, wired BEFORE Sch D netting → Sch 1 line 4 / Sch D line 11 / unrecap §1250→SDTW)
> + RENDER (`361f19c`: `render_4797_1040` compute-driven face, the render_1116 precedent; f4797_2025 map
> comments fixed) + DIAGNOSTICS (`a8d2bff`: `rules_4797.py` 7 `D_4797_*` bridge-gated) + INPUT (`132041b`:
> serializers expose the new fields; React Disposition recapture block + `Form4797ReturnFactsPanel`; tsc 0
> err / vitest 217) + ASSERTIONS (7 `FA-1040-4797-*`, `_run_4797_assertion`; gate 332→339). 34 tests (27
> in test_compute_4797 + 7 FA). v1 boundaries: Sch D line-11 face cell blank (number flows right), ≤4
> props/Part, Part IV net 35a/b only, 4684/6252/8824 RED-defer. The ENTITY-side `render_4797`
> (DepreciationAsset) is unchanged — `render_4797_1040` is the parallel 1040 path.

> **2026-06-26 -- Form 1116 (Foreign Tax Credit §901/§904) ★★ UNIT COMPLETE -- tag `1040-form-1116-complete` -- all 7 legs green.**
> INPUT leg (2026-06-26): `Form1116Section` React tab (Form8615Section singleton precedent) in `FormEditor.tsx` +
> `Form1116Row` + returnData wiring + tab dispatch; `IndividualNav.tsx` tab "Foreign Tax Credit (1116)" placed in the
> **Credits** group (the FTC lands on Schedule 3 line 1 — NOT "Other taxes"; flagged to Ken) + `D_1116_→form_1116`
> RULE_TAB_MAP + singleton has-data dot. GREEN inputs only (no computed model columns; credit shows on Sch 3 L1).
> Gates: tsc 86→84 (0 new; fixed 2 pre-existing — emptyRd `form_5329s` + duplicate `D_5329_` key), vitest 217/217,
> vite build clean; no backend change (flow gate stays 332). tts INPUT commit follows `a1f522a`.
> Ken chose FULL form, "Tighter v1 — Passive-only". SPEC (`load_1040_form_1116.py` + `check_1116_integrity.py` ALL
> PASS; seeded + `1116_spec.json`) + SEED (`Form1116` singleton, migs 0121/0122, `seed_1116` 31 lines, serializer +
> `form-1116` route, manifest `f1116`) + COMPUTE (`compute_1116.py` → Sch 3 line 1, wired FIRST among Sch-3 credits;
> 18/18) + RENDER (`f1116_2025.py` + `render_1116`, map vs 118-widget PDF; 7/7) + DIAGNOSTICS (8 `D_1116_*`; 11/11)
> + ASSERTIONS (7 `FA-1040-1116`; **flow gate 325 → 332**) all GREEN + committed. Two paths → Schedule 3 line 1:
> §904(j) election (≤ $300/$600 → min(tax, regular)) + full Passive §904 limit (L21 = L20 × L17/L18, L24 =
> min(L14,L23), carryforward; L18 = 1040 L15 + Sch 1-A L37 SENIOR ded only; L20 = 1040 L16 + Sch 2 L1z). v1
> RED-defers (8 D_1116_*): non-passive, above-exception QD, Form 2555, carryover→Sch B, exotic. **RESUME = React
> `Form1116Section` tab (Form8615Section precedent) → then tag `1040-form-1116-complete`.** RS `acf4078` / tts
> `9ec9f79`(seed)..`a1f522a`(assertions).

> **2026-06-25 -- Form 5329 FULL (Parts I-IX) ★★ UNIT COMPLETE -- tag `1040-5329-full-complete`** (Ken chose
> FULL form + DUAL taxpayer/spouse). Additional Taxes on Qualified Plans: Part I early-distribution 10%/25%
> (full 01-23/99 exception catalog), Part II education/ABLE (10%), Parts III-VIII excess contributions (6% of
> smaller-of total-excess-or-12/31-value), Part IX excess accumulation / missed RMD (SECURE 2.0 10%/25%). Every
> part → Schedule 2 line 8 = the SUM of both owners. Dedicated `Form5329` model (one row/owner, 37 leaf fields
> + 6 nullable caps) + `RetirementDistribution.owner`; `compute_5329.py` (port of the verified RS `f5329_full`);
> model-driven dual render (one PDF/owner); `Form5329Section` React UI (9 parts); 5 dual `D_5329_*`; 6
> `FA-1040-5329-*` (gate 319→325). All 7 legs green. RS spec = the `F5329_*` blocks in `load_1040_retirement`
> (37/12/58/4/10). Commits RS `e9b042f` / tts `3128e28`..(assertions). v1: no contribution-limit modeling
> (preparer keys the leaves); SIMPLE-25% applies to all of L3 (flagged).



> **2026-06-25 -- STALE-LIST RECONCILE -- Form 2210 (Underpayment of Est. Tax, §6654) ★★ UNIT COMPLETE
> -- tag `1040-form-2210-complete` (built 2026-06-15; this tracker just never recorded it).** Full §6654:
> Part I required annual payment (90% / 100%-110% safe harbor, $1,000 de-minimis, prior-yr AGI >$150k→110%)
> + Part III regular quarterly method (7%/6% §6621 rate periods, four due dates) + Schedule AI annualized
> method → 1040 line 38. Seed `seed_form_2210` (`t2210_*` Taxpayer facts, mig 0082) / `compute_2210.py`
> (wired at END of the 1040 pass, after total tax + payments) / `f2210_2025.py` render / `rules_2210.py`
> (D_2210_NO_PENALTY + D_2210_PRIOR_YEAR) / 6 `FA-1040-2210-01..06` active in the 319 gate. All 6 legs
> verified GREEN today (5 leg files + flow gate, 354 passed). No RS spec (lookup 404) — local
> `2210_spec.json` + `check_2210_integrity.py`. v1 boundaries flagged (waiver/farmer/AI-full-tax deferred).

> **2026-06-25 -- Form 1040-X (Amended Returns) ★★ UNIT COMPLETE -- tag `1040x-complete` (Ken approved W1-W6).**
> Three-column A/B/C delta over a filed 1040 (A=as-filed baseline, C=corrected 1040, B=C−A). RS spec
> `load_1040_form_1040x.py` (seeded; 12 facts/12 rules/61 lines/8 diag/2 scenarios/6 FA; `check_1040x_integrity.py`
> ALL PASS; vs the real Form 1040-X Rev. Dec 2025). `AsFiledBaseline` (frozen Column A snapshot, snapshot-copy,
> auto-captures on mark-as-filed) + `Form1040X` singleton (migs 0117/0118). `compute_1040x` diffs the baseline vs
> the recomputed 1040 (verified map L6←1040 L18/L7←L21/L11←L24/L15←L28+L31; RED-defers carryback/superseding/
> cascade/missing-baseline). `render_1040x` (f1040x AcroForm, appended in the package) + 8 `D_1040X_*` + 6
> `FA-1040X`. Full suite 343 passed; flow gate 313→319. Open: Form1040XSection UI shows placeholders (wire to the
> computed FFVs); Part I deps recap + line 22/23 split = v1 blanks. RS `957a9d6` / tts `460abad`..`6a84ee0`.

> **2026-06-25 -- GA Form 500 (Georgia Individual) IN PROGRESS** -- the FIRST state individual return.
> RS spec SEEDED + EXPORTED (500_spec.json: 75 facts / 20 rules / 76 lines / 12 diag / 12 tests / 12 FA;
> integrity gate ALL PASS; Ken approved W1-W6). v1 scope MAXIMAL: resident + part-year/nonresident (Sch 3),
> standard + military retirement exclusions, Sch 4 NOL (80% limit), Low Income Credit, IND-CR 202 child-care.
> Constants: rate 5.19% (2025) / 4.99% (2026, HB 463); dependent exemption $4k / $5k. Depreciation
> sec 168(k)/179/OBBBA decoupling = Sch 1 direct-entry (W1).
> **UNIT COMPLETE (v1)** (all 6 legs + create_state_return + UI: seed 9 sec/131 lines, compute byte-for-byte vs gate, render, 12 diagnostics, 12 flow assertions [gate 313], GA-500 data-entry tabs [client tsc 84/vitest 211/build clean]);
> attaches to the 1040 via the state_returns FK (GA-600S precedent).
> **★★ UNIT COMPLETE — tag `ga500-complete` 2026-06-25.** Render follow-ups A/B (`d1cea56`/`33a0ca8`) +
> the "Finish GA-500" pass: (1) line 42-46 reconciliation (`75b118b`/RS `14e4410`) — UET=L42/Interest=L44/
> Amount-Due=L45/Refund=L46 corrected + 10 gift check-offs L32-41 modeled (Ken: MAXIMAL); (2) HB 463 TY2026
> std deduction $15k/$30k (`ff36384`/RS `fd9ebc9`, Ken confirmed TY2026 over conflicting sources); (3)
> header-v2 (`7f97ba3`) — filing status/residency/dependents/DOB. The full printed Form 500 = identity
> header + lines 8-46 + full-package append. GA-500 suite 44/44 + flow gate 313 + RS integrity 14 scenarios.
> Open (low-priority): MI/suffix/DL/part-year-date-range; HB 463 effective-year vs the bill text (REVIEW_QUEUE);
> HB 463 annual step-ups begin TY2027 (not modeled). NEXT = Kens direction.

> **2026-06-24 — Form 8615 (Kiddie Tax §1(g)) UNIT FULLY COMPLETE** — all 6 legs + the QDCGT cap-gain follow-up, tag `1040-8615-complete` (ordinary + qualified-dividend/net-cap-gain).
> Singleton `form-8615` CRUD route + "Kiddie Tax (8615)" tab (Other taxes group); ordinary
> §1(g) core → 1040 line 16; QD/net-cap-gain + SDTW/Sch J/8814 RED-deferred. Flow gate 312
> 9 FA active (FA-1040-8615-01..09; gate 301). QDCGT cap-gain follow-up DONE (mig 0116
> sibling QD/cap-gain fields; Line 5 Worksheets 1/2/3; D_8615_008 retired; FA-06 flipped).
> Resume = Ken's direction. CPA-review care items: WS2/WS3 prorations. See the row + STATUS.md.
*Last updated: 2026-06-24 (**FORM 8829 (Business Use of Home) — ★ UNIT COMPLETE — ALL 6 LEGS GREEN — tag 1040-8829-complete (spec/seed/compute/render/input/diagnostics/assertions; flow gate 261→270 active 1040 FA, full test_flow_assertions.py suite 292/292).** NEW form after the quick-wins bundle (Ken chose Form 8829 home office, then "BROADER: build the Schedule A split"). No prior RS spec (`lookup/8829` → 404), spec-first. Verified vs the actual 2025 f8829.pdf (read directly) + i8829 (Rev. Mar 4 2026) + IRC §280A(c)(5) + §168/Pub 946. The actual-expense home-office engine: Part I business % (area + daycare hours-of-use) → the §280A(c)(5) gross-income limitation in 3 ordered tiers (deductible-anyway 9-14 → operating 16-27 → casualty+depreciation 29-33, each capped at the income remaining; depreciation last → carries over first) → line 36 → Schedule C line 30; Part III 39-yr nonresidential mid-month SL depreciation (Jan 2.461%…Dec 0.107% first year, 2.564% subsequent); Part IV carryover. v1 COMPUTES the RE-tax SALT split (lines 11/17) reusing `compute_schedule_a.salt_line5e` (the OBBBA $40k cap + $500k-MAGI 30% phasedown) incl. the >$500k circular MAGI↔home-office↔AGI iteration (W4 = fixed-point loop); the mortgage Pub-936 split (10/16) stays a preparer fact for itemizers (W3 — matches Sch A; standard-deduction path computed); RED-defers line-35 → Form 4684 + multi-business line-36. RS `load_1040_form_8829.py` (`b933b6e`) = 47 facts/13 rules/44 lines/8 diag/11 scenarios/9 FA/20 links/4 sources; `check_8829_integrity.py` ALL PASS (depreciation table + daycare + SALT + helpers + 11 scenarios re-derived; caught 2 self-authored col-(a)/(b) errors). Ken approved the review walk (W1-W6) → flipped READY_TO_SEED → seeded (Created 8829) → deployed export verified (HTTP 200) → canonical `server/specs/8829_spec.json` + 9 FA staged. LEGS 1-4 (SEED/COMPUTE/RENDER/INPUT) DONE + GREEN — seed `620cbb4`, compute `a572424`, render `372d15d`, input leg (writable Form8829 CRUD route `schedule-c/<sc_id>/forms-8829` mirroring ScheduleCOtherExpense + React Schedule C actual-expense panel, NO migration). LEGS 1-6 DONE + GREEN — UNIT COMPLETE (tag `1040-8829-complete`). LEG 5 (diagnostics, `1426df1`): 8 `D_8829_*` bridge-gated + `D_SC_007` narrowed in the diagnostic (not the helper). LEG 6 (assertions, this commit): merged 9 `FA-1040-8829-*` via NEW `scripts/merge_8829_assertions.py` + a NEW `_run_8829_assertion` (PURE `compute_8829` re-derivation + `inspect.getsource` source-pins); flow gate 261→270 active 1040 FA (full suite 292/292; the predicted "341→350" was stale — verify gate numbers empirically). RESUME = Ken's direction. The simplified $5/sq ft method already exists (`compute_schedule_c.py:252`); actual-expense is RED-deferred today via `D_SC_007`. Decisions: DECISIONS.md 2026-06-24 Form 8829. Prior: **QUICK-WINS BUNDLE — DONE. QW1 Simplified Method joint-survivor = ALREADY BUILT (verified end-to-end; the 2026-06-22 audit was stale — engine has the full Table-2 path, model/serializer/UI all wired, D_SM_003 lifts on survivor age; no work). QW2 Form 2441 claim gate = DONE + GREEN (commit `132d29e`): Ken chose amend-spec + gate-compute — the per-Dependent `claims_dependent_care` checkbox is now the authoritative §21 claim election, so a qualifying-but-unclaimed dependent computes $0 (reconciles `compute_2441` with the `d_credit_dc` diagnostic). Spec-first: `FORM_2441` R-2441-QUALIFYING amended (canonical `form_2441_spec.json` + RS loader, commit `93965c4`, NOT re-seeded — next-deploy TODO); `qualifying_dependents` gated on the flag; new `_is_sec21_qualifying`/`sec21_qualifying_unclaimed`; new `D_2441_007` (info) no-silent-gap nudge; `FA-1040-2441-07` + runner. Flow gate 282→283, 2441 tests 36/36, NO migration. RESUME = Proforma producer (audit #4), deferred to ~2026-06-29 (Ken back in office). Decisions: DECISIONS.md 2026-06-24 Form 2441. Prior: FORM 6251 (AMT engine) — UNIT COMPLETE — ALL 6 LEGS GREEN, tag `1040-6251-complete` (flow gate 273→282). LEG 6 (ASSERTIONS, commit `5b08564`, NO app-code/migration): merged the 9 staged `FA-1040-6251-01..09` via NEW `scripts/merge_6251_assertions.py` (flip staged→active — the W-2G lesson; pending emptied) + a NEW `_run_6251_assertion` runner (FA-1040-6251-* id-prefix in all 3 entry points; PURE `compute_6251` re-derivation + constant pins + `inspect.getsource` source-pins on `compute_6251_db`/`compute_return`): FA-01 L11→Sch 2 L2, FA-02 AMT=max(0,TMT−reg), FA-03 exemption phaseout (2026 OBBBA 50%/$500k), FA-04 TMT 26/28% breakpoint, FA-05 AMTI std add-back + QBI retained (no qbi param), FA-06 Part III reuses QDCGT worksheet, FA-07 constants year-keyed, FA-08 RED-defer no-silent-gap, FA-09 line 10 excludes Schedule J. Flow gate 273→282 (2.4s, pure). The full Form 6251 AMT engine now computes the common case → Sch 2 L2 → total tax (AMTI add-backs/exemption phaseout/26-28 TMT/Part III cap-gains), RED-defers ISO/QSBS/NOL/estate-trust/exotic/AMT-FTC + §1250-28% SDTW, renders the 2-pp face + AMT input card, fires 8 D_6251_*, gated by 9 FA. RESUME = Ken's direction (next DEFERRAL_AUDIT big-ticket). Decisions: DECISIONS.md 2026-06-24 leg 6. Prior: BUILD LEG 5 (DIAGNOSTICS) COMPLETE + GREEN (commit `fbc7805`). NEW `apps/diagnostics/rules_6251.py` wires the 8 `D_6251_*` from `6251_spec.json`, each BRIDGE-GATED to `compute_6251_face` (single source the render reads): `D_6251_001..006` (error) fire EXACTLY when the engine red-defers on that preference (read from the `red_defer` list; §1250/28% Part III SDTW routes under 005), `D_6251_007` (info) when AMT (line 11) > 0, `D_6251_008` (warning) when capital gains drove Part III; `RULES_6251` in `runner.seed_builtin_rules` (self-seeds on deploy). NARROWED `D_AMT_DEFER` (`rules_amt.py`): dropped private-activity-bond interest from `amt_indicators` (the engine now computes it as line 2g → `D_6251_007`; removes the AMT=0 false positive), KEPT K-1 AMT items (engine doesn't auto-import them — flagged v1 gap; the return-level twin of `D_K1_AMT`). Fixed a leg-2 fixture regression: `test_amt_red_defer._set_sch2_line2_amt` now sets `is_overridden=True` (as production does) so the engine preserves a manual Sch 2 L2 escape-hatch entry. Gates: 21/21 diagnostics+AMT, flow gate 273 (unchanged) + 6251 compute/render 35/35 (308 total), check/makemigrations clean, NO migration. RESUME = build leg 6 (assertions — LAST: merge 9 staged FA via `merge_6251_assertions.py` + `_run_6251_assertion`, gate 273→282, tag `1040-6251-complete`). Decisions: DECISIONS.md 2026-06-24 leg 5. Prior: BUILD LEG 4 (INPUT) COMPLETE + GREEN (commit `d9fd4ba`). PURE FRONTEND — the 11 writable `amt_*` Taxpayer facts (model migration 0111 + serializer from leg 1) are now editable in the React FormEditor: NEW `Form6251Section` on a NEW `form_6251` tab ("AMT (6251)") in the "Other taxes" nav group — 5 common-case add-backs as GREEN override inputs with YELLOW "blank = auto from <line>" hints (std L12 / SALT Sch A L7 / senior Sch 1-A L37 / refund Sch 1 L1; depreciation L2l direct) + 6 RED-defer preference items (ISO/QSBS/NOL/estate-trust/other/AMT-FTC) with RED "entering defers to manual" hints; a computed-AMT banner reads Schedule 2 line 2 (computed AMT > 0 / no-AMT note / RED defer warning). Diagnostic→tab routing wired ahead of leg 5: `RULE_TAB_MAP` += `D_6251_`→`form_6251` AND `D_AMT_`→`form_6251`; `IndividualNav.test.tsx` +2 routing cases. Gates: client tsc clean / vitest 205/205 (nav 86/86) / build clean; NO Python, NO migration → flow gate 273 unchanged. RESUME = build leg 5 (diagnostics: `rules_6251.py` wiring the 8 `D_6251_*` bridge-gated to `compute_6251_face` + narrow `D_AMT_DEFER`). Decisions: DECISIONS.md 2026-06-24 leg 4. Prior: BUILD LEG 3 (RENDER) COMPLETE + GREEN. The Form 6251 face prints when AMT > 0. `field_maps/f6251_2025.py` = the 62-widget AcroForm map, every acro_name VERIFIED vs the real 2025 f6251.pdf by position (page 1 f1_1..f1_33 = 2 header + Part I 1a-4 + Part II 5-11; page 2 f2_1..f2_29 = Part III 12-40). NEW `compute_6251.compute_6251_face` (render+diagnostics single source) re-derives via `compute_6251_result` from the persisted 1040 rows + adds Part I 1a/1b — **W1 RESOLVED:** 1a = 1040 L12+L13 (deductions − senior), 1b = AGI(L11) − 1a = taxable income + restored senior; 1b+2a+2b+2g+2l == engine line 4 (AMTI) by construction. `renderer.render_6251` MODEL-DRIVEN, gated on line 11 > 0 (RED-defer/AMT-free → prints nothing; the D_6251_* REDs own the deferred case); fills Part I (1a-4) + Part II (5-11) + Part III anchors 12/40 when the cap-gains worksheet is used; f6251 ∈ ACROFORM_FORM_IDS; appended after Form 8959 (seq 32); page 2 skipped via SKIP_PAGES unless part_iii. `test_6251_render_leg.py` 9/9 (field map vs PDF existence/no-dup/full-62 + face render + 4 DB pipeline incl. rendered L11 == Sch 2 line 2 + 1a/1b reconciliation + no-print gates); flow gate 273 unchanged; NO migration. v1 flagged: Part III intermediate 13-39 not surfaced (0/15/20% breakdown inside the reused QDCGT WS); §1250/28% Part III RED-deferred (leg 2). RESUME = build leg 4 (input: surface the 11 `amt_*` facts in the React FormEditor — a Form 6251 AMT card). Decisions: DECISIONS.md 2026-06-24 leg 3. Prior: BUILD LEG 2 (COMPUTE) COMPLETE + GREEN.  NEW `apps/returns/compute_6251.py` = the AMT engine. Pure `compute_6251` is a Decimal port of the RS loader + gate math (constants + line-5 exemption phaseout [2026 OBBBA $500k/$1M @ 50%] + 26/28% TMT BYTE-FOR-BYTE; the 10 spec scenarios re-derive). AMTI base = taxable income + std-ded/SALT add-back + senior add-back (QBI retained) + PAB 2g + depreciation 2l − refund 2b → exemption → 26/28% TMT (or Part III) → AMT = max(0, TMT − regular tax) → line 11. Part III reuses `compute_qdcgt_worksheet` on the AMT base with the 26/28% rate as the `ordinary_tax_fn` (0/15/20% case computed; §1250/28%-rate SDTW path RED-deferred, W4). DB wrapper `compute_6251_db` gathers the 1040 quantities (taxable income, PRE-Sch-J regular tax for line 10, std/SALT/senior add-backs defaulted from the return + `amt_*` override, PAB aggregate, cap-gain inputs), writes Schedule 2 line 2, reflows via `compute_sch_2` → 1040 line 17 → total tax (the Form 8962 late-feeder precedent); WIRED into `compute_return` after `_compute_1040_tax`, before Schedule J + the first downstream pass. RED-defer guard (6 facts + §1250/28%) BLANKS line 11 (no silent gap; manual escape-hatch override preserved). Common case AMT = 0 → line 2 blank → no reflow → no regression. `test_6251_compute_leg.py` 26/26 (20 pure incl. 10-scenario math gate + 6 DB incl. full-pipeline) + flow gate 273 + 8995-A/Topic 8 regression 70/70 + check/makemigrations clean; NO migration. v1 flagged: Part III §1250/28% deferred, line-10 Sch2-L1z/Sch3-L1/4972 adjustments deferred, itemizing uses the elect flag, Part III intermediate rounding unpinned. RESUME = build leg 3 (render: `f6251` 2-pp face, 62 widgets; PIN 1a/1b vs the real 1040 lines, W1; single-source via `compute_6251_result`). Decisions: DECISIONS.md 2026-06-24 leg 2. Prior: BUILD LEG 1 (SEED) COMPLETE + GREEN.  Migration `0111` adds 11 return-level `Taxpayer` AMT facts (5 common-case add-backs the engine computes — `amt_salt_deduction`/`amt_standard_deduction`/`amt_senior_deduction`/`amt_tax_refund`/`amt_depreciation_adj` — + 6 RED-defer preferences `amt_iso_exercise`/`amt_qsbs`/`amt_nol`/`amt_estate_trust`/`amt_other_preference`/`amt_ftc`), all DecimalField(15,2) default 0, NO RLS migration (scalar cols on the RLS-enabled `returns_taxpayer`, 0110 precedent); `amt_pab_interest` is NOT a field (line 2g derived from the existing `pab_interest` aggregate per the STATUS lock). Manifest `f6251` added + downloaded (2pp, 62 AcroForm widgets, SHA recorded). `seed_6251.py` (the `seed_8995a` template) seeds 38 lines / 3 Parts from canonical `6251_spec.json` line_map (`is_computed` follows line_type; idempotent; build.sh auto-discovers). `TaxpayerSerializer` exposes the 11 facts WRITABLE (GREEN). Stale trip-wire fixed: `test_tts_forms.py` manifest count 25→54. `test_6251_seed_leg.py` 10/10 + check/makemigrations clean + flow gate 273 unchanged. RESUME = build leg 2 (`compute_6251.py` — the engine from the spec rules, byte-for-byte vs `check_6251_integrity.py`; reuse `compute_regular_tax` for line 10 + QDCGT/SDTW for Part III; → Schedule 2 line 2). Decisions: DECISIONS.md 2026-06-24 leg 1. Prior: **FORM 8995-A (above-§199A-threshold QBI) — UNIT COMPLETE — ALL 6 LEGS GREEN — tag `1040-8995a-complete` (flow gate 264→273). Above the §199A threshold the full Form 8995-A now computes the QBI deduction → 1040 line 13 (Parts I-IV + Schedule A SSTB % + Schedule B aggregation + Schedule C loss-netting), renders the 2-pp face + statement pages, takes the per-business inputs, fires the 7 D_8995A_* diagnostics, and is gated by 9 flow assertions. v1 RED-defers the Schedule D patron reduction (L14) + §199A(g) DPAD (L38) → L13 blank + D_8995A_001/002 + the narrowed D_8995_001 (no silent gap). LEG 6 (ASSERTIONS): merged the 9 staged FA-1040-8995A-01..09 via `scripts/merge_8995a_assertions.py` (flips staged→active — the W-2G lesson) + a NEW `_run_8995a_assertion` runner (FA-1040-8995A-* id-prefix dispatch in all 3 `test_flow_assertions.py` entry points, the schf/schj/k1 precedent) — PURE re-derivation through `compute_8995a` for the math (FA-02..06), constants pins (FA-07), `inspect.getsource` source-pins on `compute_8995_db`/`compute_8995a_db` for the cross-form flow + routing + no-silent-gap (FA-01/08/09); flow gate 264→273, no app code change, pending emptied. LEG 5 (DIAGNOSTICS): NEW `apps/diagnostics/rules_8995a.py` wires the 7 positive-validation D_8995A_* (001 patron / 002 DPAD errors fire at ANY income — the no-silent-gap coverage below threshold that the above-threshold-only D_8995_001 misses; 003 aggregation-unverified warn / 004 SSTB-above-ceiling info / 005 no-W2-UBIA warn [reads the POST-aggregation `col_lines`, not raw businesses] / 006 loss-carryforward info / 007 SSTB-in-aggregation error — gate on the above-threshold 8995-A regime so the simplified-8995 D_8995_002..005 own below-threshold), each bridge-gated to the `compute_8995a` helpers; registered via `RULES_8995A` in `runner.seed_builtin_rules` (self-seeds on deploy via `build.sh seed_rules`). `_gather_8995a_businesses` gains a display-only `label` (the pure engine ignores it) so the diagnostics name the EXACT businesses the engine computes — zero drift. D_8995_001 already narrowed in leg 2 (the complementary RED owning the line-13 blank). `test_8995a_diagnostics_leg.py` 14/14 + 8995a compute/render/input regression 31/31 + flow gate 264; NO migration; check/makemigrations clean. GOTCHA: lingering test_postgres → `scripts/drop_test_db.py` between DB-test runs. Decisions: DECISIONS.md 2026-06-23 leg 5. RESUME = leg 6 (merge the 9 staged `flow_assertions_1040_8995a_pending.json`, gate 264→273, tag `1040-8995a-complete`). Prior leg 2 (COMPUTE) — GREEN. NEW `apps/returns/compute_8995a.py` = byte-for-byte port of `check_8995a_integrity.py` (RS): Schedule A SSTB applicable% + Schedule B aggregation combine + Schedule C loss netting + Part II W-2/UBIA limit + Part III phase-in + Part IV REIT/PTP + income limit → L39 → 1040 line 13. `_gather_8995a_businesses` (ScheduleC + non-accrual ScheduleF + each §199A-bearing ScheduleK1) + `compute_8995a_db`→`(red_defer,result)` + `compute_8995a_red_defer`. WIRED into `compute_schedule_c.compute_8995_db` above-threshold branch (net_cap_gain hoisted; → L13 = `_q(L39)` INSTEAD of the old blank; simplified-8995 rows still blanked — 8995-A is the render-leg face). `D_8995_001` NARROWED (coupled to compute — its "line 13 blank/not built" message became false): bridge-gated to `compute_8995a_red_defer`, fires only for the patron/DPAD Schedule D gap. KNOWN v1 GAP (flagged): no patron/DPAD INPUT field → red_defer DORMANT (above-threshold ag-coop patron computes an overstated number) — guard wired, re-arms when the input lands. `test_8995a_compute_leg.py` 16/16 (10-scenario math gate + 4 pipeline + 2 const); flow gate 264; check + makemigrations clean; NO migration. Updated 2 obsolete tests (topic8 above-threshold → L13=0.00 not blank; pure-farm-above-threshold → L13=0.00, D_8995_001 silent). GOTCHA: lingering test_postgres → `DROP DATABASE test_postgres WITH (FORCE)`. Decisions: DECISIONS.md 2026-06-23 leg 2. RESUME = leg 3 (render: un-stub `f8995a_2025.py` Parts I-IV + Sch A/B/C faces, single-source off `compute_8995a`, resolve W7). Prior leg 1 (SEED) `c6a4234`: mig 0108 per-business W-2/UBIA/SSTB/aggregation + `QBIAggregation` + `seed_8995a` + 13/13.** **FORM 8582 PER-ACTIVITY — STAGE B (passive Sch C / Sch F) — UNIT COMPLETE — ALL LEGS GREEN — tag `1040-8582-per-activity-stage-b-complete` (flow gate 264). Extends the per-activity §469 allocation to passive (material_participation is False) Schedule C / Schedule F → Form 8582 Part V, closing the last 2 activity types. PURE ENGINE UNCHANGED (Sch C/F are bucket-V activities); no RS amendment (R-8582-WS-NET already names them), no migration (cols from 0107). compute `93c0208` (`_gather_passive_business` + keymaps dict; MAGI add-back via `form_8582_magi(passive_cf_loss=)` since a passive Sch C/F loss is already in first-pass AGI via Sch 1 L3/L6, unlike rentals/K-1; Sch 1 L3/L6 overwrite = active-net+passive-income−passive-allowed; QBI excludes suspended via `_adjust_passive_cf_qbi`; SE untouched—moot; test_schedule_e_8582_stage_b 7/7 incl. MAGI-addback headline special-allowance $15k-not-$20k). diagnostics `0cadf27` (`d_sf_passive` retired-by-narrow → fires only for un-allocated residual; `d_sc_006`/`d_sf_loss` reworded; test_schedule_f_diagnostics_leg 17/17). input (ScheduleC/F serializers mark allowed/suspended read-only; FormEditor 8582 block on the Sch C/F cards; test_..._stage_b_input 4/4, tsc/vitest 203). render (Sch C/F surface on Part V/VII/VIII with "Sch C"/"Sch F" labels; test_..._stage_b_render 2/2). assertions = flow gate 264 holds, no new FA (FA-05/06/07 type-agnostic). Part IX still RED-deferred (D_8582_MULTIFORM). Known v1 edges flagged (income+prior, QBI vintage, passive-income SE). Decisions: DECISIONS.md 2026-06-23 Stage B. GOTCHA: Sch C/F POST recomputes → YELLOW cols computed post-POST. **STAGE A (rentals + passive K-1):** UNIT COMPLETE — ALL 6 LEGS GREEN — tag `1040-8582-per-activity-complete` (flow gate 264).** Legs 4-6: INPUT (`d071a89`) = serializers expose the 3 cols on RentalProperty+ScheduleK1 (prior-unallowed GREEN/writable; allowed/suspended YELLOW/read-only) + FormEditor.tsx 8582 block on the rental + K-1 cards; test_schedule_e_8582_input_leg 4/4, tsc/vitest 203/build clean. DIAGNOSTICS (`39cdef6`) = `d_k1_passive_loss` NARROWED (bridge-gated on persisted allowed+suspended==0 → fires only for UN-allocated passive losses: PTP/un-engaged; non-PTP now allocated → no RED; d_k1_ptp_loss warning kept) + NEW `d_8582_multiform` RED (Part IX: passive activity allocated by 8582 with a §1231/28%/§1250 component = losses on 2+ forms; interpretation flagged — §1231/§1250-on-passive-loss proxy since the model can't detect same-activity-on-2-forms); 28 tests; self-seeds on deploy. ASSERTIONS (`39cdef6`) = merged FA-1040-8582-05/06/07 (conservation/loss-ratio/Part-IX-RED) via scripts/merge_8582_per_activity_assertions.py + extended `_run_8582_assertion` (per-activity ids dispatch first); flow gate 261→264, pending emptied. **RESUME = STAGE B (passive Sch C/F) or Ken's direction** — extend `_gather_passive_activities` for ScheduleC/F passive rows + Sch 1 L3/L6 add-back (cols exist on Sch C/F from 0107). Stage A legs: SPEC LEG + BUILD LEGS 1 (seed: migration 0107, per-row `prior_year_unallowed_passive` + computed `passive_8582_allowed`/`_suspended` × RentalProperty/ScheduleC/ScheduleF/ScheduleK1) + 2 (compute STAGE A) + 3 (render Parts IV-VIII) DONE + GREEN. LEG 3 (render `78ccded`): Form 8582 Parts IV-VIII now PRINT, model-driven via the shared pure `per_activity_allocation` (extracted as the SINGLE SOURCE OF TRUTH for compute+render; `compute_8582_per_activity` attaches `out["face"]`, per_activity/C/aggregate BYTE-IDENTICAL). Field map 17→**175** fields from the real f8582.pdf widget names (Table_Part4/5/6/7/PartVIII; 5 activity rows + a totals row/part; ratio cols rendered as TEXT). `render_8582._f8582_per_activity` fills Part IV (active rental) / V (other passive incl. passive K-1) / VI (special-allowance alloc, line 9>0) / VII (unallowed alloc) / VIII (allowed loss) + per-col totals + the "form or schedule" reporting-line column (Sch E 22/28/33); VI-VIII only when line C>0; >5 activities/part → supporting statement page (no silent gap); Part IX RED-deferred. test_schedule_e_8582_render_leg **12/12** (shape 175 + single-rental+prior row-1 positions + 2-rental 3:1 ratio split totals + passive-K-1 Part V/VIII + 6-rental overflow statement) + engine 10/10 + flow gate **261** + compute/diagnostics regression 30. NEXT = leg 4 (input: expose `prior_year_unallowed_passive` GREEN + `passive_8582_allowed`/`_suspended` YELLOW on the RentalProperty + ScheduleK1 serializers/UI; Stage A scope) → diagnostics (add `D_8582_MULTIFORM`; RETIRE `d_k1_passive_loss` keep PTP) → assertions (merge 3 staged FA, gate 261→264) → tag `1040-8582-per-activity-complete`. Leg 2: pure `compute_8582_per_activity` engine (10/10 vs PA1/PA2/PA3 + conservation + col-g wiring); `_gather_passive_activities` wires rentals (Part IV/V) + passive NON-PTP K-1 (Part V) through Schedule E p1 line 22 / p2 col (g) → line 41 → Sch 1 L5; `_persist_per_activity` writes the handoff columns; `_inject_legacy_prior` folds the aggregate field pro-rata. Ken-approved scope: PTPs OFF 8582 (§469(k)/i8582), Sch C/F = Stage B fast-follow. `seed_8582` +6 lines (C,P4-P8 → 23). test_schedule_e_8582_compute_leg spec 10 + pipeline 4 (incl. passive-K-1-fed PA2-shape + PTP-off) + K-1 compute/render 58 + flow gate 261. `d_k1_passive_loss` stale until leg-5 retire (flagged). NEXT = render (f8582 Parts IV-VIII model-driven) → input → diagnostics (RETIRE d_k1_passive_loss + add D_8582_MULTIFORM) → assertions → tag. Commits leg2a `11873bd` + leg2b (this). SPEC LEG was DONE + SEEDED + EXPORTED + STAGED (Ken-approved walk).** AUDIT CORRECTION (the Phase-0 lesson): the Schedule E p1 + Form 8582 AGGREGATE unit was ALREADY built + tagged `1040-schedule-e-8582-complete`; the Tier-2 "8582 fed by K-1/Sch C-E-F" item = the per-activity §469 allocation EXTENSION (passive K-1/C/F losses were RED-deferred via `d_k1_passive_loss`, never feeding 8582's Part V). Ken chose FULL per-activity (Parts IV-VIII), Part IX RED-deferred, all 4 activity types feed Part V. Verified vs the real 2025 f8582.pdf (3 pages, 205 widgets — Parts IV-IX FILED) + i8582. RS `load_1040_schedule_e.py` amended (FORM_8582 only, `--only` scoped → SCHEDULE_E untouched): +6 rules (WS-NET/ALLOC-VI/ALLOC-VII/ALLOWED-VIII/CARRYFWD/MULTIFORM) +`f8582_line_c` +6 lines (C,P4-P8) +`D_8582_MULTIFORM` RED +4 scenarios (PA1/PA2/PA3 worked math + PG1) +12 links +Parts IV-IX excerpt +3 FA → FORM_8582 **18/12/23/7/11/20 + 9 FA**; `check_schedule_e_8582_integrity.py` independent per-activity recompute **ALL PASS**; deployed export HTTP 200; canonical `server/specs/form_8582_spec.json` re-committed + 3 FA staged (`flow_assertions_1040_sche_8582_pending.json`); RS `9f34027` (pushed). Core mechanic: line C = (line 3 loss) − line 9; Part VI/VII allocate by loss-ratio (Σ Part VII = line C); Part VIII allowed = loss − unallowed → its schedule; Σ allowed = line 11. NEXT = build leg 1 (per-activity prior-unallowed field on RentalProperty/ScheduleK1/ScheduleC/ScheduleF + migration) → compute → render (f8582 Parts IV-VIII) → input → diagnostics (RETIRE `d_k1_passive_loss`; add `D_8582_MULTIFORM`) → assertions (merge 3 FA) → tag `1040-8582-per-activity-complete`. Brief: `server/specs/_schedule_e_8582_per_activity_brief.md`. Prior: **TIER 1 ITEM 3 LEG 2 (DD-ATTESTATION HARD PRINT-GATE) DONE + GREEN (9/9): `tts_forms/views.py` `due_diligence_print_blockers`+`_enforce_print_gate` hard-block BOTH return-level print endpoints (`render-pdf` AND `render-complete` — closes the bypass; no-ops for entities) for a 1040 when (a) `preparer_id is None` OR (b) a §6695(g) covered credit is claimed with incomplete due diligence; condition (b) BRIDGE-GATES to `d_8867_001_due_diligence_unanswered` (the exact UI diagnostic — can't disagree). Ken: block on BOTH, 1040-only, allow an audit-logged override, HTTP 400. `override:true` (body or `?override=`) bypasses + proceeds. NEW first-class `AuditAction.PRINT` (migration `audit/0002` = choices-only AlterField, SQL `(no-op)`) + `log_print` helper: blocked→`event=blocked`, override→`event=override` (both `{blockers,endpoint}`); clean render logs nothing. `tests/test_print_gate.py` 9/9 (renderer mocked) + combined Item 3 run 30/30. **KEN DECLARED ITEM 3 COVERED** — the office-usability trio (Tier 1 Items 1-3: proforma / "$0-limited" explainer / DD-attestation+preparer-of-record) is COMPLETE; leg 3 (contemporaneous DD notes) is a tracked follow-up. NEXT = Ken's direction on the next tier (DEFERRAL_AUDIT.md: Form 8582 fed by K-1/Sch C-E-F [highest volume], Form 8995-A above §199A threshold, full Form 6251 AMT engine). Prior: **TIER 1 ITEM 3 LEG 1 (PREPARER-OF-RECORD) DONE + GREEN (8/8): preparer-of-record = the existing `TaxReturn.preparer` FK (Ken decision — no new field, no migration). Two genuine gaps shipped (the rest — `_sync_preparer_info`→`PreparerInfo`→signature render + the FormEditor/ReturnManager assign UI — ALREADY EXISTED): (1) ENFORCE — new `apps/diagnostics/rules_preparer.py` `D_PREPARER_001` (WARNING, Ken-chosen: warning now / hard-block at the print-gate; fires when the 1040's `preparer_id is None`; `_ctx_1040`-gated no-op for entities; `RULES_PREPARER` in `runner.py`; self-seeds on deploy via `build.sh seed_rules`); (2) AUDIT ATTRIBUTION — `update_info` saved the model directly, bypassing `AuditViewSetMixin.perform_update`, so the §6695 assignment went unlogged → `snapshot`+`log_update` added; GOTCHA fixed: `audit/service._safe_value` only stringified Models, so a changed `preparer_id`(UUID)/`signature_date`(date)/`tentative_tax`(Decimal) crashed the `changes` JSONField → added UUID/date/datetime/Decimal→str (fixes ALL audited FK/date/Decimal updates, lossless display-only). `tests/test_preparer_of_record.py` 8/8 + `test_audit.py` 13/13; no compute math → no flow-assertion gate. NEXT = LEG 2: DD-attestation HARD print-gate (block `render-pdf` when a §6695(g) covered credit is claimed + not attested AND/OR no preparer-of-record; log the blocked attempt; confirm HTTP code + scope with Ken). Prior: **TIER 1 ITEM 2 (EXPLAINER) LEG 1 — §179 income-limitation diagnostics DONE + GREEN: new `apps/diagnostics/rules_4562.py` wires `D_4562_011` (warning, spec D011 — current-year election line 9 > income limit line 11) + `D_4562_014` (info, spec D014 — carryover line 13 > 0), both BRIDGE-GATED on the persisted Taxpayer §179 fields (the same source `render_4562` reads → can't disagree with the math); line 9 derived = L12+L13−L10; reads `sec_179_business_income_limit` for L11 (the COMPUTED limit, NOT the `taxable_income_limitation` override field). 9/9 `test_section_179_diagnostics.py` (3 no-DB trip-wires + 6 DB incl. 1 end-to-end through `compute_return`). Surfaces via the EXISTING diagnostics path (`run_diagnostics`→`DiagnosticsTab` renders info) — no UI work. Self-seeds in prod on deploy (`build.sh` runs `seed_rules` pass 2) — NOT manually seeded (avoids the dev-shares-prod race window). SCOPING: the CREDIT "$0/limited" explainer already existed (`credit_gates.py` EIC/CTC/ODC/DC/AOTC + the existing D_SC_005/D_8812_004/8582/8962 limitation explainers); §179 was the one genuinely-uncovered surface. Interpretation flagged: read D011's `elected_179` as line 9 (distinct from D014), not `(L9+L10)`. NEXT = Ken's call: Item 2 sufficient → Item 3 (DD-attestation + preparer-of-record), or more non-credit explainers (Sch 1-A ITIN, generic-message strengthening). NOT committed at write time.** Prior: **TIER 1 PROFORMA (item 1) — COMPLETE (read/roll side). Legs 2b+3+4 landed: `_populate_individual_from_prior_year` roll-forward (views.py, hooked into create_return; snapshot-COPY of demographics + dependents + the 5 carryforwards [§179 prior 4562 L13→CY L10] + 8606 basis; `_PROFORMA_*_KEYS` document the 1040 snapshot contract; test_proforma_1040 6/6), §179 serializer surface + UI (provenance banner + Form 4562 §179 card), and the 4 FA-4562-179-* merged into the active gate (257→261). REMAINING: the snapshot PRODUCER (TaxWise importer / app-to-app snapshotter) — flagged in STATUS, not silent. Prior: BUILD LEG 2a (§179 CONSUMPTION COMPUTE + RENDER) DONE + GREEN: NEW 1040-gated `compute_section_179_limitation` (compute.py, wired into compute_return after aggregate_1040_income, before the farm/biz families) computes Form 4562 lines 9-13 per the seeded spec (R001/R011/R014/R015) — L11 = override (`Taxpayer.taxable_income_limitation`) else SUGGESTION (Σ Sch C net + Σ Sch F net BEFORE §179 + 1040 L1a wages, MFJ combined, min L5, floor 0; circularity broken via net-before-§179), L12 = min(L9+L10, L11), L13 = (L9+L10)−L12; L12 distributed PRO-RATA by current §179 elected (fallback positive net) into each business's depreciation column (Sch C L13 / Sch F L14) so AGI/SE/QBI reflect it; `landed<L12` guard = no homeless carryover; common case BYTE-IDENTICAL; entities untouched. Migration 0106 (additive, applied) = 4 Taxpayer fields (`taxable_income_limitation` override + computed `sec_179_business_income_limit`/`sec_179_deduction`/`sec_179_carryover_next`). `render_4562` shows lines 10-13 for individuals from the persisted values (Part I now renders for a pure carryover too). `test_section_179_carryover.py` 12/12 (5 PURE spec scenarios + 6 PIPELINE incl. pro-rata 30:20 split + income-limited net floor + 1 RENDER); regression topic8+schedule_f 71+2 + flow gate 257 + manage.py check clean. ENV gotcha: `poetry run` intermittently hangs >120s — invoke the venv python directly. NEXT = Leg 2b/3 (1040 `_populate_*_from_prior_year` roll-forward [PY L13→CY L10] + UI + serializer override input) then Leg 4 (merge 4 staged FA-4562-179-*, gate 257→261). Prior: §179 RS SPEC LEG DONE + BUILD LEG 1 DONE: the mandatory 4562 spec fetch found it SILENT on Form 4562 line 10 (carryover in)/line 13 (out) + a WRONG line-12 formula → Ken chose "amend RS spec first, then build inline" → NEW `load_4562_section179_carryover.py` AMENDS the existing multi-entity 4562 ADDITIVELY (entity_types untouched): fact section_179_carryover_prior + L10/L13 + FIXED L12 (add 9+10, cap at 11) + enriched L11 (Individuals business-income def incl. W-2 wages 1040 L1a, w/o §179/§164(f)/NOL, MFJ combine) + R014 [L12=min(L9+L10,L11)]/R015 [L13=(L9+L10)−L12] + enriched R011 + D014 (proforma continuity) + 3 verbatim excerpts + 4 staged FA; math gate check_4562_section179_integrity.py ALL PASS; Ken-approved review walk → seeded onto 4562 v1 (DB facts 19→20/rules 10→12/lines 24→26/diags 8→9/tests 9→14) → deployed export HTTP 200 → canonical server/specs/form_4562_spec.json re-committed + flow_assertions_4562_section179_pending.json staged (RS 96a0dd6, tts e3f83b6). Law verified vs 2025 f4562.pdf (pymupdf) + i4562 (irs.gov), NOT memory. BUILD LEG 1 (model/migration): migration 0105 additive — Taxpayer.sec_179_carryover_prior (5th proforma carryforward; other 4 already typed) + TaxReturn.rolled_from_year/rolled_from_source (provenance pointer, snapshot semantics not a live FK); makemigrations --check clean; flow gate 257; tts bc9ead1. 2 BUILD DECISIONS LOCKED (DECISIONS.md): (1) line 11 = compute SUGGESTED (Σ Sch C + Σ Sch F net before §179 + W-2 wages L1a, MFJ combine, floor 0, YELLOW) + preparer OVERRIDE; (2) consumed carryover lands PRO-RATA across all §179 businesses. NEXT = Leg 2a §179 consumption compute+render (renderer.py:1748 + aggregate_depreciation + per-business §179 flow) then roll-forward+UI (Leg 2b/3) then tests+FA merge (Leg 4, gate 257→261). Architecture: snapshot-COPY (Ken-approved). Prior: Schedule J (farm income averaging) — UNIT COMPLETE — ALL 6 LEGS GREEN — tag `1040-schedule-j-complete`: build leg 6 (ASSERTIONS) merged the 8 staged FA-1040-SCHJ-01..08 into the active gate (**249 → 257**) via `merge_schedule_j_assertions.py` + a `_run_schj_assertion` runner (id-prefix dispatch in all 3 `test_flow_assertions.py` entry points, the `_run_schf_assertion` precedent) — PURE chain re-derivation (FA-02/03/07) + source-/constant-pins (FA-01/04/05/06) + no-silent-gap registry+blocker (FA-08); `test_flow_assertions.py` 257 passed, no app code change. Prior leg-5 (DIAGNOSTICS): `rules_schedule_j.py` (NEW) wires the 5 D_SJ_* (3 error/2 warning) + NEW pure `compute_regular_tax` for D_SJ_ELECT_HIGH; 12/12 + 5 rules seeded active. NEXT = Ken's call on the next unit (SPRINT_SCOPE NEXT-UP leads with Schedule C + SE + 8995 + 4562). Still tracked: deferred base-year QDCGT/SDTW statement pages (Sch J render follow-up). See the Schedule J row. Prior leg-1..4: **Schedule J — SPEC LEG COMPLETE — SEEDED + EXPORTED + STAGED (Ken-approved review walk): SCHEDULE_J seeded into RS (DB 65→66 forms, FA 232→240; 42 facts/20 rules/25 lines/5 diagnostics/10 scenarios/23 cited links + 8 FA), canonical `schedule_j_spec.json` committed + 8 FA staged in `flow_assertions_1040_schedule_j_pending.json`; math gate `check_schedule_j_integrity.py` ALL CHECKS PASS (independent SJ-T1 chain L23=31,982 + cell-by-cell constant cross-check); law verified vs the actual 2025 IRS PDFs (23-line chain + base-year rate schedules 2022/23/24 + base-year QDCGT + the identical-47-line Schedule D Tax Worksheet) → brief `_schedule_j_source_brief.md`; walk rulings Q-A reduce/Q-B half-up/Q-C 44-46/Q-D warning. NEXT = build leg 1 (ScheduleJ model + seed + merge base-year schedules into spine TAX_BRACKETS). See the Schedule J row. Prior: **Schedule F (1040 farm) — UNIT COMPLETE — ALL 6 LEGS GREEN — tag `1040-schedule-f-complete`**: leg 6 (assertions) merged the 8 staged FA-1040-SCHF-01..08 into the active gate (**241 → 249**) via `merge_schedule_f_assertions.py` + a new `_run_schf_assertion` runner (id-prefix dispatch in all 3 entry points, the `_run_k1_assertion` precedent) — PURE re-derivation via `compute_schedule_f` helpers for the cash math (FA-01 L34=L9−L33 + dual SE-1a/Sch1-L6 pins; FA-02 line-9 right-column sum=49,250; FA-03 L33 incl. other-expenses + line-14; FA-08 farm-optional min(⅔×gross,$7,240) + 2025 constants + 2026 unsupported), source-pins for the DB feeders (FA-04 depreciation→L14, FA-05 farm QBI→8995 L2 accrual-excluded, FA-06 CRP→SE 1b negative), no-silent-gap registry check FA-07. `test_flow_assertions.py` → **249 passed**; no app code change. **THE UNIT IS DONE — all 6 build legs green (seed/compute/render/input/diagnostics/assertions); tagged `1040-schedule-f-complete`.** NEXT = Schedule J (farm income averaging — its OWN RS spec, spec-first). Prior: **Schedule F (1040 farm) — BUILD LEG 5 (DIAGNOSTICS) COMPLETE + GREEN**: new `rules_schedule_f.py` wires the 8 D_SF_* (ACCRUAL/PASSIVE/ATRISK errors; 461L/CCC_ELECTION/CROPINS_DEFER/WEATHER_DEFER warnings; LOSS info) + the 2 D_SE_FARMOPT (2026 error / INELIG warning) via `RULES_SCHEDULE_F` in `runner.seed_builtin_rules`, each bridge-gated to the `compute_schedule_f` helpers (loss rules re-derive line 34 + skip accrual farms; SE rules gate on `farm_optional_supported`/`farm_optional_eligible`) with offending farm/proprietor names appended; **D_SF_461L = flat $100k per-farm-loss operational reminder (Ken-approved — §461(l) is aggregate)**; **`d_8995_001_above_threshold` made farm-aware** (`farms=bool(qbi_farms)` → no silent gap on a pure-farm-above-threshold return). 17/17 `test_schedule_f_diagnostics_leg.py` + flow gate 241 + Topic 8 + Schedule F regression 80/80 + `manage.py check` clean + 10 rules seeded active in the shared DB. NEXT = leg 6 (assertions — merge 8 staged FA, gate 241→249, tag `1040-schedule-f-complete`). Prior: **Schedule F (1040 farm) — BUILD LEG 4 (INPUT) COMPLETE + GREEN**: un-stubbed the `schedule_f` 1040 tab → `ScheduleF1040Section` per-farm card UI (the ScheduleC precedent) — header A-G (accrual→RED banner, EIN RED-validation, E-G tri-state) + Part I income (1c/9 YELLOW) + Part II expenses (direct-entry lines 10-31; L14 depreciation YELLOW from the 4562 engine) + line-32a-f nested other-expense rows (32/33/34/QBI YELLOW) + at-risk 36a/36b + SEHI/SE-retirement GREEN + the farm-optional-method election card (Schedule SE Part II, per farm-attached proprietor) + Σ-L34 → Sch 1 L6 total; dispatch branches `activeTab==="schedule_f"` on `isIndividual` (1040 model card vs the entity FormFieldValue `ScheduleFSection` — same tab id, separate plumbing, the `f1040sf`/`fschedf` split mirror); IndividualNav un-stub (no 1040 stub tabs left; STUB_COPY emptied) + `schedule_f_farms` collection dot + `D_SF_`/`D_SE_FARMOPT`→`schedule_f` routing wired ahead of leg 5. Pure frontend (CRUD/serializer exist from the seed leg); **tsc clean / vitest 201 (+5 Schedule F nav tests) / build clean.** NEXT = diagnostics leg (`rules_schedule_f.py` — 8 D_SF_* + 2 D_SE_FARMOPT + farm-aware `d_8995_001`). Prior: **Schedule F (1040 farm) — BUILD LEG 3 (RENDER) COMPLETE + GREEN**: `field_maps/f1040sf_2025.py` un-stubbed — the 89 generic AcroForm fields correlated to the cash-method lines by POSITION off the actual 2025 PDF (left-col f1_23-36 = lines 10-22, right-col f1_37-46 = 23-31, f1_47-58 = 32a-f, f1_59/60 = 33/34; comb B/D; radio-by-distinct-name "X"; no pre-printed parens) + `render_schedule_f_1040` (ONE COPY PER FARM, bridge-gated via `compute_schedule_f_lines`; line 14 ← 4562; line-32 a-f itemized rows; loss-only line-36 at-risk box; an ACCRUAL farm renders no face — RED-defer) + `f1040sf` registered in ACROFORM_FORM_IDS + a distinct manifest entry (same PDF as the 1120-S-entity `fschedf`) + packet append after Schedule C + page 2 (accrual Part III) stripped via `SKIP_PAGES["Schedule F"]`. 10/10 `test_schedule_f_render_leg.py` (map-vs-PDF existence/no-dup/full-coverage + face position sweeps incl. resale margin 1c / L14 depreciation / line-32 rows / loss `(25,000)` + at-risk box / header A-G; accrual-skip; 2-farm 2-copies; packet 1-page) + flow gate 241, no regression. NEXT = input leg (un-stub the `schedule_f` React tab). Prior: **Schedule F (1040 farm) — BUILD LEG 2 (COMPUTE) COMPLETE + GREEN**: `compute_schedule_f_family` wired into `compute_return` before the Schedule C family — per-farm cash chain → 5 model cols + accrual zero-gate; Schedule SE feed (Σ L34 → 1a, −Σ CRP → 1b, Σ L9 → farm-optional gross base); Schedule 1 line 6 = Σ farms' L34 (engage-gated + reflow, flipped direct→computed); farms folded into the Schedule C family's QBI pro-rata allocation as additional §199A businesses (½-SE + farm SEHI + farm SE-retirement → `ScheduleF.qbi_amount`; Sch 1 L16/L17 carry farm contributions); Form 8995 line 2 + `qbi_engaged(farms=)`; depreciation `flow_to=schedule_f` → ScheduleF.depreciation line 14 (migration 0102, §179 IN). 12/12 `test_schedule_f_orchestration.py` (FA-01..08 pipeline) + 14/14 pure + flow gate 241 + **Topic 8 regression 54/54 (no value moved)**. Also fixed the prior-session `schedule_se_spec.json` re-export drift — the 4 SE-FARMOPT scenarios it silently added (count trip-wire 5→9, SE scenario loop teaches the farm-optional inputs + null=RED, `compute_schedule_se_lines` emits line 14 = FARM_OPT_MAX). NEXT = render leg (un-stub `f1040sf_2025.py` + `render_schedule_f`). Prior: **Effort #5 — Schedule E page 2 — recipient K-1 full router — COMPLETE (all 6 legs green, tag `1040-k1-router-complete`)**. BUILD LEG 6 (ASSERTIONS) DONE + GREEN: merged the 7 staged FA-1040-K1-01..07 into the active gate (**234 → 241**) + **semantic-repointed FA-1040-SCHE-01 → line 41** via `scripts/merge_schedule_k1_assertions.py`; new `_run_k1_assertion` runner wired into all 3 `test_flow_assertions.py` entry points by an **id-prefix guard** (`FA-1040-K1-*`) — PURE re-derivation through a new pure core `schedule_e_p2_totals_from_rows()` (extracted from `schedule_e_p2_totals`; the DB wrapper delegates) for FA-01/02/03 (line 41 = 26+32+37; line 32 = (h+k)−(i+j) col g = 0; line 37 = (d+f)+(c+e) col c = 0; `_FakeK1` rows), source-pins on the cross-form feeders for FA-04/05/06 (intdiv 2b/3b/3a, Sch D 5/12, Sch SE 1065-only, 8995 2/6), no-silent-gap blocker check for FA-07; flow gate **241** + K-1 compute/render/diagnostics legs **53 passed — no regression** (the from_rows refactor is value-identical); `_FakeK1` bool-coercion gotcha fixed (bool ∈ int — don't Decimal-coerce material_participation). Prior leg: BUILD LEG 5 (DIAGNOSTICS) DONE + GREEN: new `rules_schedule_e_p2.py` wires the 10 D_K1_* (5 RED-defer errors + 3 warnings + 2 info) + 2 D_SCHE_* (REMIC line 39 / Form 4835 line 40) via `RULES_SCHEDULE_E_P2` in `runner.seed_builtin_rules`, each reusing the compute helpers — new shared `k1_sche_net()` in `compute_schedule_k1.py` (passive/PTP-loss bridge-gate refactored from the 3 inline `net=` sites) + `schedule_e_p2_totals` (flow info); offending-entity names appended to each finding; 18/18 `test_schedule_k1_diagnostics_leg.py` + flow gate 234 + K-1 compute leg (251 passed, no regression) + `manage.py check` clean + 12 rules seeded active in the shared DB; client routing `D_K1_`→`schedule_e_p2` already wired in leg 4. Prior leg: BUILD LEG 4 (INPUT) DONE + GREEN: un-stubbed the `schedule_e_p2` React tab + `ScheduleK1Section` (per-entity K-1 cards — source_type 1065/1120-S/1041 drives the IRS box labels + hides inapplicable fields; owner T/S/J; material-participation + PTP toggles; grouped Schedule-E / cross-form / v1-RED-defer money + flags; YELLOW computed-totals preview reading SCHEDULE_E line 32/37/41 from field_values; RED entry-level EIN validation) + `IndividualNav` un-stub (collection dot + `D_K1_`→tab routing) + `ScheduleK1Row` type. Pure frontend (CRUD/serializer exist from the seed leg); tsc clean / vitest 196 / build clean. NEXT = diagnostics leg (`rules_schedule_e_p2.py`). Prior leg: BUILD LEG 3 (RENDER): `f1040se_2025.py` page-2 field map (153 entries, 0 missing/dup vs the actual PDF) + `render_schedule_e` Parts II/III/V (per-entity line 28/33 rows + per-column totals + **line 41** + page-2 header; lines 31/36 abs-injected for the pre-printed parens) + conditional `SKIP_PAGES["Schedule E"]` un-skip via `schedule_e_p2_has_content` + Sch D 5/12 K-1 face (direct + K-1) + 8995 line-1 K-1 §199A QBI rows; new pure `schedule_e_p2_rows()` FACE decomposition reconciles to the compute totals (single source of truth); 18/18 `test_schedule_k1_render_leg.py` + flow gate 234 + K-1 compute/topic8/topic9 (99) green; `manage.py check` clean. Row caps line 28 ×4 / line 33 ×2 / 8995 line 1 ×5 (overflow → totals, flagged). NEXT = input leg (un-stub `schedule_e_p2` UI). Prior leg: RS SPEC + seed + compute (SCHEDULE_K1 seeded; SCHEDULE_E amended page 2; RS DB 63→64, FA 217→224; `schedule_k1_spec.json` + 7 FA staged; **summary is line 41** not 40 vs the real 2025 PDF; §199A 20Z/17V/14I → 8995, SE 1065 14A → Sch SE). Prior: **Effort #4 (Retirement & Misc reorg) COMPLETE — local, NOT pushed. Part A: Social Security split (SSA-1099 card → new `SocialSecuritySection` under the un-stubbed `social_security` tab; pure frontend). Part B: **Form W-2G (certain gambling winnings §61) — ALL 5 LEGS GREEN** (seed `8bfe4c9` / compute `2cf14e7` / diagnostics `a3ff8b6` / assertions `9eaf3d6` / input `16630c1`): box 1 winnings → Sch 1 line 8b (Σ box1 + the non-W-2G `other_gambling_winnings` fact), box 4 fed WH → 1040 line 25c (a ROSTER = Form 8959 line 24 + Σ W-2G box4, NOT 25b); FormW2G doc model (mig 0096/RLS 0097); `MiscIncomeSection` (W-2G doc cards + non-W-2G fact, 8h/8i/8z stay on Schedule 1); no render face (input doc, the 1099-G precedent). Flow gate 234, topic8 49 (no regression), client tsc/vitest 193/build clean. Prior: **Credit-claim Leg B.4 — Taxpayer Info smart SSN + calculated age + IP-PIN (built+reviewed, committed local `568fb33`..`fd0da8d`, NOT pushed): `Taxpayer.identity_protection_pin`/`spouse_identity_protection_pin` (mig 0094, additive) → rendered onto the 1040 signature block (HEADER_MAP `f2_41`/`f2_43`, verified vs the real PDF); read-only `age`/`spouse_age` serializer fields (YELLOW); `TaxpayerInfoSection` extracted from FormEditor.tsx; smart taxpayer/spouse SSN UI (hyphenate/inline-400/ITIN tag) reusing the Leg-0 backend derivation. NO compute/flow-assertion change (IP-PIN+age are metadata; render IS done). vitest 186/186, mig-check clean. Found a PRE-EXISTING Sch 1-A tips-deduction bug (L13=$0 not $5k) — tracked separately, not a B.4 regression. Prior: **Credit-claim Leg B.3 — per-dependent AOTC checkbox: new nullable `EducationStudent.dependent` FK (mig 0093, SET_NULL) auto-links an `owner=dependent` Form 8863 student; `D_CREDIT_AOTC` unified-engine gate (barred/no-expenses→error, MAGI-over-ceiling→info); stale 8863 ❌ row corrected. Prior: UI Batch #2 effort #3 — Interest/Dividend input UI redesigned to a row-per-payer PayerGrid + accordion; additive non-computational `owner` T/S/J field (mig 0092); Sch B fields relocated to the Interest page + `sch_b` tab removed; `D_INTDIV_011` W/H-no-EIN warning. NO change to any form's compute/render/assertion coverage — built+reviewed, committed on `main` locally, awaiting Ken's push. Prior: **W-2 UNIT 3 COMPLETE — Form 7206 SEHI §162(l): 2% S-corp shareholder Box-5 SEHI + the Schedule C ½-SE/SEP limit fix → Schedule 1 line 17; FORM_7206 seeded RS DB 60→61 / FA 200→206; new `compute_7206.py` + `rules_7206.py` (6 diags) + 6 FA; flow gate 217→223; NO migration. Prior: TOPIC 9 SPEC LEG COMPLETE — seeded+exported+staged (Ken approved walk; RS DB 44→46; 12 FA-1040-SCHD staged; sibling D_INTDIV_001/002 + D_8995_002 retired in deployed RS); NEXT = build leg 1 (CapitalTransaction model + seeds). See the Schedule D row — prior: TOPIC 8 BUILD LEG 2 (compute leg) DONE `776ec8b` — NEW `compute_schedule_c.py` (Sch C per-business chain → model columns; Sch SE per-proprietor AUTO-CREATED rows, year-keyed wage base 176,100/184,500, W-2-SS cap; 8995 simplified QBI year-keyed thresholds + pro-rata reductions → 1040 L13, above-threshold BLANKS + D_8995_001; 8959 threshold-reduced-by-wages → Sch 2 L11 + 25c, engage incl. withholding-reconciliation) + `aggregate_depreciation` flow_to=schedule_c (migration 0060, line 13 incl. §179) + feeder repoints Sch 1 L3/15/16/17, Sch 2 L4/11, 1040 L13/25c (engage-gated, override escape hatch) + 7 REDs `rules_schedule_c.py` (D_SC_001/002/003/007, D_SE_003, D_8995_001, D_8959_002); **48/48 `test_topic8_compute_leg.py`** (24 spec scenarios pure + 13 pipeline incl. depreciation→L13 + MFJ-both-SE + clean-W-2-untouched), **flow gate 108**, regression 76 passed; FLAGGED: 8995 L11 vs Sch 1-A 13b (Ken pending) + the 8959 engage widening; **render leg next** — prior: BUILD LEG 1 (seed leg) DONE — models 0058 / RLS 0059 / 4 seeds in build.sh / manifest / CRUD; 33/33 `test_topic8_seed_leg.py`, flow gate 108 — prior: SPEC LEG seeded + exported — spec-first probe found Schedule C/SE absent in RS (404) + Form 8995 a wrong K-1-oriented draft → re-author; Ken-confirmed 4 kickoff scope decisions (COGS Part III in, home office simplified inline, multiple Schedule C, **Form 8959 in**); ALL constants verified vs IRS/SSA (SE wage base 2025 $176,100 / 2026 $184,500; 8959 $250k/$125k/$200k; 8995 thresholds 2025 $394,600/$197,300 & 2026 $403,500/$201,775/$201,750; home office $5×300). **SPEC LEG DONE 2026-06-12:** the 4-form loader `load_1040_schedule_c.py` authored (RS `aac4a38` — SCHEDULE_C 24f/9r/56L/7d/7s + SCHEDULE_SE 17/12/24/4/5 + **8995 RE-AUTHORED** 13/8/21/5/7 [`_retire_stale_8995` drops the wrong stub on seed] + 8959 13/6/24/4/5; 14 flow assertions; 6 new sources + RP 2025-32 §4.26 excerpt; 4 `requires_human_review` walk items), `READY_TO_SEED=False` (guard refuses, **zero DB writes, RS DB still 41 forms**); `check_topic8_integrity.py` **GREEN — ALL CHECKS PASS** (RS `338a373`). **Ken APPROVED the walk in-session → FLIPPED + SEEDED (RS `26e3244`): RS DB 41→44 forms, FlowAssertions 91→105** (SCHEDULE_C/SCHEDULE_SE/8959 created, **8995 RE-AUTHORED** — `_retire_stale_8995` deleted 28 stale stub rows; all rules cited); **deployed exports verified** (HTTP 200 + counts + 8995 re-author pins) → **canonical `server/specs/{schedule_c,schedule_se,8995,8959}_spec.json` committed + 14 assertions staged** in `flow_assertions_1040_topic8_pending.json` (active 1040 gate 86, flow gate 108 — no tts-tax-app code touched). next = **build leg 1 (seed leg)**: the multi-business Schedule C FK model + seed commands + f1040sc/f1040sse/f8959 manifest; then compute→render→input→diagnostics→assertions per form. **Prior — TOPIC 7 (EIC) COMPLETE — tag `1040-eic-complete`** — LEG 5 (diagnostics) wired the 10 deferred rules into `rules_eic.py`/`RULES_EIC` [D_EIC_002/004/008/010/011/014 + D_SEI_001 + D_8867_001/002 + D_8862_001 → **20 EIC-family diagnostics**: 16 D_EIC + 1 D_SEI + 2 D_8867 + 1 D_8862], all reusing the `compute_eic` pure helpers; **D_8867_001 uses the BROAD §6695(g) covered-credit gate** (EIC engaged OR L27>0 / CTC L19·L28 / HOH / AOTC L13) so existing CTC/HOH returns surface the unanswered-DD error; **D_EIC_014 sourced from the seeded 8862 `part_v` row**; **`_sei_age_answers` relocated `renderer.py`→`compute_eic.py`** (bridge-gate for D_SEI_001, render byte-identical); 26/26 `test_topic7_diagnostics_leg.py`, regression 151 passed, dev DB re-seeded. LEG 6 (assertions) merged the 9 staged FA-1040-EIC-01..09 (**gate 77→86**) via `scripts/merge_eic_assertions.py` + new `_run_eic_assertion` (PURE re-derivation + source inspection) wired into all 3 entry points → flow gate **108 passed**. NO migration. Prior — **Topic 7 (EIC) BUILD LEG 4 (input leg) COMPLETE** — `TaxpayerSerializer` +19 EIC facts (**trip-wire 60→79**) + `DependentSerializer` +`eic_qualifying_child`/`is_full_time_student`; **`views.py` dependents handlers now `_recompute_1040`** (latent CTC line-19 bug closed alongside EIC line-27); **`compute_eic.py` backfills the 8867/8862 rows when engaged** (the one gated-file touch — pure row-creation, no math; flow gate stayed 99); client **`eic` tab** (`EICSection` — 16 Yes/No/Unanswered tri-state selects [GREEN once answered, neutral while unanswered; REDs live in the diagnostics leg] + 4 money fields + Dependent EIC-flag table) followed by a `StandardSection` for the 10 seeded 8867/8862 sections. **9/9** in `test_topic7_input_leg.py` (facts round-trip + engage + drive L27a 4,328 through the real routes; dependent PATCH recomputes + Schedule EIC attaches; 8867/8862 rows backfilled + editable via /fields/); **flow gate 99**, compute+render leg 52, `test_1040` 22/22 (alone), `tsc`/`build` clean. NO migration. Next: diagnostics leg. Prior — **Topic 7 (EIC) BUILD LEG 3 (render leg) COMPLETE** — three EIC faces + the 1040_EIC worksheet statement page, strictly from the specs (Topic 5 precedent): **`field_maps/f1040sei_2025`** (Schedule EIC — model-driven per-child columns from the Dependent rows; gate ≥1 FLAGGED QC; 4-digit-year cells; 4a/4b derived via `_sei_age_answers`; page-2 instructions stripped) + **`f8867_2025`** (data-map — 12 seeded three-state answers → `{line}_yes/_no` boxes + DERIVED EIC/CTC/HOH header boxes; gate = engaged OR any answer) + **`f8862_2025`** (data-map — line-1 year + which-credit boxes; gate = disallowed AND not math-error) + **`render_eic_worksheet_statement`** (bespoke ReportLab statement page, `eic_engaged` gate, reached rows only); three form_ids in ACROFORM_FORM_IDS; packet Schedule EIC(43)→EIC worksheet→8862(43A)→[8812]→8867(70). **KEY:** pymupdf `widget.field_name` is DISTINCT per Yes/No kid → each box its own map key (the filler draws "X" when truthy; on-states irrelevant). **28/28** in `test_topic7_render_leg.py`; **flow gate 99**; render regression `test_topic5_render_leg` 18 + schb/sch123/8812/sch1a/f1040-header 214. NO migration / NO compute change. Next: input leg. Prior — **Topic 7 (EIC) BUILD LEG 2 (compute leg) COMPLETE** — **`apps/returns/compute_eic.py`** built from `eic_spec.json`: Step-5/WS-B earned income + `eic_table_lookup` ($50-bracket midpoint ROUND_HALF_UP, year-keyed `EIC_PARAMS` {2025,2026} verbatim — published TY2026 0-QC **664** not 663.42, 3+QC **8231**) + investment-income §32(i) gate + lower-of-AGI → **1040 line 27a** (override-respecting); wired into `compute_return` AFTER the first formula pass + `compute_sch_8812` (AGI/1z final), **"27" added to the 19/28 DB-refresh** so 27a → 32 → 33/34/37. **ENGAGE gate** `eic_engaged` (flagged QC / answered EIC fact / SE-investment amount) — non-engaged returns keep the legacy `eitc_claimed` and fire zero D_EIC (no false REDs / zero regression). **`rules_eic.py`** 10 credit-zeroing+no-silent-gap REDs (D_EIC_001/003/005/006/007/009/012/013/015/016) reuse the compute helpers (bridge-gate); **deferred** the 6 info/warn + D_8867/D_8862/D_SEI to the diagnostics leg. NO migration (0057), NO seed flip (27 already computed). **23/23** in `test_topic7_compute_leg.py` (8 PURE math + 3 edge pins + 8 PIPELINE gates + 3 wiring/engage/registration); **flow gate 99**; regression `test_1040` 22/22 + topic5/8812 green; dev DB re-seeded (10 D_EIC active). **8812 COUPLING REPOINTED (Ken-approved):** `compute_eic` now runs before `compute_sch_8812`; 8812 Part II-B L_24 reads the computed line 27a (face = Sch 3 L11 + 1040 L27), fallback to `eitc_claimed` only when line 27 is blank — regression-safe, `test_8812_part_iib_line24_reads_engine_27a` + TS14b green. Next: render leg (Schedule EIC/8867/8862 faces + 1040_EIC statement page). Prior — **Topic 7 (EIC) BUILD LEG 1 (seed leg) COMPLETE** — **migration 0057** [additive, applied to dev DB: `Dependent` +`eic_qualifying_child`(nullable override)/`is_full_time_student`; `Taxpayer` +20 EIC return-level facts — **`nontaxable_combat_pay` REUSED from the 8812 ACTC block, NOT duplicated** (same W-2 box 12 code Q amount; makemigrations flagged the would-be Alter); the 16 eligibility/election booleans are nullable `default=None` (three-state, fail-safe gates)]; **seeds in build.sh** `seed_1040_eic` [1040_EIC pseudo-form, 18 computed lines, renders as a STATEMENT page] + `seed_schedule_eic` [7 model-driven per-child lines] + `seed_8867` [12 BOOLEAN] + `seed_8862` [6]; **8867/8862 preparer answers are seeded FormFieldValue rows** (Schedule-B-Part-III precedent), NOT Taxpayer fields, so D_8867_001 can detect unanswered; **manifest** +`f1040sei`(2pg)/`f8867`(2pg)/`f8862`(3pg) downloaded + SHA + acroform-verified; **13/13 in `test_topic7_seed_leg.py`** (structure vs each spec line_map + computed sets/field types + idempotency + a spec-driven trip-wire pinning every stored return-level fact to a Taxpayer field); `makemigrations --check` clean; **flow gate 99 passed** (seed-leg precedent — no gated compute/render). `eic_childless_age_qualifies`/`eic_num_qualifying_children` are DERIVED at the compute leg (not stored). Next: compute leg `compute_eic.py` + EIC_PARAMS + the §32 gates. Prior — **Topic 7 (EIC + 8867/8862) SPEC LEG COMPLETE — seeded + exported + staged** [Ken approved review walk in-session]: RS `load_1040_eic.py` (commits `48e1fef` author / `5051f81` math gate / `00a550c` flip+seed) seeded 1040_EIC [computational pseudo-form: Step-5/WS-A/WS-B earned income + EIC-Table $50-midpoint ROUND_HALF_UP lookup + lower-of-AGI rule + Pub 596 Worksheet-1 investment income + Rules-for-Everyone/childless gates → 1040 L27a; **year-keyed `EIC_PARAMS` both years**] + SCHEDULE_EIC (model-driven per-child face) + 8867/8862 (data-map faces) + 9 flow assertions FA-1040-EIC-01..09; `check_eic_integrity.py` **ALL PASS**; **RS DB 37→41 forms, FA 82→91**; canonical `server/specs/{eic,schedule_eic,8867,8862}_spec.json` committed + 9 assertions STAGED in `flow_assertions_1040_eic_pending.json` (active gate stays 77, flow gate **99 passed**). Walk outcome: WS-B SE sourcing = Sch 1 L3 − L15. EIC REUSES `Dependent` (no new doc model). Next: build legs (seed→compute→render→input→diagnostics→assertions). Prior — **Topic 6 (CTC/ACTC, Form 8812) COMPLETE — tag `1040-8812-complete`** [DoD-CLOSEOUT ✅]: verification pass, NO compute/render/migration change — verified all 30 rules (R001-R030) match `sch_8812_spec.json` (RS lookup key `SCH_8812`, not `8812` which 404s); constants $2,200/$1,700 confirmed per-year independently (2025 OBBBA §70104 / 2026 RP 2025-32 §4.05, `_LATEST_VERIFIED_YEAR=2026`); R012 ceil-phaseout correct; **NEW `test_sch_8812_phaseout_boundaries.py`** (6 tests — full / ceil(+$1,+$1k,+$1,001) / partial / R014-STOP / deep-zero, BOTH 2025+2026, BOTH thresholds, through the real `compute_sch_8812`); render verified (3 scenarios); `test_sch_8812_scenarios.py` re-pointed from an outside-repo absolute path to the in-repo canonical spec (byte-identical 18 tests); **flow gate 99 passed** (13 CTC assertions intact: 12 FA-1040-CTC + TI-1040-CTC-A). **8812 diagnostics DEFERRED** (Ken "complete now, track rest" — spec has 12, only D_1040_008 wired; D009 Worksheet-B / D010 Add'l-Medicare / D011 RRTA are the no-silent-gap ones to prioritize). Next topic: Topic 7 (EIC + Form 8867). Prior — **Topic 5 (Retirement Income) COMPLETE — tag `1040-retirement-complete`** [BUILD LEG 6 — ASSERTIONS LEG ✅]: merged the 7 staged FA-1040-RET-01..07 into `flow_assertions_1040.json` (active 70 → 77) via `scripts/merge_retirement_assertions.py`; new `_run_retirement_assertion` in `test_flow_assertions.py` (PURE re-derivation through compute_retirement + source inspection, `_FakeRetDoc`, the `_run_intdiv` precedent) wired into all three entry points keyed on `form in ("1040_RETIREMENT","5329")`; kinds sum_check (4b/5b/25b) / formula_check (5329 L4→Sch2 L8; SS ws_18→6b) / constants_check (§86 base/tier, QSS≠MFJ) / gating_check (6 blockers → registered RED + 5b blank); source-pin gotcha — split-line 4b/5b `_set_field_value` matches `'tax_return, "4b"'` not the call-open form; **`pytest test_flow_assertions.py` → 99 passed** (92 + 7, zero regressions); NO migration/NO compute-render change. **Topic 5 ALL LEGS GREEN.** Next topic: Topic 6 (Form 8812 full DoD — verify + phaseout boundary tests + render). Prior — **Topic 5 BUILD LEG 5 — DIAGNOSTICS LEG ✅**: **D_5329_001** (Form 5329 line 2 exception amount > line 1 early-in-income → error) added to `rules_retirement.py`/`RULES_RETIREMENT` — reuses `RetirementAgg.early_distributions` vs `Taxpayer.exception_amount_5329` (bridge-gate); the retirement family is now 8 active (7 D_RET + 1 D_5329); audit closed (the 7 retirement_spec diags = D_RET_001..007, the 1 5329_spec diag = D_5329_001); 6/6 in `test_topic5_diagnostics_leg.py`, flow gate 92, dev DB re-seeded; NO migration/NO gated files. Resume = build leg 6 (assertions — merge 7 staged 70→77, add a `_run_retirement_assertion` runner kind, tag `1040-retirement-complete`, closes Topic 5). Prior — **Topic 5 (Retirement Income) BUILD LEG 4 — INPUT LEG ✅**: new **Retirement Income** tab (`retirement_income`) → `RetirementIncomeSection` (per-doc 1099-R box CRUD auto-save-on-blur + SSA-1099/5329 assertion card PATCHing the six 0056 facts); `retirement_distributions` added to `TaxReturnSerializer` nested read + the six facts to `TaxpayerSerializer` (trip-wire 54→60); box 2a uses a nullable money input (blank=NULL Simplified-Method gate), the non-null boxes send "0"; 10/10 in `test_topic5_input_leg.py` [CRUD→4a/4b/5a/5b/25b through the real route; blank box 2a blanks 5b; facts→6a/6b 17,000 + Sch 2 L8 2,000/1,200]; flow gate 92, tsc/build clean, serializer regression 16/16; NO migration/NO gated files. Resume = build leg 5 (diagnostics — D_5329_001) then leg 6 (assertions — merge 7 staged 70→77, tag `1040-retirement-complete`). Prior — **Topic 5 (Retirement Income) BUILD LEG 3 — RENDER LEG ✅**: Form 5329 Part I face (`f5329` manifest + `field_maps/f5329_2025.py` + `render_5329`, gated on the `form_5329_generated` compute helper, pages 2-3 stripped) + `render_ss_worksheet_statement` (ws_1..ws_18 reached-lines-only, gate ws_1>0); packet seq 29 (Sch B < 5329 < SS worksheet < 8812); 18/18 in `test_topic5_render_leg.py`, flow gate 92, render regression 192 passed; NO migration / NO compute change. Prior — **Topic 5 (Retirement Income) SPEC LEG COMPLETE — seeded + exported, NOT yet tagged (build legs pending)**: math gate `check_retirement_integrity.py` green [independent recompute of the 18-line SS Benefits Worksheet + 5329 Part I + 1099-R aggregation, RS `a92f253`] → Ken review walk approved [TY2026 §86/5329 constants confirmed non-indexed = NO `_constants_for_year`; R-RET-CODE J/T wording tightened to RED-out-of-v1] → flip + seed [RS `af54dcb`, DB 35→37 forms, FA 75→82] → deployed exports verified → canonical `retirement_spec.json` + `5329_spec.json` + 7 staged assertions committed [tts-tax-app `e45f7c3`]; active 1040 gate untouched at 70, flow gate 92 passed. Next: build leg 1 (RetirementDistribution model + SSA fields + f5329 manifest). Prior — **Topic 4 (Schedule 1-A) COMPLETE — tag `1040-sch1a-complete`**: DoD-closeout, not a fresh build (build landed 2026-06-10). Closed the one open DoD item — senior age disqualification proven COMPUTATIONAL via L_36a/L_36b (not a warning) with 3 new dedicated tests in test_sch_1a_scenarios.py [too-young single → L_35 full $6,000 but L_36a=0; born-before-Jan-2 boundary pair 1961-01-01 vs -02; MFJ spouse-only → L_36b=0/L_36a survives]; `_build_scenario` += optional dob_override/spouse_dob_override; 16/16 in file, flow gate 92; NO compute change. Also: **SUITE_CONTRACT.md** created (repo root) — live DB introspection confirms tax app + portal share clients_client/clients_entity, sherpa-1099 keeps its own filers/recipients/forms_1099 (own `tenants` tenancy, no FK into clients_*), check-in has no client table here; master/snapshot rule + primary-SSN gap documented. Prior — **Topic 3 COMPLETE — ASSERTIONS LEG ✅**: merged the 12 staged INTDIV/SCHB assertions into flow_assertions_1040.json [58→70 active, pending emptied]; new `_run_intdiv_assertion`/`_run_schb_assertion` runners in test_flow_assertions.py [PURE re-derivation through compute_intdiv helpers + source inspection — DB-backed coverage stays in test_intdiv_scenarios.py]; wired into all three runner entry points; flow gate **92 passed**; tagged `1040-intdiv-complete`. Prior — **DIAGNOSTICS LEG ✅**: rules_intdiv.py D_INTDIV_005..010 (qualified>ordinary error; doc-adjustments-exceed-income + liquidation warnings; foreign-tax/PAB/§199A info) + new apps/diagnostics/rules_schb.py D_SCHB_001..007 (Part III three-state chain reusing part_iii_required + seller-financed buyer-SSN error + 8815/foreign-trust/FBAR), registered via RULES_SCHB in runner.seed_builtin_rules; `_DEFERRED_DIAGNOSTICS` emptied so ID-G4/SB-DG1/SB-DG2/SB-DG3 assert through the real pipeline; NO migration (0053 carried every fact); 42/42 (test_topic3_diagnostics_leg.py 20 + test_intdiv_scenarios 22) + flow 80/80; dev DB seeded (10 D_INTDIV + 7 D_SCHB active). Next: assertions leg (merge 12 staged → flow_assertions_1040.json 58→70, new runner kinds, tag 1040-intdiv-complete). Prior — INPUT LEG (`5be74a8`): 1099-DIV tab + Schedule B tab + four 0053 facts, 7/7. COMPUTE LEG (`783e7ea`): compute_intdiv.py + migration 0053 + D_INTDIV_001..004 + bridge retired, 21/21. RENDER LEG (`b7acd95`+`7185547`): f1040sb map + render_sch_b + QDCGT statement + 7b box, 22/22)*

**Form-unit gate (1040 campaign):** a form is DONE only when all four legs are green —
**Input (data-entry UI) → Compute → Render → Flow assertions.**
**As of 2026-06-10, the DoD checklists in `SPRINT_SCOPE.md` (repo root) are the
completion bar per topic** — a form/topic is not marked complete here until every
DoD item is green, regardless of what already exists.

Legend: ✅ green · 🟡 partial · ❌ not built · stub = placeholder only

---

## 1040 Family (campaign focus)

| Form | RS Spec | Input (UI) | Compute | Render | Flow asserts | Notes |
|---|---|---|---|---|---|---|
| **Form 1040 core** | ✅ SPINE seeded 2026-06-10 (45 rules, 72 lines, 16 diagnostics, 33 scenarios; exported as `1040_spine_spec.json`) | ✅ Leg 5 06-10: all 49 Taxpayer input facts serializer-exposed + tabbed UI (Taxpayer Info std-ded block + decedent/HOH, Sch 1-A, Credits & 8812, Payments) + **1040 form-line editor on Tax Summary tab** (Quality Rule 5) with RED/YELLOW/GREEN | ✅ Legs 1+2+3+4 06-10: Tax Table below $100K + TY hard stop + full std-ded chain (migration 0047) + full line plumbing (seed 19→55 lines, FORMULAS_1040 per R-INC/CR/PAY/REF, migration 0048 est payments) + **diagnostics framework** — D_1040_001..016 seeded via `seed_rules` (rules_1040.py), computational bridge gate (3a/7a blanks line 16), migration 0049 diagnostic-fact fields. **Leg 5 fixes: SCH_1A FormFieldValue backfill + form-scoped line lookups** | ✅ **Leg 6 06-10**: all 55 lines mapped (56 position assertions incl. synthesized 11b AGI carry), **HEADER_MAP repaired** (name/SSN/filing-status/occupations were on wrong widgets since the 2025 redesign), 12a-12d boxes + dependents grid + MFS/HOH-QSS names + preparer block render, render-path cross-form collision fixed (`test_f1040_2025_header_render.py` 12 tests) | ✅ **all 16 spine assertions wired (46 total)** | **TOPIC 1 COMPLETE 2026-06-10 (tag `1040-spine-complete`)** — all six legs + DoD residue cleared: digital-asset question (D_1040_017/0050/c1_10), D_1040_004 conservative interim kept, TY2026 constants Ken-verified (RP 2025-32; Tax Table interim accepted, re-pin ~Dec 2026). Per-line status: `line_status_1040.md`. E2E-1..3 + DG-1..6 green spec-driven. |
| **Schedule 8812 (CTC/ACTC/ODC)** | ✅ seeded in RS + exported | ✅ Leg 5 06-10: all 18 fields on the Credits & 8812 tab | ✅ 30 rules | ✅ | ✅ 13 | Compute/render/assertions complete (2026-05-28); input leg closed 2026-06-10 PM #9. Dependents UI ✅. Worksheet B deferred. **TY2026 amounts verified PM #11** (RP 2025-32 §4.05: $2,200/$1,700 — explicit 2026 entry + pins; `_LATEST_VERIFIED_YEAR` fallback). **TOPIC 6 COMPLETE 2026-06-11 (tag `1040-8812-complete`)** — DoD-closeout: 30 rules re-verified vs spec; NEW `test_sch_8812_phaseout_boundaries.py` (full/ceil/partial/STOP/zero × 2025+2026 × both thresholds through real compute); scenario test re-pointed to in-repo canonical spec; 13 CTC flow assertions green (gate 99). **8812 diagnostics leg DEFERRED** (Ken-approved tracked follow-up — spec has 12, 1 wired; prioritize D009/D010/D011). |
| **Schedule 1-A (OBBBA)** | ✅ seeded in RS (Ken approved 2026-06-10) + exported as canonical `sch_1a_spec.json` | ✅ Leg 5 06-10: Sch 1-A tab (tips attestation + amounts, overtime amounts, vehicle row entry w/ VIN validation) | ✅ all 6 parts | ✅ | ✅ 17 | **Part IV (car loan/QPVLI) built 2026-06-10**: CarLoanVehicle model (VIN-validated rows) + ceil-×$200 phaseout + VIN table render + 14 scenarios. All 21 Ken-reviewed RS scenarios run spec-driven (`test_sch_1a_rs_spec_scenarios.py`). **Leg 5 fixed: SCH_1A rows now backfilled on compute (UI-created returns previously never persisted/printed the schedule).** **TY2026 constants verified PM #11** (statutory non-indexed; RP 2025-32 adjusts none of the four parts; age cutoff rolls). **TOPIC 4 COMPLETE 2026-06-11 (tag `1040-sch1a-complete`)** — DoD-closeout: senior age disqualification proven COMPUTATIONAL via L_36a/L_36b with 3 dedicated tests (too-young single L_35=$6,000 but L_36a=0; born-before-Jan-2 boundary pair; MFJ spouse-only L_36b=0); 16/16 in test_sch_1a_scenarios.py; no compute change. |
| Schedule 1 | ✅ seeded 2026-06-10 (`sch_1_spec.json`) | ✅ 06-11 Schedule 1 tab (StandardSection; 70 lines) | ✅ 06-11 compute_sch123 (9/10/25/26 + S1.18 YELLOW feeder ← 1099-INT box 2; L10→1040 L8, L26→L10 computed) | ✅ 06-11 f1040s1 map + render_sch_1 (8a/8d/8s abs-in-parens, 7-repaid box, 19b comb) | ✅ 4 wired 06-11 | **TOPIC 2 COMPLETE (tag `1040-sch123-complete`)**. Diagnostics D_SCH1_001..006 ✅. 14 spec scenarios green (S1-T2 loss-year pin -21,000). S1.18 feeder pinned by compute tests (post-spec Ken-approved computed line — no RS assertion of its own) |
| Schedule 2 | ✅ seeded 2026-06-10 (`sch_2_spec.json`) | ✅ 06-11 Schedule 2 tab (1e/1f source selects; 54 lines) | ✅ 06-11 compute_sch123 (1z/3/7/18/21 **L20 EXCLUDED** — S2-T2 pins 1,884 + S2.13 YELLOW feeder ← W-2 box 12 A/B/M/N; L3→1040 L17, L21→L23) | ✅ 06-11 f1040s2 map (root `form1[0]`) + render_sch_2 (source boxes i-iv, exemption boxes) | ✅ 5 wired 06-11 (FA-SCH2-05 = the L20-exclusion pin) | **COMPLETE**. Diagnostics D_SCH2_001..005 ✅. **8812 L22 re-pointed FACE-VERBATIM ← S2.5/S2.6/S2.13 (NOT S2.4)** |
| Schedule 3 | ✅ seeded 2026-06-10 (`sch_3_spec.json`) | ✅ 06-11 Schedule 3 tab (35 lines) | ✅ 06-11 compute_sch123 (7/8/14/15; 6l signed; L8→1040 L20, L15→L31; CLW-A pre-CTC read for 8812 L13) | ✅ 06-11 f1040s3 map (6z_type = f2-named page-1 widget) + render_sch_3 | ✅ 4 wired 06-11 | **COMPLETE**. Diagnostics D_SCH3_001..003 ✅. ONE page; 6e reserved |
| Schedule B | ✅ seeded 2026-06-11 (`sch_b_spec.json` — Ken approved) | ✅ input leg 06-11: **Schedule B tab** (line 3 8815 + Part III 7a/7b/8 three-state via the default StandardSection branch; lines 1/5/2/4/6 not enterable — payer listings model-driven). DividendIncome model + DIV CRUD from the seed leg (`8e34222`) | ✅ 06-11 compute leg: lines 2/4/6 computed in compute_intdiv_aggregation (L2 = per-doc nets; L4 = L2 − L3(8815) → 1040 2b structural tie; L6 → 3b); R-SB-04/05 gates as pure fns (`schedule_b_required`/`part_iii_required`) for the render/diagnostics legs. **Diagnostics leg 06-11: D_SCHB_001..007 ✅ (rules_schb.py — Part III three-state chain reusing part_iii_required + seller-financed buyer SSN + 8815/foreign-trust/FBAR)** | ✅ 06-11 (`b7acd95`): f1040sb map (model-driven payer slots + Part III three-state boxes) + render_sch_b (seller-first; Nominee/Accrued/ABP rows reconcile to L2; overflow→continuation) + `schedule_b_required` gate + packet seq 08 | ✅ **2 wired 06-11** (FA-1040-SCHB-01 L4=L2−L3 & ==1040 2b structural tie; FA-1040-SCHB-02 required-use/Part III truth table + L6==3b) | **Topic 3 COMPLETE** (tag `1040-intdiv-complete`). All 6 SB spec scenarios green through compute_return (SB-T1 2,875/2,375 tie pinned; SB-DG1/2/3 fire D_SCHB_001/002/006). D_SCHB_001..007 ✅ diagnostics leg |
| QDCGT worksheet + 1099-INT/DIV aggregation (1040_INTDIV) | ✅ seeded 2026-06-11 (`intdiv_spec.json` — Ken approved incl. 6 judgment items) | ✅ input leg 06-11: **1099-DIV tab** (DividendIncomeSection: full box surface 1a-16 + nominee, auto-save-on-blur) + the four 0053 facts on the dividends surface (capital_gain_distributions_only assertion + 3c/7b Form-8814 child-income facts, per-tab Taxpayer PATCH); serializer trip-wire 50 → 54; `dividend_incomes` in detail payload; 7/7 API-path tests | ✅ 06-11 compute leg: `compute_intdiv.py` — R-AGG-2A/2B/3A/3B/7A/25B rosters (supersede spine; 1040 3a/3b now computed feeders, 7 written only on the Exception-1 path) + `compute_qdcgt_worksheet` (WS1-25 pure; WS18/21 HALF-UP; WS22/24 = spine tax method) + `route_line_16` gate INSIDE _compute_1040_tax (**bridge retired**: D_1040_001/R-TAX-07/DG-1/FA-1040-SPINE-15 deleted in RS, new canonical spine spec; D_INTDIV_001..004 live as the blocked-path REDs) + ws-row persist/clear. **Diagnostics leg 06-11: D_INTDIV_005..010 ✅ (rules_intdiv.py — qualified>ordinary error, doc-adjustments + liquidation warnings, foreign-tax/PAB/§199A info)** | ✅ 06-11: render_qdcgt_statement (WS1-25 clean statement page, no faked face; gated on worksheet route) + 1040 7b "Sch D not required" box (`7185547`) | ✅ **10 wired 06-11** (FA-1040-INTDIV-01..10: 2a/2b/3a/3b/25b rosters, Exception-1 truth table, QDCGT breakpoints both years, WS partition identity + WS25=min, WS22/24 source-pinned to compute_tax_line_16, blocker truth table; active gate 58→70) | **Topic 3 COMPLETE** (tag `1040-intdiv-complete`). All 15 spec scenarios green (8 WS pure incl. TY2026 MFJ + MFS boundary pair + ID-Q9 247.50→248 + TCW-cents; 7 pipeline incl. gates; ID-G4 fires D_INTDIV_005); migration 0053 applied. D_INTDIV_001..010 ✅ |
| Retirement: 1099-R agg + SS worksheet (1040_RETIREMENT) | ✅ seeded 2026-06-11 (`retirement_spec.json` — Ken approved at review walk; 25 facts/8 rules/24 lines/7 diags/18 scenarios) | ✅ input leg 06-11: **Retirement Income tab** (`retirement_income`) → `RetirementIncomeSection` — per-doc 1099-R box CRUD (auto-save-on-blur) + SSA-1099/5329 assertion card (six 0056 facts via PATCH /taxpayer/); `retirement_distributions` in detail payload; box 2a uses a nullable money input (blank=NULL Simplified-Method gate); 10/10 in `test_topic5_input_leg.py`. RetirementDistribution model + CRUD from the seed leg | ✅ compute leg 06-11: `compute_retirement.py` — R-RET-CODE routing (J/T always RED; G/H/Q→0; Y reduces 4b via qcd) + R-RET-4AB/5AB (box2a−rollover−qcd floored; IRA→4a/4b vs 5a/5b; **blocking docs blank the taxable column, no silent gap**) + R-RET-25B (full INT+DIV+1099-R box-4 roster) + R-RET-EARLY→5329 + `compute_ss_worksheet` (WS1-18, both STOPs + MFS-with-spouse 85%-from-$1 + 85% cap). Migration 0056 (6 return-level Taxpayer facts). **Diagnostics D_RET_001..007 ✅ (rules_retirement.py)**. 4a-6b flipped to computed feeders. | ✅ render leg 06-11: `render_ss_worksheet_statement` (bespoke ReportLab, ws_1..ws_18 reached-lines-only, gate = ws_1 > 0; never a faked face) — placed after Form 5329 in the packet; 18/18 in `test_topic5_render_leg.py` | ✅ **7 wired 06-11** (FA-1040-RET-01..07: 4b/5b/25b rosters, 5329 L4→Sch2 L8, SS ws_18→6b + 85% cap, §86 constants both years, the 6 unsupported-path blockers; active gate 70→77 via `merge_retirement_assertions.py`; `_run_retirement_assertion` PURE re-derivation) | **Topic 5 COMPLETE (tag `1040-retirement-complete`).** All legs green: spec+seed+compute+render+input+diagnostics+assertions. Compute 26/26; input 10/10; diagnostics 6/6; flow gate **99 passed**. NO `_constants_for_year` (SS §86 + 5329 statutory). |
| Form 5329 Part I (early-distribution tax) | ✅ seeded 2026-06-11 (`5329_spec.json` — 3 facts/3 rules/4 lines/1 diag/3 scenarios) | ✅ input leg 06-11: line-2 exception number + amount + SIMPLE-25% flag entered on the Retirement Income tab's SSA-1099/5329 assertion card (the six 0056 facts) — exception 08 $8k → Sch 2 L8 1,200 proven through the route | ✅ compute leg 06-11: `compute_5329_part_i` (L3=L1−L2, L4=10%/25%×L3) → **Sch 2 line 8** computed feeder (flipped is_computed; override = escape hatch for Parts II-VIII); line 1 fed cross-form by R-RET-EARLY; `form_5329_generated` gate (R-5329-03). **Diagnostics leg 06-11: D_5329_001 ✅** (line 2 exception amount > line 1 early-in-income → error; in `rules_retirement.py`/`RULES_RETIREMENT`, reuses `RetirementAgg.early_distributions`). | ✅ render leg 06-11: `f5329` in manifest (SHA `2ad77023…`) + `field_maps/f5329_2025.py` (Part I lines 1-4 + exception-number box + header) in ACROFORM_FORM_IDS; `render_5329` gates on `form_5329_generated` (one source of truth); pages 2-3 stripped via SKIP_PAGES; packet seq 29 (after Sch B, before 8812) | ✅ FA-1040-RET-04 wired 06-11 (L4 = rate×max(0,L1−L2) → Sch 2 L8) | **Topic 5 COMPLETE (tag `1040-retirement-complete`).** Part I ONLY (10%/25% early-dist tax → Sch 2 line 8); direct-to-Sch-2 shortcut for pure code-1 case (F5329-T1..3 green). Parts II-VIII out of scope. |
| EIC worksheets + Table lookup (1040_EIC) | ✅ seeded 2026-06-11 (`eic_spec.json` — Ken approved at review walk; 33 facts/10 rules/18 lines/16 diags/16 scenarios) | ✅ input leg 06-12: **EIC tab** (`eic`) — Taxpayer EIC-facts card (16 Yes/No/Unanswered tri-state + 4 money incl. shared `nontaxable_combat_pay`) + Dependent EIC-flag table; **serializer trip-wire 60→79** | ✅ compute leg 06-11: `compute_eic.py` — Step-5/WS-B earned income + `eic_table_lookup` ($50-midpoint ROUND_HALF_UP, year-keyed `EIC_PARAMS` both years incl. published TY2026 664/8231) + `investment_income_total` + lower-of-AGI → 1040 L27a (override-respecting); wired after the first formula pass + 8812, "27" added to the downstream refresh. **ENGAGE gate** (`eic_engaged`, no false REDs). **Diagnostics `rules_eic.py` 10 RED gates ✅** (D_EIC_001/003/005/006/007/009/012/013/015/016, reuse compute helpers). 23/23 in `test_topic7_compute_leg.py`, flow gate 99 | ✅ render leg 06-12: `render_eic_worksheet_statement` — bespoke ReportLab STATEMENT page (never a faked face; `eic_engaged` gate, reached rows only) | ✅ **9 wired 06-12** (FA-1040-EIC-01..09 via `_run_eic_assertion` — PURE re-derivation + source inspection; gate **77→86**, flow gate **108 passed**) | **TOPIC 7 COMPLETE 2026-06-12 (tag `1040-eic-complete`)** — diagnostics leg added D_EIC_002/004/008/010/011/014 (now **16 D_EIC**) + D_SEI_001 + D_8867_001/002 + D_8862_001 = **20 EIC-family rules**; D_8867_001 BROAD §6695(g) gate; D_EIC_014 from 8862 part_v; `_sei_age_answers` relocated to compute_eic (bridge-gate). 26/26 `test_topic7_diagnostics_leg.py`. Seed leg: `seed_1040_eic` 18 computed lines; line 27a computed feeder. `nontaxable_combat_pay` REUSED from 8812 ACTC. |
| Schedule EIC (qualifying child info) | ✅ seeded 2026-06-11 (`schedule_eic_spec.json` — 7 facts/1 rule/7 lines/1 diag/2 scenarios) | ✅ input leg 06-12: Dependent EIC-flag table on the EIC tab (`eic_qualifying_child` tri-state + `is_full_time_student`); both flags added to `DependentSerializer` | n/a (data-map face, no compute) | ✅ render leg 06-12: `f1040sei_2025` + `render_schedule_eic` — model-driven per-child columns 1/2/3 (name/SSN/4-digit-year cells/4a/4b/rel/months from Dependent); gate ≥1 FLAGGED QC; 4a/4b derived (`_sei_age_answers`, under-19 skip); page-2 instructions stripped | n/a | **TOPIC 7 COMPLETE 2026-06-12**. Diagnostics leg: **D_SEI_001** (flagged QC fails the 4a/4b age test → error; reuses the relocated `_sei_age_answers`). Seed leg: `seed_schedule_eic` 7 model-driven lines. `f1040sei` in manifest + ACROFORM_FORM_IDS. |
| Form 8867 (preparer due diligence) | ✅ seeded 2026-06-11 (`8867_spec.json` — 16 facts/1 rule/12 lines/2 diags/3 scenarios) | ✅ input leg 06-12: Parts I-V Y/N rows via the StandardSection branch on the EIC tab (rows backfilled by `compute_eic` once EIC engages) | n/a (data-map; header boxes derived at render) | ✅ render leg 06-12: `f8867_2025` + `render_8867` — 12 seeded three-state answers → `{line}_yes/_no` boxes; header EIC/CTC/HOH/AOTC DERIVED; gate = `eic_engaged` OR any answer (not every covered credit — that's the diagnostics leg) | n/a | **TOPIC 7 COMPLETE 2026-06-12**. Diagnostics leg: **D_8867_001** (BROAD §6695(g) covered-credit return with an unanswered DD question → error) + **D_8867_002** (AOTC line 13 answered but 8863 not built → error). Seed leg: `seed_8867` 12 BOOLEAN lines / 5 Parts (FormFieldValue rows). `f8867` in manifest + ACROFORM_FORM_IDS. **LEG C 2026-06-19 (built+reviewed, local, NOT pushed):** new return-level attestation cascades the 8867 answers (late `apply_8867_due_diligence` pass) with a `"na"` value; **render now fills the N/A checkbox on lines 2/7/8 only** (PDF probe — `NA_ELIGIBLE_LINES`); `D_8867_001` accepts `na`; **`D_8867_002` retired (deactivate-in-place, `is_active:False`)**; render gate widened to any covered credit. Frontend: attestation checkbox + `<details>` collapse + simplified EIC exceptions list. All green; mig 0095 (attestation field). |
| Form 8862 (claim after disallowance) | ✅ seeded 2026-06-11 (`8862_spec.json` — 6 facts/1 rule/6 lines/1 diag/2 scenarios) | ✅ input leg 06-12: Parts I-V rows via the StandardSection branch on the EIC tab (backfilled when engaged) | n/a (data-map) | ✅ render leg 06-12: `f8862_2025` + `render_8862` — line-1 year + Part-I which-credit boxes (part_ii/iii/iv); gate = `eic_disallowed_prior_year` AND line "2" math-error ≠ true; all 3 pages kept for the preparer | n/a | **TOPIC 7 COMPLETE 2026-06-12**. Diagnostics leg: **D_8862_001** (math-error disallowance → Form 8862 NOT required; info — fires when `f8862_was_math_error` IS true). Seed leg: `seed_8862` 6 lines / 5 Parts (line 1 TEXT). `f8862` in manifest + ACROFORM_FORM_IDS. |
| Schedule D + 8949 (1040) | ✅ **TOPIC 9 COMPLETE 2026-06-13 (tag `1040-scheduled-complete`)** — all 6 legs. **DIAGNOSTICS** (`b5e690f`): family = 16 rules (7 error + 3 warning + 6 info); `schd_route` re-derived (not persisted); 12/12. **ASSERTIONS** (`2a635b7`): 12 FA-1040-SCHD merged (gate 100→112) via `merge_topic9_assertions.py` + `_run_schd_assertion` runner (PURE re-derivation + source pins, all 3 entry points, form ∈ {SCHEDULE_D, 1040_SCHD_WS, 8949}); **flow gate 134**. | ✅ INPUT LEG DONE 2026-06-13 (`e27383e`) — Schedule D tab (`ScheduleDSection`): CapitalTransaction CRUD (12-box dropdown + i8949 adj-code dropdown w/ definitions [single-select v1; multi-code follow-up] + Exception-2 toggle; computed (h) YELLOW) + the 7 schd_* facts card (QOF three-state, files_4952, carryovers, §1250/28%/§1202 supplements). TaxpayerSerializer +7 facts (trip-wire 86→93); capital_transactions nested read + CRUD already from seed leg. **7/7 `test_topic9_input_leg.py` + trip-wire 93 = 16; tsc/build clean.** NO migration/NO gated files. | ✅ COMPUTE LEG DONE 2026-06-13 (`063194a`) — `compute_schedule_d.py`: pure 8949 col-h + per-box netting (A/G→1b…F/L→10) + `schedule_d_route` (17/20/22→ordinary\|qdcgt\|sdtw) + the four worksheets (`compute_sdtw` 47L w/ skip rules, `compute_carryover_out_worksheet` clc, `compute_28pct_worksheet` w28, `compute_unrecap_1250_worksheet` u1250 — ported from `check_topic9_integrity.py`; SDTW 0/15% breakpoints REUSE `QDCGT_CONSTANTS_BY_YEAR`, `SDTW_BRACKET32_START` new year-keyed; invariant SDTW(18=19=0)==QDCGT) + DB assembly `compute_schedule_d_db` (before formula pass → SCHEDULE_D + 1040_SCHD_WS rows + 1040 L7a) + `schedule_d_line_16` (tax phase, bypasses route_line_16 when engaged). Engage = universal fallback (txns/carryover/4-5-11-12/DIV 2b-2d/unasserted 2a); 4952/2c/2555/8814 BLANK L16. SIBLING: D_INTDIV_001/002 + D_8995_002 RETIRED; 8995 L12 += net cap gain; canonical intdiv/8995 re-exported. `rules_schedule_d.py` 6 REDs. **33/33 `test_topic9_compute_leg.py`, flow gate 122, regression 158.** | ✅ RENDER LEG DONE 2026-06-13 (`49735cd`) — `f1040sd_2025.py` (model-driven face from SCHEDULE_D rows; QOF/17/20/22 Yes-No checkbox pairs; line 21 ABS for pre-printed parens) + `f8949_2025.py` EXTENDED 12-box G/H/I/J/K/L; `render_schedule_d_1040` (gate schedule_d_engaged) + `render_8949_1040` ONE COPY PER BOX (ST→PartI/LT→PartII) + `render_schedule_d_worksheets_statement` (SDTW/28%/§1250/CLC reached sections, statement page); ACROFORM_FORM_IDS +f1040sd; packet seq 12/12A after Sch C. **10/10 `test_topic9_render_leg.py`, flow gate 122, render regression 21.** | ✅ **12 wired (`2a635b7`)** — FA-1040-SCHD-01..12 via `_run_schd_assertion` (gate 100→112, flow gate **134**) | **SEED LEG (29/29):** migration 0061 (`CapitalTransaction` 12-box A–L doc model + 7 schd_* facts) + RLS 0062; seeds `seed_schedule_d`(47L)/`seed_1040_schd_ws`(85L)/`seed_8949`(26L); manifest +f1040sd; CRUD. SPEC (RS DB 44→46, `272aeee`). 192-field 8949 map reusable; Sch D → 1040 line 7a; SDTW line-19 2026 DERIVED (re-pin ~Dec 2026). **RS follow-up: clean the 2 stale pre-Topic-9 ID-G1/G2 scenarios from `load_1040_intdiv` (local canonical already dropped them).** |
| Schedule A | ❌ | ❌ | ❌ | ❌ | ❌ | |
| **Form 7206 (SEHI §162(l))** | ✅ FORM_7206 seeded 2026-06-15 (RS DB 60→61, FA 200→206; canonical `7206_spec.json`) | n/a (inputs live on W2Income.two_percent_shareholder_health / ScheduleC.sehi_amount) | ✅ 2026-06-15 `compute_7206.py` — Sch C limit FIX (net profit − apportioned ½-SE-tax − SEP) + the **2% S-corp Box-5 path** → Sch 1 L17 (engages WITHOUT a Schedule C); 17/17 | n/a (line 17 already rendered; no Form 7206 face in v1) | ✅ 6 (FA-1040-7206-01..06 via `_run_7206_assertion`; gate 217→223) | **W-2 UNIT 3 COMPLETE 2026-06-15.** Diagnostics `rules_7206.py` 6 (D_7206_SCORP_NOWAGE/SCORP_LIM/SC_LIM/LTC_AGECAP/2555/PTC). R-SC-SEHI repointed to FORM_7206 in lock-step. NO migration. LTC age-cap + Form 2555 line-12 v1-deferred (no silent gap). |
| **Minister / Clergy (§107/§1402)** | ✅ MINISTER seeded 2026-06-16 (RS DB 61→62, FA 206→212; canonical `minister_spec.json`; 4 sources all `requires_human_review`) | ✅ 2026-06-16 — W2Screen clergy section (5 housing fields + Form 4361 checkbox via `onUpdateTaxpayer`); **mig 0088** clergy_* fields on W2Income + `clergy_4361_exempt` Taxpayer fact (trip-wire 93→94); tsc 66 baseline / vitest 34 / build ok | ✅ 2026-06-16 `compute_minister.py` — §107 least-of-three excl; EXCESS → 1040 1h (aggregate_1040_income, override-respecting); clergy SE base (wages+full housing+parsonage−expenses) → Schedule SE line 2 per owner via the existing engine (×0.9235/SS cap/½-SE→Sch1 L15/SE→Sch2 L4); Form 4361 zeroes; **supersedes Topic-8 minister RED-defer**; 18/18 | ✅ `render_minister_statement` (Pub 517 worksheet statement; W-2 = input doc, no IRS face; 4/4) | ✅ 6 (FA-1040-MIN-01..06 via `_run_minister_assertion`; active 201→207, flow gate 223→229) | **W-2 UNIT 4 COMPLETE 2026-06-16 (tag `1040-minister-complete`).** Diagnostics `rules_minister.py` 6 (D_MIN_HOUSING_INC error + EXCESS/4361/SECA/REASONABLE/DEASON info/warn). v1 RED-defers: Sch-C ministerial side income + §265/Deason, reasonable-comp test, retired clergy, 4361 adjudication, mixed two-minister-4361. Brief `server/specs/_minister_source_brief.md`. |
| **Schedule C + SE + 8995 + 8959** | ✅ specs seeded 2026-06-12 | ✅ input leg 2026-06-12 — `schedule_c` tab (per-business cards Parts I-V + nested Part V row CRUD + per-proprietor SE cards + 8995/8959 facts card); TaxpayerSerializer +7 facts (trip-wire 86); nested schedule_c_businesses/schedule_se_forms reads; DepreciationAsset +schedule_c; depreciation POST now full compute_return; DISENGAGE fix (feeders gate on businesses OR se_rows — last-business-deleted reflows L3 to 0); 9/9 input tests + compute 49/49 re-verified + flow gate 108 + tsc/build clean | ✅ compute leg 2026-06-12 (`776ec8b` + 13b amendment `ae9fae7`) — `compute_schedule_c.py`: Sch C per-business chain → model columns; Sch SE auto-created per proprietor (wage base 176,100/184,500, W-2-SS cap, $400 floor); 8995 → 1040 L13 (L11 = AGI−12−13b Ken-ruled; above-threshold blanks + D_8995_001); 8959 → Sch 2 L11 + 25c; feeders Sch 1 L3/15/16/17 + Sch 2 L4/11 computed (engage-gated); depreciation engine → line 13 (0060); 7 REDs `rules_schedule_c.py`; 49/49 + flow gate 108 | ✅ render leg 2026-06-12 — 4 field maps (f1040sc incl. the IRS 27a/27b widget swap + B/D combs; f1040sse line-7 preprinted unmapped, page 2 stripped; f8995 replaces the old stub, 1i-1v table; f8959 1-24) + `render_schedule_c` (one COPY PER BUSINESS, pure-chain bridge-gate) + `render_schedule_se` (per proprietor; gate L12>0; RED-defer never prints) + `render_8995`/`render_8959` (row-driven engage gates); parens lines abs-injected; packet seq 09/17/55/71; 21/21 `test_topic8_render_leg.py`, flow gate 108, render regression 260 | ✅ **14 wired 06-12** (FA-1040-SCHC-01..04 / SCHSE-01..03 / 8995-01..03 / 8959-01..03 / TOPIC8-01 via `_run_topic8_assertion` — PURE re-derivation + source pins; **active gate 86→100, flow gate 122 passed**) | ✅ spec seeded 2026-06-12 (RS `load_1040_schedule_c`; canonical specs committed; RS DB 44, FA 105). **TOPIC 8 COMPLETE 2026-06-12 (tag `1040-schedulec-complete`)** — diagnostics leg added the 13 info/warn rules (family = **20**: 7 error + 1 warning + 12 info); 8995 L11 = 1040 L11−L12−**L13b** (Ken ruled 06-12, RS amended in lock-step); 8959 engage incl. the withholding-reconciliation case (DECISIONS, reversible). **BUILD LEG 1 (seed leg) DONE 2026-06-12:** migration 0058 (`ScheduleC` multi-business doc model + `ScheduleCOtherExpense` Part V child + thin per-proprietor `ScheduleSE` [Ken's call — W2Income has no owner field so the SS-wage cap needs a per-proprietor home] + 8995/8959 Taxpayer facts) + RLS 0059 (3 tables, rowsecurity=True, 0 policies); seeds `seed_schedule_c`(56L)/`seed_schedule_se`(24L)/`seed_8995`(21L re-author)/`seed_8959`(24L) in build.sh, line_maps + computed sets match specs exactly (trip-wire caught Sch C L13 depreciation = computed/from-4562); manifest f1040sc/f1040sse/f8959 downloaded + f8995 registered (AcroForm-verified); CRUD schedule-c (+ nested other-expenses) + schedule-se (one-per-proprietor guard → 400). **33/33 `test_topic8_seed_leg.py`; flow gate 108 (no gated files touched); makemigrations --check clean.** **8995 RE-AUTHORED** over a wrong K-1 stub; **8959** Decision 4. Build leg 2 (compute) next |
| **Schedule F (farm) + SE farm-optional** | ✅ **SPEC LEG 2026-06-21** — SCHEDULE_F seeded (31f/11r/71L/8d/10s) + SCHEDULE_SE amended additively (Part II farm optional method + line-1a/1b computed feeders + narrowed R-SE-OPTIONAL/D_SE_003); RS DB 64→65, FA 224→232; canonical `schedule_f_spec.json` + re-export `schedule_se_spec.json` + 8 FA staged; math gate ALL PASS (id-length guards added); guard verified (zero DB writes); seed gotcha — `diagnostic_id` varchar(20), `D_SE_FARMOPT_INELIGIBLE`→`D_SE_FARMOPT_INELIG`. Brief `_schedule_f_source_brief.md`. Ken-approved walk. | ✅ **INPUT LEG DONE + GREEN 2026-06-21** — un-stub `schedule_f` 1040 tab → `ScheduleF1040Section` per-farm card UI (the ScheduleC precedent): header A-G (accrual→RED banner, EIN RED-validation, E-G tri-state) + Part I income (1c/9 YELLOW) + Part II expenses (lines 10-31; L14 depreciation YELLOW from 4562) + line-32a-f nested other-expense rows (32/33/34/QBI YELLOW) + at-risk 36a/36b + SEHI/SE-retirement GREEN + farm-optional-method election card (Schedule SE Part II, per farm-attached proprietor) + Σ-L34→Sch1-L6 total; dispatch branches `activeTab==="schedule_f"` on `isIndividual` (1040 model card vs entity FormFieldValue `ScheduleFSection`); IndividualNav un-stub (no 1040 stub tabs left) + `schedule_f_farms` collection dot + `D_SF_`/`D_SE_FARMOPT`→`schedule_f` routing. Pure frontend (CRUD/serializer from seed leg); **tsc clean / vitest 201 (+5) / build clean.** | ✅ **COMPUTE LEG DONE + GREEN 2026-06-21** — `compute_schedule_f_family` (per-farm chain → 5 cols; **accrual zero-gate**, no silent compute; SE feed 1a/1b/gross; **Sch 1 L6 = ΣL34** engage-gated + reflow) wired BEFORE `compute_schedule_c_family`; farms folded into the QBI pro-rata allocation (½-SE + farm SEHI + farm SE-retirement → `ScheduleF.qbi_amount`; Sch 1 L16/L17 carry farm); 8995 L2 + `qbi_engaged(farms=)`; depreciation `flow_to=schedule_f` → L14 (**mig 0102**, §179 IN); Sch 1 L6 flipped direct→computed (`seed_sch_1` is_computed). **12/12 `test_schedule_f_orchestration.py` + 14/14 pure + Topic 8 regression 54/54 (no value moved) + flow gate 241.** Also fixed the `schedule_se_spec.json` re-export drift (SE-FARMOPT-1..4: count 5→9, loop, line-14 emit). | ✅ **RENDER LEG DONE + GREEN 2026-06-21** — `f1040sf_2025.py` un-stubbed (89 generic AcroForm names → cash lines, **every widget correlated to its printed label by position** off the 2025 PDF; comb B/D; radio-by-distinct-name "X"; no pre-printed parens) + `render_schedule_f_1040` (ONE COPY PER FARM, bridge-gated via `compute_schedule_f_lines`; L14←4562; line-32 a-f rows; loss-only at-risk box; **accrual renders no face**) + `f1040sf` in ACROFORM_FORM_IDS + manifest entry + packet append after Sch C + page-2 strip via `SKIP_PAGES["Schedule F"]`. **10/10 `test_schedule_f_render_leg.py`** (map-vs-PDF + face position sweeps + accrual-skip + 2-farm copies + packet 1-page) + flow gate 241, no regression. | ✅ **8 wired 2026-06-21** (FA-1040-SCHF-01..08 via `_run_schf_assertion` — PURE re-derivation for the cash math + source-pins for the depreciation/QBI/CRP feeders + no-silent-gap FA-07; gate **241 → 249**) | **UNIT COMPLETE — ALL 6 LEGS GREEN — tag `1040-schedule-f-complete`; RS SPEC LEG COMPLETE; BUILD LEGS 1 (seed) + 2 (compute) + 3 (render) + 4 (input) + 5 (diagnostics) + 6 (assertions) DONE + GREEN 2026-06-21** (leg 6 assertions = LAST, 8 staged) — diagnostics: `rules_schedule_f.py` 8 D_SF_* + 2 D_SE_FARMOPT (D_SF_461L = flat $100k Ken-approved; loss rules bridge-gate + skip accrual; farm-aware `d_8995_001`); 17/17 + 10 rules seeded active. — migration 0100 (`ScheduleF` per-farm model + `ScheduleFOtherExpense` 32a-f child + 3 farm-optional cols on `ScheduleSE`) + RLS 0101 + `seed_schedule_f` (71 lines/3 sections, build.sh auto-discovers) + serializer/CRUD (`schedule-f` + nested other-expenses) + `schedule_f_farms` nested read; 19/19 `test_schedule_f_seed_leg.py` + flow gate 241 + check/makemigrations clean. Legs 2–6 next. Cash-method v1, **per-farm** (multi-Schedule-F). **Verified vs 2025 IRS PDFs:** L9 = 1c+2+3b+4b+5a+5c+6b+6d+7+8; L34 = L9−L33 → **Sch 1 L6 + Sch SE L1a**; CRP → SE 1b; L14 depr ← 4562; farm net → 8995 QBI. Farm optional SE (2025 $7,240/$10,860/$7,840; 2026 RED). RED-defers (8 D_SF_*/2 D_SE_*): accrual Part III, CCC §77 / crop-ins §451(f) / weather §451(e) elections, passive (8582), at-risk (6198), §461(l). **Schedule J = fast-follow** (own RS spec). |
| **Schedule J (farm income averaging)** | ✅ **SPEC LEG 2026-06-21 — SEEDED + EXPORTED + STAGED (Ken-approved walk; RS `60521bf`).** SCHEDULE_J seeded (42 facts/20 rules/25 lines/5 diagnostics/10 scenarios/23 cited links + 8 FA); RS DB 65→66, FA 232→240; canonical `schedule_j_spec.json` + 8 FA staged in `flow_assertions_1040_schedule_j_pending.json`; math gate `check_schedule_j_integrity.py` ALL CHECKS PASS (independent SJ-T1 chain L23=31,982 + cell-by-cell constant cross-check); guard verified; deployed export HTTP 200. Walk rulings: Q-A line-4 SDTW reduces cy Sch D by 2b/2c floored at 0; Q-B line 6 round-half-up; Q-C SDTW lines 44/46; Q-D D_SJ_ELECT_HIGH warning. Brief `_schedule_j_source_brief.md`. Spec-first probe: NO prior RS spec (404). Law verified vs the actual 2025 IRS PDFs (pymupdf) → `server/specs/_schedule_j_source_brief.md`: 23-line chain (L23→1040 L16); base-year rate schedules 2022/23/24 verbatim; base-year QDCGT (27L) + breakpoints; **Schedule D Tax Worksheet = identical 47-line structure all of 2022/23/24/25** (one year-keyed engine, only breakpoints differ). Both TY2025 (base 22/23/24) + TY2026 (base 23/24/25) buildable now. Ken-locked v1 (AskUserQuestion): base-year amounts direct-entry + optional PriorYearReturn pull; **build SDTW + base-year QDCGT**; RED-defer prior-Sch-J chaining / zero-or-less Taxable-Income worksheets / Form 2555-FEI. KEY: base-year tax (L8/12/16) uses base-year RATE SCHEDULES (never the Tax Table). Stale-cross-ref flag (SDTW lines 44/46, not the instr's 34/36 & 42/44). 4 OPEN requires_human_review Qs (brief §10): Q-A line-4 cap-gain netting (load-bearing), Q-B line-6 rounding, Q-C stale cross-ref, Q-D elect-when-higher. | ✅ **INPUT LEG DONE + GREEN 2026-06-22** — `ScheduleJSection` (FormEditor.tsx) singleton election tab (the Taxpayer pattern): "Elect income averaging" toggle + election card (2a/2b/2c + collapsible current-year preferential detail) + 3 per-base-year cards (filing-status select / taxable income negative-OK / original §1 tax / preferential detail / `used_schedule_j`+`form_2555` tri-states) + computed YELLOW preview (lines 4/6/8/12/16/17/22/23 → 1040 L16, elected-only); GREEN direct entry. Singleton CRUD `PATCH /schedule-j/` (PATCH-creates; uncontrolled money on-blur keyed on `sj.id`). Dispatch `activeTab==="schedule_j"` (1040-only); `TaxReturnData`+`schedule_j`; IndividualNav new tab (Income group after Sch F) + dot predicate + `D_SJ_`→`schedule_j` route staged. **tsc clean / vitest 203 (+2) / build clean**; pure frontend (no backend change → flow gate 249). | ✅ **COMPUTE LEG DONE + GREEN 2026-06-21** — `compute_schedule_j.py`: the 23-line §1301 chain + per-line year+status-keyed tax router (rate schedule / base-year QDCGT / Schedule D Tax Worksheet) + engage/blocker gate + `compute_schedule_j_db` routes line 23 → 1040 line 16 (the `route_line_16` precedent), wired into `compute_return` after `_compute_1040_tax`. REUSES the existing engines (no 47-line re-type): added 2022/23/24 to `compute.TAX_BRACKETS` + `QDCGT_CONSTANTS_BY_YEAR` + `SDTW_BRACKET32_START`, and parameterized `compute_qdcgt_worksheet` / `compute_sdtw` with `ordinary_tax_fn` (default unchanged → Topic 3/9 byte-identical; base years pass the rate schedule). `tax_table_lookup` now gates on `_CONSTANTS_BY_YEAR` (={2025,2026}) not bare `TAX_BRACKETS` (base years are rate-schedule-only). Q-A: line-4 SDTW reduces cy Sch D figures by 2b/2c floored at 0; base-year SDTW adds ⅓ of 2b/2c. **31/31 `test_schedule_j_compute_leg.py`** (SJ-T1 anchor L23=31,982 pure + pipeline + SJ-T2..T10 + engine-constant cross-checks vs the brief + no-2025/26-drift) + **flow gate 249** + spine/intdiv/Topic 9 regression green; check/makemigrations clean (no model change). | ✅ **RENDER LEG DONE + GREEN 2026-06-22** — `field_maps/f1040sj_2025.py` (all 27 PDF widgets mapped by position: page 1 header name/SSN + lines 1/2a/2b/2c/3-17, page 2 lines 18-23; full coverage, no dup) + `render_schedule_j` (ONE face per return, model-driven via `compute_schedule_j_chain`, gated on `schedule_j_engaged` — non-election/blocker prints no face; -0- literal for lines 7/8/12/16 when zero; both pages content, no SKIP_PAGES) + registered in ACROFORM_FORM_IDS + packet append after Schedule F (seq 20). **9/9 `test_schedule_j_render_leg.py`** (map-vs-PDF + full-coverage + SJ-T1 anchor position sweep [L1 180,000 … L23 31,982] + negative-base-year "(5,000)"/"-0-" + engage/blocker gates + packet 2-page) + flow gate 249. **DEFERRED (tracked):** base-year QDCGT/SDTW **statement pages** (brief §11) — the face shows the worksheet-derived taxes; the line-by-line worksheet breakdown needs a `schedule_j_worksheet_details` helper (compute stores only the final columns). | ✅ **8 wired 2026-06-22** (FA-1040-SCHJ-01..08 via `_run_schj_assertion` — PURE chain re-derivation for 02/03/07 + source-/constant-pins for the route + rate-schedule/SDTW/QDCGT engine-reuse 01/04/05/06 + no-silent-gap registry+blocker FA-08; **gate 249 → 257**) | **UNIT COMPLETE — ALL 6 LEGS GREEN — tag `1040-schedule-j-complete`. SPEC LEG COMPLETE + BUILD LEGS 1 (SEED) + 2 (COMPUTE) + 3 (RENDER) + 4 (INPUT) + 5 (DIAGNOSTICS) + 6 (ASSERTIONS) DONE + GREEN 2026-06-22.** Assertions leg (leg 6 = LAST, 8 staged): merged via `merge_schedule_j_assertions.py` + `_run_schj_assertion` (id-prefix dispatch in all 3 entry points), flow gate 257, no app code change. Diagnostics leg: `apps/diagnostics/rules_schedule_j.py` (NEW) wires the 5 D_SJ_* (3 error: 2A_EXCEED/CHAIN/2555; 2 warning: NEG_TI/ELECT_HIGH), each gated on an active election (`sj.elected`); blockers read the `schedule_j_blocker` predicates but evaluated INDEPENDENTLY so they co-fire; D_SJ_ELECT_HIGH gates on `schedule_j_engaged` + NEW pure `compute_regular_tax` (re-derives the regular tax the compute leg overwrote on 1040 L16) vs the chain's line 23; base years dynamic via `base_year_of`. Registered via `RULES_SCHEDULE_J` in `runner.seed_builtin_rules`; **12/12 `test_schedule_j_diagnostics_leg.py` + flow gate 249 + Sch-J compute/render/seed regression 55/55 + check clean + 5 rules seeded active in the shared DB.** NO migration / NO gated-file change. NEXT = build leg 6 (ASSERTIONS — the FINAL leg): merge the 8 staged FA-1040-SCHJ-01..08 (gate 249→257) via `merge_schedule_j_assertions.py` + a `_run_schj_assertion` runner (the `_run_schf_assertion` precedent), tag `1040-schedule-j-complete`. Still tracked: deferred base-year QDCGT/SDTW statement pages (render follow-up). Seed leg: `ScheduleJ` OneToOne(TaxReturn) model (42-field surface, base-year TI negative-OK) + **migration 0103 + RLS 0104 (applied)** + `ScheduleJSerializer` (computed cols read-only) + **singleton CRUD** (`/schedule-j/` GET/PUT/PATCH/DELETE) + nested read `schedule_j` (SerializerMethodField) + `seed_schedule_j` (25 lines) + `f1040sj` manifest/PDF. **15/15 `test_schedule_j_seed_leg.py`** (spec trip-wires + CRUD + negative-TI + nested read) + flow gate 249 (no regression) + check/makemigrations clean. NEXT = build leg 2 (COMPUTE): `compute_schedule_j.py` — 23-line chain + year+status-keyed rate-schedule/QDCGT/**NEW SDTW** engines + `route_line_16` + **merge 2022/23/24 into spine `compute.TAX_BRACKETS`**; then render → input → diagnostics (5 D_SJ_*) → assertions (gate 249→257). QDCGT + `route_line_16` precedent `compute_intdiv.py`. The Schedule F fast-follow. |
| **Schedule E Part I (rentals/royalties) + Form 8582** | ✅ `schedule_e_spec.json` + `form_8582_spec.json` | ✅ "Rental (Sch E)" tab | ✅ `compute_schedule_e.py` + `compute_8582.py` | ✅ `f1040se`/`f8582` | ✅ 6 (SCHE-01/02, 8582-01..04) | **BUILT 2026-06-14** (Ken-approved walk) — the prior ❌ was a stale tracker. Part I net + the $25k §469(i) special allowance; line 26 → Sch 1 L5 (becomes an addend within line 41 at effort #5). |
| **Schedule E page 2 — K-1 router (SCHEDULE_K1)** | ✅ **SPEC LEG 2026-06-21** — SCHEDULE_K1 seeded (31f/11r/21L/10d/12s) + SCHEDULE_E amended page 2 (lines 27-43, +3r/+2d); RS DB 63→64, FA 217→224; canonical `schedule_k1_spec.json` + 7 FA staged; integrity gate ALL PASS; guard verified | ✅ **build leg 4 (input) 2026-06-21** — un-stub `schedule_e_p2`; `ScheduleK1Section` (per-entity cards; source_type drives box labels; owner T/S/J; mat-part/PTP toggles; grouped Sch-E/cross-form/v1-defer fields; YELLOW computed preview; RED EIN validation); nav un-stub + `D_K1_`→tab; tsc/vitest 196/build clean | ✅ **build leg 2 (compute) 2026-06-21** — `compute_schedule_k1.py` (central aggregator) + `compute_schedule_e_db` page 2 / **line-41 repoint** + cross-form addends (intdiv 2b/3b/3a, Sch D 5/12, Sch SE 1065-14A, 8995 2/6) | ✅ **build leg 3 (render) 2026-06-21** — `f1040se_2025.py` page-2 map (153 entries, 0 missing/dup) + `render_schedule_e` Parts II/III/V (per-entity rows + totals + **line 41** + page-2 header; 31/36 abs-injected) + conditional page-2 un-skip + Sch D 5/12 K-1 face + 8995 line-1 K-1 QBI; 18/18 `test_schedule_k1_render_leg.py` | ✅ **build leg 6 (assertions) 2026-06-21** — merged 7 FA-1040-K1-01..07 (gate **234 → 241**) via `merge_schedule_k1_assertions.py` + **repointed FA-1040-SCHE-01 → line 41**; new `_run_k1_assertion` (PURE re-derivation via the extracted `schedule_e_p2_totals_from_rows` core for FA-01/02/03 + source-pins on the cross-form feeders for FA-04/05/06 + no-silent-gap blocker check FA-07), id-prefix dispatch in all 3 entry points; flow gate 241 + K-1 legs 53, no regression. **TAG `1040-k1-router-complete`** | **EFFORT #5 COMPLETE — recipient K-1 full router, all 6 legs green (tag `1040-k1-router-complete`).** Recipient K-1 full router (1065 partner / 1120-S shareholder / 1041 beneficiary). **Verified vs 2025 IRS PDFs:** §199A 20Z/17V/14I → 8995 (2/6); SE 1065 14A → Sch SE; **line 41** (not 40) → Sch 1 L5. v1 RED-defers passive losses / §1231 / AMT / basis / REMIC / 4835 (10 D_K1_* + 2 D_SCHE_*). New per-entity model (NOT `k1_allocator.py`, which is issuer-side). Source brief `_schedule_k1_source_brief.md`. **BUILD LEG 1 (seed) DONE 2026-06-21 (`ef7633a`):** `ScheduleK1` model + mig 0098 / RLS 0099 + serializer/nested/CRUD + `seed_schedule_e` page-2 lines (Sch E now 52L/4 sections); 12/12 seed tests + flow gate 234 (no regression). **BUILD LEG 2 (compute) DONE 2026-06-21:** central `compute_schedule_k1.py` + page-2 Part II/III aggregation + line-41 repoint (Part-I-only line 41 == line 26, FA-1040-SCHE-01 source-string stays green) + cross-form routing (2b/3b/3a, Sch D 5/12, Sch SE 1065-only, 8995 2/6); passive losses RED-deferred (col g/c = 0); 13/13 `test_schedule_k1_compute_leg.py` + flow gate 234 + Sch E/intdiv/topic8/topic9 regressions green. **BUILD LEG 3 (render) DONE 2026-06-21:** `schedule_e_p2_rows()` per-entity FACE decomposition (reconciles to the compute totals) + page-2 field map + `render_schedule_e` page 2 + conditional `SKIP_PAGES` un-skip (`schedule_e_p2_has_content`) + Sch D 5/12 (direct + K-1) + 8995 line-1 K-1 QBI rows; 18/18 render tests + flow gate 234 + compute-leg/topic8/topic9 (99) green. Row caps: line 28 ×4, line 33 ×2, 8995 line 1 ×5 (overflow → totals, flagged). **BUILD LEG 4 (input) DONE 2026-06-21:** un-stubbed the `schedule_e_p2` tab + `ScheduleK1Section` (per-entity cards; source_type 1065/1120-S/1041 drives the box labels + hides inapplicable fields; owner/mat-part/PTP; grouped Sch-E / cross-form / v1-defer money + flags; **YELLOW** computed-totals preview from `field_values`; **RED** EIN entry validation); `IndividualNav` un-stub (collection dot + `D_K1_`→tab); `ScheduleK1Row` type. tsc clean / vitest 196 / build clean (no backend change). **BUILD LEG 5 (diagnostics) DONE 2026-06-21:** new `rules_schedule_e_p2.py` wires the 10 D_K1_* (5 RED-defer errors — passive-loss/§1231/28%-§1250/AMT/other-income; 3 warnings — basis-at-risk/foreign-K3/PTP-loss; 2 info — S-corp-not-SE/line-41-flow) + 2 D_SCHE_* (REMIC line 39 / Form 4835 line 40), registered via `RULES_SCHEDULE_E_P2` in `runner.seed_builtin_rules`; each reuses the compute helpers — new shared **`k1_sche_net()`** in `compute_schedule_k1.py` (passive/PTP-loss bridge-gate, refactored from the 3 inline `net=` sites so the RED can never disagree with the column math) + `schedule_e_p2_totals` (flow info); offending-entity names appended to each finding. 18/18 `test_schedule_k1_diagnostics_leg.py` + **flow gate 234 + K-1 compute leg (251 passed, no regression)** + `manage.py check` clean; 12 rules seeded active in the shared DB (client routing `D_K1_`→`schedule_e_p2` already wired in leg 4). **BUILD LEG 6 (assertions) DONE 2026-06-21:** merged the 7 staged FA-1040-K1-01..07 into the active gate (**234 → 241**) + **semantic-repointed FA-1040-SCHE-01 → line 41** via `merge_schedule_k1_assertions.py`; new `_run_k1_assertion` runner (id-prefix dispatch `FA-1040-K1-*` in all 3 entry points) — PURE re-derivation through the extracted pure core `schedule_e_p2_totals_from_rows()` for FA-01/02/03 (line 41 = 26+32+37; line 32 = (h+k)−(i+j), col g = 0; line 37 = (d+f)+(c+e), col c = 0), source-pins on the cross-form feeders for FA-04/05/06 (intdiv 2b/3b/3a, Sch D 5/12, Sch SE 1065-only, 8995 2/6), no-silent-gap blocker check for FA-07. **flow gate 241** + K-1 compute/render/diagnostics legs **53 passed — no regression** (the from_rows refactor is value-identical). **EFFORT #5 COMPLETE — all 6 legs green, tag `1040-k1-router-complete`.** |
| **Form 8863 (Education — AOTC + LLC §25A)** | ✅ `form_8863_spec.json` (NEXT-UP #8) | ✅ Education (8863) tab (`EducationSection`) + **per-dependent AOTC checkbox (Leg B.3, 2026-06-18 — auto-links an `EducationStudent` owner=dependent via the new `dependent` FK, mig 0093, SET_NULL; check→POST pre-filled, uncheck→confirm-if-detail→DELETE)** | ✅ `compute_8863.py` (AOTC 27-30 → Part I; LLC; Credit Limit Worksheet → Sch 3 L3; refundable AOTC → 1040 L29) | ✅ `f8863_2025.py` | ✅ FA-1040-8863 (`flow_assertions_1040_8863_pending` merged) | **NEXT-UP #8 COMPLETE.** Diagnostics `rules_8863.py` (7: MFS/dependent bars + dual-student/no-credit/lockout/AOTC-inelig/TY2026). **Leg B.3** added the claim bridge + **`D_CREDIT_AOTC`** unified-engine gate (barred/no-expenses→error, MAGI-over-ceiling→info; bridge-gated on `compute_8863`). *(EIC, Form 2441, and 8995 are tracked in their own rows/topics — this row replaces the stale catch-all that wrongly read ❌.)* |
| **Form 8615 (Kiddie Tax §1(g))** | ✅ `8615_spec.json` (seeded 2026-06-24 — 31 facts/10 rules/19 lines/7 diag/8 scenarios/9 FA) | ✅ **INPUT LEG 2026-06-24** — singleton `form-8615` CRUD route (the Schedule J pattern) + `Form8615Section` on a new "Kiddie Tax (8615)" tab (Other taxes group): `applies` §1(g) + parent facts (TI L6/tax L10/filing status/QD/net-cap-gain/Σ siblings L7) GREEN, child line-2 detail + 3 RED-defer flags tri-states, YELLOW child amounts + computed preview (L18→1040 L16), RED defer banner | ✅ **COMPUTE LEG 2026-06-24** — `compute_8615.py` ordinary §1(g) core (L2-18 → max(L16,L17) → 1040 L16 late-feeder after Sch J; reuses `compute_tax_line_16`); RED-defers QD/net-cap-gain (`QDCGT_PENDING`), §1250/28% SDTW, Sch J, Form 8814 | ✅ **RENDER LEG 2026-06-24** — `f8615_2025.py` (19 lines + 2 header, verified vs the real PDF) + `render_8615` reads persisted columns | ✅ 9 FA merged 2026-06-24 (FA-1040-8615-01..09 via `merge_8615_assertions.py` + `_run_8615_assertion`; gate 292→301) | **UNIT FULLY COMPLETE — tag `1040-8615-complete`** (all 6 legs + the QDCGT cap-gain follow-up; ordinary + qualified-dividend/net-cap-gain). DIAGNOSTICS: `rules_8615.py` 7 D_8615_* (D_8615_008 RETIRED w/ the QDCGT follow-up); NARROWED D_1040_004 → defer-to-applies + verified $2,700 TY2026 threshold + error→warning. ASSERTIONS: PURE compute_8615 re-derivation + source-pins; FA-06 asserts the compute_qdcgt_worksheet reuse. **QDCGT FOLLOW-UP (mig 0116):** sibling QD/cap-gain fields (Ken: collect per-sibling); `_line5_split` = i8615 Line 5 Worksheets 1/2/3; `_tax_at` routes L9/15/17 through compute_qdcgt_worksheet. Parent + per-sibling data = preparer-asserted. Bugs fixed: `compute_8615_db` + `rules_8615` + `render_8615` all fetch fresh (the reverse-OneToOne cache bit all 3 readers). CPA-review care items: WS2/WS3 prorations; net cap gain from 1040 L7 (no-Sch-D, Sch-D 15/16 netting a future refinement). |

**1040 supporting infrastructure (verified 2026-06-10 PM #9):**
- Models: Taxpayer (incl. migrations 0047 std-ded + 0048 estimated-payments + 0049 diagnostic facts + 0053 Topic 3 facts + **0056 Topic 5 SSA/5329 facts**), W2Income + Box12/Box14 entries, InterestIncome (17-box + FATCA/nominee/accrued/seller-financed block, 0051), DividendIncome (full 1099-DIV surface, 0051, RLS 0052), **RetirementDistribution (full 1099-R box surface, box 2a nullable, 0054, RLS 0055)**, Dependent, CarLoanVehicle, Employer DB (3,828 rows) with EIN autofill + learning loop
- Input tabs (client): Taxpayer Info (incl. 12a-12e std-ded block + decedent/HOH), Dependents, W-2 Income, Interest Income, **Dividend Income (1099-DIV + Form-8814 assertion card, Topic 3)**, **Retirement Income (1099-R box CRUD + SSA-1099/5329 assertion card, Topic 5)**, **EIC (Taxpayer EIC-facts card + Dependent EIC-flag table + 8867/8862 StandardSection, Topic 7)**, **Schedule B (line 3 + Part III three-state, Topic 3)**, Schedule 1/2/3, Sch 1-A (OBBBA), Credits & 8812, Payments, Tax Summary (summary card + editable 1040 form-line list)
- All **79** Taxpayer input facts (migrations 0042/0043/0044/0047/0048/0049/0050/0053/0056/**0057**) serializer-exposed — pinned by `test_taxpayer_input_leg.py` trip-wire (**+ the 2 Dependent EIC flags `eic_qualifying_child`/`is_full_time_student`**)
- Diagnostics: 17 D_1040 rules (rules_1040.py; **001 retired/inactive 06-11** — bridge replaced) + 14 D_SCH1/2/3 rules (rules_sch123.py) + **10 D_INTDIV rules (rules_intdiv.py)** + **7 D_SCHB rules (rules_schb.py)** + **8 retirement-family rules (rules_retirement.py — 7 D_RET + 1 D_5329, Topic 5)** + **20 EIC-family rules (rules_eic.py — 16 D_EIC + 1 D_SEI + 2 D_8867 + 1 D_8862, Topic 7)** on the shared DiagnosticRule machinery, seeded by `seed_rules` (in build.sh); generic EIN/address/dates/income checks gated away from individuals
- `server/specs/flow_assertions_1040.json`: **86 assertions wired** (8812 + Sch 1-A + 15 spine [SPINE-15 retired 06-11] + ALL 13 Sch 1/2/3 + 10 INTDIV + 2 SCHB + 7 RET [Topic 5] + **9 EIC [Topic 7]**); all 1040 pending files now empty staging slots. Flow gate `tests/test_flow_assertions.py` = **108 passed**.
- Field maps: `f1040_2025.py` (audited), `f1040s8_2025.py`, `f1040s1a_2025.py`, `f1040s1_2025.py`, `f1040s2_2025.py`, `f1040s3_2025.py`
- Manifest: 25 IRS PDF templates
- ~~`build.sh` was missing `seed_sch_8812` + `seed_sch_1a`~~ **fixed 2026-06-10**

## Business entities (mature — pre-campaign)

| Package | Status | Notes |
|---|---|---|
| 1120-S + K-1 + GA-600S | ✅ production-grade | 323 seeded lines, 20 flow assertions, full render package (~25 forms incl. 4562/4797/8825/7203/8949/Sched D/L/M-1/M-2) |
| 1065 + K-1 | ✅ | 285 lines, render package |
| 1120 | 🟡 | 172 lines, basic |
| Trust (1041) | ❌ | Entity type enabled, no forms |

## Rule Studio (deployed: sherpa-tax-rule-studio.onrender.com)

- 30 forms seeded in RS DB (mostly 1120-S family + business forms; all status=draft)
- 1040-relevant seeded specs: **SCH_8812** (1,140 facts/rules, exported + implemented), **SCH_1A** (38 rules, 47 lines, 21 scenarios — seeded 2026-06-10, exported + implemented), **1040 SPINE** (45 rules, 91 facts, 72 lines, 16 diagnostics, 33 scenarios — seeded 2026-06-10 PM #5 on Ken's approval; Session-14 stub retired), plus business forms reusable at 1040 level (8995/8995-A/4562/4797/8949/6198/8582/3800/8283)
- RS has no per-form `ready_to_seed` DB field — the sentinel lives in each loader command; form `status` choices are draft/review/approved/archived
