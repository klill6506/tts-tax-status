# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s332 close, 2026-09-05

**▶▶ START HERE (s332 close, 2026-09-05): Ken's three rulings are EXECUTED,
BATCH-014 is closed, and BATCH A1 (015 #3) is BUILT and landed (blocks below). 🏁 TRIAGE DONE — the forty are ranked by MEASURED yield at the
foot of `CC_CODE_CHANGES_1040_BATCH-015.md` (pointer in 296). ⚠ VERIFY-FIRST caught
three 'open' items ALREADY BUILT (296 #32 s267 · #47 · #66 s272 — the code is
the census). **BATCH A, each verified open: 015 #3 → 015 #17 → 015 #1 → 015 #7
→ 015 #18 → 015 #19+296 #60 → 015 #8 → 296 #84 → 015 #13 → 015 #2.** 015 #20 is
CLOSED by ruling 2; 296 #85 needs Ken. **BATCH A IS WORKED: A1, A3, A4, A6, A7, A8, A9 BUILT; A5 ALREADY BUILT by 014 #2 (replay-verified, pinned); A2 half built + half Ken's; A10 STOPPED for Ken (one question with 296 #85, REVIEW_QUEUE).** NEXT: **Batch B — B1 296 #48 (Form 4136 fuel credit) → B4 015 #6 → B5 015 #16 → B6 015 #14 → B7 296 #43 → B8 296 #20 → B9 015 #12 → B10 015 #15**. Ken is
running Codex on the ENTRY of the returns BATCH-014 freed.** Earlier this
session: the unknown-page pass (`c43ea59b`) and the Form 5329 reader
(`5ed62a3d`), 24 + 8 returns landed, every one a tie.

**⛔ BATCH A10 — 015 #2 STOPPED FOR KEN (s332).** A taxable 1099-C on
Schedule 1 line 8c to the Georgia RIE worksheet line 10: R-GA500-RIE defines
line 10 as the §111 refund + unemployment ONLY — Ken's 2026-08-30 Gate-1
ruling — so a third component is an amendment of his ruling, and the same
question decides 296 #85 (the §108(f)(5) student loan the vendor also put on
line 10 though it is not in Georgia taxable income). **One question in
REVIEW_QUEUE with a recommendation** (line 10 = every Schedule 1 other-income
item INCLUDED in Georgia taxable income, r. 560-7-4-.02): adopting it ties
015 #2's fixture (16,782 / 323 / 996) and keeps the excluded loan off. No
engine change; spec first on Ken's word.

**🏁 BATCH A9 — 015 #13 BUILT, differently from the ask (`7a58642d`, deploy
`dep-dadqn3k9v7es73ajseqg` LIVE).** The item wanted a non-itemizer's business mortgage interest KEPT on
Form 8829 line 10(b); the 2025 i8829 ("Taxpayers claiming the standard
deduction … using lines 16 and 17") and the RS spec put it on line 16. The
engine zeroed 10(b) correctly but DROPPED the printed amount — line 16(b)
now carries it (the RE-tax line's two-representation routing, mirrored).
Replay: the 676 miss closed to 2, and the 2 is the payload's RE tax (2,718
keyed; the page prints 2,738) — keyed as printed, **every federal and
Georgia line ties**. 22 tests in the two 8829 files.

**🏁 BATCH A8 — 296 #84 BUILT (`60f07144`, deploy `dep-dadqn3k9v7es73ajseqg` LIVE) — A TAX-LAW SIGN
ERROR IN THE FORM 8582 MAGI.** The item's payload replayed to its exact
misses; the engine's 8582 line 6 printed 150,719 = AGI + taxable Social
Security, so the $25,000 allowance was 0 and the whole active rental loss
was suspended. §469(i)(3)(E)(i) and the 2025 i8582 line-6 example (92,000 −
5,500 = 86,500) REMOVE taxable SS; `form_8582_magi` ADDED it — it treated
the list's one income item like the deductions beside it. One sign, cited.
**Replayed: every federal line ties** (11 = 101,617 … refund 1,432; 8582
6/9 = 62,711 / 2,756); the Georgia misses left are 296 #85 (Ken). **Movement
gate (rolled back): 49 returns recomputed rolled back (41 with an 8582 line 6 in play, 46 with rentals carrying passive amounts): 33 unchanged; 16 moved ONLY on the printed 8582 line 6 itself (the MAGI, lower on every one — no 1040 line 11/24, Schedule 1 line 5, or rental allowed/suspended amount changed); ONE of them (client 4797, filed) also moved 1040 line 24 by $1 — and the CONTROL (the identical recompute with the OLD formula monkeypatched back) moves it identically (Schedule 3 line 1 foreign tax credit 21 → 20, a pre-existing recompute drift on that return), so ZERO tax-line movers are attributable to the fix. ⚠ The commit message's 'only line 6 moved' overstated it by that one control-proven artifact.** Tests: the instructions' example + the item's
shape. ⚠ Client 2490 stays draft for the entry lane.

**🏁 BATCH A7 — 015 #8 BUILT (`cb10f530`, deploy `dep-dadqbs4s728c73ff5e80` LIVE).** The GA 1099-G
report row on the fixture prints NO ownership X at all (the item thought it
printed one in a combined column); the federal report's row does. The GA
parser no longer aborts; the reconciliation (`_merge_g1099`, extracted and
unit-tested) gives an ownerless GA row the owner of the ONE federal row whose
amounts fit and refuses on 0 or 2+ fits. The fixture emits its taxpayer row
(4,380 / 444 / GA / 264 / TIN). Zero Gail packets carried this wall — a
Georgianna-book print shape. 272 extractor tests pass.

**🏁 BATCH A6 — 015 #19 + 296 #60 BUILT (`58c6e4fe`, deploy `dep-dadq9fss728c73ff3sqg` LIVE;
migration 0392 applied ahead).** Schedule 2 line 13 as a documented-source
total when the packet prints no per-W-2 box-12 rows (no TaxWise reader emits
box 12 at all). The line-14/line-8 trio shape across model, lane (trio +
two-writers refusal), serializer, `compute_sch_2` (rows PREFERRED — the source
stands in only with no A/B/M/N row), `D_SCH2_L13_SRC` (warning; error when
rows shadow it) and the emitter. **The item's payload replays to a TIE
(line 13 = 5, 1040 line 24 = 279, refund 421)**; client 3776 stays draft for
Codex's batch. **296 #60's filed fixture (client 2662) CORRECTED in the app**
— the $11 moved from the line-14 trio to line 13 under a guarded transaction
(1040 23/24/34/37 unchanged, `D_6252_009` silent); its unbatched sibling
(…3840) now refuses on the qualified-tips per-W-2 box-7 split instead. ⚠⚠
**THE DEPLOY RUNS `migrate` ONLY — a new diagnostic code is INERT until
`seed_rules` runs** (seeded by hand this sitting; DEFERRAL_AUDIT (10)). 17
new tests; gates 522 passed.

