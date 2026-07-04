# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-04, fifth session (**BROKERAGE 1099-B SUMMARY-EXTRACTION SKELETON
BUILT** — commit `c25635f`, mig 0160 applied to the shared DB). Session began on the S3
resume pointer but Form 4835 has NO Rule Studio spec (404; RS server confirmed up), so per
the mandatory RS rule I STOPPED rather than improvise; Ken ruled "pivot to an unblocked
unit" → built the season-one brokerage import boundary end-to-end (8949 Exception 2).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gate
- **1040 flow-assertion gate: 381** — `cd server && pytest tests/test_flow_assertions.py -q`
  (held this session; ran inside a 425-passed combined regression run).
- **This session's verification:** new `test_brokerage_summary_skeleton.py` **8/8**
  (`--create-db`, 8m19s) · combined `test_flow_assertions` + `test_topic9_compute_leg` +
  `test_topic9_diagnostics_leg` = **425 passed** after the trip-wire fix (`--reuse-db`, 41m) ·
  registration trip-wire re-run green (16→17 Schedule D rules). mig 0160 applied to the shared DB.
- Test DB `test_postgres` is CURRENT (recreated this session, migs through 0160).

## ▶ RESUME HERE — idle, Ken directs (S3/S4 blocked on missing RS specs)
The brokerage 1099-B summary-extraction skeleton is DONE (commit `c25635f`). The next queued
1040 ATS scenarios are **blocked**: Form 4835 (S3) and Forms 8835 + 8936 (S4) return **404**
from Rule Studio (`lookup/<form>/export/`) — no spec exists. RS server is up (8283 → 200).
Per the mandatory RS rule, these units cannot be built until Ken authors the specs in Rule
Studio. Only Form 3800 (S4) has a spec.

**Unblocked pickups if Ken wants to keep going (no new spec needed):**
- Proforma/rollover snapshot **PRODUCER** (read/roll side already built, migs 0105/0106) —
  was parked "until Ken's back in office"; confirm before starting.
- Any Ken-directed unit.

**What the brokerage skeleton is / isn't (so the next session doesn't rebuild it):**
- IS: `apps/returns/brokerage_1099b.py::import_brokerage_summary()` — normalized per-box
  category totals → Exception-2 `CapitalTransaction` summary rows (idempotent per broker,
  `provenance=IMPORTED`, `import_confirmed` gate `D_8949_006`). Compute auto-applies code M
  to summary rows (Leg A, RS `R-8949-SUMMARY`). Model + mig 0160 add
  `provenance`/`import_source`/`import_confirmed`.
- ISN'T (deferred to Aug "extraction to production", by design): the OCR/AI PDF parsing
  front-end that produces the `categories` payload; the frontend YELLOW rendering of imported
  rows; a broker-PDF-as-BinaryAttachment wire into a real e-file scenario.

**Waiting on Ken (carried):**
1. **Author RS specs for 4835, 8835, 8936** to unblock S3/S4 (or reorder scope).
2. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5); **S2
   ready** — sign via `mef_build_ats_scenario2 --efin … --practitioner-pin … --taxpayer-pin …`.
3. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
4. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
5. **One-time:** create the public GitHub repo `klill6506/tts-tax-status` (mirror push fails
   until then; local mirror repo is ready and committing).
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
