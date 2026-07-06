# BUILD_ORDER.md — The single "what's next" spine

*Adopted 2026-07-04, re-cut 2026-07-05 against live STATUS (session 14→15). SUPERSEDES the
retired SEASON_CHECKLIST.md. Top unblocked SPINE item = the next thing to build.
Nobody re-decides sequence. Dates survive only as GATE guardrails, not as the organizer.*

**CANONICAL HOME: the `tts-tax-status` repo** — the shared brain both CC contexts (tax-app
AND rule-studio) pull at boot. This is the SINGLE source of order for BOTH lanes. Relationship
to the others: SEASON_PLAN (also canonical here) = fixed points (scope, gates, risk).
PRODUCT_MAP (also canonical here) = scope. WORK_ORDERS (RS repo) = the RS front-door
MECHANISM only (gap-check + DRAFTING→AWAITING KEN→APPROVED→DONE transitions + Gate-1); it
does NOT maintain its own ordered backlog — it takes its next authoring order FROM this
file's SPINE and holds the working detail of the current order. THIS file = order + ledger.

**Lanes:** `[RS]` Ken authors in Rule Studio · `[APP]` CC builds in tax app · `[EXT]` Ken
external. **Tick:** `- [x] … — YYYY-MM-DD `SHA``. Parallel-safe items `∥`.
**Status marks below are reconciled to live STATUS.md + form_coverage_tracker.md as of
2026-07-05 (session 15).** Keep them current at session close; never trust a stale mark.

## ▶ NOW WORKING ON — **S-4 1065 core (IN PROGRESS). Legs 1a + 1b + 2 DONE; next = leg 3 (M-1/M-2
tie-outs).** ✅ **Legs 1b + 2 (2026-07-05, eighteenth session).** **Leg 1b — Schedule K 2025 renumber +
Analysis line + diagnostics:** renumber (`seed_1065`→290 lines) foreign taxes K16a→line 21 (`K21`), line 16→
`K16` K-3-attached checkbox (intl RED-defer), K13d→`K13b`, +`K13c`/`K13e`, computed `K_ANALYSIS` = (Σ K 1-11)
− (Σ K 12-13e + 21); new `rules_1065_schk.py` (D_SCHK_HANDOFF/K3/9C + D_1065P1_4797/COGS); ★ fixed a latent
cross-form bug — `aggregate_schedule_d` (1120-S, keys K7/K8a) ran ungated and zeroed 1065 royalties (K7)
every recompute → added a `code=="1065"` guard. **Leg 2 — Schedule L balance check:** new `rules_1065_l.py`
(D_L_BALANCE_BOY/EOY error, D_L_21_M2_TIE + D_L_M3 warning, D_L_EXEMPT info; B6 exemption suppresses balance
errors); the L14/L22 total compute already existed → validation-only. Render still DEFERRED.
(Prior: ✅ **S-9 NC D-400 COMPLETE all 4 legs** — compute `69cf82b`/input `b31d5c7`/render `c704f21`/diag
`8358e74`; flat 4.25%, 85% bonus/§179 add-back; the three state forms SC1040/AL40/NC D-400 are done.)
## ▶ NEXT — app: **S-3 brokerage front end** (∥, not spec-gated) · **S-11 1041 module** app build (RS
authoring DONE 2026-07-05) · **S-4 1065 core** compute build · **S-5/S-6** boundary + PAL/basis app builds ·
S-10 GA-700 (gated behind S-4 1065 core) · **S-13/S-14 1120 + state C-corp** app builds (RS authoring DONE
2026-07-05 `9a41581`/`87b66a4`). **▶ RS authoring NOW: S-15 NC + AL pass-through entity batch (WO-13)** — the
current RS rock (S-13 1120 + S-14 state C-corp both DONE; all earlier RS spine authoring DONE). After S-15,
net-new RS scope depends on the TaxWise forms-usage report or a law change.

---

## ⛔ GATE GUARDRAILS (the only dates that matter — from SEASON_PLAN)
- **~Jul 16** — retry LLC e-file application (EIN propagation).
- **Sep 15** — bank agreement signed, or desk-collect fallback locked.
- **Oct 1** — GA DOR approval path confirmed w/ dates; entity decision (LLC vs TTS creds).
- **Nov 1** — regression bed ≥95% to the dollar; A2A healthy.
- **~Nov** — IRS publishes TY2026 schemas → re-cut + re-test window opens.
- **Dec 1** — FEATURE FREEZE (bug-fix only).
- **Jan 2** — go/no-go per module. *(Downstream: Mar 15 entities, Apr 15 1040/1041.)*

