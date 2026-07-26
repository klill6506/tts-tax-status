# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-25, session 111 (the ChatGPT back-entry QA batch — six
defects, all shipped: Sch E depreciation flow · clean 1099-R drafts · durable
field clears · GA-500 12a pull · in-app delete confirm · care-of address line).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s110 is archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s111 (2026-07-25, Ken-relayed ChatGPT/Codex QA batch): ALL SIX DEFECTS
SHIPPED.** Commits `6c014e6` (items 1-3) + `5422101` (items 4-6), pushed
`9186d03..5422101`. Migration `returns.0212` (additive `Taxpayer.in_care_of`,
BOTH DBs). **Audit-before-accepting held again (the s108 lesson):**

1. **Sch E depreciation (item 1): THE ENGINE PATH ALREADY EXISTED AND WAS
   RIGHT** — `flow_to="8825"` + the `rental_property` FK feeds Sch E line 18
   on a 1040, exactly as RS `R-SCHE-NET` specs ("depreciation rides the
   existing rental_property_id linkage retargeted for the 1040"). What
   shipped: the worksheet now speaks Schedule E on a 1040 (labels, group
   headers, flow option — same stored value, NO migration), the Sch E line-18
   input gets the read-only "Calculated" guard the entity editor had, the
   exact QA regression is pinned (187,365 building, PIS 2023-01 → 6,813 →
   net 5,187 → Sch 1 line 5), and a REAL stale-flow bug was fixed: deleting
   or repointing an asset now pulls its depreciation back OUT of the old
   target (aggregate_depreciation early-returns on zero assets, so the old
   amount survived forever).
2. **1099-R inheritance (item 2): a DOM-identity bug, not a data bug** — the
   draft card's constant React key + uncontrolled inputs meant "+ Add" was
   reconciled as the SAME element; the new card showed the old row's values.
   Fix = a `draftSeq` generation in the key (remount once per NEW draft,
   never mid-entry). Schedule D's draft row had the identical latent defect.
3. **Clearing reverts (item 3): two real mechanisms** — payer enrichment
   re-ran on every EIN blur and refilled just-cleared blanks (now only on an
   EIN *change*); and the shared 1099-INT/DIV grids sent `v || null`, which
   400s on the blank=True-but-NOT-nullable text columns and non-null money
   → the row silently kept its old value. Text now clears to "", nullable
   money to null, non-null money to "0" (`SlimColumn.clearValue`).
4. **GA-500 12a (item 4): the RIE class again** — the election logic was
   right; 12a was never fed. New override-respecting pull writes 12a =
   federal Sch A 17 exactly when the federal return itemized (NEW
   `compute_schedule_a.federal_itemized_used()`), and back to blank when it
   stops. compute_ga500 also blanks the UN-taken 11/12c line. QA regression
   exact: Sch A 60,860 / MFJ → 12a/12c 60,860, GA taxable (60,860).
5. **Asset delete (item 5):** NEW `ConfirmDialog.tsx` replaces the native
   `confirm()` (busy guard = one DELETE per confirm; Escape; focus contract).
   27 other native confirm sites inventoried, deliberately not bulk-converted.
6. **Care-of line (item 6):** NEW `Taxpayer.in_care_of` threaded through
   serializer → proforma → taxpayer screen → 1040/GA/AL/NC/SC faces (ONE
   shared `compose_care_of_street()` — the faces have no c/o field, so it
   composes "% NAME street" at render time; stored street never modified) →
   MeF `InCareOfNm`, where an entered name also satisfies the
   personal-representative case non-MFJ decedent returns used to refuse on.

**LIVE-VERIFIED ON THE DEMO PROJECT** (production never touched): the full
Sch E regression on John & Judy Jones (asset → 6,813 on-screen line 18
read-only "Calculated" → net 5,187 → Sch 1 line 5 → AGI 35,492→40,679, then
delete-confirm via the new dialog → depreciation pulled back to 0) · a
scripted rapid-entry 1099-R landed on ONE record and the second card opened
fully blank · care-of set → survived a hard reload → cleared → DB blank.
Every scratch record deleted; the return re-verified byte-identical
(AGI 35,492, zero rentals/assets/1099-Rs).

**▶ NEXT (cold-start pointer): unchanged — the 8962 manifest+diagnostics
leg** (backlog #12, ONE authoritative generated-form manifest driving Forms
view, e-file packaging and diagnostics). The Codex/ChatGPT entry fleet can
re-check Joe Bennett + the 1099-R returns once the deploy is verified (see
Active gates).

## ▶ Waiting on Ken / external
1. **s111 ratifications (REVIEW_QUEUE):** GA deduction election coupled to
   the federal election · pull GA line 5 filing status from federal? ·
   1099-INT "Seller-financed" currency cell sits over a Boolean field.
2. **86 backfill review rows** (`backfill_review.csv`) — now 83 effective.
3. **S-24 hub-ein blanking leg** (s97, unblocked) — awaiting explicit go.
4. Auth env vars (s94) · A2A WSDL · WISP (s96) · SEC-5 [EXT] · Resend (s83) ·
   role assignments (s84) · e-services reply · CAF (s69) · ERO EFIN/PIN (s94) ·
   beta clauses (s96).
5. **Ratifications pending:** s110 (tri-state gates · QM PIN warning) · s106 ·
   s101 (4) · s100 (3) · s99a · s97 · s96 (4) · s95 · s94 · s93 · s89 ·
   s85/s84 · s83 · s76..s72.

## Active gates
- **NEW tests: server** `test_schedule_e_depreciation_flow` 4 ·
  `test_clear_saved_fields` 6 · `test_ga500_itemized_pull` 5 ·
  `test_care_of_address` 11; **client** `scheduleEDepreciationFlow` 8 ·
  `clearSavedFields` 10 · `depreciationDeleteDialog` 6 · `careOfAddress` 4 ·
  retirement1099RDraftRow 14→17 · scheduleDDraftRow 13→14.
- **Server bands: flow assertions 520 + GA-500 + Sch A + taxpayer-input =
  662 passed**; adjacent Sch E/topic8/SchF/mar30/depr-engine 122+28;
  efile/proforma/renderer batch **295 passed / 1 failed** — the failure is
  `test_manifest_is_valid_json`, one of the SEVEN KNOWN pre-existing s108e
  failures. ⚠ No full server suite this session (s108e's 6,192/7/21 stands).
- **vitest 459** (was 427) · **`npx tsc --noEmit -p tsconfig.renderer.json`
  52 errors — the pre-existing baseline, zero new** (the bare `tsc --noEmit`
  is still a no-op; never trust it).
- **Migration `returns.0212`** (additive) applied to BOTH DBs. No seeder
  changes; `seed_rules`/FormDef reseeds NOT needed this session.
- **Spec mirrors refreshed verbatim from deployed RS exports:**
  `schedule_e_spec.json` · `form_4562_spec.json` · `500_spec.json` (drift was
  authority-source/test text only; no rule changes).
- ⚠ **DEPLOY VERIFICATION IN FLIGHT at close:** pushed `5422101`; the
  zero-hit baseline on live bundle `index-D-t_Kwp7.js` was taken BEFORE the
  push for three s111-only markers (`In care of (c/o)` ·
  `Schedule E rentals (Line 18)` · `the return recalculates after deletion`).
  **Verify: grep the new prod `/assets/index-*.js` for those markers** before
  telling the entry fleet to re-check. Server side needs no deploy step
  (migration applied directly; no seeders).
- Follow-up chip filed: the 1099-G card still has the pre-s108
  POST-per-blur shape (task chip pending Ken's click).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
