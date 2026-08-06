# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-06 (s217). The 1120-S CC-Changes queue was empty at
boot, so the loop returned to the **1040 lane** (`D:\tax-test-data\CC Code
Changes\`). Worked BATCH-047 items #11-#15 verify-first: **all five had been
mis-marked as "worked in the s202-s208 era" — none had shipped.** #12
REFUTED with a regression + a regulation citation; **#11 BUILT** (migration
0253, the taxpayer/spouse name suffix, end to end). #13/#14/#15 remain open
and are the next pickup.*

*Previous (s216): BATCH-012 closed, all 10 items, one deploy `7c5ac63`,
migration 0252.*

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
- **1120-S** (`1120S\CC Changes\`): only its README — nothing queued. One item
  is still deliberately reserved for a future batch-013: the signed /
  explicit-zero Schedule L replacement recurrence (direct-field application of
  signed and explicit-zero values, NOT row replacement). The s216 annex asks
  the entry agent to re-post it first **and to re-run that packet after the
  s216 deploy before re-reporting** — batch-012 items 5 and 6 both touched
  L24d/L27d, so the deltas may have moved.
- **1040** (`CC Code Changes\`): **BATCH-047 #13, #14, #15 are the next
  pickup**, all three verified genuinely unbuilt this session:
  - **#13** source-level 1099-MISC rows — no model, no import section at all.
  - **#14** Form 7206 source facts — `compute_7206` and its diagnostics exist,
    but only `sehi_amount` (the already-computed line-14 answer) is keyable;
    the function's `ltc` argument has **no input feeding it**.
  - **#15** Form 4952 — not built; `D_SCHD_001` exists precisely to say so.
  Also still open elsewhere in that lane: BATCH-046 #1 (Form 1310), and NZ
  #4/#5/#6/#9 (HSA · Schedule F · SS lump-sum · 1099-G).

### ✅ s217 in one paragraph
Verify-first paid immediately: the 1040-lane ledger claimed #11-#15 were
"worked but not re-verified", and **a grep of shipped code showed none of the
five existed**. **#12 is refuted twice over.** Its evidence is arithmetic: the
packet's two payer rows (6,106 and 3,415) against a filed 6,468/3,053 Georgia
attribution is not an unrepresentable split — **3,053 is exactly half of
6,106**, so it is one jointly-owned account plus one the taxpayer owns, which
the existing owner tags already express. And the requested per-row allocation
would let a preparer key something Georgia forbids: **Ga. Comp. R. & Regs.
R. 560-7-4-.02(2)** — "if property is jointly owned, income derived is
allocated to each taxpayer at 50 percent of the total". The 50/50 is
mandatory, not a placeholder. (Our own GA source brief flags this as
unconfirmed open question W6; it is now confirmed against the regulation and
the brief should be updated.) **#11 was built end to end** against Pub 4164
(Rev. 12-2025) §12.5/§13.5 read from the in-repo copy: a closed generational
suffix list, the `<suffix` name-line leg (`JOHN C<BROWN<III`, matching TABLE
12-1), model/serializer/import/proforma/Slate/paper/Georgia. Two cases where
MeF **cannot** carry a suffix are handled deliberately — a joint return with
different surnames already spends both permitted less-than signs, and a spouse
suffix is never emitted ("suffixes should follow the primary taxpayer's last
name only"). There is no `Suffix` element anywhere in the 2025v5.3 schema set.

### ⚠ Classes that MOVE existing returns or output on next recompute
1. **Print — every 1040-family face**: the printed surname now appends the
   suffix (no federal face has a suffix box). **No stored return changes**, and
   nothing moves until someone actually keys a suffix — the column defaults
   blank on all existing rows.
2. **Print — the Georgia face**: GA Form 500's own SUFFIX box now fills. Same
   condition: only once a suffix is keyed.
3. **MeF — the filer name line**: gains a third segment when a suffix is
   present. Again inert until keyed.
Nothing recomputes; this is an additive identity field.

### ⚠ Known red / rotted (not this session's changes)
- **NEW (found s217)**: `test_4868.py` — 4 tests fail on the Schedule 3 line-10
  feeder (`FormLine.DoesNotExist` / `SCH_3` line 10 returns None):
  `TestEndpoint::test_delete_clears_the_sch3_derive` and all three
  `TestSch3Line10Feeder` tests. **Proved pre-existing: reverted every s217
  change to a clean HEAD and the failure is byte-identical.** Not yet
  diagnosed. ⛔ KEN: a 4868 payment not reaching Schedule 3 line 10 is a real
  number, not a test artifact — worth a look.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  the batch-005 #9 PTET-gate class, red on main since s212.
- `test_mef_scenario5_1120s_compute.py` (both tests) — `M2_3a` computes 4,975
  against an expected 5,461. Re-verified s216 on a clean stash: byte-identical
  without any s216 change. ⛔ KEN: an IRS ATS scenario with a published answer
  key needs a ruling, not a test edit.
- closeout↔cleanup SUITE-ORDER contamination: `test_backentry_cleanup.py`
  fails 3 tests on a `DiagnosticRule` unique-code collision when run after
  the closeout files. Green alone (6 passed). Pre-existing, unchanged.

### ⚠ Test-run hazard confirmed this session
**Never run two `pytest` invocations concurrently** — they share one test
database, so the second drops the first's seeded `FormLine` rows and produces
*false* failures (5 spurious errors in `test_1040v_es.py` did exactly this,
and passed cleanly when re-run alone). Every result above was re-verified
serially.

### Artifacts left on the shared DB (deliberate, s217)
- Migration 0253 rides the deploy — two NULLABLE-equivalent `Taxpayer` CharFields,
  both carrying **`db_default=""`** so the s190 deploy-skew rule is satisfied
  (Django's AddField otherwise drops the Postgres default and old code inserting
  without the column fails between migrate and deploy).
- No new tables, no RLS pair, no seeded FormLines, no probe rows. Every test
  ran against the test DB.

### ⛔ KEN DECISIONS OUTSTANDING
- **NEW (s217)**: the closed suffix list (JR, SR, I-X) is OUR inference. Pub
  4164 never enumerates suffixes; it bounds them only by "replace numerals with
  Roman numerals" and by dropping titles like "M.D.". Ratify or widen.
- **NEW (s217)**: suffix + DECD both want the third name-line segment and the
  pub is silent on the combination. We emit `JOHN A<DOE<III DECD`.
- **NEW (s217)**: the `test_4868` Schedule 3 line-10 red above.
- Carried (s216): `D_4562_BASIS` warning→error escalation; the L24d current-year
  book bridge needs ratification.
- Carried (s215): `D_4562_VCLASS` warning→error escalation; the ATS scenario-5
  `M2_3a` expectation vs the s213 K16a→OAA routing — which is right?
- Form 6765: RS spec authored s212, ⏳ awaiting Ken's seed approval.
- M2_3a auto-rollup question (s213): should shareholder capital contributions
  EVER auto-route into AAA? Built as the explicit `M2_3A_OTHER` input only.
- The A–M item-7 asset decisions; states + K-2/K-3 holds (unchanged).

### RS AGENDA (s217 additions — built per primary sources, need ratification)
- **GA-500 RIE**: jointly-owned income splits 50/50 **as a mandate**, per Ga.
  Comp. R. & Regs. R. 560-7-4-.02(2), not as a default awaiting a richer
  allocation model. `_ga500_source_brief.md` open question **W6 is now
  answered** and should be closed in the brief.
- **Name suffix / Pub 4164 §12.5**: the `<suffix` leg; the two-less-than-sign
  cap that makes a suffix unrepresentable on a joint different-surname return;
  "suffixes follow the primary taxpayer's last name only" (no spouse suffix in
  MeF ever); and §13.5's "omit punctuation marks, titles and suffixes" for the
  name control.
- Carried s216 items (Schedule L L24d bridge · GA-600S per-asset 7/8 pair ·
  GA-600S `S1_3` mislabel in the RS stub · Form 4562 line 16 · §168(k)
  impossible-basis · Schedule L needs no tax-register tie), s215 (7203 line 3m ·
  §280F · Schedule L fully-recovered subset), s214 (8949 columns (f)/(g) ·
  K15a passthrough · 2553), s213 and s212 unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — unchanged.
