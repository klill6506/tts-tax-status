# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-19, session 99 (autonomous — Ken out of office).
**S-21c SHIPPED: the 1065 Schedule B Q4 auto-answer, spec-first**
(RS `7a55f57` · tts `b7f77b6`; no migrations; flow 518 stands).
RS 1065_B gained R-B4-AUTO (amended in its owning loader by lookup):
Q4 (app row B6) derives as a YELLOW/overridable answer — 4a receipts
< $250K strict per the i1065 verbatim sum (positive-only), 4b L15d
< $1M strict, 4c presumed TRUE (Ken-ratified), 4d derived under 4a·4b
(no M-3 support; RE-partner edge → override, ratification s99a).
tts: `_auto_answer_b6_1065_db` after the 1065 formula pass (the B11
clone); waiver wiring was ALREADY live (D_L_EXEMPT/D_M1/M2_EXEMPT +
GATE-SMALL-PTNR pins); B6 label re-cut face-verbatim + reseeded BOTH
DBs; client Auto pill now covers (1120-S,B11) ∪ (1065,B6). NEW
test_1065_schb_q4 7/7; the L/M tie fixtures now answer Q4 'No' by
override (the auto-answer had made those small fixtures legitimately
exempt). Live demo probe: Blue Ridge B6 auto-'false' (1a=485,000),
amber pill DOM-verified. TWO Ken calls filed (REVIEW_QUEUE s99): the
4d deviation + the queued tts sched_b FACE-RENUMBER (stale paraphrase
block: has non-face B2/B5, misses Q24/Q30/Q33). `/bugs`: clean (start
of session). WSDLs absent.*

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
1. **Start every session with `/bugs`** (s55; s99 sweep: clean).
2. **S-17g A2A channel still jumps the queue the moment the WSDLs land**
   (`docs/mef/wsdl/` still absent; .p12 DONE; ASID 61135801 ENROLLED).
3. **The s71 ratified queue is now COMPLETE** (bootstrap_demo 1065+1041 →
   S-21b → S-21c all shipped). The next NEW autonomous item is **the
   repo-wide RENUMBER-STALE ROLLUP KEY sweep** (the s98 open chip:
   every OTHER_DED_LINE_KEY / SUBSCHEDULE_CONFIG / MappingRule target
   must resolve against its form — closes the s98 bug class), then
   Ken-direction is needed: the 1120/709 authoring waves and the
   1120-S ATS upload lane are Ken-gated; the tts sched_b face-renumber
   awaits the s99b call.
4. **Ken-gated follow-ups:** S-24 prod backfill on the key hand-off →
   hub-ein blanking (s97) · SEC-5 plumbing on the s95 ratification ·
   S-22b triage confirmations (6252 · 9325) · sched_b renumber (s99b).
5. **Auth lane (s94):** Ken's env vars → `supabase_users --list` +
   live `supabase_login` verify → P1 identity model.
6. **Ken ratifications pending:** s99 (Q4 4d + sched_b renumber) ·
   s97 (S-24 trio) · s96 (WISP 4 calls) · s95 (retention · PITR · HSTS
   preload) · s94 (8879 col-C @ ATS + stockpiling proxy) · s93 (4h
   idle) · s89 (8915-F rounding) · s85/s84 pairs · s83 Resend ·
   s76..s72 notes.
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
- **Flow-assertion gate GREEN at 518** (s94 level; s99 re-ran green —
  no formula changes; the 1065 FA mirror refreshed export-minus-pending
  at 39 with the GATE-SMALL-PTNR-B build-gap text closed). Mirrors:
  1120S 41 · 1065 39 (+4 s64 staged) · 1040 415.
- NEW s99: test_1065_schb_q4 7 (boundaries/loss-exclusion/8825/override)
  · 1065+SchB band 153 (L/M tie fixtures _b6_no'd — see the file
  docstrings). s98: test_1065_other_deductions 2 ·
  test_1065_k1_diagnostics_leg 11. s97: test_tax_identity 24 · blast
  band 122. s94 suites stand: test_8879_8878 33 · returns 110 ·
  MeF/extract 105 · tsc 0 · vitest 300. s89: test_8915f 49 · FULL
  efile band 966 · tts_forms 355.
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: migs through returns 0207 + audit 0004 +
  core 0004 + clients 0010 BOTH DBs (s99 shipped NO migrations);
  seed_1065 re-run BOTH DBs (s99 — the face-verbatim B6 label).**
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
