# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-07 (s225). **Two units: (1) the L24d book bridge RATIFIED
and specced into Rule Studio; (2) NZ #5 (Schedule F) and NZ #6 (SS lump-sum)
both DONE — again lane-only gaps.** Deploys `2a959a5` and `18b4db2`; RS
`d53da62`. **NO migrations** — no model changes in either unit. **The NZ list
goes 7 → 9 of 10; the decision queue is EMPTY.***

*Previous (s224): NZ #9 and NZ #4 — Form 1099-G and Form 8889/HSA through the
lane (`e1a97ac` + `804b088`, migrations 0264/0265).*

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
**He has his laptop — availability is MINIMAL BUT NOT ZERO** (Ken, 2026-08-07),
so work can continue and questions can be asked. Batch them and keep them
low-friction: a few crisp choices he can answer from a laptop in minutes, never
a wall of prose. Nothing is on a clock in that window; the next hard deadline is
2026-09-15 (extended entity returns).

## ✅ THE DECISION QUEUE IS EMPTY — all 20 settled
Ken settled the last one, the **Schedule L line 24d book bridge**, on
2026-08-07 (s225): **yes to the book basis, yes to the three exclusions, yes to
Rule Studio.** Recorded as DECISIONS.md "Scope + gate rulings" **item 15**, and
authored into RS as **1120S_SCHL R010** — R005 and D003 corrected to tie through
the bridge, three scenarios pinning it (the 3,500 case, the zero-bridge
book=tax case, and an exclusions pin that must stay at zero).
`server/specs/1120s_schl_spec.json` re-exported (9 rules / 7 scenarios) and
`compute.py` now cites the rule. ⚠ Ken: **"we may come back to it at some
point"** — ratified, not frozen; the exclusion list is the part most likely to
need revisiting on an unusual return. No engine change: 14 comment-only lines.

## ▶ THE QUEUE IS FULL — s224's decision pass cleared the backlog
Everything below is ruled and buildable without asking:
1. **`CC_A_M_REMAINING_BLOCKERS`** — work as a normal batch (six code requests;
   it was never blocked on Ken)
2. **NZ #5 Schedule F** and **NZ #6 SS lump-sum election** — ⚠ verify what
   shipped first
3. **Form 6765** — spec approved, build scheduled
4. **Form 8853 Section C** (LTC only) — constants in hand: §7702B(d)(4) per diem
   **$420/day** for 2025
5. **§213(d)(10) LTC premium cap** — apply at all three touchpoints
6. **1065 K17a** — build the partnership AMT adjustment (nothing computes it)
7. **Georgia bulk-sale difference** → Other Subtractions + attached schedule
8. **Both e-file refusals** — build out (⚠ the bulk-sale allocation is unsolved)
9. **Identity read-back** — SSN *and* date of birth, from the master record
10. **Form 1310 box B upload path** + a **shared `ForeignAddressType` builder**
11. **CR-2026-001** — Rule Studio seeder fix (mine, no ruling needed)

---

## ▶ RESUME HERE

