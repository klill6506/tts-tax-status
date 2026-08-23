# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-22 late night (s274, Saturday).*

*⚠⚠ RESUME POINT — **THE STANDING RED IS CLEARED** (Ken's directed step from
the s273 close). Commit `0a82983` pushed on Ken's go; deploy verification
was in flight at close — **CHECK IT LANDED LIVE before believing anything
else** (dep-da54qseq1p3s73avn7n0; API key in `D:\dev\Passwords & Secrets\`).
After the deploy verifies: regenerate the published schema (deploy-time
rule — closes the stale-`form_8829` + SSA-description drift the entry
session found), then build **#78+#80 as ONE deploy** (GA-500 joint-split
conserving splitter — six `(amt/2)` sites, largest-remainder at the
aggregate, TP floor, odd dollar to spouse; the held $1-off packet is the
fixture (its key is in the entry session's hold notes, NOT here);
+ line-5 filing-status derivation with the explicit key as override and
the staging warning). Then #79 (§402(l) PSO — VERIFY statute/Pub 575
first, incl. per-taxpayer-vs-per-plan), #75 (Sch A line-16 MeF statement,
design in the s273 scratchpad), #76 (Credit Limit Worksheet B), #77
(GA-500 line 19 — VERIFY vs IT-511), **#82 NEW** (source_defects cannot
record an ATTACHED-form defect — [client]/Ulp 8829 fixtures, live
demonstration in its addendum), the CTC missing-DOB staging warning, the
shell-lookup disambiguation (city), the cleanup-API unknown-key 400, and
the **staging-guard family**: 1310 decedent ⇒ date-of-death required;
asset `flow_to` with unresolvable `link_key` while a parent exists;
`mortgage_deductible` teaching refusal on the standard-deduction branch
(all cheap pre-commit refusals for facts diagnostics catch post-commit).*

*✅ s274 — THE STANDING-RED TRIAGE, complete (`0a82983`, no migrations):
the sweep's 51 failures + 7 errors across 22 files → **all 22 files green
in isolation; full suite 11 failed + 7 errors of 9,902**, every survivor a
PRE-DOCUMENTED reuse-db/order family: `test_1040` ×6 (s234), the
order-dependent QUINTET ×5 (`test_backentry_cleanup` ×3 +
`test_backentry_oos_states_s258` ×2), `test_mappings` ×7 errors (s239).
Those consolidate into ONE named unit: **TEST-ISOLATION / seed_builtin_rules
leakage** (signature: `diagnosticrule_code_key` + `formdefinition_code_
tax_year` UniqueViolations in combined runs; minimal repro
8829_diagnostics_leg + backentry_cleanup, s273). Classification detail is
in the commit message; per-file citations are in the tests themselves.*

*⚠⚠ s274 REAL DEFECTS FIXED (both deploy with `0a82983`):*
- *THE UNDER-62 DISABILITY RIE IMPORT ROUTE WAS DEAD since the s272
  refusal — -DIS accepted, nothing opened the -APPL gate, exclusion
  silently $0. `compute_ga500.rie()` now gates APPL **or** DIS
  (O.C.G.A. §48-7-27(a)(5)); the views derive mirrors DIS → APPL.
  DECISIONS has the rule. **MOVES**: any committed return carrying
  -DIS with blank -APPL gains its exclusion on next recompute (a
  correction; population ~zero — pre-refusal payloads keyed APPL).*
- *`M2_DIST_EXCESS` + `L24_BOOK_BRIDGE` seeded (the K17b
  silent-persistence class). 1120-S seed = 374 lines/12 sections.
  **MOVES**: DB rows only on next recompute; no face/print change.*

*✅ s274 rulings (Ken, both live): **8829 tier 1 requires ITEMIZING** —
engine right, filed wrong; confirmed DIRECTLY in the entry session (gate
protocol held); Ulp filed `tie_with_exception` with the $424 across twelve
federal+GA lines (item 82's addendum records why twelve lines instead of
the one 8829 line — the gap itself). DECISIONS.md has both entries.*

*▶ ENTRY-LANE STATE (their reports, 2026-08-22 evening): **eleven filed
today**, Inbox 386 / Done 534. Filed proofs live for items 65 (Terrell
Dane), 30 (Hinely), 26 ([client] — its own fixture — and Ulp). Their
corrected backlog numbers: **132 genuinely unheld** (not 314 — 177 holds
live only in `tmp/` notes invisible to an Inbox scan; a healthy share
likely stale post-65/30/25). Item 65's true class is smaller than 66:
a foreign tax credit on a zero-tax return is NOT a blocker (Frazar).
AUTHORING_GUIDE de-rotted s274 (8829/8863/states; now defers to the
generated SUPPORTED-SECTIONS.md).*

*▶ OPEN FOR KEN: **Sims Julian (client, packet held)** — SOURCE RE-EXPORT
request: the invoice itemizes "2 CLERGY WORKSHEETS" that never exported;
§107 least-of-three needs used+FRV, only the designation is inferable.
Also carried: item 37/67's D_6251_005 zero-taxable-income exemption
(addendum covers dividend/K-1 §1250 facts — SEVEN tied returns held),
#18's $486 NIIT, packet 214's PDFs, the 1120-S pre-incorporation trailer,
groups C/D, item 68, SEHI↔PTC. **LA is seeded but NOT cleared to build**
(no S-corp/PTE return — CIT-620 computes AS A C CORP; design conversation
needed before porting the MS/AL pattern).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.
- *The s273 resume pointer and its day-log moved to `STATUS_ARCHIVE.md` (s274).*

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS (WHEN BUILDS RUN)
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax` (srv-d6geloa4d50c73el2trg); demo = `tts-tax-demo`. ⚠ **A push
is not a deploy — CHECK THE DEPLOY STATUS after pushing** (API key in
`D:\dev\Passwords & Secrets\render-api-key.txt`).

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions** — Tax Return Entry
(1040 entry lane) and Delvio-states (state campaign). They route ALL
questions here; Ken answers through this session on their behalf. GATE
EXCEPTIONS stay human end-to-end: RS prod seeds, pushes/deploys, and
tax-treatment rulings need Ken's word DIRECTLY in the acting session —
**proven twice now** (the LA seed; the s274 8829-tier ruling, where the
entry session correctly refused the relay and surfaced its own prompt).
Coordination: ListAgents + SendMessage; ONE delvio-tax tree holder; ONE
pytest/test_postgres holder; explicit-path staging; NO stash on the shared
tree.

## ▶ LANE MAP (Ken-ruled 2026-08-22, s273; unchanged s274)
- **Tax Return Entry (Claude)**: 1040s only, in tandem with this session.
- **Codex**: owns the 1065 entry lane (six pilot packets + the gated
  multistate packet; must NOT re-enter the two FILED returns; re-derives
  payloads from source).
- **Delvio-states (Claude)**: MS/AL verticals merged and live; stands off
  pytest until this session hands a window. LA seeded, NOT cleared (above).
- **This session**: holds the main tree + test_postgres; builds, deploys on
  Ken's go, annexes.

---

## ⚠ Known red / rotted — THE ONE LIST (post-s274)
Everything below is the **test-isolation unit**'s scope; each is
pre-existing, documented, and passes in isolation:
- **The quintet**: `test_backentry_cleanup::TestBackEntryCleanup` ×3 (s225) +
  `test_backentry_oos_states_s258::TestCleanupDisposition` ×2 —
  seed_builtin_rules leakage between modules (proven s273 at `9689a9b`).
- **`test_1040.py` — 6 pipeline tests** — unscoped `_fv` `.get()` (s234);
  reuse-db only.
- **`test_mappings.py` — 7 setup ERRORS** (`TestMappingEndpoints` /
  `TestNoTemplateMessage` / `TestApplyMappingAmbiguousFederalReturn`) —
  FormDefinition unique-violation at fixture setup; the s239 reuse-db
  cross-module class.
- `test_4868.py` (4) — ⛔ KEN (s217); skipped/quiet in the s274 full run.
- **Client typecheck**: green under `npm run typecheck` (s265); untouched
  by s274 (no client changes).

### ⚠ Test-run hazards (standing — unchanged, s274-verified)
- Never run two pytest invocations concurrently (one shared `test_postgres`).
- **`--reuse-db` sidesteps the stale-DB lifecycle errors** (states-session
  calibration, s274-confirmed) but SURFACES the cross-module families above;
  `--create-db` hides them and does not reliably drop. A KILLED mid-run
  pytest leaves a stale DB — launch long runs DETACHED (Start-Process), not
  under a tool timeout.
- A full suite is ~1-2 h. Never pipe pytest through `Select-Object`;
  redirect to a file. `poetry run` only from `server\`. Windows `python`
  cannot read Bash's `/tmp` — use the scratchpad.
- ⚠ msys `tail -f` on a file BLOCKS PowerShell `Add-Content` to it (s274) —
  don't monitor a file another process appends to with tail; poll or use a
  DONE-marker file.
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; `filing_status` is `"mfj"`; return CRUD routes carry the
  trailing slash.

## 🔎 Carried for triage — NOT claims
- (s268) 1,604 queries/run across 957 rules remain post-memo; the ~9s/packet
  figure is arithmetic, not measurement.
- (s241) `Form8606`/`HSAAccount` allow duplicate owners and their computes
  iterate; browser POST unguarded.
- (s234) a materially-participating 1120-S K-1's $250k nonpassive ordinary
  income never reached Schedule 1 line 5 / AGI (repro in
  `test_8960_line4b_clamp.py`).
- (s274, entry session) the Sims shared-policy pair (spouse-side 99%
  allocation) is a natural 8962 allocation regression fixture if wanted.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+, s230) · 1040 v5.4 business rules not in
  hand · 1120-S Inbox: 180 / 214 / pre-incorporation trailer · 170 is a
  BUILD ITEM (GA-600S §179 HB 1199) · 17a / 17d.

## RS AGENDA — carried unchanged from s273 (see STATUS_ARCHIVE):
FA-1040-SCHF-04 re-export · AL_FORM_40NR no spec (#52) · FORM_2441 three
amendments · Form 4136 no spec (#48) · collectibles_28 deferral notes ·
SC1040 scenario pins 2,360 (published table 2,361 — now also the APP's
pinned truth, s274) · NC D400 part-year dates · the ten staged FA
definitions (s242x) · 8862 per-line re-author · SCHEDULE_H draft · GA QEE /
4547 / 8879_TA no spec · `500` spec silent on RIE feeds **and on the
DIS-opens-the-gate rule (s274 — amend when the RIE rules are authored)** ·
1065 K-1 box-15 letters (URGENT).
