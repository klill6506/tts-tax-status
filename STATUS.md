# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-14, session 86. **FORM 8888 FULL UNIT SHIPPED** — the
second of the six Gate-1-dispatched tts legs (RS spec WO-29). NEW Form8888
singleton (mig 0198 + RLS mig 0199, BOTH DBs — 0199 also back-fills the RLS
that mig 0197/Form9465 missed at s85) + compute_8888 pinned to the RS spec
scenarios (L5 = the account sum; the two-way tie F8888-001-04/002-03 — the
group-sum half holds BY CONSTRUCTION; whole-dollar-only rows; the published
Active F8888-* e-file gate read verbatim from the TY2025v5.3 CSV) + 12
D_8888_* diagnostics (seeded BOTH DBs) + f8888 Rev. 12-2025 AcroForm print
(20 fields label-verified; face page only — the template's pages 2-3 are the
included instructions; line 4 'Reserved for future use' deliberately
unmapped) + NEW IRS8888 MeF document (ReturnData1040 directly before
IRS8889; extract refuses on bond asks / single-account / row gaps / any
blocker) + the THREE 1040-side ties: Form8888Ind checks on 35a w/
referenceDocumentId (IND-091/092), the return-level 35b-d emit suppressed
(IND-084), and the header RefundDisbursementGrp fans out per account (max 3).
ALSO: the 1040 print face gained the 35a 8888-checkbox + the 35b-d DD fields
(the S-17b print-parity gap closed in passing — blank when 8888 rides), and
the probe caught a LIVE lost-update race on concurrent card autosaves —
form-8888 AND form-9465 PATCH are now select_for_update row-locked (the
8941 class). **FA-8888-TIE/SPLIT/NOBOND activated in RS (`a3fb215`), 1040
mirror refreshed export-verbatim (403 = 404 export − the s71-staged
4835-06), runners in both chains — flow gate 503 → 506.** Suites: NEW
test_8888 37 (spec oracles · builder order · live-XSD full-return w/
IRS8888 + Form8888Ind/IND-084 pins · extract gate · endpoint · diagnostics
· field-map/PDF) · MeF+payment+9465 126 · FULL efile/mef/scenario band 964
· tts_forms+acroform 201 (manifest 88 → 89) · returns 76 · tsc 0 · vitest
300 · live demo probe (card + mis-tie blocker repaint + the CONCURRENT-
VOLLEY lock verification + print landing 7,000/640/7,640 + packet order
…8867/8888… + the 1040 35a X w/ blank 35b-d; probe rows torn down).*

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
1. **Start every session with `/bugs`** (s55; s86 sweep: clean).
2. **S-17g A2A channel still jumps the queue the moment the WSDLs land**
   (`docs/mef/wsdl/` still absent at s86; .p12 DONE; checklist
   `docs/mef/A2A_ENROLLMENT.md`; ASID 61135801 ENROLLED + ACTIVE).
3. **The six dispatched tts legs: 9465 ✅ (s85) · 8888 ✅ (s86). FOUR remain,
   in order:** **1040-V/ES vouchers** (next NEW autonomous item; print-only,
   suppression ties — EFW election → no V, debited quarter → no paper
   voucher; THREE-way GA Charlotte address trap V 1214 / ES 1300; mirrors
   `server/specs/1040v_spec.json` + `1040es_spec.json` cached s83) →
   **4868 print + NEW extension submission builder + Sch3-L10 tie** (its
   OWN MeF family — package already local in docs/mef/schemas/2026v1.0) →
   **8915-F full unit** (inputs + per-disaster compute + render + IRS8915F
   + the 5329 suppression seam; 179/180-day asymmetry). **Then 8879/8878 →
   SEC-2..6 → S-24.** Still open from the S-22b triage: confirm 6252 · 9325.
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
- **Flow-assertion gate GREEN at 506 (was 503)** — FA-8888-TIE/SPLIT/NOBOND
  activated + runners live in both chains; 1040 mirror = 403 active
  export-verbatim (404 export − the s71-staged FA-1040-4835-06).
  Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) · 1040 403 (+1 staged).
- **s86 suites: NEW test_8888 37/37 · MeF+payment+9465 126 · FULL
  efile/mef/scenario band 964 · tts_forms+acroform 201 (manifest trip-wire
  88 → 89 with f8888) · returns 76 · tsc 0 · vitest 300 · live demo browser
  probe green** (Start card → mis-tied volley → F8888-002-03 + row-problem
  banner → concurrent-volley re-fire verified the NEW row lock → tie fixed
  → "Split is valid"; print 7,000/640/7,640 + RTNs/accounts on the face;
  packet …8867/**8888**; 1040 p2 35a **X** with 35b-d blank; probes torn
  down).
- **⚠ NEW fix riding the unit: form-8888 AND form-9465 PATCH row-locked**
  (select_for_update — the 8941 lost-update class, probe-caught live).
- s84 suites stand: test_authz_sec1 17 · s83: auth 19 · prefs 14.
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: migs returns 0198 (Form8888) + 0199 (RLS on
  form8888 AND the s85-missed form9465) applied BOTH DBs + seed_rules run
  BOTH DBs (12 D_8888_* live)** — on top of 0197 (s85), accounts 0004/0005
  + audit 0003 (s83), returns 0196 (s80).
- **RS state: WO-29 → ✅ DONE (`a3fb215`)** — FA export 404 active verified;
  8888 spec mirror verbatim-current.
- ⚠ Local test-DB note: a stale `test_postgres` blocks DB-backed runs
  (relation-missing errors against database=postgres) — run
  `django.test.utils.setup_databases(keepdb=True)` once standalone (it
  migrates test_postgres forward), then pytest `--reuse-db` works (s86).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- Design rollback points: tag `pre-ledger-design` · old presets in the picker.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
