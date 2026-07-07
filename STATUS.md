# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-07, twenty-seventh session (continuation of the regression detour). Unit:
**regression bugs FIXED + whole-dollar rounding LANDED engine-wide** — the app now reproduces
Lacerte regression return #1 with **0 differences across 59 compared lines** (refunds match to the
dollar) and **zero cent-valued FormFieldValues** on either return. Ken ruled: "a tax return should
never have cents anywhere." ⚠ ONE VERIFICATION STEP REMAINS (below) before this unit fully closes.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — regression clients by number only; identities in `D:\tax-test-data\`.

## ▶ RESUME HERE — MeF SOR intake FIRST (Ken-directed 2026-07-07 evening), pin suite alongside
**(0) FIRST ACTION — MeF: the SOR want-list packages LANDED** (Ken received the business rules +
the 2025v5.3 material today). Intake per `docs/mef/README_schemas.md`: drop the zips in `docs/mef/`,
file/hash/extract, record in `schema_versions.md`. Expected contents vs the want-list: 1040 2025v5.3
schemas+BR · 1040 v5.4 BR · 1120x TY2025v6.2 schemas+BR (→ one-line re-stamp of the 1120-S mapper
registry + revalidate) · 1065 2025v5.3 schemas+full BR · 1041 TY2025v5.3 schemas+BR — verify what
actually arrived against that list. Then continue the MeF track (Scenario-5 doc mappers, 1125-A
first). Ask Ken where he saved the downloaded files.

**(0b) IN PARALLEL — run the 25-test pin suite + broad batches** (background; SAFE alongside MeF
work, which is serialization/docs-only — but the never-edit-compute-.py-mid-run rule applies if any
engine change comes up). Two things still need a green run:
1. `pytest $(cat pin_nodes list) --reuse-db` — the 25 tests whose cent-string pins were updated to
   whole-dollar this session (node list: `tests/test_diagnostics.py -k "m1_3b"`,
   `tests/test_returns.py -k "OtherDeductions or LineItemDetails or RentalProperties or
   ComputeOverride or Compute1065 or Compute1120 or ShareholderDistributions or update_fields"`,
   `tests/test_1040x_baseline.py -k snapshot`, `tests/test_schedule_f_diagnostics_leg.py -k
   above_qbi`). Pins were updated by write-path analysis, NOT yet executed.
2. Broad entity/state batches (1120-S / 1065 / 1041 / GA-500 / SC / NC / AL / Sch L balance / 2210 /
   retirement / K-1 / render suites). ⚠ RUN IN SMALL BATCHES (2-3 files), background, --reuse-db —
   a 5-file batch of heavy DB suites ran 3+ hours on the pooler this session and was killed.
   ⚠ NEVER edit compute .py files while a DB suite runs (imports are cached → results tainted).
3. Then delete the stale-pin risk note here and tick the BUILD_ORDER items fully.

**(A) Spine: S-11 1041 legs 6-8 unchanged** — leg 6 GA Form 501 next.
**(B) MeF entity e-file (∥): 1120-S ATS Scenario-5 doc mappers unchanged** — 1125-A first.
SOR mailbox watch continues.

## What landed this session (all committed)
- **Bug 1 — Sch E released prior-year passive loss** (`compute_8582.per_activity_allocation`): BOTH
  branches now grant every activity its full deduction (loss + prior) when not in the Part VII pool
  — incl. an activity whose own income absorbs its prior loss. Sch E line 21 = face net (no prior);
  released loss lands on line 22 → 26. Specs verified (FORM_8582/SCHEDULE_E — conforming fix).
- **Bug 2 — 8960 line 4a**: auto-feeds Sch 1 line 5 (override `e8960_rental` takes the row manual);
  auto 4b backs out non-§1411 slice (`schedule_e_non_1411_income`: self-rental recharacterized +
  nonpassive non-PTP K-1 net). Render face mirrors (4a/4b/4c added to f8960 field map). ⚠ RS spec
  amendment wanted (spec is draft; 4a listed as bare preparer entry) — handoff:
  `docs/rs_handoff/2026-07-07_8582_sche_8960_lacerte_regression_fixes.md`.
