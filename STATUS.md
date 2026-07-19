# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-19, session 95 (autonomous — Ken out of office).
**SEC-5 ENCRYPTION/BACKUPS VERIFY+DOCUMENT SHIPPED** (`f10ca9d`; ledger
`docs/audits/2026-07-19_sec5_encryption_backups.md` = the WISP input +
the DRAFT retention/deletion policy, Ken ratification → REVIEW_QUEUE
s95). TLS verified live both paths (TLSv1.3 pooler + prep.delviotax.com);
Pro-plan backup posture recorded (daily/7-day; **PITR not enabled —
recommend Ken enable on prod**). **TWO REAL FINDINGS FIXED via the
Supabase security advisors (Principle #0):** (1) prod — all 12
`scheduler_*` tables had NO RLS with a live anon SELECT grant (client
contact PII columns; 0 rows, caught pre-launch) → fixed in
sherpa-scheduler mig 0010 (`0e50ee7`), applied to prod; (2) the
FRESH-BUILD RLS NO-OP CLASS — `core.0001` is a hardcoded IF-EXISTS list
with deps=[] that silently no-ops on any fresh DB (the demo surfaced 30
RLS-less tables incl. returns_taxpayer/django_session) → NEW
`core.0004_rls_dynamic_sweep` (enables RLS on whatever lacks it at run
time); advisors now ZERO rls_disabled on BOTH projects. Plus HSTS added
to prod.py (the one Render-TLS gap; lands next deploy). s94 (8879/8878
pair + suite-auth day) stands. WO-28..33 lane EMPTY. `/bugs`: clean.*

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
1. **Start every session with `/bugs`** (s55; s95 sweep: clean).
2. **S-17g A2A channel still jumps the queue the moment the WSDLs land**
   (`docs/mef/wsdl/` still absent; .p12 DONE; checklist
   `docs/mef/A2A_ENROLLMENT.md`; ASID 61135801 ENROLLED + ACTIVE). The
   s94 8879 extract gate + SID capture are the S-17g transmit hooks.
3. **The next NEW autonomous item is SEC-6 — the Delvio WISP draft**
   (cites the sec1..sec5 ledgers in `docs/audits/`; includes the s95
   advisor-recheck cadence + infra-log-retention decisions) → **S-24
   `clients_tax_identity`** (reads reuse the s92 `log_view`). Then the
   SEC-5 plumbing leg once Ken ratifies the retention numbers (purge
   jobs + a `clearsessions` cron — flagged unscheduled in the ledger).
   Still open from the S-22b triage: confirm 6252 · 9325.
4. Then the s71 queue: **bootstrap_demo 1065+1041 demo returns** → **S-21b
   1065 partner-percentage diagnostic** → **S-21c Sch B Q4 auto-answer**
   (spec-first). The 1120/709 authoring waves + the 1120-S ATS lane stay Ken-gated.
5. **Auth lane (s94):** once Ken sets the env vars (Waiting §1) → CC runs
   `supabase_users --list` + verifies live `supabase_login` → then **P1
   identity model in the identity home**. P1 also scopes the
   `handle_new_user` SECURITY DEFINER grants (s95 flag).
6. **Ken ratifications pending:** **s95 (SEC-5: retention numbers ·
   enable-PITR-on-prod recommendation · HSTS preload at go-live)** ·
   s94 (8879 1040-X col-C arm [verify @ ATS] + stockpiling proxy) ·
   s93 (4h idle window) · s89 (8915-F ROUND_HALF_UP) · s85 pair ·
   s84 pair · s83 email provider (Resend) · s76/s75/s74/s73/s72 notes.
7. **Design: Ledger live + Ken-ratified; cross-app application on Ken's
   go (`Design/LEDGER_DESIGN_SYSTEM.md`).**

## ▶ Waiting on Ken / external
1. **Auth env vars (s94):** SUPABASE_URL + ANON_KEY (+ SERVICE_ROLE) on
   Render `tts-tax-app` AND local `server/.env` → CC verifies live.
2. **A2A: ONLY the WSDL toolkit remains.**
3. **SEC-5 [EXT] legs (s95, now due):** pull the Supabase SOC 2
   attestation (dashboard → Legal/Compliance) · schedule the TESTED
   restore drill (suggested shape in the ledger) · the **enable-PITR-
   on-prod** decision (paid add-on; recommended).
4. **Email provider setup (s83)** — Resend, SPF/DKIM, Render env vars,
   `set_user_email` per preparer.
5. **Role assignments (s84):** `manage.py set_user_role`.
6. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
7. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
8. File-1018 Lacerte reprint. 9. PWA install check. 10. Density feel-check (s52).
11. s69: firm's real CAF number + fax on the Preparer records.
12. Demo Render service (env=demo; GEMINI_API_KEY blank there).
13. S-22b triage confirmations (6252 · 9325 — add or skip).
14. **ERO EFIN/PIN source (s94):** wire the 8879/8878 cards from the
    Preparer record (future nicety).
15. **Cross-app flag (s95):** delvio-1099 lane — recreate
    `filing_dashboard` as security_invoker on its next touch (not
    anon-exposed today; low severity).

## Active gates
- **Flow-assertion gate GREEN at 518** (s94 level; s95 touched no
  compute). Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) · 1040 415.
- **Supabase security advisors: ZERO `rls_disabled_in_public` on BOTH
  projects** (was 12 prod + 30 demo before s95). Default-deny posture
  now self-healing on fresh builds via `core.0004_rls_dynamic_sweep`.
- s94 suites stand: test_8879_8878 33 · returns 110 · core 1040
  MeF/extract 105 · tsc 0 · vitest 300. s93: test_session_hardening 6 ·
  blast band 152. s89: test_8915f 49 · FULL efile band 966 · tts_forms
  band 355 (trip-wire 95).
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: returns migs through 0207 + audit 0004 +
  core 0004 applied BOTH DBs**; scheduler mig 0010 applied prod (its
  Render deploy will see it recorded); seed_rules current BOTH DBs.
- ⚠ HSTS lands on the next tts Render deploy (prod.py change).
- ⚠ Local test-DB note: after a new migration run the s86 recipe once —
  standalone `setup_databases(keepdb=True)` under `config.settings.test`
  (done for core 0004), then pytest `--reuse-db` works.
- ⚠ Restart the running django-demo server after adding `@action`
  endpoints (DRF registers routes at startup — the s94 catch).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- Design rollback points: tag `pre-ledger-design` · old presets in the picker.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
