# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-26, session 114 (QA Batch-001 item 11 — the Form 8867
per-question rebuild SHIPPED end-to-end).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s113 is archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s114 (2026-07-26): QA Batch-001 item 11 — the Form 8867 PER-QUESTION
REBUILD is COMPLETE** (Ken's GO from s113). App `0f397da` + `de31033`; RS
`a0708a5`; mig returns.0214 + seed_8867 + seed_rules applied + VERIFIED on
BOTH DBs. Full detail → STATUS_ARCHIVE s114. The one-paragraph version:
the compressed 12-line model became the full Rev. 11-2024 face (21 lines,
one per printed checkbox incl. 4a/4b/7a/8/9a-9c/11/12/14/15 + the line-5
docs list); N/A exists exactly where the face prints an N/A box
(2/7/7a/8/9c/11/12 — template widget dump == MeF XSD, verified
independently); stored answers migrated per Ken's rule (merged-true →
components, merged-false/na → BLANK for re-answer — never invent); the
cascade follows the face's own routing (new arms: 8 = yes/na from Schedule-C
presence; 15 = the certification); the e-file now transmits every
previously-ABSENT sub-question element + WorkPaperDocumentNm, with the
certification reading row 15 (side flag removed); the print fills all 20
questions + docs + preparer name/PTIN; the client grid has the N/A pill on
exactly the 7 NA lines + the new Part VI section. Live demo probe:
attest → all applicable filled → un-attest → full revert.

**Gates green:** leg-C cascade 7 (3 new arms) · topic7 seed/render/
diagnostics (new 4a/4b + na-where-boxed tests) · efile mef/extract/
scenario2/print-gate/packet 63 · flow **520** · vitest 459 · tsc 52
baseline · RS harness validate_8867_rebuild ALL GREEN.

**▶ NEXT (cold-start pointer): QA Batch-001 item 15 — source-summary mode.**
Ken wants a DESIGN PROPOSAL first (not a build): a per-form "where did this
number come from" summary view. Draft the proposal, present options with a
recommendation, wait for his pick. After that: the item-6 residual (BLOCKED
on two REVIEW_QUEUE questions — GA line-5 filing-status pull · GA
deduction-election coupling to federal). Spine otherwise idle — Ken directs.

## ▶ Waiting on Ken / external
1. **s114 ratifications (REVIEW_QUEUE):** the 8867 rebuild's three judgment
   calls (merged-TRUE propagation · cascade line-8 Sch-C arm · D_8867_001
   requires line 15 always).
2. **s113 ratifications:** D_GA500_002 realignment · 2210 flat-7% (tax-law)
   · 7206 partner-arm scope.
3. **Item-6 residual — BLOCKING questions:** GA line 5 filing status from
   federal? · couple the GA deduction election to the federal election?
4. **s112 ratification:** manifest-aware RS amendment (mechanism only).
5. **86 backfill review rows** (now 83 effective) · S-24 hub-ein blanking ·
   auth env vars · A2A WSDL · WISP · SEC-5 · Resend · role assignments ·
   e-services · CAF · ERO EFIN/PIN · beta clauses · older ratifications
   (s110 · s106 · s101(4) · s100(3) · s99a · s97 · s96(4) · s95..s72).

## Active gates
- **Deploy:** BOTH s114 pushes VERIFIED live in-session — `0f397da`
  (`index-D1UrTt8d.js`, FormEditor marker) then `de31033`
  (`index-BmvSszab.js`, `f8867_part_vi_cert` count 1→2 against the
  pre-deploy baseline). Nothing pending.
- **DB state:** mig 0214 + seed_8867 (21 lines/6 sections) + seed_rules
  applied + verified on BOTH DBs (prod aws-1, demo aws-0). Pre/post
  migration audits reconciled; the one manually-answered prod return's
  overrides survived.
- ⚠ **FA-1040-4835-06 drift** (chip `task_0cf10eac`, unchanged from s113).
- ⚠ **Dev-environment nit:** an autoPort vite origin (e.g. :57351) is not
  in CSRF_TRUSTED_ORIGINS — saves 403 from a coexistence-port dev client;
  reads work. Add a localhost wildcard or list the range if it bites again.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
