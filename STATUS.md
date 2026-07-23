# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-23, session 105 (Ken-directed PRIORITY — the live
back-entry pilot's QA queue). **THE SEVEN ENTRY-LAYER BUGS FROM THE 1017
PILOT ARE CLOSED (`166e6ef`; mig returns.0209 state-only; flow 518
stands — ZERO compute code touched).** The pilot's headline finding was
that the COMPUTATION ENGINE IS RIGHT: every line matched TaxWise to the
dollar (SS taxability worksheet, the 2025 senior deduction w/ MAGI
phaseout, age-65 standard deduction, brackets, withholding). Everything
found was INPUT-layer, and all of it is fleet-scale. Fixed: **(1)** the
10x amount-mask bug → NEW `lib/selectOnFocus.ts`, ONE delegated focusin
handler at boot, select-all on every amount box (TaxWise/Lacerte) +
15-char cap — one place, not an onFocus prop on ~280 raw inputs;
**(2)** card lists re-sorting mid-entry → the cause was
`Meta.ordering = ["order", <editable name>]` with every card at order=0
(so: sort alphabetically by payer, re-sorted per keystroke; "New Payer"
jumped above "Teachers Retirement System" and the next keystrokes landed
on the WRONG card → box 2a 4362511485, $1.68B refund). Tie-break is now
immutable `created_at` on ALL EIGHT document-card models; **(3)** numeric
overflow 500 → NEW `MoneyBoundedSerializer` 400s at ≥$1B with a per-field
message (the columns are wide enough to STORE the garbage, so DRF's own
max_digits never fired); **(4)** the filing-status wipe → `useTaxpayerFacts`
keeps dirty fields across a server re-seed AND flushes them with whatever
commits; **(5)** DOB commits when COMPLETE (not only on blur) + NEW
`D_1040_018` warning (blank DOBs silently ate $15,200 of deductions on the
pilot); **(6)** `D_EIC_007` → INFO when auto-resolved from the DOB,
`LATE_FILING` suppressed for `backfill-2025`; **(7)** the seeded-shell
banner now says "Seeded for 2025 back-entry". PLUS the NEW DEFAULT
**"Tax Shelter" forest theme** (Ken: "more forest green, less cream") —
forest #1e362a structure, pale-green canvas, gold #a9884f garnish;
**Ledger stays as the one-click rollback**; RED/YELLOW/GREEN untouched.
**The fleet can now run.***

*s104 (2026-07-23, Ken addendum to S-25). **THE
2025 BACK-ENTRY SHELLS ARE SEEDED** — `seed_backfill_returns`
(`920a940`): roster-driven from all six client-master exports (D-24
matching reused verbatim; never creates clients/entities — asserted in
code + tests 6/6; idempotent/resumable; dry-run default). Ken approved
the counts (3,287/149/86) → trial 25 → full chunked run: **3,279 draft
2025 shells on prod (2,809×1040 · 300×1120-S · 70×1065 · 26×1120 ·
74×1041), all stamped rolled_from_source='backfill-2025' (the cull
key), clients 3,675 / entities 3,979 unchanged, post-run dry-run
to-create 0.** 86 review rows await Ken in
D:\dev\client-master-exports\backfill_review.csv (41 name-variant ·
37 contact-conflict · 3 multi-candidate · 3 no-entity-of-type · 2
merge-email). `build_federal_return` is now a module service — one
build path for API + seeder. **The back-entry project is UNBLOCKED
end-to-end: shells exist; Start Return covers strays.***

*s103 (2026-07-23, Ken-directed PRIORITY, Spine
S-25). **THE START RETURN FLOW SHIPPED (`7b1312b`) — the 2025
back-entry blocker is CLEARED.** There is now a correct way to attach
a return to an EXISTING hub client: POST `/tax-returns/start/`
{entity, year} find-or-creates the clients_taxyear and builds the
right form for the entity type, NEVER creating client/entity rows
(invariant test-pinned; existing return → "open existing"; row-locked
against concurrent fleet double-create). Reachable from the entity
folder page (Start Return button, year defaults to the shared
ACTIVE_TAX_YEAR=2025) and the NEW Ctrl+K Client Search palette
(digits = exact client_number; replaces the disabled menu placeholder;
also the New Return action). "+ New Client" now has a did-you-mean
typeahead against existing clients in both RM and Client Manager.
Returns menu gained Individual/Trust lists (dead ?form= links fixed).
Gates: test_start_return 8/8 · returns regression · tsc 0 · vitest
300 · demo probe · **PROD ACCEPTANCE LIVE: "1017" → the pilot client →
folder → Start Return → 2025 → blank 1040; zero new client/entity
rows (3675/3979 unchanged); probe artifacts fully removed.** Render
deploys `7b1312b` → Ken can repeat on prep.delviotax.com. (The RM
client_number column half of the ask was already live from s102.)*

