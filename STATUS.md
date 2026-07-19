# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-19, session 98 (autonomous — Ken out of office).
**TWO UNITS SHIPPED: (1) bootstrap_demo 1065+1041 demo returns · (2)
S-21b D_K1_PCT100 partner-percentage diagnostic.** Plus a REAL BUG found
and fixed building the 1065: **the 2025 renumber left the other-deductions
rollup key stale (`1065_L20` matched NO FormLine after other-deductions
moved 20→21) — every 1065's OtherDeduction rows silently never reached
the face.** Fix: seed_1065 keys line 21 as `1065_L21` + OTHER_DED_LINE_KEY
updated + the 12 TB-mapping rules retargeted; 1065 reseeded BOTH DBs;
regression pin test_1065_other_deductions 2/2 (incl. the key-resolves
drift guard). Demo now carries 11 returns across FOUR form families:
Blue Ridge Landscaping 1065 (2 partners 60/40, GP, other-ded statement,
K-1s persisted; every number ties: 23=275,600=K1, K-1s 165,360/110,240,
K14a=230,360) + Whitcomb Family Trust 1041 (complex trust; DNI 18,000 →
IDD 15,000 → TI 17,900, 2 beneficiaries 60/40). bootstrap_demo is now
TRULY idempotent (per-builder client-name guards — a re-run used to
duplicate every ATS client; the s98 dupes were cleaned, originals kept).
D_K1_PCT100 (ratified verbatim s71): WARNING when profit/loss %s over
ACTIVE partners ≠ 100; seeded BOTH DBs; 4 new DB tests; live demo probe
fires/restores clean. `/bugs`: clean. WSDLs absent.*

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
1. **Start every session with `/bugs`** (s55; s98 sweep: clean).
2. **S-17g A2A channel still jumps the queue the moment the WSDLs land**
   (`docs/mef/wsdl/` still absent; .p12 DONE; ASID 61135801 ENROLLED).
3. **The next NEW autonomous item is S-21c — the 1065 Schedule B Q4
   auto-answer (SPEC-FIRST: amend the RS 1065 SCH_B block first — the
   rs-amend-shared-form recipe — then the tts leg).** The approved Q11
   recipe clone: derived-YELLOW/overridable; (a) receipts < $250K +
   (b) assets < $1M from return data, (c) K-1s-timely PRESUMED TRUE
   (Ken-ratified), (d) from the M-3 flag; Yes waives Sch L/M-1/M-2/
   item L (the 1120-S sibling wiring). After that: the 1120/709
   authoring waves + the 1120-S ATS lane stay Ken-gated.
4. **Ken-gated follow-ups:** S-24 prod backfill on the key hand-off →
   hub-ein blanking (s97) · SEC-5 plumbing on the s95 ratification ·
   S-22b triage confirmations (6252 · 9325).
5. **Auth lane (s94):** Ken's env vars → `supabase_users --list` +
   live `supabase_login` verify → P1 identity model.
6. **Ken ratifications pending:** s97 (S-24 trio) · s96 (WISP 4 calls) ·
   s95 (retention · PITR · HSTS preload) · s94 (8879 col-C @ ATS +
   stockpiling proxy) · s93 (4h idle) · s89 (8915-F rounding) ·
   s85/s84 pairs · s83 Resend · s76..s72 notes.
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
- **Flow-assertion gate GREEN at 518** (s94 level; s98 re-ran green
  after the rollup-key fix — no compute formulas changed). Mirrors:
  1120S 41 · 1065 39 (+4 s64 staged) · 1040 415.
- NEW s98: test_1065_other_deductions 2 (the key drift guard) ·
  test_1065_k1_diagnostics_leg 11 (4 new PCT100) · 1065 band 42 green.
- s97: test_tax_identity 24 · blast band 122. s94 suites stand:
  test_8879_8878 33 · returns 110 · MeF/extract 105 · tsc 0 ·
  vitest 300. s89: test_8915f 49 · FULL efile band 966 · tts_forms 355.
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: migs through returns 0207 + audit 0004 +
  core 0004 + clients 0010 BOTH DBs (s98 shipped NO migrations);
  seed_1065 + seed_1065_mapping + seed_rules re-run BOTH DBs (s98).**
- ⚠ **Render prod still has NO identity keys** (s97 Waiting §1).
- ⚠ HSTS lands on the next tts Render deploy (s95).
- ⚠ Local test-DB after new migrations: the s86 setup_databases(keepdb)
  recipe, then `--reuse-db`.
- ⚠ Restart django-demo after server edits (--noreload) AND hard-reload
  the SPA tab afterwards (vite keep-alive; s97).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help first).
- Design rollback points: tag `pre-ledger-design` · old presets.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
