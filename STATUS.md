# TTS Tax App - STATUS (current state only)

*Last updated: 2026-08-02, session 181 (**import lane batches 003 + 004 RUN
ON PROD: 19 more returns committed, 14 tied to the dollar and FILED** —
lane running total: 39 imported, 33 filed. Batch-005 was interrupted by
the account's USAGE-CREDIT limit mid-authoring (3 of 10 payloads done).
Zero code changes this session — every miss was payload-fixed, data-fixed,
or held to REVIEW_QUEUE per Ken's standing order. Deploy debt: ZERO.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — finish Ken's standing order (2026-08-02): batches 005
## and 006 remain of the READY_NOW list. BLOCKED ON USAGE CREDITS —
## authoring subagents died mid-batch-005 ("out of usage credits").
##
## When credits are back, the loop continues exactly as proven tonight:
##  - batch-005: 3 payloads ALREADY AUTHORED + validated conventions
##    (`D:\tax-test-data\tmp\batch-005\`: johnson-dennis · loggans ·
##    lasalle). 7 still to author: JOHNSON DANIEL #2700 · KINCAID JOSHUA
##    #2795 · KINCAID MARY #2796 · KING #2803 · LINN #2945 · LUFFLER
##    #2999 · MAGANA #3026. Then stage/dryrun/commit/markfiled/
##    diagnostics/report, PDFs → Done.
##  - batch-006 (all 10 to author): MARTIN LANCEY #3083 · MARTIN TEMPIE
##    #3086 · MASHBURN #3092 · MCCARTHY #3130 · MCELHANNON BREANNA #3157
##    · MCELHANNON HAROLD #3159 · MELTZ #3220 · MOODY PEGGY #3293 ·
##    MOODY RONALD #3294 · MURRAY #3364.
##  - Full client_number map: `tmp\pilot-001\batch003_resolution.json`.
##  - ⚠ LOGGANS will no-tie on EIC (+255) until the eic_valid_ssn
##    allowlist build lands (REVIEW_QUEUE, Ken's call) and on GA by the
##    BARROW $5 LIC family. Expect and hold, or wait for the builds.
##  - Driver session was logged out; re-mint (`mint_magic_link.py
##    --prod-db`) + `pilot_driver.py auth` before staging.

**Batch-003 results (batch `batch-003` on prod, 2026-08-02):**
- **9 authored, 9 committed, 7 TIED + FILED** (#1807, #1929, #1996, #1997*,
  #2010, #2051*, #2158, #2171, #2249 — * = held). Shapes: code-6 1035
  exchange (first in lane, exact), 8× 1099-R senior, 8949 boxes B/D/E,
  UNKNOWN-payer INT, 2210 penalties computed EXACT three times (73/263/90).
- **2 committed HELD**: #1997 GA 45 −47 = the known GA UET gap (s178
  line-dispute, engine can't compute GA UET); #2051 fed 37/38 −20 (est
  payment DATE not carryable — mid-year payment) + GA UET −10.
- **1 refused by design**: COMPTON #1738 — client-copy print set, 1099-R
  detail pages absent (REVIEW_QUEUE; notes inventory what's ready).
- **CRIM #1807 needed a data fix, then tied 17/17**: her shell carried a
  pre-s176 UI workaround override (Sch 1 8z = 27,679 "CODE 6 UNSUPPORTED")
  that double-counted once the engine computed code 6 correctly — cleared
  via `fix_crim_8z.py` (s173's lesson in data form; REVIEW_QUEUE FYI).

**Batch-004 results (batches `batch-004`/`-004h` on prod, same night):**
- **10 authored, 10 committed, 7 TIED + FILED** (#2274, #2277, #2315,
  #2334, #2336, #2341, #2448). Shapes: HOH non-dependent qualifying child,
  QCD-checkbox return, S1-10 US-obligation subtraction (NEW convention,
  found+fixed pre-staging on FLOYD, used by GILL to the dollar), 8949
  wash-sale code W.
- **3 committed HELD (all REVIEW_QUEUE, precise deltas):**
  - #2623 HYDER: EIC −2,303 exactly — `eic_valid_ssn_taxpayer` (compute_eic
    Rule 2 / D_EIC_016) has NO backentry field; correction b004h added
    dependent tin_type, still gated. **Proposed build: allowlist the two
    filer EIC-SSN booleans (tin_type precedent).**
  - #2434 HARPE: fed tax −118 on IDENTICAL taxable income (QDCGT-worksheet
    divergence suspect; first qual-div + CGD + 8949 mixed shape).
  - #2464 HAWKS: GA 16 +461 ≈ 5.19% × (captx 3,920 + CGD 4,971) — first
    65+ RIE return WITH capital gains; RIE-includes-capgains question.
    Plus fed 38 +63 (2210 PY facts not entered — check packet p.?).
- Post-commit diagnostics ran on ALL 19 committed clients both batches; only
  known-class findings (D_8867_001 questionnaire gap on credit/HOH returns ×3,
  D_EIC_016 on HYDER, W-2 box-3/5-not-printed variance warnings, 2210/UET
  prior-year info). Reports: `tmp\batch-003\batch003_report.json`,
  `tmp\batch-004\batch004_report.json` + `batch004h_report.json`.

**Conventions earned this session (AUTHORING_GUIDE updated in place):**
- **GA Sch 1 direct-entry subtractions via ga500_fields** (`"S1-10"` US
  obligations etc.) — a printed S1-10 left out overstates GA tax by
  5.19% × amount (FLOYD).
- Guide gaps fixed pre-launch: car_loan_vehicles section was MISSING
  (STATUS's "guide carries every convention" claim was false), hard-rules
  still said Sch D unsupported (would have refused all capgains packets),
  locator example still ssn+last_name.
- DICKERSON JAMES L: NO hub client at all (REVIEW_QUEUE — not FINLEY
  ambiguity, the client simply isn't in the hub).

**Ken-blocked residuals (do NOT self-serve):** BARROW GA LIC ruling ·
FINLEY which-is-which · DICKERSON missing client · COMPTON re-export ·
EIC filer-SSN allowlist build · HARPE QDCGT question · HAWKS RIE question ·
174 invoice-only re-exports. Next BUILD era (with Ken): Sch C + SE, then
depreciation import, Sch E, Sch A.

## QA Batch 002 — remaining queue (unchanged from s179; paused behind the
## lane; ChatGPT Batch-003 adds overlapping items 6/7 = family #1)
1. Row-creation transactional family (add-lanes minting silent duplicates).
2. Rental `+ Add property` silently inert (blocker-class).
3. Diagnostics-staleness SCREEN-side fix (report side served by Leg D).
4. K-1 material participation defaults to unsupported YES.
5. Diagnostic gating: D_6251_008 / D_GA500_008.
6. GA retirement-exclusion feeders (Sch C earned income + Sch D gains) —
   note: HAWKS (above) may be this family's capgains half, live on prod.
7. Form 8995 prior-year QBI loss carryforward inputs.
8. Schedule A Medicare feeder provenance.
9. Form 8960 filed PDF omits pure-arithmetic lines.
10. Source-summary INT box-1 400 unwired.
11. PDF preview canvas race.
12. Stale totals strip on Slate INT/DIV screens.

## Active gates
- **Deploy debt: ZERO** — no code changes this session; `main` == deployed
  `facc4d6` era. All misses were payload/data/held.
- **RS agenda (unchanged)**: retirement-exclusion input; R-RET-CODE code-6
  edit; check_ga500_integrity 7c→7a scenario edit.
- **REVIEW_QUEUE**: 5 NEW s181 items + BARROW + GA UET dispute + FINLEY.
- ⚠ `LEDGER_AUTOPOST_ENABLED` stays unset until production cutover (Jan 2027).
- ⚠ One test DB — never overlap pytest runs.
- ⚠ ChatGPT works the SAME Inbox alphabetically from the top.

## 🔑 Method notes (s181 additions)
1. **A non-virgin shell's overrides survive replace_documents** — replace
   deletes payload-supplied document sections only; stale workaround
   OVERRIDES (CRIM 8z) and prior EIC answers (CHANELL) persist and can
   flip a reconciliation either way. Probe overrides on any non-virgin
   no-tie first.
2. **The batch-002 "CHANELL EIC exact" proof was shell-dependent** — her
   started shell carried the EIC answers; a virgin shell fails Rule 2.
   A green result can be proving the leftover data, not the payload.
3. Diagnostics runs POST to `/api/v1/diagnostic-runs/run/` with the
   TaxYear UUID (`run_diagnostics.py` next to the driver).
4. ⚠ Subagent fleets die silently on usage-credit exhaustion — check for
   partial payload files before assuming a batch dir is complete.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. **The app is TESTING until January 2027.**

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
- **Slate lane:** `Design/SLATE_IMPLEMENTATION_PLAN.md` ·
  `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md`.
