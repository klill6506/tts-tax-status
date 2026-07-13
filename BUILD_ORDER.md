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

## ▶ NOW WORKING ON — **s71 (2026-07-13): the 1040 FA-MIRROR EXPORT-VERBATIM REBASE SHIPPED (DEFERRAL_AUDIT s69 item 1 CLOSED — the one non-Ken-gated candidate). Flow gate 484 → 500.** The s64 recipe re-run on the 1040 gate: mirror re-adopted from the deployed export (**397 active — the additive-append era over on ALL THREE mirrors**) · 15 new ids id-routed to real runners (NEW `_run_461_assertion` §461(l) quartet + `_run_4835_assertion` Form-4835 eight · SCHE-03/8582-08 id-first arms · FA-8941-01's 1040 arm rode the existing prefix chains) · **FA-1040-4835-06 STAGED pending (the green-lie guard: no 4562-engine → 4835 line-12 feeder exists; `flow_assertions_1040_pending.json` + the staged-pin test)** · 130 drifted shared definitions adopted as-is; one runner re-kind (FA-1040-8911-04 gating_check → flow_check per the RS Form-3800 amendment, + the Sch-3 6a landing pin). Tests + spec mirrors only — no app/compute code. `/bugs` at boot: clean. **THEN (same session, Ken live): the RATIFICATION SWEEP — all 12 open REVIEW_QUEUE calls answered via AskUserQuestion, every filed recommendation adopted; the queue is EMPTY. Three build units authorized → NEW Spine S-21.** **THEN (act three, "go"): S-21a SHIPPED — the 1120-S M-2 NNA-cap compute leg + the face column rotation (mig 0194).** The R002 cap live (divergence pin: beginning 10k / NNA year / 65k distributions → line 7 = 10,000, line 8 = (6,000)); **the fold-in caught a LIVE PRINT BUG — the legacy b/c/d letters printed every M-2 column value one face column off (positional AcroForm map); rotated to the face and print-probe-verified** (K17c dividends now land in the face AE&P column (c) on paper). Shared DB migrated + reseeded (359 clean); ORM 11/11 + browser + print probes green. ▶ NEXT: **S-21b — the 1065 partner-percentage diagnostic** → S-21c Sch B Q4 auto-answer (the 1120/709 authoring waves + the 1120-S ATS lane remain Ken-gated).

