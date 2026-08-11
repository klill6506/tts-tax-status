# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-11 (s242i). **✅✅ BATCH-005 IS COMPLETE — 10 OF 10,
the file moved to Done.** #8's final leg landed (`193acfd`): the 2025 face
mapped (Part I only — Parts II/III deliberately unmapped, named defers),
render with the election-silence behavior and Statement overflow, and the
`IRS6781` document (every row in one doc; a ticked election refuses by
name). Ten items → eight builds + two verify-and-closes, across ten
deploys and migrations 0289-0298.*

*The lane's remaining queue: **BATCH-004 #1 (the 1040-X lifecycle)** — the
last open build — then BATCH-003's six and BATCH-001/002's remainders.*

*Previous (s242d): ✅ #6 complete (§469(g), mig 0294). (s242c): ⛔→✅ IRS4797
closed. (s242b): ✅ #3 (migs 0292+0293). (s242): ✅ #7 (mig 0291). (s241z):
✅ #1/#5/#9 (migs 0289+0290). **BATCH-005 is 8 of 10 + #4 half done.***

*Previous (s241x): the BATCH-005 triage, 10/10 — the annex in the batch file
is the design record. Key: #4 8839 = draft-trap 4th; #8 6781 = NO spec; #7
needs the line-1h registry conversion (s230); #6 = rental facts + ⛔ IRS4797.
(s241w): ✅✅ BATCH-004 #5 Schedule H COMPLETE, all six legs (migs 0287+0288;
`server/specs/_schedule_h_source_brief.md`). BATCH-004 is 9 of 10.*

*Previous (s241v): ✅ #2 GA QEE credit COMPLETE (design record:
`server/specs/_ga_qee_credit_source_brief.md`); (s241r): ✅ #10 Form 4547
COMPLETE (`server/specs/_4547_source_brief.md`); (s241o): ✅ #9 Form 1099-PATR
COMPLETE. Full per-leg writeups: STATUS_ARCHIVE + the briefs + the batch
annexes — do not re-triage any of them.*

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
**Availability MINIMAL BUT NOT ZERO.** Batch questions; keep them low-friction.
Nothing is on a clock in that window; the next hard deadline is 2026-09-15.

---

## ▶ RESUME HERE

### ⭐ NEXT UNIT — **BATCH-004 #1: the 1040-X amended-return lifecycle**
(LARGE — expect multiple iterations; read the batch item + its triage
annex in `CC_CODE_CHANGES_1040_BATCH-004.md` first). ⚠ Standing
constraints recorded across sessions: **`IND-476`** (a Form 4547 must not
ride a post-original return — s241r's refusal enforces it; the amended
flow must not resurrect it) and **the Schedule H business-rule seams** on
any re-transmission. ⚠ A `Form1040X` model exists (a `1040-X` form code
appears in rules_1040x.py) — VERIFY-FIRST what already ships before
designing. Then BATCH-003's six remaining items (⚠ build #3 together with
the s239 Georgia work), Form 8853 A/B+C (the twelve-session standing
unit), and the IRS1116 e-file gap.

### ✅✅ BATCH-004 #5 (Schedule H) IS COMPLETE — s241w, one session
⛔ **The design record is `server/specs/_schedule_h_source_brief.md`** — do
not re-triage. What future work must know:
- **The RS `SCHEDULE_H` spec is a DRAFT covering 7 of ~27 lines** — the
  third draft-trap occurrence (8379 s238, 8862 s241c). Built from the 2025
  face + 2025 Instructions (thresholds fetched live) + `IRS1040ScheduleH.xsd`.
  **No gate checks a spec export's `status` — check it by hand every time.**
- **Schedule 2 line 9 is RECONCILED, never written** (`D_SH_S2L9`, `!=` ± $1 —
  a DEDICATED line is wrong in both directions, unlike s241u's shared-line
  `<`). The line was already keyed/seeded and rides `SCH2_L21_ADDENDS` →
  1040 line 23; two feeds would be the s234 defect.
- **⚠⚠ Lines C and 9 put their "No" checkbox FIRST on the 2025 face** while
  A/B/10-12/27 put "Yes" first — verified positionally, pinned by a test
  that re-derives both orientations from the PDF's captions. And C/9 are ONE
  stored fact (`futa_quarterly_wages_over_limit`) printed in two places, at
  most one of which a filed form marks (the skip cascade).
- **⚠⚠ The MeF business rules RESHAPED the extract**: `SH-F1040-005` (exactly
  one Yes among A/B/C → the cascade governs emission; skipped = ABSENT);
  `SH-F1040-016-01` + `S2-F1040-146-02` (a line-9-No form OMITS lines 25/26 —
  Schedule 2 sums its line 8 instead); 008/009 ($2,800 threshold), 006
  (line 7 non-zero), 022 (SSN required) — each refused by name at extract.
