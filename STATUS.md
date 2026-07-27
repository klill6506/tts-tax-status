# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-27, session 123 (QA Batch-001 item 10 — Form 2210
reconciliation panel + Part III face **SHIPPED AND VERIFIED LIVE**; RS
`7bdad04`; app `167e908` + deploy fix `2eef57c` + diagnostic fix `42a854b`;
migration 0221 applied BOTH DBs; `seed_rules` BOTH DBs after the deploy; panel
probed on the deployed demo build).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s119–s122 detail is archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s123 built QA Batch-001 item 10 (Ken scoped "panel + Part III grid"
in-session).** Rule Studio leg is SHIPPED and live. The app leg is complete and
green but **NOT pushed — migration 0221 is staged and Ken pulls that trigger**
(push = deploy = migrate on the shared DBs).

### The audit changed the work again (sixth session running)

**The $1–3 deltas the QA reported were already fixed.** Re-running both QA fact
patterns against current code: the MFJ high-income case computes **289** (was
286) and the single-retiree case **189** (was 188) — both exact against the
prior software. s113's flat-7%-through-4/15 correction closed them. Item 10's
first half needed nothing.

**What was actually missing was the reconciliation view**, and it is worth more
than $3. Compute derived per-period installments, per-period underpayments, day
counts and per-payment accrual — then discarded all of it, storing only the
first installment and the SUM of the underpayments. On screen a preparer got one
sentence with the total. On the single-retiree return, entering the packet's
prior-year figures drops the penalty **189 → 0**; nothing on screen said the
safe harbor was what moved it.

### Three defects the audit turned up, none previously logged

1. **The stored Part III line numbers came from a SUPERSEDED Form 2210.** The
   2025 face runs Part III Section A as lines **10-18** (10 = required
   installment, 17 = underpayment, 18 = **overpayment**). The app stored the
   installment on "18" and the underpayment on "25" — and Part III has no line
   25 at all (25 is a Schedule AI line). RS carried the identical numbering.
   Confirmed three ways: the face text, the widget grid (exactly nine 4-column
   rows + line 19), and the IRS template's own subform names
   (`SectionATable[0].Line10[0]` … `Line18[0]`). Line "25" is **retired**, not
   left beside its replacement.
2. **Part I lines 1/2/3/6/8 were never rendered.** The printed form showed line
   9 as "the smaller of line 5 or line 8" with **line 8 blank** — the
   prior-year safe harbor, the exact number the QA called unauditable.
3. **Section A's allocation was wrong for late payments.** The old code carried
   only OVERPAYMENTS forward; the face's line 14 makes each column's payments
   cover the previous column's unpaid balance first. A $5,000 payment in period
   3 against $2,500 installments used to report periods 3-4 as paid; the form
   reports all four still short. **No penalty changed** — Section B already
   applied payments earliest-first.

**A correction I made to my own framing, on Ken's challenge.** I had said the
2210 is never transmitted, citing s75. Ken pushed back; he was right. IRS rule
**F2210-002-02** (Active/Reject, read from the business-rules file this repo
holds) says a transmitted 2210 must carry one of Part II boxes A-E, and the face
says *"You must file Form 2210"* whenever one applies. The 2210 **is** filed in
real cases. What is true is only that *this app* transmits none — it models box
C alone and refuses at extract. That s75 note is still sitting in REVIEW_QUEUE
marked "ratify"; I had cited it back to Ken as his decision, which it was not.

**Line 17 is a RUNNING outstanding balance, not each period's own shortfall** —
my first authored expectation was wrong and the harness caught it. Both readings
integrate to the same amount-days, so the penalty is identical; the harness now
pins that tie directly (Section A ↔ Section B, two independent routes).

**Also shipped:** the documented source override — a controlling outside figure
recorded *without* discarding the computed penalty, moving 2210 line 19 and 1040
line 38 **together** per F2210-006-01 (overriding line 38 alone splits them,
which is what the QA preparer had to do). `D_2210_TIE` catches that workaround.

**Gates green:** NEW server `test_2210_reconciliation_item10.py` **23** (both QA
cases verbatim, the safe-harbor swing, the Section A↔B tie across four shapes,
the late-catch-up correction with the retired behaviour as a negative control,
and a silent-case control on each new diagnostic) · the five existing 2210
suites **54** · flow assertions **521** · NEW client
`form2210Reconciliation.test.tsx` **7** · vitest **543/543** (was 536) · tsc
**52 = the recorded baseline exactly**. RS harness baseline-checked on clean HEAD
first, then **three negative controls each observed failing** before restore.

