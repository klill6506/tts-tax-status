# TTS Tax App — STATUS (current state only)

*⭐ s300 (2026-08-26 night): **BUILD-QUEUE ITEM ④ SHIPPED — the Rev Proc
2014-41 §5.03 limit inside the SEHI↔PTC iterative** (`5f890ef5`, deploy
`dep-da7o66mk1f9s73cf81bg`; BATCH-296 QBI-item leg 2). Verified verbatim
from rp-14-41.pdf (downloaded + extracted): §5.03(2) caps the §162(l)
deduction at the LESSER of (A) earned income (already in the iterative's
cap) and **(B) (specified premiums − APTC) + the §1.36B-4T(a)(3)(iii)
limitation on additional tax — which was ABSENT from the loop entirely**.
New `sehi_503_limit()` (lowest-tier-first test; test household deducts
unpaid premiums + candidate limitation + non-specified §162(l); None when
APTC=0 or no tier — the limb provably collapses into premiums−PTC);
ceiling applied at Step 1 AND every Step-3/5 pass; Step-6 convergence
aligned verbatim (BOTH deduction and PTC must move < $1; was
deduction-only ≤ $1). MeF read-side bridge shares the pure function.
**LEG-2 VERDICT: on client 2137 the limit reproduces the packet's Pub 974
worksheet exactly (line 5=375, line 6=971, tentative PTC 5,363) but NEVER
BINDS** — the 19 residual is Lacerte's §5.02 ALTERNATIVE method (624) vs
our §5.01 ITERATIVE (643, Ken decision 2 / R-8962-SEHI), both sanctioned
by Rev Proc §3; bounded vendor-method divergence, pinned both ways in
`test_8962_sehi_503_limit.py`. The entry lane's AGI-closes prediction
REFUTED and told. In passing: the 8962 seed-leg test red since s277
(EXPECTED_LINES 42 vs seeder 43 SEHI-BASE) fixed. Gates: 11 new tests (3
injection-proven red) · flow + 8962 suites 583 · seed 13 ·
MeF/backentry/W-2 272 — all green. No migration, no client code, no
schema change. Annexed in BATCH-296. **s300b same session
(`87f4f329`, second deploy): D_8962_NOCONVERGE (error)** — the states
lane's separation argument (fallback waits on S-14; VISIBILITY is
method-independent) + the s240 fix-the-silence precedent:
`sehi_ptc_iterative` now returns a converged flag, the read side gains
`iterative_sehi_converged`, and a return whose iterative exhausts 25
iterations with a Step-6 delta ≥ $1 errors by name instead of silently
keeping the last iterate. Proven on a constructed 300%-FPL-boundary
two-cycle; injection-proven; 683 green.*

