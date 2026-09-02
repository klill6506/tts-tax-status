# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s327, 2026-09-02

**▶ THE LACERTE-LAYOUT EXTRACTOR PASS, LEG 1, SHIPPED (`6f1a7858` +
`a1ce1047`, `scripts/lacerte1040/`; 42 new tests, 262 green across the
three extractor suites).** BUILD_ORDER ⑥ is OPEN and IN PROGRESS — a
multi-session unit; this session measured the layout and built the
classifier + the answer-key emitter + the census; the next session builds
the document readers. Measurement record (codes only):
`D:\tax-test-data\1040\tmp\s327_lacerte_measurement.md`.

- **The font is the data.** Every value Lacerte prints is in
  `QuickTypeIICourierA` (depreciation schedules `QuickTypeIICondensed`).
  `lacerte1040/pagewords.py` flags data words by font, strips the
  vendor's trailing period on DATA words only, aligns rows by BASELINE.
  With that word model the TaxWise 1040 p1/p2, GA-500, GA Schedule 1 and
  RIE-worksheet parsers read four Lacerte packets to the entry lane's
  answer keys changing only label bands (`lacerte1040/layout.py`; the
  taxwise1040 parsers took the geometry as parameters, defaults unchanged).
- **The library is NOT blank.** Unfiled forms print pre-computed shadow
  values (1040-SR copy, 6251, 8960, 8962, 500X, 2210 + Sch AI …), so the
  packet's own "Forms needed for this return" page is the ONLY authority on
  what was filed. `classify.py` claims faces per JURISDICTION (Georgia's
  "Sch 3" ≠ federal), classifies Lacerte's supplements by their bold-14
  header title, counts shadows, and REFUSES on an unmapped token, a listed
  form with no page (the negative control, tested), or a non-Georgia state
  on the manifest. Manifest ∪ amount-bearing pages = 100% of the 1,578
  pages the entry lane read (57 payloads' `source.pdf_pages`); the manifest
  alone 99.3% — the misses are input-bearing SHADOWS (2210 = the prior-
  year-tax witness, Sch B below $1,500, Sch EIC, 4852), now named.
- **`emit.py` (leg 1):** coverage gate → 1040 faces (answer key, boxes,
  preparer, digital assets) → identity (p1 header rows, filing-status X,
  GA 500 p1 name boxes + DOB, surname split resolved by the GA box or a
  unique shell) → the 12b aged tripwire → dependents grid (Lacerte
  geometry, column 4 pinned on two witnesses) + Form 8867 → the GA answer
  key (face, Sch 1, RIE) → the INCOME GATE: any face income line refuses
  by name until the Federal Worksheets readers exist. So leg 1 emits
  nothing importable by design; its product is the wall list.
- **CLI:** `python -m lacerte1040 census <dir> --out <json>` and
  `python -m lacerte1040 extract <dir> --out-dir <dir> [--dob-index …]`
  (run from `scripts\`). Runs this session: the classify census
  (`1040\tmp\lacerte_classify_census_s327.{txt,json}`) and the leg-1
  extract over the whole Inbox (`1040\tmp\lacerte_extract_s327.txt`,
  `1040\PipelineOut\lacerte-s327\*.refused.json`). **255 packets: 0
  emitted (by design), 255 refused; 1 is not a Lacerte packet (code 10071,
  no manifest page).** Classify census: 254 list a 1040, 234 a GA 500,
  181 Sch 1, 172 Form 8995, 131 Sch D, 126 Sch B, 119 Sch 2, 113 Sch 3,
  110 Sch E p2, 109 8949, 95 1116, 84 4562, 79 8867, 76 Sch A, 75 Sch 1-A,
  75 Sch C, 65 8582, 62 7203 …; 45 packets carry a NON-GEORGIA state
  (NC 7, SC 7, NY 5, IL 4, CA 3, MA/CO/AR/OH/KY 2 …) and refuse on it.
  ⚠ Four over-counts in that run were fixed AFTER it started (the blank
  GA 4562's `$`-constants, 8879-TA listed-but-absent, the Underpayment
  Penalty Worksheet and e-file Payment Record titles) — a clean re-run
  was launched at close into `PipelineOut\lacerte-s327b` (log
  `tmp\lacerte_extract_s327b.txt`); read THAT one at the next boot.
- **The wall list (255 packets, packets carrying, `lacerte-s327`):**
  Federal Worksheets 254 · Schedule 1 181 · 8995 172 · Sch D 131 ·
  Federal Statements 129 · Sch B 126 · Sch 2 119 · Sch 3 113 · 8949 109 ·
  1116 95 · Georgia Statements 88 · 4562 84 · Sch A 76 · Schedule of Loss
  Limitations 75 · Sch 1-A 75 · Sch C 75 · Sch E 63 (+p2 75) · 8582 65 ·
  Federal Basis Limitation Worksheets 63 · 7203 62 · 1116 Sch B 56 · 8889
  44 · 7206 25 · 8962 24 · Vehicle/Unreimbursed Expenses 22 · 2441 21 ·
  4797 20 · 8829 20 · IND-CR 17 · listed 2210 16 · Sch F 14 · 8995-A 14 ·
  NOL Worksheets 13 · 8582 Worksheets 12 · 8863 10 · GA Sch 3 10 · 8283 9
  · 8880 9 · 5329 8 · GA Sch 2 5 · 8606 5 · 8990 5 · 4952 6. Median
  packet carries ~9 refusal classes — the Lacerte book is DEEP: after the
  worksheets, the faces the TaxWise parsers already read (Sch 1/2/3, B,
  A, D+8949, C, E) unlock most of it through band measurement, not new
  parsers.
- **DOB source built:** `1040\tools\lacerte_dob_index.py` → `Lacerte
  Inbox\lacerte-dob-index.json` (SSN-keyed; Ken's client list joined to
  the packet identity list: 258 of 258 SSN-bearing packets, 152 with a
  spouse DOB; the organizer scan (`tmp\organizer_scan_s327.json`, 379
  organizers, code-keyed, US dates) merged via `--organizer`: 206 packet
  records, DEPENDENT DOBs by SSN → 384 index rows). The 12b aged
  cross-check remains the guard on a wrong match.
- Tools: `1040\tools\lacerte_dump.py <CODE> <pages> [--data|--tmpl|--grep
  RX|--y A:B]` — the positional dump for these pages.

**▶ NEXT (leg 2 — the Federal Worksheets readers, the top wall in every
packet):** the section tables are all data-font with header rows, geometry
in the measurement record: **Wage Schedule** (Taxpayer/Spouse − Employer ·
Wages x1 296 · Fed W/H 365 · FICA 413 · Medicare 461 · State W/H 515 ·
Local 569; no EINs, no box 3/5/12 — D_EFILE_001 stays; the entry lane
keyed `social_security_wages`/`medicare_wages` from FICA÷6.2% and the
W-2 box-1 shape) → `w2s`; **Pension and Annuities Schedule** + **IRA
Distribution Schedule** (Payer · Total 363 · Taxable 432 · Fed W/H 500 ·
State W/H 564) → `r_1099s` (IRA flag from the schedule it sits in);
**Interest Income / Dividend Income** (payer x0 56 · amount x1 564; owner
on MFJ from the RIE worksheet columns, the s326 rule) → `int_1099s` /
`div_1099s`; **Social Security Benefits Worksheet (Pub 915)** line 1 →
`ssa_box5_net_benefits`, SSA withholding = face 25b − 1099-R withholding
(the entry lane's own derivation; tie 25d); the **State and Local Refund**
worksheet → `sr_filed_taxable_refund`. Then Schedule 1 / Sch B / Sch A /
Sch D+8949 / Sch C faces through the existing TaxWise parsers with Lacerte
bands (measure each band first — the 1040 pattern), then the first DRY-RUN
tie on HITTC/BITTLE-class packets, then the shadow-2210 reader.

**▶ KEN'S RULING, 2026-09-02 (s327 sitting) — A TIE IS A FILED RETURN, and
the Filed count is the practice's true count.** The DB audit that prompted
it: 920 federal 1040s showed Filed; the extractor's 165 dollar-verified
returns (and 160 of their GA-500s) sat in DRAFT because the in-process
commit never marked them; 38 more GA-500s under hand-filed federals were
never marked; the Individual tab counted SC/NC/AL state rows as returns
(932 vs 920). **Done:** backfill applied with the real →filed side effects
(federal 165 + GA-500 161; rollback ids in
`1040\tmp\s327_mark_filed_done.json`) — **federal 1040 Filed is now 1,085
of 2,978; GA-500 under filed federals 1,015 filed**; `commit_staged_return`
now marks filed on a tie (federal + attached states) so the sweep is a
no-op; the list and its tab counts are FEDERAL-only. **⛔ ENTRY LANE: 37
GA-500s under hand-filed federals were never tie-verified and stay Draft**
— clients 1019 1033 1034 1035 1056 1075 1076 1081 1089 1090 1094 1136 1164
1165 1215 1216 1218 1254 1259 1262 1273 1274 1281 1307 1315 1367 1372 1411
1587 1588 1600 1601 1609 1842 1857 1858 1891 — verify the GA face and
mark, or record why no GA return applies.

**▶ KEN'S CLIENT-BASE RULINGS, 2026-09-02 night (relayed to the CRM
verbatim; tax-app writes done):** #1751 dba "Construction Outsource";
#4725 Mighty Muffler Shop = June FYE (6/30 set), final return filed for
FYE 6/2025 → INACTIVE; MASH DYNAMO "not a client" (never existed in the
hub — nothing to retire); the three SOS suffixes yes; Peggy's 15
confirmed; the bonding-list individual still a client (record to be created — Ken to name the entity); fees Lucrative Leads $220 / Max
Merchandising $150 / Ground Effects $220, "McCoys is done". STILL OPEN:
Nashville Skyline (left blank), one bookkeeping client's monthly-vs-quarterly. Staffing =
D-041 (three slots; CRM migration staged for Ken).

**▶ NEW SUITE MODULE — delvio-research (Ken, 2026-09-02, planning only):**
its session asked for the linking keys and got them from the code (client_id
+ firm_id + client_number stable; entity_id re-pointable on a merge; year as
an int). Ken approved its plan and said "build those 2 doors" (2026-09-02
night) → BUILT: `GET /api/v1/suite/clients/?q=` (active, non-temporary,
name/number/entity-name/email match, `ein_present` booleans, no
identifiers), `GET /api/v1/suite/documents/?client_id=[&tax_year=]` and
`GET /api/v1/suite/documents/<id>/url/` (a link minted by this app's
storage; bucket keys stay here); `apps/suiteapi/research.py`, 9 tests.
**⛔ KEN: set `RESEARCH_SERVICE_TOKEN` on BOTH Render services** (tax app +
research) — no fallback, both doors are inert (503) until it exists.

**▶ ENTITY CLOSEOUT — two more gate defects fixed (`6f32bb56`, live):** the
gate compared the fresh verdict to the bare `tie`, so a return committed
`tie_with_exception` (source defects acknowledged, Ken's "file as is and
note the divergence") could NEVER close out — now `FILEABLE_VERDICTS`,
only unacknowledged misses named; and the closeout's reconcile omitted
`state_key`, so a 1065 closeout silently skipped its `expected.ga700`
lines (a tie that proves nothing) — now the entity's own key, as at
commit. ⚠ Two 1065s HAD closed out on the vacuous state-side re-check
(#4834, #4836, 2026-09-02 morning) — their GA-700 lines were verified at
COMMIT time (the lane's dry runs surfaced ga700 misses before the pins
were fixed), so the filed data stands; re-closeout on the fixed gate CONFIRMED
(26 ga700 rows each, all tie). Test added. **Landed by the entity
lane 2026-09-02:** #2927 and #4758 FILED through `accepted_errors`; the
seven Lacerte S-corps — #4775 (the EIN-bearing Rugged record), #1091, #1128, #3461,
#1202 TIE + FILED (the Feb/Jul hand entries had stale L14a, a
sign-flipped L25a, a TB-only A5/line-17 split — replaced, diffs in the
lane's reports); #3462 and #3790 `tie_with_exception` (Lacerte charges
distributions against a negative AAA, IRC 1368(e)(1)(A)) COMMITTED, re-
closeout DONE on `6f32bb56` → 7/7 FILED. Five engine findings → DEFERRAL_AUDIT
(9)–(13), incl. the GA-600S Sch 4 refund-line gap.

**▶ ENTITY CLOSEOUT — the error-acceptance path (s327, late):** Ken ruled
(relayed by the entity lane, verbatim-quoted): #2927 accepted by the IRS —
note the GA addback diagnostic and mark it filed; #4758 not required to
do a balance sheet and chose not to — accepted, mark filed and note it;
#3137/#4835 not yet filed, holds stand. The gate had NO path past an
error finding, so `run_entity_closeout` now takes `accepted_errors`
({rule_id: Ken's ruling}, requires `source_verified`); the error becomes a
DiagnosticAcknowledgment noted "ACCEPTED ERROR (Ken's ruling): …", a
rule id absent from the fresh run is reported `stale_accepted_errors`
and releases nothing. 4 tests. The lane runs the two closeouts once the
deploy is live (INT_GA_BONUS_ADDBACK on #2927, MATH_BALANCE_SHEET on
#4758 — the rule ids on their latest runs). The seven new Lacerte entity
packets in `1120S\Inbox` all resolve onto EXISTING drafts (none filed,
none imported): three Schedule-B-only shells, four hand-keyed never-tied
drafts — the lane was told to skip none and match on EIN.

**Second pass, same sitting (the s325 lesson applied — audit by the DATA,
never the staged row):** 133 more returns had been committed IN-PROCESS
with no staged bookkeeping, sat in Draft with documents, and re-reconciled
LIVE against their payload answer keys 133/133 TIE → marked filed
(`tmp\s327_mark_filed_plan2/done2.json`). **Federal 1040 Filed is now
1,218 of 2,978; GA-500 under filed federals 1,147 filed / 37 draft (the
entry-lane list above).** Also found: **58 emitted payloads never
committed** (`PipelineOut\r51` 31 · gail-s325 10 · georgianna-g2/g4 10 ·
shellfix-rerun 4 · 3 singles) — the standing commit authorization covers
them: DRY-RUN then commit at the next boot. 8 returns are filed with zero
document rows (clients 1400 1106 1200 1974 4036 3878 1598 3264) — probably
SSA-only/hand-keyed; verify one before assuming.

**The practice's true 2025 import count (DB, 2026-09-02 end of sitting):**
federal 1040s Filed 1,218 (all tie-verified); 1120-S Filed 175 (+170
GA-600S); 1065 Filed 5. **The remaining ~1,760 individual shells:** ~190
have no packet in hand at all; the TaxWise Inbox holds 1,609 packets — 656
refused by the extractor (381 of them on ONE class; walls by count:
unknown pages 142 · asset_detail 71 · sch_e 43 · f5329 42 · f6251 38 ·
f4562 32 · state_nonconformity_wks 28 · f8582 27 · sched_line_detail 27 ·
26 not-a-packet · ctc_ext_carryover_wks 23 · pension/interest/IRA
decomposition 22/17/14 …), 506 never run through the pipeline (the newest
subfolders + 224 HOLD-marked), 58 emitted awaiting commit; the Lacerte Inbox
holds 255 (207 to import). None of this is "one by one": each cleared
class lands its packets as a batch; the data-entry sessions hand-key only
the residue.

**▶ ENTITY LANE RECONCILED TO KEN'S LACERTE "ACCEPTED" LISTS (2026-09-02,
s327 sitting; lists parsed to `D:\tax-test-data\tmp_lacerte_accepted_lists_
2026-09-02.json`, matched by EIN):**
- **1120-S: 186 accepted → 175 filed in the app (173 tie-verified + 2
  hand-filed), 11 not:** 3 packets on Hold for non-GA states/K-2 (packet
  codes ABE = SC, 187 = CA, 101 = NC); **packet 219 had been moved to Done
  with a DRAFT shell, no import row, no GA-600S — re-queued to the Inbox
  with a HOLD note**; 6 have NO packet on disk (the six "shell draft" rows
  with an empty packet list in the audit — names in the JSON) → Ken prints
  them from Lacerte; one is a DUPLICATE client pair (#4775 carries the EIN
  and no 2025 return; #3855 carries the 2025 draft shell and no EIN) →
  Ken's merge call, then a packet.
- **1065: 69 accepted → 2 filed, 67 draft — and 66 of the 67 have their
  Lacerte packet sitting in `1065\Inbox` (96 packets there).** The
  partnership import IS the whole book; nothing is missing from disk.
- **The 7-file TaxWise drop in `1120S\Inbox` (Ken, 2026-09-02):** 4 are
  S-corps with 2025 shells (files …9341, …0198, …9913, and the "2025
  Source Docs" scan — ⚠ that one is source documents, not a return print);
  3 have NO client/entity in the app at all (files …2637, …8078, …1075 —
  the last is a 1065). TaxWise entity prints are NOT readable by the
  Lacerte-based entity lane; **hand entry through the import channel (so
  they tie and mark filed) is the route — Ken offered the data-entry
  account; seed the 3 clients first.**

**▶ THREE CLIENT-RECORD RULINGS, BUILT THE SAME SITTING (Ken, 2026-09-02;
DECISIONS "Three client-record rulings"):** ① **an SSN or EIN is required
to create a client** — `ClientSerializer.validate` refuses a creation with
neither (the desk's temporary-capture path is the one exemption, via
serializer context); the New Client form labels the required key by entity
type and refuses before posting; 91 client tests re-keyed with synthetic
identities + `tests/test_client_rules_s327.py`. ② **fiscal year-ends on
the entity** — `Entity.fiscal_year_end_month/_day` (migration
`clients.0015`, NULL = calendar year), `is_fiscal_year`,
`fiscal_year_end_for(year)` (a fiscal year is labelled by the calendar
year it BEGINS in — Form 1120 instructions "Period Covered"), both entity
serializers, the Slate entity-info screen ("Fiscal year-end (MM/DD)") and
the editor's save payload. **Not yet built: the due-date calendar that
consumes it** (BUILD_ORDER queue). ③ **the duplicate S-corp client**: the
2025 shell moved to #4775 (the EIN record), #3855 set inactive
(`1120S\tmp_rugged_merge_s327.json`); Ken is printing its packet.

**▶ ENTITY LANE (other account) IN FLIGHT on the six TaxWise entity packets
(Ken's assignment, 2026-09-02):** it asked for a one-writer check + engine
answers (given: F14 is engine-written from `sched_f` rows; AMT overrides
are honored, K17a = Σ(regular − §179) − (AMT − §179)); it could not create
clients, so **this lane seeded the three partnership shells through
`build_federal_return`: #4834 (…2637), #4835 (…8078), #4836 (…1075)**
(`1065\tmp_seed_taxwise_partnerships_s327.json`); the three S-corp shells'
"41 entity rows" are the `OTHER_DEDUCTION_PRESETS` skeleton (safe to
`replace_documents`; #3137 also carries a Feb-2026 partial hand entry).
**Landings AUDITED by the data (2026-09-02 evening):** #4834 and #4836 (1065) FILED + GA-700 filed, tie; #2927 and #4758 (1120-S) committed with verdict TIE but DRAFT — the entity closeout gate refused them on error-severity diagnostics (#2927 INT_GA_BONUS_ADDBACK: the filed 600S carries no GA add-back for 1,250 of federal bonus; #4758 MATH_BALANCE_SHEET BOY: assets 0 vs L+E −767, BOY paid-in unknown) — **⛔ KEN: source defect or rule over-fire? left Draft on purpose, the tie rule does not override a red diagnostic**; #3137 (1120-S) and #4835 (1065) held NO_TIE (§1245 recapture the filed 4797 reported as §1231 + a Schedule L that does not roll; K14a $1 rounding + a Schedule L with no liabilities) — **⛔ KEN**. Record: `1120S\ENTITY-PROGRESS-2026-09-02.md`. Two more lane findings queued: M1_1 / M2_3 are INPUTS (pinning them without keying reads 0); the entity payload has no address keys while closeout treats MISSING_ADDRESS as error-severity (fix = address keys in `backentry-entity.v1`, read off the packet face). Lane findings
for the build queue: `source.vendor` is absent from `backentry-entity.v1`
(staging refuses what the local validator passes — the validator reads the
1040 schema); the 1065 `expected` set lacks `K14b`; 1120-S line 12 / K16c /
L10c-d are engine-computed.

**▶ DISREGARDED ENTITY TYPES BUILT (Ken's go, 2026-09-02, s327):**
`disregarded_c` / `_f` / `_e` on `EntityType` (migration `clients.0016`);
non-filing by name (`NON_FILING_ENTITY_TYPES` names the schedule); EIN
required at creation and the owner must be an individual
(`EntityCreateSerializer.validate`); never a new client's primary type
(`ClientSerializer.validate`); UI: the add-entity pickers on Client Detail
and Entity Detail (Client Detail's form gains a required EIN box for these
types), label maps in the palette / Start Return / folders / returns
pages, non-filing notes. Not built: a "reports on" link from the entity to
the 1040 schedule row — the EIN on `ScheduleC.ein` / `ScheduleF.ein` is the
join today; a display of the owner's schedule from the entity page is a
small follow-up if Ken wants it. **Searchable by EIN (Ken's ask):** the
palette and the clients-page search read a nine-digit or dashed term as an
EIN and find the client carrying it on any entity (`_ein_search_q`); a
short digits-only term still means client number. A plain Schedule C with
an EIN and no LLC uses the same `disregarded_c` type (Ken: fine).
**Cross-app fold with delvio-crm (in flight):** three of the CRM's
standalone business clients now have disregarded entities under their
owners (#4806→#3638, #4801→#2120, #4803→#1267;
`tax-test-data\tmp_disregarded_fold_s327.json`); the CRM re-points its
engagement rows (owner `client_id` + `entity_id`), then THIS lane sets the
standalone clients inactive. Ken (via the CRM lane, verbatim-quoted):
Blazers is a Schedule C under #4060, the business is the SPOUSE's → folded
(#4786 → entity under #4060, disregarded_c); Eve's Garden got its EIN
from Ken (via the CRM lane) → folded (#4808 → entity under #2963,
disregarded_c; taxpayer/spouse owner UNCONFIRMED — Ken named the spouse,
default spouse when the owner field ships). Retirement pass DONE 2026-09-02
(Ken's go via the CRM lane; its migration landed, five engagements
re-pointed first): #4806 #4801 #4803 #4786 #4808 → status inactive, EINs
left on the retired rows as history. **The CRM's re-point migration
(client_profile UNIQUE(client_id) → partial indexes) is HELD FOR KEN by the
CRM lane; no standalone client retires until it lands.** Follow-up queued:
an `owner` (taxpayer/spouse) field on disregarded entities to pre-fill the
Schedule C proprietor.

**s326 carried (all verified):** the 19-hold triage landed 16 (`9e4cbc0d`);
the §6654 family is Ken's; the 1040 landed corpus is +16 (Gail +12, Jenny
+4). **s325 carried:** 138 landed from the four re-extracted books; the
3-char `r_1099s.distribution_codes` model gap holds 4 packets + …4450
(`link_key` naming) — 5 staging errors.

⚠⚠ **ANOTHER CC ACCOUNT WRITES THIS LANE — THE SHARED FILES ARE THE ONLY
PLACE ITS WORK IS VISIBLE.** Do not order events by `updated_at` (real
clock skew across machines). The entry lane (tax-test-data-7d) is idle and
released; it offered to witness-read the first emitted Lacerte packets —
take it at the first dry-run tie.

**▶ BUILD QUEUE after the Lacerte legs:** ② the 3-char `distribution_codes`
model gap (migration; 4 packets since s324) · ③ the TaxWise extractor
walls by measured count (f6251 = 13 · sched_line_detail = 6+ · f5329 · the
classifier patch, GA part-year detector first) · ④ the three re-raised
Lacerte engine holds (clients 1922, 2386, 3517: per-property nonpassive
lever · GA nonresident NR-46 + itemizer credit · §172 absorption) · ⑤ the
GA 7b military-exclusion engine leg (7 witnesses; waits on the states-lane
spec export) and the 7c/7f DIS transcription · ⑦ the §6654 family decision
(Ken) · ⑧ BATCH-013 (posted, UNWORKED, 10 Tom-lane product gaps; ⚠ its item
5's premise is refuted — Schedule C has no business_address on the MODEL, a
migration not a sync) · carried: the 8615 parent-first guard ·
out_of_scope_states · the zero-activity GA-attach gap.

**⛔ WAITING ON KEN:** may the Lacerte pipeline file a packet whose manifest
carries a non-Georgia state return (Colorado, Arkansas, Massachusetts …)
federal + GA only, as the entry lane did with `out_of_scope_states`? (leg 1
refuses them — states on hold) · the §6654 family (…0500/…0534/…7701/…7044)
· seed ONE client (the …4641 taxpayer in Jenny's book; ⚠ do NOT edit
#4054) · client #3572's contaminated name (#4514 same shape) · three
clients with no 2025 1040 shell (two report as 1120-S filers) · does the
standing commit authorization extend to ENTRY-LANE HAND-KEYED commits?
(client 3250 staged, dry-run TIE) · client-2149's filed GA 17a = 2
exemptions on a single/zero-dependent return (IT-511) · the 61 Gail
federal reprints (`tmp\GAIL-TRIAGE-2026-08-31.md`) · the …4203 W-code
question · one Georgianna reprint + five named reprints · the asset
METHOD-DERIVATION TABLE review (annex) · vendor-name allowlist for the
mirror guard? · carried: 1071 · 1141 · R-GA500-RIE · 4059 W-2G address ·
Sch D carryover · GA RIE L10 · 4081's $169 · standing 1–8 · 2a scope flag ·
AL 40 · the 4 Tom-book holds (…8505 · …2276 · …2827 · …8791).

**▶ OFF-SPINE, SHIPPED 2026-09-01 (Bob lane):** `GET
/api/v1/suite/clients/by-phone/` (`apps/suiteapi/caller.py`, 8 tests) —
caller-ID lookup; `Client` has no phone column (business = `Entity.phone`,
individuals = `Taxpayer.daytime_phone`); contract in the bob repo.
