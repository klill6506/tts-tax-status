# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-02, session 186 (**b008's four holds RESOLVED and
FILED — lane at 64.** `sch1_fields` shipped for educator/student-loan
adjustments; the CRAFT "W-2G double count" was REFUTED — shell residue,
not an engine bug. Correction batch `b008c2`: 4/4 dry-run TIE → committed
→ filed; PDFs moved to Done.)*

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
now carries s185 Sch A + s186 sch1_fields/CRAFT addenda) · **CC sessions
= engine/tax-law work only** (s186's correction batch was Ken-directed).

### s186 shipped + deployed (`4cdecba`, verified live by the acceptance run itself)
- **`sch1_fields`** — Schedule 1 DIRECT-ENTRY adjustments: `"11"`
  educator, `"21"` student-loan interest (FILED amounts). Spec-conformant:
  sch_1_spec.json types both `line_type=input`, no source facts, so the
  import mirrors the UI (ga500_fields precedent) — NOT Taxpayer facts +
  a feeder. Applied before compute; R-S1-04 → line 26 → 1040 line 10.
  Computed feeder lines refuse at staging.
- **CRAFT refuted**: his shell carried `other_gambling_winnings=1220`
  (browser-lane residue duplicating his one W-2G). Engine 8b math is
  correct single-count. NEW commit warning fires on the residue shape
  (payload W-2Gs + preserved nonzero shell value). Fix = explicit 0 in a
  correction batch.
- Tests revert-proven; backentry 50 green; FA 521 green; fixture now
  seeds SCH_1.
- **Acceptance: batch `b008c2` on prod — 4/4 TIE (every expected federal
  + GA line), committed, mark-filed swept, report at
  `D:\tax-test-data\tmp\b008\b008c2_report.json`, 4 PDFs → Done.**

### Next engine work per BUILD_ORDER
**Sch C / Sch E schema era** — scope engine support FIRST (row-family
models + SE tax / 8582 passive paths; NOT a Sch-A-shaped leg). Behind:
B002 row-creation family · Ken's ② MeF (`build_irs5695`) · ④
year-constant ruling.

## Lane scoreboard: 64 FILED — 0 unfiled, 0 held
60 (s180–s184) + 4 (s186 b008c2). Codex continues b009+; ChatGPT has the
HARD pile.

---

## Known traps (carried — do not re-learn)
- **s186: a "reproduced engine defect" can be shell residue** — CRAFT is
  the 4th wrong-source/leftover-shell class member (LINN, PEGGY, CRIM,
  CHANELL). Probe the shell's scalars + overrides BEFORE believing a
  no-tie names the engine. The commit now warns on the W-2G/other_gambling
  shape specifically.
- **s186: sch1_fields is for spec-typed INPUT lines only** — direct-
  setting a computed feeder (15/16/etc.) would fight compute every pass;
  staging rejects them.
- **s185: Sch A 5a is DERIVED** (documents' W/H + dated payments);
  nonzero `scha_salt_income_or_sales` direct entry wins. Gambling cap
  derived from W-2Gs — staging rejects it.
- **s184: blank prior-year 2210 COMPUTES the 90% fallback.** Printed
  2210 → line 8; else Three-Year Summary; TaxWise-blank → 0/omit.
- **A correction payload must send explicit 0** — omit preserves.
- **`replace_documents` does NOT clear field-level overrides or taxpayer
  scalars.**
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
  (s186 touched neither — sch1_fields implements the sch_1_spec line_map
  as written).
- `pytest tests/test_flow_assertions.py` after any compute change (s186: 521 green).
- Dev environment shares the **production** Supabase DB — every write is a
  production write.
