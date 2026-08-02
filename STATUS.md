# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-02, session 184 (**THE LANE IS COMPLETE FOR ITS
ELIGIBLE POOL — 60 FILED, 0 unfiled, 0 held.** Ken ruled the 2210 90%
fallback [reversing his own s182 ruling #6]; MOODY PEGGY filed EXACT via
correction batch b006c; the Codex kickoff prompt is delivered.)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS
Render auto-deploys from `main`. **s184 refined the map:** prod
(prep.delviotax.com) = Render service `delvio-tax`; demo (demo.delviotax.com)
= `tts-tax-demo`. **A THIRD, orphan service (`tts-tax-app`, no custom
domain) also builds every push and FAILS in ~55s every time — Ken should
delete it in the Render dashboard.** s184 also found prod's build queue
JAMMED behind a 2+-hour hung build (normal ≈ 3 min); the Render API key in
`Passwords & Secrets` cancels a hung deploy and the queue resumes. Open
question for Ken (carried): set `autoDeploy: false`?

---

## ▶ RESUME HERE

### The import lane: HANDED OFF. This session lane = engine/tax-law work.
- Ken's split (s184): **ChatGPT-browser** takes the 175 HARD packets;
  **Codex** takes the ~24 lane-eligible (kickoff prompt written for Ken to
  post: `D:\tax-test-data\CODEX_KICKOFF_PROMPT.md` — mandatory one-packet
  smoke test, b007+ batch keys, ≤10/batch, never AllowNoTie); **CC
  sessions do engine work only** and stop hand-authoring packets.
- Next engine work per BUILD_ORDER: **the Sch A / Sch C / Sch E schema
  era** (Sch A alone unlocks 16 HARD packets). Behind it: the B002
  row-creation family · Ken's ② MeF (`build_irs5695`) · ④ year-constant.

### Done this session (all deployed to BOTH services and verified live)
| What | Detail |
|---|---|
| **2210 90% fallback (Ken's ruling, reverses s182 #6)** | Blank prior-year tax → line 9 falls back to line 5 (90% of current-year) and the penalty computes + files. Gate removed in `compute_2210.py`; pins inverted (*_s183: blank→471 vs harbor→372 — the gap is the proof); revert-proven; DECISIONS.md s184 entry |
| **Regression** | Rolled-back sweep over all 61 answer-keyed clients under the new engine: 60 tie; the 1 miss was PEGGY's own wrong-source shell datum (below) |
| **MOODY PEGGY #3293 FILED — tie EXACT (fed 37=3641, 38=114)** | Her b006 payload carried `t2210_prior_year_tax` 7465 from the Three-Year Summary (the LINN wrong-source class) against Ken's recorded "I entered none." Correction batch `b006c` sent explicit 0s (commit is only-payload-fields — omit would preserve), commit → tie → mark-filed. Report: `tmp\batch-006\b006c_report.json` |
| JONES-TINA #2743 | Now GENUINELY ties (38: 83 vs filed 82, inside the $5 penalty tolerance) — the "permanent exception" label is obsolete; REVIEW_QUEUE updated |
| AUTHORING_GUIDE | s184 note added: prior-year facts still mandatory when they exist; when TaxWise itself had none, enter 0/omit — the filed penalty IS the fallback |

## Lane scoreboard: 60 FILED — 0 unfiled, 0 held
s180 10 · s180c 9 · s181 14 · s182 5 · s183 21 · s184 PEGGY.
The answer-keyed eligible pool is exhausted; what remains is Codex's ~24
and ChatGPT's 175 HARD.

---

## Known traps (carried + new — do not re-learn)
- **s184: blank prior-year 2210 now COMPUTES the fallback** — an authored
  packet that omits genuinely-present prior-year facts will no-tie with a
  PHANTOM penalty (HAWKS class). Sourcing rules unchanged: printed 2210 →
  line 8; else Three-Year Summary; TaxWise-blank → 0/omit.
- **The wrong-source error travels into payloads and outlives rulings** —
  PEGGY still no-tied AFTER the engine fix until her shell datum was fixed.
- **A correction payload must send explicit 0** to overwrite wrong shell
  data (only-payload-fields commit; omitting preserves).
- If the packet PRINTS a 2210, prior-year tax = LINE 8, never the
  Three-Year Summary (LINN).
- **`replace_documents` does NOT clear field-level overrides.**
- **1040 line 37 as printed INCLUDES the line-38 penalty.**
- **The dry-run IS the commit, rolled back.**
- GA `LIC-NODEP` gates the whole LIC; set when GA prints 17a/17c, never for
  claimed-as-dependent filers.
- Flat `est_payment_q1..q4` drive 1040 line 26; dated
  `federal_estimated_payments` drive only the §6654 accrual. Keep both.

---

## Standing gates
- `git pull origin main` at session start; push with `git push origin HEAD:main`.
- Never `git stash`, never `checkout` mid-session.
- Rule Studio spec required before touching `compute*.py` / `renderer.py`
  (2210 has NO RS spec — authority = `specs/_2210_source_brief.md` + form face).
- `pytest tests/test_flow_assertions.py` after any compute change (s184: 521 green).
- Dev environment shares the **production** Supabase DB — every write is a
  production write.