*▶ NEXT (build queue, in order): ⑤ the f8949 extractor unit (RE-MEASURE
the census first — s295/6/7: solo counts are upper bounds). ⑥ §469(g)
PTP release + Form 8990 1040-surface (specs in BATCH-296; §469(g) now the
ONLY thing holding 1792). ⛔ Carried for Ken (REVIEW_QUEUE): the s298
truncated-name PREFIX-MATCH tier + organizer-name enrichment; NEW s300
FYI — the §5.02 alternative-method Lacerte-parity option (one-line ruling
if ever wanted; decision 2's iterative stands, no recommendation).*

*Peer state (s300 session): both peers confirmed tree + test_postgres
mine at boot (delvio-states-aa checked the live roster first — the s299
lesson landed). STATES lane: amended R-SE-L2 (clergy addend — spec now
describes the engine; NO engine change; the 8z SE-subject addend stays
deliberately un-bundled per Ken — do not "fix"); their
declared-input-absent-from-formula audit is RETRACTED (fires on 921/1407
— do not build; the working cut is fact-key-resolution → 64 dangling,
needs hands). R-8962-SEHI §5.03 amendment RELAYED, verified by
them against rp-14-41.pdf independently, and staged as **S-14**
(`b0fe36d`) — they ESCALATED the method choice to the headline question
(§5.01 iterative vs Lacerte's §5.02 = a knowing $19-class divergence
from filed returns: keep / follow vendor / electable — plus the
non-convergence fallback the Rev Proc prescribes and our loop lacks, and
the entry lane's no-fault-disposition question; three facets, one Ken
ruling — see REVIEW_QUEUE s300 + DEFERRAL_AUDIT s300). ENTRY lane: mid-run on the 41
DOB-unblocked packets (2 committed+filed at boot); L021+ stages
`source.vendor`; ⚠ their new findings — a committed return re-verifies
via stage+dryrun IF `-Merge replace_documents` is passed to the DRY-RUN
too (confirmed client 2049; tax_return_id PRESERVED, markfiled then
skips); part-year GA works (residency part_year + S3-5/6/7 A/C columns);
`gen_backentry_schema.py` writes DIRECTLY into their tree (Import
Templates) — a failed regen leaves them stale silently; D_6251_008 fires
on qualified-dividends-only (no Sch D) — observation, not defect. The 47
integrity-gate sweep (12 FAIL) stays staged for Ken as their S-10.*

*⛔ KEN remaining: **PROVENANCE NOTE on s300b (states lane recorded it in
S-14, agreeing it belongs in front of you): D_8962_NOCONVERGE shipped
WITHOUT an individual ruling, on the standing s240 fix-the-silence
precedent** (an error diagnostic naming a condition true under every
S-14 outcome; no tax-law choice beyond severity). If the precedent
covers it, this line is provenance only; if not, downgrade/retire it in
the S-14 ruling (is_active=False + kept stub per s266). ·
**the s298 REVIEW_QUEUE additions above** · the s295
int/div ruling (narrowed) · the entry lane's shell + birth-year packets ·
the 51-dependent DOB ask (`1040/Lacerte Inbox/DEPENDENT-DOB-ASK-
2026-08-26.md`) · #21, #48 (RS 404), #56, #63, #69, #10 — the tail tier.
Carried: entity second-state-face transport (#3);
`OVERRIDE_HONORED_STATE_LINES`; 146-packet re-export; NC/CA/SC
linked-state reopens; #6 1065X/AAR; #68 optimizer; s274 PII narrowings;
RS 8990 re-authoring gate; 6765 Sec G; client-4545 D_8606_BASIS_ONLY;
1065 BATCH-004 #4; Analysis line-2 active/passive proxy; the unfloored
8960 line5 §1211(b) question; tier-3 PII scrub (private code comments)
deferred to its own session. ✅ dep_released_by_form_8332 BUILT s299
(queue item ③ closed) — its REVIEW_QUEUE seed can be marked built.*

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
- s300 deploys: `5f890ef5` → `dep-da7o66mk1f9s73cf81bg` (§5.03 iterative
  limit, no migration, no seeds) — **API-confirmed LIVE 2026-08-26**;
  `87f4f329` (s300b, D_8962_NOCONVERGE — rule row seeded at build) →
  `dep-da7ocvbncjis73bp22sg` — **API-confirmed LIVE 2026-08-26**.
- s299 deploy: `5d1d683e` → `dep-da7nhgajobas739gb800` (8332 unit, mig
  0366 + five new rule rows seeded at build) — **API-confirmed LIVE
  2026-08-26**.
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
NEW (s299): **R-DEP-03's formula should spell out the §152(e) release
branch** — the spec lists `dep_released_by_form_8332` as an input but the
formula text never states the mechanics; the engine implements both
sides. The states lane VERIFIED and staged it for Ken as **S-11** (their
finding sharpens it: R-DEP-03 read literally is INVERTED on both sides
of a release — spec/engine divergence, engine correct; use the
instructions' "treated as" language, and note the four-condition scope +
the notes' dropped dependent-care-benefits exclusion) and as **S-12**
(SCH_8812 R002 still consumes the RETIRED key `dep_qualifies_ctc` where
R-DEP-03 now emits `dep_ctc_qualifying` — the D-43 rename updated the
producer, left the consumer; spec-only, compute_8812 correct). Both are
Gate-1 — Ken's call; worth ruling together (same dependent chain).
NEW (s300): **R-8962-SEHI → states-lane S-14** (`b0fe36d`, staged +
independently verified against rp-14-41.pdf): the formula is wrong on
two counts (deduction-only ≤$1 vs the Rev Proc's both-deltas <$1 —
engine FIXED s300, spec not), missing the §5.03 limbs (engine implements
s300), missing the non-convergence §5.02-fallback branch (NEITHER
implements — live boundary, DEFERRAL_AUDIT s300), `outputs: []` while
feeding Sch 1 line 17; authority row add Rev Proc 2014-41 §§5.01/5.03.
HEADLINE facet: the §5.01-vs-§5.02 method choice (Lacerte files §5.02 —
a knowing $19-class divergence from filed returns: keep / follow vendor
/ electable) + the entry lane's no-fault disposition question
(REVIEW_QUEUE s300) — one Ken ruling, three facets.
