# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-19, session 97 (autonomous — Ken out of office).
**S-24 CLIENTS_TAX_IDENTITY SHIPPED** — the encrypted canonical SSN home
(SUITE_CONTRACT §3 gap CLOSED; clients migs 0009 + 0010 RLS applied BOTH
DBs). NEW `clients_tax_identity` (one row per taxpayer per role, Fernet
ciphertext + keyed-HMAC match token + last-4, firm+client FKs) · NEW
`apps/clients/identity.py` (the ONLY encrypt/decrypt module; env-keyed,
inert without keys — the s94 pattern) · identity is MASTER: return
creation populates the snapshot FROM it, taxpayer-card edits write BACK ·
`backfill_tax_identities` command (demo: 10 rows; prod dry-run: 590/0
conflicts) · **SEC-2 deferral CLOSED: the return list ships last-4 only**
(identity-annotated subquery) + audited per-record reveal endpoint
(`log_view` → `clients.TaxIdentity` VIEW rows) + per-row Show/Hide in
Return Manager (replaces the global toggle — ratification s97). Gates:
NEW test_tax_identity **24/24** · flow **518** · blast band 122 · tsc 0 ·
vitest 300 · live demo probe green (masked list → reveal → audit row →
cleaned). s96 WISP + s95 SEC-5 stand. `/bugs`: clean. WSDLs absent.*

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
1. **Start every session with `/bugs`** (s55; s97 sweep: clean).
2. **S-17g A2A channel still jumps the queue the moment the WSDLs land**
   (`docs/mef/wsdl/` still absent; .p12 DONE; ASID 61135801 ENROLLED).
3. **The next NEW autonomous item is the s71 queue: bootstrap_demo
   1065+1041 demo returns → S-21b 1065 partner-percentage diagnostic →
   S-21c Sch B Q4 auto-answer (spec-first).** The 1120/709 authoring
   waves + the 1120-S ATS lane stay Ken-gated. Ken-gated S-24 follow-ups:
   prod backfill on key hand-off → hub-ein blanking leg (REVIEW_QUEUE
   s97). SEC-5 plumbing (purge jobs + clearsessions cron) fires on the
   s95 ratification. S-22b triage: confirm 6252 · 9325.
4. **Auth lane (s94):** once Ken sets the Supabase env vars → CC runs
   `supabase_users --list` + verifies live `supabase_login` → then P1
   identity model in the identity home (also scopes `handle_new_user`).
5. **Ken ratifications pending:** **s97 (S-24: per-row reveal UX ·
   hub-ein blanking leg · key hand-off)** · s96 (WISP v0.1: ratify ·
   72h notice · pen test · log stance) · s95 (retention numbers · PITR ·
   HSTS preload) · s94 (8879 1040-X col-C [verify @ ATS] + stockpiling
   proxy) · s93 (4h idle) · s89 (8915-F ROUND_HALF_UP) · s85/s84 pairs ·
   s83 email provider (Resend) · s76..s72 notes.
6. **Design: Ledger live; cross-app application on Ken's go.**

## ▶ Waiting on Ken / external
1. **S-24 identity keys (s97):** copy TAX_IDENTITY_ENCRYPTION_KEY +
   TAX_IDENTITY_HMAC_KEY from local `server/.env` to Render
   `tts-tax-app` (SAME values — a different pair splits the store!);
   then CC runs `backfill_tax_identities` on prod (590 staged, dry-run
   verified) → then the hub-ein blanking leg on your go.
2. **Auth env vars (s94):** SUPABASE_URL + ANON_KEY (+ SERVICE_ROLE) on
   Render AND local `server/.env` → CC verifies live.
3. **A2A: ONLY the WSDL toolkit remains.**
4. **WISP v0.1 ratification (s96)** — 4 calls in REVIEW_QUEUE s96.
5. **SEC-5 [EXT] legs (s95):** SOC 2 pull · restore drill · PITR call.
6. **Email provider setup (s83)** — Resend, SPF/DKIM, Render env vars.
7. **Role assignments (s84):** `manage.py set_user_role`.
8. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
9. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
10. File-1018 Lacerte reprint. 11. PWA install check. 12. Density feel-check (s52).
13. s69: firm's real CAF number + fax on the Preparer records.
14. Demo Render service (env=demo; if it exists, it also needs the
    DEMO key pair from local `server/.env.demo`).
15. S-22b triage confirmations (6252 · 9325 — add or skip).
16. **ERO EFIN/PIN source (s94):** wire the 8879/8878 cards from the
    Preparer record (future nicety).
17. **Cross-app flag (s95):** delvio-1099 — `filing_dashboard` →
    security_invoker on its next touch.
18. **Beta-agreement security clauses (s96):** with counsel.

## Active gates
- **Flow-assertion gate GREEN at 518** (s94 level; s95-s97 touched no
  compute formulas; s97 re-ran the gate green after the create-return
  hook). Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) · 1040 415.
- NEW **test_tax_identity 24/24** · blast band (returns+authz+audit+
  session) 122 · tsc 0 · vitest 300. s94 suites stand: test_8879_8878
  33 · returns 110 · core 1040 MeF/extract 105. s89: test_8915f 49 ·
  FULL efile band 966 · tts_forms band 355 (trip-wire 95).
- **Supabase advisors: ZERO rls_disabled BOTH projects** (s95 level;
  s97 added clients_tax_identity WITH its RLS pair — advisor-clean by
  construction; next sweep per WISP §5.1 cadence).
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: returns migs through 0207 + audit 0004 +
  core 0004 + clients 0010 applied BOTH DBs**; seed_rules current.
- ⚠ **Render prod has NO identity keys yet** — identity features inert
  there by design until Ken's hand-off (Waiting §1). Local dev + demo
  fully keyed (`server/.env` / `.env.demo`, s97).
- ⚠ HSTS lands on the next tts Render deploy (s95 prod.py change).
- ⚠ Local test-DB after new migrations: the s86 setup_databases(keepdb)
  recipe (run for clients 0009/0010), then `--reuse-db`.
- ⚠ Restart the running django-demo server after adding `@action`
  endpoints AND after server-side edits (--noreload; the s94/s97 catch);
  after restarting, force a FULL reload in the SPA tab — vite keep-alive
  + hash-navs can leave stale payloads painted (s97).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- Design rollback points: tag `pre-ledger-design` · old presets in the picker.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
