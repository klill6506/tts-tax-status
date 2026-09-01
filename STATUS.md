# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s324 (mid-flight if the Gail batch is draining), 2026-08-31 late night

**▶ SCHEDULE E + F READERS SHIPPED — FORM 8582 IS NOW THE TOP UNLOCK
(22 packets).** Ken's build ask, done and deployed (`8bf9d5e5`,
`deacb63a`). Both were READER-ONLY: RENTAL_FIELDS/RentalProperty and
SCHF_FIELDS/ScheduleF already carried every field.
- `sch_e_p1.py`: 3 property columns by x-window; ⚠ the A/B/C letters
  sit on the "Income:" row ~12pt BELOW the "Properties:" caption (all
  54 witnesses failed until the band scan); MULTI-PAGE (>3 properties
  starts a second page 1 — 6 witnesses) — **the form's own FORM-WIDE
  23a/23d/23e totals are what caught it**; line 26 parsed so Schedule 1
  line 5 decomposes. 51/54 pages parse, **75 properties**; the 3 that
  don't REFUSE rather than mis-assign.
- `sch_f.py`: two-column form, each line identified by its ANCHOR X
  (Part I 1a-2 @x391 · 3-6 a-side @x260 · b-side+7/8/9 @x489 · Part II
  10-22 @x206 · 23-32f @x486); 32a-f other expenses with descriptions;
  line 33 and line 34 (= 9 − 33) as self-checks; Schedule 1 line 6
  decomposes. **27/27 pages parse, ZERO failures, both self-checks
  green on every one.**
- ⚠ EMIT YIELD IS LAYERED: both classes are GONE from the refusal
  census but emits over the 78 gated packets moved 0 → 3. Next walls,
  measured: **Form 8582 = 22** · Form 6251 (AMT) = 13 · Sch C/E/F
  line-detail worksheet = 4 · assorted unknowns. **8582 is the single
  biggest remaining unlock** (a rental return almost always carries
  one) and is the next build.

⚠⚠ **ANOTHER CC ACCOUNT WRITES THIS LANE — THE SHARED FILES ARE THE
ONLY PLACE ITS WORK IS VISIBLE.** s325: the Lacerte book was fully
worked in an account this session cannot see, while STATUS still said
"the importer has never seen these" and Ken's list still carried it as
an open go/no-go. Two concrete hazards measured by the entry lane the
same day: (a) their held batches for 1141/2234 are still `status=staged`
yet those returns CARRY ROWS, and batch keys `L058-2455-a`/`-b` already
existed on the server under their own naming convention, `-b` bump
included — i.e. a key collision between accounts is real, not
theoretical; (b) **DO NOT ORDER EVENTS BY `updated_at`** — the DB
reports 08-28 on two of those returns and 08-31 on a third, the lane's
files are dated 08-31, and this box's clock says 08-28. There is
genuine CLOCK SKEW ACROSS MACHINES. ⚠ This retro-weakens one strand of
the s325 Gail evidence: I cited "min updated_at is 5s after batch
creation" as support. The CONCLUSION stands on the answer-key
comparison (122/122 tie), which uses no timestamps — but the timestamp
strand should not be reused.

**▶▶ s325 COMMIT BATCH — KEN'S "commit them" IS EXECUTED AND VERIFIED
BY THE DATA.** Final ledger, footing exactly to the 283 payloads:
- **LANDED 138** — every one a per-return-transaction FULL TIE
  (gail 9 new · jenny 102 · jacob 23/23 · whit 4/4, the last after one
  pooler retry). Verified against the DB, not the log: 138 staged rows
  `status=committed` with `committed_at` + `resolved_return` set.
- **FENCE 121** — Gail's already-landed set refused by the double-commit
  fence, exactly as designed.
- **NO_TIE HELD 19** (rolled back in-transaction) — triage list at
  `1040/tmp/s325_holds_triage.txt`; the deltas cluster in the KNOWN
  families: GA RIE (RIE-SP-17 ×7 · RIE-TP-17 ×6 · S1-7/S1-13) and the
  federal payments/§6654 lines (37/38).
