# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-26, session 113 (QA Batch-001 finishing run: items 7, 8,
10 shipped; item 3 verified already-built; s112 deploy verified live on prod).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s112 is archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s113 (2026-07-26): Ken ordered the REMAINING QA Batch-001 items run to
completion.** Shipped this session (each RS-first, committed + pushed):

1. **s112 deploy VERIFIED live** (prod probe: diagnostic run on return 1019 —
   the false Sch 1/2/3 warnings gone; probe recipe → `.claude` auto-memory).
2. **Item 7 (GA RIE) CLOSED** (`5f36876`; RS `b2318a8`): the s108 pull was
   already 90% of it — what was missing was the two spec diagnostics.
   D_GA500_002 realigned to the spec (DOB-required ERROR; the app had shipped
   different semantics under that ID), old body re-homed as NEW D_GA500_016,
   NEW D_GA500_017 warns when alimony/cap-gains/other/Sch-E-rental exist
   federally but the RIE worksheet lines 8/9/10/13 are blank (the categories
   the pull can't attribute per spouse). seed_rules BOTH DBs, verified.
3. **Item 3 (autosave) — ALREADY BUILT** in s108d `e3c7168` (validation
   isolation + latest-write-wins + honest status, test-pinned). No work needed.
4. **Item 8 (7206 partner arm) SHIPPED** (`7050f93`; RS `6761b65`; mig 0213
   BOTH DBs): ScheduleK1 gains sehi_amount (13M) + se_retirement_amount (13R);
   the K-1 activity runs the SAME 7206 lines 4-10 on box 14A; line 5 pools
   POSITIVE Sch C + K-1 profits only (2025 face verbatim — losses excluded);
   K-1 SE retirement → Sch 1 L16; form_7206_rows mirrors for MeF parity;
   client K-1 card gains the two rows. QA acceptance pinned (Sch C loss
   −1,838 + K-1 2,602 + premiums 764 → L15 54 · L17 764).
5. **Item 10 (2210 rates) SHIPPED** (`8b927d4`; RS `744ba30`): ChatGPT was
   RIGHT — verified against the published 2025 i2210 Penalty Worksheet
   (fetched live): × 0.07 in ALL FOUR rate periods; Rate Period 4 =
   1/1-4/15/2026 as ONE 7% period. The 6% Q2 stub was a pre-publication
   assumption UNDERSTATING penalties (the $1-3 TaxWise deltas; return
   ...4331: 188 vs 189). Pins moved 461→466 · 143→145 · 217→219 · 369→372.
   The RS authority "excerpt" that had paraphrased the assumption as i2210
   text is now faithful worksheet text.

**▶ NEXT (cold-start pointer): QA Batch-001 item 11 — the Form 8867 rebuild.**
Survey COMPLETE (in the session task list + STATUS_ARCHIVE s113): official
Rev. 11-2024 face (still current) = 21 per-question lines incl. 4a/4b/7a/
9a-c/10-12/14/15 with Yes/No/N-A columns; app + RS spec both carry the
compressed 5-merged-line boolean model. Plan: RS re-model first (choice
facts), seed re-key + data migration (merged "true"→component "yes"; merged
"false"→BLANK so D_8867_001 forces re-answer — never invent an answer),
attestation cascade, render map, e-file check, client grid. After 11:
item 15 (source-summary mode — needs a design proposal for Ken) and the
item-6 residual (two REVIEW_QUEUE questions BLOCK it — see below).

## ▶ Waiting on Ken / external
1. **s113 ratifications (REVIEW_QUEUE):** D_GA500_002 spec realignment ·
   the 2210 flat-7% correction (tax-law) · the 7206 partner arm scope.
2. **Item-6 residual — BLOCKING questions:** pull GA line 5 filing status
   from federal? · couple the GA deduction election to the federal election?
3. **s112 ratification:** manifest-aware RS amendment (mechanism only).
4. **86 backfill review rows** (now 83 effective) · S-24 hub-ein blanking ·
   auth env vars · A2A WSDL · WISP · SEC-5 · Resend · role assignments ·
   e-services · CAF · ERO EFIN/PIN · beta clauses · older ratifications
   (s110 · s106 · s101(4) · s100(3) · s99a · s97 · s96(4) · s95..s72).

## Active gates
- **Bands green this session:** flow **520** (2210 FA runners re-pinned) ·
  2210 compute leg 22 · 7206 band 20 (spec-driven; SC-T9 auto-pinned) ·
  ga500 diagnostics 15 · RIE pull band 28 · MeF scenario-12 23 · w2u3
  diagnostics 9 · vitest **459** · tsc-renderer 52 baseline unchanged.
- **seed_rules + mig 0213 applied + VERIFIED on BOTH DBs** (prod aws-1,
  demo aws-0).
- **RS side:** 3 amendments seeded to RS prod, deployed exports verified,
  mirrors refreshed verbatim (500/7206/2210 spec.json). Harnesses ALL PASS
  (ga500 18 · 7206 9 · 2210 10 scenarios).
- ⚠ **Deploy verification for s113's server-only changes PENDING** (three
  pushes `5f36876`/`7050f93`/`8b927d4`; client bundle DID change this time —
  FormEditor K-1 rows — so the bundle-grep recipe works: grep the prod
  bundle for `13M` in the K-1 box map, baseline zero-hit taken pre-push).
- ⚠ **FA-1040-4835-06 drift:** the RS 1040 FA export carries a Form 4835
  assertion the app gate never merged (no runner). Chip filed (task
  `task_0cf10eac`); the 2210 FA entries were updated surgically instead of
  a wholesale export refresh.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
