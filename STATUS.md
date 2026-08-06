# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-05, session 213 (the CC-Changes loop). This session
CLOSED **BATCH-007** (final item #4, deploy `1bda19c`) and **BATCH-008**
(all 10, deploy `8d022c8`, migration 0242), and deployed **BATCH-009 at
9 of 10** (`f4647b2`, migrations 0243-0244) and **BATCH-010 at 6 of 10**
(`aea1712`, migration 0245). 007+008 are annexed and in `CC Changes
Done`; 009 and 010 carry partial annexes and STAY IN THE QUEUE.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

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
`D:\tax-test-data\1120S\CC Changes\` holds **BATCH-009** (open: **#7
Form 2553** — RS spec EXISTS, 28 facts/8 rules/45 lines/19 diags; a full
form unit: typed election section + shareholder consents + attachment
REFERENCES + closeout presence gate) and **BATCH-010** (open: **#1**
guarded entity-shell bootstrap · **#3's** L24 reconciliation STATEMENT
(the carry itself shipped in 008 #3) · **#7** passthrough K-1 AMT row
family → K15a · **#9** 8949 adjustment code/amount columns on
dispositions). These four builds + 2553 are the next session's work, in
whatever order; then the loop resumes watching for BATCH-011.

### ✅ s213 in one paragraph (detail = the four annexes + commit messages)
The §280F engine grew four verified layers (all Rev. Procs 2018-25→
2025-16 fetched from irs.gov and transcribed): PIS-year cap tables, the
deemed-bonus schedule basis (trigger: explicit prior_bonus OR priors
exceeding the no-bonus caps), §280F(a)(1)(B) post-recovery continuation
(all three arms), and the gross-doubled keying normalization (schedule
basis = gross − prior bonus − prior §179 when original_cost==cost_basis).
Final-recovery-year rows take the REMAINING basis (bounded $10; never a
catch-up). New authorable surfaces: `state_sec_179_prior`,
`current/amt/state_current_depreciation_override`,
`suspended_charitable_cash/_noncash` (7203 line 42),
`other_stock_basis_decreases` (7203 line 13), `L3_BOOK_ADJ`,
`M2_3A_OTHER`, `M2_3D_OTHER`, `F_TO_PAGE1` farm routing. M-2/L bridges:
L24d carries the L24a-vs-ΣM2_1 timing difference; M2_7d derives (OAA
distribution charge); **K16a moved AAA→OAA**; **K15b sign corrected to
AMT−regular** (i1120s verbatim). replace_documents now really empties
(EMPTIED_SECTION_DERIVED_LINES + key-presence rollups). D_4562_RECON
reconciles amortization to D_AMORT. GA pair excludes amortization.
Closeout carries an authoritative `summary` line (runner prints it).
Batch-009 #2/#3/#6 and parts of #5 were REFUTED/not-reproduced with
primary-source or live-probe evidence (A-17 prints 12.27; S3 signs
preserved; GA dry-run auto-create works — all pinned).

### ⚠ Classes that MOVE existing returns on next recompute (all correct-direction)
1. K16a returns: M-2 column (a)→(d) (RE/GA-net-worth neutral).
2. L24a≠M2_1a returns: L24d moves by the difference.
3. K15b: sign flips on every disposition-AMT return — **any packet that
   TIED K15b pre-s213 tied on the WRONG sign and will now no-tie.**
4. Excess-distribution + OAA returns: M2_7d now auto-charges.

### ⚠ Known red / rotted (not this session's changes; verified vs clean HEAD)
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable`
  — the batch-005 #9 PTET-gate class, red on main since s212.
- closeout↔cleanup SUITE-ORDER contamination: running
  `test_backentry_entity_closeout.py` before `test_backentry_cleanup.py`
  fails 3 cleanup tests (each file green alone). Pre-existing.
- (FIXED in s213: test_mar30_session2's rotted MagicMock `_make_asset` —
  3 tests had been red on main with TypeErrors.)

### Artifacts left on the shared DB (deliberate, s213)
- Migrations 0242-0245 ride the deploys (all additive/nullable).
- Read-only probes only (GA-600S FormDefinition count: exactly 1).

### ⛔ KEN DECISIONS OUTSTANDING (new s213 items marked ●)
- Form 6765: RS spec authored s212, ⏳ awaiting Ken's seed approval.
- ● M2_3a auto-rollup question: should shareholder capital contributions
  EVER auto-route into AAA (the batch-008 #8 packet's Lacerte
  presentation)? Built as
  explicit `M2_3A_OTHER` input only — needs a Ken/RS ruling.
- The A–M item-7 asset decisions; states + K-2/K-3 holds (unchanged).

### RS AGENDA (s213 additions — all built per primary sources, need spec ratification)
- 4562 R008/R019: PIS-year cap tables (RP 2018-25…2024-13) + the
  deemed-bonus schedule basis (§168(k)(1), RP 2011-26/2019-13 family) +
  §280F(a)(1)(B) post-recovery continuation + final-year remaining-basis.
- 1120S_M2: K16a routes to OAA col (d) NOT AAA 3a (i1120s/§1368(e)(1)(A))
  — **the spec + exported FA005 assertion encode the wrong column**;
  M2_7d OAA distribution charge; M2_3A_OTHER/M2_3D_OTHER inputs.
- 1120S Sch L: the L24d timing-difference carry (RC001 export needs the
  L24a input added — edited locally).
- 1120S Sch K: K15b = AMT − regular (sign).
- Carried s212 items (GA600S S1 PTET gate · 6765 · M1-6b ratification ·
  8825 22a/22b fact) unchanged.

## ⚠ Open items for Ken — carried from s212 unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — unchanged.
