# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-14, session 83. **AUTH-1 MAGIC-LINK LOGIN UNIT SHIPPED** —
emailed single-use sign-in links (hashed token, 15-min TTL, single-use w/ full burn),
enumeration-safe + throttled request/redeem endpoints, LOGIN audit rows for every
sign-in and link request, email-as-username on password login (password login KEPT,
unthrottled by design — shared office NAT). Provider-agnostic SMTP email infra via env
vars (console backend until `EMAIL_HOST` set — links print to server logs). Migrations
accounts 0004/0005 (+RLS) + audit 0003 applied to BOTH prod AND demo DBs. Live-probed
end-to-end on demo (request → link from log → redeem → signed in → replay rejected;
ORM-verified token burn + audit rows). **Ken-external remainder: provider account +
Cloudflare SPF/DKIM + Render env vars + `set_user_email` per preparer —
`docs/AUTH_EMAIL_SETUP.md` (recommendation: Resend; REVIEW_QUEUE s83).**
`/bugs` s83: clean. WSDLs still absent. Ken still holds FIVE Gate-1 walks (WO-28..32).*

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
1. **Start every session with `/bugs`** (s55; s83 sweep: clean).
2. **NEXT UNIT (Ken-directed): S-17g A2A channel — the .p12 is DONE.** The ONLY
   remaining prereq is the gated MeF A2A WSDL toolkit → `docs/mef/wsdl/` (still
   absent at s83; checklist in `docs/mef/A2A_ENROLLMENT.md`; ASID 61135801
   ENROLLED + ACTIVE). The moment the WSDLs land, CC builds `A2ATransmitter`
   (SOAP + WS-Security X.509, Login/SendSubmissions/GetNewAcks against the REAL
   WSDLs) behind the Transmitter ABC → A2A comms test in ATS under ETIN 14192.
