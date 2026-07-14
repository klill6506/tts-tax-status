# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-14, session 79. **Form 8915-F drafted to Gate-1 (WO-32, the s67 recipe,
fifth consecutive gated order)** — `READY_TO_SEED=False`, SQLite-validated **87/0**, NOT
seeded, tts leg NOT started. **Ken now holds FIVE Gate-1 walks (WO-28 9465 · WO-29 8888 ·
WO-30 1040-V/ES · WO-31 4868 · WO-32 8915-F)** — one approve-all clears the whole lane. No
tts-tax-app code changed this session — RS-lane artifacts only. `/bugs`: clean. WSDLs absent.*

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
1. **Start every session with `/bugs`** (s55; s79 sweep: clean).
2. **NEXT UNIT (Ken-directed): S-17g A2A channel — the .p12 is DONE.** The ONLY
   remaining prereq is the gated MeF A2A WSDL toolkit → `docs/mef/wsdl/` (still
   absent at s79 boot; checklist in `docs/mef/A2A_ENROLLMENT.md`; ASID 61135801
   ENROLLED + ACTIVE). The moment the WSDLs land, CC builds `A2ATransmitter`
   (SOAP + WS-Security X.509, Login/SendSubmissions/GetNewAcks against the REAL
   WSDLs) behind the Transmitter ABC → A2A comms test in ATS under ETIN 14192.
3. **S-22b WAVE 1:** ✅ Sch E + 8582 (s73) · ✅ 8949/Sch D (s73b) · ✅ 7203 (s74) ·
   ✅ compute-done XML row (s75) · ✅ EFW payment records (s76) · ✅ payment-cluster
   RS batch at Gate-1 (s77) · ✅ 4868 RS draft at Gate-1 (s78) · **✅ 8915-F RS
   draft at Gate-1 (s79 — WO-32; IRS8915F rides ReturnData1040 max 6; the
   179/180-day period helpers pin all eight published date examples; walk in RS
   WORK_ORDERS + tts REVIEW_QUEUE s79)**. On Ken's approve-all (FIVE walks):
   flip sentinels → seed → verify exports → cache tts mirrors → **dispatch the
   six tts legs as a set** (9465 print+IRS9465 · 8888 print+IRS8888+35a ·
   voucher pair print-only w/ suppression ties · 4868 print + the NEW extension
   submission builder + Sch 3 L10 tie · 8915-F inputs+compute+print+IRS8915F +
   the 4b/5b landings + the 5329 suppression seam). Next NEW autonomous item
   while the gates hold: **W-2G (spec-first gap check)** → 8879/8878 print pair.
   Still open from the list triage: confirm 6252 · 9325 additions.
4. Then the s71 queue stands: **bootstrap_demo 1065+1041 demo returns** →
   **S-21b 1065 partner-percentage diagnostic** → **S-21c Sch B Q4 auto-answer**
   (spec-first). The 1120/709 authoring waves + the 1120-S ATS lane stay Ken-gated.
5. **Ken ratifications pending: the FIVE Gate-1 walks (s77 ×3 + s78 4868 + s79
   8915-F)** — REVIEW_QUEUE s77/s78/s79; recommendations = approve all. Plus the
   s76 pair (draft-to-gate plan — EXECUTED, retro-ratification; post-deadline
   EFW date policy), s75 (2210 policy · 8962 QSEHRA · 8889 multi-account), s74
   (7203 line-1 fold · loss-without-7203 refusal · partnership 28(e)), s73
   (23c/23d · Sch E line 27 · print truncation), s72 (8867 face-fidelity · Sch B
   K-1 listing row). Standing non-decisions: 3115 OMB nit · the 8824 RS note.

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
- **Flow-assertion gate GREEN at 500** (s79 touches no tts code at all).
  Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) · 1040 397 export-verbatim
  (+1 staged: FA-1040-4835-06 pending the 4562→4835 feeder).
- **s79 RS-lane gate: validate_8915f = 87/0** (guard-refusal + twice-run + all
  eight published date pins + the convention-flag presence pins). s78: 97/0.
  s77: 85/0 · 53/0 · 63/0. All five loaders ship gated; `seed_all` soft-fails
  them by name. RS prod UNTOUCHED; deployed exports still 404 for all six
  forms (correct — nothing seeds before Gate-1).
- s76 suites stand: MeF 1040 pure 79 · efile/mef band 408 · tsc 0 · vitest 300.
  Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: mig 0195 applied to BOTH prod AND demo DBs (s76).**
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
