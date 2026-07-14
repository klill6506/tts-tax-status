# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-13, session 74. **S-22b Wave 1 third item SHIPPED —
the Form 7203 basis attachment.** The 1040-side S-corp shareholder basis
form (`IndividualForm7203`, compute existed since the K-1 flow-through
unit) gained its missing render + MeF legs in one motion: Form 7203 pages
print in the 1040 packet, the IRS7203 XML document e-files (1004-slot),
and Schedule E 28(e) checks — all three surfaces off the ONE
`k1_basis_computation` derivation. Compute fix riding it (ratification
queued): the outside stock-basis adjustment folds into Part I line 1
(§1368 ordering). NEW e-file refusal: an S-corp K-1 claiming
loss/deduction items without a confirmed 7203 refuses at extract.*

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
1. **Start every session with `/bugs`** (s55; s74 sweep: clean).
2. **NEXT UNIT (Ken-directed): S-17g A2A channel — ⚡ the .p12 LANDED (s74
   verified: `MEF_A2A_CERT_PATH`/`MEF_A2A_CERT_PASSWORD` set in
   `server/.env`, file exists, 7,276 bytes).** The ONLY remaining prereq is
   (c) the gated MeF A2A WSDL toolkit → `docs/mef/wsdl/` (absent end of
   s74 — Ken checks SOR the morning of 07-14, then e-help; checklist in
   `docs/mef/A2A_ENROLLMENT.md`; ASID 61135801 ENROLLED + ACTIVE). The
   moment the WSDLs land, CC builds `A2ATransmitter` (SOAP + WS-Security
   X.509, Login/SendSubmissions/GetNewAcks against the REAL WSDLs) behind
   the Transmitter ABC → A2A comms test in ATS under ETIN 14192.
3. **S-22b WAVE 1 (in progress — work down unless S-17g unblocks):**
   ✅ Sch E Parts I-IV XML + IRS8582 (s73, `a2cbab0`) · ✅ 8949-detail/
   Sch D full path (s73b, `3b90188`) · ✅ **7203 basis attach + Sch E
   28(e) (s74)**. Next biggest: **the compute-done XML row**
   (2210/8959/8960/8962/8889/8880/8606/5329) → EFW payment + 8888 + 9465
   → 4868 (separate MeF family + e-services checkbox) → 8915-F → W-2G →
   the 8879/8878 print pair. Each unit = the s72 recipe (mirror print via
   bridge-gates; refusal beats fabrication; XSD-parsed enums; **run the
   FULL efile/mef band after any new refusal** — s74's refusal had zero
   scenario blast radius, verified 369 green). Still open from the list
   triage: confirm the suggested additions (6252 · 1040-ES/V · 9325).
4. Then the s71 queue stands: **bootstrap_demo 1065+1041 demo returns** →
   **S-21b 1065 partner-percentage diagnostic** → **S-21c Sch B Q4
   auto-answer** (spec-first). The 1120/709 authoring waves + the 1120-S
   ATS lane stay Ken-gated.
5. **Ken ratifications pending: 3 NEW s74 items (non-blocking,
   recommendations filed)** — (a) the 7203 outside-adjustment line-1 fold
   (§1368 ordering; divergence pin); (b) the new S-corp-loss-without-7203
   e-file refusal policy; (c) the partnership 28(e) boundary. Plus the 3
   s73 items (23c/23d zero→sum · Sch E line 27 · print 3-property
   truncation) and the 2 s72 items (8867 face-fidelity · Sch B K-1
   listing row). Standing non-decisions: 3115 OMB nit rides the next RS
   3115 amendment; the 8824 RS note rides the next 8824 touch.

## ▶ Waiting on Ken / external
1. **A2A: ONLY the WSDL toolkit remains** (the .p12 is DONE — s74 verified;
   see RESUME item 2).
2. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. File-1018 Lacerte reprint (item 10). 5. PWA install check.
6. Density feel-check (s52). 7. s69: firm's real CAF number + fax on the
   Preparer records (Admin → Preparers) for the 2848 L2 autofill.
8. Demo Render service LIVE (https://tts-tax-demo.onrender.com, env=demo;
   login demo/delvio-demo). Nit: GEMINI_API_KEY blank on the demo service.
9. S-22b triage confirmations (6252 · 1040-ES/V · 9325 — add or skip).

## Active gates
- **Flow-assertion gate GREEN at 500** (s74 touches compute_7203_individual —
  no mirror pins on it; verified green post-change). Mirrors: 1120S 41 ·
  1065 39 (+4 s64 staged) · 1040 397 export-verbatim (+1 staged:
  FA-1040-4835-06 pending the 4562→4835 feeder).
- **s74 suites: MeF 1040 pure 64 (62 + IRS7203 structural + live-XSD
  2025v5.3 full-return w/ 7203 + the 28(e) checkbox pins) · NEW
  test_efile_7203_extract 7 · Sch E render leg 14 (28(e) checked/unchecked
  + packet + shape) · 7203 compute suites 32 (every pre-fix pin survived
  the line-1 fold) · FULL efile/mef band 369 · Sch E/K-1/8582/7203 band
  274 (4 pre-existing skips).** tsc/vitest untouched (no client code).
  Last full-suite GREEN = s54 `cd9b186`.
- Shared-DB deploy state: mig 0194 applied + seed 359 clean (s71); s74 adds
  **NO migrations — push-deploy only, no DB step**. The 7203 spec mirror
  refreshed from the deployed RS export (format-only drift; content
  identical — 7 rules / 4 diags / 4 tests unchanged).
- ⚠ Local test-DB note: a stale `test_postgres` blocked DB-backed tests at
  boot — dropped + the one fixture that relied on ambient seeding now
  requests `credit_forms` (test_compute_7203_individual).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