3. **S-22b:** Wave-1 built row ✅ Sch E + 8582 (s73) · ✅ 8949/Sch D (s73b) ·
   ✅ 7203 (s74) · ✅ compute-done XML row (s75) · ✅ EFW payment records (s76) ·
   ✅ W-2G MeF document (s80). RS-gated at Gate-1: the payment-cluster batch
   (s77) + 4868 (s78) + 8915-F (s79). On Ken's approve-all (FIVE walks): flip
   sentinels → seed → verify exports → cache tts mirrors → **dispatch the six
   tts legs as a set** (9465 · 8888 · voucher pair · 4868 print + extension
   submission builder · 8915-F full unit). **AUTH-1 is DONE (s83).** Next NEW
   autonomous item while the gates hold (re-cut s83 act three): **SEC-1 authz
   audit** (Principle #0 — see below) → **the 8879/8878 print pair** →
   SEC-2..6. Still open from the list triage: confirm 6252 · 9325 additions.
3b. **NEW (s83): Principle #0 — "PII protection outranks the Prime Directive"
   (Ken, verbatim, DECISIONS.md top) + Spine S-23 pre-beta security block**
   (SEC-1 authz/RBAC audit · SEC-2 PII plumbing sweep · SEC-3 view-access
   audit logging · SEC-4 session hardening · SEC-5 encryption/backups
   verify+document+restore drill · SEC-6 Delvio WISP; MFA already decided =
   passkeys at go-live). Also s83 act two: **clients_client RATIFIED as the
   canonical suite clients table** (plain-UUID refs for non-tax-app apps;
   client_number deferred; `3d208ad`) — sherpa-scheduler Phase 2 unblocked.
   **Acts four/five: demo login screen no longer prints credentials** (Ken:
   premature for third parties; note kept, live-verified) **+ the TWO SSN
   rulings ratified** — full SSN never crosses the tax-app boundary
   (email/phone/name matching permanent for other apps; `ssn_last4` the only
   derivative), and canonical taxpayer identity = NEW Spine **S-24
   `clients_tax_identity`** (encrypted tax-app-owned table, never the shared
   hub; plain-hash match tokens pre-refused — keyed HMAC only).
4. Then the s71 queue stands: **bootstrap_demo 1065+1041 demo returns** →
   **S-21b 1065 partner-percentage diagnostic** → **S-21c Sch B Q4 auto-answer**
   (spec-first). The 1120/709 authoring waves + the 1120-S ATS lane stay Ken-gated.
5. **Ken ratifications pending: the FIVE Gate-1 walks (s77 ×3 + s78 4868 + s79
   8915-F)** — REVIEW_QUEUE; recommendations = approve all. **NEW s83: email
   provider pick (Resend recommended) + the Ken-external email setup steps —
   `docs/AUTH_EMAIL_SETUP.md`.** Plus the s76 pair, s75 (2210 · 8962 QSEHRA ·
   8889 multi-account), s74 (7203 line-1 fold · loss-without-7203 refusal ·
   partnership 28(e)), s73 (23c/23d · Sch E line 27 · print truncation), s72
   (8867 face-fidelity · Sch B K-1 listing row). Standing non-decisions: 3115
   OMB nit · the 8824 RS note.
6. **Design (s81–s82): Ledger is live and Ken-ratified.** Remaining optional
   follow-ups: mockup-style refund BANNER in the editor · PWA PNG icons still
   carry old branding (raster regen) · dark-Ledger variant. None scheduled.
   **STANDING: apply Ledger across the other Sherpa apps later (Ken directs
   when) — start from `Design/LEDGER_DESIGN_SYSTEM.md`.**

## ▶ Waiting on Ken / external
1. **A2A: ONLY the WSDL toolkit remains** (the .p12 is DONE; RESUME item 2).
2. **The FIVE Gate-1 walks** — WO-28 9465 · WO-29 8888 · WO-30 1040-V/ES ·
   WO-31 4868 · WO-32 8915-F.
3. **NEW (s83): email provider setup** — Resend (or equal) account, verify
   delviotax.com (SPF/DKIM in Cloudflare), Render env vars, then
   `manage.py set_user_email <username> <email>` per preparer account.
   Checklist: `docs/AUTH_EMAIL_SETUP.md`. Until then, magic-link links print
   to server logs only; password login unaffected.
4. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
5. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
6. File-1018 Lacerte reprint (item 10). 7. PWA install check.
8. Density feel-check (s52). 9. s69: firm's real CAF number + fax on the
   Preparer records (Admin → Preparers) for the 2848 L2 autofill.
10. Demo Render service LIVE (https://tts-tax-demo.onrender.com, env=demo).
    Nit: GEMINI_API_KEY blank on the demo service. s83 note: the demo user
    gained email demo@delviotax.com during the probe (`bootstrap_demo --reset`
    clears it — harmless).
11. S-22b triage confirmations (6252 · 9325 — add or skip).
12. **s81 design check**: Ken should eyeball the Ledger look in his own
    browser — picker has instant rollback. Deployed on next Render deploy.

## Active gates
- **Flow-assertion gate GREEN at 500 — untouched by s83** (auth unit: zero
  compute/render code). Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) · 1040 397
  export-verbatim (+1 staged).
- **s83 suites: test_auth_magic_link 19/19 (new) · test_user_preferences 14/14 ·
  tsc 0 · vitest 300/300 · live demo browser probe green (request → redeem →
  session → single-use replay rejected; ORM token-burn + audit rows verified).**
- s82 suites stand: test_returns 76/76 · s80: MeF 1040 pure 81 · extract 13 ·
  bands 205+161.
- s79 RS-lane gates stand: validate_8915f 87/0 · s78 97/0 · s77 85/0·53/0·63/0.
  All five loaders gated; `seed_all` soft-fails them; RS prod untouched.
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: accounts 0004/0005 + audit 0003 applied to BOTH
  prod AND demo DBs (s83; on top of mig 0196 s80).**
- ⚠ Local test-DB note (s74): a stale `test_postgres` blocks DB-backed runs
  with FormDefinition.DoesNotExist — drop it if hit.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- **Design rollback points: git tag `pre-ledger-design` (hard) · theme picker
  presets Default/Editorial/Charcoal (instant, per-user).**

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
