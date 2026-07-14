# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-14, session 84. **SEC-1 AUTHZ AUDIT SHIPPED** — every
URL-exposed view audited for firm scoping (ledger:
`docs/audits/2026-07-14_sec1_authz_audit.md`); THREE write-side gaps found and
fixed (K-1 import owner_id was unscoped — a foreign UUID could pull another
firm's K-1 data; `/info/` PATCH preparer FKs unvalidated; documents raw
create/PATCH could re-point ownership); FIRST role enforcement live —
Preparers + Print Packages writes are ADMIN-only (`IsFirmAdminOrReadOnly`),
reads stay member-wide; NEW `manage.py set_user_role` (last-admin guard).
Verified holders: prod Ken×2 admin / Whit preparer; demo `demo` admin — nobody
locked out. Pins: NEW `tests/test_authz_sec1.py` 17/17 · neighbors 141 green ·
live demo probe 6/6 (403/405/201 matrix). Zero compute/render/client code.*

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
1. **Start every session with `/bugs`** (s55; s84 sweep: clean).
2. **S-17g A2A channel still jumps the queue the moment the WSDLs land**
   (`docs/mef/wsdl/` still absent at s84; .p12 DONE; checklist
   `docs/mef/A2A_ENROLLMENT.md`; ASID 61135801 ENROLLED + ACTIVE).
3. **SEC-1 is DONE (s84).** Next per Ken's s83 sequencing: **the six
   dispatched tts legs** (Gate-1 cleared s83, mirrors cached):
   9465 print+IRS9465 · 8888 print+IRS8888+35a · 1040-V/ES vouchers
   (print-only, suppression ties) · 4868 print + NEW extension submission
   builder + Sch3-L10 tie · 8915-F full unit (inputs + per-disaster compute +
   render + IRS8915F + the 5329 suppression seam). **Then 8879/8878 →
   SEC-2..6 → S-24.** Still open from the S-22b triage: confirm 6252 · 9325.
4. Then the s71 queue: **bootstrap_demo 1065+1041 demo returns** → **S-21b
   1065 partner-percentage diagnostic** → **S-21c Sch B Q4 auto-answer**
   (spec-first). The 1120/709 authoring waves + the 1120-S ATS lane stay Ken-gated.
5. **Ken ratifications pending:** NEW s84 pair (front-desk role timing ·
   ADMIN-only destructive deletes — recommendations filed) · s83 email
   provider pick (Resend recommended; `docs/AUTH_EMAIL_SETUP.md`) · the s76
   EFW late-filing arm · s75 (2210 · 8962 QSEHRA · 8889 multi-account) · s74
   (7203 line-1 fold · loss-without-7203 refusal · partnership 28(e)) · s73
   (23c/23d · Sch E line 27 · print truncation) · s72 (8867 face-fidelity ·
   Sch B K-1 listing row). Standing non-decisions: 3115 OMB nit · 8824 RS note.
6. **Design (s81–s82): Ledger live + Ken-ratified.** Optional follow-ups
   unscheduled (refund banner · PWA PNG regen · dark variant). **STANDING:
   apply Ledger across the other Sherpa apps when Ken directs —
   `Design/LEDGER_DESIGN_SYSTEM.md`.**

## ▶ Waiting on Ken / external
1. **A2A: ONLY the WSDL toolkit remains** (RESUME item 2).
2. **Email provider setup (s83)** — Resend (or equal) account, SPF/DKIM in
   Cloudflare, Render env vars, then `set_user_email` per preparer.
   Until then magic-link prints to server logs; password login unaffected.
3. **s84 role assignments are Ken's lever now:** `manage.py set_user_role
   <username> <role>` (admin/preparer/reviewer) — e.g. promote Whit if he
   should edit Preparers/Print Packages.
4. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
5. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
6. File-1018 Lacerte reprint (item 10). 7. PWA install check.
8. Density feel-check (s52). 9. s69: firm's real CAF number + fax on the
   Preparer records (Admin → Preparers) for the 2848 L2 autofill.
10. Demo Render service LIVE (env=demo). Nit: GEMINI_API_KEY blank there.
11. S-22b triage confirmations (6252 · 9325 — add or skip).
12. s81 design check: eyeball Ledger in your own browser (instant rollback
    in the picker).

## Active gates
- **Flow-assertion gate GREEN at 500 — untouched by s84** (zero compute code).
  Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) · 1040 397 export-verbatim (+1 staged).
- **s84 suites: NEW test_authz_sec1 17/17 · blast radius green: test_firms +
  k1-import legs + bulk-assign + preparer-of-record (49) · test_documents +
  test_returns + test_1040x_route (86) · test_k1_import_stage3 6/6 (gained
  its missing module seed fixture — was order-dependent standalone).**
- s83 suites stand: test_auth_magic_link 19 · user-prefs 14 · tsc 0 ·
  vitest 300 (client untouched s84). s82: test_returns 76 → 86 with s84 pins.
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: no new migrations in s84** (top remains accounts
  0004/0005 + audit 0003 on BOTH DBs, s83; returns 0196 s80).
- ⚠ Local test-DB note (s74): a stale `test_postgres` blocks DB-backed runs
  with FormDefinition.DoesNotExist — drop it if hit.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- Design rollback points: tag `pre-ledger-design` · old presets in the picker.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
