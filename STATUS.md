# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-05, session 214 (the CC-Changes loop). This session
CLOSED **BATCH-010** (the last 4 items, deploy `9030f08`, migrations
0246-0248) and **BATCH-009** (final item #7, Form 2553, deploy `e61d47b`,
migrations 0249-0250). Both files are annexed and moved to `CC Changes
Done`. **BATCH-011 arrived mid-session and is UNTOUCHED — it is the next
session's work.***

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
`D:\tax-test-data\1120S\CC Changes\` holds **BATCH-011 only** (posted
2026-08-05, 10 items, NOT yet triaged). Read it first; it is the whole
next session. Highest-severity items to lead with:
- **#10** — `replace_documents` returns **HTTP 500** (dry-run aborts
  before reconciliation) when the full scalar set AND all four populated
  row families are present together (2 shareholders · 2 dispositions ·
  12 other-deductions · 11 line-item-details) against a 44-row shell.
  Reducing ANY family to one row makes it pass. Their probe matrix
  already ruled out asset count and each 1/2/3-family combination. They
  also ask for a sanitized error code / correlation ID instead of the
  bare 500.
- **#8** — `replace_documents` on a NONEMPTY other-deductions family
  computes line 20 = 518,249. **Lead: 518,249 − 264,724 = 253,525,
  exactly the row total — the eleven imported rows are counted TWICE
  (once through the rows, once through a surviving old total), while
  meals ride once.** Note the merge block DOES
  `getattr(target, rel).all().delete()` when `section in payload`, so
  the duplication is probably NOT the row delete — suspect the line-20
  rollup composing against a stale/pinned FFV, or the dry-run path.
  Verify before building.
- **#5** — MQ rate tables select the wrong QUARTER (a Q4 asset rated at
  Q1): 5-yr 200DB/MQ year-2 should be 38.00% and 150DB/MQ 28.88%. Use
  the placed-in-service MONTH for every arm. Distinct from batch-007 #9
  and batch-009 #2.
- **#6 / #7** — two SCHED_L_DEPR_TIE defects: the promised
  fully-recovered-subset exception (batch-007 #8) is still firing, and
  the check reads the **AMT** prior arm where it must read the regular
  book arm (their delta is exactly 75,074 − 74,686 = 388).
- **#1 · #2 · #3 · #4 · #9** are input-surface builds (Sch F → page-1
  routing input · Schedule L A/R gross + allowance sublines · 7203 line
  3m other stock-basis INCREASES — note batch-010 #4 built the
  DECREASES twin · K-1 box I loan balances independent of 7203 Part II
  debt rows · `A4` added to `LINE_DETAIL_LINES` for the 1125-A §263A
  statement).

### ✅ s214 in one paragraph
Batch-010's remainder: the **guarded entity-shell bootstrap**
(`POST /api/v1/backentry/entity-shell-bootstrap/` — preview / 409 with
named conflicts on an EIN or normalized-name match / atomic audited
create on `confirm`, reusing `build_federal_return` so a bootstrapped
shell is identical to a UI-built one, presets included); the **L24
statement** now walking the prior-year timing difference; the
**passthrough K-1 AMT row family** (`PassthroughK1AMTAdjustment` →
K15a, deriving even with NO owned assets, with a sources statement);
and **Form 8949 columns (f)/(g)** on dispositions (codes verified live
against i8949, gain = proceeds − basis + adjustment, `render_8949` now
one copy per box). Then batch-009 #7: the **Form 2553 import unit** —
the form was already built, but the LANE could not author it, so a
packet listing "2553 (Return)" imported and closed out silently missing
a required form. Added the typed `form_2553` object section with nested
consent/QSST rows, the `ReturnAttachmentReference` model (references,
never files), and a closeout **presence gate** driven by
`source.forms_needed`.

### ⚠ Classes that MOVE existing returns on next recompute
1. Schedule D returns with 8949 adjustments now compute gain including
   column (g) — previously the adjustment did not exist, so no stored
   value changes, but any packet that faked an adjustment INTO basis
   will now double-count if the code/amount are also keyed.
2. A return whose last passthrough K-1 AMT row is deleted now blanks a
   stale K15a (it previously survived).
3. Form 8949 now prints one page PER BOX instead of one page forcing
   box A/D — page counts change on any return with mixed boxes.

### ⚠ Known red / rotted (not this session's changes)
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable`
  — the batch-005 #9 PTET-gate class, red on main since s212.
- closeout↔cleanup SUITE-ORDER contamination: running
  `test_backentry_entity_closeout.py` (or the new
  `test_backentry_entity_2553.py`) before `test_backentry_cleanup.py`
  fails 3 cleanup tests on a `DiagnosticRule` unique-code collision.
  Each file is green alone. Pre-existing, unchanged.

### Artifacts left on the shared DB (deliberate, s214)
- Migrations 0246-0250 ride the deploys (all additive; two new tables,
  each with its RLS pair migration).
- No probe rows: every test ran against the test DB.

### ⛔ KEN DECISIONS OUTSTANDING
- Form 6765: RS spec authored s212, ⏳ awaiting Ken's seed approval.
- M2_3a auto-rollup question (s213): should shareholder capital
  contributions EVER auto-route into AAA? Built as the explicit
  `M2_3A_OTHER` input only — needs a Ken/RS ruling.
- The A–M item-7 asset decisions; states + K-2/K-3 holds (unchanged).

### RS AGENDA (s214 additions — built per primary sources, need ratification)
- **8949**: the column (f) code set + `(h) = (d) − (e) + (g)` and the
  one-copy-per-box print rule (i8949 2025, fetched from irs.gov).
- **1120S Sch K 15a**: K15a = the register adjustment PLUS passthrough
  K-1 box 17A amounts received (the spec describes only the register).
- **2553**: the lane's authorable surface + the closeout presence gate;
  the spec's 28 facts/8 rules/45 lines all map to existing model fields.
- Carried s213 items (4562 R008/R019 §280F family · 1120S_M2 K16a→OAA
  and the wrong FA005 assertion · Sch L L24d carry · K15b sign) and
  s212 items (GA600S S1 PTET gate · 6765 · M1-6b · 8825 22a/22b)
  unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — unchanged.
