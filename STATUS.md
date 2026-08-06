# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-06, session 215 (the CC-Changes loop). This session
CLOSED **BATCH-011** — all 10 items, ONE deploy `ac4a70e`, migration 0251.
The file is annexed and moved to `CC Changes Done`. **The CC-Changes queue is
EMPTY; the watcher re-arms.***

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
`D:\tax-test-data\1120S\CC Changes\` holds **only its README** — batch-011 is
closed and there is nothing queued. Next session: sweep the folder at boot;
if a batch-012 has landed, work it. Otherwise Ken directs.

### ✅ s215 in one paragraph
BATCH-011, all ten items, one deploy. **Verify-first paid three times: three
of the ten reported causes were wrong, and each wrong cause pointed at a
different real defect.** #5's "MQ rate tables select the wrong quarter" was
the **§280F passenger-auto cap** applied to an unclassified listed vehicle
(the quarter and both Pub 946 tables were already correct and reproduce the
source exactly) — so `D_4562_VCLASS` now PRICES the fallback and errors when
the missing classification actually moves the number. #7's "the check reads
the AMT arm" was the check being RIGHT and the keyed **beginning** balance
being 388 high (the source's own balance sheet opened one asset at its AMT
prior depreciation) — the finding now names the beginning balance as the
origin. #8's "the rows are appended twice" was the shell's own **named
line-20 components**, keyed by an earlier commit and pinned, summing to
exactly the row total — `replace_documents` now releases the components an
`other_deductions` payload does not key. #2's "the allowance cells don't
exist" was a wrong column letter (`L2c`; the cells are L2a/L2b/L2d/L2e) — an
unknown line number now names the seeded siblings sharing its stem. #10's
HTTP 500 does not reproduce anywhere, including against live prod, so it
shipped a **correlation id + exception class** instead so the next one is
diagnosable. Genuinely built: #1 the `source` provenance audit surface,
restored after s214 tightened it and the generated schema kept its own
hardcoded copy (both now read one definition); #3 7203 line 3m; #4 K-1 item I
loan balances (never populated at all before); #6 the fully-recovered-subset
exception, repaired to key on the cost/accumulated pair moving together; #9
the 1125-A line 4 sub-schedule — plus sub-schedule detail rows now PRINT as
Statement pages, which nothing had ever done.

### ⚠ Classes that MOVE existing returns or output on next recompute
1. **Print**: single-amount sub-schedule lines (1125-A A4/A5, page-1 line 5,
   the M-1 detail lines) now emit a Statement page. **Page counts change on
   any return carrying those rows.**
2. **Print**: Schedule K-1 item I (loans from shareholder) now populates — it
   was blank on every K-1 before. Falls back to the 7203 Part II loan rows
   when the new fields are zero.
3. **Diagnostics**: `D_4562_VCLASS` becomes ERROR severity (never
   acknowledgable) on a Vehicles-group asset with no classification whose
   depreciation the §280F fallback actually changes. Returns that closed out
   over that warning will now hold. ⛔ needs Ken's ratification.
4. **Diagnostics**: `SCHED_L_DEPR_TIE` clears on a return whose cost and
   accumulated lines are short by the same amount, bounded by the
   fully-recovered population. A one-sided gap still errors.
5. `replace_documents` + a nonempty `other_deductions` section now blanks the
   shell's unkeyed named line-20 components. Any shell holding BOTH surfaces
   was already computing a wrong line 20.

### ⚠ Known red / rotted (not this session's changes)
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  the batch-005 #9 PTET-gate class, red on main since s212.
- `test_mef_scenario5_1120s_compute.py` (both tests) — `M2_3a` computes 4,975
  against an expected 5,461. **Red on the prior deploy too**; s213 changed the
  K16a→OAA routing per i1120s and the ATS scenario-5 expectation was never
  re-derived. ⛔ KEN: an IRS ATS scenario with a published answer key needs a
  ruling, not a test edit.
- closeout↔cleanup SUITE-ORDER contamination: running
  `test_backentry_entity_closeout.py` / `_2553.py` / `_b011.py` before
  `test_backentry_cleanup.py` fails 3 cleanup tests on a `DiagnosticRule`
  unique-code collision. Each file is green alone. Pre-existing, unchanged.

### Artifacts left on the shared DB (deliberate, s215)
- Migration 0251 rides the deploy — three additive Shareholder columns, each
  with `db_default` (the s190 deploy-skew rule). No new tables, no RLS pair.
- No probe rows. Every diagnostic run against production data was a dry-run
  inside a rolled-back transaction, and every test ran against the test DB.

### ⛔ KEN DECISIONS OUTSTANDING
- **NEW (s215)**: `D_4562_VCLASS` warning→error escalation (see above).
- **NEW (s215)**: the ATS scenario-5 `M2_3a` expectation vs. the s213 K16a→OAA
  routing — which is right?
- Form 6765: RS spec authored s212, ⏳ awaiting Ken's seed approval.
- M2_3a auto-rollup question (s213): should shareholder capital contributions
  EVER auto-route into AAA? Built as the explicit `M2_3A_OTHER` input only.
- The A–M item-7 asset decisions; states + K-2/K-3 holds (unchanged).

### RS AGENDA (s215 additions — built per primary sources, need ratification)
- **7203 Part I line 3m**: the RS spec's `line_map` has no 3a–3m at all (one
  calculated line 3). The letter→meaning map used is the app's own field map
  plus the MeF element `OthItemsIncreaseStockBasisAmt`.
- **§280F**: an unclassified listed Vehicles-group asset defaults to the
  passenger-automobile caps; over-6,000-lb vehicles are not §280F passenger
  automobiles (§280F(d)(5)).
- **Schedule L**: the fully-recovered-subset exception's new signal (the cost
  and accumulated lines move by the same amount) and the beginning-balance
  origin note.
- Carried s214 items (8949 columns (f)/(g) · K15a passthrough · 2553), s213
  items (4562 R008/R019 §280F family · 1120S_M2 K16a→OAA and the FA005
  assertion · Sch L L24d carry · K15b sign) and s212 items (GA600S S1 PTET
  gate · 6765 · M1-6b · 8825 22a/22b) unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — unchanged.
