# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-15, session 87. **THE 1040-V / 1040-ES VOUCHER PAIR
SHIPPED** — the third of the six Gate-1-dispatched tts legs (RS spec WO-30,
ONE loader/TWO TaxForms). PRINT-ONLY BY DESIGN — the s76 EFW/ES-debit
records are the electronic siblings and SUPPRESSION is the interlock: an
EFW election kills the 1040-V, a debited quarter kills its paper ES
voucher, and both suppressions ARE the render gates (a suppressed voucher
cannot reach paper — the bridge-gate convention applied to print). NEW
PaymentVouchers singleton (mig 0200 + RLS mig 0201, BOTH DBs — the
model+RLS pair rule) + compute_vouchers pinned to all 10 RS scenario
oracles (R-V-USE / the $100M split / R-ES-RAP with the 90-100-110-66⅔
arms, the $150k-exactly boundary, farmers-never-110% / the $1,000 gate +
no-liability exception / per-quarter emission at the FPYMT-088-11 calendar
/ joint bars / BOTH mailing charts as partition-pinned rosters — the GA
three-way trap: V→1214, ES→1300, foreign→1303) + 15 D_V_*/D_ES_*
diagnostics (seeded BOTH DBs) + f1040v (2025) + f1040es (2026 package)
AcroForm print legs (manifest 89→91; ES prints ONLY the sheets carrying an
emitted quarter; V4 rides PDF p13, V3/2/1 ride p14) + the packet "voucher"
tier (sorts LAST, after state returns) + payment-vouchers endpoint
(row-locked PATCH from birth) + the Payments-tab card (derived-default
V amount ← line 37, withholding ← 25d, prior tax ← the return's own §6654
tax shown via the compute_2210 chain, AGI ← line 11).
**FA-1040V-EFW/FA-ES-RAP/FA-ES-QDEBIT activated in RS, 1040 mirror
refreshed export-verbatim (406 = 407 export − the s71-staged 4835-06),
runners in both chains — flow gate 506 → 509.** Suites: NEW test_1040v_es
41 · flow 509 · tts_forms+acroform+returns 277 (manifest trip-wire
re-pinned 91) · tsc 0 · vitest 300 · live demo probe (KY return: V→931000
/ ES→931100 live on the card; 6-PATCH concurrent volley all landed; both
render endpoints 200 %PDF; page map ends …1040-V, 1040-ES ×2; the
diagnostics run fired exactly the six expected findings; probes torn
down). WO-30 → ✅ DONE in RS.*

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
1. **Start every session with `/bugs`** (s55; s87 sweep: clean).
2. **S-17g A2A channel still jumps the queue the moment the WSDLs land**
   (`docs/mef/wsdl/` still absent at s87; .p12 DONE; checklist
   `docs/mef/A2A_ENROLLMENT.md`; ASID 61135801 ENROLLED + ACTIVE).
3. **The six dispatched tts legs: 9465 ✅ (s85) · 8888 ✅ (s86) · V/ES ✅
   (s87). TWO remain, in order:** **4868 print + NEW extension submission
   builder + Sch3-L10 tie** (next NEW autonomous item; its OWN MeF family
   — package already local in docs/mef/schemas/2026v1.0; mirror
   `server/specs/4868_spec.json` cached s83) → **8915-F full unit**
   (inputs + per-disaster compute + render + IRS8915F + the 5329
   suppression seam; 179/180-day asymmetry; mirror cached s83). **Then
   8879/8878 → SEC-2..6 → S-24.** Still open from the S-22b triage:
   confirm 6252 · 9325.
4. Then the s71 queue: **bootstrap_demo 1065+1041 demo returns** → **S-21b
   1065 partner-percentage diagnostic** → **S-21c Sch B Q4 auto-answer**
   (spec-first). The 1120/709 authoring waves + the 1120-S ATS lane stay Ken-gated.
