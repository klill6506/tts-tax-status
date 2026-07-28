# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 126d (QA Batch-001 **1065 partnership repair
brief** — item 6 SHIPPED on top of items 1-5; migrations **0222 + 0223 + 0224 +
0225 STAGED, NOT applied**; nothing pushed).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s119–s124 detail is archived in `STATUS_ARCHIVE.md`; s125–s126d per-item
  detail lives in the BUILD_ORDER 🟢 ledger blocks.)*

## ▶ RESUME HERE — the 1065 repair brief, item 7 (Form 8990)

**Ken's brief** (`D:\tax-test-data\QA Reports\1065\Batch-001\CC_1065_FIXES.md`)
is a 9-item partnership repair against seeded return
`ed62b605-84bb-4aaf-bd3b-273e0b834387` (409 Family Holdings LLC). Work it
**in Ken's order**. Do not create or delete returns. **Do not push or deploy**
— Ken authorises that separately.

Item 7 scope (from the brief): Schedule B question 24 is Yes and the Lacerte
source contains a three-page Form 8990 with zero current-year amounts. Add the
required 8990 facts, calculations, rendering, package inclusion, and
diagnostics; the QA return must include Form 8990 preserving the source zero
amounts. **RS spec is LIVE but there is NO app mirror — mirror the spec to
`server/specs/` FIRST** (carry-forward note from s126c scoping). The RS
`1065_B` spec's D_B24_8990 diagnostic (no app runner yet, see REVIEW_QUEUE)
is adjacent — check whether item 7 should land it.

### Done — items 1-2 (s125, `13ee449`) · item 3 (s126, `5cb9d7c`) · item 4 (s126b, `8868341`) · item 5 (s126c, `efdf902`) · item 6 (s126d, `7bfe0cf`)

- Items 1-5: see STATUS_ARCHIVE / tracker / BUILD_ORDER ledger.
- **Item 6 — Schedule L labels + BOY diagnostic: COMPLETE.** The audit
  settled the open question: `MATH_BALANCE_SHEET` (legacy generic in
  diagnostics/rules.py) is indeed a different, older rule than the
  RS-specced D_L family. Its 1065 arm read app **L14/L23** — face 13
  "other assets" vs face 21 "partners' capital" — which is why QA saw
  "BOY assets 0" on a saved, balanced 789,297 sheet. Now reads the true
  totals **L15/L24** (face line 14 total assets vs face line 22 total
  liabilities and capital). The valid EOY imbalance finding survives.
  UI: the 1065 Schedule L screen prints IRS face numbers 1-22 with
  subletters (app L7→7a … app L24→22) via a display-only map; face
  detected from the seed rows (item-4 precedent), 1120-S untouched;
  capital divider now "Partners' Capital" at app L23; Other-Liabilities
  detail drill-down moved to app L22 (face 20) from app L21 (face 19b).
  Internal keys and the internal→AcroForm translation untouched.
  ⚠ An out-of-balance 1065 now double-fires (MATH + D_L_BALANCE_*) —
  retire-the-1065-arm recommendation in REVIEW_QUEUE for Ken.

### Items 7-9 — not started

Carry-forward: item 7 Form 8990 (RS spec live, no app mirror — mirror
first) · item 8 Georgia (AUDIT FIRST — `render_ga700_overlay` exists) ·
item 9 pct precision (`max_digits=8, decimal_places=5` migration; regression
values live in the brief §9 — 1.00013 / 98.99987 / 100).

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE)

1. **Retire MATH_BALANCE_SHEET's 1065 arm?** (s126d) — double-fires with
   D_L_BALANCE_*; recommend scoping to 1120-S/1120.
2. **RS R-M2-3-TIE adjudication** (s126b) — M2_3 computed from M1_9?
3. **K-1 box 13/11 type codes** (s125).
4. **RS 1065_B stale D_B2_B1 note + 5 unbuilt Sch B diagnostics** (s126).
5. **s124's `D_4562_RECON` scoping pair**.

## Active gates
- **Nothing pushed.** Local commits: `13ee449` · `5cb9d7c` · `8868341` ·
  `efdf902` · `7bfe0cf`. Migrations **0222+0223+0224+0225 STAGED, NOT
  applied**. At deploy: `seed_1065` rerun + `seed_rules` BOTH DBs (D_M2_1).
- ⚠ The checked-out code cannot serve 1065 partner screens against the LIVE
  shared DB until 0222/0224 apply (staged columns) — live QA follows Ken's
  push+migrate; the test DB carries the proof (item 6 was proven there too:
  live browser QA deliberately skipped per the s126b staged-migration rule).
- **Gates at s126d:** NEW server `TestMathBalanceSheet1065Keys` **3**
  (QA-shaped 789,297 regression: BOY reads saved face-14, EOY finding
  survives, imbalance reads true totals; leg file 10 passed) · NEW client
  `schedL1065FaceNumbers.test.tsx` **10** (face chips · stale-seed face
  detection · dividers · detail drill-down · 1120-S unchanged) ·
  diagnostics+flow band **562** · 1065 band **84** · tsc **52 = baseline** ·
  vitest **556** (was 546, +10).
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
