# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-02, session 189a (**the b010/b011 findings triaged
and resolved** — one real engine defect fixed (GA RIE rollover netting),
three misdiagnoses refuted against the printed packets, three HOLDs
resolved with verified-tie corrections, new GA worksheet answer-key
lines + a staging guardrail shipped. All bands green; pushed → deploys.)*

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
now carries an **s189a addendum**: five new transcription rules, three
resolved-HOLD corrections, the new GA worksheet answer-key lines) · **CC
sessions = engine/tax-law work only.**

### s189a shipped (the b010 + partial b011 findings)
- **① The ONE real engine defect — GA RIE rollover netting (FIXED)**:
  the GA-500 retirement-exclusion pull read raw 1099-R box 2a; worksheet
  L11/L12 are the FEDERAL per-document taxable, so it now uses
  `doc_taxable` (2a − rollover − QCD — the same value that feeds 1040
  4b/5b). Found by a b010 MFJ return whose spouse IRA excluded the gross
  9,547 instead of the filed 4,547. Tests: rollover + QCD fixtures + a
  dual-earner worksheet fixture pinned to a printed packet.
- **② Three QA misdiagnoses REFUTED against the printed packets** (each
  delta factored exactly): "explicit 0 didn't clear" was really the
  **itemize-below-standard election** missing from the payload (34,700
  std w/ two age-65 boxes vs 34,272 elected itemized; 428 = the gap, 94
  fed = 22% × it, 533 GA = 5.19% × the GA-deduction gap) · the "GA
  earned-income exclusion gap" — the engine ties the printed worksheet
  to the dollar · b011's repeat claim was an **owner-tag** issue (the
  W-2 detail report's TP|SP designator column, position-verified).
- **③ Explicit-0 semantics pinned**: regression test proves a payload 0
  overwrites a nonzero shell scalar through dry-run AND commit (the
  mechanism was never broken — the "shell value" never existed).
- **④ New staging guardrail**: Schedule A facts + expected federal 12
  BELOW the payload-computable standard deduction without
  `scha_elect_itemize` → warning naming the fix (pure, warnings-only).
- **⑤ GA worksheet answer-key lines**: `S1-7`, `RIE-TP-17` (Sch 1 7a),
  `RIE-SP-17` (7d) joined GA500_SUMMARY_LINES — echo + reconcilable, so
  a GA no-tie names WHICH spouse's column missed. Schema regenerated.
- **⑥ Three HOLDs resolved with exact corrections written into their
  HOLD files, each verified to a FULL 21-line tie by CC dry-runs on the
  shared DB** (staged verify batches `b010c5-ccverify` / `b010c5b-ccverify`,
  never committed). Codex re-stages after the deploy.
- Docs: AUTHORING_GUIDE "b010 return-face lessons" (5 rules incl. the
  TP|SP owner-authority rule and 1099-INT box-3-additive), kickoff s189a
  addendum, CC_IMPORT_LANE_HANDOFF answer-key lists (12 + the three GA
  worksheet lines).
- Tests: staging + RIE-pull bands 56 green; commit band 24 green; flow
  assertions 521 green; ga500/compute/reconcile/markfiled bands 54 green.
- REVIEW_QUEUE additions: GA 12b blank-on-filed-itemizers (verify IT-511
  2025) · should the RIE L2 pull feed Schedule C net profit (IT-511
  verify + Ken go; no live return demonstrates it).

### Next engine work — queue
**Ken's s189 lane-extension trio stands as ruled (s188 close), UNCHANGED:**
- ① Taxable state refund joins the lane (`sr_*` fields; engine exists).
- ② IRA deduction (Sch 1 line 20) joins `sch1_fields` — GATE: RS Sch 1
  spec line 20 must be input-typed.
- ③ `amt_medicare_wages_agg` editable browser-UI surface.
**NEW since the ruling (Ken to sequence):** b011 finding #1 — the
self-employed EIC/ACTC path (fed 33/34 short 2,209) · b011 finding #3 —
rental-property QBI missing from the 8995 feeder (S-corp QBI flows,
rental doesn't).
**Behind those (unchanged):** August GA unit (UET line-42 worksheet +
S4-8/S4-NB-18 NOL) · B002 row-creation family · MeF `build_irs5695` ·
year-constant ruling · Sch F / 8829 / 6198. SB 31 TY2026 military = RS W-item.

### Codex: b010/b011 resume state
The two b010 HOLDs and the b011 owner-tag HOLD have verified-tie
corrections written into their HOLD files (the rollover one needs the
deploy first). b011 findings #1 and #3 stay HOLD (engine investigations
queued above). Everything else in b011 proceeded under the s188 addendum.

## Lane scoreboard: 64 FILED + b011's in-flight — 3 resolvable HOLDs pending re-stage

---

## Known traps (carried — do not re-learn)
- **s189a: the QA delta factors into known constants — DO THE ARITHMETIC
  FIRST.** 428 = std-vs-itemized · 94 = 22% × 428 · 533/210 = 5.19% ×
  the GA base gap. Three of four b010/b011 "engine defects" dissolved
  under factoring before any code was read.
- **s189a: every packet page prints the PRIMARY SSN in its header** — it
  proves nothing about ownership. The TP|SP designator column on the
  W-2/INT/DIV detail reports and the RIE worksheet's printed columns are
  the owner authority.
- **s188: the TaxWise invoice number is NOT the Delvio client number** —
  `-Action lookup` name search, then a human confirms the display label.
- **s188: `amt_medicare_wages_agg` is a FALLBACK, not an override** —
  any W-2 row carrying box 5 wins; survives `replace_documents`.
- **s187: the era-scoping prediction is itself a hypothesis** — engine-
  scope-first decides, in either direction.
- **s187: never import computed row columns** (passive_8582_*, net_profit,
  qbi_amount…). Staging rejects them.
- **s186: a "reproduced engine defect" can be shell residue** — probe the
  shell's scalars + overrides BEFORE believing a no-tie names the engine.
- **s186: sch1_fields is for spec-typed INPUT lines only** (11/21).
- **s185: Sch A 5a is DERIVED** (documents' W/H + dated payments);
  nonzero `scha_salt_income_or_sales` direct entry wins; **explicit 0
  restores the derived path (s189a-pinned).**
- **s184: blank prior-year 2210 COMPUTES the 90% fallback.**
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
  (s189a fetched the 500 spec — `g_*_rie_taxable_ira` is the "taxable
  IRA" fact, which sanctions the doc_taxable netting).
- `pytest tests/test_flow_assertions.py` after any compute change (s189a: 521 green).
- Dev environment shares the **production** Supabase DB — every write is a
  production write.
