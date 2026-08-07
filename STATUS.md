# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-06 (s223). **BATCH-046 #1 BUILT and the batch CLOSED** —
Form 1310, the deceased-taxpayer refund claim, end to end. Two gaps found and
fixed in passing, neither reported. One deploy: `658915e`; migrations 0262 +
0263.*

*Previous (s222): BATCH-047 #13 — source-level Form 1099-MISC rows; batch 047
CLOSED; two GA-500 line-24 roster defects fixed (`d885ab7`, migrations
0260 + 0261).*

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
- **1040** (`CC Code Changes\`): **batches 046 and 047 are both CLOSED** and
  filed to `CC Code Changes Done` with result annexes. The pickup is the **NZ
  list**: **#4 Form 8889/HSA · #5 Schedule F · #6 SS lump-sum election ·
  #9 1099-G unemployment** (5 of 10 already done). Then the PULLIAM pilot
  **#7** (the K-1 basis/at-risk allowed-loss worksheet). NZ #10 (multi-state)
  stays parked under Ken's states-on-hold ruling, and
  `CC_A_M_REMAINING_BLOCKERS` stays blocked on Ken's two asset decisions.

### ✅ s223 in one paragraph
Form 1310 did not exist in any form. Built end to end — and the interesting
part is that **almost every rule on this form is a REJECT rule**, so the work
was mostly about refusing correctly rather than computing. There is no Rule
Studio spec (that is the house pattern for a form with no arithmetic — 8879,
2848, 9465 and 8888 all 404 too), so the authority is **Rev. December 2025**,
`IRS1310.xsd` and the `F1310-*` / `IND-298/299/300` business rules, which turned
out to *be* the specification: **box A can never be e-filed** (the schema's
line-A element is commented out and Part I is a strict B-XOR-C choice), **box B
is valid only on an amended or superseded return** with a court certificate, and
**box C is transmittable only with 2a=No, 2b=No, 3=Yes** — the other answers are
legitimate on paper, so they print and are refused at e-file with the rule
named. Two things the schema hides in comments would have been easy to miss: the
claimant's **name is not on Form 1310 at all** (it becomes the return header's
in-care-of name), and `ValidProofOfDeathInd` is **required but has no box on the
printed face**. The who-must-file gate is also narrower than it looks — it is
conditioned on a **refund**, so a balance-due decedent return needs nothing.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **Nothing moves.** Form 1310 is inert until a row exists, and no return has
  one. The two lane additions (`taxpayer_date_of_death` /
  `spouse_date_of_death` / `in_care_of`) only widen what a payload may carry.
- **One refusal is REMOVED, not added**: a non-MFJ decedent return previously
  could not compose for e-file at all — `_in_care_of_name` raised for want of a
  personal-representative name. A Form 1310 claimant now supplies it.
- Carried from s222: any Georgia return with a GA-withheld **W-2G** has a higher
  GA-500 line 24 (and refund) than before; any Georgia return with a **1099-G**
  row no longer 500s on the federal pull.

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
  RenameClient, Clients, FormEditor). Baselined s220; **s223 re-measured at
  exactly 127 and adds none.**

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB.
- **The hazard is CROSS-REPO** (s221): `delvio-tax` and `delvio-rule-studio`
  both point at the same Supabase instance and both name their test database
  `test_postgres`. `--reuse-db` works; `--create-db` collides.

### Artifacts left on the shared DB (deliberate, s223)
- **Migrations 0262 (new table `returns_form1310`) + 0263 (its RLS ALTER,
  default-deny per the new-table rule — this table carries a claimant SSN, so
  it matters more than usual)**, applied by the deploy.
- Five new `DiagnosticRule` rows (the `D_1310_*` family).
- No seeded FormDefinition (Form 1310 has no computed face), no probe rows.

### ⛔ KEN DECISIONS OUTSTANDING
- **NEW (s223)**: **box B's court certificate** is recorded as a
  `ReturnAttachmentReference`, not a real PDF, so a box-B return still refuses
  e-file. Producing a genuine `BinaryAttachment` needs the packet PDF the lane
  never receives — worth deciding whether the browser lane should accept an
  upload for this.
- **NEW (s223)**: a **foreign claimant address** is refused at composition —
  `IRS1310.xsd` supports it but no `ForeignAddressType` builder exists anywhere
  in the 1040 mapper. Low priority; flagged rather than invented.
- **NEW (s223)**: the stale duplicates `CC_CODE_CHANGES_BATCH-046 - 3.md` and
  `CC_CODE_CHANGES_BATCH-047 - 2.md` now contradict the closed originals (they
  lack the annexes and read as if the items are open). Deletion needs Ken's go.
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

### RS AGENDA (s223 additions)
- **Form 1310**: record that it has NO computation and therefore no RS spec, and
  that its real specification is the MeF business-rule set — box A not
  e-fileable, box B amended-only + certificate, box C forced to 2a=No/2b=No/
  3=Yes, `InCareOfNm` carrying the claimant name, `ValidProofOfDeathInd`
  required with no printed box.
- **A general pattern worth recording**: for any form, the MeF business rules
  can be *narrower than the printed face*. Where they are, the face is still
  the paper truth and the rules are the transmission truth — the app must
  support both and refuse the difference by name.
- Carried s222 (no RS spec exists for ANY information return · the 1099-MISC
  year-sensitivity · the double-count rule), s221, s220, s219, s218, s217,
  s216, s215, s214, s213, s212 items unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
