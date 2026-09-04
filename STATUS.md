# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s330 close, 2026-09-03 night

**▶▶ START HERE: KEN CLEARED THE BACK DOOR. Five rulings landed at the close and
the next session's job is to SPEND them — build the unblocks, then run the books.**
All five are in DECISIONS under *"Five entry-lane rulings clearing the back door"*.
Nothing below needs re-asking.

| # | Ken's ruling | What it releases | Work |
|---|---|---|---|
| 1 | A Lacerte packet with a NON-GEORGIA state files **federal + GA only** | **47 packets** | drop the refusal; set the other state aside |
| 2 | §6654 penalty deltas: **record and file** | ~10 packets | accept the delta as a vendor divergence, note it |
| 3 | A **hand-keyed** return that ties **IS filed** | client 3250 + the entry lane | extends the standing commit authorization |
| 4 | **Ignore** the doubled Sch 1-A / Sch 3 pages in code | **78 packets** (62 + 16) | reader tolerates 4 pages where it wants 2, and 2 where it wants 1 |
| 5 | Gail needs **NO reprint** | — | the old "print 61 federals" ask is RETRACTED |

**⚠ Ruling 1 carries a ROADMAP ask that is NOT a licence:** Ken wants state
software built soon (*"someone needs to work on the app itself at some point"*).
That is a scope conversation — which state, what "the app" means — tracked in
BUILD_ORDER. Do NOT start entering other states' returns on the back of ruling 1.

**▶ FRESH CENSUS AT THE CLOSE (a count is a timestamp — re-run it, never quote
this one later):** 2025 federal 1040 shells **2,978 — filed 1,258**, draft 1,716,
in progress 4. GA-500 under a filed federal: 1,224 — 1,187 filed, 37 draft.
**1120-S 327 shells / 193 filed. 1065 104 shells / 9 filed.**

**⚠⚠ THE PARTNERSHIP SIDE IS THE THIN ONE, NOT THE S-CORPS.** Ken believed both
were solid; the data says 9 of 104. The 1065 import has never been built and 66
packets sit in `1065\Inbox`. That is a BUILD item, not a Ken blocker.

**▶ WHERE THE 1,716 UNENTERED 1040s ACTUALLY SIT.** The back door has ever
*reached* only **75** of them (a StagedReturn resolves to the shell; 126 valid /
33 invalid / 4 excluded / 1 committed rows). The other **1,641 have never had a
staged row at all** — either their packet refused before staging, or no packet
exists in any inbox. **1,218 packets sit refused as of their latest run**, so the
ceiling is PACKETS AND READERS, not the commit path.

**▶ THE REFUSAL CENSUS (one count per packet, latest run only — re-measure before
building, the s295/6/7 rule).** Lacerte, 252 of 255 packets refuse: Sch 1 **181** ·
8995 **172** · Sch D **131** · Sch B **126** · Sch 2 **119** · Sch 3 **113** ·
8949 **109** · 1116 **95** · 4562 **84** · Sch A **76** · Sch C **75** · Sch E p2
**75** · GA 4562 **72** · 8582 **65** · 7203 **62**. TaxWise books, 963 packets:
unclassifiable page **148** · asset detail **72** · no federal face **66** · the
doubled Sch 1-A **62** (ruling 4) · Sch E **46** · 5329 **42** · 6251 **41**.