---

## THE SPINE (dependency-ordered — just keep rolling)

**S-1 · [RS]✅→[APP]✅ (acceptance pending) · 1040 ATS gap specs (WO-01)** — 4835/8835/8936/
8936_SCHA specs authored+seeded+exported (RS, 7/4). App mappers BUILT: the whole 1040 ATS
scenario set (S2/S3/S4/S5/S8/S12/S13) is built and live-XSD-valid; S1 dropped.
- [x] RS specs — 2026-07-04   - [x] tts S3/S4 (+all) mappers — 2026-07-04 (built, live-XSD-valid)
- [ ] S3/S4/S5/S8 ATS **accepted** ← on the Shelf: artifacts UNSIGNED (placeholder EFIN);
  awaiting Ken sign + IFA upload (+ S5/S8 pre-§12.5 artifact rebuild).

**S-2 · [APP]✅ · Proforma/rollover PRODUCER** — DONE 2026-07-04 `3a55f31`. App-to-app
snapshotter; fires on 1040 DRAFT→FILED; owns the year-shift (Sch D carryover, 4562 L13, 8606
L14, Roth basis, aggregate PAL). v1 follow-ups (non-blocking): per-activity PAL roll (v1 is
aggregate); `_sr_py_prior_refunded`/`_sr_py_unused_credits` sourcing; TaxWise 1040 importer (Oct).

**S-3 · [APP]⏳ ∥ · Brokerage extraction-to-production front end** — skeleton DONE (7/4,
`import_brokerage_summary` → 8949 Exception-2, mig 0160). REMAINS: OCR/parse front end +
YELLOW render + preparer-confirm UI. Not spec-gated. Parallel.

**S-4 · [RS]✅→[APP]⏳ · 1065 core (WO-02) — RS authoring DONE; APP build IN PROGRESS (2026-07-05).**
▸ 6 specs cached (`a4f3370`). ⚠ found the app's 1065 page-1 + Sch K were on PRE-2025 line numbering (verified
vs f1065.pdf). **✅ leg 1a — page-1 2025 renumber (`10f1fd2`):** ord business income now line 23 (was 22),
K1←line 23, NEW line 20 §179D, seed→286 lines, 15 DB + flow 398 + SE 36 green. **✅ leg 1b — Schedule K 2025
renumber + Analysis line (2026-07-05, eighteenth session):** K16a→line 21, line 16→K-3 checkbox, K13d→K13b,
+K13c/K13e, computed `K_ANALYSIS`, seed→290 lines; 5 new diagnostics (`rules_1065_schk.py`); k1_allocator
renames; ★ fixed `aggregate_schedule_d` zeroing 1065 royalties (K7); 7 pure + 9 DB, flow 398 (`8ad96d8`).
**✅ leg 2 — Schedule L balance check (2026-07-05, eighteenth session):** new `rules_1065_l.py` (D_L_BALANCE_BOY/EOY
error, D_L_21_M2_TIE + D_L_M3 warning, D_L_EXEMPT info; B6 exemption suppresses balance errors); the L14/L22
total compute already existed → validation-only; 7 DB tests (`f55a0c8`). Build-gap #2 closed. ▶ NEXT leg 3: M-1/M-2 tie-outs
(assert K_ANALYSIS=M1_9=M2_3, RECON-ANALYSIS); then leg 4 K-1 alloc reconcile, leg 5 issuer-side
`PartnerK1Computed`+1065→1040 import, leg 6 1065 flow-assertion gate. ⚠ f1065 page-1+SchK render recalibration
DEFERRED (coords stale for 2025).
All 6 core specs authored+seeded+exported (200): Schedule K spine (`1065_PAGE1`+`SCH_K_1065`),
K-1+alloc, M-1/M-2, L/B; 8825/4562/3800 confirmed cover 1065; 7-form batch in `approved_specs.py`.
Reconciled to live RS STATUS 2026-07-05 (corrected the stale "untouched beyond SE" mark). Unblocks
1065 ATS, issuer-side 1065 K-1 → 1040 import, AND the GA-700 app build (see S-10 dependency).
- [x] Schedule K — 2026-07-04   - [x] Schedule K-1 — 2026-07-04   - [x] M-1/M-2 — 2026-07-04   - [x] L/B — 2026-07-04   - [x] 8825 (covers 1065)
- [ ] issuer-side 1065 K-1 persistence → 1040 import (parity w/ 1120-S)  `[APP]`
- [ ] tts compute build/reconcile of the 35 formulas against the specs  `[APP]`