**One PRE-EXISTING stale test found and re-pinned, not inherited:**
`test_face_penalty_lands` expected a penalty of **369** — the answer under the
retired 6% stub. s113 corrected the rate but never re-pinned this render-leg
value. Proved pre-existing by running it on clean HEAD with the session's work
stashed; it failed there too. Now 372.

**Full server suite: 6,401 passed / 8 failed (1:11:33).** All 8 verified
PRE-EXISTING by checking out `c96ce13` (the s122 close) and re-running them —
identical failures there, and none touches a file this commit changed. They are
NOT inherited as "ordering noise" (the s108e lesson); they are:
`test_8915f::TestLandingChain` ×2 · `test_mar30_session4::TestAAANegative` ×2 ·
`test_supporting_forms_spec::TestOfficerCompensationFlow` ×2 ·
`test_section_179_diagnostics::test_family_registration` (a stale D_4562_ family
list missing `D_4562_BASIS`/`DEST`/`RECON`, all added by the s116/s118
depreciation legs) · `test_tts_forms::TestManifest::test_manifest_is_valid_json`
(expects 93 manifest entries; there are 95). **The last two are one-line
re-pins someone should take** — they are stale expectations from completed work,
exactly the class that hid a real bug in s108e.

## ▶ NEXT (cold-start pointer)

**Ken's call on pushing 0221.** Then: **Form 2210 Part II boxes A/B/D/E** (the
unit that would let a 2210 actually be transmitted — box D is a penalty-REDUCING
election we don't offer) · **item-6-P1 GA residual** (still BLOCKED on the two
GA REVIEW_QUEUE questions) .

Batch-001 is now **13 of 16** done; opens = GA residual · the two QA cases item
15 deliberately did not cover (W-2 boxes 3/5, Schedule C net loss with no
detail).

## ▶ Waiting on Ken / external
1. s123 ratifications (REVIEW_QUEUE): the narrowed 2210 e-file policy · Part II
   box sequencing (incl. box D) · D_2210_TIE at warning · **the stale-2210-face
   fix when line 38 is overridden** (my recommendation is logged; it carries a
   tax-law question about whether the worksheet prints at all).
3. s122 ratification: the source-summary listing line (nominee/accrued/ABP arm).
4. s121 ratification: the `D_8283_017` severity ladder (conservation arm).
5. s118 ratifications: §280F AMT-arm derivation · GA no-bump table.
6. s115 ratifications: 8962 Part IV blank-pct · line 34/4-row cap · line-9 marriage-alt.
7. s114 ratifications: the 8867 rebuild's three judgment calls.
8. s113 ratifications: D_GA500_002 realignment · 2210 flat-7% · 7206 partner-arm scope.
9. Item-6-P1 GA residual — BLOCKING questions: GA line 5 filing status from
   federal? · couple the GA deduction election to the federal election?
10. s112 ratification: manifest-aware RS amendment (mechanism only).
11. 86 backfill review rows (83 effective) · S-24 hub-ein blanking · auth env
    vars · A2A WSDL · WISP · SEC-5 · Resend · role assignments · e-services ·
    CAF · ERO EFIN/PIN · beta clauses · older ratifications (s110 · s106 ·
    s101(4) · s100(3) · s99a · s97 · s96(4) · s95..s72).

## 🔴 THE FIRST DEPLOY FAILED — cause found and fixed (`2eef57c`)

Ken said push. It went out as `167e908`, **migrations applied to BOTH shared
DBs, and the code never shipped.** The s105 shape exactly: `build.sh` runs
`migrate` then `seed_all` under `set -o errexit`, so a raise in a seeder kills
the build *after* the databases have moved forward.

**Cause: `FormFieldValue.form_line` is `on_delete=PROTECT`.** My
`_retire_superseded_lines` deleted the FormLine rows directly, which raises the
moment any return holds a value on them — the shared DBs carried **108** such
rows. Proved by running the delete inside a rolled-back transaction against the
real database. Dependents are now deleted first; the overridden-row guard still
refuses to touch entered data.

**Why every test missed it:** a fresh test DB has no value rows attached when
the seeder runs, so the delete had nothing to protect against. Two new tests
attach values first (the production shape) and pin that an overridden row is
still left alone.