**▶ NEXT — Ken's sequencing at the close (2026-09-03 night): "Can you work on batch 013 immediately after the refresh?"**
**0. ⛔ FIRST: `D:\tax-test-data\1040\CC Changes\CC_CODE_CHANGES_1040_BATCH-013.md`** — Codex's ten
   Tom-lane product gaps, posted 2026-08-31, UNWORKED. The CC Changes loop applies verbatim
   (CLAUDE.md): verify-first triage of every item against the code and the fixture, ONE deploy
   for the batch, a result annex appended to the file, then move it to `CC Changes Done\`.
   ⚠ Item 5's premise is REFUTED (the published schema and the production allowlist agree;
   Schedule C has no `business_address` on the MODEL — a migration, not a sync). Item 1 is
   the positive-net Schedule D RIE spouse allocation (BATCH-296 item 57 fixed only the
   negative case). Item 2 needs a source-backed 1040 line 1h row. Item 3 a printed Form 5329
   line 48 route. BATCH-296 (the running file, items 54–59 already triaged by the data) stays
   open beside it; do not merge 013's items into 296.
**0b. ⛔ ALSO FIRST — two PRODUCT DEFECTS the entry lane surfaced from Gail's 10 holds (2026-09-03
   night, tax-test-data lane; verify-first, same loop as 013):**
   - **`schedule_cs[].depreciation_filed` SUPPRESSES the Schedule C depreciation deduction.** On
     #2454, four dry-run arms: with the key present the net profit carries NO depreciation at all
     (neither the filed 808 nor the register's — an absurd-basis arm left line 11 UNMOVED at
     65,052); removing the key TIES at 64,301. `D_4562_RECON` still REPORTS the 808, so the face
     reads right while profit is overstated. **CENSUS ASK 1: how many already-FILED 1040s keyed
     `depreciation_filed`?** If the extractor keys it routinely this is overstating AGI across
     the book. Find the WRITER of Schedule C line 13 and the branch the key takes.
   - **The GA low-income-credit gate (`LIC-NODEP`) is missing on already-FILED returns.** #2127,
     #3158, #3569 needed nothing but `LIC-NODEP` (+`LIC-65=1` for the 70-year-old); each filed
     GA-500 prints the credit on line 17 ($8 / $5 / 2×$5) and the filed record lacks it. **CENSUS
     ASK 2: filed 1040s with federal AGI < $20,000, GA tax > 0, and no `LIC-NODEP`** — outside
     Gail there is no second print to catch them.
   - **Reader fix:** the extractor emits 1099-R box 7 as `"4 D"` / `"7 D"` — three chars against a
     `max_length=2` field; the PDF prints `4D`/`7D` as ONE token, the space is the reader's. Strip
     it at the source (it recurs on every two-letter box 7). This IS build-queue item ② (the
     3-char `distribution_codes` gap) seen from the other side — one fix, not two.
   - ⚠ Scope fact: six of Gail's ten "held" targets were ALREADY entered and filed — on that book
     "held" ≠ "not entered"; the document count answers it. Five of the seven need `-Merge
     replace_documents` against FILED returns — on Ken's list in the entry lane, not this one.
   - **CLOSED by the entry lane the same night (verified here against the DB before recording):**
     Ken answered it directly. Nine of Gail's ten holds are LANDED (`status=filed`, a committed
     staged row each): #2793 #3010 #3160 (box-7 fix), #2127 #3158 #3569 (LIC gate, merged with
     `replace_documents` on Ken's word), #4133 (MFS lived-with-spouse 85% SS), #2454
     (`depreciation_filed` REMOVED so the register drives line 13 — the defect above is real and
     this is its workaround, not its fix), #1219 (committed `tie_with_exception`, source defects on
     federal 37/38 citing 6654(d)(1)(B)(ii) and (d)(1)(C)(i) — **ruling 2 confirmed to that lane
     by Ken directly**). **Only #2019 is open**, and on the GEORGIA gate, not the lane: Ken said
     "enter $17,500 and hold"; it is staged (`valid`, uncommitted) with `MIL-TP-U62`/`MIL-TP-1`/
     `MIL-TP-6=13620`, no_tie by exactly $908 of GA tax, federal untouched; hold note beside the
     packet in `1040\Inbox\Gail`. Do NOT re-ask any of this.
   - **Reader item from #2019:** the extractor put the filed $35,000 on the answer key as
     `RIE-TP-17` (the ordinary retirement exclusion) when it was claimed on Schedule 1 line **7b**
     (military). The answer-key line is MIS-MAPPED for every under-62 military retiree regardless
     of the gate question — queue with the reader items, not to be rediscovered.
   - **⚠ For the two censuses:** `mark-filed` on the entry lane's batches returns `filed 0 /
     skipped N, reason "already filed"` — a NO-OP, not a failure; the commit lands the status
     itself on that lane. Any census that reads `mark-filed` output as evidence of what was
     written reads wrong. Count from the DATA (the s325 rule).
**Then, in the order that lands the most returns:**
1. **Ruling 4's duplicate-page tolerance** (78 packets, an afternoon).
2. **Ruling 1's out-of-state release** (47 packets, a refusal to drop).
3. **Ruling 2's penalty acceptance** (~10 packets) + **ruling 3** (client 3250).
4. **The Lacerte face readers by wall count** — Sch 1 → 8995 → Sch D → Sch B →
   Sch 2/3 → 8949 → Sch A → Sch C. Drive ONE single-wall packet by hand through
   each before trusting the census (s328's lesson: a corpus run never reaches a
   new reader).
5. **⑥c `manage.py merge_client`** (D-044; the CRM's 34 duplicate pairs are the input).
6. **The 1065 import** — 95 partnership returns behind it.

**▶ CODEX / the entry lane may resume one-by-one entry immediately.** Nothing in
this lane blocks it, and ruling 3 is what it was waiting on. ⚠ If it reports being
blocked, the known cause is a **fresh production session that only Ken can mint**.

**🏁 SHIPPED THIS SESSION (all deployed, Render-API verified live):**
- **⑥b the four Clients-as-hub doors** (`a3cedfd4`) — create · status · latest
  returns · attach document. One shared creation path (`apps/clients/service.py`)
  that the in-app form runs too, so they cannot drift. `CRM_SERVICE_TOKEN` is SET
  by Ken at both ends and verified from outside.
- **A live 500 on all four doors, found and fixed** (`50ba3b20`): `hmac.compare_digest`
  RAISES on a non-ASCII string, so one odd character in a pasted token crashed every
  door. Compare BYTES. Regression tests both directions.
- **The v2.3.1 app-header tokens adopted** (`de7b1eb1`) — the bar no longer hardcodes
  white-alpha. Verified in the SERVED stylesheet, not just deployed.
- **delvio-1099's Slate v2.3 merged on Ken's explicit go** (`fb536dd`, D-027).
- **Six pre-existing stale tests fixed** (`c27e0197`, `7a1766c4`, `d6d2c30b`), every
  one proven red at `070f0c87` BEFORE being touched.
- **GLOBAL_STATUS full review** — first since 2026-07-21.

**⚠⚠ THE EVENING'S OWN LESSON, SIX TIMES: reasoning from a MODEL of the thing
instead of OPENING the thing** — a gate with hexes typed into it · a hand-typed
accent list that dropped two apps · float maths where the browser rounds to 8-bit ·
a per-app deploy status read off a per-colour table · an app judged by a FOLDER NAME ·
a clone 37 commits stale reported as current · and the Gail triage reading FILENAMES
instead of the PDFs. **One of these reached Ken as a false alarm on the IRS filing
app.** Before stating what something contains: `git fetch`, then open the artefact
the running system actually consumes.

**▶ RELAYED BY THE tax-test-data LANE, 2026-09-03 night (recorded here, NOT this
lane's work):** ① **Nilkanth Diamonds #3413 is FILED**, verdict `tie_with_exception`,
with Ken's M-2 ruling on the return as a SOURCE defect: the transmitted return carries
AAA at **-253,783**; the correct figure is **-233,602**. ⚠⚠ **THE 2026 RETURN MUST OPEN
AT -233,602, NOT AT THE PAPER** — a proforma that matches the filed original would carry
the error forward; whoever rolls 2026 needs this. ② **RAPTAP HOLDINGS LLC #3728** (1065,
`in_progress`) was entered by Codex under the same review process — on the radar only,
unreviewed by either lane, and it is a 1065, where the entity lane's own leg has history.

**▶ NEXT:** **⑥c `manage.py merge_client`** — D-044's duplicate merge + hard
delete. Already ruled and half-specified in DECISIONS: survivor = the EIN/SSN
holder; the command REFUSES to delete any client an uncommitted StagedReturn or
a PipelineOut payload resolves to; #4000–#4002 are report-to-Ken, never merged
by the tool; every FK in every suite schema re-pointed first, a zero-reference
sweep, dry by default, a reviewable plan Ken approves before any delete. First
input = the CRM's 37 SSN-found pairs. Then the Lacerte leg resumes: ③ the face
readers by wall count (Sch 1 → Sch B → Sch 2/3 → Sch D+8949 → Sch A → Sch C;
8995 via `taxwise1040/f8995.py`) — drive ONE single-wall packet per face before
trusting the census · ② the 3-char `distribution_codes` model gap (8 packets) ·
④ the shadow-2210 reader (face line 38) · ⑤ the GA 7b unborn-dependent engine
leg (8 witnesses).

**▶ CARRIED FROM s329 (2026-09-02 night) — THE SECOND PII HISTORY REWRITE IS
EXECUTED at narrow scope.** `main` was force-pushed once from a filtered mirror
clone: `8f87ed1b` → **`75de3a2d`**, 2,720 commits, 902 SHAs changed. All four
gates passed BEFORE the push: V1 tip TREE hash identical · V2 commit count
unchanged · V3 zero residual hits across all 11,593 reachable text blobs and
every commit message **with a positive control — the same 41 patterns hit the
untouched backup 41/41 in blobs and 11/11 in messages** · V4 commit map.
Scrubbed: the s327 fixture's real identifiers (2 SSNs + 5 name/street words)
and 17 Lacerte packet codes absent at tip. **⏳ DEFERRED (Ken's "for now"):**
~86 TaxWise packet surnames living only in old `STATUS.md` /
`STATUS_ARCHIVE.md` blobs, plus the s297 tier-3 tip-present tokens —
REVIEW_QUEUE's **REWRITE #3**, Ken's call whenever he wants it. Backup mirror
(KEEP): `D:\tax-test-data\repo-backups\delvio-tax-pre-rewrite-2026-09-02.git`.
Old→new SHA map: `docs/history/rewrite-2026-09-02-commit-map.txt` (2,721 rows).
**⚠⚠ EVERY delvio-tax SHA quoted before 2026-09-02 night is STALE — resolve it
through the map, never from memory. ⛔ PEER LANES MUST RE-CLONE.** The public
`tts-tax-status` mirror never carried these values and was NOT rewritten.

**▶ KEN'S RULING, 2026-09-03 morning — CLIENTS (delvio-crm) IS THE SUITE HUB AND
THE PLACE A CLIENT IS BORN (firm D-042; DECISIONS; SUITE_CONTRACT §1/§3
amended).** Most of the hub was already ruled and live (D-033 client record,
D-041 staffing, C-7 balance); the one clause that changed is "Tax is the
only place a client is born." The tax app stays the sole WRITER — creation
becomes a suite door. Ken's answers: **SSN pass-through** (never held
outside Tax; the CRM's forwarder must prove last-4-only logs) · **CHECK-IN
FOLDS INTO CLIENTS ENTIRELY (D-043, superseding the morning's "link" answer):
the kiosk is gone, Carissa checks everyone in, she CREATES walk-in clients at
the desk through the same door with the SSN/EIN in hand (D-036's "cannot
create" superseded), mail log + drop-offs become searchable CRM records, the
check-in app's UI retires once Clients holds the data** · **CRM lane starts
now, this lane's doors are the NEXT UNIT after the current Lacerte leg**
(BUILD_ORDER ⑥b). **STATUSES (D-044):** Inactive = former client only;
duplicates are MERGED into the EIN/SSN survivor and then HARD-DELETED (Ken's
pick over a tombstone state, the Ledger/audit cost named) — every reference
in every schema re-pointed first, a zero-reference sweep before the delete,
a reviewable plan Ken approves, a name never a key → **⑥c `manage.py
merge_client`** in this lane; first input = the CRM's 37 SSN-found pairs.
Census today: 3,757 active · 73 inactive (50 never had a return — the
duplicate/junk class; 23 carry a 2025 shell — genuine former clients). Notices
are Delvio Research's Matters (D-040) — the CRM reads them, builds nothing.
This lane's unit: `POST /api/v1/suite/clients/` (create; `CRM_SERVICE_TOKEN`;
factor `ClientViewSet.create/perform_create` into one service function first
so the door and the in-app form cannot drift) · `GET
/api/v1/suite/clients/<uuid>/returns/latest/` (latest federal + GA per
entity with AGI / tax / refund-or-due — dollars permitted, staff-only
caller) · `PATCH /api/v1/suite/clients/<uuid>/status/` (reactivate / mark
former — Ken: clients leave for a year or two and come back) · `POST
/api/v1/suite/clients/<uuid>/documents/` (the receptionist's Attach on the
drop-off record → this app's `tax-documents` store; + categories
`financial_statements` / `trial_balance` / `source_docs`, one migration) ·
retire "+ New Client" to a link into Clients (bulk imports stay). CRM lane's work order:
`delvio-crm/docs/WORK_ORDER_hub_2026-09-03.md`. **✅ Ken, same morning:
"Retire the tax app check-in screen. It always felt awkward there." — DONE:**
`/check-in`, the FRONT_DESK standalone mount, `desk-search` / `desk-create` /
`temporary` and the `allow_no_identity` serializer escape are removed (zero
temporary clients, zero front_desk members in the live DB first); the
lockout middleware stays with a trimmed allowlist; a front_desk login is told
to use checkin.delviotax.com. The SSN-or-EIN rule now has NO exemption.
639 server tests green, typecheck clean, bundle builds. **⛔ KEN, console:**
set `CRM_SERVICE_TOKEN` on both Render services when the door ships.
Ken's check-in clarifications (in the CRM work order): mail log by date ·
the day's check-ins · both on the client record · the preparer is NOTIFIED
on every check-in (requirement; channel = the lane's call) · Reactivate on
the record and on check-in of a Former client. Google Sheets sync KEPT for check-ins, mail log and drop-offs (Ken) — moves
with the data to Clients.
**⛔ KEN (documents, found 09-03):** the suite has TWO document stores with
no link — the tax app's `tax-documents` (what return prep + Research read)
and the portal's `portal-documents` (client uploads, /staff/ manager, the
CRM's count). Ken walked the desk flow and chose **Attach on the drop-off
record** (no file naming, no client numbers): the scanner saves to its
folder (scan-to-Google-Drive at Conyers), the receptionist attaches the
newest file from the drop-off she has open, the CRM forwards it to this
app's document door → `tax-documents`. Source docs live in THIS app's store
for January; unify the two stores after season (REVIEW_QUEUE). The CRM's
`/desk` (receptionist check-in + create) waits on the ⑥b door.

**🏁 SLATE v2.3 RATIFIED (Ken, 2026-09-03 late afternoon: "I'm pretty comfortable
with where we are now… duplicate this overall system with every app"; firm
D-045).** Canonical in delvio-design (tokens + §2.2 spec + CHANGELOG, tag
`v2.3`). The tax app IS the reference (live through `09a0447d`); every other
app is being synced to v2.3 by one session each, in parallel, with its own
base color — Clients in check-in's orange with the C tile; delvio-1099 on a
branch for Ken's go (D-027); delvio-checkin skipped (retiring, D-043). Ken:
"I won't promise that I won't make some more changes next week" — the
rule-change protocol is in §2.2.
**ROLLOUT RESULT (all seven sessions reported, 2026-09-03 evening):** LIVE —
Clients `418625a` (orange; 276 tests; also purged 50 dead Ledger-era token
refs), Ledger `d02accd`+`e235897` (green; 477 tests; the color-mix fix),
Portal `edb5178` (sky; staff surface full v2.3, client pages labels/boxes/
accent only; 100 tests), Scheduler `7ccca49` (violet; 436 pass + 1 PRE-
EXISTING red in test_stripe_checkout; public booking pages keep The Tax
Shelter gold per Ken's 2026-08-01 rule — his call to change), Research
`90b6d2b` (olive; 156 tests), Launcher `ee0701e` (Clients tile orange + C;
Check-In tile kept → TWO orange C tiles until check-in retires — Ken may
want it greyed). **⛔ KEN: delvio-1099 is on branch `slate-v2.3` @ `bc6184a`
(43 tests, contrast gate 956 pairs) — review, `git merge --no-ff
slate-v2.3` into main, push = deploy.** Two ports hit the color-mix trap
(accent set on a wrapper → navy bands); documented in §2.2 (`abccbf4`).
**▶ SLATE v2.3 PASS 1 — LIVE (Ken, 2026-09-03: "hammer the tax app today and
get it exactly where I want it", Lacerte as the reference: dark labels, dark
boxes, a little more color; then every other app follows the tax app with
its own base color; Clients takes check-in's ORANGE and the C icon).**
Tokens (vendored `slate-tokens.css`, all proposed upstream as v2.3):
`--text-label` = gray-1000 (new; `.slate-inputrow-text` now medium weight on
it) · `--value-entered-border` gray-400 → **gray-700** (every worksheet
field box) · `--border-emphasis` → gray-700 (the client-search frame) ·
`--surface-section` = accent 10% on white and `--surface-toolbar` = accent 5%
(section bands, table heads, toolbars carry the app's base color — navy
here, orange in Clients — automatically via `--accent`) · `--surface-row-alt`
→ gray-100 · `.slate-secheader` and `.slate-rm-th` text in `--accent`.
⚠ Rule-change protocol, stated to Ken: he amends a rule by saying so in
chat; it is recorded dated and the old one is superseded; "do not
re-litigate" binds this lane, not him. Next: Ken's reaction → pass 2.
**▶ UI HOUSEKEEPING, earlier the same morning:** the Return Manager client-search frame is darker —
new vendored Slate token `--border-emphasis: var(--gray-600)` on
`.slate-rm-search` (was `--border-strong`, gray-400; gray-500 is one shade
off on this scale). Proposed upstream to delvio-design as v2.3 with
`--surface-row-alt` → gray-100. The CRM roster (no row shading, a search box
still on retired Ledger tokens) is the CRM lane's — handed over per D-037:
`delvio-crm/docs/WORK_ORDER_roster_ui_2026-09-03.md` (exact CSS, same
tokens, so the two rosters match). Ken's forwarded Claude-Chat shell/forms
redesign was NOT adopted (replaces Slate v2.x wholesale, D-018; stale app
names); its one sound rule — a field border never lighter than gray-400 —
is already Slate's.

**▶ BOOT TASK ① DONE — the "59 never committed" overnight job finished
(`tmp\commit_s328_uncommitted.txt`, batch `s328-uncommitted-commit-001`):
31 landed · 7 fenced (already carried rows — correct) · 17 no_tie · 4
staging errors.** Then the holds were RE-EXTRACTED with current code (the
stale-payload rule) — `tmp\s328-rie-refix-src` → `PipelineOut\s328-rie-refix`
→ `tmp\commit_s328_rie_refix.py` (batch `s328-rie-refix-commit-001`): **11
GA holds → 9 emitted → 8 TIE and LANDED** (clients 3815 · 3825 — the
11,038-vs-8,596 RIE case — · 4419 · 2019 · 1794 · 2228 · 4751 · 1810;
verified by the data: federal filed + GA-500 filed; six carry document
rows and **clients 2228 and 4419 correctly carry none — both are sole
proprietors whose only income is Schedule C** (AGI 3,833 / 15,517, SE tax
582 / 2,447), so zero document rows is the right shape, not a coverage
gap), **1 real hold: client 4081** (GA S1-7 / RIE-TP-17 43,756 vs
43,925 — the carried "$169" item, still Ken's), **2 refused by name:
clients 4429 and 3871** (GA 7b unborn-dependent exemption — the s323
class, an ENGINE leg nobody has built; witnesses now 8). Still held from
the overnight job, by class: **§6654 federal 37/38 penalty deltas —
clients 1938 · 3680 · 1219 · 3514 · 2774 · 4093** (Ken's family, six more
witnesses) · **`r_1099s.distribution_codes` 3-char — clients 2793 · 3010 ·
3160 · 4589** (build queue ②, now 8 packets).

**▶ FRESH DB CENSUS AT THE s329 CLOSE (a count is a timestamp — re-run it,
never quote this one later):** 2025 federal 1040 shells **2,978 — filed
1,258** (was 1,218 at the s327 close: +40 tonight), draft 1,716, in
progress 4. GA-500 under a filed federal: 1,224 — **1,187 filed, 37 draft**
(the same 37 the entry lane owes a face check, listed below).

**▶ BATCH-296 items 54–59 triaged by the DATA (annex appended to the
file):** 54 / 56 / 58 / 59 are FILED (58 + 59 = the s326 owner-witness
family); **55 (client 4502) and 57 (client 4547) stay DRAFT — re-extracted
tonight, both now REFUSE BY NAME ("joint return with ownerless
documents")** — the s326 rule refusing to guess an owner; they need an
owner witness or hand-keying. No build in 54–59. `/bugs`: no open reports.

**▶ NEXT (s329's list, superseded by the s330 NEXT above — ⑥b is SHIPPED a3cedfd4):** ⑥b the two Clients-as-hub suite doors — DONE, four doors not two · ⑥c `merge_client` (duplicate merge + hard delete, dry by default, zero-reference sweep, Ken-approved plan; D-044) · ② the 3-char
`distribution_codes` model gap (migration; 8 packets) · ③ the Lacerte face
readers by wall count (Sch 1 → Sch B → Sch 2/3 → Sch D+8949 → Sch A → Sch
C; 8995 via `taxwise1040/f8995.py`) — for each, drive ONE packet whose
only wall is that face before trusting the census · ④ the shadow-2210
reader (face line 38) · ⑤ the GA 7b unborn-dependent engine leg (8
witnesses; spec with the states lane).

**▶ THE LACERTE-LAYOUT EXTRACTOR PASS (BUILD_ORDER ⑥, OPEN) — state at
the s328 close, unchanged tonight:** leg 2 shipped (`5173e51f` +
`c5101c8c`; 276 green across the four extractor suites): the Federal
Worksheets document readers (`lacerte1040/worksheets.py`), the GA 500
income-statement block (`ga500_stmts.py`), the shared Schedule 1-A emitter
(`taxwise1040/sch_1a_emit.py`). Lacerte prints no W-2 / 1099 facsimiles —
Wage Schedule → `w2s`; Pension + IRA schedules → `r_1099s` (blank Taxable
= 0; `***` = 8606-computed → refuse); Interest / Dividend lists →
`int_1099s` / `div_1099s` (printed ONLY when no Sch B is filed); Pub 915
line 1 → `ssa_box5_net_benefits`; payer FEIN + GA withholding ONLY from
the GA 500 INCOME STATEMENT DETAILS block. Owners on MFJ: the MFJ-vs-MFS
comparison page → the s326 RIE rule → refuse. Lacerte prints every X
~10pt LEFT of its label (pinned on a corpus census in
`lacerte1040/layout.py`). The s328b corpus run: 255 packets → 3 emitted,
252 refused; **🏁 client 3251 = the first Lacerte-pipeline landing** (tie,
committed, data-verified); #1522 / #1570 fenced (filed by the entry lane).
Wall list (faces gate first): Sch 1 181 · 8995 172 · Sch D 131 · Sch B
126 · Sch 2 119 · Sch 3 113 · 8949 109 · 1116 95 · 4562 84 · Sch A 76 ·
Sch 1-A 75 (READ) · Sch C 75 · Sch E p2 75 · GA 4562 72 · 8582 65 · Sch E
63 · 7203 62 …. Single-wall packets: Sch B 2 · Sch 2 1 · 8606 1 · Federal
Statements 1 · Maryland 1 · one non-Lacerte · one filled shell · the
packet in `tmp/s328_ken_questions.md` (GA 500 p1 names a different primary
SSN — ⛔ KEN). **Rule: never write a Lacerte packet code outside
tax-test-data** (codes ARE surnames; map `1040\tmp\s328_code_to_client.json`).

**▶ KEN'S 6252 RULING (relayed by the entity lane as an OPTION SELECTION,
2026-09-02 night; DECISIONS "Form 6252 line 19"):** round the gross
profit percentage to four decimals as Lacerte prints. Verified: the 2025
instructions say "rounded to at least 4 digits" — a FLOOR, both methods
comply, a permitted vendor accommodation Ken chose for dollar
reconciliation (not an engine error). DEFERRAL_AUDIT (17) ruled; build
stays in the auto-dealer unit (rounding MODE = a convention to pin on a
witness). #3773 was closed out FILED by the entity lane on Ken's
"file as is and note the credit"; the shareholder 1040 (batch ee7d11b9,
key i-skoglund-4000-d) still waits on the auto-dealer unit (items 2–4).

**▶ KEN'S RULING, 2026-09-02 (s327 sitting) — A TIE IS A FILED RETURN, and
the Filed count is the practice's true count.** `commit_staged_return`
marks filed on a tie (federal + attached states). **Federal 1040 Filed
1,218 of 2,978 at the s327 close (all tie-verified); GA-500 under filed
federals 1,147 filed / 37 draft.** ⛔ ENTRY LANE: 37 GA-500s under
hand-filed federals were never tie-verified and stay Draft — clients
1019 1033 1034 1035 1056 1075 1076 1081 1089 1090 1094 1136 1164 1165
1215 1216 1218 1254 1259 1262 1273 1274 1281 1307 1315 1367 1372 1411
1587 1588 1600 1601 1609 1842 1857 1858 1891 — verify the GA face and
mark, or record why no GA return applies. 8 returns are filed with zero
document rows (clients 1400 1106 1200 1974 4036 3878 1598 3264) —
verify one before assuming SSA-only/hand-keyed.

**▶ ENTITY LANE (other account):** five Skoglund S-corps FILED (#1153,
#2920, #3103, #3773, #4460); 1065: 2 of 69 filed, 66 packets in
`1065\Inbox` — the partnership import is the next entity job (BUILD_ORDER
Ken-directed block). Carried entity findings: DEFERRAL_AUDIT (9)–(18).
**Relayed 2026-09-03 night (DECISIONS "Three entity-lane rulings"):** client
219 — **CLOSED 2026-09-03 night, NOT pending: Ken "Innova is no longer a client
please mark it as inactive"** (relayed by the tax-test-data lane; verified here by the
DATA — client **#2632** INNOVA ELECTRICAL SERVICES INC is `inactive` as of 20:52 UTC,
its 2025 1120-S shell still draft, nothing ever committed). Do not enter the packet.
⚠⚠ **The packet's printed "Client 219" is the LACERTE ID, not the Delvio client
number (2632); NO client_number=219 exists — searching by it finds the wrong record
or none.** The return's acceptance was never in doubt; the client has gone · #3137 DROPPED → Inactive
(the §1245 and balance-sheet questions are moot) · #4835 stays held (not
final; the $1 K-14a rounding is accepted as `tie_with_exception`).
**Entry-lane flag answered by the DATA (2026-09-03 night):** #4000 has NO SSN
twin — #4001 / #4002 exist but are different individuals (surname family,
no identity row, no return, created 02-25); #4000 holds the only identity
and the only 1040, with FOUR uncommitted staged rows pointing at it
(`i-skoglund-4000-b/-c/-probe1/-d` — the entry lane owns the three stale
ones and will EXCLUDE them via `POST …/returns/<key>/exclude/` once Ken mints
its session; ⚠ `-probe1` is a diagnostic that asserts a FALSE year-of-sale
and must never commit — the reason nothing here ever selects a staged row
by client: commit scripts consume payload FILES by explicit key). The ⑥c merge command gains the survivor rule + a staged-work guard
(DECISIONS): it refuses to delete any client an uncommitted StagedReturn
or a PipelineOut payload resolves to, and #4000–#4002 are report-to-Ken,
never merged by the tool.

**▶ NEW SUITE MODULE — delvio-research:** doors built
(`apps/suiteapi/research.py`); **⛔ KEN: set `RESEARCH_SERVICE_TOKEN` on
BOTH Render services** — both doors are inert (503) until it exists.

**▶ DISREGARDED ENTITY TYPES + THREE CLIENT-RECORD RULINGS (s327):** built
and live (`clients.0015`/`0016`); follow-ups queued in BUILD_ORDER
(owner field on disregarded entities; the due-date calendar).

**▶ CLIENT-BASE RULINGS relayed to the CRM (s327 night):** done; still
open: one bookkeeping client's monthly-vs-quarterly.

⚠⚠ **ANOTHER CC ACCOUNT WRITES THIS LANE — THE SHARED FILES ARE THE ONLY
PLACE ITS WORK IS VISIBLE.** Do not order events by `updated_at`.

**▶ BUILD QUEUE after the Lacerte legs:** ② the 3-char
`distribution_codes` model gap (migration; 4 packets since s324) · ③ the
TaxWise extractor walls by measured count (f6251 = 13 ·
sched_line_detail = 6+ · f5329 · the classifier patch, GA part-year
detector first) · ④ the three re-raised Lacerte engine holds (clients
1922, 2386, 3517) · ⑤ the GA 7b military-exclusion engine leg (7
witnesses; waits on the states-lane spec export) + the 7c/7f DIS
transcription · ⑦ the §6654 family decision (Ken) · ⑧ BATCH-013 (posted,
UNWORKED, 10 Tom-lane product gaps; ⚠ item 5's premise is refuted —
Schedule C has no business_address on the MODEL, a migration not a sync)
· carried: the 8615 parent-first guard · out_of_scope_states · the
zero-activity GA-attach gap.

**⛔ WAITING ON KEN:** may the Lacerte pipeline file a packet whose
manifest carries a non-Georgia state return federal + GA only (47
packets refuse on it — states on hold)? · the packet named in tmp/s328_ken_questions.md (Lacerte): the GA 500
page 1 names a different primary filer (SSN …7699) with the 1040's
filer (…5844) as the spouse — swapped spouses on the GA return, or the
wrong GA return in the packet? · the §6654 family (…0500/…0534/…7701/
…7044 + clients 1938 and 3680 tonight — line 37/38 penalty deltas) · seed ONE client (the
…4641 taxpayer in Jenny's book; ⚠ do NOT edit #4054) · client #3572's
contaminated name (#4514 same shape) · three clients with no 2025 1040
shell · does the standing commit authorization extend to ENTRY-LANE
HAND-KEYED commits? (client 3250 staged, dry-run TIE) · client-2149's
filed GA 17a = 2 exemptions on a single/zero-dependent return · the 61
Gail federal reprints (`tmp\GAIL-TRIAGE-2026-08-31.md`) · the …4203
W-code question · one Georgianna reprint + five named reprints · the
asset METHOD-DERIVATION TABLE review (annex) · vendor-name allowlist for
the mirror guard? · carried: 1071 · 1141 · R-GA500-RIE · 4059 W-2G
address · Sch D carryover · GA RIE L10 · 4081's $169 · standing 1–8 · 2a
scope flag · AL 40 · the 4 Tom-book holds (…8505 · …2276 · …2827 · …8791).

**▶ OFF-SPINE, SHIPPED 2026-09-01 (Bob lane):** `GET
/api/v1/suite/clients/by-phone/` (`apps/suiteapi/caller.py`, 8 tests).
