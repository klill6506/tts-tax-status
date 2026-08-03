# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-02, session 189b (**the b011/b012 findings landed**
— two real engine gaps fixed (Schedule 8812 SE earned income; rental QBI
into Form 8995), the GA RIE Schedule C earned-income pull built on b012's
live proof, three new importable QBI fields, three HOLDs verified to full
ties. Migration 0232 applied; all bands green; pushed → deploys.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax`; demo = `tts-tax-demo`. **The orphan third service
`tts-tax-app` still fails every push — Ken should delete it.** Carried
open question: set `autoDeploy: false`?

---

## ▶ RESUME HERE

### Worker split (Ken, s184 — unchanged)
ChatGPT-browser = the HARD pile · Codex = the import lane (kickoff now
carries an **s189b addendum**) · **CC sessions = engine/tax-law work only.**

### s189b shipped (the b011 + b012 findings; migration 0232 applied)
- **① Schedule 8812 ACTC earned income (REAL, fixed)**: the Session-1
  proxy `deductible_se_half × 2` returns the SE TAX (~15% of earnings),
  not SE earnings — every SE-only return computed near-zero ACTC (b011:
  a Sch C HOH parent got $21 instead of $2,230; the QA's 6,971 = correct
  EIC 6,950 + that 21 — the delta factored before code was read). The
  8812 now shares the EIC engine's Earned Income Worksheet derivation
  (`earned_income_for_return`, new shared helper; combat pay always
  included per §24(d)(1) — no election, unlike EIC's). Spec fact
  `earned_income_for_actc` (SCH_8812 export, fetched) defines exactly
  this derivation.
- **② Rental QBI into Form 8995 (REAL, fixed)**: rentals had NO path in.
  `RentalProperty.qbi_trade_or_business` (migration 0232; the Form 4835
  field pattern — preparer-asserted §162 / Rev. Proc. 2019-38, default
  NOT QBI) now feeds both the simplified 8995 and the 8995-A gather:
  income in full, a LOSS at its Form 8582 ALLOWED portion (Reg.
  §1.199A-3(b)(1)(iv), the existing Sch C/F convention). ⚠ AHEAD of the
  RS 8995 spec (R-8995-QBI models Sch C rows only) — flagged, not a
  silent divergence: RS spec-corrections agenda.
- **③ GA RIE Schedule C/F earned income (the s189a REVIEW_QUEUE item —
  live proof arrived in b012 and it was built)**: the filed worksheet
  carries Schedule C **line 31 net profit** on L2 (not net of ½ SE —
  settled by the printed page, position-verified). The GA-500 pull now
  derives L2 from Sch C + Sch F net profit by `proprietor`. D_GA500_016
  clears on affected shells at recompute.
- **④ Import fields**: `qbi_loss_carryforward_prior` (8995 L3, NEGATIVE),
  `qbi_reit_ptp_income` (L6), `qbi_reit_ptp_carryforward_prior` (L7,
  NEGATIVE) + the rental QBI flag joined the allowlists; schema
  regenerated; guide gained rules 6–10 (incl. printed-37-includes-38 and
  the two-spouse-K-1 §199A owner rule).
- **⑤ Class scoreboard for the "GA earned-income family"**: of three
  same-family QA reports, only ONE was an engine gap — the other two were
  a missing itemize election (s189a) and owner-tag transcription.
- **⑥ Verified ties (CC dry-runs on the shared DB, staged batches
  `b011c3/b011c4-ccverify`, never committed)**: the SE-EIC/ACTC return
  (payload verbatim — engine only), the rental-QBI return (+ QBI flag,
  + expected 37 = the printed 18,711: line 37 includes the 728 penalty —
  the standing trap, bitten in transcription), and the owner-tag return
  (W-2s → spouse per TP|SP designators; div/capital → taxpayer per the
  worksheet columns). Exact corrections written into the HOLD files.
- Tests: commit band 27 (incl. the SE-EIC/ACTC end-to-end pin + two
  rental-QBI pins; fixture now seeds SCH_8812 — the s187 lesson again),
  RIE pull 30, 8812 scenarios 20 + render 3, wide sweep 708, flow 521 —
  all green.

### Next engine work — queue
**Ken's s189 lane-extension trio stands as ruled (s188 close), UNCHANGED:**
- ① Taxable state refund joins the lane · ② IRA deduction (Sch 1 L20)
  joins `sch1_fields` (GATE: RS spec line 20 input-typed) · ③
  `amt_medicare_wages_agg` editable browser-UI surface.
**Coverage gaps queued behind Ken's sequencing** (b012 triage, all HOLD):
Schedule F · 8889/HSA · 8606 · 8824 · 1099-MISC 8z lane extensions.
**Behind those (unchanged):** August GA unit (UET line-42 worksheet +
S4-8/S4-NB-18 NOL) · B002 row-creation family · MeF `build_irs5695` ·
year-constant ruling · Sch F / 8829 / 6198. SB 31 TY2026 military = RS W-item.
**RS agenda:** 8995 spec correction (rental rows) + R-EIC-WSB-SE (carried).

### Codex: resume state
Three b011 HOLDs re-stageable (two need the deploy; the owner-tag one
does not). b012's nine coverage-gap HOLDs stay held. The b012 QBI-
carryforward return was already filed and needed nothing.

---

## Known traps (carried — do not re-learn)
- **s189a/b: FACTOR THE DELTA INTO KNOWN CONSTANTS FIRST.** Five of six
  b010–b012 "engine defects" dissolved or localized by arithmetic before
  any code was read (std-vs-itemized · 22%×gap · 5.19%×base · 15%×(SE-tax
  −2,500) · the included penalty).
- **s189a: every packet page prints the PRIMARY SSN in its header** — the
  TP|SP designator column + the printed worksheet columns are the owner
  authority.
- **s189b: 1040 line 37 as printed INCLUDES the line-38 penalty** —
  now also a guide rule (8) with a pre-staging arithmetic check.
- **s188: the TaxWise invoice number is NOT the Delvio client number.**
- **s188: `amt_medicare_wages_agg` is a FALLBACK, not an override.**
- **s187: never import computed row columns** (staging rejects them).
- **s187/s189b: the backentry fixture must seed EVERY form the flow
  touches** (SCH_8812 was the latest miss — ACTC silently 0 in tests).
- **s186: a "reproduced engine defect" can be shell residue.**
- **s186: sch1_fields is for spec-typed INPUT lines only** (11/21).
- **s185: Sch A 5a is DERIVED; explicit 0 restores the derived path.**
- **s184: blank prior-year 2210 COMPUTES the 90% fallback.**
- **A correction payload must send explicit 0** — omit preserves.
- **`replace_documents` does NOT clear overrides or taxpayer scalars.**
- **The dry-run IS the commit, rolled back.**
- GA `LIC-NODEP` gates the whole LIC; set when GA prints 17a/17c.
- Flat `est_payment_q1..q4` drive 1040 line 26; dated
  `federal_estimated_payments` drive only the §6654 accrual.
- ⚠ PS 5.1: inline `python -c` fails silently — always exec a script file.

---

## Standing gates
- `git pull origin main` at session start; push with `git push origin HEAD:main`.
- Never `git stash`, never `checkout` mid-session.
- Rule Studio spec required before touching `compute*.py` / `renderer.py`
  (s189b fetched SCH_8812 + 8995 exports; the 8812 fix implements the
  spec's own fact note; the rental feeder is a FLAGGED spec-ahead).
- `pytest tests/test_flow_assertions.py` after any compute change (s189b: 521 green).
- Dev environment shares the **production** Supabase DB — every write is a
  production write. Migration 0232 is applied there.
