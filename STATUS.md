# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-08 (s231). **One unit: Form 3800 Part III pass-through
rows 1c / 4h / 4i + the §38(c)(5) eligible-small-business determination —
BUILT.** The unit Ken promoted off the s230 note: one build unblocks BOTH
entity-lane credits. Form 8941's K-1 box 13 code BA and Form 6765's code M each
reached a shareholder's 1040 and dead-ended — the recipient `ScheduleK1` had no
credit field at all. Migration 0271. No movement on any existing return.*

*Previous (s230): Form 6765 built; Schedule K line 13g became COMPOSED (`973a5b8`).*

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
**He has his laptop — availability is MINIMAL BUT NOT ZERO** (Ken, 2026-08-07).
Batch questions; keep them low-friction. Nothing is on a clock in that window;
the next hard deadline is 2026-09-15 (extended entity returns).

---

## ▶ RESUME HERE

### ⭐ NEXT UNIT — sweep the CC queues at boot, then the s224 ruled list
All three CC queues were EMPTY (or parked) at s231 boot, unchanged from s230.
The ruled backlog remains: **8853 Section C** (long-term care, §213(d)(10)
cap), then the 1065 K17a, GA bulk-sale, both e-file refusals, identity
read-back, 1310 box B upload + `ForeignAddressType`, CR-2026-001.

**⚠ New candidate from this session** — the **§38(c)(6)(A) MFS threshold**
defect (below). It is small, it OVER-allows, and it is fully specified by the
statute; it needs only a preparer assertion about the spouse's return.

