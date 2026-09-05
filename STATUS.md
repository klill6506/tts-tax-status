# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s332 close, 2026-09-05

**▶▶ START HERE (s332): BATCH-014 is CLOSED and the lock is LIFTED (block below). Earlier the same session, TWO READER UNITS SHIPPED AND LANDED — the
unknown-page pass (`c43ea59b`, deploy `dep-dadeepp7lnhs73d4nejg`) and the
Form 5329 reader (`5ed62a3d`, deploy `dep-dadfq1h7lnhs73d6hlig`), both LIVE.
The merged Gail book went from 10 emitted → 34 → 43, and **24 + 8 returns
committed, EVERY ONE A TIE, zero no-ties** (one held, below). The next unit
is the Form 6251 reader (6 sole walls, 17 packets in total) — but read the
census warning before sizing it.** Nothing below needs re-asking.

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

**⛔ KEN — TWO RULINGS FROM BATCH-014, NEITHER BLOCKS ANYTHING ELSE:**
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
| 1040 · `CC_CODE_CHANGES_BATCH-296.md` | 14 | 2026-09-02 | ⚠ UNWORKED and older than 013/014. Also carries the **item-writing convention** (symptoms + evidence; a cause theory must be LABELLED a theory) — read that header before writing any annex |
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
