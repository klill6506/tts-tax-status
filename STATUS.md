# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-08 (s229). **One unit: `CC_A_M_REMAINING_BLOCKERS` worked
and CLOSED 6 of 6 — with NO code change and NO deploy.** Four of its six code
requests had already shipped (s198/s199/s206); the `SCHED_L_DEPR_TIE` half of
two more was a false fire fixed in s199 (confirmed dead live). The real residue
was **three stored-row DATA repairs**, each proved on production data inside a
rolled-back transaction to move ZERO face lines before being applied. Six of
the seven subject returns are now cleanup-eligible and filed to `Done`; the
seventh is held CORRECTLY. **The legacy root queue is down to ONE open file.***

*Previous (s228): pilot #7 `K1_BASIS_704D` built; mixed-return pilot batch
closed 7/7 (`165c972`).*

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

### ⭐ NEXT UNIT — Form 6765 (RS spec approved), then the s224 ruled list
Both CC queues that feed new work were EMPTY at s229 boot and the legacy root
is now down to one parked file, so the next unit comes off the ruled backlog:
**Form 6765** (spec approved, DECISIONS "Scope + gate rulings" item 3 — Ken
ruled BUILD; Section D deferred behind a hard-hold diagnostic, Section G must be
re-authored before a TY2026 season). After it: 8853 Section C, §213(d)(10) LTC
cap, 1065 K17a, GA bulk-sale, both e-file refusals, identity read-back, 1310
box B upload + `ForeignAddressType`, CR-2026-001.

### The queue right now
- **1120-S** (`1120S\CC Changes\`): EMPTY at s229 boot.
- **1040** (`1040\CC Changes\`): EMPTY at s229 boot.
- **Legacy root** (`CC Code Changes\`): **ONE open file** — the NZ file (9 of
  10; #10 multi-state parked under the states-on-hold ruling).
  `CC_A_M_REMAINING_BLOCKERS` closed and moved to Done this session.

### ✅ s229 in one paragraph
Worked the A-M blocker file as a normal batch per DECISIONS item 1. **Verify-first
found four of the six code requests already shipped**: excess social security
across two same-owner W-2s (s198, Schedule 3 line 11, per person); the "stale
8995" (s199 — nothing was stale, `qbi_engaged` had four frozen call sites); 1099-R
code W plus the `D_RET_005` coverage gate (s199); and the Form 8283 lane section
(s206). **Items 2 and 3 each had two halves** — `SCHED_L_DEPR_TIE` was an
entity-only rule with no return-type guard erroring unclearably on every 1040
with a depreciation asset, fixed s199 and now confirmed dead (it appears in NONE
of seven fresh diagnostics runs) — while the `D_4562_DEST` / method / convention
half **needed no code at all**: the diagnostics were correct and the data was
incomplete. Ran the file's own release test (`run_cleanup_check`) over all seven
subject packets: 3 already eligible, 4 held, every hold a data gap. **Three
stored-row repairs**, each target fixed by an exact dollar match rather than a
guess — a Machinery & Equipment asset routed to 8825 on a return with zero
rentals → the Schedule C keyed at exactly its $230; an `Apartments` asset → the
sole rental keyed at exactly its $2,698; a 27.5-year asset stored as HY
(computing $0 for want of a published table) → the §168(d)(2)-mandatory MM,
producing 1,942, which uniquely matched one of three rentals (others 0 and
3,761). Each was applied first inside a rolled-back `transaction.atomic()` with a
full before/after `FormFieldValue` snapshot: **all three moved ZERO face lines**,
so the corrected register reproduces the filed figure instead of adding to it —
which settles the s199 double-count concern with evidence. Post-repair cleanup:
**3 of 3 eligible, still exact ties, still Filed**; PDFs and Done notes filed.
Regressions re-run green (19 · 54 · 5). Annex appended, batch moved to Done,
both queue READMEs corrected.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **s229**: no code changed, so nothing new moves. The three repaired returns
  were each proved to move zero face lines.
- Carried from s227/s228: a 1065 K-1 whose §704(d) worksheet is SAVED moves;
  a 1065 row with the basis checkbox ticked swaps its warning code;
  #10 8959 single-W-2 engage (intended); #6 Sch 1 24k engine-fed blank→0
  (cosmetic watch — **corroborated on three more returns this session**, along
  with the same blank→'0' on `SCH_3` line 11 from the s198 write).

### ⚠ Known red / rotted (not this session's changes) — carried from s227/s228
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  `ValidationError`. Pre-existing (s219). Not diagnosed.
- `test_4868.py` — 4 tests on the Schedule 3 line-10 feeder (s217). ⛔ KEN.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  batch-005 #9 PTET-gate class, red since s212.
- **DiagnosticRule unique-code contamination** (s225): `test_backentry_
  cleanup.py` (red alone under `--reuse-db`), `test_ga500_auto_attach_
  s106.py`, `test_ga500_rie_federal_pull.py`.
- **Client typecheck**: 55 error lines standalone (s227/s228 measure).
- ⚠ The Slate 8889 fixture cast `as HSAAccountRow` still swallows new
  required fields.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). `--reuse-db`.
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout
  loses ALL output — redirect to a file and tail it.
- ⚠ A `poetry run python script > file` redirect BUFFERS — a long cleanup run
  shows an empty output file until the process exits. Use `-u` or wait it out;
  a full `run_cleanup_check` over 7 returns took ~22 minutes.

### ✅ KEN DECISIONS OUTSTANDING
- **⛔ KEN (s227)**: the out-of-scope-state packet-disposition marker (full
  write-up top of REVIEW_QUEUE). Unchanged.
- **⛔ KEN (s229, NEW — informational, no action needed to proceed)**: the
  s224 ruling that `CC_A_M_REMAINING_BLOCKERS` contained "no asset decisions"
  was made on a reading of the file; two asset decisions *were* genuinely open
  (s199 escalated them as STATUS #13/#14). They are now **resolved by evidence**
  rather than by ruling — each target was fixed by an exact dollar match and
  proved to move zero face lines — so nothing is owed. Recorded only so the
  record is straight.
- **⛔ KEN (s229, NEW)**: STATUS open items **#13/#14 are now CLOSED** by the
  above; they should not be carried forward again.
- The one live external item: **1040 v5.4 business rules still not in hand,
  active 2026-08-09** (v5.4 schemas ARE on disk; 1041 v5.5 closed).

### 🔎 NEW for triage (s229) — not chased, out of the batch's scope
A filed, exact-tie 1040 shows **worksheet drift on a bare recompute**:
`1040_SCHD_WS` `clc_1` 139,889 → 134,398 and `clc_3` 140,738 → 135,247 (−5,491
each), with the face still reconciling as an exact TIE and cleanup unaffected.
Nothing filed is wrong, but a Schedule D Tax Worksheet line moving on recompute
means an upstream engine change since filing has never been reconciled. Worth a
sweep: how many filed returns move a worksheet line on recompute?

### RS AGENDA
- Carried: s228 (a) `D_K1B_FULLY_ALLOWED`'s spec condition also matches an
  all-zero worksheet on a loss K-1; (b) the s226 `requires_human_review`
  verbatims. s227 notes (8959 derivation, SCHEDULE_D owner facts, SCH_1 24k
  typing), s224 items, s223 and earlier unchanged.

## ⚠ Open items for Ken — #13/#14 CLOSED this session (see above); the rest
## carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
