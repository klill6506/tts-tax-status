# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-27, session 122 (QA Batch-001 item 15 — source-summary
entry basis BUILT; RS `7cdf804`; app `1a02124` + `2d9a864`; **migration 0220
NOT applied, nothing pushed — awaiting Ken**).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s119–s121 detail is archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE — ONE DECISION IS WAITING

**s122 built QA Batch-001 item 15 (Option A) end to end. Everything is
committed locally and green. NOTHING IS PUSHED, because the push IS the
migration decision (the s119 rule) and migration 0220 adds columns the new
code selects.** Ken's go-ahead is the only thing outstanding.

**The ship sequence, once Ken says go** (order matters — s121's lesson):
1. `git push origin main` in `delvio-tax` (RS `7cdf804` is already pushed).
2. The deploy runs `migrate` → 0220 lands on **both** DBs.
3. **THEN** `seed_rules` on **BOTH** DBs — never before the deploy, or the
   unresolvable `rule_function` for `D_INTDIV_012/013/014` becomes a red
   finding on every 1040.
4. Verify the deploy: bundle-grep markers `Add a source-packet total` /
   `Source summary — no payer detail`. **Baseline not yet taken — take the
   zero-hit baseline BEFORE pushing.**
5. Live browser verify (blocked until 0220 is applied — the dev server shares
   the production DB, so the UI cannot be exercised before the migration).

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

After item 15 ships: **item-6-P1 GA residual** (BLOCKED on the two GA
REVIEW_QUEUE questions) · **2210 reconciliation panel**. Option B (per-form
"where did this number come from" provenance view) stays **deferred, not
dropped** — raise it only after A has run on real conversions.

Batch-001 is now **12 of 16** done; opens = 15 (shipping) · GA residual · 2210.

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
1. **THE PUSH + migration 0220 on both DBs** (see ▶ RESUME HERE).
2. s122 ratification (REVIEW_QUEUE): the source-summary listing line,
   specifically the nominee/accrued/ABP **adjustment** arm, which is my call.
3. s121 ratification: the `D_8283_017` severity ladder (conservation arm).
4. s118 ratifications: §280F AMT-arm derivation · GA no-bump table.
5. s115 ratifications: 8962 Part IV blank-pct · line 34/4-row cap · line-9 marriage-alt.
6. s114 ratifications: the 8867 rebuild's three judgment calls.
7. s113 ratifications: D_GA500_002 realignment · 2210 flat-7% · 7206 partner-arm scope.
8. Item-6-P1 GA residual — BLOCKING questions: GA line 5 filing status from
   federal? · couple the GA deduction election to the federal election?
9. s112 ratification: manifest-aware RS amendment (mechanism only).
10. 86 backfill review rows (83 effective) · S-24 hub-ein blanking · auth env
    vars · A2A WSDL · WISP · SEC-5 · Resend · role assignments · e-services ·
    CAF · ERO EFIN/PIN · beta clauses · older ratifications (s110 · s106 ·
    s101(4) · s100(3) · s99a · s97 · s96(4) · s95..s72).

## Active gates
- **Deploy: NOTHING PUSHED.** Two local commits on `main` (`1a02124` server,
  `2d9a864` client) awaiting Ken's go. RS `7cdf804` IS pushed and seeded.
- **DB state: migration 0220 NOT APPLIED to either shared DB.** Additive —
  three `entry_basis` columns (default `detail`); the two `payer_name`
  AlterFields are Django-level only (`blank=True`), no column change, ~700
  existing prod rows untouched.
- **Rule catalogue:** `seed_rules` NOT yet run for `D_INTDIV_012/013/014` —
  must run **after** the deploy, on BOTH DBs.
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
