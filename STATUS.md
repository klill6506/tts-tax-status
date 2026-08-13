# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-13 (s260). **✅ SWEEP LEG 2 LIVE (`e1d7da8`) —
five income screens guarded, and the SERVER half of the contract the
client was missing**: INT/DIV/1099-G/MISC/PATR field PATCHes ride
per-row verdict-returning lanes (the s259 threading lights the cell
overlays), every create guarded via useRecordSaves with
addPending/addError threaded. **Found LIVE: the six income-document
create endpoints IGNORED X-Idempotency-Key** — the guarded add's retry
after the (reproduced) 30s timeout DUPLICATED the row (two rows, one
intent, Django 201-after-timeout). `@idempotent_create` added to all
six (including capital-transactions — s259's own latent gap); a
parametrized test pins each. 8 screen tests + 6 idempotency pins;
1706 client + 526 FAs + autosave-stabilization green; no migration.
Earlier today: s259 (sweep leg 1 + the PayerTable find), s258 (OOS
hold), s257 (MFS threshold), s256 (the NOL unit + hotfix),
s255/s254/s253b (the NOL build day) — all LIVE.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax`; demo = `tts-tax-demo`. *(The orphan third service was
DELETED 2026-08-13 on Ken's live approval — s253b.)*

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

## ⚠ KEN IS AWAY 2026-08-09 → ~2026-08-19 (10 days)
**Availability MINIMAL BUT NOT ZERO.** Batch questions; keep them low-friction.
Nothing is on a clock in that window; the next hard deadline is 2026-09-15.

---

## ▶ RESUME HERE

### ✅ s260 — sweep leg 2: five income screens + the server idempotency gap
Design record: the s260 commit + `test_sweep_idempotent_creates_s260.py`.
Load-bearing:
- **⚠⚠ A CLIENT-SIDE IDEMPOTENCY KEY IS HALF A CONTRACT** — useRecordSaves
  re-sends the ORIGINAL intent key on retry, but an endpoint without
  `@idempotent_create` ignores it and DUPLICATES. Reproduced live (the
  add timed out at 30s, Django answered 201 late, the retry made row 2).
  When guarding a create client-side, CHECK THE ENDPOINT HAS THE
  DECORATOR — the parametrized test is the template for future slices.
- The 1099-G block has TWO adds: the legacy button's `addDoc` + the
  guarded `addLane` — name collisions in big FormEditor components are
  real; grep the component before inserting.
- ⚠ tsc --noEmit MISSED the duplicate-const (esbuild/vitest caught it) —
  run vitest collection as the syntax gate, never tsc alone.

### ⭐ NEXT UNIT — sweep leg 3: the singleton-PATCH sections
form-1116 `update`, schedule-j, 8615-style — `await patch(...)` with no
ok check; enlane + verdict. Then the remaining `void update...`
boundaries (W-2G, depreciation, rentals — grep `onUpdate={` in
FormEditor). Client-mostly; check each endpoint's @idempotent_create
when guarding creates.

### ✅ s259 — the raw-mutation sweep, leg 1 (Schedule D + PayerTable)
Design record: the s259 commit message + the 2 new
slateScheduleDScreen tests. Load-bearing:
- **⚠⚠ PayerTable's commitSlim DROPPED every verdict** — the s253
  per-cell overlays were unreachable on EVERY PayerTable screen no
  matter how the caller was wired. Now returns onUpdate's result; a
  verdict-returning handler lights the cell, a void caller changes
  nothing. Future sweep slices get the cell overlays FREE by just
  dropping the `void` at their FormEditor boundary.
- The SchD draft flow untouched (its card IS the pending state).
- Live verification reproduced the s251 class faithfully: a ~30s local
  PATCH (pooler amplification) times out client-side EXACTLY as Django
  completes — "may not have reached the server" is the honest message;
  the idempotent retry replayed without duplicating (row count 1).
- The synthetic verification fixture: client #99259 on dev's demo
  firm (a synthetic identity) — reusable for future sweep-slice
  live checks.

### ✅ s258 — the out-of-scope-state named hold (Ken ruling #2)
Design record: `server/tests/test_backentry_oos_states_s258.py` (8) +
the NZ-file annex. Load-bearing:
- **A payload key, not a model field** — the payload lives whole on
  StagedReturn, so the marker needed NO migration. Read server-side by
  `payload_out_of_scope_states` (codes only — PII-safe by construction,
  the source_provenance pattern).
- **NON-blocking is the tested contract**: the cleanup row stays
  eligible with the marker; the closeout entry carries
  `state_disposition: "federal complete; CA → the paper preparer"`.
- GA refuses inside the marker (it is IN scope); unknown codes refuse
  (typo fence); the published batch-import.schema.json documents it.

### ✅ s257 — the §38(c)(6)(A) MFS $12,500 threshold (Ken ruling #1)
Design record: `server/tests/test_form3800_mfs_threshold_s257.py` (13) +
the RS amendment (rule-studio `ee4dece`). Load-bearing:
- **The statute's second sentence is the build's substance**: MFS +
  spouse-has-NO-credit keeps $25,000. The request said "halve it";
  the verbatim text said "unless". Verify-first caught the difference.
- **The spouse fact is preparer-asserted, nullable** — never derivable
  from this return. Unanswered → $12,500 (conservative) + D_3800_010.
- **One reader** (`form_3800_mfs_threshold`) feeds compute AND the
  diagnostic — they cannot diverge.
- The spec amendment rode the same session (never let app and spec
  drift): R-3800-MFS-THRESHOLD + the key excerpt + D_3800_010 in the
  spec's diagnostics; cache refreshed + content-verified.

### ✅ s256 — Form 172 leg 3: the NOL unit COMPLETE (+ the commit hotfix)
Design record: the s256 annexes in BATCH-001/BATCH-002 +
`server/tests/test_form_172_statement.py` (11). Load-bearing:
- **⚠⚠ THE HOTFIX**: a OneToOne backentry section needs ALL THREE
  registries — SECTION_RELATED + LIST_SECTIONS + **SINGLETON_SECTIONS**.
  Missing the third made `_section_qs` hit the raising reverse accessor
  on EVERY commit (s255→s256 window). The invariant test in
  `test_form_172_generation.py` now derives the requirement from the
  descriptor type — the next singleton lane can't repeat this.
- **One data source for the attach duty**: `form_172_statement_data`
  reads persisted rows only; print page + MeF text both consume it.
- **MeF sign flip**: Schedule 1 8a transmits POSITIVE
  (USAmountNNType; S1-F1040-080 subtracts) with referenceDocumentId →
  NOLCarryforwardDedStatement (slot 2406). Emitted iff 8a > 0 — both
  IND rules hold by construction.
- **Keyed 8a with no Form 172 detail = named e-file refusal** (the app
  holds only a total; it will not improvise the computation statement).
  Print attaches nothing in that shape.

### ✅ s250 — BATCH-006 #10 built (the 8959 multi-W-2 aggregate)
Design record: the s250 batch annex in `CC_CODE_CHANGES_1040_BATCH-006.md`
+ `server/tests/test_batch006_item10_8959_agg.py` (11). Load-bearing:
- **`amt_8959_filed` is TRANSCRIBED EVIDENCE, not a computed arm** — the
  representation-choice class of the s227 identity valve, NOT a revival
  of the removed 'line 22 > 0' arm (spec amendment 2026-07-02 stands).
  Substance gate: engages only when line 4 or line 19 is nonzero.
- **The engagement decision now lives in ONE place** —
  `form_8959_engagement` (compute + all three D_8959_* rules). Any
  future valve goes THERE or the diagnostics go blind again.
- Per-row box 5 still wins over the aggregate (duplication guard);
  without the flag the multi-row aggregate shape still disengages.
- D_8959_001 fires only when line 18 > 0 (its threshold-exceeded
  message was false on no-tax-due engagements; D_8959_003 owns the
  25c-reconciliation story and now fires on aggregate-sourced returns).
- Codex re-stages the two-W-2 production batch adding
  `"amt_8959_filed": true` (annex guidance).

### ✅ s253b — THE NOL SPEC ROUND-TRIP (Ken live: rulings + Gate 1 approved)
Ken answered the s246 brief's four questions (~23:15) and approved Gate 1
in-session. FORM_172 authored / seeded / exported / **cached to
`server/specs/form_172_spec.json` (contents verified)**. Rulings in
DECISIONS.md: BOTH SIDES v1; farming carryback refuses by name; ATNOLD
preserve-only; the 80% base verbatim — ⚠⚠ including the clause the brief's
short form dropped: the cap is 80% of (TI-without-NOL/QBI/§250 MINUS
pre-2018 NOLs); spec scenario T7 pins 16,000-not-40,000. The absorption
synthesis is requires_human_review, Ken-approved. **The 404-STOP lifted
and the full app build shipped the same day (s254/s255/s256):
BATCH-001 #4 + BATCH-002 #10 closed; D_CFWD_001 retired for
`nol_regular`.**

### ⭐ LATER UNIT — behind the two small ruled units: **the raw-mutation sweep, leg 1**
BATCH-006's named screens are done; ~95 raw `await post/patch` call
sites remain outside the guarded machinery. Sweep by traffic, highest
first, converting each to `useRecordSaves`/lane + verdict-returning
commits (per-cell states come free via FieldStateInput now):
- The capital-transactions `patchRow` (FormEditor ~17985 — the same
  naked shape the 1099-R one had) + its `createDraft`.
- The PayerTable-family screens' `onUpdate` chains (interest,
  dividends, 1099-G/MISC/PATR…) — many ride `useNestedRowSaves.upd`
  already (verdicts free); convert the ones that don't.
- The singleton-PATCH sections (form-1116 `update`, schedule-j,
  8615-style) — `const res = await patch(...); onRefresh(freshOf(res))`
  with no ok check.
Grep the remaining sites: `grep -rn "await post(\|await patch(" pages/
slate/ components/ | grep -v saveScope|recordSaves|test`. Convert in
slices; each tick's slice ships with tests. ⚠ Don't break the 1099-R/
Schedule-D client-draft flows (their card IS the pending state).
⚠ Client-only; vitest + `tsc --noEmit` gates. After the sweep: the NOL
computes stay parked on Ken's brief answers; the RS agenda carries the
rest; Codex may post a new batch any boot.

### ✅ s249 — BATCH-006 #1 built (alimony received, the s241j twin)
Two lines, not a document family: `sch1_fields["2a"]/["2b"]` import;
D_SCH1_007 enforces BOTH Topic 452 directions; D_SCH1_008 prompts the
modification question both sides; D_SCH1_009 flags a generic duplicate.
Regression: `test_alimony_received_s249.py` (6). No migration.

### ✅ s248 — BATCH-006 #9 built (the Form 1099-C unit)
Design record: the s248 batch annex + `_1099c_source_brief.md` +
`test_1099c_unit.py` (15). Load-bearing: Schedule 1 line 8c now DERIVES
(`compute_1099c_db`, the 8v/8h shape; `sch1_fields["8c"]`
un-importable); business routes feed nothing (Pub 4681, the PATR
doctrine); §108 exclusions transcribe the filed 982 figure (D_1099C_001
demands the manual 982); box-3 interest carves only on asserted
deductibility. Migs 0316/0317.

### ✅ s247 — BATCH-006 triage: the trio closed at HEAD (#6/#7/#8)
- **#6 REFUTED, inference corrected**: 6b = 5,602 (cap) stable ×2,
  D_RET_012 quiet, AGI = the dry-run's own 142,229 WITH 6b correct —
  the $2,229 is the adjudicated source-side Schedule D non-foot.
  Regression: `test_batch006_item6_ss_stability.py`.
- **#7/#8 FIXED by s243b** (deployed before the batch published);
  residues named in the annex.

### ✅ s246b — BATCH-001 #5 built (REP nonpassive routing; Ken's live go)
Design record: the s246b batch annex + `test_8582_rep_nonpassive.py`.
ONE predicate (`rental_rep_nonpassive`) shared by compute/diagnostics;
the bypass = the §469(g)-release mechanics; the REP loss is a MAGI
ADD-BACK; §469(f) former-passive rows STAY passive (D_8582_FPA).

### ✅ s243b (earlier today) — Ken's four-return unblock (`bb282b0`, LIVE)
Verdicts: Return W / T / G tie; **Return P NO TIE by design** (the FILED
Schedule D face doesn't foot by exactly $2,229; source-side gap).
Regression home: `server/tests/test_four_return_unblock_s243b.py`.

### ⚠ s241's Form 5329 cross-check — still waiting on Sections A/B
Form 5329 line 36 takes "Form 8853 line 8" (Archer MSA — Section A/B
territory). Parked with Ken's s224 keyed-only ruling.

### ✅ THE E-FILE GAP LIST IS EMPTY (as of s242z)
What remains refused at composition is NAMED per-case, never a missing
builder.

### The rest of the queue
- **1040** (`1040\CC Changes\`): **BATCH-006 — ✅✅ COMPLETE (all ten),
  moved to Done (s253)**. **BATCH-001 — every buildable item CLOSED**;
  #4 ✅ CLOSED (the NOL unit shipped s253b→s256; final annex posted).
  **BATCH-002 — open as to item 9's RS-gated charitable compute ONLY
  (item 10 closed s256)**;
  **BATCH-003/004/005 — ✅ DONE, moved.** Every worked file carries a
  result annex; read it first.
- **1120-S** (`1120S\CC Changes\`): EMPTY (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of
  10; #10 multi-state parked under the states-on-hold ruling). Unchanged.

---

## ⚠ Classes that MOVE existing returns or output on next recompute
- **⚠ s257 MOVES MFS RETURNS with a GBC and line 12 > $12,500** (a
  correction): line 13 rises to the §38(c)(6)(A) threshold → less
  credit allowed unless the preparer answers spouse-has-no-credit
  (which restores $25,000). D_3800_010 (error) fires on the unanswered
  shape. No existing MFS-with-GBC returns are known in the test data.
- **⚠ s256 MOVES PRINT + E-FILE OUTPUT on NOL returns (corrections)**:
  the 1040 PDF gains the line-8a statement page; MeF flips 8a to its
  positive magnitude + emits the statement document; a keyed-8a-no-
  detail return now REFUSES e-file by name (it previously composed an
  invalid negative with a missing document — an IRS reject either way).
  D_172_80PCT_STATEMENT downgrades warning→info. **The HOTFIX restores
  backentry commits** (500 since `ca6202c`).
- **s255: NONE beyond new-fact reach** (generation needs a Form172 row).
- **s250: NONE beyond new-fact reach on ENGAGEMENT** (`amt_8959_filed`
  is False everywhere until keyed). **⚠ but the DIAGNOSTICS bridge-gate
  repair moves rule output on already-engaged aggregate returns**: an
  aggregate-sourced return that compute engaged (the s227 single-W-2
  shape) now fires D_8959_003 where all D_8959_* rules were silent; a
  no-tax-due engaged return LOSES a false D_8959_001 (correction both
  ways). No dollar-line movement.
- **s249/s241j MOVE DIAGNOSTICS**: post-2018 alimony instruments fire
  `D_SCH1_007` (error) on BOTH the paid and received sides.
- **s248: NONE beyond new-row reach** (8c derives only when a
  `Form1099C` row exists; none do).
- **s246b: NONE beyond new-fact reach** (REP routing needs
  `material_participation` True — NULL on every existing row).
- **⚠⚠ s243b MOVES THREE CLASSES (each a correction):** (1) basis-only
  8606 + IRA-path 1099-Rs — 4b regains box-2a taxable; (2) employer DCB
  below the plan cap — 2441 line-17 cap raises 1e/1z/AGI; (3) GA
  under-62 disability RIE prints on S1 7c/7f with date/type.
- **⚠ s243 MOVES GEORGIA RETURNS carrying a federal 2441 credit**
  (IND-CR 202 feed — correction).
- **⚠ s244 MOVES E-FILE OUTPUT + PRINT on the 8862 class** (all
  corrections toward i8862 — category flags gate boxes/parts; no
  resolving category refuses by name; CTC/AOTC-only recerts now attach).
- **⚠ s242z/y/x MOVE E-FILE OUTPUT** on the amended / full-1116 /
  keyed-8e classes (compose-or-named-refusal; no compute movement).
- **⚠ s241o MOVES GEORGIA RETURNS carrying a 1099-PATR** (RIE L10 feed).
- Carried from s240: passive/PTP K-1 §1231 losses fire RED; a non-zero
  Schedule 1 line 4 refuses at MeF composition.
- Carried from s239: Roth 1099-Rs move 5a/5b → 4a/4b; GA partnership K-1s
  move RIE L2↔L13; code-U un-blanks the pension taxable column.
- Carried from s236/s235: GA RIE line 13 on suspended passive K-1 losses;
  GA dependent exemptions on an untouched 7a.

### ⚠ Known red / rotted
- **`--reuse-db` cross-module contamination**: `test_backentry_cleanup.py`
  (3, s225) and `test_mappings.py::TestApplyMappingAmbiguousFederalReturn`
  (3, s239).
- **`test_1040.py` — 6 pipeline tests**, unscoped `_fv` `.get()` (s234).
- `test_apr01_fixes.py` (8) + `test_mar30_session4.py` (1) — MagicMock UUID
  (s219). `test_4868.py` (4) — ⛔ KEN (s217).
  `test_supporting_forms_spec.py::TestGA600SSingleState::test_s1_taxable` —
  PTET-gate class (s212).
- **⚠ PRE-EXISTING 1120-S defect (s241o)**:
  `test_line_key_registry_sweep.py::test_formula_targets_resolve` —
  `FORMULA_REGISTRY["1120-S"]` targets `M2_DIST_EXCESS` / `L24_BOOK_BRIDGE`,
  neither seeded. 1120-S only. Deserves its own unit.
- **Client typecheck**: project tsconfig CLEAN (s244 ran `tsc --noEmit`
  exit 0); vitest 1,680 passed / 140 files.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB,
  cross-repo (`test_postgres`). A stalled background sweep beside foreground
  runs is contention, not a hang (s241o).
- A broad `-k` sweep blows the 600s timeout — background it; keep `-k` tight.
- `--create-db` does not reliably drop here; prove a pre-existing red via
  `git worktree` at a pristine SHA with the main venv + copied `server/.env`
  (worked again in s244).
- A timed-out `pytest | Select-Object` loses ALL output — redirect to a file.
- `poetry run python > file` BUFFERS (use `-u`); stdout redirects go through
  cp1252 (write UTF-8 from inside Python); **never rewrite a UTF-8 file via
  `Set-Content`/`Add-Content`** — use the Write/Edit tools or Python io.
- **`poetry run` only works from `server\`**; Windows `python` cannot read the
  Bash tool's `/tmp` — use the scratchpad; DB probes: a throwaway
  `tests/test_zz_*.py` with `-s`, deleted after.
- `manage.py seed_rules` against the pooler takes >5 min — background it.
- Cloudflare-403 law sites and `rules.sos.ga.gov`: the in-app browser gets the
  text where WebFetch and curl fail (s239/s241o).
- Staging answers **201 even for an invalid payload** — the verdict is
  `row["status"]`; return CRUD routes are `/api/v1/tax-returns/…` with the
  trailing slash; `filing_status` is `"mfj"`.
- `_finding(...)` kwargs land under `["details"]`; `ScheduleF` has no
  `business_name` (use `principal_activity`); `order_by("owner")` puts
  "spouse" before "taxpayer" (s241w).

### 🔎 Carried for triage — NOT claims
- **From s241o**: RIE L8 alimony underived; a fuller L10 derive possible in
  principle but per-owner attribution + the (4)(b)2 gambling carve-out make
  it a design pass.
- **From s241**: `Form8606`/`HSAAccount` allow duplicate owners and their
  computes ITERATE (double-count, not vanish); browser POST unguarded.
- **From s234, potentially large**: a materially-participating 1120-S K-1's
  $250k nonpassive ordinary income never reached Schedule 1 line 5 / AGI.
  Repro: `server/tests/test_8960_line4b_clamp.py`, `_build(k1_ordinary="250000")`.
- Carried (s229): exact-tie 1040 shows `1040_SCHD_WS` clc_1/clc_3 drift on a
  bare recompute (−5,491 each), face still a tie.

### ✅ KEN DECISIONS OUTSTANDING
- ~~⛔ KEN (s231)~~ **✅ RULED 08-13 (live): BUILD the §38(c)(6)(A) MFS
  $12,500 halving** — queued after the NOL app build.
- ~~⛔ KEN (s227)~~ **✅ RULED 08-13 (live): BUILD the out-of-scope-state
  named hold** (`out_of_scope_states` + cleanup-gate surfacing) — queued
  after the NOL app build.
- **⛔ KEN (s230)**: Form 6765 Section G required for TY2026+ — re-author
  before a TY2026 season.
- **1040 v5.4 business rules still not in hand** (v5.4 schemas ARE on disk).
  ⚠ s240/s241w read the **v5.3** rules — re-check `S1-F1040-118-01` and the
  `SH-F1040-*` family against v5.4 on arrival.

### RS AGENDA
- **(s242x) The TEN staged FA definitions**: FA-1040-8853C-01..05 + FA-4562-
  DEST/ROUND/280F + FA-1040-2210-08/09 — author in RS, re-export, move from
  `flow_assertions_1040_pending.json` to the gate mirror.
- **✅ RESOLVED s253b: THE NOL SPEC EXISTS** — FORM_172 authored, Gate-1
  approved by Ken in-session, seeded (RS DB 136 forms), exported (200),
  cached (`server/specs/form_172_spec.json`, verified). Ken's four rulings
  in DECISIONS.md (BOTH SIDES; farming-carryback refusal; ATNOLD
  preserve-only; the 80% base verbatim incl. the pre-2018 subtraction).
  The APP BUILD is the next unit; the five FA-1040-NOL definitions are
  authored in RS and export with the flow-assertion feed.
- (s241b, reaffirmed s244): the `8862` spec is a draft collapsing each PART
  to one boolean — re-author per-line from the Rev. 12-2025 face. ⚠ The
  seeded app face still carries a `part_v` pseudo-line from that draft; the
  Dec-2025 revision DROPPED Part V (qualifying-child-of-more-than-one-person
  is now caution text, no answer field). D_EIC_016 keys on the row as an
  app-internal preparer flag — harmless, but the re-authoring should retire
  or rename it.
- (s241w): `SCHEDULE_H` is a DRAFT covering 7 of ~27 lines — re-author.
- (s241s): the GA QEE credit has NO SPEC (two carryforward regimes).
- (s241p): `4547` and `8879_TA` have NO SPEC; record `IND-476`.
- (s241o): the `500` spec has NO rule for what feeds RIE lines 1/2/6-13.
- Carried: `5329` roll-forward silence (s241); `R-8582-MULTIFORM` stale cite
  + `4797` K-1 §1231 silence (s240); `R-RET-CODE` outrun ×3 (s239); `8379`
  draft (s238); `R-SCHA-CHARITABLE` buckets + RIE-13 (s236); SCHEDULE_A
  carryover aggregation + `500` line 7a typed `input` (s235); s232/s231/s230
  items; the 1065 K-1 box-15 letters (still URGENT).

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
