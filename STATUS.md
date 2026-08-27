# TTS Tax App — STATUS (current state only)

*⭐ s306 (2026-08-27, build lane): **the HSA excess detector could not see an
employer-only over-contribution** — three BATCH-296 entry-lane items in one
pass. The headline is that `D_8889_EXCESS` **already existed and already ran**:
its condition was `line 2 (own contributions) > line 13 (the deduction)`, the
taxpayer's own excess and nothing else. Per **i8889** the employer's excess is a
different quantity against a different comparand — *"the excess, if any, of your
employer's contributions over your limitation on **line 8**"* — and **i5329 line
47** is the SUM of the two, *"Also include on line 47 any excess contributions
your employer made."* When the over-contribution is entirely the employer's,
lines 2, 12 and 13 are ALL zero, so the old condition was **structurally
incapable of firing**. Both quotes verified verbatim from the downloaded 2025
instruction PDFs (the states lane independently pulled both and confirmed them).
**Prod census: 70 HSA rows, 6 carry an excess, ALL 6 employer-only, old
condition fires on ZERO.** 3 keyed and agreeing (429 · 1,000 · 2,700 — three
independent witnesses for the formula), 3 blank at 1,000 each. ⚠ **The $180 of
missing excise is an UPPER BOUND, not a measured loss** — the excise does not
apply to a timely-withdrawn excess and the return does not record withdrawal.
Shipped: `excess_contributions()` in `compute_8889.py` (reads `owner_lines`, so
diagnostic and compute cannot drift) + the rule rewritten to name the total, its
composition, the missing excise, where to key it, and the i8889 *"report it as
Other income if it was not in W-2 wages"* consequence. **Two deliberate calls,
both evidence-based rather than taste: it stays a WARNING** (back-entry commit
gates on ZERO error-severity findings and errors are not acknowledgeable —
`backentry.py` Gate 4 — so an error would hard-block returns whose excess was
properly withdrawn, against Ken's "warn, never hard-block"), and **line 47 is NOT
auto-derived** because `Form5329.hsa_curr_excess` is `default=0` **non-nullable**,
so a keyed zero is indistinguishable from an unkeyed field and there is no
off-switch for the withdrawal case (s237 class). Silent when line 47 already
states the derived amount. Verified read-only over every prod HSA return: fires
on exactly the 3 blank, silent on the 3 keyed.*

*s306 items ② and ③: **the two opposite sign conventions** now carry
descriptions that each name the other —
`int_1099s[].accrued_interest_adjustment` is a POSITIVE MAGNITUDE the engine
subtracts (keying it negative overstates by exactly 2×; the entry lane's tell,
*"a delta that is exactly 2× an input means an inverted sign, not a missing
figure"*, is recorded in the field itself), while
`taxpayer.qbi_loss_carryforward_prior` carries its own sign;
`form_5329s.hsa_curr_excess` gained one too. **③ the 4547 enum casing was FIXED,
not documented** — staging upper-cases `form_4547s[].children[].relationship` in
place (the MeF `ChildRelationshipCd` enum is UPPERCASE while
`dependents[].relationship` on the SAME payload is lowercase), so `"son"` is
accepted and `"cousin"` still refuses. Narrow by design: normalising every enum
generically would change behaviour for lowercase sections. Published back-entry
schema regenerated.*

*⚠⚠ **s306's OWN LESSON — A COUNT TAKEN AGAINST A LIVE CORPUS IS A TIMESTAMP,
NOT A FACT.** My census read 68 rows / 5 excess / 2 keyed; a verification run 40
minutes later read 70 / 6 / 3 — same query, same firm. **Both wrong moves were
available**: quote whichever run I happened to have first, or treat the
difference as a defect and go hunting. Reconciling at ONE grain before quoting
either is what turned an apparent contradiction into a stronger item. The cause
is now MEASURED, not inferred, because the entry lane held the other half of the
evidence and supplied it on request: both new rows are theirs and both are NEW
RETURNS rather than rows changing state — one with 5,825 employer contributions
against a 9,550 limit (**no excess**, which is why rows moved 68→70 while excess
moved only 5→6) and one with 8,979 against 8,550 (**the 429**). The
decomposition is exact. ⭐ Splitting MEASURED from INFERRED *before* handing the
number on is the S-18 discipline applied before it cost anything.*

*⛔ **KEN — NEW: S-19** (states lane, staged): `R-8889-EXCEPTIONS` states the
same narrow `line 2 > line 13` condition the code had, and its diagnostic
message still says the 6% excise is *"not computed here"* — stale, since Form
5329 Part VII computes it from line 47. Spec and engine should tell one story.
⚠ The states lane checked and DISSOLVED a third suspected defect in the same
rule (i5329 names line 12 where the spec compares line 13, but `line 13 = min(
line 2, line 12)` makes those arithmetically equivalent) — recorded so a false
finding cannot be used to argue the true one down.*

*🔎 **The "not keyed vs not obtainable" class reaches its THIRD instance**
(entry lane, s306): `D_5329_003` holds a packet because the 12/31 HSA account
value is blank — and that value lives on the year-end HSA statement, which a
Lacerte partial-return print does not carry, so the hold cannot be cleared by
further reading. ⭐ It is the cleanest instance of the class because **the
direction of error is known and one-way: `hsa_value` can only ever REDUCE the
tax**, so a guess could silently under-report while its absence cannot make the
answer wrong (that return charged the full excise and ties). The other two are
`D_EFILE_001`'s two EIN holds, where a wrong guess is undetectable downstream.
That asymmetry is the argument for allowing a documented-absence designation on
the 5329 hold *before* the EIN ones.*

*🔴 **s306b (same session, LIVE REGRESSION found while Ken was blocked by it):
the Add W-2 button had been 400ing on EVERY return.** Reported through the entry
lane as "adding a W-2 throws a 404 / the route is missing". It was neither.
**s287 (Ken item 11, "no placeholder name") changed the button to POST
`{employer_name: "", wages: "0.00", federal_tax_withheld: "0.00"}`; nothing
relaxed the server, where `W2Income.employer_name` was `CharField(max_length=255)`
with no `blank=True`** — so DRF answered `400 {"employer_name":["This field may
not be blank."]}` to the exact payload the UI sends, for everyone, on every
return. **Fix: `blank=True, default=""` + mig 0370** (`ec00bcb3`). A nameless row
is a legitimate INTERMEDIATE state (add the row, then type the name) and cannot
reach a filed return — the MeF extract already refuses a W-2 with no employer
name BY NAME and the diagnostics already render it "employer not named", so
completeness stays at those gates rather than a create-time 400 that blocks data
entry outright. 5 new tests + 84 adjacent (W-2 / EIN-autofill / e-file extract).
**Deploy `dep-da8b65tbedkc73agspg0` (prep) + the demo twin, BOTH API-confirmed
LIVE on `ec00bcb3`. VERIFIED FUNCTIONALLY, not just deployed:** a POST of the
exact UI payload to the real route returned **201** with `employer_name=''`,
followed by a 204 cleanup DELETE. ⚠ That live POST was run against **DEMO, not
prep** — proving it needs a real write and every prep return is a live client
record (one mid-edit by another session). Demo serves the same commit and the fix
is a model-level constraint in code, so demo passing proves prep; prep is
API-confirmed live on that commit and its deploy runs `migrate --noinput`.*

*⚠⚠ **s306b's METHOD LESSON — A PROBE AND A SYMPTOM LOOK IDENTICAL IN A REQUEST
LOG.** The escalation named a 404, pointed at two of the day's deploys, and
offered a list of 404s as corroboration. **Every one of those 404s was the
reporting lane's OWN URL-guessing** — `/api/v1/w2s/`, `/api/v1/income/w2s/` etc.
carry a `WindowsPowerShell` user-agent, and `/api/tax-returns/...` simply lacks
the `/v1/`. The app generates exactly ONE 404 (`GET .../prior-year/`, benign, and
it fires 6 seconds before the failing POST, which is why the two got conflated).
**The real failure was a 400, and two of the three predate both deploys**, so
neither caused it. ⭐ What identified it was the RESPONSE SIZE: the body is 50
bytes, and enumerating every serializer field × every standard DRF message showed
only one 50-byte single-field error that the UI could provoke. ⭐ Second keeper:
**`OPTIONS` on that endpoint advertises twenty TAX-RETURN fields and no
`employer_name`**, which reads exactly like "the wrong serializer is bound" — it
is a DRF artifact (`TaxReturnViewSet.get_serializer_class()` returns the LIST
serializer for every action but `retrieve`, and metadata is built from it), so
**OPTIONS on any `@action` of that viewset describes a tax return.** It means
nothing; it is pinned in the test docstring so the next person does not lose the
same hour. The end-to-end test drives the real route with the real payload —
proof by behaviour, not by reading.*

*⭐ **s306c (same session, Ken's direct go): THE GENERAL-CATEGORY FORM 1116 IS
BUILT** (`6930e093`, deploy `dep-da8c5hv10e5c73bobam0`, API-confirmed LIVE and
PROD-VERIFIED — line 28 seeded with its right label, `general` defers on
nothing, `951a` still refuses, the mixed guard fires). ⭐⭐ **Sized as a
multi-session storage-model change; it was ONE session, and re-reading the
engine before writing is what found that**: the §904 limitation (Parts I+III) is
**category-AGNOSTIC** — the same computation applied separately per basket — so
general needed NO new arithmetic. Authority verbatim from the downloaded 2025
i1116 + the IRS face: *"a separate Form 1116 for each category of income"*, and
Part IV gives each basket its own line (**27 passive · 28 GENERAL**). Shipped:
the gate narrowed to {passive, general} (`non_passive_category` renamed
`unsupported_category`); the credit on ITS OWN Part IV line (was hard-coded 27,
so a general credit would have printed on the passive line); line 28 seeded +
both lines persisted; the print widget pinned **POSITIONALLY** against the
caption, never inferred from the field-name sequence (s285). ⚠⚠ **THE REAL TRAP:
the foreign-tax SOURCE.** `_foreign_tax_total` added the 1099-INT/DIV aggregate
to EVERY Form 1116 — that aggregate is **passive-basket tax by definition**
(payee-statement income), so a general form would have absorbed it and inflated
the general basket, a §904(d) violation producing a **plausible wrong number
rather than a failure**. A general form now takes only its keyed
`additional_foreign_tax`. ⚠⚠ **AND THE MeF HALF, the same class as a detector
that cannot fire:** the builder hard-coded `ForeignIncPassiveCategoryInd`, so a
general credit would have **TRANSMITTED UNDER THE PASSIVE INDICATOR** — a silent
wrong filing. Indicator + Part IV element now category-driven, verified against
`IRS1116.xsd` directly. **THE SAFETY PROPERTY** (what makes single-category
support safe without the one-row-per-category model change): two shapes the
singleton cannot hold refuse BY NAME — `D_1116_010` (a general form beside 1099
foreign tax = a genuine two-basket return) and `D_1116_011` (§904(j) is
passive-only by statute). ⚠ The `form_1116s` back-entry shape is UNCHANGED — no
new keys, no model change. Gates: 614 (all 1116 suites + 526 flow assertions) +
680 e-file/MeF. RS amendment staged with the states lane (spec, `R-1116-SUMMARY`,
`FA-1040-1116-07`, and the two new diagnostics).*

*⭐⭐ **s306d — THE CREDIT ORDERING IS CONFIRMED, AND THE TIE THAT "PROVED" IT WAS
HIDING A REAL $1** (`86db00c0`, deploy `dep-da8cs2m417fc73d99e5g`, LIVE). The
entry lane committed the general-category return TIE and then refused to let
that stand as proof — their reasoning exact: **if the ODC had wrongly stayed at
500, the FTC would simply be limited to the remainder, line 21 would still total
the tax, and 22/24 would still be zero — the return would TIE WITH BOTH CREDITS
WRONG.** Reading the committed return's stored lines settles the ordering: **19 =
70, not 500**; 20 = 9,892; 21 = 9,962; 22/24 = 0; the credit on **line 28
(general)**, zero on 27. ⚠ **But the filed return shows 9,893 / 69 to our 9,892 /
70 — identical total, which is exactly why it tied.** MEASURED cause: line 19's
§904 ratio was quantized to FOUR decimals (`L17 75,997 / L18 76,530 =
0.9930354…`; 4dp → 9,892, 5dp → 9,893, **exact → 9,893**). **i1116 rounds this
ratio to "at least four places" — FOUR IS A MINIMUM and we emitted the least
accurate value permitted.** FIVE is the ceiling, not a preference: MeF
`RatioType` is `fractionDigits=5`/`totalDigits=6`. Prod census BEFORE the change
(s302 rule): **8 full-path returns, 7 unchanged, 1 moves by exactly $1 — into
agreement with the filed return.** ⚠ Line 3f deliberately LEFT at four (its
→3g→7 blast radius unmeasured, no divergence reported). Gates 614 + 620 MeF.
⭐ **The keeper: a TIE is not evidence when two errors can offset.** Ask what the
green result was CAPABLE of catching — the same class as the vacuous SE fixture
and the s303 timestamp. It was caught only because the lane refused to bank an
unverified claim, and because the stored lines were readable after commit.*

*⚠ **s306c, against myself — I broke my own standing rule.** A PS5.1
`Get-Content | Set-Content -Encoding UTF8` rewrite of a test file double-encoded
it (every em-dash mangled, BOM added) — the exact rewrite the hazard list BANS.
Caught only because a following Edit would not match; repaired by reversing the
cp1252→UTF-8 round trip, and the whole working diff was scanned for mojibake and
BOM before commit. **The rule exists because the damage is invisible in a
terminal and survives into the commit.***

*🔎 Carried, NOT built: **multi-category (general AND passive on ONE return)**
remains the larger unit — the `Form1116` OneToOne→FK change + all three
registries + `SINGLETON_SECTIONS`, a real Part IV across baskets, multi-face
render, multi-document MeF, per-category §904(c) carryover. At least 10 clients
in the Lacerte set file that way; they refuse by name today.*

*▶ NEXT: **item ⑦'s SINGLE-CATEGORY half is DONE (s306c); the MULTI-CATEGORY half
is not started and not ordered** —
multi-session and it changes a model (OneToOne→FK + all three registries +
`SINGLETON_SECTIONS`), so it wants Ken's word before it starts. Then extractor
follow-ons by the **RE-MEASURED (r17) residual, which is NOT the ranking s303
left**: f8995 coverage (6 solo / 128 touched) > asset_detail (4) >
student_loan_educator_wks (4) > sch_c (3) = **line-20 Sch 3 face class (3, was
"5")** = f8880 (3) = f5695 (3) = f8889 (3). ⚠ Every one is an UPPER BOUND until
the corpus re-runs with that class open (fifth confirmation). Open BATCH-296
entry-lane items still unworked: the `div_1099s.us_government_income` → GA S1-10
auto-derive design (needs an off-switch decision, s237 class) · the D_SCHD_006
QOF import surface.*

*Peer state (s306): I hold the tree + test_postgres; both lanes confirmed no
collision at boot. **Entry lane is UNBLOCKED and running — 33 packets filed**
(auth good, Ken gave explicit `--prod-db` go in their session). Their open holds:
two on Ken's 2024 figures, one sanctioned Pub 974 method difference, one on the
general-category 1116 (item ⑦ — that packet is not enterable at all). States lane
holds neither the tree nor test_postgres; RS suite 254 passed / 0 failed.
⛔ **NOTHING of S-14…S-19 has been ruled** — the states lane's standing warning
applies: if any lane says Ken ruled one, that is a RELAY, check with him directly
(three relays on 8/25–26 differed from what he meant or were retracted).*

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
- **s306 deploy — API-CONFIRMED LIVE 2026-08-27: `64c4dc15` →
  `dep-da8ap4ia6suc73bdidtg`** (finished 21:35Z), the currently live deploy. It
  carries all three s306 pushes: `1ae86753` (the D_8889_EXCESS widening + the
  4547 casing normalisation + the schema descriptions — its own deploy
  `dep-da8aocpt0dsc73bvgfu0` BUILT and went update_in_progress before being
  superseded, which is the normal deactivation), then `e1754760` and `64c4dc15`
  (markdown only). **No migration, no seeder, no schema-vocabulary change** — the
  published back-entry schema WAS regenerated, but only field `description`s
  moved, so no allowlist or enum shifted. ⚠ The diagnostic rule's name/description
  reach prod because `seed_builtin_rules` is `update_or_create` and the deploy
  seeds at BUILD time; severity is unchanged (`warning`), so no rule row changes
  behaviour beyond the widened condition.
- s303 deploys, BOTH API-confirmed LIVE 2026-08-27: `54c187ba` (head of
  `5d0c33c5` + the scratch-file removal) → `dep-da8377cs728c73cubirg` =
  the staging-predicate fix itself; then `0d7673c9` (carrying `692d899d`,
  the empty-array test, + the session-close docs) →
  `dep-da83q215efls7394k1vg`, **the currently live deploy**. No migration,
  no seeder, no schema regeneration in either (the change is a staging
  predicate + docstrings; no vocabulary or allowlist moved).
- s302 deploys: `9999f2c6` → `dep-da7q5rek1f9s73chfqe0` (⑥, mig 0367 +
  D_K1_PTP_469G seed) — **API-confirmed LIVE**; `a9f97025` →
  `dep-da7qalvlk1mc738cgd60` (s302b line 26, no migration) —
  **API-confirmed LIVE**; `f882e494` → `dep-da7qdacs728c73cml8i0` (s302c
  D_EFILE_004 seed, no migration) — **API-confirmed LIVE**; `ffedba4d` →
  `dep-da7qp749v7es73bl9psg` (s302d SE line 8a, no migration) —
  **API-confirmed LIVE**. All four verified 2026-08-26.
- s301 deploys: `d6ab0e42` → `dep-da7ovk6417fc738ffrq0` · `03c96795` →
  superseded by `2a2bb807` — LIVE (s301b e8960 surface confirmed by the
  entry lane's committed 1723).
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any vocabulary/allowlist/
  state-seeder change MUST regenerate the published schema at close.
  (s302 regenerated; current as of `9999f2c6` — s302b/c changed no schema.)

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. s302 coordinated at boot;
three annexes appended to BATCH-296 (every load-bearing exchange annexed).
Peers stage; Ken decides. ⚠ A peer's factual claim can be RETRACTED
minutes later (the s302 "second fixture" correction) — build only against
what a batch file or your own probe shows.

## ⚠ Known red / rotted — THE ONE LIST (post-s302)
- ~~12 of 47 RS `check_*_integrity.py` gates FAIL~~ — **THE 12 WAS WRONG AND
  S-10 IS CLOSED** (states lane, 2026-08-27, correcting their own s302
  number). Their triage: **3 were never failing** (two pass every check then
  die printing a non-ASCII success line to a cp1252 console; one exits
  `ImproperlyConfigured` importing a loader without configuring Django) and
  **2 were their own defects from hours earlier**. True pre-existing count
  **7**, of which **10 of 12 are resolved and 10 gates are now pinned in
  `tests/test_integrity_gates.py`**; RS suite **254 passed / 0 failed**.
  ⚠⚠ **The sweep that produced the 12 read EXIT CODES without reading
  OUTPUT** — the same one-signal-generalised error class as s303's own
  timestamp misreads. ⭐ Their lesson: **a red gate does not merely fail to
  catch new defects, it CAMOUFLAGES them** — two fresh defects hid inside
  pre-existing redness, in gates nothing runs. ⚠ S-10a survives separately:
  `R-1040X-SUPERSED` has no authority link, and "superseding" appears ZERO
  times in the 2025 i1040x (they pulled and committed it as evidence), which
  also casts doubt on the sibling rule's existing citation. S-10c
  (`D_8582_PTP`'s unwritten recompute) also stands.
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof. (s302: the two s258
  fails appeared again in the sweep and PASSED in isolation — unchanged.)
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ~~the GA700 answer-key coverage test~~ — REPAIRED s302 (was red since
  s274; greped only FORMULAS_GA700, blind to the pull-map writes).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s302).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): five phantom failures. New files/markdown/scratchpad only.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c`; a script run by absolute path needs
  `sys.path.insert(0, server)` (s298). ⚠ `python -m taxwise1040`
  does NOT resolve from `server\` — run the package's `__main__.py` BY
  PATH (s297).
- 🔧 ⚠ **`BaseModelSerializer` partial updates save ONLY the patched
  columns** (s298) — a serializer-side derived write must be injected into
  `validated_data`, never `setattr` on the instance (it silently no-ops).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ (s302 re-confirmation: `-replace` +
  `Set-Content` mangled an em-dash annex — python io did it clean.)
  ⚠ Embedded double-quote in a here-string arg to a NATIVE exe SPLITS the
  argument. ⚠ `$1` in an unquoted PS arg EXPANDS to empty. ⚠ PS
  `Sort-Object -Unique` on a one-element pipeline UNWRAPS to scalar (s296).
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash. ⚠ A
  COMMITTED return refuses plain stage+dryrun with HTTP 409 (s298 —
  verification route = the in-place rolled-back recompute, s289 pattern;
  s302 ran the 1792 probe this way clean, incl. the commit reconciliation).
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ **`str(Decimal)` preserves the STORED SCALE** — a diagnostic detail
  written as `str(amount)` reads back `"429.00"`, not `"429"`. Compare
  `Decimal(details[k])`, never the raw string (s306; two test failures).
- 🌐 ⚠ **A BACKGROUNDED pytest piped through `Select-Object` buffers ALL
  output until exit** — no interim progress at all, so a long sweep looks
  identical to a hung one. (The standing rule already bans that pipe; this is
  the detached-run reason.) Check liveness with the process's CPU time.
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); synthetic `insert_text` PDFs may
  LOSE leading spaces (s290) — the extractor parses POSITIONALLY everywhere.
  ⚠ **The AcroForm FILLER flattens ALL widgets out of its output** (s296).
  ⚠ **TaxWise RE-TYPESETS whole schedules** (s297): pin from packets only.
  ⚠ s301: a fixture that encodes the WINDOW's geometry instead of the
  CORPUS's keeps a blind spot green.
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section. Local templates beat both.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292–s302 ran probes this way). ⚠ Scripts touching
  client-named returns live in SCRATCHPAD or tax-test-data, never the repo
  (PII). ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294).
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295/6/7/s301).
- 🔧 ⭐ **DRY-RUN THE CORRECTION PASS AND READ EVERY ROW before --commit**
  (s298). ⭐ s302 corollary: **CENSUS THE PROD BLAST RADIUS before changing
  a computed line's source** (51 returns read, zero moved — the do-no-harm
  half; the failing case rides the tests).
- 🌐 ⭐⭐ **THE THIRD QUESTION (s302, states lane): "does anything ACT on
  it?"** — after "does it exist?" and "does it run?". A green detector
  nobody reads is functionally no detector, and it is HARDER to spot
  because it announces itself as a pass (D_2210_DATED fired correctly for
  weeks while line 26 dropped the money it was flagging). ⚠ s302d: the
  same question applies to a TEST — mine ran, was green, and measured
  nothing.
- 🔧 ⚠⚠ **A FIXTURE THAT KEYS A COMPUTED COLUMN MEASURES NOTHING** (s302d):
  `ScheduleC.net_profit` is written by compute from gross receipts, so a
  test that keys it gets line 31 = 0 — and every rate/ratio assertion then
  passes as `0 == 0`. Key the INPUT (`gross_receipts`), and assert the
  intermediate is nonzero BEFORE asserting anything about a rate applied
  to it.

## 🔎 Carried for triage — NOT claims
- (s303) **The simplified home-office 300 sq ft cap is applied PER SCHEDULE C,
  but Rev. Proc. 2013-13 §4.08(6) caps ONE TAXPAYER'S AGGREGATE across all
  qualified business uses of the SAME home** ("A taxpayer who has more than
  one qualified business use of the same home for a taxable year is limited
  to a maximum of 300 square feet ... must allocate the square footage among
  the qualified business uses"). `compute_schedule_c` caps `biz.home_office_
  sqft` per business, and BOTH diagnostics (D_SC_004 / D_SC_005) iterate per
  business — so one proprietor with two home-based Schedule Cs could claim
  600 sq ft, overstating by up to $1,500. **Prod census: ZERO returns hit it
  today** (17 simplified home offices, no proprietor with two). Latent,
  overstating, unfiled work. ⚠ The sibling rule is the OPPOSITE and the
  engine is RIGHT about it: §4.08(5) lets spouses sharing a home EACH use up
  to 300 sq ft of *different portions* — so the entry lane's 300+250=550
  packet is correct as filed, and a naive per-return aggregate check would
  wrongly flag it. Group by `proprietor`, never by return.
  ⚠⚠ **THE THRESHOLD IS AGGREGATE > 300, NOT "TWO ROWS" — do NOT ship an
  unconditional refusal.** I proposed refusing on "same proprietor, two
  simplified rows" (reasoning from §4.08(7), *"may use the safe harbor method
  for only one home for that taxable year"*). **The entry lane refuted it from
  the FIRST SENTENCE of §4.08(6), which I had already printed and read past:**
  *"A taxpayer who has more than one qualified business use of the same home
  for a taxable year and who elects the safe harbor method **must use the safe
  harbor method for each qualified business use of the home**."* Two elected
  rows in the same home are **MANDATORY**, not a defect — a refusal there
  would push the enterer to un-elect one, which is exactly what (6) forbids.
  The three cases for one proprietor with two simplified offices:
  **same home + aggregate ≤ 300 = LEGITIMATE AND COMPELLED** · same home +
  aggregate > 300 = defect under (6) · different homes = defect under (7).
  So: **aggregate > 300 per proprietor → refusable** (the missing home id then
  changes only which clause the message cites); **aggregate ≤ 300 → WARNING
  ONLY**, because compelled-same-home is indistinguishable from a (7) breach
  without a home identifier. `proprietor` + aggregate sqft is the key;
  `proprietor` alone is not.
- (s303) **§4.08(4) monthly averaging is unrepresentable in our data** — a
  business starting, ending or resizing mid-year must use *"the average of the
  monthly allowable square footage"*, no more than 300 in any one month, and a
  month counts only with *"15 or more days of a qualified business use"* (the
  Rev. Proc.'s own example: 400 sq ft from July 20 → average 125, not 300).
  `schedule_cs` keys ONE `home_office_sqft` with no in-service date, so a
  mid-year start silently claims the full-year amount. Same gap as the missing
  home identifier — size them together.
- (s302d, entry lane) **D_EFILE_001 cannot distinguish "EIN not keyed"
  from "EIN not obtainable."** Most packets satisfy it from the GA-500
  income-statement grid, but a payer that withheld no Georgia tax never
  appears there — a third-party sick-pay payer on one packet has no EIN
  anywhere in the source. That packet is held on a gap the preparer
  cannot close from the documents. If the shape recurs, the refusal
  should split (and the s298 21-blank-row class is the same question
  from the other side, still on Ken's prefix-tier call).
- (s302, states lane S-10c) **`D_8582_PTP` remains unverified** — one of
  seven keys `check_schedule_e_8582_integrity` reports as "no independent
  recompute mapped". s302's §469(g) build is validated only by its own
  i8582 PDF reading, which is why that reading was done. ⚠⚠ Their wider
  finding: `check_topic8`-style gates compare a rule's DECLARED inputs
  against the facts and never compare the FORMULA against the
  declaration — so `inputs: []` is vacuously valid and **the emptier a
  rule's declaration, the safer it looks** (exactly how R-SE-L8D-L9's
  missing L8a input survived; found by a $5,717 error, not by a gate).
- (s302) `div_1099s.us_government_income` is attribution-only BY DESIGN
  (splits a keyed ga500_fields S1-10 by owner; derives nothing) — the
  entry-lane note asks whether it should auto-derive S1-10; needs an
  off-switch decision (s237 class). · D_SCHD_006 QOF answer has no import
  surface (warning-level, filed).
- (s301) One packet's 5b decomposition: extracted pension rows carry
  115,150 vs face 15,150 — probe the 1099-R parse before assuming a
  rollover/exclusion. · One packet's Sch D carries 1b grid totals with NO
  f8949 pages + its own h-identity break — suspected misparse. · A blank
  printed 8949 page (synthetic packet) refuses "no transaction rows" —
  over-refusal by design.
- (s298) 21 named-but-blank W-2/1099-R rows have no unambiguous EIN match
  — held on D_EFILE_001; movement waits on Ken's prefix-tier call.
- (s297) The X mark at (474.7, y≈389) in the 1040 p2 EIC row region —
  unidentified, parser-ignored. · `est_payments_wks` (27) and `f8863` (6)
  gate at the face. ⚠ s302b note: when the est_payments_wks face class
  opens, the extractor should emit dated `federal_estimated_payments`
  rows — they now drive line 26 directly.
- (s296) The 22 sch_d GEOMETRY-error packets refuse loudly by design.
- (s295) 7 auxiliary Inbox PDFs refuse as non-packets — correct. ·
  `_summary_lines` GA500_SUMMARY_LINES lacks S1-6.
- (s290) GA RIE interest row excludes K-1 16A tax-exempt interest —
  stated boundary. · The rendered 8995 TIN prints unformatted — cosmetic.
- (s289) `IndividualForm7203`: no §179/charitable carryover keying;
  the §179 cap doesn't extend to 1065. · K-1 capital gains reach Sch D
  but not the L9 gain/loss WEIGHTS (⚠ s298: also the #78 aggregate
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
Everything from s277–s301 stands (see STATUS_ARCHIVE s300/s301 entries).
NEW (s302): **S-15** (SCHEDULE_K1 §469(g) disposition trio + the
R-8582-PTP related-party CORRECTION — the spec as written frees losses on
a related-party sale, the exact case §469(g)(1)(B) bars; + rule-2/rule-4
branch naming + the fully-taxable definition) and **S-16** (R-PAY-04
dated-rows source, matching R-2210-REG's precedence) — both staged by the
states lane, both Ken's. Recorded lane-side, not staged: the RS 8990 spec
models NO Schedule A surface (the seam if it is ever specified is
`partner_excess_bie`).
NEW (s306): **S-19** — `R-8889-EXCEPTIONS` states the same narrow
`line 2 > line 13` condition the code carried (blind to an employer-only
HSA excess, which i8889 measures against line 8) and its diagnostic message
still says the excise is "not computed here", which is stale since Form 5329
Part VII computes it from line 47. Staged by the states lane, who pulled both
instruction PDFs and confirmed the quotes verbatim, and who DISSOLVED a third
suspected defect in the same rule (line 12 vs line 13 are arithmetically
equivalent given `line 13 = min(line 2, line 12)`) — recorded because a false
finding attached to a true one is how a good item gets argued down.
⚠ Also noted by them, no action from this lane: `R-8889-EXCEPTIONS` declares
`inputs: []` while its formula reads lines 2 and 13 — the THIRD confirmed
instance of the S-17 under-declaration class, after `R-SE-L8D-L9` and
`R-1116-SUMMARY`.
