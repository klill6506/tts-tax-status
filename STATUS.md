# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s324 (mid-flight if the Gail batch is draining), 2026-08-31 late night

**⚠⚠ FIRST ACTION AT BOOT: check the GAIL commit batch**
(`tmp\commit_s324_gail131.txt` under tax-test-data\1040): 132 gail-g4
emits committing LIVE (batch key `s324-gail-commit-001`, 2/132 at this
writing, zero holds; per-return transactions — a kill is safe). If no
SUMMARY line: count `COMMITTED`, rebuild the remainder from the gail-g4
emit list minus landed (fresh batch key `-002`; the 409 fence reports
HELD for already-landed — skip in the tally). Then post the tally to
the entry lane + a BATCH-296 annex. Landed Gail packets do NOT move
(Gail's originals stay in `Inbox\Gail`; the merged copies in
`tmp\merged-gail-v5\Gail` are the pipeline artifacts).

**s323 boot actions: ALL DONE.** ① Tom batch FINISHED: **158/162
tie-committed** (96 s323-001 + 62 s324-003); 4 holds (below). Packet
accounting AIRTIGHT after the Done\Tom audit (see Carried): Done\Tom =
158 = exactly the committed set; Inbox\Tom = 182 = 4 holds + the
178-refusal work-list; 23 silently-parked uncommitted packets found
and RESTORED to Inbox. ② Migration 0014 applied (had to precede
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

**▶ NEXT:** ① finish/verify the Gail commit batch (boot action) ·
② **the Schedule E + Schedule F extractor legs** (the activity-depth
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

**⛔ WAITING ON KEN:** ① the 61 Gail federal reprints
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
- 🌐 ⚠⚠ a feature can pass tests+typecheck and ship as TREE-SHAKEN DEAD
  CODE — the routed-page mount test + the deployed-bundle grep are the
  gates (s324, the dba build).
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
- **(s324) RESOLVED-BUT-OPEN: 23 packets were silently parked in
  Done\Tom uncommitted** (all A–C — an unrecorded worker's trail;
  entry-lane audit + build-lane StagedReturn check; exactly one
  pre-existing Done packet was legitimately committed). ALL 23 restored
  to Inbox\Tom; the book is airtight (Done 158 = the committed set;
  Inbox 182 = 4 holds + 178 refusals). Names in the BATCH-296 annex.
  OPEN: whose hand — and the Codex-brief amendment (notes BEFORE
  moves) is Ken's call.
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