- **STAGING ERRORS 5** — four the KNOWN 3-char `r_1099s
  distribution_codes` model gap (carried from the s324 Gail holds; the
  model caps at 2 chars, packets print 3 — a real product item), one
  depreciation `link_key` naming.
Batch log: `1040/tmp/commit_s325_books.txt`.

⚠⚠ **THE RUN ITSELF FAILED TWICE BEFORE IT RAN, AND BOTH FAILURES ARE
LESSONS:** (1) my killed first launch left its `ImportBatch` row behind
(a bare `.create` autocommits) and the relaunch died on the unique key
BEFORE the first payload — with the traceback in an err file my monitor
was not watching, so **the silence read as progress and I told Ken it
was committing**; the entry lane caught it by MEASURING (pid dead, log
63 bytes, err file has the cause). Scripts now `get_or_create` the
batch and merge stderr into the one watched log; **verify a launch by
its OUTPUT GROWING before reporting it running.** (2) A third
statement-timeout on one return was a LOCK, not slowness: my own killed
attempt's connection sat `idle in transaction` for 13 minutes holding
the uncommitted insert of the SAME (batch, return_key) — every retry
waited on the invisible tuple. `pg_terminate_backend` on the orphan,
then the return tied first try. **After killing a batch process, sweep
for its idle-in-transaction connection.**

⚠ **THE BOOKKEEPING FIX IS FORWARD-ONLY** (the entry lane's sharpening,
adopted verbatim): in-process commits now write
`status/committed_at/commit_result/resolved_return`, so the staged row
tells the truth **from tonight on** — but every in-process commit
BEFORE tonight still looks uncommitted. The audit rule stays "audit by
the DATA against the payload's `expected` key"; the discriminator is
reliable looking forward and unreliable looking back.

**🔎 THE SSN-COVERAGE FINDING (for Ken's EIN/SSN priority, via the CRM
lane's ruling relay):** `backfill_tax_identities --dry-run` after the
138 landings found only **1** identity to create — because the seeded
shells already carry SSNs (that is how identity matching works), the
import campaign adds essentially NO SSN coverage. **The real gap is
1,704 individuals with no SSN source anywhere the returns can reach**
— closing it needs a SOURCE decision (another export, or organizer
data), not more imports. The CRM lane holds the EIN half (516/520
Lacerte rows settled, their ledger).

**▶ s325 BUILD: THE FORM 8582 LEG SHIPPED (`e6cf21c0`, deploy verified)
AND ALL FOUR BOOKS ARE RE-EXTRACTED ON EVERY CURRENT LEG.** The top
unlock opened: f8582 BLOCK→EXTRACT, and the page's two INPUTS route —
column (c) prior-year unallowed (single-candidate rule, aggregate
"SCH E RENTAL"/"K1S" rows, ambiguity refuses) and the **Part IV/V
placement as the active_participation witness** (model default True =
the $25,000 allowance granted silently; a Part V rental now keys
false). 13/13 witnesses parse; the first opened packet DRY-RAN the
full live chain and **tied 15/15** — and that first packet exposed two
upstream defects a session old (s318 censoring, 3rd+4th instances):
sch_e_p1 emitted property_type WORDS where the model stores DIGITS
(parsing ≠ staging — no rental payload had ever reached staging), and
the asset register's abbreviated rental names never matched the
server's exact-equality link (emit now reconciles against the packet's
own parsed properties; unique-prefix or single-property only).
⚠ Plus the s317 class AGAIN: the extract runner DIED PRINTING a
refusal containing Σ to the cp1252 console — Jenny stopped at 22 of
177 with exit 0 and NO traceback, and the truncated run looked
complete. `__main__` now reconfigures stdout/stderr to UTF-8.

