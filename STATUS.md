# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-06 (s219). The 1120-S CC-Changes queue was NOT empty at
boot — Codex posted **BATCH-013** (10 items + 1 reopened regression) that
morning, so the loop returned to the **1120-S lane**. **7 of 11 resolved**:
5 BUILT, 2 REFUTED with the data repair named. The batch file **stays in
`CC Changes`** with a PART 1 annex; items **2, 4, 5, 8** are four genuine
builds and are the next pickup. Three deploys' worth of commits pushed as one
release: `28f8a91`, `da9f85b`, `01b2ca9`; migration **0256**.*

*Previous (s218): BATCH-047 #15 Form 4952 BUILT, #14 refuted, cause found one
line away. (s217): #11 name suffix BUILT, #12 REFUTED (migration 0253).*

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

---

## ▶ RESUME HERE

### The queue right now
- **1120-S** (`1120S\CC Changes\`): **BATCH-013 PART 2 is the next pickup** —
  items **2, 4, 5, 8**, still open in the posted file under its PART 1 annex.
  All four are builds, not fixes; each needs a model, migration, allowlist
  entry, regenerated import schema, browser UI and compute wiring:
  - **#2** a stable bulk-sale group/link shared by a `dispositions` row and its
    participating `depreciation_assets` — grouped assets keep driving the
    registers and the Schedule L removals while the linked aggregate sale alone
    drives Form 4797 character, with regular AND AMT adjusted basis computed
    from the linked rows so K15b stays source-reconcilable, and no individual
    zero-proceeds gains emitted. Two packets of evidence.
  - **#4** an aggregate Schedule D surface (net short/long amounts) for sources
    that print only corporation-level totals — **must not fabricate** Form 8949
    proceeds or basis, and detail rows stay authoritative when they exist.
  - **#5** a Form 6252 `installment_sales` row family for the entity lane.
    Note `compute_6252.py` already exists for the 1040 — check what is reusable
    before building new.
  - **#8** an explicit Georgia depreciation-pair presentation mode
    (`component_net` vs `aggregate_gross`). Batch-012 #7 made component-net
    global; four packets prove the aggregate-gross convention also exists.
    `aggregate_gross` = regular depreciation **including** a linked line-16
    amount, **excluding** amortization and separately passed-through §179.
    Net Georgia income must be identical under both modes.
- **1040** (`CC Code Changes\`): untouched this session. **BATCH-047 #13** (the
  source-level 1099-MISC rows — a full build) is still the pickup there, plus
  BATCH-046 #1 (Form 1310) and NZ #4/#5/#6/#9.

### ✅ s219 in one paragraph
Two of the eleven were refuted **by arithmetic on the reporter's own numbers**,
before any code was read. **#7**: the payload's beginning equity cells sum to
216,614 *before* the adjustment, so applying the keyed −20,000 gives 196,614 —
production's figure and the right answer; the packet's own answer key is only
self-consistent with the −20,000 in the **ending** column, so the two columns
are transposed. **#6**: the report's three probes are jointly consistent with
production being right — the gross-only probe TIES, which means the source's
total assets *count* the gross, which an equal allowance would have netted
away; the figure keyed as the allowance is the **net column**. Of the five
built, **#10** was the consequential one: K15a differenced two totals that each
carried §179, cancelling *only by luck*, so a truthful `amt_current_override:
0` on a §179 row handed the whole election to the adjustment. The engine now
reports the §179 it placed in each arm and the adjustment differences the
depreciation-only figures. **#1** found that the amortization branch returned
before the override loop existed at all — and that the three per-arm overrides
had **never been rendered on any screen** since batch-009 #10 built them.
**#9** added `f4562_bucket`, so a real book asset can print on Form 4562 line
16 while still rolling into Schedule L, page-1 line 14 and K15a — the scalar
keeps its batch-012 exclusion. **#3** replaced an exact-100 ownership test with
a per-owner last-place tolerance in exact Decimal. The reopened **batch-007 #1**
regression was fixed at the right layer: an emptied class now ends at exactly
zero instead of at a roll-forward residue.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **1065 Schedule K line 15a** — the write was unguarded by form code, and on a
  1065 that line is the **Low-Income Housing Credit (§42(j)(5))**. Every 1065
  with a depreciation register has been writing a depreciation adjustment into
  a credit line; it is now 1120-S only, so those returns will **blank** on next
  recompute. ⛔ KEN: any closed-out 1065 is worth re-checking.
- **1120-S K15a** now writes at zero (was skipped), so a stored figure that
  falls to zero finally clears. Returns overriding exactly one arm of a §179
  row will move — that is the #10 fix landing.
- **Schedule L ending lines 10d/10e/13d/13e** blank to zero on any return whose
  register is entirely disposed. That is the intended batch-007 #1 correction.
- `f4562_bucket` defaults to `""`, so no stored row changes until one is marked.

### ⚠ Known red / rotted (not this session's changes)
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock fixtures
  hitting a UUID `ValidationError`. **Proved pre-existing this session** by
  reverting to clean HEAD: identical 9 failures. Not diagnosed.
- `test_4868.py` — 4 tests fail on the Schedule 3 line-10 feeder. Proved
  pre-existing in s217. ⛔ KEN: a 4868 payment not reaching Schedule 3 line 10
  is a real number, not a test artifact.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  the batch-005 #9 PTET-gate class, red on main since s212.
- `test_mef_scenario5_1120s_compute.py` (both) — `M2_3a` computes 4,975 against
  an expected 5,461. ⛔ KEN: an IRS ATS scenario with a published answer key
  needs a ruling, not a test edit.
- closeout↔cleanup SUITE-ORDER contamination in `test_backentry_cleanup.py`
  (3, on a `DiagnosticRule` unique-code collision). Green alone. Unchanged.
- *(Fixed this session: `test_tts_forms.py::test_manifest_is_valid_json` — s218
  registered f4952's template and missed the hardcoded count. Re-pinned to 98.)*

### ⚠ Test-run hazard (standing, confirmed s217 and again s219)
**Never run two `pytest` invocations concurrently** — they share one test
database, so the second drops the first's seeded `FormLine` rows and produces
*false* failures. Every s219 run was serial: 119 engine · 200 depreciation +
Schedule L · 521 flow assertions · 94 entity lane · 46 diagnostics · 212
4562/engine/L/entity · 569 flow + b011 + b012 · 162 tts_forms · 24 b013 ·
51 vitest — all green.

### Artifacts left on the shared DB (deliberate, s219)
- Migration **0256** — one CharField on `DepreciationAsset`, `db_default=""`
  (the s190 deploy-skew rule: Django's AddField otherwise drops the Postgres
  DEFAULT and old code inserting between migrate and deploy fails).
- No new tables, no RLS pair, no seeded FormLines, no probe rows. Every test
  ran against the test DB.
- `D:\tax-test-data\Import Templates\entity-batch-import.schema.json`
  regenerated from the lane's own allowlist (never hand-edited).

### ⛔ KEN DECISIONS OUTSTANDING
- **NEW (s219)**: the **1065 K15a credit-line contamination** above — how far
  back to re-check closed-out 1065s.
- **NEW (s219)**: BATCH-013 item **6**'s repair asks Codex to confirm the
  printed line 2. If column (b) genuinely prints the allowance equal to the
  gross, the *source's own* total assets are internally inconsistent and that
  becomes the finding rather than a keying repair.
- Carried (s218): the **§213(d)(10) long-term-care age cap** — seeding the Rev.
  Proc. table in Rule Studio would let the engine cap it; needs the
  authoritative source, so it is not being guessed. Form 4952's **1041
  routing** out of scope — confirm that season-one boundary. The 4952 spec's
  "not required to file" condition names only two of three exception tests.
- Carried (s217): the closed suffix list (JR, SR, I–X) is OUR inference; suffix
  + DECD both want the third MeF name-line segment and the pub is silent.
- Carried (s216): `D_4562_BASIS` warning→error escalation; the L24d
  current-year book bridge needs ratification.
- Carried (s215): `D_4562_VCLASS` warning→error escalation; the ATS scenario-5
  `M2_3a` expectation vs the s213 K16a→OAA routing — which is right?
- Form 6765: RS spec authored s212, ⏳ awaiting Ken's seed approval.
- M2_3a auto-rollup question (s213); the A–M item-7 asset decisions; states +
  K-2/K-3 holds (unchanged).

### RS AGENDA (s219 additions)
- **Form 4562**: the line-16 vs line-17 presentation is now a source-authorable
  row property (`f4562_bucket`). The spec should record that a line-16 row is a
  book asset that rolls into Schedule L, while the register-less scalar is not.
- **Georgia depreciation pair**: BATCH-013 #8 will need the spec to say which
  presentation a return uses and what `aggregate_gross` includes (regular
  depreciation incl. a linked line-16 amount; excluding amortization and
  separately passed-through §179).
- Carried s218 (Form 4952 v1 ratification · Form 7206's missing LTC fact),
  s217, s216, s215, s214, s213, s212 items unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — unchanged.
