# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-09, session 38 (the A2A/PWA/usability day — no scenario work).
The 1120-S ATS lane is Ken-gated (e-services email out); CC lane = **S-19 usability
batch 1, mid-stream — resume at item 11**.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs; clients by Lacerte
  file number only (identities in `D:\tax-test-data\`).

## ▶ RESUME HERE — S-19 usability batch 1 (Ken's + Whit's 1120-S list)
**`USABILITY_QUEUE.md` is the working checklist** (tax-app root, NOT mirrored). Done:
items 1 (entity sidebar) · 4 (zip autofill) · 7 · 13 (+ the CLIENT↔SERVER **parity gate**)
· 13b (L3a inventory, spec-first RS R008) · 15. **NEXT: item 11 — the balance-sheet
sub-schedule focus-steal** (cause already found: `SubSchedulePanel` keys rows by id and
swaps local→server id on first save → remount steals focus; fix = stable key). Then: 2
(entity header shows entity name only) · 3 (infinite scroll) · 5 (state dropdowns) ·
6 (widths) · 8+14 (label capitalization — server `seed_1120s.py` + siblings, needs
reseed) · G-a (placeholder sweep, ~130 across 26 files) · 9 (M&E 4-line — **RS spec
amendment first**, Ken ruled: add the 100% line) · 12 (Sch B Q11 auto-answer — **RS spec
first**, Ken ruled: auto-answer only) · 16 (CC retrospective, last). Batch 2 arrives from
Ken when batch 1 is done.

## ▶ Waiting on Ken / external
1. **E-services email reply** (sent 2026-07-09) — S7/S8 declared-forms ruling · 8941
   key-inversion (Ken RATIFIED the finding 2026-07-09 — engine right, key wrong) · 1040
   production flip · SOR re-request. Unblocks the 1120-S upload loop.
2. **IdenTrust cert** (packet OVERNIGHTED 2026-07-09; ~7-14 day vetting; ⚠ 30-day
   download clock runs from the notarization date). Then: AE enrollment → activate ASID →
   A2A comms test. Runbook: `docs/mef/A2A_ENROLLMENT.md` (not mirrored).
3. **Ken reprints the file-1018 client's 2024 return from Lacerte** (its export PDF
   printed a $1-placeholder Schedule L) → drop in the import folder → CC re-imports
   (usability item 10; importer itself verified faithful across all 278 records).
4. **Ken verifies the PWA install** (S-18c): Chrome install icon → "Delvio Tax" · two
   windows on two clients · next-deploy pickup on reload. Header fixes + sidebar + prefs
   land the same deploy.
5. TaxWise forms-usage report · client-numbering claude.ai-chat conclusion (paste when
   found; numbering deferred to the bulk client import per Ken).

## Session-38 state (what shipped — detail in STATUS_ARCHIVE s38)
- **A2A enrollment kicked off** (cert product/forms/AE field values all settled) — on
  hold at the cert step; see the runbook.
- **S-18 platform trio**: 18c PWA CODE-COMPLETE (manifest "Delvio Tax"; /api/ never
  cached; Ken-verify pending) · 18e header quick-fixes · 18f per-preparer default entity
  tab (last-clicked persists; sort already persisted) · 18d tickets parked design-pending.
- **S-19 batch 1** through item 15 (see RESUME) incl. two spec-first compute units
  (L3a R008; the parity gate caught 7 client-formula drift lines, all fixed).
- **⚠ New standing gates**: `computeFields.parity.test.ts` (client screen math vs the S6
  server fixture — regenerate via `manage.py dump_client_parity_fixture` when 1120-S
  compute changes) · computeFields skips manual-seeded + overridden lines MID-CHAIN.

## Active gates
- **Flow-assertion gate 447** — green (run this session after the L3a unit).
- Client vitest 278 — green. ⚠ `tsc -p tsconfig.renderer.json` = 148 pre-existing error
  lines (despite s36's "tsc 0" note; invocation drift) — maintenance item.
- ⚠⚠ 1120-S upload gate unchanged: full scenario set + the e-help answers first.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · **`USABILITY_QUEUE.md`** (the S-19 lane) ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
