# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-04, eleventh session (**Proforma/rollover snapshot PRODUCER COMPLETE**
— `3a55f31`. The app-to-app snapshotter that closes the prior-year roll-forward loop: the roll
side was already built (migs 0105/0106) but nothing WROTE a 1040 snapshot, so it was a prod
no-op. Now a finalized 1040 freezes into a `PriorYearReturn(year=CY)` on DRAFT→FILED, and next
year rolls its carryforwards forward.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md` *(not read at boot)*; deferrals → `DEFERRAL_AUDIT.md`;
  open questions → `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.

## Active gate
- **1040 flow-assertion gate** — `cd server && pytest tests/test_flow_assertions.py -q`
  → **397 passed** (unchanged — the producer runs at finalize, touches no compute/flow). Fast (~1.3s, pure).
- **Proforma producer gate** — `cd server && pytest tests/test_proforma_producer.py -q --reuse-db`
  → **8 passed** (~2 min DB). Producer correctness (year-shifted carryforwards + demographics +
  deps + 8606 L14 + Roth next + Sch A facts), a key-CONTRACT drift guard, the full produce→roll
  round-trip, and guards.
- Test DB `test_postgres` CURRENT (migs through **0166**; NO new migration this session — the
  producer is pure read/write over existing models + a new `apps/returns/proforma.py`).

## ▶ RESUME HERE — idle; Ken directs the next unit
This session built the **proforma snapshot PRODUCER** (`server/apps/returns/proforma.py`,
`capture_prior_year_1040_snapshot`), wired at the 1040 DRAFT→FILED transition in `views.py`
`update_info` beside `capture_as_filed_baseline`. It closes the loop the read/roll side left open.

Per `SEASON_PLAN.md`, remaining runnable/near-term CC items:
- **4868 extension mapper** — **BLOCKED**: only `docs/mef/schemas/2026v1.0/4868_2026v1.0.zip`
  (TY2026) is on disk; there is **no 4868 in the ATS-active `2025v5.4` package**. Every built
  scenario is 2025v5.4, so 4868 has no matching TY2025 schema to validate against yet — that's
  the SOR pull to watch for.
- **TaxWise season-one 1040 importer** — the OTHER proforma producer path (imports prior-year
  1040s from TaxWise for season-one priors). Deliberately SEPARATE from this session's app-to-app
  snapshotter; rides the **October** TaxWise→Sherpa migration (needs the export format + sample).
- **1120-S / 7004 mappers** — blocked on the business-family schema pull (Ken's IRS inbox).

Ask Ken which to pick up. The whole 1040 ATS scenario set is built (S2/S3/S4/S5/S8/S12/S13; S1 dropped).

**Proforma producer — DONE this session (eleventh):**
- Source-format fork RESOLVED: built the **app-to-app snapshotter**, not the TaxWise importer
  (only cleanly-buildable option now; the importer rides Oct). See the `.claude` auto-memory
  `proforma-producer-deferred.md` (now marked COMPLETE) for the full design + rationale.
- **Producer owns the year-shift** — each carryforward stored as the amount carried TO NEXT year
  so the roll is a dumb copy: Sch D carryover-OUT `1040_SCHD_WS` clc_8 (ST)/clc_13 (LT); Form 4562
  line 13 = `Taxpayer.sec_179_carryover_next`; Form 8606 line 14 per owner via **reusing
  `compute_8606.owner_lines`** (no drift); next-year Roth opening basis via the `RothIRABasis`
  recovery formula; aggregate suspended PAL = Σ per-activity `passive_8582_suspended`.
- `_sr_py_*` block = THIS year's own Sch A facts (filing status, Sch A 5d/5e/17, 1040 L15,
  std-deduction override, itemized = L12>std, sales-tax election, AMT from SCH_2 line **2**) →
  next year's §111 state-refund prior-year inputs.
- Idempotent/safe: overwrites an app-produced snapshot on re-file; never clobbers an imported
  prior (`source_software` not `sherpa*`) unless `force`. `source_software="sherpa_proforma"`.
- ⚠️ v1 limits (flagged in the module docstring): PAL rolls at AGGREGATE granularity (matches the
  aggregate-only read side); `_sr_py_prior_refunded` + `_sr_py_unused_credits` have no clean CY
  source → absent. `_sr_py_amt` IS carried.
- Retires the Category-1 deferrals that only failed for lack of prior-year data (state-refund
  taxability, cap-loss carryover, §179, IRA basis) — once a return is finalized in Sherpa.

## ▶ Waiting on Ken (carried)
1. IFA-upload scenarios 8 (SIGNED) + 5 — REBUILD artifact sets first (pre-§12.5). **S2 + S3
   + S4 ready** (UNSIGNED, placeholder EFIN) — sign via
   `mef_build_ats_scenario2/3/4 --efin … --practitioner-pin … --taxpayer-pin …`.
2. SOR pulls: TY2025 1040 business rules · 2025v5.3 1040 schema · **TY2025 4868 schema** (unblocks
   the 4868 mapper — currently only TY2026 4868 is on disk).
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
- **Proforma v1 follow-ups** (flagged, non-blocking): per-activity PAL roll (v1 is aggregate);
  `_sr_py_prior_refunded`/`_sr_py_unused_credits` sourcing; the TaxWise season-one 1040 importer (Oct).

## Authoritative files read at boot
- `SPRINT_SCOPE.md` · `SEASON_PLAN.md` (dated Jan-2027 runway) · `SEASON_CHECKLIST.md` (tick-list) · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` · `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md` (when RS work on deck).
