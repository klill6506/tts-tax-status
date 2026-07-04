# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-04, sixth session (**FORM 4835 RENDER LEG BUILT + GREEN → FORM 4835
COMPLETE, all four legs**; commit `cee838f`). Render leg: AcroForm field map
`f4835_2025.py` (63 widgets, BIJECTIVE label-verified), manifest entry (sha256 verified vs a
fresh irs.gov download), `render_4835()` (one copy per activity, model-driven via the pure
`compute_4835_lines` chain), registered in `ACROFORM_FORM_IDS`, packet emit after Schedule F,
`SKIP_PAGES["Form 4835"]={1,2}` (instruction pages stripped). No migration / no compute change.
Prior legs (compute/diagnostics/seed) shipped `c7cae44`, mig 0161.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gate
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q`
  (**HELD**: combined `test_flow_assertions` + all four 4835 legs = **412 passed**, reuse-db 15m).
- **4835 verification (ALL FOUR LEGS GREEN → FORM TICKS):** pure `test_4835_compute_leg.py`
  **13/13** (spec vectors T1-T12+S3) · `test_4835_integration_leg.py` **2/2** (S3 → Sch E
  40=11,061 / 42=17,035 / Sch 1 L5=11,061, none to SE) · `test_4835_diagnostics_leg.py` **5/5** ·
  **NEW `test_4835_render_leg.py` 11/11** (bijective map over all 63 widgets; income/loss/§263A
  face sweeps; matpart → no copy; one-copy-per-activity; packet places one page).
- Test DB `test_postgres` is CURRENT (migs through 0161; render leg added no migration).

## ▶ RESUME HERE — S3 scenario + mapper leg (Form 4835 is now COMPLETE)
Form 4835 is fully built (all four legs green). Remaining for S3:
1. **S3 scenario + mapper leg** — Lynette Heather (1040, 1099-R, Sch 1/2/D/E/F/SE, 4835,
   8995-A patron, Farm Optional Method). All forms now built; reuse the S2 forensics recipe
   (the +29/−29 key decode + vector-checkbox forensics, scratchpad notes).

Then **S4**: build 8835 + 8936 + Schedule A (specs cached in `server/specs/`), then the S4
scenario — a credit-LIMITATION test (see the 8835/8936 authoring notes + test vectors).

**4835 — what's built (don't rebuild), all four legs:** compute `compute_4835.py` (full loss
path §465→§469 $25k allowance + MAGI phaseout), model `Form4835` (mig 0161, multi-instance,
fields 1:1 with spec facts), `rules_4835.py` (9 diags), `seed_4835`; RENDER `render_4835()` +
field map `f4835_2025.py` + manifest `f4835` (sha256 verified) + `SKIP_PAGES["Form 4835"]`.
Feeds Schedule E line 40 (net) / 42 (gross); a Form4835 ENGAGES Schedule E; NEVER to Schedule
SE (§1402(a)(1)); matpart → no face (belongs on Schedule F). v1 render note: the six 30a-f
"other expenses" print as a single aggregate on line 30a labeled "Other" (model stores the sum).

**Waiting on Ken (carried):**
1. **Author RS specs for 4835, 8835, 8936** to unblock S3/S4 (or reorder scope).
2. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5); **S2
   ready** — sign via `mef_build_ats_scenario2 --efin … --practitioner-pin … --taxpayer-pin …`.
3. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
4. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
5. ~~**One-time:** create the public GitHub repo `klill6506/tts-tax-status`~~ **DONE 2026-07-04** —
   public repo created (`repo`-scope PAT), mirror pushed and verified live; `sync_status_mirror.ps1`
   now also mirrors RS STATUS.md + session_log.md into a `rule-studio/` subfolder.
6. Preparer Manager visual-review batch (carried in STATUS_ARCHIVE): the Sch A line-11
   dotted-literal position + the "…of which qualified contributions" FormEditor input.

## ▶ Carryover follow-up (older, still open)
- **NEW:** add `D_8949_006` to the 8949 RS spec on the next Schedule D touch (app-level
  workflow/confirmation gate, added in code this session — not silently written to the cached
  `8949_spec.json`).
- RS SCHA spec fact for `scha_qualified_contributions_cash` (input-only; DEFERRAL_AUDIT
  fourth-session item 3) — amend by lookup on the next Schedule A touch.
- RS 8995 sibling loader's D_8995_001 retirement note (DEFERRAL_AUDIT third-session item 2).
- 1065 beyond the SE unit resumes only on Ken's explicit direction — `1065_status_assessment.md`.
- Minor parked sweep: 1065-section selects non-key uncontrolled; legacy `boxCheckbox` (pre-existing).
- EIN/ZIP re-import stays PARKED.

## Authoritative files read at boot
- `SPRINT_SCOPE.md` · `SEASON_PLAN.md` (dated Jan-2027 runway) · `SEASON_CHECKLIST.md` (tick-list) · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` · `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md` (when RS work on deck).
