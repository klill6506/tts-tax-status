# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-24, session 107 (Ken-directed bug fix — the Schedule D
"+ Add transaction" button. First work inside P1's Schedule D/8949 unit.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s106 and its four same-day addenda are archived in `STATUS_ARCHIVE.md` — "Session 106".)*

## ▶ RESUME HERE

***s107b addendum (2026-07-24, Ken, parallel session):* the floating AI-help
button is REBRANDED "Ken-Bot"** (`AiHelpPanel.tsx` — button title/aria, panel
header, empty state; the backend stays the `ai_help` Gemini service until the
separate Ken-Bot service lands, then only the `post()` target changes; the
IRS-grounded/broad modes and the no-PII notice are unchanged). ALSO: the vite
dev server now honors an assigned PORT + `autoPort` in launch.json (the s102
coexistence pattern) so it can run alongside another session's server on 5173.
**s107b-2 (Ken): Ken-Bot is now SMALLER (36px, was 48), DRAGGABLE (pointer
drag w/ 5px click-vs-drag threshold; position persists in
localStorage `kenbot.pos`, clamped on resize; panel opens beside him,
quadrant-aware), and HIDEABLE (eye-off in the panel header; Help menu →
"Show Ken-Bot" restores via the `kenbot:show` window event). ⚠ Lesson: an
early `return null` ABOVE two later hooks silently killed the component on
hide (fewer-hooks render) — the hidden gate now sits after every hook.**
Verified live on the demo app (port 60930, all paths incl. hide/restore +
position persistence across reloads); vitest 355 · tsc 0.*

**s107 (2026-07-24): THE SCHEDULE D "+ ADD TRANSACTION" BUTTON IS FIXED — Schedule
D could not be started at all.** `ScheduleDSection.handleAdd` POSTed
`description: ""` the instant the button was clicked; `CapitalTransaction.description`
is a non-blank CharField, so DRF answered **400 "This field may not be blank."**,
`onRefresh()` re-read an unchanged list, and **no editable row ever appeared**. The
preparer saw a dead button. Client-only fix, ZERO compute code, no migrations.

**The new shape (all in `ScheduleDSection`, `client/src/renderer/pages/FormEditor.tsx`):**
- The click seeds a **client-side draft row** (`SCHD_DRAFT_ID`) held in component
  state and renders it immediately, fully editable. **No request fires on click.**
- Nothing is POSTed until the description is non-blank. Columns keyed *before* the
  description accumulate in `draftValuesRef` and are sent **with** it in the single
  create — one transaction is always one record.
- **The duplicate/split guard:** `creatingRef` holds the in-flight create promise,
  assigned synchronously with the POST's first `await`. Any blur landing mid-flight
  awaits that promise and PATCHes the id it returns. There is exactly ONE code path
  that can POST and it is gated on both `draftIdRef` and `creatingRef` — fast tabbing
  through six columns can no longer split a lot across two 8949 rows.
- **Validation is inline**: a note inside the draft card shows either the
  missing-description prompt or the DRF 400 message (`role="alert"`); the row and
  everything typed into it SURVIVE, and the next edit retries the create.
- **No mid-entry remount**: once persisted, the row keeps rendering AS the draft row
  (same key, same DOM nodes) with the server row merged in for computed column (h),
  rather than reappearing as a fresh list row. This is the s105/s106 lesson — a
  remount wipes whatever field the preparer had not blurred yet.
- Delete on an unsaved draft is local-only; on a saved draft it awaits any in-flight
  create first so no record is orphaned. Ordinary-row edit/delete/calculation paths
  are untouched. `ScheduleDSection` is now exported for testing (the
  `DueDiligenceAttestation` precedent).

**Gates:** NEW `scheduleDDraftRow.test.tsx` **13** (draft appears w/ zero requests ·
one record per transaction · pre-description values land on it · gated-promise rapid
entry → 1 POST · 400 keeps the row · reload persists · unchanged PATCH path) →
**vitest 355** (was 342). NEW `tests/test_schedule_d_draft_row.py` **10** (the old
payload still 400s — so holding the draft is the only correct behaviour, not a
preference · one create + PATCHes = 1 row · GET round-trips every column · **gain
3,000 → Sch D 16 → 1040 line 7, and GA-500 line 8 == 1040 line 11 → GA AGI line 10
+3,000**; loss case and PATCH-recompute case too). Flow-assertion gate + Schedule D /
8949 / GA-500 bands re-ran: **582 passed**. `tsc` 86 errors before AND after —
identical set, all pre-existing.

