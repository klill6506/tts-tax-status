# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 126e (QA Batch-001 **1065 partnership repair
brief** — item 7 SHIPPED on top of items 1-6; migrations **0222 + 0223 + 0224 +
0225 STAGED, NOT applied**; nothing pushed).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s119–s124 detail is archived in `STATUS_ARCHIVE.md`; s125–s126e per-item
  detail lives in the BUILD_ORDER 🟢 ledger blocks.)*

## ▶ RESUME HERE — the 1065 repair brief, item 8 (Georgia partnership package)

**Ken's brief** (`D:\tax-test-data\QA Reports\1065\Batch-001\CC_1065_FIXES.md`)
is a 9-item partnership repair against seeded return
`ed62b605-84bb-4aaf-bd3b-273e0b834387` (409 Family Holdings LLC). Work it
**in Ken's order**. Do not create or delete returns. **Do not push or deploy**
— Ken authorises that separately.

Item 8 scope (from the brief): add Form 700, required schedules/partner
summaries, and GA-8453P to the 1065 package; this return is an even/zero
Georgia return with two Georgia partner summaries and no payment; State Only
and All Forms must include the Georgia forms. ⚠ **AUDIT FIRST** —
`render_ga700_overlay` already exists (the GA-700 seed, spec mirror
`server/specs/ga700_spec.json`, FORMULAS_GA700, and RULES_GA700 all exist
too), so this is likely wire-into-the-1065-package + partner summaries +
GA-8453P, not build-from-scratch. Check what `seed_ga700` seeds and whether a
GA-700 return object gets created alongside the 1065 (the GA-500/GA-600S
attach patterns are the precedents).

### Done — items 1-2 (s125, `13ee449`) · 3 (s126, `5cb9d7c`) · 4 (s126b, `8868341`) · 5 (s126c, `efdf902`) · 6 (s126d, `7bfe0cf`) · 7 (s126e, `8f6a78e`)

- Items 1-6: see BUILD_ORDER ledger / tracker.
- **Item 7 — Form 8990 §163(j): COMPLETE.** RS spec 8990 mirrored + implemented
  verbatim (R-8990-BIE/ATI/LIMIT/ALLOW/PTE; the spec's 6 scenarios are
  test-pinned). New seeded `f8990` section (keys BI<face-line>; 356→401),
  compute in FORMULAS_1065 + a post-pass for the 5-decimal line-35 ratio
  (the registry pass quantizes to whole dollars), real field map replacing
  the empty stub (geometry-mapped — no tooltips), manifest registration
  (template was on disk UNREGISTERED), packet inclusion behind Schedule B-1
  gated on Sch B Q24/entered data, 5 info diagnostics (D_8990_*), client
  tab + face-number chips. i8990-verified: line 22 zero-floor (spec silent),
  line 30 smaller-of, $31M §448(c) for 2025, ratio "0" prints when line 26
  is zero. Deliberate gaps in DEFERRAL_AUDIT s126e: Sch A/B row grids
  (totals only), CFC header block, Part III (1120-S leg), **K-1 EBIE/ETI
  box flow (13K/20AE-AF) — the flagged next 8990 unit**.

### Items 8-9 — not started

Carry-forward: item 9 pct precision (`max_digits=8, decimal_places=5`
migration; regression values in the brief §9 — 1.00013 / 98.99987 / 100).

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE)

1. **RS D_8990_DISALLOW vs D_8990_EBIE conflicting carryforward guidance on a
   1065** (s126e) — recommend scoping DISALLOW to non-partnerships in RS.
2. **Retire MATH_BALANCE_SHEET's 1065 arm?** (s126d).
3. **RS R-M2-3-TIE adjudication** (s126b).
4. **K-1 box 13/11 type codes** (s125).
5. **RS 1065_B stale D_B2_B1 note + 5 unbuilt Sch B diagnostics** (s126).
6. **s124's `D_4562_RECON` scoping pair**.
7. **Real One Heart EIN in committed test fixtures** (s126e chip
   `task_f06ee3ed`) — synthetic swap vs public-record acceptance.

## Active gates
- **Nothing pushed.** Local commits: `13ee449` · `5cb9d7c` · `8868341` ·
  `efdf902` · `7bfe0cf` · `8f6a78e` (+ docs commits). Migrations
  **0222+0223+0224+0225 STAGED, NOT applied**. At deploy: `seed_1065` rerun
  (f8990 section, 401 lines) + `seed_rules` BOTH DBs (D_M2_1 + the 5 D_8990_*).
- ⚠ Live QA follows Ken's push+migrate (staged columns block live 1065 partner
  screens); the test DB carries the proof — items 6 and 7 both proven there,
  live browser QA deliberately skipped per the s126b rule.
- **Gates at s126e:** NEW server `test_1065_8990.py` **23** (spec scenarios
  A-F verbatim · zero-floor edge · tri-state quiet arms · QA zero-amount
  packet regression · PDF face values by template-rect sweep) · seed
  trip-wires re-baselined (lines 356→401 · sections 10→11 · manifest 96→97) ·
  diag+seed+flow band **585** · 1065 band **87** · forms band **201** ·
  vitest **557** (+1) · tsc **52 = baseline**.
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
