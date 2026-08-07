# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-07 (s224). **NZ #9 and NZ #4 both DONE — Form 1099-G and
Form 8889/HSA through the back-entry lane.** Two deploys: `e1a97ac` (migration
0264) and `804b088` (migration 0265). Two defects fixed in passing: a third
GA-500 line-24 roster arm, and Form 8889 line 4. **The NZ list goes 5 → 7 of 10.***

*Previous (s223): BATCH-046 #1 — Form 1310 built end to end; batch 046 CLOSED
(`658915e`, migrations 0262 + 0263).*

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
Nothing is on a clock in that window. The next hard deadline is 2026-09-15
(extended entity returns).

---

## ▶ RESUME HERE

### The queue right now
- **1120-S** (`1120S\CC Changes\`): **EMPTY** since batch-013 closed (s220).
  Sweep at boot; work batch-014 if Codex has posted one.
- **1040** (`CC Code Changes\`): batches 046 and 047 both CLOSED. The pickup is
  the **NZ list, now 7 of 10**: **#5 Schedule F · #6 SS lump-sum election**.
  Then the PULLIAM pilot **#7** (the K-1 basis/at-risk allowed-loss worksheet).
  NZ #10 (multi-state) stays parked under Ken's states-on-hold ruling, and
  `CC_A_M_REMAINING_BLOCKERS` stays blocked on Ken's two asset decisions.
- **⚠⚠ VERIFY-FIRST ON #5 AND #6 — BOTH #9 AND #4 WERE LANE-ONLY GAPS ON FORMS
  THAT WERE ALREADY BUILT.** Each item read like a full form unit; in each case
  the model, compute, diagnostics and Slate screen had shipped in Phase 2 and
  only the back-entry section was missing. **`compute_schedule_f.py`,
  `rules_schedule_f.py` and `_schedule_f_source_brief.md` all exist** — check
  what actually shipped before designing anything. The deeper reason: a source
  brief's `input` build leg means the BROWSER lane, so a form's plan can be
  fully ticked while the form stays un-importable.

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
- **Nothing moves from either unit.** Form 8889's `archer_msa_contributions`
  defaults to 0 and `hsa_deduction`'s new parameter defaults to 0, so every
  existing return and every existing caller computes exactly as before.
- **Nothing moves from NZ #9 either — by design.** A **blank** box 10a still counts as Georgia on
  GA-500 line 24: every 1099-G row keyed before s224 has a blank one because
  the column did not exist, and this practice files GA. The new
  `D_1099G_STATE` warning surfaces the fallback rather than letting it be
  silent. Only a row explicitly keyed to a non-GA state now drops off line 24.
- The lane widens (`g_1099s`); no existing payload changes meaning.

### ⚠ Known red / rotted (not this session's changes)
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

### ⛔ KEN DECISIONS OUTSTANDING
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
- Carried (s216): `D_4562_BASIS` warning→error escalation; the L24d
  current-year book bridge needs ratification.
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
