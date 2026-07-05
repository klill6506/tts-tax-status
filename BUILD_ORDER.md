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

## ▶ NOW WORKING ON — **S-7 SC1040 app build** — compute leg landed `307a810` (2026-07-05);
input/render/diagnostics legs remain. (Paused mid-input-leg for this planner migration.)
## ▶ NEXT — authoring: **S-4 Schedule K (1065 core)** [RS/Ken] · app: finish S-7 SC1040, then
S-8 AL Form 40 / S-9 NC D-400 builds ∥ S-3 brokerage front end

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

**S-4 · [RS]⬜→[APP] · 1065 core (WO-02) — THE next authoring rock (untouched beyond SE).**
Fresh-author per D-1; reconcile the 35 existing compute formulas against each spec. Unblocks
1065 ATS, issuer-side 1065 K-1 → 1040 import, AND the GA-700 app build (see S-10 dependency).
- [ ] Schedule K   - [ ] Schedule K-1   - [ ] M-1/M-2   - [ ] L/B   - [ ] 8825
- [ ] issuer-side 1065 K-1 persistence → 1040 import (parity w/ 1120-S)

**S-5 · [RS]⬜→[APP] ∥ · Boundary diagnostics (WO-04)** — M-3 threshold, K-2/K-3 DFE-fail,
§704(c), §754, entity apportionment. Author alongside S-4.

**S-6 · [RS]⬜→[APP] · PAL/basis deepening (WO-03).** FORM_8582 exists (12 rules) — these are
AMENDMENTS. Do BEFORE the regression bed locks (they change 8582 output; else re-tie returns).
- [ ] R1 self-rental + R2 PTP (one 8582 unit)   - [ ] R3 REP checkbox   - [ ] R4 at-risk diag   - [ ] R5 §461(l) diag

**S-7…S-10 · [RS]✅→[APP] · States — SPECS ALREADY AUTHORED (7/4–7/5), app build remains.**
Authoring done a month early; these dropped OFF Ken's authoring path. Forms build now; e-file
turn-on waits on the Shelf (DOR approvals).
- [x] S-7 SC1040 + SC_SCHEDULE_NR spec — 2026-07-04   → [~] **app build IN PROGRESS** — compute
      leg done 2026-07-05 `307a810` (SC1040TT midpoint tax verified 138/138 vs SCDOR;
      16 pure tests; flow gate 398). Input/render/diagnostics legs remain.
- [x] S-8 AL Form 40 spec — 2026-07-04   → [ ] app build (⚠ fed-tax-deduction quirk)
- [x] S-9 NC D-400 spec — 2026-07-04   → [ ] app build
- [x] S-10 GA-700 + PTET spec — 2026-07-05   → [ ] app build **⚠ gated behind S-4 1065 core
      (GA partnership numbers depend on the federal 1065 flow — render OK, trust numbers only after S-4)**

**S-11 · [RS]⬜→[APP] · 1041 module (WO-09), greenfield RS-first.**
- [ ] spine (entity types/rates/exemptions)   - [ ] DNI/IDD/Sch B   - [ ] Sch G   - [ ] K-1 (1041)   - [ ] GA 501   - [ ] Sch I RED-defer diag (D-2)

**S-12 · [APP]⏳ ∥ · Season infrastructure** (not spec-gated; roll whenever CC has a lane).
- [ ] diagnostics/review layer + workflow states   - [ ] printing at volume
- [ ] multi-preparer concurrency   - [ ] invoice/client letter for 1040 packets

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
- **GA-500 military-exclusion fix** (7/5, `min(mret, 35000)`, IT-511-verified) was made
  app-side with **RS spec reconciliation pending**. Author/reconcile the spec so compute and
  spec agree — the app should not stay ahead of the law. Handoff:
  `docs/rs_handoff/2026-07-05_ga500_military_exclusion_fix.md`.

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
