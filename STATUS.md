# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-08 (s232). **RS LANE: Form 8853 Section C — authored,
Gate-1 APPROVED by Ken in-session, seeded, exported, cached.** No app code, no
migration, no deploy. Ken picked spec-first for his last working day before a
10-day absence, so the day went on the one thing that needs him; the app build
now needs nothing further from him.*

*Previous (s231): Form 3800 Part III pass-through rows 1c/4h/4i + the §38(c)(5)
ESB determination, migration 0271 (`9905906`).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax`; demo = `tts-tax-demo`. **The orphan third service
`tts-tax-app` still fails every push — Ken should delete it.**

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

## ⚠ KEN IS AWAY 2026-08-09 → ~2026-08-19 (10 days)
**Availability MINIMAL BUT NOT ZERO** (Ken, 2026-08-07). Batch questions; keep
them low-friction. Nothing is on a clock in that window; the next hard deadline
is 2026-09-15 (extended entity returns). **s232 was deliberately spent on the
Ken-gated item so the away window has unattended work ready.**

---

## ▶ RESUME HERE

### ⭐ NEXT UNIT — DISPATCH THE FORM 8853 SECTION C APP BUILD
The spec is seeded, exported and cached at
`server/specs/8853_sec_c_spec.json`; the deployed
`lookup/8853_SEC_C/export/` returns 200. **All four legs are PENDING and none of
them needs Ken.** Build order and every design decision are in the spec; the
gotchas are in `f8853_1099ltc_source_brief.md` (RS repo).

1. **Model + lane (input).** A Section C record per INSURED (not per policyholder,
   not per 1099-LTC) + 1099-LTC source rows. Row shape and the full lane-registry
   checklist are in the source brief. ⚠ **1099-LTC boxes 4 and 5 must be NULLABLE**
   — they are OPTIONAL for the payer, so a `False` default silently encodes "not a
   qualified contract". Box 3 likewise needs an `unchecked` state distinct from
   both values (it "may not be checked" when the insured was terminally ill).
2. **Compute.** `20 = 18+19` · `21 = 420 × days` · `23 = MAX(21,22)` ·
   **`25 = MAX(0, 23−24)`** · `26 = MAX(0, 20−25)`, plus the terminally-ill short
   circuit (needs BOTH line 16 = Yes AND ADB-only). Rate through
   `_constants_for_year()` — it is indexed.
3. **Render.** ⚠ f8853 is **absent from `forms_manifest.json`** — register the
   2025 face with SHA256 `5582f8137b70251d6292426ee89b78862412cf48d76577b8407e5f5f8775e5e9`
   and build an AcroForm map for Section C. ⚠ Three of the four Filing-Requirements
   populations complete Section C only PARTIALLY (one fills line 17 and nothing
   else) — lines outside the applicable set render BLANK, never zero.
4. **MeF + flow assertions.** This form IS attached and IS transmitted (unlike the
   s226 §704(d) worksheet). Wire FA-1040-8853C-01..05.

**Acceptance criterion to delete:** `tests/test_form_manifest.py` currently pins
the comment *"Form 8853 — never generated"*.

### ⚠⚠ THE TWO THINGS THE BUILD MUST NOT GET WRONG
1. **Schedule 1 line 8e is COMPOSED, not owned.** Its MeF element is
   `TotArcherMSAMedcrLTCAmt` (Archer MSA + Medicare Advantage + LTC) and the face
   says "include this amount in the **TOTAL** on line 8e". Per DECISIONS.md
   (s230 K13g, extended by s232): `8e = Section C component + preparer-keyed A/B
   residual`, and **a return with no Section C keeps whatever the preparer typed**
   (zero-movement guarantee, scenario T13). A single-writer build silently erases a
   keyed Archer figure — a DISAPPEARED number, which is why nobody would report it.
2. **Line 25 FLOORS AT ZERO and the FACE DOES NOT SAY SO.** §7702B(d)(2) makes the
   limitation "the **excess (if any)** of (A) … over (B)". The face prints "-0-"
   guidance on line 26 only. Unfloored, line 26 exceeds line 20 — taxing more than
   was received. Ken approved the floor explicitly at Gate 1. Scenario T14.

### The queue right now
- **1120-S** (`1120S\CC Changes\`): EMPTY at s232 boot (README only).
- **1040** (`1040\CC Changes\`): EMPTY at s232 boot (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

### After the 8853 build — the ruled backlog, unchanged
**§38(c)(6)(A) MFS threshold** (below, needs nothing from Ken), then the 1065 K17a,
GA bulk-sale, both e-file refusals, identity read-back, 1310 box B upload +
`ForeignAddressType`, CR-2026-001.

### ✅ s232 in one paragraph
Ken chose **spec-first** for 8853 Section C — the right call for the last day
before a 10-day absence, since the Gate-1 walk was the only thing that needed him.
Gap confirmed: `lookup/8853`, `lookup/1099LTC` and `lookup/1099_LTC` all 404, no
cached spec, no source brief, and no app compute/model/field map/template.
⚠ **The destination already existed and already failed** — Schedule 1 line 8e is
seeded "Income from Form 8853" as a KEYED line and `form_manifest.py` already
declares `AttachmentRequirement("Form 8853")` on it, so today an LTC client's
taxable payments must be hand-keyed while the manifest correctly reports a required
attachment the app cannot produce. Authored `8853_SEC_C`: 23 facts / 10 rules (all
cited) / 14 face lines (14a-26) / 12 diagnostics / 14 scenarios / 5 flow
assertions, with `check_8853_sec_c_integrity.py` sharing no math with the loader.
**The $420 rate is confirmed three independent ways** (Rev. Proc. 2024-40 §2.62
verbatim, printed on the 2025 face at line 21, and i8853 Example 1's own footnote)
— and ⚠ it is §2.62, the cap on an **EXCLUSION**, not §3.28's age-band cap on
deductible LTC **PREMIUMS** already in DECISIONS.md. **Scenarios T1-T3 transcribe
the IRS's own worked examples verbatim**, so the rate, the greater-of and the zero
floor are validated against an IRS answer key; the gate also reproduces Example 2
Step 3's allocation on the **unrounded** ratio (33,311, not 33,308), confirming the
s230 never-split-an-already-rounded-share rule from the IRS's own arithmetic.
Refusals are declared, never silent, each with its sign checked. Ken approved at
Gate 1 ("Approve as drafted", explicitly including the line 25 floor and the
composed 8e) → sentinel flipped → seeded (135 forms, 18 authority links, all rules
cited) → deployed export 200 → cached, with the cached file's contents **verified
rather than assumed**. RS `a65ce4f` · `61dc5ae` · `f956f74` · `7451a27`.

### ⚠⚠ THE FINDING THAT ALMOST SHIPPED — the statute corrected the draft
§7702B(d)(2) defines the per diem limitation as **"the excess (if any) of (A) the
greater of … over (B) … reimbursements."** "Excess (if any)" is the Code's
floor-at-zero idiom — so **line 25 cannot go negative**. I drafted it UNFLOORED,
because the face prints a floor only on line 26 and the first statute fetch (LII)
returned a **paraphrase with the phrase silently dropped**. A second fetch from
uscode.house.gov caught it. The defect was live, not theoretical: line 20 = 10,000
with reimbursements driving line 25 to −5,000 produced line 26 = **15,000** —
taxing half again more than the taxpayer ever received. Reachable on any short
contract period with low costs and a large expected reimbursement.
⚠ **My integrity gate had independently re-typed the same face-only reading**, so
the two agreeing proved nothing — the s180 lesson (a value landing via two paths
proves nothing) applied to a gate rather than a pin. Now fixed in the helper, the
rule, the fact note and the gate, pinned by scenario T14 **plus a structural
invariant hardcoded independently of the scenarios** (line 26 may never exceed
line 20). The IRS examples still reproduce exactly, confirming the floor doesn't
disturb the published cases.
**The lesson: a paraphrase is not a verbatim, and the face is not the statute.**

### ⚠ Classes that MOVE existing returns or output on next recompute
- **s232: NONE.** Spec-only session — no app code, no migration, no deploy. The
  cached JSON is inert until the build consumes it.
- ⚠ **When the build lands**, making 8e engine-fed IS a movement class: any return
  with a keyed 8e must be unchanged unless a Section C exists (scenario T13 is the
  regression that pins it).
- Carried from s227/s228: a 1065 K-1 whose §704(d) worksheet is SAVED moves;
  a 1065 row with the basis checkbox ticked swaps its warning code;
  #10 8959 single-W-2 engage (intended); #6 Sch 1 24k engine-fed blank→0.

### ⚠ Known red / rotted (not this session's changes)
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  `ValidationError`. Pre-existing (s219). Not diagnosed.
- `test_4868.py` — 4 tests on the Schedule 3 line-10 feeder (s217). ⛔ KEN.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  batch-005 #9 PTET-gate class, red since s212.
- **DiagnosticRule unique-code contamination** (s225): `test_backentry_
  cleanup.py` (red alone under `--reuse-db`), `test_ga500_auto_attach_
  s106.py`, `test_ga500_rie_federal_pull.py`. ⚠ The 12 new `D_8853C_*` codes are
  spec-side only — nothing seeded into the app catalogue this session.
- **Client typecheck**: 55 error lines standalone (untouched by s232 — no client code).
- ⚠ The Slate 8889 fixture cast `as HSAAccountRow` still swallows new
  required fields.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). `--reuse-db`.
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout
  loses ALL output — redirect to a file and tail it.
- ⚠ A `poetry run python script > file` redirect BUFFERS. Use `-u`.
- ⚠ `grep -rl ... tests/` matches `__pycache__/*.pyc` — pass `--include=*.py`.
- ⚠ The Bash tool's cwd PERSISTS across calls — use absolute `cd` each call.
- ⚠ **New (s232)**: a stdout **redirect goes through cp1252** on this box and dies
  on ligatures (`ﬁ`) and the U+2212 minus that IRS PDFs and our own specs use.
  Write UTF-8 from inside Python (`open(..., encoding="utf-8")`) rather than
  redirecting; it killed an instruction dump mid-file at page 4 and truncated a
  JSON inspection.
- ⚠ **New (s232)**: a script that does `django.setup()` against the RS project
  must run **from the RS repo root** or `server` is not importable. Copy a
  scratchpad helper in, run, delete.
- ⚠ **New (s232)**: `manage.py shell -c "..."` opens the interactive console and
  prints nothing. Use a script file (the standing MEMORY.md note, now confirmed
  for `shell -c` too).

### ✅ KEN DECISIONS OUTSTANDING
- **⛔ KEN (s231, carried — a real defect, queued not fixed): §38(c)(6)(A), the
  MFS threshold.** `compute_3800.SEC38C1_THRESHOLD` is a flat $25,000 (pinned by
  `test_form3800_compute_leg.py:39`). The statute makes it **$12,500** for an MFS
  taxpayer whose spouse has any business credit. **⚠ The sign: this OVER-allows.**
  Needs a preparer assertion about the spouse's separate return + UI + a
  diagnostic. **It requires nothing from Ken to BUILD** — it is the next unblocked
  unit after the 8853 build. Full write-up in `DEFERRAL_AUDIT.md`.
- **⛔ KEN (s227)**: the out-of-scope-state packet-disposition marker (full
  write-up top of REVIEW_QUEUE). Unchanged.
- **⛔ KEN (s230)**: Form 6765 Section G becomes REQUIRED for tax years
  beginning after 2025 (i6765 verbatim). The RS spec must be re-authored
  before a TY2026 season; `D_6765_G_TY2026` fires from 2026.
- The one live external item: **1040 v5.4 business rules still not in hand,
  active 2026-08-09** (v5.4 schemas ARE on disk; 1041 v5.5 closed).

### 🔎 Carried for triage (s229) — not chased
A filed, exact-tie 1040 shows **worksheet drift on a bare recompute**:
`1040_SCHD_WS` `clc_1` 139,889 → 134,398 and `clc_3` 140,738 → 135,247 (−5,491
each), face still an exact TIE. Worth a sweep: how many filed returns move a
worksheet line on recompute?

### RS AGENDA
- **NEW (s232), folded into the existing `[WO-SOURCETYPE-RECON]` order rather
  than opened as new orders** — two adjacent findings with the same root cause
  (Django does not enforce `choices`): (a) **Rev. Proc. 2024-40 lives under THREE
  `source_code`s** (`RP_2024_40` / `REV_PROC_2024_40` / `IRS_RP_2024_40`); s232
  reused `RP_2024_40` and attached its §2.62 excerpt there rather than minting a
  fourth, and deliberately did NOT repair that row's own invalid
  `source_type="revenue_procedure"` (that rewrites published exports).
  (b) **`TaxForm.status` has 5 rows carrying `active`**, which is the
  *FlowAssertion* vocabulary — the two Status classes have cross-contaminated
  exactly as `source_type` did. ✔ Re-ran that order's survey: counts match its
  2026-08-05 figures, and the alarming-looking `1065`/`1041`/`1120s` values are
  precisely its documented 13 false positives (TestScenario `inputs` payloads) —
  the caveat earned its keep.
- **NEW (s232), small and unowned**: the RS **export serializer omits
  `requires_human_review`**, so a build session reading `server/specs/*.json`
  cannot see which authorities are flagged unverified. For `8853_SEC_C` the one
  live flag is `IRC_101_G` (§101(g)(3) conditions), recorded in
  `DEFERRAL_AUDIT.md` and the work order instead.
- Carried: the s231 Form 3800 five spec defects (the `1a`/`1b`/`1c` `line_map`
  rows describing the PRE-2023 face; no `4h`/`4i` row; no §38(c)(5) rule; the
  ambiguous bare Part III numbering; the three app-added `D_3800_00*`
  diagnostics wanting adoption).
- **Carried and still URGENT for the 1065 arm**: the **1065 Schedule K-1 box-15
  letters** for §41 (6765 spec: `[UNVERIFIED]`) and §45R (8941 spec: unnamed).
  `D_3800_008` excludes those credits until both are verified.
- Carried: s230 Form 6765 items (a)-(e); s228 `D_K1B_FULLY_ALLOWED`; the s226
  `requires_human_review` verbatims; s227, s224, s223 and earlier unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
