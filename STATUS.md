# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-24 (s286 — the Lacerte parity queue is DRAINED).*

*⚠⚠ RESUME POINT — **s286 BUILT EVERY REMAINING PARITY-QUEUE ITEM**
(1 typography/header/stamp · 3 Analysis of Net Income · 4 8825 two-line
address · 5 K-1 20N derived · 6 Statement A grid · 9 dep-schedule redesign
· 10 GA-700 header gaps · 11 vendor tag · 12 blank-page skip). Full
readings + rulings in `LACERTE_PARITY_QUEUE.md` s286 annex. Deploy
verification: check Render status if this file is stale.*

*① **Typography**: fills float to 10pt (the MEASURED Lacerte fill —
QuickTypeIICourierA 10.0 — box-geometry caps + shrink-to-fit). 1065
header = the Lacerte one-box block (`header_block.py`, reusable toward
the substitute-forms engine per A1; f1065-only patch registry).
Continuation stamps ride the form's own header line ("Form 1065 (2025)
NAME  EIN  Page 2"). "Delvio Tax" 6pt source tag bottom-center of every
packet page.*

*② **Analysis of Net Income line 2 built** — and the queue's "general
partners" parenthetical was WRONG: i1065 2025 p.60 verbatim, "Report all
amounts for LLC members on the line for limited partners," and the filed
3444 face agrees (58,873 on 2b/(ii)). Split = `_allocate_line_exact`
(sums exactly, the instruction's own requirement); individuals
active/passive via the §1402 proxy (REVIEW_QUEUE note). ⚠ The Analysis
cells nest under `Table_Line2[0].BodyRowA/B[0]` — the first bare-name map
printed NOTHING silently.*

*③ **K-1 box 20 code N now DERIVES** (i1065 p.51): components = page-1
line 15 (+farm F21a/b) interest → "line 1", 8825 interest → "line 2",
split by each carrying line's own convention; by-line statement page
attaches. GUARDS return {} on transcribed 20/N, EBIE 13/K, 8990 ledger
rows, or any BI* value — a §163(j)-limited return is never guessed.
3444 pin: 42,962 at 50/50 → 21,481/partner, matching the filed
statement. **RS AGENDA: R-K1-20N amendment queued** (spec routes the code
but has no derivation rule). Statement A prints as the i1065 GRID
(`statement_a.py`; REIT-dividends + other-deductions rows blank —
DEFERRAL, no model fields).*

*④ **Dep schedule redesigned to Ken A4**: 18 columns with Cur §179 / Cur
SDA / Prior §179 / Prior SDA SPLIT (one better than Lacerte), bordered
page, ruled bands, underlined heads, per-rental-activity groups, and the
derived Salvage/Basis-Redn + Depr-Basis columns (exact against the filed
3444 page: 350,000 / 832,239 / SUV 0) + the ENGINE-sourced Rate column
(never back-divided). **GA-700**: header gaps filled (Original-X, records
city/state, date began, accounting-method + nonresident abs_pos marks —
shared-name option boxes can't ride a name fill; K-1 counts; preparer
signature line; box T blank = stated no-source boundary; the accounting
method reads the FEDERAL row — the state row's default was masking
accrual) and blank schedule pages DROP (word-count diff vs template;
face+signature hard-kept; 3444 prints Lacerte's exact 4 pages).*

*⑤ Evidence: ~60 new tests across `test_parity_header_typography` /
`test_1065_analysis_partner_type` / `test_parity_8825_two_line_address` /
`test_parity_ga700_header` / `test_parity_k1_20n` /
`test_depreciation_schedule_report` (s286 class) + amended
behavior-pinning legacy tests (each with a written reason). Spec gate:
1065_PAGE1 + SCHEDULE_K1_1065 RS exports fetched live; derivations cited
to i1065 2025 pp.51/60 and the R018 basis identity. ⚠ SESSION LESSON:
five phantom flow-assertion failures came from editing source DURING a
detached pytest (inspect.getsource slices the new file with the old
import's line numbers) — never edit `apps/` while a detached run is in
flight.*

*▶ NEXT (fresh session): **the parity queue is empty — check
`LACERTE_PARITY_QUEUE.md` for new Ken appends first** (he is entering
more completed returns), else fall back to BUILD_ORDER. Client 3444
remains the comparison bed; a fresh side-by-side print of the full 3444
packet against the Lacerte PDF is the natural next verification.*

*▶ AWAITING (carried): entry-lane token (Ken) → 40NR variant path (client
4569 is now COMMITTED + FILED per the entry lane — restage cleared);
`GA_OCGA_48_7` ownership call (Ken) → GA-500 S3-9 seed; 1040 BATCH-012 /
BATCH-296 stay in `CC Changes` (built/held per s281/s282). **AL 40NR
scenario-G (client 2961): peer-verified — the 2025 40NR booklet confirms
gross taxable-to-a-resident pension enters Column B; open question is
whether the TRS/ERS exempt-listing reduces the correction from 16,214 to
3,181 (ratio ~39.97% vs the fixture's 30.65%) — Ken to rule; the RS
Part-IV "taxable" vs booklet "taxable-to-a-resident" tension is flagged,
not resolved. Jason Houston has no seeded 2025 1040 shell — entry lane
stopped rather than bootstrap a client (Ken).**

*⛔ KEN — outstanding (carried): entity second-state-face transport (#3);
`OVERRIDE_HONORED_STATE_LINES` convention; client-2961 AL 40NR ruling
(above); 146-packet re-export; NC/CA/SC linked-state reopens; #6 1065X/
AAR; #68 optimizer; s274 PII narrowings; RS 8990 re-authoring gate; 6765
Sec G; client-4545 D_8606_BASIS_ONLY; 1065 BATCH-004 #4 (Codex Box-2);
Analysis line-2 active/passive proxy (REVIEW_QUEUE).*

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
hold only for a named reason.**
⚠⚠ **ORDERING (s279/s282): push → deploy LIVE → seed → verify — and the
deploy ITSELF seeds (`build.sh seed_all` auto-discovers `seed_*` at BUILD
time). Manual post-deploy seed = the idempotent VERIFY; `check_rule_paths`
is one command.**

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder (s283: two runs collided — wait for the background batch). Peers
stage; Ken decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s286)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s281). No client changes s282-s286.

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): source-inspection flow assertions slice the NEW file with the
  OLD import's line numbers — five phantom failures. New files/markdown/
  scratchpad only during a run.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\` (the Bash tool prints NOTHING for poetry one-liners — use the
  PowerShell tool). ⚠ `poetry run python -c "<multi-line>"` via the
  PowerShell tool can bind wrong — script FILES, not inline `-c`.
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8; grep the DIFF for mojibake after any shell
  touch. ⚠ Embedded double-quote in a here-string arg to a NATIVE exe
  SPLITS the argument — long native args via a FILE (`git commit -F`).
  ⚠ A long @files arg list overflows the command line (s286) — relative
  paths, or chunk the run.
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed —
  suspect ENCODING (UTF8.GetBytes + charset).
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.
- 🔧 ⚠ pytest-randomly NOT installed (s281). Test order is not randomized.
- 🌐 ⚠ (s283) **pymupdf `get_text()` does NOT reliably surface AcroForm
  widget VALUES** — a filled-forms coverage scan must also read
  `page.widgets()`. ⚠ (s286) word bboxes span the FULL line height —
  position pins judge by the word TOP, never the bbox bottom.

## 🔎 Carried for triage — NOT claims
- (s285) Sch 4 nonresident arm still apportions the WHOLE widened base by
  the S7 ratio and adds S2_6×profit% — allocated-everywhere income (S2_2)
  is not backed out of the apportionable share. Zero on 3444 (S2_2=0);
  needs an IT-711 read when a return carries S2_2.
- (s284) K-1 supplemental/Statement-A formatting parity now BUILT (s286
  item 6); packet-vs-Lacerte side-by-side of the full 3444 print still
  unreviewed as a whole.
- (s283) The stamp excludes 1040 packets (name+SSN privacy call — Ken).
- (s282) `OVERRIDE_HONORED_STATE_LINES` hand-synced; third member ⇒ derive
  both sides from one declaration.
- (s281) Out-of-scope-state line-18 prompt-shaped diagnostic — specified,
  not built. · Stage allowlists `schd_fields` keys, `ga500_fields` not at
  all. · ⭐ evidence is only about what it could have observed; a sweep
  reports its own coverage.
- (s280) regression fixtures insensitive to their rule — Gate-1 approved,
  execution with the states lane.
- (s279 late) cleanup `source_verified` all-or-nothing; client-4545 ⛔ KEN.
- (s268) 1,604 queries/run + memoization candidates.
- (s241/s281) `Form8606` unique-constraint candidate · 🔴 `HSAAccount`
  half CLOSED — multiple HSAs legitimate.
- (s275/s281) `.first()`-on-per-form-rules sweep — narrowed; remainder:
  multi-instance forms + 9 named seeders.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried + s286 add:
Everything from s277–s285 stands (AL_FORM_40NR amendments; GA-500 S3-9
seed BLOCKED on `GA_OCGA_48_7`; R-AL-TAX mechanism; D-36 reads for
TN_FAE170/SC1120; R-GA700-PARTNERS income-base amendment). **s286 ADD:
R-K1-20N** — SCHEDULE_K1_1065 routes box 20 code N (R-K1-CODED) but
carries NO derivation rule; the built derivation (i1065 2025 p.51:
components by carrying line, EBIE/8990/transcription guards) needs spec
authorship. s286 touched allocator code (20N) — flow assertions run green
in the final batch; spec-fetch gate satisfied live for 1065_PAGE1 +
SCHEDULE_K1_1065.
