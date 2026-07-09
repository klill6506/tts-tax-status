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

## ⚡ MISSION (Ken, 2026-07-09): finish **1040 · 1120-S · 1120 · 1065 · 1041 · 709 by end of 2026**. 1120 + 709 are Ken-directed scope ADDITIONS (see the SEASON_PLAN scope-change note; 709 verified MeF-e-fileable — IRS opened the 709 family on MeF 7/14/2025). **No piecemeal ATS testing** — complete ALL work for the full 1120-S scenario set FIRST, then run the upload loop (the S5-only upload is OFF; e-services business-family approvals Ken-verified 2026-07-09).

## ▶ NOW WORKING ON — **idle — the 1120-S ATS completion lane is Ken-gated**. 🏁 s37: **S6 unit 3 — the Scenario-6 build COMPLETE** (tts `f824dc0`; all 41 key lines to the dollar; PIN-signed; live-XSD-valid; ⚠⚠ upload gated on the 8941 e-help ask) → **the s34→s37 S6 lane AND the overnight work order (units 1→2→3) are DONE**. The lane continues per the no-piecemeal rule: **S7/S8 scope ruling (declared-forms, Pub 1436 — Ken/e-help) → any required S7/S8 builds → full-set bundle → the live upload loop** (8941 key-inversion + the s36 entity-8824 ratifications resolve in-loop). CC-startable without Ken: the REVIEW_QUEUE interleaves (RS FA-export reconciliation pass · 1065/1041 allocator residual-offset units) or the 1065 ATS scenario lane.

