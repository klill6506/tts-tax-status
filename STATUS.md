# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-03, session 196 (**Georgia depreciation was silently
returning the FEDERAL number**; found, fixed, and tied to Ken's Lacerte GA
schedule to the dollar). Migration 0234 latest — s196 added none.*

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
`tts-tax-app` still fails every push — Ken should delete it.**

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195, plainly: **no 2025 returns are being prepared in the app.**
Entries exist to find defects. Do not append compliance cautions to every
number — state the finding and move on.

---

## ▶ RESUME HERE — route the Georgia depreciation pair to the GA-600S

The engine now produces a correct Georgia number (s196, below). Nothing
consumes it yet, so the preparer's hand-keyed Schedule 8 override is still
what reaches the GA return. Closing that is the next unit — and it is what
unblocks the K-1 → individual gaps, which all need this number as data.

**⚠ ONE DECISION FOR KEN BEFORE BUILDING — which presentation.**
- **RS spec GA600S R001/R002** says the addition is `ga_addition_bonus_depr`
  — *federal BONUS only* — with `ga_subtraction_depr` the GA depreciation.
- **Ken's Lacerte GA Shareholder Summary** uses the **GROSS pair**: 2b other
  increases = federal depreciation, 3b other decreases = Georgia
  depreciation.
Both net to the same −3,578 on the test entity, but they print very
different figures on the face. **Ken picks; do not guess.**

**Then the mechanics (both known, neither built):**
1. The pair lands on GA-600S **S7_7 "Other Additions (Attach Schedule)"** and
   **S8_4 "Other Subtractions (Attach Schedule)"** — the real GA form has no
   dedicated depreciation line; "other + attach schedule" is where Georgia
   puts it, which is where Ken hand-keyed it.
2. ⚠ `_populate_ga_from_federal` (`returns/views.py` ~1870) is a **one-shot
   pull at state-return CREATION**, not a recompute. An existing GA-600S
   will not pick the pair up without a re-pull. Its own comment
   (`views.py` ~738, on GA700_FEDERAL_PULL) says GA depreciation "require[s]
   the GA Form 4562 recompute and stay[s] preparer-entered" — **that comment
   is now stale**; the recompute exists.
3. ⚠ S7_7/S8_4 are CATCH-ALL lines. Writing them unguarded stomps any other
   preparer entry (the s157 rollup-stomp class). Write only when empty, or
   give the pair its own lines.

---

## Shipped in s196 (deployed, `517f653`)

**Georgia depreciation was silently returning the FEDERAL number.**
The test entity read GA current depreciation 37,931 — identical to federal —
against Lacerte's 41,509.

- **Cause:** `_calculate_state_ga` started from `asset.cost_basis`, which per
  RS 4562 R018 is NET of prior-year §179 **and prior-year bonus** once the
  split history is keyed. Georgia disallows bonus (GA-600S spec R002), so the
  arm was handed a basis Georgia does not recognise. On every asset with
  bonus history it returned the federal figure **as though it were
  Georgia's** — not a visible gap, a wrong number that looks right.
- **Fix:** Georgia depreciates its OWN basis — GROSS cost (`original_cost`
  when the R018 split is keyed) less the §179 Georgia allowed, never reduced
  by federal bonus. Prior-year §179 joins the current election (gross has not
  been reduced by it); only the CURRENT-year election is a current deduction.
- **`state_prior_depreciation` is now READ.** It was importable (CSV),
  stored, and printed in the register, but **no computation touched it** — an
  Input/Compute chain break. It caps the deduction at Georgia's remaining
  basis, so an asset Georgia already expensed under §179 does not depreciate
  a second time.
- **Verified to the dollar** on the 30-asset test register: 27 assets with no
  bonus history → GA == federal (35,545); 3 assets with bonus history → GA
  5,964 vs federal 2,386; **GA total 41,509 = Lacerte**. Federal unchanged at
  37,931.
- **Tests:** 3 regression tests + the real disposal record, **revert-proved**.
  The mock fixture never set `original_cost`, so it read as a truthy
  MagicMock and fed the Georgia arm a mock — 6 tests were passing on that.
  859 green across the 4562/GA/flow sweep.

