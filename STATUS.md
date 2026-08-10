# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-10 (s241n). **BATCH-004 #9 (Form 1099-PATR) — lane and
five diagnostics built** (`a841368`, no migration). ⛔ #9 NOT finished: the
Georgia RIE feed and a CRUD surface remain. Twelve units today; BATCH-004 is 5
of 10 with #9 close.*

*Previous (s241m): the PATR model + 8z composition (`b4c949e`, migs 0281+0282)
— ⚠ 8z has a SINGLE writer that returned early without a 1099-MISC, so a
PATR-only return would have silently dropped its income; (s241j) the alimony
TCJA repeal gate; (s241i) ✅ Form 8862 COMPLETE; (s241g) the false-red class
RETIRED.*

*Previous (s241j): the alimony TCJA repeal gate — `D_SCH1_003` checked the
instrument date was PRESENT, never what it SAID (`fdd2459`); (s241i) ✅ Form
8862 COMPLETE; (s241g) the `inspect.getsource` false-red class RETIRED.*

*Previous (s241i): ✅ Form 8862 COMPLETE, Section A print + transmission
together (`87979f2`); (s241g) the `inspect.getsource` false-red class RETIRED,
22 failed → 900 passed (`cdbec66`).*

*Previous (s241g): the `inspect.getsource` false-red class RETIRED — 24
assertions, `-k "diagnostic"` 22 failed → **900 passed, 0 failed** (`cdbec66`);
(s241f) the 8862 diagnostics (`35a406a`); (s241e) the printed 8862 caught up
with Part II (`1f5355c`); (s241d) Section B + the ODC split (`7ebd348`);
(s241c) five fabricated sworn answers removed (`3a87b2f`, migs 0279 + 0280).*

*Previous (s241f): the 8862 diagnostics `D_8862_002` / `D_8862_003`
(`35a406a`); (s241e) the printed 8862 caught up with the transmission
(`1f5355c`); (s241d) Section B + the ODC split (`7ebd348`); (s241c) five
fabricated sworn answers removed (`3a87b2f`, migrations 0279 + 0280).*

*Previous (s241e): the printed 8862 caught up with what we transmit
(`1f5355c`); (s241d) Section B + the ODC split (`7ebd348`); (s241c) five
fabricated sworn answers removed (`3a87b2f`, migrations 0279 + 0280).*

*Previous (s241d): Part II Section B transmits and ODC persons stop posing as
CTC children (`7ebd348`); (s241c) five fabricated sworn answers removed
(`3a87b2f`, migrations 0279 + 0280).*

*Previous (s241c): the same form's MeF builder had been fabricating FIVE of the
taxpayer's sworn answers while its docstring promised it did not; two overrode
facts the app stores and uses to compute the credit being certified
(`3a87b2f`, migrations 0279 + 0280).*

*Previous (s241b): BATCH-004 #6, the `education_students` lane (`d55ff15`) —
the uniqueness constraint s241 had just added to Form 5329 would have been a
DEFECT there, and checking rather than pattern-matching is what caught it.*

*Previous (s241): BATCH-004 #7 + BATCH-003 #2 built together as one `form_5329s`
section; a duplicate-owner row made $110.40 of tax vanish (`97ea4a5`, migration
0278). (s240): BATCH-004 opened + triaged 10/10, #4 built (`c6aab19`).*

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
**Availability MINIMAL BUT NOT ZERO.** Batch questions; keep them low-friction.
Nothing is on a clock in that window; the next hard deadline is 2026-09-15.

---

## ▶ RESUME HERE

### ✅✅ BATCH-004 #8 (Form 8862) IS COMPLETE — s241c → s241i, seven units
Model + migrations 0279/0280 · MeF (five fabricated sworn answers removed,
Section B, the ODC group, lines 6 and 8) · print (Parts I-IV, 16 → ~100 map
entries) · browser CRUD + lane section · `D_8862_002` / `D_8862_003`.
⛔ One data gap NAMED, not hidden: `Dependent` has no date-of-death field, so
line 8's death half cannot derive (DEFERRAL_AUDIT).

### ✅ BATCH-004 #3 (alimony) DONE — s241j, `fdd2459`
Mostly refuted: 19a/19b/19c already existed as spec inputs and 19a already
reached AGI. The real finding was that **nothing checked the instrument DATE**,
so a post-2018 divorce deducted alimony and reduced AGI (⚠ overstates the
deduction). `D_SCH1_007` now enforces the repeal; `D_SCH1_008` prompts on the
modification arm the app cannot decide. Lane gained 19a/19b/19c.

### ⭐ NEXT UNIT — **BATCH-004 #9, Form 1099-PATR**. ⛔ **TRIAGED 2026-08-10 —
### the design below is settled. BUILD IT; do not re-triage.**
The reported packet has $1,004 of `FARM CREDIT PATRONAGE DIVIDENDS` on Schedule
1 line 8z → 1040 line 8 → AGI, and it participates in the Georgia
retirement-income exclusion.

**Triage findings — verified in code, do NOT re-derive:**
- ✅ **The item is REAL.** There is no `Form1099PATR` model, no PATR field, no
  patronage source anywhere. The `patronage` hits in the tree are all
  **8995-A / 8835 QBI context** (the §199A(b)(7) patron reduction), a different
  thing entirely — exactly as s240's triage predicted.
- ⚠⚠ **THE CRUX: SCHEDULE 1 LINE 8z ALREADY HAS TWO OWNERS, AND THE COMPOSITION
  IS ALREADY BUILT.** `compute_1099misc.py`'s own docstring: *"SCHEDULE 1 LINE
  8z ALREADY HAD AN OWNER … Two unconditional writers would fight — last one
  wins, and the state-refund disengage path would erase a 1099-MISC amount. So
  8z is COMPOSED by a single final writer: 8z = the STATE_REFUND worksheet's own
  `sch1_8z` output row + Σ (box 3 + box 8) on rows routed to 8z."*
  **1099-PATR is the THIRD contributor**, so it must EXTEND that composition,
  never add a fourth writer. This is the s230 "a shared line's first writer must
  not become its owner" rule, already learned once on this very line — the
  failure mode is a DISAPPEARED number, which nobody reports.
  ⚠ `compute_1099misc_db` runs after `compute_state_refund_db` and before
  `compute_sch123`; PATR has to slot into that same ordering.
