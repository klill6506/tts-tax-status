# TTS Tax App — STATUS (current state only)

*⭐ s301 (2026-08-26 night): **BUILD-QUEUE ITEM ⑤ SHIPPED — the f8949
extractor unit** (`d6ab0e42`, deploy `dep-da7ovk6417fc738ffrq0`,
API-confirmed LIVE). The census was RE-MEASURED FIRST (r15: identical to
r14, 18/267 — the corpus growth was all Lacerte-side; f8949 confirmed top
solo class at 9). New `scripts/taxwise1040/f8949.py`: per-page box X,
transaction grid on fixed right-aligned column edges, (f) code letters,
totals row; refusal-grade identities (h=d−e+g per row, totals=Σrows,
unknown/missing codes, Y/Z/Q refuse by name). Date masks VA/RI/OUS →
VARIOUS, IN/HE/RIT → INHERITED; negatives print MINUS, never parens.
Rows emit as `capital_transactions` (owner=joint on MFJ — compute-inert
metadata). The s296 Sch D 1b/2/3/8b/9/10 box-total REFUSAL became a
both-directions to-the-dollar CROSS-CHECK. Two sch_d parser fixes the
coverage short-circuit had hidden: COL_G_X (436,486)→(440,503) — grid
(g) right-aligns ending ~x501, silently MISSED before — and marker
scan now excludes grid-anchor rows (small (g) x0 lands in the marker
band; two live packets). Calibration: 190 pages / 300 rows, ZERO
geometry failures. r16 = **19 emitted / 267**; ⚠ the upper-bound lesson
fired a FOURTH time (solo 9 → 1 freed; the other 8 fall to
pre-existing downstream gates the short-circuit had hidden: line-20
Sch 3 face gate ×5, MFJ ownerless int/div ×4, one 1099-R 5b
decomposition, one SchB payer-less exception — all named). 18 prior
payloads byte-identical (zero drift); the 1 new emit tie-verified in a
rolled-back instrumented commit (verdict tie, every fed+GA+RIE line
delta 0). 14 new tests, extractor suite 122 green.*

*⭐ s301b (same session, second deploy): **the e8960 back-entry surface**
(`03c96795`, deploy `dep-da7p46jl550s73cgj02g` — VERIFY LIVE at next
boot if this line is stale; build was in progress at close). BATCH-296's
new entry-lane item, verify-first confirmed a pure allowlist omission:
TAXPAYER_FIELDS gains the EIGHT additive 8960 facts (3/4b/5b/6/7 +
9a/9b/9c); the FOUR overrides (interest/dividends/net_gain_override +
e8960_rental line 4a) stay deliberately un-importable (masking; annex
documents the boundary + the f8880/f2441 derive-shadow path if ever
needed). Proven on the item's own fixture (client 1723, staged
5cf51e1c…) in a rolled-back commit: 9b=120 closes the 5-delta, verdict
tie, federal 24=50,516. Published schema + SUPPORTED-SECTIONS.md
REGENERATED (current as of 03c96795). 4 new tests + staging suite 96
green. Annex appended in BATCH-296; entry lane told to re-stage 1723.*

*▶ NEXT (build queue, in order): ⑥ §469(g) PTP release + Form 8990
1040-surface (specs in BATCH-296; §469(g) the ONLY thing holding client
1792). ⑦ CANDIDATE (new, entry-lane measured 2026-08-26): **the
general-category Form 1116** — D_1116_003 refuses 4 of the 12
1116-carrying packets in the 41-packet Lacerte set, and client 2303 is a
9,893 credit = the ENTIRE liability (packet not enterable at all); the
other two are small (424 / 177). Measured priority signal, not a new
defect — the limitation is declared; sits after ⑥ unless Ken reorders.
Extractor follow-ons by measured residual: the line-20 Sch 3 face
class (5 packets — needs a sch3 parser or valve) > MFJ ownerless int/div
(4, needs an ownership source beyond the alloc worksheet) > the 1099-R
5b-decomposition packet (one, probe the 100,000 discrepancy) > SchB
payer-less-row exception (one).*

*Peer state (s301 session): states lane confirmed at boot I hold tree +
test_postgres (they checked the live roster first — only live delvio-tax
session). Their 8949-chain integrity check: `8949` / `SCHEDULE_D` /
`1040_SCHD_WS` all CLEAN (their earlier "no SCH_D form in RS" claim
RETRACTED — wrong lookup key, the real code is SCHEDULE_D). ENTRY lane
mid-run on the 41 DOB-unblocked Lacerte packets (4 committed+filed: 1303,
2049, 1464, 2445; 1723 held on the e8960 gap — NOW UNBLOCKED, told to
re-stage; 2236 held on an unprinted W-2 box-12 code; 2303 NOT ENTERABLE
on D_1116_003 general-category). They write inside `1040/Lacerte Inbox/`
— ⚠ 379 Organizer PDFs + 255 Partial Returns live THERE (different
document class; the TaxWise `1040/Inbox` is clean, all 344 are packets).*

