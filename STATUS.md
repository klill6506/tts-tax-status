# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-06 (s222). **BATCH-047 #13 BUILT and the batch CLOSED** —
source-level Form 1099-MISC rows, end to end. Two live defects found and fixed
in passing, neither reported, both in the GA-500 line-24 withholding roster.
One deploy: `d885ab7`; migrations 0260 + 0261.*

*Previous (s221): a long production-support session on ONE 1040 — both reported
blockers refuted, three other defects fixed; `M2_3a` and CR-2026-001 closed.*

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
- **1040** (`CC Code Changes\`): **BATCH-047 is now CLOSED** and filed to
  `CC Code Changes Done` with a result annex. The pickup is **BATCH-046 #1 —
  Form 1310** (deceased-taxpayer refunds; no code exists, a true build).
  Then NZ **#4 Form 8889/HSA · #5 Schedule F · #6 SS lump-sum election ·
  #9 1099-G unemployment**, then the PULLIAM pilot **#7** (the K-1
  basis/at-risk allowed-loss worksheet). NZ #10 (multi-state) stays parked
  under Ken's states-on-hold ruling.

### ✅ s222 in one paragraph
Form 1099-MISC had **no model, no import section and no source-level
representation anywhere** — the only treatment was a flat Schedule 1 line 8z
amount that discarded the payer, the box identity, the owner and the
withholding. Built end to end. The authority is the form itself: **no
information return is specced in Rule Studio** (1099MISC/INT/DIV/R/NEC/G and W2
all 404), so this follows the Form 1099-G precedent and ships a source brief
written from **Rev. April 2025** — the revision that reports CY2025. That
mattered: **box 14 reads "Reserved for future use" on it** (the golden-parachute
box was dropped after Rev. 1-2024) while Rev. 12-2026 makes 13a/13b/14 Cash
tips / TTOC / Overtime, so the box layout is not stable across years and the
lane refuses box 14. The design turns on one point: **only boxes 3 + 8 (→
Schedule 1 line 8z) and box 4 (→ 1040 line 25b) are additive feeders.** Every
other box reports on an activity's own schedule, whose income the preparer
already keys — adding it again would double count — so a routed row is
traceability, and `D_1099MISC_RECON` catches the *opposite* error instead: an
activity keyed for LESS than the 1099-MISC routed to it, which nothing on the
return caught before. Line 8z already had an owner (`compute_state_refund_db`
writes *and blanks* it), so it is now composed by a single final writer that
reads the worksheet's share back from the worksheet's own row.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **Every Georgia return carrying a 1099-G row**: the GA-500 federal pull
  **raised `AttributeError` and 500'd** — it read `g.state_withholding`, which
  does not exist on `Form1099G` (the column is `box11_state_withholding`). The
  pull now completes, so line 24 and everything downstream of it appear for the
  first time on those returns. ⛔ KEN: this was a live production failure.
- **Every return with a W-2G carrying Georgia withholding**: `FormW2G` was
  missing from the GA-500 line-24 roster entirely, so that withholding never
  reached the state return. It does now — **GA-500 line 24 rises and the
  Georgia refund rises** on any such return. Line 24 stays overridable.
- **Nothing else moves.** The 1099-MISC legs are inert until a row exists: no
  return in the system has one, so Schedule 1 line 8z, 1040 line 25b and
  Schedule A line 5a are unchanged everywhere else.

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
  RenameClient, Clients, FormEditor). Baselined s220; **s222 re-measured at
  exactly 127 and adds none.**

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB.
- **The hazard is CROSS-REPO** (s221): `delvio-tax` and `delvio-rule-studio`
  both point at the same Supabase instance and both name their test database
  `test_postgres`, so a rule-studio run collides with a tax-app one —
  `--create-db` fails with "being accessed by other users". `--reuse-db` works.

### Artifacts left on the shared DB (deliberate, s222)
- **Migrations 0260 (new table `returns_form1099misc`) + 0261 (its RLS ALTER,
  default-deny per the new-table rule)**, applied by the deploy.
- A new seeded `FORM_1099MISC` FormDefinition (11 lines), picked up
  automatically by `seed_all`'s name discovery — no registration needed.
- Ten new `DiagnosticRule` rows (the `D_1099MISC_*` family).
- No probe rows, no test data.

### ⛔ KEN DECISIONS OUTSTANDING
- **NEW (s222)**: the **W-2G Georgia-withholding movement class** above — any
  closed-out Georgia return with a GA-withheld W-2G had line 24 understated.
  Worth deciding how far back to re-check.
- **NEW (s222)**: **routing 1099-MISC boxes 13a/13b/14** (cash tips, TTOC,
  overtime) into the OBBBA Schedule 1-A deductions is its own unit, not built.
  The `Taxpayer` placeholder fields whose help text already names "1099-MISC
  box 3" are the eventual consumers. Not urgent — the boxes do not exist on the
  form that reports TY2025.
- **NEW (s222)**: the stale duplicate `CC_CODE_CHANGES_BATCH-047 - 2.md` in the
  1040 queue now contradicts the closed original (it lacks the annexes and
  reads as if #13 is open). Deletion needs Ken's go — see the queue README.
- Carried (s221): the **8582 MAGI movement class** — how far back to re-check
  closed-out 1040s with rentals + nonpassive K-1 income; whether a return's
  identity card should **read back** from `clients_tax_identity` when its own
  snapshot is blank (a PII-plumbing call); the **duplicate client pairs**
  sharing one SSN hash; **CR-2026-001 triage** in Rule Studio (proven a false
  positive, still needs Ken's click); the **1040 v5.4 business rules** are still
  not in hand, and the 1041 package that arrived is v5.3 where **v5.5** is
  already extracted.
- Carried (s219): the **1065 K15a credit-line contamination** — how far back to
  re-check closed-out 1065s.
- Carried (s220): the **Georgia sale-difference face destination** for a bulk
  sale; the two **e-file refusals** (aggregate Schedule D net; bulk sale).
- Carried (s218): the **§213(d)(10) long-term-care age cap** needs the Rev.
  Proc. table in Rule Studio. Form 4952's **1041 routing** out of scope.
- Carried (s217): the closed suffix list (JR, SR, I–X) is OUR inference.
- Carried (s216): `D_4562_BASIS` warning→error escalation; the L24d
  current-year book bridge needs ratification.
- Carried (s215): `D_4562_VCLASS` warning→error escalation.
- Form 6765: RS spec authored s212, ⏳ awaiting Ken's seed approval.

### RS AGENDA (s222 additions)
- **Form 1099-MISC**: there is no RS spec for ANY information return, and that
  looks deliberate rather than accidental — worth Ken confirming, because if
  information returns SHOULD be specced then W-2, 1099-INT/DIV/R/G and this one
  are all carrying their box semantics in code + a source brief instead.
- **Record the year-sensitivity**: the 1099-MISC box layout differs across
  Rev. 1-2024 / Rev. 4-2025 / Rev. 12-2026, and box 14 means three different
  things (golden parachute / reserved / overtime) across them.
- **Record the double-count rule**: for any information return, the boxes that
  are engine feeders are exactly those whose destination has no other keyed
  source. Everything else belongs to an activity and must be reconciled, never
  added.
- Carried s221 (Form 8582 line-6 MAGI · Form 1040 line 35a identity · Form 4562
  §179-before-§168(k) · 1120-S M-2 tax-exempt interest is OAA), s220, s219,
  s218, s217, s216, s215, s214, s213, s212 items unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts, so a
## shareholder's pass-through 4797 gain must be re-keyed by hand with nothing
## tying it back to the K-1 our own 1120-S produced.
