# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-03, session 193 (**THE DUPE-CLIENT MERGE EXECUTED** —
Ken's s192c merge-to-standalone ruling applied on prod: 276 of the 278
duplicate S-corp pairs merged; 2 held for Ken. Data-only session — no code
deploy, no migration.)*

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

### s193 shipped (Ken's s192c ruling executed — the dupe-client merge)
- **276 of 278 duplicate business pairs merged to the standalone client**
  (script `server/scripts/merge_dupe_business_clients_20260803.py`, dry-run
  → apply, per-pair transactions). Per pair: the empty Jul-wave shell entity
  deleted FIRST (the `unique_client_entity_name_type` constraint), then the
  Feb-wave entity (FEIN + converted data) re-parented onto the Jul standalone
  client — its TaxWise-style client_number survives. **All 278 pairs were
  S-corps — Ken's observation confirmed in the dry run.** Zero owner clients
  left entity-less; owners keep their 1040s. 20 merged entities that lacked a
  2025 tax year got a fresh 2025 backfill shell via `build_federal_return`
  (the one build path) so they stay back-entry-lane-ready.
- **Post-verify on prod**: exactly 2 duplicate business names remain — the
  2 flagged holds below. Spot-checks show one entity per merged client,
  EIN present, returns intact.
- **Rollback net**: pre-state snapshot + per-pair detail at
  `D:\tax-test-data\dupe_merge_20260803_prestate.json` / `_apply.txt`.
- **Emptiness-metric lesson**: seeded business shells carry 41 standard $0
  `OtherDeduction` scaffolding rows — "has data" must exclude
  `source=standard, amount=0` or every shell looks non-empty.
- Three shells created 7/29 (not 7/22) were the identical empty pattern —
  merged under the same ruling after individual verification.

### ▶ THE 2 HELD PAIRS — KEN'S CALL (next decision)
1. **Barcode Supply #1202**: BOTH sides now carry data — the standalone got
   the s192c 1120-S draft; the owner-attached Feb entity has the FEIN
   (14-1890165) + converted data. Likely resolution: move the FEIN + any
   prior-year data onto #1202's entity and delete the Feb twin — but that
   deletes an entity WITH data, so it needs Ken's explicit go.
2. **Robert Lomax LCSW LLC #3798**: the standalone shell has data AND
   already carries the SAME EIN as the Feb-side entity (47-2791267) — it was
   never an empty shell. Needs eyes on what's in each before merging.

### Codex: resume state (unchanged from s192)
b027 re-staging + cleanup endpoint per handoff §12b, PLUS the s192/s192b
addenda: state-refund, line-20, and box-12 packets are lane-eligible —
first packet of each shape is its own smoke test; re-check any payload
authored under the old dated-payments "INSTEAD" wording. The s192 deploy
is live (verified via the Render timeline during s192c).

### Next engine work — queue (awaiting Ken's sequencing, carried from s192b)
**⚠ Reframe from the s192b triage: many "unsupported" forms have FULL
engine support already** — their gap is lane schema only (the s185/s192
pattern, one session each): **8880** (f8880_* taxpayer scalars) · **8962
annual method** (s106e) · **2441** · **8863** · **5695** · **8606**
(compute_8606 exists) · **4797** (f4797_* facts) · **6252** · **7203** ·
**1116**. Bigger builds (engine + lane): **Sch F into the lane** ·
**8889/HSA completeness** (blocks 3 — top of the b014 list) · **7206
SEHI linkage** · **1099-G unemployment** · **1099-MISC 8z** · **8839** ·
**8824**.
**Other queued:** the TB default-template Rent/Taxes computed-line fix
(spawned s192c task) · per-activity QBI carryforward · 1099-R
printed-aggregate fallback (RS fact granularity decides) · RentalProperty
`owner` + GA RIE rental pull · DividendIncome US-obligation field (GA S1-10
structured) · GA payment line from dated state payments · shell-lookup
disambiguation metadata (PII call) · packet preflight tooling
(page-vs-invoice) · confirm dialog on the entity editor's "Import trial
balance" nav item (re-runs the import on click).
**Ken's-call items:** the 2 held dupe pairs (above) · the cleanup browser
screen · 1040-X + non-GA scope · shell creation authority (MCURLEY
duplicate blank shells need a data cleanup ruling).
**Needs reproduction on prod before any build (carried):** D_8995_STALE ·
the BARROW GA LIC claim · the BENKOSKI card-creation family (= queued B002).
**Behind those (unchanged):** August GA unit (UET line-42 worksheet +
S4-8/S4-NB-18 NOL) · B002 row-creation family · MeF `build_irs5695` ·
year-constant ruling · 8829 / 6198. SB 31 TY2026 military = RS W-item.
**RS agenda:** 8995 spec correction (rental rows) + R-EIC-WSB-SE (carried).

---

## Known traps (carried — do not re-learn)
- **s193: seeded business shells are NOT FFV-empty** — 41 standard $0
  OtherDeduction rows per shell; any "is this entity empty" check must
  exclude `source=standard, amount=0`.
- **s192: an unknown `sr_py_filing_status` computes silently wrong** —
  the §111 std-deduction lookup returns $0 basic for an unrecognized
  status. Staging validates the enum; the browser UI does not (CharField).
- **s192: the lane never sends Schedule 1 line 1** — `sr_*` inputs only;
  the worksheet owns line 1/8z when engaged (disengage never clobbers a
  pure direct entry — that manual path is for the RED exception cases).
- **s191: the 8867 answer rows are cascade-managed.** Un-attested compute
  BLANKS every non-overridden 8867 value; attested compute OVERWRITES
  everything (even overridden). Imported answers must be
  `is_overridden=True`; attested shell + `f8867_fields` = loud warning.
- **s191: a registry-count pin goes stale silently** when its module isn't
  in the session's bands. When adding a rule, grep for family count pins.
- **s190: migration-before-deploy skew on the shared DB is a CERTAINTY.**
  NOT NULL without `db_default` 500s every insert from still-deployed
  code (rule in DECISIONS.md). Check Render deploy timestamps vs
  `django_migrations.applied` before diagnosing "dry-run works, live
  fails" as a code defect.
- **s189a/b: FACTOR THE DELTA INTO KNOWN CONSTANTS FIRST.**
- **s189a: every packet page prints the PRIMARY SSN in its header** — the
  TP|SP designator + printed worksheet columns are the owner authority.
- **s189b: 1040 line 37 as printed INCLUDES the line-38 penalty.**
- **s188: the TaxWise invoice number is NOT the Delvio client number.**
- **s188: `amt_medicare_wages_agg` is a FALLBACK, not an override**
  (unchanged by its s192 UI surfacing — per-row box 5 always wins).
- **s187: never import computed row columns** (staging rejects them).
- **s187/s189b: the backentry fixture must seed EVERY form the flow
  touches** (s192 added seed_state_refund to the commit fixture).
- **s186: a "reproduced engine defect" can be shell residue.**
- **s186/s192: sch1_fields is for spec-typed INPUT lines only** (11/20/21).
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
  (s193 touched neither — data-only session).
- `pytest tests/test_flow_assertions.py` after any compute change (none in s193).
- Dev environment shares the **production** Supabase DB — every write is a
  production write. No migration this session (0233 remains latest).
