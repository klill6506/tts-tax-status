# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-08 (s233). **1040 lane reopened: Codex posted TWO new
batch files (20 items).** BATCH-001 worked 4 of 10 — items 1, 3, 9 (three
defects in one Georgia RIE derive) BUILT, item 7 REFUTED as deploy skew with
the underlying class built out. One deploy. BATCH-001 stays in the queue.*

*Previous (s232): Form 8853 Section C spec authored, Gate-1 approved, seeded,
exported, cached (`434ade4`). No app code — that build is still pending and
still needs nothing from Ken.*

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
**Availability MINIMAL BUT NOT ZERO.** Batch questions; keep them low-friction.
Nothing is on a clock in that window; the next hard deadline is 2026-09-15.
**There is now more unattended work than the away window can absorb** — the
1040 queue alone holds 16 open items plus the 8853 build.

---

## ▶ RESUME HERE

### ⭐ NEXT UNIT — FINISH 1040 BATCH-001 (6 items), THEN BATCH-002 (10)
Both files are in `D:\tax-test-data\1040\CC Changes\`. BATCH-001 carries a
**PARTIAL result annex** naming exactly what is done and what is not; read it
before starting so nothing is re-triaged.

**BATCH-001 open: items 2, 4, 5, 6, 8, 10.** Each is a build, not a defect fix:
8582 AMT carryovers per activity · regular federal NOL carryovers by loss-year
vintage · per-activity nonpassive rental treatment · Form 8862 · Georgia
IND-CR 202 from the federal 2441 · Form 1099-Q Coverdell/QTP.

**BATCH-002: all 10 open.** ⚠ Two of them are the highest-value items in either
file and should probably jump the queue:
- **#2 dependent standard deduction from earned income** — a LIVE computed-tax
  defect, not a missing feature. A dependent with $10,699 of earned income gets
  a $1,350 standard deduction instead of the filed $11,149, creating $933 of
  federal tax where the filed return has zero, and turning a $478 refund into a
  $455 balance due. §63(c)(5). Small, statutory, high blast radius.
- **#5 Form 8960 not engaging on interest-only NII** — a $3,271 NIIT
  understatement; also asks that K-1 portfolio interest count regardless of
  material participation.

### ⚠⚠ THE OVERLAP TO BUILD ONCE, NOT THREE TIMES
**BATCH-001 #4, BATCH-001 #10, BATCH-002 #1, #9 and #10 are all the same
shape**: a future-year tax attribute (regular NOL by vintage, §179 carryover,
charitable carryover by year and limitation class, 1099-Q basis) that the
current face ties without, and therefore silently discards. They want one
attribute-preservation layer — source-year-keyed rows, independent roll-forward,
and a cleanup diagnostic that refuses completion when a filed carryover
worksheet has no home — not five bespoke sections. Design it once.

### ⭐ ALSO STILL PENDING AND STILL UNBLOCKED — the Form 8853 Section C build
Unchanged from s232 and untouched this session. Spec cached at
`server/specs/8853_sec_c_spec.json`; deployed `lookup/8853_SEC_C/export/`
returns 200. All four legs pending, none needs Ken. **The two things the build
must not get wrong** (Schedule 1 line 8e is COMPOSED, not owned; line 25 FLOORS
AT ZERO though the face does not say so) are written up in full in
`STATUS_ARCHIVE.md` under s232 — read that before starting.

### The queue right now
- **1040** (`1040\CC Changes\`): **BATCH-001 (6 open, partial annex appended)
  + BATCH-002 (10 open)**.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

### ✅ s233 in one paragraph
Boot sweep found the 1040 queue reopened with 20 items. Led with the one
claiming a live production failure. **Item 7 REFUTED**: `rules_6765.py` was
never missing — the failing run started 03:26:56Z and the commit adding the
module landed **93 seconds later**. Deploy skew, because the Supabase DB is
SHARED, so seeding rule rows from a laptop publishes rows naming a module prod
does not yet have. Proved prod healthy rather than assuming: a fresh
authenticated run on the same tax year went **24 findings → 12, zero execution
failures**. The mechanism was real and unguarded though, so the class is now
closed — see the movement note below. Then items **1, 3 and 9, which are three
separate defects in the same Georgia RIE derive**: a 1041 K-1 box 5
`other_portfolio` never reaching the base; federal interest fed whole so
U.S.-obligation interest already subtracted on S1-10 was subtracted twice; and
an owner-tagged capital LOSS split 50/50 onto the spouse because the allocator's
weighting helper skipped `amt <= 0`. All three settled by one authority found
this session — **Ga. Comp. R. & Regs. r. 560-7-4-.02**, which says in terms
"Only retirement income that is **included in Georgia taxable income** shall be
included when computing the retirement income exclusion" and "**One spouse may
not use any income attributable to the other spouse**". 8 new RIE tests + 7 new
engine-fault tests; 100 GA-500 tests and all 526 flow assertions green.

### ⚠⚠ THE FINDING WORTH CARRYING — an engine fault wearing a taxpayer's face
Twelve infrastructure failures were recorded as **error-severity findings on a
real return**, and the run that produced them was stamped **COMPLETED**. Both
halves were wrong: the reader cannot tell a deployment problem from a defect in
the return, and nothing downstream could tell a degraded run from a clean one.
Now: import failure and evaluation failure are distinguished and labelled
`details.fault = "engine"`; **any run in which a rule failed to reach a verdict
is FAILED, not COMPLETED** (this strengthens the cleanup gate — findings stay
error-severity and unacknowledgeable); and a Django system check fails
`manage.py check` on a rule the build cannot load.
⚠ **Deliberate design call for Ken**: the system check validates the CODE-SIDE
registry, **not** the DB rows. Hard-failing startup on an unresolvable DB row
would let a rule seeded from a laptop take production down — strictly worse than
degraded diagnostics. The DB side is covered at runtime by the FAILED-run change.
**The lesson: when a shared DB names code, the DB can describe a build that does
not exist — and the failure will surface wherever the code is read, not where
the row was written.**

### ⚠ Classes that MOVE existing returns or output on next recompute
- **Diagnostic runs**: a run containing any engine fault now reports **FAILED**
  where it previously reported COMPLETED. Findings are unchanged in severity.
- **GA RIE line 13**: a return with a 1041 K-1 carrying `other_portfolio` now
  gets a LARGER exclusion (it was missing income it was entitled to exclude).
- **GA RIE line 6**: a return with a nonzero S1-10 now gets a SMALLER exclusion.
  ⚠ Sign check: this RAISES Georgia tax on affected returns — correctly, since
  the same dollars were being subtracted twice.
- **GA RIE line 9**: an MFJ return with owner-tagged capital rows in an all-loss
  year reallocates. Joint/untagged is pinned unmoved by a movement guard.
  ⚠ **Second-order**: current-year weights and carryover weights now COMBINE
  where carryovers alone previously decided, so a return carrying both
  allocates differently.
- Carried from s227/s228: a 1065 K-1 whose §704(d) worksheet is SAVED moves;
  a 1065 row with the basis checkbox ticked swaps its warning code;
  #10 8959 single-W-2 engage (intended); #6 Sch 1 24k engine-fed blank→0.

### ⚠ Known red / rotted (not this session's changes)
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  `ValidationError`. Pre-existing (s219). Not diagnosed.
- `test_4868.py` — 4 tests on the Schedule 3 line-10 feeder (s217). ⛔ KEN.
- `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  batch-005 #9 PTET-gate class, red since s212.
- **DiagnosticRule unique-code contamination** (s225): `test_backentry_cleanup.py`
  (red alone under `--reuse-db`), `test_ga500_auto_attach_s106.py`,
  `test_ga500_rie_federal_pull.py` was NOT affected this session (100 green).
