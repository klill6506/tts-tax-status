# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-06 (s221). A long production-support session on ONE 1040
(the "Littleton" hold, referred to below by return id only). **Both originally
reported blockers were REFUTED**; the real defects were three others, all fixed
and deployed. Also closed the standing **ATS scenario-5 `M2_3a`** question — the
engine was right and our own test was wrong — and **CR-2026-001** (a false
positive). Commits: `a57832c`, `ce2ce1e`, `e9fe025`, `e057fbb`, `11d4260`,
`1c081ae`; delvio-rule-studio `b7b5aeb`. No migrations.*

*Previous (s220): BATCH-013 PART 2 — the four builds (2/4/5/8), batch CLOSED,
migrations 0257–0259.*

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
(extended entity returns). Chat/Codex continues on the 1040 above.

---

## ▶ RESUME HERE

### The queue right now
- **1120-S** (`1120S\CC Changes\`): **EMPTY** — batch-013 closed s220 and filed
  to `CC Changes Done`. Sweep at boot; work batch-014 if Codex has posted one.
- **1040** (`CC Code Changes\`): the pickup. **BATCH-047 #13** — the
  source-level 1099-MISC rows, a genuine full build (model + migration +
  routing 8z / Schedule E / Schedule C / withholding + lane + Slate +
  diagnostics). Then BATCH-046 #1 (Form 1310) and NZ #4/#5/#6/#9.
- **The 1040 under support is Chat's** — do not edit its data.

### ✅ s221 in one paragraph
Verify-first paid four times and one of the four was **my own regression**. The
hold reported a §179 business-income limitation capping the deduction and a
persistence failure; **both were refuted against the live row** — §179 was
already correct (s198's K-1 arm, 516,179 base against a 478,857 election) and
the reported figure was the pre-s198 state, while the "lost" SSNs were sitting
in the canonical `clients_tax_identity` store with **no read-back** into the
return snapshot. The real defect was elsewhere: **Form 8582 line 6 MAGI omitted
nonpassive Schedule E page-2 income**, understating MAGI 119,335 against 355,828
and buying a §469(i) special allowance of 15,332 the return was not entitled to
— the entire federal AND Georgia delta. Then **Form 1040 line 35a never settled
the line-38 penalty** (the post-2210 refresh rewrote line 37 only), so a refund
return paid the penalty out; fixing it exposed that **the Return Manager read
line 34, the overpayment, not 35a**. And fixing THAT broke `D_1040_012`, whose
`35a + 36 == 34` identity the offset made obsolete — a regression my own code
comment had pointed at and I did not follow.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **Every 1040 with rentals AND nonpassive K-1/PTP income**: the 8582 MAGI base
  now includes that income, so a §469(i) special allowance that should never
  have been granted disappears and the deductible rental loss falls. ⛔ KEN:
  any closed-out 1040 of that shape is worth re-checking — the direction is
  always *less* deductible loss, i.e. we were over-deducting.
- **Every 1040 refund return carrying a line-38 penalty**: line 35a drops by
  the penalty. The printed 1040, Form 8879, the 8888 direct-deposit split and
  the MeF `RefundAmt` all follow automatically (they already read 35a).
- **The Return Manager refund column and season totals** now read 35a, so they
  fall on any return with a penalty or an applied-forward election.
- `D_4562_ELECTGAP` stops firing on fully-§179-expensed assets;
  `D_1040_016` stops firing on the ordinary refund-with-penalty case.
- **1120-S Schedule M-2**: no value moves, but a return with tax-exempt
  interest plus another AAA other-addition can now COMPOSE for e-file at all
  (the statement decomposition disagreed with the face and refused).

### ⚠ Known red / rotted (not this session's changes)
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock fixtures
  hitting a UUID `ValidationError`. Proved pre-existing in s219. Not diagnosed.
- `test_4868.py` — 4 tests fail on the Schedule 3 line-10 feeder. Proved
  pre-existing in s217. ⛔ KEN: a 4868 payment not reaching Schedule 3 line 10
  is a real number, not a test artifact.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  the batch-005 #9 PTET-gate class, red on main since s212.
- closeout↔cleanup SUITE-ORDER contamination in `test_backentry_cleanup.py`
  (3, on a `DiagnosticRule` unique-code collision). Green alone.
- **Client typecheck**: 127 error lines on clean `main` (PdfViewer,
  RenameClient, Clients, FormEditor). Baselined s220; s221 adds none.
- *(FIXED this session: `test_mef_scenario5_1120s_compute.py` — see below.)*

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB.
- **NEW (s221): the hazard is CROSS-REPO.** `delvio-tax` and
  `delvio-rule-studio` both point at the same Supabase instance and both name
  their test database `test_postgres`, so a rule-studio run collides with a
  tax-app one — `--create-db` fails with "being accessed by other users".
  `--reuse-db` works. Proved pre-existing by stashing and reproducing worse.

### Artifacts left on the shared DB (deliberate, s221)
- **None.** No migrations, no new tables, no seeded rows, no probe rows.
- Every live investigation was **read-only**, or a `compute_return` inside
  `transaction.atomic()` with a deliberate rollback and a re-read afterwards to
  prove nothing persisted. Presence booleans were printed, never SSN values.

### ⛔ KEN DECISIONS OUTSTANDING
- **NEW (s221)**: the **8582 MAGI movement class** above — how far back to
  re-check closed-out 1040s with rentals + nonpassive K-1 income.
- **NEW (s221)**: should a return's identity card **read back** from the
  canonical `clients_tax_identity` store when its own snapshot is blank?
  `sync_from_taxpayer` is write-only today. It is clearly right in principle but
  changes where an SSN surfaces (today the only path outside the entry card is
  the audited `reveal-ssn`), so it is a PII-plumbing call, not mine.
  ⚠ `TaxIdentity` has **no date_of_birth column at all** — a DOB lives only on
  the return's `Taxpayer` row, so a missing one cannot be recovered by code.
- **NEW (s221)**: **duplicate client pairs** sharing one SSN hash (two pairs
  found on this family). Near-matches stay advisory by design after s217's
  exact-duplicate gate — but they split the identity trail. Worth merging.
- **NEW (s221)**: **CR-2026-001 triage** in Rule Studio. Proven a FALSE
  POSITIVE (Form 6252 unchanged — live page-1 text byte-identical, all 49
  AcroForm field names identical). The seeder bug behind it is fixed both
  sides; the register item itself still needs Ken's click, because the watcher
  never overwrites the authority record on a detected change by design.
- **NEW (s221)**: the **1040 v5.4 business rules** are still not in hand — what
  arrived was v5.3, which we already had. Ken is re-sending to
  `mefmailbox@irs.gov`. Also: the 1041 package that arrived is v5.3 where
  **v5.5** is already extracted from the July BMF drop — confirm why v5.3 was
  requested before using it.
- Carried (s219): the **1065 K15a credit-line contamination** — how far back to
  re-check closed-out 1065s. BATCH-013 item 6's repair asks Codex to confirm
  the printed line 2.
- Carried (s220): the **Georgia sale-difference face destination** for a bulk
  sale (reported by `D_SCHK_BULK_GA`, deliberately not auto-written); the two
  **e-file refusals** (aggregate Schedule D net; bulk sale).
- Carried (s218): the **§213(d)(10) long-term-care age cap** needs the Rev.
  Proc. table in Rule Studio. Form 4952's **1041 routing** out of scope.
- Carried (s217): the closed suffix list (JR, SR, I–X) is OUR inference.
- Carried (s216): `D_4562_BASIS` warning→error escalation; the L24d
  current-year book bridge needs ratification.
- Carried (s215): `D_4562_VCLASS` warning→error escalation.
- Form 6765: RS spec authored s212, ⏳ awaiting Ken's seed approval.
- **RESOLVED (s221)**: the ATS scenario-5 `M2_3a` question is CLOSED — it never
  needed a ruling. See below.

### ✅ Closed this session — two long-standing ⛔ items
- **ATS scenario-5 `M2_3a` (open since s213).** 5,461 − 4,975 = 486, exactly
  K16a tax-exempt interest. Our own transcription of the printed IRS key records
  the AAA other-additions schedule as 2,800 + 3,625 = 6,425 with **no 486 in
  it**, and the 486 landing in "M-2 col (d) OAA". So 5,461 was OUR arithmetic,
  frozen into an assertion pre-batch-010 #8 — never the IRS figure. §1368(e)(1)(A)
  agrees (AAA excludes tax-exempt income). Engine, key and statute all agreed;
  only the test dissented. **AND it uncovered a live e-file defect**:
  `_M2_3A_COMPONENTS` still listed K16a, so the statement decomposition and the
  face disagreed and `_stmt_reconcile` refused composition on any 1120-S with
  tax-exempt interest plus another AAA addition.
- **CR-2026-001 (Form 6252 "changed on irs.gov").** False positive. The
  manifest's `sha256` is the hash of the TEMPLATE ON DISK, and f6252's is
  trimmed to its one form page while the download bundles three instruction
  pages — the only derived template of 98. New `source_sha256` records the raw
  hash; the rule-studio seeder now prefers it and refuses to seed a derived hash
  otherwise.

### RS AGENDA (s221 additions)
- **Form 8582**: record that line-6 MAGI includes nonpassive and PTP Schedule E
  page-2 income, guaranteed payments and portfolio income — the exclusion list
  in §469(i)(3) is closed and i8582 says "include any income that's treated as
  nonpassive income".
- **Form 1040**: record the line-35a identity — `max(0, 34 − 36 − 38)` — and
  that line 38 is subtracted from 35a or added to 37, never both.
- **Form 4562**: §179 reduces basis before §168(k), so a fully expensed asset
  has no bonus to elect out of (the D_4562_ELECTGAP exemption).
- **1120-S Schedule M-2**: tax-exempt interest is an OAA (column d) item, never
  AAA — and the itemized-statement decomposition must track the face formula.
- Carried s220 (GA depreciation-pair presentations · bulk sale as one 4797
  property · entity 6252 · aggregate Schedule D), s219, s218, s217, s216, s215,
  s214, s213, s212 items unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED this session: the
## received `ScheduleK1` has no fields for box 17 code K §179-disposition
## facts, so a shareholder's pass-through 4797 gain must be re-keyed by hand
## with nothing tying it back to the K-1 our own 1120-S produced.
