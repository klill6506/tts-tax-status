# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-15, session 94 (Cowork). **SUITE AUTH DAY** — zero
compute code, no migrations, flow gate 515 stands. (1) **Repo renamed:**
GitHub = `delviotax/delvio-tax` (local folder unchanged; remote updated).
(2) **Five suite-auth rulings RECORDED + sequencing RATIFIED**
(DECISIONS.md "Suite auth architecture"; SUITE_CONTRACT §6; plan
`docs/auth/implementation_sequencing.md`): identity home = shared prod
Supabase project (GoTrue live there via delvio-1099); domain migration
gates ONLY the Phase 4 cookie cutover (Ken narrowed same-day).
(3) **`prep.delviotax.com` LIVE** (Phase 0 slice done; CUSTOM_DOMAIN set).
(4) **DB-password incident resolved** — a Supabase DB-password reset broke
every direct-PG app (prod 500s on login + local dev); fixed in Render +
`server/.env`; portal/checkin also updated (memory: signature = site
serves but all DB ops 500). (5) **P2a/P3a SHIPPED: Supabase-first login**
— `/auth/login/` verifies GoTrue first when `SUPABASE_URL`+
`SUPABASE_ANON_KEY` set (map-by-email → Django session; Django pw =
fallback; outage falls through; audit `supabase_login`) + NEW
`manage.py supabase_users --list|--invite|--recovery` (service key;
invite/recovery links only — no admin-set passwords, ruling 5).
`apps/accounts/supabase_auth.py` stdlib-only (no new deps, lock
untouched). Gates: NEW test_supabase_login 8/8 + magic-link band 19/19.
**⏳ KEN NEXT: set SUPABASE_URL/ANON_KEY (+SERVICE_ROLE) on Render
tts-tax-app + local .env, then `supabase_users --list`** — GoTrue email
must equal the Django email (ken@thetaxshelter.com) to map. s93 SEC-4 ·
s92 SEC-3 · s91 SEC-2 stand. WO-33 still ⏳ AWAITING KEN.*

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
1. **Start every session with `/bugs`** (s55; s93 sweep: clean).
2. **S-17g A2A channel still jumps the queue the moment the WSDLs land**
   (`docs/mef/wsdl/` still absent at s93; .p12 DONE; checklist
   `docs/mef/A2A_ENROLLMENT.md`; ASID 61135801 ENROLLED + ACTIVE).
3. **⟨GATE-1⟩ WO-33 (8879/8878 pair) ⏳ AWAITING KEN** — the walk is in RS
   WORK_ORDERS + REVIEW_QUEUE s90 (recommend approve-all; four seams w/
   recommendations). On approval: flip the sentinel → seed → verify
   `lookup/{8879,8878}/export/` → cache the tts mirrors → **dispatch the
   tts print-pair leg** (signature-input surface + two AcroForm prints +
   header-tie diagnostics + extract gating — the s87 print-only recipe).
4. While WO-33 is gated, the next NEW autonomous items: **SEC-5
   encryption/backups verify+document** (CC legs: record Supabase
   at-rest/TLS + Render TLS posture, backup posture per plan, draft the
   retention/deletion policy; [EXT] Ken legs: pull the Supabase SOC 2
   attestation, schedule the TESTED restore drill) → **SEC-6 Delvio
   WISP draft** (cites the sec1..sec4 ledgers) → **S-24
   `clients_tax_identity`** (reads reuse the s92 `log_view`). Still
   open from the S-22b triage: confirm 6252 · 9325 (9325 also pairs
   with the 8879 SID association).
5. Then the s71 queue: **bootstrap_demo 1065+1041 demo returns** → **S-21b
   1065 partner-percentage diagnostic** → **S-21c Sch B Q4 auto-answer**
   (spec-first). The 1120/709 authoring waves + the 1120-S ATS lane stay Ken-gated.