*⛔ KEN remaining: the s298 truncated-name PREFIX-MATCH tier +
organizer-name enrichment (REVIEW_QUEUE) · S-14 (SEHI §5.01-vs-§5.02
method, three facets, states lane staged) + the s300b D_8962_NOCONVERGE
provenance note · S-11/S-12 dependent-chain · the s295 int/div ruling
(narrowed) · the 51-dependent DOB ask (`1040/Lacerte Inbox/DEPENDENT-
DOB-ASK-2026-08-26.md`) · the 1723 GA Eligible-Itemizer question
(BATCH-296 ⛔ FOR KEN — filed return skipped a $600 credit the engine
derives under Ken's own 2026-08-23 ruling; is Lacerte applying an
eligibility condition we don't model?) · #21, #48 (RS 404), #56, #63,
#69, #10 tail. Carried: entity second-state-face transport (#3);
`OVERRIDE_HONORED_STATE_LINES`; 146-packet re-export; NC/CA/SC
linked-state reopens; #6 1065X/AAR; #68 optimizer; s274 PII narrowings;
RS 8990 re-authoring gate; 6765 Sec G; client-4545 D_8606_BASIS_ONLY;
1065 BATCH-004 #4; Analysis line-2 active/passive proxy; the unfloored
8960 line5 §1211(b) question; tier-3 PII scrub deferred; the 47
RS-integrity-gate sweep (12 FAIL, states lane S-10).*

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
- s301 deploys: `d6ab0e42` → `dep-da7ovk6417fc738ffrq0` (f8949 extractor
  unit, scripts+tests only, no migration, no seeds) — **API-confirmed
  LIVE 2026-08-26**; `03c96795` (s301b, e8960 allowlist + tests, no
  migration, no seeds) → `dep-da7p46jl550s73cgj02g` — build_in_progress
  at close, **VERIFY at next boot**.
- s300 deploys: `5f890ef5` → `dep-da7o66mk1f9s73cf81bg` · `87f4f329`
  (s300b, D_8962_NOCONVERGE) → `dep-da7ocvbncjis73bp22sg` — both
  API-confirmed LIVE 2026-08-26.
- s299 deploy: `5d1d683e` → `dep-da7nhgajobas739gb800` (8332 unit) — LIVE.
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (s301b regenerated; current as of `03c96795`.)

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. s301 coordinated at boot:
both peers confirmed tree + test_postgres mine; the e8960 annex is in
BATCH-296 (load-bearing exchange annexed). Peers stage; Ken decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s301)
- ⚠⚠ **12 of 47 RS `check_*_integrity.py` gates FAIL** (states lane sweep
  2026-08-26) — staged for Ken (their S-10). Re-diagnose before inheriting.
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s296; s297–s301 touched no client code).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): five phantom failures. New files/markdown/scratchpad only.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c` (bit again s301); a script run by absolute
  path needs `sys.path.insert(0, server)` (s298). ⚠ `python -m taxwise1040`
  does NOT resolve from `server\` — run the package's `__main__.py` BY
  PATH (s297).
- 🔧 ⚠ **`BaseModelSerializer` partial updates save ONLY the patched
  columns** (s298) — a serializer-side derived write must be injected into
  `validated_data`, never `setattr` on the instance (it silently no-ops).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ Embedded double-quote in a here-string
  arg to a NATIVE exe SPLITS the argument. ⚠ `$1` in an unquoted PS arg
  EXPANDS to empty. ⚠ PS `Sort-Object -Unique` on a one-element pipeline
  UNWRAPS to scalar (s296). ⚠ Git Bash heredoc appends WORKED s301 (twice)
  — the s297 "lacks cat" note may be stale; still verify after each append.
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash. ⚠ A
  COMMITTED return refuses plain stage+dryrun with HTTP 409 (s298 —
  verification route = the in-place rolled-back recompute, s289 pattern;
  s301 ran two such probes clean, incl. one against a peer's staged row
  amended in-memory).
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); synthetic `insert_text` PDFs may
  LOSE leading spaces (s290) — the extractor parses POSITIONALLY everywhere.
  ⚠ **The AcroForm FILLER flattens ALL widgets out of its output** (s296).
  ⚠ **TaxWise RE-TYPESETS whole schedules** (s297): packet marker
  geometry ≠ the blank IRS template's — pin from packets only. ⚠ s301:
  a fixture that encodes the WINDOW's geometry instead of the CORPUS's
  keeps a blind spot green (the sch_d (g)-column tests placed values at
  x1=486 because that's where the window ended; the corpus prints ~501).
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section. Local templates beat both.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292–s301 ran probes this way). ⚠ Scripts touching
  client-named returns live in SCRATCHPAD or tax-test-data, never the repo
  (PII). ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295/6/7 +
  s301 FOURTH confirmation: the coverage gate SHORT-CIRCUITS — everything
  downstream of it is unmeasured until the gate opens; 9 → 1).
- 🔧 ⭐ **DRY-RUN THE CORRECTION PASS AND READ EVERY ROW before --commit**
  (s298).

## 🔎 Carried for triage — NOT claims
- (s301) One packet's 5b decomposition: extracted pension rows carry
  115,150 vs face 15,150 — a suspicious exactly-100,000 discrepancy;
  probe the 1099-R parse before assuming a rollover/exclusion.
- (s301) One packet's Sch D carries 1b grid totals with NO f8949 pages +
  its own h-identity break — suspected misparse, refuses on 3 grounds.
- (s301) A blank printed 8949 page (seen once, on a synthetic test
  packet) refuses "no transaction rows" — over-refusal by design.
- (s298) 21 named-but-blank W-2/1099-R rows have no unambiguous EIN match
  — held on D_EFILE_001; movement waits on Ken's prefix-tier call.
- (s297) The X mark at (474.7, y≈389) in the 1040 p2 EIC row region —
  unidentified, parser-ignored. · `est_payments_wks` (27) and `f8863` (6)
  gate at the face.
- (s296) The 22 sch_d GEOMETRY-error packets refuse loudly by design.
- (s295) 7 auxiliary Inbox PDFs refuse as non-packets — correct. ·
  `_summary_lines` GA500_SUMMARY_LINES lacks S1-6.
- (s290) GA RIE interest row excludes K-1 16A tax-exempt interest —
  stated boundary. · The rendered 8995 TIN prints unformatted — cosmetic.
- (s289) `IndividualForm7203`: no §179/charitable carryover keying;
  the §179 cap doesn't extend to 1065. · K-1 capital gains reach Sch D
  but not the L9 gain/loss WEIGHTS (⚠ s298 note: also the #78 aggregate
  convention under BOTH vendors).
- (s288) `IndividualForm7203` no home for box 16 code E; 1065 box 18
  a/b/c none recipient-side. · `ctc_override`/`odc_override` +
  `Dependent.compute_qualifies_*` are DEAD — removal candidate (Ken).
- (s287) 8825 line-1 repaint covers the LINE-1 table only. · Suggested-
  field convention covers W-2 3/5 + 1099-R box 16 (CLAUDE.md note stale).
- (s285) Sch 4 nonresident arm still apportions the whole widened base.
- (s283) The stamp excludes 1040 packets (Ken). · (s282)
  `OVERRIDE_HONORED_STATE_LINES` hand-synced. · (s281) OOS-state line-18
  prompt diagnostic specified, not built; stage allowlists `schd_fields`
  keys, `ga500_fields` not at all. · (s268) 1,604 queries/run. ·
  (s241/s281) `Form8606` unique-constraint candidate · 🔴 `HSAAccount`
  half CLOSED. · (s275/s281) `.first()`-on-per-form-rules remainder. ·
  (s294) a state face left by an omitting correction batch is not
  recomputed against that batch's federal changes.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried:
Everything from s277–s300 stands (see STATUS_ARCHIVE s300 entry for the
full list: R-K1-20N; R-GA700-PARTNERS; AL_FORM_40NR; R-AL-TAX;
R-B1/B2-AUTO; 4562 line-17 reversal; R-8880-LINE1-COMPOSE; FORM_7203
Part I line-3; R-K1-179-BASIS; SCHEDULE_K1 box 16 A/B; R-8995-QBI
line-1 widening; FORM_8960 R-8960-INCOME 5b description; the unfloored
line5 §1211(b) question; 1040X derived-input amendment; R-SE-L2 clergy;
R-5329-11; S-11/S-12 R-DEP-03 release branch; S-14 R-8962-SEHI §5.03 +
method choice). NEW (s301): none — the 8949 spec was verified live and
implemented verbatim (R-8949-BOX/CODES/TOTALS); no amendment needed. The
states lane's SCH_D-gap claim was retracted (the RS code is SCHEDULE_D;
whole 1040 capital chain integrity-clean per their re-check).
