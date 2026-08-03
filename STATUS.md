# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-03, session 192 + 192b (**the s188 lane-extension
trio shipped**, then **ChatGPT's blocker backlog triaged** — two real
defects fixed same-day: the SS worksheet ran before Schedule E landed
(6b stale-low on every SS+rental return, HOLLIFIELD $387) and the
dated-payments guidance zeroed 1040 line 26 (HUFF); W-2 box 12 joined
the lane (Sch 2 L13 derives). No migration.)*

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

### s192 shipped (Ken's s188 ruling, executed exactly)
- **① Taxable state refund (Sch 1 line 1)**: the 17 `sr_*` §111
  worksheet-input fields joined `TAXPAYER_FIELDS` (model fields existed
  since NEXT-UP #9 — schema growth only, zero compute change). The lane
  transcribes TaxWise's refund-worksheet INPUTS; the engine computes
  line 1 + the 8z share. Line 1 stays REJECTED in `sch1_fields` (a direct
  entry would fight `compute_state_refund_db`). Staging guardrails:
  unknown `sr_py_filing_status` = ERROR (silently looks up a $0
  prior-year std deduction → overstates taxability) · `sr_py_age_blind_boxes`
  outside 0–4 = ERROR · refund amounts without `sr_py_itemized: true` =
  WARNING (§111 computes $0 — can never tie a nonzero filed line 1).
- **② IRA deduction (Sch 1 line 20)**: joined `SCH1_DIRECT_LINES`. The
  Ken-set gate PASSED — live RS `SCH_1` export (2026-08-03) types line 20
  `input`; verified no engine writes "20" (8582 / Sch E only read it);
  R-S1-04 sums it into line 26 → 1040 line 10.
- **③ `amt_medicare_wages_agg` browser-editable** (Ken chose editable
  over lane-only): joined the Taxpayer serializer explicit list + the
  QBI/8959 facts card (Slate Schedule C screen AND the legacy FormEditor
  card). Fallback semantics unchanged — compute reads it only when no
  W-2 row carries box 5.
- **b009 un-HOLDs shipped with the code** (the ruling's condition):
  `triage_inbox.py` BLOCKED pruned (STATE REFUND + IRA patterns →
  SUPPORTED; 1099-G stays invoice-blocked — printed Sch 1 line 7 HOLDs,
  line-1-only unblocks) · AUTHORING_GUIDE face-checks rewritten + new
  `sr_*` section · CODEX_KICKOFF s192 addendum (per-shape one-packet
  smoke tests) · handoff guide §0/§4 updated · schema regenerated
  (sr enum + 0–4 range mirrored).
- Tests: staging **33** (6 new) · commit **34** (2 new — full §111 math
  end-to-end ties AGI 72,450; line 20 ties 64,250; fixture gained
  `seed_state_refund`) · flow **521** · taxpayer+topic8 72 · tsc 0 ·
  vitest **1574** — all green. **No migration** (0233 remains latest).

### s192b shipped (ChatGPT's blocker backlog — triage + same-day fixes)
Ken relayed ChatGPT's 10-section blocker list. Triage verdicts:
- **Stale (already live, no build)**: 8867 import (s191 `f8867_fields` —
  the four named holds re-stage per handoff §12b) · `qbi_loss_carryforward_prior`
  (s189b, return-level; per-ACTIVITY carryforward stays queued) · taxable
  state refund + IRA deduction + educator (s186–s192) · the cleanup gate
  API (s191; the SCREEN is Ken's call) · ATKINS ACTC (fixed s189b) ·
  rental-199A-after-passive (engine already uses `passive_8582_allowed`).
- **REAL, FIXED in s192b (`5b25335`)**: ① the SS Benefits Worksheet ran
  BEFORE Schedule E landed — R-RET-SS-01's WS3 reads 1040 line 8, which
  the Sch E reflow changes AFTER — so 6b was stale-low on EVERY SS+rental
  return (HOLLIFIELD $387); compute_return now re-runs the worksheet
  after the reflow (revert-proven pin: 17,000 vs stale 9,600; 8582 MAGI
  unaffected — it already excludes 6b per §469(i)). ② the AUTHORING_GUIDE
  told authors to use dated payments "INSTEAD of" flat buckets — but
  line 26 reads ONLY flat and the §6654 accrual ONLY dated (HUFF's
  $6,000 → $0 on line 26); wording corrected to BOTH-in-agreement + a
  staging warning on divergence. ③ W-2 box 12 imports (`box_12_entries`
  nested; codes A/B/M/N derive Sch 2 line 13 — HUFF's $34).
- **Known v1 boundaries (not defects; workaround exists)**: GA RIE
  Schedule E rental share (RentalProperty has no `owner` — enters via
  `ga500_fields` RIE lines; permanent fix = owner field + pull, queued) ·
  dividend US-obligation GA subtraction (the `ga500_fields "S1-10"`
  convention; a structured DividendIncome field queued).
- **Needs reproduction on prod before any build**: D_8995_STALE (DUVALL) ·
  BARROW GA LIC (post-s182 rules) · [client] card creation (predates
  deploys per the report itself; = the queued B002 row-creation family).
- **Ken decisions queued**: shell-lookup disambiguation metadata (masked
  last4/DOB = PII exposure on the lookup endpoint — Ken's call) · amended
  1040-X + non-GA states in lane scope? · the cleanup browser screen.

### s192c shipped (Ken's live asks — the FIRST REAL 1120-S TB IMPORT)
- **Barcode Supply #1202 2025 1120-S drafted from a scanned BS+P&L** — the
  first real client through the TB import lane. Scan transcribed to a
  balanced TB CSV (assigned account numbers) → client-scoped 1120-S
  template (47 EXACT rules by account number) → upload → plan (ties,
  47/49 mapped) → commit → compute. Face reconciles to the books within
  whole-dollar rounding (ordinary income 251,104 vs book NOI 251,107.06).
  Notes + opens: `D:\tax-test-data\1120S\barcode_supply_2025_notes.md`
  (vehicle 4797 detail · tax depreciation · beginning AAA · shareholders ·
  EIN · 401k/GA-PTE classifications — all Ken inputs).
- **Found: the default TB templates write Rent/Taxes onto COMPUTED face
  lines** (11/12 are formulas over D_RENT_*/D_TAXES_* details that have NO
  mapping keys) — the import lands then the formula pass zeroes it.
  Worked around by direct detail entry; the proper fix is a spawned task
  (seeder mapping keys + repoint the rules, check 1065/1120 for the class).
- **Also fixed the s192-morning seeding gap**: the three default mapping
  templates lived only in Dev Tax Firm — copied to The Tax Shelter
  (52/55/59 rules), which is why Ken's earlier ledger-TB 1065 attempt
  silently mapped nothing.
- **Return-manager entity chips restyled** (Ken's ask): 34px, per-type
  Slate-scale tints (steel/green/violet/info/neutral), live-verified.
- **"2 clients for every client" NOT reproduced**: 3,747 clients, 3,739
  roster rows (all 2025; GA state returns excluded server-side; only 21
  true same-name pairs). The rendered roster shows one row per client.
  Awaiting Ken's pointer to the exact screen he saw.

### Codex: resume state
b027 re-staging + cleanup endpoint per handoff §12b, PLUS the s192/s192b
addenda: state-refund, line-20, and box-12 packets are lane-eligible —
first packet of each shape is its own smoke test; re-check any payload
authored under the old dated-payments "INSTEAD" wording. Wait for the
Render deploy before staging returns using the new fields.

### Next engine work — queue (awaiting Ken's sequencing)
**⚠ Reframe from the s192b triage: many "unsupported" forms have FULL
engine support already** — their gap is lane schema only (the s185/s192
pattern, one session each): **8880** (f8880_* taxpayer scalars) · **8962
annual method** (s106e) · **2441** · **8863** · **5695** · **8606**
(compute_8606 exists) · **4797** (f4797_* facts) · **6252** · **7203** ·
**1116**. Bigger builds (engine + lane): **Sch F into the lane** ·
**8889/HSA completeness** (blocks 3 — top of the b014 list) · **7206
SEHI linkage** · **1099-G unemployment** · **1099-MISC 8z** · **8839** ·
**8824**.
**Other queued:** per-activity QBI carryforward · 1099-R printed-aggregate
fallback (RS fact granularity decides) · RentalProperty `owner` + GA RIE
rental pull · DividendIncome US-obligation field (GA S1-10 structured) ·
GA payment line from dated state payments · shell-lookup disambiguation
metadata (PII call) · packet preflight tooling (page-vs-invoice).
**Ken's-call items:** the cleanup browser screen · 1040-X + non-GA scope ·
shell creation authority (MOODY RONALD stays unattached — Ken deleted
him s183; MCURLEY duplicate blank shells need a data cleanup ruling).
**Behind those (unchanged):** August GA unit (UET line-42 worksheet +
S4-8/S4-NB-18 NOL) · B002 row-creation family (covers the [client]
claims) · MeF `build_irs5695` · year-constant ruling · 8829 / 6198.
SB 31 TY2026 military = RS W-item.
**RS agenda:** 8995 spec correction (rental rows) + R-EIC-WSB-SE (carried).

---

## Known traps (carried — do not re-learn)
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
  (s192 touched neither — the line-20 gate was answered by the LIVE SCH_1
  export, re-fetched 2026-08-03; sr_* follows specs/state_refund_spec.json).
- `pytest tests/test_flow_assertions.py` after any compute change (s192: 521 green).
- Dev environment shares the **production** Supabase DB — every write is a
  production write. No migration this session (0233 remains latest).