*s102 (2026-07-23, Ken-directed UI unit). **CLIENT
NUMBER SURFACED IN THE RETURN MANAGER** — sortable `#` column (narrow ·
muted · tabular-nums · blank when un-numbered, matching Ledger's desk-
search presentation), server-side sort (`ordering=client_number`,
un-numbered last both directions), and digits-only search now hits
client_number exactly (the clients-endpoint convention; tab counts stay
in lockstep). Enablers were read-only (list-serializer field + ordering
map + search OR — no migrations, no compute); the Clients page already
had its column since Phase 8. NEW test: list payload/sort/search pinned
in test_returns (78 green) · tsc 0 · vitest 300 · live demo probe green
(sort click + "1001"→Blue Ridge DOM-verified). **Demo DB clients are now
NUMBERED 1001–1011** (`assign_client_numbers --apply`, was never run on
demo). **Dev-port coexistence shipped** (the ledger parallel-session
conflict): django-demo honors PORT + `autoPort: true` in launch.json,
and writes `client/.api-port` (gitignored, 12h-fresh) that the vite
proxy follows — the demo pair now coexists with another app's server
on 8000. ClientFolders (documents list) still lacks the number —
follow-up candidate, deliberately out of scope.*

*2026-07-22 (delvio-ledger phase 5, cross-repo unit): **apps.ledgerlink
added** — on a return's …→approved transition in update_info, the keyed
INV_* fees auto-post to Delvio Ledger as a posted invoice (outbox +
service token + inline delivery, `manage.py flush_ledger_outbox` retries;
INV_TOTAL empty = soft-skip). Env-gated: `LEDGER_API_URL`/
`LEDGER_SERVICE_TOKEN` unset = fully inert (currently unset — goes live at
Ledger's Render deploy). Migration ledgerlink.0001 applied to the shared
DB. 13 new tests (tests/test_ledgerlink.py). Governance: this unit belongs
to delvio-ledger's build (its SPEC §Cross-repo work units) — the Ledger
repo's STATUS/DECISIONS carry the details; nothing new in this repo's
queue.*

*s101 (2026-07-19, Ken directing — back from OOO).
**THE 1065 SCHED_B 2025-FACE RENUMBER SHIPPED (the s99b call, Ken's go;
mig returns.0208; flow 518 stands).** The stale pre-face Schedule B
block (25 rows, own numbering, face Q4 on app B6) is now FACE-TRUE:
67 rows = questions 1-33 verbatim vs f1065.pdf pp. 2-4 + the RS 1065_B
line map (10e/13b/31 reserved, not seeded) + inline detail fields +
the PR designation block. Mig 0208 renamed keys in place (FFVs ride
the FK; B6→B4 the headline), deleted the dead questions (old-B2
disregarded-entity, old-B5 Form 8893/TEFRA), and split old B12b into
face 10b/10c ('No' to both, 'Yes' to neither — none existed). Q4
auto-answer, L/M-1/M-2 waiver gates, FA runner pins, and the client
Auto pill all moved B6→B4 in lockstep. BOTH DBs migrated + reseeded +
audited: every carried answer landed on its face key; zero overridden
values existed. NEW face pins in test_seed_1065 (keys + labels — the
s100 discipline). Four convention calls → REVIEW_QUEUE s101 (B3→3b
carry · B12b split · B33_PR_TIN app-boundary · RS facts gap for inline
amounts). Sch B print = pre-existing gap, unchanged (DEFERRAL s101).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE
**Ken directives standing (s48 + s52 addendum): work AUTONOMOUSLY down this list;
full gates + live probes; Ken-decisions → REVIEW_QUEUE with a recommendation, then
move on; mandatory session close before context exhausts.**
0. **s105 leftovers from the 1017 pilot QA that were NOT in Ken's list —
   both still open:** (a) **the PREPARER ROSTER is incomplete** — only
   Georgianna Lill, Ken Lill, Whit exist; **Gail Daniels (PTIN P00141816)
   actually prepared 1017** and is missing. Seed the full preparer list
   WITH PTINs before the fleet runs (Ken supplies the roster). (b) **GA
   Form 500 was never attempted on the pilot** — that is the next pilot
   step (TaxWise answer key: GA w/h 1,503 · GA refund 1,503 · GA tax 0
   after the retirement exclusion). Also noted by the pilot: no obvious
   8879 PIN / third-party-designee entry section was found in the UI.
1. **Start every session with `/bugs`** (s55; s101 sweep: clean).
2. **S-17g A2A channel still jumps the queue the moment the WSDLs land**
   (`docs/mef/wsdl/` still absent; .p12 DONE; ASID 61135801 ENROLLED).
3. **The autonomous queue is EMPTY again** — the s99b sched_b renumber
   completed s101 on Ken's go. Next candidates remain Ken-gated or
   Ken-directed: the 1120/709 authoring waves · the 1120-S ATS upload
   lane (full scenario set + e-help) · the SEC-5 plumbing (s95
   ratification) · S-24 prod backfill (key hand-off). **Idle — Ken
   directs**, unless the WSDLs land first.
4. **Ken-gated follow-ups:** S-24 prod backfill on the key hand-off →
   hub-ein blanking (s97) · SEC-5 plumbing on the s95 ratification ·
   S-22b triage confirmations (6252 · 9325).
5. **Auth lane (s94):** Ken's env vars → `supabase_users --list` +
   live `supabase_login` verify → P1 identity model.
6. **Ken ratifications pending:** s101 (4 sched_b conventions) · s100
   (other-ded fix FYI + 3 mapping conventions) · s99a (Q4 4d; s99b
   RESOLVED — shipped s101) · s97 (S-24 trio) · s96 (WISP 4 calls) ·
   s95 (retention · PITR · HSTS preload) · s94 (8879 col-C @ ATS +
   stockpiling proxy) · s93 (4h idle) · s89 (8915-F rounding) ·
   s85/s84 pairs · s83 Resend · s76..s72 notes.
7. **Design: Ledger live; cross-app application on Ken's go.**

## ▶ Waiting on Ken / external
1. **S-24 identity keys (s97):** copy both TAX_IDENTITY_* keys from
   local `server/.env` to Render `tts-tax-app` (SAME values) → CC runs
   the prod backfill (590 staged) → then the hub-ein blanking leg.
2. **Auth env vars (s94):** SUPABASE_URL + ANON_KEY (+ SERVICE_ROLE).
3. **A2A: ONLY the WSDL toolkit remains.**
4. **WISP v0.1 ratification (s96)** — 4 calls in REVIEW_QUEUE s96.
5. **SEC-5 [EXT] legs (s95):** SOC 2 pull · restore drill · PITR call.
6. **Email provider setup (s83)** — Resend, SPF/DKIM, Render env vars.
7. **Role assignments (s84):** `manage.py set_user_role`.
8. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
9. E-services email reply (S7/S8 · 8941 key-inversion · production flip · SOR).
10. File-1018 Lacerte reprint. 11. PWA install check. 12. Density feel-check.
13. s69: firm's real CAF number + fax on the Preparer records.
14. Demo Render service (env=demo; needs the DEMO key pair from
    `server/.env.demo` if/when it exists).
15. **ERO EFIN/PIN source (s94):** wire 8879/8878 cards from Preparer.
16. **Cross-app flag (s95):** delvio-1099 `filing_dashboard` →
    security_invoker on next touch.
17. **Beta-agreement security clauses (s96):** with counsel.

## Active gates
- **Flow-assertion gate GREEN at 518** (s94 level; s105 re-ran green —
  no compute code was touched). Mirrors: 1120S 41 · 1065 39 (+4 s64
  staged) · 1040 415.
- NEW s105: `test_entry_layer_s105` **10** (stable card order + the
  no-editable-name-in-Meta.ordering DRIFT GUARD across W-2 / 1099-INT /
  1099-DIV / 1099-R; overflow 400-not-500) · `test_entry_layer_diagnostics_s105`
  **11** (D_EIC_007 severity both ways · LATE_FILING suppressed only for
  backfill-2025 · D_1040_018). Client: `selectOnFocus.test.ts` 17 ·
  `useTaxpayerFacts.test.tsx` 4 · `apiErrors.test.ts` 4 → **vitest 325**
  (was 300) · tsc 0. **`test_1040_spine_diagnostics` rule-count pin re-cut
  17 → 18** (D_1040_018) — when it fails: verify the new rule, update the
  count, never delete the pin.
- NEW s101: test_seed_1065 face pins (test_schedule_b_face_keys +
  test_schedule_b_face_labels — **when a pin fails: verify the new
  face, re-key, update the pin; never delete it**). s100:
  test_line_key_registry_sweep 13. s99: test_1065_schb_q4 7 (now pinned
  on B4). s97: test_tax_identity 24. s94 suites stand: test_8879_8878
  33 · returns 77 (355-line 1065 pins) · MeF/extract 105 · tsc 0 ·
  vitest 300. s89: test_8915f 49 · FULL efile band 966.
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: migs through returns 0209 + audit 0004 +
  core 0004 + clients 0010 BOTH DBs (s105 shipped returns.0209, a
  state-only AlterModelOptions — applied BOTH DBs); s105 re-ran
  `seed_rules` on BOTH DBs (registers D_1040_018); s101 reseeded
  seed_1065 BOTH DBs (355 lines; sched_b 67 face-true rows);
  post-migration audit clean on BOTH.**
- 🔴 **THE RENDER DEPLOY WAS BROKEN SINCE s104 — FIXED s105 (`e86a718`+).**
  `build.sh` runs `manage.py seed_all`, which DISCOVERS every `seed_*`
  command by name and runs it with NO arguments against prod. s104's
  `seed_backfill_returns` is not a form seeder — it takes a required
  `folder` positional pointing at roster CSVs that exist only on Ken's
  machine — so it raised, `set -o errexit` stopped the build, and the
  deploy died. **THREE deploys were silently blocked (s104, s105, the
  s105 follow-up); prod bundle verification confirmed it was still
  serving s103 code** (`Start Return` present, `Seeded for 2025
  back-entry` and `theme-forest` absent). Fix: commands opt out with
  `deploy_safe = False`; `seed_all` skips + warns; NEW
  `tests/test_seed_all_deploy_safety.py` **5** fails CI instead of the
  deploy. **THE CONTRACT: a `seed_*` command runs on every production
  deploy — it must be idempotent, runnable with NO arguments, and
  dependent on nothing outside the repo + DB. If not, `deploy_safe =
  False` and run it by hand.**
- ✅ **DEPLOY VERIFIED GREEN 2026-07-23** — prod bundle
  `/assets/index-vIpvftZV.js` (byte-identical hash to the local
  `vite build`) carries `theme-forest`, `data-select-on-focus`, and the
  seeded-shell banner; `/api/v1/version/` 200. Since Django+WhiteNoise
  serves the SPA from the same container, a new bundle means the SERVER
  code is s105 too. **s104 + s105 are finally live on
  prep.delviotax.com.** ⚠ When probing a bundle, do NOT grep
  JSX-interpolated UI text (`Seeded for {year} back-entry` is split into
  two literals — it false-negatives); use a class name, a `data-*`
  attribute, or an uninterrupted sentence.
- ⚠ **Render prod still has NO identity keys** (s97 Waiting §1).
- ⚠ HSTS lands on the next tts Render deploy (s95).
- ⚠ Local test-DB after new migrations: the s86 setup_databases(keepdb)
  recipe, then `--reuse-db` (0208 is data-only — reuse-db was safe s101).
- ⚠ Restart django-demo after server edits (--noreload) AND hard-reload
  the SPA tab afterwards (vite keep-alive; s97). The demo server is
  API-only — SPA probes ride the `vite` launch config at 5173 (s99).
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help first).
- Design rollback points: tag `pre-ledger-design` · old presets.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