- **Bug 3 (found during verification) — 1040 line 24 stale total**: mid-chain Sch-2 re-sums (8960
  NIIT, 6252, HSA) update the line-23 FFV through their own row fetches; `values["23"]` was never
  refreshed → line 24 totaled $10 short. Fixed: refresh_from_db sync before the second downstream
  pass (compute.py).
- **Whole-dollar rounding engine-wide**: ~131 quantize sites flipped to `Decimal("1"),
  ROUND_HALF_UP` across every compute module + views rollups + 8812/Sch-1A writers + the 25c roster.
  KEPT at cents (deliberate): FICA penny tolerances (w2_fica_check), §72 per-payment amount, SC1040
  proration ratio, 8962 line-7 applicable figure, §179 share allocation internals. K-1 allocators
  were already whole. PDF `format_currency` always printed whole (`,.0f`) — unchanged.
- **Test pins updated** (25 cent-string pins → whole; overrides/direct-writes/in-memory fakes left).
- **Read-only audit (Ken/Chat request): client-table inventory** of the shared Supabase →
  `docs/audits/2026-07-07_client_table_inventory.md`. Headlines: no canonical shared clients table
  (1099 = parallel universe; 27/68 filers TIN-match the central record, only 5/321 recipients); NO
  client-number-like column exists anywhere; portal rides `clients_client` via portal_portaluser;
  **NEW `checkin` schema** (2,566 free-text-identity events, zero FKs — third client universe);
  central record still has no primary-individual SSN column. SUITE_CONTRACT deltas noted in the
  audit file (its MCP-stale-password note is obsolete). Feeds the upcoming client-ID project.

## Active gates
- **Flow-assertion gate — 422 passed** (after all changes; two getsource pins were preserved by
  keeping the pinned source lines intact — see auto-memory).
- 8960 compute leg 13 passed · new `test_pal_release_and_8960_rental.py` 6 passed (pure) · client
  suite 275 passed · py_compile + `manage.py check` clean · regression-return-#1 comparator exit 0 (0 diffs).
- ⚠ Pin suite + broad batches = the RESUME item above; not yet run against final code.

## Lacerte regression harness (built session 26, unchanged)
`scripts/lacerte_regression/` parse → load → compare; procedure in its README. Return #1 now fully
green — rerun the comparator after any engine change: it is the fastest full-return smoke test.

## ▶ RS / compute follow-ups
- **RS spec amendments from this unit**: R-8960-INCOME 4a auto-feed (+ 4b auto back-out); RS
  integrity gate `check_schedule_e_8582_integrity.recompute_per_activity` re-verify vs the
  gain-activity case; RS scenario pins re-pin whole-dollar on next touch. Handoff doc above.
- 8582 cosmetic: app fills Parts II/III when line 3 ≥ 0 (form says stop) — fold into a later leg.
- 8960 render face line 15 not shown (pre-existing, cosmetic).
- Carried items unchanged (S-11 legs 6-8; S-6 §172/6198; f1065 M-1 nuance; GA-700/NC/AL/SC/8867;
  S-4 staged assertions; see session-26 STATUS in STATUS_ARCHIVE).

## ▶ Waiting on Ken / external
1. SOR mailbox watch (want-list sent 2026-07-07).
2. IFA-upload 1040 scenarios 8 + 5 rebuild (pre-§12.5).
3. TY2025 4868 schema (carried).
4. ~~Regression return #1 dependent DOBs~~ DONE — Ken entered them (8812 shows 4 QCs, CTC $0 ✓).
5. ~~Rounding policy decision~~ RESOLVED — Ken ruled whole-dollar everywhere (landed this session).

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
