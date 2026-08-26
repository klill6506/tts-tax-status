# TTS Tax App — STATUS (current state only)

*⭐ s298 (2026-08-26 evening): **BUILD-QUEUE ITEMS ① AND ② SHIPPED, LIVE,
AND VERIFIED — three deploys.** ① **Per-vendor GA RIE split** (`a79f638a`,
deploy `dep-da7m63cs728c73br7tog`): `TaxReturn.import_vendor` (mig 0364,
db_default) set at back-entry commit — explicit `source.vendor` (closed
enum) > the Lacerte pdf_filename regex (`^(Partial Return for|Organizer
Forms-)`, the lane's 743/743 census) > blank; ⚠ the pinned
"from source.entry_method" design was REFUTED by the lane's census
('transcribed-from-pdf' on BOTH vendors — never re-derive it). Bucket
builder: lacerte splits each joint SOURCE ROW (largest-remainder, odd
dollar to TAXPAYER) via `split_conserving(tie_to=...)`; default keeps the
#78 line-aggregate fold. Backfill marked the committed Lacerte returns.
**Verified BOTH legs, two witnesses each**: client 2143 rolled-back probe
(joint interest 2,764→2,765 TP) + the lane's 1792 isolating dry-run
(NO_TIE −1/+1 → clean TIE; honest payload's residual now symmetric
97/96 = exactly the open §469(g) 193). 1792's ±1 GA exception RETIRED.
② **EIN backwards lookup** (`6c35e83f` + guard `a346139f`, deploys
`dep-da7mhcflk1mc7388bs80` / `dep-da7mlv9srm7s73bbregg`): shared matcher
(exact + ≥0.90 fuzzy; two-EINs-above-threshold refusal), commit-time fill
for W-2/1099-R (opt-out `ein_autofill:false`), `ein_source='employer_db'`
provenance (mig 0365) + D_EIN_DB_FILL review warning + serializer
clear-on-manual-edit, `fill_missing_eins` correction command (NOT a
seed_*). ⭐⭐ TWO GUARDRAILS grew inside the ruling's ambiguity clause:
(a) truncation-suspect names (Lacerte cuts at ~35 chars mid-word; ≥33 raw
or dangling single-letter token) fill on EXACT only — lane corpus intel;
(b) the SUBSTITUTION guard — the first dry run proposed "TEACHERS
RETIREMENT SYSTEM OF **GA**" ← the DB's "...OF **LA**" at 0.97 (sole
in-DB candidate, guardrail-blind): same-entity variants are
insert/delete-only, a character substitution = a different entity; any
'replace' opcode disqualifies. Guard deployed BEFORE the pass wrote.
**Correction pass RUN on prod**: 7 fills / 944 imported returns; 21
named-but-blank rows stay blank (truncation, as the lane predicted — the
D_EFILE_001 backlog does NOT clear on names alone). Gates all green
(20+20 new tests · 526 flow · 190 backentry+vendor · 46 W-2/ret serializer
· 77 RIE). Published schema regenerated TWICE (source.vendor enum +
ein_autofill); lane re-pulls before staging L021.*

*▶ NEXT (build queue, in order): ③ **the Form 8332 released-dependent
flag** (Ken s297e ruling 2 "Build it": per-dependent release — CTC/ODC
excluded on the releasing return, EIC/HOH retained per §152(e),
diagnostics demand the form; RS fact `dep_released_by_form_8332` is the
spec hook). ④ QBI item leg 2 (Rev Proc 2014-41 §5.03 in the iterative —
verify against the Rev Proc itself; the entry lane's 2137 re-run already
confirmed the residual is exactly §5.03-shaped). ⑤ the f8949 extractor
unit (re-measure the census first — s295/6/7: solo counts are upper
bounds). ⑥ §469(g) PTP release + Form 8990 1040-surface (specs in
BATCH-296; §469(g) now the ONLY thing holding 1792). ⛔ NEW for Ken
(REVIEW_QUEUE s298): the truncated-name PREFIX-MATCH tier + the lane's
organizer-name enrichment (coverage vs wrong-fill risk — the lane's ~21
held rows are the price of exact-only).*

