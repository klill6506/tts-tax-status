# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-25, session 108e (Batch-001 QA backlog: GA-500 RIE pull, individual
state editor, GA attach-from-any-source, 1099-R draft card, autosave isolation — then the
test-failure settlement, push, and deploy verification.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s106 and its four same-day addenda are archived in `STATUS_ARCHIVE.md` — "Session 106".)*

## ▶ RESUME HERE

**s108 (2026-07-25): FIVE FIXES FROM THE BATCH-001 QA BACKLOG — ✅ PUSHED AND
DEPLOY-VERIFIED (s108e, same day).** Commits `05bc6a9` `e994354` `3128919` `e3c7168`,
plus the test settlement `093f9d9`; pushed `932d80f..093f9d9` (8 commits, incl. the
parallel session's `9c79daf` AI-Help work) after Ken paused entry and gave the go.
**Deploy CONFIRMED LIVE on prod, not assumed:** bundle moved
`index-BkUWdA1A.js` → **`index-XZ-tCoas.js`**, and the s108-only marker
`"Federal filing status"` (new `StateIndividualInfoSection`) greps present in it.
Baseline was checked BEFORE the push (0 hits) so the hit proves the new build, not a
pre-existing string. Django + SPA are one Render service, so the server-side GA-500
pull/attach code shipped in the same deploy.

**⚠⚠ THE BACKLOG WAS HALF-STALE.** ChatGPT authored an 8-workstream sprint from
`D:\tax-test-data\QA Reports\Batch-001\PRIORITIZED-CODE-FIXES.md`; the QA batch ran
against a pre-s106b build. Schedule D (s107), 8962 annual mode (s106e), 5695 compute,
and the fed→GA sync (s106b) were ALREADY BUILT. It also dropped a QA-ranked **P0 —
Form 8879 persistence** — while keeping lower items. **Audit any externally-authored
backlog against the code before accepting its order.**

1. **GA retirement exclusion was $0 on every retiree return — the ENGINE WAS RIGHT.**
   `GA500_FEDERAL_PULL` had only 2 entries (fed AGI, taxable SS), so
   `compute_ga500.rie()` computed correctly over an all-zero base. The v1 comment
   deferred the pull as needing "per-spouse allocation judgment" — but **RULE STUDIO
   COVERS GEORGIA** (`500`/`GA501`/`GA600S`/`GA700`; live-verified, cache identical)
   and `R-GA500-RIE` specs it: *"Each spouse qualifies separately; jointly-owned
   income split 50/50."* Now pulled per owner: W-2 wages→L1 · mat.-part. K-1→L2
   earned · interest/dividends→L6/L7 (joint 50/50) · 1099-R split on the IRA box
   →L11/L12 · APPL/65 gates derived from DOB, overridable. NOT pulled + never zeroed:
   alimony L8 · cap gains L9 (per-owner split of the NETTED Sch D result ≠ sum of
   owner-tagged transactions) · other income L10 · Sch E pg1 rental (no `owner` field).
   `GA_RIE_EARNED_CAP` $5,000 CONFIRMED (Ken + O.C.G.A. §48-7-27(a)(5)) — it was the
   only UNCITED constant in a cited block; citation added, value unchanged.
2. **GA-500 opened the ENTITY editor.** `GA500_SECTION_TABS` starts with `"info"` →
   `InfoSection` (EIN/incorporation/officers). Now `isIndividualStateForm()` →
   `StateIndividualInfoSection` for GA-500/SC1040/AL40/NC_D400; GA-501 (fiduciary) +
   GA-600S/GA-700 (entity) keep the old screen. `Taxpayer` is OneToOne to the 1040 →
   NEW read-only `federal_taxpayer` serializer field (read-only on purpose: a second
   editable copy of name/SSN is how fed/state identity drifts).
3. **⚠ FOUND LIVE, NOT BY A TEST: Georgia never attached without a W-2.** s106b keyed
   auto-attach to `W2StateEntry` alone → a retiree with pension income and no wages got
   NO Georgia return, so the exclusion had nothing to populate. Now attaches on any
   GA-tagged W-2/1099-R/1099-INT/1099-DIV. **1099-G deliberately excluded** (no state
   box; the file-as-GA convention would attach GA to every state-refund return).
   Derived RIE lines now also follow their source back DOWN (deleting a 1099-R used to
   leave a stale exclusion → overstated refund); safe because a preparer edit sets
   `is_overridden=True`.
4. **1099-R split into several partial records.** TWO independent create paths
   (`handleUpdate` + `handlePayerEinBlur`), NO in-flight guard → a POST per blur, each
   holding only its own field; `+ Add` also POSTed a `"New Payer"` placeholder. Now the
   s107 draft-card shape: no request on click · nothing POSTs until `payer_name` ·
   pre-payer values ride the single create · `creatingRef` serialises concurrent blurs ·
   EIN funnels through the SAME path · **enrichment fills BLANKS ONLY** (never
   overwrites a typed payer name) · inline validation keeps the card · no remount.
5. **Autosave.** `commit()` bundles all dirty fields into one PATCH (s105, deliberate),
   but DRF rejects the whole payload on one bad field → an invalid PIN discarded the
   spouse/address edits. Now reads DRF's field-level errors, drops only the named
   fields, retries once with the remainder. Result grew `boolean` → `{ok, failed[]}`;
   rejected fields stay dirty, accepted ones clear. Monotonic `factSeqRef` = latest-
   write-wins (a slow FAILED save could paint "Save failed" after a newer success).
   **⚠ Found while testing:** `settle()` read `formRef` which was only assigned during
   RENDER — a resolved promise runs `.then` BEFORE React re-renders, so accepted fields
   stayed dirty; masked in prod only by network latency. Mirror is now synchronous.

**LIVE-VERIFIED on the DEMO Supabase project (`django-demo` launch config → separate
`tts-tax-demo`; NEVER production).** Scratch MFJ retiree, since deleted: GA-500
auto-attached from a 1099-R with no W-2 · Info tab rendered identity with zero entity
controls · worksheet showed SP L11 521 + L12 44,913 → **L17 45,434** and TP L12 61,497
→ L17 61,497, gates checked from DOB. **45,434 is the Batch-001 answer-key figure,
DERIVED from the rule, not fitted.** ⚠ **1099-R + autosave are NOT browser-verified** —
the Browser pane is hidden, so synthetic focus events don't reach React's `onBlur`
(the s105 limitation). Their component tests drive the real components via RTL.

**▶ NEXT (cold-start pointer): FORM 8879 PERSISTENCE — the QA-ranked P0 the external
backlog silently dropped.**
- **DO NOT REBUILD THE FORM.** 8879 + 8878 were built whole in **s94** (WO-33):
  models `Form8879`/`Form8878` (migs 0206/0207 + RLS both DBs), input cards on the
  **Payments tab**, `compute_8879_8878` pinned to RS scenarios 8879-A..H / 8878-A..F,
  `rules_8879` (9) + `rules_8878` (7), f8879 render + a "signature" packet tier, FAs
  `FA-8879-NEED`/`RESIGN`. Gate: `test_8879_8878` **33/33**. See
  `form_coverage_tracker.md` ~L102.
- **So the QA finding is an ENTRY-LAYER persistence bug, the same class as s107's
  Schedule D `+ Add` and s108's 1099-R card** — the shape to expect is a create path
  that POSTs a placeholder, an unguarded second create path, or a blur-commit that
  never lands. **Audit entry-vs-compute BEFORE writing code** (the s107 lesson: a green
  leg in `form_coverage_tracker.md` is NOT proof the leg is usable from an EMPTY form).
- Source of the finding: `D:\tax-test-data\QA Reports\Batch-001\PRIORITIZED-CODE-FIXES.md`.
  **Reproduce it on the DEMO project (`django-demo` launch config), never prod.**
- Then: Form 5695 input/generated form, then the 8962 manifest+diagnostics leg
  (= backlog #12, one authoritative generated-form manifest for Forms view / e-file /
  diagnostics).

**Ken's standing s106b call still open:** re-triage the 26-return batch against
`D:\tax-test-data\SUPPORTED_FORMS.md` BEFORE more engine work. The QA sprint is
recorded in `SPRINT_SCOPE.md` but deliberately NOT placed on the BUILD_ORDER spine.

**Entry guidance:** the s108 fixes are now LIVE, so the 1099-R one-record card, the
GA-500 identity screen, and the autosave validation isolation are all in the deployed
app. Entry may resume normally. Leave 8879 until last — its persistence leg is the
next build item, not yet fixed.

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
- **vitest 389** (s108 +34: stateIndividualInfo 13 · 1099-R draft 14 · autosave 7) ·
  **tsc 0 errors**.
- **GA-500 band + flow assertions: 604 passed** (584 + the 20 new
  `test_ga500_rie_federal_pull` legs). Flow-assertion band GREEN.
- ✅ **FULL SERVER SUITE (2026-07-25, post-fix): 6,192 passed, 7 failed, 21 skipped,
  55:25.** The 7 are EXACTLY the known pre-existing set (`test_8915f::TestLandingChain`
  x2 · `test_mar30_session4::TestAAANegative` x2 ·
  `test_supporting_forms_spec::TestOfficerCompensationFlow` x2 ·
  `test_tts_forms::TestManifest::test_manifest_is_valid_json`). +8 over the earlier
  6,184 = 7 fixed + 1 new test. ⚠ Runner hygiene worth keeping: **Python block-buffers
  stdout when it is not a TTY**, so a healthy suite looks dead — use `PYTHONUNBUFFERED=1`
  plus `--create-db`, and confirm no earlier run is still alive (a collision on
  `test_postgres` errors EVERY test).
- ✅ **THE 7 "UNEXPLAINED" FAILURES ARE SETTLED AND FIXED (`093f9d9`).** Neither set was
  an s108 regression — both fail identically at pre-s108 `9c79daf` (deciding experiment
  run as specced). **They were two DIFFERENT problems, and calling them one bucket is
  what hid the real one for two sessions:**
  - **`test_user_preferences` x3 — a GENUINELY BROKEN TEST, not ordering noise.** s84
    (`c8e9e87`) made `/preparers/` writes ADMIN-only (`IsFirmAdminOrReadOnly`, SEC-1);
    the tests were written in `3f70d97` BEFORE that and PATCH as a `Role.PREPARER`, so
    the endpoint correctly 403s. **The app was right the whole time.** Fixture now
    promotes to ADMIN. ⚠ **The SEC-1 rule itself had NO test** — added
    `test_preparer_link_rejects_non_admin`. That coverage hole is exactly why three red
    tests sat unexplained. **s106e's "pass in isolation" note about these three was
    WRONG** — they fail in isolation too; nobody re-checked.
  - **`test_seed_backfill_returns` x4 — real full-suite pollution.** ~30 test modules
    seed Client/Entity/TaxYear/TaxReturn from **module-scoped** fixtures via
    `django_db_blocker.unblock()`, which commits OUTSIDE the per-test transaction, so
    those rows survive the whole pytest session. These four asserted **global**
    `TaxReturn.objects.count()`; now scoped to the test's own firm via `_returns(firm)`.
    ⚠ **Standing rule: never assert a global row count in a test.** Pollution depends on
    fixture instantiation order, so a targeted 2-module repro does NOT reproduce it —
    only the full suite proves it (it did: all 4 green in the 6,192 run).
  ⚠⚠ **SHELL LESSON (cost a detached HEAD in s108):** never put a git command in
  BACKTICKS inside a shell string — bash executes backtick content as a command
  substitution. Route multi-line prose edits through a script FILE (Write tool), never an
  inline heredoc or `-c` string. **PowerShell also has no heredoc** — `git commit -F -`
  with `<<'MSG'` is a parse error; write the message to a file and `git commit -F <file>`.
- **NO migrations in s108.** No production DB writes at any point; all live work was on
  the separate demo project and the scratch client was deleted (0 remaining).
- ⚠ Not unit-tested: the autosave mutation-sequence guard lives inside FormEditor's
  closure — verified by construction only; worth extracting.
- ✅ **Deploy verification CLOSED** (was open from s106e/s107): prod bundle
  `index-XZ-tCoas.js` carries the s108 marker, so s106e/s107/s108 are all live.
  **"Pushed" != "deployed" still stands** — the check is: grep the prod
  `/assets/index-*.js` for a string that exists ONLY in the new code, and take the
  baseline BEFORE pushing so a hit proves the new build.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