6. **Ken ratifications pending:** s93 (4h session idle window —
   recommend keep) · s90 (⟨GATE-1⟩ WO-33 above) · s89 (the
   8915-F ÷3.0 ROUND_HALF_UP convention — recommend keep) · s85 pair (RS
   9465 condition-wording nit · the F9465-027-01 effective-payment seam —
   re-check at ATS) · s84 pair (front-desk role · ADMIN-only destructive
   deletes) · s83 email provider pick (Resend recommended;
   `docs/AUTH_EMAIL_SETUP.md`) · the s76 EFW late-filing arm · s75 (2210 ·
   8962 QSEHRA · 8889 multi-account) · s74 (7203 line-1 fold ·
   loss-without-7203 refusal · partnership 28(e)) · s73 (23c/23d · Sch E
   line 27 · print truncation) · s72 (8867 face-fidelity · Sch B K-1
   listing row). Standing non-decisions: 3115 OMB nit · 8824 RS note.
7. **Design (s81–s82): Ledger live + Ken-ratified.** Optional follow-ups
   unscheduled. **STANDING: apply Ledger across the other Sherpa apps when
   Ken directs — `Design/LEDGER_DESIGN_SYSTEM.md`.**

## ▶ Waiting on Ken / external
1. **⟨GATE-1⟩ WO-33 — the 8879/8878 walk** (RESUME item 3).
2. **A2A: ONLY the WSDL toolkit remains** (RESUME item 2).
3. **Email provider setup (s83)** — Resend (or equal), SPF/DKIM, Render env
   vars, `set_user_email` per preparer (`docs/AUTH_EMAIL_SETUP.md`).
4. **Role assignments (s84):** `manage.py set_user_role <username> <role>`
   — e.g. promote Whit if he should edit Preparers/Print Packages.
5. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
6. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
7. File-1018 Lacerte reprint. 8. PWA install check. 9. Density feel-check (s52).
10. s69: firm's real CAF number + fax on the Preparer records.
11. Demo Render service (env=demo; GEMINI_API_KEY blank there).
12. S-22b triage confirmations (6252 · 9325 — add or skip).
13. s81 design check: eyeball Ledger (instant rollback in the picker).
14. **SEC-5 external legs (heads-up, not yet due):** Supabase SOC 2
    attestation pull + backup/restore drill scheduling — CC will flag
    when SEC-5 starts.

## Active gates
- **Flow-assertion gate GREEN at 515 (unchanged — zero compute/render
  code this session).** Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) ·
  1040 412 (+1 staged). The three WO-33 FAs are staged DRAFT in RS —
  they join the gate only with the tts build leg.
- **RS state: WO-33 ⏳ AWAITING KEN (`49c82b1`)** — `load_8879_8878.py`
  gated (READY_TO_SEED=False), harness `validate_8879_8878.py` 77/0,
  brief + walk filed. The WO-28..32 lane remains EMPTY (all ✅ DONE).
- **s93 suites:** NEW test_session_hardening 6 · blast band 152
  (auth+audit+authz+returns+ai_help over the new middleware). s92:
  test_audit 22 · return-open VIEW pin in test_returns. s89
  suites stand: test_8915f 49 · flow 515 · seam band 150 · FULL efile
  band 966 · tts_forms band 355 (trip-wire 93) · tsc 0 · vitest 300.
  s88: test_4868 44 · s87: test_1040v_es 41 · s86: test_8888 37 ·
  s84: test_authz_sec1 17 · s83: auth 19 · prefs 14.
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: returns migs through 0205 + audit 0004
  applied BOTH DBs** (audit 0004 s92 — choices-only AlterField ·
  0204/0205 s89 · 0202/0203 s88 · 0200/0201 s87 · 0198/0199 s86 ·
  0197 s85); seed_rules current BOTH DBs.
- ⚠ Local test-DB note: after a new migration run the s86 recipe once —
  standalone `django.test.utils.setup_databases(keepdb=True)` **under
  `config.settings.test`** (it migrates the LOCAL test_postgres
  forward), then pytest `--reuse-db` works.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- Design rollback points: tag `pre-ledger-design` · old presets in the picker.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