**▶ THE COMMIT QUEUE IS RE-BASED ON THE s325 RUNS (re-extract-first is
SATISFIED — commit from these, the older run dirs are VOID):**
`PipelineOut\gail-s325` **150** emits (⚠ includes the 122 already
committed — the double-commit fence / replace semantics handle them;
~28 are NEW) · `jenny-s325` **106** · `jacob-s325` **23** (the first-opened
8582 witness now carries its routed inputs) · `whit-s325` **4**. Census vs the
old runs: 283 vs 271, and EVERY movement attributes to a named leg —
Gail +18 (8582/asset/matcher) · Jenny −7 ALL the vacuous-GA guard
(packets that would have "tied" an all-blank Georgia answer key —
the s324 lesson enforcing itself) +1 the compound-surname matcher ·
Jacob/Whit unchanged. Only 1 of 283 emits carries the new 8582 inputs
yet — the other 8582 witnesses sit behind their NEXT walls (f6251 13 ·
sched_line_detail 6+ · f5329 · unknowns), the layered-yield truth.
**VOID: gail-g4, jenny-p2, jacob-p2, whit-p1** (superseded by -s325).

**GAIL IS DONE AND VERIFIED AGAINST THE DB (s325 boot): 124 commit
events across 122 distinct clients**, all 122 re-verified this boot as
TIE against their own `expected` answer keys (86 in
`s324-gail-commit-001` + 3 in `-002-netretry` + 35 in
`-003-remaining`; 2 clients committed in more than one batch).
⚠ The old figure here read "121 (86 + 35)" — it OMITTED the netretry
batch's 3 entirely and was 3 low. Corrected s325.

⚠⚠ **TWO COMMIT ROUTES SHARE ONE FIELD — DO NOT AUDIT COMMITS VIA
`StagedReturn.committed_at`.** Gail was committed by an IN-PROCESS
script calling `commit_staged_return()` directly, and that function
says so at `backentry.py:8770`: *"Does NOT touch staged.status /
batch.status — the view persists those (so dry_run stays write-free by
rollback)."* So a SUCCESSFUL in-process commit leaves `status='valid'`,
`committed_at` NULL, `commit_result` `{}`, `resolved_return` NULL —
byte-identical to never having committed. The 1,248 corpus rows that DO
carry timestamps are HTTP-lane commits. **Audit by the DATA (compare
the return against the payload's `expected` block), never by the staged
row.** This is the same discriminator that produced the s324 "23 Tom
packets silently lost" retraction; it has now cost two sessions. ⚠ 38 mid-batch "holds" were ONE DNS DROP, not data — read the
error text before counting holds. Landed Gail packets do NOT move
(originals stay in `Inbox\Gail`; `tmp\merged-gail-v5\Gail` holds the
pipeline artifacts).

**THE 11 GAIL HOLDS — the triage list** (suffixes only; surnames in the
BATCH-296 annex): five no_tie (…0500 · …5816 · …7970 · **…0680 — the
FIRST live test of the asset-detail import against a filed return,
decompose it first** · …7193) · three staging errors (…8546 · …4344 ·
…2981) · …9164 (below) · plus one
reading `ga500:23 expected 10 actual 18` — a bare Δ8, the LIC-gate
signature, which a re-extract on `6ed55795` should convert.

**▶ THE COMMIT QUEUE, all extracted and waiting:** ① the TWO LIC holds
(…2276, …8505 — `PipelineOut\tom-lic2`, expected to TIE) · ② the four
georgianna-g4 joint-valve emits (`tmp\commit_s324_g4_4.py`, ready) ·
③ the new books' **135+ emits** (jenny-p2 112 + jacob-p2 23 + whit 4)
· ④ the 9 shell-refusal conversions (`PipelineOut\shellfix-rerun`).
⚠ RE-EXTRACT THE BOOKS FIRST — every run dir predates some of
tonight's legs (LIC gate, matcher, §179 allowlist, empty-GA gate,
Sch E/F).

**⚠ RE-EXTRACT THE BOOKS BEFORE COMMITTING THEM** — gail-g4, tom-t1
and jenny/jacob-p2 all predate tonight's later legs (the LIC gate
`6ed55795`, the matcher `e629f7b5`/`04207d7c`, the §179 allowlist).
Evidence it will pay: a Gail hold in flight reads `ga500:23 expected
10 actual 18` — a bare Δ8, the LIC signature exactly.

