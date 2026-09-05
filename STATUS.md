# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s334 close, 2026-09-05 (night)

**▶▶ START HERE (s334 close, 2026-09-05, night): THE FORM 6251 (AMT) TAXWISE READER IS
BUILT, GREEN AND DEPLOYED; THE WALL IS GONE ON ALL 37 PACKETS THAT PRINT THE PAGE, AND
THE WALLS BEHIND IT ARE MEASURED.** Delvio `5f8bdc98` (+ the close commit). No RS
change (the 6251 spec already said what the engine now does).
- `scripts/taxwise1040/f6251.py`: line 2l → `amt_depreciation_adj`; the RED-defer
  preferences (2i / 2h / 2e+2f / 2j / 2c-2t+3 / 8) → their `amt_*` fields, keyed as
  printed with the engine's D_6251_00x RED firing by design; every engine-derived line
  is an identity gate (4 = 1b + Σ2 + 3, the §55(d) exemption after phase-out, 26%/28%
  or Part III line 40, 9, 11); 1a/1b/2a/2b/2g/10 cross-check the packet's own faces in
  the emitter; line 11 explains face 17 beside the 8962's 29. 10 reader units.
- **⚠⚠ ENGINE FIX SHIPPED WITH IT:** `compute_6251` read the senior add-back from 1040
  line 13b — which is Schedule 1-A line 38, ALL FOUR OBBBA deductions — instead of line
  37 (the spec's `a_senior_deduction`; `compute_1116` already read 37). AMTI was
  overstated by tips / overtime / car-loan on every return carrying them. Fixed; the
  render face's 1a = 12 + 13 + 13b − 37. Flow assertions 548; the 6251 legs green.
- **Acceptance:** 37 faces parse (36 on the first smoke — a two-digit value starts at
  x0 ≈ 562, window by RIGHT edge); 5 packets emit; **2 landed** (…0307, …4793 — full
  tie, zero error diagnostics, batch `s334-6251-commit-001` under the standing commit
  authorization; see `1040\tmp\commit_s334_6251.txt`); 3 held with the 2m RED
  (D_6251_005, by design — the AMT passive-loss app build, DEFERRAL_AUDIT s333 (1)) and
  non-AMT no-ties. Two of the five first FAILED STAGING on an empty asset `link_key` (a
  Schedule C with no printed name blanks the asset report's "Form:" label) — reconciled
  like the NEC rows (exact → unique prefix → single activity; still blank refuses by name).
- **The walls behind the 6251 page (37 packets):** Form 4562 face ×14 · GA
  state-nonconformity worksheet ×12 · CTC/extension carryover worksheet ×7 ·
  sched_line_detail ×6 · lump-sum SS worksheet ×4 · 8995-A ×4 · 1116 ×4 · 3800 ×4.
- **REVIEW_QUEUE s334 (Ken):** Form 6251 line 1b on a zero-taxable-income return — the
  form keeps the negative, TaxWise floors at zero, the engine runs on line 15 + the
  senior deduction (three witnesses, no AMT on any). Recommendation: the form's letter.
- **The entry lane (tax-test-data-b9) tonight, via the cross-session channel:** Ken's
  penalty ruling widened to both directions (DECISIONS s330 ruling 2 addendum); the three
  hand-keyable classes I named were worked (4 committed · 4 already correct · 1 Ken);
  their three reader findings are DEFERRAL_AUDIT s334 (2)-(4): the 1099-R "E" marker is
  two sub-classes (simplified-method annuity = a reader item, 7 packets), the lump-sum
  SS refusal fires on the worksheet's PRESENCE (compare to face 6b), farm-QBI sign /
  line-A link / middle-name split. Two censuses (non-GA state forms; 1065 page classes)
  land in their annex `1040\tmp\classes-20260905\NOTES.md`.
⚠⚠ Standing facts every build must know: (1) **the deploy runs `migrate` ONLY — a new
diagnostic code is INERT until `manage.py seed_rules` runs** (none added tonight); (2)
**client numbers only in STATUS / BUILD_ORDER / REVIEW_QUEUE; no real personal names in
checked-in fixtures**; (3) **never start a foreground pytest while a background pytest
runs**; (4) **a CRLF packet list through `read -r` puts `\r` on every path**.

**▶ NEXT, in the order that lands the most returns:**
1. **Ken's remaining answers** (Wednesday below) — the RIE line-10 definition (A10 +
   296 #85), the Schedule B ownership tag (A2(b), ~26 packets), the former-passive row
   (B7), the 8615 witness's filed line 9, seed_rules in build.sh, **the 6251 line-1b
   base (s334)**.
2. **The named TaxWise reader walls by measured count** — the Form 4562 face (×14 in the
   6251 population, ×3 sole in s331; frees A4's two farm registers) → the GA
   state-nonconformity worksheet (×12; the engine derives the GA bonus add-back — decide
   ignore-vs-cross-check off the page) → 1116 (5) → sched_line_detail (6) → the 1099-R
   simplified-method "E" row (7, DEFERRAL s334 (2)) → the lump-sum SS presence release
   (s334 (3)) → the qualified-tips per-W-2 box-7 split → the portfolio K-1 consolidation
   (DEFERRAL (13)). Drive ONE single-wall packet by hand through each before trusting
   the census (s328); a blocking class's sole-wall count is a lower bound (s331, s334).
3. **The OWN UNITS of the triage foot** — 015 #4 Form 3800 · 015 #10 Georgia Form 500X
   · 296 #52 Alabama 40NR · 296 #10 Form 4835 · 296 #56 Form 6252 contingent-payment ·
   the IND-CR 212 app build · the AMT passive-loss app build (DEFERRAL s333 (1); 9 of
   the 37 6251 faces print 2m).
4. **The four fenced clients' filed flag** (2777 · 3630 · 4159 · 4160) and ruling 2's
   penalty acceptance (~10, now both directions) + ruling 3 (client 3250).
5. **The Lacerte face readers by wall count** — Sch 3 (107) → A (37) → B (32) → C (18);
   port f6251 through `lacerte1040.pagewords` when a Lacerte 6251 packet is the wall.
6. **⑥c `manage.py merge_client`** (D-044) — first worklist: the 13 all-blank
   duplicate-name groups (DEFERRAL (12)).
7. **The 1065 import** — 96 partnership packets; the entry lane's page census is coming.

**⛔ WAITING ON KEN — WEDNESDAY AGENDA (s332–s334):** the two season-2026 design
questions (what replaces the answer key; how scanned documents get in) — recommendations
in REVIEW_QUEUE, shelf units in BUILD_ORDER · a Form 4136 spec in Rule Studio (296 #48) ·
the RIE line-10 definition (015 #2 + 296 #85, one question) · add `manage.py seed_rules`
after `migrate` in `build.sh`? (DEFERRAL_AUDIT (10)) · the Schedule B ownership question
(A2(b)) · code L in `EARLY_CODES` · the former-passive row (B7) · the 8615 witness's
filed line 9 (file the engine's 4 or accept the vendor's 1,048) · whether to rewrite the
two s333 SHAs that briefly carried fixture names · the AMT passive-loss APP build's place
in the queue (DEFERRAL_AUDIT s333 (1)) · **the 6251 line-1b base on zero-taxable-income
returns (REVIEW_QUEUE s334)** · **"start building state software" — a SCOPE question
(which state, what "the app itself" means); the entry lane was told NOT to start it.**

**⛔ WAITING ON KEN (carried):** the BATCH-013 item-2 RIE placement flag · the packet in
tmp/s328_ken_questions.md (GA 500 p1 names a different primary SSN) · seed ONE client
(the …4641 taxpayer in Jenny's book; do NOT edit #4054) · client #3572's contaminated
name (#4514 same shape) · three clients with no 2025 1040 shell · client 2149's filed GA
17a = 2 exemptions on a single/zero-dependent return · the …4203 W-code question · the
asset METHOD-DERIVATION TABLE review · vendor-name allowlist for the mirror guard? ·
carried: 1071 · 1141 · R-GA500-RIE · 4059 W-2G address · Sch D carryover · GA RIE L10 ·
4081's $169 · standing 1–8 · 2a scope flag · AL 40 · the 4 Tom-book holds (…8505 ·
…2276 · …2827 · …8791) · `D_1099MISC_RECON` per-document vs aggregate (DEFERRAL 19e) ·
the Schedule C address model gap (DEFERRAL 19a) · the …7479 duplicate shells (#3195/#3196,
entry lane) · **the 59 Gail clients still need a full federal + GA reprint (s331 ruling 5).**

**▶ BUILD QUEUE after the named walls:** ④ the three re-raised Lacerte engine holds
(clients 1922, 2386, 3517) · ⑤ the GA 7b military-exclusion engine leg (8 witnesses +
Gail #2019; waits on the states-lane spec export) + the 7c/7f DIS transcription · ④ the
shadow-2210 reader · the rental-row asset link (the D_4562_DEST pair on …0482, DEFERRAL
s334 (5)) · carried: the 8615 parent-first guard · the zero-activity GA-attach gap · the
Schedule C address (DEFERRAL 19a) · the IRS1099NEC document question (19b) · the
AMT-1116 twin head, still unwitnessed · the client-1410 Georgia residual.

⚠⚠ **ANOTHER CC ACCOUNT WRITES THIS LANE — THE SHARED FILES ARE THE ONLY PLACE ITS WORK
IS VISIBLE.** Do not order events by `updated_at`.
