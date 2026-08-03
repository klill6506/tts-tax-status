# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-03, session 191 (**the b027 findings landed** — the
Inbox-cleanup gate is now a server endpoint (four Done gates + fresh
diagnostics + source-verified warning acks + the closeout report), Form
8867 due-diligence answers import per question (`f8867_fields`, clears
D_8867_001 on the four held returns), schema + handoff guide regenerated.
No migration. A stale s172-era EIC registry pin also caught and fixed.)*

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
ChatGPT-browser = the HARD pile · Codex = the import lane · **CC sessions
= engine/tax-law work only.**

### s191 shipped (the b027 findings)
- **① Inbox-cleanup gate (b027 items 1/3/4)**: NEW
  `POST /api/v1/backentry/cleanup/` (≤10 packets, cross-batch). Per
  packet: batch + reconciliation verdict, live return status, per-family
  persistence counts (committed vs live), and a **fresh diagnostics run
  made by the call itself** with error/warning rule IDs. The four Done
  gates: TIE · filed · committed families persist · zero error-severity
  findings. Any failure → `eligible:false` + named gates + rule IDs; the
  packet stays in Inbox. WARNING findings block only until
  `source_verified:true` records the reviewer's acknowledgment — through
  the EXISTING DiagnosticAcknowledgment mechanism (fingerprint-keyed,
  re-alarms when the numbers change; errors never acknowledgable, same as
  the UI). The response IS the closeout report (`moved_to_done` /
  `held_in_inbox`). The server cannot move Ken's local PDFs — the operator
  moves the eligible list; the "batch cleanup screen" ask is satisfied
  API-side; a browser screen is Ken's call (queued below).
- **② Form 8867 import (b027 item 2)**: NEW `f8867_fields` payload
  section — per-question transcription of the packet's filed 8867
  (`"1"`–`"15"` + subs + `"5_docs"`; true/false, `"na"` only on the seven
  printed-N/A lines — widget-truth-set enforced at staging). Written as
  MANUAL (overridden) entries so the un-attested compute cascade preserves
  them; `preparer_due_diligence_attested` stays non-importable BY DESIGN
  (the attestation cascade would overwrite the transcription — a
  commit warning fires on attested-shell residue). Omit-not-guess enforced:
  blank questions are omitted, `"na"` outside the N/A set is a staging
  error. Clears D_8867_001 (the four b027 held returns).
- **③ Stale pin caught**: `test_full_eic_family_rules_registered` pinned
  the EIC family at 001–017; D_EIC_018 (Ken's s172 ruling, 7/30) was never
  added. Stale since 7/31 — this module isn't in the usual bands.
- Schema (`batch-import.schema.json`) regenerated; handoff guide gained
  the 8867 bullet (§4) + the cleanup section (§12b).
- Tests: NEW `test_backentry_cleanup.py` 6 (incl. the b027 scenario
  end-to-end: D_8867_001 error holds → f8867 correction batch → eligible;
  warning-ack recorded; errors never ackable via the crashed-rule path),
  commit band 32 (f8867 land/validate/attested-warn), flow 521,
  reconcile+markfiled 18, topic7 29 — all green. **No migration.**

### Codex: resume state
Re-stage the four D_8867_001 holds as correction batches (new keys) with
`f8867_fields` transcribed from each packet's filed 8867, then run the
cleanup endpoint per handoff §12b before ANY PDF moves to Done. The Done
file-move is gated on `eligible:true` — holds carry the failing rule IDs.

### Next engine work — queue
**Ken's s189 lane-extension trio stands as ruled (s188 close), UNCHANGED:**
- ① Taxable state refund joins the lane · ② IRA deduction (Sch 1 L20)
  joins `sch1_fields` (GATE: RS spec line 20 input-typed) · ③
  `amt_medicare_wages_agg` editable browser-UI surface.
**Coverage gaps queued behind Ken's sequencing** (b012 + b014 triage, all
HOLD; b014 blocking counts in parens):
- 8889/HSA inputs + compute (blocks 3 held returns — top of the b014 list)
- 2441 + GA IND-CR 202 child-care flow (blocks 2)
- 8606 nondeductible IRA + GA education-credit direct inputs (blocks 1)
- Return-level 1099-R printed-aggregate fallback (blocks 1) — the
  `amt_medicare_wages_agg` pattern; RS spec fact granularity decides.
- Prior-year QBI loss carryforward **by activity** (affects 1)
- 4797 business-property sale (blocks 1)
- Schedule F · 8824 · 1099-MISC 8z (carried from b012).
**New (b027, Ken's call):** a browser "batch cleanup" screen over the
cleanup endpoint — the API is live; a UI adds visibility but cannot move
the local PDFs.
**Behind those (unchanged):** August GA unit (UET line-42 worksheet +
S4-8/S4-NB-18 NOL) · B002 row-creation family · MeF `build_irs5695` ·
year-constant ruling · 8829 / 6198. SB 31 TY2026 military = RS W-item.
**RS agenda:** 8995 spec correction (rental rows) + R-EIC-WSB-SE (carried).

---

## Known traps (carried — do not re-learn)
- **s191: the 8867 answer rows are cascade-managed.** Un-attested compute
  BLANKS every non-overridden 8867 value; attested compute OVERWRITES
  everything (even overridden). Imported answers must be
  `is_overridden=True`, and an attested shell + `f8867_fields` = a loud
  commit warning, never a silent stomp.
- **s191: a registry-count pin goes stale silently** when its module isn't
  in the session's bands (D_EIC_018 for two days). When adding a rule,
  grep for count pins on its family.
- **s190: migration-before-deploy skew on the shared DB is a CERTAINTY.**
  NOT NULL without `db_default` 500s every insert from still-deployed
  code (b014; rule in DECISIONS.md). Check Render deploy timestamps vs
  `django_migrations.applied` before diagnosing "dry-run works, live
  fails" as a code defect.
- **s189a/b: FACTOR THE DELTA INTO KNOWN CONSTANTS FIRST.**
- **s189a: every packet page prints the PRIMARY SSN in its header** — the
  TP|SP designator + printed worksheet columns are the owner authority.
- **s189b: 1040 line 37 as printed INCLUDES the line-38 penalty.**
- **s188: the TaxWise invoice number is NOT the Delvio client number.**
- **s188: `amt_medicare_wages_agg` is a FALLBACK, not an override.**
- **s187: never import computed row columns** (staging rejects them).
- **s187/s189b: the backentry fixture must seed EVERY form the flow
  touches** (s191 added seed_8867 to both backentry fixtures).
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
  (s191 touched neither — lane/endpoint/tests only; the 8867 import shape
  follows the seeded per-question face from specs/8867_spec.json).
- `pytest tests/test_flow_assertions.py` after any compute change (s191: 521 green).
- Dev environment shares the **production** Supabase DB — every write is a
  production write. No new migration this session (0233 remains latest).
