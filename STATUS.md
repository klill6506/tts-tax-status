# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-02, session 188 (**the b009 findings landed** — all
three ChatGPT/Codex batch-009 improvement requests verified against the
code and implemented: ① read-only shell-locator lookup, ② four new triage
blockers, ③ Form 8959 aggregate Medicare-wages fallback. Bands green,
migration 0231 applied, pushed → deploys.)*

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
now carries an **s188 addendum**: locator lookup mandatory per packet, the
four b009 triage blockers, 8959 aggregate smoke-test-first) · **CC
sessions = engine/tax-law work only.**

### s188 shipped (b009 findings; migration 0231)
- **① Shell-locator lookup** (b009: invoice-number locators resolved
  packets to unrelated clients — the TaxWise invoice number is NOT the
  Delvio client number): read-only `GET /api/v1/backentry/shell-lookup/
  ?q=<name>` — tokenized, order-insensitive client-name search over the
  firm's seeded 2025 1040 shells; returns client_number + PII-safe display
  label + status + existing-doc count. Runner grew `-Action lookup
  -Query 'SURNAME FIRST'`. Rule everywhere: confirm the PDF's printed name
  against the display label BEFORE staging; never infer from the invoice.
- **② Four triage blockers the invoice list missed**: taxable state
  refund (Sch 1 L1), 1099-MISC other income (Sch 1 8z), deductible IRA
  (Sch 1 L20 — sch1_fields stays 11/21 only), any non-GA state return
  (even beside a GA part-year). `triage_inbox.py` gained a BLOCKED list
  (checked before SUPPORTED) + a non-GA `STATE…RETURN` regex; the
  AUTHORING_GUIDE gained a "b009 return-face checks" section (the invoice
  alone cannot see Sch 1 lines 1/8z/20 — check the printed face).
- **③ Form 8959 aggregate Medicare wages, narrowly** (b009: a packet
  printed the $250,327 line-1 aggregate + $3 AMT but its W-2 detail
  omitted every box 5): new Taxpayer input `amt_medicare_wages_agg` (migration 0231,
  applied) — a FALLBACK compute uses ONLY when no W-2 row carries box 5;
  per-row box 5 always wins. Per-spec: RS 8959 line 1 is the input fact
  `amt_medicare_wages_l1` (fetched + verified this session). Staging warns
  on agg-vs-rows mismatch, on the unevaluable single-W-2 >$200k engage
  arm, and on W-2 rows with wages but no box 5 anywhere. Deriving box 5
  from rounded box 6 stays forbidden (documented, not enforced in code —
  it is an authoring rule). Engine engage gate UNCHANGED (spec fidelity);
  the aggregate engages via the line-4>threshold arm, which covers the
  b009 shape exactly ($250,327 MFJ → $3, test-pinned).
- **Safety behavior preserved per b009 finding #4** — frozen staged
  batches, explicit prod URL, named commits, re-dry-run, tie-only
  mark-filed, display labels in stage output: all untouched.
- Tests: staging band + topic8 band 82 green (incl. 3 new 8959 pipeline
  tests + 5 staging-guardrail tests + 5 lookup tests); flow assertions +
  backentry commit/reconcile/markfiled 562 green. Schema
  `batch-import.schema.json` regenerated (carries the new field).
- Docs: CC_IMPORT_LANE_HANDOFF (§2 endpoint, §3 locator rule, §0 table,
  §4 W-2 bullet), AUTHORING_GUIDE (triage section, w2s box-5/8959 rule,
  hard-rules list), CODEX_KICKOFF_PROMPT s188 addendum.

### Next engine work — KEN RULED at s188 close (2026-08-02, three calls)
**The s189 unit is the lane-extension trio, AHEAD of the August GA unit:**
- **① Taxable state refund joins the lane** — carry the §111 worksheet's
  prior-year inputs (the existing `sr_*` Taxpayer fields; engine
  `compute_state_refund` already computes Sch 1 L1 + 8z from them).
- **② IRA deduction (Sch 1 line 20) joins `sch1_fields`** — GATE: fetch
  the RS Sch 1 spec first; proceed only if line 20 is input-typed (the
  s186 precedent). The FILED amount is already phaseout-limited.
- **③ `amt_medicare_wages_agg` gets an EDITABLE browser-UI surface**
  (Ken chose editable over lane-only/read-only) — serializer + Slate
  screen; preserve the fallback semantics on the face (per-row box 5
  wins; label it as the 8959 line-1 aggregate as filed).
- Each lane extension = smoke-test-first at the boundary per standing
  rule; both un-HOLD their b009 triage classes once shipped (update
  triage_inbox.py BLOCKED list + AUTHORING_GUIDE + kickoff prompt then).
**Behind the trio (unchanged order):** the August GA build unit (GA UET
line-42 worksheet + S4-8/S4-NB-18 NOL lines) · B002 row-creation family ·
Ken's ② MeF (`build_irs5695`) · ④ year-constant ruling · Sch F / 8829 /
6198 (HARD-pile gaps, Ken's go). SB 31 TY2026 military = standing RS W-item.

### Codex: b010 CLEARED on prod (Ken, s188 close)
Resume under the s188 addendum — mandatory name lookup per packet, the
four triage blockers, and the b009 Medicare-aggregate packet re-staged as
its own one-return smoke test.

## Lane scoreboard: 64 FILED — 0 unfiled, 0 held
Codex continues b009+ with the s188 addendum live (lookup mandatory, four
new blockers, 8959 aggregate smoke-test-first); ChatGPT has the HARD pile.

---

## Known traps (carried — do not re-learn)
- **s188: the TaxWise invoice number is NOT the Delvio client number** —
  locator resolution goes through `-Action lookup` name search, then a
  human confirms the display label. Never infer.
- **s188: `amt_medicare_wages_agg` is a FALLBACK, not an override** —
  any W-2 row carrying box 5 wins; the field survives
  `replace_documents` like every taxpayer scalar (correction = send 0).
- **s187: the era-scoping prediction is itself a hypothesis** — engine-
  scope-first (read compute + the model) decides, in either direction.
- **s187: never import computed row columns** — passive_8582_allowed/
  _suspended are Form 8582 OUTPUTS (input = `prior_year_unallowed_passive`);
  net_profit/qbi_amount/etc. likewise. Staging rejects them.
- **s186: a "reproduced engine defect" can be shell residue** — probe the
  shell's scalars + overrides BEFORE believing a no-tie names the engine.
- **s186: sch1_fields is for spec-typed INPUT lines only** (11/21).
- **s185: Sch A 5a is DERIVED** (documents' W/H + dated payments);
  nonzero `scha_salt_income_or_sales` direct entry wins.
- **s184: blank prior-year 2210 COMPUTES the 90% fallback.** Printed
  2210 → line 8; else Three-Year Summary; TaxWise-blank → 0/omit.
- **A correction payload must send explicit 0** — omit preserves.
- **`replace_documents` does NOT clear field-level overrides or taxpayer
  scalars** — covers the s187 families too (children cascade).
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
  (s188 fetched the 8959 spec before the compute change — line 1 is
  input-typed, which sanctions the aggregate direct entry).
- `pytest tests/test_flow_assertions.py` after any compute change (s188: green).
- Dev environment shares the **production** Supabase DB — every write is a
  production write.
