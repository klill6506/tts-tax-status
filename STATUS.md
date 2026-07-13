# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-13, session 73. **S-22b Wave 1 item 1 SHIPPED
(`a2cbab0`) — Schedule E Parts I-III + Form 8582 are now real 1040 MeF
documents** (the biggest live e-file gap from Ken's TaxWise list: any 1040
with rentals/royalties/K-1 pass-through REFUSED until today; now the full
schedule e-files, with the IRS8582 passive-loss document attached and
28(g)/33(c) reference-linked). Both mirror the print via bridge-gates
(`schedule_e_p2_rows` for the K-1 face rows; `per_activity_allocation` +
the line-C≤0 gate for the 8582 worksheets). REMIC (Part IV) refuses — no
model. Fix-forward: the s72 8867 refusal had silently broken Scenario 2's
IFA artifact test — the scenario now attests due diligence and IRS8867
joins its document sequence. Also fixed live: Schedule E 23c/23d printed
blank (hard-coded ZERO) — now the line-12/18 sums per spec (ratification
queued).*

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
1. **Start every session with `/bugs`** (s55; s73 sweep: clean).
2. **NEXT UNIT (Ken-directed 2026-07-13): S-17g A2A channel — cert INSTALLED,
   ASID 61135801 ENROLLED + ACTIVE (AE 100% done).** Remaining prereqs on Ken
   (checklist in `docs/mef/A2A_ENROLLMENT.md`): (b) export the cert+key as a
   password-protected .p12 OUTSIDE the repo → `MEF_A2A_CERT_PATH`/
   `MEF_A2A_CERT_PASSWORD` in `server/.env` (public Signing .cer already at
   `C:\Users\Ken\Documents\mef_a2a_signing.cer`, thumbprint FA27333…ADA9);
   (c) the gated MeF A2A WSDL toolkit → `docs/mef/wsdl/` (not in SOR end of
   s72 — Ken checks the morning of 07-14, then e-help). CC then builds
   `A2ATransmitter` (SOAP + WS-Security X.509, Login/SendSubmissions/
   GetNewAcks against the REAL WSDLs) behind the Transmitter ABC → **A2A
   comms test in ATS under ETIN 14192** (precedes the Nov-1 A2A-healthy
   gate). The SOAP skeleton CAN start against a scratch key if the WSDLs
   arrive before the .p12.
3. **S-22b WAVE 1 (in progress — work down unless S-17g unblocks):**
   ✅ **Sch E Parts I-IV XML + IRS8582 (s73, `a2cbab0`)**. Next biggest:
   **8949-detail/Sch D full path** → 7203 attach (+ the Sch E 28(e)/28(f)
   checkboxes that ride it) → the compute-done XML row (2210/8959/8960/
   8962/8889/8880/8606/5329) → EFW payment + 8888 + 9465 → 4868 (separate
   MeF family + e-services checkbox) → 8915-F → W-2G → the 8879/8878 print
   pair. Each unit = the s72 Schedule B/8867 recipe (mirror print via
   bridge-gates; refusal beats fabrication; XSD-parsed enums). Still open
   from the list triage: confirm the suggested additions (6252 · 1040-ES/V
   · 9325).
4. Then the s71 queue stands: **bootstrap_demo 1065+1041 demo returns** →
   **S-21b 1065 partner-percentage diagnostic** → **S-21c Sch B Q4
   auto-answer** (spec-first). The 1120/709 authoring waves + the 1120-S
   ATS lane stay Ken-gated.
5. **Ken ratifications pending: 3 OPEN s73 items (non-blocking,
   recommendations filed)** — (a) the Schedule E 23c/23d zero→sum compute
   fix; (b) the never-answered Sch E line 27 question (derived-No leg
   candidate); (c) the print 3-property truncation vs full-XML emission
   (print continuation-page leg candidate). Plus the 2 s72 items (8867
   face-fidelity leg · Sch B K-1 listing row). Standing non-decisions: 3115
   OMB nit rides the next RS 3115 amendment; the 8824 RS note rides the
   next 8824 touch.

## ▶ Waiting on Ken / external
1. **A2A prereqs** (.p12 export + WSDL toolkit — see RESUME item 2).
2. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. File-1018 Lacerte reprint (item 10). 5. PWA install check.
6. Density feel-check (s52). 7. s69: firm's real CAF number + fax on the
   Preparer records (Admin → Preparers) for the 2848 L2 autofill.
8. Demo Render service LIVE (https://tts-tax-demo.onrender.com, env=demo;
   login demo/delvio-demo). Nit: GEMINI_API_KEY blank on the demo service.
9. S-22b triage confirmations (6252 · 1040-ES/V · 9325 — add or skip).

## Active gates
- **Flow-assertion gate GREEN at 500** (unchanged — s73's 23c/23d fix pins
  nothing in the mirror; verified). Mirrors: 1120S 41 · 1065 39 (+4 s64
  staged) · 1040 397 export-verbatim (+1 staged: FA-1040-4835-06 pending
  the 4562→4835 feeder).
- **s73 suites: MeF 1040 pure 59 (56 + Sch E/8582 structural + live-XSD
  2025v5.3 full-return validation) · NEW test_efile_sche_8582_extract 7 ·
  schedule_e/8582 band 127 · scenario2 29 (8867 fix-forward) · full
  efile/mef band 350.** s72/s70 suites otherwise unchanged (test_3115 39 ·
  manifest/acroform 201 · pair 36 · tsc 0 · vitest 300). Last full-suite
  GREEN = s54 `cd9b186`.
- Shared-DB deploy state: mig 0194 applied + seed 359 clean (s71); s73 adds
  NO migrations — **push-deploy carries `a2cbab0` with no DB step**.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
