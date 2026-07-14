# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-14, session 80. **W-2G MeF document leg SHIPPED (mig 0196)** — the
"W-2G spec-first gap check" queue item resolved: the RS spec EXISTS (`FORM_W2G`, 200 — the
s79-era 404 expectation was a lookup-key artifact; the key is `FORM_W2G`, not `W2G`), the
input/compute legs shipped 2026-06-20, and the REAL gap was e-file — line 25c transmitted
W-2G box-4 withholding with no backing IRSW2G document (F1040-034-08 arm (4) = Math-Error
reject). Ken still holds FIVE Gate-1 walks (WO-28/29/30/31/32). `/bugs` s80: clean.
WSDLs still absent.*

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
1. **Start every session with `/bugs`** (s55; s80 sweep: clean).
2. **NEXT UNIT (Ken-directed): S-17g A2A channel — the .p12 is DONE.** The ONLY
   remaining prereq is the gated MeF A2A WSDL toolkit → `docs/mef/wsdl/` (still
   absent at s80; checklist in `docs/mef/A2A_ENROLLMENT.md`; ASID 61135801
   ENROLLED + ACTIVE). The moment the WSDLs land, CC builds `A2ATransmitter`
   (SOAP + WS-Security X.509, Login/SendSubmissions/GetNewAcks against the REAL
   WSDLs) behind the Transmitter ABC → A2A comms test in ATS under ETIN 14192.
3. **S-22b:** Wave-1 built row now ✅ Sch E + 8582 (s73) · ✅ 8949/Sch D (s73b) ·
   ✅ 7203 (s74) · ✅ compute-done XML row (s75) · ✅ EFW payment records (s76) ·
   **✅ W-2G MeF document (s80 — IRSW2G after IRSW2, mig 0196; refusals =
   published FW2G rejects; stale-8b bridge-gate)**. RS-gated at Gate-1: the
   payment-cluster batch (s77) + 4868 (s78) + 8915-F (s79). On Ken's
   approve-all (FIVE walks): flip sentinels → seed → verify exports → cache tts
   mirrors → **dispatch the six tts legs as a set** (9465 · 8888 · voucher pair ·
   4868 print + extension submission builder · 8915-F full unit). Next NEW
   autonomous item while the gates hold: **the 8879/8878 print pair**. Still
   open from the list triage: confirm 6252 · 9325 additions.
4. Then the s71 queue stands: **bootstrap_demo 1065+1041 demo returns** →
   **S-21b 1065 partner-percentage diagnostic** → **S-21c Sch B Q4 auto-answer**
   (spec-first). The 1120/709 authoring waves + the 1120-S ATS lane stay Ken-gated.
5. **Ken ratifications pending: the FIVE Gate-1 walks (s77 ×3 + s78 4868 + s79
   8915-F)** — REVIEW_QUEUE s77/s78/s79; recommendations = approve all. Plus the
   s76 pair, s75 (2210 · 8962 QSEHRA · 8889 multi-account), s74 (7203 line-1
   fold · loss-without-7203 refusal · partnership 28(e)), s73 (23c/23d · Sch E
   line 27 · print truncation), s72 (8867 face-fidelity · Sch B K-1 listing
   row). s80 adds NO new ratifications (every refusal = a published Active
   reject rule). Standing non-decisions: 3115 OMB nit · the 8824 RS note.

## ▶ Waiting on Ken / external
1. **A2A: ONLY the WSDL toolkit remains** (the .p12 is DONE; RESUME item 2).
2. **The FIVE Gate-1 walks** — WO-28 9465 · WO-29 8888 · WO-30 1040-V/ES ·
   WO-31 4868 · WO-32 8915-F.
3. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
4. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
5. File-1018 Lacerte reprint (item 10). 6. PWA install check.
7. Density feel-check (s52). 8. s69: firm's real CAF number + fax on the
   Preparer records (Admin → Preparers) for the 2848 L2 autofill.
9. Demo Render service LIVE (https://tts-tax-demo.onrender.com, env=demo;
   login demo/delvio-demo). Nit: GEMINI_API_KEY blank on the demo service.
10. S-22b triage confirmations (6252 · 9325 — add or skip).

## Active gates
- **Flow-assertion gate GREEN at 500** (s80 touches no compute code — extract/
  builder/model-fields only). Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) ·
  1040 397 export-verbatim (+1 staged: FA-1040-4835-06 pending the 4562→4835
  feeder).
- **s80 suites: MeF 1040 pure 81 (79+2, live-XSD w/ IRSW2G docs) · NEW
  test_efile_w2g_extract 13 · efile band 205 + scenario band 161 · tsc 0 ·
  vitest 300 · live demo browser+ORM probe green (probe data cleaned).**
- s79 RS-lane gates stand: validate_8915f 87/0 · s78 97/0 · s77 85/0·53/0·63/0.
  All five loaders gated; `seed_all` soft-fails them; RS prod untouched.
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: mig 0196 applied to BOTH prod AND demo DBs (s80).**
- ⚠ Local test-DB note (s74): a stale `test_postgres` blocks DB-backed runs
  with FormDefinition.DoesNotExist — drop it if hit.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
