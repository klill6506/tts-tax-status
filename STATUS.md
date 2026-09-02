# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s326, 2026-09-02 early morning

**▶ THE 19-HOLD TRIAGE IS DONE: six families, five fixed, 16 of 19 LANDED
(`9e4cbc0d`, deploy dep-dabolebbc2fs73eofgeg live).** Every hold was re-run
in a rolled-back dry run printing the engine's GA RIE worksheet rows against
the packets' printed worksheets (parsed positionally in one pass); the deltas
were tabulated BEFORE any hold was read. Names and per-hold detail:
`D:\tax-test-data\1040\tmp\s326_holds_triage.md`.
- **A — 11 of the 13 GA holds were ONE defect:** the s297 payer-less
  consolidated INTEREST/DIVIDENDS/cap-gain rows reached the engine as
  `owner: joint` (the s324 below-$1,500 rule) and split 50/50 — Δ = exactly
  half the face amount, odd dollar to the spouse, every time — while the
  filed RIE worksheet had attributed them to one spouse all along.
  `ga500.parse_ga_rie_worksheet` now reads row 9; `emit.attribute_valve_by_rie`
  holds the rules (rows sum to the face → as printed; short by exactly one
  NON-qualifying spouse's blank column → the remainder is theirs — **a blank
  column means "none" OR "under 62, column omitted"**; anything else refuses;
  neither qualifies → the joint row stands). Mixed splits become two labeled
  rows; the distributions-only 7a aggregate rides the owner-tagged DIVIDENDS
  row as box 2a; MFJ 8949 rows take the whole-column owner. **12/12 tie.**
- **C — MFS §86 base (…5428):** `mfs_lived_with_spouse` defaults False =
  apart = the $25,000 base; the filed return taxed 85%. 1040 line 6d is the
  witness (`parse_p1_mfs_lived_apart`, one corpus X at x≈469); keyed on any
  MFS return carrying benefits. Tie.
- **D — the EIC granted to two ITIN filers (…9146 9xx number; …0001 a
  Form W-7 in the packet):** 603 / 649, the table amounts. `eic_valid_ssn_*`
  were NOT importable (assume-everyone default) — allowlisted, schema
  regenerated; new classify key `f_w7` (ignore role, its PRESENCE is the
  input — it had passed the coverage gate unseen). Both tie.
- **E — the first asset-detail packet against a filed return (…0680):** the
  engine computed the asset EXACTLY (808 = 808), but Sch C line 28 was 808
  short — the emitter keyed only `depreciation_filed`, and **"the filed total
  wins" is implemented as a SKIP of the register write with nothing else
  filling the line → line 13 = 0, every diagnostic green.** Emitter keys
  `depreciation` too; staging WARNS on the filed-only shape (Sch C/F/rental).
  Tie.
- **F — GA MILITARY retirement exclusion, Schedule 1 line 7b (…5816, age 58,
  DFAS):** the wrapped 7b label pushes its value onto the 7c label's row, so
  35,000 read as a DISABILITY exclusion and pinned a RIE-TP-17 the filed return
  never used. The MILITARY word on the value row discriminates; **7b/7e refuse
  by name (no engine leg — the 7th witness) and 7c/7f refuse (the RIE-DIS
  assertion is not transcribed).**
- **B — §6654 penalty, 3 holds (…0500 315 vs 20, …0534 55 vs 74, +
  carried …7701/…7044):** KEN'S — vendor divergence, never codified. ⚠ …0500's
  packet prints NO Form 2210 (the invoice lists it) — undecomposable further.

**▶ LANDING LEDGER (`commit_s326_holds.py`, batch `s326-holds-commit-001`,
per-return transactions, staged-row bookkeeping written; log
`1040/tmp/commit_s326_holds.txt`):** 16 payloads — the 12 attribution
re-extracts (`PipelineOut\s326-rie-refix`) + …5428, …0680, …9146,
…0001 (`PipelineOut\s326-refix2`). **16/16 LANDED — verified against the
DB after the run: 16 staged rows `status=committed`, 16 `resolved_return`
set, every `commit_result.reconciliation.verdict == "tie"`, zero
idle-in-transaction orphans.** The 1040 landed corpus is +16 (Gail +12,
Jenny +4).

**▶▶ THE LACERTE BOOK IS A GO — "Import them" (Ken, 2026-09-02, RELAYED
verbatim by the entry lane tax-test-data-7d; recorded in DECISIONS.md as a
relayed ruling).** BUILD_ORDER ⑥'s condition is met; the leg is this lane's
and is the NEXT unit. **Depth-probe census done this session (read-only,
`1040/tmp/lacerte_census_s326.py` / `_census2_`; JSON beside them):**
- **255 Partial Return packets** (the reconcile file says 260 by page-index
  count — 5 to explain), **median 580 pages each (min 8, max 882), 147,643
  pages total**. The Lacerte "Partial Return" export prints EVERY form
  shell in the program — 254 packets carry Form 982, FinCEN 114, Schedules
  H/J/LEP/P, 1040-X, 8938, 8865 schedules … BLANK. **The book is ~95% blank
  form shells; the classifier's first job is blank-vs-filled, per page.**
- ⚠⚠ **The TaxWise currency-token census is BLIND here:** pass 2 called a
  median 563 of 580 pages "value-bearing" because blank Lacerte forms print
  digit runs (line numbers, years, OMB numbers). The candidate discriminator
  is Lacerte's own AMOUNT signature — `<label> <amount>.` at END of line with
  a TRAILING PERIOD (`tools/lacerte_face.py` TAIL regex) — UNMEASURED; the
  first task of the leg is to measure it against the 48 known-filed packets.
