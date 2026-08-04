# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-04, session 209. **MIXED-PILOT #2 is BUILT and
DEPLOYED** (`c179e0e`): the K-1 item K1 §752 liability share now carries
Beginning AND Ending columns end to end — migration **returns.0236 is
latest** (applied to the shared DB this session, db_default=0 per the s190
skew rule). NEXT: **MIXED-PILOT #7 (K-1 basis/at-risk allowed-loss
worksheet)**, then the 1040 lane resumes at **8606 (046 #10)**.*

*s208c (2026-08-04): the PUBLIC-mirror PII history purge EXECUTED on Ken's
order — 4 tip commits squashed + force-pushed (the sanctioned exception),
local reflog expired + gc'd, zero hits across all refs. Pre-purge bundle:
`D:\tax-test-data\tts-tax-status-prepurge-20260804.bundle` (contains the
PII — never commit it anywhere). Residual: GitHub keeps the 4 old SHAs as
dangling objects until its own GC; a Support ticket clears them fully.*

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
⚠ NEW (s208): FOUR same-day deploys during a live pilot session produced
the pilot's #5/#6 symptoms (stale-chunk self-heal reloads + restart
windows). Batch pushes when a pilot is actively entering.

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

---

## ▶ RESUME HERE

### ✅ Done in s209 — MIXED-PILOT #2: item K1 BOY/EOY liability columns
The 2025 K-1 (1065) item K1 prints Beginning AND Ending per §752 bucket;
the app modeled one share. Built end to end (`c179e0e`, deployed):
- **DB**: Partner gains `liability_recourse_boy` / `liability_qnr_boy` /
  `liability_nonrecourse_boy` (**migration 0236**, db_default=0 —
  APPLIED to the shared DB this session); serializer carries them.
- **Renderer**: all six item K1 AcroForm boxes feed (the f1065sk1_2025
  map already had them; only the EOY trio was wired). Render pin added.
- **Roll-forward**: NEW `_populate_partners_from_prior_year` on the
  return-creation chokepoint — copies active partners from the prior
  **in-app** 1065 (there is NO PriorYearReturn partner parsing): prior
  EOY → BOY on item J %s, item K1 liabilities, item L capital;
  current-year flows zero; special allocations deliberately NOT carried
  (§704(b) is per-year). Activates next season (needs a prior in-app
  1065); 7 tests prove it now.
- **Diagnostic**: NEW `D_K1_ITEMK_BOY` warning — BOY ≠ prior in-app EOY
  (TIN-digit match, else exact name; silent with no prior year).
  seed_rules run locally; rides build.sh on deploy anyway.
- **Both chromes**: Slate Partners screen gets the item-J-style
  beginning/ending grid (capital account moved to its own full-width
  span); legacy FormEditor card gains the BOY row.
- **MeF**: N/A — no 1065 MeF builder exists. **Import**: the Partner API
  surface; the entity back-entry lane has no 1065 support yet (when it
  gains one, the BOY fields must join its allowlist).
- **Ahead-of-spec**: RS `R-K1-ITEM-K` models the ENDING share only — the
  BOY columns are the s204-class Ken-sequenced surface; spec edit on the
  RS agenda below.
Live-verified on the dev server against a real 1065 (2 partners): all
six cells render with data-field wiring, a BOY commit PATCHes 200 and
survives a hard reload (the s208 `?tab=partners` deep link held too);
test value reset to 0 afterward (both PATCHes 200). Gates: 42 new/
touched server tests + flow/allocator/render sweep **556** · client
vitest 20 (2 files) · tsc 0.

### ✅ s208 (compact — full detail in STATUS_ARCHIVE)
The MIXED-PILOT batch, quick wins first: #5 REFUTED→the `?tab=` URL fix ·
#1 1065 K-1 largest-remainder residual dollar (1120-S keeps
last-owner-absorbs — RS ruling on the agenda) · #4 QBI_W2_WAGES reviewed
override + D_SCHK_QBI_W2_OVERRIDE · #6 failure+Retry on three lying
screens (remainder = a designed build, queued) · #3 REFUTED (GA-700
exists; verify vs the pilot fixture still open) · 4 rotted pins repaired.

