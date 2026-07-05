# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-04, tenth session (**S4 (Sarah Smith) MeF scenario + mapper leg
COMPLETE** — the LAST 1040 ATS scenario; 3 NEW mappers IRS8835 / IRS8936+IRS8936ScheduleA /
IRS3800; 8-doc submission live-XSD-valid; every engine line ALL MATCH). The S4 arc's three
feeder units (3800 + 8936 + 8835) AND the scenario are now all done — the whole 1040 ATS
scenario set (S2/S3/S4/S5/S8/S12/S13; S1 dropped) is built.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gate
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q`
  → **397 passed** (unchanged this session — the S4 leg touched only the MeF mapper/scenario
  layer, no compute). Fast (~1.3s, pure).
- **S4 compute gate** — `pytest tests/test_mef_scenario4_compute.py -q --reuse-db --timeout=1800`
  (the mgmt-command build already proved ALL MATCH + both XSDs valid; the pytest is the
  formal DB gate — first run hit the transient pooler "terminating connection due to
  administrator command" drop, re-running).
- Test DB `test_postgres` CURRENT (migs through **0166**; NO new migration this session —
  the S4 leg adds only read_model/builder/scenario/test code). Shared (prod) DB seeded
  through seed_8835/seed_3800/seed_8936.

## ▶ RESUME HERE — idle; Ken directs the next unit
The whole 1040 ATS scenario set is built (S2/S3/S4/S5/S8/S12/S13; S1 dropped). Per
`SEASON_PLAN.md`, remaining July CC items: **4868 extension mapper** (runnable now — needs
the TY2025 4868 schema package from SOR), **proforma/rollover snapshot PRODUCER** (read/roll
side already built, migs 0105/0106). The 1120-S/7004 mappers stay **blocked** on the
business-family schema pull (Ken's IRS inbox). Ask Ken which to pick up, or take 4868/proforma.

**S4 scenario + mapper leg — DONE this session (tenth):**
- 3 NEW mappers (all 1:1 vs the 2025v5.4 XSDs, verified via `manage.py mef_build_ats_scenario4`
  + `test_mef_scenario4_compute`):
  - `IRS8835` (one per RenewableFacility; Part I facility info + the Part II §45 chain;
    `build_irs8835` bridge-gates to `compute_8835.facility_lines`).
  - `IRS8936` + `IRS8936ScheduleA` (MAGI groups + the personal-use Part III group; the
    Schedule A New/PrevOwned/Commercial `<choice>` — v1 builds NEW only).
  - `IRS3800` (Part I/II face lines in schema order + the Part III current-year credit
    groups; `_F3800_LINE_ORDER` + `_F3800_P3_WRAPPER`).
- `read_model.py`: `F8835Source` / `F8936Source` + `CleanVehicleScheduleASource` /
  `F3800Source` + `F3800InflowRow` dataclasses; `_extract_f8835s` / `_extract_f8936` /
  `_extract_f3800`; wired into `extract_return` + `ReturnExtract`.
- `builder.py`: the 3 builders + ReturnData wiring at the verified positions (IRS3800 after
  IRS2441, IRS8835 after IRS8283, IRS8936+SchA after the 8911 block).
- `scenario4.py` (Sarah Smith) + `mef_build_ats_scenario4` + `test_mef_scenario4_compute.py`;
  the "Transfer Election Statement" reportlab PDF rides as `BinaryAttachment1`.
- **THREE documented divergences from the stale key** (README + module docstring): 8835 →
  Form 3800 line **4e** (SPECIFIED, the 4-yr window) NOT the key's 1f/transfer-column-f;
  the 8936 vehicle modeled **PERSONAL** (Sch 3 6f) not the key's biz→1y=130; §6417/§6418
  transfer mechanics OUT of scope (Ken 3800 J4 — no header B entry). The whole $13,200 §45
  credit carries forward (line 38 = 0, Sch 3 6a blank); refund 4,581.
- Engine pins ALL MATCH: wages 36,014 · AGI 36,014 · taxable 20,264 · tax **2,195** ·
  Sch 3 6f 2,195 · total tax 0 · refund 4,581 · 8835 line 15 13,200 · 3800 P3-4e 13,200 /
  line 38 0. submission.xml (8 docs) + manifest validate vs 2025v5.4.

## ▶ Waiting on Ken (carried)
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5). **S2 + S3
   + S4 ready** (UNSIGNED, placeholder EFIN) — sign via
   `mef_build_ats_scenario2/3/4 --efin … --practitioner-pin … --taxpayer-pin …`.
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · **TY2025 4868 schema** (the
   4868 mapper is the next runnable CC item once this lands).
3. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
4. Preparer Manager visual-review batch (carried): Sch A line-11 dotted literal + qualified-
   contributions input; Clean Vehicles (8936) + Business Credit (3800) + Renewable
   Electricity (8835) tabs + the rendered 8936/SchA/3800/8835 faces + the 3800 carryforward
   statement.

## ▶ RS follow-ups (carried, small)
- FA-1040-8911-04's RS-side description still says "Form 3800 unbuilt" — refresh the text.
- RS 8936 spec: add the FACE-verified transfer stop (R-8936-TRANSFER) + D_8936_004
  wrong-year-PIS note (DEFERRAL_AUDIT eighth session, parts 1-2).
- RS 3800 J4: §6417/§6418 transfer treatment stays RED-deferred (D_3800_004) — S4 documents
  the divergence (key routes to 1f/transfer column f; engine treats own-production → 4e).

## ▶ Carryover follow-up (older, still open)
- Next RS Schedule D touch — (a) add `D_8949_006` to the 8949 spec; (b) consider an FA for
  the 1a/8a aggregate netting.
- RS SCHA spec fact for `scha_qualified_contributions_cash` (input-only) — amend by lookup.
- RS 8995 sibling loader's D_8995_001 retirement note (DEFERRAL_AUDIT third-session item 2).
- 1065 beyond the SE unit resumes only on Ken's explicit direction — `1065_status_assessment.md`.
- Minor parked sweep: 1065-section selects non-key uncontrolled; legacy `boxCheckbox` (pre-existing).
- EIN/ZIP re-import stays PARKED.

## Authoritative files read at boot
- `SPRINT_SCOPE.md` · `SEASON_PLAN.md` (dated Jan-2027 runway) · `SEASON_CHECKLIST.md` (tick-list) · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` · `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md` (when RS work on deck).