**Session 37 wrap (2026-07-09; "go" — the standing overnight work order, unit 3):**
- [x] `[APP]` **S6 unit 3 — the Scenario-6 build `mef_build_ats_1120s_s6`** — 2026-07-09
  `f824dc0`. `apps/efile/ats/scenario6_1120s.py` + command (rollback-txn, seq 22) + 4 DB
  tests: ALL 41 pinned key lines tie TO THE DOLLAR (first scenario exercise of the s34
  8949/SchD chain — K7 78,649 — and the s35/s36 8941 + entity-8824 units); submission +
  manifest live-XSD-valid 2025v6.2; **PIN signature end-to-end** (Signature1120SInfo, no
  binaries). NEW mapper channel: **K-1 box-10 code** (the XSD group requires a per-item
  code) — caller-supplied `extract.k10_other_income_code` (listed_evidence pattern; the
  scenario asserts the key's A; an un-asserted nonzero K10 still refuses) → DEFERRAL_AUDIT
  s37 (app input leg + print-side bare-amount box 10). Documented-quirk overrides at build
  only: 8941 = LAW (K13g 51,014; the key's inverted 12,753 chain documented — upload
  e-help-gated) · truck 8824 `is_real_property=True` (face = key exact, zero feeds; Ken's
  override ruling) · M1_6b=0 override (M1_1 back-computes to the key's book loss (10,842)) ·
  1125-A's own 540 typo absorbed in the Misc row · Silverado 24,492 · K-1 odd dollars to
  Carrie via sort_order 0. Suites: S6 4 DB · MeF 1120-S 64 · flow gate 447 (no compute
  changes). ⚠ new infra note: a fresh pytest right after a completed run can hit "test DB
  in use" → `--reuse-db`.

**Session 36 wrap (2026-07-09; "go" — the standing work order):**
- [x] `[APP]` **S6 unit 2 — entity-8824 unit, ALL LEGS** — 2026-07-09 `e2cae48` (RS `a54c406`).
  entity_8824_feeds_from_rows (pure) → aggregate_dispositions (L21+ord-L22 → entity 4797 L16;
  §1231 L22 → L5 → K9/K10 + page-1 4/6, both entities) + aggregate_schedule_d (capital L22 →
  Sch D (1120-S) 5/12 → K7/K8a nets; entity §1043 EXCLUDED by construction) · Like-Kind tab on
  both entity editors (shared section/endpoint) · MeF refuse seam RETIRED → per-exchange IRS8824
  docs (ReturnData ref 1291; extract refuses red-defer/§1043/related-party — Part II identity
  unmodeled; ⚠ NO refDocId channel exists 8824↔SchD/4797 in the XSD — ties numeric via
  reconcile-or-refuse; ⚠ NN line 19 omits on a realized loss, signed L24 carries it) ·
  D_8824_001-010 now fire on entities (009 = the F8824-E1 truck arm) + D_8824_010 entity-§1043
  ERROR arm + NEW D_8824_011 (1065 capital → K8/9a manual; no Sch D (1065) aggregation/spec) —
  both spec-silent rulings → tax-app REVIEW_QUEUE for Ken · render_8824_entity (shared core,
  1040 output unchanged 8/8) · FA-ENT-8824-01 ACTIVATED (Supabase reseeded; deployed export
  30 actives; pinned mirror 26) + runner — **flow gate 447**. Suites: 12 unit (5 pure + 7 DB) ·
  MeF 64 live-XSD (incl. IRS8824 ×2) · tsc 0/vitest 275. ⚠ pooler degraded window (rotating
  admin-kill single-test errors on long batches — every test green in ≥1 run; not code).

**Session 35 wrap (2026-07-09 overnight; the standing work order — autonomous):**
- [x] `[APP]` **S6 unit 1 — Form 8941 unit, ALL LEGS** — 2026-07-09 `9e65aff`. compute_8941
  (statutory §45R(d)(3)(A) chain; WS3 floor-to-$1,000; WS5/WS6 reductions clamped ≥0; face
  $33,000 vs WS6 $33,300 encoded verbatim, D_8941_005 bands) · Form8941 model (migs 0179+0180
  RLS prod) · `form-8941` GET/PATCH w/ mutation-recompute (**row-locked — probe caught
  concurrent focusout autosaves losing fields via stale full-row serializer saves**) ·
  "8941 Credit" 1120-S editor tab (live-UI probe verified: autosave → K13g 51,014 on Sched K;
  probe deleted) · K13g → residual-offset K-1 allocation + box 13 code **BA** (i8941 verbatim;
  key's P = pre-2023 quirk) on the shared `k13g_is_8941_sourced` bridge-gate; un-sourced K13g
  still refuses; print box 13 packs 5 rows dynamically · f8941 AcroForm render + manifest +
  packet + action · IRS8941 MeF doc (XSD lineNumber-verified; ReturnData ref 1697; K13g
  OtherCreditsAmt refDocId; reconcile-or-refuse) live-XSD valid · D_8941_001-006 seeded
  (§280C warning-only per the Ken ruling) · **FA-8941-01/02 ACTIVATED** (RS `ab3b0ab`,
  Supabase reseeded) + tts runners — **flow gate 446**. ⚠ FA-mirror PINNED (prior set + 8941):
  the deployed export now carries the s32-drift actives (FA008-012/RC001-variant/ENT-BND/
  4562-179) with no tts runners — the queued reconciliation pass adopts them.
- Deferrals filed: 1065-K15 + 1040-3800-4h 8941 lanes · manual-other-credit + 8941 K13g
  coexistence · entity-boundary PATCH same race class (low stakes).

**Session 34 continuation wrap (2026-07-08 night; Ken: "Can you author those?" + four rulings):**
- [x] `[RS]` **The S6 trio authored + seeded** — 2026-07-08 RS `b4c71b8`. **load_8941 GREENFIELD**
  (verbatim f8941 2025 face + fetched i8941 2025; 16 facts/8 rules/23 lines/6 diags/4 scenarios/
  2 staged FAs; Ken rulings: line 5 preparer-entered · §280C diagnostic-only) · **load_8824
  entity amendment** (entity_types += 1120S/1065; R-8824-ENTROUTE — entity Sch D L5/L12 +
  entity 4797 L16/L5; deferred = M-1 timing never AAA; line 25 informational, §1.168(i)-6 =
  future unit; F8824-E1 = the S6 truck as a personal-property RED; FA-ENT-8824-01 staged;
  Ken rulings: routing-only · hard RED + scenario override) · **SCHD_1120S renumbered to the
  2025 face** (verbatim vs f1120ssd.pdf; net ST = line 7 → K7; stale pre-2025 line rows
  DELETED in-loader; R011/R012 + Sch K R013/R014 corrected). SQLite-validated → Supabase
  (TaxForms 121/FA 550) → deployed exports verified → tts mirrors refreshed (8941_spec.json
  NEW). ⚠ TWO new REVIEW_QUEUE flags from the verbatim sourcing: the f8941 face
  $33,000/$67,000 vs i8941 WS6 $33,300 SELF-CONTRADICTION (D_8941_005) · ⚠⚠ the ATS S6 key
  INVERTS the WS5 FTE phaseout (12,753 = the reduction; law = 51,014 — F8941-T1 pins law,
  F8941-T2 documents the key). Retraction: the "null export ids" claim = wrong-key dump.

**Session 34 wrap (2026-07-08 night; "go" — 1120-S Scenario 6 kickoff):**
- [x] `[APP]` **S6 buildable mapper legs COMPLETE** — 2026-07-08. **IRS8949 doc mapper**
  (first anywhere — the 1040 Sch D is aggregate-path-only): per-box Part I/II groups,
  rows + totals. **IRS1120SScheduleD mapper**: box groups 1b/2/3//8b/9/10 + line 7/15
  nets, reconcile-or-refuse vs flowed K7/K8a (K-without-rows refuses); QOF caller-supplied;
  refDocId links (Sch K 7/8a → SchD → 8949). **Disposition `form_8949_box`** (mig 0178
  prod; blank → C/F via `resolve_8949_box`, contradictions refuse; serializer + entity
  Dispositions-tab select, live-UI probe-verified). **PIN-signature header**
  (`Signature1120SInfo`: PractitionerPINGrp/PINEnteredByCd/SignatureOptionCd/officer
  TaxpayerPIN+jurat facts/IRSResponsiblePrtyInfoCurrInd; half-signed PIN option refuses).
  Refuse seams: capital-tx expenses-of-sale · 'various' sold date · **entity
  LikeKindExchange rows (8824 entity gap — declared-form fidelity)**. Suite 55 (live-XSD
  capgains+PIN green) · flow 444 · S5 DB green · tsc 0/vitest 275.
- [x] `[APP→RS]` **S6 RS-gap handoff filed** — 8941 greenfield (`lookup/8941` 404) ·
  8824 entity extension (entity_types ['1040']; Sch D (1120-S) L5/L12 + entity 4797 L16
  routing) · SCHD_1120S renumber (draft; R011 "L5→K7" vs the 2025 face's line 7; null
  diagnostic/test ids in the export). REVIEW_QUEUE s34 item + the handoff doc carry the
  full authoring scope. **S6 build sequence:** RS items → app 8941 unit + entity-8824
  unit (drop the refuse seam) → `mef_build_ats_1120s_s6`.

**Session 33 wrap (2026-07-08 night; "go" — S-17b, the last non-e-help queue item):**
- [x] `[APP]` **S-17b — 1040 refund direct deposit end-to-end** — 2026-07-08. Extract:
  `DirectDepositSource` + `_extract_direct_deposit` (elected only on a line-35a refund with
  BOTH TaxReturn bank fields; partial/invalid REFUSES — never a silent paper-check downgrade;
  no refund → bank fields ignored). Builder: IRS1040 35b/c/d directly after RefundAmt +
  header `RefundDisbursementGrp` RTN/DAN (R0000-250/251); ⚠ unpublished enum → DD emits the
  reasoned "1" via module constant (`REFUND_DISBURSEMENT_CD_DIRECT_DEPOSIT`, e-help Q#5
  one-line fix); paper check keeps the live-accepted "0". `values.py` RTN/DAN validators
  pinned to efileTypes.xsd 2025v5.3. Input leg: NEW "Refund Direct Deposit" card on the 1040
  Payments tab (the bank fields had been entity-editors-only — a 1040 preparer had NO input
  path). Verified: pure 51 (live-XSD DD return) · flow gate 444 · tsc 0 / vitest 275 ·
  live-UI probe-firm round-trip (autosave → DB exact; probe deleted). DEFERRAL_AUDIT s28
  item (2) RESOLVED.

**Session 32 wrap (2026-07-08 evening; "go" — the Ken-set non-e-help queue, all spec-first):**
- [x] `[RS+APP]` **§179-disposition pass-through unit COMPLETE** — 2026-07-08 RS `d9923e7` /
  tts `421d365`+`5b8fa25`. RS 4797 amended with i4797 + i1120s (p.15/41) + i1065 (p.20/52)
  VERBATIM (`R-4797-ENTPASS` · `D_4797_ENTPASS` · E-scenarios · NEW `IRS_2025_1065_INSTR`
  source · FA-ENT-4797-179 + FA-1120S-M2-179; integrity gate extended, ALL PASS). Engine:
  aggregate_dispositions EXCLUDES §179-passed-through disposals (elected **+ prior** —
  ⚠ `sec_179_prior` is the prior-year-§179 field; the first pin run caught a 1,000 leak into
  current-year K11/L14); `sec179_disposition_facts` single source → K-1 box 17 code K "STMT"
  + statement page + per-shareholder `DisposOfPropWithSect179DedStmt` XML w/ refDocId;
  `ENT179_GAIN` (seeded) → M-2 3a formula + MeF statement row; refuse-seam RETIRED; the S5
  truck LIVE (engine emits the LAW — pro-rata 700/500/500 per K-1, gain 1,400 once; the key's
  full-facts/2,800 = documented quirk). Flow gate 440→443. Boundaries: 1065 box-20-L print
  rides the future partner-K-1-PDF unit; owner-side 1040 reporting = own unit.
- [x] `[RS+APP]` **K-1 residual-offset rounding COMPLETE** — 2026-07-08 RS `05cbe72` / tts
  `100bd71`. RS K1_1120S `R-K1-ROUND` + `FA-K1-ROUND`; `k1_issuer.allocate_whole` (half-up
  per owner, residual to the LAST shareholder, canonical order) — **Σ K-1 == Schedule K
  exactly** (matches S5 1,788/1,787·5,732/5,731 AND S6 odd-dollar-to-first). Pins re-pinned;
  DB-verified. Gate 443→444. ⚠ 1065 + 1041 allocators carry the same drift class →
  REVIEW_QUEUE (each its own spec-first unit).
- [x] `[RS]` **load_ga501 amendments (the s31 handoff, all 3 items)** — 2026-07-08 `05cbe72`.
  HB 1199 conformity text (D_GA501_CONFORM/DEPR) · line_map 15→27 (9c credit cap + settle
  block 11a-20 face verbatim, NEW R-GA501-SETTLE + 6 facts + GA501-T6) · deployed export
  verified; tts mirror refreshed + T6 pure pin (17/17).
- [x] `[APP]` **S5 binary-attachment leg (8453-CORP + 8822-B)** — 2026-07-08. ⚠ 8453-CORP =
  `f8453crp.pdf` on irs.gov (8453-C gone; 8453-S superseded — manifest annotated); templates
  downloaded+hashed; field maps `f8453corp_2025`/`f8822b_2025` (PDF-validated);
  `apps/tts_forms/attachments.py` renderers; builder `build_binary_attachment_docs`
  (ReturnData ref 5014, LAST) + header binaryAttachmentCnt; the S5 package carries both PDFs
  in /attachment/ (production: swap in the officer-SIGNED 8453 scan). → **S5 = complete
  uploadable package.**
- [x] `[APP]` **4797 full-file DB run** (the s29 pooler-in-flight stamp) — green in the final
  s32 batch.
- [x] `[APP]` **1120-S scenario 6/7/8 fact sheets** — agent-extracted (docs/mef/scenarios/,
  local-only). S6 = next buildable (8824 LKE + 8941 + 8949/Sch D, PIN-signed); S7 =
  M-3/5471/Schedule N (likely OUT of declared-forms ATS scope — Pub 1436 decision for Ken);
  S8 = 8975 CbC + 8858 (⚠ its PDF text layer silently drops regions — rasterize when mapping).
- [x] Drive-bys: stale manifest trip-wire re-baselined (56→80); RS FA-export drift documented
  (FA008-012 dropped from the deployed export + previously-staged FAs now active) →
  REVIEW_QUEUE reconciliation pass.

**Session 31 wrap 3 (2026-07-08; Ken: "go ahead with the 1041 FA authoring plan"):**
- [x] `[RS]` **1041 flow-assertion family consolidated + activated** — 2026-07-08 RS `adc4710`.
  Root cause of the "zero FAs" export: the 2026-07-05 S-11 authoring had staged 10 DRAFTS across
  the spine/K-1 loaders, never activated. NEW `load_1041_flow_assertions` = the single FA home
  (17 active + 2 staged; FA-1041-TAX/FA-K1041-RECON disabled as superseded; drafts tombstoned out
  of the spec loaders so a reseed can't regress). SQLite-validated · Supabase-seeded · deployed
  export = exactly the 17 actives.
- [x] `[APP]` **S-11 leg 8a — the 1041 flow-assertion gate** — 2026-07-08 `06c8946` + tag
  `1041-complete`. Export cached (`flow_assertions_1041.json`); `_RUNNERS_1041` 17 pure runners
  over compute_1041 / rate-schedule / cap-gain worksheet / allocate_k1_boxes / compute_ga501.
  **Full gate 440.** → **S-11 COMPLETE end-to-end; the 1041 fully ticks.** Staged pair
  (trust-side 8960 · K-1 box-14H import) activates when each compute lands.

**Session 31 continuation wrap (2026-07-08; "continue"):**
- [x] `[APP]` **S-11 legs 7 + 8b — beneficiary UI/API + f1041sk1 K-1 PDF + live frontend verify** —
  2026-07-08 `7c62cc0`. K-1 PDF: position-correlated f1041sk1 map (box 9 3 rows / 11 five / 12 five /
  13 three / 14 six); render_k1_1041 prints the SAME single-source allocations persisted to
  BeneficiaryK1Computed (box-11 overflow → STMT + statement; grantor refuses); k1s package +
  render_complete wired. Input link closed: BeneficiarySerializer + nested payload + CRUD endpoints
  w/ mutation-recompute; FIDUCIARY_TABS (the 1041 editor had been falling through to the 1120-S tab
  set — Sch G/ESBT/entity/payments unreachable) + Beneficiaries tab + GA501_SECTION_TABS. LIVE UI
  verify (isolated probe firm, deleted after): compute round-trips, Sch B IDD, 60/40 K-1s, GA-501
  created in-app (L8 449 ✓), K-1 Package PDF served. 6 DB tests; tsc 0; vitest 275.
- [x] `[APP]` **Leg 8a BLOCKER FOUND**: RS flow-assertion export for 1041 (all key spellings) = 0
  assertions — the S-11 RS authoring shipped no FAs. Gate-file-only FAs are forbidden (drift rule) →
  REVIEW_QUEUE item w/ the proposed FA set (DNI chain · taxable chain · §642(b) · Sch G · ESBT ·
  K-1 reconciliation · payments). `[RS]` authoring awaits Ken's green light.

**Session 31 wrap (2026-07-08; "go" — autonomous, top unblocked spine item):**
- [x] `[APP]` **S-11 leg 6 — GA Form 501 fiduciary state return (all four legs, one unit)** —
  2026-07-08 `370fdb0`. Spec-first from the live RS GA501 export. Dedicated `compute_ga501`
  (fed-1041-ATI base → §48-7-27 Sch 2 netting → beneficiary share at L4 → $1,350/$2,700
  exemption → 5.19% year-keyed → face 9c cap → 11c-20 settle block as face-arithmetic-beyond-
  spec); all 4 RS pins to the dollar, 16 pure. seed_ga501 42 lines prod-seeded; trust→GA-501
  map + create/refresh dispatch + 1041-L17 pull + FID_TYPE-from-ENTITY_TYPE; client Form 501
  option (tsc 0). Render on the DOR web-version fillable (GA-700 recipe; shared-name 6a/6b
  checkboxes → abs_pos literals; visually verified). 8 D_GA501_* incl. the NR RED-defer;
  HB 1199-corrected conformity text (RS spec drift flagged → docs/rs_handoff/
  2026-07-08_ga501_spec_drift.md: amend diagnostic text + extend line_map 9c/11c-20).
  DB 5/5 (50s); flow gate 422. Drive-by: GA-700 refresh-from-federal fall-through fixed.

**Session 30 continuation wrap (2026-07-08; "go"):**
- [x] `[APP]` **Statement-source revision (leg-6 amendment)** — 2026-07-08 `bebd7db`. Line 19 +
  M-2 3a/5a statements decompose their COMPUTE FORMULAS (D_* components + K items; LineItemDetail
  = the M-2 override tier); SUBSCHEDULE_CONFIG drops the computed lines; Sch B "other/Hybrid"
  method (`accounting_method_other_desc`, mig 0177 prod-applied); `MEF_SOFTWARE_ID_1120`. Suite 45.
- [x] `[APP]` **1120-S Scenario-5 leg 7 — engine-driven scenario build** — 2026-07-08 `9754388`.
  `apps/efile/ats/scenario5_1120s.py` + `mef_build_ats_1120s_s5` (rollback-txn): ALL key lines tie
  TO THE DOLLAR; full submission XSD-valid (2025v6.2) w/ statements + Hybrid method; 4 DB tests.
  Truck §179 disposition DELIBERATELY absent (refuse-seam test proves it); preparer overrides
  carry the key's book-vs-engine gaps; engine-truth divergences documented in the module.
  ⚠ NEW Ken flag: K-1 whole-dollar rounding drifts K-1 sums vs Sch K (REVIEW_QUEUE).
- [x] `[APP]` **4797 DB verify discriminated** — the stalling test GREEN alone (327s) → pooler-load
  class, refactor exonerated; full-file run carried to a healthy window.

**Session 30 wrap (2026-07-08; "go" — autonomous continuation):**
- [x] `[APP]` **1120-S Scenario-5 leg 6 — Itemized*Schedule attachment statement mappers** — 2026-07-08
  `ca7e956`. Eleven LineItemDetail-backed statement documents (page-1 5/19 · 1125-A A5 · Sch L
  6/9/14/18 two-column · M-1 2/6 w/ synthesized 6a Depreciation row · M-2 3a/5a); each
  reconciles-or-refuses vs the emitted parent line; refDocId anchors incl. 1125-A OtherCostsAmt;
  ReturnData order per the XSD sequence (ItemizedOtherDeductionSch2 rides LAST, ref 3706).
  SUBSCHEDULE_CONFIG rollup extended (5/19/A5/M2_3a/M2_5a; client UI = deferred input leg).
  7 new pure tests incl. live-XSD full-set validation — mapper suite 41.
- [x] `[APP]` **§179-disposition pass-through gap FOUND + refuse-seamed** — same commit. i4797:
  pass-through entities don't report §179-property disposals on their own 4797 (K-1 17K +
  statement + M-2 3a instead); engine currently mis-routes them through corp 4797; RS 4797 spec
  SILENT → mapper refuses, print unchanged, full unit queued for Ken (tax-app REVIEW_QUEUE).
- [x] `[APP]` **Leg-7 fact sheet** — agent-extracted scenario5_1120s_analysis.md (all forms,
  line values, checkbox resolutions, [MISMATCH] ledger — SSN pairing reversed, 156,855 = line-11
  rents, stale TY2023 §179 threshold on the scenario 4562 face).

**Session 29 wrap (2026-07-08; "go" — autonomous continuation of the Ken-picked S5-mapper lane):**
- [x] `[APP]` **1120-S Scenario-5 legs 4+5 — IRS4797 + IRS8825 doc mappers** — 2026-07-08 `f5724fa`.
  4797: routing extracted into shared `apps/returns/dispositions_4797.classify_disposal`
  (aggregate_dispositions refactored onto it, behavior identical); Part I/II rows + Part III
  columns; S-corp face rules (skip 8/9/11/12); **reconcile-or-refuse** vs the flowed FFVs
  (L17↔page-1 L4 · L7↔K9 · unrecap↔K8c); links L4/K9. 8825 (v6.2 = Dec-2025 face): parallel
  per-property groups; interest→one line 8; mgmt+supplies+other→L17 so L18 ties exactly; L23
  reconciled to flowed K2; per-property L14 → its rental's IRS4562 doc id; K2 links IRS8825;
  royalties refuse seam. 9 new pure + 2 live-XSD tests (suite 34); flow gate 422. ⚠ DEFERRAL:
  **render_4797 print diverges from compute** (pre-existing — group-label §1250 + 26a=0 vs
  resolve_recapture_type + resolved addl depr) — print fix = its own render leg.
- [x] `[APP]` **1120-S Scenario-5 leg 3 — per-activity IRS4562 doc mapper** — 2026-07-08 `ac927b5`.
  One IRS4562 per activity (page1/8825-per-rental/farm = the aggregate_depreciation flow split);
  Part I §179 return-level on doc 1 (OBBBA $2.5M/$4M, never the PDF's stale 2023 figures);
  per-activity line 22 excludes §179 → ties exactly to page-1 L14 / 8825 flow. **Bridge-gate:**
  asset classification extracted into shared `apps/tts_forms/f4562_derivation.py`; render_4562
  refactored onto it, print output unchanged. referenceDocumentId links: page-1 DepreciationAmt →
  trade 4562, Sch K L11 → Part-I doc (`plan_4562_documents` exposes rental doc ids for the 8825
  leg). Refuse seams: unmapped line-19 life (print silently skips — XML refuses), §179 on ≤50%
  listed. 8 new pure tests + live-XSD two-doc validation; flow gate 422; render 21; MeF pure 75.
  DEFERRAL_AUDIT ×3 (24a/24b evidence input · entity line-11 §179 income limit · print-side 4562
  gaps: aggregate-not-per-activity, line-22 §179 inclusion, missing 7/29/43).

**Session 28-continuation-2 wrap (2026-07-08; "go" — Ken delegated the 3903 + disbursement-code rulings):**
- [x] `[APP+EXT]` **🏁 1040 MeF ATS COMPLETE — ALL SEVEN SCENARIOS ACCEPTED** — 2026-07-08
  `6c24813` + tag `mef-1040-ats-accepted`. Rounds 5-6-7 live loop in one sitting: r5 accepted
  all five content fixes; r6 = S4/S8/S12 ACCEPTED; r7 = S2/S3/S5/S13 ACCEPTED. Round-5 acks: ALL five round-4 content errors CLEARED ×7; sole reject =
  IND-195-01 regression (`MEF_DEVICE_IP` lived only in the r3/4 shell) → persisted in
  `server/.env` + build-command guard. Round-6 acks: THREE ACCEPTED; four rejects fixed same
  sitting (S2 decedent-header family IND-018/019/424/425/426 + IND-089 exemption counts · S3
  referenceDocumentId links — "attached to" = LINKED · S5 counts · S13 spouse signature trio)
  → round-7 bundle (seqs 31-34, `--only` flag). 51 pure tests green.
- [x] `[APP]` **ATS round 5: all five round-4 content fixes + the seven-scenario bundle** —
  2026-07-08 `ed5c823`. R0000-248/249 `AdditionalFilerInformation`
  always-on (⚠ RefundDisbursementCd enum UNPUBLISHED — "0"=paper-check reasoned; e-help Q#2) ·
  IND-114 `CapitalDistributionInd` · IND-434 S8 synthetic MFS spouse (Pub 1436 "00" test range) ·
  F1040-003/111 dependent counts · S1-F1040-120-01 `ClaimStorageFeesInd` (Ken-delegated: scenario
  form list has no 3903). NEW `mef_build_ats_round5` bundles all 7 (seqs 11-17; spouse PIN
  S13-only). 3 new pure tests; suites green; fixes grep-verified in the built XML.
  Deferrals logged: ClaimStorageFeesInd app input; direct-deposit RTN/DAN in the header group.

**Session 28-continuation wrap (2026-07-07 evening→midnight; Ken-directed "my priority is MeF testing"):**
- [x] `[APP+EXT]` **FIRST LIVE ATS SUBMISSIONS — 4 upload/ack rounds in one evening** (Ken uploaded,
  CC fixed): the full stack is PROVEN into per-return content validation (MeF computed S5's refund
  = the engine's $7,640 to the dollar). Gateway-only defects found+fixed: **MIME multipart file**
  `4a067f6` (Pub 5446; bare ZIP = "Invalid mime format") → **envelope root namespaces** `30ea3b4`
  (X0000-008's REAL trigger — placeholder ack identity = transmission-level tell; Return-root fix
  `676f3ca` was necessary-not-sufficient) → **FilingSecurityInformation telemetry** `2c36799`
  (IND-189…203; XSD-optional/BR-required; settings MEF_DEVICE_ID/MEF_DEVICE_IP). Loop log:
  `docs/mef/ats_receipts.md`. Round-5 queue: R0000-248/249 header group · IND-114 CapDistribInd ·
  IND-434 S8 spouse data · F1040-003/111 dependent counts · ⚠ Ken-ruling S1-F1040-120-01
  (moving expense w/o 3903 → ClaimStorageFeesInd?).
- [x] `[APP]` **1120-S ATS Scenario-5 doc mappers legs 1-2**: 1125-A `e7bebb9` + 1125-E `35e3b59`
  (16 tests incl. live XSD). NEXT legs: 4562(×2 activities — needs a per-activity derivation
  bridge-gated to the print renderer) → 4797 → 8825 → Itemized*Schedule statements.
- [x] `[APP]` **S2/S3 whole-dollar re-pins** `53a229b` (first run since the s27 sweep; 8995-A cents
  boundary superseded; hand-verified + execution-proven).

**Session 28 wrap (2026-07-07 evening; Ken-directed "go back to MeF — 1040 business rules + v5.3"):**
- [x] `[APP]` **MeF SOR want-list intake** — 2026-07-07 `199cfff`. 5 SOR zips hashed+extracted+recorded
  (`schema_versions.md`). Received: 1040 v5.3 schemas+**BR (first 1040 BR in hand)**, 1120x v6.2 sch+BR,
  1065 v5.3 sch+full BR. ⚠ 1041 came **v3.0 not v5.3** (re-request); 1040 v5.4 BR missing.
- [x] `[APP]` **Both mappers re-stamped to ATS-ACTIVE** — same commit. 1040 v5.4→**v5.3** (flip back
  8/9/2026); 1120-S v6.3→**v6.2** (v6.3 = Jan-2027 prod stamp). Deltas not emitted; 59+10 tests green.
- [x] `[APP]` **`business_rules.py` loader** — same commit. Reject-number → text/category/severity;
  CSV (1040 v5.3) + XLSX (1120x v6.2) variants. 5 tests.
- [x] `[APP]` **Engine fix: 2441 line-8 ratio** flattened to "0" by the s27 whole-dollar sweep — same
  commit; caught by IRS2441.xsd during the v5.3 revalidation. Scenario 5+8 rebuilt+revalidated (42 DB green).
- [x] `[APP]` **Engine fix: whole-dollar zero-clears** — 2026-07-07 `117cb68`. s27 sweep left literal
  "0.00" clears (K2/K7/K8a/disposition lines) + a K2 cents quantize; the s27-updated pins caught them.
  Flow gate 422; pin suite fully green; dispositions/4797 (39) green.
- [x] `[APP]` **Session-27 residual pin suite** — DONE this session (was 0b in STATUS). All green.
  ⚠ broad state/2210/retirement sweep NOT fully re-run (pooler degraded); blast-radius covered only.

**Session 27 wrap:** regression bugs ALL FIXED + whole-dollar rounding landed (`0afdcb4`) — comparator
0 diffs / 59 lines, flow gate 422. (Residual pin suite → closed session 28 above.)

**Session 26 (Ken-directed detour; ADDED to the plan — was not on the Spine):**
- [x] `[APP]` **Lacerte regression harness v1** — 2026-07-07 `39e7258` (scripts/lacerte_regression/: parse → load →
  compare; procedure in its README). First real 2025 return recreated in-app and diffed: **30 lines match
  Lacerte exactly, 29 differ — ALL traced to two engine bugs.** Details: tax-app `STATUS.md` (bugs) +
  `STATUS_ARCHIVE.md` 2026-07-07 (full session detail). Return identity lives in `D:\tax-test-data\` only.
- [x] `[APP]` **Sch E line 22 — released prior-year passive loss** — 2026-07-07 `0afdcb4` (both 8582
  branches; specs verified, conforming fix; line 21 face corrected).
- [x] `[APP]` **Form 8960 line 4a rental NII** — 2026-07-07 `0afdcb4` (auto-feed Sch 1 L5 + auto 4b
  back-out; RS spec amendment handoff filed). BONUS fixes same commit: 1040 line-24 stale values["23"]
  ($10 short) + **whole-dollar rounding engine-wide (Ken-ruled — no cents anywhere)**.
  **Comparator re-run: 0 diffs / 59 lines; flow gate 422.**
- [ ] `[APP]` **Session-28 FIRST ACTION: run the 25 updated-pin tests + broad entity/state batches**
  (2-3 files per batch — a 5-file batch ran 3+ hrs on the pooler and was killed; see tax-app STATUS).

**Prior NOW item — MeF entity e-file (∥): 1120-S mapper leg 1 DONE — next = ATS Scenario-5 doc mappers.** 2026-07-07
(twenty-fifth session; ADDED to the plan — was not on the Spine; Ken directed "start S-corp MeF testing"):
**July SOR release filed** (`9b77c7e` — BMF 1041 v5.5 / 1065 v5.4 / 1120x v6.3 extracted+hashed, IMF v5.5/2026v2.0
recorded; `docs/mef/schema_versions.md`). **1120-S MeF mapper leg 1 (`c2edd0d`):** schema_locator corp-tree support;
`read_model_1120s` + `builder_1120s` 1:1 vs TY2025v6.3 XSDs (v6.3 = the Jan-2027 PRODUCTION version — right target;
ATS window opens fall 2026); synthetic S corp XSD-validates GREEN; 10 tests + 59 regress. ⚠ live-page check: ATS-active
TODAY = 1040 v5.3 / 1120x v6.2 / 1065 v5.3 / 1041 v5.3 (the in-hand 1041 v5.5 is "Not valid for ATS") — **SOR
want-list email SENT 2026-07-07** (all four families). **NEXT MeF legs:** Scenario-5 doc mappers (1125-A, 1125-E,
4562, 4797, 8825 — engine-computed already, serialization-only) + Itemized*Schedule attachment docs + engine-driven
scenario build (1040-Scenario-8 playbook) + DB leg on a real return + v6.2 re-stamp when SOR lands. Data-model gaps:
signing officer (BusinessOfficerGrp), B4a/B4b tables, K10/K13g codes. Scenario PDFs → `docs/mef/scenarios/`.

**Prior spine item — S-11 1041 fiduciary module, APP BUILD (legs 1/2/3/4/5 DONE), REMAINS legs 6-8** — 2026-07-06
(twenty-fourth session, Ken-directed "start 1041" + "continue"): the federal 1041 now computes + issues K-1s +
diagnoses + renders. **Leg 5 f1041 render (`72b38bb`):** downloaded f1041.pdf + f1041sk1.pdf from irs.gov;
`field_maps/f1041_2025.py` (73 AcroForm fields, position-correlated) — page-1 L1-24 / Sch B B1-B15 / Sch G Part I
G1a-G9 / entity-type checkboxes / header; registered `f1041` in ACROFORM_FORM_IDS + form_code_to_id; visually
probe-verified. **REMAINS: leg 6 GA 501 · 7 frontend verify · 8 flow gate + the per-beneficiary K-1 PDF.** Below,
greenfield estates & trusts entity type.
**Legs 1-2 (`539b204`):** `seed_1041` (`1041` FormDefinition; `ENTITY_FORM_MAP` `trust→1041`; EntityType.TRUST +
React frontend already existed) + dedicated `compute_1041.py` (`compute_1041_db` early-return in `compute_return`,
NOT the FORMULA_REGISTRY) — DNI/IDD §643(a) / §662 tiers / §642(b) exemptions / §1(e) rate sched + 0/15/20
cap-gain wksht / ESBT 37% / NIIT / total tax; year-guarded TY2025; 13 pure + 1 DB. **Leg 4 diagnostics
(`d1117d8`):** 11 `D_1041_*` (AMT/bankruptcy RED-defer per D-2); 6 DB. **Leg 3 Schedule K-1 (1041) issuance
(`99a8943`):** `Beneficiary` + `BeneficiaryK1Computed` models (migs 0175 create / 0176 RLS, prod + test DB);
`k1_allocator_1041` (character-retained `box[c]=ent×dni_pct/100`, grantor→no K-1, persist wired into
`compute_return`); `k1_sources` seed section (→ 9 sec / 83 lines); 6 `D_K1041_*`; 6 pure + 3 DB. Flow gate 422.
**REMAINS: leg 5 f1041 render (⚠ IRS PDF not downloaded) · 6 GA 501 · 7 frontend verify · 8 flow-assertion
gate.** Prior (twenty-third session): ✅ **S-6 PAL/basis deepening
app build COMPLETE, all 5 R-items** (R1 self-rental `c4cd928`; R2-R5 boundary diagnostics + Form 461 `07fb29f`)
— extends the existing 8582/Sch E engine; R1 = the only real compute change (type-7 net income→non-passive),
R2-R5 diagnostic-only; migs 0172/0173/0174; 27 tests; flow 422. ✅ **S-5 leg 2 — entity-boundary input UI**
(`d74b016`) → **S-5 COMPLETE end-to-end** (new "Boundary" tab + `entity-boundary` GET/PATCH API + 4 tests).
Prior (twenty-second session): ✅ **S-5 entity-boundary diagnostics leg 1** (`50f2874`) — 6 `D_EB_*` REDs for
1065/1120-S + `EntityBoundaryAssertion` model (migs 0170/0171 prod), superseded `D_L_M3` deactivated. ✅ **S-4
f1065 render recalibration COMPLETE — the 1065 form fully ticks; S-4 done end-to-end** (`2599621`): `f1065`
coordinate overlay → AcroForm-fill backend (whole 6-page form, `field_maps/f1065_2025.py`, in
`ACROFORM_FORM_IDS`), M-1 line-4/7 totals also done (`c0dbff8`) → no known display gaps; 3 DB tests; flow
gate 422; visually verified. Prior
(twenty-first session): ✅ **S-10 GA-700 (Georgia partnership + PTET) COMPLETE — all 4
legs**. ✅ **leg 3 render (`0d59255`):** `render_ga700_overlay` fills the DOR
"web version" GA-700 fillable PDF via the AcroForm text-overlay pipeline (`_acroform_fill`) — the DOR PDF ships
SEMANTIC field names (S1L1..S8L12, FEI_NUMBER, ...) that map ~1:1 to the compute keys, so the template is stored
UNMODIFIED and `field_maps/ga700_2025.py` points straight at them (no AcroForm-Creator injection, unlike GA-600S).
The "viewer-only" blocker was a false alarm — the DOR "Print Blank Forms" static PDF is a normal AcroForm. Ratio→
6-dec text (+ copy to Sch2 L4); Total Tax→Sch3 L1; PTET→CBX_ELECT_TO_PAY; S8_6→form line 7. 3 DB tests; visually
verified all 4 schedule pages; flow gate 422. ✅ **legs 1/2/4 (twentieth session):** compute `FORMULAS_GA700`
(`1d7f102`) · input `seed_ga700` + federal pull (`6c26d72`) · diagnostics `rules_ga700.py` (`f70a9d4`). ✅ **GA
§179 CONFLICT RESOLVED (`b975300` tts / `09d55d9` RS):** Ken-ruled GA CONFORMS to OBBBA §179 = **$2.5M/$4M** for
TY2025 (HB 1199 retroactive to TY≥Jan-1-2025, supersedes the Aug-2025 Form 4562's $1.25M + the stale 2021 $1.05M);
swept depreciation_engine + compute + rules + RS load_ga700/load_ga600 + CLAUDE.md/DECISIONS.md; SC/NC untouched
(their own correct rules). ⚠ RS reseed+export still needed to refresh the cached spec mirrors. (Prior: ✅ **S-4
1065 core COMPLETE all legs 1a–6**; ✅ **S-9 NC D-400**; SC1040/AL40/NC D-400 done.)
## ▶ NEXT — the S6 build ✅ s37 (`f824dc0`) — now: **the 1120-S ATS completion lane** (mission rule: full scenario set, THEN test) — S7/S8 declared-forms ruling (Ken/e-help; recommendation: declare only the forms the firm files — 5471/8975/8858 out → S7/S8 likely NOT required) → any required builds → full-set bundle → live upload loop (8941 key-inversion resolves in-loop). Then the mission ladder: **1065 ATS lane** (scenario/doc-mapper legs — the S5/S6 playbook) → **1041 ATS** (SOR v5.3 schemas+BR still block) → **S-13/S-14 1120 + state C-corp APP build** (RS specs DONE `9a41581`/`87b66a4`; scope ADDED by the 2026-07-09 mission — the old "not season-one" mark is SUPERSEDED) → **Form 709 module** (greenfield: RS spec authoring first — no spec exists; MeF-e-fileable since 7/14/2025; needs its own SOR schema request for the 709 family). Interleave: the REVIEW_QUEUE set (1065/1041 allocator residual-offset units · RS FA-export reconciliation pass · the s36 entity-8824 ratifications). *(The 8941 unit ✅ s35 + the entity-8824 unit ✅ s36 + the S6 scenario build ✅ s37 — wraps under NOW; the S6 lane is closed.)* Also teed up: **S-3 brokerage front end** (∥) · small follow-ups (S-6 Form-461 Sch-1 add-back/NOL; 1065 compute-vs-spec M-1 4c/7b nuance; GA-700
display-subtotal compute leg + Sch-8 spec line-numbering). ⚠ SEASON_PLAN's month-by-month runway needs a re-cut for 1120/709 (dedicated planning session with Ken).
**▶ RS authoring NOW: S-15 NC + AL pass-through entity batch (WO-13)** — the S6 trio (8941 ·
8824 entity · SCHD_1120S renumber) DONE 2026-07-08 s34-continuation, RS `b4c71b8`. After
S-15, net-new RS scope depends on the TaxWise forms-usage report or a law change.

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
- [x] 🏁 ALL SEVEN scenarios ATS **ACCEPTED** — 2026-07-08 `6c24813` tag `mef-1040-ats-accepted`
  (rounds 5-7 live loop; S-1 CLOSED).

**S-2 · [APP]✅ · Proforma/rollover PRODUCER** — DONE 2026-07-04 `3a55f31`. App-to-app
snapshotter; fires on 1040 DRAFT→FILED; owns the year-shift (Sch D carryover, 4562 L13, 8606
L14, Roth basis, aggregate PAL). v1 follow-ups (non-blocking): per-activity PAL roll (v1 is
aggregate); `_sr_py_prior_refunded`/`_sr_py_unused_credits` sourcing; TaxWise 1040 importer (Oct).

**S-3 · [APP]⏳ ∥ · Brokerage extraction-to-production front end** — skeleton DONE (7/4,
`import_brokerage_summary` → 8949 Exception-2, mig 0160). REMAINS: OCR/parse front end +
YELLOW render + preparer-confirm UI. Not spec-gated. Parallel.

**S-4 · [RS]✅→[APP]✅ CORE · 1065 core (WO-02) — RS authoring DONE; APP CORE build COMPLETE (2026-07-05, legs 1a–6). Render recalibration = a deferred follow-on leg.**
▸ 6 specs cached (`a4f3370`). ⚠ found the app's 1065 page-1 + Sch K were on PRE-2025 line numbering (verified
vs f1065.pdf). **✅ leg 1a — page-1 2025 renumber (`10f1fd2`):** ord business income now line 23 (was 22),
K1←line 23, NEW line 20 §179D, seed→286 lines, 15 DB + flow 398 + SE 36 green. **✅ leg 1b — Schedule K 2025
renumber + Analysis line (2026-07-05, eighteenth session):** K16a→line 21, line 16→K-3 checkbox, K13d→K13b,
+K13c/K13e, computed `K_ANALYSIS`, seed→290 lines; 5 new diagnostics (`rules_1065_schk.py`); k1_allocator
renames; ★ fixed `aggregate_schedule_d` zeroing 1065 royalties (K7); 7 pure + 9 DB, flow 398 (`8ad96d8`).
**✅ leg 2 — Schedule L balance check (2026-07-05, eighteenth session):** new `rules_1065_l.py` (D_L_BALANCE_BOY/EOY
error, D_L_21_M2_TIE + D_L_M3 warning, D_L_EXEMPT info; B6 exemption suppresses balance errors); the L14/L22
total compute already existed → validation-only; 7 DB tests (`f55a0c8`). Build-gap #2 closed.
**✅ leg 3 — M-1/M-2 tie-outs (2026-07-05, eighteenth session):** new `rules_1065_m.py` (D_M1_ANALYSIS error,
D_M2_3 warning, D_M1_EXEMPT/D_M2_EXEMPT info) wiring K_ANALYSIS=M1_9=M2_3 (RECON-ANALYSIS, unblocked by leg 1b);
sum compute already existed → validation-only; 3 pure + 4 DB (`01a9a95`); D_M2_1 deferred to the K-1 leg.
**✅ leg 4 — K-1 alloc reconcile (2026-07-05, eighteenth session):** promoted `k_to_box`→`K_TO_BOX` + wired
K13c/K13e; new `rules_1065_k1.py` (D_K1_RECON error, D_K1_9C/D_K1_CAPPCT info, D_K1_SPECIAL_ALLOC/D_K1_ITEML
warning); 3 pure + 7 DB (`b911498`); D_K1_704C/D_K1_706D deferred (Partner item-M/mid-year fields + migration).
**✅ leg 5 — issuer-side K-1 persistence + 1065→1040 import (2026-07-05, nineteenth session, `6b51c3e`):** NEW
`PartnerK1Computed` model (migs 0168 create + 0169 RLS, **applied to prod**); issuer PERSIST
(`persist_all_partner_k1s_db` hooked post-SE in compute.py; compute pre-existed via `allocate_all_k1s`); import
side in `k1_import.py` (box 14a→se_earnings; merged `available_k1_offers` w/ `owner_kind`/`owner_id` dispatch;
`ScheduleK1.imported_from_partner`; `K1ImportDismissal` shareholder-nullable + partner FK); 2 pure + 6 DB green.
**✅ leg 6 — 1065 flow-assertion gate (2026-07-05, nineteenth session, `f1c095b`):** the first for the
partnership entity. RS export → `flow_assertions_1065.json` (22 active) + `_pending.json` (6 staged:
ENTITY_BOUNDARY×3 / 8990 / §704(c)/§706(d) / item-L capital); pure runners (`_run_1065_assertion`) exercise the
built FORMULAS_1065 / k1_allocator / compute_1065_se / diagnostics; new `gating_check` type. Full gate 423
passed. **S-4 CORE COMPLETE.** ✅ f1065 render recalibration DONE 2026-07-06 (`2599621`) → **the 1065 form
now fully ticks; S-4 complete end-to-end.** Still deferred (separate future legs): the 6 staged assertions +
D_M2_1/D_K1_704C/D_K1_706D (Partner-field migration). (M-1 line-4/line-7 total boxes DONE `c0dbff8`.)
All 6 core specs authored+seeded+exported (200): Schedule K spine (`1065_PAGE1`+`SCH_K_1065`),
K-1+alloc, M-1/M-2, L/B; 8825/4562/3800 confirmed cover 1065; 7-form batch in `approved_specs.py`.
Reconciled to live RS STATUS 2026-07-05 (corrected the stale "untouched beyond SE" mark). Unblocks
1065 ATS, issuer-side 1065 K-1 → 1040 import, AND the GA-700 app build (see S-10 dependency).
- [x] Schedule K — 2026-07-04   - [x] Schedule K-1 — 2026-07-04   - [x] M-1/M-2 — 2026-07-04   - [x] L/B — 2026-07-04   - [x] 8825 (covers 1065)
- [x] issuer-side 1065 K-1 persistence → 1040 import (parity w/ 1120-S) — 2026-07-05 `6b51c3e`  `[APP]`
- [x] 1065 flow-assertion gate (leg 6) — 2026-07-05 `f1c095b` (22 active + 6 staged) → **S-4 CORE COMPLETE**  `[APP]`
- [x] tts compute build/reconcile of the formulas against the specs — the tie chain (legs 1a-5) is gated by leg 6
- [x] f1065 render recalibration (WHOLE form → AcroForm backend, 2025 numbering) — 2026-07-06 `2599621` `[APP]`
  → the deferred follow-on. Root cause was bigger than "page-1 + Sch K coords": `f1065` was on the
  never-calibrated legacy coordinate overlay while the IRS PDF is a 6-page AcroForm, so the fix converted
  the whole form (`field_maps/f1065_2025.py`, 191 maps, registered in `ACROFORM_FORM_IDS`). 3 DB tests;
  flow gate 422 unchanged. M-1 line-4/line-7 total boxes DONE same session (`c0dbff8`) → no known display gaps.

**S-5 · [RS]✅→[APP]✅ COMPLETE ∥ · Boundary diagnostics (WO-04) — RS DONE 2026-07-05; APP leg 1 DONE
2026-07-06 `50f2874`; APP leg 2 DONE 2026-07-06 `d74b016`.** New consolidated `ENTITY_BOUNDARY` form: M-3
threshold, K-2/K-3 DFE gate, §704(c), §754/§743(d)/§734(d), multistate apportionment (P.L. 86-272).
- [x] M-3 · [x] K-2/K-3 DFE gate · [x] §704(c) · [x] §754 · [x] apportionment — 2026-07-05 (RS)
- [x] tts app build leg 1: 6 `D_EB_*` diagnostics wired into 1065/1120-S (`rules_entity_boundary.py`) +
  `EntityBoundaryAssertion` model (migs 0170/0171 prod) + `seed_rules` reseed (superseded `D_L_M3` deactivated)
  — 2026-07-06 `50f2874`. M-3 + K-2/K-3 foreign gate AUTO-DERIVE from data; rest read preparer-assertion flags
  (default non-firing). 13 tests. `[APP]`
- [x] leg 2: React input UI for the `EntityBoundaryAssertion` flags — 2026-07-06 `d74b016`. New "Boundary" tab
  on the 1065 + 1120-S editors (`EntityBoundarySection.tsx`) + `entity-boundary` GET/PATCH API action +
  `EntityBoundaryAssertionSerializer` + 4 endpoint tests. The preparer-flag arms now fire from the UI (no
  longer admin-only). Flow gate 422; client tsc 0 err / vitest 275. **S-5 COMPLETE.** `[APP]`

**S-6 · [RS]✅→[APP]✅ COMPLETE · PAL/basis deepening (WO-03) — RS authoring DONE 2026-07-05; APP build DONE
2026-07-06 (R1 `c4cd928`, R2-R5 `07fb29f`).** Amendments to FORM_8582/SCHEDULE_E + new Form 461. Extends the
existing 8582 per-activity engine. Ken ruled R3 diagnostic-only + R5 preparer-entered inputs (2026-07-06).
- [x] R1 self-rental + R2 PTP (one 8582 unit) — 2026-07-05   - [x] R3 REP checkbox + §1.469-9(g) election — 2026-07-05   - [x] R4 at-risk diag — 2026-07-05   - [x] R5 §461(l) Form 461 diag ($313k/$626k) — 2026-07-05
- [x] tts app build — 2026-07-06 `c4cd928`/`07fb29f`. **R1 self-rental = the only real compute change** (type-7
  net income → non-passive, excluded from the 8582 passive buckets; mig 0172; `D_SCHE_SELFRENTAL`). R2 PTP
  (`D_8582_PTP` auto-derived from `is_ptp`) · R3 REP (`D_8582_RE_PRO` RED→info + `_REP_MATLPART`/`_REP_TESTS`) ·
  R4 at-risk (`D_8582_ATRISK`) · R5 §461(l) (`compute_461` + `rules_461` D_461_*) — all diagnostic-only, migs
  0173/0174. 27 tests; flow 422.  `[APP]`
- [x] **follow-ups DONE 2026-07-06** (`296b9f3`; RS `d5b76b2`): §461(l) EBL now **adds back to Schedule 1 line
  8p** (verified vs i461 2025) so returns compute correctly + **RS reconciliation** (FORM_461/FORM_8582/SCHEDULE_E
  promoted draft→active, RE_PRO message de-staled, cached mirrors refreshed). ⚠ still deferred: §172 NOL carryover
  to next year, R3 REP compute bypass, R4 6198 at-risk compute (all diagnostic-only, see DEFERRAL_AUDIT).  `[APP]`

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
- [x] S-10 GA-700 + PTET spec — 2026-07-05   → **app build COMPLETE 2026-07-06 (all 4 legs)**: compute
      (`1d7f102` `FORMULAS_GA700` PTET 5.19% + single gross-receipts apportionment) · input (`6c26d72` `seed_ga700`
      + 1065→GA-700 federal pull + frontend) · **render (`0d59255` `render_ga700_overlay` — AcroForm text-overlay
      on the DOR fillable PDF whose semantic field names map ~1:1 to compute keys; template stored unmodified)** ·
      diagnostics (`f70a9d4` 10 D_GA700_*). 24 tests green, prod-seeded; flow gate 422. ⚠⚠ GA §179 conflict still
      open for Ken (GA700 $1.05M/$2.62M vs GA600 $1.25M/$3.13M). v1 = entity-level; partner NRW/allocation +
      display-subtotal compute leg + Sch-8 spec line-numbering deferred (non-blocking follow-ups).

**S-11 · [RS]✅→[APP]⬜ · 1041 module (WO-09), greenfield RS-first. RS authoring COMPLETE 2026-07-05
(front door + Gate-1 scope walk = RS DECISIONS D-10; f1041_source_brief.md). APP build remains.**
- [x] spine (entity types/rates/exemptions)   - [x] DNI/IDD/Sch B   - [x] Sch G — 2026-07-05 *(all three = the one
  consolidated `1041` form — `load_1041_spine.py`; core 4 + ESBT computed; grantor/PIF/bankruptcy = structure/route/defer)*
- [x] K-1 (1041) `SCHEDULE_K1_1041` — 2026-07-05 *(issuer-side; full verbatim box codes)*   - [x] GA 501 `GA501` —
  2026-07-05 *(resident-only)*   - [x] Sch I RED-defer diag (D-2)
- All 3 forms seeded/exported (`lookup/{1041,SCHEDULE_K1_1041,GA501}/export/` = 200); RS prod 99 TaxForms / 471 FA / 859 rules.
- [~] tts app build STARTED 2026-07-06 (Ken-directed): **legs 1/2/3/4 DONE** (`539b204` seed+compute, `d1117d8`
  diagnostics, `99a8943` K-1). Leg 1 `seed_1041` + `ENTITY_FORM_MAP` `trust→1041`. Leg 2 `compute_1041.py`
  dedicated `compute_1041_db` (DNI/IDD §643(a) / §662 tiers / §642(b) exemptions / §1(e) rate sched + 0/15/20
  cap-gain wksht / ESBT 37% / NIIT / total tax); 13 pure + 1 DB. Leg 4 `rules_1041.py` 11 `D_1041_*`
  (AMT/bankruptcy RED-defer per D-2); 6 DB. **Leg 3** `Beneficiary` + `BeneficiaryK1Computed` (migs 0175/0176,
  prod+test) + `k1_allocator_1041` (character-retained K-1, grantor→no K-1, wired into compute_return) +
  `k1_sources` seed section + 6 `D_K1041_*`; 6 pure + 3 DB. **Leg 5** f1041 AcroForm render (`72b38bb`):
  downloaded f1041.pdf + f1041sk1.pdf; `field_maps/f1041_2025.py` (73 fields); registered in ACROFORM_FORM_IDS +
  form_code_to_id; entity-type checkbox from ENTITY_TYPE; visually probe-verified; render test. Flow 422.
  **Leg 6 GA-501 DONE 2026-07-08 `370fdb0`** (all four legs one unit — compute/input/render/diagnostics,
  RS pins to the dollar, DOR fillable render, 8 D_GA501_*, prod-seeded; RS handoff: HB 1199 text + line_map
  9c/11c-20 extension). **Legs 7 + 8b DONE 2026-07-08 `7c62cc0`** (beneficiary UI/API + FIDUCIARY_TABS +
  f1041sk1 K-1 PDF + live frontend verify). **Leg 8a DONE 2026-07-08 `06c8946` + RS `adc4710`** (the FA
  family consolidated/activated in RS — 17 active + 2 staged; tts gate 422 → 440). 🏁 **S-11 COMPLETE —
  tag `1041-complete`. Remaining = MeF-lane only: the 4 ATS scenarios (1041 v5.3 schemas owed by SOR).**  `[APP]`
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

**S-15 · [RS]✅→[APP]⬜ · NC + AL pass-through entity batch (WO-13), greenfield RS-first. RS authoring COMPLETE
2026-07-05 `b501fc2`. Gate-1 D-15.** Completes the adjacent-state pass-through track (GA-700 + SC1065/SC1120S +
NC + AL). 4 forms seeded/exported (`lookup/{NC_D403,NC_CD401S,AL_FORM_65,AL_FORM_20S}/export/` = 200); prod 106→110.
- [x] **NC D-403** + **NC CD-401S** + **NC Taxed PTE** — 4.25% (individual rate), owner-side **DEDUCTION** (NC-PE);
  CD-401S computes NC franchise (net worth); NR withholding 4.25%; 85% bonus/§179 $25k/$200k add-back.
- [x] **AL Form 65** + **AL Form 20S** + **AL Electing PTE** (Act 2021-1) — 5% (Form EPT), owner-side **refundable
  CREDIT** (Sch EPT-C); composite PTE-C 5%; AL conforms §168(k)/§179; Form 20S L32 LIFO/BIG/excess-passive.
- ⚠ [UNVERIFIED] exact NC/AL line numbers (2025 instruction PDFs didn't text-extract) — re-pull before the app build.
- [ ] tts app build: NC/AL pass-through compute + PTET (owner deduction/credit downstream to D-400/AL-40)  `[APP]`

**▶ RS AUTHORING — original spine COMPLETE (2026-07-05).** All planned rocks DONE (S-1..S-11 + WO-10 5227 +
S-13 1120 + S-14 state C-corp + S-15 NC/AL pass-through). The [APP] lane (tts builds, on hold) + Shelf items
(Ken's external actions) are the rest of that board.

**S-16 · [RS] · Federal-forms gap-fill queue (WO-14+) — Ken's 2026-07-05 pick from the federal not-built list.**
A new RS authoring queue of federal forms a tax practice needs that weren't in the 92-form prod set. **AUTHOR IN
THIS EXACT ORDER** (each = its own front-door order: gap-check → research-verify verbatim → source brief → Gate-1
scope walk → author `READY_TO_SEED=False` → SQLite-validate → Ken review → seed → export). At each boot, take the
TOP unchecked item as the current RS rock.
- [x] **WO-14 · Form 8990** (§163(j) business-interest limitation) — ✅ DONE 2026-07-05 `376f644` (RS). Finished the
  1120 module's deferred leg: Part I ATI on the OBBBA EBITDA basis (L11 dep/amort/depletion add-back) → 30% limit →
  allowable + indefinite carryforward; Part II/III EBIE/ETI; $31M §448(c) exemption. entity_types 1120/1065/1120S/1040;
  prod 110→111; `lookup/8990/export/` = 200; validated 19/0. tts app build = [APP] lane.
- [x] **WO-15 · Schedule H** — Household Employment Taxes (1040) — ✅ DONE 2026-07-05 (RS). FICA (SS 12.4% /
  Medicare 2.9% / Add'l Medicare 0.9% over $200k) on cash wages ≥ **$2,800** (research caught the stale $2,700) →
  FUTA Section A 0.6% / Section B credit-reduction (year-keyed **CA 1.2% / VI 4.5%**, Fed. Reg. 2026-00342) →
  total to Schedule 2 line 9; gating A/B/C + exclusion/Part-IV/EIN diagnostics. entity_types 1040; prod 111→112;
  `lookup/SCHEDULE_H/export/` = 200; validated 31/0. DECISIONS D-17. tts app build = [APP] lane.
- [x] **WO-16 · Form 4684** — Casualties & Thefts — ✅ DONE 2026-07-06 (RS). Section A personal (loss = min(basis,
  FMV decline) − insurance; **FDD-only §165(h)(5)**; $100/$500 floor + 10% AGI; qualified-disaster path, OBBBA window
  to **9/2/2025**) → Sch A; Section B business Part I + Part II **§1231/ordinary routing to Form 4797** L3/L14;
  Section C Ponzi safe harbor **95%/75%** (Rev. Proc. 2009-20); Section D §165(i) + OBBBA financial-scam theft =
  diagnostics. entity_types 1040/1065/1120S/1120; prod 112→113; `lookup/4684/export/` = 200; validated 29/0.
  DECISIONS D-18. tts app build = [APP] lane.
- [x] **WO-17 · Form 4952** — Investment Interest Expense Deduction — ✅ DONE 2026-07-06 (RS). §163(d): L8 =
  min(total investment interest L3, net investment income L6) → Sch A L9 (1040) / Form 1041 L10; excess L7 carries
  forward **indefinitely**; the **4g election** (include QD + net cap gain, loses the preferential rate) + L5
  misc-itemized / interest-exclusion diagnostics. §163(d) UNCHANGED by OBBBA. entity_types 1040/1041; prod 113→114;
  `lookup/4952/export/` = 200; validated 26/0. DECISIONS D-19. tts app build = [APP] lane.
- [x] **WO-18 · Form 8379** — Injured Spouse Allocation — ✅ DONE 2026-07-06 (RS). **Confirmed 8379** (Ken's "8679"
  was a typo). Allocation form (not a tax computation): Part I decision tree → is_injured_spouse; Part III allocates
  joint items col (a)=(b)+(c) (W-2 to earner, withholding follows income, std deduction 50/50, EIC IRS-allocated);
  community-property override (9 states); IRS computes the refund share (NOT estimated); 8379 (injured) ≠ 8857
  (innocent). No OBBBA impact. entity_types 1040; prod 114→115; `lookup/8379/export/` = 200; validated 29/0.
  DECISIONS D-20. tts app build = [APP] lane.
- [x] **WO-19 · Form 8814** — Parents' Election to Report Child's Interest & Dividends — ✅ DONE 2026-07-06 (RS).
  §1(g)(7) election (parent reports child's int/div/cap-gain-dist instead of the child filing 8615 — closes the
  8615 spec's D_8615_004). 3 tiers: first **$1,350** free / next $1,350 at 10% (max **$135** → 1040 L16) / over
  **$2,700** carried to parent split by character (L9 QD → 1040 3a/3b, L10 cap-gain → Sch D 13, L12 ordinary → Sch1
  8z); don't-file if ≥ **$13,500**. 8 eligibility conditions. 8615 link cited to §1(g)/Pub 929 (not i8814). entity_types
  1040; prod 115→116; `lookup/8814/export/` = 200; validated 26/0. DECISIONS D-21. tts app build = [APP] lane.
- [x] **WO-20 · Form 8839** — Qualified Adoption Expenses — ✅ DONE 2026-07-06 (RS). §23 credit (Part II) + §137
  employer-benefit exclusion (Part III); max **$17,280**/child, MAGI phaseout **$259,190–$299,190**. **★ OBBBA 2025:
  up to $5,000/child is now REFUNDABLE** (L11b → L13 → 1040 L30 — first year partly refundable); nonrefundable
  remainder → Sch 3 6c with 5-yr carryforward; special-needs full credit + §70403 tribal parity. $5,000-cap indexing
  cited to statute (§36C, $5,120 for 2026), not i8839. entity_types 1040; prod 116→117; `lookup/8839/export/` = 200;
  validated 30/0. DECISIONS D-22. tts app build = [APP] lane.
- [x] **WO-21 · Form 709** — United States Gift (and GST) Tax Return *(the biggest module)* — ✅ DONE 2026-07-06 (RS).
  Unified cumulative gift tax: §2001(c) rate schedule (top 40%); Part 2 tentative(current+prior) − tentative(prior) −
  applicable credit **$5,541,800** (= tentative tax on the **$13,990,000** BEA; the 2024 fig was $5,389,800) → gift tax
  due. Schedule A: annual exclusion **$19,000**/donee ($38k split §2513), unlimited marital (citizen) / **$190,000**
  noncitizen (§2523(i)), charitable. GST (Sch D) = 40% × inclusion ratio, exemption $13.99M; DSUE (Sch C) → Part 2 L7.
  **★ OBBBA $15M BEA is 2026+ (year-keyed, NOT 2025).** entity_types 709; prod 117→118; `lookup/709/export/` = 200;
  validated 32/0. DECISIONS D-23. ⚠ **[UNVERIFIED] Part 1/Sch A recon/Sch D sub-line #s** (raw PDF face unfetchable in
  research) — re-verify before the tts build; Part 2 lines 1-8 verified. tts app build = [APP] lane.
- [x] **WO-22 · Form 8832** — Entity Classification Election (check-the-box) — ✅ DONE 2026-07-06 (RS). Structural
  election (Reg. §301.7701-3): eligibility/classification decision tree (per-se corp ineligible; 60-month change gate)
  → elect corp/partnership/disregarded; defaults (domestic 2+ = partnership / 1 = disregarded; foreign by limited
  liability); effective window (75 days before / 12 months after); Rev. Proc. 2009-41 late relief (3yr 75 days);
  S-election → Form 2553 (not 8832); updated addresses (Kansas City/Ogden). Rev. 12-2013 (no annual reissue); no OBBBA
  impact. entity_types 1065/1120/1120S/1040; prod 118→119; `lookup/8832/export/` = 200; validated 31/0. DECISIONS D-24.
- [x] **WO-23 · Form 3115** — Application for Change in Accounting Method (§481(a)) — ✅ DONE 2026-07-06 (RS). Ken's
  specialty. The §446(e)/§481(a) method-change application: the §481(a) spread engine (negative 1-yr / positive 4-yr
  ratable / de minimis **$50k** 1-yr / under-exam 2-yr, Rev. Proc. 2015-13 §7.03); the Schedule E depreciation catch-up
  = (depr taken present) − (depr allowable proposed) at BOY + **DCN 7** routing (Rev. Proc. 2025-23 §6.01); the
  Schedule A cash↔accrual 2a–2h netting → Line 26; scope limits (under-exam L6a / 5-year L11a / cut-off L25 / DCN-7
  ≥2-yr) = diagnostics. **Form 3115 Rev. 12-2022** (no annual reissue); **no OBBBA impact** on the procedural
  machinery/§481(a). entity_types 1040/1065/1120/1120S; prod 119→**120**; `lookup/3115/export/` = 200; validated 36/0.
  DECISIONS D-25. tts app build = [APP] lane. **← the LAST S-16 item.**
- **✅ S-16 federal-forms gap-fill queue FULLY DRAINED 2026-07-06** (all 10: 8990 · Sch H · 4684 · 4952 · 8379 · 8814
  · 8839 · 709 · 8832 · 3115). Net-new RS scope now needs the **TaxWise forms-usage report** or a law change.

**S-17 · [APP] · PRODUCTIONIZE 1040 E-FILE (added 2026-07-08, Ken-directed after the ATS
completion — "how do we expand the forms / will there be an e-filing UI").** ATS proved the
stack (tag `mef-1040-ats-accepted`); this block turns it into something real clients file
through. Sequenced; each item is its own leg. ⚠ Serialization-only lane — competes with
1120-S/1041 ATS for CC time, Ken sequences.
- [ ] **17a · E-file coverage audit** — inventory the practice's real form mix (Lacerte
  regression data + the TaxWise forms-usage report when it lands) vs. the mapper registry →
  a RANKED doc-mapper backlog (expected top: 8949 detail, K-1→1040 XML, 4562, 2210). Output:
  a checklist appended here as 17f.
- [x] **17b · Direct deposit** — 2026-07-08 `0e8d096` (s33; stale mark reconciled
  2026-07-09). 1040 35b/c/d + header RefundDisbursementGrp RTN/DAN wiring + Payments-tab
  input card; refuse-not-downgrade; RTN prefix 01-12/21-32. ⚠ emits unpublished enum "1"
  (e-help Q#5). Detail: STATUS_ARCHIVE s33 / auto-memory s17b.
- [ ] **17c · E-file readiness diagnostics** — surface UnmappableValue reasons as per-return
  REDs ("can't e-file yet: needs Form X") through the existing diagnostics framework.
- [ ] **17d · Form 8879 workflow** — generate/print the e-file signature authorization
  (Practitioner PIN method requires a retained signed 8879 per return).
- [ ] **17e · E-file models + UI** — EfileTransmission/Submission/Acknowledgement models
  (+RLS, firm-scoped; long-deferred) → per-return E-file panel (readiness → build → status →
  acks w/ plain-language rule text via `business_rules.py`) → firm-level EF-Center dashboard.
  IN the tax-app SPA, not a separate app (DECISIONS: `apps/efile/` narrow-interface seam).
- [ ] **17f · Doc-mapper grind** — the ranked backlog from 17a, one leg per form.
- [ ] **17g · A2A channel** — Automated Enrollment (ASID) + X.509 cert + ID.me (Ken/EXT,
  ~7-14 day cert lead) → `A2ATransmitter` behind the existing Transmitter ABC → **A2A
  communication test in ATS** (channel-only; the scenario acceptances stand) → production
  A2A. IFA manual upload stays the pilot channel until this lands.
- [ ] **17h · GA Fed/State piggyback** — DOR software-developer registration + state ATS
  (already on the Shelf as "State e-file enablement"; listed here for sequence).

**S-18 · [APP] ∥ · PLATFORM PRE-LAUNCH TRIO (added 2026-07-09, Ken-directed — ideas fleshed
out in Claude-chat; prompts in hand for 18a-c).** Non-tax platform work — no compute risk;
interleaves with the Ken-gated ATS waits. Ken's framing: TaxWise's crash-prone lock-file
system is a reason this app exists (18a); PWA desktop install à la QBO = big value add (18c).
- [ ] **18a · Optimistic concurrency on returns** — version column on the return record;
  DRF save-time version check (reject stale saves with 409); React "modified elsewhere —
  reload" prompt. **NO pessimistic locks anywhere** — reentry must never depend on cleanup
  by a crashed process (the TaxWise failure mode). ⚠ Build-time design detail: version
  granularity vs the per-field focusout-autosave cadence (FormFieldValue) — a preparer must
  not 409 against their own parallel autosaves; composes with the s35 `select_for_update`
  row locks (same-row write serialization) — versioning adds the cross-session staleness
  guard. Priority: pre-January; Ken's roadmap slots it after client numbering wraps.
- [ ] **18b · Presence indicator** — "X also has this return open" banner via heartbeat
  with self-expiring TTL (~60s). Warns, never blocks. Fast follow to 18a.
- [ ] **18c · PWA installability — CODE COMPLETE 2026-07-09 (same session; on MAIN,
  Ken-ruled; manifest name "Delvio Tax", Ken-ruled).** vite-plugin-pwa: standalone
  manifest, palette colors (#1e40af/#e2e8f0), TTS-mark icons 192/512+maskable
  (`scripts/generate_pwa_icons.py`); SW autoUpdate+skipWaiting+clientsClaim; **/api/ never
  cached** (no runtimeCaching; nav-fallback denylist). Zero Django URL changes —
  WHITENOISE_ROOT already serves SW/manifest at root scope; prod.py adds the .webmanifest
  MIME. CC-verified live (`npm run preview`): SW ACTIVATED at root scope · manifest 200 ·
  API fetches bypass cache · precache = shell only · vitest 275 green (tsc's ~148 error
  lines are PRE-EXISTING — stash-compared; maintenance item). **TICKS when Ken verifies:**
  Chrome install → 2 independent windows → next-deploy pickup on reload.
- [ ] **18d · Support ticket system — DESIGN PENDING (parked 2026-07-09 so it isn't
  forgotten; do NOT build before a design pass with Ken).** Two-fold: (1) pre-release
  internal — dogfood channel to help finish the app; (2) production — internal + licensee
  firms once the app sells. Sketch: in-app button → issue form → routes to CC → CC triages
  with Ken → fix/respond loop; possibly a Slack channel as the transport. Details TBD.
- [x] **18e · Editor-header quick fixes (Ken's PWA-install feedback)** — 2026-07-09
  `334a1d7`. Breadcrumb dedupes client/entity when identical (was "X / X"); toolbar type
  bumped one step (breadcrumb text-base); **document.title = "Client — Form Year"** while
  a return is open → each installed-app window is identifiable in the Windows taskbar /
  Alt-Tab (the practical answer to "one taskbar button per open client": windows group
  under the one app icon, but hover-previews/Alt-Tab now show the client names).
  Live-verified via probe firm (deleted after; clean cascade); vitest 275 green.
- [x] **18f · Per-preparer settings (default entity tab; sort already existed)** —
  2026-07-09 `a022817`. `UserPreferences.return_list_entity_type` (mig 0003 → prod):
  the LAST-CLICKED entity tab becomes the login's landing tab (server-synced, "all"
  sentinel); saved sort (`return_list_ordering`, incl. alphabetical) already existed.
  Ken setup: click **Individual** once + sort **Name** once — every login lands there.
  Server prefs 14 + vitest 276 green; probe-verified live. **NEXT: Ken's further
  landing-page cosmetics (he has a list) — Ken directs.**
- [ ] **18g · Multi-client visibility — open-returns tab bar (QBO-desktop feel)** — a
  persistent in-app tab strip of the returns open in this window (click = switch, x =
  close), so "what do I have open" is visible in ONE window instead of many. DESIGN with
  Ken first: what counts as "open" (per-window tabs vs firm-wide presence), interplay with
  18b presence + 18a concurrency. (Native PWA "tabbed" display mode is still
  experimental/Chromium-flagged — build our own strip; revisit the manifest flag later.)

---

## THE SHELF (blocked on external — each with its unblock action)
*These are the "holes." Work the unblock action; the item then drops onto the Spine.*

- **1120-S mapper + 7004 mapper** `[APP]` ← **IRS business-family access notice.**
  UNBLOCK: watch e-Services inbox; e-help 866-255-0654 if not arrived in days. *High impact.*
- ~~Remaining 1040 ATS scenarios / S8-S5 upload~~ **✅ RESOLVED 2026-07-08 — ALL SEVEN ACCEPTED** (S-1).
- **1040 PRODUCTION status** ← **Ken calls the ATS assistor/e-help** (866-255-0654): confirm the
  scenario set complete; ask the production-cutover steps + A2A enrollment in the same call.
- **LLC e-file application** ← **EIN propagation.** UNBLOCK: retry ~Jul 16 (gate).
- **A2A (ASID/cert/SOAP/comm test)** ← **cert issuance 7–14 days.** SOAP build can roll; comm test
  gated. (Sequenced as Spine item 17g — channel-only ATS comm test, scenario acceptances stand.)
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