### ▶ NEXT: MIXED-PILOT #7 (true build), then the 1040 lane
**#7 K-1 basis/at-risk allowed-loss worksheet (1040 side)** — the
basis-limited checkbox only FLAGS today; the full box-1 loss routes to
Schedule E. Build the worksheet (beginning basis · additions ·
distributions · current loss · allowed · suspended c/f), route only the
allowed loss, keep QBI un-double-limited, persist the suspension.
*(Adjacent to the parked K-1→individual GAP family — consider designing
together.)* Then the 1040 lane resumes: **8606 (046 #10)**, then 047
items 16-20 · the true builds.

### Artifacts left on the shared DB (deliberate, s208/s209)
- **Migration 0236 APPLIED to the shared DB (s209)** — three Partner BOY
  liability columns, db_default=0; deploys re-run it as a no-op.
- seed_rules run locally s209 (registers D_K1_ITEMK_BOY); rides build.sh.
- The s209 live-verification value on 409 FAMILY HOLDINGS was RESET to 0
  (both PATCHes 200) — no droppings.
- The subject 1120-S (s208) has its GA-600S + the 103,365 QBI_W2_WAGES
  override + its D_SCHK_QBI_W2_OVERRIDE warning awaiting source-verified.

### ⛔ TWO KEN DECISIONS STILL BLOCK A–M ITEM 7 (the 4562 assets)
Unchanged from s207 — see "Open items" #13/#14.

---

## ⚠ Open items for Ken
*(carried from s207 except as noted)*

1. **The §179 base needs a rule for three components I would not guess.**
   Included: nonpassive K-1 ordinary income (Reg. §1.179-2(c)(6)(ii)-(iii)).
   **Excluded pending Ken/RS:** §707(c) guaranteed payments, ordinary 4797
   gain from an active business, 1041 beneficiary K-1s. All three can only
   UNDERSTATE the base. The RS 4562 spec never enumerates.
2. **Rotted pins keep surfacing** — s198 found one, s208 found FOUR more
   (red on main for weeks; scoped sweeps never touch them). The full
   server suite (~6,900) is not run routinely. Worth a weekly full-suite
   cron or a pre-push canary set.
3. **RS `GA600S R001`** still describes the addition as bonus-only — the
   presentation Ken rejected for the gross pair (s197). Fix RS-side.
4. **RS `R-GA500-DEPR` is stale** — denies GA §179 conformity vs HB 1199.
5. **GA §179 does NOT cover certain REAL PROPERTY** (Ken, s196) — needs an
   RS rule; `_calculate_state_ga` applies one flat limit.
6. **No GA 4562 is produced** though both GA-600S copies say "attach GA
   4562" — now ALSO a MIXED-PILOT #3 follow-up (Form 700 packets include one).
7. **Other GA-600S returns will move on their next recompute** (s197).
8. **The Lacerte PDF importer does not read the GA depreciation pages.**
9. **Federal-bonus-history + no keyed GA prior over-depreciates for
   Georgia** — diagnostic wanted, not built.
10. **RS rule for the shareholder-side §179 disposition** — blocks K-1 GAP 1.
11. **GA PTET base on a separately stated gain** — unchanged from s195.
12. ~~046 #8~~ — RULED + BUILT (s199e). **RS `R-AGG-SUMMARY` spec edit**
    still pending on the RS agenda.
13. **The three A–M #2/#3 assets are not linked to an activity** — return A
    has exactly ONE rental (auto-link unambiguous), return B has THREE,
    return C has NONE (its laptop asset probably belongs to Schedule C).
    Auto-link only when exactly one candidate, or preparer pick?
14. **One stored asset still carries convention HY** and computes $0 — but
    the return is recorded as an EXACT TIE, so a repair to MM (1,942/yr)
    could DOUBLE-COUNT. Needs Ken's eyes before any data change.
15. **`SCHED_L_DEPR_TIE` can false-fire on a no-Schedule-L entity return**
    (the B11 gate exists on the s205 D_1120S_L_* family, not this older rule).
16. **Client #2969 duplicate** · **retire `reparent_business_entities`** ·
    **client-delete UI (no path exists)** · **duplicate guard blind to
    entity names**.
17. ~~M-2 AAA distribution cap~~ — RULED + BUILT (s205b). M-2 line-7 FACE
    stays IRS-capped — pin equity/Schedule L, not that face.
18. ~~February 'false' residue~~ — RULED + EXECUTED (s205b); rollback
    snapshot in tax-test-data.
19. **The N-Z #8 8962 target (s204)** — relayed full repayment contradicts
    i8962 Table 5 at 305% FPL. Verify against the actual PDF at entry;
    escalate, don't force the tie.

## The three K-1 → individual gaps (Ken's unit, parked for the backlog)
**GAP 1** shareholder-side §179 disposition — BLOCKED on an RS 4797 rule.
**GAP 2** Georgia Shareholder Summary — buildable; **the Lacerte artifact
is not on disk**, ask Ken to re-send.
**GAP 3** GA individual modifications carryover — needs open item 4 first.
*(MIXED-PILOT #7's basis worksheet is adjacent — consider one design.)*

## Carried queue
**Lane-schema-only (engine-complete)**: 8863 · 5695 · 8606 · 4797 ·
6252 · 7203 · 1116. *(8880 s202 · 2441 s203 · 8962 s204 · 8283 s206.)*
**True builds**: Sch F lane · 8889/HSA · 7206 · 1099-G · 1099-MISC 8z ·
8839 · 8824 · **MIXED-PILOT #7 (basis/at-risk worksheet) · MIXED-PILOT #6
remainder (idempotent field-save lanes + newer-edit-wins + per-field
unsaved indicator + bootstrap retry)**. *(#2 item K BOY/EOY — DONE s209.)*
**Other queued:** TB default-template Rent/Taxes computed-line fix ·
depreciation-importer prior-split hardening · per-activity QBI carryforward ·
1099-R printed-aggregate fallback · DividendIncome US-obligation field ·
GA payment line from dated payments · packet preflight · TB-import nav
confirm dialog · ⚠ 47 Slate screens / 88 bare checkbox sites + tri-state
selects still lack SlateCheck (DEFERRAL_AUDIT).
**RS agenda:** 8995 rental rows · R-EIC-WSB-SE · 4562 same-year-disposal ·
4797 shareholder-side §179 · GA §179 real-property carve-out ·
R-GA500-DEPR conformity correction · GA600S R001 gross-pair correction ·
§179 active-trade-or-business income enumeration (s198) ·
`D_GA500_017` stale condition (line 13 IS pulled since s199) ·
no RS rule polices the depreciation CONVENTION ·
R-AGG-SUMMARY threshold edit (Option A, s199e) ·
impossible-bonus diagnostic (s205 #4) · RS 7203 Part III map (draft spec) ·
**NEW (s208): ratify the 1065 K-1 residual mechanism (largest-remainder)
— the 1120-S R-K1-ROUND uses last-owner-absorbs; same RECON invariant,
different tie-break** · **NEW (s208): author the Rev. Proc. 2019-11 W-2
wage worksheet spec (MIXED-PILOT #4's full ask) before building** ·
**NEW (s209): extend `R-K1-ITEM-K` with the three BOY facts — the 2025
item K1 prints Beginning/Ending per §752 bucket; the app now models both
(s204-class ahead-of-spec, Ken-sequenced); D_K1_ITEMK_BOY should also
join the spec's diagnostics**.
