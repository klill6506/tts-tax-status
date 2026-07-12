# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-11, session 55 (Ken-directed break from the build order).
**In-app bug reporter SHIPPED** (S-12 season-infrastructure item, per
WORK_ORDER_bug_reporting — spec file itself not found on disk; built from Ken's
summary): "Report a Bug" header button + Help menu item, auto-context capture
(route / form_code / section / tax_return_id / client+server versions /
user-agent / viewport / recent-errors ring buffer), `bug_reports` Supabase
table (migs bugs 0001/0002, RLS default-deny, applied to the shared DB),
firm-scoped `/api/v1/bug-reports/` (create/list/retrieve/patch — NO destroy,
candidates keep history), `manage.py bugs` triage tool, `/bugs` session-start
slash command (`.claude/commands/bugs.md`). Screenshots DEFERRED —
screenshot_ref always null. 10 new server tests green; tsc 0 / vitest 300
green; probe-verified live end-to-end (201 → row in Supabase → context shown
by `--show` → closed cannot_reproduce). Probe login user `cc_probe` created in
the shared DB (Dev Tax Firm membership).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE
**Ken directives standing (s48 + s52 addendum): work AUTONOMOUSLY down this list;
full gates + live probes; Ken-decisions → REVIEW_QUEUE with a recommendation, then
move on; mandatory session close before context exhausts.**
1. **Start every session with `/bugs`** (NEW s55) — list open in-app bug reports
   and triage with Ken. Reports are CANDIDATES; Ken decides. Any
   computation-touching fix re-runs regression before merge.
2. **RS renumber unit #2: SCH_K_1120S** (fresh session; fabricated 13f · missing
   17c AE&P · wrong L18 formula) → K1 → SCHL → 6198 → M3 line_map → 3800.
   Ledger: `docs/rs_handoff/2026-07-09_early_era_face_audit.md`.
   **Fold in the s54 RS amendment**: retirement-spec RET-G5 "unsupported
   exception 13" scenario is STALE (the 5329 full unit validates the whole
   i5329 01-23+99 table — 13 is valid; engine right; REVIEW_QUEUE s54).
3. **RS FA-export reconciliation pass** (queued since s32).
4. **S-20 B2-17 form units**: 8283-entity → 2553 → 2848 → 3115 app build.
5. **Ken ratifications pending (REVIEW_QUEUE):** R007 AMT-matrix · 40% transitional
   election · s49 candidates · s53 partner-percentage diagnostic.

## ▶ Waiting on Ken / external
1. If `WORK_ORDER_bug_reporting.md` contains requirements beyond the s55 chat
   summary, point CC at the file — reconcile in a follow-up leg.
2. R007 AMT-correction ratification + 40%-election ruling (s47).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. TaxWise forms-usage report. 8. Density feel-check (s52).

## Active gates
- **Full suite GREEN as of s54** (`cd9b186` — 5,382 P / 21 skip / 0 F / 0 E) and is
  the regression gate for bug-report fixes that touch computation.
- **Flow-assertion gate untouched s55** (no compute changes — bug reporter is
  additive: new app `apps.bugs`, UI, tests only).
- Client green s55: tsc 0 errors / vitest 300 passed (incl. the AppShell wiring).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- ⚠ RS renumber queue: SCH_K/K1/SCHL/6198/3800 still inherit fabrications.
- ⚠ ENGINE-BUG CLASS to watch (s54): DECIMAL/RATIO face lines fed through
  whole-dollar writers (`_write_row` `_q` / state `_format`) — check the write
  path whenever a rate-bearing line lands.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
