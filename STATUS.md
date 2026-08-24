# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-24 (s285 — parity queue items 0/7/8/13 built, item 2
verified).*

*⚠⚠ RESUME POINT — **s285 WORKED THE LACERTE PARITY QUEUE** (deploy
verification: check Render status if this file is stale). Items 0, 7, 8, 13
built + items 2 and 7 answered; full readings + rulings in
`LACERTE_PARITY_QUEUE.md` s285 annex (repo root).*

*① **THE 4562 FIELD MAP WAS OFF BY ONE WIDGET FROM LINE 8 THROUGH 23** —
the map skipped line 7's widget (f1_15), so on EVERY 4562 the app ever
printed: line 12 (§179) printed in line 11's box, bonus (14) in the
carryover box (13), prior MACRS (17) on line 16 — the mislabeled cell Ken
caught on the 3444 Lacerte compare — the listed total (21) on line 17, the
grand total (22) on line 21. The compute was never wrong; the values landed
one row early. Fixed; line 23 split to the 2025 face's 23a/23b; POSITION
pins added (label-row anchoring, provably red under the old map, 41pt).
Lesson generalized from the s284 7004 swap: scalar chains in field maps get
coordinate pins — a name-existence check can never catch a shift, because
every wrong name still exists.*

*② **Item 0 ruled (i4562 2025 + i8825 Rev. Dec 2025)**: line 16 = ACRS/
pre-MACRS only; continuing MACRS = line 17; AND a 4562 that exists only for
Part V listed property leaves Parts I–IV EMPTY (rental depreciation is
claimed on 8825 line 14 alone) — built as `print_parts_i_iv_suppressed`,
narrow (no listed property ⇒ no gate; any §179/bonus/current-year/line-16
trigger reopens). MeF per-activity IRS4562 deliberately not gated (line 17
ties each activity's DepreciationAmt link). **Item 7**: the 3444 4562 IS
required — the 8825 claims 4,358 auto/travel (the "vehicle deduction on a
non-Schedule-C form" trigger); that is why Lacerte fills Part V Section A/B
and leaves I–IV blank.*

*③ **Item 8 built**: Part V line 26 carries cost / bus% as "60.00" (face
pre-prints %) / basis-for-depr (cost × bus%) / period / "S/L-HY" / §179
col; **24a/24b default Yes** when listed property exists (Ken A2;
orientation coordinate-verified). Deferred (DEFERRAL_AUDIT): the keyed
override input (D_4562_24A/24B seed pattern). **Item 13 fixed**: GA-700
Sch 4 income base was 1065 page-1 line 23 ONLY — $0 on every rental
partnership; now Schedule 8 line 12 less S8_5 GP (GP re-added per partner,
IT-711 pp.11-13), fallback to the federal line when S8 never computed;
3444 prints 29,437/partner. **Item 2 verified**: entry gap — 3444's
B33_PR_* all empty; render leg fine (p4 widgets mapped). Entry lane must
key the B33 block when the packet shows a PR.*

*④ Evidence: 13 new tests in `test_parity_4562_lacerte.py` (gate render ×3,
Part V columns, 24a/24b, derivation unit pins ×7) + 3 position-pin tests +
the rental Sch 4 case in `test_1065_ga_package.py`; injection: position pin
proven red under the old map. Spec gate: 4562 + ga700 RS exports fetched
live (line semantics agree; no compute change — flow assertions skipped
WITH REASON: render/field-map only). ⚠ RS AGENDA add: R-GA700-PARTNERS
under-specifies the income base ("ordinary" — its worked example has only
ordinary income); amendment queued.*

*▶ NEXT (fresh session): **CONTINUE `LACERTE_PARITY_QUEUE.md`** — remaining
items: 1 (typography/darker/more-lines theme + 1065 header box + the
continuation-header restyle — build toward substitute forms per Ken A1),
3 (Analysis of Net Income, 1065 p6), 4 (8825 two-line address), 5 (K-1 20N
§163(j)), 6 (Statement A restyle), 9 (dep schedule redesign, four-column
179/SDA split per A4), 10 (GA-700 header/face gaps), 11 (vendor tag), 12
(blank-page skip). ⚠ Ken may have appended more items — re-read the queue
file first. Client 3444 is the comparison bed.*

*▶ AWAITING (carried): entry-lane token (Ken) → client-4569 restage + 40NR
variant path; `GA_OCGA_48_7` ownership call (Ken) → GA-500 S3-9 seed; 1040
BATCH-012 / BATCH-296 stay in `CC Changes` (built/held per s281/s282).*

*⛔ KEN — outstanding (carried): entity second-state-face transport (#3);
`OVERRIDE_HONORED_STATE_LINES` convention; client-2961 AL 40NR source
defect; 146-packet re-export; NC/CA/SC linked-state reopens; #6 1065X/AAR;
#68 optimizer; s274 PII narrowings; RS 8990 re-authoring gate; 6765 Sec G;
client-4545 D_8606_BASIS_ONLY; 1065 BATCH-004 #4 (Codex Box-2).*

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

## ⚠ Known red / rotted — THE ONE LIST (post-s285)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s281). No client changes s282-s285.

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\` (the Bash tool prints NOTHING for poetry one-liners — use the
  PowerShell tool). ⚠ `poetry run python -c "<multi-line>"` via the
  PowerShell tool can bind wrong — script FILES, not inline `-c` (bit
  again in s285).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8; grep the DIFF for mojibake after any shell
  touch. ⚠ Embedded double-quote in a here-string arg to a NATIVE exe
  SPLITS the argument — long native args via a FILE (`git commit -F`).
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed —
  suspect ENCODING (UTF8.GetBytes + charset).
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.
- 🔧 ⚠ pytest-randomly NOT installed (s281). Test order is not randomized.
- 🌐 ⚠ (s283) **pymupdf `get_text()` does NOT reliably surface AcroForm
  widget VALUES** — a filled-forms coverage scan must also read
  `page.widgets()` or it under-reports.

## 🔎 Carried for triage — NOT claims
- (s285) Sch 4 nonresident arm still apportions the WHOLE widened base by
  the S7 ratio and adds S2_6×profit% — allocated-everywhere income (S2_2)
  is not backed out of the apportionable share. Zero on 3444 (S2_2=0);
  needs an IT-711 read when a return carries S2_2.
- (s284) **GA-700 prints all 8 DOR pages; Lacerte prints 4** — parity
  item 12 (queued). K-1 supplemental/Statement-A formatting parity
  unreviewed (item 6).
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

## RS AGENDA — carried + s285 add:
Everything from s277–s282 stands (AL_FORM_40NR amendments; GA-500 S3-9
seed BLOCKED on `GA_OCGA_48_7`; R-AL-TAX mechanism; D-36 reads for
TN_FAE170/SC1120). **s285 ADD: R-GA700-PARTNERS income-base amendment**
(base = Schedule 8 line 12 total less GP, not "ordinary" — the p.13
worked example never exercised a rental partnership). s283–s285 touched no
compute; spec-fetch gate satisfied live for 4562 + ga700 (line semantics
agree; render/field-map changes only).