**S-5 · [RS]✅→[APP]⬜ ∥ · Boundary diagnostics (WO-04) — RS authoring DONE 2026-07-05.** New consolidated
`ENTITY_BOUNDARY` form (seeded to prod, 96 TaxForms, export 200): M-3 threshold, K-2/K-3 DFE 4-criteria gate
(computed, RED on fail), §704(c), §754/§743(d)/§734(d), multistate apportionment (P.L. 86-272). APP build remains.
- [x] M-3 · [x] K-2/K-3 DFE gate · [x] §704(c) · [x] §754 · [x] apportionment — 2026-07-05
- [ ] tts app build: wire the ENTITY_BOUNDARY RED diagnostics into the 1065/1120-S compute path  `[APP]`

**S-6 · [RS]✅→[APP]⬜ · PAL/basis deepening (WO-03) — RS authoring DONE 2026-07-05.** Amendments to
FORM_8582/SCHEDULE_E + new Form 461; seeded to prod (95 TaxForms), exports 200. APP build remains.
- [x] R1 self-rental + R2 PTP (one 8582 unit) — 2026-07-05   - [x] R3 REP checkbox + §1.469-9(g) election — 2026-07-05   - [x] R4 at-risk diag — 2026-07-05   - [x] R5 §461(l) Form 461 diag ($313k/$626k) — 2026-07-05
- [ ] tts app build: self-rental recharacterization, PTP segregation, REP checkbox, at-risk route, §461(l) EBL diagnostic  `[APP]`

**S-7…S-10 · [RS]✅→[APP] · States — SPECS ALREADY AUTHORED (7/4–7/5), app build remains.**
Authoring done a month early; these dropped OFF Ken's authoring path. Forms build now; e-file
turn-on waits on the Shelf (DOR approvals).
- [x] S-7 SC1040 + SC_SCHEDULE_NR spec — 2026-07-04   → [x] **app build COMPLETE 2026-07-05**
      across compute (`307a810`, SC1040TT 138/138) · input (`a8cb291`/`81e0809`/`4cb497a`) · render
      face (`af2c6a7`) + identity header (`13443d5`) · diagnostics (`118613d`) · **Schedule NR render
      (`d9fa2b0`** — part-year/nonresident summary lines 31-48, PYNR-gated append after the face; v1
      renders the aggregate, income-detail lines 1-30 blank). RS spec hygiene DONE 2026-07-05
      (`6e22b70`): promoted draft→active + fixed the wrong $50k pin ($2,533 → $2,360), re-seeded.
- [x] S-8 AL Form 40 spec — 2026-07-04   → [x] **app build COMPLETE 2026-07-05** (form_code "AL40"):
      compute (`28ceeab`, all 5 RS pins + the ★ liability-based federal-income-tax deduction L12) · input
      (`3edce72`, seed + AL→AL40 wiring + FIT-worksheet federal pull scoped to the 1040 + frontend) · face
      render (`938846c`, flat 2-page ALDOR template `fal40`, single value column) + identity header
      (`c81bcf2`) · diagnostics (`941cd46`, 6 D_AL40_*). RS spec is `draft` — promote→active (RS follow-up).
- [x] S-9 NC D-400 spec — 2026-07-04   → [x] **app build COMPLETE 2026-07-05** (form_code "NC_D400"):
      compute (`69cf82b`, all 8 RS pins — flat 4.25% §105-153.7 year-guarded, 85% bonus/§179 add-back w/
      conformity frozen Jan-1-2023, AGI-banded child deduction, restricted NC-itemized subset, Schedule PN
      proration) · input (`b31d5c7`, seed 4 sections + NC→NC_D400 wiring + federal-AGI pull + frontend;
      also fixed the latent AL40 `refresh_from_federal` GA-pull fall-through) · render (`c704f21`, flat
      NCDOR handwritten template `fnc_d400` w/ instructions-cover stripped, two "00" value columns +
      identity header) · diagnostics (`8358e74`, 8 D_NCD400_*). RS spec is `draft` — promote→active (RS follow-up).
- [x] S-10 GA-700 + PTET spec — 2026-07-05   → [ ] app build **⚠ gated behind S-4 1065 core
      (GA partnership numbers depend on the federal 1065 flow — render OK, trust numbers only after S-4)**

