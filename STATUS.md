# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-23 ~04:00 (s276 — the overnight shift continues on
Ken's standing authorization from ~00:15).*

*⚠⚠ RESUME POINT — **s276 SHIPPED THREE UNITS ACROSS THREE VERIFIED
DEPLOYS**: ① **BATCH-296 #37+#67 as one unit** (`2210c69`,
dep-da59rpjl550s7384a2k0) — Form 6251 Part III now COMPUTES via the
Schedule D Tax Worksheet (2025 face lines 12-40 transcribed verbatim, both
skip rules; the gather reproduces the regular SDTW via the same pure
`compute_sdtw` and PROVES it by tying line 47 to the return's line 16
within $1 — mismatch keeps the D_6251_005 defer). Zero taxable excess
never defers on §1250/28% gain; basis-difference facts still error;
D_6251_008 warns. **Unblocks the EIGHT tied returns held solely by
D_6251_005** — entry lane asked to re-run cleanup (no re-stage) and post
the clears. Four injected defects each caught; FA 526. ② **The two
entry-lane channel asks** (`129daab`, dep-da5a1qad0e5s73bg2jug) —
shell-lookup rows now carry city/state (1040 Taxpayer / entity lanes;
same-name disambiguation), and the cleanup API 400s on any unknown
top-level or packet key naming the key + index + allowed vocabulary
(a typo'd `source_verified` used to drop silently). ③ **The s275 #75
deferred UI leg** (`44e9511`, dep-da5a549t0dsc73c8mveg verified LIVE
~03:37) —
`scha_other_itemized_type` dropdown on BOTH Schedule A screens (legacy +
Slate), enum shared via `client/src/renderer/lib/otherItemizedTypes.ts`;
typecheck clean, vitest 6/6 injection-proven.*

*▶ NEXT: **still awaiting the entry session's hold shapes** for the
staging-guard family (1310 date-of-death; asset `flow_to` unresolvable
`link_key`; `mortgage_deductible` teaching refusal) + the CTC missing-DOB
warning — re-asked by message this session. **The "EIC opt-out class
(line 27c)" one-liner was verified NOT literal** — `eic_opt_out` exists
end-to-end (BATCH-005 #2, D_EIC_017, mig 0156); asked the entry session
for the real shape before building anything. Then the remaining BATCH-296
opens (many are entry-shape- or Ken-gated; see the batch file's annex
ledger).*

*⛔ KEN — MORNING QUESTIONS (carried from s275, still open): (1) **#77
MFJ**: is the GA eligible-itemizer credit $600 on a joint return where
both spouses itemize ("$300.00 per taxpayer", §48-7-27.1)? Engine derives
nothing for MFJ until you rule. (2) **#82's fixture ruling**: is 2.564%
(i8829 39-year) right for the client-4175 permanent note? (3) Carried:
#8 GA-700 Sch 4 scope; #6 1065X/AAR spec + season-one scope; #68
optimizer "best"; the s274 PII items (mirror history rewrite; guard
blocklist hardening).*

*▶ s276 side effects: no migrations. The D_6251_005 seeded description
narrows at deploy via seed_all/seed_rules (behavior is code-side and
already live). RS note: the 6251 spec needed NO amendment — its
R-6251-P3-CAPGAINS already sanctioned the SDTW reuse (the spec was ahead
of the app); its D_6251_005 diagnostic description could take the
narrowed wording in a courtesy pass (agenda'd, low priority).*

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
`D:\dev\Passwords & Secrets\render-api-key.txt`). *(s276: all three
deploys API-verified LIVE.)*

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** Overnight
arrangement (Ken, ~00:15 8/23): both siblings route questions here;
anything needing Ken directly (RS prod seeds, tax-treatment rulings)
HOLDS for morning. GATE EXCEPTIONS stay human end-to-end. Coordination:
ListAgents + SendMessage; ONE delvio-tax tree holder; ONE
pytest/test_postgres holder; explicit-path staging; NO stash on the
shared tree. *(s276 holds tree + test_postgres.)*

## ▶ LANE MAP (Ken-ruled 2026-08-22, s273; unchanged s276)
- **Tax Return Entry (Claude)**: 1040s only. Owes s275/s276: the re-stage
  proofs (#78 client 4502; the two #76 fixtures; #75 clients 2003/3731),
  **the cleanup re-run on the eight D_6251_005 holds (s276)**, the
  staging-guard hold shapes, the PSO E-flag sweep count, any MFJ
  itemizer's printed line 19, the real EIC-opt-out-class shape.
- **Codex**: 1065 entry lane; re-passes the 1065 Inbox against `e3e88a4`.
- **Delvio-states (Claude)**: idle overnight; LA seeded NOT cleared;
  state-spec priorities routed to Ken (AL 40NR / SC Sch NR / NC D-400).
- **This session**: builds, deploys (Ken's overnight authorization),
  annexes.

---

## ⚠ Known red / rotted — THE ONE LIST (post-s276)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage; fails
  even per-file on a contaminated reuse-db; `--create-db` on the affected
  files = reset AND non-implication proof (s275).
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔
  KEN s217).
- **Client typecheck**: green (`npm run typecheck`); s276's client change
  (Schedule A dropdown) typechecked + vitest 6/6.

### ⚠ Test-run hazards (standing)
- One shared `test_postgres` (RS suite included). Long runs DETACHED.
  Never pipe pytest through `Select-Object`; redirect to a file.
  `poetry run` only from `server\`. Windows python can't read Bash /tmp.
- ⚠⚠ PS5.1 encoding traps (s275, three hits in one night): `Set-Content
  -Encoding utf8` BOMs (one reached a commit subject via `-F`);
  `Get-Content -Raw` without -Encoding mojibakes UTF-8 before a rewrite
  (double-encoded a batch-file annex AND compute_8839.py — the latter
  restored from HEAD); regex-replace file rewrites are BANNED here — use
  the Edit tool or `[IO.File]` with explicit BOM-less UTF8.
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.

## 🔎 Carried for triage — NOT claims
- (s268) 1,604 queries/run across 957 rules; + s275 adds
  `credit_limit_worksheet_b` running twice per pass (8839 limit + 8812);
  + s276 adds the Part III gather re-running `compute_sdtw` per pass on
  §1250/28% returns — all memoization candidates if the s268 unit lands.
- (s241) `Form8606`/`HSAAccount` duplicate owners · (s234) the $250k
  nonpassive K-1 AGI gap (repro in `test_8960_line4b_clamp.py`) ·
  (s274) the shared-policy pair 8962 fixture · (s275, states session)
  `.first()`-on-per-form-rules sweep chip (task_b1fd177f).

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 170 (GA-600S §179
  HB 1199) · 17a / 17d.

## RS AGENDA — carried + s276 addition:
FA-1040-SCHF-04 re-export · AL_FORM_40NR no spec (#52) · FORM_2441 three
amendments · Form 4136 no spec (#48) · collectibles_28 notes · SC1040
pins 2,360 · NC D400 part-year dates · ten staged FA definitions · 8862
re-author · SCHEDULE_H draft · GA QEE / 4547 / 8879_TA no spec · `500`
spec silent on RIE feeds, DIS gate, joint-split rounding, line-5 and
line-19 derivations (s275) · R-RET-5AB lacks the §402(l) PSO fact (s275)
· **6251 spec D_6251_005 description could take the narrowed
irreproducible-worksheet wording (s276, courtesy)** · 1065 K-1 box-15
letters (URGENT).
