# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-04, ninth session (**Form 8835 unit COMPLETE — all four legs green,
tag `1040-form-8835-complete`, migs 0165/0166 applied to the shared DB, flow gate 397**).
The S4 arc's three feeder units (3800 + 8936 + 8835) are now ALL built; next is the S4
MeF scenario + mapper leg.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gate
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q`
  → **397 passed** (was 391; +6 for FA-1040-8835-01..06). Fast (~2s, mostly pure).
  Combined with the 8835/3800/8936 leg files = **453 passed** (reuse-db 8:36, no regressions).
- Test DB `test_postgres` CURRENT (migs through **0166**); shared (prod) DB seeded through
  seed_8835/seed_3800/seed_8936/seed_rules. RS Supabase reseeded (8835: 9 diags / 6 FAs);
  cached `server/specs/8835_spec.json` refreshed from the live deployed export (5→9 diags).

## ▶ RESUME HERE — the S4 scenario + mapper leg (Form 8835/8936/3800 units ALL COMPLETE)
Build the S4 MeF scenario + mappers, 1:1 vs the 2025v5.4 XSDs:
- **New mappers**: IRS8835 · IRS8936 (+ Schedule A doc) · IRS3800.
- ⚠ **Reconcile the Transfer Election Statement attachment vs the draft face's 4a=No** — a
  dealer-transferred credit never lands on Sch 3. The BLESSED all-carries shape (pinned in the
  3800 W4 walk + verified end-to-end in `test_form3800_pipeline.test_pipeline_w4_ordering_gbc_is_last`
  and `test_form8835_pipeline.test_pipeline_s4_all_carries_shape`): the 8936 personal credit
  absorbs the tiny tax first (Sch 3 6f, inside 3800 line 10b) → 3800 line 38 = 0, **Sch 3 6a
  blank, the whole $13,200 §45 credit carries** + a carryforward statement page.
- The S4 solar facility vector: 440,000 kWh × $0.006 ×5 (8a/8b/8c all Yes) = **$13,200**,
  PIS 9/22/2023 → route **4e**.

**Form 8835 unit — DONE this session (commits `7399dca` pipeline/diag/render · `6147eba` UI ·
`6d0e1cf` FA; RS `f3e5cd8`):**
- Model: `RenewableFacility` (migs 0165/0166, one face per facility, J1) + Taxpayer
  `f8835_passthrough_credit`/`f8835_passthrough_pis_date` (J4).
- Compute: `compute_8835.py` — year-keyed 2025 rate table + OBBBA before-2025 gate + the
  Part II chain (×5/+10%/bond-15%/EPE-90%); `form_8835_state` (THE shared gatherer) →
  `form_8835_credit() -> (amount,"1f"|"4e")` LIVE into compute_3800 (runs BEFORE 3800, lands
  NOTHING on Sch 3). Routing R-8835-ROUTE (J3 binary-per-year).
- Diagnostics: `rules_8835.py` 9 rules — spec 001-004 + RATE_YEAR + the J2/J3/J4 RED-defers
  **D_8835_005 mixed / 006 straddle / 007 passthrough-unrouted / 008 missing-PIS** (all error,
  ≤20 chars, bridge-gated on form_8835_state; escape hatch = the 3800 1zz/4z facts).
- Render: f8835.pdf (seq 835, sha pinned) + `f8835_2025` LABEL-VERIFIED bijective map +
  `render_8835` (ONE FACE PER FACILITY via facility_lines; lines 14/15 first-face) + 3-page
  PNG pass + packet emit before 3800.
- UI: `RenewableFacilitiesSection` (Credits group, before 3800) + the two passthrough inputs +
  D_8835_ → form_8835 routing; tsc clean.
- FA: FA-1040-8835-01..06 (transcribed verbatim from the RS canonical set; runner
  `_run_8835_assertion`). RS loader homes seeded (D_8835_005-008 + FA-05/06).
- Gates: pure compute 19/19 (first run) · DB pipeline 8/8 · diagnostics 10/10 · render 6/6 · flow 397.

## ▶ RS follow-ups (carried from the eighth session, small)
- FA-1040-8911-04's RS-side description still says "Form 3800 unbuilt" — refresh the text.
- RS 8936 spec: add the FACE-verified transfer stop (R-8936-TRANSFER) + D_8936_004
  wrong-year-PIS note. Both in DEFERRAL_AUDIT (eighth session, parts 1-2).

## ▶ Waiting on Ken (carried)
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5); **S2 + S3
   ready** — sign via `mef_build_ats_scenario2/3 --efin … --practitioner-pin … --taxpayer-pin …`.
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
4. Preparer Manager visual-review batch (carried): Sch A line-11 dotted literal + qualified-
   contributions input; Clean Vehicles (8936) + Business Credit (3800) + **Renewable
   Electricity (8835)** tabs + the rendered 8936/SchA/3800/8835 faces + the 3800 carryforward
   statement.

## ▶ Carryover follow-up (older, still open)
- Next RS Schedule D touch — (a) add `D_8949_006` to the 8949 spec; (b) consider an FA for
  the 1a/8a aggregate netting.
- RS SCHA spec fact for `scha_qualified_contributions_cash` (input-only) — amend by lookup.
- RS 8995 sibling loader's D_8995_001 retirement note (DEFERRAL_AUDIT third-session item 2).
- 1065 beyond the SE unit resumes only on Ken's explicit direction — `1065_status_assessment.md`.
- Minor parked sweep: 1065-section selects non-key uncontrolled; legacy `boxCheckbox` (pre-existing).
- EIN/ZIP re-import stays PARKED.

## Authoritative files read at boot
- `SPRINT_SCOPE.md` · `SEASON_PLAN.md` (dated Jan-2027 runway) · `SEASON_CHECKLIST.md` (tick-list) · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` · `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md` (when RS work on deck).
