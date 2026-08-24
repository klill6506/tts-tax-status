# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-24 (s284 — the three Lacerte-comparison gaps, Ken-ranked
and built).*

*⚠⚠ RESUME POINT — **s284 SHIPPED THE THREE GAPS KEN RANKED** (`96b4da8`,
deploy verification in flight at close — CHECK RENDER STATUS FIRST if this
file is stale; earlier today `d5fbea1`/`3134bf7`/`66d0d09` all Render-API
LIVE). Ken ruled live: build the s283 gaps; and **PII SCOPE REFINED — a
client's BUSINESS name is permitted in COMMIT MESSAGES** (DECISIONS.md;
personal names/SSNs/EINs/addresses still barred; the public mirror stays
fully strict, sync guard unchanged).*

*① **FORM 8879-PE BUILT** (official Rev. Dec 2025 template downloaded +
manifested; 18-field map; `render_8879pe`). Part I = 1065 lines 1c / 3 / 23 /
Sch K line 2 / Sch K line 3c — verified against the downloaded official face,
the same five the Lacerte packet prints. ERO firm + authorize box from the
effective preparer block. Joins the 1065 packet at step 1b (instructions page
skipped) and the invoice's federal list. The partnership packet finally
carries a federal e-file authorization page.*

*② **FORM 7004 — the claim was wrong, the data was the gap.** ⚠ Correction
against s283's own record: the packet gate ALWAYS existed (assembly step 7);
the compared packet lacked the page because `extension_filed` was never set
by the entry. Moved to step 1c (extension rides directly behind the e-file
auth — the Lacerte order, exactly once); invoice lists it under the same
gate. **REAL pre-existing defect found while verifying: the 7004 header map
had f1_7/f1_8 SWAPPED — the ZIP printed in the COUNTRY column on every 7004
ever rendered** (f1_7 = Country x382-489, f1_8 = ZIP x489-576; fixed, pinned
by a position test that is provably red under the old map).*

*③ **THE 1065 PREPARER SIGNATURE/DATE CELLS print** — the IRS template has
no AcroForm fields there; abs_pos literals print the preparer name in the
signature cell (IRS accepts a printed signature; Lacerte prints it) and the
date when `signature_date` is set. Client 3444's packet now matches the
Lacerte original page-for-page through the federal side; final packet PDF
delivered to Ken in-chat.*

*④ **Evidence**: 8 new tests (8879-PE values incl. a negative; packet
inclusion exactly-once + ordering; the ZIP-column position pin; the
signature-cell band), 166 render/filler/print regressions green. DATA on
client 3444: `extension_filed` set (transcribes the filed packet).*

*▶ NEXT (fresh session): verify the `96b4da8` deploy reached LIVE (poller
running at close). Then the carried queue: ⛔ #3 entity transport (Ken);
batch items as posted. Residual Lacerte-parity nits, NOT ranked: our GA-700
prints 8 pages vs Lacerte's 4 (we render the full DOR form; theirs omits
blank schedule pages — a skip-blank-pages pass is a candidate); K-1
supplemental/Statement-A formatting differences unreviewed.*

*▶ AWAITING (carried): entry-lane token (Ken, in their session) →
client-4569 restage + the 40NR packet through the variant path; the
`GA_OCGA_48_7` authority-row ownership call (Ken) → unblocks the approved
GA-500 S3-9 seed; 1040 BATCH-012 / BATCH-296 stay in `CC Changes`.*

*⛔ KEN — outstanding (carried): the entity second-state-face transport (#3,
REVIEW_QUEUE); state-face override-honor convention
(`OVERRIDE_HONORED_STATE_LINES`, 2 members); client-2961 AL 40NR
source-defect disposition; the 146-packet re-export; NC/CA/SC linked-state
reopens; #8 GA-700 Sch 4; #6 1065X/AAR; #68 optimizer; s274 PII items
(⚠ the mirror-history rewrite question — NARROWED by today's ruling: commit
messages may carry business names, so only the mirror-reached PERSONAL-name
instances remain); RS 8990 re-authoring gate; Form 6765 Section G;
client-4545 D_8606_BASIS_ONLY; per-rule cleanup acknowledgment; **1065
BATCH-004 #4 blocked on Codex's Box-2 arithmetic**.*

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

## ⚠ Known red / rotted — THE ONE LIST (post-s284)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s281). No client changes s282-s284.

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\` (the Bash tool prints NOTHING for poetry one-liners — use the
  PowerShell tool). ⚠ `poetry run python -c "<multi-line>"` via the
  PowerShell tool can bind wrong — script FILES, not inline `-c`.
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
- (s284) **GA-700 prints all 8 DOR pages; Lacerte prints 4** (blank
  schedule pages omitted) — a skip-blank-pages pass is a candidate, not
  ranked. K-1 supplemental/Statement-A formatting parity unreviewed.
- (s284) ⚠ **The s283 "7004 never joins the packet" claim was FALSE** —
  the gate existed; the page was missing because the DATA flag was unset.
  Recorded because the correction is the lesson: a missing page names a
  missing FORM only after the gate's INPUT is checked.
- (s283) The stamp excludes 1040 packets (name+SSN privacy call — Ken).
- (s282) `OVERRIDE_HONORED_STATE_LINES` hand-synced; third member ⇒ derive
  both sides from one declaration.
- (s281) Out-of-scope-state line-18 prompt-shaped diagnostic — specified,
  not built. · Stage allowlists `schd_fields` keys, `ga500_fields` not at
  all. · `D_8812_015` fires on 1 live row — CORRECT. · ⭐ evidence is only
  about what it could have observed; a sweep reports its own coverage.
- (s280) regression fixtures insensitive to their rule — Gate-1 approved,
  execution with the states lane.
- (s279 late) cleanup `source_verified` all-or-nothing; client-4545 ⛔ KEN.
- (s268) 1,604 queries/run + memoization candidates.
- (s241/s281) `Form8606` unique-constraint candidate (DB clean; Ken's
  call). 🔴 `HSAAccount` half CLOSED — multiple HSAs legitimate.
- (s275/s281) `.first()`-on-per-form-rules sweep — narrowed (62/71 seeders
  guarded); remainder: multi-instance forms + 9 named seeders.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried:
Everything from s277–s282 stands: the AL_FORM_40NR diagnostic-condition
amendments; the GA-500 S3-9 seed BLOCKED on `GA_OCGA_48_7` ownership;
R-AL-TAX mechanism amendment staged; D-36 reads for TN_FAE170 / SC1120 in
progress on the states lane. s283/s284 touched no spec (render-only; the
spec-fetch gate skipped WITH REASON: no computation or line-mapping change).
