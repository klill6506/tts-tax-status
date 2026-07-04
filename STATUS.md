# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-03, fourth session (**8995-A DB GATE CLEARED ✅ + S2 (JONES) SCENARIO
+ MAPPER LEG COMPLETE** — commits `89a4f88`/`2a151b5`/`c10e51c`/`9b5dcad`, mig 0159 applied.
The third session's two 🔴 items are both closed: the 8995-A Schedule D unit's DB legs went
green across three pooler-fought runs (4 failures were ALL seeder/test-side — the SD FormLines
were missing from `seed_8995a`'s SECTIONS list; new f8995a_schd section, 58 lines / 8 sections
reseeded on the shared DB; `seed_rules` also run) and the S2 scenario shipped end-to-end:
full key forensics, FOUR new MeF mappers (IRS1040ScheduleA / IRS8283 Section A /
PaidPreparerInformationGrp / Sch C AdditionalVehicleInfoGrp), scenario2.py + command +
29/29 test file, every enacted pin matching the engine, 11-document submission validating
against the live 2025v5.4 XSDs.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gate
- **1040 flow-assertion gate: 381** — `cd server && pytest tests/test_flow_assertions.py -q`
  (381 passed this session).
- **This session's verification:** 8995-A DB legs ALL GREEN (66→50→40 across three runs; the
  ~21 E/F pooler connection-kills all re-ran clean) · S2 test file **29/29**
  (`pytest tests/test_mef_scenario2_compute.py -q --reuse-db --timeout=1800` — the 13-seed
  fixture outlasts the default 300s cap) · scenario command **ALL MATCH** on every 1040 /
  Sch 1 / Sch A / Sch C / 8995-A-XML pin · efile pure 31/31 · acroform 33/33 · flow 381 ·
  tsc 0 · vitest 275. `seed_rules` + `seed_8995a` applied to the shared DB; mig 0159 applied.
- Test DB `test_postgres` is CURRENT (created fresh this session, migs through 0159).
  `scripts/drop_test_db.py --keep-db` (NEW flag) terminates stale sessions WITHOUT dropping —
  the killed/timed-out-pytest lock case.

## ▶ RESUME HERE — S3 scenario: Form 4835 unit first
S2 is done (artifacts in `docs/mef/ats_out/scenario2`, UNSIGNED placeholder EFIN — Ken's
upload list). Next per SEASON_CHECKLIST: **S3 needs Form 4835 (farm rental) built first** —
spec-first (Rule Studio lookup `4835`; if 404 STOP and tell Ken), then compute/render/input/
diagnostics legs, then the S3 scenario + mapper leg (reuse the S2 forensics recipe in
auto-memory [[s2-scenario-mapper-leg]]: +29 char-shift fills, [l]-path vector checks,
✔ glyphs). After S3: S4 (3800/8835/8936 — OBBBA-sunset check at each spec leg).

**Waiting on Ken (carried):**
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5); **NEW: S2
   ready** — sign via `mef_build_ats_scenario2 --efin … --practitioner-pin … --taxpayer-pin …`.
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
4. **One-time:** create the public GitHub repo `klill6506/tts-tax-status` (mirror push fails
   until then; local mirror repo is ready and committing).
5. Preparer Manager visual-review batch (carried in STATUS_ARCHIVE) — **NEW this session:**
   the Sch A line-11 dotted-literal position (abs_pos x=330/y=578) + the "…of which qualified
   contributions" FormEditor input.

## ▶ Carryover follow-up (older, still open)
- RS SCHA spec fact for `scha_qualified_contributions_cash` (input-only; DEFERRAL_AUDIT
  fourth-session item 3) — amend by lookup on the next Schedule A touch.
- RS 8995 sibling loader's D_8995_001 retirement note (DEFERRAL_AUDIT third-session item 2).
- 1065 beyond the SE unit resumes only on Ken's explicit direction — `1065_status_assessment.md`.
- Minor parked sweep: 1065-section selects non-key uncontrolled; legacy `boxCheckbox` (pre-existing).
- EIN/ZIP re-import stays PARKED.

## Authoritative files read at boot
- `SPRINT_SCOPE.md` · `SEASON_PLAN.md` (dated Jan-2027 runway) · `SEASON_CHECKLIST.md` (tick-list) · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` · `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md` (when RS work on deck).