**A second regression the fix surfaced, caught before shipping:** with no
`--year` (how `seed_all` invokes it) only 2025 seeded, but the shared DBs also
carry a **TY2026** FORM_2210. It would have kept the superseded numbering while
compute wrote the corrected one — every `_write_row` no-opping, and a TY2026
return **silently rendering no 2210 face at all**. A no-arg run now seeds the
default year plus every year that already has a definition.

**How the failure was detected — behaviour, not the bundle hash.** The bundle
was unchanged, which alone reads as "still building". The decisive probe: an
existing route (`k1-allocations`) answered **403** on both services while
`form-2210-reconciliation` answered **404** — route absent, code not live.

**Verified before re-pushing:** the fixed seeder dry-run inside a rolled-back
transaction against BOTH shared DBs — no superseded rows left, prod 94 lines
(47 × two years), demo 47, neither database changed. 600 tests green.

## ✅ DEPLOY VERIFIED LIVE — and the probe found one more thing

**Verified by BEHAVIOUR, not just the bundle hash.** The decisive check: an
existing route (`k1-allocations`) answers **403** while the new
`form-2210-reconciliation` answered **404** — route absent. After `2eef57c` it
answers **403** on prod, demo, and prep. Bundle rolled `index-CVBMlEDz.js` →
`index-D9JtpMC0.js`; six new markers present, **and the marker method itself was
validated with control strings** (my first asset path was wrong and 404'd, so
every count including the "baseline" was measuring nothing).

**`seed_rules` run on BOTH DBs AFTER the deploy** (the required order):
`D_2210_SRC` and `D_2210_TIE` present + active + severity `warning` on both;
D_2210 family 8/8. Superseded lines retired: **0 remaining** on both DBs
(prod 94 FORM_2210 lines = 47 × two years, demo 47).

**Live probe on the deployed demo build** (return with a $141 penalty). The
panel renders in full: Part I lines 1-9 with "← sets line 9" on the controlling
harbor and line 8 reading *"not available — no prior-year tax entered"*; the
Section A grid across all four columns; and the Section B accrual table with
per-chunk day counts at 7%. Computed penalty 141.00, matching the engine.

### The probe found a PRE-EXISTING defect (logged, not fixed)

The header showed **$70** while the panel derived **$141**. Cause:
`compute_2210_db` returns early whenever 1040 line 38 carries a preparer
override, so the FORM_2210 rows are neither refreshed nor blanked — the face
keeps a stale penalty. On that return line 38 is deliberately **blank** (the
i2210 "let the IRS figure it and bill you" election, which the ATS scenarios
model), line 19 still held $70 from an older run, and the current facts derive
$141. The $70 would print.

`D_2210_TIE` was firing there and blaming the preparer for splitting two
numbers. **Fixed in `42a854b`:** a blank line-38 override is recognised as the
election, and the finding now names the stale worksheet amount and says to
recompute. The early return itself is UNTOUCHED — it is a deliberate rule and
the fix carries a tax-law question (should the 2210 print at all when the IRS is
asked to figure the penalty?). Recommendation logged in REVIEW_QUEUE.

## Active gates
- **Deploy: `42a854b` VERIFIED live** on prod + demo + prep (route 403, bundle
  `index-CVBMlEDz.js` → `index-D9JtpMC0.js`, six markers present with the grep
  method control-validated). The FIRST attempt (`167e908`) failed — see above.
- **DB state: migration 0221 APPLIED to BOTH DBs.** Additive only —
  `t2210_penalty_source_amount` (nullable decimal; NULL means "no override",
  deliberately distinct from an override of $0), `_label`, `_note`.
- **Seeder:** `seed_form_2210` retires the superseded bare rows ("18"/"25") —
  dependent value rows first (`FormFieldValue.form_line` is PROTECT), refusing
  any row a preparer overrode. Verified post-deploy: **0 superseded rows remain**
  on either DB. A no-arg run now seeds every year that has a definition.
- **Rule catalogue:** `seed_rules` RUN on both DBs after the deploy —
  `D_2210_SRC` / `D_2210_TIE` present, active, warning; D_2210 family 8/8.
- **RS:** `FORM_2210` at `7bdad04` — 15 facts / 5 rules / 21 lines / 8
  diagnostics / 14 scenarios / 9 flow assertions; **already live** (RS's own
  Supabase project backs the deployed instance), export verified and mirrored
  verbatim to `server/specs/2210_spec.json`.
- ⚠ FA-1040-4835-06 drift (chip `task_0cf10eac`, unchanged since s113).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