**✅ BATCH A5 — 015 #18 ALREADY BUILT (verify-first; pin only).** The
triage row read the pre-ruling code; 014 #2's item-level allocation
(`543ba99d`) already splits each JOINT capital row on its own through the
conserving split (odd dollar to the spouse = the vendor's 205/206). **The
item's own staged payload replays to a full TIE on the current engine**
(RIE-TP-17 65,000 / SP-17 29,861 / S1-7 94,861, zero diagnostic errors) —
not landed by this lane (a Codex entry-lane packet; client 3627 stays draft
for Codex's batch). Pinned in `test_b014_item2_rie_item_level.py`.

**🏁 BATCH A4 — 015 #7 BUILT (`fabbcb5a`, deploy `dep-dadpul15efls739cmg0g` LIVE; migration
0391, no-op SQL).** Nondepreciable land needs no averaging convention.
⚠ Verify-first shrank it: the engine already zeroes Land/NONE before reading
a convention, the UI's Land preset already seeds a blank, the RS 4562 export
has convention NOT required — the walls were the model column and the lane's
required loop. Model blank-able; lane requires the convention for every
method except NONE (keyed on the METHOD — a land improvement under the Land
heading still averages); extractor emits land AS PRINTED. ⚠⚠ **The witness
run found two facts the item could not see:** these farm books print the
literal token `LAND` in the Method column (the s324 shape fired only on a
blank), and head the pickup-truck rows "Class: Autos" (unmapped). Both fixed →
**client 1766's register parses 91/91, client 1772's 42/42, zero refusals,
four land rows blank.** The item's filed-total gates are NOT reachable yet:
both packets still refuse on other walls (f4562 face ×2 each,
sched_line_detail, ctc_ext_carryover_wks, state_nonconformity_wks,
detail_sheet, f6251) — DEFERRAL_AUDIT (9). 7 new tests; gates 491 passed.

**🏁 BATCH A3 — 015 #1 BUILT (`ef62bf5e`, deploy `dep-dadpkh17lnhs73e8cr70` LIVE).** (a) The
diagnostic: `D_1040_014` is silent when the election is checked AND an
`ss_lump_sums` row exists (the engine computes the Pub 915 election;
`D_RET_008` carries the comparison) and errors only for an election with no
row. (b) ⚠ **The reader half decomposed to a CHECKBOX, not a worksheet
reader:** the eight lump-sum packets print NO Pub 915 worksheet — only the
SSA-1099 box-3 breakdown totals — so no reader could key the election; but
the face prints its X's as words (5–6 per page, the positive control) and the
**line-6c election box is unmarked on all eight** = regular method, the
amounts informational. New `parse_p1_lump_election`; the emitter refuses only
when 6c is MARKED (no per-year facts to key) or unreadable. **4 of 8 landed,
all ties, zero diagnostic errors** (batch `s332-lump-commit-001`). The other
four show their next wall: a Sch C at-risk box misparse (…6407, no item —
DEFERRAL_AUDIT), ownerless joint Schedule B (…1678 — A2(b), Ken), an
ambiguous shell (…7479 — 015 #14), OPM 1099-R cost columns unmapped (…3873 —
DEFERRAL_AUDIT). Gates 338 passed. Annex on the 015 file.

**▶ BATCH A2 — 015 #17, HALF BUILT, HALF FOR KEN (s332).** (a) **Built:** a
joint return's face-2a tax-exempt interest with no payer page now imports as
one labeled consolidated row, owner `joint` — tax-exempt interest has NO
per-owner consumer in the return (IT-511 p.24 excludes exempt interest from
the RIE; `compute_intdiv` sums it return-level), so the old MFJ refusal
guarded a distinction nothing consumes. Both emitter arms; lane test. The
three 2a-class packets all carry a second wall (the ownership class ×2, GA
Schedule 1 additions ×1) — zero released by this alone. (b) **For Ken
(REVIEW_QUEUE):** Schedule B payer rows with NO ownership on a joint return —
~26 packets, 26 of 28 print a RIE worksheet so attribution matters; the
worksheet gives only per-spouse category TOTALS; the s293 corpus proved a
subset-sum guess can hide a joint payer. *Recommendation:* tag the rows
`joint` and key the worksheet's per-spouse line 6/7 totals as the RIE inputs
(what the filed return asserts; RIE-17 still gates the tie; the owner tag has
no other consumer). (c) **Deferred:** the Form 8995-A facts reader (the item's
second half — `qbi_loss_carryforward_prior` / `qbi_reit_ptp_income` already in
the lane; the page is ROLE_REVIEW) — a reader unit of its own.

**🏁 BATCH A1 — 015 #3 BUILT (`9bcb61a3`, deploy `dep-dadp3a6q1p3s73cftu0g`
LIVE; RS `2873cbb`; migration 0390 applied).** Schedule 2 line 8 reported
directly ("Form 5329 not required"): the documented-source trio
`taxpayer.sch2_l8_source_amount/_label/_note` (the line-14 shape) ADDS to the
engine's Form 5329 total; refused beside a `form_5329s` row; `D_5329_SRC`
warns/errors. ⚠ **The census could not see the 1099-R codes: all five
sole-wall packets are CODE 1, a shape the engine derives itself** — keying a
source would have DOUBLED line 8. The emitter now has two routes: cross-check
the filed line 8 against 10% × taxable over the code-1/S rows (decompose), or
key the documented source only for shapes the engine does not derive (L/J/T).
**4 of the 5 landed, every one a TIE** (batch `s332-sch2-l8-commit-001`; standing commit authorization). **1 HELD, rolled back — …0394:** the FEDERAL face ties (line 8 decomposed by the engine's own rule) but the Georgia RIE does not — RIE-TP-17 filed 6,810 vs engine 5,000, i.e. an UNEARNED 1,810 the engine does not attribute to the taxpayer (the earned cap alone). A separate RIE-attribution gap, not this item; decompose before re-running. ⚠ Open spec question recorded, not decided (RS session_log):
should code L (§72(p) deemed distribution) join `EARLY_CODES` {1, S}? The
vendor computed the 10% on it. client 2766 (the item's fixture)
refuses earlier in David's book (a THIRD cover-letter layout; a generic detail
sheet) and also needs A6 + A10. **NEXT: A2 — 015 #17, the ownerless
tax-exempt-interest aggregate (~11 packets).**

**🏁 KEN'S THREE RULINGS EXECUTED (s332, 2026-09-05 — the same sitting they
were given).**
- **BATCH-014 #2 — BUILT `543ba99d`, deploy `dep-dadog2c9v7es73aifod0` LIVE.** Spec first:
  R-GA500-RIE amended in Rule Studio and seeded (RS `ba9c70f`), the export
  cached. Source: 2025 IT-511 p.24 VERBATIM — *"Income or losses should be
  allocated to the person who owns the item. If any item is held jointly, the
  income or loss should be allocated to each taxpayer at 50%."* Engine: each
  owner's OWN capital rows + OWN cap-gain distributions + OWN carryover
  component (joint 50/50), used as the RIE line-9 columns ONLY when the two
  nets re-add to 1040 line 7 exactly (no cap binding); otherwise unchanged.
  Model: `schd_st/lt_carryover_tp_share` (migration 0389, NULLABLE — the pool
  amount is untouched, so Schedule D / proforma / e-file never see it); the
  lane validates it. **Movement gate: a rolled-back dry run over every filed
  MFJ return with owner-tagged capital items — 88 returns, ZERO moved.** One
  pre-existing test expectation (the #57 joint-loss case) superseded and
  re-pinned with the citation. ⚠ 0389 was applied to the shared DB ahead of
  the deploy (two nullable columns; old code ignores them) so the dry run
  could read it. ⚠ Open edge, recorded in the rule as UNSPECIFIED: how a
  CAPPED loss splits — the booklet is silent; the engine keeps the old
  allocation there. **Codex re-pass:** the fixture stages with
  `schd_st_carryover_prior: 3084` (owner omitted) + `schd_st_carryover_tp_share: 939`.
- **BATCH-014 #5 — SPEC AUTHORED (RS `ba9c70f`).** `R-GA500-PRECEPTOR` in the
  Form 500 spec from IT-511 p.64 (Form IND-CR 212, Rev. 07/09/25): § 48-7-29.22;
  $500/$1,000 physician, $375/$750 APRN-PA; 3 + 7 rotation caps; no carry;
  AHEC certification; window TY2019–2026. 5 facts, lines 212-A1…C1,
  `D_GA500_018`, T21/T22, FA-GA500-15; line 20 = CC-3 + 212-C1 + residual.
  Harness ALL PASS (23 scenarios). ⚠ The statute text was not fetched (the
  form is the primary transcription). **The APP BUILD is a separate unit**
  (DEFERRAL (3)); the fixture imports through the aggregate meanwhile.
- **Ruling 3 (Codex may build on a branch under the contract)** — recorded in
  DECISIONS; Ken is now running Codex on the ENTRY of the returns BATCH-014
  freed; a fresh production import session was minted on his go.

**⚠⚠ CORRECTION — "BATCH-296 (14 items, unworked)" WAS WRONG.** `CC_CODE_CHANGES_BATCH-296.md`
is the 1040 lane's RUNNING CORRESPONDENCE FILE (85 numbered items, 82 annex /
triage sections, worked s267 → s329), not a batch. A closure census (each
item's own status line + any later verdict mention) finds **~20 items with no
recorded closure**: 43 (prior passive K-1 losses beside positive current
income) · 45 (filed-split K-1 → 8582 / 8960) · 46 (2441 Part II vs Part III —
"half built") · 47 (return-level unrecaptured §1250 aggregate; an $82
understatement on the fixture) · 48 (Form 4136 fuel credit, OPEN) · 52 (AL
Form 40NR — spec live, APP BUILD queued) · 10 (Form 4835) · 20 (7206 for a
>2% S-corp shareholder) · 21 (partial PTP loss) · 32 (8949 dates → MeF) · 40
(AMT passive losses) · 56 (6252 contingent-payment) · 60 (Sch 2 line 13
aggregate — also BATCH-015 #19) · 63 (a commit 500) · 66 (8839 never feeds
Sch 3) · 68 (SEP/saver's optimizer — a feature ask) · 69 (NC D-400 S line
17) · 70 (Form 4361 → Sch C) · 84 (§469(i) $25,000 allowance, OPEN, blocks
client 2490) · 85 (KEN RULING: 1099-MISC box 3 backed out of AGI — worksheet
presentation, $0 face impact). **These plus BATCH-015's twenty are the
queue; triage into batches of ten by yield before building.** Ken's
assignment: this lane builds them; Codex enters returns.

**🏁 1040 BATCH-014 IS CLOSED (s332, 2026-09-05) — THE FIRST BATCH CODEX BUILT,
REVIEWED AND LANDED BY THIS LANE. The one-writer lock is LIFTED; `main` is the
working branch again.** Merge `57e00201` (`--no-ff`, review record in the
message); deploy `dep-dadnsl142hec73boj0kg` LIVE (Render-API verified);
migrations 0379–0388 applied on the shared DB (`showmigrations` ✓). Annex on
the batch file in `CC Changes Done`, Codex's own draft beside it.
- **7 BUILT** — `schb_fields` (Sch B Part III answers, nullable, explicit No) ·
  the MFS six-month residence checkbox made NULLABLE + `mfs_eic_special_rule`
  in the lane · `ira_distribution_summaries` (one filed IRA pool per owner, NO
  fabricated 1099-R, MeF refuses by name until the real document) ·
  `mortgage_interest_details` + `medical_expense_details` (ordered filed rows
  deriving the Schedule A aggregates ONCE; statements, not fake 1098s) ·
  `education_claimed_on_another_return` (the ACTUAL claim; verified against
  the 2025 i8863) · `schedule_cs[].gross_receipt_details` (non-document rows
  reconciling line 1 once with the NEC/MISC roster).
- **1 REFUTED** (item 1 — Codex found the worksheet inputs on page 8; the
  existing route ties). **1 STOPPED FOR KEN** (item 2, below). **1 DEFERRED**
  (item 5 — no IND-CR 212 spec; the fixture ties through
  `g_indcr_other_credits` today; RS-lane authoring item).
- Review: every migration read (db_default where required, none on the new
  tables' FK per 0375/0377, RLS default-deny); PII scan with a POSITIVE
  CONTROL (0 hits); every diagnostics diff read (none weakened); broad gate
  on a fresh LOCAL test DB **1,562 passed / 2 failed — and the identical run
  on unmodified main fails the same two** (test-order dependence on seeded
  diagnostics; DEFERRAL_AUDIT (1)). Published schema regenerated.
- ⭐ **`config.settings.test` runs the gates on LOCAL PostgreSQL** (never the
  shared Supabase): `DJANGO_SETTINGS_MODULE=config.settings.test` + a
  synthetic `SECRET_KEY`; pytest invocations must be SERIAL (the conftest
  kills test-DB connections on exit).

**(ruled 2026-09-05 — kept for the record; see the block above)** Two rulings from BATCH-014 as they were put to Ken:
1. **Item 2 — how a jointly filed capital loss splits between spouses on the
   Georgia RIE worksheet.** The filed worksheet's columns equal each spouse's
   SIGNED NET (own current result minus own carryover: −508 / −1,670 → RIE-17
   12,639 / 6,031). The engine prorates the return-level loss by WEIGHT
   (R-GA500-RIE) → 12,253 / 6,417. Same total either way. **Say which is the
   rule**; if the worksheet's, it is a Rule Studio amendment first, then the
   paired-carryover build. Nothing is coded around it; the packet holds.
2. **Item 5 — IND-CR 212 (community-based faculty preceptor credit)** needs a
   Rule Studio spec before a per-credit form exists. The return imports today
   through the aggregate line-20 fact.

**⛔⛔ THE CC CHANGES QUEUES STILL HOLD WORK (re-swept s332). Ken decides whether
Codex or this lane builds them — the contract and the review recipe now exist
(s332 memory). Work these before the Form 6251 reader.**

| Lane / file | Items | Posted | State |
|---|---|---|---|
| 1040 · `CC_CODE_CHANGES_1040_BATCH-014.md` | 10 | 2026-09-04 | 🏁 **CLOSED s332** (Codex-built, reviewed, landed) — in Done |
| 1040 · `CC_CODE_CHANGES_1040_BATCH-015.md` | **20** | 2026-09-04 | ⚠ NOT a work batch — it is the *pending list* published alongside 014 ("current count 20/20"). Triage into batches of ten; do not treat as one unit |
| 1040 · `CC_CODE_CHANGES_BATCH-296.md` | ~20 open of 85 | running file | ⚠ the lane's CORRESPONDENCE FILE, not a batch (see the correction above); carries the **item-writing convention** — read its header before any annex |
| legacy root · `CC_CODE_CHANGES_NZ_2026-08-03.md` | — | 2026-08-03 | still undrained (the legacy root is not swept clean) |
| 1120-S · `CC Changes\` | — | — | EMPTY (README only) |

BATCH-014's ten, one line each (Tom lane, every one with a named fixture and
a tie gate): filed Schedule C **line-30 home office** with no method
worksheet · **paired taxpayer/spouse capital-loss carryover** for Georgia RIE
(the schema holds only one) · **Schedule B Part III** foreign-account/trust
answers (no field exists) · the **MFS final-six-months lived-apart**
exception, separate from the Social Security lived-together fact · Georgia
**IND-CR 212** preceptor credit · a **source-backed aggregate IRA
distribution** with withholding and thin payer metadata · Schedule A **1098
mortgage-interest detail rows** (only the 8a aggregate survives) · Schedule A
**medical detail rows** · Form 1040 **line 12a dependency ELIGIBILITY vs
CLAIM** for Form 8863 · **Schedule C gross-receipts detail rows** without
inventing 1099s.

⚠ Two of BATCH-015's twenty overlap work from tonight and must be re-verified
before building: **#3 "Schedule 2 line 8 reported directly without Form
5329"** (the box the face itself offers — the new f5329 reader now decomposes
line 8 only when a 5329 face is present, so this is the missing sibling
route) and **#6 "Form 8606 taxable Roth earnings into Form 5329 Part I"**.
**#4 asks for Form 3800 support** — this session typed 3800 as a NAMED WALL,
so that item would open the class.

**▶ THE s331 LANDINGS (recorded at that close; still accurate).** Nothing
is in flight: both extract runs and both commit batches finished, the working
tree is committed and pushed, and both deploys are Render-API verified LIVE.
- Batch `s331-unknown-commit-001` — 24 landed, 4 fenced (they already tie).
- Batch `s331-5329-commit-001` — **8 landed, 1 HELD**: packet …2404 stages
  with `depreciation_assets[0..3].link_key: must be the linked activity's
  name` — a PRE-EXISTING extractor gap in the depreciation reader, unrelated
  to the 5329 work. It is the first item to pick up, and it is small.
- Both commit scripts are RE-RUNNABLE: each fences a return that already
  committed in its batch, so re-running lands only what is missing.
  `1040	mp\commit_s331_5329.py` · `commit_s331_unknown.py`.

**🏁 THE FORM 5329 READER (`5ed62a3d`).** `scripts/taxwise1040/f5329.py`,
calibrated against every f5329 page in the book (13 packets, 24 pages) —
never modelled. Parts I and VII import (identity gates re-run the engine's
own math over the printed values); every other part refuses BY NAME with
the whole marker map transcribed, so a value anywhere on the face is
DETECTED rather than dropped. Schedule 2 line 8 — where both the Part I 10%
and the Part VII 6% land — now decomposes instead of refusing.
- **Part IV refuses on a REAL LANE GAP, not caution.** It has a live corpus
  witness, but the face never prints the 12/31 account value and the lane
  has `roth_value` with no `roth_value_at_least_excess` — the assertion
  BATCH-013 #3 built for the HSA alone. **The sibling witness that note
  asked for now exists; the field is a build item.**
- Geometry worth keeping: the columns were measured off the page's own
  ruled drawings (inner marker box x 371-395, outer 474-496), a LEFT-GAP
  rule separates a boxed value from a word inside a sentence (two template
  tokens land in a value window and false-refused all 24 pages without it),
  and **the header fence is PER PAGE** — a continuation page's first marker
  sits above page 1's fence.

**⚠⚠ THE WALL BEHIND THE WALL — Form 8889.** Opening the 5329 class showed
the 8889 reader REQUIRED lines 6 and 8, and **this vendor does not print a
pure carry-forward line**: every one of the ten packets printed line 5 but
neither 6 (= 5, no married split) nor 8 (= 6 + 7), and most omitted 12 and
13. An omitted line now asserts nothing and the engine derives it; a
PRINTED line that disagrees still refuses. Safe because the import row
never carried those lines — they are checks, not inputs. This released two
further packets that had nothing to do with the 5329.

**⚠⚠⚠ AND THE CENSUS LESSON: A SOLE-WALL CENSUS OVER A *BLOCKING* CLASS
MEASURES ONE WALL, NOT ONE UNIT OF WORK.** A `ROLE_BLOCK` page
short-circuits the packet before any downstream parser runs, so its refusal
list carries exactly one entry BY CONSTRUCTION. All ten "sole wall = f5329"
packets had Form 8889 sitting behind it, invisible until the class opened.
**Only a PARSER_BACKED class's sole-wall count predicts releases; for a
blocked class it is a lower bound of one.** Size the 6251 unit accordingly.

**🏁 WHAT THE PASS DID.** 37 not-yet-filed Gail packets were held by ONE wall:
"uncovered value-bearing page: unknown". Typing those pages was one unit:
- **The 1099-NEC detail report reader** (`reports.parse_nec_detail`, the
  9-column dash-ruler table grouped by activity label). Rows land on the
  `nec_1099s` section BATCH-013 #4 built the same day; each row reconciles to
  its Schedule C by name (exact → unique prefix → unlinked with a warning),
  and a per-group subtotal mismatch refuses. **29 NEC rows landed, 62 in the
  app, ALL linked to an activity — zero orphans.**
- **Ruling 1 EXTENDED from Lacerte to the TaxWise books** (an explicit
  extension, not a new ruling): a non-Georgia state page is typed
  `state_<code>`, role ignore, and the codes emit as `out_of_scope_states`
  (OK · CA · OR · NC · DC · CO · SC · MI). Georgia never appears; an
  unnameable state page still refuses. Two packets landed carrying the
  marker (NC, OR).
- **Real forms with no reader became NAMED walls** — 1116 (anchored so the
  AMT twin stays `unknown` rather than double-counting), 3800, 8615, 8379.

**Acceptance on the 346-packet merged book: emitted 10 → 34; "unknown" pages
37 sole walls → ZERO anywhere on the book.** Of the 34: 24 draft (all landed),
6 already filed (skipped — filed = tie-verified), 4 fenced (below).
Gates: 307 extractor tests green; teeth proven by injecting both NEC defects.

**⚠⚠ TWO LESSONS WORTH THE SESSION** (full detail in the s331 memory file):
1. **WITNESS THE MASTHEAD, NEVER MODEL IT.** The first `f3800` fixture was my
   own model of the head. TaxWise prints `Form   3800` with a RUN OF SPACES;
   the test passed against the model while the real page stayed `unknown`
   through an entire acceptance run. Also: one state = SEVERAL mastheads
   (MI-1040 p1 vs p2/3 vs form 3423 vs Schedule NR vs MI-8453 vs the voucher).
2. **`refresh_from_db()` DISCARDS what the commit set in memory.**
   `commit_staged_return` assigns `staged.resolved_return` without saving; a
   refresh before reading it returns `None` and the landing check reads a
   vacuous "0 rows landed". Read the in-memory object or `result["tax_return_id"]`.
   (Also: `out_of_scope_states` must be OMITTED when empty — `[]` is refused.)

**▶ THE FOUR FENCED PACKETS — KEN'S OWN RULE APPLIED, NOTHING OVERWRITTEN.**
Four packets refused with "target return already carries rows". Ken's rule
(2026-09-04): *if what is already in the app ties to TaxWise, keep it and
ignore the new work; only if it does NOT tie do we use the new work, and only
if that ties.* Measured: **all four ALREADY TIE on every answer-key line
(15/15, 16/16, 16/16, 16/16)** — the existing entry is correct, so the new
work was ignored and no rows were touched. They sit in **Draft** only because
they never passed through a commit (the s327 flip lives inside the commit).
**Clients 2777 · 3630 · 4159 · 4160 need the filed flag and nothing else** —
a status-only sweep, not a re-entry. Probe: `1040\tmp\probe_s331_four_tie.py`.

**▶ THE REMAINING GAIL WALLS — RE-MEASURED, AND THE FIRST NUMBERS WERE WRONG.**
⚠⚠ **A CENSUS BUCKET THAT STRIPS DIGITS CANNOT NAME A FORM.** My first pass
replaced every digit with `N`, so `f6251` and `f5329` collapsed into one class
(`uncovered page: fN`, 29) — and I then read a *whole-book* histogram (filed
and multi-wall packets included) as if it were the sole-wall count, publishing
"6251 ×15 · 5329 ×13". Corrected census keeping the form number
(`1040\tmp\probe_s331_walls_by_form.py`; refused 312 → filed 140 · draft 159 ·
unresolved 13; **sole-wall 94, multi-wall 65**):

| Sole wall (fixing it releases the packet) | n |
|---|---|
| not a TaxWise 1040 packet (**the GA-only reprint pile — Ken's print, parked**) | 16 |
| **Form 5329** | **10** |
| Form 6251 | 6 |
| Form 1116 | 5 |
| Schedule 2 "additional tax on early distributions" (**the 5329's own number**) | 5 |
| lump-sum Social Security on the income worksheet | 4 |
| sched_line_detail | 4 |
| detail_sheet · ctc_ext_carryover_wks | 3 each |
| Form 1310 · Sch 3 foreign tax credit > face · Sch 1-A W-2 tips · 8995 Part I | 2 each |
| ownerless documents · 1099-R exclusion marker · 4562 · 8615 · ga500x · misc | 1 each |

Form 6251 also sits behind another wall in **11** more packets, so its reader
releases 6 now and helps 17 in total. `state_nonconformity_wks` appears in 10
multi-wall packets and nothing else.

**⛔⛔ KEN — THE PRINT ASK STANDS (ruling 5's retraction is REVERSED).** You do
need to print the full federal + GA for the **59** Gail 1040-lane clients. The
s330 "everyone already has both returns" finding counted any page containing
the string "(Form 1040)" — which appears on Schedule C/SE/A footers and on the
orphan Schedule 1 page a Georgia-only print drops at the end. Measured three
independent ways, all agreeing: **18 of the 61 held clients carry a real Form
1040 page 1; 43 do not**, and the 18 are not enterable either (no source-detail
page in any of the 61). Census: `1040\tmp\s331_gail_masthead.json`. DECISIONS
ruling 5 carries the method and both figures.

**▶ KEN'S FIVE ENTRY-LANE RULINGS — WHERE THEY STAND:**

| # | Ruling | State |
|---|---|---|
| 1 | Non-Georgia state → file federal + GA only | 🏁 SHIPPED both lanes (Lacerte `cab486ca`, TaxWise `c43ea59b`) |
| 2 | §6654 penalty deltas: record and file | ▶ NEXT-ish (~10 packets) |
| 3 | A hand-keyed return that ties IS filed | ▶ open (client 3250) |
| 4 | Ignore the doubled Sch 1-A / Sch 3 pages | 🏁 SHIPPED `34553e7a` (5 classes, not 2; merge artifact, not a print setting) |
| 5 | ~~Gail needs no reprint~~ | ⛔ REVERSED — Ken must print (above) |

**⚠ Ruling 1 carries a ROADMAP ask that is NOT a licence:** Ken wants state
software built soon. That is a scope conversation, tracked in BUILD_ORDER. Do
NOT start entering other states' returns on the back of ruling 1.

**▶ NEXT, in the order that lands the most returns:**
1. **The named TaxWise walls by MEASURED sole-wall count** — **5329 (10 + the
   5 Schedule 2 siblings)** → 6251 (6 sole, 17 total) → 1116 (5) →
   sched_line_detail (4) → detail_sheet / ctc_ext_carryover_wks (3 each).
   The lane and the engine already exist for both 5329 (`form_5329s`) and
   6251 (the `amt_*` taxpayer facts + `compute_6251`), so these are
   READER-ONLY units. Drive ONE single-wall packet by hand through each
   before trusting the census (s328).
2. **Ruling 2's penalty acceptance** (~10) + **ruling 3** (client 3250).
3. **The four fenced clients' filed flag** (2777 · 3630 · 4159 · 4160).
4. **The Lacerte face readers by wall count** — Sch 3 (107) → A (37) → B (32)
   → C (18). ⚠⚠ **A WALL CENSUS COUNTS FIRST BLOCKERS, NOT RELEASABLE
   PACKETS** — 234 of 255 Lacerte packets are blocked by more than one class,
   so the book needs the whole face chain before anything emits. Sch 3's exact
   cause is known: **Lacerte splits the sub-letter into its own word (`5` at
   x=46, `a` at x=51) where TaxWise prints `5a`**; its gutters already match.
5. **⑥c `manage.py merge_client`** (D-044; the CRM's 34 duplicate pairs).
6. **The 1065 import** — 95 partnership returns behind it, 66 packets in Inbox.

**▶ FRESH CENSUS (s331 close — a count is a timestamp, re-run it):** 2025
federal 1040 shells **2,978 — filed 1,387, draft 1,587** (filed was 1,258 at
s330 close; this lane landed 24 of the rise, the entry lane the rest).
1120-S 327 / 193 filed. 1065 104 / 9 filed — **the partnership side is the
thin one**; the 1065 import has never been built.

**▶ EARLIER THE SAME SESSION (all shipped, all deployed):**
- **1040 BATCH-013 CLOSED** — ten Codex product gaps, ONE deploy
  (`24bee7aa`, migrations 0372–0378), nine built and one refuted; filed to
  `CC Changes Done` with its annex. New tables `Form1099Nec` +
  `CashContribution`; `entry_basis: "source_summary"` on 1099-R/MISC; the
  line-`1h` route; the 5329 HSA line-48 assertion; Schedule E line-19
  statement rows; positive-net GA RIE line 9. **⛔ KEN, one flag, nothing
  blocks:** item #2's GA RIE **line 2 (earned)** placement for a described
  line-1h row is an assumption from the face's own label — say so if it
  should be unearned or nowhere.
- **Lacerte Schedule 1 + Schedule 2 readers wired** (`9214f74b`) — walls
  172 → 0 and 112 → 0; emitted still 0 (the first-blocker lesson above).
- **The cover letter's SECOND layout typed `ignore`** (`57d13e12`).
- **⚠ RETRACTED the same night — "139 Gail packets held only by a merge
  decision":** 138 of the 139 are already FILED. The pipeline refusing to
  overwrite them is CORRECT, and Ken's proposed rule is already the
  behaviour. One draft: client 1017.

**▶ OPEN FROM THE ENTRY LANE (carried, not this lane's work):** only #2019 of
Gail's ten holds is open, on the GEORGIA military gate (staged `valid`,
uncommitted, no_tie by exactly $908 of GA tax; hold note beside the packet).
Reader item: the extractor mis-maps the under-62 military exclusion (Schedule 1
line 7b) onto the answer key's `RIE-TP-17` — queue with the reader items.
`mark-filed` on already-filed batches is a NO-OP (`filed 0 / skipped N`), not
a failure — count from the DATA.

**▶ RELAYED (recorded, NOT this lane's work):** Nilkanth Diamonds #3413 is
FILED `tie_with_exception`; the transmitted AAA is −253,783 and the correct
figure is −233,602 — **the 2026 return must open at −233,602, not the paper.**
RAPTAP HOLDINGS LLC #3728 (1065, `in_progress`) was entered by Codex,
unreviewed by either lane.

**▶ STANDING (unchanged, s329/s330):** every delvio-tax SHA quoted before
2026-09-02 night is STALE — resolve through `docs/history/rewrite-2026-09-02-commit-map.txt`;
REWRITE #3 (~86 TaxWise surnames in old STATUS blobs) is Ken's call
(REVIEW_QUEUE). Clients (delvio-crm) is the hub (D-042/043/044); the four
suite doors are live (`a3cedfd4`); ⑥c `merge_client` is the next hub unit.
Slate v2.3.1 is canonical; delvio-1099 merged on Ken's go. `RESEARCH_SERVICE_TOKEN`
still unset on both Render services (both research doors 503).

**⛔ WAITING ON KEN — WEDNESDAY AGENDA (s332):** the two season-2026 design
questions Ken raised (what replaces the answer key; how scanned documents get
in) — recommendations in REVIEW_QUEUE, shelf units in BUILD_ORDER.
**Also for Wednesday (s332, later):** add `manage.py seed_rules` after `migrate` in
`build.sh`? — the deploy migrates only, so every NEW diagnostic code is inert
until someone seeds it by hand (DEFERRAL_AUDIT (10)); the Schedule B ownership
question (A2(b), REVIEW_QUEUE); code L in `EARLY_CODES`; 296 #85 (the §108(f)(5)
other-income placement on the GA RIE worksheet — the four Georgia misses still
open on client 2490 after 296 #84's federal fix).

**⛔ WAITING ON KEN (carried):** the BATCH-013 item-2 RIE placement flag above ·
the packet in tmp/s328_ken_questions.md (GA 500 p1 names a different primary
SSN) · seed ONE client (the …4641 taxpayer in Jenny's book; do NOT edit #4054)
· client #3572's contaminated name (#4514 same shape) · three clients with no
2025 1040 shell · client 2149's filed GA 17a = 2 exemptions on a
single/zero-dependent return · the …4203 W-code question · the asset
METHOD-DERIVATION TABLE review · vendor-name allowlist for the mirror guard? ·
carried: 1071 · 1141 · R-GA500-RIE · 4059 W-2G address · Sch D carryover · GA
RIE L10 · 4081's $169 · standing 1–8 · 2a scope flag · AL 40 · the 4 Tom-book
holds (…8505 · …2276 · …2827 · …8791) · `D_1099MISC_RECON` per-document vs
aggregate (DEFERRAL 19e) · the Schedule C address model gap (DEFERRAL 19a).

**▶ BUILD QUEUE after the named walls:** ④ the three re-raised Lacerte engine
holds (clients 1922, 2386, 3517) · ⑤ the GA 7b military-exclusion engine leg
(8 witnesses; waits on the states-lane spec export) + the 7c/7f DIS
transcription · ④ the shadow-2210 reader · carried: the 8615 parent-first
guard · the zero-activity GA-attach gap · the Schedule C address (DEFERRAL
19a) · the IRS1099NEC document question (19b) · the AMT-1116 twin head, still
unwitnessed in this corpus.

⚠⚠ **ANOTHER CC ACCOUNT WRITES THIS LANE — THE SHARED FILES ARE THE ONLY
PLACE ITS WORK IS VISIBLE.** Do not order events by `updated_at`.