*Peer state (s298 session): the STATES lane fixed check_ga500_integrity
themselves (RS `10f1146` — D-36 quantize, all 21 GA500 scenarios pass,
gate now pinned in pytest; DEQUEUED from our list). ⚠⚠ Their sweep of all
47 `check_*_integrity.py` gates: **35 pass, 12 FAIL** (`1040x`, `1065_se`,
`4562_recon`, `6252`, `k1_basis_704d`, `schedule_a`, `schedule_e_8582`,
`schedule_f`, `simplified_method`, `spine`, `topic8`, `topic9`) — NONE
were in pytest; staged for Ken as their S-10 with the re-diagnose-first
caution; take them a few at a time, each fix adding its gate to their
`tests/test_integrity_gates.py`. The ENTRY lane: L021+ will stage
`source.vendor`; ⚠ their annexed finding — a COMMITTED return cannot be
re-verified by plain stage+dryrun (HTTP 409, needs merge=replace) — the
in-place rolled-back recompute is the verification route for the 18
committed Lacerte returns. 2143's stored face still carries pre-clause
figures (refresh = deliberate lane/Ken act, never housekeeping).*

*⛔ KEN remaining: **the s298 REVIEW_QUEUE additions above** · the s295
int/div ruling (narrowed) · the entry lane's shell + birth-year packets ·
the 51-dependent DOB ask (`1040/Lacerte Inbox/DEPENDENT-DOB-ASK-
2026-08-26.md`) · #21, #48 (RS 404), #56, #63, #69, #10 — the tail tier.
Carried: entity second-state-face transport (#3);
`OVERRIDE_HONORED_STATE_LINES`; 146-packet re-export; NC/CA/SC
linked-state reopens; #6 1065X/AAR; #68 optimizer; s274 PII narrowings;
RS 8990 re-authoring gate; 6765 Sec G; client-4545 D_8606_BASIS_ONLY;
1065 BATCH-004 #4; Analysis line-2 active/passive proxy; the unfloored
8960 line5 §1211(b) question; tier-3 PII scrub (private code comments)
deferred to its own session; dep_released_by_form_8332 is now queue item
③, not just a REVIEW_QUEUE row.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS (WHEN BUILDS RUN)
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax` (srv-d6geloa4d50c73el2trg); demo = `tts-tax-demo`. ⚠ **A push
is not a deploy — CHECK THE DEPLOY STATUS after pushing** (API key in
`D:\dev\Passwords & Secrets\render-api-key.txt`). **Standing push
authorization (Ken 2026-08-23): push at own judgment; verify every deploy;
hold only for a named reason.** ⚠⚠ **ORDERING (s279/s282): push → deploy
LIVE → seed → verify — and the deploy ITSELF seeds (`build.sh seed_all`
auto-discovers `seed_*` at BUILD time).**
- s298 deploys (all API-confirmed LIVE 2026-08-26): `a79f638a` →
  `dep-da7m63cs728c73br7tog` (RIE vendor split, mig 0364 + backfill) ·
  `6c35e83f` → `dep-da7mhcflk1mc7388bs80` (EIN unit, mig 0365,
  D_EIN_DB_FILL seeded) · `a346139f` → `dep-da7mlv9srm7s73bbregg`
  (substitution guard).
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (s298 regenerated twice; current as of `a346139f`.)

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. s298 coordinated at boot:
both peers confirmed tree + test_postgres mine; all load-bearing exchanges
(entry_method refutation, truncation intel, 1792 verification, 409
finding) are annexed in BATCH-296. Peers stage; Ken decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s298)
- ⚠⚠ **12 of 47 RS `check_*_integrity.py` gates FAIL** (states lane sweep
  2026-08-26; list above) — none in pytest until their `10f1146` started
  the pin; staged for Ken (their S-10). Re-diagnose before inheriting.
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s296; s297/s298 touched no client code).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): five phantom failures. New files/markdown/scratchpad only.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c`; a script run by absolute path needs
  `sys.path.insert(0, server)` (s298). ⚠ `python -m taxwise1040` does NOT
  resolve from `server\` — run the package's `__main__.py` BY PATH (s297).
- 🔧 ⚠ **`BaseModelSerializer` partial updates save ONLY the patched
  columns** (s298) — a serializer-side derived write must be injected into
  `validated_data`, never `setattr` on the instance (it silently no-ops).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ Embedded double-quote in a here-string
  arg to a NATIVE exe SPLITS the argument. ⚠ `$1` in an unquoted PS arg
  EXPANDS to empty. ⚠ PS `Sort-Object -Unique` on a one-element pipeline
  UNWRAPS to scalar (s296). ⚠ This machine's Git Bash lacks `cat` on PATH
  (s297) — but `python - <<EOF` heredocs work; use them for file appends.
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash. ⚠ A
  COMMITTED return refuses plain stage+dryrun with HTTP 409 (s298 —
  dry-run is a full commit in a rollback; verification route = the
  in-place rolled-back recompute, s289 pattern).
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); synthetic `insert_text` PDFs may
  LOSE leading spaces (s290) — the extractor parses POSITIONALLY everywhere.
  ⚠ **The AcroForm FILLER flattens ALL widgets out of its output** (s296).
  ⚠ **TaxWise RE-TYPESETS schedule templates** (s297): packet marker
  geometry ≠ the blank IRS template's — pin from packets only.
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section. Local templates beat both.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292-s298 ran probes this way). ⚠ Scripts touching
  client-named returns live in SCRATCHPAD or tax-test-data, never the repo
  (PII). ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295/s296/s297).
  The FACE is the guard; the emitted-set tie probe finds silent-no_tie.
- 🔧 ⭐ **DRY-RUN THE CORRECTION PASS AND READ EVERY ROW before --commit**
  (s298): the first EIN dry run proposed a wrong-entity fill (GA←LA, 0.97)
  that no automated guardrail could see; reading 8 lines caught it.

## 🔎 Carried for triage — NOT claims
- (s298) 21 named-but-blank W-2/1099-R rows on imported returns have no
  unambiguous EIN match (mostly Lacerte truncations) — held on
  D_EFILE_001 by design; movement waits on Ken's prefix-tier /
  organizer-enrichment call (REVIEW_QUEUE).
- (s297) The X mark at (474.7, y≈389) in the 1040 p2 EIC row region —
  unidentified, parser-ignored; identify on an EIC face divergence.
- (s297) `est_payments_wks` (27) and `f8863` (6) gate at the face (26/29)
  — when their page-parsers land, the face gates become consistency checks.
- (s296) The 22 sch_d GEOMETRY-error packets refuse loudly by design.
- (s295) The Inbox holds 7 auxiliary PDFs that refuse as "not a TaxWise
  1040 packet" — correct; a sub-folder would clean the census denominator.
- (s295) `_summary_lines` GA500_SUMMARY_LINES lacks S1-6.
- (s290) The GA RIE interest row excludes K-1 16A tax-exempt interest —
  stated boundary. · The rendered 8995 TIN prints unformatted — cosmetic.
- (s289) `IndividualForm7203`: no §179/charitable carryover keying
  (D_K1_7203_DEDUCTION_LIMITED warns); the §179 cap doesn't extend to 1065.
- (s288) `IndividualForm7203` has no home for box 16 code E; 1065 box 18
  a/b/c none recipient-side. · `ctc_override`/`odc_override` +
  `Dependent.compute_qualifies_*` are DEAD — removal candidate (Ken).
- (s287) The 8825 line-1 repaint covers the LINE-1 table only. · The
  suggested-field convention covers W-2 3/5 + 1099-R box 16 — CLAUDE.md's
  W-2-only note is stale.
- (s285) Sch 4 nonresident arm still apportions the whole widened base.
- (s283) The stamp excludes 1040 packets (name+SSN privacy — Ken).
- (s282) `OVERRIDE_HONORED_STATE_LINES` hand-synced.
- (s281) OOS-state line-18 prompt diagnostic specified, not built. ·
  Stage allowlists `schd_fields` keys, `ga500_fields` not at all.
- (s268) 1,604 queries/run + memoization candidates.
- (s241/s281) `Form8606` unique-constraint candidate · 🔴 `HSAAccount`
  half CLOSED.
- (s275/s281) `.first()`-on-per-form-rules sweep remainder.
- (s289) K-1 capital gains reach Schedule D but not the L9 gain/loss
  WEIGHTS — pre-existing. ⚠ NOTE s298: the L9 path also keeps the #78
  aggregate convention under BOTH vendors (stated boundary in the vendor
  annex) — a Lacerte column-level L9 divergence is a NEW item.
- (s294) A state face left by an omitting correction batch is NOT
  recomputed against that batch's federal changes — visible via the
  live-face reconciliation + the named warning.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried:
Everything from s277–s297 stands (R-K1-20N; R-GA700-PARTNERS; AL_FORM_40NR
amendments; R-AL-TAX; R-B1/B2-AUTO; the 4562 line-17 reversal;
R-8880-LINE1-COMPOSE; FORM_7203 Part I line-3 sub-map; R-K1-179-BASIS;
SCHEDULE_K1 box 16 A/B routing; R-8995-QBI line-1 population widening;
FORM_8960 R-8960-INCOME 5b description; the unfloored line5 §1211(b)
question; the 1040X derived-input amendment + queued
`x_is_superseding_derived` app follow-up; R-SE-L2 clergy +
SE-subject-other-income addends — verification NOT started; R-5329-11
staged for Ken with the states lane's research doc). ✅ R-GA500-RIE's
vendor clause is now IMPLEMENTED (s298) — spec and engine tell one story.
