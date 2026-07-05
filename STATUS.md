# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-05, fourteenth session (**GA-500 Schedule 1 render leg**): rendered the
3-page GA Form 500 Schedule 1 (Adjustments grid + RIE worksheet + Military RIE worksheet) — new
flat template `fga500_schedule1` extracted from the GA-DOR packet + coordinate overlay, appended
after the Form-500 face. Ken's requested "GA-500 OT/tips exclusion" was ALREADY BUILT (2026-07-02,
`ga500-hb463-tips-ot-complete`) — a stale-tracker resurrection, now corrected; Ken redirected to
the render leg. ⚠️ Surfaced (NOT fixed) an OPEN military-exclusion over-exclusion compute bug —
see "Waiting on Ken".*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gates (all green)
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q`
  → **398 passed** (unchanged — render leg touches no compute/flow). Fast (~1.3s, pure).
- **GA-500 render leg** — `pytest tests/test_ga500_render_leg.py -q` → **9 pure + 4 DB passed**
  (DB ~2:47). Adds the Schedule 1 structure/manifest tests + 3 DB tests (absent / grid-only /
  all-3-pages incl. the tips-fold), plus the existing 5-page-face test still green.
- **Pure MeF 1040 suite** — `pytest tests/test_efile_mef_1040.py -q` → **40 passed** (thirteenth
  session; unchanged).
- `manage.py check` clean. Test DB `test_postgres` still needs **mig 0167** on its next fresh build
  (help_text-only; applies on the next migrate). No new migration this session (render-only).

## ▶ RESUME HERE — idle; Ken directs the next unit (military-bug decision pending)
This session (fourteenth) built the **GA-500 Schedule 1 render leg** (Ken's pick after the OT/tips
unit turned out already-built). The S1-* adjustment lines previously reached the printed return only
via the Form-500 line-9 net; now the full 3-page Schedule 1 renders.

**What was built (render-only, no compute change):** the Schedule 1 template wasn't in the repo
(`ga500.pdf` = the 5-page main return only). Downloaded the GA-DOR blank-form-printing packet (27
pages), extracted Schedule 1 = source pages 5-7 → NEW flat template
`resources/state_forms/GA/2025/ga500_schedule1.pdf` (manifest `fga500_schedule1` / `GA-500-SCH1`,
sha `fbbf7128…`). NEW `coordinates/fga500_schedule1.py` (namespaced keys P1-*/P2-{TP,SP}-*/
P3-{TP,SP}-* + SSN comb, registered in `COORDINATE_REGISTRY`) + `render_ga500_schedule1()` in
`renderer.py`, appended after the face inside `render_ga500`. Ken ruled (AskUserQuestion): **all 3
pages** (grid + RIE worksheet + military worksheet) + **tips/OT fold into printed line 12** (the
2025 form has no 12a/12b cell → render L12 = S1-12+S1-12a+S1-12b; exact on the TY2025 bed). Attach
only when adjustments exist; p2 only if RIE applies, p3 only if military. All 3 pages visually
calibrated (baseline = box-bottom + 2pt via get_drawings "re"). Gates: 9 pure + 4 DB render tests,
flow 398 held, `manage.py check` clean.

**🔴 OPEN — needs Ken's ruling (see "Waiting on Ken" #1):** while rendering page 3 I found
`compute_ga500.mil()` **over-excludes** military retirement of $17,501–$34,999 (GA earned ≥ $17.5k):
it returns `l3 + l8` = `min(mret,17500)+min(mret,17500)`, which can exceed the income received.
IT-511-verified correct = `min(mret, 35000)`. Untested (T5-military uses $40k, above the cap). I did
NOT touch compute (tax law = Ken's). Fix now or defer?

Per `SEASON_PLAN.md`, remaining runnable/near-term CC items (unchanged): **4868 mapper** BLOCKED (no
TY2025 4868 schema on disk — SOR pull); **1120-S/7004 mappers** BLOCKED (business-family schema pull —
Ken's IRS inbox); **TaxWise season-one 1040 importer** = October. **No unblocked build candidate
remains** — the previously-listed **GA-500 OT/tips exclusion** was ALREADY BUILT 2026-07-02 (tag
`ga500-hb463-tips-ot-complete`, `140bf87`; §48-7-27(a)(16)/(17), S1-12a/12b, per-taxpayer $1,750,
TY2026-2028 window; ★★ UNIT COMPLETE in `form_coverage_tracker.md` line 544). The prior "not-yet-started"
note was a stale-tracker resurrection (fourteenth-session verify: flow gate 398 green on current code).
Its only open follow-up (re-verify the per-taxpayer cap at the 2026 IT-511; DEFERRAL_AUDIT item 4) is
NOT actionable until DOR publishes the 2026 IT-511 / Form 500 face (~late 2026).

Ask Ken which to pick up. The whole 1040 ATS scenario set is built (S2/S3/S4/S5/S8/S12/S13; S1 dropped).

## ▶ Waiting on Ken
0. **🔴 NEW — GA-500 military RIE over-exclusion: fix now or defer?** `compute_ga500.mil()` returns
   `l3 + l8` (both `min(mret,17500)`) → over-excludes military retirement of **$17,501–$34,999** with
   GA earned ≥ $17,500 (e.g. $20k military → excludes $35k). **IT-511-verified** (p21-22 + Form 500
   Sch 1 p3): correct = `min(mret, 35000)` (proceed) / `min(mret, 17500)` (STOP). Fix = the proceed
   branch → `min(mret, 2*base)` + a mid-range test + re-check the RS GA-500 spec first. NOT touched
   (tax-law = Ken's). Full context: REVIEW_QUEUE Open + `.claude` memory. Doesn't affect the render.

## ▶ Waiting on Ken (carried)
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5). **S2 + S3 + S4
   ready** (UNSIGNED, placeholder EFIN) — sign via
   `mef_build_ats_scenario2/3/4 --efin … --practitioner-pin … --taxpayer-pin …`.
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · **TY2025 4868 schema** (unblocks the
   4868 mapper — currently only TY2026 4868 is on disk).
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
4. Preparer Manager visual-review batch (carried): Sch A line-11 dotted literal + qualified-
   contributions input; Clean Vehicles (8936) + Business Credit (3800) + Renewable Electricity (8835)
   tabs + the rendered 8936/SchA/3800/8835 faces + the 3800 carryforward statement.

## ▶ RS follow-ups (rides a dedicated RS session — parallel RS work may be uncommitted)
- **NEW this session:** `8867_spec.json` — R-8867-RENDER + `f8867_claims_aotc` notes + `D_8867_002` say
  "Form 8863 not built → AOTC RED"; STALE (8863 built, D_8867_002 retired). Refresh text to "AOTC derived
  from Form 8863 (line 7 > 0)" + mark D_8867_002 retired. Notes-only, no tax-law change.
- Carried: RS Schedule D — add `D_8949_006` to the 8949 spec; consider an FA for the 1a/8a aggregate
  netting. RS SCHA spec fact for `scha_qualified_contributions_cash` (input-only; amend by lookup).
  RS 8995 sibling loader's D_8995_001 retirement note. (The six prior handoff items were applied by the
  RS session 2026-07-04 — see the archive.)

## ▶ Carryover follow-up (older, still open)
- 1065 beyond the SE unit resumes only on Ken's explicit direction — `1065_status_assessment.md`.
- Minor parked sweep: 1065-section selects non-key uncontrolled; legacy `boxCheckbox` (pre-existing).
- EIN/ZIP re-import stays PARKED.
- **Proforma v1 follow-ups** (flagged, non-blocking): per-activity PAL roll (v1 is aggregate);
  `_sr_py_prior_refunded`/`_sr_py_unused_credits` sourcing; the TaxWise season-one 1040 importer (Oct).

## Authoritative files read at boot
- `SPRINT_SCOPE.md` · `SEASON_PLAN.md` (dated Jan-2027 runway) · `SEASON_CHECKLIST.md` (tick-list) · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` · `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md` (when RS work on deck).
