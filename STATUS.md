# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-04, eighth session (**FORM 8936 + FORM 3800 UNITS COMPLETE — two
forms, all four legs each**. 8936: CleanVehicle per-vehicle model (migs 0162/0163) +
two-phase compute (Sch 2 1b/1c repayment before the credit chain; Sch 3 6f/6m) + the
FACE-verified transferred-credit stop + PNG-verified render + UI tab, tag
`1040-form-8936-complete` @ `44c4f15`. 3800: the FULL same-session RS round-trip on Ken's
"go ahead" — the 1040-side amend-by-lookup spec authored (RS `5407bb2`), J1-J4 scope +
W1-W4 review walks approved, seeded + export-verified, then the tts unit (mig 0164):
compute_3800 (§38(c)(1) Section A verbatim; §38(c)(4)(A) Section C with TMT ZEROED;
line 38 → Sch 3 6a, computed LAST among nonrefundable credits), passive tri-state gates,
carryforward statement page, subset field map over the 9-page face + PNG pass, Business
Credit UI tab. **D_8911_004 RETIRED · D_8936_003 softened to info** (both re-seeded live).
Combined gate **451 passed** (7:04). The W4 S4-shape pin: the 8936 personal credit absorbs
the tax first → the whole $13,200 specified 8835 credit carries, 6a blank — Ken-blessed.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gate
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q`
  (**HELD + extended**: 369 assertions in the JSON / 391 gate tests; combined with all
  3800 + 8936 + 8911 leg files = **451 passed**, reuse-db 7:04).
- Test DB `test_postgres` CURRENT (migs through **0164**; the scratchpad
  `migrate_test_db.py` NAME-override trick).
- Shared (prod) DB: migs 0162/0163/0164 applied; `seed_8936` (25 lines) + `seed_3800`
  (42 lines) + `seed_rules` run (D_8911_004 is_active=False + D_8936_003 info + the
  D_3800_001-006 family verified live). `seed_all` re-runs idempotently on deploy.
- RS: the 3800 1040-side amendment SEEDED (RS commit `5407bb2`); deployed
  `lookup/3800/export/` verified (entity rules kept, 42 lines, P3 rows, 38 → SCH_3.6a);
  canonical `server/specs/3800_spec.json` cached.

## ▶ RESUME HERE — Form 8835 unit (the last S4 form), then the S4 scenario
1. **Form 8835 unit** (§45 renewable electricity) — spec cached (`8835_spec.json`, 7 rules /
   38 lines / 5 diags / 5 tests + `form_8835_authoring_notes.md` with the S4 solar vectors:
   440,000 kWh × $0.006 × 5 [PWA ×5 increased credit] = **$13,200**). Its landing is
   ALREADY WIRED: `compute_3800.form_3800_inflows` try-imports
   **`compute_8835.form_8835_credit(tax_return) -> (amount, "1f"|"4e")`** — implement that
   helper (the 1f-vs-4e §38(c)(4)(B)(iv) 4-year-PIS-window routing = the spec's own
   R-8835-ROUTE; S4's facility PIS 9/22/2023 → **4e**, specified). Passive assertion
   `Taxpayer.f3800_ps_8835` already exists. Standard four legs; multi-facility model
   decision at build (the S4 scenario has ONE facility).
2. **S4 scenario + mapper leg** — all three forms then exist. ⚠ The scenario carries a
   "Transfer Election Statement" attachment but the draft 8936 face has 4a=No — reconcile
   at the key-forensics step (a transferred credit NEVER lands on Sch 3 — the face-verified
   stop). ⚠ The blessed shape: line 16 tax ~2,193 absorbed by the 8936 personal credit →
   3800 line 38 = 0, Sch 3 6a blank, the $13,200 carries (the carryforward statement is
   part of the expected output). New MeF mappers: IRS8835 / IRS8936 (+Schedule A doc?) /
   IRS3800 — 1:1 vs the 2025v5.4 XSDs.

**RS follow-ups (next RS session, small):**
- FA-1040-8911-04's RS-side description still says "Form 3800 unbuilt" — refresh the text
  (the tts runner already pins the new reality: D_8911_004 retired, line 3 → row 1s).
- The RS 8936 spec: add the FACE-verified transfer stop (R-8936-TRANSFER — a qualifying
  dealer-transferred credit never re-lands; only a denial repays) + note D_8936_004's
  wrong-year-PIS extension. Both flagged in DEFERRAL_AUDIT (eighth session, parts 1-2).

**Waiting on Ken (carried):**
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5); **S2 + S3
   ready** — sign via `mef_build_ats_scenario2/3 --efin … --practitioner-pin … --taxpayer-pin …`.
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
4. Preparer Manager visual-review batch (carried): Sch A line-11 dotted literal + the
   "…of which qualified contributions" FormEditor input. **NEW: the Clean Vehicles (8936)
   and Business Credit (3800) tabs + the rendered 8936/SchA/3800 faces + the 3800
   carryforward statement** (PNG passes done in-session; Ken's visual habit).

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