- ⚠ **The `8z_type` LITERAL also composes** — it names each source present. PATR
  must join it, or the face will describe the wrong payment.
- ⚠ **ROUTING IS THE TAX QUESTION, NOT THE BOXES.** Patronage dividends land on
  8z, Schedule C **or** Schedule F depending on the activity they arose from,
  and the item explicitly warns against double-feeding where the amount is
  already in Schedule C/F gross income. **Reuse the `misc_1099s` `routing` +
  `link_key` pattern (s222) verbatim** — same carrier, same commit-time
  resolution, same "unresolved → warn + leave unlinked, never guess".
- ⚠ **#9 does NOT go through the 404-STOP gate** — per s222 **no RS spec exists
  for any information return**. Build from the IRS form + a
  `server/specs/_1099patr_source_brief.md` (the s222/s223 shape).

✅ **LEG 1 DONE (s241l, `f04d351`): `server/specs/_1099patr_source_brief.md`.**
The IRS form IS the specification here, so the brief is the first leg, not a
note. It holds the **verbatim** box captions and Instructions for Recipient from
**Rev. April 2025** (the CY2025 revision) — **read it before writing code; do
not re-fetch.** Two traps it names that the batch item does not:

- **⚠⚠ BOX 1 IS NOT UNCONDITIONALLY INCOME.** Verbatim: *"Any dividends paid on
  (1) property bought for personal use or (2) capital assets or depreciable
  property used in your business are not taxable. However, if (2) applies,
  reduce the basis of the assets by this amount."* A blind box-1 → 8z feed
  **taxes a non-taxable amount** (⚠ overstates income), and the basis reduction
  is a future-year attribute of exactly the s235 `CarryforwardAttribute` kind.
  Box 1 needs a taxable/nontaxable split (a preparer assertion — the app cannot
  know what the purchase was for), never an unconditional route.
- **⚠⚠ BOXES 6-9 MUST NOT FEED ANYTHING.** The app already carries
  `qbi_is_patron` / `qbi_patron_alloc_qbi` / `qbi_patron_alloc_wages` and
  `compute_8995a` already implements the §199A(b)(7) patron reduction. Feeding
  them would be the **s234 two-sources defect**. Transcribe and RECONCILE (the
  `D_CFWD_002` doctrine). ⚠ And boxes 8/9 are *subsets of* boxes 1/2/3/5 by
  their own wording — never add them to income.
- The ordinary-income set is **boxes 1, 2, 3 and 5**, not box 1 alone.
- Box 4 → 1040 line 25b. Boxes 10-12 are credit pass-throughs — ⚠ **refuse an
  unmodelled credit BY NAME rather than dropping it** (s236: a refused amount
  is not claimed at all, so it overstates tax).

✅ **LEGS 2-3 DONE (s241m, `b4c949e`):** the `Form1099PATR` model (migrations
0281 + 0282 RLS), `compute_1099patr.py`, and the **8z composition**.
⚠⚠ **The trap that was live and is now pinned:** `compute_1099misc_db` is the
SINGLE 8z writer and **returns early when there is no 1099-MISC**, so a
PATR-only return would have had its patronage income silently dropped — the
exact disappeared-number failure the composition exists to prevent, reproduced
by the fix meant to avoid it. Both the early return AND the disengage path now
carry the patronage share, and both are pinned by tests (a PATR-only return
writes 8z; deleting a 1099-MISC hands the line back rather than blanking it).

✅ **LEGS 4-5 DONE (s241n, `a841368`):** the `patr_1099s` lane section
(`routing` + `link_key`, ordered after `schedule_cs`/`schedule_fs`, unresolved
key → committed UNLINKED with a warning rather than a guess) and **five
diagnostics** — `D_1099PATR_LINK` (error), `_BOX1` (warning, the nontaxable
carve-out), `_BASIS` (info, recorded not applied), `_199A` (warning, reconciled
against `qbi_is_patron` and never fed — s234) and `_CREDIT` (error, an
unmodelled credit REFUSED BY NAME — s236, since a dropped credit overstates
tax). Staging also catches a carve-out larger than box 1, and a basis reduction
larger than the carve-out.

⛔ **Remaining legs — do NOT record #9 as finished until these land:**
1. **The Georgia RIE feed** the item names ("it participates in the filed
   Georgia taxpayer retirement-income exclusion"). ⚠⚠ **VERIFY AGAINST Ga.
   Comp. R. & Regs. r. 560-7-4-.02 FIRST, and treat the item's assertion as a
   hypothesis** — s233, s236 AND s239 each found that feed wrong in a different
   way, and the reg's own unearned list is the controlling text. ⚠ The s239
   finding especially: the reg uses DIFFERENT TESTS for different entity types
   in one sentence, so "patronage dividends are unearned" needs reading, not
   assuming. ⚠ And check the s241 lesson: the RIE base is *income included in
   Georgia taxable income*, so the box-1 NONTAXABLE part must not reach it.
2. **A browser CRUD surface** (`patr-1099s`), so the five new diagnostics are
   actionable — the s241c lesson: ship the input surface with the rule that
   needs it, or the refusal is a dead end.

Then the rest of BATCH-004 by size: #10 Form 4547 + 8879-TA ≈ #2 GA education
credit + IT-QEE-TP2 ≈ #5 Schedule H << #1 1040-X (large). ✅ #10's source check
is CLOSED (s240) — Form 4547 is real, Rev. December 2025, filed WITH the
e-filed return, so its MeF leg is part of that build.

### ✅ DONE in s241g — the false-red class is retired (see Known red / rotted)

⚠ **Carry the s241c design rule forward**: lines 4 and 11 DERIVE from the EIC
engine's own Rule 13 / Rule 12 gates and must never gain a keyed copy (s234 —
two sources for one relationship). Ages likewise derive from the DOBs. Only
line 3 and the 9a/9b day counts are genuinely keyed.