**Ken re-confirmed GA §179 = $2,500,000 / $4,000,000** via HB 1199
(2026-08-03). He had proposed $1,220,000/$3,050,000 — the federal **TY2024**
amounts — and reversed on the evidence. Constants unchanged. *(This is the
second time that line has drifted to a stale historical figure.)*

---

## ⚠ Open items for Ken

1. **Which GA-600S presentation** — RS bonus-only vs Lacerte's gross pair
   (the resume-here decision above).
2. **GA §179 does NOT cover certain REAL PROPERTY** (Ken, s196; confirmed
   against GA DOR + conformity commentary). `_calculate_state_ga` applies one
   flat `GA_179_LIMIT` with **no property-type test**, so a client expensing
   qualified real property under §179 federally gets a Georgia deduction
   Georgia does not allow. No test entity exercises it. **Needs an RS rule
   before it is encoded.**
3. **RS `R-GA500-DEPR` is stale** — still says GA "did NOT adopt OBBBA... GA
   uses its own §179 limit", contradicting the GA600S spec and Ken's HB 1199
   ruling. Changes no number today (both lines direct-entry) but it is the
   rule that governs the auto-populate that GAP 3 *is*. Fix RS-side.
4. **The Lacerte PDF importer does not read the GA depreciation pages** —
   `state_prior_depreciation` / `state_method` / `state_life` came in empty on
   all 30 test assets and had to be hand-keyed from Ken's schedule. A CSV path
   exists (`csv_depr_parser.py`); the PDF path does not.
5. **An asset with federal bonus history and NO keyed GA prior now
   over-depreciates for Georgia** (it gets full gross basis with no cap).
   Guarded by nothing yet — a diagnostic is wanted. Not built.
6. **RS rule for the shareholder-side §179 disposition** — still blocks GAP 1.
7. **GA PTET base on a separately stated gain** — unchanged from s195.
8. **Client #2969** holds an empty duplicate individual entity — unchanged.
9. **Retire `reparent_business_entities`** — unchanged.
10. M1_5b is COMPUTED — a return with other book-income-not-on-K items needs
    an override carrying the total.

## The three K-1 → individual gaps (Ken's unit, still open)
**GAP 1 — shareholder-side §179 disposition:** BLOCKED on an RS rule (0 of
the 9 4797 rules mention a shareholder/partner/§179 carryover).
**GAP 2 — the Georgia Shareholder Summary (Form 600S):** buildable, but its
line structure exists only as prose in this file — **the Lacerte artifact is
not on disk**; ask Ken to re-send before building labels.
**GAP 3 — GA individual modifications carryover:** GA-500 `S1-3` (add) /
`S1-11` (sub) are spec'd as direct-entry, and `R-GA500-DEPR` explicitly
anticipates "the depreciation engine's Asset.state_* fields auto-populate
later" — that is this gap. Needs item 3 above fixed first.

## Carried queue (unchanged from s195 unless noted)
**Lane-schema-only (engine-complete)**: 8880 · 8962 annual · 2441 · 8863 ·
5695 · 8606 · 4797 · 6252 · 7203 · 1116. **True builds**: Sch F lane ·
8889/HSA · 7206 · 1099-G · 1099-MISC 8z · 8839 · 8824.
**Other queued:** TB default-template Rent/Taxes computed-line fix ·
depreciation-importer prior-split hardening · per-activity QBI carryforward ·
1099-R printed-aggregate fallback · RentalProperty `owner` + GA RIE rental
pull · DividendIncome US-obligation field · GA payment line from dated
payments · packet preflight · TB-import nav confirm dialog ·
**client-delete UI (there is NO path to delete a client — the pages that
can are unrouted)** · **duplicate guard is blind to entity names**.
**RS agenda:** 8995 rental rows · R-EIC-WSB-SE · 4562 same-year-disposal ·
**4797 shareholder-side §179 (GAP 1)** · **GA §179 real-property carve-out** ·
**R-GA500-DEPR conformity correction**.