- Year-keyed constants **RAISE on an unverified year**; the credit-reduction
  states are an ANNUAL LIST (2025: CA 0.012, VI 0.045).
- **Named RED defers (DEFERRAL_AUDIT)**: Worksheet 1 (late contributions —
  credit OVERSTATED without it) and Worksheet 2 (credit-reduction states —
  tax UNDERSTATED without it) — opposite signs; the state-disability group;
  the browser-lane entry screen (import lane only today).
- Section B: two printed rows, overflow → Statement page, line 18 totals
  cover EVERY row; the XML takes all rows in ONE group. One schedule per
  spouse (`maxOccurs="2"`, DB constraint + staging duplicate check).

### ⭐ STILL UNBLOCKED, still passed over — now ELEVEN sessions
- **Form 8853 Sections A/B + Section C.** Spec cached; all four legs pending.
  Read the s232 write-up in STATUS_ARCHIVE first — Schedule 1 line 8e is
  COMPOSED not owned, line 25 FLOORS AT ZERO. ⚠ s241 gave it a second reason:
  Form 5329 line 36 takes "Form 8853 line 8" and no `Form8853` model exists.

### ⛔⛔ THE E-FILE GAP LIST — still TWO named documents, unchanged
- **`IRS4797` (s240)** — no 1040-side builder; `S1-F1040-118-01` rejects
  without it; refusal fails loudly at composition. **BATCH-005 #6 needs it.**
  The 1120-S builder is the worked example.
