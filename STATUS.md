# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-27, session 124 (suite-gate settlement — all 8
pre-existing failures explained and closed — plus the Form 4562 `D_4562_RECON`
§179 fix that work uncovered; app `931f4a6` + the RECON leg; RS `6e61341`;
**no migration**).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.
- *(s119–s123 detail is archived in `STATUS_ARCHIVE.md`.)*

## ✅ PUSH-HOLD LIFTED AND PUSHED — Ken's call, 2026-07-27 evening

`91f303d..9abe664` is on origin/main. 15 commits went together: s124's §179
RECON fix, the parallel session's TB-import/mappings work, and Plan 2's SSO
trust layer. Working tree verified green before pushing (72 mappings/import-TB
tests + 114 sso/auth tests).

**Two migration sets ride this deploy — verify both applied:**
- `mappings/0002_mappingtemplate_form_code.py` — **not additive-only**: adds
  `form_code`, backfills, then **drops `one_default_per_firm` and replaces it**
  with a per-form unique constraint. This was the reason for the hold.
- `sso/0001_initial` + `sso/0002_enable_rls_on_supabaseidentity` — new table
  only, additive, RLS default-deny.

**Still owed after this deploy (house rule: migrations run against BOTH DBs):**
1. Confirm Render's deploy applied the migrations to **prod**, then run the
   **demo** leg by hand: `TTS_ENV=demo migrate`. A drifted demo is a broken
   sales demo (DECISIONS.md).
2. Probe the live site, not the deploy status — the launcher go-live lesson
   was that a green deploy proves nothing about what is served.
3. Run `powershell -File scripts\sync_status_mirror.ps1` for the
   `tts-tax-status` mirror (held while this was unpushed; `BUILD_ORDER.md` is
   already edited locally in `D:\dev\tts-tax-status` and the sync's
   `git add -A` will carry it).

⚠ The parallel session still had **uncommitted** work in the tree at push time
(`server/apps/returns/views.py` modified, plus untracked
`server/apps/returns/tb_import.py` and `server/tests/test_tb_import.py`).
Those were deliberately NOT staged — they are that session's to finish. Nothing
committed references them, so what shipped is self-contained.

## ▶ RESUME HERE

**PLAN 2 IS BUILT AND GREEN, FLAG-OFF — 3 commits, still UNPUSHED behind the
hold above.** Ken's directive (2026-07-27 evening) was the SSO trust layer in
this repo. Done through Task 7 of
`delvio-launcher/docs/superpowers/plans/2026-07-27-delvio-tax-sso-trust-layer.md`
(plan doc written before the code, as directed).

New `apps/sso/`: reader for the Launcher's chunked `sb-<ref>-auth-token`
cookie, ES256 JWT verification against the project JWKS, a `SupabaseIdentity`
mapping table + migration (**generated, NOT applied** — push-hold),
`app_access('tax')` enforcement, middleware + DRF auth class, and the SPA
login redirecting to the Launcher. **114 tests green** (59 new sso + 55
existing auth/session/firm, no regressions). `SSO_ENABLED` defaults false, so
main behaves exactly as it did.

**Before this can go live (Plan 2 Task 8 tail, then Plan 3):**
1. Ken lifts the push-hold -> push -> the `sso` migration applies on BOTH DBs.
2. Flag-on rehearsal against the real project with Ken's own login, local
   only — including confirming which `auth_user` row is his. That table holds
   just 4 rows and the launcher's notes assume `id=2`; verify, don't assume.
3. `SSO_ENABLED=true` on Render only at cutover, with `SSO_REQUIRE_AAL2`
   staying false until MFA enrollment is done (that split is deliberate —
   see DECISIONS.md).

**Four findings from the prep-step auth audit that change Plan 3's scope:**
- **delvio-1099 reads DIFFERENT cookie names** (`sb-access-token`) than the
  Launcher writes (`sb-<ref>-auth-token`, chunked). Setting `COOKIE_DOMAIN`
  there shares its OWN cookie — it will NOT see the Launcher session. Port
  `apps/sso/cookies.py`; it was written dependency-free for exactly that.
- **delvio-1099 auto-enrols any authenticated user as `staff`**
  (`api/auth.py:91-117`). Under a shared suite cookie that silently grants
  everyone 1099 access. Security fix; must land before 1099 joins.
- **sherpa-portal is on `portal.delvio.com`** — a different zone — and its
  staff surface is Django admin only. Its client magic-link sessions ride the
  same `sessionid` cookie, so widening `SESSION_COOKIE_DOMAIN` there would
  broadcast CLIENT sessions suite-wide.
