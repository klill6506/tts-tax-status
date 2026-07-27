# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-27, session 122 (QA Batch-001 item 15 — source-summary
entry basis **SHIPPED**; RS `7cdf804`; app `22a7dd7`; migration 0220 applied
BOTH DBs via the deploy; deploy VERIFIED live; `seed_rules` run BOTH DBs after
the deploy).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s119–s121 detail is archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s122 SHIPPED QA Batch-001 item 15 (Option A) end to end** — Ken approved the
push in-session. Deploy verified live, migration applied, rules seeded, and the
whole path exercised on a real demo return. **Nothing pending.**

**Live acceptance on the deployed build (demo return, then restored):** clicking
"+ Add a source-packet total" created a dividend row with **no payer name**
showing "Source summary — no payer detail", and the return-level banner appeared
("1 dividend record entered from a source packet"). The QA-8621 amounts were
ACCEPTED (200) — qualified 12,533 · capital gain 32,686 · foreign tax 68 ·
§199A 43 — while box 1a was REFUSED (400) with the reason. After recompute:
**line 3a = 12,533 · line 7 = 32,686 · line 16 = 61.** Line 16 computing is the
whole point: that case used to leave the tax BLANK, which is what made the
fabricated payer load-bearing. A fresh diagnostics run fired both
`D_INTDIV_013` and `D_INTDIV_014` through the real runner.

### What the audit changed (fifth session running)

Option A was written as "add a flag, downgrade the nagging detail warnings to
INFO." Parsing **all 734 diagnostics** found essentially nothing to downgrade:
Delvio never demands a payer address, and what it does demand is either
legally required or needed to compute a number. Exactly ONE genuine nag
existed (`D_INTDIV_011` fired "payer EIN is blank" with an EMPTY payer name on
a conversion total that has no payer by construction) — now excluded. And the
**8949 broker-summary leg was already built and already correct**, so no flag
was added there.

**The real defect was ENTRY, not severity.** A return-level total could not be
held without inventing a payer, and the invention was load-bearing:
- the fabricated payer **printed on Schedule B Part II** and was
  **transmitted in the MeF XML as a payer element** (QA return 8621 shipped
  the literal string `SOURCE TOTALS - DETAIL OMITTED`);
- typing the capital gain distributions onto 1040 line 7 instead left
  `exception_1_state.sum_2a == 0`, so `route_line_16` returned BLOCKED and
  **line 16 — the tax — went blank**. There was no honest way to make that
  return compute.

**The rule** (RS `R-AGG-SUMMARY`), from the 2025 Schedule B face transcribed
the same day: it itemizes by payer **exactly two** amounts — taxable interest
(Part I line 1) and ordinary dividends (Part II line 5). Everything else is a
return-level figure no form attributes to a payer. A conversion total may carry
those; it may never carry the two the law requires listed, nor an adjustment to
one (`D_INTDIV_012`, blocking). The flag never loosens a legally required
listing.

**KEN RULED in-session 2026-07-27** (JUDGMENT ITEM 7): a source-summary record
has no boxes 2b/2c/2d to inspect, so Exception 1 condition (2) computes on the
preparer's assertion, with `D_INTDIV_013` (info) recording that the check was
**vacuous** rather than satisfied. Blocking was rejected on the record — it
leaves the return with no computed tax, the exact pressure that produced the
fabricated payer.

On 8283 rows the flag changes the **wording, never the verdict**: §170(f)(11)
makes the item facts a condition of the deduction, so `D_8283_005/007` stay
blocking and keep s121's missing-fact listing. That closes item 16's deferred
fourth bullet.

**Also fixed (off-scope, never silent):** RS carried **two contradictory
versions of scenarios ID-G1/ID-G2** — the pre-Topic-9 pair asserting the
retired `D_INTDIV_001/002` block, and the Schedule-D pair that replaced it. The
Topic 9 leg authored its corrections under NEW `scenario_name`s and
`update_or_create` keys on the name, so the originals were orphaned rather than
replaced. Undetected for six weeks because this repo's spec mirror was last
refreshed 2026-06-12 and carried no ID-G scenario at all, so the
spec-parametrized runner had never executed either one.
`_retire_topic9_superseded()` now deletes them; both survivors run and pass.

