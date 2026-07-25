# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-25, session 109 (Form 8879 persistence — the QA-ranked P0 the
external backlog dropped. Client-only; zero server source changed.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s108 and its addenda are archived in `STATUS_ARCHIVE.md` — "Session 108".)*

## ▶ RESUME HERE

**s109 (2026-07-25): FORM 8879 "PERSISTENCE" FIXED — and the SERVER WAS RIGHT
ALL ALONG.** Commit `edfca9c`, pushed `47002c3..edfca9c`. **Client-only: ZERO
server source changed, no migrations, no production DB writes at any point.**

**THE FINDING:** a preparer started an MFJ Form 8879, keyed both PINs and both
signature dates, captured the Part I snapshot, opened Forms (the 8879 WAS in the
generated package), came back to Input — and the card read "Start Form 8879"
with every field blank.

**NOTHING WAS EVER LOST — and the QA report contained its own proof.** The run's
final diagnostics still listed `D_8879_NEED`, and `d_8879_need` returns `[]`
when the Form8879 row is absent. So the row demonstrably survived the navigation
that appeared to destroy it. The tester read the surviving warning as evidence
of the loss; it was evidence of the opposite. *(That diagnostic is itself a
problem — see REVIEW_QUEUE below.)*

**THE ACTUAL DEFECT — the seam, not the form.** A "self-managing" card seeds
`useState` from its `initial` prop **once at mount** and thereafter PATCHes the
server directly, never telling FormEditor. `returnData.form_8879` therefore sat
at its page-load `null` for the entire session. The Payments tab is
conditionally rendered (`activeTab === "payments" ? <PaymentsSection/>`), so
leaving for Forms **unmounts** the card and coming back **remounts** it onto
that stale seed. Clicking "Start Form 8879" again was harmless — the endpoint is
`get_or_create`, so the PINs came back — but a preparer seeing a blank card
concludes the work is gone.

**NINE CARDS SHARED THE SHAPE, ALL NINE FIXED** (not just the reported one):
8879 · 8878 · 4868 · 8888 · 9465 · payment vouchers · 2848 · 3115 · 2553. Each
now takes an `onChange` that reports **only what the server confirmed** (a
refused PATCH reports nothing, so the parent can never cache a rejected record);
FormEditor's new `setSingleton` records it with an **in-place key write, not a
refetch** — none of these cards feeds the 1040 computation from the client, and
a refetch would risk stomping unflushed keystrokes elsewhere.

**⚠ THE GENERAL LESSON: a "self-managing" card is only safe while it is
mounted.** Anything conditionally rendered must report its server state upward,
or the parent's copy becomes a stale seed that silently un-does completed work.

**LIVE-VERIFIED on the DEMO Supabase project** (`django-demo` → `tts-tax-demo`;
production never touched). John & Judy Jones MFJ 1040 — the same return s94
probed, AGI 35,492: started → Forms → back to Input keeps the card · PINs
54654/16546 + both 07/23/2026 dates + the Part I snapshot survive a **hard
reload** AND a second Forms round trip ("within tolerance") · Form 8879 present
in the generated package (it takes ~15s to render — an early check reads as
absent). Scratch record deleted; the demo return is as found.

**▶ NEXT (cold-start pointer): FORM 5695 — input + generated form.** Then the
8962 manifest+diagnostics leg (backlog #12: ONE authoritative generated-form
manifest driving Forms view, e-file packaging and diagnostics).
- **Audit ENTRY vs COMPUTE from an EMPTY form first** — s107 (Schedule D), s108
  (1099-R) and s109 (8879) were all entry-layer, all on forms whose compute legs
  were long green. A green leg in `form_coverage_tracker.md` is not proof the
  leg is usable.
- **s109 adds a second audit question to that habit:** does the card survive a
  TAB SWITCH? The 8879 defect was invisible to every test because nothing ever
  unmounted and remounted a card mid-session.
- Source of the findings: `D:\tax-test-data\QA Reports\Batch-001\PRIORITIZED-CODE-FIXES.md`.
  **Reproduce on the DEMO project (`django-demo`), never prod.**

**Ken's standing s106b call still open:** re-triage the 26-return batch against
`D:\tax-test-data\SUPPORTED_FORMS.md` BEFORE more engine work. The QA sprint is
recorded in `SPRINT_SCOPE.md` but deliberately NOT on the BUILD_ORDER spine.

**s109b (same day, Ken: "make the spec change and fix the date on 8879") —
BOTH REVIEW_QUEUE items CLOSED.** Commit `cb756a1`; RS `8a30b6c`.

1. **`D_8879_NEED` warning → INFO, spec-led.** Amended at **Rule Studio, which
   owns this field** — loader amended, harness **79/0**, seeded to RS prod,
   deployed export verified, then `server/specs/8879_spec.json` refreshed from
   that export (the ONLY diff vs the old mirror was that one severity;
   rules/line_map/facts/tests/metadata byte-identical). **Enforcement did not
   move** — `D_8879_UNSIGNED` stays the ERROR that blocks transmission, and both
   the RS harness and a new app test pin the pair so it can never go
   all-non-blocking. ⚠ **The severity lives in TWO places in the app**: the
   `_finding()` the rule emits AND the `RULES_8879` registration that seeds the
   DB rule row the diagnostics catalogue reads — moving only one would have had
   the UI file a finding under one severity while the catalogue advertised
   another. `seed_rules` re-run + verified on BOTH DBs; confirmed in the running
   app via `/api/v1/diagnostic-rules/`.
