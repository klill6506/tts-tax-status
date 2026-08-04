# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-03, session 197 (**the Georgia depreciation pair now
reaches the GA-600S**; Ken's gross-pair ruling built, Barcode ties Lacerte).
Migration 0234 latest — s197 added none.*

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

## ▶ RESUME HERE — the three K-1 → individual gaps

GAP 3's blocker is now half-cleared: the entity side produces the Georgia
depreciation modification, so there is a real number for the individual
return to carry. Ken's unit, unchanged in shape:

**GAP 1 — shareholder-side §179 disposition:** BLOCKED on an RS rule (0 of
the 9 4797 rules mention a shareholder/partner/§179 carryover, and the gain
depends on the owner's UNUSED carryover).
**GAP 2 — the Georgia Shareholder Summary (Form 600S):** buildable. Its line
structure exists only as prose in this file — **the Lacerte artifact is not on
disk**; ask Ken to re-send before building labels. The s197 pair is what its
lines 2b/3b would show, so the two now reconcile by construction.
**GAP 3 — GA individual modifications carryover:** GA-500 `S1-3` (add) /
`S1-11` (sub) are spec'd as direct-entry, and `R-GA500-DEPR` explicitly
anticipates "the depreciation engine's `Asset.state_*` fields auto-populate
later" — that is this gap. **Fix open item 2 below first** (that rule is stale).

---

## Shipped in s197 (deployed, `014bd1f`; no migration)

**The Georgia depreciation pair now reaches the GA-600S.** s196 made the
engine right; nothing consumed it, so the preparer's hand-keyed net difference
was still what reached the return.

- **Ken's ruling, built:** the GROSS PAIR on dedicated lines — `S7_7a`
  federal depreciation (Schedule 7 ADDITION) and `S8_3a` Georgia depreciation
  (Schedule 8 SUBTRACTION), both joining the `S7_8` / `S8_5` totals. Matches
  the Lacerte GA Shareholder Summary (its 2b / 3b).
- **The constraint, confirmed and handled:** the official DOR AcroForm has NO
  depreciation widget (`S7_1..S7_8` / `S8_1..S8_5` only) — which is exactly why
  Georgia words the catch-alls "(Attach Schedule)". The dedicated lines print
  on the **preparer/client copy**; on the **filed copy** they roll into
  `S7_7` / `S8_4`. Both faces carry identical totals. Verified by rendering both.
- **Source:** `ga600s_depreciation_pair()` sums the FEDERAL register's
  `current_depreciation` / `state_current_depreciation`. §179 sits in both
  halves and nets to zero (GA conforms, HB 1199); §168(k) bonus is the delta.
- **The one-shot pull is fixed:** `_auto_sync_ga600s` re-pulls after every
  federal recompute (override-respecting, never raises), so an existing
  GA-600S tracks the register instead of freezing at creation-day values. New
  lines are backfilled first, so the pair lands on the FIRST sync, not the second.
- **Verified on the test entity against Ken's Lacerte:** federal 37,931 /
  Georgia 41,509 on the face → GA net income **232,915** · income tax
  **12,088** — the same answer the hand-keyed 3,578 gave, with both figures
  now showing. That stale override was cleared; it double-counted once the
  pair landed.
- ⚠ **The stored per-asset Georgia figures were pre-s196 stale** on the test
  register — the engine was right, the register had not recomputed since the
  fix. The pair reads STORED values **deliberately**, so the 600S can never
  disagree with the 4562; the freshness comes from the federal recompute that
  precedes every sync.
- **Tests:** 10 new, **all four legs revert-proved individually** (formula
  totals · creation pull · resync hook · filed-copy roll-up). 984 green across
  the GA / depreciation / renderer / flow-assertion sweep. The GA-600S
  line-count pin in `test_state_filing` had gone stale (82 → 84).

---

## ⚠ Open items for Ken

1. **Other GA-600S returns will move when next recomputed.** The pair is now
   pulled for all of them, and one test entity has a real delta (federal
   44,444 vs Georgia 8,889 on a single bonused asset) where the return
   previously showed NO depreciation modification at all. Expected and
   correct — but it is a visible change on returns Ken did not touch.
2. **RS `R-GA500-DEPR` is stale** — still says GA "did NOT adopt OBBBA... GA
   uses its own §179 limit", contradicting the GA600S spec and Ken's HB 1199
   ruling. It is the rule that governs the auto-populate GAP 3 *is*. Fix RS-side.
3. **RS `GA600S R001` describes the addition as `ga_addition_bonus_depr`
   (bonus only)** — the presentation Ken rejected in favour of the gross pair.
   Changes no number (same net), but the spec and the code now disagree on
   the face. Fix RS-side.
4. **GA §179 does NOT cover certain REAL PROPERTY** (Ken, s196).
   `_calculate_state_ga` applies one flat `GA_179_LIMIT` with **no
   property-type test**. No test entity exercises it. **Needs an RS rule.**
5. **The Lacerte PDF importer does not read the GA depreciation pages** —
   `state_prior_depreciation` / `state_method` / `state_life` came in empty on
   all 30 test assets and had to be hand-keyed. A CSV path exists
   (`csv_depr_parser.py`); the PDF path does not.
6. **An asset with federal bonus history and NO keyed GA prior
   over-depreciates for Georgia** (full gross basis, no cap). A diagnostic is
   wanted. Not built.
7. **A GA 4562 is not produced.** Both 600S copies now say "attach GA 4562"
   and the filed copy's "(Attach Schedule)" lines carry the figures — the
   schedule itself does not exist yet.
8. **RS rule for the shareholder-side §179 disposition** — still blocks GAP 1.
9. **GA PTET base on a separately stated gain** — unchanged from s195.
10. **Client #2969** holds an empty duplicate individual entity — unchanged.
11. **Retire `reparent_business_entities`** — unchanged.
12. M1_5b is COMPUTED — a return with other book-income-not-on-K items needs
    an override carrying the total.

## Carried queue (unchanged from s196 unless noted)
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
**R-GA500-DEPR conformity correction** · **GA600S R001 gross-pair correction**.
