# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 126g (QA Batch-001 **1065 partnership repair
brief COMPLETE — 9 of 9 items shipped**; migrations **0222–0226 STAGED, NOT
applied**; nothing pushed).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s119–s124 detail is archived in `STATUS_ARCHIVE.md`; s125–s126g per-item
  detail lives in the BUILD_ORDER 🟢 ledger blocks.)*

## ▶ RESUME HERE — the brief is DONE; idle, Ken directs

**Ken's brief** (`D:\tax-test-data\QA Reports\1065\Batch-001\CC_1065_FIXES.md`)
against seeded return `ed62b605-84bb-4aaf-bd3b-273e0b834387` (409 Family
Holdings LLC) is **complete — all 9 items shipped locally**. **Nothing is
pushed or deployed** — Ken authorises that separately. What deploy day needs
is listed under Active gates.

### Done — 1-2 (s125) · 3 (s126) · 4 (s126b) · 5 (s126c) · 6 (s126d) · 7 (s126e) · 8 (s126f) · 9 (s126g)

- Items 1-8: see BUILD_ORDER ledger / tracker.
- **Item 9 — five-decimal percentages: COMPLETE.** Migration **0226 STAGED**
  widens `Partner` profit/loss/capital_pct (+ `_boy`),
  `PartnerAllocation.percentage`, and the `PartnerK1Computed.profit_pct`
  audit snapshot from numeric(7,4) → numeric(8,5). The brief's regression
  split (ending capital 1.00013% / 98.99987%, total 100%) was rejected with
  HTTP 400; it now saves, round-trips exactly, and prints on K-1 item J —
  proven against the rendered PDF bytes (template-rect sweep), per the
  brief. D_K1_PCT100 now also validates ending capital + the three BOY
  columns when in use (tolerance 0.01); ending profit/loss stay always-on.
  RS SCHEDULE_K1_1065 re-fetched — it carries no PCT100 diagnostic (the
  rule is the app-side Ken-ratified S-21b check), so no spec conflict.
  Client totals display five decimals; entry inputs needed no change.

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE)

1. **RS D_8990_DISALLOW vs D_8990_EBIE conflicting guidance on a 1065** (s126e).
2. **Retire MATH_BALANCE_SHEET's 1065 arm?** (s126d).
3. **RS R-M2-3-TIE adjudication** (s126b).
4. **K-1 box 13/11 type codes** (s125).
5. **RS 1065_B stale D_B2_B1 note + 5 unbuilt Sch B diagnostics** (s126).
6. **s124's `D_4562_RECON` scoping pair**.
7. **Real One Heart EIN in committed test fixtures** (chip `task_f06ee3ed`).

## Active gates
- **Nothing pushed.** Local commits: `13ee449` · `5cb9d7c` · `8868341` ·
  `efdf902` · `7bfe0cf` · `8f6a78e` · `47b6f94` · the s126g item-9 commit
  (+ docs). Migrations **0222+0223+0224+0225+0226 STAGED, NOT applied**.
  At deploy: migrate · `seed_1065` rerun (401 lines) · `seed_ga700` rerun ·
  `seed_rules` BOTH DBs (D_M2_1 + D_8990_* + the D_K1_PCT100 description) ·
  then live QA on the 409 Family Holdings return.
- ⚠ Live QA follows Ken's push+migrate; the test DB carries the proof
  (items 6-9 all proven there; live browser QA deliberately skipped per the
  s126b staged-migration rule).
- **Gates at s126g:** NEW server `test_1065_pct_precision.py` **12** (API
  400 repro · exact DB roundtrip · allocation + snapshot width · item J PDF
  prints via template-rect sweep · D_K1_PCT100 column/tolerance cases) ·
  K-1/diagnostics band **73** · flow + 1065 band **610** · client vitest
  **557** · tsc **52** = baseline.
- **A parallel session's uncommitted work stays unstaged**:
  `server/apps/returns/views.py`, `server/apps/returns/tb_import.py`,
  `server/tests/test_tb_import.py`.
- ⚠ FA-1040-4835-06 drift (chip `task_0cf10eac`).
- ⚠ One test DB — never overlap pytest runs.
- ⚠ `server/.venv` repaired s124; use `.venv\Scripts\python.exe` directly.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
