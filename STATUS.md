# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s331 close, 2026-09-04

**▶▶ START HERE: BATCH-013 IS DONE — ten items, one deploy, nine built, one
refuted, filed to `CC Changes Done` with its annex. The next unit is SPENDING
KEN'S FIVE ENTRY-LANE RULINGS (DECISIONS *"Five entry-lane rulings clearing the
back door"*), in the order that lands the most returns.** Nothing below needs
re-asking.

| # | Ken's ruling | What it releases | Work |
|---|---|---|---|
| 1 | A Lacerte packet with a NON-GEORGIA state files **federal + GA only** | **47 packets** | drop the refusal; set the other state aside |
| 2 | §6654 penalty deltas: **record and file** | ~10 packets | accept the delta as a vendor divergence, note it |
| 3 | A **hand-keyed** return that ties **IS filed** | client 3250 + the entry lane | extends the standing commit authorization |
| 4 | **Ignore** the doubled Sch 1-A / Sch 3 pages in code | **78 packets** (62 + 16) | reader tolerates 4 pages where it wants 2, and 2 where it wants 1 |
| 5 | ~~Gail needs **NO reprint**~~ | ⛔ **REVERSED s331** | **KEN MUST PRINT FEDERALS** — see below |

**🏁 RULING 4 IS SHIPPED AND MEASURED (`34553e7a`; deploy `dep-dad4fidckfvc7393gf7g`
LIVE).** ⚠⚠ **THE CAUSE WAS THE MERGE, NOT A PRINT SETTING.** The Inbox
originals carry **ZERO** duplicates (1,606 files censused). Every duplicate is
in the **merged** packets, because the Georgia print binds a verbatim COPY of
the federal return, so whole forms repeat — Schedule 1-A at `[3,4,22,23]`,
Schedule 3 at `[4,32]`. Cleared for **five** classes, not the two the census
named: Schedule 1-A, Schedule 3, Schedule 1 (the wall behind the wall),
Schedule 2 and Schedule A. First printing wins (this lane's own rule); a later
copy that **DIFFERS still refuses** (an amended page bound behind an original
is a real difference), and a count that is not a whole number of printings
refuses by name. Verified duplicates: 120/120 · 183/183 · 21/21 byte-identical.
**Acceptance on the 346-packet merged book: all five walls report ZERO and
emitted returns went 4 → 10.** 14 new tests, 370 green, teeth proven by
injecting both defects. Remaining walls there: `not a TaxWise 1040 packet` (18)
and the unknown-page runs (the peer lane has now typed all 73 — 13 forms, and
**the 1099-NEC detail report is 40 of them**, which lands on the `nec_1099s`
section built earlier tonight; that is the next reader).

**⛔⛔ KEN — RULING 5'S RETRACTION IS REVERSED. YOU DO NEED TO PRINT FEDERALS.**
The s330 finding ("59 of 63 already carry both returns, ZERO lack a federal")
counted any page containing the string **"(Form 1040)"** — which appears on
Schedule C, SE and A footers, and on the **orphan Schedule 1 / 1-A page a
Georgia-only print drops at the end**. Measured three independent ways, all
agreeing: **18 of the 61 held clients carry a real Form 1040 page 1; 43 do
not** (this lane used the classifier's `f1040_p1` masthead type, which cannot
match a schedule footer; of the 43, **15 carry an orphan Schedule 1-A** — the
very pages the loose test counted). The peer lane adds that the **18 are not
enterable either** — none of the 61 held packets carries any source-detail
page (`main_info` / `w2_detail` / `r1099_detail` / `income_wks`), verified
against a 5-of-5 positive control on packets that committed. **So the practical
answer is the 2026-08-31 ask: print the full federal + GA for the 59 1040-lane
clients** (61 less the fiduciary 501 …4685 and the partnership 700 …8078). The
peer lane has sent you the 59. Census: `1040\tmp\s331_gail_masthead.json`.
DECISIONS ruling 5 is amended with the method and both figures.

**⚠ Ruling 1 carries a ROADMAP ask that is NOT a licence:** Ken wants state
software built soon. That is a scope conversation, tracked in BUILD_ORDER. Do
NOT start entering other states' returns on the back of ruling 1.

**▶ s331 LATE SITTING — TWO MORE SHIPPED, ONE CORRECTED, ONE MEASURED:**
- **Lacerte Schedule 1 + Schedule 2 readers WIRED (`9214f74b`)** — the shared TaxWise
  parsers read Lacerte as-is (Sch 1 60/60 both pages; Sch 2 21/21). Walls 172 → 0 and
  112 → 0; **emitted STILL 0** — Sch 3 (107) / A (37) / B (32) / C (18) sit behind.
  ⚠⚠ **A WALL CENSUS COUNTS FIRST BLOCKERS, NOT RELEASABLE PACKETS** — 234 of 255
  Lacerte packets are blocked by MORE THAN ONE class; the book needs the whole face
  chain before anything emits. Sch 3's exact cause is known: **Lacerte splits the
  sub-letter into its own word (`5` at x=46, `a` at x=51) where TaxWise prints `5a`**;
  its gutter positions already match. Sch A / B fail on their own bands (control test
  refuses to mark them covered unmeasured).
- **The cover letter's SECOND layout typed `ignore` (`57d13e12`)** — 7 unknown pages
  across three books, anchored on the letter's own boilerplate, never the name.
- **⚠ RETRACTED THE SAME NIGHT — "139 Gail packets held only by a merge decision":**
  checked against the LIVE DB, **138 of the 139 are already FILED** (= tie-verified,
  Ken's s327 rule). They are second copies of returns already landed; the pipeline
  refusing to overwrite them is CORRECT and Ken's proposed rule (keep what ties,
  replace only with a payload that ties) is already the behaviour. One draft: client
  1017. No decision needed.
- **THE HONEST GAIL POOL (live status, pipeline resolver):** 336 refused → 140 filed
  (done) · 13 unresolved · **183 draft**, of which 118 have exactly ONE wall:
  **unknown page 37** (Form 1116 · the SSN-headed 1099-NEC detail report · OR-40 /
  NC D-400 state pages → ruling 1's set-aside extended to TaxWise) · the GA-only
  prints 18 (Ken's reprint pile, parked) · one uncovered form 8 (6251 ×3, 1310 ×2,
  5329, 4562, 4952) · engine items ~15. **NEXT UNIT = the unknown-page pass (37).**
  Scripts: `1040\tmp\probe_s331_unfiled_walls3.py` (the census), the peer's inventory
  `1040\tmp\gail-held-20260903\UNKNOWN-PAGES-BY-FORM.md`.

**▶ NEXT, in the order that lands the most returns:**
1. **Ruling 4's duplicate-page tolerance** (78 packets, an afternoon).
2. **Ruling 1's out-of-state release** (47 packets, a refusal to drop).
3. **Ruling 2's penalty acceptance** (~10 packets) + **ruling 3** (client 3250).
4. **The Lacerte face readers by wall count** — Sch 1 → 8995 → Sch D → Sch B →
   Sch 2/3 → 8949 → Sch A → Sch C. Drive ONE single-wall packet by hand through
   each before trusting the census (s328's lesson).
5. **⑥c `manage.py merge_client`** (D-044; the CRM's 34 duplicate pairs are the input).
6. **The 1065 import** — 95 partnership returns behind it.

**🏁 SHIPPED THIS SESSION — 1040 BATCH-013, ONE DEPLOY (`4ba94d83` → `c8bc9f4b`
→ `24bee7aa`; migrations 0372–0378; deploy: see the line at the foot of this
block). Full per-item annex: `D:\tax-test-data\1040\CC Changes Done\CC_CODE_CHANGES_1040_BATCH-013.md`.**
- **#1** GA RIE line 9: a POSITIVE federal line 7 allocates by each owner's NET
  current-year position (loss rows in; joint loss halved by the #78 convention).
- **#2** `other_income_items[].route: "1h"` — a described line-1h row; composed
  1h + the type literal beside the line; rides GA RIE line 2 (earned). **⛔ KEN
  (one flag, nothing blocks):** that L2 placement is an assumption from the
  face's own label; say so if it should be unearned or nowhere.
- **#3** `form_5329s[].hsa_value_at_least_excess` — the filed line 48 IS the full
  excess; silences `D_5329_003` for HSA only; refused beside `hsa_value`.
- **#4** `nec_1099s` (new table `Form1099Nec`): payer identity, activity link
  (box 1 feeds NOTHING), box 4 → 25b, box 5 → state rosters, three diagnostics,
  UI CRUD. No IRS1099NEC MeF document (the MISC precedent — DEFERRAL 19b).
- **#5 REFUTED**: the published schema never carried `business_address` and
  `schedule_cs` is `additionalProperties: false`; the MODEL lacks the address
  (DEFERRAL 19a, a build item). Schema + SUPPORTED-SECTIONS regenerated.
- **#6 / #10** `entry_basis: "source_summary"` on `r_1099s` and `misc_1099s` — a
  filed aggregate with no payer document. The 1099-R row is skipped by the
  IRS1099R document list (the aggregate-pension fixture's `D_EFILE_001` clears); neither may carry
  withholding; the MISC row is 8z-only. **⚠ The item-10 e-file premise was a
  MODEL of the composer — no IRS1099MISC document is transmitted for any row.**
- **#7** farm-routed 1099-MISC reconciles against the whole Part I roster.
- **#8** `rental_properties[].other_expenses` accepts the line-19 statement rows;
  the PRINT now sums them into line 19 (line 20 and MeF already had); a Schedule
  E line-19 statement page.
- **#9** `cash_contributions` (new table `CashContribution`): Schedule A line 11
  rows; the flat `scha_charitable_cash*` facts DERIVE from them on every commit
  and CRUD; a flat fact beside rows must reconcile exactly; a line-11 statement.
- **Gail-lane defects (STATUS 0b):** `depreciation_filed` now WRITES the filed
  figure to the depreciation line (census first: **7** filed-1040 rows key it,
  all equal to their line — nothing moved); the TaxWise 1099-R reader strips its
  own word-join from box 7 (`"4 D"` → `"4D"` — build-queue ② closed at the
  source). **LIC-NODEP census: 6 filed GA-500s** with AGI < $20,000, GA tax > 0
  and no gate — **clients 1423 · 2403 · 2796 · 2867 · 3157 · 4371** — for the
  entry lane's face check (the gate also needs "not claimable as a dependent").
- Gates: 308 green across the new + touched suites; 526 flow assertions;
  `makemigrations --check` clean. No client change (no typecheck needed).
- **Deploy:** Render `dep-dad3i3navr4c73fdinjg` **LIVE** at `24bee7aa` (Render-API
  verified; migrations 0372–0378 applied on the shared DB, checked by
  `showmigrations`).

**▶ FRESH CENSUS (s330 close — a count is a timestamp, re-run it):** 2025
federal 1040 shells 2,978 — filed 1,258 (s331 census read 1,262), draft 1,716.
GA-500 under a filed federal: 1,224 — 1,187 filed. 1120-S 327 / 193 filed.
1065 104 / 9 filed — **the partnership side is the thin one**; the 1065 import
has never been built and 66 packets sit in `1065\Inbox`.

**▶ WHERE THE 1,716 UNENTERED 1040s SIT:** the back door has ever reached only
75 of them; 1,641 have never had a staged row; 1,218 packets sit refused as of
their latest run — the ceiling is PACKETS AND READERS. Refusal census (re-measure
before building): Lacerte 252 of 255 refuse — Sch 1 181 · 8995 172 · Sch D 131 ·
Sch B 126 · Sch 2 119 · Sch 3 113 · 8949 109 · 1116 95 · 4562 84 · Sch A 76 ·
Sch C 75 · Sch E p2 75 · GA 4562 72 · 8582 65 · 7203 62. TaxWise 963 packets:
unclassifiable 148 · asset detail 72 · no federal face 66 · doubled Sch 1-A 62
(ruling 4) · Sch E 46 · 5329 42 · 6251 41.

**▶ OPEN FROM THE ENTRY LANE (carried, not this lane's work):** only #2019 of
Gail's ten holds is open, on the GEORGIA military gate (staged `valid`,
uncommitted, no_tie by exactly $908 of GA tax; hold note beside the packet).
Reader item: the extractor mis-maps the under-62 military exclusion (Schedule 1
line 7b) onto the answer key's `RIE-TP-17` — queue with the reader items.
`mark-filed` on already-filed batches is a NO-OP (`filed 0 / skipped N`), not
a failure — count from the DATA.

**▶ CODEX / the entry lane may resume one-by-one entry.** If it reports being
blocked, the known cause is a **fresh production session that only Ken can
mint**. Re-pass instructions for the ten BATCH-013 fixtures are in the annex.

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

**⛔ WAITING ON KEN (carried):** the item-2 RIE placement flag above · the
packet in tmp/s328_ken_questions.md (GA 500 p1 names a different primary SSN)
· seed ONE client (the …4641 taxpayer in Jenny's book; do NOT edit #4054) ·
client #3572's contaminated name (#4514 same shape) · three clients with no
2025 1040 shell · client 2149's filed GA 17a = 2 exemptions on a
single/zero-dependent return · the …4203 W-code question · the asset
METHOD-DERIVATION TABLE review · vendor-name allowlist for the mirror guard? ·
carried: 1071 · 1141 · R-GA500-RIE · 4059 W-2G address · Sch D carryover · GA
RIE L10 · 4081's $169 · standing 1–8 · 2a scope flag · AL 40 · the 4 Tom-book
holds (…8505 · …2276 · …2827 · …8791) · `D_1099MISC_RECON` per-document vs
aggregate (DEFERRAL 19e) · the Schedule C address model gap (DEFERRAL 19a).

**▶ BUILD QUEUE after the rulings:** ③ the TaxWise extractor walls by measured
count (f6251 = 13 · sched_line_detail = 6+ · f5329 · the classifier patch, GA
part-year detector first) · ④ the three re-raised Lacerte engine holds (clients
1922, 2386, 3517) · ⑤ the GA 7b military-exclusion engine leg (8 witnesses;
waits on the states-lane spec export) + the 7c/7f DIS transcription · ④ the
shadow-2210 reader · carried: the 8615 parent-first guard · out_of_scope_states ·
the zero-activity GA-attach gap · the Schedule C address (DEFERRAL 19a) · the
IRS1099NEC document question (19b).

⚠⚠ **ANOTHER CC ACCOUNT WRITES THIS LANE — THE SHARED FILES ARE THE ONLY
PLACE ITS WORK IS VISIBLE.** Do not order events by `updated_at`.
