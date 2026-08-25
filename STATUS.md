# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-25 (s288 — the punchlist queue drains: items 7, 14, 15,
16 shipped; 10 half-shipped; four questions staged for Ken).*

*⚠⚠ RESUME POINT — **nothing is mid-build. The next move is Ken's.**
`REVIEW_QUEUE.md` holds FOUR staged decisions, each with evidence, a design and
a recommendation: ① Form 7203 Part I 3g/3h route by an old face layout and
§1231 gain increases basis by NOTHING (every S-corp shareholder, overstates
tax); ② should K-1 box 16 code A also land on 1040 line 2a (it feeds §86
provisional income, so it can move tax); ③ punchlist item 10's EIC
default-eligible + AOTC picker halves; ④ retire `D_1040_008`? Also still open:
punchlist **18** needs Ken to name the return where the 8995 looked dead.
Head `813da73`; three deploys today, all Render-verified.*

*① Item 7 — PY-fact ink. A `priorYear` OVERLAY on FieldStateInput
(`.slate-field.is-pyfact`, muted, still editable), deliberately NOT a sixth
field state: the five states describe PROVENANCE, PY-ness is what a field
MEANS. ⚠ Distinct from `is-pyimported` (a snapshot value not yet shadowed,
which switches off when you type); this one is permanent. Applied across the
whole inventory — Schedule L BOY + M-2 beginnings, partner J/K1/L beginnings,
shareholder Shares BOY, 7203 lines 1/16/21, prior/bonus/AMT/§179 depreciation,
and ~20 1040 carryover surfaces. One shared predicate
(`slate/priorYearField.ts`); 30 tests pin the NEGATIVES ("priority_code" must
not gray). State/local tax PAYMENTS excluded on purpose — a prior-year balance
paid this year is a CURRENT-year §164 payment.*

*② Item 15 — Schedule 1 split into "Pt I — Add'l Income" / "Pt II —
Adjustments". `sch_1` keeps its id (stored last-tab state + the D_SCH1_ route
stay valid); D_SCH1_005 gets an exact key to Part II, the alimony family
straddles both parts and stays on the default.*

