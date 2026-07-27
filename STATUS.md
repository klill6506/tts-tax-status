# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-27, session 121 (QA Batch-001 item 16 — structured
Form 8283 workflow SHIPPED `3ed3c76`; RS `0c8fc7f`; no migration).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s119–s120 detail is archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s121 (2026-07-27): QA Batch-001 item 16 — structured Form 8283 workflow
SHIPPED** (app `3ed3c76`, RS `0c8fc7f`, **no migration**).

Audit-first held for the **fourth** time: item 16 was ~80% already built —
the `NoncashContribution` model, `compute_8283`, `render_8283` + AcroForm
map, the MeF `IRS8283` document, 16 diagnostics and the client item grid all
shipped in the s57 1040 leg and the s65 entity amendment. Four real gaps
remained, all now closed:

1. **Reconciliation** (RS `R-8283-RECON` / `D_8283_017`). `R-8283-SCHA12`
   lets a flat line-12 entry override the 8283 row total per field
   (deliberate, unchanged) — but the override was **silent**: the printed
   Form 8283 carried the rows, line 12 carried the override, and nothing
   compared them. A conversion keeping a packet total but keying only part
   of the detail claimed the difference with no form item behind it. The
   1065 already had this guard (`D_8283_016`); the 1040 did not.
   **Effect-scaled** (the `D_4562_DEST` convention): error when line 12
   exceeds the non-withheld row total with no withheld row present; warning
   on the reversed delta; **warning whenever a conservation/historic row is
   present** — `D_8283_006` has the preparer key the allowable amount by
   hand, so a gap is expected there. No amount moves.
2. **1040 finding routing** — `RULE_TAB_MAP`'s 1040 scope had **no
   `D_8283_` entry at all** (both entity scopes did), so every 8283 finding
   on a 1040 produced no tab dot and nothing to click.
3. **Line-12 guidance** (the literal item-16 ask) — the Schedule A line-12
   fields had no >$500 notice and no link into the item workflow. Inline
   notice mirroring the server ladder + a jump that scrolls to and
   highlights the grid; `CONSERVATION_TYPES` exported so card and grid share
   one transcription.
4. **Missing facts** — `D_8283_005/007` named the offending item but not
   *which* facts were missing. They now list the specific columns
   ((e)/(f)/(g)), row facts, zero amounts and out-of-year dates.

Plus **Form 8283 registered in the s112 generated-form manifest** (it was
absent, so `is_form_generated` returned False), calling `render_8283` itself
so the $500 engagement gate has no second copy.

**QA acceptance pinned verbatim:** a synthetic $1,100 noncash aggregate with
no detail keeps the $1,100 Schedule A total, blocks, and lists the missing
facts; after complete entry the form generates and every finding clears with
itemized deductions unchanged.

**Gates green:** NEW server `test_8283_item16_recon.py` **13** · 8283 band
**46** · Schedule A legs **40** · manifest **10** · flow assertions **521** ·
NEW client `form8283Item16.test.tsx` **14** · vitest **525/525** · tsc **52
baseline** · live demo probe green end-to-end (error arm with correct
numbers, jump+highlight, the real runner fired `D_8283_017`; demo restored).

**Also fixed (off-scope, never silent):** RS `check_8283_integrity.py` had
been **RED since the s65 entity amendment (2026-07-12)** — T14/T15/T16 were
authored without extending the harness's transcription, so all three
reported "expected key not produced" and the gate had stopped meaning
anything. Entity arm modeled; 19/19 green, with a negative control run to
prove the new K12b override check actually fails when broken.

## ▶ NEXT (cold-start pointer)
Item 15 (source-summary/conversion mode) — **still awaiting Ken's A/B/C pick**
(rec C); it is also item 16's remaining fourth bullet. Then item-6-P1 GA
residual (BLOCKED on the two GA REVIEW_QUEUE questions) · 2210 reconciliation
panel. Spine otherwise idle — Ken directs.

## Known follow-ups from s121 (tracked in DEFERRAL_AUDIT)
- Item 16's **conversion/source-summary bullet is deferred** — it rides item
  15, which is blocked on Ken's pick. Everything else in item 16 is done.
- Section B still is not e-fileable (`UnmappableValue`, the J7 wet-ink
  appraiser/donee signature seam) — unchanged, pre-existing boundary.
- 1065 MeF still has no `IRS8283` document (rides the future 1065 mapper).
- `NoncashContribution` rows are per-return, not per-owner.

## ▶ Waiting on Ken / external
1. **Item-15 pick (A / B / rec C)** — proposal at `Design/item15_source_summary_proposal.md`.
2. s121 ratification (REVIEW_QUEUE): the `D_8283_017` severity ladder — most
   notably the conservation arm that warns rather than errors.
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
- **Deploy:** s121 push `3ed3c76` **VERIFIED live in-session** — prod + demo
  bundles rolled `index-DQfYbJEX.js` → `index-BhoKt46x.js`; markers
  `no item on the form to support it` / `Enter the Form 8283 items` /
  `Review the Form 8283 items` each ×1 against the 0-hit pre-push baseline.
  Nothing pending.
- **Rule catalogue:** `seed_rules` run on **BOTH DBs after the deploy** (the
  required order — the runner turns an unresolvable `rule_function` into a red
  finding, so seeding first would have put a spurious error on every 1040 on
  prod; a deploy does **not** run `seed_rules`). Verified: `D_8283_017` present
  and active on both, D_8283 family 17/17 on both.
- **DB state:** no new migrations (latest remains 0219, applied BOTH DBs in s119).
- **RS:** 8283 spec at `0c8fc7f` (10 rules / 17 diagnostics / 19 scenarios);
  deployed export verified and mirrored to `server/specs/8283_spec.json`.
- ⚠ FA-1040-4835-06 drift (chip `task_0cf10eac`, unchanged from s113).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