- **`IRS1116`** — the oldest live e-file gap. s238's `IRS8379` is the worked
  example end to end.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-001 — 6 open** (2, 4, 5, 6, 8, 10);
  **BATCH-002 — 9/10 open as to COMPUTE only** (NOL-spec-blocked);
  **BATCH-003 — 6 open** (1, 3, 6, 8, 9, 10 — ⚠ build #3 together with the
  s239 Georgia work); **BATCH-004 — ONE open (#1)**; **BATCH-005 — 10 open,
  UNTRIAGED**. Every worked file carries a result annex; read it first.
  ⚠ None has moved to Done, deliberately.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

---

## ⚠ Classes that MOVE existing returns or output on next recompute
- **s241w: NONE.** Two new tables (additive CreateModel + RLS ALTER — the
  s190 `db_default` trap does not apply; the one boolean with a default has
  `db_default`), a pure compute module, a lane section, eight diagnostics, a
  render function and a MeF document — ALL reached only when the return
  carries a `ScheduleH` row, and none exists until a preparer or payload
  makes one. The published import schema gains `schedule_hs` (additive).
  `render_complete` gains a Schedule H appendix on returns that carry one.
- **⚠ s241o MOVES GEORGIA RETURNS carrying a 1099-PATR** (RIE L10 feed;
  L10 became a derived line — browser writes set `is_overridden`).
- **⚠ s241j MOVES DIAGNOSTICS**: post-2018 alimony instruments fire
  `D_SCH1_007` (error) — those returns are genuinely wrong today.
- **⚠ s241d/s241c MOVE E-FILE OUTPUT** on any return transmitting Form 8862 —
  every change a correction toward the truthful, narrower claim.
- Carried from s240: passive/PTP K-1 §1231 losses fire RED; a non-zero
  Schedule 1 line 4 refuses at MeF composition.
- Carried from s239: Roth 1099-Rs move 5a/5b → 4a/4b; GA partnership K-1s move
  RIE L2↔L13; code-U un-blanks the pension taxable column (largest mover).
- Carried from s236/s235: GA RIE line 13 on suspended passive K-1 losses;
  GA dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted
- **`--reuse-db` cross-module contamination**: `test_backentry_cleanup.py`
  (3, s225) and `test_mappings.py::TestApplyMappingAmbiguousFederalReturn`
  (3, s239).
- `test_topic7_input_leg.py::TestEICFacts::test_non_engaged_return_leaves_27a_quiet`
  — pre-existing (s235), not diagnosed.
- **`test_1040.py` — 6 pipeline tests**, unscoped `_fv` `.get()` (s234).
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  (s219). `test_4868.py` (4) — ⛔ KEN (s217).
  `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  PTET-gate class (s212).
- **⚠ PRE-EXISTING 1120-S defect (s241o)**:
  `test_line_key_registry_sweep.py::test_formula_targets_resolve` —
  `FORMULA_REGISTRY["1120-S"]` targets `M2_DIST_EXCESS` / `L24_BOOK_BRIDGE`,
  neither seeded, so the formula pass computes both and silently never
  persists them. 1120-S only. Deserves its own unit.
- **Client typecheck**: 55 error lines standalone (unchanged); vitest
  1,680 passed / 140 files.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB,
  cross-repo (`test_postgres`). A stalled background sweep beside foreground
  runs is contention, not a hang (s241o).
- A broad `-k` sweep blows the 600s timeout — background it; keep `-k` tight.
- `--create-db` does not reliably drop here; prove a pre-existing red via
  `git worktree` at a pristine SHA with the main venv + copied `server/.env`.
- A timed-out `pytest | Select-Object` loses ALL output — redirect to a file.
- `poetry run python > file` BUFFERS (use `-u`); stdout redirects go through
  cp1252 (write UTF-8 from inside Python); **never rewrite a UTF-8 file via
  `Set-Content`** — use the Write/Edit tools.
- **`poetry run` only works from `server\`**; Windows `python` cannot read the
  Bash tool's `/tmp` — use the scratchpad; DB probes: a throwaway
  `tests/test_zz_*.py` with `-s`, deleted after.
- Cloudflare-403 law sites and `rules.sos.ga.gov`: the in-app browser gets the
  text where WebFetch and curl fail (s239/s241o).
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes are `/api/v1/tax-returns/…` with the
  trailing slash; `filing_status` is `"mfj"`.
- `_finding(...)` kwargs land under `["details"]`; `ScheduleF` has no
  `business_name` (use `principal_activity`); `order_by("owner")` puts
  "spouse" before "taxpayer" (s241w).

### 🔎 Carried for triage — NOT claims
- **From s241o**: RIE L8 alimony underived; a fuller L10 derive possible in
  principle (all 8a-8z are FormFieldValues) but per-owner attribution and the
  (4)(b)2 gambling carve-out make it a design pass.
- **From s241**: `Form8606`/`HSAAccount` allow duplicate owners and their
  computes ITERATE (double-count, not vanish); browser POST unguarded.
- **From s234, potentially large**: a materially-participating 1120-S K-1's
  $250k nonpassive ordinary income never reached Schedule 1 line 5 / AGI.
  Repro: `server/tests/test_8960_line4b_clamp.py`, `_build(k1_ordinary="250000")`.
- Carried (s229): exact-tie 1040 shows `1040_SCHD_WS` clc_1/clc_3 drift on a
  bare recompute (−5,491 each), face still a tie.

### ✅ KEN DECISIONS OUTSTANDING
- **⛔ KEN (s231)**: §38(c)(6)(A) MFS threshold — flat $25,000 vs statutory
  $12,500; OVER-allows. Buildable without Ken; the ruling is the gate.
- **⛔ KEN (s227)**: the out-of-scope-state packet-disposition marker.
- **⛔ KEN (s230)**: Form 6765 Section G required for TY2026+ — re-author
  before a TY2026 season.
- **1040 v5.4 business rules still not in hand** (v5.4 schemas ARE on disk).
  ⚠ s240/s241w read the **v5.3** rules — re-check `S1-F1040-118-01` and the
  `SH-F1040-*` family against v5.4 on arrival.

### RS AGENDA
- **⛔ BLOCKING two batch items: NO NOL SPEC** (`172`/`NOL`/`FORM_172`/`1045`
  all 404). BATCH-001 #4 + BATCH-002 #10 wait. Preservation is built; only
  the computation waits. Highest-value authoring order on this list.
- **NEW (s241w): `SCHEDULE_H` is a DRAFT covering 7 of ~27 lines** — the
  computed skeleton only (no inputs, no A/B/C router, no Section B table).
  Third draft-trap occurrence; **the export's `status` field is STILL not
  checked by any gate.** Re-author per-line from the brief; record the two
  worksheets (late-contribution 90%, credit-reduction) and the annual
  constants (wage thresholds, the credit-reduction state list).
- (s241s): the GA QEE credit has NO SPEC (all aliases 404; `500` types line
  21 `input`). Author the Schedule 2 credit-detail + `IT_QEE_TP2` specs from
  the brief — MUST carry the two carryforward regimes.
- (s241p): `4547` and `8879_TA` have NO SPEC. Author from the brief; keep
  lines 6/7 as separate tests; record `IND-476`.
- (s241o): the `500` spec still has NO rule for what feeds RIE lines 1/2/6-13
  — five defects and counting. Author with the earned/unearned split by
  entity type, the $5,000 cap, the gambling carve-out, GA-taxable-only.
- (s241b): the `8862` spec is a draft collapsing each PART to one boolean —
  re-author per-line.
- Carried: `5329` roll-forward/Part VIII silence (s241); `R-8582-MULTIFORM`'s
  stale no-silent-gap cite + `4797` K-1 §1231 silence (s240); `R-RET-CODE`
  outrun three times (s239); `8379` draft (s238); `R-SCHA-CHARITABLE` three
  buckets vs seven K-1 codes + RIE-13 missing (s236); SCHEDULE_A carryover
  aggregation + `500` line 7a typed `input` (s235); R-STD-04 + `R-8960-INCOME`
  (s234); s232/s231/s230 items; the 1065 K-1 box-15 letters (still URGENT).

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
