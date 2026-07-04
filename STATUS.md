# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-04, seventh session (**S3 (HEATHER) SCENARIO + MAPPER LEG COMPLETE** —
4 new MeF mappers `IRS1040ScheduleD`/`IRS1040ScheduleE`/`IRS1040ScheduleF`/`IRS4835` +
the Schedule D **1a/8a aggregate compute wire** (spec R-SCHD-L7-L15 already said "combine
1a..6 / 8a..14"; the "UNUSED v1" boundary retired: netting + engagement + computed 1a_h/8a_h
+ field-map widgets Row1a/Row8a + seed labels), `scenario3.py` + `mef_build_ats_scenario3` +
`tests/test_mef_scenario3_compute.py`. **11-doc submission live-XSD-valid (2025v5.4); every
pin engine-verified ALL MATCH** — income 72,235 · AGI 71,821 · 13a 567.40 (FFV cents; XML
567) · TI 55,504 · L16 6,328 (QDCGT) · SE 827 (farm optional method — the existing SE
builder already emitted 4b/15, verified no change) · owe 3,750. Artifacts **UNSIGNED** in
`docs/mef/ats_out/scenario3` (placeholder EFIN — Ken signs). No migration.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gate
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q`
  (**HELD**: combined flow + acroform maps + the S3 scenario file = **440 passed**, reuse-db
  16m; one test-shape fix — engaged Sch D writes the 8949 box-pair rows as "0.00", the
  no-8949 assertion now accepts zero).
- **S3 verification:** `mef_build_ats_scenario3` ALL MATCH (1040/Sch1/Sch2/SchD/SchE FFV pins +
  SchF/4835/SE pure-chain pins + 8995-A XML pins) · pure efile 50/50 · live-XSD submission +
  manifest [OK].
- Test DB `test_postgres` CURRENT (migs through 0161; this session added none).

## ▶ RESUME HERE — S4 (3800/8835/8936 + Schedule A scenario) or Ken directs
S3 is fully built. **S4 needs its forms first**: build **8835 + 8936** (RS specs authored,
cached in `server/specs/` — spec-first units per form: model/compute/diagnostics/render) and
**Schedule A is already built**; only then the S4 scenario (a credit-LIMITATION test — see the
8835/8936 authoring notes + test vectors; ⚠ SEASON_PLAN appendix-4 OBBBA-sunset check at each
spec leg). 3800 has a spec. Alternatively Ken may reprioritize (July queue: proforma producer,
4868 mapper — needs the TY2025 schema from SOR).

**S3 key-forensics corrections this session (analysis note updated in place):**
Sch F's 2,970 = line 26 **Seeds and plants** (not supplies) · PEC "You" checked ·
Sch E questions A/B both YES (Sch F line F = No) · line 38 blank + no prior-year facts →
the scenario elects **IRS-figures-the-penalty** (i1040 line 38; FFV override — compute_2210
has NO §6654(e)(2) zero-prior-year exemption, a stated boundary in DEFERRAL_AUDIT).
⚠️ Recipe upgrade: **render key pages to PNG and visually verify** — the S3 Sch F fills were
invisible to both ±29 text decoders.

**New stated e-file boundaries (DEFERRAL_AUDIT seventh-session block):** Sch D XML = the
1a/8a aggregate path only (8949 box totals → raise; IRS8949 doc mapper unbuilt) · Sch E XML =
Part V only (Parts I-IV raise) · Sch F accrual/passive raise · 4835 34c signed emit + PAL
literal unexercised (verify on a loss-year run) · SCHEDULE_D seed labels re-run on next deploy.

**Waiting on Ken (carried):**
1. **Author RS specs — DONE for 4835/8835/8936** (cached); build 8835 + 8936 units for S4.
2. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5); **S2 + S3
   ready** — sign via `mef_build_ats_scenario2/3 --efin … --practitioner-pin … --taxpayer-pin …`.
3. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · TY2025 4868 schema.
4. Watch the IRS inbox for the business-family access notice (blocks 1120-S + 7004 mappers).
5. Preparer Manager visual-review batch (carried in STATUS_ARCHIVE): the Sch A line-11
   dotted-literal position + the "…of which qualified contributions" FormEditor input.

## ▶ Carryover follow-up (older, still open)
- **NEW:** on the next RS Schedule D touch — (a) add `D_8949_006` to the 8949 spec (app-level
  confirm gate, in code since `c25635f`); (b) consider an FA for the 1a/8a aggregate netting
  (covered today by the scenario tests only, no RS FA export).
- RS SCHA spec fact for `scha_qualified_contributions_cash` (input-only) — amend by lookup on
  the next Schedule A touch.
- RS 8995 sibling loader's D_8995_001 retirement note (DEFERRAL_AUDIT third-session item 2).
- 1065 beyond the SE unit resumes only on Ken's explicit direction — `1065_status_assessment.md`.
- Minor parked sweep: 1065-section selects non-key uncontrolled; legacy `boxCheckbox` (pre-existing).
- EIN/ZIP re-import stays PARKED.

## Authoritative files read at boot
- `SPRINT_SCOPE.md` · `SEASON_PLAN.md` (dated Jan-2027 runway) · `SEASON_CHECKLIST.md` (tick-list) · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` · `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md` (when RS work on deck).