**⚠ NOT live-verified in a browser.** `server/.env` points dev at the PRODUCTION
Supabase project, so clicking through Schedule D in the preview writes real
`CapitalTransaction` rows to prod. The 13 client tests render the real
`ScheduleDSection` and drive real DOM change/blur events, which covers the
interaction. If Ken wants a live probe, run it against a scratch return and revert.

**▶ NEXT: continue P1 — the Schedule D/8949 unit** (~9 returns, the biggest
back-entry unlock). The entry layer is now usable; **audit ENTRY vs COMPUTE per the
rest of the form before building** — the compute leg (Topic 9) is long-shipped and
green, so expect more entry-layer gaps like this one rather than engine work. Then
GA Form 500 RIE (the 1017 retirement-exclusion mismatch — hits every retiree).

**What remains (Ken's s106b rulings still standing):**
1. **KEN'S CALL: re-triage the 26-return batch against `D:\tax-test-data\SUPPORTED_FORMS.md`
   FIRST** (entry agents, before any engine build) — most of the ~20 "blocked"
   returns are enterable now. Real build gaps queue behind it: GA-500
   retirement-exclusion verification · K-1 passive-loss 8582 · Simplified Method ·
   lump-sum SS · digital-asset question · 8814/8839/8919 · Sch A 4684.
2. **Still pending Ken:** 8867 consolidation · date year-segment + AGI-lag (need repro).
3. Standing queue (s105-era): S-17g A2A on WSDLs landing · 1120/709 waves ·
   1120-S ATS lane · SEC-5 plumbing · ratification backlog.

## ▶ Waiting on Ken / external
1. **86 backfill review rows** (`backfill_review.csv`) — now 83 effective:
   the 3 no-entity-of-type rows are the REVIEW_QUEUE s106 scorp-entity call.
2. **S-24 hub-ein blanking leg (s97, UNBLOCKED by s106d):** keys are on Render and
   the prod backfill ran (601 rows) — awaiting Ken's explicit go to blank the ~358
   legacy full SSNs in individual `clients_entity.ein` down to last-4 (data surgery).
3. Auth env vars (s94) · A2A WSDL toolkit · WISP ratification (s96) ·
   SEC-5 [EXT] legs (s95) · Resend setup (s83) · role assignments (s84) ·
   e-services reply · CAF number (s69) · ERO EFIN/PIN source (s94) ·
   beta-agreement clauses (s96).
4. **Ken ratifications pending:** s106 (LATE_FILING born-late · dedup businesses ·
   ack-with-note design) · s101 (4) · s100 (3) · s99a · s97 · s96 (4) · s95 · s94 ·
   s93 · s89 · s85/s84 · s83 · s76..s72.

## Active gates
- **Flow-assertion band GREEN** (s107 re-ran it — zero compute code touched;
  the s106e 518 / s106 539 bands both stand).
- **vitest 355** (s107 +13) · **tsc 0 new errors** (86 pre-existing, unchanged set).
- s107 suites: `scheduleDDraftRow` **13** (client) · `test_schedule_d_draft_row`
  **10** (server). Schedule D / 8949 / GA-500 / flow re-run: **582 passed**.
- **NO migrations s107.** No DB writes of any kind — the fix is client-side and the
  server tests run against the pytest DB.
- ⚠ **Deploy verification OPEN (two builds):** s106e's 8962 annual method (bundle grep
  `Line 11 — Annual Calculation`) and now s107 (bundle grep `SCHD_DRAFT_ID` is
  minified — grep the draft note text `enter a description in column (a)` instead).
  **"Pushed" ≠ "deployed"** — grep the prod `/assets/index-*.js`; `/api/v1/version/`
  is useless.
- ⚠ Follow-up worth a look (s106e): the 7 pre-existing full-suite failures that fail
  identically on unmodified main (8915f landing ×2 · manifest-json · AAA-negative ×2
  · officer-comp ×2).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help first).
- Demo DB: untouched this session.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
