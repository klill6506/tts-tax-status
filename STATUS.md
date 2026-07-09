# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-09, thirty-sixth session ("go" — the standing overnight work order).
Unit: **S6 unit 2 — the entity-8824 unit COMPLETE end-to-end** (tts `e2cae48`; RS `a54c406`
FA activation). Stopped CLEANLY at the unit boundary per the work order's 80% context
guardrail — unit 3 (the S6 scenario build) remains.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ⏰ OVERNIGHT WORK ORDER (Ken, 2026-07-08 night — autonomous, NO QUESTIONS) — STILL STANDING
Units 1→2→3 in order, asking nothing; every judgment call pre-ruled (the four S6 rulings +
standing policy). **Unit 1 ✅ (s35) · Unit 2 ✅ (s36).** Law-vs-key on the 8941: engine
emits the LAW (51,014); UPLOAD stays Ken/e-help. The S6 truck 8824: personal property =
hard RED (D_8824_009 — now firing on entity returns too); the scenario reproduces the
key's 8824 face via a documented-quirk override only. Commit+push every green leg; stop
cleanly before 80% context (done here — unit boundary).

## ▶ RESUME HERE — S6 unit 3: `mef_build_ats_1120s_s6`
Engine-driven scenario build (the S5 playbook: `apps/efile/ats/scenario5_1120s.py` +
`mef_build_ats_1120s_s5` rollback-txn command). PIN-signed via `Signature1120SInfo`
(no binaries). ⚠⚠ UPLOAD-GATED on the 8941 key-inversion e-help item (REVIEW_QUEUE s34).
S6 quirks to honor (docs/mef/scenarios/scenario6_1120s_analysis.md + the s34/s35 memories):
- 8941: engine = statutory 51,014 chain (F8941-T1); the key's inverted 12,753 chain
  (L8/L9/L12/L16, K13g, K-1 6,377/6,376) documented only — build to LAW, note divergence.
- The truck 8824 = PERSONAL property → D_8824_009 hard RED + the MeF extract refuses;
  the key's 8824 face rides a documented-quirk override at build (like the S5
  full-facts K-1) — decide the override shape at build time, never engine change.
- K12d §59(e)(2) row carries the §212 502,369 · K-1 odd dollars to FIRST owner in the
  key (verify vs allocate_whole residual-to-LAST) · stale 4562 Part I limits on the key
  face (OBBBA figures win) · box 13 code BA not P.
- Capital chain now fully live for the build: Dispositions → 8949/SchD docs + the NEW
  entity-8824 lane (unit 2) if S6 carries a real-property exchange arm.

## Session-36 state (tts `e2cae48` + RS `a54c406`)
- **Entity-8824 unit complete** — all legs green; detail in form_coverage_tracker (s36
  entry). R-8824-ENTROUTE: entity 4797 L5/L16 + Sch D (1120-S) L5/L12 → K7/K8a; IRS8824
  MeF docs (ref 1291); entity render; D_8824_* entity coverage; FA-ENT-8824-01 active.
- **Two spec-silent rulings shipped refuse-not-drop (REVIEW_QUEUE s36, Ken to ratify):**
  entity §1043 = D_8824_010 escalates to ERROR + MeF refuse; 1065 capital-character LKE =
  NEW D_8824_011 RED ("enter K8/K9a manually" — no Schedule D (1065) aggregation or spec).
- **XSD fact worth keeping**: no refDocId channel exists between IRS8824 and SchD/4797
  (SchD's fixed reference list = BinaryAttachment IRS8949; IRS4797 has no
  referenceDocumentId) — the resume pointer's "link via refDocId" was XSD-impossible;
  ties are numeric via reconcile-or-refuse.
- **FA mirror**: pinned set now 26 (25 + FA-ENT-8824-01); deployed export 30 actives —
  the drift reconciliation pass stays queued (BUILD_ORDER ▶ NEXT).
- No new migrations; seed_rules run on prod (D_8824_011 + description updates).

## Active gates
- **Flow-assertion gate 447** — green (446 + FA-ENT-8824-01). MeF 1120-S suite 64
  (live-XSD incl. IRS8824 ×2) · 8824 entity 12 (5 pure + 7 DB) · 1040-lane 8824 18 +
  render 8 · tsc 0 / vitest 275.
- ⚠ Pooler degraded window during s36: rotating single-test "terminating connection due
  to administrator command" kills on long DB batches (test_4797_pipeline_leg) — every
  test green in at least one run; infrastructure class, re-verify on a healthy window
  if it recurs.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Recorded in SEASON_PLAN (scope-change note) + BUILD_ORDER (mission header + NEXT ladder).
1120 + 709 = Ken-directed scope ADDITIONS (709 verified MeF-e-fileable — IRS opened the
709 family on MeF 7/14/2025; needs RS spec authoring + its own SOR schema request).
**No piecemeal ATS testing** (Ken rule, same day): finish ALL work for the full 1120-S
scenario set FIRST, then run the upload loop — the S5-only upload is OFF.

## 🟢 IRS e-services approvals VERIFIED (Ken, 2026-07-09): 1120-S / 1065 / 1120 / 1041
Business-family ATS testing is authorized. Per family: **1120-S** — S5 package ready,
S6 = unit 3 (NOW), S7/S8 need the declared-forms ruling; test when the SET is complete.
**1065** — mapper leg 1 only; needs its scenario lane. **1041** — engine complete;
SOR-blocked (ATS-active v5.3 schemas+BR re-request). **1120** — RS specs done
(S-13/S-14); app build now IN scope per the mission. Proof to stash when convenient
(runbook + A2A enrollment): the e-file Application Summary (form families / provider
options / ETIN-EFIN status) → PDF into docs/mef/.

## ▶ Waiting on Ken / external
0. **TaxWise forms-usage report incoming** (Ken, ~next few days): all forms across last
   year's returns WITH per-form counts, entities included. When it lands → the
   **forms-gap-analysis unit** (diff vs engine + MeF-builder coverage → (a) per-family
   IRS questionnaire checklists for the e-services Software Package checkboxes — Ken
   marks ONLY what we support, extend as forms land; (b) prioritized app-build queue;
   (c) RS spec-authoring queue for Ken's lane). Ken holds the checkbox marking until
   the checklist exists. Also Ken-noted: **A2A build should NOT wait until November** —
   pull it forward after the 1120-S scenario set (BUILD_ORDER re-order pending).
1. **S7/S8 declared-forms ruling** (shapes "the full 1120-S set"): recommendation —
   declare only forms the firm files (5471/8975/8858 OUT → S7/S8 likely not required);
   confirm with the ATS assistor/e-help (Pub 1436; REVIEW_QUEUE).
2. **8941 key-inversion** — law (51,014) vs key (12,753) — raise with e-help before the
   S6 upload rides the full-set loop (REVIEW_QUEUE s34).
3. Ratify the two s36 entity-8824 spec-silent rulings (REVIEW_QUEUE s36).
4. ATS assistor / e-help call (866-255-0654) — the five asks (docs/mef/ATS_UPLOAD_RUNBOOK.md).
5. 1041 ATS-active v5.3 schemas+BR + 1040 v5.4 BR — SOR re-request. NEW: add the **709
   family schemas** to the next SOR want-list.
6. SEASON_PLAN month-by-month re-cut for 1120/709 — dedicated planning session with Ken.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
