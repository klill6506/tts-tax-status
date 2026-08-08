# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-07 (s227). **One unit: 1040 CC batch-001 worked
end-to-end — all ten items confirmed on verify-first (none refuted), nine
built in ONE deploy (`9b9673c`), #9 answered by the SEASON_PLAN states
ruling with one ⛔ KEN sliver filed. Migration 0266 applied. Batch file
annexed and moved to `1040\CC Changes Done\`.** Suites green: the new 30-test
batch file, flow assertions, topic8/topic9/staging/reconcile/commit/RIE/7203;
vitest 18; tsc no new errors.*

*Previous (s226): pilot #7 unblocked — K1_BASIS_704D spec authored, Gate-1
approved, seeded, cached. No app code that session; the build is still the
next unit.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax`; demo = `tts-tax-demo`. **The orphan third service
`tts-tax-app` still fails every push — Ken should delete it.**

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

## ⚠ KEN IS AWAY 2026-08-09 → ~2026-08-19 (10 days)
**He has his laptop — availability is MINIMAL BUT NOT ZERO** (Ken, 2026-08-07).
Batch questions; keep them low-friction. Nothing is on a clock in that window;
the next hard deadline is 2026-09-15 (extended entity returns).

---

## ▶ RESUME HERE

### ⭐ NEXT UNIT — BUILD pilot #7 against the approved spec (unchanged from s226)
`K1_BASIS_704D` (partner §704(d) basis limitation, preparer-asserted) is
Gate-1 approved, seeded, cached at `server/specs/k1_basis_704d_spec.json`
(8 facts / 6 rules / 5 diagnostics / 7 scenarios / FA-1040-K1B-01…05).
Build steps, pins and the movement class are written out in the s226 STATUS
(STATUS_ARCHIVE) and the spec itself: per-K-1 worksheet model → partnership
arm in `k1_sche_net()` beside the 7203 arm (`max(raw, −allowed)` once, 1065 +
material participation v1) → the five D_K1B_* diagnostics → **BOTH lanes** →
persistence (suspended survives reload/roll-forward/import) → NO MeF/render
leg (spec-pinned R-K1B-CARRY) → QBI takes §199A as entered (R-K1B-QBI).
Pilot pins: loss 26,850 / allowed 10,621 / suspended 16,229 → Sch E 41
90,041→106,270, AGI 195,006→211,235, GA follows.
⚠ s227's batch-001 #8 added `IndividualForm7203.nondeductible_expenses` and
the nested lane `form_7203` — the K1_BASIS_704D build touches the SAME
`k1_sche_net()` and K-1 lane; rebase mentally on `9b9673c`.

