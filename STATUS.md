# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-06 (s220). **BATCH-013 IS CLOSED** — the four remaining
builds (items **2, 4, 5, 8**) all landed in ONE deploy, commit `0239b11`,
migrations **0257–0259**. The batch file moved to `CC Changes Done` with a
PART 2 annex. The 1120-S CC-Changes queue is **EMPTY**; the loop returns to the
1040 lane.*

*Previous (s219): BATCH-013 PART 1 — 7 of 11 (5 BUILT, 2 REFUTED), migration
0256. (s218): BATCH-047 #15 Form 4952 BUILT. (s217): #11 name suffix BUILT.*

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
- **1120-S** (`1120S\CC Changes\`): **EMPTY** — batch-013 closed and filed to
  `CC Changes Done`. Sweep it at boot; if Codex has posted batch-014, work it.
- **1040** (`CC Code Changes\`): this is the pickup. **BATCH-047 #13** — the
  source-level 1099-MISC rows, a full build — is the top item, plus BATCH-046
  #1 (Form 1310) and NZ #4/#5/#6/#9.

### ✅ s220 in one paragraph
All four were genuine builds, and the theme was **one engine, two lanes**.
**#5** turned out to need no engine at all: `InstallmentSale` and
`compute_6252` have been form-agnostic since the 1040 build, so the entity lane
was missing only the *surface* — building a second gross-profit-percentage
implementation was the trap. The frozen §453 percentage is honored rather than
recomputed (450,000 × 0.8892 = 400,140; recomputing 844,777/950,000 would report
400,157). **#2** is the consequential one: a bulk sale's regular AND AMT
adjusted basis are now summed from the *linked asset rows*, which is exactly
what makes K15b tie (−161,558 / −22,318) where an aggregate row that cannot see
its members reports zero — and the members stop emitting the eleven-way
zero-proceeds loss that had been carrying through K18, M-1/M-2, Schedule L and
Georgia. **#8** added the `aggregate_gross` presentation and, with it,
`sec_179_in_state_current` — the Georgia twin of the §179 component batch-013
#10 introduced federally — so the pair subtracts the engine's own answer instead
of re-deriving the election's caps. **#4** gave a net-only Schedule D surface
that refuses to fabricate 8949 detail and refuses to e-file. Two Georgia/e-file
boundaries were **stated, not guessed** (below).

### ⚠ Classes that MOVE existing returns or output on next recompute
- **Nothing moves by default.** All three new columns default to blank/zero, so
  a return only changes once a packet names a bulk-sale group, an aggregate
  capital net, an installment sale, or the aggregate-gross GA presentation.
- The Schedule K line-7/8a clear-then-write and the K9/line-4 clear are
  unchanged; the new feeds are additive addends.
- Carried from s219 and still true: the **1065 K15a credit-line
  contamination** — every 1065 with a depreciation register had been writing a
  depreciation adjustment into the Low-Income Housing Credit line, now 1120-S
  only, so those returns blank on next recompute. ⛔ KEN: any closed-out 1065 is
  worth re-checking.

### ⚠ Known red / rotted (not this session's changes)
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock fixtures
  hitting a UUID `ValidationError`. Proved pre-existing in s219. Not diagnosed.
- `test_4868.py` — 4 tests fail on the Schedule 3 line-10 feeder. Proved
  pre-existing in s217. ⛔ KEN: a 4868 payment not reaching Schedule 3 line 10
  is a real number, not a test artifact.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  the batch-005 #9 PTET-gate class, red on main since s212.
- `test_mef_scenario5_1120s_compute.py` (both) — `M2_3a` computes 4,975 against
  an expected 5,461. ⛔ KEN: an IRS ATS scenario with a published answer key
  needs a ruling, not a test edit.
- closeout↔cleanup SUITE-ORDER contamination in `test_backentry_cleanup.py`
  (3, on a `DiagnosticRule` unique-code collision). Green alone.
- **Client typecheck**: `npx tsc --noEmit -p client/tsconfig.renderer.json`
  reports 127 error lines on clean `main` (PdfViewer, RenameClient, Clients,
  FormEditor). Baselined this session by reverting — s220 adds none.

### ⚠ Test-run hazard (standing, confirmed s217, s219, s220)
**Never run two `pytest` invocations concurrently** — they share one test
database, so the second drops the first's seeded `FormLine` rows and produces
*false* failures. Every s220 run was serial: 50 batch-013 · 67 entity lane ·
46 diagnostics · 521 flow assertions · 203 depreciation + GA · 329
renderer/4797/6252/Schedule D · 79 MeF 1120-S · 1,645 vitest — all green.

### Artifacts left on the shared DB (deliberate, s220)
- Migrations **0257–0259** — three CharFields (`ga_depreciation_presentation`,
  `bulk_sale_group` ×2), two engine-written Decimals (`sec_179_in_current`,
  `sec_179_in_state_current`) and one nullable Decimal (`net_gain_loss`). Every
  non-null add carries `db_default` (the s190 deploy-skew rule).
- No new tables, no RLS pair, no seeded FormLines, no probe rows. Every test
  ran against the test DB.
- `D:\tax-test-data\Import Templates\entity-batch-import.schema.json`
  regenerated from the lane's own allowlist (never hand-edited).

### ⛔ KEN DECISIONS OUTSTANDING
- **NEW (s220)**: the **Georgia leg of batch-013 #2**. The item asks the grouped
  bulk-sale computation to feed the Georgia gain/loss adjustment. The figure IS
  computed and `D_SCHK_BULK_GA` reports it with its arithmetic — but it is NOT
  written to a face line, because the GA-600S has no sale-difference line
  (Schedules 7/8 carry the annual §168 pair only, which took its own ruling) and
  **no non-bulk sale computes one either**. Where a sale difference belongs on
  the Georgia face is an RS/Ken call, not a guess.
- **NEW (s220)**: two **e-file boundaries** now refuse loudly rather than
  emitting wrong XML — an aggregate Schedule D net (IRS8949 needs transaction
  detail) and a bulk sale (the aggregate 4797 row has never been carried into
  IRS4797). Both are honest refusals; both are real gaps if such a return ever
  needs to e-file.
- Carried (s219): the **1065 K15a credit-line contamination** — how far back to
  re-check closed-out 1065s. BATCH-013 item 6's repair asks Codex to confirm the
  printed line 2.
- Carried (s218): the **§213(d)(10) long-term-care age cap** needs the
  authoritative Rev. Proc. table in Rule Studio. Form 4952's **1041 routing**
  out of scope — confirm that season-one boundary. The 4952 spec's "not required
  to file" condition names only two of three exception tests.
- Carried (s217): the closed suffix list (JR, SR, I–X) is OUR inference; suffix
  + DECD both want the third MeF name-line segment and the pub is silent.
- Carried (s216): `D_4562_BASIS` warning→error escalation; the L24d
  current-year book bridge needs ratification.
- Carried (s215): `D_4562_VCLASS` warning→error escalation; the ATS scenario-5
  `M2_3a` expectation vs the s213 K16a→OAA routing — which is right?
- Form 6765: RS spec authored s212, ⏳ awaiting Ken's seed approval.
- M2_3a auto-rollup question (s213); the A–M item-7 asset decisions; states +
  K-2/K-3 holds (unchanged).

### RS AGENDA (s220 additions)
- **GA-600S**: record the two depreciation-pair presentations
  (`component_net` / `aggregate_gross`) and what `aggregate_gross` includes —
  regular depreciation including a linked Form 4562 line-16 amount, excluding
  amortization and separately passed-through §179.
- **GA-600S**: the open question above — does a SALE's federal-vs-Georgia gain
  difference have a face destination, or does it stay a keyed Schedule 7/8
  "(Attach Schedule)" amount?
- **Form 4797**: a bulk sale is one property computed over many register rows.
  The spec should record that the aggregate row's own basis columns are ignored
  and that a §179 asset cannot join a group (i4797 sends it to the K-1).
- **Form 6252**: the entity arm exists. §453A interest has no entity
  destination — record that boundary.
- **Schedule D (1120-S)**: an aggregate net-only row is a legitimate source
  shape; record that it prints no Form 8949 and cannot e-file.
- Carried s219 (Form 4562 line-16 row property), s218 (Form 4952 v1
  ratification · Form 7206's missing LTC fact), s217, s216, s215, s214, s213,
  s212 items unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — unchanged.