**⚠⚠ THE MeF BUILDER FABRICATES FIVE OF THE TAXPAYER'S SWORN ANSWERS — AND ITS
OWN DOCSTRING SAYS IT DOES NOT.** `build_irs8862` opens with: *"Every
per-child/per-student answer derives from the SAME model facts the credits
computed from (bridge-gate: the 8862 can't contradict the claim)."* **That
sentence is false.** Hardcoded literals (`builder.py:1486-1518`):

| Face line | Element | Emitted | Is there a real fact? |
|---|---|---|---|
| II-3 | `EICEligClmIncmIncorrectRptInd` | `"false"` | **No** — nothing stores it |
| II-4 | `EICEligClmQlfyChldOfOtherInd` | `"false"` | **No** — nothing stores it |
| III-16 | `DependentInd` | `"true"` | derivable (the dependent row exists) — defensible |
| III-17 | `USCitizenOrNationalInd` | `"true"` | **YES — `Dependent.citizenship_status`** |
| IV-19a | `EligibleStudentInd` | `"true"` | **YES — the `EducationStudent` AOTC flags / `aotc_elected()`** |

⚠ **The two marked YES are the worst of the five**, because the app stores the
fact, USES it to compute the credit, and then transmits the opposite of it on
the certification for that same credit. `Dependent.citizenship_status` has five
choices and its own help_text says *"CTC/ODC require US citizen / national /
resident alien per §152(b)(3)"* — so a `nonresident_alien` or
`mexico_canada_resident` dependent must answer **No** to line 17, and the face's
caution is *"If the answer is 'No' for question 14, 15, 16, or 17, you cannot
claim the CTC/ACTC/ODC for that child."* ⚠ **Sign: it certifies eligibility the
taxpayer does not have — it OVERSTATES the credit, on a sworn statement.**
Same shape for 19a against `aotc_elected()`.

**Nothing stores the Part II answers at all** — the only 8862 facts in the repo
are `Taxpayer.eic_disallowed_prior_year` (a boolean that merely says the form is
required) and FormFieldValue line "1", the tax year. So lines 3 and 4 go to the
IRS without the taxpayer having been asked.
**Both cautions are verbatim off the printed 2025 face** (`resources/irs_forms/
2025/f8862.pdf`, Rev. December 2025):
- **Line 3** — *"If the only reason your EIC was reduced or disallowed was
  because you incorrectly reported your earned income or investment income,
  check 'Yes.' Otherwise, check 'No.'"* Caution: *"If you checked 'Yes,' **do
  not complete the rest of Part II.**"* ⚠ Hardcoding "No" both contradicts that
  taxpayer's answer AND transmits the rest of Part II, which the form says to
  omit.
- **Line 4** — *"Could you (or your spouse if filing jointly) be claimed as a
  qualifying child of another taxpayer for the year entered on line 1?"*
  Caution: *"If you (or your spouse if filing jointly) answer 'Yes' to question
  4, **you cannot claim the EIC.**"* ⚠ **Sign: hardcoding "No" transmits an EIC
  claim the taxpayer is BARRED from — it OVERSTATES the refund, on a sworn
  statement.** `taxpayer_claimed_as_dependent` is NOT this fact ("claimed as a
  dependent" ≠ "could be claimed as a qualifying child"); verify before reusing.
*This is the item's "preserve every required Part I-IV answer" ask, and it is a
live e-file defect rather than a lane gap.* Fix it in the same unit.

**⚠⚠ THE RULE STUDIO SPEC IS THE s238 TRAP AGAIN — a 200 that is not a green
light.** `lookup/8862/export/` answers 200 so the 404-STOP gate does not fire,
and the export is **`"status": "draft"`, version 1, with 6 facts, ONE rule, 6
`line_map` entries, 1 diagnostic and 2 tests**. Four of those six line_map
entries are pseudo-lines (`part_ii`, `part_iii`, `part_iv`, `part_v`) that
collapse an **entire Part into a single boolean**. The printed face has ~40
numbered lines, and the batch item itself names answers the spec cannot
represent at all — residency days, age, the line-3 income-reporting question,
the line-4 qualifying-child-of-another question, per-child Section A rows.
**Implementing it faithfully would ship exactly the form we already have.**
Build the s222/s223/s238 way instead: the IRS face + `IRS8862.xsd` + the
`F8862-*` business rules are the real specification, written up in
`server/specs/_8862_source_brief.md`. ⚠ **Grep the business rules FIRST**
(`1040_Business_Rules_2025v5.*.csv`) — s223's lesson is that MeF rules are often
NARROWER than the printed face and are the real spec.

**The face's real line inventory (dumped from `f8862.pdf`, Rev. 12-2025 — the
draft spec's six pseudo-lines do not describe this form):**
- **Part I**: 1 tax year · 2 credit checkboxes (EIC / CTC-ACTC-ODC / AOTC).
- **Part II (EIC)**: 3 income-report-only? · 4 QC-of-another? · **Section A
  (with children)** 5a-c child names, 6 Schedule EIC shows a QC?, 7, 8 ·
  **Section B (childless)** 9a/9b days in the US, 10a/10b ages, 11a/11b
  claimed as a dependent?
- **Part III (CTC/ACTC/ODC)**: 12a-d child names · 13a-d other-dependent names ·
  14, 15 per child · 16, 17 per child AND per other dependent.
- **Part IV (AOTC)**: 18a-c student names · 19a, 19b per student.
- **Part V**: qualifying child of more than one person.

⚠ **The reported packet is the Part II SECTION B (childless EIC) path** — line
9a days in the US, line 10a age, line 11a claimed-as-a-dependent — and **Section
B has no representation anywhere in the app**: not in the model, not in the
render map, not in the MeF builder. That is the actual reason the packet cannot
be entered.

**Derivable vs. must-be-keyed (the item says "derive taxpayer/spouse age where
appropriate", so decide each one deliberately):**
- **10a/10b ages — DERIVE** from `date_of_birth` against the line-1 year. Never
  key an age; it would let a payload contradict the DOB on the same return.
- **11a/11b claimed-as-a-dependent — DERIVE** from
  `Taxpayer.taxpayer_claimed_as_dependent` (and the spouse twin, if one exists —
  check).
- **Line 4 QC-of-another — KEY IT.** ⚠ This is NOT the same fact as 11a:
  "could be claimed as a **qualifying child**" (§152(c)) is narrower than
  "claimed as a **dependent**", and the face asks both separately with different
  cautions. Reusing 11a for 4 would be the s239 "one sentence, two tests" error.
- **Line 3, 9a/9b — KEY THEM.** Nothing in the app knows either.
- **16/17/19a — DERIVE** from the existing dependent/student facts (that is the
  fabrication fix above).

**What exists / what is missing:**
- EXISTS: `f8862_2025.py` (render, but keyed to the same collapsed
  `part_ii`/`part_iii`/`part_iv` booleans), the `IRS8862` MeF document,
  `seed_8862.py` (FormFieldValue lines on the same collapsed shape),
  `renderer.render_8862` gated on `eic_disallowed_prior_year` AND not a math
  error, and `D_EIC_008` which surfaces the requirement.
- MISSING: the input model entirely (→ migration + **RLS default-deny, the
  new-table rule**), the real per-line facts, the `backentry.v1` section, and
  the diagnostics the item asks for (missing answers, incompatible credit
  claims, claiming a disallowed credit with no certification).
- ⚠ The render map and `seed_8862.py` will BOTH need re-cutting onto the real
  line inventory — they were built to the draft spec's collapsed shape, so
  "render and transmit the filed form" is not satisfied by what exists today.

⚠ Read the s241/s241b pair before starting — **the two sessions reached OPPOSITE
answers on the same question**, and that contrast is the reusable lesson:
- Form 5329's compute reduced its rows to `{r.owner: r for r in ...}`, so one
  row per owner was a real contract the DB did not enforce → constraint added.
- Form 8863's compute ITERATES a list and Parts I/II aggregate, so several rows
  are normal → **a constraint would have been a defect**, and a test now pins
  that compute still iterates so the reasoning cannot be silently outrun.
**Check the consumer before deciding uniqueness. Do not pattern-match.**

⚠ Read the s241/s241b pair before starting — **the two sessions reached OPPOSITE
answers on the same question**, and that contrast is the reusable lesson:
- Form 5329's compute reduced its rows to `{r.owner: r for r in ...}`, so one
  row per owner was a real contract the DB did not enforce → constraint added.
- Form 8863's compute ITERATES a list and Parts I/II aggregate, so several rows
  are normal → **a constraint would have been a defect**, and a test now pins
  that compute still iterates so the reasoning cannot be silently outrun.
**Check the consumer before deciding uniqueness. Do not pattern-match.**

⚠ Also carried from both: refuse derived/aggregate columns **BY NAME**
(s238 doctrine, now applied three times), and use the `link_key` carrier shape
for any FK a packet cannot know as a UUID.

The rest, roughly by size: #3 pre-2019 alimony ≈ #9 Form 1099-PATR ≈ #10 Form
4547 + 8879-TA ≈ #2 GA education credit + IT-QEE-TP2 ≈ #5 Schedule H << #1
1040-X amended lifecycle (large).

⚠ **#10's source check is CLOSED — Form 4547 is REAL** (Rev. December 2025,
created by OBBBA; filed WITH the current-year e-filed return, so its MeF leg is
part of the build). ⚠ **#9 does NOT go through the 404-STOP gate** — per s222 no
RS spec exists for any information return.

### ⭐ STILL UNBLOCKED, still passed over — now NINE sessions
- **Form 8853 Section C.** Spec cached at `server/specs/8853_sec_c_spec.json`;
  `lookup/8853_SEC_C/export/` returns 200; all four legs pending. Read the s232
  write-up in `STATUS_ARCHIVE.md` first — Schedule 1 line 8e is COMPOSED not
  owned, and line 25 FLOORS AT ZERO though the printed face does not say so.
  ⚠ **s241 gave this a second reason to happen**: Form 5329 line 36 takes its
  value "from Form 8853, line 8" and **no `Form8853` model exists at all**, so
  the Archer arm of the 5329 excess chain cannot be reconciled the way the HSA
  arm now is (`D_5329_006`). Sections A/B and Section C are near neighbours —
  doing them in one pass is the cheap order.

### ⛔⛔ THE E-FILE GAP LIST — still TWO named documents, unchanged
- **`IRS4797` (s240)** — no 1040-side Form 4797 document builder; MeF rejects
  the return without it (`S1-F1040-118-01`, Reject, Active). s240 added a
  refusal so it fails loudly at composition. **The higher-volume of the two** —
  every disposition, installment sale, like-kind exchange AND K-1 §1231 return.
  The 1120-S `IRS4797` builder is the worked example.
- **`IRS1116`** — the oldest live e-file gap. A full-path Form 1116 return is
  paper-only in code. s238's `IRS8379` build is a worked example end to end.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — 6 open** (2, 4, 5, 6, 8, 10);
  **BATCH-002 — items 9 and 10 open as to their COMPUTE half only** (both
  RS-blocked on the missing NOL spec); **BATCH-003 — 6 open** (1, 3, 6, 8, 9,
  10 — ⚠ build #3, mixed passive/nonpassive on one K-1, TOGETHER with the s239
  Georgia work); **BATCH-004 — 8 open, all triaged**. Every file carries a
  result annex naming what is done and what is blocked; read it before
  starting. ⚠ None of the four has moved to Done, deliberately.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

### ✅ s241b in one paragraph
BATCH-004 #6 built: `education_students`, one row per Form 8863 Part III
student. Refuted as stated, real as a lane gap — the form has been complete
since migration 0071 and only the importer was missing. **The finding worth
carrying is a negative one**: the uniqueness constraint s241 had added to Form
5329 hours earlier would have been a *defect* here, because `compute_8863`
iterates its rows and Parts I/II aggregate across students. Checking the
consumer rather than pattern-matching the previous session is what caught it,
and a test now pins that compute still iterates. Also settled that
`student_name`/`student_ssn` live on the row rather than on the dependent link
(MeF refuses a student without them), and built the `dependent_key` carrier for
the student-to-dependent link. §25A answer key hand-derived and tied: $2,500 →
$1,000 refundable + $1,500 nonrefundable.

### ✅ s241 in one paragraph
Two batch items turned out to be one section. BATCH-004 #7 (Form 5329 Part III,
traditional-IRA excess) and BATCH-003 #2 (Part VII, HSA excess) are the same
`_excess_part()` helper inside one function, and both were lane-only gaps — the
engine has computed Parts I–IX and routed both owners' totals to Schedule 2 line
8 since migration 0119. Built `form_5329s` carrying the leaves of **all nine
parts**, with `owner` required and every derived line refused by name; built the
excess roll-forward the item asked for; added `D_5329_006` reconciling line 44
against the return's own Form 8889 line 16. Behind them sat a defect nothing
could report. Both answer keys tie to the filed returns ($110 and $15).

### ⚠⚠ THE FINDING WORTH CARRYING — a dict keyed by a row attribute makes duplicates VANISH, not double
`compute_5329_db` and the diagnostics' `_f5329_state` both do
`{r.owner: r for r in Form5329.objects.filter(...)}`. That is an ordinary,
readable line of Python, and it silently encodes a uniqueness contract the
database never enforced. A second row for the same owner does not double-count —
**the last one wins and every earlier row's additional tax is dropped**, with no
error, no diagnostic and no rendered form. Measured before the fix: a taxpayer
row with $1,840 of prior traditional-IRA excess plus a second taxpayer row with
$250 of prior HSA excess wrote Schedule 2 line 8 = **$15.00 instead of $125.40**.
*The general shape: when compute reduces a row SET to a dict keyed by one field,
that key is a uniqueness constraint — and if the DB does not have it, the
overflow is invisible rather than wrong.* The contrast is the tell: this form's
own siblings, `Form8606` and `HSAAccount`, ITERATE their rows, so a duplicate
there at least shows up as a double count someone would notice. ⚠ **Sign:
UNDERSTATES tax** — the direction nobody reports. Now closed at the DB
(migration 0278), the browser POST, the re-owner PATCH, and staging.
⚠ An independent confirmation nobody had connected: `ReturnData1040.xsd` caps
`IRS5329` at **maxOccurs 2** — the schema has said "one per owner" all along.

### ⚠ THE SECOND ONE — a form that names its own source is telling you what to reconcile
Form 5329 line 44 reads, on the printed face, *"2025 distributions from your
HSAs **from Form 8889, line 16**"*. Line 16 is the TAXABLE portion; a fully
qualified distribution puts -0- there. The reported packet has a fully qualified
$631 distribution, so keying the gross on line 44 wipes out the $250 carried
excess and its $15 tax — **and nothing visible changes on the face, because the
part just prints as zero**. Where a form line states its source in words, that
sentence is a reconciliation the app can run; `D_5329_006` runs it. ⚠ Line 36
says the same thing about Form 8853 line 8 and **cannot be reconciled — there is
no Form8853 model** (DEFERRAL_AUDIT).

### ✅ s241m in one paragraph — the fix nearly reproduced the bug it prevents
Form 1099-PATR's model, income rules and 8z composition. Two things the batch
item never mentioned came out of the IRS instructions: **box 1 is not
unconditionally income** (dividends on personal-use or capital/depreciable
business property "are not taxable", and the second case *reduces asset basis*),
and **boxes 6-9 must not feed anything** because the app already keys the patron
§199A facts — routing them would be s234 all over again. ⚠⚠ **But the sharpest
moment was the composition itself.** Line 8z tolerates one final writer, so PATR
had to ride `compute_1099misc_db` rather than add a fourth — and that writer
*returns early when there is no 1099-MISC*, so a PATR-only return would have had
its patronage income **silently dropped**. *The fix for a disappeared-number
class was one line away from creating a new instance of it.* Both the early
return and the disengage path now carry the share, and both are pinned.

### ✅ s241j in one paragraph — a completeness check is not a correctness check
BATCH-004 #3 asked for an alimony document model; 19a/19b/19c already existed as
spec inputs, already required the SSN and the date, and 19a already reached AGI.
**The defect was one level down: `D_SCH1_003` checked that the line-19c date was
PRESENT and never what it SAID.** So a divorce executed in 2020 deducted alimony
and reduced AGI — ⚠ overstating the deduction — while a rule named "alimony
completeness" reported the return clean. *A rule that validates the SHAPE of a
fact can read as though it validates the FACT; check what it actually asserts.*
`D_SCH1_007` now enforces the TCJA §11051 repeal (Topic 452 fetched verbatim,
boundaries pinned at 12/31/2018 and 01/01/2019), and `D_SCH1_008` prompts on the
modification arm because **a post-2018 modification alone does not end the
deduction** and the app stores neither the date nor the express election —
guessing there would deny a deduction, the opposite sign. ⚠ The severity was
**proved**: a two-way compute pins AGI down by exactly $8,400.

### ✅ s241i in one paragraph
Form 8862 COMPLETE. Section A's print and transmission shipped together
deliberately; line 6 is the Section A/B router (one fact, one derivation); line
8 prints only for an in-year birth ("Otherwise, skip this line"); the death half
is blank because `Dependent` has no date-of-death field.

### ✅ s241h in one paragraph — I found the hole in my OWN guard first
Parts III/IV of Form 8862 now print (16 → 87 map entries), with the render feed
mirroring `build_irs8862` so paper and XML cannot diverge. **⚠ The widget
numbering is not contiguous across lines** — line 16's children are
`c2_11..c2_14`, its OTHER DEPENDENTS `c2_15..c2_18`, and line 17 restarts at
`c2_19` — so "eight consecutive widgets per line" would put other-dependent 1 in
child 4's column. Every index was read off the PDF. **Then the guard itself was
audited before being trusted**: the per-column check re-derives each box from its
printed label, but "Other dependent 1" is printed on BOTH line 16's row and line
17's, so a copy-paste between them would have satisfied it. Added a row-identity
check and injected exactly that copy-paste — it failed, naming the duplicated
row. *A positional guard is only as good as the thing that distinguishes the
rows; check what your own test CANNOT see.*

### ✅ s241g in one paragraph — the lesson is about what a test PROVES
Retired the `inspect.getsource(seed_builtin_rules)` class: 24 assertions, 24
files, now `assert_registry_wired()` in `tests/registry_asserts.py`. The sweep
went **22 failed / 878 passed → 900 passed, 0 failed**. **⚠⚠ THE CHEAP REPAIR
WAS REJECTED AND THAT IS THE WHOLE POINT.** `all_registered_rules()` imports
each registry by name, so repointing the same sniff at it would have gone green
in one line — and still proved nothing. **Demonstrated, not argued**: the teeth
test injected `RULES_8889 = []` into that function, which CONTAINS the literal
`"RULES_8889"`, so a repointed sniff would have PASSED while the family reached
neither the seeder nor the database. The new assertion failed instead, naming
all 8 orphaned codes. *A test that searches for a NAME proves the name exists;
only a test that follows the VALUE proves the wiring.*

### ✅ s241f in one paragraph
BATCH-004 #8's diagnostics: `D_8862_002` (required but incomplete, naming the
missing lines — every unanswered question on that form has an answer that BARS
the credit) and `D_8862_003` (Part I line 2 vs the credits actually claimed,
both directions). One of its 12 tests caught a trap in its own setup: the
8862-row helper was a filter-and-update against rows that only exist after an
engaged-EIC recompute, so two tests had been passing by absence.