### The queue right now
- **1120-S** (`1120S\CC Changes\`): EMPTY (swept at boot s227).
- **1040** (`1040\CC Changes\`): EMPTY — **batch-001 worked and moved to Done
  this session** (10/10; annex in the Done file).
- **Legacy root** (`CC Code Changes\`): 3 open files — the pilot batch (its #7
  is the unit above), `CC_A_M_REMAINING_BLOCKERS` (six code requests, never
  blocked — DECISIONS item 1), and the NZ file (9 of 10; #10 multi-state
  parked under the states-on-hold ruling).
- After #7: the s224 ruled-and-buildable list stands — Form 6765 (RS spec
  approved), 8853 Section C, §213(d)(10) LTC cap, 1065 K17a, GA bulk-sale,
  both e-file refusals, identity read-back, 1310 box B upload +
  `ForeignAddressType`, CR-2026-001.

### ✅ s227 in one paragraph
Codex posted the first 1040-lane batch (10 findings from the source-entry
loop); every claim verified TRUE against the code before building — four
parallel verification passes, then one deploy. Landed: the schedule_fs
nested-rows schema emission + a generator that tests can import (#1); the
NOT-SUPPORTED doc purge with a doc-contract test (#2, plus the third stale
entry — 1099-G); `f1040_fields` line-36 applied-forward election with the
post-compute ≤34−38 refusal (#3); `35a`/`36` reconcilable + tolerance
inheritance + the disposition staging warning (#4); the 2210
documented-source trio + `t2210_prior_full_year` on the lane with the
serializer's cross-field rule re-expressed at staging (#5); the 1041 K-1
§67(e) box-11A field → SCH_1 24k feeder → AGI → GA, gated to 1041 rows in
both lanes (#6); T/S/J carryover owners driving the GA RIE split ONLY in the
no-current-gross branch — untagged returns provably do not move (#7);
`nondeductible_expenses` → 7203 Part I 8a + the nested `form_7203` lane
object (#8); CA Form 540 RULED out by SEASON_PLAN scope with the
packet-disposition sliver ⛔ KEN in REVIEW_QUEUE (#9); and the 8959
single-W-2 aggregate derivation that satisfies the pinned $200k-flat arm
without touching the engage rule (#10).

### ⚠ Classes that MOVE existing returns or output on next recompute
- **#10 (intended)**: any 1040 with EXACTLY ONE W-2 row, no per-row box 5,
  and `amt_medicare_wages_agg` > $200,000 newly engages Form 8959 on
  recompute (writes the 8959 rows, Sch 2 line 11, and the line-24 → 25c
  recovery). That is the fix's target class; anything under $200k or
  multi-row is untouched.
- **#6 (watch, cosmetic)**: Schedule 1 line 24k is now an engine-fed line
  (the line-18 feeder convention), so a blank 24k becomes a stored "0" on
  every recomputed 1040. Same class as line 18 has always been; flag if a
  rendered face unexpectedly prints a 0 where blank was expected.
- **#7 moves nothing untagged** (default joint = the old 50/50, regression-
  pinned); an owner-tagged carryover moves only the GA RIE split — that is
  the fix.
- Old committed batches: reconciliation verdicts do NOT retro-change; the
  new 35a/36 checks apply when a batch is re-staged/re-run.

### ⚠ Known red / rotted (not this session's changes)
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  `ValidationError`. Proved pre-existing in s219. Not diagnosed.
- `test_4868.py` — 4 tests on the Schedule 3 line-10 feeder. Pre-existing
  (s217). ⛔ KEN: a 4868 payment not reaching Sch 3 line 10 is a real number.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  the batch-005 #9 PTET-gate class, red on main since s212.
- **DiagnosticRule unique-code contamination** (s225 finding): fix the
  fixtures with `update_or_create` — `test_backentry_cleanup.py` (red alone
  under `--reuse-db`), `test_ga500_auto_attach_s106.py`,
  `test_ga500_rie_federal_pull.py` *(ran green standalone this session)*.
- **Client typecheck**: pre-existing error classes only (TaxpayerLike-null,
  fixture `string|undefined`); re-measured s227 at 55 error lines standalone
  — no NEW errors from s227 files.
- ⚠ The Slate 8889 fixture is cast `as HSAAccountRow` — new required fields
  don't fail the typecheck; worth dropping the cast.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB.
- **The hazard is CROSS-REPO** (s221): both repos name their test database
  `test_postgres`. `--reuse-db` works; `--create-db` collides.
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout loses
  ALL output — redirect to a file and tail it (s224).

### ✅ KEN DECISIONS OUTSTANDING — one new
- **NEW (s227) ⛔ KEN**: the out-of-scope-state packet-disposition marker
  (batch-001 #9's fallback; full write-up top of REVIEW_QUEUE). Until ruled,
  a federal-TIE packet with a CA return stays un-filed, state named in
  `source.notes`.
- The one live external item: **1040 v5.4 business rules still not in hand,
  active 2026-08-09** (v5.4 schemas ARE on disk; 1041 v5.5 closed).
- Carried items unchanged — see DECISIONS.md / ARCHIVE.

### RS AGENDA
- **NEW (s227)**: three notes worth recording in specs — (a) 8959: the
  engage input `amt_max_single_w2_medicare_wages` may be DERIVED from
  `amt_medicare_wages_agg` when exactly one W-2 row exists (implemented;
  rule text unchanged); (b) SCHEDULE_D: the two carryover facts gained
  T/S/J owner companions consumed by R-GA500-RIE's per-spouse split —
  worth adding as facts; (c) SCH_1: line 24k is now engine-fed from the
  recipient-K-1 §67(e) field (spec types it input; the feeder is the
  line-18 convention — consider typing it calculated with the K-1 fact as
  source).
- Carried: s226 (K1_BASIS_704D `requires_human_review` verbatims), s224
  items, s223 and earlier unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
