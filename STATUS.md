# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-10 (s241t). **BATCH-004 #2 (Georgia QEE credit) — legs
2-3 built** (`ce8fd6b`, migrations 0285 + 0286 RLS): `GeorgiaCredit` /
`GeorgiaQEEDetail` and `compute_ga_qee.py` carrying the statute, **both
carryforward regimes included**. ⛔ #2 NOT finished — lane, render and
diagnostics remain. Eighteen units today; **BATCH-004 is 7 of 10 with #2 three
legs of six done.***

*Previous (s241r): ✅✅ #10 (Form 4547) COMPLETE, all six legs (`e2dce0e`);
(s241q) its lane + eight diagnostics (`1158887`); (s241p) brief + models
(`c113bd3`, migs 0283 + 0284).*

*Previous (s241o): ✅ BATCH-004 #9 (Form 1099-PATR) COMPLETE — the Georgia RIE
feed and the browser CRUD surface, and the unit found a defect in its OWN
previous session's diagnostic (`19d9579`).*

*Previous (s241n): the PATR lane + five diagnostics (`a841368`); (s241m) the
model + 8z composition (`b4c949e`, migs 0281+0282) — ⚠ 8z's SINGLE writer
returned early without a 1099-MISC, so a PATR-only return would have silently
dropped its income; (s241j) the alimony TCJA repeal gate; (s241i) ✅ Form 8862
COMPLETE; (s241g) the `inspect.getsource` false-red class RETIRED.*

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

