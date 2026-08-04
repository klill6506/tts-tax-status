# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-04, session 199. Ken's relayed ChatGPT backlog is at
7 of ~20. Migration returns.0235 (`RentalProperty.owner`) is latest and is
APPLIED on the shared DB.*

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
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

---

## ▶ RESUME HERE — the relayed backlog, item 8 of ~20

Ken interrupted the Georgia lane on 2026-08-03 to clear a backlog relayed from
ChatGPT, in five source files under `D:\tax-test-data\CC Code Changes\`:
`CC_A_M_REMAINING_BLOCKERS_2026-08-03.md` · `CC_CODE_CHANGES_BATCH-041.md` ·
`CC_CODE_CHANGES_BATCH-046.md` (+ a byte-identical `- 2.md` duplicate) ·
`CC_CODE_CHANGES_NZ_2026-08-03.md`. **Ken's order: work them in order, then
return to Georgia.**

**⚠ Unlike the s192 backlog, this one is mostly ACCURATE** — but 1 of the
5 done was refuted (see #5). Verify each claim before building; two prescriptions
so far pointed at the wrong layer.

### ✅ Done in s198 (all revert-proved, all deployed)
1. **Excess social security (A–M #1)** — `5f0adb2`. Schedule 3 line 11
   now computes from same-owner W-2 box 4; ties $939.
2. **Schedule D QOF answer (N-Z #1)** — `3379e91`. Allowlist gap only.
3. **§179 limit + K-1 income (046 #3)** — `3379e91`. Full election
   deductible, carryover $0.
4. **SS worksheet per-line rounding (N-Z #2)** — `20914e0`. Ties
   $4,233 / AGI $25,774.
5. **The $2 Schedule D case (batch-041) — REFUTED, no code change** — `c1f8308`. The
   worksheet was correct; the payload omitted 1099-DIV box 2b ($33 unrecaptured
   §1250). Payload + HOLD note corrected in `D:\tax-test-data\tmp\b041\`.

### ✅ Done in s199 (all revert-proved)
6. **GA RIE ← Schedule E page 1 (046 #2)** — `df515c1`, deployed. NEW
   `RentalProperty.owner` (T/S/J, migration 0235, db_default) +
   `schedule_e_page1_by_owner()`, which splits the post-§469 result so a
   suspended loss never reaches Georgia and whose parts reconstruct Schedule E
   line 26 exactly. Ties the filed 36,034. Root-fixed alongside: `_attribute`
   dropped half of any `joint` source on a non-MFJ return.
7. **`SCHED_L_DEPR_TIE` + the 4562 convention import (A–M #2/#3)** —
   PARTIALLY done, see the two open questions below. `SCHED_L_DEPR_TIE` no
   longer runs on forms without a Schedule L; the depreciation importer now
   corrects an explicitly-supplied convention that contradicts §168(d)(2).

### ▶ NEXT — item 8: `D_8995_STALE` on replace-document (A–M #4)
The subject return is an exact tie, committed and Filed, and fresh cleanup
still blocks on `D_8995_STALE`. Requested: recompute or clear stale derived
Form 8995 state during a replace-document commit, BEFORE the fresh diagnostic
run. (⚠ s181: `replace_documents` does NOT clear overrides — check whether that
is the same mechanism.) Subject named in the source file under
`D:\tax-test-data\CC Code Changes\` — NOT here; this file mirrors public.

### ⛔ TWO KEN DECISIONS BLOCK THE REST OF ITEM 7
Both are on real Filed returns; I did not guess. Details in "Open items" #12/#13.

### Then, in order
- **Diagnostics/lane interaction (2 left):** `D_8995_STALE` (A–M #4) ·
  1099-R **code W** + `D_RET_005` (A–M #6).
- **New lane sections over EXISTING engines (4):** 8880 (N-Z #3) · 2441
  (N-Z #7) · 8962/1095-A (N-Z #8) · 8283 item detail (A–M #5).
- **True builds (6):** Form 1310 (046 #1) · 8889/HSA (N-Z #4) · Schedule F lane
  `schedule_fs` (N-Z #5) · SS lump-sum election (N-Z #6) · 1099-G unemployment
  (N-Z #9) · multi-state import NC + MO (N-Z #10).

---

## ⚠ Open items for Ken

1. **The §179 base needs a rule for three components I would not guess.**
   Included: nonpassive K-1 ordinary income (Reg. §1.179-2(c)(6)(ii)-(iii)).
   **Excluded pending Ken/RS:** §707(c) guaranteed payments (compensation, not
   a distributive share — arguable), ordinary Form 4797 gain from an active
   business, and 1041 beneficiary K-1s. All three can only UNDERSTATE the base,
   never inflate a deduction. The RS 4562 spec names
   `active_trade_or_business_taxable_income` but never enumerates it.
2. **A rotted test pin was found RED on `main`** and repaired (s198):
   `test_sch123_scenarios`'s Schedule 8812 L_24 assertion hardcoded the EIC
   addend as 0, written before the Topic 7 EIC engine landed. Worth asking how
   long the suite has been red — nobody re-ran it.
3. **RS `GA600S R001`** still describes the GA-600S addition as bonus-only —
   the presentation Ken rejected in favour of the gross pair (s197). Same net,
   different face. Fix RS-side.
4. **RS `R-GA500-DEPR` is stale** — still denies GA §179 conformity,
   contradicting HB 1199 and the GA600S spec. It governs the auto-populate that
   GAP 3 *is*.
5. **GA §179 does NOT cover certain REAL PROPERTY** (Ken, s196);
   `_calculate_state_ga` applies one flat limit with no property-type test.
   Needs an RS rule.
6. **No GA 4562 is produced**, though both GA-600S copies now say "attach GA
   4562" and the filed copy's "(Attach Schedule)" lines carry the figures.
7. **Other GA-600S returns will move on their next recompute** (s197). One test
   entity has federal 44,444 vs Georgia 8,889 where the return previously
   showed no depreciation modification at all.
8. **The Lacerte PDF importer does not read the GA depreciation pages** —
   `state_prior_depreciation` / `state_method` / `state_life` import empty.
9. **An asset with federal bonus history and NO keyed GA prior over-depreciates
   for Georgia.** A diagnostic is wanted; not built.
10. **RS rule for the shareholder-side §179 disposition** — blocks K-1 GAP 1.
11. **GA PTET base on a separately stated gain** — unchanged from s195.
12. **The three A–M #2/#3 assets are not linked to an activity** (`flow_to`
    = "8825" but `rental_property_id` is NULL), so `D_4562_DEST` still fires.
    There is no single mechanical fix and I will not guess: return A has
    exactly ONE rental property (an auto-link would be unambiguous), return B
    has THREE (ambiguous — which one owns the asset?), and return C has NONE
    at all, with a laptop asset pointing at the rental arm (there
    `D_4562_DEST` looks CORRECT — it is probably a Schedule C asset). Ken's
    call: auto-link only when exactly one candidate exists, or leave all three
    for a preparer pick? Returns identified in the source file under
    `D:\tax-test-data\CC Code Changes\`.
13. **One of those stored assets still carries convention HY** and so computes
    $0. The s199 parser fix protects FUTURE imports only — a code fix does not
    refresh stored per-asset values (the s197 lesson). ⚠ Repairing it to MM
    makes the asset compute **1,942/yr**, and the return is currently recorded
    as an EXACT TIE — so the filed depreciation must already be reaching the
    face another way, and the repair could DOUBLE-COUNT. Needs Ken's eyes on
    the return before any data change.
14. **`SCHED_L_DEPR_TIE` can still false-fire on an entity return that is not
    required to complete Schedule L** (the Schedule B $250k test). Same shape
    as the 1040 defect, smaller blast radius; not fixed.
15. **Client #2969** duplicate individual entity · **retire
    `reparent_business_entities`** · **client-delete UI (there is NO path)** ·
    **duplicate guard is blind to entity names**.

## The three K-1 → individual gaps (Ken's unit, parked for the backlog)
**GAP 1** shareholder-side §179 disposition — BLOCKED on an RS 4797 rule.
**GAP 2** Georgia Shareholder Summary — buildable; **the Lacerte artifact is
not on disk**, ask Ken to re-send. s197's pair is what its 2b/3b would show.
**GAP 3** GA individual modifications carryover — needs open item 4 first.

## Carried queue (unchanged)
**Lane-schema-only (engine-complete)**: 8880 · 8962 annual · 2441 · 8863 ·
5695 · 8606 · 4797 · 6252 · 7203 · 1116. **True builds**: Sch F lane ·
8889/HSA · 7206 · 1099-G · 1099-MISC 8z · 8839 · 8824.
**Other queued:** TB default-template Rent/Taxes computed-line fix ·
depreciation-importer prior-split hardening · per-activity QBI carryforward ·
1099-R printed-aggregate fallback · RentalProperty `owner` + GA RIE rental
pull · DividendIncome US-obligation field · GA payment line from dated
payments · packet preflight · TB-import nav confirm dialog.
**RS agenda:** 8995 rental rows · R-EIC-WSB-SE · 4562 same-year-disposal ·
4797 shareholder-side §179 · GA §179 real-property carve-out ·
R-GA500-DEPR conformity correction · GA600S R001 gross-pair correction ·
**§179 active-trade-or-business income enumeration (s198)** ·
**`D_GA500_017` condition still lists Schedule E page-1 rental/royalty among
the un-pulled categories — line 13 IS pulled as of s199 (new)** ·
**no RS rule polices the depreciation CONVENTION at all** — `D_4562_CONVENTION`
and the s199 importer correction are both written straight against
§168(d)(2), logged here rather than silently diverged.
