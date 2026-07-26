# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-26, session 113 boot (s112 deploy VERIFIED live on
prod — the generated-form manifest is serving; the false Sch 1/2/3
"attach the manually prepared form" warnings are gone on a real
previously-affected return).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s111 is archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s112 (2026-07-25): QA backlog #12 SHIPPED — ONE generated-form manifest
now drives the Sch 1/2/3 attachment diagnostics.** App commit `a84afb7`
(pushed `5cf9cc6..a84afb7`); RS commit `a5a1a13`. No migration; no client
change.

1. **RS went first (the s109b pipeline):** R-S1-10 / R-S2-09 / R-S3-07 and
   D_SCH1_004 / D_SCH2_004 / D_SCH3_003 amended in the owning loader
   (`load_1040_sch123.py`) — mechanism only, line lists and severities
   untouched (the rules' own titles said "warn until their topics build";
   the topics built). Harness ALL PASS → seeded RS prod → deployed export
   verified carrying all six amendments → `server/specs/sch_{1,2,3}_spec.json`
   refreshed verbatim. NEW `exceptions` on R-S3-07: §904(j) de minimis files
   NO Form 1116 and never warns.
2. **NEW `apps/tts_forms/form_manifest.py`:** the manifest answers "is form
   X in this return's packet?" by calling the SAME 19 render callables
   `render_complete_return` stages (bytes-or-None IS the truth) — so the
   manifest, the printed packet, and the Forms sidebar can never disagree;
   there is NO second copy of any engagement gate. It also owns the
   Sch 1/2/3 line → required-form maps (moved out of rules_sch123) and the
   two spec-cited legal exceptions (§904(j); the R-5329-03 direct-report
   shortcut — Sch 2 line 8 with no Form 5329 when no owner must file).
3. **rules_sch123 rewired:** warn only on a GENUINE omission — amount
   present AND required form absent from the manifest AND no exception.
   Hand-keyed amounts with no source data STILL warn (that omission is
   real). Static SCH{1,2,3}_ATTACHMENT_LINES maps removed. Catalogue
   names/descriptions re-seeded and VERIFIED on BOTH DBs.
4. **The backlog's acceptance test is a real test:** for every nonzero
   Sch 1/2/3 attachment-backed line, `test_form_manifest.py` compares the
   required set against the FINAL rendered packet and asserts diagnostics
   report only genuine omissions; plus manifest↔packet parity BOTH
   directions per registry form, and e-file extract parity pins (5329 both
   sides, 8880). QA-reported conflicts covered: D_SCH3_003 vs D_1116_001
   (de minimis) and D_SCH2_004 vs a generated 5329.

**▶ NEXT (cold-start pointer): idle — Ken directs.** Backlog #12 was the
last queued unit; remaining prioritized-fix items and the P1 audit queue
need Ken's order. ✅ **s112 deploy VERIFIED live (2026-07-26 02:18 UTC,
s113 boot):** prod probe — magic-link token minted locally for the `dev`
probe account (shared DB), redeemed against `prep.delviotax.com`, then
`POST /api/v1/diagnostic-runs/run/` on return **1019** TY2025 (its
07-24 pre-deploy run carried D_SCH2_004 + D_SCH3_003 from the old static
lists; data unchanged — old code would have re-fired them). Post-deploy
run `8485d5b5…`: **zero Sch 1/2/3 attachment findings**; the 4 remaining
findings are all legitimate (D_GA500_008/001 info · D_2210_NO_PENALTY
info · D_8962_REPAYMENT warning — an 8962-generated return, exactly the
formerly-false class). Session logged out; token single-use-consumed.

## ▶ Waiting on Ken / external
1. **s112 ratification (REVIEW_QUEUE):** the manifest-aware RS amendment
   of R-S1-10/R-S2-09/R-S3-07 (mechanism only).
2. **s111 ratifications (REVIEW_QUEUE):** GA deduction election coupled to
   the federal election · pull GA line 5 filing status from federal? ·
   1099-INT "Seller-financed" currency cell sits over a Boolean field.
3. **86 backfill review rows** (`backfill_review.csv`) — now 83 effective.
4. **S-24 hub-ein blanking leg** (s97, unblocked) — awaiting explicit go.
5. Auth env vars (s94) · A2A WSDL · WISP (s96) · SEC-5 [EXT] · Resend (s83) ·
   role assignments (s84) · e-services reply · CAF (s69) · ERO EFIN/PIN (s94) ·
   beta clauses (s96).
6. **Ratifications pending:** s110 (tri-state gates · QM PIN warning) · s106 ·
   s101 (4) · s100 (3) · s99a · s97 · s96 (4) · s95 · s94 · s93 · s89 ·
   s85/s84 · s83 · s76..s72.

## Active gates
- **NEW `tests/test_form_manifest.py` 10** (registry trip-wires ·
  manifest↔packet parity · the only-genuine-omissions acceptance sweep ·
  §904(j) + R-5329-03 exception scenarios · e-file extract parity pins).
- **Bands re-run this session:** `test_1040_sch123_diagnostics` 41 ·
  **flow assertions 520** — all green. No client change (no vitest/tsc
  delta; s111 baselines stand: vitest 459 · tsc-renderer 52 pre-existing).
- **`seed_rules` re-run + verified on BOTH DBs** (the catalogue rows carry
  the amended names). No FormDef reseeds; no migration.
- **RS side:** `check_sch123_integrity` ALL PASS · seeded to RS prod ·
  deployed export verified · mirrors refreshed verbatim (`a5a1a13`).
- ✅ **Server deploy VERIFIED** (see the NEXT pointer — prod diagnostic
  run on return 1019, false attachment warnings gone).
- Follow-up chips: the 1099-G POST-per-blur card (s111) still pending
  Ken's click.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
