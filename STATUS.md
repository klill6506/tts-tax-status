# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s331 close, 2026-09-04

**▶▶ START HERE: THE UNKNOWN-PAGE PASS IS SHIPPED, ACCEPTED AND LANDED
(`c43ea59b`; deploy `dep-dadeepp7lnhs73d4nejg` LIVE). 24 returns committed,
every one a tie, zero no-ties. The next unit is the NAMED extractor walls in
measured order — 6251 (15) → 5329 (13) → 1116 (5) → 4562 (3) →
sched_line_detail (4) → 1310 (2).** Nothing below needs re-asking.

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

**▶ THE REMAINING GAIL WALLS (sole-wall, not-filed, after the pass):**
6251 ×15 · 5329 ×13 · **the GA-only reprint pile 16 (Ken's print, parked)** ·
1116 ×5 · sched_line_detail ×4 · 4562 ×3 · ctc_ext_carryover_wks ×3 ·
detail_sheet ×3 · 1310 ×2 · engine items ~15. Census:
`1040\tmp\probe_s331_unfiled_walls4.py`. Every one is a NAMED reader now.

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
1. **The named TaxWise walls by measured count** — 6251 (15) → 5329 (13) →
   1116 (5) → sched_line_detail (4) → 4562 (3) → 1310 (2). Drive ONE
   single-wall packet by hand through each before trusting the census (s328).
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