### ✅ s241e in one paragraph — and the lesson is about MY OWN earlier change
The Form 8862 field map was a deliberate coarse data-map whose docstring said
the granular sections were *"left blank for the preparer to complete by hand — a
faithful data-map, not an adjudication."* **True until s241c/s241d gave the app
real answers and began transmitting them** — after which a blank printed face
beside a populated transmission is the paper and the XML disagreeing about what
the taxpayer swore. *Fifth occurrence here of "adding support makes an existing,
correct rule wrong" (s225, s233, s238, s240) — and the first where the earlier
change was mine, two sessions back.* Mapped every Part I/II line the app knows.
⚠ `[0]`=Yes / `[1]`=No was **verified positionally** against the printed labels
(the s236 Form-7203 trap), with a test that re-derives it from the PDF and was
**proven by injecting the swap**.

### ✅ s241d in one paragraph
Two more Form 8862 legs, **both found in the XSD rather than on the printed
face** (the s223 rule: the schema decides what transmits). Part II **Section B**
— the childless-EIC path, which is exactly the reported packet's shape — was
stored after s241c but never emitted; it now is, with all three of its
schema-required members present or a refusal, because each one carries a caution
that bars the EIC outright. And **`ODCPersonInformationGrp` had never been
emitted at all**: every ODC person was travelling inside
`CTCACTCChildInformationGrp`, asserting `QualifyingChildInd` — a qualifying-child
claim an ODC qualifying relative cannot make, on the wrong line of the form.
Full e-file sweep 1,132 passed / 0 failed.

