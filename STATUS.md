# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-21 (s272, Friday — Ken back in office). **Nine
deploys today, every one verified live.** The headline: **#11 half B is
CLOSED WITH NO CODE** — Ken reversed his own "unearned" ruling after I
showed that a 14/14 sweep had measured TaxWise, not Georgia; the engine's
earned routing was right all along. Everything after that was the machinery
to close returns whose FILED figures are wrong: `source_defects` exceptions
(stuck returns file; next year's preparer is told why), which took **four
rounds of production-found holes** (an inert allowlist key, three unpatched
verdict gates, a `-65` flag the guard missed, and `null` writing a blank
override instead of clearing). ⚠ **147 staged returns hand-key the GA RIE
worksheet; the harm is MASKING, not magnitude.** Now warned on every payload
and REFUSED on never-committed ones (Ken's call).*

*⚠ **A parallel entry session ("2025 Form 1040 returns processing") found
every one of those holes by testing against production while I tested
against tests.** It is waiting on Ken's DIRECT go-ahead to reopen four filed
returns — correctly refusing to act on my relay of his decision.*

*Three commits on main today are Ken's own from the `delvio-states` session
(`c64fd84` credential removal + rotation, `f6eaf78` its DECISIONS entry,
`b32730d` the state-code parse fix). Recorded here so the log is honest; not
this session's work.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.
- *s272 moved the s271 per-leg blocks into `STATUS_ARCHIVE.md` (46 lines, purely
  additive — verified by `git diff --stat`), continuing what s268 started.*

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS (WHEN BUILDS RUN)
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax` (srv-d6geloa4d50c73el2trg); demo = `tts-tax-demo`. ⚠ **A push
is not a deploy — CHECK THE DEPLOY STATUS after pushing** (API key in
`D:\dev\Passwords & Secrets\render-api-key.txt`): 30 pushes "deployed"
into a canceled-build void over two days (08-13→08-15) before anyone
looked. Ken raised the build spend limit $50 → $200 on 2026-08-15, which
resolved that void.

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

---

## ▶ RESUME HERE

### ✅ s272 — BATCH-296 #46: Form 2441 line 16, and line 3 stops double-reducing (`aec81be`, mig 0326, **LIVE 17:53 UTC — verified**)
The item asked for ONE fact. The 2025 face had **three** defects, and the
item's own acceptance was unreachable without the second.
- **① The line-16 source fact.** `Taxpayer.f2441_dcb_qualified_expenses`,
  **NULLABLE** — NULL keeps the derive, a value (**0 included**) is the
  assertion. 0 had to stay distinct from blank: an asserted zero says no
  qualified expenses were incurred, which correctly makes every benefit
  dollar taxable (the s243b Tucker shape). Wired into `_gather_2441_inputs`,
  the ONE chokepoint both consumers read — not into either consumer.
- **⚠⚠ ② LINE 3 DOUBLE-REDUCED.** It computed
  `max(0, min(col_d_sum, cap) − excluded)`, but column (d) is **already net**:
  both the line-2(d) and line-30 instructions say *"don't include in column
  (d) any benefits shown on line 28."* The face's chain is
  `27 → 28 = 24+25 → 29 = 27−28 → 30 = the col-(d) sum → 31 = min(29,30) →
  line 3`. The fixture computed line 3 = **0** and lost the **whole** credit.
  **Adding the line-16 fact alone would NOT have fixed it** — the two defects
  had to be separated, and a test pins each half independently.
- **⚠ ③ Logical "31" held the WRONG figure AND PRINTED IT.** It carried the
  excluded-benefit amount — the pre-2025 face's numbering, inherited from the
  RS spec's stale `line_map`. Widget geometry against the IRS template proves
  `f2_20` **is** paper line 31 (f2_4..f2_20 = paper 15..31 in order), so the
  face printed $5,000 where the filed face prints $1,000. Paper lines 28/29/30
  are now seeded + rendered, along with the s243b Part III detail (15-25),
  which had been computed and persisted since s243b but **never printed**.
- **AUTHORITY: the 2025 face + instructions, not the spec.** ⚠ But the RS
  spec is **not contradicted** — scenario **2441-T4 still passes unchanged**,
  because every spec test puts the GROSS figure in column (d), where the two
  formulas agree. `R-2441-EXPENSE-CAP`'s shorthand was never wrong in any
  scenario the spec exercises; it is simply not general. Both it and the
  pre-2025 `line_map` go on the RS agenda.
- **Reachability**: on BOTH screens (Slate + FormEditor) via
  `useTaxpayerFacts`' `nullableKeys`, so a cleared box sends `null`, never
  `"0"` (the 2210-override precedent). Staging refuses a negative and warns
  twice. Regression: `server/tests/test_batch296_item46_s272.py` (19).
  Gates: 526 flow assertions + 975 (2441 blast radius) + 496 backentry +
  typecheck + 1715 vitest. Teeth proven by injecting BOTH defects (5 pins
  fell, then 2). **Four stale line-31 pins corrected in the same pass — one
  of them the flow assertion `FA-1040-2441-05` (RS agenda: re-export).**

### ✅ s272 — #11 half A: `D_K1_SCORP_SERET` (`be41747`, no migration)
Ken ruled 2026-08-20: **no 1120-S retirement feed — diagnostic instead.** An
1120-S K-1 carrying `se_retirement_amount` now WARNS (Pub 560, quoted from a
live fetch: a shareholder-employee is not self-employed; the corporation
deducts on the 1120-S). The legitimate **1065 feed is pinned untouched** —
that was the real risk of touching this area. 6 tests; teeth proven by
dropping the entity-type filter (fired on a partner, failed the pin).
`seed_rules` is a DEFERRED_SEED so the deploy registers the rule.
- **#11 half B — ✅ RULED 2026-08-21: EARNED. NO CODE; the s239 routing in
  `k1_ordinary_is_earned` is correct and unchanged.** Ken ruled "unearned"
  on 08-20 and reversed on 08-21: *the nine packets that filed it unearned
  are all TaxWise; Lacerte and the GA rule treat it as earned.*
  ⚠⚠ **A sweep of the held population had found 14/14 decisive owner-columns
  reproducing the filed figure under UNEARNED with zero counter-examples —
  and that was still the wrong conclusion, because all nine clients came
  from ONE office on ONE software. THE SWEEP MEASURED TAXWISE, NOT GEORGIA.**
  See DECISIONS.md for the authorities and the general lesson.
- **⚠ NINE CLIENTS CAN NEVER TIE** (nine packets, listed by key in the batch annex — ~$243k of over-claimed exclusion, ~$12.6k GA tax
  at 5.19%). Their TaxWise answer keys are wrong on the GA RIE line, so
  `reconcile_expected` can never return a tie. **They need the SOURCE-DEFECT
  EXCEPTION disposition below — not an indefinite HOLD.**

### ⚠⚠ s272 INCIDENT — the entry session wrote code in this repo, and my `git add -A` shipped it
The "2025 Form 1040 returns processing" session (supposed to be entry-only;
**Ken has since told it so directly**) built BATCH-296 **#61** (QBI ½-SE-tax
allocation: non-QBI SE earnings — clergy, 1065 box 14A — join the
`allocate_qbi_reductions` weights so a farm/business no longer absorbs the
clergy share of the ½-SE deduction). My `git add -A` swept its IN-PROGRESS
test + compute change into `be41747` under the #11 message; it then repaired
its own test in `95d390c`. **Both are LIVE (deploy verified).**
- **The code is now VERIFIED GREEN**: its 3 tests + **636** across the QBI /
  8995 / clergy / Schedule-F radius, run clean after the collision cleared.
  The change is a plausible correction (Treas. Reg. §1.199A-3(b)(1)(vi)) and
  is treated as SHIPPED; the #61 annex/audit trail is owed to the batch file.
- ⚠⚠ **We also COLLIDED ON THE SHARED TEST DB** mid-verification
  (`test_postgres does not exist`) — the standing hazard, realized. Any test
  result from ~22:50-23:05 UTC 2026-08-20 is untrustworthy.
- **New working rules (proposed to the sibling, adopted here):** this session
  stages EXPLICIT PATHS, never `git add -A`, while any sibling shares the
  tree; one session at a time on pytest with an explicit hand-off.

### ▶ IN FLIGHT — a SECOND CC session is entering returns (opened 2026-08-19)
Ken is running a parallel session, **"2025 Form 1040 returns processing"**
(`local_48d66d7c-e56b-4b93-a93a-eef3331c2d52`, cwd `D:	ax-test-data`), in the
Codex role: it enters returns and posts blockers; **this** session verifies,
builds, deploys and annexes. **Work paused 2026-08-19 evening at Ken's
direction (travelling) — resume tomorrow.**

- **Split**: they own the HOLD/READY/QA notes and posting items; **we own
  `STATUS.md`, `form_coverage_tracker.md`, `BUILD_ORDER.md` and the batch
  file's ANNEX sections.** Neither edits the other's files (shared git repo).
- **⚠⚠ NEVER RUN PYTEST WHILE THE OTHER SESSION IS RUNNING IT** — one shared
  `test_postgres` across repos; concurrent runs corrupt each other. Coordinate
  before any sweep (the backentry sweep is ~6.5 min, the 2441 radius ~5.5).
- **Backlog measured 2026-08-19**: 966 packet dirs under `1040	mp`, **553 on
  HOLD**, 112 closed, 30 completed, 30 ready.
- **⚠ 47 of the 553 holds cite a blocker that is ALREADY BUILT and live** —
  20 the dividend aggregates (`div_qualified_agg` / `div_capgain_dist_agg` /
  `div_unrecap_1250_agg`, all in `TAXPAYER_FIELDS`), 11 Form 2441 (#46), 5
  W-2G identity (#34), 5 box-2b (#47), 4 deceased-GA (#36), 3 collectibles
  (#39), 1 each #35/#42 and #49. The packet list was sent to the other
  session and saved (client names — NOT in this file, NOT in the mirror) at
  `D:	ax-test-dataD0	mp\RERUN-CANDIDATES-2026-08-19-from-build-session.md`.
  ⚠ Keyword-matched — a re-run CANDIDATE list, not a proven one.
- **⚠ Only 36 of 553 holds name a CC item number**; the rest describe blockers
  in prose with no dominant cluster. **We have ASKED the other session to rank
  what is actually costing it the most returns** — that ranking should drive
  the next build order. Until it arrives, the ranking is unknown, not empty.
- **⛔ Do not re-report to them**: #11/#44/#50 are Ken's; #48/#52 have no RS spec.

### ✅ s272 (Fri 08-21) — THE SOURCE-DEFECT EXCEPTION UNIT, end to end in prod
Ken's ask: *"a notification when we file the 2026 return that the GA
retirement exclusion was calculated incorrectly in 2025 ... I just want to
get them done and out of the way so that we don't revisit them."* Both asks
are ONE mechanism because they need the same data. **Proven live on two
real clients** (one one-cause, one two-cause), read back from prod.
- **Why those returns were stuck, which nobody had named**: `_mark_return_
  filed` files ONLY on verdict `tie`. A wrong TaxWise answer key can never
  tie, so the return sat committed-but-never-filed forever.
- **`source_defects`** on the payload (`279c5c5`, mig 0328): each row
  releases ONE `(form, line)` from the tie requirement; verdict
  `tie_with_exception`. **Reason + authority REQUIRED** — an unexplained
  "source was wrong" is indistinguishable from giving up, and that text IS
  next year's note. **Per line** (a second unrelated mismatch still blocks);
  **per owner** for free (RIE-TP-17 / RIE-SP-17 — on the one-cause fixture the taxpayer
  column caps and hides the defect, only the spouse column moves).
- **`comparison_notes`** on `PriorYearReturn`: filed vs computed vs reason
  vs authority, surfaced by the proforma roll-forward. **Grouped per CAUSE
  (the authority), never per line** — a cascade is one note listing every
  line it moved; `causes_for_line` stamps how many causes a line has
  (derived, never narrated).
- **A line can have TWO causes** (`0a27187`): uniqueness is `(form, line,
  AUTHORITY)`. The two-cause fixture: a federal Pub-560 defect AND the GA routing
  move four Georgia lines together; neither alone yields the 1,611.
- **⚠⚠ FOUR PRODUCTION-FOUND HOLES, all mine, all found by the entry
  session:** (1) `source_defects` never joined `KNOWN_RETURN_KEYS` → inert
  in prod (`4b3d598`; the same commit found `amendment` had NEVER been
  published — the schema was stricter than prod; one equality invariant now
  closes both directions); (2) FOUR places gate on the verdict, I patched
  one → a return marked FILED but held by cleanup (`72e3c06`,
  `FILEABLE_VERDICTS`); (3) the RIE guard matched lines 1-16 so `RIE-*-65`
  — the $65k-vs-$35k cap flag — fell through (`0bcbcdf`; now the whole
  worksheet except the un-derivable disability assertions); (4) `null` in
  `ga500_fields` wrote an override to BLANK, so there was NO lane-side way
  to undo a stored override (`e3b7484`; null now clears).

### ⚠⚠ s272 — 147 STAGED RETURNS HAND-KEY THE GA RIE WORKSHEET
`ga500_fields` declares ZERO properties with a permissive catch-all, so any
line-shaped key staged clean. The retirement exclusion is engine-derived;
keying its inputs forces the answer, so a green tie may be proving keyed
data rather than a computation — **invisible precisely because it is
green.** A/B on never-committed returns (diverging LINE SET, not verdict):
one packet INERT; **one LOAD-BEARING — hiding a $3 divergence**.
**The harm is MASKING, not magnitude**: the identical reflex hid $27,501
(one spouse column) and $54,360 (one return, two columns), same green result.
- Warned once per return naming every key (`ab98b70` → `0bcbcdf`).
- **REFUSED on never-committed returns (Ken, 08-21; `4268c86`)** — the
  population cannot grow; committed ones keep staging (they cannot be A/B
  tested anyway). ⚠ Scope = the BULK IMPORT LANE only; preparers entering
  2026 returns never see it.
- **Four-way semantics for a stored preparer entry, now pinned in a test:**
  absent = inherit · 0 = override · null = CLEAR (was: blank override).

### ⛔ WAITING ON KEN — a DIRECT go-ahead to the entry session
**Reopen the four filed returns named in the batch annex.**
Ken ruled it to ME; the entry session correctly refuses to reopen four FILED
production returns on a relay. It has the sequence (draft first — the filed
sweep SKIPS already-filed returns, so a re-commit would never re-fire the
note; null-clear every stored RIE key; dry-run; one ack per (line,
authority); replace; sweep; cleanup) and the measured divergence sets for
two of them (8 and 6 lines — ⚠ the second is NOT capped like the earlier
example; that cap theory was about a spouse's $62k of gains, not the cap). **Ken tells it directly;
it runs all four one at a time; I read each snapshot back from prod.**

### ⚠ s272 — the import-lane DOC had rotted; the schema never had
`CC_IMPORT_LANE_HANDOFF.md` section 0 was a hand-kept DENYLIST that still
listed 1116 / education / adoption / Schedule H / every non-GA state as
unsupported. The entry session re-scored 361 holds against the real schema:
**102 stale and re-openable.** Fix is the SHAPE: the generator now emits
`SUPPORTED-SECTIONS.md` (an exhaustive ALLOWLIST) beside the schema
(`a323a78`), pinned to equal the published keys. ⚠ Two traps hit building
it: the first cut omitted dict-shaped keys (incl. `state_returns`, the whole
AL/NC/SC lane), and the test then passed only because I had named a key in
prose. Generate, never narrate.

### ✅ s272 — #23/#24/#53 LEG 1: the filed depreciation total wins (`66bb3e6`, mig 0327)
**The guard had to exist BEFORE the import lane, not after.**
`aggregate_depreciation` opens `if not assets.exists(): return` — until now
THAT early return was the only thing protecting an imported flat filed total
(`backentry.py` says the Sch C L13 / Sch F L14 totals are importable
*precisely because* "an import-built return has no asset register"). The
moment leg 2 can create asset rows, the engine would overwrite the filed face
that every acceptance criterion in the three items demands survive.
- `depreciation_filed` on `ScheduleC` / `RentalProperty` / `ScheduleF`,
  **NULLABLE** — NULL = engine behaves exactly as before; a value (**0
  included**) is the source assertion and wins. The three blind `.update()`
  calls route through one `_write_parent_depreciation` helper. **Per parent**,
  not per return. The register still computes and still persists every
  per-asset result — it is never inert.
- **⚠ THE DIAGNOSTIC ALREADY EXISTED** — `D_4562_RECON` is the permanent
  silent-routing-gap guard and IS the reconciliation Ken asked for, so it was
  TAUGHT the assertion rather than duplicated. Disagreement → **warning**
  naming both figures; a genuine routing gap with no assertion still raises
  the original blocking **error**. It reads the engine's own saved
  `current_depreciation` and never re-derives depreciation (s142).
- Regression: `test_batch296_asset_register_filed_wins_s272.py` (10). Gates:
  526 FA + 348 (depreciation radius, 16 files) + 136 diagnostics. Teeth proven
  by injecting both defects (5 pins fell, then 2). **Movement: NONE** —
  `depreciation_filed` is NULL everywhere and the write path is then
  byte-identical.
- ⚠ `FA-1040-SCHF-04` was a source-text grep for the literal `.update(` the
  refactor replaced; it now asserts the new shape AND the guard. **RS agenda:
  re-export it.**
- ⚠ **The two `backentry.py` comments describing the old structural
  protection are now STALE — correct them when leg 2 ships (s225).**

### ▶ LEG 2 — the import surface itself (NOT started)
A `depreciation_assets` section in `backentry.v1`: the field allowlist, the
parent resolution (`flow_to` + which Schedule C / rental / farm), staging
validation, and `depreciation_filed` joining the three parent sections.
⚠ Model + engine + diagnostics are DONE; leg 2 is the lane only.

### ▶ THEN — 1040 BATCH-296, CLUSTER 4
`1040\CC Changes\CC_CODE_CHANGES_BATCH-296.md` — **items now run to 53,
OPEN, 24 closed.** The running annex in the file is the record; read it first.

**▶ NEXT:** the mid-size 1040 units **#12, #13, #15, #16, #17, #18, #20,
#21, #22, #25, #28, #30**, then the big unit **#23/#24/#53** (see below —
they are ONE build, not three). ✅ **THE DESIGN CALL IS RULED (Ken, 2026-08-20):
THE FILED TOTAL WINS** — DECISIONS.md, "Back-entry asset registers".
**LEG 1 IS BUILT** (`66bb3e6`, mig 0327); the import lane itself is leg 2.

⚠⚠ **s272 CORRECTION (2026-08-20): the model is `DepreciationAsset`, NOT
"DepreciableAsset"** — the earlier triage, the batch annex and the message to
the entry session all used the wrong class name, and the resulting grep
("zero references in `backentry.py`") was FALSE. There are two references,
and they document a deliberate design decision. Corrected below and in the
batch annex.

⚠ **#37 duplicates #2** — treat 37 as the live spec; do not work both.
⛔ **#40 is a LARGE multi-session build** (AMT passive losses = an AMT shadow
of Form 8582) and belongs after the big units.

### ⚠ s272 TRIAGE of the five new items (50-53) — full evidence in the batch annex
⚠ **The batch file numbers TWO different items 51.** Codex asked to renumber.

| Item | Verdict |
|---|---|
| 50 (AL Form 40 line-12 PYR override) | ⛔ **KEN — the engine is RIGHT, the filed return is wrong.** Confirmed at HEAD ($527 computed vs $568 filed), but the **2025 Alabama Form 40 Booklet p.5** makes the proration MANDATORY and the engine's ratio matches it verbatim. Building the override would encode an Alabama error — the **#11 pattern**. Recommend: record the $41 as a source defect. |
| 51 (jury duty → Sch 1 line 8h) | ✅ **REFUTED — fully built since s241z.** `other_income_items.route` is a CLOSED enum containing `"8h"`, labelled "jury duty pay" in the code. The existing suite already pins the item's exact fixture: a **$25 "County jury duty"** row → line 8h → line 9 → line 10 (25 tests green at HEAD). **No code needed.** |
| 51 (GA Sch 1 line 5 other additions) | ⚠ **PREMISE REFUTED — keyable today.** `ga500_fields` has **no allowlist** (unlike `sch1_fields`), and `S1-5` "Add: other additions" is a seeded **non-computed** line with `S1-6` summing above it. Still open + smaller than stated: the *description* half, and an end-to-end fixture run. Re-scope. |
| 52 (Alabama Form 40NR) | ⛔ **BLOCKED — no RS spec.** `AL_FORM_40NR` → 404; `AL_FORM_40` → **200** proves the convention. Code half confirmed (`compute_al40`: *"True nonresidents file Form 40NR (not computed)"*). Whole new form. **RS agenda.** |
| 53 (Schedule F depreciation assets) | ⚠ **NOT a third item — #23/#24/#53 are ONE build.** `DepreciationAsset` **already** has FKs to `schedule_c`, `rental_property` AND `schedule_f`, plus a `flow_to` router. The import surface is missing, but **NOT by oversight — by DESIGN**: `backentry.py` documents that the flat filed `depreciation` total is importable *because* `aggregate_depreciation` early-returns when a return has no asset rows, and says asset-register continuation "rides the existing conversion importer, not this lane". ⚠⚠ **That importer needs a Lacerte TXT export these PDF packets do not have**, so the lane is genuinely the right home — and building it RETIRES that early-return protection. See the design question in KEN DECISIONS. |

⚠ **The verify-first streak is unbroken:** of five new items, ZERO were
buildable as written (one refuted, one premise refuted, one contradicted by
its own state's instructions, one spec-blocked, one collapsing into #23/#24).

---

## ⚠ Classes that MOVE existing returns or output on next recompute
- **✅ s272 #23/#24/#53 leg 1 MOVES NOTHING.** `depreciation_filed` is NULL on
  every existing row, and with it NULL the depreciation write path is
  byte-identical to before. The class it will create arrives with **leg 2**,
  when a packet can actually assert a filed total.
- **⚠⚠ s272 #46 MOVES A CLASS — every 2441 return WITH dependent care
  benefits whose column-(d) sum is below the $3,000/$6,000 cap gains
  credit** on next recompute. `new = min(cap − excl, col_d) >= old =
  min(col_d, cap) − excl` **always**, so it can only rise — a correction
  toward the printed face, pinned by a test over a grid of inputs. **A return
  with NO dependent care benefits does not move at all** (line 28 = 0 makes
  the two formulas bit-identical). Separately, every 2441 return with benefits
  **gains printed values** on paper lines 15-17/20/21/23/25/28/29/30 and a
  **corrected** line 31 (it was printing the excluded-benefit amount).
- **✅ s271 #49 and #47 MOVE NOTHING that exists today.** Both new facts are
  absent/NULL on every existing row, and no compute path changed for a return
  that does not carry them. ⚠ But when a payload DOES carry
  `div_unrecap_1250_agg`, note it can change the tax **ROUTE** (QDCGT → the
  Schedule D Tax Worksheet), not just line 19.
- **⚠⚠ s270 #41 MOVES EVERY SC RETURN with taxable income in [$7,000,
  $100,000)** — up to ±$3 of SC tax on next recompute, a CORRECTION toward
  the published SC1040TT (the $50-row assumption was wrong above $7,000).
- **s270 #39 MOVES a class only a new import can create**: a return with a
  nonzero K-1 `collectibles_28` gains Schedule D line 18 and, where the 28%
  rate group binds, more line-16 tax. Zero such returns exist in the shared
  DB today.
- **⚠⚠ s270 #36 MOVES A CLASS: every GA-home-address 1040 with no GA-tagged
  document and no GA-500 gains an auto-attached Georgia return on its next
  save or import.** For GA-resident retirees that is the missing state filing
  — a correction, not a regression.
- **✅ s270 #38 MOVES NOTHING** — `sch2_l14_source_amount` is NULL everywhere.
- **⚠⚠ s269 MOVES DIAGNOSTICS ON EVERY W-2G RETURN, and it will look like a
  regression.** `D_W2G_PAYER_ID` (error) fires on any W-2G row missing the
  payer name, a nine-digit EIN, or the full US payer address — essentially
  every already-committed W-2G return, because none of those fields was
  importable until then. **Those returns genuinely could not have been
  e-filed.** It clears by keying the payer block or re-importing.
  **No tax-output movement.**
- **⚠⚠ s268 cluster 3 MOVES FOUR CLASSES (each a correction):**
  (1) **every Form 8962 return between 300% and 400% FPL on an ODD
  percentage** — the applicable figure was 0.0001 low, so the credit falls
  and the excess-APTC repayment RISES. The largest reach of the cluster;
  (2) a joint GA return with an untagged US-obligation subtraction stops
  shrinking the spouse's RIE (`D_GA500_020` explains); (3) D_GA500_016 goes
  quiet on joint returns where one owner's column is empty; (4) an EIC/CTC/
  AOTC recertification return gains its ticked 8862 part.
- **⚠⚠ s267 MOVES FOUR CLASSES (every one a correction):** a Part-III-only
  8606 + traditional-IRA 1099-Rs regains the taxable on 4b; box-7 code M
  stops blanking the whole pension column; e-file composition now succeeds
  where it refused (8959 high-wage; ISO-dated 8949 rows); the published
  schema accepts `expected.sc1040/al40/nc_d400`.
- **⚠ s257 MOVES MFS RETURNS with a GBC and line 12 > $12,500** (a
  correction) — line 13 rises to the §38(c)(6)(A) threshold unless the
  preparer answers spouse-has-no-credit.
- **⚠ s256 MOVES PRINT + E-FILE OUTPUT on NOL returns** (corrections).
- **s249/s241j MOVE DIAGNOSTICS**: post-2018 alimony instruments fire
  `D_SCH1_007` (error) on BOTH sides.
- **⚠⚠ s243b MOVES THREE CLASSES (each a correction):** basis-only 8606 +
  IRA-path 1099-Rs; employer DCB below the plan cap; GA under-62
  disability RIE.
- Carried from s240/s239/s236/s235: passive/PTP K-1 §1231 losses fire RED;
  Roth 1099-Rs move 5a/5b → 4a/4b; GA partnership K-1s move RIE L2↔L13;
  code-U un-blanks the pension taxable column; GA RIE line 13 on suspended
  passive K-1 losses; GA dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted
- **`--reuse-db` cross-module contamination**: `test_backentry_cleanup.py`
  (3, s225) and `test_mappings.py::TestApplyMappingAmbiguousFederalReturn`
  (3, s239).
- ⚠ **`test_backentry_oos_states_s258.py::TestCleanupDisposition` (2) fails
  in a backentry sweep and PASSES alone** — the same cross-module isolation
  class. Reconfirmed in s272 (both cleared in isolation; neither s271 nor
  s272 touches OOS states or cleanup).
- **`test_1040.py` — 6 pipeline tests**, unscoped `_fv` `.get()` (s234).
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  (s219). `test_4868.py` (4) — ⛔ KEN (s217).
  `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  PTET-gate class (s212).
- **⚠ PRE-EXISTING 1120-S defect (s241o)**:
  `test_line_key_registry_sweep.py::test_formula_targets_resolve` —
  `FORMULA_REGISTRY["1120-S"]` targets `M2_DIST_EXCESS` / `L24_BOOK_BRIDGE`,
  neither seeded. 1120-S only. Deserves its own unit.
- **Client typecheck**: green under `npm run typecheck` (s265).

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB
  (`test_postgres`), cross-repo.
- **⚠⚠ A `-k` sweep over all of `tests/` is ~5 HOURS here.** Scope a sweep to
  the actual blast radius (a diagnostics change = the 76 files matching
  `run_diagnostics`, ~22 min).
- **⚠ NEVER pipe pytest through `Select-Object`** — it buffers the whole
  stream and a timeout loses ALL output. Redirect to a file and tail it.
- ⚠ **NEW (s271): after restoring a file from a `.bak`, BUMP ITS MTIME and
  drop the stale `__pycache__` `.pyc`** — a restore that leaves an older
  mtime than the cached bytecode makes pytest keep running the INJECTED code
  and reads as "the restore failed".
- ⚠ **NEW (s271): a `poetry run python -c @"..."@` heredoc can silently NOT
  RUN.** Write the script to the scratchpad and run the FILE; verify the
  effect before trusting it.
- `--create-db` does not reliably drop here; prove a pre-existing red via
  `git worktree` at a pristine SHA with the main venv + copied `server/.env`.
- `poetry run python > file` BUFFERS (use `-u`); **never rewrite a UTF-8 file
  via `Set-Content`/`Add-Content`** — use the Write/Edit tools or Python io.
  ⚠ `Measure-Object -Line` miscounts here — verify with `git diff --stat`.
- **`poetry run` only works from `server\`**. Windows `python` cannot read the
  Bash tool's `/tmp` — use the scratchpad.
- `manage.py seed_rules` against the pooler takes >5 min — background it.
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes are `/api/v1/tax-returns/…` with the
  trailing slash; `filing_status` is `"mfj"`.
- `_finding(...)` kwargs land under `["details"]`; `ScheduleF` has no
  `business_name` (use `principal_activity`); `order_by("owner")` puts
  "spouse" before "taxpayer" (s241w).

### 🔎 Carried for triage — NOT claims
- **From s268**: after the memo cache, **1,604 queries per run remain across
  957 rules** — no single hot spot left. Levers: more loaders on `run_cache`,
  or the gunicorn worker timeout (Ken's call).
- ⚠ **The ~9s/packet production figure is still ARITHMETIC, not a
  measurement.** Time a real production run when convenient.
- **From s241o**: RIE L8 alimony underived; a fuller L10 derive needs a design
  pass (per-owner attribution + the (4)(b)2 gambling carve-out).
- **From s241**: `Form8606`/`HSAAccount` allow duplicate owners and their
  computes ITERATE (double-count, not vanish); browser POST unguarded.
- **From s234, potentially large**: a materially-participating 1120-S K-1's
  $250k nonpassive ordinary income never reached Schedule 1 line 5 / AGI.
  Repro: `server/tests/test_8960_line4b_clamp.py`, `_build(k1_ordinary="250000")`.

---

## ⛔ KEN DECISIONS OUTSTANDING

### ✅ RULED 2026-08-20/21 — #11, #44, #50 (kept one line each; detail in DECISIONS.md)
- **#11 half A**: no 1120-S retirement feed; `D_K1_SCORP_SERET` warns (Pub 560). Built.
- **#11 half B**: materially-participating S-corp income is **EARNED** for the GA RIE.
  Ken ruled unearned on 08-20 and REVERSED on 08-21 — the packets that filed it
  unearned are all TaxWise, and TaxWise is wrong. **No code; engine was right.**
- **#44**: the ≤$1 source-defect rule applies. Record-only close.
- **#50**: the AL booklet makes the proration mandatory; engine right. Record-only close.

### Carried
- **⛔ KEN (s230)**: Form 6765 Section G required for TY2026+ — re-author
  before a TY2026 season.
- **1040 v5.4 business rules still not in hand** (v5.4 schemas ARE on disk).
  ⚠ s240/s241w read the **v5.3** rules — re-check `S1-F1040-118-01` and the
  `SH-F1040-*` family against v5.4 on arrival.
- **1120-S Inbox still holds THREE for Ken** (see `SOURCE_DECISIONS_NEEDED.md`):
  180 (Lacerte negative-AAA override), 214 (mixed-entity PDF), CATALANC
  (trailer contribution). *(227 needs a 6765 spec, not a source answer.)*
  ⚠ **170 is a BUILD ITEM, not a held packet** — the GA-600S Schedule 7/8
  adjustment must not treat federal §179 as a Georgia difference for TY2025
  (HB 1199 conformity). Verify the mechanism first.
  ⛔ 17a (TaxWise report) · ⛔ 17d (WO-33) unchanged.

### RS AGENDA
- ⚠ **NEW (s272): `FA-1040-SCHF-04` needs re-export** — it was a source-text
  grep for a literal `.update(` call the leg-1 refactor replaced; the app-side
  assertion now pins the new shape plus the filed-wins guard.
- ⚠ **NEW (s272): Alabama Form 40NR has NO SPEC** (`AL_FORM_40NR` 404;
  `AL_FORM_40` 200 proves the convention) — blocks BATCH-296 #52 entirely.
- ⚠ **NEW (s272): the `FORM_2441` spec needs three amendments** — (a) its
  `line_map` is on the PRE-2025 face (no 16/17/26/28/29/30; "31" described as
  "excludable/deductible benefits", which on the 2025 face is line 28);
  (b) `R-2441-EXPENSE-CAP`'s formula is a shorthand valid only when column (d)
  carries the GROSS figure — the face's rule is `line 3 = line 31 =
  min(29, 30)`; (c) flow assertion **FA-1040-2441-05** was corrected in-app
  and needs re-export. ⚠ Its scenario **2441-T4 still passes** — the spec's
  tests never exercised the case that was wrong.
- ⚠ **NEW (s271): Form 4136 has NO SPEC** — blocks BATCH-296 #48 entirely.
- (s270) `w28_4`'s line-map note, `R-K1-RED-DEFER`, and `FA-1040-K1-07`'s
  `blockers` all still name `collectibles_28` as deferred; the FA gate carries
  a `__COMPUTES__` sentinel until the amendment lands.
- (s270) the RS `SC1040` scenario still pins **2,360** — the published table
  says **2,361**.
- (s270) the NC `D400` spec defines no part-year residency DATES.
- **(s242x) The TEN staged FA definitions**: FA-1040-8853C-01..05 + FA-4562-
  DEST/ROUND/280F + FA-1040-2210-08/09.
- (s241b, reaffirmed s244): the `8862` spec collapses each PART to one
  boolean — re-author per-line from the Rev. 12-2025 face. ⚠ The seeded app
  face still carries a `part_v` pseudo-line the Dec-2025 revision DROPPED.
- (s241w): `SCHEDULE_H` is a DRAFT covering 7 of ~27 lines.
- (s241s): the GA QEE credit has NO SPEC. (s241p): `4547` / `8879_TA` none.
- (s241o): the `500` spec has no rule for what feeds RIE lines 1/2/6-13.
- Carried: `5329` roll-forward silence; `R-8582-MULTIFORM` stale cite +
  `4797` K-1 §1231 silence; `R-RET-CODE` outrun ×3; `8379` draft;
  `R-SCHA-CHARITABLE` buckets + RIE-13; SCHEDULE_A carryover aggregation +
  `500` line 7a typed `input`; the 1065 K-1 box-15 letters (still URGENT).

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
