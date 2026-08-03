# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-03, session 194 + 194b (**Barcode pair merged ·
the 2025 Lacerte depreciation schedule imported, 30/30 tied · Ken's
inputs entered — THE 1120-S FACE NOW TIES THE BOOKS END-TO-END (L22
379,312 = book NI)** · back button + entity Email shipped · **s194b: A
DEPLOY-TIME SEEDER WAS WIPING EVERY PREPARER OVERRIDE on every computed
1120-S line — fixed `5368945`**. No migration — 0233 latest.)*

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

### s193 (this morning): the dupe-client merge EXECUTED
276/278 pairs merged to the standalone client (all S-corp); 2 holds —
**Barcode (resolved in s194, below)** and **Robert Lomax LCSW #3798
(STILL HELD — its "shell" has data AND the same EIN as its twin; Ken's
eyes needed)**. Detail in STATUS_ARCHIVE; rollback snapshots in
`D:\tax-test-data\`.

### s194 shipped (Ken's live asks)
- **Barcode pair merged** (Ken's go): #1202 gained the EIN / inc date /
  NAICS / 100%-shareholder + officer rows from the twin; twin deleted
  after copy; owner keeps his own client + 1040. One Barcode remains.
- **Barcode 2025 depreciation imported from the Lacerte schedule** (PDF →
  row-reconstructed text → `import_depreciation` → the R018 re-key script
  `scripts/rekey_barcode_depreciation_20260803.py`): **30/30 assets tie
  Lacerte's 2025 amounts exactly; L14 = 37,931 = the book number** (no
  M-1 depreciation difference). Return carries the **§168(k)(7)
  elect-out for class 5** (matches Lacerte's no-bonus MQ math on the 2025
  additions). Parser fix shipped: **Lacerte prints straight line "S/L" —
  the method regex rejected it** (the 27.5 defect class; every S/L asset
  imported method-less with life 0). The in-app Depreciation-screen
  import gets the fix via the same deploy.
- **Back button** in the Slate app bar (browser-history back; hidden on
  the Return Manager root). **Entity Email field** on the Client
  Information screen (both serializers + both renderings, one autosave
  lane). Both live-verified; suites green (client 1,577 · clients band
  28 · lacerte-import band 40+6).
- **Importer lesson (engine-adjacent, no engine change)**: the Lacerte
  schedule's combined "Prior 179/SDA/Depr." column must be re-keyed to
  the R018 basis split (`cost_basis` NET of prior 179/bonus) or
  fully-written-off assets depreciate again; the importer also stamps
  historical `bonus_pct` (a stale pct double-reduces the table basis).
  Candidate importer hardening: derive the split automatically — queued.

### s194b shipped (same day — Ken's inputs + THE DEPLOY-WIPE FIX)
- **Ken's Barcode inputs entered** (`scripts/barcode_ken_inputs_20260803.py`):
  3 vehicle sale prices (sum = the books' 237,361 exactly) → L4 128,208
  (Tahoe 43,208 + Cybertruck 99,611 all-§1245, F350 −14,611 short-term);
  beginning AAA 656,869 → M2_1a; S election 1/1/2004; officer comp
  100,000. **The 1120-S face ties the books end-to-end: L22 = 379,312 =
  book net income.** GA is PTET per Ken — the `GA_PTET` face field exists;
  set when the GA-600S is attached (blocked on the GA depreciation
  schedule).
- **THE L20 MYSTERY SOLVED — IT WAS THE DEPLOYS**: `seed_1120s` (run by
  `seed_all` on EVERY deploy) cleared `is_overridden` unconditionally on
  every computed line's stored values — **every deploy wiped every
  preparer override on every computed 1120-S line, firm-wide**. Invisible
  when the override equals the formula; fatal where it differs (the TB
  rollup's L20, any Ctrl+Enter override of a computed line). Fixed: the
  clear now fires only on a manual→computed TRANSITION (the original
  intent). 3 regression tests both directions + seed-safety band 6 green.
  Deployed `5368945`; L20 restored (92,674, overridden) after the last
  bad deploy.

### ▶ Barcode return — remaining Ken items
1. **GA depreciation schedule** (prior federal bonus ⇒ GA basis differs) —
   then build the GA-600S + set GA_PTET.
2. Asset-25 prior-column oddity (source shows 25,000 prior on a year-5
   asset — adjusted basis displays high) · 401k L17 classification ·
   APIC confirm.

### ⚠ Watch / agenda (s194)
- **RS agenda add**: 4562 R003 has NO same-year-acquired-and-sold rule
  (Pub 946: no deduction). Engine NOT changed (spec-silent); the Barcode
  F350 is keyed method NONE meanwhile.
- Verify post-deploy (`5368945`): Barcode L20 still 92,674/overridden
  after the FIXED seeder runs (checked at session close — see below).

### Codex: resume state (unchanged from s192)
b027 re-staging + cleanup endpoint per handoff §12b + the s192/s192b
addenda. The s192 deploy is live.

### Next engine work — queue (awaiting Ken's sequencing, carried)
**Lane-schema-only (engine-complete)**: 8880 · 8962 annual · 2441 · 8863 ·
5695 · 8606 · 4797 · 6252 · 7203 · 1116. **True builds**: Sch F lane ·
8889/HSA (blocks 3) · 7206 · 1099-G · 1099-MISC 8z · 8839 · 8824.
**Other queued:** TB default-template Rent/Taxes computed-line fix ·
depreciation-importer prior-split hardening (s194) · per-activity QBI
carryforward · 1099-R printed-aggregate fallback · RentalProperty `owner`
+ GA RIE rental pull · DividendIncome US-obligation field · GA payment
line from dated payments · shell-lookup PII metadata · packet preflight ·
TB-import nav confirm dialog.
**Ken's-call items:** the Lomax held pair · the cleanup browser screen ·
1040-X + non-GA scope · MCURLEY blank-shell cleanup ruling.
**Needs repro before build (carried):** D_8995_STALE · BARROW GA LIC ·
[client] card family (= queued B002).
**Behind those:** August GA unit (UET line-42 + NOL) · B002 family · MeF
`build_irs5695` · year-constant ruling · 8829 / 6198. SB 31 = RS W-item.
**RS agenda:** 8995 rental rows · R-EIC-WSB-SE · **4562 same-year-disposal
rule (new, s194)**.

---

## Known traps (carried — do not re-learn)
- **s194b: A DEPLOY-TIME SEEDER CAN REVERT PRODUCTION STATE.** `seed_all`
  runs every `seed_*` on every deploy; `seed_1120s` cleared
  `is_overridden` unconditionally. Any seeder write that touches
  FormFieldValue (not just FormLine) is a firm-wide production mutation —
  gate it on an actual schema TRANSITION, never on "the line is computed".
  Swept the other seeders (1065/1041/ga700/…): they only delete FFVs of
  REMOVED lines — intentional, not this shape. 1120-S was the only one.
- **s194: Lacerte's "Prior 179/SDA/Depr." is a COMBINED column** — import
  must re-key to the R018 split (cost_basis net of prior 179/bonus) and
  zero stamped bonus_pct, or written-off assets depreciate again.
- **s194: disposal math runs at sales_price=0** the moment date_sold is
  set (`sales_price is not None` — the field defaults 0) — a sold asset
  without proceeds entered flows a phantom full-basis loss to L4/K9.
- **s193: seeded business shells are NOT FFV-empty** — 41 standard $0
  OtherDeduction rows per shell; emptiness checks must exclude
  `source=standard, amount=0`.
- **s192: an unknown `sr_py_filing_status` computes silently wrong**
  (staging validates; the browser CharField does not).
- **s192: the lane never sends Schedule 1 line 1** (`sr_*` inputs only).
- **s191: the 8867 answer rows are cascade-managed** (import as
  `is_overridden=True`; attested shell + `f8867_fields` = loud warning).
- **s191: a registry-count pin goes stale silently** — grep family pins.
- **s190: migration-before-deploy skew is a CERTAINTY** — new NOT NULL
  columns on service-inserted tables carry `db_default`; check deploy
  timestamps vs `django_migrations.applied` first.
- **s189a/b: FACTOR THE DELTA INTO KNOWN CONSTANTS FIRST.**
- **s189a: every packet page prints the PRIMARY SSN** — TP|SP designator
  is the owner authority.
- **s189b: 1040 line 37 as printed INCLUDES the line-38 penalty.**
- **s188: the TaxWise invoice number is NOT the Delvio client number.**
- **s188: `amt_medicare_wages_agg` is a FALLBACK, not an override.**
- **s187: never import computed row columns.**
- **s187/s189b: the backentry fixture must seed EVERY form the flow touches.**
- **s186: a "reproduced engine defect" can be shell residue.**
- **s186/s192: sch1_fields = spec-typed INPUT lines only** (11/20/21).
- **s185: Sch A 5a is DERIVED; explicit 0 restores the derived path.**
- **s184: blank prior-year 2210 COMPUTES the 90% fallback.**
- **A correction payload must send explicit 0** — omit preserves.
- **`replace_documents` does NOT clear overrides or taxpayer scalars.**
- **The dry-run IS the commit, rolled back.**
- GA `LIC-NODEP` gates the whole LIC; set when GA prints 17a/17c.
- Flat `est_payment_q1..q4` drive 1040 line 26; dated rows drive only §6654.
- ⚠ PS 5.1: inline `python -c` fails silently — always exec a script file.
- ⚠ The entity editor's "Import trial balance" nav item re-runs the
  import ON CLICK (confirm dialog queued).

---

## Standing gates
- `git pull origin main` at session start; push with `git push origin HEAD:main`.
- Never `git stash`, never `checkout` mid-session.
- Rule Studio spec required before touching `compute*.py` / `renderer.py` /
  the depreciation engine (s194 fetched the 4562 spec; engine deliberately
  NOT changed on the spec-silent same-year rule — flagged instead).
- `pytest tests/test_flow_assertions.py` after any compute change (none in s194).
- Dev environment shares the **production** Supabase DB — every write is a
  production write.
