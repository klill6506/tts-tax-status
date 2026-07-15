# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-15, session 88. **FORM 4868 SHIPPED** — the fourth
of the six Gate-1-dispatched tts legs (RS spec WO-31). Print + its OWN
MeF SUBMISSION FAMILY: ReturnTypeCd "4868" (Return4868/ReturnHeader4868/
ReturnData4868, Extensions package 2026v1.0 — extracted from the
already-local zip; NEW builder_4868 + read_model_4868 + Mapper4868TY2025
registered (2025, "2026v1.0", "4868"); schema_locator gained
family_version_root for the 3-deep standalone family packages, existing
families regression-pinned identical). NEW Form4868 singleton (mig 0202
+ RLS mig 0203, BOTH DBs) + compute_4868 pinned to all 10 RS oracles
(the L6 floor · F4868-001/-002 windows · Oct-15 / DERIVED Dec-15 · the
90% two-prong harbor · the payment-triggered signature/jurat ladder —
a no-payment e-filed 4868 carries NO signature at all (R0000-098) · the
FPYMT-052-02 EFW tie · BOTH where-to-file columns partition-pinned —
**the GA Charlotte trap is FOUR-way: V 1214 / ES 1300 / 4868 1302 /
foreign 1303**) + 16 D_4868_* seeded BOTH DBs + f4868 (2025) AcroForm
print (manifest 91→92; suppression IS the render gate — a recorded
e-payment confirmation means the extension already processed, IND-900;
**STANDALONE ONLY, never in the packet** — 'Don't attach the 4868 to
the return', pinned by test) + the form-4868 endpoint (row-locked PATCH
from birth, **recompute on PATCH/DELETE — this card FEEDS compute**) +
the Payments-tab card. **The R-4868-CREDIT tie is live: line 7 → Sch 3
line 10 YELLOW feeder in compute_sch_3 (relation-guarded; override
survives; DELETE clears the stale derive — the s66 class caught on the
live probe; fetched FRESH, never via the reverse-cache — the singleton
DELETE class caught by test). The L5 derive sums components
(25d+26+27..31 − Sch3 L10), never line 33 itself — the
subtract-from-33 form DIVERGES one L10 per recompute when 33 is
overridden (caught by the endpoint test).**
**FA-4868-L6/EFW/CREDIT activated in RS (export 410 verified), 1040
mirror 409 export-verbatim (ASCII — the cp1252 loader), runners in both
chains — flow gate 512.** Suites: NEW test_4868 44 (incl. live-XSD ×2
on the Extensions tree) · flow 512 · forms/returns/payment band 391
(trip-wire 92) · FULL efile/mef/scenario band 966 · tsc 0 · vitest 300
· live demo probe (TX return: Austin no-pay → typed L4 → Charlotte 1302
flip · Sch3 L10→L31→L33 chain ORM-verified · 6-PATCH volley · render
200 %PDF · diagnostics fired exactly ADDR/CREDIT/EPAY · delete-clears
re-probed clean; one transient pooler drop mid-recompute, not code).
WO-31 → ✅ DONE in RS.*

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
1. **Start every session with `/bugs`** (s55; s88 sweep: clean).
2. **S-17g A2A channel still jumps the queue the moment the WSDLs land**
   (`docs/mef/wsdl/` still absent at s88; .p12 DONE; checklist
   `docs/mef/A2A_ENROLLMENT.md`; ASID 61135801 ENROLLED + ACTIVE).
3. **The six dispatched tts legs: 9465 ✅ (s85) · 8888 ✅ (s86) · V/ES ✅
   (s87) · 4868 ✅ (s88). ONE remains:** **8915-F full unit** (next NEW
   autonomous item — inputs + per-disaster compute + render + IRS8915F
   MeF document + the 5329 suppression seam; ⚠⚠ the 179/180-day
   asymmetry; item A/B naming; married = a SEPARATE form per spouse;
   maxOccurs=6; mirror `server/specs/8915f_spec.json` cached s83).
   **Then 8879/8878 → SEC-2..6 → S-24.** Still open from the S-22b
   triage: confirm 6252 · 9325.
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
- **Flow-assertion gate GREEN at 512 (was 509)** — FA-4868-L6/EFW/CREDIT
  activated + `_run_4868_assertion` live in both chains; 1040 mirror =
  409 active export-verbatim (410 export − the s71-staged
  FA-1040-4835-06). Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) · 1040
  409 (+1 staged).
- **s88 suites: NEW test_4868 44/44 (incl. live-XSD ×2 vs the 2026v1.0
  Extensions tree) · flow 512 · tts_forms+acroform+returns+payment band
  391 (manifest trip-wire re-pinned 91 → 92 with f4868) · FULL
  efile/mef/scenario band 966 · tsc 0 · vitest 300 · live demo browser
  probe green** (magic-link sign-in → TX return Payments tab → Start
  card → derived L4/L5 painted → typed L4 5000 → the with-payment
  Charlotte-1302 flip painted live → Sch3 L10 4391 → 1040 L31 4391 →
  L33 5000 ORM-verified → 6 concurrent PATCHes all landed (the birth
  row lock) → render-4868 200 %PDF (1 face page) → the diagnostics run
  fired exactly D_4868_ADDR/CREDIT/EPAY → delete → the stale-derive +
  reverse-cache classes caught live → BOTH fixed + tested → re-probed
  clean; demo return restored; one transient pooler connection drop
  mid-recompute logged, not code).
- s87 suites stand: test_1040v_es 41 · s86: test_8888 37 · s84:
  test_authz_sec1 17 · s83: auth 19 · prefs 14.
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: migs returns 0202 (Form4868) + 0203 (RLS)
  applied BOTH DBs + seed_rules run BOTH DBs (16 D_4868_* live)** — on
  top of 0200/0201 (s87), 0198/0199 (s86), 0197 (s85), accounts
  0004/0005 + audit 0003 (s83), returns 0196 (s80).
- **RS state: WO-31 → ✅ DONE** — FA export 410 active verified; the
  4868 spec mirror verbatim-current.
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
