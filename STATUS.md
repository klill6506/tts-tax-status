# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-15, session 94 (WO-33 build leg). **FORM 8879 + 8878
e-file SIGNATURE-AUTHORIZATION PAIR SHIPPED** — Ken approved WO-33 in-session
("approve WO-33", the four walk seams adopted as recommended). NEITHER FORM
TRANSMITS (ERO-retained print artifacts; the Return Header PIN block the app
already e-files IS the electronic signature — NO MeF document). RS seeded
(`ea28aff`) + FAs activated (`0d724cb`); tts leg commits `0346354` (server)
/ `827cb9f` (FAs+runner) / `6474d87` (client cards). migs 0206/0207 (+RLS)
BOTH DBs. **flow gate 515 → 518.** The four seams: (a) Part I L3 = 1040 line
25d; (b) 1040-X arm = amended col-C [verify @ ATS]; (c) extract_return
REFUSES an unsigned/re-sign-required 8879; (d) signed-at Part I snapshot
drives the $50/$14 re-sign tolerance. Live-verified on the demo Jones 1040
(both cards render; the 8879 banner painted from the live return + the MFJ
transmit gate). **The suite-auth day items (below) still await Ken's env
vars.** WO-28..33 dispatch lane now EMPTY (all ✅ DONE).*

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
   (`docs/mef/wsdl/` still absent; .p12 DONE; checklist
   `docs/mef/A2A_ENROLLMENT.md`; ASID 61135801 ENROLLED + ACTIVE). NOTE:
   the s94 8879 extract gate + SID capture are the S-17g transmit hooks —
   the D_8879_UNSIGNED refusal placement is finalized there.
3. **The next NEW autonomous item is SEC-5 encryption/backups verify+
   document** (CC legs: record Supabase at-rest/TLS + Render TLS posture,
   backup posture per plan, draft the retention/deletion policy; [EXT] Ken
   legs: pull the Supabase SOC 2 attestation, schedule the TESTED restore
   drill) → **SEC-6 Delvio WISP draft** (cites the sec1..sec4 ledgers) →
   **S-24 `clients_tax_identity`** (reads reuse the s92 `log_view`). Still
   open from the S-22b triage: confirm 6252 · 9325 (9325 pairs with the
   8879 SID association — the D_8879_SID reminder already points at it).
4. Then the s71 queue: **bootstrap_demo 1065+1041 demo returns** → **S-21b
   1065 partner-percentage diagnostic** → **S-21c Sch B Q4 auto-answer**
   (spec-first). The 1120/709 authoring waves + the 1120-S ATS lane stay Ken-gated.
5. **Auth lane (from the s94 suite-auth day):** once Ken sets the env vars
   (Waiting §1) → CC runs `supabase_users --list` + verifies live
   `supabase_login` end-to-end → then **P1 identity model in the identity
   home** (the shared prod Supabase project) is the next auth unit.
6. **Ken ratifications pending:** s94 (the 8879 1040-X col-C arm + the
   stockpiling-clock proxy → DEFERRAL_AUDIT s94; recommend keep as built) ·
   s93 (4h session idle window — recommend keep) · s89 (8915-F ÷3.0
   ROUND_HALF_UP — recommend keep) · s85 pair (RS 9465 wording nit ·
   F9465-027-01 effective-payment seam — re-check at ATS) · s84 pair
   (front-desk role · ADMIN-only destructive deletes) · s83 email provider
   (Resend recommended; `docs/AUTH_EMAIL_SETUP.md`) · s76 EFW late-filing
   arm · s75/s74/s73/s72 notes. Standing non-decisions: 3115 OMB nit · 8824 RS note.
7. **Design (s81–s82): Ledger live + Ken-ratified.** Optional follow-ups
   unscheduled. **STANDING: apply Ledger across the other Sherpa apps when
   Ken directs — `Design/LEDGER_DESIGN_SYSTEM.md`.**

## ▶ Waiting on Ken / external
1. **Auth env vars (s94 suite-auth day):** set `SUPABASE_URL` + `SUPABASE_ANON_KEY`
   (+ `SUPABASE_SERVICE_ROLE_KEY`) on Render `tts-tax-app` AND in local
   `server/.env`, then CC runs `supabase_users --list` + verifies live
   `supabase_login` (GoTrue email must equal the Django email
   ken@thetaxshelter.com to map).
2. **A2A: ONLY the WSDL toolkit remains** (RESUME item 2).
3. **Email provider setup (s83)** — Resend (or equal), SPF/DKIM, Render env
   vars, `set_user_email` per preparer (`docs/AUTH_EMAIL_SETUP.md`).
4. **Role assignments (s84):** `manage.py set_user_role <username> <role>`.
5. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
6. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
7. File-1018 Lacerte reprint. 8. PWA install check. 9. Density feel-check (s52).
10. s69: firm's real CAF number + fax on the Preparer records.
11. Demo Render service (env=demo; GEMINI_API_KEY blank there).
12. S-22b triage confirmations (6252 · 9325 — add or skip).
13. s81 design check: eyeball Ledger (instant rollback in the picker).
14. **SEC-5 external legs (now on deck):** Supabase SOC 2 attestation pull +
    backup/restore drill scheduling — CC flags when SEC-5 starts.
15. **ERO EFIN/PIN source (s94):** the 8879/8878 cards take the ERO EFIN +
    self-selected PIN as inputs; wiring them from the Preparer record is a
    future nicety (DEFERRAL_AUDIT s94).

## Active gates
- **Flow-assertion gate GREEN at 518** (was 515; +3 = FA-8879-NEED/RESIGN +
  FA-8878-EFW, activated with the s94 build leg). Mirrors: 1120S 41 · 1065
  39 (+4 s64 staged) · 1040 **415** (export 416 minus the staged
  FA-1040-4835-06). RS FAs now ACTIVE (`0d724cb`).
- **RS state: WO-33 ✅ SEEDED + ACTIVE** (`ea28aff`/`0d724cb`) — 8879 +
  8878 TaxForms live, exports 200, seed_all reconstructable. The WO-28..33
  dispatch lane is EMPTY (all ✅ DONE). No queued next RS order.
- **NEW test_8879_8878 33/33** (RS scenarios 8879-A..H + 8878-A..F pinned;
  endpoint + diagnostics + field-map/PDF agreement + render gates + extract
  refusal). test_4868 + test_1040v_es green · returns 110 · core 1040
  MeF/extract band 105 · tsc 0 · vitest 300.
- s93 suites stand: test_session_hardening 6 · blast band 152. s92:
  test_audit 22. s89: test_8915f 49 · seam band 150 · FULL efile band 966 ·
  tts_forms band 355 (trip-wire NOW 95 — f8879+f8878 added).
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: returns migs through 0207 + audit 0004 applied
  BOTH DBs** (0206/0207 s94 model+RLS pair · 0204/0205 s89 · 0202/0203 s88 ·
  0200/0201 s87 · 0198/0199 s86 · 0197 s85); seed_rules current BOTH DBs.
- ⚠ Local test-DB note: after a new migration run the s86 recipe once —
  standalone `setup_databases(keepdb=True)` under `config.settings.test`,
  then pytest `--reuse-db` works.
- ⚠ The running django-demo server must be RESTARTED after adding
  `@action` endpoints (DRF registers routes at startup — a stale process
  404s the new routes; the s94 live-probe catch).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- Design rollback points: tag `pre-ledger-design` · old presets in the picker.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
