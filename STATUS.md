# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-26 (s296 — extractor session 4, unit 1 + one
batch item. ① The Schedule D extractor unit + the dividend aggregate
valves (`0b79525`): sch_d faces parse; face 3a → `div_qualified_agg`;
no-Sch-D positive 7a → `div_capgain_dist_agg` (the i1040 line-7
distributions-only exception); carryovers/1a-8a/QOF feed their existing
keys; generational-suffix shell matching. r9 census: **9 emitted / 258
refused, tie probe 9/9 TIE** (one NEW valve-only emit — no Sch D page in its packet). ② BATCH-296's newest entry-lane item —
**Form 5329 Part IX RC reasonable-cause waiver, built end-to-end**
(`0908f73`, migration 0363): compute taxes the un-waived remainder
(i5329 p.10 + IRS5329.xsd "excluding any tax calculation"), staging
refuses by name, D_5329_007/008/009, print prints -0- + the "RC
(amount)" dotted-line overlay + the explanation statement page, MeF
emits 54a/54b at value 0 as the RC-attribute carrier + the
WaiveTaxOnExcessAccumQRPStmt, both screens keyed. Published schema
REGENERATED (rc_waiver keys verified). Annex in BATCH-296.*

*⚠⚠ s296 method fact (the s295 lesson, now measured corpus-wide): with
the coverage gate opened for EVERYTHING, only 13 of 344 packets have no
deeper refusal. The TRUE demand ranking of survivors: sch1 line 8 (189
packets, 14 solo) > 7a (147) > 3a qualified (146) > 13b deduction (128,
9 solo) > sch1 line 10 (97) > 2b (84, Ken-gated) > mixed-ownerless (73,
design refusal) > EIC/dependent parse issues (71). The depth-probe
scripts live in the s296 scratchpad (probe_depth_all.py /
rank_survivors.py); depth_probe.json is the corpus snapshot.*

*▶ NEXT — extractor session 4 continues (or 5 opens): ① **sch1_p1 face
parser + line-8/10 component routing** — the top measured class (189
packets touched; 14 freed by line-8 alone if its components route; check
each 8a-8z/24-line component against vocabulary before building — 8h
jury duty (#51 BUILT), 8z other_income_items, unemployment g_1099s
exist). ② the 13b deduction class (9 solo — tips/OT/car-loan Schedule
1-A inputs when no filer is 65+). ③ f8949 summary rows →
`capital_transactions` (frees the f8949-solo packet + pairs with sch_d for
three combo packets). ⛔ Ken still gates the 18-packet int/div
no-source-page class (2a/2b/3b consolidated rows — REVIEW_QUEUE s295;
3a/7a are NOW EXPRESSIBLE via the valves, so the remaining ask is
narrower than written: payer-less rows for taxable interest + ordinary
dividends only). THEN the 50-of-John's pilot. Then defects by cost:
item-59 −487 residual + S1-13 double-landing (hold b926) · item 84
(§469(i)) · #60 · #43-medium · #70 · #16.*

*Entry lane: BLOCKED on Ken's auth mint (production session cookie
expired ~08-26 morning; their session has asked him). Queued: the L012
re-stage (recipe in the BATCH-296 s295b annex) + L013
(5329 waiver — recipe in the s296 annex; ⚠ they must RE-PULL the
published schema first). The GA −1 records as tie_with_exception citing
item B's annex (agreed with the lane in-session).*

*States lane (live tonight): carries BOTH RS amendments queued from the
build side — R-SE-L2 (s295b) and R-5329-11 (s296, waiver clause; the
draft spec has no waiver and is behind the app) — authoring them and
putting them to Ken with the app-shipped-first framing. They also
volunteered a check whether R-SE-L2 and the 1065_SE spec describe line
2 consistently.*