**s323 boot actions: ALL DONE.** ① Tom batch FINISHED: **158/162
tie-committed** (96 s323-001 + 62 s324-003); 4 holds (below).
Done\Tom = 186 = the 158 backentry-committed + 28 worked through
ANOTHER ROUTE (hand-keyed, marked `filed`, no StagedReturn row);
Inbox\Tom = 154. ⚠⚠ See the Carried entry: my "23 silently lost
packets" call was WRONG and is RETRACTED. ② Migration 0014 applied (had to precede
the batch — the /clear killed the old-code process and every new one
needs the dba column); pushes `8bb8f932` → `1d1efe13` all deployed +
Render-verified live (dep-daavm1bl550s73etkm00). ③ Mirror sync ran
clean after the states lane scrubbed the publisher name (their f43c13a).

**s324 shipped (all deployed + verified):**
- **The dba feature ACTUALLY shipped** (`9d24dc59`+`1d1efe13`): s323g
  had built it on UNROUTED legacy pages (pages/ClientDetail + Clients)
  — vite tree-shook it; the deployed bundle had ZERO dba code. Moved to
  the ROUTED ClientReturns + ReturnManager, tag reads client_dba (now
  in the list serializer), /tax-returns/ search + counts span dba (the
  desk surface s323 missed), 4 new tests incl. a routed-page mount.
  Browser-verified live on the dev server; the firm row now carries
  "NATIONAL TAX INC dba The Tax Shelter" in the DB (Ken's ruling —
  keyed through the live editor). ⚠ RenameClient is ALSO only
  referenced by the dead page — the August rename UI never shipped;
  flagged to Ken, files kept.
- **The asset-detail import** (Ken green-lit s323): ASSET DETAIL REPORT
  → depreciation_assets rows. 17-ruler-column parse, Form Totals sum
  identity, Form:/Rental context routing (packet E/F page census breaks
  free-text farm names), the **GA-basis discriminator** splits the
  combined 179+Spec column (equal state basis = §179, differing =
  bonus). Fixed ALLOWLIST DRIFT: 1040 section lacked
  sec_179_elected/prior (entity lane always had them). Sold assets +
  unmapped shapes refuse by name. The asset refusal class is CLEARED
  (83 Gail instances → 0); ⚠ the "solo" census was an upper bound —
  ex-solos surfaced their NEXT refusal layers (s318 censoring): the
  emit yield arrives with the **Schedule E + Schedule F legs — the
  next build unit**.
- **Gail's book joined** (Ken's split-print convention — KEEP IT):
  716 files → 389 clients → 328 merged packets (content-aware merge
  v5 in `tmp\merge_gail_v5.py`; the GA print binds a fed copy + bank
  paper, cut at merge). gail-g4 census: **132 emitted / 196 refused**.
  ⚠ VOID: `tmp\merged-gail*` (v1–v4 dirs), `PipelineOut\gail-g1/g2/g3`
  (stale-staging + pre-asset iterations). **61 Gail clients have NO
  federal print** — Ken's reprint list in `tmp\GAIL-TRIAGE-2026-08-31.md`
  (56 GA-only, 3 other-state, 1 trust + 1 partnership = other lanes).

**s324 LATE — four more legs shipped + KEN FINISHED THE PRINTING:**
- **THE CORPUS IS COMPLETE** (Ken, late): every 2025 TaxWise return is
  PDFed, plus the previously-printed Lacerte book (individuals,
  partnerships, S-corps). **~4,560 PDFs**: 1040 lane 4,276 (preparer
  books 1,338 in Inbox + 223 in Done; flat 308 Inbox / 618 Done;
  **Lacerte Inbox 635 — CORRECTED s325: these have ALREADY BEEN
  WORKED, in a DIFFERENT CC ACCOUNT.** Ken told the entry lane directly;
  they then verified it against the live DB rather than relaying it —
  client 2455 `filed` with 13/13 pinned lines matching an INDEPENDENT
  transcription built from scratch (incl. NIIT + a general-category 1116
  + a zero GA nonresident allocation), 2234 13/13, 1141 11/11. The book
  looks untouched only because the Lacerte export was thinner than the
  TaxWise ones, so Ken re-exported the ORGANIZERS to pick up birth dates
  for clients and dependents) ·
  1120-S 182 · 1065 103. The constraint has MOVED from data to
  extractor coverage.
