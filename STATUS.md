# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-07 (s230). **One unit: Form 6765 (§41 credit for
increasing research activities) BUILT** — the RS spec Ken ruled BUILD in the
s224 pass. Complete on the 1120-S: input → compute → render → flow to Schedule
K line 13g → K-1 box 13 code M, with all six spec pins matching to the dollar
on the first run. The architectural piece: **Schedule K line 13g is now
COMPOSED across its source forms instead of owned by Form 8941.** Migrations
0269 + 0270. No movement on any existing return.*

*Previous (s229): `CC_A_M_REMAINING_BLOCKERS` closed 6/6 with no code change
and no deploy (`c2728c8`).*

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

## ⚠ DATE CORRECTION (s230)
The s228 and s229 entries in STATUS/tracker/archive are labelled **2026-08-08**;
both commits (`fa807c9`, `c2728c8`) are dated **2026-08-07**. The 08-08 labels
are wrong. s230 is also 2026-08-07 — three sessions, one long day.

---

## ▶ RESUME HERE

### ⭐ NEXT UNIT — sweep the CC queues at boot, then the s224 ruled list
All three CC queues were EMPTY (or parked) at s230 boot and nothing new is
expected while Ken is away, so the next unit comes off the ruled backlog:
**8853 Section C** (long-term care, §213(d)(10) cap), then the 1065 K17a, GA
bulk-sale, both e-file refusals, identity read-back, 1310 box B upload +
`ForeignAddressType`, CR-2026-001.

**⚠ Strong candidate to promote:** the **1040-side general business credit leg
(Form 3800 Part III lines 1c / 4i + the §38(c)(5) eligible-small-business
determination)**. It is now the single blocker for BOTH pass-through credits —
Form 8941's box 13 code BA and Form 6765's code M each reach a shareholder's
1040 and dead-end there. One unit unblocks two forms. Ken's call.