**S-11 · [RS]✅→[APP]⬜ · 1041 module (WO-09), greenfield RS-first. RS authoring COMPLETE 2026-07-05
(front door + Gate-1 scope walk = RS DECISIONS D-10; f1041_source_brief.md). APP build remains.**
- [x] spine (entity types/rates/exemptions)   - [x] DNI/IDD/Sch B   - [x] Sch G — 2026-07-05 *(all three = the one
  consolidated `1041` form — `load_1041_spine.py`; core 4 + ESBT computed; grantor/PIF/bankruptcy = structure/route/defer)*
- [x] K-1 (1041) `SCHEDULE_K1_1041` — 2026-07-05 *(issuer-side; full verbatim box codes)*   - [x] GA 501 `GA501` —
  2026-07-05 *(resident-only)*   - [x] Sch I RED-defer diag (D-2)
- All 3 forms seeded/exported (`lookup/{1041,SCHEDULE_K1_1041,GA501}/export/` = 200); RS prod 99 TaxForms / 471 FA / 859 rules.
- [ ] tts app build: fiduciary compute (DNI/IDD engine, §662 tiers, Sch G tax, K-1 issuance, GA 501) + its 4 ATS scenarios  `[APP]`
- [x] **WO-10 — Form 5227** split-interest trusts (CRAT/CRUT/PIF/CLT/§4947) — RS authoring DONE 2026-07-05
  (`load_5227.py`; DECISIONS D-11; `lookup/5227/export/` = 200). §664(b) four-tier char engine (tier-level) +
  §664(c)(2) 100% UBTI excise; CRAT/CRUT compute, PIF/CLT/§4947 structure. RS prod **100 TaxForms / 475 FA / 867 rules**.
  - [ ] tts app build: 5227 four-tier distribution character + UBTI excise (fiduciary lane)  `[APP]`

**S-12 · [APP]⏳ ∥ · Season infrastructure** (not spec-gated; roll whenever CC has a lane).
- [ ] diagnostics/review layer + workflow states   - [ ] printing at volume
- [ ] multi-preparer concurrency   - [ ] invoice/client letter for 1040 packets

**S-13 · [RS]✅→[APP]⬜ · 1120 C-corp module (WO-11), greenfield RS-first. RS authoring COMPLETE 2026-07-05
`9a41581` (+ `bf58d48` GA600 jurisdiction fix). Gate-1 D-13.** The first C-corporation entity type. 3 forms
seeded/exported (`lookup/{1120,1120_SCHL,GA600}/export/` = 200); prod 100→103 TaxForms.
- [x] `1120` spine — page-1 income/deductions → Sch C DRD (§243 50/65/100% + §246(b) limit/loss exception) →
  §172 NOL 80% → §11 flat **21%** → Schedule J total tax. §163(j)/CAMT/PHC/AET/§59A/§1062 screen+route.
- [x] `1120_SCHL` — Schedule L balance (L15==L28) + M-1/M-2 ties + Sch K $250k/$10M-M-3 gates.
- [x] `GA600` — GA C-corp: 5.19% income + §168(k)/§179 delta ($1.25M/$3.13M) + single-factor apportionment + net worth tax table.
- [x] confirmed 1125-A/1125-E/3800/4562/4797/8949/7004 already cover 1120 (entity_types carry '1120').
- ⚠ Stale GA §179 in CLAUDE.md ($1.05M/$2.62M = 2021) spun off (`task_1c8d891e`). tts compute build = [APP] lane (on hold).
- [ ] tts app build: new 1120 entity type + income→DRD→NOL→21%→total-tax compute + Sch L/M-1/M-2 + GA600 dual tax  `[APP]`

**S-14 · [RS]✅→[APP]⬜ · State C-corp batch (WO-12), greenfield RS-first. RS authoring COMPLETE 2026-07-05
`87b66a4`. Gate-1 D-14.** Reuse-state C-corp returns extending S-13 to GA's income-tax neighbors. 3 forms
seeded/exported (`lookup/{SC1120,AL_FORM_20C,NC_CD405}/export/` = 200); prod 103→106 TaxForms.
- [x] `SC1120` — 5% + §168(k) decouple/§179 $1.25M/$3.13M + license fee. ⚠ H.3368/OBBBA live-wire flag (`D_SC1120_H3368`).
- [x] `AL_FORM_20C` — 6.5% + apportioned FIT deduction (constitutional, Amendment 662) + AL conforms §168(k)/§179 + GILTI/§174 decouples.
- [x] `NC_CD405` — combined 2.25% income (S.B. 105) + net-worth franchise tax + 85% bonus/§179 $25k/$200k add-back.
- [ ] tts app build: SC/AL/NC C-corp compute (reuses the 1120 flow)  `[APP]`

