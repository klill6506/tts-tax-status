# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-13, session 72. **S-22a SHIPPED — Schedule B + Form
8867 are now real 1040 MeF documents** (the two live pre-season e-file gaps:
Sch B is required over $1,500 of interest/dividends; 8867 is required on
every paid-preparer EIC/CTC/ODC/AOTC/HOH claim — our ATS scenarios passed
without them, production returns would not). Both mirror the print exactly
(bridge-gates: `schedule_b_required` / the `render_8867` claim derivations);
IRS1040ScheduleB sits in its 920-slot with the literal-coded write-ins and
XSD-parsed FIPS country codes; IRS8867 in its 1902-slot, paid-preparer-only,
composite rows → primary elements only, attestation → the certification.
Refusal boundaries: nonzero Sch B line 3 (no IRS8815 doc) and a
blank-checklist covered-credit claim. Origin: Ken's e-services call — the
software-package screens need the supported-forms lists (delivered in-chat:
1040 package + 1120-S package, e-fileable sets only).*

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
1. **Start every session with `/bugs`** (s55; s72 sweep: clean).
2. **⏳ KEN IS PASTING a TaxWise-derived list of forms the practice needs** —
   when it lands, rank by firm volume and cut **Spine S-22b** into MeF-document
   units (the s72 Schedule B/8867 recipe: extract source + builder + XSD
   position + pure/DB tests). Known print-only-today candidates already
   inventoried in DEFERRAL_AUDIT s72 (8959 · 8960 · 8962 · 2210 · 8582 ·
   8880 · 8889 · 8606 · 5695 · 5329 · 1116 · 8615 · Sch J · 6252 · 4797 ·
   8949-detail · 8815).
3. Otherwise the s71 queue stands: **bootstrap_demo 1065+1041 demo returns**
   (two-partner LLC w/ 8825 + GPs + K-1s; a trust w/ DNI + beneficiary K-1;
   demo DB + browser-verify) → **S-21b 1065 partner-percentage diagnostic** →
   **S-21c Sch B Q4 auto-answer** (spec-first). The 1120/709 authoring waves +
   the 1120-S ATS lane stay Ken-gated.
4. **Ken ratifications pending: 2 NEW (s72, non-blocking, recommendations
   filed)** — (a) 8867 face-fidelity leg (seed the Schedule-C row + the
   sub-question rows, extend the cascade); (b) the Schedule B "From
   Schedule(s) K-1" listing row so the payer rows foot to lines 2/6 in print
   AND XML. Standing non-decisions: 3115 OMB nit rides the next RS 3115
   amendment; the 8824 RS note rides the next 8824 touch.

## ▶ Waiting on Ken / external
1. **Ken's TaxWise forms list (S-22b)** — also answers the e-services
   software-package screens; the e-fileable-today sets were delivered in-chat
   s72 (1040: Sch 1/2/3/A/C/D/E/EIC/F/SE/8812 + 2441/3800/4835/6251/7206/
   7217/8283/8835/8862/8863/8911(+A)/8936(+A)/8995/8995-A(+D)/W-2/1099-R,
   NOW + Sch B + 8867; 1120-S: Sch B/D/K/K-1/L/M-1/M-2 + 1125-A/1125-E/4562/
   4797/8824/8825/8941/8949).
2. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
3. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
4. IdenTrust cert (⚠ 30-day download clock). 5. File-1018 Lacerte reprint (item 10).
6. PWA install check. 7. Density feel-check (s52).
8. s69 (no blocker): enter the firm's real CAF number + fax on the Preparer
   records (Admin → Preparers) so the 2848 L2 autofill carries them.
9. Demo Render service LIVE (https://tts-tax-demo.onrender.com, env=demo;
   login demo/delvio-demo). Remaining nit: GEMINI_API_KEY blank on the demo
   service (AI help dark in demos until Ken sets it).

## Active gates
- **Flow-assertion gate GREEN at 500** (unchanged from s71; s72 touched only
  the efile mapper — no compute code). Mirrors: 1120S 41 · 1065 39 (+4 s64
  staged) · 1040 397 export-verbatim (+1 staged: FA-1040-4835-06 pending the
  4562→4835 feeder).
- **s72 suites: MeF 1040 = 56 (Sch B + 8867 pure/structural + live-XSD
  2025v5.3 validation) · NEW test_efile_schb_8867_extract = 7 · Sch B
  render + intdiv + MeF = 104.** s70 suites unchanged (test_3115 39 ·
  manifest/acroform 201 · pair 36 · tsc 0 · vitest 300). Last full-suite
  GREEN = s54 `cd9b186`.
- Shared-DB deploy state: mig 0194 applied + seed 359 clean (s71); **push
  promptly after each session — the deployed Render code must carry the
  s71/s72 commits** (the s60/s63 mismatch-window recipe).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