- **Check-In has no staff auth at all** on `/desk/*` (shared password on
  `/admin` only). Build-from-zero retrofit, not a migration.

The design doc's "the other apps already speak Supabase" holds for 1099 only.
Full audit table + the three design departures: plan doc §1-§2; departures
recorded in DECISIONS.md ("P2b trust layer — four build decisions").

**s123's Form 2210 work is fully shipped and verified live** — nothing left
hanging there (deploy `42a854b`, migration 0221 on BOTH DBs, `seed_rules` run
after the deploy). No open in-app bug reports.

s124 took the one clearly-unblocked thing on the board: the **8 test failures
s123 had verified as pre-existing but never explained**, two of which it had
flagged as "one-line re-pins someone should take."

### All 8 settled — and none of them was ordering noise

Re-run in **isolation first**: all 8 still failed. So the s108e lesson held —
they were not inheritable as noise.

**Five were stale expectations; the product code was correct in every one.**
The `D_4562` family list predated `D_4562_DEST`/`BASIS`/`RECON` (s116/s118) ·
the manifest trip-wire said 93 forms against a 95-form manifest (f8879 + f8878,
the s94 signature pair) · `TestOfficerCompensationFlow` used the pre-renumber
1120-S page-1 numbering (the 2025 face runs **20** = Other deductions, **21** =
Total deductions, **22** = Ordinary business income, per RS `1120S_PAGE1`).

The fourth was a tax-law one. `TestAAANegative` pinned *"distributions can
drive AAA negative."* They cannot — **Reg. §1.1368-2(a)(3)(iii)** decreases AAA
by distributions **but not below zero**, and RS `1120S_M2` R002 was corrected to
match on 2026-07-12 (Ken-ratified 07-13). Because `test_1120s_spec.py` already
pins both arms (`TestM2DistributionCapNNA` for the cap,
`TestM2LossCanMakeNegative` for losses), that class is **RETIRED with a
pointer**, not rewritten into a duplicate asserting the opposite rule.

**The sixth was a real coverage hole.** `test_8915f::TestLandingChain` asserts
Schedule 2 line 8 — the §72(t) additional tax. That line has **two silent
gates**: `compute_retirement` runs `compute_5329_db`, its only writer, solely
when the 5329 FormDefinition exists, and `_write_sch2_line8` no-ops when SCH_2
is unseeded. The module seeded neither, so line 8 simply stayed blank and those
assertions had **never tested anything** — they could only ever have passed on
seeds leaked from another module's `django_db_blocker` fixture, which commits
outside the per-test transaction. The product path was proved correct first
($18,000 code-1 distribution → $1,800), then the module was made self-sufficient.

## 🔴 That work uncovered a live defect — FIXED (Ken-approved in-session)

Widening the stale `D_4562` family made `D_4562_RECON` visible on the §179
pipeline test, and it was **raising a blocking RED on a CORRECT return.**

$10,000 of equipment fully elected under §179 against $8,000 of Schedule C
income: the engine does the right thing — Schedule C line 13 carries the
**allowed 8,000** and Form 4562 line 13 carries **2,000** to next year, exactly
as §179(b)(3) requires. The s116 rule compared the destination against
Σ(`current_depreciation`), still holding the **full 10,000**, and told the
preparer *"The difference would file a wrong return."*

The RS spec's condition was the generic unconditional equality with nothing
about the limitation, so it was flagged rather than guessed. **Ken chose the
proper fix** over downgrading the severity or deferring.

**The fix (RS `6e61341` → R020, then the app leg).** Reconcile in two parts:
- **(a)** every destination must carry at least its **non-§179** total — less
  than that is ordinary depreciation that failed to route, the original
  silent-skip class, still blocking;
- **(b)** the §179 that actually landed across the business and farm schedules
  must equal Form 4562 **line 12** — the amount allowed after the limitation,
  not the elected amount and not line 9.

With no §179 on the return the two parts **collapse to the original strict
equality**, so this is *strictly stronger* for the ordinary return rather than a
relaxation: a real routing gap now breaks the tie to line 12, where before the
return was already failing for a benign reason and the real gap was
indistinguishable from it.

**A correction I made to my own report.** I told Ken this was "live on prod
today." A read-only scan says otherwise — only **2 tax years** in the shared DB
carry a §179 asset at all and neither currently trips it. The defect is real and
reproducible; it had not yet bitten a stored return.

