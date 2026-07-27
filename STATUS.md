# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-27, session 123 (QA Batch-001 item 10 — Form 2210
reconciliation panel + Part III face **BUILT**; RS `7bdad04` seeded + deployed
export verified + mirrored; **migration 0221 STAGED, NOT APPLIED — awaiting
Ken's push**).*

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

## ▶ NEXT (cold-start pointer)

**Ken's call on pushing 0221.** Then: **Form 2210 Part II boxes A/B/D/E** (the
unit that would let a 2210 actually be transmitted — box D is a penalty-REDUCING
election we don't offer) · **item-6-P1 GA residual** (still BLOCKED on the two
GA REVIEW_QUEUE questions) .

Batch-001 is now **13 of 16** done; opens = GA residual · the two QA cases item
15 deliberately did not cover (W-2 boxes 3/5, Schedule C net loss with no
detail).

## ▶ Waiting on Ken / external
1. **PUSH GATE: migration 0221** (three additive nullable/blank Taxpayer fields
   for the documented source override). Staged, not applied.
2. s123 ratifications (REVIEW_QUEUE): the narrowed 2210 e-file policy · Part II
   box sequencing (incl. box D) · D_2210_TIE at warning.
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

## ⚠ NOT VERIFIED IN A BROWSER — and why

**The panel has NOT been probed live.** Migration 0221 is unapplied on both
shared DBs (Ken's gate), and Django selects every model field, so the new
`t2210_penalty_source_amount` column makes **every 1040 taxpayer query fail**
against an un-migrated DB — the demo editor 500s before the tab renders. I did
not migrate the demo DB to get around it; that is Ken's trigger to pull.

**Consequence for the deploy, and it is not a soft one:** this code **cannot**
ship without 0221 landing in the same deploy. Code-without-migration is not
degraded, it is a broken 1040 for every client. Render runs `migrate` in the
build, which is the normal path — but the two must go together.

**First thing after the push:** open a 1040 with a penalty on the demo build and
confirm the panel renders (Part I with the harbor line, the Section A columns,
the accrual trace). That is the s114 check — four green server legs and a full
vitest run once missed a whole tab that never rendered.

## Active gates
- **Deploy: NOTHING PUSHED this session.** Last live deploy is s122's `22a7dd7`.
- **DB state: migration 0221 STAGED, NOT APPLIED to either DB.** Additive only —
  `t2210_penalty_source_amount` (nullable decimal; NULL means "no override",
  deliberately distinct from an override of $0), `_label`, `_note`. No existing
  row changes behaviour.
- **Seeder note for the deploy:** `seed_form_2210` now DELETES the two
  superseded bare rows ("18"/"25") and their compute-written values, which the
  next recompute rebuilds. It REFUSES to touch any row a preparer has
  overridden. Audited 2026-07-27 on the shared DB: 15 returns hold FORM_2210
  values, **0 overridden anywhere**.
- **Rule catalogue:** `seed_rules` must run on BOTH DBs **AFTER** the deploy
  (the required order) for the new `D_2210_SRC` / `D_2210_TIE`.
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