### ✅ s241c in one paragraph
BATCH-004 #8 partially built. The item reads as "no input model", but the real
defect was that `build_irs8862` **fabricated five of the taxpayer's sworn
answers** while its own docstring promised *"the 8862 can't contradict the
claim"*. Two of the five overrode facts the app stores AND uses to compute the
credit being certified (`Dependent.citizenship_status`, the student AOTC facts),
so the certification could contradict its own claim. All five now derive or
refuse. New `Form8862` model holds only the three facts the return cannot
otherwise know. **The correction worth remembering came from reading the ATS
fixture, not the triage**: line 4 was going to be keyed until scenario 5 showed
the taxpayer already carries `eic_qualifying_child_of_another` — so it derives,
and a keyed copy would have been the s234 two-sources defect. Render, Section B
emission and diagnostics remain open and are named in the annex.

### ⚠⚠ THE s241 / s241b PAIR — the same question, OPPOSITE right answers
s241 found that `compute_5329_db` reduced its rows to `{r.owner: r for r in …}`,
making a duplicate row VANISH, and closed it with a DB constraint. Hours later
s241b faced the identical question on `EducationStudent` — and the right answer
was the reverse: `compute_8863` **iterates** a list and Parts I/II aggregate
across students, so several rows per return is the normal case and a constraint
would have destroyed the multi-student return. *The transferable rule is not
"add the constraint" — it is **read what the consumer does with the row set,
every time**. A dict-by-attribute is a uniqueness contract; a list is not, and
the two are one character apart at the call site.* s241b pins the premise with a
test asserting `compute_8863_db` still iterates, so a future refactor to a dict
fails loudly instead of silently dropping a student's credit.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **s241n: NONE.** The five `D_1099PATR_*` rules fire only on returns that
  carry a 1099-PATR row, and none exists until a preparer or payload makes one.