**S-15 · [RS]▶ DRAFTING→[APP]⬜ · NC + AL pass-through entity batch (WO-13), greenfield RS-first. ADDED 2026-07-05
(Ken — completes the adjacent-state pass-through track; SC pass-through done via SC1065/SC1120S/PTET, D-9). THE
CURRENT RS AUTHORING ROCK.** Reuses NC/AL conformity + the SC pass-through pattern. Required set (all GAP at gap-check):
- [ ] **NC D-403** (partnership) + **NC CD-401S** (S-corp) + the **NC Taxed PTE** election (each state's PTET differs — verify, don't clone GA)
- [ ] **AL Form 65** (partnership) + **AL Form 20S** (S-corp) + the **Alabama Electing PTE** tax (Act 2021-1)
- reuse: NC 85% bonus/§179 $25k/$200k + Jan 1 2023 conformity (as NC D-400/CD-405); AL conforms §168(k)/§179 + GILTI/§174 (as AL 20C).
- scope-walk items: PTET compute depth per state; owner-side credit vs exclusion; NC franchise on CD-401S (S-corps pay NC franchise); AL BPT separate.

---

## THE SHELF (blocked on external — each with its unblock action)
*These are the "holes." Work the unblock action; the item then drops onto the Spine.*

- **1120-S mapper + 7004 mapper** `[APP]` ← **IRS business-family access notice.**
  UNBLOCK: watch e-Services inbox; e-help 866-255-0654 if not arrived in days. *High impact.*
- **Remaining 1040 ATS scenarios** ← **SOR schema pulls** (TY2025 1040 business rules,
  2025v5.3 schema, 4868 schema from e-Services SOR).
- **S8 / S5 IFA upload + S2/S3/S4 acceptance** ← artifact rebuild (pre-§12.5) + **Ken sign** (EFIN/PINs).
- **LLC e-file application** ← **EIN propagation.** UNBLOCK: retry ~Jul 16 (gate).
- **A2A (ASID/cert/SOAP/comm test)** ← **cert issuance 7–14 days.** SOAP build can roll; comm test gated.
- **State e-file enablement** ← **DOR software-dev approvals** (the four calls). *(Specs done;
  only the e-file turn-on waits here — the forms/compute build now.)*
- **Bank integration** `[APP]` ← **bank agreement** (⛔ Sep 15). UNBLOCK: TPG/Republic/Pathward calls.
- **Regression bed** `[RS/EXT]` ← Ken pull 40–50 real returns + **TaxWise as-filed-XML ticket.**
- **TY2026 re-cut + re-test** ← **IRS publishes TY2026 schemas (~Nov).**
- **Forms-usage-driven scope adds** ← **TaxWise forms-usage report** (any form on 20+ returns
  not yet built becomes a new Spine item).

---

## Open reconciliation debts (RS-first drift — close these)
- ✅ **RESOLVED 2026-07-05 (RS side) — GA-500 military exclusion.** Reconciled: the **RS spec is CORRECT
  and authoritative** (under-62 gate; $17,500 base + $17,500 only if GA earned income > $17,500; max
  $35,000; 62+ falls under the general exclusion) — research-verified vs 2025 IT-511 + Form 500 Sch 1 p3 +
  O.C.G.A. §48-7-27(a)(5.1). The **app's `min(mret, 35000)` was OVER-inclusive** (ignores the age gate, the
  earned-income condition, and the $17,500 base ceiling) — the app was BEHIND the law, not ahead. **tts fix
  handed off (`task_f550dfd2`)** to correct the app compute to the worksheet. No RS spec change needed for
  TY2025. RS loader annotated with a year-keyed **SB 31** flag (full military exemption starts **TY2026**;
  the 2026 placeholder in `load_ga500_form_500.py` is stale — re-verify enrolled SB 31 before the TY2026 build).

## Maintenance
- **CANONICAL HOME: tts-tax-status repo** (both CC contexts pull at boot). No duplicate copy in
  the tax-app repo — the tax-app boot pulls tts-tax-status and reads this file from there.
  SEASON_CHECKLIST.md is retired (tombstoned in tax-app; removed here).
- Session close: tick completed Spine items w/ SHA; move newly-unblocked Shelf items onto the
  Spine; update NOW WORKING ON + NEXT. New work = added to Spine (or Shelf if externally
  blocked), never silent.
- Hard dependencies (do not reorder): spec-before-app-build (RS-first); S-4 K/K-1 before 1065
  ATS, before 1065→1040 K-1 flow, AND before the GA-700 app build; S-6 before the regression
  bed locks. Soft: remaining state app-build order, S-11 timing, all `∥` items.