- **Three (then four) new preparer books, first contact:** Jenny
  **112/177** · Jacob **23/40** · Whit **4/14** (arrived mid-session) ·
  David **0/5** (five unrelated causes, not a class).
- **The GA LOW INCOME CREDIT gate** (`6ed55795`): compute_ga500 fires
  the credit only on `truthy("LIC-NODEP")`, so an unkeyed gate computes
  ZERO and lands as a bare no_tie of (exemptions × the IT-511 p35 band
  amount). Every input is PRINTED — 1040 12a claimed-as-dependent
  (eligibility), 12b age-65 (LIC-65), the GA-500's own 17a/17b/17c —
  so the extractor derives and RECONCILES against the printed count,
  refusing by name on any mismatch. ⚠ The LIC count is NARROWER than
  line 7c (IT-511 p35: natural/adopted children ONLY). Both Tom holds
  now reproduce their filed rows exactly.
- **match_shell could NEVER match a compound/hyphenated surname**
  (`04207d7c` + `e629f7b5`): it tested the surname STRING against a
  token SET. 29 of 33 "no seeded shell" refusals were FALSE — relaying
  them would have duplicated 29 live clients in prod. Now surname-set
  EQUALITY vs the roster's surname half (measured over 2,962 names:
  loses nothing, and RESOLVES four ambiguous pairs — a simple surname
  #2373 vs its two-word neighbour #2372, #4518 vs a contaminated
  roster row #4514, and a pair whose forename and surname are each
  other's reversal, #1067/#1874). Names in the BATCH-296 annex.
- **The shell refusal now DIAGNOSES** (`7f48c155` + `a56db608`): it
  names the nearest roster rows with client numbers, and an exact name
  match whose SSN differs is its own case — reporting both SSN tails
  and BOTH causes (a namesake needing a NEW client vs a wrong number)
  while concluding NEITHER. ⚠⚠ Its first draft asserted "one identity
  is wrong", which would have overwritten a correct FILED client;
  corrected within the hour. **SEEDING QUEUE IS ONE: the …4641
  taxpayer in Jenny's book — client #4054 must NOT be edited (a
  different, real person who shares the name).** Name in the annex.

**▶ OFF-SPINE, SHIPPED 2026-09-01 (Bob lane, not this lane):** a second
read-only suiteapi endpoint, `GET /api/v1/suite/clients/by-phone/`
(`apps/suiteapi/caller.py`, `tests/test_suiteapi_caller.py`, 8 tests).
Bob answers Ken's phone and needs to know whether a caller is a client;
it sends the caller ID and gets back a client name + client_number +
which field matched. Same BOB_SERVICE_TOKEN, same PII boundary as
returns.py. Touches NO form, engine, or entry code. Note for anyone
working clients: `Client` has no phone column — business numbers are
`Entity.phone`, individuals' are the 1040 `Taxpayer.daytime_phone`, and
the endpoint searches both. Contract lives in the bob repo at
`docs/CALLER_MATCH_CONTRACT.md`.

**▶ NEXT:** ① finish/verify the Gail commit batch (boot action) ·
①b re-extract the books on tonight's legs, THEN commit the queue
above · ② **the Schedule E + Schedule F extractor legs** (the activity-depth
domain: 27 packets gate on E, 21 on F across the books; rental_
properties + schedule_fs backentry sections already exist) · ③ the
classifier patch (1099-NEC DETAIL REPORT — new page type, many Gail
witnesses · 1116 / 8283 / 8615 / Coverdell pages · az140/dc_d40/or40p
BLOCK types · **the GA PART-YEAR DETECTOR FIRST** — the …2036 hazard) ·
④ the 8615 parent-first commit guard (parent …3046 landed, child …8484
waits) · ⑤ out_of_scope_states marker (BATCH-007 approved) · ⑥
self-rental flag · ⑦ GA 12b + 7b engine legs when the states-lane
exports land · ⑧ the zero-activity GA-attach gap (3 witnesses) · ⑨ the
§6654 recompute divergences (…7701 Δ65 · …7044 Δ9) · ⑩ commit …7595 +
…7107 (NOT …4203 — named issue; NOT the 7b six).