### The queue right now
- **1120-S** (`1120S\CC Changes\`): EMPTY at s230 boot.
- **1040** (`1040\CC Changes\`): EMPTY at s230 boot.
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

### ✅ s230 in one paragraph
Built **Form 6765** end-to-end on the 1120-S from the RS spec (24 facts, 7
rules, 50 mapped lines, 10 diagnostics, 6 tests; cached to
`server/specs/6765_spec.json`). Verify-first confirmed the form existed
NOWHERE in `server/` — a genuine from-scratch build, not a lane gap. Legs:
`Form6765` singleton + RLS (0269/0270); `compute_6765.py` implementing
R-6765-METHOD/QRESUM/REG/ASC/280C/DEST/SECTE — **all six spec pins F6765-T1…T6
matched to the dollar on the first run**; twelve `D_6765_*` diagnostics; the
`f6765` AcroForm face; the `form-6765` GET/PATCH endpoint plus
`SlateForm6765Screen` under a new `form_6765` tab. **The template is Rev.
December 2024 and that is CORRECT — Form 6765 is CONTINUOUS-USE, so the
12-2024 revision IS the TY2025 face (instructions are i6765 Rev. 12-2025); the
s224 "`f<form>.pdf` is the next revision" trap does not bite a continuous-use
form.** Every page-1/page-2 widget is mapped (61 keys, zero unmapped, zero
missing) and both pages were rasterized and read back to confirm each value
landed on its own line. **The hard part was K13g** — see the DECISIONS entry:
Schedule K line 13g had one writer (Form 8941, code BA) and a boolean
bridge-gate read by the printed K-1, the MeF K-1 mapper and the read model;
6765 reports to the same line as code M. New `apps/returns/k13g.py` makes the
line a SUM over a registry of sources, and allocates **each source in its own
right** through the existing residual-offset allocator (synthetic `K13g_BA` /
`K13g_M` share keys) so Σ over shareholders of each code equals that form's
entity amount exactly. Reconcile-or-refuse widened from "K13g == the 8941's
line 16" to "K13g == Σ sources"; when it fails, no code is emitted and both
consumers refuse, as before. Beyond the spec's ten diagnostics, two RED holds
were added under SPRINT_SCOPE quality rule 2: `D_6765_METHOD_MISSING` (QREs
keyed with no method chosen computes nothing) and `D_6765_DEST_UNWIRED` (only
the 1120-S Schedule K route is built). An engaged 6765 also **refuses MeF
composition** — no IRS6765 schema exists on disk, and transmitting a Schedule
K credit with its substantiating form silently omitted is worse than refusing.
Suites: new `test_form_6765.py` (27) + `slateForm6765Screen.test.tsx` (11),
both green first run; regressions green — 8941/MeF (103), flow assertions +
acroform (565), entity/K-1/diagnostics (233), the full K-1-allocator/packet
sweep (1,353). Client typecheck unchanged at the 55-line baseline.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **s230: NONE.** Nothing writes K13g unless a credit form is engaged, and
  with only an 8941 engaged the composed sum IS that 8941's line 16 —
  byte-identical to the old single-writer behavior (pinned by the unchanged
  8941/MeF suites). The K-1 box-13 code path is likewise unchanged for an
  8941-only entity.
- Carried from s227/s228: a 1065 K-1 whose §704(d) worksheet is SAVED moves;
  a 1065 row with the basis checkbox ticked swaps its warning code;
  #10 8959 single-W-2 engage (intended); #6 Sch 1 24k engine-fed blank→0
  (cosmetic watch).

### ⚠ Known red / rotted (not this session's changes)
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  `ValidationError`. Pre-existing (s219). **Re-confirmed at exactly 8 in
  s230's sweep** — not touched by the K13g refactor. Not diagnosed.
- `test_4868.py` — 4 tests on the Schedule 3 line-10 feeder (s217). ⛔ KEN.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  batch-005 #9 PTET-gate class, red since s212.
- **DiagnosticRule unique-code contamination** (s225): `test_backentry_
  cleanup.py` (red alone under `--reuse-db`), `test_ga500_auto_attach_
  s106.py`, `test_ga500_rie_federal_pull.py`. ⚠ The 12 new `D_6765_*` codes
  were checked against the live catalogue before seeding — **zero collisions**.
- **Client typecheck**: 55 error lines standalone (unchanged by s230).
- ⚠ The Slate 8889 fixture cast `as HSAAccountRow` still swallows new
  required fields.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). `--reuse-db`.
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout
  loses ALL output — redirect to a file and tail it.
- ⚠ A `poetry run python script > file` redirect BUFFERS. Use `-u`.
- ⚠ **New (s230)**: `grep -rl ... tests/` matches `__pycache__/*.pyc` and
  pytest then errors out with "no tests ran" — pass `--include=*.py`.
- ⚠ **New (s230)**: a bare `cd server && …` in Bash fails when the shell is
  already in `server/`, and the command silently does nothing but the STALE
  output file still tails as if it ran. Check `pwd` first.

### ✅ KEN DECISIONS OUTSTANDING
- **⛔ KEN (s227)**: the out-of-scope-state packet-disposition marker (full
  write-up top of REVIEW_QUEUE). Unchanged.
- **⛔ KEN (s230, NEW — a scheduling call, not a blocker)**: promote the
  1040-side Form 3800 Part III 1c/4i + §38(c)(5) ESB leg? It is the single
  blocker for both pass-through credits (8941 code BA, 6765 code M). See
  "NEXT UNIT" above.
- **⛔ KEN (s230, NEW — a dated obligation)**: **Form 6765 Section G becomes
  REQUIRED for tax years beginning after 2025** (i6765 verbatim). The RS spec
  must be re-authored before a TY2026 season; `D_6765_G_TY2026` fires from
  2026. This is on the season runway, not the backlog.
- The one live external item: **1040 v5.4 business rules still not in hand,
  active 2026-08-09** (v5.4 schemas ARE on disk; 1041 v5.5 closed).

### 🔎 Carried for triage (s229) — not chased
A filed, exact-tie 1040 shows **worksheet drift on a bare recompute**:
`1040_SCHD_WS` `clc_1` 139,889 → 134,398 and `clc_3` 140,738 → 135,247 (−5,491
each), face still an exact TIE. Worth a sweep: how many filed returns move a
worksheet line on recompute?

### RS AGENDA
- **NEW (s230), Form 6765 — five spec items:**
  (a) **`D_6765_BOTH_METHODS` cannot fire as written.** Its condition is
  "Section A lines nonzero AND Section B lines nonzero", but the spec's own
  facts SHARE lines 1/14, 2/15 and 3/16 as single facts, so no stored state
  can put amounts in both. Implemented instead against what the model *can*
  express — a method chosen with the OTHER section's exclusive inputs still
  populated (the preparer switched methods and left figures behind), which is
  the error the spec is reaching for. (The s142 rule: *a spec condition that
  cannot fire is a defect in the spec.*)
  (b) **`D_6765_FBP_RANGE` does not cover a BLANK fixed-base percentage** —
  its condition is `<= 0 OR > 0.16`, and a null is neither. A blank is the
  common case and it CANNOT compute a base amount; treating it as zero would
  OVERSTATE the credit. Implemented to include null, gated on line 48 > 0 so
  merely opening Section A stays quiet.
  (c) **The Section F header question has no fact and no line_map entry** —
  the face asks "Are you required to complete Section G?" For TY2025 the
  answer is derivable for every filer (i6765: "Section G will be optional for
  all filers for tax years beginning before 2026"), so it renders "No" below
  2026 and blank from 2026. It should be a spec fact.
  (d) **Two app-added diagnostics want adopting**: `D_6765_METHOD_MISSING`
  and `D_6765_DEST_UNWIRED`.
  (e) **The 1065 Schedule K-1 box-15 letter is still `[UNVERIFIED]`** in the
  spec — re-pull i1065sk1 before anyone wires the 1065 arm.
- Carried: s228 (a) `D_K1B_FULLY_ALLOWED`'s spec condition also matches an
  all-zero worksheet on a loss K-1; (b) the s226 `requires_human_review`
  verbatims. s227 notes (8959 derivation, SCHEDULE_D owner facts, SCH_1 24k
  typing), s224 items, s223 and earlier unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
