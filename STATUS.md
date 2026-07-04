# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-03, third session (**8995-A SCHEDULE D PATRON UNIT BUILT** — Ken ruled
"Is this a MeF test scenario? If so build 8995-A Schedule D" for the S2 leg. Spec-first RS
round-trip (RS `9b4490c`: +3 facts, SD line map, D_8995A_001/002 error→info per Ken, +008/009,
T11–T15 hand-computed, FA-08 formula_check; export drift = exactly the amendment; canonical
refreshed 24 facts/15 tests). tts `f10e864` + `2c6e8a1`, **mig 0158 APPLIED**: engine
below-threshold face line-3 skip + per-column SD2–6 chain + DPAD cap min(input, L33−L37);
`form_8995a_patron_engaged` routing (patron → 8995-A at ANY income) replaced the retired
red-defer; D_8995_001 retired-dormant; render merges f8995ad copies (23-widget map bijective);
FormEditor patron-alloc inputs; MeF `IRS8995A` + `IRS8995AScheduleD` builders + F8995ASource
extract, ReturnData slots XSD-verified. Earlier same session: the 4797 K9c gate CLEARED
(cca5e34 verified 17/17) and the season-checklist protocol adopted.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gate
- **1040 flow-assertion gate: 381** (FA-1040-8995A-08 amended in place — gating→formula_check).
  Run: `cd server && pytest tests/test_flow_assertions.py -q` — **381 passed this session.**
- **This session's verification (all PURE gates green):** 8995-A math 17/17 (15 spec scenarios
  incl. T13 = the S2 Jones below-threshold zero-wage patron) · render pure 7/7 (f8995ad map
  bijective vs the 23-widget PDF) · efile pure **31/31** incl. LIVE 2025v5.4 XSD validation of
  IRS8995A + IRS8995AScheduleD · flow 381 · tsc 0 · vitest 275 · `manage.py check` clean.

## 🔴 OPEN GATE — the 8995-A DB legs (pooler-blocked tonight)
Three runs all died as fixture connection-kills (the documented class; zero real failures —
nothing even executed). When the pooler recovers, run:
`cd server && pytest tests/test_8995a_compute_leg.py tests/test_8995a_input_leg.py
tests/test_8995a_render_leg.py tests/test_8995a_diagnostics_leg.py tests/test_8995a_seed_leg.py
-q --reuse-db` (venv python directly; be patient 20+ min).
- ALSO pending: **`seed_rules` on the shared DB** (D_8995A_008/009 registration + the reworded
  001/002) — run it before relying on the new findings in the UI.
- The unit does NOT tick in the tracker until these are green (four-leg rule).

## ▶ RESUME HERE — S2 (Jones) scenario + mapper leg (the QBI blocker is CLEARED)
Ken's QBI-patron question is RESOLVED (Schedule D built). The scenario leg needs:
1. **Enacted-pin divergence (S13-class):** the key stipulates 13a BLANK ("patrons do not
   qualify" = ATS fiat); enacted law computes a NONZERO 13a (L13 = 20% × Sch C QBI via the
   face line-3 skip; reduction $0 — no qualified payments given) and the artifact carries
   IRS8995A + IRS8995AScheduleD the key lacks. Hand-compute the full enacted pin set.
2. **Fill forensics:** +29/−29 char-shift font; checkboxes are VECTOR marks — use
   `page.get_drawings()` or pin from the key. Facts in auto-memory [[s2-identity-arms-and-27c]].
3. **NRA statement PDF:** generate in `ats/scenario2.py`, ride `extract.binary_attachments`.
After S2: S3 (4835) → S4 (3800/8835/8936 — OBBBA-sunset check at each spec leg).

**Waiting on Ken (carried):**
1. IFA-upload scenarios 8 (SIGNED) + 5, bring back the acks — REBUILD both artifact sets first.
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
4. **One-time:** create the public GitHub repo `klill6506/tts-tax-status` (mirror push fails
   until then; local mirror repo is ready and committing).
5. Preparer Manager visual-review batch (carried list in STATUS_ARCHIVE) — **NEW: the patron
   Sch D allocation fields (Sch C + F QBI sections) + the softened DPAD label**.

## ▶ Carryover follow-up (older, still open)
- 1065 beyond the SE unit resumes only on Ken's explicit direction — `1065_status_assessment.md`.
- Minor parked sweep: 1065-section selects non-key uncontrolled; legacy `boxCheckbox` (pre-existing).
- EIN/ZIP re-import stays PARKED.

## Authoritative files read at boot
- `SPRINT_SCOPE.md` · `SEASON_PLAN.md` (dated Jan-2027 runway) · `SEASON_CHECKLIST.md` (tick-list) · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` · `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md` (when RS work on deck).
