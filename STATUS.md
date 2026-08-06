# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-06 (s218). The 1120-S CC-Changes queue was empty at
boot again, so the loop stayed in the **1040 lane** (`D:\tax-test-data\CC Code
Changes\`). Worked BATCH-047 **#15 (Form 4952) and #14 (Form 7206 source
facts)**. **#15 BUILT end to end** — model, compute, Schedule A line 9, the
Schedule D Tax Worksheet, diagnostics, lane, Slate, the printed face and MeF.
**#14's stated cause is REFUTED**; the real gap was one line away and is
built. One deploy `2627220`; migrations **0254** and **0255**.*

*Previous (s217): BATCH-047 #11 name suffix BUILT (migration 0253), #12
REFUTED. (s216): BATCH-012 closed, migration 0252.*

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
  s216 deploy before re-reporting**.
- **1040** (`CC Code Changes\`): **BATCH-047 #13 is the next pickup** — the
  source-level 1099-MISC rows. Re-verified this session: there is no
  `misc_1099s` model, no import section, no source-level 1099-MISC anywhere;
  the only treatment is a flat Schedule 1 line 8z amount. It is a **full
  build**, not a lane gap — model + migration + routing (8z / Schedule E /
  Schedule C / withholding) + lane + Slate + diagnostics — and is the largest
  of the three items this batch had left.
  Also still open in that lane: BATCH-046 #1 (Form 1310), and NZ #4/#5/#6/#9
  (HSA · Schedule F · SS lump-sum · 1099-G).

### ✅ s218 in one paragraph
**Form 4952 is built**, from the Rule Studio spec `4952` v1 (fetched live and
cached; authorities Form 4952 (2025) Cat. No. 13177Y + 26 U.S.C. §163(d)). All
five of the spec's own scenarios pass verbatim, and the batch item's return
reproduces exactly: 17 + 14 = 31 total interest against 5,532 − 5,525 = 7 of
investment income, so line 8 = **7** and line 7 = **24**. The wiring is where
the judgement was. **Schedule A line 9 now takes Form 4952 line 8 whenever the
form is keyed** — the one Schedule A line where the derived figure outranks the
flat entry, because a stale hand-keyed line 9 silently beating computed source
facts is exactly the s216 class of defect; `D_4952_SCHA9` reports the
disagreement rather than dropping the entry without a word. The **SDTW lines
3/4 now read 4g/4e** (hard zeros before). **`D_SCHD_001` was reworked, not
retired**: half of it was never about the missing form — a return that declares
Form 4952 and keys nothing would run the worksheet at 4g = 4e = 0 and produce a
defensible-looking wrong number — so it now fires only on
declared-and-unkeyed, and that case still blanks line 16. The form also prints
(official template, SHA-recorded, the generic `f1_NN` map asserted
**positionally** against the template's own geometry) and transmits
(`build_irs4952`, element names and order checked against the 2025v5.4 XSD's
own `<LineNumber>` annotations by a test that re-parses the schema).
**#14 is refuted on its stated cause**: `sehi_amount` is Form 7206 **line 1**,
the source premium — `compute_7206` has always applied the earned-income limit
to it, and the item's own 65,765 − 4,646 = 61,119 limit re-derives from the
premium. The real gap was **line 2**: `compute_7206`'s `ltc` argument had no
feeder anywhere, so the printed and e-filed line 2 was a structural zero. New
`sehi_ltc_amount` across the three SE arms, fed through all four call sites.

### ⚠ Classes that MOVE existing returns or output on next recompute
**None.** Every new column (8 `f4952_*` on Taxpayer, `sehi_ltc_amount` on
ScheduleC/F/K1) defaults to 0, so no stored return computes, prints or
transmits differently until someone keys a Form 4952 or an LTC premium. The
one behaviour change without new data: a return that had ticked "files Form
4952" now gets an actionable *"complete the form"* error instead of *"not
supported — prepare manually"*. Line 16 stays blank in both cases.

### ⚠ Known red / rotted (not this session's changes)
- `test_4868.py` — 4 tests fail on the Schedule 3 line-10 feeder
  (`FormLine.DoesNotExist` / `SCH_3` line 10 returns None). Proved pre-existing
  in s217 by reverting to a clean HEAD. Not diagnosed. ⛔ KEN: a 4868 payment
  not reaching Schedule 3 line 10 is a real number, not a test artifact.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  the batch-005 #9 PTET-gate class, red on main since s212.
- `test_mef_scenario5_1120s_compute.py` (both tests) — `M2_3a` computes 4,975
  against an expected 5,461. ⛔ KEN: an IRS ATS scenario with a published
  answer key needs a ruling, not a test edit.
- closeout↔cleanup SUITE-ORDER contamination: `test_backentry_cleanup.py`
  fails 3 tests on a `DiagnosticRule` unique-code collision when run after the
  closeout files. Green alone (6 passed). Pre-existing, unchanged.

### ⚠ Test-run hazard (standing, confirmed s217)
**Never run two `pytest` invocations concurrently** — they share one test
database, so the second drops the first's seeded `FormLine` rows and produces
*false* failures. Every s218 run was serial: 30 (4952) · 9 (7206 LTC) ·
98 (Schedule D) · 120 (Schedule A + 7206 + SEHI) · **771 (the flow-assertion
gate + MeF 1040 + the lane)** — all green.

### Artifacts left on the shared DB (deliberate, s218)
- Migration **0254** — 8 `Taxpayer` DecimalFields, each `db_default=0`.
- Migration **0255** — one DecimalField on each of ScheduleC / ScheduleF /
  ScheduleK1, each `db_default=0` (the s190 deploy-skew rule: Django's AddField
  otherwise drops the Postgres DEFAULT and old code inserting between migrate
  and deploy fails).
- No new tables, no RLS pair, no seeded FormLines, no probe rows. Every test
  ran against the test DB.
- One new tracked binary: `resources/irs_forms/2025/f4952.pdf`, downloaded
  through `scripts/update_irs_forms.py` with its SHA256 recorded in the
  manifest.

### ⛔ KEN DECISIONS OUTSTANDING
- **NEW (s218)**: the **§213(d)(10) long-term-care age cap**. The new Form 7206
  line-2 input takes an ALREADY-capped figure (`D_7206_LTC_AGECAP` says so).
  Seeding the Rev. Proc. age-cap table in Rule Studio would let the engine cap
  it. Worth doing — it needs the authoritative source, so it is not being
  guessed.
- **NEW (s218)**: Form 4952's **1041 routing** (the spec routes line 8 to Form
  1041 line 10) is out of scope — `compute_4952` is called from the 1040 engine
  only. Confirm that's the right season-one boundary.
- **NEW (s218)**: the spec's "you don't have to file Form 4952" diagnostic
  names only two of the form's three exception tests. We check all three (the
  spec's two alone would flag every ordinary limited return). RS should amend.
- Carried (s217): the closed suffix list (JR, SR, I-X) is OUR inference; suffix
  + DECD both want the third MeF name-line segment and the pub is silent — we
  emit `JOHN A<DOE<III DECD`; the `test_4868` Schedule 3 line-10 red above.
- Carried (s216): `D_4562_BASIS` warning→error escalation; the L24d
  current-year book bridge needs ratification.
- Carried (s215): `D_4562_VCLASS` warning→error escalation; the ATS scenario-5
  `M2_3a` expectation vs the s213 K16a→OAA routing — which is right?
- Form 6765: RS spec authored s212, ⏳ awaiting Ken's seed approval.
- M2_3a auto-rollup question (s213): should shareholder capital contributions
  EVER auto-route into AAA? Built as the explicit `M2_3A_OTHER` input only.
- The A–M item-7 asset decisions; states + K-2/K-3 holds (unchanged).

### RS AGENDA (s218 additions)
- **Form 4952 spec v1 → ratify.** Built exactly as specced. Three notes back:
  (1) the "not required to file" diagnostic condition is incomplete (above);
  (2) the diagnostics carry no codes — ours are house-convention `D_4952_*`;
  (3) the 4g attribution order (net capital gain first, then qualified
  dividends) is carried as an internal split for the SDTW and changes no 4952
  line — confirm that reading.
- **Form 7206**: the spec has **no LTC fact** even though its own `line_map`
  line 2 is "qualified long-term care premiums (age-capped)". Add the fact, and
  decide whether the age cap becomes engine-applied (see the Ken decision).
- Carried s217 items (GA-500 jointly-owned income splits 50/50 as a MANDATE per
  Ga. Comp. R. & Regs. R. 560-7-4-.02(2) — brief question W6 is answered and
  should be closed; Pub 4164 §12.5 name-suffix legs), s216 (Schedule L L24d
  bridge · GA-600S per-asset 7/8 pair · the GA-600S `S1_3` mislabel in the RS
  stub · Form 4562 line 16 · §168(k) impossible-basis · Schedule L needs no
  tax-register tie), s215 (7203 line 3m · §280F · Schedule L fully-recovered
  subset), s214 (8949 columns (f)/(g) · K15a passthrough · 2553), s213 and s212
  unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — unchanged.