- **Client typecheck**: 55 error lines standalone (untouched by s233 — no client code).
- ⚠ The Slate 8889 fixture cast `as HSAAccountRow` still swallows new
  required fields.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). `--reuse-db`.
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout
  loses ALL output — redirect to a file and tail it.
- ⚠ A `poetry run python script > file` redirect BUFFERS. Use `-u`.
- ⚠ A stdout **redirect goes through cp1252** on this box and dies on ligatures
  and the U+2212 minus. Write UTF-8 from inside Python.
- ⚠ `manage.py shell -c "..."` prints nothing — use a script file. **New (s233):
  a multi-line `python -c` through the Bash tool also silently produced no
  output; a script file worked.** Write the script.
- ⚠ **New (s233)**: a `bash` heredoc carrying prose with apostrophes/backticks
  failed to parse even quoted. Write the file with the Write tool and append it
  from Python instead.
- ⚠ The Bash tool's cwd PERSISTS across calls — use absolute `cd` each call.

### ✅ KEN DECISIONS OUTSTANDING
- **⛔ KEN (s231, carried): §38(c)(6)(A), the MFS threshold.**
  `compute_3800.SEC38C1_THRESHOLD` is a flat $25,000; the statute makes it
  **$12,500** for an MFS taxpayer whose spouse has any business credit.
  **⚠ The sign: this OVER-allows.** Requires nothing from Ken to BUILD.
