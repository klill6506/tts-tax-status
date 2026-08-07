# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-07 (s226). **One unit: mixed-entity pilot #7 UNBLOCKED —
the `K1_BASIS_704D` spec (partner §704(d) basis limitation) was authored,
Gate-1 approved by Ken in-session, seeded, export-verified and cached into
`server/specs/k1_basis_704d_spec.json`. NO app code changed this session —
the app build is the next unit.** RS commits `b4f147e`/`0dab0f3`/`ae228e9`;
no delvio-tax deploy, no migrations.*

*Previous (s225): L24d book bridge ratified + specced (RS R010); NZ #5/#6 done
— lane-only gaps again. NZ is 9 of 10; the decision queue was emptied.*

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

### ⭐ NEXT UNIT — BUILD pilot #7 against the approved spec
**The Rule Studio gate is CLEARED.** `K1_BASIS_704D` (partner §704(d) basis
limitation, preparer-asserted) is Gate-1 approved, seeded, and cached at
`server/specs/k1_basis_704d_spec.json` (8 facts / 6 rules / 5 diagnostics /
7 scenarios / 5 flow assertions FA-1040-K1B-01…05). Build to the spec:

1. **Model**: a per-K-1 basis worksheet (the six preparer-asserted figures —
   beginning basis, additions, distributions, current loss, allowed, suspended).
   Existence = the confirm signal (the `IndividualForm7203` precedent).
2. **Compute**: a partnership arm in `compute_schedule_k1.k1_sche_net()`
   BESIDE the 7203 arm — cap = `max(raw, −allowed)`, applied ONCE. v1 scope:
   1065 + materially participating only.
3. **Diagnostics**: implement D_K1B_ARITH (**error** — allowed+suspended≠loss
   or allowed>available; never acknowledgable), D_K1B_EXCESS_DISTRIB,
   D_K1B_PASSIVE, D_K1B_UNASSERTED (the `d_k1_basis` warning becomes this —
   saved worksheet clears it), D_K1B_FULLY_ALLOWED (info).
4. **⚠⚠ BOTH LANES** — the Slate screen AND a `backentry.v1` section. Four
   consecutive NZ items were lane-only gaps; do not repeat it on a new build.
5. **Persistence** (the item's regression #4): suspended survives reload,
   roll-forward, import/export. **NO MeF document, NO render leg** — the
   Partner's Instructions impose no attachment (the s225 scope finding, now
   spec-pinned in R-K1B-CARRY).
6. **QBI**: Form 8995 consumes the K-1's §199A amount AS ENTERED — never apply
   the worksheet cap to QBI (R-K1B-QBI; Reg §1.199A-3(b)(1)(iv)).
7. **Pilot pins**: loss 26,850 / allowed 10,621 / suspended 16,229 → Sch E
   line 41 90,041→106,270, AGI 195,006→211,235, GA follows. Wire the five RS
   flow assertions.

⚠ Movement class: returns with a partnership K-1 marked `basis_at_risk_limited`
do NOT move until a worksheet is saved (no worksheet → today's behavior + the
warning). The pilot return moves when its worksheet is entered — that is the fix.

### The queue right now
- **1120-S** (`1120S\CC Changes\`): EMPTY (swept at boot s226).
- **1040** (`1040\CC Changes\`): EMPTY (swept at boot s226).
- **Legacy root** (`CC Code Changes\`): 3 open files — the pilot batch (its #7
  is the unit above), `CC_A_M_REMAINING_BLOCKERS` (six code requests, never
  blocked — DECISIONS item 1), and the NZ file (**9 of 10**; #10 multi-state
  stays parked under the states-on-hold ruling).
- After #7: the s224 ruled-and-buildable list stands — Form 6765 (RS spec
  approved), 8853 Section C, §213(d)(10) LTC cap, 1065 K17a, GA bulk-sale,
  both e-file refusals, identity read-back, 1310 box B upload +
  `ForeignAddressType`, CR-2026-001.

### ✅ s226 in one paragraph
Pilot #7's blocker was the missing RS spec; Ken chose spec-first (option (a)),
then walked Gate 1 the same session. Authored `load_1040_k1_basis_704d.py` from
freshly-fetched primary sources (§704(d) verbatim; the 2025 Partner's
Instructions ordering basis→at-risk→passive→EBL, agreeing with FORM_6198 R008;
Reg §1.199A-3(b)(1)(iv) QBI timing). The shape: the preparer ASSERTS allowed +
suspended from the source return's worksheet; the app routes `max(raw,
−allowed)` once in `k1_sche_net()`, checks the two identities, and diagnoses —
never derives the limit. Integrity gate green (independent re-typing). ⚠ The RS
source_type RATCHET caught the first draft copying `"statute"` from the 7206
template — the valid choice is `code_section`; never copy an older loader's
vocabulary without checking the enum. Two sources carry `requires_human_review`
on verbatim status (Reg §1.704-1(d) — eCFR blocked the fetch; §704(d)(2)'s odd
"repaid" sentence).

### ⚠ Classes that MOVE existing returns or output on next recompute
- **Nothing moves from s226** — no app code changed. The RS DB gained one form
  spec (additive; RS suite green, 226+13).
- ⚠ **NZ #2, when it is worked, WILL move returns** — worksheet rounding
  touches the regular SS worksheet. Budget a movement analysis for it.

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
  `test_ga500_rie_federal_pull.py`.
- **Client typecheck**: 127 error lines on clean `main` (re-measured s224).
- ⚠ The Slate 8889 fixture is cast `as HSAAccountRow` — new required fields
  don't fail the typecheck; worth dropping the cast.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB.
- **The hazard is CROSS-REPO** (s221): both repos name their test database
  `test_postgres`. `--reuse-db` works; `--create-db` collides.
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout loses
  ALL output — redirect to a file and tail it (s224).

### ✅ KEN DECISIONS OUTSTANDING — none new; queue still empty
Pilot #7's decision (spec-first) AND its Gate-1 walk both closed in-session
s226. The one live external item: **1040 v5.4 business rules are still not in
hand and go active 2026-08-09** (v5.4 schemas ARE on disk; 1041 v5.5 closed).
Carried items (8853 scope, 1099-G Rev-12-2026 renumbering, 1310 box B upload,
`ForeignAddressType`, W-2G movement class, MISC 13a/13b/14 → Sch 1-A, 8582
MAGI class, duplicate client pairs, K15a contamination, GA bulk-sale, §213(d)(10),
suffix list, D_4562_BASIS/VCLASS escalations) — see DECISIONS.md / ARCHIVE.

### RS AGENDA
- **NEW (s226)**: confirm the two `requires_human_review` verbatims on
  K1_BASIS_704D's sources when convenient (Reg §1.704-1(d); §704(d)(2)).
- Carried: s224 items (8889 line-4 Archer note — DONE in the 8889 spec?
  verify; the build-plan lane-leg pattern; 1099-G revision authority; the
  `f<form>.pdf`-is-next-revision pattern; missing-COLUMN-read-as-missing-BOX),
  s223 and earlier unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