- **s241m: NONE.** `Form1099PATR` rows only exist where a preparer creates one,
  and the 8z composition is unchanged on every return without one — the
  existing 27 1099-MISC/8z tests are green and the sweep was 537/0.
- **⚠ s241j MOVES DIAGNOSTICS, and one class of return should CHANGE.** Any
  return deducting alimony under a line-19c instrument dated 2019 or later now
  fires `D_SCH1_007` (**error**) where it was silent — and that deduction is
  genuinely not allowed, so those returns are wrong today and the AGI must
  come back up when the amount is removed. Every pre-2019 alimony return gains
  `D_SCH1_008` (info). No computed value changes on its own.
- **⚠ s241h MOVES PRINTED OUTPUT.** A rendered Form 8862 now fills Part III's
  child and other-dependent grids (lines 12-17) and Part IV's student grid
  (18-19), where they were blank. The values are the same ones already being
  transmitted, so nothing new is asserted — the paper simply stops lagging the
  XML. No dollar figure changes.
- **⚠ s241e MOVES PRINTED OUTPUT.** A rendered Form 8862 now shows Part II
  lines 3 and 4, and on a childless EIC claim Section B's 9a/9b, 10a/10b and
  11a/11b, where those areas were blank. ⚠ An unanswered question still prints
  BLANK, never "No". No dollar figure changes.
- **⚠ s241d MOVES E-FILE OUTPUT.** (a) A childless EIC 8862 now emits Part II
  Section B, and **refuses** when the line-9 day count, a date of birth, or the
  Rule 12 answer is missing — those returns previously transmitted with Section
  B silently absent. (b) An ODC-only dependent moves from
  `CTCACTCChildInformationGrp` to `ODCPersonInformationGrp` and **stops
  asserting `QualifyingChildInd`**. ⚠ Sign: both are corrections toward the
  truthful, narrower claim. No dollar figure changes.
- **⚠ s241c MOVES E-FILE OUTPUT, and this one is not "none".** Any return
  transmitting a Form 8862 changes: (a) an EIC 8862 whose line 3 is unanswered
  now **REFUSES at composition** instead of transmitting a fabricated "No" —
  answer it on the new `form-8862` endpoint; (b) a CTC/ODC dependent whose
  `citizenship_status` is `nonresident_alien` or `mexico_canada_resident` now
  transmits line 17 = **false** where it said true; (c) a student failing the
  half-time / first-4-years / felony tests now transmits line 19a = **false**.
  ⚠ Sign on (b) and (c): the OLD value overstated eligibility, so the new one is
  the correct, more conservative answer. No dollar figure changes.
- **s241b: NONE.** No compute changed; `education_students` rows only exist
  where a payload or preparer creates one.
