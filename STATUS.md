# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-26, session 115 (QA Batch-001 item 9 second half — the
Form 8962 Part IV shared-policy allocation SHIPPED end-to-end; item 15 design
proposal DRAFTED, awaiting Ken's pick).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s114 is archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s115 (2026-07-26): two deliverables.**

1. **QA Batch-001 item 15 — the design proposal Ken asked for is DRAFTED**
   at `Design/item15_source_summary_proposal.md` (`2b4936e`): Option A =
   per-record source-summary flag + reconciliation panel (the QA ask, ~1
   session); Option B = per-form "where did this number come from" provenance
   panel (2-3 sessions); **recommendation C = A first, B rides A's chrome**.
   NO BUILD until Ken picks.
2. **QA Batch-001 item 9 (second half) — Form 8962 Part IV SHIPPED** (app
   `9e13f89`; RS `16a5bc4`; mig 0215+0216 on BOTH DBs; deploy VERIFIED live,
   bundle `index-q3S2nCYI.js`, markers ×3/×1). The s75 "Parts IV/V unmodeled"
   boundary is half-closed: the 1095-A is now entered AS RECEIVED and the app
   multiplies by the allocation percentages (R-8962-PART4, the face's own
   line-34 mechanic, whole-dollar per policy-month; blank pct = retain 100%);
   ONE aggregation feeds compute + print + e-file; page-2 grid + line 9/10/34
   checkboxes print; MeF SharedPolicyAllocationGrp transmits; 4 new
   diagnostics (EMPTY error / OVERLAP error / BLANK_PCT warning / TOO_MANY
   error) + the s106e annual trio spec-homed in RS; client grid on each
   1095-A card behind the line-9 checkbox; live demo probe green (grid
   reveal + row round-trip), demo DB restored. **Part V (marriage alt)
   remains unmodeled** (flag-only, line 9 asserts it).

**Gates green:** part4 leg 15 · 8962 family 44 · efile/scenario/packet sweep
952 · acroform+mef 120 · flow **521** (new FA-1040-8962-07) · vitest 459 ·
tsc 52 baseline · RS harness check_8962_integrity ALL PASS (7 scenarios).

**▶ NEXT (cold-start pointer): Ken's pick on the item-15 proposal (A/B/C).**
After that, the remaining Batch-001 opens: **P0 item 6 (the depreciation
multi-book rebuild — BIG, needs Ken's design sign-off)** · item 16's
Schedule-A→8283 guided workflow (its conversion-mode half depends on item
15) · the item-6-P1 GA-sync residual (BLOCKED on the two REVIEW_QUEUE
questions) · the 2210 reconciliation panel (deferred UI). Spine otherwise
idle — Ken directs.

## ▶ Waiting on Ken / external
1. **s115: the item-15 proposal pick (A / B / C-recommended)** — see
   `Design/item15_source_summary_proposal.md`.
2. **s115 ratifications (REVIEW_QUEUE):** Part IV blank-pct = retain-100%
   semantics · line 34 always-Yes + the 4-row cap · line 9 true on the
   flag-only marriage-alt assertion.
3. **s114 ratifications:** the 8867 rebuild's three judgment calls.
4. **s113 ratifications:** D_GA500_002 realignment · 2210 flat-7% · 7206
   partner-arm scope.
5. **Item-6-P1 residual — BLOCKING questions:** GA line 5 filing status from
   federal? · couple the GA deduction election to the federal election?
6. **s112 ratification:** manifest-aware RS amendment (mechanism only).
7. **86 backfill review rows** (now 83 effective) · S-24 hub-ein blanking ·
   auth env vars · A2A WSDL · WISP · SEC-5 · Resend · role assignments ·
   e-services · CAF · ERO EFIN/PIN · beta clauses · older ratifications
   (s110 · s106 · s101(4) · s100(3) · s99a · s97 · s96(4) · s95..s72).

## Active gates
- **Deploy:** s115 push `9e13f89` VERIFIED live in-session (bundle
  `index-q3S2nCYI.js`; `form-8962-allocations` ×3 + the new line-9 label ×1).
  Nothing pending.
- **DB state:** mig 0215 (Form8962Allocation) + 0216 (RLS default-deny) +
  seed_form_8962 (42 lines) + seed_rules applied + VERIFIED on BOTH DBs
  (prod aws-1, demo aws-0). Prod audit: exactly ONE return carries
  f8962_shared_allocation — it now fires D_8962_PART4_EMPTY for guided
  re-entry (intended; the flag-only entry could never file Part IV).
- **RS:** specs.0003 widened FormDiagnostic.diagnostic_id 20→40 (applied to
  RS prod) — the app's canonical codes must live in the spec verbatim.
- ⚠ **FA-1040-4835-06 drift** (chip `task_0cf10eac`, unchanged from s113).
- ⚠ **Dev-environment nit:** an autoPort vite origin is not in
  CSRF_TRUSTED_ORIGINS — saves 403 from a coexistence-port dev client;
  reads work (bit again in s115's probe; worked around server-side).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
