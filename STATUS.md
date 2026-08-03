# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-02, session 187 (TWO units: **① the Sch C / Sch E
era SHIPPED** — Schedule C, Schedule E both pages, and recipient K-1s join
the import lane (`856dfe8`, deployed); **② the GA-500 RS spec
RECONCILIATION SHIPPED** — Ken Gate-1 approved; the spec caught up to the
season's three Ken-ruled engine corrections, re-seeded + re-exported, GA
band 596 green.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax`; demo = `tts-tax-demo`. **The orphan third service
`tts-tax-app` still fails every push — Ken should delete it.** Carried
open question: set `autoDeploy: false`?

---

## ▶ RESUME HERE

### Worker split (Ken, s184 — unchanged)
ChatGPT-browser = the HARD pile · Codex = the import lane (kickoff prompt
now carries an s187 addendum: Sch C/E supported, smoke-test-first at the
boundary, **only on Ken's go**) · **CC sessions = engine/tax-law work
only.**

### s187 shipped + deployed (`856dfe8`, both services; NO migration/seed/client code)
- **backentry.v1 grew three row-family sections**: `schedule_cs` (with
  nested Part V `other_expenses` rows), `rental_properties` (Sch E p1),
  `schedule_k1s` (Sch E p2 / the K-1 router). ZERO compute change — the
  engine already ran Sch C → SE → Sch 1/2 feeders, §469/8582, and the
  K-1 router end-to-end; only the lane couldn't carry the rows (the s185
  Sch-A shape, despite the carried "NOT Sch-A-shaped" warning).
- **Depreciation (Sch C 13 / Sch E 18) imports as the FILED total**:
  `aggregate_depreciation` early-returns with no asset register, so the
  direct entry holds (test-pinned). Asset detail is NOT carried — future
  years ride the conversion importer.
- Allowlists = model INPUT columns only; K-1 RED-defer presence boxes
  importable (diagnostics speak) but a nonzero amount = HOLD.
- Tests revert-proven ×4; backentry band 54 green; FA 521 green; commit
  fixture now seeds SCH_2/SCH_C/SCHEDULE_E/8582/SE/8995.
- Docs: AUTHORING_GUIDE (3 new sections + hard-rules rewrite),
  CC_IMPORT_LANE_HANDOFF (§0/§4/§8), CODEX_KICKOFF_PROMPT s187 addendum,
  batch-import.schema.json regenerated.
- **Deploy verified live**: a staging probe on the deployed demo service
  accepted all three sections (locator-only error on a bogus client
  number, no "unknown section"); prod builds from the same push/commit.

### s187b shipped same day (RS `432a5b9`/`90fb04e` + tts `500_spec.json`)
- **The GA-500 RS spec EXISTS and always did** (authored/seeded 6-25;
  lookup code is `500` — "GA-500" 404s; the "no spec" memory hook was
  wrong and is corrected). This unit RECONCILED it to the engine:
  ① R-GA500-MIL per the unexecuted 7/05 over-exclusion handoff (L8
  entered alone; the mis-transcribed authority excerpt fixed at the
  source) · ② 7c = 7a + 7b derived, unborn count in L14 · ③ LIC =
  IT-511 p35 children-only count via new fact `g_lic_children`, and
  `g_lic_not_dependent` default → false (explicit assertion — Ken
  approved the flip at Gate 1).
- Integrity gate 20 scenarios ALL PASS (new T19 military-midrange + T20
  LIC-count shapes); re-seeded, re-exported, **id-level diff = only the
  intended set**; tts GA-500 band + FA = 596 green. REVIEW_QUEUE s176
  RS-sync item closed; the 7/05 handoff doc marked DONE. No engine
  change — the spec caught up to the code.

### Next engine work per BUILD_ORDER
The **"August GA build unit"** is now the scoped follow-on: GA UET
worksheet computation (line 42, direct-entry today) + the S4-8/S4-NB-18
seeded-computed-but-never-written NOL lines. Behind it: **B002
row-creation family** · Ken's ② MeF (`build_irs5695`) · ④ year-constant
ruling · Sch F / 8829 / 6198 (remaining HARD-pile import gaps, only on
Ken's go). SB 31 TY2026 military full exemption = standing RS W-item.

## Lane scoreboard: 64 FILED — 0 unfiled, 0 held
Codex continues b009+ (Sch C/E packets gated on Ken's go + per-shape
smoke tests); ChatGPT has the HARD pile.

---

## Known traps (carried — do not re-learn)
- **s187: the era-scoping prediction is itself a hypothesis** — the
  carried "NOT a Sch-A-shaped leg" warning was wrong; engine-scope-first
  (read compute + the model) decides, in either direction.
- **s187: never import computed row columns** — passive_8582_allowed/
  _suspended are Form 8582 OUTPUTS (the input is
  `prior_year_unallowed_passive`); net_profit/qbi_amount/etc. likewise.
  Staging rejects them.
- **s186: a "reproduced engine defect" can be shell residue** — probe the
  shell's scalars + overrides BEFORE believing a no-tie names the engine.
  The commit warns on the W-2G/other_gambling shape specifically.
- **s186: sch1_fields is for spec-typed INPUT lines only** (11/21).
- **s185: Sch A 5a is DERIVED** (documents' W/H + dated payments);
  nonzero `scha_salt_income_or_sales` direct entry wins.
- **s184: blank prior-year 2210 COMPUTES the 90% fallback.** Printed
  2210 → line 8; else Three-Year Summary; TaxWise-blank → 0/omit.
- **A correction payload must send explicit 0** — omit preserves.
- **`replace_documents` does NOT clear field-level overrides or taxpayer
  scalars** — and it now covers the s187 families too (children cascade).
- **1040 line 37 as printed INCLUDES the line-38 penalty.**
- **The dry-run IS the commit, rolled back.**
- GA `LIC-NODEP` gates the whole LIC; set when GA prints 17a/17c.
- Flat `est_payment_q1..q4` drive 1040 line 26; dated
  `federal_estimated_payments` drive only the §6654 accrual.

---

## Standing gates
- `git pull origin main` at session start; push with `git push origin HEAD:main`.
- Never `git stash`, never `checkout` mid-session.
- Rule Studio spec required before touching `compute*.py` / `renderer.py`
  (s187 touched neither — backentry implements the models, which
  implement the SCHEDULE_C / SCHEDULE_E+FORM_8582 / SCHEDULE_K1 specs).
- `pytest tests/test_flow_assertions.py` after any compute change (s187: 521 green).
- Dev environment shares the **production** Supabase DB — every write is a
  production write.
