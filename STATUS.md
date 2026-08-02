# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-02, session 183 (**THE LANE SCALED — 59 FILED.**
Batches 005 + 006 authored and filed; the last held returns cleared; the
whole filed back-catalogue regression-swept clean; the plaintext-SSN
importer leak closed; the import lane documented for Codex. ⚠ Two of my own
diagnoses were WRONG and Ken overturned both — see "Open rulings".)*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS
Render auto-deploys **both** services (prod + demo) from `main` —
`render.yaml` sets no `autoDeploy: false`. Verified s183 by matching bundle
hashes on both hosts against a local build, plus the `LIC-CHILD` seed row on
the prod DB. `build.sh` runs `seed_all` on every build, so seeds ride along.
**Treat every push as a production deploy.** Open question for Ken: set
`autoDeploy: false` so the "Ken's go" gate is mechanically real?

---

## ▶ RESUME HERE — three things, none of them blocked on code

### 1. Two open rulings from Ken (both written up in `REVIEW_QUEUE.md`)

**(a) Form 2210 — the 90% fallback.** When a return carries NO prior-year
tax figure, should line 9 fall back to line 5 (90% of current year) and
compute a penalty, instead of asserting none? Today we assert none (s182
`765ec3d`, Ken's ruling #6). Evidence, rollback-tested, zero writes:

| Return | Filed 38 | Ours now | With fallback |
|---|---|---|---|
| MOODY PEGGY #3293 | 114 | 9 | **114 exact** |
| JONES-TINA #2743 | 82 | 0 (blank) | **83, in tolerance** |

Regression check across all 61 answer-keyed clients: **zero** returns move
outside tolerance. It would file PEGGY (the last unfiled return) and close
JONES-TINA. Recommendation: make the change. **NOT implemented — it
reverses Ken's own ruling and changes real penalty math.**

**(b) `autoDeploy: false`?** See the standing fact above.

### 2. ~24 lane-eligible packets, no code change needed
- **19 mis-bucketed HARD packets** carry no blocking form — several were
  bucketed only for Form 8960 (NIIT), which the engine computes. List:
  the empty-array entries in `tmp\pilot-001\hard_blockers.json`.
  ⚠ Author ONE and confirm it ties before trusting all 19.
- **5 of the 8 AMBIG/probe packets**: ARRINGTON · COOPER · HAGGARD ·
  HARRISON · HUFF. (BROOME is the ChatGPT lane's. HUTCHISON and LITTLE are
  blocked only by 1099-G unemployment.)
- **Who does these is now a live question** — see §3.

### 3. Codex now has the import lane — decide the split
The lane is documented and handed off (`D:\tax-test-data\CC_IMPORT_LANE_HANDOFF.md`).
Three workers now exist and the boundary must stay capability-based, never
alphabetical (the old A→D split collided because both lanes drew from one
pool):
- **ChatGPT (browser UI)** — the 175 HARD packets. Only the UI can enter
  Sch C/E/F, depreciation, K-1, 2441, 8863, 8962, HSA.
- **Codex (import lane)** — the ~24 lane-eligible packets above.
- **This session** — engine/tax-law work; stop authoring packets by hand if
  Codex takes them.

---

## Lane scoreboard: 59 FILED — 1 unfiled, 0 held
s180 pilot 10 · s180c batch-002 9 · s181 batches 003/004 14 · s182 +5 ·
s183: +3 held returns, +1 FORRESTER re-file, +9 batch-005, +8 batch-006.
*(counted on prod, not from session notes: 100 total 2025 1040s with
status=filed = 59 lane + ~41 the UI lane.)*
**Unfiled: MOODY PEGGY #3293** — blocked only on ruling 1(a).

---

## What shipped this session (all deployed — pushing deploys)
| Area | What |
|---|---|
| Importers | 4 write sites across 3 importers stopped refilling `clients_entity` with plaintext SSNs; new `identity.capture_individual_ssn()` contract; `import_prior_year` matches individuals by identity HMAC |
| Data | MOODY RONALD #3294 TY2025 return deleted (Ken's ruling); S-24's 587-row SSN cleanup ran and is complete |
| Docs | `docs/CLIENT_DATA_QUALITY.md` §1a/§1b; `scripts/gen_backentry_schema.py`; the Codex handoff set in `D:\tax-test-data` |
| Lane | AUTHORING_GUIDE gained the 2210-line-8 rule and the GA `LIC-NODEP` rule |

---

## Known traps (earned this session — do not re-learn)
- **Prior-year tax for Form 2210: if the packet PRINTS a 2210, use LINE 8.**
  The Three-Year Summary's "tax liability after credits" can disagree and is
  only the fallback when no 2210 is printed. (LINN: 1,187 vs 9,693.)
- **A source that reconciles thirty times can still be the wrong source** —
  it only shows up on returns where the number actually matters.
- **`replace_documents` does NOT clear field-level overrides**, and replaces
  only the sections the payload supplies.
- **1040 line 37 as printed INCLUDES the line-38 penalty** in these packets.
- **The dry-run IS the commit, rolled back** — not a simulated plan.
- **GA `LIC-NODEP` gates the whole Low Income Credit** and is derived from
  nothing. Set it when the GA face prints 17a/17c; do NOT set it when the
  filer is claimed as someone's dependent (BREANNA is the counter-example).
- Flat `est_payment_q1..q4` drive 1040 line 26; dated
  `federal_estimated_payments` drive only the §6654 accrual. **Keep both.**

---

## Standing gates
- `git pull origin main` at session start; push with `git push origin HEAD:main`.
- Never `git stash`, never `checkout` mid-session.
- Rule Studio spec required before touching `compute*.py` / `renderer.py`.
- `pytest tests/test_flow_assertions.py` after any compute change.
- Dev environment shares the **production** Supabase DB — every write is a
  production write.