- **Organizers: 204 of 255 packets pair by code** (`Organizer Forms-<CODE>`);
  175 organizers have no packet (other names/codes — check). The packets
  carry NO dates of birth; the organizer is the second source per packet
  (`tools/organizer_extract.py` already reads TP/SP/dependent DOB, matched on
  SSN); GA Form 500 page 1 also prints TP/SP DOB (`08081954`).
- Handoff artifacts (entry lane): `tmp\lacerte-reconcile.json` (code →
  client_number + status; ~12.5% false-negative match rule; treat the 44
  "unmatched" as unresolved), `tmp\lacerte-packets.json`, the per-packet
  `tmp\L0NN-<client>\build.py` conventions, `Lacerte Inbox\LACERTE-PROGRESS.md`
  (48 filed — do NOT re-import; the fence will refuse), `SOURCE-QUESTIONS-FOR-
  KEN.md` (#8 recurs). **1922 / 2386 / 3517 stay OUT of the pass by number.**
- Leg requirements (BUILD_ORDER ⑥, verbatim): coverage gate from day one;
  reconcile the page map against the page COUNT and dump every unclaimed
  page; a negative-control fixture (known form removed → must NOT tie).
- Lacerte layout hazards on record: amounts 1–3pt ABOVE the label baseline;
  dot leaders swallow x-positions if `.@x` tokens are stripped; checkbox X
  ~9–13pt LEFT of its inline label; Yes/No/N/A grids align X under the
  column; the s290 form-face indexer silently omitted whole forms (2210 on
  1071, GA Sch 3 on 2455). The entry lane offered to witness-read the first
  emitted packets — take it.

**Run lessons (s326):** ~4 min per return with diagnostics on the shared
DB; **use a DISTINCT `batch_key` per concurrent dry-run process** (two
`get_or_create`s of one key inside open transactions serialize on the unique
index — the s325 lock shape by construction). `sed` with a backslash-heavy
Windows path silently produced an empty script and the chained launch ran a
missing file — Write tool + `grep -n` before launching. Killed the
confirmation-only holds run at 14/19; `pg_stat_activity` swept clean.

⚠⚠ **ANOTHER CC ACCOUNT WRITES THIS LANE — THE SHARED FILES ARE THE ONLY
PLACE ITS WORK IS VISIBLE.** Do not order events by `updated_at` (real
clock skew across machines). The entry lane (tax-test-data-7d) is idle and
released; it asked the four Lacerte questions this session and got its
answers from the planner (all four items ARE in BUILD_ORDER/STATUS).

**s325 carried (all verified by the data):** 138 landed from the four re-
extracted books; Gail 122 verified TIE; the corpus is complete (~4,560
PDFs); the SSN-coverage finding (imports add ~zero — 1,704 individuals need
a SOURCE decision); the 3-char `r_1099s.distribution_codes` model gap holds
4 packets (…8546 · …4344 · …2981 · …8307)
+ …4450 (depreciation `link_key` naming) — 5 staging errors.

**▶ NEXT:** ① **THE LACERTE-LAYOUT EXTRACTOR PASS** (GO — above; start by
MEASURING the blank-vs-filled discriminator on the 48 known-filed packets,
then classify → positional readers → emit → the staging/commit chain; a
multi-session unit) · ② **the
3-char `distribution_codes` model gap** (migration; blocks 4 packets, carried
since s324) · ③ the next extractor walls by measured count: f6251 = 13 ·
sched_line_detail = 6+ · f5329 · the classifier patch (GA PART-YEAR DETECTOR
FIRST) · ④ the three re-raised Lacerte engine holds (clients 1922, 2386,
3517: per-property nonpassive lever · GA nonresident NR-46 + itemizer credit ·
§172 absorption) · ⑤ the GA 7b military-exclusion engine leg (7 witnesses
now; waits on the states-lane spec export) and the 7c/7f DIS transcription
(no real witness yet) · ⑥ CONDITIONAL on Ken's Lacerte answer · ⑦ the
§6654 family decision (Ken) · ⑧ BATCH-013 (posted, UNWORKED, 10 Tom-lane
product gaps; ⚠ its item 5's premise is refuted — Schedule C has no
business_address on the MODEL, a migration not a sync) · carried: the 8615
parent-first guard · out_of_scope_states · the zero-activity GA-attach gap.

**⛔ WAITING ON KEN:** the §6654 family
(…0500/…0534/…7701/…7044 — engine statutory vs TaxWise; …0500 has no
printed 2210) · seed ONE client (the …4641 taxpayer in Jenny's book; ⚠ do NOT
edit #4054) · client #3572's contaminated name (#4514 same shape) · three
clients with no 2025 1040 shell (two report as 1120-S filers) · does the
standing commit authorization extend to ENTRY-LANE HAND-KEYED commits?
(client 3250 staged, dry-run TIE) · client-2149's filed GA 17a = 2 exemptions
on a single/zero-dependent return (IT-511) · the 61 Gail federal reprints
(`tmp\GAIL-TRIAGE-2026-08-31.md`) · the …4203 W-code question · one
Georgianna reprint + five named reprints · the asset METHOD-DERIVATION TABLE
review (annex) · vendor-name allowlist for the mirror guard? · carried: 1071 ·
1141 · R-GA500-RIE · 4059 W-2G address · Sch D carryover · GA RIE L10 · 4081's
$169 · standing 1–8 · 2a scope flag · AL 40 · the 4 Tom-book holds (…8505 ·
…2276 · …2827 · …8791).

**▶ OFF-SPINE, SHIPPED 2026-09-01 (Bob lane):** `GET
/api/v1/suite/clients/by-phone/` (`apps/suiteapi/caller.py`, 8 tests) —
caller-ID lookup; `Client` has no phone column (business = `Entity.phone`,
individuals = `Taxpayer.daytime_phone`); contract in the bob repo.
