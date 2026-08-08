# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-08 (s228). **One unit: pilot #7 BUILT — `K1_BASIS_704D`,
the partner §704(d) basis limitation, end to end against the s226
Gate-1-approved spec (deploy `165c972`; migrations 0267 + 0268). The
mixed-return pilot batch closes 7 of 7 — annexed and moved to
`CC Code Changes Done\`.** Suites green: the new 25-test spec suite, flow
assertions 526 (FA-1040-K1B-01…05 now active), K-1 diagnostics leg + s227
batch + seed leg 66, backentry staging/commit 160; vitest 8; tsc at the
55-line pre-existing baseline.*

*Previous (s227): 1040 CC batch-001 — all ten items, one deploy (`9b9673c`).*

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

### ⭐ NEXT UNIT — `CC_A_M_REMAINING_BLOCKERS_2026-08-03.md` (legacy root queue)
Never worked; its ledger's "blocked on Ken" label is STALE — DECISIONS "Scope
+ gate rulings" item 1 (Ken, 2026-08-07) ruled it **NOT blocked**: six
implementable code requests. Work it as a normal batch (verify-first, one
deploy, annex, move to Done). After it: Form 6765 (RS spec approved), 8853
Section C, §213(d)(10) LTC cap, 1065 K17a, GA bulk-sale, both e-file
refusals, identity read-back, 1310 box B upload + `ForeignAddressType`,
CR-2026-001 (the s224 ruled list).

### The queue right now
- **1120-S** (`1120S\CC Changes\`): EMPTY at s228 boot.
- **1040** (`1040\CC Changes\`): EMPTY at s228 boot.
- **Legacy root** (`CC Code Changes\`): TWO open files — `CC_A_M_REMAINING_
  BLOCKERS` (the next unit) and the NZ file (9 of 10; #10 multi-state parked
  under the states-on-hold ruling). **The mixed-return pilot batch moved to
  Done this session (7 of 7).**

### ✅ s228 in one paragraph
Built pilot #7 against the cached `K1_BASIS_704D` spec: a per-1065-K-1
§704(d) worksheet model (six preparer-asserted figures; migrations
0267/0268 with default-deny RLS); the `max(raw, −allowed)` cap applied once
in `k1_sche_net()` beside the 7203 arm, materially-participating partners
only (passive keeps the 8582 path + D_K1B_PASSIVE); five D_K1B_*
diagnostics with the d_k1_basis handover (the old warning no longer covers
1065 rows — D_K1B_UNASSERTED succeeds it, never both); BOTH lanes (browser
panel + `basis-704d` upsert endpoint refusing non-1065 rows; nested
`basis_704d` lane object published in the regenerated schema with the
NOT-SUPPORTED tail corrected); persistence per R-K1B-CARRY (proforma
`_k1_basis_704d` snapshot; roll-forward seeds a shell K-1 + an
everything-still-suspended worksheet; K-1 re-import updates in place); NO
render/MeF leg by spec ruling. FA-1040-K1B-01…05 merged active with gate
runners. The pilot movement pin: Schedule E line 41 −26,850 → −10,621
(Δ +16,229) → Schedule 1 line 5 → AGI; QBI consumes the source figure
un-double-limited.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **s228 (intended, opt-in)**: ONLY a 1065 K-1 whose §704(d) worksheet is
  SAVED moves (the cap engages on recompute). The flag-only
  `basis_at_risk_limited` checkbox still moves nothing — regression-pinned —
  and now warns via D_K1B_UNASSERTED instead of the old D_K1_BASIS text.
  **The LIVE mixed-pilot 1040 still shows the uncorrected figures until its
  worksheet is keyed** (browser panel or lane; the six figures are in the
  batch annex) — line 41/AGI then take +16,229 and GA follows.
- **s228 (diagnostic-only)**: a 1065 row with the checkbox ticked swaps its
  warning code (D_K1_BASIS → D_K1B_UNASSERTED) on the next diagnostics run.
- Carried from s227: #10 8959 single-W-2 engage (intended); #6 Sch 1 24k
  engine-fed blank→0 (cosmetic watch).

### ⚠ Known red / rotted (not this session's changes) — carried from s227
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  `ValidationError`. Pre-existing (s219). Not diagnosed.
- `test_4868.py` — 4 tests on the Schedule 3 line-10 feeder (s217). ⛔ KEN.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  batch-005 #9 PTET-gate class, red since s212.
- **DiagnosticRule unique-code contamination** (s225): `test_backentry_
  cleanup.py` (red alone under `--reuse-db`), `test_ga500_auto_attach_
  s106.py`, `test_ga500_rie_federal_pull.py`.
- **Client typecheck**: 55 error lines standalone (s227/s228 measure, same
  classes: TaxpayerLike-null, fixture `string|undefined`). No new errors
  from s228 files.
- ⚠ The Slate 8889 fixture cast `as HSAAccountRow` still swallows new
  required fields.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). `--reuse-db`.
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout
  loses ALL output — redirect to a file and tail it.

### ✅ KEN DECISIONS OUTSTANDING — unchanged from s227
- **⛔ KEN (s227)**: the out-of-scope-state packet-disposition marker (full
  write-up top of REVIEW_QUEUE). Until ruled, a federal-TIE packet with an
  out-of-scope state return stays un-filed, state named in `source.notes`.
- The one live external item: **1040 v5.4 business rules still not in hand,
  active 2026-08-09** (v5.4 schemas ARE on disk; 1041 v5.5 closed).
- Carried items unchanged — see DECISIONS.md / ARCHIVE.

### RS AGENDA
- **NEW (s228)**: (a) D_K1B_FULLY_ALLOWED's spec condition (`allowed ==
  current_loss AND suspended == 0`) also matches an ALL-ZERO saved
  worksheet on a loss K-1 — which zeroes the Schedule E loss (max(raw, −0))
  while the info text says "allows the entire loss". Implemented
  spec-verbatim, not improvised; propose a companion condition (worksheet
  current_loss magnitude < the K-1's raw loss → warning) for the spec.
  (b) The s226 `requires_human_review` verbatims stand (Reg §1.704-1(d);
  §704(d)(2)).
- Carried: s227 notes (8959 derivation, SCHEDULE_D owner facts, SCH_1 24k
  typing), s224 items, s223 and earlier unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## ⚠ Housekeeping note (s228): STATUS_ARCHIVE has NO s226/s227 entries — the
## s227 close never prepended them. The s228 entry is archived; s226/s227
## detail survives in BUILD_ORDER's ledger entries and the git history.
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