*(s70 below)*
**s70 (2026-07-12): S-20d COMPLETE — the Form 3115 tts print unit SHIPPED. Flow gate 475 → 484. THE S-20 ADMIN-FORMS BLOCK (a/b/c/d) IS FULLY DONE.** Print-only recipe third clone: Form3115 singleton (migs 0191/0192/0193) · §481(a) helpers pinned to the 7 spec oracles (DCN-7 catch-up / Sch-A 2a-2h netting / the §7.03 period router with de-minimis precedence / ratable spread) · D_3115_* ×8 code-registered + prod-seeded · f3115 Rev. 12-2022 AcroForm render (**Sch E L4a/L7 have NO face fields — Statement pages carry them AND satisfy L26's attach-a-summary**; overall_method = the Other box + specify text, no face box exists) · packet-on-attach (defaults ON — automatic changes attach) · entity Elections tab + NEW 1040 "Method Change (3115)" tab (self-managing card + seq guard) · FA-3115-CATCHUP/SPREAD/SCHA ACTIVATED + runners in both chains + all three mirrors refreshed (1120S 41 / 1065 39 / 1040 382 additive). Gates: flow 484 · test_3115 39 · manifest/acroform 201 (trip-wire 87) · combined 760 · tsc 0 · vitest 300 · ORM 14/14 · browser probe green. Boundaries → DEFERRAL_AUDIT s70 (7); RS-3115 OMB-citation nit → REVIEW_QUEUE. ▶ NEXT: **no unblocked Spine build item — the 1120/709 authoring waves + the 1120-S ATS completion lane are Ken-gated; idle — Ken directs** (candidates when unblocked: e-services reply → S7/S8 + upload loop · TaxWise forms-usage report → net-new RS scope · the s69/s70 1040 FA-mirror rebase is available as a maintenance pass).

*(s69 below)*
**s69 (2026-07-12): S-20b/c COMPLETE — the Form 2553 + Form 2848 tts print-unit pair SHIPPED (both units, one session). Flow gate 463 → 475.** Print-only recipe: models+rows (migs 0189/0190 + firms 0005 — Preparer gains caf_number/fax, exposed in Preparer Manager) · pure window/clock helpers pinned to the published oracles · 19+17 diagnostics code-registered verbatim + prod-seeded · AcroForm renders (2553 overflow copies + the 2013-30 margin legend; 2848 TIN routing + the §1.6012-1(a)(5) statement) · the approved 2848 L2 preparer autofill (YELLOW→GREEN) · entity Elections tab + 1040 POA tab (self-managing cards + the seq-guard fix caught live) · FA-2553-*/FA-2848-* ACTIVATED (RS `0e71f68`) + runners + all three mirrors in one motion. Gates: flow 475 · pair 36 · manifest/acroform/packet 206 · tsc 0 · vitest 300 · ORM 21/21 · browser probe green. Boundaries → DEFERRAL_AUDIT s69 (7 incl. the 1040-mirror rebase). ▶ NEXT: **S-20d Form 3115 tts app build (WO-23 spec DONE — no gate)**; Ken-gated items unchanged; Ken-next: enter the firm's CAF/fax on the Preparer records.

*(s68 draft leg below)*
**s68 (2026-07-12): S-20c FORM 2848 — RS DRAFT COMPLETE (WO-27). Superseded same-day: APPROVED + SEEDED (above).** The s67 draft-to-gate recipe re-run: gap 404 → research verbatim (f2848 Rev. 1-2021 + i2848 Rev. 9-2021, current; **⚠⚠ a Recent Development posted 08-Jul-2026 — FOUR DAYS before the draft — 5a-other/any-5b entries record the POA as "MODIFIED" on the CAF (the rep loses TDS + Tax Pro Account installment agreements); "never check line 4 unless truly specific-use"; encoded D_2848_MODCAF/D_2848_L4CAF**) → `f2848_source_brief.md` → gated `load_2848.py` (34 facts / 9 rules — the 45/60-day rep-signature window · the future-period CAF clock (receipt-year+3) · the "All years" RETURN-the-POA error · the URP four-condition gate · 4-rep/2-notice-copy limits · L6 attach-to-retain — / 30 lines / 17 diag / 9 scenarios / 3 draft FAs; **the L2 preparer-autofill value-add specced, FA-2848-CAFFILL**; entity_types all six; print-first, no MeF) → harness `validate_2848.py` **73/0** → WO-27 `⏳ AWAITING KEN` (+ tts REVIEW_QUEUE s68, approve-all recommendations). ▶ NEXT: **S-20d 3115 tts app build (WO-23 spec exported — buildable NOW, no gate)**; the 2553+2848 tts print units dispatch as a pair when the Gate-1s clear.

*(s67 below)*
**s67 (2026-07-12): S-20b FORM 2553 — RS DRAFT COMPLETE, ⏳ AWAITING KEN'S GATE-1 (WO-26). NOT seeded, tts NOT started.** The first post-S-16 greenfield order ran the front door to the gate and STOPPED there (the WORK_ORDERS two-human-gates rule — a draft never crosses Gate-1 unattended): gap re-confirmed (`lookup/2553/export/` 404) → research verbatim (Form 2553 Rev. 12-2017 + i2553 Rev. 12-2020, both current; **the printed Q1 user fee $6,200 SUPERSEDED → $5,750, Rev. Proc. 2026-1 App. A quoted from the IRB 2026-1 PDF, year-keyed; §1362(b)(5) PLR $14,500; KC/Ogden addresses live-verified**) → `f2553_source_brief.md` → `load_2553.py` DRAFT (28 facts / 8 rules / 45 face lines / 19 diag / 10 scenarios — **the three published i2553 window examples pinned (Jan 7→Mar 21 · Jan 1→Mar 15 · Nov 8→Jan 22)** — / 3 FAs staged DRAFT; entity_types ['1120S']; print-first, NO MeF channel) → harness `validate_2553.py` **82/0** (guard-refusal + twice-run + oracle pins) → WO-26 staged `⏳ AWAITING KEN` with the W1-W4 walk (+ tts REVIEW_QUEUE s67, recommend approve-all). `seed_all` fails soft on the gated loader. **2848 (S-20c) gap-confirmed 404 — same draft-to-gate recipe next.** ▶ NEXT: **S-20c 2848 RS draft-to-gate ∥ S-20d 3115 tts app build (RS spec DONE WO-23 — buildable NOW, no gate)**; the 2553 tts print unit dispatches when Ken approves Gate-1.

*(s66 below)*
**s66 (2026-07-12): S-20a ENTITY FORM 8283 — UNIT COMPLETE, BOTH LEGS (RS s65 `8b6faca` + tts s66).** The tts build leg: **1120-S K12b feeder** (a `_derive_schk_inputs_db` arm — reads the noncash_summary bridge-gate, writes the non-withheld total via the override-respecting pre-pass; typed GREEN wins; stale derives self-clear; feed flows below the $500 FORM gate) · **1065 print+diagnose only** (J-E2 — combined 13a face line; D_8283_016 coverage warning) · render_8283 opened to entities (legal name/EIN header) + the entity packet block · **MeF IRS8283 document** (DECLARED ReturnData1120S 2025v6.2 ref 1228; the SHARED build_irs8283/_extract_f8283 — corporate DonatedPropertyType ≡ IMF; K12b refDocId; Section B REFUSES, the J7 wet-ink seam; 1065 MeF = future mapper) · row-level D_8283_002-013 sweep entity rows (`_row_returns`) · NEW D_8283_014/015/016 code-registered + prod-seeded · **D_SCHK_8283 RETIRED** (is_active=False, the D_1040_001 precedent) · UI: the worksheet mounts on the entity Schedule K tab (entity-aware blurb/hint; NavScope D_8283_→sched_k) · **FA-ENT-8283-01/02 ACTIVATED with runners + both export-verbatim mirrors refreshed in one motion — flow 460→463**. Gates: 570 batch · MeF 83 (S5/S6 byte-stable) · tsc 0 · vitest 300 · NEW test_8283_entity 11 (T14-T16 oracles) · live ORM probe 15/15 · live browser probe (typed 3,000 → autosave → K12b server-painted YELLOW). Two s63 fixtures updated (typed K12b carries is_overridden — the derived-write convention). Boundaries → DEFERRAL_AUDIT s66 (5); J-E1/E2/E3 ratifications queued (shipped to the recommendations). ▶ NEXT: **S-20b Form 2553 app build (spec-first — RS 2553 spec check; 404 → STOP)** → 2848 → 3115 (Ken-gated items unchanged).

**Session 71 wrap, act three (2026-07-13; "go"):**
- [x] `[APP]` **S-21a · 1120-S M-2 NNA-cap compute leg + face column rotation —
  UNIT COMPLETE** — 2026-07-13 (mig 0194): the ratified R002 cap in the formula
  registry (spec scenarios = oracles; the superseded Mar-30 "cap removed" tests
  rewritten to law); the s63 off-face fold-in surfaced a LIVE PRINT BUG (b/c/d
  one column off on paper) — FormLine keys rotated to the face + JSON/mapping/PY
  stores + MeF builder + client grid; K17c derive → M2_7c; shared DB migrated +
  reseeded; full gates + ORM/browser/PRINT probes green.

**Session 71 wrap, continued (2026-07-13; Ken live — "ask me the yes/no calls"):**
- [x] `[docs]` **⟨RATIFICATION SWEEP⟩ ALL 12 open REVIEW_QUEUE calls ANSWERED** —
  2026-07-13, the house AskUserQuestion format, every recommendation adopted:
  M-2 NNA cap · R007 AMT · 8283 J-E1/E2/E3 · K-1 ZZ keep · 40% election
  leave-unimplemented · partner-pct BUILD · $300 DFE proxy keep · RC001
  as-is · Sch B Q4 auto-answer APPROVED · SCHB+Q11 ratified · 8824 both ·
  3115 OMB fold-in. REVIEW_QUEUE now EMPTY; Spine S-21a/b/c added
  (M-2 NNA compute leg first — mission-lane); NOW/NEXT re-cut.

**Session 71 wrap (2026-07-13; "go" — autonomous):**
- [x] `[APP]` **1040 FA-mirror export-verbatim rebase (DEFERRAL_AUDIT s69
  item 1; not previously on the Spine — ADDED per the never-silent rule)** —
  2026-07-13: drift ledger (382 pinned vs 398 exported; 0 mirror-only) →
  16 new ids id-routed (NEW _run_461_assertion + _run_4835_assertion;
  SCHE-03/8582-08 id-first arms; FA-8941-01 prefix-routed already);
  FA-1040-4835-06 staged pending (no 4562→4835 line-12 feeder — green-lie
  guard) + test_flow_assertions_1040_pending_staged; 130 drifted shared
  definitions adopted as-is (FA-1040-8911-04 re-kind fix); flow 484 → 500
  GREEN. Tests + specs only — no app code, no migration, no deploy needed.

**Session 70 wrap (2026-07-12; "go" — autonomous):**
- [x] `[APP]` **S-20d Form 3115 tts print unit — UNIT COMPLETE** — 2026-07-12:
  Form3115 singleton (migs 0191/0192/0193) · compute_3115 pinned to the 7
  spec oracles · D_3115_* ×8 code-registered + prod-seeded · f3115 Rev.
  12-2022 map/render (Sch E statements; the value-first statement-line
  rule) · form-3115 + render-3115 endpoints · entity + 1040 mounts w/ seq
  guard · FA-3115-* activated + runners + mirror refresh (flow 484) ·
  ORM 14/14 + browser probe green · manifest 87.
- [x] `[docs]` Spine ticks: 20a (missed at s66 close — recorded) · 20d.
  S-20 block closed; NOW/NEXT re-cut (idle — Ken directs).

**Session 69 wrap (2026-07-12; "go" — autonomous):**
- [x] `[APP]` **S-20b Form 2553 tts print unit — UNIT COMPLETE** — 2026-07-12:
  Form2553 + consent/QSST rows (migs 0189/0190) · the §1362(b) 2mo15d
  corresponding-day calculator (published i2553 Examples 1-3 + Dec-31 +
  leap-year pinned) + the 2013-30 relief router · D_2553_* ×19 code-registered
  + prod-seeded · AcroForm render (consent overflow page-2 copies · QSST
  page-4 copies · the margin legend abs_pos literal) · packet on
  attach_to_return only · entity Elections tab · FA-2553-WINDOW/COUNT/8832
  activated + runners.
- [x] `[APP]` **S-20c Form 2848 tts print unit — UNIT COMPLETE** — 2026-07-12:
  Form2848 + rep×4/matter×3 rows · firms 0005 (Preparer + caf_number/fax +
  Manager UI) · **the approved L2 preparer autofill (CAF-or-'None', YELLOW →
  manual-PATCH GREEN)** · future-clock/sign-window/modified-CAF/URP helpers ·
  D_2848_* ×17 code-registered + prod-seeded · AcroForm render (Table_Line3/
  Table_PartII nesting; the §1.6012-1(a)(5) statement) · 1040 + entity mounts ·
  FA-2848-FUTURE/SIGN45/CAFFILL activated + runners; all three tts gate
  mirrors refreshed (flow 475).

**Session 68 wrap, continued (2026-07-12; Ken live — "Can you give me those questions more clearly?"):**
- [x] `[RS]` **⟨GATE-1⟩ ×2 APPROVED + SEEDED — 2553 (WO-26) + 2848 (WO-27)** —
  2026-07-12: plain-English walk re-present → AskUserQuestion → "Approve" ×2;
  sentinels flipped (+approval recorded in docstrings); harnesses re-run
  post-flip 82/0 + 73/0; both prod-seeded (18 + 16 authority links);
  `lookup/2553|2848/export/` = 200 verified; tts mirrors cached from the
  deployed endpoint; FA export clean (drafts excluded, 1120-S = 32).
  RS TaxForms = 122. → tts print units DISPATCHED as a pair.

**Session 68 wrap (2026-07-12; "go" — autonomous):**
- [~] `[RS]` **S-20c Form 2848 RS greenfield — DRAFTED TO GATE-1 (WO-27, ⏳ AWAITING
  KEN)** — 2026-07-12: source brief + gated loader (34/9/30/17/9 + 3 draft FAs) +
  harness 73/0; the 08-Jul-2026 "modified"-CAF Recent Development caught + encoded;
  the L2 preparer-autofill value-add specced; NOT seeded — Gate-1 is Ken's; the
  W1-W4 walk filed in WO-27 + REVIEW_QUEUE s68. tts leg not started.

**Session 67 wrap (2026-07-12; "go" — autonomous):**
- [~] `[RS]` **S-20b Form 2553 RS greenfield — DRAFTED TO GATE-1 (WO-26, ⏳ AWAITING
  KEN)** — 2026-07-12: source brief + gated loader (28/8/45/19/10 + 3 draft FAs) +
  harness 82/0; Q1 fee $6,200→$5,750 catch (Rev. Proc. 2026-1, IRB-quoted);
  NOT seeded — Gate-1 is Ken's; the W1-W4 walk filed in WO-26 + REVIEW_QUEUE s67.
  tts leg not started (front-door rule). 2848 gap re-confirmed (404).

**Session 66 wrap (2026-07-12; "go" — autonomous, same conversation):**
- [x] `[APP]` **S-20a entity Form 8283 tts leg — UNIT COMPLETE** — 2026-07-12:
  K12b feeder (override-respecting pre-pass) · 1065 no-feed + coverage warning ·
  entity render/packet · MeF IRS8283 + refDocId + Section-B refuse · entity
  diagnostics sweep + D_SCHK_8283 retired · entity UI mount · FA-ENT-8283-01/02
  activated + runners + mirror refresh (flow 463) · 11 new tests · ORM 15/15 +
  browser probes green · prod seed_rules run.

*(s65 below)*
**s65 (2026-07-12): S-20a 8283-ENTITY RS LEG COMPLETE — RS lane (RS `8b6faca`, prod-seeded, export verified, tts mirror refreshed).** The shared 8283 spec's entity STATED BOUNDARY (D_8283_010) closed by an additive amend-by-lookup, i8283 Rev. 12-2025 verbatim (PTE + Members paragraphs excerpted): **R-8283-ENTFILE** (PTE noncash > $500 files Section A/B WITH the 1065/1120-S; IRS8283 = a DECLARED ReturnData1120S 2025v6.2 document — MeF leg buildable; 1065 MeF rides the future mapper) · **R-8283-ENTSECB (the $5,000 Section-B test reads the ENTITY item/group amount — NEVER per-member; T15 pins the division-first regression: $6,000 item, two 50% shareholders, each allocation $3,000, still Section B)** · **R-8283-ENTFEED** (1120-S: rows → K12b YELLOW default / typed GREEN override, the override-respecting pre-pass recipe; **1065: NO FEED — the 2025 face 13a is ONE combined cash+noncash line; print+diagnose, D_8283_016**) · **R-8283-ENTCOPY** (completed copy to each allocated member — D_8283_015 info + K-1 package note). D_8283_010 re-scoped in place (member side only); NEW D_8283_014 (entity > $500 no rows — replaces tts D_SCHK_8283 in the tts leg) / 015 / 016; scenarios T14-T16 with oracles. **FA-ENT-8283-01/02 staged DRAFT** (the new-FAs-default-ACTIVE trap vs the s64 export-verbatim mirrors — verified: the deployed 1120S FA export still serves exactly 30); the tts leg activates + writes runners + refreshes mirrors together. J-E1/E2/E3 → REVIEW_QUEUE s65 w/ recommendations. Harness 45 green (twice-run, polluted-title overwrite, oracles); tts flow 460 + test_compute_8283 green vs the refreshed mirror. ▶ NEXT: **S-20a tts leg** (the full audit map is in tax-app STATUS RESUME item 2: render gate open-up + entity packet · K12b feeder pre-pass · CRUD recompute chokepoint · builder_1120s IRS8283 + the Section-B PDF-attachment refuse seam · D_8283_014/015/016 code-registered + D_SCHK_8283 retired · entity UI mount · FA runners + RS activate + mirror refresh · probes) → 2553 → 2848 → 3115 (Ken-gated items unchanged).

**Session 65 wrap (2026-07-12; "go" — autonomous, same conversation as s64):**
- [x] `[RS]` **8283 entity-arm amendment (S-20a RS leg)** — 2026-07-12 RS
  `8b6faca`: 4 entity rules + 3 diagnostics + D_8283_010 re-scope + T14-T16 +
  2 DRAFT-staged FAs, i8283 Rev. 12-2025 verbatim; harness 45 green; prod
  seeded (9 rules/16 diags/16 scenarios, all cited); deployed export verified;
  tts mirror refreshed (flow 460 green against it). J-E1/E2/E3 queued.

*(s64 below)*
**s64 (2026-07-12): RS FA-EXPORT RECONCILIATION PASS COMPLETE — APP lane (test runners + spec mirrors only; queued since s32).** Both flow-assertion gate mirrors re-adopted from the deployed RS export: **1120S = export VERBATIM (30 actives)** — the s35 hand-splice pin era over; **1065 = export minus 4 staged-pending build-gaps (32 actives)**. Every s32-drift id id-routed to a runner: FA008-012 (the Mar-30 8825/Sch-L set, RS declarative definitions adopted as-is) → NEW `_run_8825_schl_assertion` (shared-code pins; FA012 re-derives L15a/L15d from BOTH registries); FA-ENT-BND-01/02/03 → NEW `_run_entbnd_assertion` (the S-5 boundary module is BUILT — the pending-staging was stale; activated on both entity gates); FA-4562-179-02/03 (runners pre-existed); RC001's expected_formula-only variant → NEW formula_equals fallback (pins the L24d=ΣM2_8a-d tie, vacuousness-probed 40,000==40,000); FA-8941-01 (1065, pure-math runner pre-existed). Still staged (test-pinned): GATE-8990-163J · GATE-704C-706D-DEFER · RECON-M2-CAPITAL · **FA-ENT-8824-01's 1065 arm** (capital 8824 → D_8824_011 manual refuse until the Schedule D (1065) unit — activating would green-lie: its Sch-D source pins pass under 1065 where the code never runs). Gate files ensure_ascii (mojibake healed). **Flow gate 447 → 460 GREEN**; +face-renumber/schk pin suites = 493. REVIEW_QUEUE: s35 item RESOLVED + 2 advisory notes ($300 DFE proxy = keep · RC001 shape = leave RS as-is). `/bugs` boot sweep: clean. ▶ NEXT: **S-20a 8283-entity (spec-first; its D_SCHK_8283 warning is live)** → 2553 → 2848 → 3115 (Ken-gated items unchanged).

**Session 64 wrap (2026-07-12; account-switch boot, "pick up the queue"):**
- [x] `[APP]` **RS FA-export reconciliation pass (queued since s32)** — 2026-07-12:
  drift ledger (1120S 30v26, 1065 36v23) → id-routed runners for every drift id
  (NEW _run_8825_schl_assertion + _run_entbnd_assertion + the formula_equals
  expected_formula fallback); mirrors re-adopted from the deployed export;
  1065 pending set re-cut (ENT-BND activated, FA-ENT-8824-01 1065-arm staged);
  flow 447→460 green; REVIEW_QUEUE s35 resolved.

*(s63 below)*
**s63 (2026-07-12): SCH_K INPUT SUB-UNIT (the s56 tail items 4-7) COMPLETE — APP lane (mig 0188).** The 1120-S K12 family re-keyed to the 2025 face IN PLACE (K12a cash / NEW 12b noncash / 12c inv-int / 12d 59(e)(2) / 12e other; prod renamed rows were ALL BLANK — zero-risk; MappingRule.target_line strings + py_manual/baseline JSON re-keyed, the 0187 recipe). NEW: **K16e derives BOTTOM-UP from ShareholderLoan.loan_repayments** (override-respecting `_set_field_value` pre-formula-pass, NOT a registry formula — **⚠⚠ the seeder clears is_overridden on newly-computed rows and every manual PATCH sets it; a formula flip would stomp typed values on reseed**) with **PER-RECIPIENT K-1 box 16 code E** (each owner's own repayments — RS K1 R016; pro-rata fallback; Σ K-1 == Sch K by construction) + loan-CRUD recompute chokepoint; **K16f** enters K18 (face verbatim R019 "11 through 12e and 16f") + M2_5a (i1120s p.50 AAA adjustment 2 verbatim) + K-1 code F; **K14a/14b = the K-2/K-3 CHECKBOXES** (the pre-K-2 "Name of Country" text row converted; MeF emits DomesticFilingExceptionInd and REFUSES a checked 14a — K-2 unbuilt, the ADS-seam precedent; D_EB_K2K3/DFE_OK gain the 1120-S arm: K16f>0 or 14a); **M2_7d ← K17c** (the AE&P column line-7 reduction, M2_8d follows — ⚠ the M-2 grid's internal letters are OFF-FACE b=OAA/c=STPI/d=AE&P vs face b=PTEP/c=AE&P/d=OAA; fold that re-key into the Ken-gated NNA compute leg). K-1 box 12 codes now A/C/H/J/ZZ (noncash = C; NEW D_SCHK_CHARCODE = the RS K1 D006 mirror), box 16 A-F; NEW D_SCHK_8283 (noncash>$500, form rides S-20a) + D_SCHK_17C_DIV (1099-DIV channel = the sherpa-1099 lane). M1_6b gains K12b; MeF statement decompositions widened. Gates: flow 447 · suites 80+530+347 (the 3 k1_import_stage3 errors pre-existing) · seed pins 359 · tsc 0 · vitest 300 · **shared DB: mig 0188 + seed_1120s rerun (359) + seed_rules DONE** · live ORM probe 14/14 (values/K-1/print/diagnostics, cascade-deleted) · live browser probe (8 new rows render; real-key K12b 2000 → server-painted K18 −2000/M2_5a 2000; K17c 12000 → the M-2 AE&P cell paints; K14a boolean chips). Boundaries → DEFERRAL_AUDIT s63 (7 items). ▶ NEXT: **RS FA-export reconciliation pass (queued since s32)** → S-20a 8283-entity (its D_SCHK_8283 warning is already live) → 2553 → 2848 → 3115 (Ken-gated items unchanged).

**Session 63 wrap (2026-07-12; "go" — autonomous):**
- [x] `[APP]` **SCH_K input sub-unit (the s56 tail items 4-7)** — 2026-07-12
  (mig 0188): K12 family face re-key + charitable cash/noncash split; K16e
  bottom-up derive + per-recipient K-1 code E + loan-CRUD recompute; K16f →
  K18/M2_5a/code F; K14a/b K-2/K-3 checkbox conversion + the MeF 14a refuse
  seam + the 1120-S D_EB_K2K3 arm; M2_7d ← K17c; NEW rules_1120s_schk (3,
  prod-seeded); shared-DB migrate+reseed+rules; live ORM (14/14) + browser
  probes green; tests: NEW test_schk_input_subunit (13) + pins/fixtures swept.

*(s62 below)*
**s62 (2026-07-12): RENUMBER UNIT #7 (1120S_M3 line_map) COMPLETE — RS lane. The s44 audit renumber queue is CLEARED (3800 rides the future GBC entity unit).** The M-3 face entered the repo — f1120ss3.pdf **Rev. December 2019, the current revision** (⚠ filename trap: irs.gov `f1120sm3.pdf` = the C-corp 1120 M-3) + i1120ss3; manifest-registered (84, trip-wire bumped). The old P1-*/P2-*/P3-* map proved FABRICATED (Part III ends at 32 — spec had 33-36; P1 line 1 = the 1a/1b statement questions; P1-FS/P1-RS/P2-DEP/P3-DEP on no revision). Rebuilt verbatim: 30 face facts (1a-12d incl. 4b GAAP/IFRS/tax-basis/other + the 12a-d grid) · R001-R005 · 87 face lines (I/II/III prefixed; II-21a-g; III-23a/b; III-22 Reserved) · D001-D007 · 5 scenarios (published Example 1 "$12M FS / $8M Sch L = not required" · P1 L11 combine · P3 L32 sign-flip). **NEW: R005 = the $50M complete-ENTIRELY tier + the through-Part-I + M-1 option (M-1 L1 = P1 L11) — the tier the pre-s44 "$50M" error CONFLATED with the $10M filing gate (which reads SCHEDULE L assets)**; tie chain: P1 L11 = P2 L26(a) (or M-1 L1) · **P2 L26(d) = Schedule K L18** (the M-1 L8 anchor). 5 verbatim excerpts (the IRS's "(Form 1065)" typo flagged, not propagated); SCHB's stale "$50M" link note healed (refresh-delete). Harness 165/0 (twice-run pre-polluted) · prod stale-deletes exact (13 facts + all 20 fabricated lines) · idempotent rerun clean · export verified (87/5/30/7/5) · NEW tts mirror `1120s_m3_spec.json`. tts = boundary-RED by design (no M-3 leg; the three $10M constants verified, reading Sch L 15d). Gates: manifest 3/3 · flow 447. ▶ NEXT: **SCH_K input sub-unit (the s56 tail 4-7)** → FA-export reconciliation → S-20 (Ken-gated items unchanged); 3800 renumber rides the GBC entity unit when that lane opens.

**Session 62 wrap (2026-07-12; "go" — autonomous):**
- [x] `[RS]` **1120S_M3 line_map renumber (audit-ledger unit #7)** — 2026-07-12:
  fabricated P-map → the Rev. 12-2019 face verbatim (87 lines); NEW R005 $50M
  entirely/through-Part-I tiers; the L26(d)=K18 tie specced; SCHB stale note
  healed; harness 165/0; prod seeded, deletes exact; export verified; tts
  mirror cached.
- [x] `[APP]` **f1120ss3.pdf template download + manifest registration** —
  2026-07-12 (84 forms; trip-wire bumped; the filename trap documented).
  tts drift check: boundary-RED by design, $10M constants correct — no app
  change; manifest 3/3 + flow 447 green.

*(s61 below)*
**s61 (2026-07-12): RENUMBER UNIT #6 (Form 6198) COMPLETE — RS lane.** The fabricated 6198 line_map rebuilt verbatim vs f6198.pdf **Rev. November 2025 (a NEW revision)** + i6198 Rev. 11-2025: 21 face-keyed facts (incl. the 15/16/18 a/b checkbox choice facts) · R001-R009 (the §465 substance KEPT re-keyed — Part I combine 1-4 / Part II 6-10b / Part III 11-19b w/ the 15b prior-19b-never-10b caution / L20 larger-of / L21 smaller-of + pro-rata carryover / QNF / §465(e) recapture / basis→at-risk→passive ordering / prior-year nondeductible in Part I) · 25 face lines · D001-D006 · 7 scenarios incl. the THREE published i6198 L21 examples + the p.3 L5 income-offset example; the paraphrase "At-risk computation" excerpt → VERBATIM under the same label + 5 new (forms_supporting.py mirrored); RuleAuthorityLink refresh-delete + full in-loader self-heal. Harness 144/0 (twice-run pre-polluted) · RS prod seeded (stale-deletes exact: 9 facts + lines 2/10 + 2 scenarios) · idempotent rerun clean · deployed export verified · NEW tts mirror `form_6198_spec.json`. **tts had NO 6198 leg — no drift possible**; f6198.pdf hash-verified + manifest-registered (83, trip-wire bumped). Gates: manifest 3/3 · flow 447. ▶ NEXT: **M3 line_map (unit #7 — M-3 face template download FIRST: f1120sm3 → forms_manifest + update_irs_forms.py) → 3800 (rides the GBC entity unit)** → SCH_K input sub-unit → FA-export reconciliation → S-20 (Ken-gated items unchanged).

**Session 61 wrap (2026-07-12; "go" — autonomous):**
- [x] `[RS]` **Form 6198 2025-face renumber (audit-ledger unit #6)** — 2026-07-12:
  fabricated line_map → the Rev. 11-2025 face verbatim (a NEW revision); §465
  rules kept re-keyed; published L21 + L5 examples pinned as scenarios; excerpts
  verbatim (6 labels); self-heal deletes exact on prod; export verified; tts
  mirror cached.
- [x] `[APP]` **f6198.pdf manifest registration** — 2026-07-12: the template was
  UNREGISTERED (bulk 1040-era download); hash-verified vs a fresh irs.gov
  download + registered (83 forms); trip-wire bumped; manifest 3/3 + flow 447
  green. tts otherwise untouched (no 6198 leg exists — render/compute/MeF/seeds
  drift-checked, all absent).

*(s60 below)*
**s60 (2026-07-12): RENUMBER UNIT #5 COMPLETE — the tts FFV semantic re-key SHIPPED (`e4c4ac8`, mig 0187).** Internal 1120-S page-1 keys now EQUAL the 2025 face (19=7205 NEW input · 20/21/22 · 23a-c · 24a-d/z w/ NEW 24d EPE · 25 · 26/27 · 28a + NEW computed 28b); mig 0187 renamed FormLine keys IN PLACE on the shared DB (278 FFVs/key verified carried; seed rerun 355 lines / ZERO stale deletes) + py_manual/baseline JSON re-keys; imported PY line_values needed NO migration (the 2024 print already used the new numbering — the re-key FIXED the live CY-vs-PY compare misalignment); **LIVE FIX: owed/overpayment formulas gained the line-25 penalty term (RS R014)**; print shim RETIRED (f1_37/f1_47/f1_53 widget-verified); MeF LINE_ORDER + 7205/EPE XSD elements; GA-600S S6_1 · 8879-S/8453-S · diagnostics · client footer/PY lists re-keyed; K3c duplicate-alias removed (pre-existing s56 validator trip). s56-tail 1-3 DELIVERED; 4-7 = the SCH_K input sub-unit. Gates: flow 447 · consolidated 583 · S5+S6 8/8 · affected 208+106+36 · tsc 0 · vitest 300 · live ORM probe PASS. ▶ NEXT: **6198 renumber (unit #6) → M3 line_map (face template download first) → 3800** → SCH_K input sub-unit → FA-export reconciliation → S-20 (Ken-gated: M-2 NNA-cap ratification · R007 · density feel-check · e-services · item 10).

**Session 60 wrap (2026-07-12; "go" — autonomous):**
- [x] `[APP]` **renumber unit #5 tts leg — the page-1 FFV semantic re-key + data
  migration + s56-tail items 1-3** — 2026-07-12 `e4c4ac8` (mig 0187): keys = face;
  penalty-term live fix; shim out; MeF/GA/diagnostics/client re-keyed; shared-DB
  migration + reseed verified; full gates + live ORM probe.

*(s59 below)*
**s59 (2026-07-12): PAGE1+M1+M2 RENUMBER (audit unit #5) — RS LEG COMPLETE (RS `bb42381`).** PAGE1 → 2025 face (NEW line 19 = Form 7205 §179D, the 19/20/21→20/21/22 shift, + the NEVER-SPECCED Tax and Payments block 23a-28e; R011-R015/D005-D006/3 scenarios); **M-1 was a WHOLE-FORM FABRICATION** ("3a guaranteed payments §707(c)" is a 1065 line — real face: 3a depreciation/3b T&E, 4=1+2+3, 7=5+6, **L8 ties SCHEDULE K L18**, never page-1 OBI; fabricated fact/rule/diag/line deleted, itemization facts + R007 applicability + R008 3b-composition added); M-2 rows 2/4 → page-1 **L22** + PTEP/AE&P columns added + **R002 AAA distribution cap corrected to the §1368(e)(1)(C) without-net-negative-adjustment base (published i1120s pp.50-51 example + divergence pin = scenarios; Ken ratification → REVIEW_QUEUE s59)**; fabricated/composite excerpts → verbatim (8 deleted / 19 added, pymupdf 2026-07-12); link-note refresh deletes + whitelist self-heal everywhere; SQLite harness **319/0** (twice-run self-heal proof); prod seeded + ORM-verified + deployed exports verified; tts mirrors refreshed (flow 447 · spec 45 · pins 18 green — no tts code this session). ▶ NEXT: **unit #5 tts leg — the page-1/K FFV semantic re-key + FFV data migration + the s56 print-shim removal + the seven s56 deferrals** (the key-by-key map is in tax-app STATUS.md; + the M-2 NNA cap in compute once Ken ratifies) → 6198 → M3 line_map → 3800 → FA-export reconciliation → S-20 form units (Ken-gated: NEW M-2 NNA-cap ratification · R007 ratification · density feel-check · e-services · item 10).

**Session 59 wrap (2026-07-12; "go" — autonomous):**
- [x] `[RS]` **1120S PAGE1+M1+M2 2025-face renumber, RS leg (audit-ledger unit #5)** —
  2026-07-12 RS `bb42381`: PAGE1 7205 + tax/payments block; M-1 fabrication replaced
  (K18 tie); M-2 L22 tie + PTEP/AE&P + the R002 NNA-cap tax-law fix; excerpt hygiene;
  harness 319/0; prod seeded/verified; tts mirrors refreshed + gates green.
- [x] `[APP]` **unit #5 tts leg** — FFV semantic re-key + data migration — 2026-07-12
  `e4c4ac8` (s60; the s56 deferral tail items 4-7 completed s63, mig 0188).

**Session 58 (2026-07-11): 1120S_SCHL RENUMBER (audit unit #4) COMPLETE — RS spec's TWO fabricated numbering systems replaced with the 2025 face verbatim (assets 1-15 w/ contra pairs · liabilities 16-21 NO subtotal · equity 22-26 · total 27; fabricated R002 total-liabilities rule DELETED; R001 gains lines 4/6/7/8 + the 2/10/11/13 contra pairs; R004 = L15==L27; NEW R009 L15(d)→page-1 item F, i1120s p.49 verbatim; R005-R008 substance kept; fabricated source excerpts replaced with real p.49 text; prod stale-deletes 26 facts + R002 + L11/L28), seeded + exported + mirror refreshed (RS `bfcb95a`). **tts verified FACE-CORRECT end-to-end** (seeds L1a-L27d · compute L15/L27 match R001/R003 exactly · L24d←M-2 · $250K check reads L15d · MeF SCHL_LINE_ORDER w/ derived contra nets · item F ← L15d print+MeF) — RS-only drift, no app fix; NEW TestScheduleLRowPins y-band pins. Gates: pins 18 + flow 447 (465) · schl_boy_inventory 4/4.**

**Session 58 wrap (2026-07-11; "go" — autonomous):**
- [x] `[RS]` **1120S_SCHL 2025-face renumber (audit-ledger unit #4)** — 2026-07-11 RS `bfcb95a`:
  both fabricated numbering systems → the face (65 facts / 8 rules / 31 lines / 7 diag /
  4 scenarios); R002 deleted; NEW R009 item-F tie (p.49 verbatim); fabricated excerpts
  replaced verbatim; stale self-heal (prod: 26 facts + R002 + L11/L28); export verified;
  tts mirror refreshed.
- [x] `[APP]` **tts Schedule L face-verify** — 2026-07-11: seeds/compute/print/MeF all
  face-correct (item F ← L15d both sides) — no fix; `TestScheduleLRowPins` added
  (equity-block placement + total rows); pins 18 + flow 447 green.

**Session 57 wrap (2026-07-11; "go" — autonomous):**
- [x] `[RS]` **K1_1120S 2025-face renumber (audit-ledger unit #3)** — 2026-07-11 RS `a0e908c`:
  rebuilt verbatim vs f1120ssk.pdf 2025 + i1120s pp.30-48 (44 facts / 17 rules / 33 lines /
  6 diag / 7 scenarios / 7 verbatim code-table excerpts); boxes 14/15/18/19 + Part I/II
  items added; the box 12/13 sub-line-letter claims replaced with the real 2025 code
  tables; R-K1-ROUND's "box 17 AC health insurance" parenthetical corrected; in-loader
  fact/line/scenario self-heal; SQLite-validated · Supabase-seeded · deployed export
  verified · tts mirror refreshed.
- [x] `[APP]` **K-1 code-letter print/MeF fixes (the audit's verify arm — s37 belief
  falsified)** — 2026-07-11 `69e7e08`: box 13 pre-2023 alphabet → 2025 (C/D/E/F/G/I);
  K12c I→J · K12d L→ZZ (+ typed statement); health insurance AC→ZZ (+ statement,
  K17_AC→K17_HEALTH); K13e now allocates (code G both sides);
  `TestK1CodeLetters2025` pins (5); flow 447 · pins 49 · MeF+forms 234 · S5+S6 8/8.
- [x] `[docs]` REVIEW_QUEUE s57: K-1 health-insurance ZZ presentation ratification
  (shipped + pinned; recommendation = keep the ZZ info item); DEFERRAL_AUDIT s57
  (charitable A/B–C-G split · ZZ typed breakdowns · box-10 print code · K16e/16f
  allocation rides the SCH_K input unit · F2/F3/item-I inputs · item-D print blank).

**Session 56 wrap (2026-07-11; "go" — autonomous):**
- [x] `[RS]` **SCH_K_1120S 2025-face renumber (audit-ledger unit #2, WO-25)** — 2026-07-11 RS `d7aaf69`:
  rebuilt verbatim (52 facts / 19 rules / 47 lines / 6 diag / 6 scenarios); 13f biofuel,
  17a-d split (17c AE&P → 1099-DIV), L18 → M-1 L8 tie (the R018/D012 "= Page 1 L21"
  tax-law error corrected); stale deletes in-loader; export verified; tts mirror refreshed.
- [x] `[APP]` **1120-S face-print fixes** — 2026-07-11 `f2d4e89`: page-1 old-19-down re-route
  (7205 insertion shift + tax/payments block), K13 one-field-off, K12 amounts, K3 unmapped;
  K17c off the K-1 (i1120s p.40) + prod relabel; `test_1120s_face_renumber_pins.py` (11);
  flow gate + 752 affected tests green. Deferral tail (7) → DEFERRAL_AUDIT s56.
- [x] `[RS]` **RET-G5 stale-scenario amendment (REVIEW_QUEUE s54)** — 2026-07-11: rename-orphan
  row deleted; `load_1040_retirement` now self-heals scenario renames; tts exclusion removed
  (topic5 41 passed).
- ⚠ ADDED to the renumber queue (new s56 finding): **1120S_PAGE1 + M1 + M2 blocks**
  (pre-Form-7205 numbering; fabricated M-1 excerpt line) — after SCHL; the tts side
  carries the page-1/K FFV re-key + data migration + the s56 deferral tail.

**Session 54 wrap (2026-07-11; "go" — autonomous, the s48/s52 directives):**
- [x] `[APP]` **Full-suite straggler triage (STATUS item 1) COMPLETE** — 2026-07-11
  `169401d`…`ca0a710` (ten commits, ~45 test files). The s53 packet-sequence
  "possibly-real print bug" = a stale test (compute-owned Sch 2/2 AMT + Sch 3/1
  FTC seeded without override — NOT a print bug; real returns unaffected).
  Dominant classes: whole-dollar pins (state PULL lines byte-preserve — classify
  per line) · compute-ownership seeding · CRUD mutation-recompute semantics ·
  registry/seed trip-wires lagging the s41-s48 units · sibling-spec drift
  (retirement spec 18→23; LSE/MED routed to their own homes) · sidebar packet
  tiers · R-SE-ROUND per-line ±$1 · mock rot · recompute-memo pass aliasing.
  Full taxonomy: auto-memory `s54-fullsuite-triage-complete` + STATUS_ARCHIVE s54.
- [x] `[APP]` **TWO live rate-line print bugs fixed (flow 447 green ×2)** —
  2026-07-11 `ca0a710`. Form 8880 L9: the applicable decimal (.50/.20/.10)
  through `_write_row`'s whole-dollar quantize stored/printed "1"/"0" (credit
  math unaffected — the face lied). SC Sch NR L45: `_format`'s PERCENTAGE
  branch was byte-identical to CURRENCY → proration 0.60 printed "1". NEW
  watch-class: DECIMAL/RATIO face lines through whole-dollar writers. Plus
  `import_lacerte_clients` now prefers the `--tax-year` FormDefinition
  (latest-first chained a 2025 TaxYear to a future-year definition).
- [x] `[docs]` REVIEW_QUEUE s54: the stale retirement-spec RET-G5 exception-13
  scenario (13 VALID since the 5329 full unit; engine right; test routed
  around it) → RS amendment folded into the SCH_K renumber trip.

**Session 53 wrap (2026-07-11; "go" — autonomous, the s48/s52 directives):**
- [x] `[APP]` **D2 · 1120-S client formula-mirror retirement** — 2026-07-11
  `7913796` (−2,545 lines). FORMULAS_1120S + val/sumLines + COMPUTED_LINES +
  computeFields() deleted (setReturnWithCompute → setReturnWithOverlay);
  every form paints computed cells from the autosave's ?fresh_return=1
  payload — server FORMULA_REGISTRY is the single screen-math source. The
  parity gate retired WITH the mirror (parity test + S6 fixture +
  dump_client_parity_fixture + the collision-guard test). tsc 0 · vitest 300
  (305 − the 5 retired) · flow untriggered (client-only). Live probe
  (isolated firm, 400-obj cascade): typed I&D → downstream BLANK until the
  flush (no client math), then server-exact paint (1c 45,000 / A8 6,000 /
  21 39,000; salaries round 2 → 20/21 followed, DB ties). Retrospective
  item D fully CLOSED.
- [x] `[APP]` **missing_shareholders_check 1065 fix + siblings** — 2026-07-11
  `2e33d5f`. The s50 false "No partners entered" ERROR dead (counts Partner
  on a 1065); shareholder_ssn_check same-bug silent no-op fixed (iterates
  Partner); shareholder_ownership_check explicitly 1120-S-only (item-J
  three-percentage semantics → REVIEW_QUEUE recommendation for a direct
  partner-pct WARNING). test_diagnostics 41 (+5) · live run_diagnostics
  probe both arms (isolated firm, cascade-deleted).
- [x] `[APP]` **Straggler triage openers (STATUS item 4, partial)** — 2026-07-11
  `2cda054` + `cc849c3`. apr01 fixture rot (fixtures never existed → 11 pass) ·
  TestRenderK1 (⚠ the R-K1-ROUND class: residual-to-LAST makes a SOLE
  sub-100% owner absorb 100% by design — fixed the FIXTURE by adding the 40%
  owner, never the engine → 4 pass). Full-suite rerun in flight at close;
  remaining classes (dominant: ~227 cents-pin occurrences / 40+ files) =
  STATUS item 1.
- [x] `[docs]` **B2-17 → Spine S-20a–d** (8283-entity · 2553 · 2848 · 3115 app
  build) — USABILITY_QUEUE batch 2 fully dispositioned.

**Session 52 wrap (2026-07-11; "go" — autonomous, the s48 standing directive):**
- [x] `[APP]` **B2-15 · Global density pass + B2-11 remainder** — 2026-07-11
  `8c8409d`. Vertical: the shared FFV inputs (Currency/Text/Integer/Percentage/
  select/PYCell) py-1→py-0.5 (30→26px) + FieldRow rows py-1.5→py-1 → seeded
  rows 42→35px (~17% denser), both entities AND the 1040 sections (shared
  components move together). Width offenders found by DOM survey (⚠ ONLY
  after preview_resize 1280 — the pane's native ~351px viewport hid every
  one): Admin cards max-w-3xl (invoice descriptions 839→320 · memo 887→448 ·
  quarterly DATE boxes 767→128) · 1065 partner classification note 746→576.
  Owner grids deliberately left; depreciation edit form rides the input trim,
  further squeeze = Ken's B2-11 feel-check. Autosave+compute round-trip
  verified post-trim (K2 → K_ANALYSIS flowed). tsc 0 · vitest 305 · probes
  cascade-deleted (691 objs).

**Session 51 wrap (2026-07-11; "go" — autonomous, the s48 standing directive):**
- [x] `[APP]` **B2-3 · Manual PY columns on I&D + B2-7b · 8825 PY column** —
  2026-07-11 `baed26c` (mig 0186). Editable "PY 2024" comparison column on every
  entity I&D row (income · COGS · named deductions · meals tiers · free-form ·
  summary) + per-property on the 8825 cards (income + 13 expense lines + legacy
  unclassified); both entities via the shared components. Store = NEW
  `TaxReturn.py_manual_values` (keys `L:<line>` / `R:<propId>:<field>`) —
  deliberately NOT PriorYearReturn: its row-EXISTENCE drives compute L3a
  defaulting (`_default_l3a_from_cogs_db`) and the four create-time
  BOY/M-2/shareholder/officer populates, and the importer owns its
  (entity,year,form_code) row. Writes = row-locked single-key
  `/py-manual/` merge PATCH (per-cell autosaves commute — the s35 race shape
  dead by construction); whole dollars server-side (G-b); no recompute
  (reference-only data). Display: manual GREEN shadows imported YELLOW;
  clearing a key reveals the import — the Ken ruling's "importer/proforma
  auto-fill later, keyed stays overridable" falls out of precedence, no
  future merge migration. PY Compare shows the merged overlay and its edits
  now write the manual store; the old `prior-year/update-line` action
  RETIRED (it mutated importer-owned line_values and 404'd with no import —
  the common case). Flow 447 not triggered (no compute/render change).
  tsc 0 · vitest 305 · NEW test_py_manual 7/7 + adjacent files green · live
  probes both entities (isolated firm, 685-obj cascade delete; the PY-Compare
  edit left importer line_values byte-identical). Boundaries →
  DEFERRAL_AUDIT s51 (no adopt/reconcile UI yet · interest-trend ignores the
  manual store · Sch L keeps its read-only display · 8825 Sch A detail rows
  excluded — no cross-year row identity).

**Session 50 wrap (2026-07-10; "go" — autonomous, the s48 standing directive):**
- [x] `[APP]` **B2-2 · Entity nav form-status markers** — 2026-07-10 `f7bb426`.
  The entity sidebars already called the 1040's `tabStatus`, but it was hardwired
  to 1040 tab ids/rule codes — entity rows never marked. NEW `NavScope`
  ("1040"/"1120S"/"1065") in IndividualNav.tsx: per-form_code rule→tab maps (the
  s49 shared-list lesson) — legacy BUILTIN codes (MATH_*/MISSING_*/INT_*/F4797_*/
  OWNER_*…) + entity D_* families; owner checks split shareholders vs partners;
  entity-4797 findings → Depreciation (B2-14 semantics); 1065 D_K1_→allocations ·
  D_SE_→partners · D_SCHK_/D_L_/D_M*_→their tabs; TB_* deliberately unmapped.
  Entity data predicates + FFV-section fallback over the editors' own tab defs.
  Entity Diagnostics findings became clickable jump-to-tab links (visible-tab
  gated; were 1040-only). Warnings never downgrade a ✓ (probe: a D_8825 warning
  left Rental ✓). vitest 305 (+27) · tsc 0 · live probes both entities
  (isolated firm, 700-obj cascade delete). ⚠ found in passing: server
  `missing_shareholders_check` counts Shareholder rows on a 1065 → the "No
  partners entered" ERROR fires WITH partners present (Cowork task chip
  spawned; queued in tax-app STATUS item 3).
- [x] `[APP]` **B2-5 · Meals one-line reshape (per the Ken ruling)** — 2026-07-10
  `b42e49f`. Six always-on s41 worksheet rows → ONE atomic "Meals" block at the
  same alphabetical list position: a row per tier actually USED (default one 50%
  row), amount = FieldInput (provenance colors) + compact tier select (100 / 80
  DOT / 50 / 0 Ent.; seeded labels ride as tooltips), "+ Add tier", × clears+
  removes, computed DED/NONDED collapsed to a one-line muted hint. A tier switch
  MOVES the amount between the four FFV lines (⚠ first cut passed line NUMBERS
  to the onChange that keys by FormLine id — silent no-op writes; caught live,
  fixed, DB-tied). s41 compute/spec/print/MeF and client FORMULAS/parity
  untouched. Live probe: 10,000@50% + 2,000@100% → hint 7,000/5,000; switch
  100%→80% DOT → 6,600/5,400, DB D_MEALS_DOT=2000/D_MEALS_100='' and line 20
  ties at 6,600; 1065 renders the same block. tsc 0 · vitest 305.

**Session 49 wrap (2026-07-10; "go" — autonomous, the s48 standing directive):**
- [x] `[APP]` **Batch-2 quick sweep: B2-1/6/13/14/16** — 2026-07-10 `11d711a`.
  B2-13 nav spell-outs (Schedule K · Schedule L (Balance Sheet) · Schedule B, both
  entity navs). **B2-14 per the Ken ruling** — Dispositions → **Schedule D (Capital
  Gains)**; entry-path audit FIRST: the entity tab's "Form 4797" option was an
  ORPHANED path feeding NOTHING (entity 4797 rides ONLY the depreciation worksheet
  via aggregate_dispositions/DepreciationAsset.date_sold; compute_4797 +
  Disposition(is_4797) is 1040-only, and NO 1040 tab renders DispositionsSection;
  worksheet Land rows cover business land sales — classify_disposal routes
  zero-depreciation → Part I §1231) → 4797 entry removed, legacy is_4797 rows get a
  red banner + one-click Convert-to-Schedule-D (round-trip verified), the dead
  Form4797ReturnFactsPanel + per-row recapture panel deleted; Like-Kind Exchange
  (8824) rename. **B2-6** I&D footer = Total Deductions + Ordinary Income only,
  per-entity face sets (⚠ the old shared list had the 1065 WRONG — manual lines
  11/12/19/20 sat in the computed footer while the real totals 22/23 were
  alphabetized into the named list; fixed); hidden rollups still flow (footer ties
  live 14,080/35,920). **B2-16 per the Ken ruling** — filing-method trio hidden →
  one "Paper file this return" checkbox writing BOTH method lines + letter.py
  blank-means-e-file (default holds on existing returns). **B2-1 = retrospective
  item F applied** (approved-for-opportunistic): `fieldProvenanceClass` — GREEN
  typed / YELLOW system-supplied / neutral blank on every FFV input (FieldInput
  family + M-2 grid + tax-lines card; BooleanField keeps its s42 amber chips); the
  amber "Manual override" dot REMOVED (it rendered only in FieldRow and survived
  value deletion = both Whit complaints). Display-only — is_overridden semantics
  untouched (the L3a override-to-blank hold intact); the stale FLAG-on-blank class +
  an entity is_4797 server diagnostic filed (DEFERRAL_AUDIT s49). Gates: tsc 0 ·
  vitest 278 · flow 447 + print packages (470 passed) · live browser probe
  (isolated firm PROBE-B2SWEEP-UI, 685-obj cascade delete; ⚠ Browser-pane
  screenshots time out this machine — DOM-assertion proofs).

**Session 48 wrap (2026-07-10; "go" — the batch-2 8825 group):**
- [x] `[RS+APP]` **8825 Rev-12-2025 unit COMPLETE (B2-7a + B2-8 + B2-9), spec-first** —
  2026-07-10 RS `c4c94bc` / tts `a4435f4`. RS: renumbered verbatim vs f8825.pdf +
  i8825.pdf Rev. 12-2025 (both fetched 2026-07-10) + IRS8825.xsd 2025v6.2 — 34 facts
  (structured address · property_type/other_info_code choice lists · schedule_a_category)
  · R001-R007 (R005 2c = 2a+2b NEW · R006 Schedule A (Form 8825) M-3-conditional verbatim
  · R007 col (c) M-3-only Note) · 58-line map incl. Schedule A A1-A31 · D001-D005 ·
  5 scenarios · 8 re-fetched verbatim excerpts (2 stale 2026-03-18 excerpts deleted) ·
  in-loader stale fact/line DELETE · **SCOPED prod seeder** (`scratchpad/
  seed_8825_renumber.py` — the full load_remaining_1120s would stomp 4562 R011 owned by
  the later carryover amend loader) · SQLite validation harness 123 checks · deployed
  export verified · tts mirror refreshed. tts: migs 0184+0185 (RLS default-deny) ·
  `line17_other_deductions` single source (model = MeF = print) · aggregate 20a = Σ 2c
  (FA008 gross-pin healed inline) · the four print-bug fixes + a line-1 column x-order
  pin (the s45 y-band class, applied as x-bands) · f8825sa manifest/map/append-per-4-
  properties · MeF 2b/2c/13/1(c) elements + linked GeneralDependencyMedium statements
  (reconcile-or-refuse; grep-Dependencies-first honored — no declared Schedule A doc
  exists in 2025v6.2) · D_8825_001-005 code-registered + seeded · nested other-deductions
  CRUD through the recompute chokepoint · UI card rebuild. Deferrals (DEFERRAL_AUDIT
  s48): 8825 L21/L22a input path + rental-4797 auto-split · col (c) description
  statement · Sch E detail-rows UI · B2-7b rides B2-3. Batch-1 ledger: s38 shipped 1/4/7/13/13b/15 + the parity gate + L3a RS `R008`; s39 shipped 11 · 2 · 3 · 5 + the dead "+ Add" fix (mig 0181); s40 shipped 6 · 8+14 · G-a; s41 shipped 9 — the M&E four-tier unit (tts `d8dbeba` / RS `96931bf`); s42 shipped 12 — the Sch B Q11 auto-answer, spec-first (tts `94380af` / RS `b7907bc`; RS 1120S_SCHB renumbered to the verified 2025 face — the old block was FABRICATED pre-face numbering — + NEW R006 derived/YELLOW/overridable; 3 REVIEW_QUEUE rulings filed); **s43 shipped 16 — the CC retrospective (`docs/RETROSPECTIVE_SCORP_2026-07.md`, analysis only): 7 keeps · 6 prioritized proposals (A local pytest-only Postgres [kills the pooler test tax] · B RS early-era face-audit sweep [SCHB = 3rd confirmed drift; do BEFORE the S-15/1120 authoring waves] · C race-class + bare-key maintenance pass · D client-formula-mirror retirement trial [the parity-gate root cause] · E FormEditor 18k-line split · F provenance color primitive) · 3 explicit rejects (FFV re-typing · compute_return restructure · grid libs) · a Part-4 decision table — awaits Ken's discussion (REVIEW_QUEUE)**. Item 10 stays Ken-gated (Lacerte reprint of file 1018); batch 2 arrives from Ken. The 1120-S ATS completion lane stays Ken-gated: **e-services email OUT 2026-07-09** (S7/S8 ruling · 8941 key-inversion [Ken RATIFIED the finding — engine right, key wrong] · 1040 production flip · SOR re-request) → any S7/S8 builds → full-set bundle → the live upload loop. A2A (17g): IdenTrust packet OVERNIGHTED 2026-07-09, cert vetting ~7-14 days (⚠ 30-day download clock from notarization); AE values pre-agreed in `docs/mef/A2A_ENROLLMENT.md`. S-18: PWA CODE-COMPLETE (Ken install-verify pending) · 18f prefs DONE · 18a/18b/18g queued · 18d parked.

**Session 47 wrap (2026-07-10; "go" — the s46 stated follow-on):**
- [x] `[RS+APP]` **§168(k)(7) bonus opt-out election unit COMPLETE, spec-first** —
  2026-07-10 RS `fdeadfb` / tts s47. RS: NEW R009 (i4562 2025 p.7 verbatim — per-class,
  ALL property in the class, statement mechanics, 301.9100-2 cure, irrevocable,
  per-entity; four engine effects) · NEW facts bonus_eligible + bonus_electout_classes ·
  D008 conflict-error + D009 allowed-or-allowable warning · 3 election scenarios ·
  **R007 CORRECTED (tax-law fix): the 150DB AMT recompute keys on ELIGIBILITY, never
  claiming — elected-out post-2015 property has NO AMT adjustment (i6251 2l verbatim;
  (k)(7) switches off only paras (1)/(2)(F) so the (2)(G) exemption survives); the s46
  arm re-engaged it — REVERSED, Ken ratification pending (REVIEW_QUEUE s47)**. tts:
  mig 0183 (shared DB) · engine is_elected_out (class token + PIS-year gate) forces
  bonus 0 → Table 2 + GA add-back 0 follow · derive_amt_method(bonus_eligible) ·
  /info/ PATCH whitelists + RECOMPUTES · D_4562_ELECTBONUS/ELECTGAP seeded · print
  "Sec. 168(k)(7) Election Statement" page after the 4562 · MeF
  `ElectNotClaimSpclDeprecAllwnc` (the DECLARED ReturnData ref-1921 doc,
  position-pinned; 1065 MeF = future-mapper deferral) · UI 7-class checkbox card +
  per-asset Bonus-Eligible checkbox. Gates: engine 63 (+8) · flow 447 · MeF 69 (+3) ·
  render 25 (+2) · S5+S6 8/8 · tsc 0/vitest 278 · ORM probe 7-step lifecycle +
  browser probe toggle-to-paint (both isolated, cascade-deleted). Deferrals (s47):
  proforma year-shift bonus_pct zeroing · the 40%-instead-of-100% transitional
  election (mechanics spec-silent) · software class token.

**Session 46 wrap (2026-07-10; Ken delivered the Lacerte method list):**
- [x] `[RS+APP]` **Depreciation-methods unit COMPLETE, spec-first** — 2026-07-10 RS
  `d5e4386` / tts `5539168`. RS: 4562 `depreciation_method` → Ken's 7 codes (200DB/150DB/
  SL/SL_RES/SL_NONRES/ADS_SL/NONE; passenger auto = CLASSIFICATION never a method, Ken
  ruling) · `recovery_period` += 30/31.5-legacy/40 · NEW facts vehicle_classification +
  amt_method · NEW R007 AMT post-1998 matrix (i6251 2l verbatim: 150DB same-life ONLY for
  200DB-no-bonus; bonus-claimed = NO adjustment per §168(k)(2)(G)) · NEW R008 vehicle caps
  (RP 2025-16 T1/T2 · §179(b)(5)(A) $31,300 · 6-ft-bed exception) · R003 = Chart 1/2
  published-table routing + refuse-on-unmapped (D007) · IRS_PUB_946 +6 verbatim table
  excerpts, NEW sources IRS_RP_2025_16/IRS_RP_2024_40/IRC_280F · D005/D006/D007 · 9
  published-value scenarios. tts: **TWO LIVE ENGINE BUG CLASSES fixed against the verbatim
  tables — the whole 200DB MQ dict was derived-wrong (Q4 5-yr summed 99.00%) → published
  A-2..A-5 all six lives; 150DB 10-yr SL switch a year late → A-14; luxury-auto constants
  wrong ($19,500/$12,400 vs published $19,600/$12,200) AND miscited RP 2025-13 (the §831(b)
  micro-captive proc — real source = RP 2025-16)** · NEW 150DB MQ (A-15..A-18) + SL/MM
  31.5/ADS-30/ADS-40 tables + month-aware MM final year · derive_amt_method + serializer
  amt_method_derived (YELLOW auto/override) · SUV §179 clamp + classification-gated §280F ·
  mig 0182 (vehicle_classification) applied · 4562 print Section C rows + MeF ADS refuse
  seam · D_4562_VCLASS/SUV179/METHOD seeded · UI: 7-method + 11-life + 3-classification
  dropdowns + AMT Auto label. Gates: engine 55 (21 new published pins) · flow 447 ·
  render 23 · MeF 66 · S5+S6 8/8 · tsc 0/vitest 278 · ORM + browser probes green
  (cascade-deleted). Deferrals (DEFERRAL_AUDIT s46): **§168(k)(7) election unit = NEXT**
  (statement doc, GA add-back kill, 280F T2 flip, AMT re-engagement) · ADS MeF leg
  (refuse seam) · 280F on AMT/state parallels · UOP/ACRS = Ken-RULED OUT same day (skip).

**Session 45 wrap, extended (2026-07-10; Ken delivered batch 2 + "Work on the UI cluster"):**
- [x] `[APP]` **W2G taxpayer-relation guard** — 2026-07-10 `1125252`. Full-suite run
  complete (36 min, 5,147/169/6): root cause #1 = compute_w2g_db read
  `tax_return.taxpayer` unguarded (getattr guarded the ATTRIBUTE, the relation raises
  first) → 500 on EVERY recompute endpoint for a return with no Taxpayer row (~25
  failures cleared; the compute_1116 convention). Remaining classes diagnosed
  (s27 cents pins dominant · fixture rot · pipeline pins · ⚠ flow ×2 in-suite-only) —
  triage queue in tax-app STATUS.
- [x] `[APP]` **Batch-2 depreciation cluster (items 4/10/11/12)** — 2026-07-10 `8445ecc`.
  Depreciation tab regrouped by ACTIVITY (Business → Page 1 L14/16 · per-8825-property
  cards · Sched F · ⚠ unassigned-8825 card; whole-dollar flow totals → destinations);
  "Property/Location Group" (`property_label`) input retired; edit form densified;
  page-1 depreciation + fed 8825 cards render Calculated/readOnly while a worksheet
  exists (the engine stomps them every recompute — the editable boxes were lies).
  Live probe (isolated firm; L14 10,003 / rental 8,751; cascade-deleted 363 objs);
  tsc 0 · vitest 278 · parity untouched (L14 has no mirror formula). 1120-S+1065 shared.
  Batch-2 intake: 17 items triaged + 4 Ken rulings (meals one-line+add-rows · PY manual-
  now-import-later · Dispositions → "Schedule D (Capital Gains)", 4797 rides the
  worksheet · filing fields hide+fallback). **NEW Ken-directed unit teed up: the
  depreciation-methods RS spec session (10-20 methods + §168(k)(7) opt-out election +
  siblings) — gated on Ken's Lacerte method list; then the engine leg (published-table
  pins only); B2-17 adds 8283-entity/2553/2848/3115 to the Shelf.**

**Session 45 wrap (2026-07-10; "go" — the s44 renumber queue, unit #1):**
- [x] `[RS+APP]` **4562 renumbered to the 2025 face (renumber unit #1) + the live print-row
  fix** — 2026-07-10 RS `e695c1a` / tts `4951f41`. RS: `_load_form_4562` rebuilt verbatim
  vs f4562.pdf 2025 + IRS4562.xsd 2025v6.2 (26 → 60 line rows — full face incl. Section C
  20a-20e, Part IV 23a/23b, Part V 24c aircraft, Part VI 42-44; lines 6/7/8 fabrications
  fixed; lines 10-13 mirror the Ken-approved carryover loader verbatim; recovery_period
  +50; in-loader stale-row DELETE; re-letter scenario pin); Supabase-seeded, deployed
  export verified, tts mirror refreshed. tts: **the 2025 face's NEW 19h 50-year row had
  shifted residential→19i/nonres→19j, and the AcroForm row groups follow the FACE letters
  — the field map was printing residential in the 50-year row** → MACRS_LINE_MAP
  re-lettered (27.5→i, 39→j, +50→h), field map re-keyed + NEW L19j, MeF _GDS_ELEMENTS
  re-lettered + GDS50YearPropertyGrp (required-PIS refuse seam); MeF XML residential/nonres
  UNCHANGED (semantic elements — S5/S6 byte-stable). Gates: MeF 66 (+2) · flow 447 ·
  4562 render 35 (incl. the NEW y-band row-placement pin — the class guard for face
  re-letters) · S5+S6 8/8 (parallel scratch-DB recipe: TEST_DB_TEST_NAME). ⚠ deferral:
  SL/MM 50-yr pct table unsupported (silent 0%, pre-existing; DEFERRAL_AUDIT). The
  first-ever full-suite run still in flight at close (~43%; straggler triage = next
  session's first action; that run holds pre-s45 modules).

**Session 44 wrap (2026-07-09; Ken: "Let's implement your proposals and I agree on D"):**
- [x] `[APP]` **Retrospective item A — pytest DB moved to LOCAL PostgreSQL 17** — 2026-07-09.
  winget install + `config/settings/test.py` → localhost (dev/prod unchanged on Supabase;
  TEST_DB_* env-overridable). Proof: the S5+S6 scenario pair 343s → 47s inside a FOUR-file
  batch; the batch cap is dead; first-ever full-suite run kicked. Immediately caught 6
  stale test_returns pins (seed counts 338/285 → 352/313 from s41; trust→400 from the
  pre-1041 era → now asserts a 1041 creates) — the file had been too expensive to run.
  DECISIONS.md records the standard.
- [x] `[APP]` **Retrospective item C — the lost-update class killed at one point** — 2026-07-09.
  NEW `BaseModelSerializer` (68 serializers re-parented): PARTIAL updates save only the
  patched columns (update_fields + auto_now stamps; property-writes fall back to full save)
  → concurrent per-field autosaves commute, no locking needed; regression test proves the
  stale-instance clobber shape is dead. entity-boundary PATCH row-locked (the 8941 recipe).
  Bare-key audit (subagent ledger): ZERO live hazards — all 1040-family reads scoped; two
  latent sites hardened (interest-trend .get(); 8879-S/8453-S renderer reads).
- [x] `[RS+APP]` **Retrospective item B — early-era face audit + the M-3 $10M tax-law fix** —
  2026-07-09 RS `c374e89`. Two audit agents swept every Session-11-vintage 1120-S-family
  block vs the 2025 faces (ledger: `docs/rs_handoff/2026-07-09_early_era_face_audit.md`).
  SCHD CLEAN (s34 renumber held) · SCHL internally-contradictory · SCH_K worst (fabricated
  13f FTC row; missing 17c AE&P; wrong L18 formula) · K1 code-tables wrong · **4562 missed
  the 2025 face's new 19h 50-year row (19i/19j shift)** · 6198/3800 fabricated · M3
  face-not-in-repo **+ a REAL tax-law error: M-3 threshold $50M vs the law's $10M (i1120s
  2025 p.49 verbatim ×2) — FIXED everywhere (incl. yesterday's SCHB R003 which propagated
  it), seeded, export-verified; tts engine was already correct at $10M.** Renumber queue
  filed: 4562 → SCH_K → K1 → SCHL → 6198 → M3-line_map → 3800.
- [x] `[APP]` **Retrospective item D — the D1 trial on GA-600S** — 2026-07-09. FORMULAS_GA600S
  + the GA net-worth bracket table DELETED from the client (the only copy of that table now
  lives in server compute.py); GA-600S rides the ?fresh_return=1 payload. Live probe: PATCH
  → fresh payload → screen paints server values (S3_4/S3_6 = 500,000 · S3_7 = 250 to the
  bracket table); probe cascade-deleted. **Ken's typing-feel check = the gate** before the
  1120-S mirror (the big one) goes the same way. E/F recorded approved-for-opportunistic.

**Session 43 wrap (2026-07-09; "go" — S-19 batch 1 item 16, the LAST batch-1 item):**
- [x] `[APP]` **CC retrospective (usability item 16) — analysis only, batch 1 CLOSED** —
  2026-07-09. `docs/RETROSPECTIVE_SCORP_2026-07.md`: evidence-grounded review of 4 months
  of S-Corp/entity work (measured the pain: FormEditor.tsx 18,038 lines carrying the ~249-
  tuple TS duplicate of the Python formula registry · 351 test files vs the 2-3-file pooler
  batch cap). 7 keeps · 6 proposals (A pytest-only local Postgres · B RS face-audit sweep ·
  C race+bare-key maintenance pass · D formula-mirror retirement · E FormEditor split ·
  F provenance primitive) · 3 rejects recorded · decision table for Ken. Nothing
  implemented; REVIEW_QUEUE carries the ask (green-light A/B/C recommended; B before the
  next RS authoring wave). No gates touched (no code).

**Session 42 wrap (2026-07-09; "go" — S-19 batch 1 item 12):**
- [x] `[RS+APP]` **Schedule B Q11 auto-answer (usability item 12), spec-first** — 2026-07-09
  RS `b7907bc` / tts `94380af`. RS: 1120S_SCHB rebuilt verbatim vs f1120s.pdf 2025 pp.2-3
  (B1-B17 line map incl. B12_amount/B15_amount; 28 face-keyed facts; 7 stale lines + 20
  stale facts DELETED in-loader — the F4797-G2 recipe; practice rules kept re-keyed and
  labeled non-face) + NEW R006 Q11 auto-answer (derivation formula, the Sch-L/M-1-unchanged
  boundary, positive-only interpretive note, TY2026 re-verify note) + the verbatim face/
  instruction excerpt pair + scenarios (Yes 180k/90k · strict-threshold No 249,999/250,000);
  Supabase-seeded, deployed export verified, tts mirror cached NEW
  `server/specs/1120s_schb_spec.json`. tts: `_auto_answer_b11_db` post-formula-pass
  (receipts = p1 1a/4/5 + K3/K4/K5a/K6 + positive-only K7/K8a/K9/K10 + Σ 8825 gross rents
  + legacy L21/L22a; assets = computed L15d; `_set_field_value` respects overrides) ·
  ScheduleBSection amber "Auto" pill + BooleanField `derived` amber chips (YELLOW tier;
  click → is_overridden GREEN permanent). Gates: 5 new DB tests · flow 447 · MeF 1120-S 64
  · S5+S6 8/8 · parity fixture regenerated BYTE-IDENTICAL · vitest 278 · tsc 0 · live
  probe (derived true → UI override → recompute-safe), 359-row cascade delete. ⚠ K3a
  gross not captured (net K3 substitutes — understatement edge) → REVIEW_QUEUE with the
  1065 Sch B Q4 sibling ruling + the SCHB renumber ratification.

**Session 41 wrap (2026-07-09; "go" — S-19 batch 1 item 9):**
- [x] `[RS+APP]` **M&E four-tier worksheet unit (usability item 9), spec-first** — 2026-07-09
  RS `96931bf` / tts `d8dbeba`. RS: 1120S_PAGE1 R009 (DED = 1.00·100pct + 0.80·DOT +
  0.50·std; Line-19 component) + R010 (NONDED = 0.50/0.20/1.00 → K16c/M-1 3b/M-2 5a) +
  D004 exception-only warning + four-tier scenario (7,800/8,200); 1065_PAGE1
  R-1065P1-MEALS/-MEALSND twins (→ K18c/M-1 4b) + D_1065P1_MEALS100 + P1-5; **NEW source
  IRS_2025_PUB463** (Pub 463 2025 ch. 2 VERBATIM: the six §274(n)(2)(A)/(e) 100%
  categories + "The percentage is 80%"); i1120s/i1065 Special-Rules excerpts; IRC_274
  secondary links; Supabase-seeded, deployed exports verified, tts mirrors cached.
  tts: NEW `D_MEALS_100` both entity seeds · compute D_MEALS_DED gains the 100% addend
  (1120-S + 1065) · free-form rows → D_FREE17 both entities (19/21 sums + MeF `_d19_rows`
  mirror) · UI worksheet inline in the deductions list (100/80/50/0 + the two computed
  YELLOW math rows; never splits columns; old sub-section removed) · print M&E statement
  gains the 100% section · MEALS_NO_LIMITATION covers the tier · seeders re-run
  (352/313 lines). Gates: flow 447 · parity fixture REGENERATED + vitest 278 · MeF
  1120-S 64 · new pure pins both entities · live probe verify (19=7800/K16c=8200/
  M1_3b=8200), probe cascade-deleted. ⚠ TY2026 WATCH in R009: §274(o) applies to
  amounts paid after 12/31/2025.

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
- [x] in-app bug reporter (WORK_ORDER_bug_reporting; Ken-directed off-order 2026-07-11) — 2026-07-11 `40a50fd`:
  "Report a Bug" header button + Help menu item, auto-context capture (route/form/section/return-id/
  versions/recent-errors ring buffer), `bug_reports` Supabase table (RLS default-deny, migs bugs
  0001/0002), firm-scoped `/api/v1/bug-reports/` (no destroy — candidates keep history), `manage.py bugs`
  triage command + `/bugs` session-start slash command. Screenshots DEFERRED (screenshot_ref null).
  Guardrails encoded in /bugs: reports = candidates (Ken triages); computation-touching fixes re-run
  regression before merge. 10 server tests + probe-verified live (201 → row → triage → close).

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

**S-19 · [APP] ∥ · PREPARER USABILITY QUEUE (opened 2026-07-09, Ken-directed — "start
hammering away on cosmetic/usability fixes").** Sources: **Whit's 1040 list + Whit's
2-page 1120-S list + Ken's own items** — intake: Ken pastes text or photographs the pages;
CC triages into `USABILITY_QUEUE.md` (tax-app root) with per-item status, then works items
one at a time (live-verify each; commit per item or small batch). Guardrails: the
RED/YELLOW/GREEN data-entry color semantics are untouchable; incremental changes only, no
redesigns; anything that turns out to be a COMPUTE issue gets promoted out of this queue
into a proper form unit (spec-first). Interleaves freely; drops when the ATS lane unblocks.
- [ ] 19-intake · receive + triage Whit's 1040 list → USABILITY_QUEUE.md
- [ ] 19-intake · receive + triage Whit's 1120-S list (2 pages) → USABILITY_QUEUE.md
- [ ] 19-intake · Ken's own items (incl. the landing-page cosmetics he mentioned at 18f)

**S-20 · [RS→APP] · B2-17 FORM UNITS (promoted from USABILITY_QUEUE batch 2, s53
2026-07-11 — "new forms wanted"; form units, not usability items).** Each is its own
spec-first unit; sequence after the SCH_K renumber queue unless Ken reorders.
- [x] **20a · 8283 entity leg** — 2026-07-12 (s65 RS `8b6faca` + s66 tts). UNIT COMPLETE
  both legs (K12b feeder · 1065 print+diagnose · entity render/packet/MeF IRS8283 ·
  D_SCHK_8283 retired · FA-ENT-8283-01/02 activated; flow 463). *(Tick was missed at the
  s66 close — recorded s70.)*
- [x] **20b · Form 2553** — 2026-07-12 (s67 RS + s69 tts). RS: WO-26 drafted → Ken Gate-1
  APPROVED live → seeded + export verified + mirror cached. tts: the print unit COMPLETE —
  input model + consent/QSST rows + the §1362(b) window calculator + D_2553_* ×19 +
  AcroForm render (overflow copies + the 2013-30 margin legend) + packet-on-attach +
  FA-2553-WINDOW/COUNT/8832 activated with runners (flow 475).
- [x] **20c · Form 2848** — 2026-07-12 (s68 RS + s69 tts). RS: WO-27 drafted → Ken Gate-1
  APPROVED live → seeded + export verified + mirror cached. tts: the print unit COMPLETE —
  input model + rep×4/matter×3 rows + **the L2 preparer autofill value-add (Preparer +
  caf_number/fax)** + future-clock/sign-window/modified-CAF helpers + D_2848_* ×17 +
  AcroForm render + 1040+entity mounts + FA-2848-FUTURE/SIGN45/CAFFILL activated with
  runners (flow 475).
- [x] **20d · 3115 app build** — 2026-07-12 (s70). UNIT COMPLETE — print-only v1 per the
  Gate-1 D-25 scope: Form3115 singleton (migs 0191-0193) + the §481(a) helpers pinned to
  the 7 spec oracles (catch-up / Sch-A netting / §7.03 period / spread / DCN 7) +
  D_3115_* ×8 + f3115 Rev. 12-2022 AcroForm render (Sch E L4a/L7 = Statement pages — no
  face fields) + packet-on-attach (defaults ON) + entity/1040 mounts +
  FA-3115-CATCHUP/SPREAD/SCHA activated with runners (flow 484). **S-20 fully DONE.**

**S-21 · [APP] · RATIFICATION-SWEEP UNITS (unblocked by Ken's s71 live sweep, 2026-07-13 —
all 12 REVIEW_QUEUE calls answered; the queue is EMPTY).** Three build units authorized:
- [x] **21a · 1120-S M-2 NNA-cap compute leg + face column rotation** — 2026-07-13
  (s71 third act, mig 0194). M2_7a computes under the ratified R002 cap
  (§1368(e)(1)(C); the published pp.50-51 example + divergence pin = test oracles in
  test_1120s_spec). The fold-in FOUND A LIVE PRINT BUG: the legacy letters
  (b=OAA/c=STPI/d=AE&P) fed a POSITIONAL AcroForm map, so every b/c/d M-2 value
  printed one face column off — keys rotated to the face (b=PTEP/c=AE&P/d=OAA;
  FormLine rename-in-place + JSON stores + MappingRule + PY m2_balances), K17c
  derive → M2_7c, MeF builder re-keyed, client grid = face headers. Gates: 636+137
  suites · tsc 0 · vitest 300 · ORM 11/11 · browser + PRINT probes GREEN.
  Boundaries → DEFERRAL_AUDIT s71 (cascade auto-split · Lacerte col-a · R005/R006).
- [ ] **21b · 1065 partner-percentage diagnostic** — small code-registered unit: WARNING
  when profit_pct or loss_pct over ACTIVE partners doesn't sum to 100 (capital_pct stays
  informational — D_K1_CAPPCT covers it). Own unit with DB tests (the s53 recommendation,
  ratified verbatim).
- [ ] **21c · 1065 Schedule B Q4 auto-answer** — the approved Q11 recipe clone:
  derived-YELLOW/overridable; (a) receipts < $250K + (b) assets < $1M from return data,
  (c) K-1s-filed-timely PRESUMED TRUE at prep time (Ken-ratified presumption), (d) from
  the M-3 flag. Yes waives Sch L/M-1/M-2/item L — same waiver wiring as the 1120-S
  sibling. Spec-first (RS 1065 SCH_B block).

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
- **Marketing video (conference ~60s cut)** `[APP/EXT]` ← spec v2 FILED 2026-07-12 at
  `tts-tax-app/marketing/MARKETING_VIDEO.md` (Ken delivered; no v1 was ever committed —
  this starts the file's history). UNBLOCK (all three, per the spec): (a) **brand name
  final** — ⚠ the script says working-name "Sherpa" but the DBA on record is **Delvio
  Tax** (delviotax.com; the PWA already ships as "Delvio Tax") — Ken's call re-records
  every product-naming line; (b) demo screens conference-ready; (c) the regression bed
  backs the "to the dollar" claim (see the Regression bed Shelf item above). Natural
  window: post-freeze December polish or post-season. *Not season-critical.*

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
