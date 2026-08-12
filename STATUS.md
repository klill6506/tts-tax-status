# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-12 (s245b). **✅ BATCH-001 #10 BUILT — the Form
1099-Q unit (Coverdell ESA / QTP), end to end; EVERY BUILDABLE BATCH-001
ITEM IS NOW CLOSED** (#5 ⛔ KEN-parked, #4 NOL-parked on the RS agenda —
the batch file stays in the queue carrying both). The unit: `Form1099Q`
model (migs 0313 + RLS 0314), `compute_1099q` (one classifier shared by
compute/diagnostics/render; the Pub 970 (2025) worked examples are the
pinned answer key — $18 / $735 / $25 incl. the Coverdell FMV derivation
with the Pub's whole-dollar step rounding), Σ taxable riding the COMPOSED
Schedule 1 line 8z as the sixth contributor ("QTP"/"Coverdell ESA"
literals; removal reflows), six D_1099Q_* rules (the 5329 Part II
reconciliation respects Ken's keyed-leaves ruling — never feeds), a
distribution-worksheet statement page, the `q_1099s` lane (all six
registries + generator) and full browser CRUD. Source of authority:
`server/specs/_1099q_source_brief.md` (no RS spec exists for any
information return — s222). The reported packet's $2,625/$2,625
fully-covered shape computes ZERO tax without needing earnings, survives
the face tie, and prints its facts. Post-deploy commands ALREADY RUN
against the shared DB: `migrate` (0313/0314, additive) + `seed_rules`.
Earlier today: s245 (#2 verified-built, #5 ⛔), s244 (#6 8862
multi-category, LIVE), s243b (Ken's four-return unblock, LIVE — Codex
can rerun the four production dry-runs), s243 (#8).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax`; demo = `tts-tax-demo`. **The orphan third service
`tts-tax-app` still fails every push — Ken should delete it.**

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

## ⚠ KEN IS AWAY 2026-08-09 → ~2026-08-19 (10 days)
**Availability MINIMAL BUT NOT ZERO.** Batch questions; keep them low-friction.
Nothing is on a clock in that window; the next hard deadline is 2026-09-15.

---

## ▶ RESUME HERE

### ✅ s244 COMPLETE — BATCH-001 #6, the Form 8862 multi-category expansion
Design record: the s244 annex in `CC_CODE_CHANGES_1040_BATCH-001.md`.
Load-bearing facts a future session needs:
- **The category predicate** (claimed ∧ previously disallowed) is shared
  verbatim by `_f8862_engagement` (builder), `_f8862_categories`
  (rules_eic), and render_8862's per-Part gates. "CTC claimed" includes
  **Schedule 8812 line 12** (`L_12` in the DB) — a line-19-zeroed,
  ACTC-opted-out return still claims; ATS Scenario 5's answer key pins
  that shape and its fixture now states all three disallowances.
- D_8862_002 stays EIC-scoped + ENGAGEMENT-gated on purpose (claimed-
  gating would be circular: the missing facts it names are what zero the
  credit). Parts III/IV have nothing keyed to be missing — their
  shortfall is D_8862_006 (category unanswered) + D_8862_004/005.
- A keyed 8862 with NO resolving category REFUSES at composition by
  name; a claimed credit whose flag is None never transmits its
  box/part (unanswered ≠ No ≠ Yes).
- The back-entry `taxpayer` lane also gained
  `eic_qualifying_child_of_another` / `eic_claimed_as_dependent` — the
  form_8862s refusal message directed packets there while the lane
  refused the keys (a childless-EIC 8862 was unimportable end-to-end).
- Housekeeping: `test_non_engaged_return_leaves_27a_quiet` (the s235
  known-red) was a STALE pre-Leg-C pin contradicting
  `test_leg_c_eic_autocompute` — proven pre-existing via worktree at
  `bb282b0`, updated to an out-of-band DOB. The known-red list shrinks.
- Regression home: `server/tests/test_8862_multicategory.py` (24) + the
  CTC-only render test in `test_topic7_render_leg.py`.

### ✅ s243b (earlier today) — Ken's four-return unblock (`bb282b0`, LIVE)
**Codex can rerun the four production dry-runs.** Verdicts: Return W /
T / G tie (G's payload must supply the new DIS-DATE/TYPE via
`ga500_fields` — D_GA500_7C_DETAILS demands them); **Return P NO TIE
by design — refuted as an app defect** (the FILED Schedule D face
doesn't foot by exactly $2,229; source-side gap, waiting on Ken/Codex).
`seed_ga500` (157 lines) + `seed_form_2441` (23 lines) already run.
Regression home: `server/tests/test_four_return_unblock_s243b.py`.

### ✅ s245 — BATCH-001 #2 closed (verified built), #5 ⛔ KEN (parked)
- **#2**: every ask resolves to the s235 layer; the reported shape is now
  pinned (`test_8582_amt_carryover.py`, 6 — regular computes 27,960 per
  activity while the AMT pool holds 19,490; both roll independently; the
  worksheet prints; D_CFWD_001 errors). The "computed next-year AMT"
  refigure needs FORM_8582 AMT rules that DO NOT EXIST (zero AMT content
  in the export) → RS agenda. Design record: the s245 batch annex.
- **#5**: asks to build the nonpassive-rental routing Ken's 2026-06-13
  8582 kickoff scope decision RED-deferred (`D_8582_RE_PRO`). ⛔ KEN —
  batched in REVIEW_QUEUE with a build recommendation. (The BUILD_ORDER
  note's "2026-07-06" date was loose; the ruling on record is the
  kickoff's scope decision 3, STATUS_ARCHIVE.)

### ✅ s245b — BATCH-001 #10 built (the 1099-Q unit). Design record:
the s245b batch annex + `server/specs/_1099q_source_brief.md` +
`server/tests/test_1099q_unit.py` (24). Load-bearing facts:
- ONE classifier (`compute_1099q.classify_row`) drives compute,
  diagnostics and the worksheet: skip / covered / computed /
  refuse_partial / refuse_underivable. A COVERED row (AQEE ≥ gross) is
  tax-free WITHOUT earnings — box 2 blank must not refuse there.
- The 8z share rides `compute_1099misc_db` (sixth contributor, the 8814
  block pattern); `form_1099q_8z_share` also persists each row's
  `computed_taxable_earnings` (never importable).
- 5329 Part II line 5 = `Form5329.edu_able_dist` (the MODEL field; the
  `f5329_line5_*` names are compute-INPUT keys — a rule reading those
  off the model AttributeErrors, caught by the quiet-case test).
- The Pub rounds EVERY derivation step to whole dollars.

### ⭐ NEXT UNIT — the queue's next posted batch, or (if none) the
highest-value RS-agenda authoring (the NOL spec blocks BATCH-001 #4 +
BATCH-002's computes — but RS authoring is Rule Studio work, check
whether the loop's charter covers it before starting). BATCH-002's
non-NOL residue is closed; the 1120-S and legacy queues are unchanged.

### ⚠ s241's Form 5329 cross-check — still waiting on Sections A/B
Form 5329 line 36 takes "Form 8853 line 8" (Archer MSA — Section A/B
territory). The s242u model is Section C only; the line-36 cross-check
stays unbuilt until an A/B carrier exists (parked with Ken's s224
keyed-only ruling; revisit only on his direction).

### ✅ THE E-FILE GAP LIST IS EMPTY (as of s242z)
`IRS4797` (s242c) · `IRS8853` (s242x) · `IRS1116` (s242y) · amended MeF
(s242z). What remains refused at composition is NAMED per-case, never a
missing builder.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — every buildable item
  CLOSED** (#10 ✅ s245b, #2 ✅ s245, #6 ✅ s244, #8 ✅ s243,
  #1/#3/#7/#9 earlier); #4 NOL-parked (RS agenda), #5 ⛔ KEN — the batch
  file stays in the queue carrying those two. **BATCH-002 — open as to
  the NOL-blocked COMPUTES only**; **BATCH-003/004/005 — ✅ DONE,
  moved.** Every worked file carries a result annex; read it first.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of
  10; #10 multi-state parked under the states-on-hold ruling). Unchanged.

---

## ⚠ Classes that MOVE existing returns or output on next recompute
- **s245b: NONE beyond new-row reach.** The 8z composition gains its
  sixth share only when a `Form1099Q` row exists (none do); the share is
  0 on every existing return by construction, and the worksheet renders
  only when rows exist. Migrations 0313/0314 additive.
- **⚠ s244 MOVES E-FILE OUTPUT + PRINT on the 8862 class (all
  corrections toward i8862):** (1) a return transmitting an 8862 whose
  CTC/AOTC boxes rode claims alone now transmits them ONLY when the
  category's flag is True — unanswered flags drop the box/part and
  D_8862_006 warns; (2) a keyed 8862 with no resolving category now
  REFUSES composition by name (was: transmitted per claims); (3) a
  CTC/AOTC-only recert now renders/attaches an 8862 it previously
  omitted entirely; (4) 8867 line 7a now answers "true" on a CTC/AOTC
  disallowance. No dollar-line movement anywhere.
- **⚠⚠ s243b MOVES THREE CLASSES (each a correction):** (1) basis-only
  8606 + IRA-path 1099-Rs — 4b regains box-2a taxable (AGI, SS
  worksheet, GA RIE L11 move); (2) employer DCB with expenses below the
  plan cap — the 2441 line-17 cap raises 1e/1z/AGI (zero expenses → all
  benefits taxable); (3) GA under-62 disability RIE prints on S1 7c/7f
  with date/type; missing details fire D_GA500_7C_DETAILS.
- **⚠ s243 MOVES GEORGIA RETURNS carrying a federal 2441 credit** (the
  IND-CR 202 feed; line 20 gains the credit — a correction).
- **⚠ s242z/y/x MOVE E-FILE OUTPUT** on the amended / full-1116 / keyed-8e
  classes respectively (compose-or-named-refusal changes; no compute
  movement). s242q moves the stale-line-7 disengage class (correction).
- **⚠ s241o MOVES GEORGIA RETURNS carrying a 1099-PATR** (RIE L10 feed).
- **⚠ s241j MOVES DIAGNOSTICS**: post-2018 alimony instruments fire
  `D_SCH1_007` (error).
- Carried from s240: passive/PTP K-1 §1231 losses fire RED; a non-zero
  Schedule 1 line 4 refuses at MeF composition.
- Carried from s239: Roth 1099-Rs move 5a/5b → 4a/4b; GA partnership K-1s
  move RIE L2↔L13; code-U un-blanks the pension taxable column.
- Carried from s236/s235: GA RIE line 13 on suspended passive K-1 losses;
  GA dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted
- **`--reuse-db` cross-module contamination**: `test_backentry_cleanup.py`
  (3, s225) and `test_mappings.py::TestApplyMappingAmbiguousFederalReturn`
  (3, s239).
- ~~`test_non_engaged_return_leaves_27a_quiet`~~ — **FIXED s244** (stale
  pre-Leg-C pin; out-of-band DOB preserves its intent).
- **`test_1040.py` — 6 pipeline tests**, unscoped `_fv` `.get()` (s234).
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  (s219). `test_4868.py` (4) — ⛔ KEN (s217).
  `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  PTET-gate class (s212).
- **⚠ PRE-EXISTING 1120-S defect (s241o)**:
  `test_line_key_registry_sweep.py::test_formula_targets_resolve` —
  `FORMULA_REGISTRY["1120-S"]` targets `M2_DIST_EXCESS` / `L24_BOOK_BRIDGE`,
  neither seeded. 1120-S only. Deserves its own unit.
- **Client typecheck**: project tsconfig CLEAN (s244 ran `tsc --noEmit`
  exit 0); vitest 1,680 passed / 140 files.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB,
  cross-repo (`test_postgres`). A stalled background sweep beside foreground
  runs is contention, not a hang (s241o).
- A broad `-k` sweep blows the 600s timeout — background it; keep `-k` tight.
- `--create-db` does not reliably drop here; prove a pre-existing red via
  `git worktree` at a pristine SHA with the main venv + copied `server/.env`
  (worked again in s244).
- A timed-out `pytest | Select-Object` loses ALL output — redirect to a file.
- `poetry run python > file` BUFFERS (use `-u`); stdout redirects go through
  cp1252 (write UTF-8 from inside Python); **never rewrite a UTF-8 file via
  `Set-Content`** — use the Write/Edit tools.
- **`poetry run` only works from `server\`**; Windows `python` cannot read the
  Bash tool's `/tmp` — use the scratchpad; DB probes: a throwaway
  `tests/test_zz_*.py` with `-s`, deleted after.
- `manage.py seed_rules` against the pooler takes >5 min — background it.
- Cloudflare-403 law sites and `rules.sos.ga.gov`: the in-app browser gets the
  text where WebFetch and curl fail (s239/s241o).
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes are `/api/v1/tax-returns/…` with the
  trailing slash; `filing_status` is `"mfj"`.
- `_finding(...)` kwargs land under `["details"]`; `ScheduleF` has no
  `business_name` (use `principal_activity`); `order_by("owner")` puts
  "spouse" before "taxpayer" (s241w).

### 🔎 Carried for triage — NOT claims
- **From s241o**: RIE L8 alimony underived; a fuller L10 derive possible in
  principle but per-owner attribution + the (4)(b)2 gambling carve-out make
  it a design pass.
- **From s241**: `Form8606`/`HSAAccount` allow duplicate owners and their
  computes ITERATE (double-count, not vanish); browser POST unguarded.
- **From s234, potentially large**: a materially-participating 1120-S K-1's
  $250k nonpassive ordinary income never reached Schedule 1 line 5 / AGI.
  Repro: `server/tests/test_8960_line4b_clamp.py`, `_build(k1_ordinary="250000")`.
- Carried (s229): exact-tie 1040 shows `1040_SCHD_WS` clc_1/clc_3 drift on a
  bare recompute (−5,491 each), face still a tie.

### ✅ KEN DECISIONS OUTSTANDING
- **⛔ KEN (s231)**: §38(c)(6)(A) MFS threshold — flat $25,000 vs statutory
  $12,500; OVER-allows. Buildable without Ken; the ruling is the gate.
- **⛔ KEN (s227)**: the out-of-scope-state packet-disposition marker.
- **⛔ KEN (s230)**: Form 6765 Section G required for TY2026+ — re-author
  before a TY2026 season.
- **1040 v5.4 business rules still not in hand** (v5.4 schemas ARE on disk).
  ⚠ s240/s241w read the **v5.3** rules — re-check `S1-F1040-118-01` and the
  `SH-F1040-*` family against v5.4 on arrival.

### RS AGENDA
- **(s242x) The TEN staged FA definitions**: FA-1040-8853C-01..05 + FA-4562-
  DEST/ROUND/280F + FA-1040-2210-08/09 — author in RS, re-export, move from
  `flow_assertions_1040_pending.json` to the gate mirror.
- **⛔ BLOCKING two batch items: NO NOL SPEC** (`172`/`NOL`/`FORM_172`/`1045`
  all 404). BATCH-001 #4 + BATCH-002 #10 wait. Preservation is built; only
  the computation waits. Highest-value authoring order on this list.
  **✅ s246: the AUTHORING BRIEF is drafted** —
  `server/specs/_nol_authoring_brief.md` (Form 172 Rev. 12-2024 + i172
  fetched; ⚠ Pub 536 is RETIRED, its rules live in i172 now; the 80%-cap
  base stated verbatim; four one-decision questions for Ken's walk with
  recommendations — v1 = deduction-side only, farming carryback refuses,
  AMT NOL stays preserve-only). The authoring session in
  delvio-rule-studio starts from the brief; NOTHING seeds without the walk.
- (s241b, reaffirmed s244): the `8862` spec is a draft collapsing each PART
  to one boolean — re-author per-line from the Rev. 12-2025 face. ⚠ The
  seeded app face still carries a `part_v` pseudo-line from that draft; the
  Dec-2025 revision DROPPED Part V (qualifying-child-of-more-than-one-person
  is now caution text, no answer field). D_EIC_016 keys on the row as an
  app-internal preparer flag — harmless, but the re-authoring should retire
  or rename it.
- (s241w): `SCHEDULE_H` is a DRAFT covering 7 of ~27 lines — re-author.
- (s241s): the GA QEE credit has NO SPEC (two carryforward regimes).
- (s241p): `4547` and `8879_TA` have NO SPEC; record `IND-476`.
- (s241o): the `500` spec has NO rule for what feeds RIE lines 1/2/6-13.
- Carried: `5329` roll-forward silence (s241); `R-8582-MULTIFORM` stale cite
  + `4797` K-1 §1231 silence (s240); `R-RET-CODE` outrun ×3 (s239); `8379`
  draft (s238); `R-SCHA-CHARITABLE` buckets + RIE-13 (s236); SCHEDULE_A
  carryover aggregation + `500` line 7a typed `input` (s235); s232/s231/s230
  items; the 1065 K-1 box-15 letters (still URGENT).

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
