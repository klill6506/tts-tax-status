# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-13, session 76. **The EFW half of the S-22b payment
cluster SHIPPED (mig 0195).** IRSPayment (balance-due direct debit, tail
slot 5661) + IRSESPayment (scheduled quarterly estimate debits, 5668, max
4) e-file off NEW inputs: efw_elected / efw_payment_date / es_debit_q1-4
on TaxReturn + daytime_phone on Taxpayer, exposed on the Payments-tab EFW
card. Refusals: bank/date/phone gaps + a requested date past the due date
(FPYMT-072-01). **The rest of the cluster is RS-gated — 8888, 9465, and
the 1040-V/1040-ES vouchers all 404 in Rule Studio; the draft-to-gate
plan is filed in REVIEW_QUEUE s76.***

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
1. **Start every session with `/bugs`** (s55; s76 sweep: clean).
2. **NEXT UNIT (Ken-directed): S-17g A2A channel — the .p12 is DONE.** The
   ONLY remaining prereq is the gated MeF A2A WSDL toolkit →
   `docs/mef/wsdl/` (still absent end of s76 — Ken checks SOR the morning
   of 07-14, then e-help; checklist in `docs/mef/A2A_ENROLLMENT.md`; ASID
   61135801 ENROLLED + ACTIVE). The moment the WSDLs land, CC builds
   `A2ATransmitter` (SOAP + WS-Security X.509,
   Login/SendSubmissions/GetNewAcks against the REAL WSDLs) behind the
   Transmitter ABC → A2A comms test in ATS under ETIN 14192.
3. **S-22b WAVE 1 (in progress):** ✅ Sch E + 8582 (s73) · ✅ 8949/Sch D
   (s73b) · ✅ 7203 (s74) · ✅ compute-done XML row (s75) · ✅ **EFW
   payment records (s76 — IRSPayment + IRSESPayment)**. The REST of the
   payment cluster (8888 · 9465 · 1040-V/1040-ES vouchers) is **RS-gated
   (404)** — next actionable move is the **RS draft-to-gate batch (9465 →
   8888 → the voucher pair, s67 recipe; each parks ⏳ AWAITING KEN)**,
   unless Ken redirects. After the cluster: 4868 (separate MeF family) →
   8915-F → W-2G → the 8879/8878 print pair. Still open from the list
   triage: confirm 6252 · 1040-ES/V · 9325 additions.
4. Then the s71 queue stands: **bootstrap_demo 1065+1041 demo returns** →
   **S-21b 1065 partner-percentage diagnostic** → **S-21c Sch B Q4
   auto-answer** (spec-first). The 1120/709 authoring waves + the 1120-S
   ATS lane stay Ken-gated.
5. **Ken ratifications pending: 2 NEW s76 items** — (a) the payment-cluster
   draft-to-gate plan (RS 404 gate); (b) the post-deadline EFW
   requested-date policy (keep the hard ≤-due-date refusal this season;
   transmit-time defaulting rides S-17g). Plus s75 (2210 policy · 8962
   QSEHRA · 8889 multi-account), s74 (7203 line-1 fold ·
   loss-without-7203 refusal · partnership 28(e)), s73 (23c/23d · Sch E
   line 27 · print truncation), s72 (8867 face-fidelity · Sch B K-1
   listing row). Standing non-decisions: 3115 OMB nit · the 8824 RS note.

## ▶ Waiting on Ken / external
1. **A2A: ONLY the WSDL toolkit remains** (the .p12 is DONE; RESUME item 2).
2. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. File-1018 Lacerte reprint (item 10). 5. PWA install check.
6. Density feel-check (s52). 7. s69: firm's real CAF number + fax on the
   Preparer records (Admin → Preparers) for the 2848 L2 autofill.
8. Demo Render service LIVE (https://tts-tax-demo.onrender.com, env=demo;
   login demo/delvio-demo). Nit: GEMINI_API_KEY blank on the demo service.
9. S-22b triage confirmations (6252 · 1040-ES/V · 9325 — add or skip).

## Active gates
- **Flow-assertion gate GREEN at 500** (s76 touches no compute code).
  Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) · 1040 397 export-verbatim
  (+1 staged: FA-1040-4835-06 pending the 4562→4835 feeder).
- **s76 suites: MeF 1040 pure 79 (75 + the 4 payment-record pins incl.
  live-XSD w/ IRSPayment + all four ES quarters) · NEW
  test_efile_payment_extract 9 · FULL efile/mef band 408 · tsc 0 ·
  vitest 300 · info-endpoint suites 86 · live demo browser probe
  (autosave → ORM-verified; probe data cleared).** Last full-suite GREEN
  = s54 `cd9b186`.
- **Shared-DB deploy state: mig 0195 applied to BOTH prod AND demo DBs
  (s76).** Render deploy re-runs it idempotently on push.
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