*③ Item 16 — the Roth entry point. ⭐ The Roth box is NOT a new field: it writes
`RothIRABasis.current_year_contributions`, which already existed per owner and
already becomes next year's basis at proforma (one path per fact). Form 8880
line 1 now SUMS its three statutory components — the 2025 face names
traditional + Roth + **ABLE** — and the old combined box became the line-1
OVERRIDE (nonzero wins, zero derives: line 2's own recipe on that form), which
is what makes it safe on live data. Migration 0358. ⚠ D_8880_002 was rewired to
the compute helper in the same pass — reading the raw field, it would have gone
SILENT on exactly the returns the new box creates.*

*④ Item 14 — K-1 box 16 A/B → Form 7203 line 3k (migration 0359). Not an
unwired input: the shared engine has computed 3k = K16a + K16b all along and
the 1040 side never supplied them, so ending basis ran LOW and §1366(d)
disallowed real losses (§1367(a)(1)(A)). Codes C/D were already live and were
NOT duplicated; E stays unmodeled. A stale field-map caption ("3k = §1374(b)(2)
built-in gain") sent me to the template, which is how finding ① surfaced.*

*⑤ Item 10 — CTC/ODC columns dropped from both dependent grids (they were
read-only echoes of the classifier; nothing computed changed). The EIC and AOTC
halves are designed in REVIEW_QUEUE, not built: EIC default-eligible reverses
the EIC spec's own DoD and moves credits on stored returns, and the all-people
student picker AOTC needs does not exist yet.*

*⑥ Gates: 1,525 client tests + typecheck green; server green on every touched
lane (562 for the 8880 unit, 611 for 7203/K-1, 346 for the 1040 spine, 169
back-entry); 526 flow assertions green. Migrations 0358/0359 applied to the
shared DB; published back-entry schema regenerated. Every new visual/behavioral
assertion was DEFECT-INJECTED and confirmed to fail before restore.*

*▶ AWAITING (carried): AL 40NR scenario-G TRS/ERS exempt-listing ruling (Ken);
entry-lane token → 40NR variant path; `GA_OCGA_48_7` (GA-500 S3-9 seed); 1040
BATCH-012/296 held; Jason Houston 1040 shell (Ken).*

*⛔ KEN — outstanding (carried): entity second-state-face transport (#3);
`OVERRIDE_HONORED_STATE_LINES`; 146-packet re-export; NC/CA/SC linked-state
reopens; #6 1065X/AAR; #68 optimizer; s274 PII narrowings; RS 8990 re-authoring
gate; 6765 Sec G; client-4545 D_8606_BASIS_ONLY; 1065 BATCH-004 #4 (Codex
Box-2); Analysis line-2 active/passive proxy.*

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

## ⚠ Known red / rotted — THE ONE LIST (post-s288)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s288, run after every client tranche).

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 ⚠⚠ **NEVER edit `apps/` source while a detached pytest is in flight**
  (s286): source-inspection flow assertions slice the NEW file with the
  OLD import's line numbers — five phantom failures. New files/markdown/
  scratchpad only during a run.
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\`. ⚠ `poetry run python -c "<multi-line>"` binds wrong —
  script FILES, not inline `-c`.
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8. ⚠ Embedded double-quote in a here-string
  arg to a NATIVE exe SPLITS the argument — use `git commit -F -` with a
  bash heredoc (used all session, s288).
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed.
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.
- 🔧 ⚠ pytest-randomly NOT installed (s281).
- 🌐 ⚠ pymupdf: `get_text()` misses AcroForm widget VALUES (s283); word
  bboxes span the FULL line height (s286); a field map guessed from SHORT
  widget names silently no-ops (s287).
- 🌐 ⚠ **A WebFetch SUMMARY of an IRS page is a paraphrase, not the text**
  (s288). Two fetches of the same box-16 instruction returned different
  "verbatim" quotes. A STRING-COUNT question ("how many times does '2a'
  occur — reply ZERO OCCURRENCES if none") is checkable; a
  "quote it exactly" prompt is not. Local templates beat both.

## 🔎 Carried for triage — NOT claims
- (s288) `IndividualForm7203` still has no home for box 16 code E
  (shareholder-loan repayment); 1065 box 18 a/b/c have none on the
  recipient side at all (`K1Basis704dWorksheet` carries only distributions).
- (s288) `ctc_override` / `odc_override` + `Dependent.compute_qualifies_*`
  are dead to compute — only `D_1040_008` still reads them.
- (s287) The 8825 line-1 repaint covers the LINE-1 table only.
- (s287) The suggested-field convention now covers W-2 3/5 + 1099-R box 16 —
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

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried + s288 adds:
Everything from s277–s287 stands (R-K1-20N; R-GA700-PARTNERS income base;
AL_FORM_40NR amendments; GA-500 S3-9 blocked on `GA_OCGA_48_7`; R-AL-TAX
mechanism; D-36 reads; R-B1-AUTO / R-B2-AUTO; the 4562 line-17 reversal).
**s288 ADDS: (a) `R-8880-LINE1-COMPOSE`** — line 1 derives from traditional +
Roth + ABLE with the stored field as override; the live spec also omits ABLE
from line 1 entirely, which the face names. **(b) FORM_7203 3g/3h/K9** — the
routing finding above; the spec must say whether 3g takes ST+LT combined.
**(c) `SCHEDULE_K1` box 16 A/B** — the recipient spec has 31 facts and no
box-16 fact; the issuer spec has all five codes. **(d) 1040_SPINE DG-4** —
the scenario expects `D_1040_008`, which is unreachable except via the dead
overrides. Spec-fetch gate satisfied live for FORM_8880; flow assertions green.