### The queue right now
- **1120-S** (`1120S\CC Changes\`): EMPTY at s231 boot (README only).
- **1040** (`1040\CC Changes\`): EMPTY at s231 boot (README only).
- **Legacy root** (`CC Code Changes\`): ONE open file — the NZ file (9 of 10;
  #10 multi-state parked under the states-on-hold ruling). Unchanged.

### ✅ s231 in one paragraph
Built the **Form 3800 Part III pass-through leg**. Verify-first: Form 3800 was
already substantially built (2026-07-04), so this was a GAP unit, not a
from-scratch one — and the gap was literal, the recipient `ScheduleK1` carried
no credit field of any kind. **Row assignments are FACE-verified** against the
SHA-pinned 2025 f3800.pdf: page 3 **1c = Form 6765**, page 4 **4h = Form
8941**, **4i = Form 6765 (ESB)**. ⚠ **The RS 3800 spec is wrong here and was
NOT copied** — its three newly-exported `line_map` rows `1a`/`1b`/`1c` describe
the PRE-2023 Part I structure, carry no facts and no rules, and the spec has no
`4h`/`4i` row and no §38(c)(5) rule at all. The destinations came from the two
source forms' own approved DEST rules (R-6765-DEST: "ESB → Form 3800 Part III
4i; others → 1c"; R-8941-DEST: "all others (1040/1120) → line 4h") plus the
face and the statute; five spec defects are on the RS agenda. **The statute
corrected recollection twice**: §38(c)(4)(B)(ii) defines the ESB research
credit by reference to **paragraph (5)(A)/(5)(B)** (not (5)(C)/(5)(D)), and
**§38(c)(5)(B) applies the $50M gross-receipts test to the SHAREHOLDER, not the
S corporation** — which is why the determination is ONE per-return assertion
(`Taxpayer.f3800_esb_gross_receipts`), not a per-K-1 field. §38(c)(4)(B)(viii)
makes §45R specified outright, so row 4h takes no ESB test. **⚠ The sign
governs the default**: a specified credit has TMT treated as zero
(§38(c)(4)(A)(ii)(I)), so 4i is the MORE permissive row — an UNANSWERED
determination therefore routes to the REGULAR row 1c, where guessing can only
under-allow. Legs: migration 0271 (two `ScheduleK1` credit decimals with
`db_default` + the nullable `Taxpayer` assertion) → `form_3800_k1_credit_rows`
returning (routable, deferred) → the new rows joined `_REGULAR_ROWS` /
`_SPECIFIED_ROWS`, **which the renderer now IMPORTS instead of restating** (it
kept a private copy — exactly the seam that silently drops a new row) → field
map Line1c/Line4h/Line4i incl. column (c) pass-through EIN → three new
`D_3800_007/008/009`, zero code collisions → serializer + the K-1 box registry
+ the Form 3800 tab's ESB selector. **The loop closes**: `k1_import` now
carries s230's synthetic `K13g_M` / `K13g_BA` share keys onto the recipient's
credit fields, so an entity 6765/8941 imports straight through. The §469
posture reuses the K-1's own `material_participation` — same question, already
answered per activity, and non-null, so a K-1 credit can never sit in the
D_3800_002 unanswered state.

### ⚠⚠ THE FINDING THE UNIT ALMOST MISSED — MeF wrapper ORDER
`IRS3800Type` declares the Part III wrappers inside an **`xsd:sequence`**, and
`build_irs3800` emitted them in *inflow* order. That was correct only by
coincidence — `form_3800_inflows` happened to append its sources in face order.
The new rows sit at BOTH ends of the block (1c first, 4h/4i last), and putting
K-1 credits at the head of the inflow list broke the coincidence outright. The
builder now sorts into schema order, taken from the 2025v5.4 IRS3800.xsd
element declarations, and a test pins it with a deliberately shuffled input.
The three new wrappers (`Form6765CYCreditsGrp`, `Form8941CYCreditsGrp`,
`Form6765ESBCYCreditsGrp`) all exist in the schema, so the rows transmit rather
than refuse.

### ⚠ Classes that MOVE existing returns or output on next recompute
- **s231: NONE.** Every new row is fed only by a K-1 credit field that did not
  exist before this migration, so it is zero on every stored return. The row
  tuples widened, but an absent row contributes zero to every total. The one
  behavioural change to existing output is the MeF Part III wrapper ORDER,
  which was already correct for the rows in play and is now enforced.
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
  s106.py`, `test_ga500_rie_federal_pull.py`. ⚠ The 3 new `D_3800_00*` codes
  were checked against the live catalogue — **zero collisions**.
- **Client typecheck**: 55 error lines standalone (**unchanged by s231** —
  verified none of the 55 name any new field).
- ⚠ The Slate 8889 fixture cast `as HSAAccountRow` still swallows new
  required fields.

### ⚠ Test-run hazards (standing)
- **Never run two `pytest` invocations concurrently** — one shared test DB;
  the hazard is CROSS-REPO (both repos name it `test_postgres`). `--reuse-db`.
- A long `pytest ... | Select-Object -Last N` that hits the 120s timeout
  loses ALL output — redirect to a file and tail it.
- ⚠ A `poetry run python script > file` redirect BUFFERS. Use `-u`.
- ⚠ `grep -rl ... tests/` matches `__pycache__/*.pyc` — pass `--include=*.py`.
- ⚠ **New (s231)**: the Bash tool's cwd PERSISTS across calls — a `cd client`
  in one call leaves `poetry` unable to find `pyproject.toml` several calls
  later. Check `pwd` or use absolute `cd` in each call.
- ⚠ **New (s231)**: Python launched from Git Bash does NOT resolve `/tmp` to
  the same place the shell's heredoc wrote it. Write helper scripts with an
  explicit Windows path.

### ✅ KEN DECISIONS OUTSTANDING
- **⛔ KEN (s231, NEW — a real defect, queued not fixed): §38(c)(6)(A), the
  MFS threshold.** `compute_3800.SEC38C1_THRESHOLD` is a flat $25,000. The
  statute makes it **$12,500** for a married-filing-separately taxpayer whose
  spouse has any business credit. **⚠ The sign: this OVER-allows** — a smaller
  threshold gives a larger line 13 → larger line 15 → smaller line 16 → LESS
  credit, so using $25,000 allows MORE than the law does. Pre-existing (this
  session did not touch line 13). Not fixed here because the exception turns on
  a fact about the SPOUSE's separate return, which this return cannot see: it
  needs an assertion + UI + a diagnostic, i.e. its own unit. Full write-up in
  `DEFERRAL_AUDIT.md`.
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
- **NEW (s231), Form 3800 — five spec items. The 3800 spec does not describe
  the pass-through rows at all; everything below was built from the face, the
  statute, and the two source forms' own DEST rules instead.**
  (a) **The `1a`/`1b`/`1c` `line_map` rows are WRONG for the 2025 face.** They
  read "General business credits from Part I", "Passive activity credits from
  Part II", "Total current year general business credits" — the PRE-2023 Part
  I structure. The 2025 face has 1a = Form 3468 Part II, 1b = Form 7207,
  **1c = Form 6765**. They also carry no `source_facts` and no `source_rules`.
  (b) **There is no `4h` or `4i` row in the spec** even though R-8941-DEST and
  R-6765-DEST both name them as destinations. Add them (4h = Form 8941, 4i =
  Form 6765 (ESB)).
  (c) **No rule encodes §38(c)(5).** `R-3800-P3-INFLOW` enumerates only the
  1f/1s/1y/1aa/1zz/4e/4z rows. The ESB determination — and specifically
  §38(c)(5)(B)'s "such partner or shareholder", which puts the test on the
  RECIPIENT — needs to be a rule with its own fact.
  (d) **The bare Part III line numbering is ambiguous.** The spec prefixes
  Part III rows `P3-` (P3-1f, P3-4e) but the three new rows were exported
  bare (`1a`/`1b`/`1c`), where bare numbers already mean Part I/II lines.
  (e) **Three app-added diagnostics want adopting**: `D_3800_007` (ESB
  unanswered → row 1c), `D_3800_008` (1065/1041 box letter unverified →
  credit EXCLUDED), `D_3800_009` (two entities on one row → Part V not built).
- **Carried and now URGENT for the 1065 arm**: the **1065 Schedule K-1 box-15
  letters** for §41 (6765 spec: `[UNVERIFIED]`) and §45R (8941 spec: unnamed).
  `D_3800_008` excludes those credits until both are verified — this is the
  single thing blocking the 1065 pass-through arm.
- Carried: s230 Form 6765 items (a)-(e); s228 `D_K1B_FULLY_ALLOWED`; the s226
  `requires_human_review` verbatims; s227, s224, s223 and earlier unchanged.

## ⚠ Open items for Ken — carried unchanged (see STATUS_ARCHIVE).
## The three K-1 → individual gaps (parked) — one NAMED in s221: the received
## `ScheduleK1` has no fields for box 17 code K §179-disposition facts.
