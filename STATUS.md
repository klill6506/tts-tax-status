# TTS Tax App - STATUS (current state only)

*Last updated: 2026-08-01, session 175 (**THE QA-BATCH SESSION — AND THE DEPLOY SHIPPED AT THE END OF IT** (see Active gates). Ken relayed a
Codex/ChatGPT defect list reproduced 2026-07-28 on the LEGACY screens, three
days before the cutover. Seven commits, six of them fixes. **The headline: of
the six items examined, THREE prescriptions were already the behaviour and TWO
items no longer reproduce at all** — reproducing before building is what this
session was actually made of. Ken will bring a NEW list next session.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE

**Ken is bringing a NEW QA list from ChatGPT/Codex.** Start there. Two items
from the CURRENT list are confirmed and unfixed, and are the default work if no
new list arrives:

1. **QA #6 — source-summary rows offer fields the server refuses.** CONFIRMED
   LIVE this session, and worse than filed. A source-summary 1099-INT row
   renders **Box 1 and Box 3 as fully enabled inputs**; both are in
   `FORBIDDEN_ON_SUMMARY`. Typed 3,400 into Box 1 → **`PATCH` 400 Bad
   Request** → **no error shown anywhere on screen and the input kept
   displaying 3,400.** From the preparer's chair that is silent data loss.
   ⚠ **The fix is already half-built and unwired**: `FORBIDDEN_ON_SUMMARY` and
   `summaryConflicts` exist in `client/src/renderer/components/forms/
   SourceSummary.tsx` and have **NO consumers** — the Slate screens never ask.
   Disable/omit those cells on a summary row, and surface the rejection ON THE
   ROW.
2. **The stale totals strip** (found while disproving QA #2). After an edit on
   the Slate dividend/interest screens the row input and the DB are both
   correct while the screen's own totals strip still shows the OLD figure until
   remount. Display-only, no data at risk. Ken: "come back to it."

## What shipped this session (all on `slate-ui`, pushed, NO deploy)

- **`ae7cde9`** — the parallel session's trial-balance refactor, carried in at
  Ken's direction: `import_tb`'s arithmetic moved verbatim into
  `apps/returns/tb_import.py` as a pure `build_tb_plan` + a `commit_tb_plan`.
  Verified, not authored, here (21 + 2 tests, `manage.py check` clean).
- **`8c0ddd4` — QA #5, a residential rental "fully depreciated" 13 years early.**
  The report blamed the badge; the badge uses neither of the things it blamed.
  THREE real defects: (1) the CSV importer defaulted a blank convention off the
  METHOD CODE ALONE, so a rental written as plain `SL` became 27.5-yr SL/HY and
  computed **NO depreciation at all**; (2) the Lacerte parser's life group was
  `(\d+)`, which cannot match "27.5", so **every rental line in a Lacerte
  conversion** imported with no method/convention/life (found by sweeping the
  family, not reported); (3) the badge could not tell "depreciated out" from "no
  published table", so a red `D_4562_METHOD` error was on screen saying the
  opposite. §168(d)(2)/§168(c) verified against the statute. 23 tests.
- **`f668f69` — `D_4562_CONVENTION` + QA #4.** The new rule asks whether a
  method/life/convention is **LEGAL**, which `D_4562_METHOD` (can the engine
  compute it?) never asked: 27.5+HY fails LOUDLY ($0 + error), **39+HY computes
  0.0256 instead of 0.02564 and nothing objects**. QA #4's prescribed fix was
  already the behaviour — `compute_schedule_e` buckets a non-active rental to
  Part V and the §469(i) allowance only draws on bucket IV, returning the QA's
  own stated figures ($0 allowed / $4,858 suspended). **The real defect: ALL SIX
  rental mutation paths called the entity-only K-2 rollup and NONE called
  `_recompute_1040`**, which returns early on a 1040 — so every rental edit
  saved its row and recomputed nothing. Fixed at the shared helper.
- **`c79249e` — QA #8 + #9 + the banner scoping.** #8: AGI ran through the
  `Math.abs` formatter (correct for refund/due, wrong for AGI) → a signed
  formatter for AGI alone. **#9 was a COMPUTE defect, not a diagnostic one**:
  with negative AGI the §170(b)(1) ceilings were multiplied straight through, so
  **Schedule A line 14 came out at −$3,223 with ZERO contributions** and invented
  a $3,223 carryover — which is what fired `D_SCHA_004`. Gating the diagnostic
  (the prescription) would have silenced the messenger and left a negative
  deduction. Contribution base now floored at zero.
- **`3641a3d`** — the back-entry banner turned OFF (Ken). It rode a permanent
  flag, so it sat on all ~3,300 seeded shells forever.
- **`5e67ed2` — QA #10.** Adding a Schedule C POSTed the literal
  `"New Business"` into **line C, a tax-form field** — a fictitious business
  name that prints and transmits. Every display site already had a fallback.
  ⚠ The other half of that item (clearing it restores the placeholder) does NOT
  reproduce.
- **`7bd00d9` — the workflow unblock + the invoice kill switch.** Read-only
  count: **ALL 3,836 PRODUCTION RETURNS WERE `draft`** — the status field, API
  and both side effects existed, but NOTHING in the app could change a status.
  The ClientHeader pill is now a select over the five states. **Ken's ruling:
  preparers set status by hand during testing (the agent marks a return `filed`
  once it ties to the return as actually filed); autonomous transitions come
  with production.** ⚠⚠ NEW **`LEDGER_AUTOPOST_ENABLED` (default OFF, checked
  BEFORE reachability)** — `is_configured()` alone was not a guard: it keys on
  the credentials, so setting them to test the Ledger integration would have
  started invoicing live clients from a test pass. Live-proven: `filed` →
  baseline captured, outbox 0; `approved` → outbox STILL 0.

## Items that DO NOT reproduce on Slate (do not rebuild without re-checking)
- **QA #1 — "new payer drafts do not persist".** Ran its own acceptance test:
  typing a payer and moving focus creates EXACTLY ONE row, which then accepts
  amounts and moves the return (1040 line 3b = 5,721 = 800 + 4,921).
- **QA #2 — "persisted 1099-DIV amounts revert to 0.00".** Typed 7,939, blurred:
  input kept it, DB stored it, 1040 line 3b = 7,939, survived a full reload.
  Only the totals strip was stale (item 2 in the resume pointer).
- Both were reproduced by QA on **2026-07-28, on the legacy screens**, three
  days before the cutover. **Re-run every screen-layer item on Slate before
  touching code that may no longer run.**

## Active gates
- **⚡ DEPLOYED 2026-08-01 ~22:40Z (Ken's go, end of s175): `main` fast-forwarded
  to `fdbd7f2` and pushed → Render built `index-BrbsO-k6.js`.** Verified live:
  the status control is in the deployed chrome chunk (`slate-pill-select` /
  "Return status" / the Filed tooltip ×1 each); `/api/v1/version/` 200 `prod`.
- **✅ THE SEED DEBT IS CLEARED — `seed_rules` RUN ON BOTH DBs post-deploy**
  (prod 771 → **772** active rules, demo 772; `D_4562_CONVENTION` seeded as
  error; **every rule_function verified resolvable on both DBs — zero
  unresolvable**). Post-deploy diagnostics probe on a real prod return: 12
  findings (10 info / 2 warning), **zero rule crashes**. ⚠ The probe wrote a
  fresh DiagnosticRun on the most-recently-updated prod 1040 (findings
  re-derived; return data untouched) — the standing server-deploy check.
- **✅ Migration 0227 was ALREADY APPLIED on prod** (an earlier deploy's
  `migrate --noinput` took it) — the carried "0227 un-applied" debt note was
  STALE. `showmigrations`: zero unapplied, all apps. This deploy was code-only.
- **Branch:** `slate-ui` checked out, in sync; `main` == `slate-ui` ==
  `fdbd7f2`. `Design/screens/` stays untracked (entity-sweep screenshots,
  unrelated).
- ⚠ **`LEDGER_AUTOPOST_ENABLED` must stay unset until the production cutover**
  (Ken: January 2027). Setting the Ledger credentials alone no longer arms it.
- ⚠ **Demo DB verified byte-clean at close** — 892/892 FFV rows, AGI 94,560,
  L15 78,810, L16/L24 12,204, L37 10,151, 1 DIV row (800/600/150), 1 INT row
  (1,250), 0 Schedule C, status back to `draft`, test as-filed baseline deleted.
- ⚠ One test DB — never overlap pytest runs (cost a full band re-run this
  session).
- ⚠ **`mint_magic_link.py` lives at the REPO ROOT** (`scripts/`), not under
  `server/scripts/`.
- ⚠⚠ **HASH ROUTER: `navigate` to the same hash URL does NOT remount the SPA.**
  Cost a wrong "my control is broken" diagnosis — the DB said `filed`, the
  screen said `draft`, and only `location.reload()` told the truth.

## 🔑 The method that made this session (keep doing this)
1. ⚠⚠⚠ **REPRODUCE BEFORE BUILDING. A QA prescription is a hypothesis about
   the cause, and it was wrong three times out of six** — #4's fix was already
   the behaviour, #9's would have hidden a negative deduction, #5's pointed at a
   badge that was only reporting. Twice more the item no longer existed at all.
2. ⚠⚠ **THE REVERT IS THE ONLY PROOF. FOUR times this session a GREEN test
   passed against a REVERTED fix**: a trip-wire matching its own docstring; a
   `not.toContain` matching its own comment; two overlapping mechanisms where
   the clamp did all the work and made the real fix untestable; and `is None`
   being true for two different reasons. Every one was caught by running the
   revert, never by the suite.
3. ⚠ **Source trip-wires must read CODE, not the prose about it** — strip
   docstrings (`ast`) and comments before matching. Three occurrences today.
4. **Sweep the family.** The Lacerte parser defect was not reported by anyone;
   it turned up because the CSV importer had the same shape.
5. **An absence is a finding.** "All 3,836 returns are draft" was one query and
   it reframed a vague workflow complaint into a precise missing control.

## 🔴 Open judgment calls for Ken
1. **Autonomous status transitions** — Ken wants them in production. Not built;
   needs his rules (does first entry move Draft → In progress?).
2. **Backfill the wrong-convention assets?** `8c0ddd4` stops NEW bad imports; it
   does not repair rows already stored. A backfill rewrites conventions on real
   client returns in the shared prod DB — Ken's call, and I would show him the
   affected rows first.
3. **Move QA entry off production.** Carpenter/Compton/Conoly/Connell/Carithers/
   Carlyle are real clients on the shared prod DB; the demo DB exists for this.
4. **RS spec gaps queued** (both written against statute, flagged not guessed):
   `D_4562_CONVENTION` has NO RS entry (the 4562 spec's R003 takes `convention`
   as a given and none of D001-D014 polices it); `R-SCHA-CHARITABLE` is silent
   on a negative AGI.
5. *(carried)* Everything in `REVIEW_QUEUE.md` from s142-s174.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. **The app is TESTING until January 2027** — with real, already-filed
returns, which is exactly why the Ledger invoice is now hard-off.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
- **Slate lane:** `Design/SLATE_IMPLEMENTATION_PLAN.md` ·
  `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md`.
