# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-25, session 110 (Form 5695 — the light 2025 face. Spec-first
at Rule Studio, then all four app legs.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s108 and s109 are archived in `STATUS_ARCHIVE.md`.)*

## ▶ RESUME HERE

**s110 (2026-07-25, autonomous "go" → the top spine item): FORM 5695 COMPLETED TO
THE LIGHT 2025 FACE.** Ken's scope call, verbatim: *"I think this form goes away
for 2026 so you can do a light version. something good enough to complete the tax
return."* Migration `returns.0211` (additive only). RS `9f2c8c0`.

**THE AUDIT FIRST (the standing habit).** All four 5695 legs were already green
from June and their 38 tests passed — so the pointer was not stale, but the
remaining work was bigger than "input + generated form" sounds. Compared line by
line against our own 2025 IRS template, **v1 modelled 17 cost boxes and nothing
else**, and three things were missing and one was wrong:

1. **THE ELIGIBILITY GATES** the face uses to DENY a credit — 5a, 7a, 17a-c, 17e,
   21a-b, 25a, 26a. Modelled **TRI-STATE**: an explicit No denies exactly the
   branch the form says to skip; an **UNANSWERED gate deliberately does NOT
   deny**. The app back-enters returns prepared elsewhere, where eligibility was
   settled off-system — silently deleting a credit the preparer computed is a
   worse failure than flagging a blank, so the blank raises `D_5695_GATE_OPEN`
   instead. ⚠ **A No on 21a/21b skips lines 22-25 AND LINE 29**, so the separate
   $2,000 heat-pump group goes with it — the one that is easy to miss because it
   sits outside the named range. Pinned by FA-08 and by pipeline tests.
2. **THE QM ID NUMBERS (§25C(h))** — required for property placed in service
   after 2024-12-31, so **TY2025 is the FIRST year it bites and, thanks to the
   OBBBA termination, the last**. Without the number the IRS **disallows** the
   item's credit, so a 5695 that omits it is not a filable form. One PIN per
   category (7 facts) + `D_5695_QM_PIN`, one finding per category because each
   number comes from a different manufacturer's certificate. Insulation and the
   home energy audit have no PIN box and never fire.
3. **THE MAIN HOME ADDRESS** — the face asks for it in four blocks but cautions
   "you can only have one main home at a time", so it is keyed once and rendered
   into all four.
4. **⚠⚠ THE DOORS ARITHMETIC WAS WRONG.** The face caps the **most expensive
   door at $250 on its own** (line 19c) *before* the $500 all-doors aggregate
   (19h). v1's `min(30% × all_doors, 500)` **overstated by up to $250 on the
   commonest doors pattern there is — one replaced front door at $2,000 gave
   $500 where the form gives $250.** One new fact fixes it.