2. **One date-entry contract — and it was worse than reported.** The 8879/8878
   were the app's only raw `<input type="date">` signature fields; they now use
   the shared masked `DateInput` like every other date. That swap exposed the
   mirror-image defect in the shared component: **it silently mangled ISO input
   — `2026-07-23` displayed `20/26/0723` and emitted `0723-20-26` (month 20, day
   26, year 723) to all 17 consumers.** Probed and confirmed BEFORE changing
   anything. `DateInput` now normalises a whole ISO date arriving in one change
   and **never emits a date that does not exist**; a complete-but-impossible
   entry is flagged `aria-invalid` + red ring and writes nothing.

**Live-verified on the DEMO project** (production never touched): digits mask as
typed (0 → 04 → 04/1 → 04/15 → 04/15/2026) · pasted `2026-07-23` → shows
`07/23/2026`, PATCHes `{"primary_signed_date":"2026-07-23"}` · `02/31/2025`
flagged, sends NOTHING · correcting it clears the flag and saves. Scratch record
deleted.

⚠ **Carry this: React's blur delegation listens on `focusout`, NOT `blur`.** A
synthetic `blur` never reaches `onBlur` in the hidden pane — which is why s108
concluded blur-driven fields were unverifiable there. **They are verifiable;
dispatch `focusout`.** (Also: a synthetic `focusout` double-fires, so it showed
two identical PATCHes — re-checked under RTL, the real path commits exactly
once, now pinned.)

**Entry guidance:** the s109 fix is **LIVE on prod** — the "my 8879 vanished"
behaviour is gone, and 8879 entry is safe to do at any point in a return.
**s109b's date + severity changes are LIVE on prod too** (deploy verified) — 8879
signature dates now take `07/23/2026`, and `D_8879_NEED` reads INFO.

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
- **vitest 408** (s109 +7 `singletonCardPersistence`; s109b +12: DateInput 5→12,
  singletonCardPersistence 7→11) · **tsc 0 errors**.
- **`test_8879_8878` 33 → 35** (s109b: the severity pin + a spec-mirror drift
  pin) · 8879 + flow-assertion bands **560 passed** · core diagnostics bands
  (`test_diagnostics`, `_ack_s106`, `1040_spine`, `entry_layer_s105`) **109
  passed**.
- **Rule Studio:** `validate_8879_8878.py` **79/0** — and REPAIRED: three of its
  assertions had failed on EVERY run since 2026-07-15 because nobody updated
  them after Ken approved Gate-1 (they still asserted `READY_TO_SEED` ships
  False and the FAs are staged DRAFT). A permanently-red harness is one nobody
  reads. Also fixed a stale loader message that printed "flow assertions (staged
  DRAFT)" on every reseed — it read as though the run had just deactivated the 3
  assertions the tts flow gate depends on. **Verified against RS prod: all 3
  still `active`, 484 active FAs total** — the message was lying, the behaviour
  was correct.
- **NEW `tests/test_8879_persistence.py` 7** — it **passed on the first run with
  no server change**, which is the cleanest available proof that the DB half was
  never broken. It pins the hard-reload payload, the **`?fresh_return=1` payload**
  (that one replaces `returnData` wholesale mid-session, so a missing key there
  would silently re-stale the seed and reopen the defect), `get_or_create`
  idempotency, and the diagnostic evidence itself.
- **`test_8879_8878` 33/33** unchanged (the s94 unit gate).
- **NO full server suite run this session** — deliberate: zero server source
  changed, and the last full run (s108e, 2026-07-25) stands at **6,192 passed /
  7 failed / 21 skipped, 55:25**, the 7 being exactly the known pre-existing set
  (8915f landing ×2 · AAA-negative ×2 · officer-comp ×2 · manifest-json).
- **NO migrations. No production DB writes.** All live work ran against the
  separate demo project; the scratch 8879 was deleted afterwards.
- ✅ **s109 DEPLOY VERIFIED LIVE ON PROD** — bundle `index-XZ-tCoas.js` →
  **`index-CdtjNmcu.js`** (the same hash the local production build produced),
  carrying the s109-only marker `onSingletonChange`. The **zero-hit baseline was
  taken BEFORE the push**, and a local production build first confirmed the
  string survives minification, so the hit proves the new build rather than a
  pre-existing string. **"Pushed" != "deployed" still stands**;
  `/api/v1/version/` is useless for this.
- ✅ **s109b DEPLOY VERIFIED LIVE ON PROD** — bundle `index-CdtjNmcu.js` →
  **`index-CgsV8fRF.js`**, the exact hash the local production build produced,
  carrying the s109b-only marker `Not a real date` (1 hit against the **0-hit
  baseline taken BEFORE the push**). The Rule Studio severity change went live
  independently of the bundle — RS serves it from its own DB and both tax-app
  DBs were reseeded directly, both verified reading `info`.
- ⚠ Still not unit-tested (carried from s108): the autosave mutation-sequence
  guard lives inside FormEditor's closure — verified by construction only.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
