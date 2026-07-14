# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-13, session 75. **S-22b Wave 1 item 4 SHIPPED — the
compute-done XML row.** Seven compute-done 1040 forms gained their MeF
document legs in one unit: IRS5329 · IRS8606 · IRS8880 · IRS8889 ·
IRS8959 · IRS8960 · IRS8962, each bridge-gated on the SAME derivation its
printed face uses, each at its cited ReturnData1040.xsd slot. Form 2210 is
deliberately NOT a document — F2210-002-02 requires a Part II box on any
transmitted 2210 and i2210 says don't file it when no box applies; the
penalty rides the 1040's EsPenaltyAmt, and the modeled box C (annualized
election) refuses pending a Schedule AI compute leg. New refusals: 8959
RRTA rows (RED-deferred compute) · 8889 multi-account-per-owner · 2210
box C. Zero scenario blast radius (full band verified).*

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
1. **Start every session with `/bugs`** (s55; s75 sweep: clean).
2. **NEXT UNIT (Ken-directed): S-17g A2A channel — the .p12 is DONE
   (`MEF_A2A_CERT_PATH`/`MEF_A2A_CERT_PASSWORD` in `server/.env`, verified
   s74).** The ONLY remaining prereq is the gated MeF A2A WSDL toolkit →
   `docs/mef/wsdl/` (still absent end of s75 — Ken checks SOR the morning
   of 07-14, then e-help; checklist in `docs/mef/A2A_ENROLLMENT.md`; ASID
   61135801 ENROLLED + ACTIVE). The moment the WSDLs land, CC builds
   `A2ATransmitter` (SOAP + WS-Security X.509,
   Login/SendSubmissions/GetNewAcks against the REAL WSDLs) behind the
   Transmitter ABC → A2A comms test in ATS under ETIN 14192.
3. **S-22b WAVE 1 (in progress — work down unless S-17g unblocks):**
   ✅ Sch E + 8582 (s73) · ✅ 8949/Sch D full path (s73b) · ✅ 7203 basis
   attach (s74) · ✅ **the compute-done XML row (s75: 5329/8606/8880/8889/
   8959/8960/8962 + the 2210 gate)**. Next: **the EFW payment cluster +
   8888 + 9465** → 4868 (separate MeF family + e-services checkbox) →
   8915-F → W-2G → the 8879/8878 print pair. Recipe = s72 (mirror print
   via bridge-gates; refusal beats fabrication; XSD-parsed enums; **run
   the FULL efile/mef band after any new refusal** — s75's three refusals
   had zero blast radius, band 395 green). Still open from the list
   triage: confirm the suggested additions (6252 · 1040-ES/V · 9325).
4. Then the s71 queue stands: **bootstrap_demo 1065+1041 demo returns** →
   **S-21b 1065 partner-percentage diagnostic** → **S-21c Sch B Q4
   auto-answer** (spec-first). The 1120/709 authoring waves + the 1120-S
   ATS lane stay Ken-gated.
5. **Ken ratifications pending: 3 NEW s75 items (non-blocking,
   recommendations filed)** — (a) the Form 2210 e-file policy (no IRS2210
   document ever; box C refuses); (b) 8962 QSEHRAInd default-false
   (schema-required, unmodeled); (c) the 8889 multi-account refusal + the
   print-side one-face-per-owner question. Plus the 3 s74 items (7203
   line-1 fold · loss-without-7203 refusal · partnership 28(e) boundary),
   the 3 s73 items (23c/23d zero→sum · Sch E line 27 · print 3-property
   truncation) and the 2 s72 items (8867 face-fidelity · Sch B K-1
   listing row). Standing non-decisions: 3115 OMB nit rides the next RS
   3115 amendment; the 8824 RS note rides the next 8824 touch.

## ▶ Waiting on Ken / external
1. **A2A: ONLY the WSDL toolkit remains** (the .p12 is DONE; see RESUME
   item 2).
2. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. File-1018 Lacerte reprint (item 10). 5. PWA install check.
6. Density feel-check (s52). 7. s69: firm's real CAF number + fax on the
   Preparer records (Admin → Preparers) for the 2848 L2 autofill.
8. Demo Render service LIVE (https://tts-tax-demo.onrender.com, env=demo;
   login demo/delvio-demo). Nit: GEMINI_API_KEY blank on the demo service.
9. S-22b triage confirmations (6252 · 1040-ES/V · 9325 — add or skip).

## Active gates
- **Flow-assertion gate GREEN at 500** (s75 touches no compute code —
  extract/builder/tests only). Mirrors: 1120S 41 · 1065 39 (+4 s64
  staged) · 1040 397 export-verbatim (+1 staged: FA-1040-4835-06 pending
  the 4562→4835 feeder).
- **s75 suites: MeF 1040 pure 75 (64 + 11 structural/enum pins + the
  live-XSD 2025v5.3 full-return carrying ALL SEVEN new documents) · NEW
  test_efile_computedone_extract 15 (FFV gates · owner-derivation mirrors
  · the 8959-RRTA/8889-dual/2210-boxC refusals · the 2210 lawful-omission
  pin) · FULL efile/mef band 395.** tsc/vitest untouched (no client code).
  Last full-suite GREEN = s54 `cd9b186`.
- Shared-DB deploy state: mig 0194 applied + seed 359 clean (s71); s75
  adds **NO migrations — push-deploy only, no DB step**.
- ⚠ Local test-DB note (s74): a stale `test_postgres` blocks DB-backed
  runs with FormDefinition.DoesNotExist — drop it (`psql -U postgres -h
  127.0.0.1`, pw tts_local_test); some fixtures rely on ambient seeding.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