**SPEC-FIRST, as the rule requires.** RS owns FORM_5695 (note: the `5695` lookup
404s — the key is **`FORM_5695`**, which is why the June brief recorded "no
spec"). Loader amended → integrity gate re-typed and passing → seeded to RS prod
→ **deployed export verified** → `server/specs/5695_spec.json` refreshed from
that deployed export. ⚠ FORM_5695 is the **first form in the RS DB to carry two
version rows**; the lookup is `order_by("-version").first()` so v2 wins
deterministically, and **v1 was set `archived`** so the working list shows one.

**Deliberately deferred (the light boundary), all preparer-visible:** the
per-item PIN slots — **arithmetic-neutral**, since 19f = 19d + 19e and
20c = 20a + 20b, so folding sibling costs into the "all other" box yields the
identical credit and only the spare PIN boxes go blank; joint-occupancy and
fractional-share allocation; the 17e construction split. All three warn.

**⚠ ONE PLACE FOR THE CAPS.** The §25C caps had been transcribed **three times**
(compute, the renderer's summary, and `d_5695_25c_cap` — which had inherited the
v1 doors bug). They now live once, in `credit_25c_parts()`; render and
diagnostics both read it.

**LIVE-VERIFIED on the DEMO project** (`django-demo` → `tts-tax-demo`; production
never touched). John & Judy Jones MFJ, AGI 35,492: `$2,000` on the most expensive
door → **$250** on screen *and* on the server (Sch 3 line 5b), with `l19c`/`l19h`
marker rows populated · the QM ID box goes RED the moment a cost is keyed without
it · 17a = No persists as `false`, zeroes Section A and prints the plain-English
reason · an unanswered gate persists as **`null`**, not `false`. Scratch values
reverted; the return is byte-for-byte as found (no leftover `e5695_*`, Sch 3
5a/5b blank, AGI 35,492).

**▶ NEXT (cold-start pointer): the 8962 manifest+diagnostics leg** — backlog #12,
ONE authoritative generated-form manifest driving the Forms view, e-file
packaging and diagnostics.
- **Keep auditing ENTRY vs COMPUTE from an EMPTY form, and add s109's
  tab-switch question.** s107/s108/s109 were all entry-layer on forms whose
  compute legs were long green; s110 adds a third variant — **all four legs
  green and tested, and the form still could not be filed**, because the tests
  only ever exercised what v1 chose to model.
- Source: `D:\tax-test-data\QA Reports\Batch-001\PRIORITIZED-CODE-FIXES.md`
  (item 14). **Reproduce on the DEMO project (`django-demo`), never prod.**

**⛔ THE 26-RETURN RE-TRIAGE IS CLOSED — Ken, 2026-07-25 (s110): "let's ignore
the triage report."** Do NOT reopen it, and do not take a build order from
`_batch_triage_2026-07-23.md` (its blocked-list was wrong — that is what
`SUPPORTED_FORMS.md` was written to correct, and it stays the ground truth for
what the app can do). Events overtook the question anyway: of the 26, **15 are
entered and verified in `Done`**, 4 produced the Batch-001 code fixes, **7 remain
in `Inbox`** (five of those already have QA reports saying what stopped them),
and the two Abney returns are unstarted — 1024 is Lacerte input sheets with **no
computed answer key**, so it cannot be verified until the computed return is
re-printed.

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
4. **Ken ratifications pending:** s110 (the tri-state "unanswered never denies"
   convention · QM PIN as WARNING not ERROR) · s106 (LATE_FILING born-late ·
   dedup businesses · ack-with-note design) · s101 (4) · s100 (3) · s99a · s97 ·
   s96 (4) · s95 · s94 · s93 · s89 · s85/s84 · s83 · s76..s72.

## Active gates
- **5695 band + flow + returns + diagnostics: 706 passed** (10:21). Within it:
  `test_form5695_compute_leg` 21 (was 12) · `_diagnostics_leg` 17 (was 10) ·
  `_render_leg` 12 (was 6) · `_seed_leg` 11 (was 10) · **flow assertions 520**
  (was 518 — FA-1040-5695-07/-08 added).
- **vitest 427** (was 408; +19 `form5695LightFace`) · **NEW client test file**.
- ⚠⚠ **`npx tsc --noEmit` IN `client/` COMPILES NOTHING AND EXITS 0** — the root
  `tsconfig.json` is `{"files": [], "references": [...]}`. The real check is
  **`npx tsc --noEmit -p tsconfig.renderer.json`**, which stands at **86 → 52
  errors**: all **34 Form 5695 errors are gone** (the `e5695_*` fields were used
  by the section from the day it shipped but never declared on `TaxpayerData`).
  The remaining 52 are pre-existing and untouched. **Sessions reporting "tsc 0"
  were running the no-op config.**
- **Rule Studio suite 72 passed** (was 70 passed / 2 failed) — `test_seed_creates_
  20_assertions` in BOTH test files had been RED on every run since `05cbe72`
  added a 21st assertion against a hardcoded 20. Same rot as the 8879 harness in
  s109b: *a permanently-red test is one nobody reads.*
- **`seed_rules` AND `seed_form_5695` re-run + verified on BOTH DBs** — 12
  `D_5695_*` rules at matching severities; the new `l19c`/`l19h` marker rows only
  appeared after the FormDef re-seed, which a deploy would have done but a live
  probe would not.
- **Migration `returns.0211`** — 25 additive columns + 2 help-text-only alters;
  applied to both DBs. **No production data written at any point.**
- ⚠ **NO full server suite this session.** Last full run stands at s108e
  (2026-07-25): **6,192 passed / 7 failed / 21 skipped, 55:25**, the 7 being the
  known pre-existing set (8915f landing ×2 · AAA-negative ×2 · officer-comp ×2 ·
  manifest-json).
- ✅ **s110 DEPLOY VERIFIED LIVE ON PROD** — bundle `index-CgsV8fRF.js` →
  **`index-D-t_Kwp7.js`**, carrying four s110-only markers (`Most expensive
  exterior door` · `QM ID number` · `manufacturer ID number` · `e5695_doors_top`),
  1 hit each. These strings did not exist in any earlier build, so the hit proves
  the new bundle without needing a pre-push baseline. The server side needed no
  deploy step — the migration and both seeders were run directly against prod.
  **"Pushed" != "deployed" still stands; `/api/v1/version/` is useless for this.**
- ⚠ **When refreshing `flow_assertions_1040.json` from RS, drop the STAGED ids
  afterwards.** RS carries `FA-1040-4835-06` as `active` while the app stages it
  in `flow_assertions_1040_pending.json` (no engine→4835 depreciation feeder
  exists); a verbatim copy leaks it into the gate mirror. Now noted in the test.
- ⚠ Also fixed in passing: `load_assertions()` read spec JSON with the Windows
  default cp1252, so an export containing a `§` failed to even COLLECT. All four
  reads in that file are now explicit UTF-8.
- ⚠ Still not unit-tested (carried from s108): the autosave mutation-sequence
  guard lives inside FormEditor's closure — verified by construction only.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
