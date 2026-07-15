# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-15, session 89. **FORM 8915-F SHIPPED — the LAST
of the six Gate-1-dispatched tts legs; THE SET (9465 · 8888 · V/ES ·
4868 · 8915-F) IS COMPLETE.** The full unit in one session: NEW
Form8915F + Form8915FDisaster (migs 0204 + RLS 0205, BOTH DBs) — one
row per OWNER per item-B disaster year (married = a separate form per
spouse) with nested item-C FEMA rows; compute_8915f pinned to all 10
RS oracles (**the 179/180-day ONE-day asymmetry pinned both directions
against every published date example incl. the SECURE-floor arm** ·
the 1a-1e ladder + single-new-disaster shortcut · the Rev-12-2025
5a/5b redesign · the ÷3.0 spread — whole-dollar ROUND_HALF_UP adopted
for the flagged convention, REVIEW_QUEUE s89 · the 11↔22 matched
opt-out boxes · the $22k/$100k cap); **the landing MOVES: 1040 5b −=
line 10 += line 15 · 4b −= line 20 += line 26 · Part IV 30/32 route by
plan type; the 8606 face now SPLITS 15a/15b/15c + 25a/25b/25c off the
8915-F lines 18/19 (owner_lines gained the QDD ties — print/XML/
compute agree); the 5329 line-6 waiver suppresses the early-tax base
(line 6 NEVER generates a 5329 — line 7 + Part IV 32 keep their
exposure)**. 15 D_8915F_* seeded BOTH DBs. f8915f Rev-12-2025 AcroForm
print (manifest 93; the lines-1-5 table SHADING-PROBED — 1a-1e fill
column (b), 5a column (a)); per-owner copies; packet seq 915 +
render-8915f endpoint. NEW IRS8915F MeF document (ReturnData1040 slot
2014, maxOccurs=6, per-document name/SSN; item A/B XSD choices;
refusals name the paper path; live-XSD valid with TWO documents).
**FA-8915F-CAP/SPRD/LAND activated (RS export 413 verified, mirror 412
export-verbatim ASCII, runners in both chains) — flow gate 515.**
Suites: NEW test_8915f 49 · flow 515 · seam band 150 · FULL efile band
966 · tts_forms band 355 (trip-wire 93) · tsc 0 · vitest 300 · live
demo probe green (banner painted 1e/6/7/15 + the per-disaster 179/180
dates live; ORM 5b 10,300 → 3,433 → delete-restores; diagnostics fired
exactly LANDINGS+WAIVER; render 200 %PDF; demo restored). WO-32 → ✅
DONE in RS — the RS dispatch lane is EMPTY.*

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
1. **Start every session with `/bugs`** (s55; s89 sweep: clean).
2. **S-17g A2A channel still jumps the queue the moment the WSDLs land**
   (`docs/mef/wsdl/` still absent at s89; .p12 DONE; checklist
   `docs/mef/A2A_ENROLLMENT.md`; ASID 61135801 ENROLLED + ACTIVE).
3. **The six dispatched tts legs are ALL BUILT (9465 s85 · 8888 s86 ·
   V/ES s87 · 4868 s88 · 8915-F s89).** Next NEW autonomous item:
   **the 8879/8878 print pair** (e-file signature authorizations —
   spec-first gap check per the house rule) → **SEC-2..6** → **S-24**.
   Still open from the S-22b triage: confirm 6252 · 9325.
4. Then the s71 queue: **bootstrap_demo 1065+1041 demo returns** → **S-21b
   1065 partner-percentage diagnostic** → **S-21c Sch B Q4 auto-answer**
   (spec-first). The 1120/709 authoring waves + the 1120-S ATS lane stay Ken-gated.
5. **Ken ratifications pending:** s89 (the 8915-F ÷3.0 ROUND_HALF_UP
   convention — recommend keep) · s85 pair (RS 9465 condition-wording nit ·
   the F9465-027-01 effective-payment seam — re-check at ATS) · s84 pair
   (front-desk role · ADMIN-only destructive deletes) · s83 email provider
   pick (Resend recommended; `docs/AUTH_EMAIL_SETUP.md`) · the s76 EFW
   late-filing arm · s75 (2210 · 8962 QSEHRA · 8889 multi-account) · s74
   (7203 line-1 fold · loss-without-7203 refusal · partnership 28(e)) · s73
   (23c/23d · Sch E line 27 · print truncation) · s72 (8867 face-fidelity ·
   Sch B K-1 listing row). Standing non-decisions: 3115 OMB nit · 8824 RS note.
