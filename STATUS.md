# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-04, session 205 (the 1120-S pilot interjection).
Ken's relayed 1040 backlog is PAUSED at 17 of ~37 (next: 8283); Ken
interjected ChatGPT's 1120-S pilot handoff — items 1–7 (release blockers)
are DONE this session (one refuted as a data mistranscription, two
queued for Ken); items 8–12 (the speed builds) await sequencing.
Migration returns.0235 is latest and APPLIED; s205 adds NO migration.*

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

## ▶ RESUME HERE — the relayed backlog, 17 of ~37 done

### ⚠ BATCH-047 (arrived 2026-08-04, tax-test-data ROOT, items 11–20)
Not yet sequenced against the existing queue — live defects first per
Ken's standing ruling. 16–20 are the same lane-schema-only pattern as
s202–s204 (5695 · 4797 · 6252 · 8824 · 6251); 13/15 are true builds
(1099-MISC rows · 4952); 11/12 are model+everywhere builds (name
suffixes · non-50/50 MFJ INT/DIV owner allocation, the GA RIE feeder).

Ken interrupted the Georgia lane 2026-08-03 to clear a backlog relayed from
ChatGPT (five source files under `D:\tax-test-data\CC Code Changes\` + the
047 file at the root). **Ken's order: work them in order, then return to
Georgia.** ⚠ Mostly accurate, but FOUR prescriptions proved wrong-layer or
false — verify each claim before building.

### ✅ Done s198–s201 (all revert-proved, all deployed — detail in STATUS_ARCHIVE / BUILD_ORDER)
1–5 (s198): excess SS per person (`5f0adb2`) · Sch D QOF allowlist ·
§179 base + K-1 income · SS worksheet per-line rounding · the $2 Sch D
case REFUTED (payload omitted 1099-DIV box 2b).
6–11 (s199): GA RIE ← Schedule E page 1 (`df515c1`, mig 0235,
`schedule_e_page1_by_owner`, suspended losses never reach GA) ·
SCHED_L_DEPR_TIE gated on the form's own lines · 4562 convention
§168(d)(2) correction · D_8995_STALE refuted → one `qbi_engaged_db()` ·
1099-R code W by statute + D_RET_005 coverage gate · dividend
summary-row 1a half-refuted → failed-save revert channel + Ken's
Option A threshold ruling BUILT.
12–13 (s200/b): Sch C home-office election refuted at the prescribed
layer → `SlateCheck` optimistic checkbox (⚠ 47 screens / 88 bare sites
remain — DEFERRAL_AUDIT) · the blank-app unmount → `ScreenErrorBoundary`
+ `vite:preloadError` self-heal (`4047d07`).
14 (s201): "VARIOUS" 8949 date REFUTED at every layer — the subject
row's column (b) was blank; repaired live, 2 pins (`56b04d7`).

### ✅ Done in s202 (deployed `707a5ea`)
15. **Form 8880 joins the import lane (046 #5 = N-Z #3).** 8 `f8880_*`
    facts (RS spec verbatim) + deferral-override staging warning; the
    engine was complete since s149/s173. 8 tests incl. both source
    shapes. Subject return #4303 needed no repair.

### ✅ Done in s203 (deployed `d896b31`)
16. **Form 2441 joins the import lane (N-Z #7).** 6 `f2441_*` facts +
    `care_providers` row section + 3 staging warnings (claim-flag
    disengage, DCB/earned-income override-shadows-derive). Printed 9b =
    HOLD (spec models no prior-year fact). 9 tests incl. the PACE $1,200
    shape.

### ✅ Done in s204 (this session — commit pending push)
17. **Form 8962 / 1095-A joins the import lane (046 #4 = N-Z #8).** The
    engine has been complete since s106e (monthly + annual methods,
    Part IV allocations, SEHI iterative); only the lane was missing.
    Added: the 9 `f8962_*` Taxpayer facts (the RS spec's 6 input facts
    verbatim — live spec re-fetched 2026-08-04, semantically identical
    to the cache — plus the three s106e annual line-11 fields, the
    Ken-directed ahead-of-spec surface the 046 item explicitly asks
    for) + a NEW `form_1095as` row section (policy header + the 36
    monthly A/B/C columns) with Part IV `allocations` NESTED per policy
    (the model FKs the policy, not the return — the first nested child
    on a new row family; months 1–12 ordered, pcts 0.00–1.00 validated
    beyond model clean). NO migration. Staging checks: unknown
    `f8962_state` = ERROR (compute silently falls back to the
    contiguous FPL table); line-11 facts without `f8962_all_year_same`
    = WARNING (the form computes NOTHING and no diagnostic fires — the
    2441 claim-flag class); line-11 + 1095-A rows = WARNING (the
    methods never mix). Schema regenerated (8962 moved OUT of the
    NOT-SUPPORTED prose); handoff guide + AUTHORING_GUIDE s204 section
    + kickoff s204 addendum shipped — held 8962 packets are
    lane-eligible again, first one is its own smoke test. 12 new tests
    (7 staging + 5 commit) incl. the 046 #4 annual shape (8,222 /
    8,086 / 7,495 → net PTC 591 → Sch 3 line 9), the partial-year
    monthly no-annualizing pin, the Table-5-limited excess repayment,
    the ≥400% full repayment, and the 50% Part IV allocation halving
    the aggregate. Gates: staging+commit 104 · 8962 sweep 61 ·
    flow 521.
    **⚠ The relayed N-Z #8 regression target is CONTRADICTED by
    i8962:** it wants a full $4,632 excess-APTC repayment at household
    income $45,995 = 305% FPL, but Table 5 (TY2025, fetched live
    2026-08-04) caps a single filer under 400% at $1,625, and repayment
    is "the smaller of line 27 or line 28". Full repayment is correct
    only at line 5 ≥ 400. Pinned to the IRS table ($1,625), NOT the
    relayed number; the entry session must verify the filed
    line 5/28/29 against the actual packet (kickoff addendum warns).

### ✅ Done in s205 (this session — the 1120-S pilot's release blockers, items 1–7)
Ken interjected ChatGPT's first 1120-S back-entry pilot handoff
(`Delvio 1120-S pilot: verified fixes and speed improvements`, relayed
2026-08-04). Items 1–7 worked in order, verify-first:
1. **Odd-dollar 50% meals (P0) — CONFIRMED, fixed.** Both halves of
   x.50 rounded HALF_UP independently (22,397 → 11,199 + 11,199).
   `D_MEALS_NONDED` is now the RESIDUAL of the book total against the
   rounded deductible — RS R010's own invariant — on BOTH the 1120-S
   and 1065 formula tables; K16c/M1_3b/M2_5a all ride it. 6 tests.
2. **M-2 AAA distribution cap (P0) — ⛔ QUEUED FOR KEN** (open item 18):
   the pilot demands uncapping line 7 (full distribution, negative
   ending AAA), but i1120s p.50 verbatim says "Decrease AAA (but not
   below zero) by property distributions" and the cap is Ken-ratified
   (RS 1120S_M2 R002, 2026-07-13). Recommendation recorded below.
3. **Form 7203 Part III lines 42–46 (P0) — CONFIRMED, fixed.** The K
   map stopped at line 41: a cash charitable contribution reduced
   NOTHING. Lines 42–46 now feed from K12a+K12b / K12c / K12d / K12e /
   K16f (i7203 Part III col (a); the RS 7203 spec is a draft with no
   Part III map — ahead-of-RS, agenda'd). Verified the template is
   Rev. 12-2022 (the f7203_2025 field-map COMMENTS mislabel rows 38+ —
   comments only, widgets align). Carryforward FIELDS for 41–46 are a
   migration — DEFERRAL_AUDIT. 3 tests incl. the pilot's exact
   6,000+992 → 6,992 shape.
4. **GA depreciation conformity (P0) — REFUTED as prescribed; the DATA
   was mistranscribed; repaired live.** The phantom 9,796 GA
   subtraction was one asset keyed with 100% prior-year BONUS on a
   2024-09-10 acquisition — impossible (TY2024 bonus was 60%,
   §168(k)(6)); a full year-one write-off can only have been §179,
   which GA conforms to (HB 1199). Re-keyed `sec_179_prior`=40,000,
   recomputed, ran the GA-600S resync chokepoint: pair 6,660/6,660,
   GA income **228,377** = the pilot's required result, ZERO law
   change. (The pilot's "0/0" presentation ask contradicts Ken's s197
   gross-pair ruling; the net is identical — not re-litigated.) An
   impossible-bonus diagnostic → RS agenda.
5. **Schedule B 14b (P1) — the field EXISTS at every layer** (seeded,
   rendered behind 14a=Yes, test-pinned). Two real defects behind the
   report: the sub-question only appears after the SLOW save round
   trip serves 14a back (the s200 class), and **⚠ a February-era
   backfill wrote 'false' into ~324 returns' Schedule B booleans**
   (blank-≠-No violation — faces print "No" for questions never
   answered; open item 19). The pilot return's 14a/14b now both true.
6. **Client Info persistence (P1) — CONFIRMED, fixed, live-verified.**
   Both debounced autosave lanes (entity + return-info) CLEARED their
   pending timer on unmount — navigating within ~800ms of an edit
   silently discarded it (officers persisted: their lane commits on
   blur). NEW unmount flush fires the pending save instead.
   Live-proven on the pilot return via the dev server: value typed →
   navigate at 200ms → persists. ALSO: `create_state_return` now
   copies the federal header cluster (staff preparer, signature date,
   product/service, business code, S-election date, shareholder count,
   accounting method — only `preparer` was carried), so a new GA-600S
   no longer opens blank. 1 test.
7. **Schedule L balance diagnostics (P0 gate) — CONFIRMED, built.**
   The 1065 has had D_L_BALANCE_BOY/EOY since S-4; the 1120-S had
   NOTHING — a visibly out-of-balance Schedule L produced zero
   findings. NEW `rules_1120s_l.py` per RS 1120S_SCHL (live-fetched):
   D_1120S_L_BALANCE_BOY/EOY (**errors**, with the dollar delta),
   D_1120S_L_M2_TIE (warning), D_1120S_L_EXEMPT (info; B11 suppresses
   — the same gate open item 15 wants for SCHED_L_DEPR_TIE). Spec D007
   (item F ≠ L15d) holds BY CONSTRUCTION (the renderer fills item F
   from L15d); D004–D006 queued. ⚠ seed_rules must run on deploy
   (rides build.sh). 4 tests.
   Gates: s205 tests 14 · meals/7203/flow sweep 629 · 1065-L+MeF+face
   66 · tsc 0 · vitest 1,611.

### ▶ NEXT — Ken's call: the pilot's speed builds (items 8–12), or resume 8283
Items 8–12 are BUILDS, not fixes, and need sequencing against the paused
1040 backlog: ⑧ an 1120-S/GA-600S back-entry lane (the 1040 lane design;
multi-session) · ⑨ an S-corp answer-key/tie panel · ⑩ a packet
pre-indexer for 487-page exports · ⑪ stable automation IDs + atomic
save on the depreciation editor · ⑫ a one-click entity closeout gate
(the s191 1040 cleanup-gate sibling). Then the 1040 lane resumes at
**8283**, then 8606=046#10 · 047 items 16–20 · the true builds.

### ⛔ TWO KEN DECISIONS BLOCK THE REST OF A–M ITEM 7 (the 4562 assets)
Both are on real Filed returns; I did not guess. Details in "Open items" #13/#14.

### Then, in order
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
12. ~~046 #8's "Expected" vs `R-AGG-SUMMARY`~~ — **RULED (Option A,
    2026-08-04) and BUILT** (s199e). **RS `R-AGG-SUMMARY` still needs the
    spec edit** — on the RS agenda.
13. **The three A–M #2/#3 assets are not linked to an activity** (`flow_to`
    = "8825" but `rental_property_id` is NULL), so `D_4562_DEST` still fires.
    There is no single mechanical fix and I will not guess: return A has
    exactly ONE rental property (an auto-link would be unambiguous), return B
    has THREE (ambiguous — which one owns the asset?), and return C has NONE
    at all, with a laptop asset pointing at the rental arm (there
    `D_4562_DEST` looks CORRECT — it is probably a Schedule C asset). Ken's
    call: auto-link only when exactly one candidate exists, or leave all three
    for a preparer pick? Returns identified in the source file under
    `D:\tax-test-data\CC Code Changes\`.
14. **One of those stored assets still carries convention HY** and so computes
    $0. The s199 parser fix protects FUTURE imports only — a code fix does not
    refresh stored per-asset values (the s197 lesson). ⚠ Repairing it to MM
    makes the asset compute **1,942/yr**, and the return is currently recorded
    as an EXACT TIE — so the filed depreciation must already be reaching the
    face another way, and the repair could DOUBLE-COUNT. Needs Ken's eyes on
    the return before any data change.
15. **`SCHED_L_DEPR_TIE` can still false-fire on an entity return that is not
    required to complete Schedule L** (the Schedule B $250k test). Same shape
    as the 1040 defect, smaller blast radius; not fixed.
16. **Client #2969** duplicate individual entity · **retire
    `reparent_business_entities`** · **client-delete UI (there is NO path)** ·
    **duplicate guard is blind to entity names**.
17. **⛔ The M-2 AAA distribution cap (s205 pilot item 2).** The pilot
    demands M-2 line 7 carry the FULL Schedule K distribution and ending
    AAA go negative (−1,419 on the subject return), matching its source
    software's print. i1120s (2025) p.50, fetched verbatim: adjustment 3
    = "Decrease AAA (but not below zero) by property distributions" —
    the current cap implements the IRS instruction and is Ken-ratified
    (RS 1120S_M2 R002, 2026-07-13). **Recommendation: keep the cap, fix
    the BRIDGE** — the real pain is Schedule L: `L24d = Σ M-2 endings`
    loses the distribution excess (the §1368(c) cascade's "reduce any
    remaining shareholders' equity accounts"), so retained earnings
    shows 0 where the books say −1,419. Carrying the excess into L24d
    ties Schedule L exactly WITHOUT uncapping the M-2. The pilot's
    answer key would still no-tie on the M-2 line-7 FACE vs its source
    software — Ken decides which face we print.
18. **⛔ The February 'false' residue (s205 pilot item 5).** ~324 of 325
    1120-S returns carry `'false'` in Schedule B booleans (B14b et al.)
    from a February-era backfill — the blank-≠-No violation predating
    the s106 tri-state contract; faces print "No" for questions nobody
    answered. Proposed repair: blank every boolean FFV whose value is
    'false', updated_at in the Feb 2026 mint window, and not overridden
    — needs Ken's go (bulk data change, s193-class).
19. **The N-Z #8 8962 target (s204)** — the relayed full-$4,632 repayment
    contradicts i8962 Table 5 at the relayed 305% FPL (single cap $1,625).
    When the packet is entered: verify the filed lines 5/28/29 against the
    actual PDF. If the filed face REALLY repays in full under 400%, that is
    either a TaxWise nuance we're missing (e.g. the Pub 974 SEHI line-28
    computation) or a mistranscribed relay — escalate, don't force the tie.

## The three K-1 → individual gaps (Ken's unit, parked for the backlog)
**GAP 1** shareholder-side §179 disposition — BLOCKED on an RS 4797 rule.
**GAP 2** Georgia Shareholder Summary — buildable; **the Lacerte artifact is
not on disk**, ask Ken to re-send. s197's pair is what its 2b/3b would show.
**GAP 3** GA individual modifications carryover — needs open item 4 first.

## Carried queue (unchanged)
**Lane-schema-only (engine-complete)**: 8863 · 5695 · 8606 · 4797 ·
6252 · 7203 · 1116 · 8283. *(8880 done s202; 2441 done s203; 8962 done
s204.)* **True builds**: Sch F lane · 8889/HSA · 7206 · 1099-G ·
1099-MISC 8z · 8839 · 8824.
**Other queued:** TB default-template Rent/Taxes computed-line fix ·
depreciation-importer prior-split hardening · per-activity QBI carryforward ·
1099-R printed-aggregate fallback · DividendIncome US-obligation field ·
GA payment line from dated payments · packet preflight · TB-import nav
confirm dialog.
**RS agenda:** 8995 rental rows · R-EIC-WSB-SE · 4562 same-year-disposal ·
4797 shareholder-side §179 · GA §179 real-property carve-out ·
R-GA500-DEPR conformity correction · GA600S R001 gross-pair correction ·
§179 active-trade-or-business income enumeration (s198) ·
`D_GA500_017` condition still lists Schedule E page-1 rental/royalty among
the un-pulled categories — line 13 IS pulled as of s199 ·
no RS rule polices the depreciation CONVENTION at all (`D_4562_CONVENTION`
+ the s199 importer correction are both written straight against
§168(d)(2)) · **R-AGG-SUMMARY threshold edit (Option A, s199e)**.
