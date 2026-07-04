# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-04, eighth session (**FORM 8936 UNIT COMPLETE — all four legs** —
Clean Vehicle Credits §30D/§25E/§45W, the first of the three S4 form units. Spec-first from
the cached RS 8936 + 8936_SCHA specs (authored 2026-07-04): `CleanVehicle` model (one
Schedule A per vehicle, mig 0162 + RLS 0163) + 4 Taxpayer facts (prior-year MAGI/status +
2 K-1 passthroughs) · `compute_8936.py` TWO-PHASE (repayment → Sch 2 1b/1c BEFORE the
credit-limit chain, the 8962 precedent; credits → Sch 3 6f/6m after 5695, before EIC/8812/
8911) · 7 diagnostics · seed_8936 (25 lines) · AcroForm field maps f8936 + f8936sa
(LABEL-VERIFIED bijective; 3-page SchA) + render_8936 + PNG-visual pass · CleanVehiclesSection
UI tab · 5 flow assertions FA-1040-8936-01..05. **Combined gate 422 passed** (flow + all four
8936 legs, reuse-db 2:54). Shared DB migrated + seeded; test DB migrated. tsc clean.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gate
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q`
  (**HELD + extended**: flow gate 364 assertions in the JSON / 386 gate tests + the four
  8936 test files = **422 passed** combined, reuse-db 2:54).
- Test DB `test_postgres` CURRENT (migs through **0163**; migrated in-session via the
  scratchpad `migrate_test_db.py` trick — settings NAME override + `migrate returns`).
- Shared (prod) DB: migs 0162/0163 applied; `seed_8936` (FORM_8936, 25 lines) + `seed_rules`
  (D_8936_001..007) run. `seed_all` re-runs idempotently on the next Render deploy.

## ▶ RESUME HERE — Form 8835 unit next (spec cached), then 3800 (BLOCKED on RS), then S4
S4 needs three form units; **8936 is done**. Next in order:
1. **Form 8835 unit** (§45 renewable electricity) — spec cached (`8835_spec.json`, 7 rules /
   38 lines / 5 diags / 5 tests + `form_8835_authoring_notes.md` with the S4 solar vectors:
   440,000 kWh × $0.006 × 5 = $13,200 → 3800 line 4e). Its credit PARKS behind a
   D_8911_004-style RED until 3800 lands (the 8911/8936 business-leg pattern).
2. **Form 3800 unit — ⛔ BLOCKED on Ken/RS**: the deployed 3800 spec is a thin 1120-S-era
   draft (10 generic lines, K-1-box-13 routing; NO Part III per-credit rows 1f/4e/1y/1aa,
   no Sch 3 6a destination, no passive split, no carryforward model). Cached at
   `server/specs/3800_spec.json` for inspection. → REVIEW_QUEUE (recommendation: author
   fresh from the 2025 f3800 + i3800, the 8835/8936 authoring-notes pattern, amend by
   LOOKUP — shared multi-entity form).
3. Then the **S4 scenario** (credit-limitation test: the $13,200 GBC vs the 8936 personal
   credit competing for ~$2.2k of tax; ⚠ the S4 draft face has 8936 line 4a transfer=No but
   the scenario carries a "Transfer Election Statement" attachment — reconcile at the
   scenario leg; a transferred credit NEVER lands on Sch 3, see the face-verified stop).

**RS follow-ups from this unit (for the next RS session, amend by lookup):**
- **R-8936-TRANSFER gap**: the RS 8936 spec is silent on the FACE-VERIFIED transfer stop
  (f8936sa 8c/8d + 13b/13c verbatim: a dealer-transferred credit stops before Parts II-IV —
  already received at the point of sale; only a DENIAL repays). Implemented face-correct in
  tts (FA-1040-8936-05 pins it); the spec needs the rule so it's never re-litigated.
- **D_8936_003 severity**: spec says info ("confirm 3800 wired"); tts ships it as ERROR
  while 3800 is unbuilt (the D_8911_004 no-silent-gap pattern; FA-1040-8936-04 pins it).
  Soften to info + retire FA-04 when the 3800 unit lands.
- **D_8936_004 scope**: tts also fires it for a wrong-year placed-in-service date (the spec
  condition covers blank only; the fact note says "must be in the tax year").
- FA-1040-8936-01..05 live only in the tts gate file — need RS loader homes
  (the fa-needs-rs-loader-home rule).

**Waiting on Ken (carried):**
1. **Form 3800 RS spec** — 1040-side authoring/amendment (blocks the 3800 unit + S4; above).
2. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5); **S2 + S3
   ready** — sign via `mef_build_ats_scenario2/3 --efin … --practitioner-pin … --taxpayer-pin …`.
3. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
4. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
5. Preparer Manager visual-review batch (carried): Sch A line-11 dotted literal + the
   "…of which qualified contributions" FormEditor input. **NEW: the Clean Vehicles (8936)
   tab + the rendered 8936/SchA faces** (PNG pass done in-session; Ken's visual habit).

## ▶ Carryover follow-up (older, still open)
- On the next RS Schedule D touch — (a) add `D_8949_006` to the 8949 spec; (b) consider an
  FA for the 1a/8a aggregate netting.
- RS SCHA spec fact for `scha_qualified_contributions_cash` (input-only) — amend by lookup.
- RS 8995 sibling loader's D_8995_001 retirement note (DEFERRAL_AUDIT third-session item 2).
- 1065 beyond the SE unit resumes only on Ken's explicit direction — `1065_status_assessment.md`.
- Minor parked sweep: 1065-section selects non-key uncontrolled; legacy `boxCheckbox` (pre-existing).
- EIN/ZIP re-import stays PARKED.

## Authoritative files read at boot
- `SPRINT_SCOPE.md` · `SEASON_PLAN.md` (dated Jan-2027 runway) · `SEASON_CHECKLIST.md` (tick-list) · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` · `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md` (when RS work on deck).
