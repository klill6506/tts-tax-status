# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-02, session 190 (**the b014 production defect
root-caused and hardened** — the two rental live-commit 500s were NOT a
dry-run/live code split: migration 0232 was applied to the shared DB
mid-batch, between the dry-runs and the live commits, while prod still ran
pre-0232 code. Migration 0233 adds a durable DB-level DEFAULT; two
regression tests pin the contract; b014's six coverage asks queued.)*

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

### s190 shipped (the b014 defect)
- **The 500s explained, to the second (Render logs + django_migrations)**:
  b014c2 dry-runs 03:09:19–24Z all tied → migration 0232
  (`qbi_trade_or_business` NOT NULL) applied to the shared DB at
  **03:09:29Z** from the s189b dev session → live commits 03:09:36Z and
  03:10:20Z 500'd with `NotNullViolation` (prod still ran s189a code,
  whose rental INSERT omits the column; Django drops the Postgres DEFAULT
  after an AddField backfill) → the s189b deploy went live 03:26Z and
  closed the window. The two no-rental returns committed fine because
  neither inserts rental rows. **No dry-run/live code defect exists** —
  the two paths are one code path.
- **Migration 0233 (applied)**: `db_default=False` on
  `RentalProperty.qbi_trade_or_business` — the column keeps a
  Postgres-level DEFAULT, so a pre-deploy service inserting without the
  column can never NULL-violate again.
- **New standing rule (DECISIONS.md, cross-cutting standards)**: any new
  NOT NULL column on a table the deployed service inserts into carries
  `db_default=` (or is nullable). The shared dev/prod DB makes
  migration-before-deploy skew a certainty, not a risk.
- **Regression tests** (`test_backentry_commit.py`, band now 29):
  `test_dry_run_tie_commits_live_same_merge_b014` (dry-run TIE ⇒ live
  commit under the same merge mode lands with the same verdict, on the
  failing shape: loss rental + direct depreciation +
  `active_participation: false` + prior unallowed passive + zero-dollar
  K-1 shell, over existing rows via replace_documents) and
  `test_rental_qbi_column_keeps_db_default_b014` (the schema pin).
- Bands green: backentry commit 29, flow 521, reconcile + markfiled 18.
- **Both held returns verified re-commitable** (CC rolled-back live-path
  runs on the shared DB): the 3-rental return ties under
  `replace_documents`; the 1-rental+K-1-shell return ties plain.

### Codex: resume state
Re-POST the two b014c2 live commits — no payload changes: the 3-rental
return with `{"merge": "replace_documents"}` (its shell carries rows), the
rental+K-1 return without merge. Both tie. b014's staging limit and
frozen-batch/new-key correction behavior stay as-is (unchanged, by design).

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
- Return-level 1099-R printed-aggregate fallback when payer detail is
  absent (blocks 1) — the `amt_medicare_wages_agg` pattern: fallback
  SOURCE only + explicit warning/provenance; RS spec fact granularity
  decides how it may exist (the s188 8959 rule).
- Prior-year QBI loss carryforward **by activity** (s189b landed the
  return-level fields; b014 asks per-activity) (affects 1)
- 4797 business-property sale (a rental disposition can't ride 8949 rows)
  (blocks 1)
- Schedule F · 8824 · 1099-MISC 8z (carried from b012).
**Behind those (unchanged):** August GA unit (UET line-42 worksheet +
S4-8/S4-NB-18 NOL) · B002 row-creation family · MeF `build_irs5695` ·
year-constant ruling · 8829 / 6198. SB 31 TY2026 military = RS W-item.
**RS agenda:** 8995 spec correction (rental rows) + R-EIC-WSB-SE (carried).

---

## Known traps (carried — do not re-learn)
- **s190: migration-before-deploy skew on the shared DB is a CERTAINTY.**
  A NOT NULL column without `db_default` 500s every insert from the
  still-deployed code until the deploy lands (the b014 incident; rule now
  in DECISIONS.md). Check the Render deploy timestamp against
  `django_migrations.applied` before diagnosing a "dry-run works, live
  fails" report as a code defect.
- **s189a/b: FACTOR THE DELTA INTO KNOWN CONSTANTS FIRST.** Five of six
  b010–b012 "engine defects" dissolved or localized by arithmetic before
  any code was read.
- **s189a: every packet page prints the PRIMARY SSN in its header** — the
  TP|SP designator column + the printed worksheet columns are the owner
  authority.
- **s189b: 1040 line 37 as printed INCLUDES the line-38 penalty** —
  guide rule 8 with a pre-staging arithmetic check.
- **s188: the TaxWise invoice number is NOT the Delvio client number.**
- **s188: `amt_medicare_wages_agg` is a FALLBACK, not an override.**
- **s187: never import computed row columns** (staging rejects them).
- **s187/s189b: the backentry fixture must seed EVERY form the flow
  touches** (SCH_8812 was the latest miss).
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
  (s190 touched neither — models/migration/tests only).
- `pytest tests/test_flow_assertions.py` after any compute change (s190: 521 green).
- Dev environment shares the **production** Supabase DB — every write is a
  production write. Migrations 0232 + 0233 are applied there.
