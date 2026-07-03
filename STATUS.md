# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-03, second session (**S2 RETURN-LEVEL ARMS — BOTH UNITS COMPLETE, flow
gate 380→381.** Unit A `86d4e38`: deceased-spouse header (dates f1_05-10 + c1_3, Pub 4164 §12.5
DECD name line, i1040 "Filing as surviving spouse" literal via the NEW `AcroField.abs_pos`
overlay), NRA-spouse §6013(g)/(h) election (mig 0155), ReturnHeader IP PINs, line-26
`divorcedSpouseSSN` ATTRIBUTE, BinaryAttachment document + count. Unit B: **Ken RULED the EIC
opt-out 27c "skip-entirely, ACTC-sibling"** — RS 1040_EIC amended `e64dbea` (fact + R-EIC-27A
gate + D_EIC_017 + EIC-G6 + FA-1040-EIC-10), seeded + export-verified + canonical refreshed;
tts mig 0156, the EXPLICIT line-27 clear, Schedule EIC suppression, c2_13, DoNotClaimEICInd.
**STALE-LIST CORRECTIONS:** statutory-employee Sch C was ALREADY BUILT (W-2 Unit 2, 2026-06-15);
former-spouse SSN needs NO statement (XSD attribute). The carried 1065 unrecap-§1250 item
RESOLVED — the parallel session committed its own fix `f23dc54`; its migration 0157 was
UNAPPLIED on the shared DB and is now applied.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gate
- **1040 flow-assertion gate: 381** (380 + FA-1040-EIC-10; RS FlowAssertions 382 incl. non-1040).
  Run: `cd server && pytest tests/test_flow_assertions.py -q` — **381 passed this session.**
- **This session's verification:** efile pure 29/29 ×2 (incl. LIVE XSD validation of the full S2
  identity-arm set + the 27c Ind) · header-render DB 5/5 · topic7 compute+diagnostics DB green
  (one 42-min pooler-sick run produced 15 fixture ERRORs — the documented connection-kill class,
  all re-ran green — plus 3 REAL test-shape fixes: the D_EIC family pin 16→17, the flip test's
  cached-taxpayer mutation, the `_fires` handler collision, and the line-33 wiring test made
  seed-state-robust) · field-map audit 2/2 · vitest 275 · tsc 0 · `manage.py check` clean.
- Migrations **0155 (nra_spouse_election) + 0156 (eic_opt_out) applied** to the shared DB, plus
  the parallel session's **0157 (section_1245_exception)** which was sitting unapplied.

## ▶ RESUME HERE — the S2 (Jones) SCENARIO + MAPPER leg
All return-level arms are now built. The scenario leg needs:
1. **ONE Ken question first:** the scenario says the Joneses are ag-co-op patrons who "do not
   qualify" for QBI, but the engine may compute QBI on the statutory-employee Sch C net profit
   (§199A patron reduction = the stated 8995-A RED-defer). Resolve before pinning (enacted-law
   divergence vs a patron arm).
2. **Fill forensics:** the scenario PDF's filled values use a +29 char-shift font (face −29);
   checkboxes are VECTOR marks invisible to text extraction — use `page.get_drawings()` or pin
   from the answer key. Decoder + full fill notes → the auto-memory
   [[s2-identity-arms-and-27c]] (facts: John statutory W-2 Southwest 29,513 → Sch C Furniture
   Sales 449110; Judy Target 8,513, DOD 9/11/2025; Jacob ODC; Sch A 5a=1,028, 11=250, 12=700).
3. **NRA statement PDF:** generate in `ats/scenario2.py`, ride `extract.binary_attachments` +
   `SubmissionContent.attachments` (both plumbed this session).
After S2: S3 (4835) → S4 (3800/8835/8936 — the OBBBA-sunset check rides each spec leg).
Also runnable now: 4868 mapper; proforma snapshot PRODUCER; GA Schedule 1 detail-page render;
packet-order pair-gluing.

**Waiting on Ken (carried):**
1. IFA-upload scenarios 8 (SIGNED) + 5, bring back the acks — **REBUILD both artifact sets
   first** (pre-date the Pub 4164 §12.5 filer-name fix; 12+13 are current).
2. SOR pulls: TY2025 1040 business rules (PDF+CSV) · the 2025v5.3 1040 schema package · the
   TY2025 4868 schema package.
3. Watch the IRS inbox for the business-family access notice (blocks the 1120-S + 7004 mappers).
4. Preparer Manager one-time setup + bulk-assign backfill + the visual-review batch (carried:
   bulk-assign UX; GA-500 Sch 1 tips/OT block; theme/sort per login; the 8812-tab election
   card + Taxpayer-Info header block; the form_8911 tab; the form_7217 tab; the Form 8283
   section on the Schedule A tab; **NEW: the NRA-election checkbox on Taxpayer Info; the EIC
   opt-out checkbox on the EIC tab; the surviving-spouse literal position (page 2 signature
   area) — visual check**).

## ▶ Open questions (REVIEW_QUEUE)
- None open. (The 27c ruling is RESOLVED — Ken chose skip-entirely; see REVIEW_QUEUE Resolved
  2026-07-03. The S2 QBI-patron question is the scenario leg's FIRST action, not yet queued.)

## ▶ Carryover follow-up (older, still open)
- 1065 beyond the SE unit resumes only on Ken's explicit direction — `1065_status_assessment.md`.
- Minor parked sweep: 1065-section selects non-key uncontrolled; legacy `boxCheckbox` (pre-existing).
- EIN/ZIP re-import stays PARKED.

## Protocol change (2026-07-03, third session)
- **Session-close checklist + status mirror ADOPTED** (Ken's directive, via chat): boot now
  starts with `git pull`; close now updates `SEASON_CHECKLIST.md` (tick `- [x] … — date SHA`,
  NOW-WORKING-ON line, ⚠ for unplanned work) and ends with `scripts\sync_status_mirror.ps1`
  (five planning files → the PUBLIC `tts-tax-status` mirror at `D:\dev\tts-tax-status`;
  PII guard built in). Full protocol → CLAUDE.md Session Boot / Session End.
- ⚠ ONE-TIME Ken step outstanding: create the **public GitHub repo `klill6506/tts-tax-status`**
  (empty, no README) — until then the mirror push fails (local mirror repo is ready).

## PAUSED IN-FLIGHT (2026-07-03, third session — resume this first)
- **`server/tests/test_4797_pipeline_leg.py` has an UNCOMMITTED working-tree change** — the
  parallel session's K9c pass-through test (companion to its committed fix `f23fc54`; adds
  `TestPipelineK9cPassThrough` + `_partner`/`_k1_by_partner` helpers). Ken OK'd committing it
  here, gated on a green run. **Two runs went pooler-sick** (the documented connection-kill
  class: run 2 = `EEE.........` then pytest-timeout on a psycopg wait — 3 fixture ERRORs,
  9 passes, no real failures seen). A third background run was in flight when Ken paused;
  its result was NOT collected. **Next session:** re-run
  `pytest tests/test_4797_pipeline_leg.py -q --reuse-db` (venv python directly, from server/;
  be patient 20+ min) — green → commit JUST that file + push; connection-sick again or real
  failures → leave uncommitted for the parallel session (Ken's stated alternative).

## Authoritative files read at boot
- `SPRINT_SCOPE.md` · `SEASON_PLAN.md` (dated Jan-2027 runway) · `SEASON_CHECKLIST.md` (tick-list) · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` · `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md` (when RS work on deck).