5. **Ken ratifications pending:** s85 pair (RS 9465 condition-wording nit ·
   the F9465-027-01 effective-payment seam — re-check at ATS) · s84 pair
   (front-desk role · ADMIN-only destructive deletes) · s83 email provider
   pick (Resend recommended; `docs/AUTH_EMAIL_SETUP.md`) · the s76 EFW
   late-filing arm · s75 (2210 · 8962 QSEHRA · 8889 multi-account) · s74
   (7203 line-1 fold · loss-without-7203 refusal · partnership 28(e)) · s73
   (23c/23d · Sch E line 27 · print truncation) · s72 (8867 face-fidelity ·
   Sch B K-1 listing row). Standing non-decisions: 3115 OMB nit · 8824 RS note.
6. **Design (s81–s82): Ledger live + Ken-ratified.** Optional follow-ups
   unscheduled. **STANDING: apply Ledger across the other Sherpa apps when
   Ken directs — `Design/LEDGER_DESIGN_SYSTEM.md`.**

## ▶ Waiting on Ken / external
1. **A2A: ONLY the WSDL toolkit remains** (RESUME item 2).
2. **Email provider setup (s83)** — Resend (or equal), SPF/DKIM, Render env
   vars, `set_user_email` per preparer (`docs/AUTH_EMAIL_SETUP.md`).
3. **Role assignments (s84):** `manage.py set_user_role <username> <role>`
   — e.g. promote Whit if he should edit Preparers/Print Packages.
4. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
5. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
6. File-1018 Lacerte reprint. 7. PWA install check. 8. Density feel-check (s52).
9. s69: firm's real CAF number + fax on the Preparer records.
10. Demo Render service (env=demo; GEMINI_API_KEY blank there).
11. S-22b triage confirmations (6252 · 9325 — add or skip).
12. s81 design check: eyeball Ledger (instant rollback in the picker).

## Active gates
- **Flow-assertion gate GREEN at 509 (was 506)** — FA-1040V-EFW/FA-ES-RAP/
  FA-ES-QDEBIT activated + `_run_1040ves_assertion` live in both chains;
  1040 mirror = 406 active export-verbatim (407 export − the s71-staged
  FA-1040-4835-06). Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) · 1040
  406 (+1 staged).
- **s87 suites: NEW test_1040v_es 41/41 · flow 509 ·
  tts_forms+acroform+returns 277 (manifest trip-wire re-pinned 89 → 91
  with f1040v + f1040es) · tsc 0 · vitest 300 · live demo browser probe
  green** (Start card → KY chart split V-931000/ES-931100 painted live →
  pay-by-check flip → 6 concurrent PATCHes all landed (the birth row
  lock) + farmer RAP re-derived 16,667 → render-1040v/render-1040es 200
  %PDF → page map …, Form 1040-V, Form 1040-ES (p.1), (p.2) at the very
  back → the diagnostics run fired exactly D_V_ADDR/PREP +
  D_ES_ADDR/FARMER/POSTMARK/REQUIRED; probes torn down).
- s86 suites stand: test_8888 37 · MeF+payment+9465 126 · efile/mef band
  964. s84: test_authz_sec1 17 · s83: auth 19 · prefs 14.
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: migs returns 0200 (PaymentVouchers) + 0201
  (RLS) applied BOTH DBs + seed_rules run BOTH DBs (15 D_V_*/D_ES_*
  live)** — on top of 0198/0199 (s86), 0197 (s85), accounts 0004/0005 +
  audit 0003 (s83), returns 0196 (s80).
- **RS state: WO-30 → ✅ DONE** — FA export 407 active verified; both
  voucher spec mirrors verbatim-current.
- ⚠ Local test-DB note: after a new migration run the s86 recipe once —
  standalone `django.test.utils.setup_databases(keepdb=True)` (it
  migrates test_postgres forward), then pytest `--reuse-db` works.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- Design rollback points: tag `pre-ledger-design` · old presets in the picker.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