- **s241: NONE.** No compute changed. `form_5329s` rows only exist where a
  payload or preparer creates one; `D_5329_006` fires only where a Form 5329 row
  and an HSA for the same owner already disagree; the roll-forward touches only
  a NEW year's return being seeded. The one blocking change (the duplicate-owner
  refusal) reaches only a state that was already producing a wrong number.
- Carried from s240: a passive or PTP K-1 carrying a net §1231 LOSS now fires
  `D_8582_MULTIFORM` (RED) or `D_K1_PTP_LOSS` (warning) where it was silent; any
  1040 with a non-zero Schedule 1 line 4 now refuses at MeF composition.
- Carried from s239: Roth 1099-Rs (codes J/T/Q) move from 5a/5b to 4a/4b; any
  Georgia return with a partnership K-1 moves income between RIE L2 and L13;
  an engaged Form 8606 changes RIE L11; a code-U 1099-R un-blanks the whole
  pension taxable column (the largest mover).
- Carried from s236: a Georgia return with a passive K-1 whose loss was partly
  or wholly suspended gets a different RIE line 13.
- Carried from s235: Georgia dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted
- ✅ **RETIRED IN s241g — the `inspect.getsource(seed_builtin_rules)` class is
  GONE.** 24 assertions across 24 files, all replaced by
  `tests/registry_asserts.assert_registry_wired()`, which checks that every code
  a registry DECLARES is actually returned by `all_registered_rules()`. The
  `-k "diagnostic"` sweep went from **22 failed / 878 passed → 900 passed, 0
  failed**. It had cost time in six consecutive sessions. ⚠ **The cheap repair
  was deliberately rejected**: repointing the sniff at `all_registered_rules`
  would have gone green instantly (that function imports each registry by name)
  but still proved nothing — the teeth test injected `RULES_8889 = []`, which
  CONTAINS the literal, so a repointed sniff would have passed while the family
  reached neither the seeder nor the DB.
- **`--reuse-db` cross-module contamination**: `test_backentry_cleanup.py` (3,
  s225 — a `D_PREPARER_001` duplicate-key on rule seeding; fails alone too) and
  `test_mappings.py::TestApplyMappingAmbiguousFederalReturn` (3, s239).
- **`test_topic7_input_leg.py::TestEICFacts::test_non_engaged_return_leaves_27a_quiet`**
  — pre-existing, verified at pristine `6e819b5` in a worktree (s235). Not diagnosed.
- **`test_1040.py` — 6 pipeline tests**, `MultipleObjectsReturned`. Their `_fv`
  helper does an **unscoped** `FormFieldValue.objects.get()`. Pre-existing (s234).
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  `ValidationError`. Pre-existing (s219). Not diagnosed.
- `test_4868.py` — 4 tests on the Schedule 3 line-10 feeder (s217). ⛔ KEN.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  batch-005 #9 PTET-gate class, red since s212.
- ✅ **FIXED in s241 (they had been red and unlisted):** the two `D_RET_`
  registration counts in `test_topic5_compute_leg.py` and
  `test_topic5_diagnostics_leg.py` — hand-counted `== 10`, RED since s239 added
  D_RET_011 without re-pinning them. Both now read the roster from
  `RULES_RETIREMENT`. Also the proforma producer's key-contract guard, which had
  never gained `_k1_basis_704d` (s228) or `_carryforward_attributes` (s235) and
  was passing only because its fixture emits neither.