6. **Design (s81–s82): Ledger live + Ken-ratified.** Optional follow-ups
   unscheduled. **STANDING: apply Ledger across the other Sherpa apps when
   Ken directs — `Design/LEDGER_DESIGN_SYSTEM.md`.**

## ▶ Waiting on Ken / external
1. **A2A: ONLY the WSDL toolkit remains** (RESUME item 2).
2. **Email provider setup (s83)** — Resend (or equal), SPF/DKIM, Render env
   vars, `set_user_email` per preparer (`docs/AUTH_EMAIL_SETUP.md`).
3. **Role assignments (s84):** `manage.py set_user_role <username> <role>`
   — e.g. promote Whit if he should edit Preparers/Print Packages.
4. `WORK_ORDER_bug_reporting.md` reconciliation flag (s55).
5. E-services email reply (S7/S8 · 8941 key-inversion · 1040 production flip · SOR).
6. File-1018 Lacerte reprint. 7. PWA install check. 8. Density feel-check (s52).
9. s69: firm's real CAF number + fax on the Preparer records.
10. Demo Render service (env=demo; GEMINI_API_KEY blank there).
11. S-22b triage confirmations (6252 · 9325 — add or skip).
12. s81 design check: eyeball Ledger (instant rollback in the picker).

## Active gates
- **Flow-assertion gate GREEN at 515 (was 512)** — FA-8915F-CAP/SPRD/LAND
  activated + `_run_8915f_assertion` live in both chains; 1040 mirror =
  412 active export-verbatim (413 export − the s71-staged
  FA-1040-4835-06). Mirrors: 1120S 41 · 1065 39 (+4 s64 staged) · 1040
  412 (+1 staged).
- **s89 suites: NEW test_8915f 49/49 (incl. live-XSD full-return ×1 w/
  TWO IRS8915F documents + the 4b/5b landing chain + the 5329 waiver +
  the 8606 split) · flow 515 · 8606/5329/retirement seam band 150 ·
  FULL efile/mef/scenario band 966 · tts_forms/acroform/manifest band
  355 (trip-wire re-pinned 92 → 93 with f8915f) · tsc 0 · vitest 300 ·
  live demo browser probe green** (Income tab → card CRUD → typed L2(a)
  10,300 + a dated DR disaster → the banner painted 1e 22,000 / line 6
  10,300 / line 15 3,433 + the per-disaster 179/180-day pair LIVE →
  ORM: 5b 10,300 → 3,433 → diagnostics fired exactly D_8915F_LANDINGS +
  D_8915F_WAIVER → render-8915f 200 %PDF (138KB, 4 filled pages) →
  DELETE 204 → 5b self-healed to 10,300; demo return restored).
  ⚠ Probe note: the Browser pane's synthetic keys/screenshots were
  flaky (environmental) — blur events driven via DOM dispatch; all
  server behavior verified by network + ORM (the s49 DOM-assert lesson).
- s88 suites stand: test_4868 44 · s87: test_1040v_es 41 · s86:
  test_8888 37 · s84: test_authz_sec1 17 · s83: auth 19 · prefs 14.
- Last full-suite GREEN = s54 `cd9b186`.
- **Shared-DB deploy state: migs returns 0204 (Form8915F +
  Form8915FDisaster) + 0205 (RLS pair) applied BOTH DBs + seed_rules
  run BOTH DBs (15 D_8915F_* live)** — on top of 0202/0203 (s88),
  0200/0201 (s87), 0198/0199 (s86), 0197 (s85).
- **RS state: WO-32 → ✅ DONE (RS `6082314`)** — FA export 413 active
  verified; the 8915F spec mirror verbatim-current. **The WO-28..32
  dispatch lane is EMPTY.**
- ⚠ Local test-DB note: after a new migration run the s86 recipe once —
  standalone `django.test.utils.setup_databases(keepdb=True)` **under
  `config.settings.test`** (it migrates the LOCAL test_postgres
  forward), then pytest `--reuse-db` works.
- ⚠⚠ 1120-S upload gate unchanged (full scenario set + e-help answers first).
- Design rollback points: tag `pre-ledger-design` · old presets in the picker.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
