# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-02, session 185 (**Sch A schema era, leg 1 SHIPPED
AND DEPLOYED: Schedule A joins the import lane** — `521e325`; Render API
shows prod `delvio-tax` AND `tts-tax-demo` live on it, no queue jam; demo
verified BY BEHAVIOR — a staging probe carrying `state_income_tax_payments`
+ a `scha_*` fact errored ONLY on its fake locator, which is the new-code
signature.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax`; demo (demo.delviotax.com) = `tts-tax-demo`. **The orphan
third service `tts-tax-app` (no custom domain) still builds and FAILS
every push — Ken should delete it in the Render dashboard.** If a deploy
seems missing, check for a JAMMED queue (s184: a 2+-hour hung build;
the Render API key in `Passwords & Secrets` cancels it). Open question
for Ken (carried): set `autoDeploy: false`?

---

## ▶ RESUME HERE

### Worker split (Ken, s184 — unchanged)
ChatGPT-browser = the 175 HARD packets · Codex = the import lane
(`D:\tax-test-data\CODEX_KICKOFF_PROMPT.md`, now with an s185 Sch A
addendum) · **CC sessions = engine/tax-law work only.**

### s185 shipped: Schedule A in the import lane (`521e325`)
- `backentry.py`: TAXPAYER_FIELDS += 20 `scha_*` preparer facts (the
  derived §165(d) gambling cap + override flag deliberately excluded);
  NEW `state_income_tax_payments` section (→ Sch A line 5a, §164
  cash-basis dates); FEDERAL_SUMMARY_LINES += line 12.
- Engine untouched — `compute_schedule_a` already did the work; any
  nonzero fact auto-engages, line 12 = max(standard, itemized).
- Tests revert-proven; backentry 47 green; flow assertions 521 green.
- `batch-import.schema.json` regenerated; AUTHORING_GUIDE gained the
  Schedule A section (5a decision ladder!); handoff + Codex kickoff
  updated. Sch-A-only HARD packets (~16) become lane-eligible — Codex
  takes them only after its original ~24 queue and on Ken's go.

### Next engine work per BUILD_ORDER
**Sch C / Sch E schema era** (the rest of the era — both need real model
scoping first: they are row-family models, not taxpayer scalars, and
Sch C/E returns also pull SE tax / 8582 passive paths — verify engine
support before growing the schema). Behind: B002 row-creation family ·
Ken's ② MeF (`build_irs5695`) · ④ year-constant ruling.

## Lane scoreboard: 60 FILED — 0 unfiled, 0 held (unchanged)
The answer-keyed pool is exhausted; Codex has ~24 + (pending Ken) the
Sch-A unlocks; ChatGPT has the HARD pile.

---

## Known traps (carried — do not re-learn)
- **s185: Sch A 5a is DERIVED** (documents' state W/H + dated
  `state_income_tax_payments`); a nonzero `scha_salt_income_or_sales`
  direct entry WINS over the auto-total. The gambling cap is derived
  from W-2Gs — staging rejects it by design.
- **s184: blank prior-year 2210 COMPUTES the 90% fallback** — omitting
  genuinely-present prior-year facts no-ties with a phantom penalty.
  Printed 2210 → line 8; else Three-Year Summary; TaxWise-blank → 0/omit.
- **The wrong-source error travels into payloads and outlives rulings**
  (PEGGY). A correction payload must send explicit 0 — omit preserves.
- **`replace_documents` does NOT clear field-level overrides.**
- **1040 line 37 as printed INCLUDES the line-38 penalty.**
- **The dry-run IS the commit, rolled back.**
- GA `LIC-NODEP` gates the whole LIC; set when GA prints 17a/17c.
- Flat `est_payment_q1..q4` drive 1040 line 26; dated
  `federal_estimated_payments` drive only the §6654 accrual. Keep both.

---

## Standing gates
- `git pull origin main` at session start; push with `git push origin HEAD:main`.
- Never `git stash`, never `checkout` mid-session.
- Rule Studio spec required before touching `compute*.py` / `renderer.py`
  (Schedule A HAS one: `server/specs/schedule_a_spec.json`; 2210 has NO
  RS spec — authority = `specs/_2210_source_brief.md` + form face).
- `pytest tests/test_flow_assertions.py` after any compute change (s185: 521 green).
- Dev environment shares the **production** Supabase DB — every write is a
  production write.