- **Client typecheck**: 55 error lines standalone. s241 touched no .tsx.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). `--reuse-db`.
- **A broad `-k` sweep is SLOW and blows the 600s Bash timeout** — s236's
  ~1,100-test sweep took **21½ minutes**. Run it with `run_in_background: true`.
  ⚠ Keep the `-k` terms tight: `k1` matches hundreds of node ids, `rie` matches
  "ret**rie**ve". (s241's `-k "retirement or 5329 or 8889 or backentry or
  schema"` = 685 tests / 7m38s — fine, and it ran in the background.)
- ⚠ `--create-db` does NOT reliably drop an existing test DB here. To prove a
  red is pre-existing, use a `git worktree` at a pristine SHA with the MAIN
  venv's interpreter, and copy `server/.env` in (s235).
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout
  loses ALL output — redirect to a file and tail it.
- ⚠ A `poetry run python script > file` redirect BUFFERS. Use `-u`.
- ⚠ A stdout **redirect goes through cp1252** on this box and dies on ligatures
  and the U+2212 minus. Write UTF-8 from inside Python.
- ⚠ `manage.py shell -c "..."` prints nothing; a multi-line `python -c` through
  the Bash tool also silently produces no output. **A script file under the
  scratchpad also fails — `config` is not importable from there.** Copy the
  script INTO `server\`, run it, delete it (s239). **Simplest reliable probe: a
  throwaway `tests/test_zz_*.py` with `-s` print statements, deleted after
  (s240/s241) — it gets fixtures, the DB and the app context for free. s241 used
  exactly this to MEASURE the duplicate-owner drop before fixing it.**
- ⚠ **`poetry run` must be invoked from `D:\dev\delvio-tax\server`** — from the
  repo root it fails with "could not find a pyproject.toml". This bites the
  schema generator too: run it as `poetry run python -u ..\scripts\gen_backentry_schema.py`
  FROM `server\`, never from the repo root.
- ⚠ **Windows `python` cannot read the Bash tool's `/tmp`** — they are different
  filesystems. Write shared files to the scratchpad path, not `/tmp` (s240).
- ⚠ The Bash tool produced NO output at all for a `poetry run python -c` this
  session where the identical PowerShell command worked. **Prefer PowerShell for
  `poetry run` on this box** (s241).
- ⚠ A Cloudflare-protected law site (justia) 403s both WebFetch and curl.
  **The in-app browser (`preview_start` + `get_page_text`) got the full
  verbatim Georgia reg** where both failed (s239).
- ⚠ Lane API shapes that cost time (s241): **staging answers 201 even for an
  invalid payload** — the verdict is `row["status"]`, not the HTTP code; the
  return CRUD routes are `/api/v1/tax-returns/…` (not `/returns/`) and the
  detail route **needs its trailing slash** or you get a 301; `filing_status`
  is `"mfj"`, not `"married_joint"` (varchar(10)).

### 🔎 Carried for triage — NOT claims
- **From s241**: `Form8606` and `HSAAccount` both allow duplicate owners and
  their compute ITERATES, so a duplicate DOUBLE-COUNTS rather than vanishing.
  `views.py:623` (the 8606 proforma roll) guards with `.exists()`, but the
  browser POST does not. Not measured; not a claim. The 5329 constraint is the
  worked example if it turns out to be real.
- **From s234, potentially large and still unchased**: a materially-participating
  1120-S K-1 carrying **$250,000 of nonpassive ordinary business income never
  reached Schedule 1 line 5 or 1040 AGI** — line 9 was identical with the K-1 at
  $0 and at $250,000 — with `SCHEDULE_E` AND `FORM_8582` both seeded, while
  `schedule_e_non_1411_income` *did* see the same row. Repro:
  `server/tests/test_8960_line4b_clamp.py`, `_build(k1_ordinary="250000")`.
- Carried (s229): a filed, exact-tie 1040 shows worksheet drift on a bare
  recompute (`1040_SCHD_WS` clc_1/clc_3, −5,491 each), face still an exact TIE.

### ✅ KEN DECISIONS OUTSTANDING
- **⛔ KEN (s231, carried): §38(c)(6)(A), the MFS threshold.**
  `compute_3800.SEC38C1_THRESHOLD` is a flat $25,000; the statute makes it
  **$12,500** for an MFS taxpayer whose spouse has any business credit.
  **⚠ The sign: this OVER-allows.** Requires nothing from Ken to BUILD.
- **⛔ KEN (s227)**: the out-of-scope-state packet-disposition marker.
- **⛔ KEN (s230)**: Form 6765 Section G becomes REQUIRED for tax years
  beginning after 2025; the RS spec must be re-authored before a TY2026 season.
- The one live external item: **1040 v5.4 business rules still not in hand,
  active 2026-08-10** (v5.4 schemas ARE on disk; 1041 v5.5 closed). ⚠ s240 read
  the **v5.3** rules for `S1-F1040-118-01`; re-check it against v5.4 on arrival.

### RS AGENDA
- **⛔ BLOCKING two batch items: THERE IS NO NOL SPEC.** `lookup/172/`,
  `lookup/NOL/`, `lookup/FORM_172/` and `lookup/1045/` all return 404.
  BATCH-001 #4 and BATCH-002 #10 both ask for Form 172, the post-2017
  limitation and the utilization ordering. **The preservation half is built and
  the pools are safe — only the computation waits.** Still the single
  highest-value RS authoring order on this list.
- **NEW (s241b/triage): the `8862` spec is a DRAFT that collapses each PART into
  one boolean.** `lookup/8862/export/` answers **200** — so the 404-STOP gate
  waves it through — and the export is `"status": "draft"`, version 1, with 6
  facts, ONE rule, 6 `line_map` entries (four of them pseudo-lines `part_ii` …
  `part_v`), 1 diagnostic and 2 tests, against a printed face of ~40 numbered
  lines. **This is the second time a draft spec has looked like permission**
  (s238's `8379` was the first, and its `status` field is STILL not checked
  anywhere). Re-author `8862` per-line, or mark it clearly as non-authoritative.
  ⚠ The app-side consequence is already live and is NOT cosmetic — see the
  fabricated Part II line 3/4 answers under NEXT UNIT.
- **NEW (s241): the `5329` spec says nothing about the roll-forward or about
  Part VIII.** Two gaps. (a) The spec has no rule stating that each part's
  total-excess line becomes next year's prior-excess line — s241 built the roll
  from the printed FACE's own wording (line 9 = "line 16 of your 2024 Form
  5329"), which is sound but unspecced. (b) **Part VIII (ABLE) has line 50 and
  no prior-year line at all**, so the form provides no chain — but whether an
  uncorrected ABLE excess is in fact taxed again the following year is a §529A
  question the FORM does not answer. s241 deliberately did not guess; the roll
  omits Part VIII and a test pins the omission. **Author the answer.**
- **NEW (s241): the `5329` spec does not state where lines 36 and 44 come
  from.** Both name a source ON THE PRINTED FACE — line 44 "from Form 8889, line
  16", line 36 "from Form 8853, line 8" — and neither has a spec rule, so
  nothing declared that line 44 is the TAXABLE distribution rather than the
  gross. That silence is the whole of `D_5329_006`'s defect.
- Carried (s240): `R-8582-MULTIFORM`'s no-silent-gap clause is now FALSE as
  written (it cites `D_K1_SEC1231`, retired 2026-06-30) and should be
  re-authored; `R-8582-WS-NET` says nothing about which OTHER forms an
  activity's losses can land on; the `4797` spec has no rule for the K-1 §1231
  feed, so nothing declares whether the amount is pre- or post-§469.
- Carried (s239): `R-RET-CODE` has been outrun three times (codes 6, W, U) —
  re-author from the current i1099-R Table 1 in one pass. The `500` spec still
  has NO rule governing what feeds RIE lines 1/2/6-13, where four defects have
  now been found; record the earned/unearned split BY ENTITY TYPE and that the
  $5,000 earned cap is statutory.
- Carried (s238): the `8379` spec is a DRAFT whose `line_map` covers 4 of ~20
  lines and returns 200, so the 404-STOP gate waved it through. **The export's
  `status` field is still not checked anywhere.**
- Carried (s236): `R-SCHA-CHARITABLE` models only three buckets while the K-1
  states **seven** — 1120-S box 12 codes A–G — so **B, D, F and G have no home
  and are REFUSED at both write paths**. ⚠ Sign: a refused code is not deducted
  AT ALL — it overstates tax.
- Carried (s236): the `500` spec's RIE line_map has no **RIE-13**.
- Carried (s235): the `SCHEDULE_A` charitable carryover modelled as ONE
  aggregate (BATCH-002 #9 needs pools by source year AND limitation class); the
  `500` spec typing line 7a as `input` when the app now DERIVES it.
- Carried (s234): R-STD-04 silent on the dependent worksheet's line 1; the
  FORM_8960 `R-8960-INCOME` rule silent on line 4b being bounded by line 4a.
- Carried (s232): the `[WO-SOURCETYPE-RECON]` additions; the export serializer
  omitting `requires_human_review`.
- Carried: the s231 Form 3800 five spec defects; the s230 Form 6765 items
  (a)-(e); the **1065 Schedule K-1 box-15 letters** for §41 and §45R (still
  URGENT — `D_3800_008` excludes those credits until both are verified);
  s228 `D_K1B_FULLY_ALLOWED`; s226/s227/s224/s223 unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