**Gates green:** NEW server `test_source_summary_item15.py` **12** (incl. the
QA 8621 acceptance case verbatim + negative controls on both the guard and the
assertion arm) · `test_intdiv_scenarios` **29** · Schedule B render + flow
assertions **521** · combined server run **584** · NEW client
`sourceSummaryItem15.test.tsx` **11** · vitest **536/536** (was 525) · tsc
**52 = the recorded baseline exactly**, none in touched files. RS harness green,
baseline-checked on clean HEAD first, then proven able to fail via two negative
controls.

**One PRE-EXISTING unrelated failure**, confirmed by stash-and-rerun on clean
HEAD: `test_topic3_input_leg::test_exception_1_assertion_drives_line_7a`
(`FormFieldValue.DoesNotExist`). Untouched, reported, not inherited as "noise".

## ▶ NEXT (cold-start pointer)

**item-6-P1 GA residual** (BLOCKED on the two GA
REVIEW_QUEUE questions) · **2210 reconciliation panel**. Option B (per-form
"where did this number come from" provenance view) stays **deferred, not
dropped** — raise it only after A has run on real conversions.

Batch-001 is now **12 of 16** done; opens = GA residual · 2210 panel · the two
QA cases item 15 deliberately did NOT cover (W-2 boxes 3/5 unavailable, and a
Schedule C net loss with no detail — separate record types, separate decisions).

## Known follow-ups from s122 (tracked in DEFERRAL_AUDIT)
- The reconciliation view is the **banner + per-row control**, not yet a
  separate panel riding the s112 manifest. The banner answers "which records
  are summarized and does anything block filing"; a fuller panel is worth
  building only once real conversions show what else preparers ask for.
- `entry_basis` is on interest / dividend / 8283 rows. **8949 reuses its own
  `is_summary`** by design; `entry_basis_of()` reads both. W-2 boxes 3/5 and
  the Schedule C "net loss with no detail" cases from the QA reports are NOT
  covered — separate record types, separate decisions.
- Schedule B payer ORDER differs between the printed face and the XML
  (pre-existing, harmless, logged in REVIEW_QUEUE — deliberately not fixed).

## ▶ Waiting on Ken / external
1. s122 ratification (REVIEW_QUEUE): the source-summary listing line,
   specifically the nominee/accrued/ABP **adjustment** arm, which is my call.
2. s121 ratification: the `D_8283_017` severity ladder (conservation arm).
3. s118 ratifications: §280F AMT-arm derivation · GA no-bump table.
4. s115 ratifications: 8962 Part IV blank-pct · line 34/4-row cap · line-9 marriage-alt.
5. s114 ratifications: the 8867 rebuild's three judgment calls.
6. s113 ratifications: D_GA500_002 realignment · 2210 flat-7% · 7206 partner-arm scope.
7. Item-6-P1 GA residual — BLOCKING questions: GA line 5 filing status from
   federal? · couple the GA deduction election to the federal election?
8. s112 ratification: manifest-aware RS amendment (mechanism only).
9. 86 backfill review rows (83 effective) · S-24 hub-ein blanking · auth env
    vars · A2A WSDL · WISP · SEC-5 · Resend · role assignments · e-services ·
    CAF · ERO EFIN/PIN · beta clauses · older ratifications (s110 · s106 ·
    s101(4) · s100(3) · s99a · s97 · s96(4) · s95..s72).

## Active gates
- **Deploy: `22a7dd7` VERIFIED live in-session** — prod + demo bundles rolled
  `index-BhoKt46x.js` → `index-CVBMlEDz.js`; markers `Add a source-packet
  total` / `Source summary` / `entered from a source packet` each ×1 against
  the 0-hit pre-push baseline. Nothing pending.
- **DB state: migration 0220 APPLIED to BOTH DBs via the deploy** (`migrate
  --check` clean). Additive — three `entry_basis` columns (default `detail`);
  the two `payer_name` AlterFields are Django-level only, no column change,
  existing rows untouched and unchanged in behavior.
- **Rule catalogue:** `seed_rules` run on **BOTH DBs AFTER the deploy** (the
  required order — seeding first would turn the unresolvable `rule_function`
  into a red finding on every 1040). Verified: `D_INTDIV_012` (error) / `013`
  (info) / `014` (info) present + active on both; D_INTDIV family 14/14.
- **RS:** `1040_INTDIV` at `7cdf804` (18 rules / 11 diagnostics / 56 facts /
  18 scenarios); deployed export verified and mirrored verbatim to
  `server/specs/intdiv_spec.json`.
- ⚠ FA-1040-4835-06 drift (chip `task_0cf10eac`, unchanged since s113).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