*⛔ KEN remaining: **the s295 int/div ruling (now narrowed — see NEXT
①)** · the auth mint (blocks the entry lane) · #21, #48 (RS 404), #56,
#63, #69, #10 — the tail tier. Carried: entity second-state-face
transport (#3); `OVERRIDE_HONORED_STATE_LINES`; 146-packet re-export;
NC/CA/SC linked-state reopens; #6 1065X/AAR; #68 optimizer; s274 PII
narrowings; RS 8990 re-authoring gate; 6765 Sec G; client-4545
D_8606_BASIS_ONLY; 1065 BATCH-004 #4 (Codex Box-2); Analysis line-2
active/passive proxy; the unfloored 8960 line5 §1211(b) question; the
s292 PII-history scrub question; dep_released_by_form_8332 build
(REVIEW_QUEUE).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.
  (The 2026-08-24 refinement covers COMMIT MESSAGES only — this file stays strict.)

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS (WHEN BUILDS RUN)
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax` (srv-d6geloa4d50c73el2trg); demo = `tts-tax-demo`. ⚠ **A push
is not a deploy — CHECK THE DEPLOY STATUS after pushing** (API key in
`D:\dev\Passwords & Secrets\render-api-key.txt`). **Standing push
authorization (Ken 2026-08-23): push at own judgment; verify every deploy;
hold only for a named reason.** ⚠⚠ **ORDERING (s279/s282): push → deploy
LIVE → seed → verify — and the deploy ITSELF seeds (`build.sh seed_all`
auto-discovers `seed_*` at BUILD time). Manual post-deploy seed = the
idempotent VERIFY; `check_rule_paths` is one command.**
- s296 deploy: `0b79525` + `0908f73` → `dep-da7darbtqb8s73a211t0`
  **LIVE, API-confirmed 2026-08-26**; post-deploy probes green:
  rc_waiver columns queryable in prod (0 rows keyed, as expected),
  D_5329_007/008/009 seeded + active by the deploy's own seed pass,
  `check_rule_paths` 1,042/1,042 resolve.
- ⚠⚠ **STANDING: `scripts\gen_backentry_schema.py` (and the entity twin)
  are LOCAL generators the deploy NEVER runs** — any session that touches
  lane vocabulary, allowlists, or a state seeder MUST regenerate the
  published schema as a close-out step. (s296 regenerated it —
  `rc_waiver_*` advertised.)

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder — coordinate EXPLICITLY before every run. s296 coordinated at boot:
both peers confirmed the tree and test_postgres free; both RS amendments
relayed LIVE to the states lane; the entry lane's auth block + queue state
recorded above. Peers stage; Ken decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s296)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s296 — ran clean with the 5329 screen changes).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): five phantom failures. New files/markdown/scratchpad only.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c` (one-liners only; bit AGAIN s296).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ Embedded double-quote in a here-string
  arg to a NATIVE exe SPLITS the argument — use `git commit -F -` with a
  bash heredoc. ⚠ `$1` in an unquoted PS arg EXPANDS to empty —
  single-quote TaxWise code tokens. ⚠ PS `Sort-Object -Unique` on a
  one-element pipeline UNWRAPS to scalar — `@(...)`-coerce before
  `.Count`/indexing (s296).
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); synthetic `insert_text` PDFs may
  LOSE leading spaces (s290) — the extractor parses POSITIONALLY everywhere.
  ⚠ **The AcroForm FILLER flattens ALL widgets out of its output** (s296) —
  post-render anchoring must read the pristine TEMPLATE's widget rects.
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Download the PDF and extract the section. Local templates beat both.
- 🔧 ⭐ **An instrumented ROLLED-BACK dry-run reproduces production behavior
  locally** (s289; s292-s296 ran whole tie probes this way — staging INSIDE
  the rolled-back transaction too). ⚠ Scripts touching client-named returns
  live in SCRATCHPAD or tax-test-data, never the repo (PII).
  ⚠ `Firm.objects.first()` is the DEV firm — probe with
  `Firm.objects.get(name="The Tax Shelter")`. ⚠ Pooler statement timeouts
  kill the connection and the reconnect can flake DNS — ping + retry loop.
- 🌐 ⚠⚠ **A probe that moves TWO variables is not a probe** (s294) —
  isolate the variable or don't write "probed".
- 🔧 ⚠ **A refusal census "solo" count is an upper bound** (s295/s296): the
  coverage gate's early return hides deeper refusals — DEPTH-PROBE before
  building a unit; the s296 scratchpad has the reusable probe pair.

## 🔎 Carried for triage — NOT claims
- (s296) The 22 sch_d GEOMETRY-error packets from the calibration sweep
  (markers missing/ambiguous on four packets + one non-return fragment) refuse loudly by design — diagnose when one of them
  is otherwise free.
- (s295) The Inbox holds 7 auxiliary PDFs (wage statements/worksheets)
  that refuse as "not a TaxWise 1040 packet" — correct behavior; consider
  moving them to a sub-folder so the census denominator is returns-only.
- (s295) `_summary_lines` GA500_SUMMARY_LINES lacks S1-6 — GA Sch 1
  additions tie only indirectly (via 16/23/30/46). Add if an additions
  no-tie ever needs direct visibility.
- (s290) The GA RIE interest row does NOT include K-1 16A tax-exempt
  interest — stated boundary, not a gap. · The rendered 8995 TIN prints
  UNFORMATTED digits — cosmetic.
- (s289) `IndividualForm7203` has no §179/charitable carryover keying
  fields — D_K1_7203_DEDUCTION_LIMITED warns. · The 7203/K-1 §179 cap does
  NOT extend to 1065 partners.
- (s288) `IndividualForm7203` still has no home for box 16 code E;
  1065 box 18 a/b/c have none on the recipient side.
- (s288) `ctc_override` / `odc_override` + `Dependent.compute_qualifies_*`
  are DEAD — removal candidate (Ken's call; fold with
  `dep_released_by_form_8332`, see REVIEW_QUEUE).
- (s287) The 8825 line-1 repaint covers the LINE-1 table only. ·
  The suggested-field convention covers W-2 3/5 + 1099-R box 16 —
  CLAUDE.md's W-2-only note is stale.
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
  WEIGHTS — pre-existing, noted.
- (s294) A state face left in place by an omitting correction batch is
  NOT recomputed against that batch's federal changes — visible via the
  live-face reconciliation + the named warning.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried:
Everything from s277–s295b stands (R-K1-20N; R-GA700-PARTNERS; AL_FORM_40NR
amendments; R-AL-TAX; R-B1/B2-AUTO; the 4562 line-17 reversal;
R-8880-LINE1-COMPOSE; FORM_7203 Part I line-3 sub-map; R-K1-179-BASIS;
R-GA500-RIE loss-allocation clause; SCHEDULE_K1 box 16 A/B routing;
R-8995-QBI line-1 population widening; FORM_8960 R-8960-INCOME 5b
description; the unfloored line5 §1211(b) question; the 1040X
derived-input amendment + queued `x_is_superseding_derived` app
follow-up; **R-SE-L2 clergy + SE-subject-other-income addends — RELAYED
LIVE to the states lane s296, they are authoring it**). **s296 QUEUES ONE
and relays it live: FORM_5329 R-5329-11 waiver clause** (facts
f5329_rc_waiver_window/_other; 54a/54b subtract the waived shortfall
before the rate; authorities i5329 2025 p.10 + IRS5329.xsd
waiveTaxOnExAccumQRPStmtAmt; the draft spec is behind the shipped app).
