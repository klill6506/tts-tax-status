# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-14, session 85. **FORM 9465 FULL UNIT SHIPPED** — the
first of the six Gate-1-dispatched tts legs (RS spec WO-28). NEW Form9465
singleton (mig 0197, BOTH DBs) + compute_9465 pinned to all 10 RS oracles
(L10 = whole-dollar ceiling of L9÷72; router/fee-ladder/Part II gate; the
published Active F9465-* e-file gate read verbatim from the TY2025v5.3 CSV) +
17 D_9465_* diagnostics (seeded BOTH DBs) + f9465 Rev. 9-2020 AcroForm print
(all 66 fields label-verified; packet FRONT tier per i9465) + NEW IRS9465 MeF
document (InstallmentAgreement family; extract refuses on every blocker,
F9465-019-02 ties line 8 to the actual IRSPayment record) + Payments-tab card.
**FA-9465-MIN/EFILE/EFW activated in RS (`7bc1e79`), 1040 mirror refreshed
export-verbatim, runners in both chains — flow gate 500 → 503.** Suites: NEW
test_9465 36 (incl. live-XSD full-return w/ IRS9465) · bands 90+200+162 ·
tsc 0 · vitest 300 · live demo probe (card + blocker round-trip + print
value landing + packet order). Boundaries → DEFERRAL_AUDIT s85 (7); two RS
nits → REVIEW_QUEUE s85.*

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
1. **Start every session with `/bugs`** (s55; s85 sweep: clean).
2. **S-17g A2A channel still jumps the queue the moment the WSDLs land**
   (`docs/mef/wsdl/` still absent at s85; .p12 DONE; checklist
   `docs/mef/A2A_ENROLLMENT.md`; ASID 61135801 ENROLLED + ACTIVE).
3. **The six dispatched tts legs: 9465 ✅ DONE (s85). FIVE remain, in order:**
   **8888 print+IRS8888+35a** (next NEW autonomous item; mirror
   `server/specs/8888_spec.json` cached s83 — ⚠ Rev-12-2025 retired savings
   bonds, 2-3-account splitter only, F8888-023 forbids the check line) →
   **1040-V/ES vouchers** (print-only, suppression ties; THREE-way GA
   Charlotte address trap) → **4868 print + NEW extension submission
   builder + Sch3-L10 tie** (its OWN MeF family) → **8915-F full unit**
   (inputs + per-disaster compute + render + IRS8915F + the 5329
   suppression seam; 179/180-day asymmetry). **Then 8879/8878 →
   SEC-2..6 → S-24.** Still open from the S-22b triage: confirm 6252 · 9325.
4. Then the s71 queue: **bootstrap_demo 1065+1041 demo returns** → **S-21b
   1065 partner-percentage diagnostic** → **S-21c Sch B Q4 auto-answer**
   (spec-first). The 1120/709 authoring waves + the 1120-S ATS lane stay Ken-gated.
5. **Ken ratifications pending:** NEW s85 pair (RS 9465 condition-wording
   nit · the F9465-027-01 effective-payment seam — re-check at ATS) · s84
   pair (front-desk role · ADMIN-only destructive deletes) · s83 email
   provider pick (Resend recommended; `docs/AUTH_EMAIL_SETUP.md`) · the s76
   EFW late-filing arm · s75 (2210 · 8962 QSEHRA · 8889 multi-account) · s74
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
- **Flow-assertion gate GREEN at 503 (was 500)** — FA-9465-MIN/EFILE/EFW
  activated + runners live in both chains; 1040 mirror = 400 active
  export-verbatim (401 export − the s71-staged FA-1040-4835-06).
  Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) · 1040 400 (+1 staged).
- **s85 suites: NEW test_9465 36/36 (10 spec oracles · FA pins · builder
  order · extract refusals · endpoint · diagnostics · field-map/PDF ·
  live-XSD full-return carrying IRS9465) · MeF 1040 + payment extract 90 ·
  acroform/manifest 200 + tts_forms 162 (manifest trip-wire 87 → 88 with
  f9465) · tsc 0 · vitest 300 · live demo browser probe green** (card
  renders w/ computed banner; typed-100 → autosave → F9465-027-01 blocker
  painted; print values land 8,400×3/117/300/phone/day; packet order
  Letter/Invoice/**9465 FRONT**/1040; probe rows torn down).
- s84 suites stand: test_authz_sec1 17 · s83: auth 19 · prefs 14.
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: mig returns 0197 (Form9465) applied BOTH DBs +
  seed_rules run BOTH DBs (17 D_9465_* live)** — on top of accounts
  0004/0005 + audit 0003 (s83), returns 0196 (s80).
- **RS state: WO-28 → ✅ DONE (`7bc1e79`)** — FA export 401 active verified;
  9465 spec mirror verbatim-current.
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