**⛔ WAITING ON KEN:** ⓪⓪ (s324 latest) **seed ONE client** — the
…4641 taxpayer in Jenny's book (⚠ do NOT edit #4054: a different real
person, correctly filed; names in the annex) · **client #3572's name
is contaminated** (a "no need to fil" note is IN the client record —
data cleanup; #4514 carries the same shape) · **three clients have no
2025 1040 shell** (two report
as 1120-S filers — if so their 1040 packets should leave the queue;
entry lane flagged directly) · ~~**the LACERTE BOOK (635)**~~ —
**ANSWERED, REMOVE FROM KEN'S LIST (s325): already worked in another CC
account**, verified against the live DB (above) · ⓪ TWO scope questions (s324): does
the standing tie-at-landing commit authorization extend to ENTRY-LANE
HAND-KEYED commits? (their client 3250 LIC return is staged, dry-run
TIE, held for your word — the lane rightly declined my extension of
the ruling) · and the client-2149 filed GA return claims 2 exemptions
(17a) on a single/zero-dependent return the form doesn't support —
filed-return substance, IT-511 question · ① the 61 Gail federal
reprints
(`tmp\GAIL-TRIAGE-2026-08-31.md`) · ② the …4203 W-code question (entry
lane's) · ③ one Georgianna reprint + five named reprints · ④ entry-lane
auth token (their session still 403'd) · ⑤ the asset METHOD-DERIVATION
TABLE review (annex; MACRS+life→DB% mapping, farm pre-2018 150DB — his
depreciation domain) · ⑥ vendor-name allowlist for the mirror guard?
(states lane's suggestion after the publisher-name trip — vendor names
colliding with client surnames; a new guard category = Ken's call) · carried: client
1071 · 1141 · R-GA500-RIE · 4059 W-2G address · Sch D carryover · GA
RIE L10 · 4081's $169 · standing 1–8 · 2a scope flag · AL 40.
**Tom-book holds for triage (4, suffixes verified from the batch log;
surnames in the BATCH-296 annex):** …8505 (GA-500 Δ8) · …2276 (GA-500
Δ5 — both smell like the low-income-credit table, undecomposed) ·
…2827 (payments Δ645) · …8791 (payments Δ315 + engine-asserted 156
other-taxes).

## How this file works (read before editing)
- **Current state only**; overwritten each session. History →
  `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; questions →
  `REVIEW_QUEUE.md`; learnings → `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` /
  `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names (masked
  suffixes ONLY, and only suffixes VERIFIED from a file). ⚠ Tom-book
  suffixes COLLIDE (s324) — in the working tree use surname+suffix or
  client_number.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS (WHEN BUILDS RUN)
Render auto-deploys from `main`: prod = `delvio-tax`
(srv-d6geloa4d50c73el2trg). Push + commit authorizations both at own
judgment; verify every deploy and every landing; hold only for a named
reason. s324 deploys verified live through `1d1efe13`
(dep-daavm1bl550s73etkm00). No held pushes.
- ⚠⚠ `scripts\gen_backentry_schema.py` is a LOCAL generator — s324
  ADDED VOCABULARY (sec_179_elected/prior on depreciation_assets):
  **regenerate + republish the schema at next boot** if the published
  copy predates `1d1efe13`. `dba` note (s323) recorded in
  SUITE_CONTRACT at next touch.

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: no 2025 returns are being prepared in the app.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
The build session manages the siblings; annexes carry anything
load-bearing; never tokens through messages. ONE tree holder; ONE
pytest holder. Codex on Tom's refusals (`1040\CODEX-BRIEF-TOM.md`) —
⚠ s324: two packets reached Done\Tom with NO note/trail (one
uncommitted — restored); if Codex moves files his brief needs a
moves-leave-notes line. Entry lane = Georgianna hand-prep + triage;
states lane = R-GA500-DED + 7b amendment (publisher scrub done f43c13a).
**Outbound names: client_number or VERIFIED surname+suffix (suffixes
collide in Tom's book); surnames only inside the working tree.**

## ⚠ Known red / rotted — THE ONE LIST (post-s302)
- **S-10a** · **S-10c** survive; RS suite 254/0 (2026-08-30).
  **S-10b: all 15 schedule_a gate failures are GATE defects.**
- **The quintet** (s225/s258) · **`test_1040.py` 6 pipeline tests**
  (s234) · **`test_mappings.py` 7 setup ERRORS** (s239) ·
  `test_4868.py` (4, ⛔ KEN s217).
- **Client typecheck: green** (s324).

### ⚠ Test-run hazards (standing — the load-bearing set)
🌐 = campaign-wide · 🔧 = this repo only.
- 🔧 ⚠⚠ NEVER edit `apps/` while a detached pytest OR a commit batch
  runs (s286/s324); one shared test_postgres; `poetry run` only from
  `server\`; inline `-c` BANNED (silent no-op, s324 re-witnessed);
  `python -m taxwise1040` doesn't resolve — run `__main__.py` BY PATH.
- 🌐 ⚠⚠ PS5.1: regex rewrites BANNED; commit messages via `-F`;
  ⚠ `Invoke-WebRequest .Content` on JS/binary is a BYTE ARRAY — regex
  over it silently matches NOTHING (s324's vacuous bundle grep):
  `-OutFile` + `Get-Content -Raw`.
- 🌐 ⚠⚠ FLAT TEXT ORDER LIES ABOUT COLUMN OWNERSHIP; grep the row you
  THINK you're grepping (s324: two probe censuses keyed the wrong
  header row and returned vacuous zeros).
- 🌐 ⚠⚠⚠ **NOTHING-TO-CHECK READS AS EVERYTHING-CHECKED** — s324's
  unifying lesson, FIVE instances across two lanes in one night: all-zero
  expectations "tie" (3 vacuous ties landed); a guard whose input is
  always falsy "passes" (`getattr(c,"tax_identity")` — the relation is
  `tax_identities`); a loop over an emptied plan file "audits" 13
  clients; a check that sees ONE route "proves" a return was never
  committed (23 files moved wrongly); no diagnostic complaint means the
  GA return "attached". **Verify by the PRESENCE of what should be
  there, never the absence of a complaint** — assert the population you
  meant to check is non-empty before reporting a pass, and feed every
  guard a case that MUST trip it. ⭐ Corollary: the error handler nobody
  exercises is where the crash hides.
- 🌐 ⚠⚠ a feature can pass tests+typecheck and ship as TREE-SHAKEN DEAD
  CODE — the routed-page mount test + the deployed-bundle grep are the
  gates (s324, the dba build).
- 🌐 ⚠⚠⚠ **A REFUSAL MUST REPORT THE EFFECT IT OBSERVED, NEVER ASSERT A
  CAUSE** (s324, twice in one hour, both lanes): "no seeded shell" was
  wrong THREE ways in an evening and the buried "or the name differs"
  was always the truth; its replacement then said "one identity is
  wrong" when a NAMESAKE was the likelier cause and the two remedies
  are OPPOSITE (seed new vs edit existing — the latter would have
  overwritten a correct filed client). When drafting refusal text ask:
  what did the check actually OBSERVE, and could a SECOND cause produce
  the same observation? A refusal message is where this failure lives,
  because writing one feels like explaining rather than reporting.
- 🌐 ⚠⚠ **A CHECK THAT OBSERVES ONE ROUTE PROVES NOTHING ABOUT THE
  OTHERS** (s324's worst call — see the Carried retraction): "no
  StagedReturn row" is evidence about the backentry lane, not about a
  return's existence. Route-agnostic answer = the minimal-payload
  -Merge probe against the filed face.
- 🌐 ⚠ THREE TIMES IN ONE DAY a SUMMARY LINE disagreed with its own
  DETAIL and the detail was right (a peer's runner-capture regex, my
  empty-SSN harness artifact, a status=draft inference). Trust the
  per-item output over the rollup.
- 🌐 ⚠⚠ the run LOG truncates refusals — census from refused.json
  (s322); a "solo" count is an UPPER BOUND (s318/s324).
- 🔧 ⭐ live-commit pattern: `tmp\commit_s324_gail131.py` (per-return
  atomic, tie lands, non-tie rolls back; fresh batch_key per run —
  NEVER replay one: a replayed run against a schema-ahead tree aborts
  whole, s324-002). `Firm.objects.get(name=...)`, never `.first()`.
- 🌐 ⚠⚠ a return with a NAMED open issue does not commit even on a
  full tie (…4203).
- 🌐 ⚠ vacuous ties over absent/zero sides are invisible — guard on the
  printed FACT (s323).
- 🌐 pymupdf/TaxWise geometry: values by RIGHT edge; detail reports =
  dash-ruler columns; TaxWise merges method tokens on asset reports
  (SL39.0 / MACRS15010.0 / MACRS SL / 200 DB / FARM — the s324 zoo);
  prints HY on some 27.5-life rows (map real property by LIFE).
- 🔧 ⚠ THE SURNAME REFLEX AT WRITE TIME (nine sessions); s318
  server/tests triage still holds three legacy instances.

## 🔎 Carried for triage — NOT claims (fresh s324 items first)
- **(s324) The four Tom holds** (⛔ list above) — the two GA-500 ones
  share the low-income-credit smell; decompose before assuming.
- **⚠⚠ (s324) RETRACTED — "23 silently lost packets" was MY ERROR; no
  packet was ever lost.** I tested Done\Tom's pre-existing packets
  against `StagedReturn` only, found none, and concluded unworked —
  then moved 23 FINISHED returns back into the work queue (duplicate-
  keying risk; reversed same session). The deep check (TaxReturn by
  client) shows **all 23 have 2 filed returns each**, as do the 5 that
  arrived later — they were worked through a route that creates NO
  StagedReturn row (hand-key/UI + mark-filed). The entry lane had
  EXPLICITLY flagged that blind spot; I acknowledged it and acted
  anyway. **THE LESSON: a check that can only observe ONE route proves
  nothing about the others — absence in one table is not absence
  (s291's evidence rule, re-learned the hard way).** Done\Tom = 186
  (158 backentry + 28 other-route); Inbox\Tom = 154. STILL OPEN, much
  smaller: notes-before-moves would have made both audits unnecessary
  (Ken's call for the Codex brief), and WHO works Tom's book by hand
  is worth knowing.
- **(s324) The 12 §179 Gail asset witnesses** re-emit on the next Gail
  re-extract (the allowlist fix landed AFTER gail-g4) — fold into the
  Sch E/F re-extract, don't re-run for this alone.
- (s323 carried): zero-activity GA-attach gap (3 witnesses) · §6654
  divergences (…7701/…7044) · 8615 pairs + second-state trio (…2036
  part-year GA hazard) · Tom census tail (41 unknown-page · 21 sch_e ·
  17 no-payer-page · 9 no-shell / 1099-R marker / ownerless — Codex's
  list) · the s321/s322 tail (K-1 Allowed · f7206 SEHI · alloc-wks
  …6559 · Sch-C line-3 gaps · 8863 reprints · GA LIC pair · FTC-205 ·
  the long tail in git history).

## ⛔ KEN DECISIONS OUTSTANDING — carried
Form 6765 Section G · 1040 v5.4 rules · 1120-S Inbox items · 17a/17d.

## RS AGENDA
With the states lane: R-GA500-DED Lacerte amendment + the GA 7b
unborn-dependent provision (engine legs build here when exports land).
Carried: S-15 / S-16 / S-19 · R-GA500-RIE · AL Form 40 · s306t
multi-employer-tips 4c.

---
**s324 deploy close-out:** verified live through `1d1efe13`
(dep-daavm1bl550s73etkm00). Nothing held. The Gail commit batch drains
in the background — its tally is the next annex.
