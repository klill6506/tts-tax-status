# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-24, session 106 (Ken-directed PRIORITY — the consolidated
QA work order from the three back-entry agent sessions: 1017/1018/1029/1053 all
verified dollar-for-dollar vs TaxWise; the ENGINE IS RIGHT, every finding was
entry-layer / diagnostics / data plumbing). **ALL FOUR P0s + both P2 blocks +
the P3 batch SHIPPED (7 commits, `5609c46`..`f65ea63`); P1 (the forms build
order) is untouched and is the next session's work.**)*

**s106 highlights:**
- **P0-1 DEDUP RESTORE (prod DB surgery, `5609c46`):** the 7/23 name-only hub
  dedup (ran outside this repo, no audit rows) deleted 25 clients. Re-audit vs
  the roster provenance: **22 individuals were FALSE POSITIVES** (distinct
  TaxWise export rows = distinct SSNs) — all RESTORED with original UUIDs +
  client_numbers + backfill-2025 shells (`server/scripts/restore_dedup_20260723.py`,
  dry-run→apply; clients 3,675→3,697, entities 3,979→4,001, shells +22, all
  SQL-verified). **3 businesses (#2668/#2926/#3091) were true client-level
  duplicates** (same company on two Lacerte module rosters) — left deleted, BUT
  their scorp entity + 2025 1120-S shell are missing → REVIEW_QUEUE s106. The
  dedup had also MERGED the deleted twins' email/phone onto 6 survivors —
  reverted. Full audit: `docs/audits/2026-07-24_dedup_restore.md`. **Both Larry
  Allen returns are unblocked.**
- **P0-2 ENTRY COMMIT WIRING (`8aae22e`):** NEW `lib/programmaticEventBridge.ts`
  (boot-installed delegated capture listener — the selectOnFocus pattern) defeats
  React's value-tracker dedupe so scripted/autofill sets reach onChange. FieldGrid
  cells are now SEMI-CONTROLLED (draft ?? server value): commit once-per-value on
  native change OR blur (no blur needed), drafts survive refreshes (EIN-lookup
  can't wipe an in-flight Box 1), zero remounts, failed saves keep showing what
  was typed; `recordId` scopes drafts per card. W2Screen add-in-flight guard
  (no silent fallback to card[0] — the $1.68B family, re-proven live). Live
  demo probe: scripted set w/o blur → PATCH to the CORRECT card id, persists.
- **P0-3 TAXPAYER SAVE PATH (`166f47a`):** the first-save 500 was the taxpayer
  CREATE RACE (parallel per-field first-saves on a fresh shell; OneToOne 500'd
  the losers) — losers now fall through to update (test-pinned deterministically).
  `useTaxpayerFacts` keeps fields DIRTY until the save confirms — a failed save
  can no longer let a re-seed silently blank a green field (the 1029 First-Name
  episode). Box 2a overflow: already pinned 400-not-500 by s105. ⚠ META: the
  "mangled URL" in commitValidated was a TOOL-DISPLAY ARTIFACT — bytes verified
  correct; do not "fix" it.
- **P0-4 D_1040_011 RE-CUT (`b8863a0`):** no longer claims "no computation
  engine yet" for AOTC/EIC — line 29 flags only a preparer OVERRIDE (engine owns
  computed values; _Ctx now carries the overridden-line set), line 27 only when
  the EIC engine is NOT engaged. seed_rules BOTH DBs.
- **P2 ROSTER (`2074ef4`):** prod PTINs set — Jacob C Lill P03248400, Tom
  Parkman P01218105, Gail Daniels P00141816; David Nelson still unknown → red
  "PTIN missing" pill in Preparer Manager + "⚠ PTIN missing" in both return
  pickers. **Return 1029 unblocked for assignment.**
- **P2 DIAGNOSTICS (`043abdd`):** 8867 tri-state fixed (blank ≠ selected-No —
  BooleanField app-wide); D_EIC_001 → INFO auto-resolved (the D_EIC_007 s105
  convention); D_EIC_002 gated on 27a>0 + 4797-question unanswered; LATE_FILING
  also suppressed for returns CREATED after their own deadline (born-late
  back-entry — the 1017 "inconsistency" was really the dead s104/s105 deploy);
  D_1040_018 verified covering "missing DOB always warns". seed_rules BOTH DBs.
- **P3 BATCH (`f65ea63`):** RM defaults to ALL preparers (`preparer=me` = the
  explicit my-returns view); back-entry progress badge (X/Y firm-wide from the
  list payload); taxpayer/spouse SSNs MASKED AT REST in the editor (full on
  focus + select-all); forest theme one step greener (#e3eae1 canvas — Ken
  never saw s105's pass, dead deploy).

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE
***s106b addendum (same day, Ken's 4-item follow-up — ALL DONE):* (1) s106
DEPLOY VERIFIED GREEN on prep.delviotax.com** (bundle `index-DTVh0zQn.js`
carries every s106 marker; live spot checks: D_1040_011 GONE on 1018's fresh
prod run — 2 info findings only; Jacob C Lill P03248400 in the prod roster;
scripted W-2 set with no blur → PATCH → API readback → reverted). **(2) the
Larry Allen mapping is published** (`D:\tax-test-data\Done\_larry_allen_mapping.md`
— #1075 = …9545/Cami/Windsor Dr, #1076 = …8621/Sandra/Hog Mountain Rd, both
shells confirmed; both returns UNBLOCKED). **(3) GA-500 AUTO-ATTACH SHIPPED
(`fc3510a`)** — a GA W-2 row auto-attaches the GA-500, auto-resyncs every
federal save (override-respecting), GA W/H line 24 pulls from W-2/1099-R/
INT/DIV/1099-G; PLUS a live-bug fix (the federal pull map was UNSCOPED across
sibling forms — bare line "11" collision, the s100 class); monitor empty state
now says "not started". **ANSWER-KEY REGRESSION: 1053 6/6 EXACT · 1029 4/4
EXACT · 1018 W/H EXACT — fully automatic. 1017 OPEN: the GA retirement
exclusion under-excludes (engine −44,639 vs TaxWise full zero) — the RIE
base/inputs unit is the next GA work.** **(4) `D:\tax-test-data\SUPPORTED_FORMS.md`
published — the 7/23 triage was WRONG: nearly everything COMPUTES incl. the
S-corp K-1/7203 path (refuted); only K-1 PASSIVE losses (8582), Simplified
Method, lump-sum SS, 8814/8839/8919 + the digital-asset question are real gaps.***

**What remains (Ken's s106b rulings applied):**
1. **KEN'S CALL: re-triage the 26-return batch against SUPPORTED_FORMS.md
   FIRST** (entry agents, before any engine build) — most of the ~20
   "blocked" returns are enterable now. Real build gaps queue behind the
   re-triage: GA-500 retirement-exclusion verification (the 1017 mismatch —
   every retiree) · K-1 passive-loss 8582 · Simplified Method · lump-sum SS ·
   digital-asset question · 8814/8839/8919 · Sch A 4684.
2. **s106b rulings EXECUTED:** the 3 businesses resolved on prod (JC & Sons
   #2667 + Mash Dynamo #3090 DELETED — no longer clients; Lil Junk Shop
   #2925 swapped to S-corp w/ 1120-S shell) · LATE_FILING born-late
   RATIFIED → DECISIONS.md. **ACK-WITH-NOTE SHIPPED (`da0405b`, Ken-approved
   — the Lacerte checkbox):** every non-error finding has an "ack" checkbox
   + optional note; acks are fingerprint-keyed (rule+severity+message+details)
   so they survive reruns and self-clear when the numbers change; errors can
   never be acked; migrations diagnostics 0003+0004(RLS) applied BOTH DBs;
   test_diagnostics_ack_s106 **5** · live demo cycle verified. **Still
   pending Ken:** 8867 consolidation · date year-segment + AGI-lag (need repro).
3. Standing queue (s105-era): S-17g A2A on WSDLs landing · 1120/709 waves ·
   1120-S ATS lane · SEC-5 plumbing · ratification backlog.

## ▶ Waiting on Ken / external
1. **86 backfill review rows** (`backfill_review.csv`) — now 83 effective:
   the 3 no-entity-of-type rows are the REVIEW_QUEUE s106 scorp-entity call.
2. **S-24 identity keys (s97):** copy both TAX_IDENTITY_* keys to Render →
   prod identity backfill (590 staged) → hub-ein blanking leg.
3. Auth env vars (s94) · A2A WSDL toolkit · WISP ratification (s96) ·
   SEC-5 [EXT] legs (s95) · Resend setup (s83) · role assignments (s84) ·
   e-services reply · CAF number (s69) · ERO EFIN/PIN source (s94) ·
   beta-agreement clauses (s96).
4. **Ken ratifications pending:** s106 (LATE_FILING born-late · dedup
   businesses · ack-with-note design) · s101 (4) · s100 (3) · s99a · s97 ·
   s96 (4) · s95 · s94 · s93 · s89 · s85/s84 · s83 · s76..s72.

## Active gates
- **Flow-assertion band GREEN at 539** (s106 re-ran; zero compute code
  touched all session — every change was entry/diagnostics/UX).
- NEW s106 suites: `test_taxpayer_save_s106` **4** (create race · 400 pins ·
  backfill badge) · FieldGrid **10** · programmaticEventBridge **4** ·
  useTaxpayerFacts **6** · preparerFilter re-cut · TaxpayerInfoSection SSN
  pin re-cut masked-at-rest → **vitest 342** · tsc 0.
- Re-cut pins s106: test_1040_spine_diagnostics **51** (D_1040_011
  override-gated; rule-count pin still 18) · test_entry_layer_diagnostics_s105
  **12** (LATE_FILING born-late) · test_topic7_diagnostics_leg **27**
  (D_EIC_001 info · D_EIC_002 gates) · returns **78** (backfill_progress).
- **Shared-DB state: NO new migrations s106** (all data ops were ORM writes:
  the 22-client restore + PTINs on prod; seed_rules re-run BOTH DBs twice).
- ✅ **s106 deploy VERIFIED GREEN (s106b)** — bundle `index-DTVh0zQn.js` +
  live server-behavior checks on prep.delviotax.com. ⚠ The GA auto-attach
  commit `fc3510a` pushed after that verification — confirm its deploy
  (bundle grep: `not started` in the monitor) before the next GA QA pass.
- NEW s106b suite: `test_ga500_auto_attach_s106` **4** (attach · no-attach ·
  resync · override survives). GA band 80 · flow 518 re-ran green.
- ⚠ Prod still has NO identity keys (s97) · HSTS pending next deploy (s95).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help first).
- Demo DB: probe user removed; Sarah Smith scenario data restored byte-exact
  after the s106 entry probes (Capital One Bank / 36,014 / 4,581).

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