### ⭐⭐ NEW AND UNTRIAGED: **1040 BATCH-005 was posted 2026-08-10 12:43**
`1040\CC Changes\CC_CODE_CHANGES_1040_BATCH-005.md`, **10 items, no triage
annex yet.** It did not exist at the previous boot. Headlines, by rough size:
ordinary digital-asset income on S1 line 8v (#1) · the line-27c EIC opt-out
election (#2) · a **foreign taxpayer mailing address** (#3) · Form 8839 adoption
credit + carryforward (#4) · **generic described 8z other income with no
document** (#5) · a **direct rental sale on Form 4797** (#6) · 1099-R disability
pension → 1040 line 1h (#7) · **Form 6781 §1256 and the 40/60 split** (#8) ·
jury-duty pay on 8h (#9) · **qualified passenger-vehicle loan interest on
Schedule 1-A** (#10).
⚠ Two of these touch work already in hand: **#5 is the third contributor
problem on 8z again** (state refund + 1099-MISC + 1099-PATR — read
`compute_1099misc.py`'s composition before designing a fourth), and **#6 needs
the `IRS4797` e-file gap**, which is already a named blocker below. **Triage
verify-first before building any of it.**

### ✅✅ BATCH-004 #9 (Form 1099-PATR) IS COMPLETE — s241l → s241o, four units
Source brief (verbatim, **Rev. April 2025**) · model + migs 0281/0282 ·
`compute_1099patr` + the 8z composition + box 4 → 1040 line 25b · the **Georgia
RIE L10 feed** · the `patr_1099s` lane section · five `D_1099PATR_*` +
`D_GA500_019` · `patr-1099s` CRUD + `SlateForm1099PATRScreen` + a nav tab.
⛔ **NO render leg and NO e-file leg, and that is a FINDING** (the s222
1099-MISC position): no 1099 has a field map or a manifest entry, and
`IRS1099R` is the only 1099 element in the 1040 MeF schema set. An information
return received by the taxpayer is an INPUT document — the 1040 neither prints
nor transmits it. The item's "render and transmit the source form" is N/A.

**The Georgia answer, since the item asserted it without qualification.**
Ga. Comp. R. & Regs. r. 560-7-4-.02 was fetched verbatim FIRST (the in-app
browser; s233/s236/s239 each found this feed wrong a different way). The claim
is CONFIRMED, with two qualifications the item does not state:
- **(4)(b)1**'s unearned list closes with *"and other similar income"* → the
  patronage is UNEARNED, on worksheet **L10 "Other income (losses)"**, which
  mirrors the federal line the income sits on. Never L7 — the 1099-DIV pull
  owns that and would clobber it.
- **(4)(b)1**'s earned list opens with *"the net business income earned by an
  individual from any trade or business carried on by such individual"* → a row
  routed to a Schedule C or F is **EARNED**, and is ALREADY in the base through
  that schedule's own net profit. **Only the 8z route contributes**; feeding the
  others would double count AND move them to the wrong (capped) portion.
- **(3)**: *"Only retirement income that is **included in Georgia taxable
  income**"* → the box-1 nontaxable carve-out must not reach Georgia, or the
  same dollars are subtracted twice.
- **(2)**: per owner — and a 1099-PATR is issued to ONE recipient TIN.

### ⭐ NEXT UNIT — **BATCH-004 #2, the Georgia QEE credit — legs 2-6.**
### ⛔ **THE DESIGN IS SETTLED: read `server/specs/_ga_qee_credit_source_brief.md`
### and BUILD. Do not re-triage, do not re-fetch the statute.**

✅ **LEG 1 DONE (s241s, `52c08ec`):** the source brief, carrying O.C.G.A.
§48-7-29.16 verbatim. The **404-STOP call is made and recorded** — every alias
(`IT_QEE_TP2`, `IT-QEE-TP2`, `ITQEETP2`, `500_SCH2`, `GA_QEE`, `QEE`) 404s, but
the statute states every limit, the liability cap, **both** carryforward regimes
and the no-double-benefit rule, so there is nothing to improvise (the s241p
three-part test; `lookup/1310/` and `lookup/4547/` are the precedents).

**⚠⚠ THE FINDING THAT DECIDES THE MODEL: TY2025 RUNS TWO CARRYFORWARD REGIMES
SIDE BY SIDE.** Current text = *"succeeding **three** years"*; pre-amendment
text = *"the succeeding **five** years"*; and **2024 Ga. Laws 598 §1-7, eff.
1/1/2025, applies "only to unused tax credits generated during taxable years
beginning on or after 1/1/2025."** So a TY2025-generated credit carries 3 years
and an older one carries 5, **both live in the same return**. ⚠ A single
`CARRYFORWARD_YEARS` constant is right for one population and wrong for the
other, and the error surfaces YEARS later when a pool expires early or late —
**the carryover pool must key its GENERATION YEAR**, not a remaining-years
counter. Both regimes were read from primary text (2024 code + 2022 code).

✅ **LEGS 2-3 DONE (s241t, `ce8fd6b`, migrations 0285 + 0286 RLS):**
`GeorgiaCredit` (one row per **CERTIFICATE**, with `source_year` as the
carryforward discriminator and a conditional unique constraint on the
certificate number) + `GeorgiaQEEDetail` (the IT-QEE-TP2 facts) +
`compute_ga_qee.py` (**writes nothing** — the statute expressed once, for the
diagnostics/render/reconciliation to share).
⚠ **MFS is NOT NAMED by §(b) and is REFUSED** — `cap_for` returns None, so the
caller refuses rather than guessing half-of-MFJ. ⚠ Only code 125 computes;
every other series-100 code is **reported by name**. ⚠ An unknown
`source_year` returns **None, not a default** — the sign cuts both ways.

**⛔ Remaining legs — do NOT record #2 as finished until these land:**
1. **Lane** — a `georgia_credits` section with a nested `qee_detail` dict (the
   `form_4547s` parent+child shape). ⚠ Register it in the **generator** too —
   that drifted twice (s227, s241q).
2. **Render** — the Georgia Schedule 2 grid + IT-QEE-TP2. ⚠ Neither is in
   `forms_manifest.json` yet.
3. **Diagnostics** — the item's list: reconciliation against GA-500 line 21,
   the **S1-5 addback reconciliation** (⚠ compare, never write), an unmodelled
   credit code, an expired carryforward pool, a missing certificate, and
   `sold_transferred` non-zero on code 125 (§48-7-29.16 permits no transfer).

**⚠ VERIFY-FIRST CORRECTED THE ITEM — carry this forward.** GA-500 **line 21**
and Schedule 1 **line S1-5** are BOTH already seeded as preparer inputs, so the
packet's $5,000 is **enterable today** in flattened form and the return is not
wrong. The gap is the DOCUMENT DETAIL (code, certificate, generated-vs-used,
carryover, the credit↔addback link), which is what the item's own second
paragraph says. *A build that "fixes" a correct return is measuring the wrong
thing.*

**⚠⚠ `S1-5` IS RECONCILED, NEVER WRITTEN.** §48-7-29.16(h)(1) makes the addback
a CONDITION ON THE CREDIT, and `S1-5` is *"Add: **other** additions"* — a shared
preparer line. Writing into it is the s230 shared-line defect (a DISAPPEARED
number). Compare, as `D_5329_006` and `D_1099PATR_199A` do.

**⚠ THE PACKET CANNOT TEST THE CAP.** MFJ with $5,000 expended is *exactly* the
(b)(2) limit, so it cannot tell capped from uncapped — and all five of its
figures being equal is a coincidence, not a rule (s219). **Use a capped case
too.** ⚠ (b)(3) is an OVERRIDE, not a fourth status: an LLC member / S-corp
shareholder / partner gets **$25,000** even when MFJ, and its "portion of income
on which such tax was actually paid" proviso needs a KEYED assertion.
**⚠ The pre-approved amount is NOT computable** — a statewide first-come queue
against a $120m cap that may be PRORATED. Transcribe it.

⚠ Then #5 Schedule H << #1 1040-X (large). ⚠ **#1 must honour `IND-476`** — a
Form 4547 on an amended return is a hard MeF reject and s241r's refusal enforces
it. ⚠ Read the s233/s236/s239/s241o Georgia history before any other GA work:
that feed has been wrong four separate ways and the `500` spec still has NO rule
for what feeds the RIE lines.

### ✅✅ BATCH-004 #10 (Form 4547 + 8879-TA) IS COMPLETE — s241p → s241r
⛔ **The design record is `server/specs/_4547_source_brief.md`** — do not
re-triage and do not re-fetch the IRS artifacts.

✅ **LEGS 1-2 DONE (s241p, `c113bd3`, migrations 0283 + 0284 RLS):** the source
brief, `Form4547` / `Form4547Child` / `Form8879TA`, and `compute_4547.py` (pure
predicates; **the form has NO compute leg** — nothing on it reaches the 1040,
Schedule 1, AGI or tax).

**⚠⚠ THE 404-STOP CALL, so nobody re-opens it.** `lookup/4547/`, `F4547`,
`FORM_4547`, `8879_TA` and `8879TA` **all 404**, and nothing is cached. Built
anyway, deliberately: the gate's own words are *"do NOT improvise the
implementation"*, and there is nothing here to improvise — `IRS4547.xsd` puts a
`<LineNumber>` on every element and carries **no amount** but
`PriorYearAGIAmt` (a signature-authentication input), and every eligibility test
is printed verbatim by the IRS. **The precedent is exact: `lookup/1310/` ALSO
404s and s223 built Form 1310 end to end** from the IRS form + XSD + business
rules, with a source brief — and 1310 is a *filed* form, so this is not the s222
information-return carve-out being stretched. Both forms are on the RS agenda.

✅ **LEGS 3 + 6 DONE (s241q, `1158887`, no migration):** the `form_4547s` lane
section (parent + nested `children` + a nested `form_8879ta`, ordered after
`dependents`; staging refuses only the UNTRANSMITTABLE — line 5's `xsd:choice`
violated, a US address with no county, no children, an unnamed responsible
party, >100 children; and **Form 8879-TA's Part I refused BY NAME**) and
**eight diagnostics** (`D_4547_AGE`, `_PILOT`, `_CITIZEN`, `_LINK`, `_UNKNOWN`,
`_AUTH`, `_AMENDED`, `_8879TA`).
⚠⚠ **The generator had ALREADY drifted and `children` is REQUIRED** — staging
validated both nested families but the published schema emitted neither, so an
author could not have discovered a field the server insists on (the s227 class,
same as `schedule_fs.other_expenses`). Fixed and pinned by a test reading the
generator's own `defs`, not the written `.json`.

✅ **LEGS 4 + 5 DONE (s241r, `e2dce0e`, no migration) — #10 IS COMPLETE.**
`f4547.pdf` registered in the manifest with its SHA256; `f4547_2025.py`;
`render_4547`; `build_irs4547` + `Form4547Source` / `Form4547ChildSource` +
`_extract_f4547s`, wired at the XSD position between `IRS4255` and `IRS4562`.

⚠⚠ **PRINT AND TRANSMIT HAVE DIFFERENT SHAPES, and that is the unit's whole
risk.** Two child columns on paper vs `maxOccurs="100"` in one document, so
`render_4547` emits **⌈children/2⌉ pages** while the builder emits **ONE
document with every child**. Both directions are pinned facing each other: a
test asserts child 3 lands in **column 1 of page 2** (not merely "present") and
that page 2 does not repeat child 1; another asserts the extract yields ONE
source with three children.
⚠ The column assignment was **verified positionally** on all 18 rows — child 1
in the x 260-417 band, child 2 in 419-576, sharing a y (s236/s241h).
⚠ **The Sign-Here and Paid-Preparer blocks are deliberately UNMAPPED**, and
that is a finding: the taxpayer signature row has **no widgets at all** (it is
hand-signed), and the seven preparer widgets were identified from their own
printed labels as firm facts this form does not hold. Mapping by inference
would assert a wrong field on a signed consent.
⚠ **The `IND-476` refusal is BUILT** and its message carries the remedy — the
Instructions say the form *"can be filed at any time"*, so it says file it
separately rather than abandon the election.

⚠ **ONE ITEM ASK REMAINS OPEN, named rather than hidden**: *duplicate child
elections* are covered **within one election** by the DB constraint
(`uniq_form4547_child_per_dependent`), but a return carrying TWO elections that
both name the same dependent is not caught. A cheap follow-up; logged in
DEFERRAL_AUDIT rather than left unstated.

**⚠⚠ Form 8879-TA DOES NOT TRANSMIT and the batch item says it does.** Its
printed header: *"ERO must obtain and retain completed Form 8879-TA."* There is
**no `IRS8879TA` element anywhere in the MeF schema set** (all three versions
searched; a test pins the absence and a teeth-check proved the search reads real
files). The IRS's own *About* page reads as though it transmits — the face and
the schema win. Its Part I is a **COPY**: all six lines name their own source,
so they DERIVE and are never keyed (a test asserts the model exposes no field to
key one into).

Then, by size: #2 GA education credit + IT-QEE-TP2 ≈ #5 Schedule H << #1 1040-X
(large). ⚠ **#1 must honour `IND-476`.** Then BATCH-003's six remaining items
(⚠ build #3, mixed passive/nonpassive on one K-1, TOGETHER with the s239
Georgia work), then the BATCH-005 triage above.

### ⭐ STILL UNBLOCKED, still passed over — now TEN sessions
- **Form 8853 Sections A/B + Section C.** Spec cached at
  `server/specs/8853_sec_c_spec.json`; `lookup/8853_SEC_C/export/` returns 200;
  all four legs pending. Read the s232 write-up in `STATUS_ARCHIVE.md` first —
  Schedule 1 line 8e is COMPOSED not owned, and line 25 FLOORS AT ZERO though
  the printed face does not say so.
  ⚠ **s241 gave this a second reason**: Form 5329 line 36 takes its value "from
  Form 8853, line 8" and **no `Form8853` model exists at all**, so the Archer
  arm of the 5329 excess chain cannot be reconciled the way the HSA arm now is
  (`D_5329_006`). Sections A/B and Section C are near neighbours — one pass.

### ⛔⛔ THE E-FILE GAP LIST — still TWO named documents, unchanged
- **`IRS4797` (s240)** — no 1040-side Form 4797 document builder; MeF rejects
  the return without it (`S1-F1040-118-01`, Reject, Active). s240 added a
  refusal so it fails loudly at composition. **The higher-volume of the two** —
  every disposition, installment sale, like-kind exchange AND K-1 §1231 return.
  ⚠ **BATCH-005 #6 now needs it too.** The 1120-S `IRS4797` builder is the
  worked example.
- **`IRS1116`** — the oldest live e-file gap. A full-path Form 1116 return is
  paper-only in code. s238's `IRS8379` build is a worked example end to end.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — 6 open** (2, 4, 5, 6, 8, 10);
  **BATCH-002 — items 9 and 10 open as to their COMPUTE half only** (both
  RS-blocked on the missing NOL spec); **BATCH-003 — 6 open** (1, 3, 6, 8, 9,
  10); **BATCH-004 — 7 open, all triaged**; **BATCH-005 — 10 open, UNTRIAGED**.
  Every worked file carries a result annex naming what is done and what is
  blocked; read it before starting. ⚠ None has moved to Done, deliberately.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

---

## ⚠⚠ THE FINDING WORTH CARRYING FROM s241o — a `getattr` default turns a WRONG ATTRIBUTE NAME into a plausible answer
`D_1099PATR_199A`, shipped hours earlier in s241n, read
`getattr(ctx.taxpayer, "qbi_is_patron", False)`. **`qbi_is_patron` is a
PER-ACTIVITY field** — it lives on `ScheduleC` and `ScheduleF`, never on
`Taxpayer`. So the expression answered *"not a patron"* on every return in
existence, and the warning fired **unconditionally and un-clearably**: ticking
the patron box, which is exactly what the message asks for, changed nothing.
That is the s192 shape (an unclearable diagnostic) arriving through a different
door.
*Two things generalise.* **(1)** A `getattr(obj, "name", default)` on a model
field is a silent typo-swallower — a plain attribute access would have raised on
the first run. **(2) The test had no teeth and looked like it did**: it asserted
the rule FIRES, which passed either way. Only the QUIET case proves a comparison
is real. Both cases are pinned now, plus a third for per-activity routing.
⚠ And the thing that caught it was the **client typecheck**, which knows the row
shapes — a server-side rule was corrected by a TypeScript error.

## ⚠⚠ THE SECOND ONE — adding a PARTIAL derive can open a silent gap where a full one would not
`D_GA500_017` asked *"is worksheet line 10 blank?"*. That was a correct test
while nothing fed L10. The 1099-PATR derive is **partial by nature** — it knows
one component of "other income" and cannot know the rest — so a return with a
1099-PATR *and* other 8-line income would have had a NON-blank L10 and been
reported complete while its base was still short. **The rule would have gone
quiet on exactly the returns it exists for.** New trigger: *is L10 still only
what the software itself derived, while the federal other-income total is
larger?* — which reduces to the old test on every return with no 1099-PATR.
⚠ Deliberately NOT a strict `keyed < federal` comparison: r. 560-7-4-.02(4)(b)2
excludes lottery and gambling from retirement income altogether, so a gambling
return's L10 is *supposed* to be smaller and would false-alarm forever.
*The general shape: when a new feeder covers PART of a line, every rule that
tested that line for EMPTINESS now tests the wrong thing.* Seventh occurrence of
"adding support makes an existing, correct rule wrong" (s225, s233, s238, s240,
s241e, s241o ×2).

## ⚠ Classes that MOVE existing returns or output on next recompute
- **s241t: NONE.** Two new tables and a pure-function module nothing calls yet.
  No compute changed; `GeorgiaCredit` rows only exist where a preparer or
  payload makes one. Migrations are additive (CreateModel + RLS ALTER), so the
  s190 `db_default` trap does not apply.
- **s241s: NONE.** A source brief and its tests. No production code changed.
- **s241r: NONE.** A new render function and a new MeF document, both reached
  only when the return carries a `Form4547` row — and none exists until a
  preparer or payload makes one. ⚠ **The one behaviour change that CAN bite:**
  an amended return carrying a Form 4547 now **refuses at composition**
  (`IND-476`) where it previously composed and would have been rejected by the
  IRS days later. That reaches only returns that were already un-transmittable.
- **s241q: NONE.** Eight new diagnostics that all no-op unless the return
  carries a `Form4547` row, and no such row exists until a preparer or payload
  makes one. No compute changed. ⚠ The published import schema DID change
  (`form_4547s` gains `children` + `form_8879ta`) — additive, so no existing
  payload becomes invalid.
- **s241p: NONE.** Three new tables and a pure-predicate module that nothing
  calls yet. No compute changed, no existing row touched; `Form4547` rows only
  exist where a preparer or payload makes one. The migrations are additive
  (CreateModel + RLS ALTER), so the s190 `db_default` trap does not apply.
- **⚠ s241o MOVES GEORGIA RETURNS — but only ones carrying a 1099-PATR.** A GA
  return with an 8z-routed 1099-PATR now gets that taxable amount on RIE
  worksheet L10, enlarging the exclusion and **reducing Georgia tax**. No
  1099-PATR row exists until a preparer or payload makes one, so no return
  changes on its own. ⚠ L10 also became a DERIVED line: it is now written
  unconditionally including back to blank, so a value keyed there WITHOUT
  `is_overridden` would be cleared on the next refresh. Every browser write path
  (`PATCH /fields`) sets `is_overridden`, and the lane writes GA fields with it
  too, so this reaches nothing in practice — pinned both ways by tests.
- **⚠ s241o MOVES DIAGNOSTICS, in the quiet direction.** `D_1099PATR_199A`
  stops firing on returns whose business or farm IS marked as a patron (it was
  firing on all of them). `D_GA500_017`'s line-10 arm changes trigger.
- **s241n: NONE.** The five `D_1099PATR_*` rules fire only on returns carrying a
  1099-PATR row.
- **s241m: NONE.** `Form1099PATR` rows only exist where a preparer creates one,
  and the 8z composition is unchanged on every return without one.
- **⚠ s241j MOVES DIAGNOSTICS, and one class of return should CHANGE.** Any
  return deducting alimony under a line-19c instrument dated 2019 or later now
  fires `D_SCH1_007` (**error**) where it was silent — and that deduction is
  genuinely not allowed, so those returns are wrong today and the AGI must come
  back up when the amount is removed. Every pre-2019 alimony return gains
  `D_SCH1_008` (info). No computed value changes on its own.
- **⚠ s241h/s241e MOVE PRINTED OUTPUT** on a rendered Form 8862 (Parts III/IV
  grids; Part II lines 3/4 and Section B). Same values already transmitted — the
  paper stops lagging the XML. No dollar figure changes.
- **⚠ s241d/s241c MOVE E-FILE OUTPUT** on any return transmitting a Form 8862:
  a childless 8862 now emits Section B or REFUSES; ODC persons move out of
  `CTCACTCChildInformationGrp`; an unanswered line 3 refuses instead of
  transmitting a fabricated "No"; lines 17 and 19a now derive. ⚠ Sign: every one
  is a correction toward the truthful, narrower claim.
- Carried from s240: a passive or PTP K-1 carrying a net §1231 LOSS now fires
  `D_8582_MULTIFORM` (RED) or `D_K1_PTP_LOSS` (warning); any 1040 with a
  non-zero Schedule 1 line 4 now refuses at MeF composition.
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
  `tests/registry_asserts.assert_registry_wired()`. The `-k "diagnostic"` sweep
  went from **22 failed / 878 passed → 900 passed, 0 failed**. ⚠ **The cheap
  repair was deliberately rejected**: repointing the sniff at
  `all_registered_rules` would have gone green instantly and still proved
  nothing — the teeth test injected `RULES_8889 = []`, which CONTAINS the
  literal, so a repointed sniff would have passed while the family reached
  neither the seeder nor the DB.
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
- **⚠ NEWLY OBSERVED (s241o), PRE-EXISTING, and it looks like a real 1120-S
  defect rather than a rotted test.**
  `test_line_key_registry_sweep.py::test_formula_targets_resolve` fails:
  `FORMULA_REGISTRY["1120-S"]` targets **`M2_DIST_EXCESS`** and
  **`L24_BOOK_BRIDGE`**, and `seed_1120s.py` creates neither FormLine — so the
  formula pass computes both and **silently never persists them**, which is
  exactly what that guard exists to catch. Both keys date from the s205 /
  batch-008 M-2 + Schedule L era. Untouched by this session (nothing in the diff
  goes near 1120-S), not diagnosed, and **1120-S only — no 1040 impact**.
  Deserves its own unit.
- **Client typecheck**: **55 error lines standalone — unchanged by s241o**,
  which was measured before and after (it briefly went to 56 and the extra one
  was a real defect, not noise; see the finding above). `npx vitest run` is
  **1,680 passed / 140 files**.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). ⚠ **s241o hit
  this**: a background `-k` sweep stalled at 13% for half an hour because
  foreground runs were started beside it. Kill and re-run alone; do not
  interpret a stalled sweep as a hang.
- **A broad `-k` sweep is SLOW and blows the 600s Bash timeout** — s236's
  ~1,100-test sweep took **21½ minutes**. Run it with `run_in_background: true`.
  ⚠ Keep the `-k` terms tight: `k1` matches hundreds of node ids, `rie` matches
  "ret**rie**ve".
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
  throwaway `tests/test_zz_*.py` with `-s` print statements, deleted after.**
- ⚠ **`poetry run` must be invoked from `D:\dev\delvio-tax\server`** — from the
  repo root it fails with "could not find a pyproject.toml".
- ⚠ **Windows `python` cannot read the Bash tool's `/tmp`** — different
  filesystems. Write shared files to the scratchpad path (s240).
- ⚠ Prefer PowerShell for `poetry run` on this box (s241).
- ⚠ A Cloudflare-protected law site (justia) 403s both WebFetch and curl, and
  **`rules.sos.ga.gov` returns only chrome to WebFetch**. The in-app browser
  (`preview_start` + `get_page_text`) got the full verbatim Georgia reg where
  both failed — s239, and again in s241o.
- ⚠ Lane/API shapes that cost time (s241): **staging answers 201 even for an
  invalid payload** — the verdict is `row["status"]`, not the HTTP code; return
  CRUD routes are `/api/v1/tax-returns/…` (not `/returns/`) and the detail route
  **needs its trailing slash** or you get a 301; `filing_status` is `"mfj"`, not
  `"married_joint"` (varchar(10)).
- ⚠ A diagnostic `_finding(...)`'s extra kwargs land under `["details"]`, not at
  the top level of the finding dict (s241o).
- ⚠ `ScheduleF` has **no `business_name`** — the header field is
  `principal_activity` (s241o).

### 🔎 Carried for triage — NOT claims
- **From s241o**: nothing in the app derives RIE **L8 alimony**, and after this
  unit L10 is only partly derived. The app DOES know the whole of federal
  Schedule 1 line 8 — every 8a-8z line is a FormFieldValue — so a fuller L10
  derive is possible in principle, but the components are return-level with no
  owner, and (4)(b)2 puts gambling (line 8b) **outside retirement income
  entirely**. Worth a design pass; not attempted here.
- **From s241**: `Form8606` and `HSAAccount` both allow duplicate owners and
  their compute ITERATES, so a duplicate DOUBLE-COUNTS rather than vanishing.
  `views.py:623` (the 8606 proforma roll) guards with `.exists()`, but the
  browser POST does not. Not measured; not a claim.
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
- **NEW (s241s): the GA QEE credit has NO SPEC — `IT_QEE_TP2`, `IT-QEE-TP2`,
  `ITQEETP2`, `500_SCH2`, `GA_QEE` and `QEE` all 404**, while the `500` spec
  models only the destination (line 21, typed `input`). s241s built the brief
  from O.C.G.A. §48-7-29.16 on the s223/s241p precedent. **Author both** — a
  Schedule 2 credit-detail spec and `IT_QEE_TP2` — with the brief as input.
  ⚠ Whatever is authored MUST carry the **two carryforward regimes** (3 years
  for credits generated in TY2025+, 5 for older ones, per 2024 Ga. Laws 598
  §1-7) and must record that **line 21 is `input` today**, so a build that
  derives it re-authors the spec rather than diverging (the s142 rule).
- **NEW (s241p): `4547` and `8879_TA` have NO SPEC AT ALL.** `lookup/4547/`,
  `F4547`, `FORM_4547`, `8879_TA` and `8879TA` all 404. s241p built the form
  from the IRS artifacts on the s223 Form-1310 precedent (which also 404s) and
  recorded the reading in `server/specs/_4547_source_brief.md` — the build is
  transcription, not improvisation, because the form **computes nothing** and
  every eligibility test is printed verbatim. **Author both specs** so the gap
  is closed rather than merely justified; the brief is the input. ⚠ Whatever is
  authored must keep line 6 and line 7 as SEPARATE tests (the account test is
  wider than the pilot test) and must record `IND-476`.
- **NEW (s241o): the `500` spec still has NO rule for what feeds RIE lines
  1/2/6-13, and that silence has now produced FIVE defects.** This unit adds the
  fifth. Author it properly and record, at minimum: the earned/unearned split
  **by entity type** (r. 560-7-4-.02(4)(b)1 uses SE-subjection for partnerships
  and material participation for S-corps, in one sentence); that the $5,000
  earned cap is statutory; that **lottery/gambling income is NOT retirement
  income at all** under (4)(b)2 (nothing in the app encodes this, and RIE L10's
  new trigger depends on it); and that **only income included in Georgia taxable
  income** enters the base. `D_GA500_017`'s spec condition also still lists L13
  and L10 among the un-pulled lines — both are now (partly) pulled.
- **NEW (s241b/triage): the `8862` spec is a DRAFT that collapses each PART into
  one boolean.** `lookup/8862/export/` answers **200** — so the 404-STOP gate
  waves it through — and the export is `"status": "draft"`, version 1, with 6
  facts, ONE rule, 6 `line_map` entries (four of them pseudo-lines), 1
  diagnostic and 2 tests, against a printed face of ~40 numbered lines. **Second
  time a draft spec has looked like permission** (s238's `8379` was first, and
  its `status` field is STILL not checked anywhere). Re-author per-line.
- **Carried (s241): the `5329` spec** says nothing about the roll-forward, about
  Part VIII (ABLE has line 50 and no prior-year line — a §529A question the FORM
  does not answer, deliberately not guessed), or about where lines 36 and 44
  come from, though both name a source on the printed face.
- Carried (s240): `R-8582-MULTIFORM`'s no-silent-gap clause is now FALSE as
  written (it cites `D_K1_SEC1231`, retired 2026-06-30); `R-8582-WS-NET` says
  nothing about which OTHER forms an activity's losses can land on; the `4797`
  spec has no rule for the K-1 §1231 feed.
- Carried (s239): `R-RET-CODE` has been outrun three times (codes 6, W, U) —
  re-author from the current i1099-R Table 1 in one pass.
- Carried (s238): the `8379` spec is a DRAFT whose `line_map` covers 4 of ~20
  lines and returns 200. **The export's `status` field is still not checked
  anywhere.**
- Carried (s236): `R-SCHA-CHARITABLE` models only three buckets while the K-1
  states **seven** — 1120-S box 12 codes A–G — so **B, D, F and G have no home
  and are REFUSED at both write paths**. ⚠ Sign: a refused code is not deducted
  AT ALL — it overstates tax. Also: the `500` spec's RIE line_map has no
  **RIE-13**.
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