**RS leg discipline held:** face text for lines 11/12/13 re-pinned **verbatim**
off the local SHA-tracked `f4562.pdf` (pymupdf, same day), never memory; the
harness re-implements the **pre-amendment** condition and asserts it *misfires*
on the limited-but-correct scenario, so it proves the amendment changes a real
verdict instead of echoing the authored answer; **three perturbation controls
each observed failing** before restore. Seeded → deployed export verified
carrying R020 → mirrored verbatim into `server/specs/form_4562_spec.json`.

## ▶ NEXT (cold-start pointer)

**Ken's call on pushing s124** (test-only + a diagnostics change; no migration,
so this is an ordinary deploy). Then the s123 pointer stands: **Form 2210 Part
II boxes A/B/D/E** (the unit that would let a 2210 actually be transmitted — box
D is a penalty-REDUCING election we don't offer) · **item-6-P1 GA residual**
(still BLOCKED on the two GA REVIEW_QUEUE questions).

Batch-001 remains **13 of 16**; opens = GA residual · the two QA cases item 15
deliberately did not cover (W-2 boxes 3/5, Schedule C net loss with no detail).

## ▶ Waiting on Ken / external
1. **s124 ratifications (REVIEW_QUEUE): the two scoping calls inside the
   `D_4562_RECON` fix** — accrual Schedule F scoped out of part (b), and part
   (b) standing down in the pure-prior-year-carryover shape. Neither is
   IRS-sourced.
2. s123 ratifications: the narrowed 2210 e-file policy · Part II box sequencing
   (incl. box D) · D_2210_TIE at warning · the stale-2210-face fix when line 38
   is overridden.
3. s122 ratification: the source-summary listing line (nominee/accrued/ABP arm).
4. s121 ratification: the `D_8283_017` severity ladder (conservation arm).
5. s118 ratifications: §280F AMT-arm derivation · GA no-bump table.
6. s115 ratifications: 8962 Part IV blank-pct · line 34/4-row cap · line-9 marriage-alt.
7. s114 ratifications: the 8867 rebuild's three judgment calls.
8. s113 ratifications: D_GA500_002 realignment · 2210 flat-7% · 7206 partner-arm scope.
9. Item-6-P1 GA residual — BLOCKING questions: GA line 5 filing status from
   federal? · couple the GA deduction election to the federal election?
10. s112 ratification: manifest-aware RS amendment (mechanism only).
11. 86 backfill review rows (83 effective) · S-24 hub-ein blanking · auth env
    vars · A2A WSDL · WISP · SEC-5 · Resend · role assignments · e-services ·
    CAF · ERO EFIN/PIN · beta clauses · older ratifications (s110 · s106 ·
    s101(4) · s100(3) · s99a · s97 · s96(4) · s95..s72).

## Active gates
- **Deploy: `42a854b` (s123) verified live** on prod + demo + prep. **s124 is
  committed and NOT pushed — held on Ken's instruction** (see the entanglement
  block at the top).
- **DB state: no migration in s124.** Migration 0221 (s123) is applied to BOTH
  DBs. ⚠ The parallel session's `mappings/0002` is committed locally and
  UNAPPLIED — it rides whichever push lands first.
- **Full server suite: 6,420 passed / 21 skipped / 0 failed (48:02)** against
  the s123 baseline of 6,401 passed / **8 failed**. All eight settled, nothing
  new broken.
- **`seed_rules` IS required after the s124 deploy** — `D_4562_RECON`'s catalogue
  *description* changed (no new rule, no severity change). Run on BOTH DBs, after
  the deploy, per the standing order.
- **RS:** `FORM_4562` at `6e61341` — 20 rules / 17 diagnostics / 39 scenarios;
  deployed export verified and mirrored verbatim to
  `server/specs/form_4562_spec.json`.
- ⚠ FA-1040-4835-06 drift (chip `task_0cf10eac`, unchanged since s113).
- ⚠ **Dev-environment repair (not a code change):** `server/.venv/pyvenv.cfg`
  pointed its base interpreter at a `C:\Users\Ken2\…\Python313` profile that no
  longer exists, and the venv stopped launching mid-session. Repointed to the
  real `C:\Users\Ken\…\Python313` — **same version, 3.13.14**, packages
  untouched; `.venv/pyvenv.cfg.bak` holds the original. Worth knowing if Ken's
  own shell hits it.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged. No piecemeal ATS testing.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
