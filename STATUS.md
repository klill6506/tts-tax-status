# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 126f (QA Batch-001 **1065 partnership repair
brief** — item 8 SHIPPED on top of items 1-7; migrations **0222 + 0223 + 0224 +
0225 STAGED, NOT applied**; nothing pushed).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s119–s124 detail is archived in `STATUS_ARCHIVE.md`; s125–s126f per-item
  detail lives in the BUILD_ORDER 🟢 ledger blocks.)*

## ▶ RESUME HERE — the 1065 repair brief, item 9 (five-decimal percentages)

**Ken's brief** (`D:\tax-test-data\QA Reports\1065\Batch-001\CC_1065_FIXES.md`)
is a 9-item partnership repair against seeded return
`ed62b605-84bb-4aaf-bd3b-273e0b834387` (409 Family Holdings LLC). **Do not
push or deploy** — Ken authorises that separately.

Item 9 scope (the LAST item): increase partner percentage precision to at
least five decimal places (`profit_pct`/`loss_pct`/`capital_pct` + BOY
variants are `max_digits=7, decimal_places=4` → the brief's
`max_digits=8, decimal_places=5` migration, which will be **0226 STAGED**);
validate total BOY/EOY profit/loss/capital percentages with an appropriate
tolerance; regression values in the brief §9 — Floyd ending capital
**1.00013%**, One Heart **98.99987%**, total **100%** (currently rejected
with HTTP 400 "no more than 4 decimal places"). Serializer + client
step/precision + the K-1 item-J / B-1 / GA Sch-4 percentage prints all
consume these fields — sweep the consumers (the item-6 lesson).

### Done — 1-2 (s125) · 3 (s126) · 4 (s126b) · 5 (s126c) · 6 (s126d) · 7 (s126e, `8f6a78e`) · 8 (s126f, `47b6f94`)

- Items 1-7: see BUILD_ORDER ledger / tracker.
- **Item 8 — Georgia partnership package: COMPLETE.** The engine existed;
  what was missing: **Schedule 4** (Income to Partners — the v1 deferral,
  now filled rows A-E from the federal partners per RS ga700
  R-GA700-PARTNERS: resident = full share, nonresident = ratio-apportioned
  GA-source; residency derived from address state), **Georgia Partner
  Summary** (one house statement page per partner, the Lacerte GAPL0201L
  content), **GA-8453P** (official 2024 DOR template — no 2025 published
  yet, flagged), and the packet wiring: `render_ga700_package` now feeds
  BOTH State Only and All Forms. **Bonus find: a live pre-existing compute
  defect** — the formula pass's whole-dollar quantize destroyed every
  fractional GA apportionment ratio (0.5 stored as 1, 0.4 as 0; prior legs
  only exercised 1.000000). Fixed via `SUB_DOLLAR_LINES` (GA-700 S7_2
  only; the pass is bit-identical for every other form), regression-pinned.

### Item 9 — not started (the last one)

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
  `efdf902` · `7bfe0cf` · `8f6a78e` · `47b6f94` (+ docs). Migrations
  **0222+0223+0224+0225 STAGED, NOT applied**. At deploy: `seed_1065` rerun
  (401 lines) + `seed_ga700` rerun + `seed_rules` BOTH DBs (D_M2_1 + D_8990_*).
- ⚠ Live QA follows Ken's push+migrate; the test DB carries the proof
  (items 6-8 all proven there; live browser QA deliberately skipped per the
  s126b staged-migration rule).
- **Gates at s126f:** NEW server `test_1065_ga_package.py` **12**
  (R-GA700-PARTNERS resident/nonresident split · EIN display · two summary
  pages · 8453P template-rect pulls · State Only + All Forms inclusion ·
  even/zero no-payment QA regression · the ratio-collapse pin at 0.5/0.4) ·
  GA-700 legs + flow gate **545** · GA-500/1065/8990 band **92** ·
  server-only (client gates unchanged: vitest 557 · tsc 52 = baseline).
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
