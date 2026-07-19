# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-19, session 100 (autonomous — Ken out of office).
**THE RENUMBER-STALE-KEY SWEEP SHIPPED (the s98 chip; no migrations;
flow 518 stands) — and it caught a LIVE BUG: the 1120-S other-deductions
rollup was writing the Form 7205 §179D energy-efficient-buildings box**
(`OTHER_DED_LINE_KEY` stayed "1120S_L19" after the 2025 face inserted
line 19; MeF would have e-filed TB other-ded totals as the energy
deduction). Fixed → L20; the four test pins that baked the bug in
retargeted; BOTH DBs audited — zero live returns damaged. Also fixed:
6 DEAD TB-mapping targets (1065 Guaranteed Payments silently dropped
on every TB import) + 14 RESOLVES-WRONG balance-sheet targets (the
shifted-by-one class: 1065 partners' capital landed on Other
Liabilities, 1120-S shareholder loans on Tax-Exempt Securities, …) —
all re-keyed to verified 2025 labels; COGS → 1125-A purchases; GP →
face line 10 (key added; partner rollup supersedes); 1120-S RE rule
dropped (engine-owned); SUBSCHEDULE_CONFIG form-gated (bare-key
collision class). NEW drift guard: tests/test_line_key_registry_sweep.py
13/13 — resolution + LABEL PINS across every enumerable registry, so
the next renumber fails loudly instead of misrouting dollars. Gates:
flow 518 · returns/mappings/imports 645 · S5/S6 parity + B11 13 · live
MappingRule audit 166/166 both DBs · reseeded BOTH DBs. Four
convention calls → REVIEW_QUEUE s100. `/bugs`: clean. WSDLs absent.*

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
1. **Start every session with `/bugs`** (s55; s100 sweep: clean).
2. **S-17g A2A channel still jumps the queue the moment the WSDLs land**
   (`docs/mef/wsdl/` still absent; .p12 DONE; ASID 61135801 ENROLLED).
3. **The autonomous queue is EMPTY** — the s71 ratified queue completed
   s99; the s98 chip (stale-key sweep) completed s100. Next candidates
   are ALL Ken-gated or Ken-directed: the 1120/709 authoring waves ·
   the 1120-S ATS upload lane (full scenario set + e-help) · the tts
   sched_b face-renumber (s99b call) · the SEC-5 plumbing (s95
   ratification) · S-24 prod backfill (key hand-off). **Idle — Ken
   directs**, unless the WSDLs land first.
4. **Ken-gated follow-ups:** S-24 prod backfill on the key hand-off →
   hub-ein blanking (s97) · SEC-5 plumbing on the s95 ratification ·
   S-22b triage confirmations (6252 · 9325) · sched_b renumber (s99b).
5. **Auth lane (s94):** Ken's env vars → `supabase_users --list` +
   live `supabase_login` verify → P1 identity model.
6. **Ken ratifications pending:** s100 (other-ded fix FYI + 3 mapping
   conventions) · s99 (Q4 4d + sched_b renumber) · s97 (S-24 trio) ·
   s96 (WISP 4 calls) · s95 (retention · PITR · HSTS preload) · s94
   (8879 col-C @ ATS + stockpiling proxy) · s93 (4h idle) · s89
   (8915-F rounding) · s85/s84 pairs · s83 Resend · s76..s72 notes.
7. **Design: Ledger live; cross-app application on Ken's go.**

## ▶ Waiting on Ken / external
1. **S-24 identity keys (s97):** copy both TAX_IDENTITY_* keys from
   local `server/.env` to Render `tts-tax-app` (SAME values) → CC runs
   the prod backfill (590 staged) → then the hub-ein blanking leg.
2. **Auth env vars (s94):** SUPABASE_URL + ANON_KEY (+ SERVICE_ROLE).
3. **A2A: ONLY the WSDL toolkit remains.**
4. **WISP v0.1 ratification (s96)** — 4 calls in REVIEW_QUEUE s96.
5. **SEC-5 [EXT] legs (s95):** SOC 2 pull · restore drill · PITR call.
6. **Email provider setup (s83)** — Resend, SPF/DKIM, Render env vars.
7. **Role assignments (s84):** `manage.py set_user_role`.
8. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
9. E-services email reply (S7/S8 · 8941 key-inversion · production flip · SOR).
10. File-1018 Lacerte reprint. 11. PWA install check. 12. Density feel-check.
13. s69: firm's real CAF number + fax on the Preparer records.
14. Demo Render service (env=demo; needs the DEMO key pair from
    `server/.env.demo` if/when it exists).
15. **ERO EFIN/PIN source (s94):** wire 8879/8878 cards from Preparer.
16. **Cross-app flag (s95):** delvio-1099 `filing_dashboard` →
    security_invoker on next touch.
17. **Beta-agreement security clauses (s96):** with counsel.

## Active gates
- **Flow-assertion gate GREEN at 518** (s94 level; s100 re-ran green —
  no formulas changed). Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) ·
  1040 415.
- NEW s100: test_line_key_registry_sweep 13 (resolution + label pins —
  **when a pin fails: verify the new face, re-key the registry, update
  the pin; never delete it**). s99: test_1065_schb_q4 7 · 1065+SchB
  band 153. s97: test_tax_identity 24. s94 suites stand:
  test_8879_8878 33 · returns 110 · MeF/extract 105 · tsc 0 ·
  vitest 300. s89: test_8915f 49 · FULL efile band 966.
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: migs through returns 0207 + audit 0004 +
  core 0004 + clients 0010 BOTH DBs (s100 shipped NO migrations);
  s100 reseeded BOTH DBs: seed_1065 (line-10 mapping key) +
  seed_default_mapping + seed_1065_mapping (the re-keyed TB rules);
  live MappingRule audit 166/166 resolvable on BOTH DBs.**
- ⚠ **Render prod still has NO identity keys** (s97 Waiting §1).
- ⚠ HSTS lands on the next tts Render deploy (s95).
- ⚠ Local test-DB after new migrations: the s86 setup_databases(keepdb)
  recipe, then `--reuse-db`.
- ⚠ Restart django-demo after server edits (--noreload) AND hard-reload
  the SPA tab afterwards (vite keep-alive; s97). The demo server is
  API-only — SPA probes ride the `vite` launch config at 5173 (s99).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help first).
- Design rollback points: tag `pre-ledger-design` · old presets.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
