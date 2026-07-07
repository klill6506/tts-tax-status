# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-07, twenty-eighth session (Ken-directed: "go back to MeF — 1040 business
rules + v5.3"). Unit: **MeF SOR want-list intake + both mappers re-stamped to today's ATS-ACTIVE
versions (1040 2025v5.3 / 1120-S 2025v6.2) + business-rules loader + two engine fixes surfaced by
the revalidation**. Pin suite (session-27 residual) fully green. Two commits: `199cfff` (MeF) +
`117cb68` (whole-dollar zero-clear fix).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — MeF entity track: 1120-S ATS Scenario-5 doc mappers (1125-A first)
The SOR intake + re-stamps are DONE (below). Next MeF unit = **1120-S ATS Scenario-5 doc mappers**
(1125-A → 1125-E → 4562 → 4797 → 8825; all engine-computed, serialization-only) + Itemized*Schedule
attachment docs, then the engine-driven scenario build. **Spine (federal) still on deck: S-11 1041
legs 6-8** (leg 6 GA Form 501 next).

## What landed this session (both commits pushed)
- **MeF SOR want-list intake** (`199cfff`) — Ken pulled 5 SOR zips to `docs/mef/`, all hashed +
  extracted + recorded in `schema_versions.md` / `README_schemas.md`. **Received:** 1040 2025v5.3
  schemas + **business rules (first time 1040 BR in hand)**, 1120x TY2025v6.2 schemas+BR, 1065
  2025v5.3 schemas + full BR. **Missing / wrong:** no 1040 v5.4 BR; **1041 arrived v3.0, not the
  wanted ATS-active v5.3** — re-request from e-help (see Waiting-on-Ken).
- **Both mappers re-stamped to today's ATS-active versions** (same commit):
  - **1040 → 2025v5.3** (was v5.4; ATS-active until 8/9/2026). v5.3↔v5.4 delta = IRS5329.xsd only,
    not emitted. builder SCHEMA_VERSION + registration + 7 scenario modules/commands + tests. 59 pure green.
  - **1120-S → 2025v6.2** (was v6.3; v6.3 stays the Jan-2027 production stamp, flip back when its
    fall-2026 ATS window opens). Delta = IRS4255/8825/8933, not emitted. 10 tests green.
    ⚠ v6.2 Return1120S.xsd does NOT fix the returnVersion attr → local validation can't catch a
    wrong 1120-S stamp.
- **NEW `apps/efile/validation/business_rules.py`** (same commit) — resolves an ATS reject number →
  full rule text/category/severity locally. Handles both publication formats (1040 v5.3 CSV cp1252;
  1120x v6.2 XLSX). 5 tests. Unsigned-return reject rules now identified: F1040-310…318 + R0000-095/096.
- **Engine fix 1 — Form 2441 line 8** (same commit): the CDCC applicable percentage (0.20–0.35) was
  flattened to `"0"` by the session-27 whole-dollar sweep (it's a RATIO). Caught by IRS2441.xsd's
  CareExpensesDecimalAmt enumeration during the v5.3 scenario-5 revalidation. Ratio audit of
  1116/8829/6252 clean. Scenario 5+8 artifacts rebuilt+revalidated vs v5.3 (42 DB tests green).
- **Engine fix 2 — whole-dollar zero-clears** (`117cb68`): the session-27 sweep left literal
  `"0.00"` clears (K2 rental, K7/K8a Sch D, K9/K10/K8c/K9c/K15b/4/6 disposition lines) + one K2
  cents quantize — cents-form values that violate no-cents-anywhere. The session-27-updated
  RentalProperties pins caught them. Fixed; the 4 affected pins + dispositions/4797 (39) green.

## Active gates
- **Flow-assertion gate — 422 passed** (after both commits).
- Pure MeF suites: 59 (1040) + 10 (1120-S) + 5 (business_rules) green.
- Scenario 5+8 DB artifacts: 42 passed vs v5.3.
- **Pin suite (session-27 residual) — DONE**: m1_3b + 1040x snapshot + sch-F above_qbi + 25/29
  test_returns pins all green; the 4 that failed were the real zero-clear bug (now fixed +
  reverified). dispositions + 4797 compute (39) green (blast-radius of the clear fix).

## ▶ Known issues / deferrals
- **Broad entity/state regression sweep NOT fully re-run this session** (pooler badly degraded today
  — a 4-test batch took 7 min, the 29-pin batch 46 min with connection drops). Re-run covered the
  compute change's blast radius only (rental/Sch-D/disposition/4797 + Compute1065/1120). The
  state/2210/retirement/K-1/render suites are UNTOUCHED by this session's engine changes and were
  green last session — re-run when the pooler recovers, low priority.
- **schema_locator version-string collision**: 1040 v5.3 owns `schemas/2025v5.3/`; the 1065 v5.3
  tree (same version string, also ships `IndividualIncomeTax/`) is STAGED under
  `schemas/bmf_july/1065_2025v5.3/` — the locator needs family awareness before the 1065 mapper leg.
- RS spec follow-ups carried from session 27 (8960 4a auto-feed, 8582 cosmetic, etc.) unchanged —
  see `docs/rs_handoff/2026-07-07_*.md` + STATUS_ARCHIVE.

## ▶ Waiting on Ken / external
1. **1041 ATS-active v5.3 schemas+BR** — the SOR drop gave v3.0 (old); re-request from e-help.
2. **1040 v5.4 business rules** — for the 8/9/2026 version flip (not in this drop).
3. SOR mailbox watch continues (1120-S family access, etc.).
4. IFA-upload 1040 scenarios 8 + 5 (now v5.3-stamped, artifacts rebuilt) — real EFIN + PINs to sign.
5. TY2025 4868 schema (carried).

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
