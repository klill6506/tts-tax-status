# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-25 (s287 — Ken's live-review day: OPM round 2 +
the 17-item punchlist, 11 shipped / 6 queued).*

*⚠⚠ RESUME POINT — **work `KEN_PUNCHLIST_2026-08-25.md`, the queued
items: 7, 10, 14, 15, 16, 18** (each carries a design note in the file;
18 first needs Ken to name the return where the 8995 looked dead). All
other punchlist items are BUILT, deployed and status-annotated in that
file. Five deploys today, all Render-verified live; head `51732fb`.*

*① Morning pair: every return editor opens with the form pane +
diagnostics dock CLOSED (ClientHeader buttons / F6 open them); 1065
Schedule B Q1/Q2a/Q2b AUTO-ANSWER from the return (B1 entity type from
the name designator else non-default roster; 2a/2b from the SAME
schedule_b1_partners analysis that attaches B-1; the B4/B11
derived-overridable recipe; R-B1-AUTO/R-B2-AUTO queued for RS).
Verified on the live comparison return: B1=domestic_llc, B2b=Yes.*

*② OPM round 2 (all five): D_B33_PR guard built + the PR block keyed
from the filed designation; the Analysis routes EVERY partner of an
LLC-answered return to row 2b (i1065 p.60 — the s286 partner_type
routing sent a general-keyed member to 2a); the 8825 line-1 table
REPAINTED to Lacerte proportions (f8825_layout.py — (a) 272pt, (d)/(e)
57pt, both pages); client numbering ignores the ≥90000 SYNTHETIC band
(the s259 test client at 99259 had max()+1 handing real clients
99260/99261 — both renumbered, 4796/4797, next=4798); 4562 Part V col
(d) reconstructs GROSS cost (cost_basis is net-of-prior per R018 — the
s285 fixture keyed gross and could not see the empty cell), (e) blank
when fully expensed, lines 28/29 print literal 0 via same-box _ZERO
text aliases.*

*③ Punchlist tranche 1: **4562 line 17 ALWAYS prints — Ken ruling
REVERSES the s285 item-0 listed-only gate** (retired to a tombstone);
1120-S K-1 item C = "e-file" (i1120s p.25 verbatim; the hardcoded
"Ogden, UT" was the paper answer), item D = Σ shareholder shares, item
F3 = new Shareholder.entity_type (§1361(b), default individual, mig
0355 + screen select); the Lomax screen-vs-print divergence root-caused
(computed L24d heals on open/print while an open screen holds stale
values — the Forms tab now refetches after rendering; data verified
consistent: ending AAA = ending RE = 5,000); Taxpayer.email + phone on
Client Info and printing in the 1040 signature block (mig 0356);
"New Employer"/"K-1 n" placeholders removed.*

*④ Tranche 2: 1099-R box 16 SUGGESTS the federal gross distribution
(ghost/Enter-accept — the W-2 box-3/5 convention, extended on Ken's
explicit direction); **the tips attestation DEFAULTS ELIGIBLE** (Ken
item 17 — W-2 box-7 tips deduct out of the box; untick = SSTB/
non-listed opt-out; mig 0357 also flips stored False rows — they were
overwhelmingly the untouched old default; D_SCH1A_003 still flags
deliberately excluded tip dollars).*

*⑤ Gates: every tranche ran green (largest batch 1,441 incl. flow
assertions; finals 235/49/52 + typecheck + 1,062-test slate suite in
the morning). Migrations 0355-0357 applied to the shared DB; entity
back-entry schema regenerated; D_B33_PR seeded (deploy seeds itself;
check_rule_paths 1,039 rows clean).*

*▶ AWAITING (carried): AL 40NR scenario-G TRS/ERS exempt-listing ruling
(Ken; peer-verified booklet reading in s286 STATUS); entry-lane token →
40NR variant path; `GA_OCGA_48_7` (GA-500 S3-9 seed); 1040 BATCH-012/296
held; Jason Houston 1040 shell (Ken); item 18's example return (Ken).*

*⛔ KEN — outstanding (carried): entity second-state-face transport (#3);
`OVERRIDE_HONORED_STATE_LINES`; 146-packet re-export; NC/CA/SC
linked-state reopens; #6 1065X/AAR; #68 optimizer; s274 PII narrowings;
RS 8990 re-authoring gate; 6765 Sec G; client-4545 D_8606_BASIS_ONLY;
1065 BATCH-004 #4 (Codex Box-2); Analysis line-2 active/passive proxy
(REVIEW_QUEUE — visible on the comparison return: our column split
follows the SE keying, Lacerte printed all-active).*

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
holder. Peers stage; Ken decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s287)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s287, run after every client tranche).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): source-inspection flow assertions slice the NEW file with the
  OLD import's line numbers — five phantom failures. New files/markdown/
  scratchpad only during a run.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c` (bit again in s287).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ Embedded double-quote in a here-string
  arg to a NATIVE exe SPLITS the argument (`git commit -F`).
  ⚠ A long @files arg list overflows the command line — relative paths.
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height — position pins judge by the word TOP
  (s286); an IRS field map guessed from SHORT widget names silently
  no-ops — dump the full paths (s287, the Analysis cells under
  Table_Line2/BodyRow).

## 🔎 Carried for triage — NOT claims
- (s287) The 8825 line-1 repaint covers the LINE-1 table only; further
  8825 column tweaks ride f8825_layout.py.
- (s287) The suggested-field convention now covers W-2 3/5 + 1099-R
  box 16 (Ken-approved extension) — CLAUDE.md's W-2-only note is stale.
- (s285) Sch 4 nonresident arm still apportions the whole widened base
  (S2_2 not backed out; zero on the comparison bed) — IT-711 read when
  a return carries S2_2.
- (s283) The stamp excludes 1040 packets (name+SSN privacy — Ken).
- (s282) `OVERRIDE_HONORED_STATE_LINES` hand-synced; third member ⇒
  derive both sides from one declaration.
- (s281) OOS-state line-18 prompt diagnostic specified, not built. ·
  Stage allowlists `schd_fields` keys, `ga500_fields` not at all.
- (s268) 1,604 queries/run + memoization candidates.
- (s241/s281) `Form8606` unique-constraint candidate · 🔴 `HSAAccount`
  half CLOSED.
- (s275/s281) `.first()`-on-per-form-rules sweep remainder.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried + s287 adds:
Everything from s277–s286 stands (R-K1-20N; R-GA700-PARTNERS income
base; AL_FORM_40NR amendments; GA-500 S3-9 blocked on `GA_OCGA_48_7`;
R-AL-TAX mechanism; D-36 reads). **s287 ADDS: R-B1-AUTO / R-B2-AUTO**
(1065_B Schedule B Q1/Q2a/2b derivations — built from i1065 p.25/p.60,
spec authorship queued) and **the 4562 line-17 ruling reversal**
(the listed-only Parts I–IV suppression is retired — Ken 2026-08-25).
Spec-fetch gate satisfied live for 1065_B / 1065_PAGE1 /
SCHEDULE_K1_1065; flow assertions green in every tranche.