### The queue right now
- **1120-S** (`1120S\CC Changes\`): **EMPTY** since batch-013 closed (s220).
  Sweep at boot; work batch-014 if Codex has posted one.
- **1040** (`CC Code Changes\`): batches 046 and 047 both CLOSED. **NZ is now
  9 of 10.** The pickup is the **mixed-entity pilot #7** (the K-1 basis/at-risk
  allowed-loss worksheet), or **NZ #2** — see the coupling note below.
  NZ #10 (multi-state) stays parked under Ken's states-on-hold ruling.
  `CC_A_M_REMAINING_BLOCKERS` is NOT blocked (DECISIONS item 1) — work it as a
  normal batch.
- **⚠⚠ NZ #2 AND NZ #6 ARE COUPLED — DO NOT WORK #2 AS AN INDEPENDENT.**
  #6's stated target (1040 line 6b = 43,950 exactly) **cannot be met until #2
  lands**. Pub 915's worksheets are WHOLE-DOLLAR forms; the engine carries
  cents: WS2 L18 = 33,988 × 85% = 28,889.80 (printed 28,890), so L21 =
  18,571.80 (printed 18,572). 43,950 = 25,378 + 18,572 and both addends are
  whole-dollar figures. The election machinery is correct **to the cent**;
  only the rounding is missing. ⚠ #2 owns the WHOLE SS worksheet family — the
  regular worksheet rounds the same way — so rounding just the lump-sum path
  would desync them, and it **MOVES existing returns**. Pinned in
  `test_schedule_f_sslumpsum_nz5_nz6_s225.py` at the engine's real answer with
  the coupling as its own test.
- **⚠⚠ VERIFY-FIRST IS NOW THE DEFAULT HYPOTHESIS, NOT A CAUTION — FOUR
  CONSECUTIVE NZ ITEMS (#9, #4, #5, #6) WERE LANE-ONLY GAPS.** Each read like a
  full form unit; in each case the model, compute, diagnostics and Slate screen
  had shipped and only the back-entry section was missing. The reason: a source
  brief's `input` build leg means the BROWSER lane, so a form's plan can be
  fully ticked while the form stays un-importable.

### ✅ s225 unit 2 in one paragraph — NZ #5 + #6
Both were lane-only, like their two predecessors. **Schedule F**: the models,
`compute_schedule_f` (1c/9/33/34 → Sch 1 line 6 + Sch SE 1a, CRP → SE 1b, the
farm-optional method), 10 diagnostics, the Slate screen, the render leg and six
test files had all shipped; there was no `schedule_fs` lane section. **SS
lump-sum**: `compute_ss_lumpsum` already implements Pub 915 Worksheets 2+4
verbatim against the RS `1040_RETIREMENT` spec and reconciles to Pub 915's own
worked example, with the model, the toggle, D_RET_004/008 and a screen — no
`ss_lump_sums` section and no importable `ss_lump_sum_election`. **Both halves
of #6 had to travel together**: the election is irrevocable and explicit, never
inferred from the rows, so importing rows alone would leave 6b silently
unelected on a packet that filed the election. Georgia needed nothing — GA-500
S1-8 is a federal pull of 6b. **A latent dead arm was repaired in passing**:
`_resolve_misc_1099_link` has always matched farms, but no farm could ever be
imported, so that branch could never resolve; `schedule_fs` now commits BEFORE
`misc_1099s` with a test pinning the order.

### ✅ s224 part 2 in one paragraph
NZ #4 (Form 8889 / HSA) is #9's twin, and the source brief shows why. The model,
`compute_8889` (Parts I–III → Schedule 1 lines 13 and 8f, Schedule 2 lines 17c
and 17d), eight diagnostics and the Slate screen all shipped Phase 2; the
back-entry lane had no `hsa_accounts` section, so no packet carrying a Form 8889
could be imported. **The brief's build plan never named that leg** — its `input`
step means the browser tab, and the two lanes are separate, so a form's plan can
be fully ticked while the form stays un-importable. One real compute gap came
with it: **Form 8889 line 4 (Archer MSA) was hardcoded to zero**, with no
column, no input and no diagnostic, and since line 5 = line 3 − line 4 that left
the contribution limit **too large** — an omission erring in the taxpayer's
favour. Now stored and floored per the 2025 face, and pushed through
`D_8889_EXCESS` too, since that diagnostic calls `hsa_deduction` positionally and
would otherwise have kept pricing the 6% excess warning against the old limit.

### ✅ s224 part 1 in one paragraph
NZ #9 reads "Add 1099-G unemployment documents", which sounds like a form unit.
It is not: the `Form1099G` model, `compute_1099g`, the 25b withholding roster,
five diagnostics and the Slate screen all shipped in Phase 2 on 2026-06-14, and
**the only missing leg was the import lane** — `backentry.v1` had no 1099-G
section, so no packet with unemployment could be back-entered, which is exactly
the blocker the item observed. Adding `g_1099s` is schema growth with zero
compute change; box 2 stays deliberately un-importable because it is the
STATE_REFUND worksheet's input. Wiring the state withholding turned up a third
defect in the same GA-500 line-24 roster s222 already fixed twice: the 1099-G
arm was **ungated**, on a comment in that method asserting the form "has no
state-code box". It has one — **box 10a State** on **Rev. March 2024**, the
revision reporting CY2025 — the column had simply never been stored, and an
absent column had been read as an absent box. Every sibling arm was already
gated; this lone exception credited any state's withholding to Georgia.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **Nothing moves from s225.** Both units are additive: the L24d work was 14
  comment lines, and NZ #5/#6 are lane allowlists only — no compute change, no
  model change, **no migration**. A return with no farm and no lump-sum rows is
  byte-identical.
- ⚠ **NZ #2, when it is worked, WILL move returns** — worksheet rounding
  touches the regular SS worksheet, not just the lump-sum path. Budget a
  movement analysis for it.
- **Nothing moves from either s224 unit.** Form 8889's `archer_msa_contributions`
  defaults to 0 and `hsa_deduction`'s new parameter defaults to 0, so every
  existing return and every existing caller computes exactly as before.
- **Nothing moves from NZ #9 either — by design.** A **blank** box 10a still counts as Georgia on
  GA-500 line 24: every 1099-G row keyed before s224 has a blank one because
  the column did not exist, and this practice files GA. The new
  `D_1099G_STATE` warning surfaces the fallback rather than letting it be
  silent. Only a row explicitly keyed to a non-GA state now drops off line 24.
- The lane widens (`g_1099s`); no existing payload changes meaning.

### ⚠ Known red / rotted (not this session's changes)
- **FIXED s225**: `test_schedule_f.py` (3) — count trip-wires red on main since
  **2026-08-05** (`8d022c8`, s213 batch-008), unrecorded. That batch added the
  two non-face ROUTING lines `F_TO_PAGE1` / `D_F_EXPENSES` to the seeder and
  nobody re-cut the pins (45→47, the computed set, and an expense count that had
  silently absorbed a non-face row by filtering on the "F" prefix). The expense
  pin now excludes the routing line **by name** so it stays a pin on the 25 face
  rows. ⚠ **Two separate stale-pin/stale-mock finds in one session** — a count
  assertion is only as good as the last person who grew the thing it counts.
- **FIXED s225**: `test_schedule_l.py` (4) — an unrecorded red on main. Four
  hand-built mocks of `calculate_asset_depreciation` omitted
  `sec_179_in_state_current`, a key the engine gained in s220 (batch-013 #8) and
  `aggregate_depreciation` reads unconditionally → `KeyError`. **Test-fixture
  rot only, no engine defect** — the engine and its wrapper both return all
  eight keys on every path, including the four early returns. The mocks were
  never updated. Now green (616 passed with the flow assertions).
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock fixtures
  hitting a UUID `ValidationError`. Proved pre-existing in s219. Not diagnosed.
- `test_4868.py` — 4 tests fail on the Schedule 3 line-10 feeder. Proved
  pre-existing in s217. ⛔ KEN: a 4868 payment not reaching Schedule 3 line 10
  is a real number, not a test artifact.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  the batch-005 #9 PTET-gate class, red on main since s212.
- **The DiagnosticRule unique-code contamination is WIDER than recorded.**
  `test_backentry_cleanup.py` (3) was logged as "green alone" — with
  `--reuse-db` it is **red alone too**, because the file's own docstring
  assumes an empty rule table and `.create()`s `D_PREPARER_001` /
  `D_8867_001` directly. Any earlier run that seeded the builtin rules into the
  reused DB collides. `test_ga500_auto_attach_s106.py` (1) and
  `test_ga500_rie_federal_pull.py` (1) join the same class in a full sweep —
  **both pass alone and neither creates a 1099-G row**, so s224 is not
  implicated. Worth fixing the fixtures with `update_or_create`.
- **Client typecheck**: 127 error lines on clean `main` (PdfViewer,
  RenameClient, Clients, FormEditor). Re-measured s224 at exactly 127 — adds none.
- ⚠ **The Slate 8889 test fixture is cast `as HSAAccountRow`**, so adding a
  required field to that interface does NOT fail the typecheck — the fixture
  silently reads `undefined`. Kept in step by hand s224; worth dropping the cast.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB.
- **The hazard is CROSS-REPO** (s221): `delvio-tax` and `delvio-rule-studio`
  both point at the same Supabase instance and both name their test database
  `test_postgres`. `--reuse-db` works; `--create-db` collides.
- ⚠ s224: a long `pytest ... | Select-Object -Last N` that hits the 120s tool
  timeout loses **all** output — the pipeline buffers to the end. Redirect to a
  file and `Get-Content -Tail` instead.

### Artifacts left on the shared DB (deliberate, s224)
- **Migration 0264** — three columns on the EXISTING `returns_form1099g`
  table (`box10a_state`, `box10b_state_id`, `payer_tin`, all `db_default=""`
  per the deploy-skew rule). No new table, so no RLS migration.
- **Migration 0265** — one column on the EXISTING `returns_hsaaccount` table
  (`archer_msa_contributions`, `db_default=0`). Likewise no RLS migration.
- Two new `DiagnosticRule` rows: `D_1099G_STATE` and `D_8889_ARCHER`.
- No seeded FormDefinition changes, no probe rows.

### ✅ KEN DECISIONS OUTSTANDING — ALL CLEARED (last one 2026-08-07, s225)
**Nothing is waiting on Ken.** Every item below was settled, closed by
verification, or is an IRS errand — the full record is in DECISIONS.md. **⚠ Four of the eight items opened in the pass
were already fixed or mislabelled**, so re-verify any "⛔ needs Ken" line against
the code before raising it again. The one live external item: **1040 v5.4
business rules are still not in hand and go active 2026-08-09** (the v5.4
schemas ARE on disk; 1041 v5.5 arrived 2026-06-29 and that half is closed).

*(Historic list retained below for the trail — treat as settled unless
DECISIONS.md says otherwise.)*
- **NEW (s224)**: **Form 8853 (Archer MSAs and Long-Term-Care Contracts) is not
  built.** Form 8889 line 4 now takes the Archer figure and reduces the HSA
  limit correctly, but the MSA side of such a return is entirely manual, and the
  8889's own face says "Complete Form 8853 ... if required" *before* you begin.
  `D_8889_ARCHER` names it. Worth deciding whether 8853 is in season-one scope.
- **NEW (s224)**: the **Rev. December 2026 Form 1099-G renumbers the boxes** —
  a new "Family leave benefits" money box takes 10 and the state trio moves to
  11a/11b/12. TY2026 needs its own columns *and* a routing decision for family
  leave benefits, which this app does not handle at all. Not a TY2025 issue;
  flagged now so it is not discovered mid-season.
- Carried (s223): box B's **court certificate** is a reference, not a real PDF,
  so a box-B Form 1310 return still refuses e-file — should the browser lane
  accept an upload? · a **foreign claimant address** is refused at composition
  (no `ForeignAddressType` builder exists in the 1040 mapper) · the stale
  duplicates `CC_CODE_CHANGES_BATCH-046 - 3.md` and
  `CC_CODE_CHANGES_BATCH-047 - 2.md` now contradict the closed originals and
  deletion needs Ken's go.
- Carried (s222): the **W-2G Georgia-withholding movement class** — how far back
  to re-check closed-out Georgia returns; **routing 1099-MISC boxes 13a/13b/14**
  into the OBBBA Schedule 1-A deductions is its own unit.
- Carried (s221): the **8582 MAGI movement class**; whether a return's identity
  card should **read back** from `clients_tax_identity`; the **duplicate client
  pairs** sharing one SSN hash; **CR-2026-001 triage** in Rule Studio; the
  **1040 v5.4 business rules** are still not in hand, and the 1041 package that
  arrived is v5.3 where **v5.5** is already extracted.
- Carried (s219): the **1065 K15a credit-line contamination**.
- Carried (s220): the **Georgia sale-difference face destination** for a bulk
  sale; the two **e-file refusals** (aggregate Schedule D net; bulk sale).
- Carried (s218): the **§213(d)(10) long-term-care age cap** needs the Rev.
  Proc. table in Rule Studio.
- Carried (s217): the closed suffix list (JR, SR, I–X) is OUR inference.
- Carried (s216): `D_4562_BASIS` warning→error escalation. *(The L24d book
  bridge is RATIFIED — decision 15, s225.)*
- Carried (s215): `D_4562_VCLASS` warning→error escalation.
- Form 6765: RS spec authored s212, ⏳ awaiting Ken's seed approval.

### RS AGENDA (s224 additions)
- **Form 8889**: record that **line 4 (Archer MSA) reduces the limit** — the
  spec/loader had it as a constant zero — and that **Form 8853 is not built**.
- **⚠⚠ A BUILD-PLAN PATTERN worth recording in every source brief**: the `input`
  leg means the BROWSER lane. **The back-entry lane is a SEPARATE leg and no
  brief names it**, so a form's build plan can be fully ticked while the form
  remains un-importable. Both NZ #9 and NZ #4 were exactly this. Every future
  brief's build plan should list the lane section explicitly.
- **Form 1099-G**: record the revision authority — **Rev. March 2024** is the
  TY2025 form (10a/10b/11); **Rev. December 2026** renumbers to 11a/11b/12 and
  adds "Family leave benefits" at box 10.
- **A general pattern, now confirmed twice** (1099-MISC box 14 in s222, this):
  **on irs.gov, `f<form>.pdf` is the NEXT revision.** For an information return,
  the TY-correct face lives under `pub/irs-prior/`. Any brief that names box
  numbers must name the revision alongside them.
- **And a second general pattern**: a **missing COLUMN gets read as a missing
  BOX**. Twice now a source brief promised to store a field, the field was
  never built, and downstream code inferred from the model that the form had no
  such box. When a brief says "stored", verify the column exists.
- Carried s223 (Form 1310 has no computation and therefore no RS spec; the MeF
  business rules can be narrower than the printed face), s222, s221, s220,
  s219, s218, s217, s216, s215, s214, s213, s212 items unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