- **⛔ KEN (s227)**: the out-of-scope-state packet-disposition marker.
- **⛔ KEN (s230)**: Form 6765 Section G becomes REQUIRED for tax years
  beginning after 2025; the RS spec must be re-authored before a TY2026 season.
- **NEW (s233), low-friction**: the CC-Changes **batch numbering collided** —
  a second, unrelated `BATCH-001` was posted while the s227 `BATCH-001` sits in
  Done. Nothing was lost. Worth telling Codex to skip to `BATCH-003`.
- The one live external item: **1040 v5.4 business rules still not in hand,
  active 2026-08-09** (v5.4 schemas ARE on disk; 1041 v5.5 closed).

### 🔎 Carried for triage (s229) — not chased
A filed, exact-tie 1040 shows **worksheet drift on a bare recompute**:
`1040_SCHD_WS` `clc_1` 139,889 → 134,398 and `clc_3` 140,738 → 135,247 (−5,491
each), face still an exact TIE. Worth a sweep: how many filed returns move a
worksheet line on recompute?

### RS AGENDA
- **NEW (s233)**: `compute_ga500.GA_RIE_EARNED_CAP` is **$5,000**, but
  Ga. Comp. R. & Regs. r. 560-7-4-.02 says the earned-income portion is limited
  to "**no more than $4,000.00**". The statute controls and has been amended
  since the reg was written, and the code cites the statute — so this is very
  likely a stale regulation, **not** a live defect. Nothing was changed. Worth
  one re-verification against current §48-7-27(a)(5) to settle it on the record.
- **NEW (s233)**: the GA-500 spec (`lookup/500/`) has no rule covering either
  the Georgia-taxable-income limit on the RIE base or owner attribution of
  capital losses — both were resolved this session from the regulation. Both
  belong in `R-GA500-RIE` so the next build does not re-derive them.
- Carried (s232): the `[WO-SOURCETYPE-RECON]` additions (Rev. Proc. 2024-40
  under three `source_code`s; `TaxForm.status` carrying FlowAssertion
  vocabulary); the export serializer omitting `requires_human_review`.
- Carried: the s231 Form 3800 five spec defects; the s230 Form 6765 items
  (a)-(e); the **1065 Schedule K-1 box-15 letters** for §41 and §45R (still
  URGENT — `D_3800_008` excludes those credits until both are verified);
  s228 `D_K1B_FULLY_ALLOWED`; s226/s227/s224/s223 unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
