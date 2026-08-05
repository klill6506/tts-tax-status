# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-05, session 212 (the CC-Changes loop, live). This
session CLEARED THE QUEUE THREE TIMES: **BATCH-004 tranche B (migrations
0237-0239, deploy `6d584d0`)**, **BATCH-005 (deploy `5cd3e09`)**, and
**BATCH-006 (migrations 0240-0241, deploy `16dfd7e`)** — all annexed and
moved to `CC Changes Done`. Also authored the **RS Form 6765 spec** on
Ken's go (WO-14, awaiting his seed approval). The loop is
WATCHING for the next Codex batch file; the 1120-S Inbox (~200 packets)
drains on Codex's side. Ken's directive this session: keep working until
Codex empties the Inbox / new batches stop arriving; move any other-state
or K-2/K-3 packets to `..\Hold\` (swept — none new; the four parked ones
stand: 101 NC · 187 CA · 202 K-2/K-3 · ABE SC).*

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
`tts-tax-app` still fails every push — Ken should delete it.**
Batch pushes when a pilot is actively entering (s208).

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

---

## ▶ RESUME HERE

### ✅ Done in s212 — CC BATCH-004 tranche B (deploy `6d584d0`, migs 0237-0239)
- **#1** M-1 book income on the K18 basis: the `M1_6b` auto-fill
  (K12a+K12b+K11) REMOVED (RS 1120S_M1 line 6 = input line); **migration
  0237** releases stale non-overridden auto-filled rows; MeF line-6
  statement adapts; packets 136/137 exact-tie regressions. Packet 1013's
  $250 charity delta = same defect, clears on retry.
- **#2** GA-600S S7_8/S8_5 SUPPRESS when federal = GA depreciation
  (checked vs Ken's s197 gross-pair ruling — Lacerte itself suppresses
  when equal; the pair stands whenever totals differ).
- **#3** `PassthroughRentalEntity` row family (**migs 0238 + 0239 RLS**):
  8825 line 22a/22b → 23 → K2. Discoveries: 8825_L21/8825_L22a FormLines
  were NEVER SEEDED (new `form8825_summary` section closes the s48 gap);
  `aggregate_rental_income` early-exited with zero properties (packet
  168's exact shape); the 22b field-map "amount" column is really the EIN.
  Render handles pass-through-only 8825s. Lane section + schema regen.
- **#4** Pub 946 recovery cap in ALL THREE arms (federal / GA / AMT),
  gated on coherent keying (lossy pre-R018 lumped history skips — the
  barn pin holds); GA identical-treatment fallback; the AMT cap keeps
  K15a neutral. Packet-155 regressions.
- **#5** `QBI_W2_EXCLUDED` input line: QBI_W2_WAGES = 7 + 8 − exclusion
  (Rev. Proc. 2019-11); all-qualified default preserved.

### ✅ Done in s212 — CC BATCH-005 (deploy `5cd3e09`, NO migration)
- **#1 REFUTED with live-shell evidence**: replace_documents explicit
  zeros WORK; the stale carrier on packets 209 AND 218 is **L14a "Other
  assets" duplicating L1a** (a seeding artifact class) — retries need
  `L14a: 0`. Permanent regression added.
- **#2 ⛔** no RS 6765 spec (404) → Ken/RS agenda; packet 227 held.
- **#3 ⛔** SC lane Ken-ruled not-yet; ABE in Hold.
- **#4** Schedule F verified authorable via F* `fields`; **F34/K10 joined
  the restricted answer key** (schema regen). ALECMTN authorable.
- **#5** locator `ein` backfills a blank entity.ein at commit (differing
  keyed EIN kept + warned) — clears MISSING_EIN (LOSSABOR).
- **#6** SCHED_L_DEPR_TIE compares GROSS original cost + full accumulated
  (prior bonus + §179, no double count) — DEERRUN regression.
- **#7** line 12 = `D_TAXES_*` input family, verified end-to-end.
- **#8** entity `dispositions` rows (is_4797) now drive
  aggregate_dispositions (same classify_disposal routing) AND the entity
  4797 render — TCPUBLIC $65,000 §1231 → K9 exact tie. Known limits:
  row AMT deltas don't feed K15b; MeF 4797 mapper still asset-only.
- **#9** GA-600S Schedule 1 gates on GA_PTET (income lines, not just the
  tax). ⚠ Existing non-PTET GA returns zero their S1 lines on next
  recompute (correct direction, s197 open-item-7 class). RS spec lacks
  the gate — RS agenda.
- **#10** JSON floats normalize via repr() before DecimalField cleaning
  (both lanes + commit path).

Gates this session: 816 + 219 + 85 + 603 (batch-004 sweep) + 817
(batch-005 sweep) — all green.

### ✅ Done in s212 — CC BATCH-006 (deploy `16dfd7e`, migs 0240-0241)
- **#1** Tables A-6/A-7 year-specific rates coordinate-extracted from Pub
  946 (2025) pp.73-74 (A-6's alternation flips phase at mid-year; A-7
  checkerboards from year 9); final-year partials subtract the real
  sequence; every month column sums to 100%. A-13/A-7a/A-13a verified
  constant. PASSRJK → $3,039 federal AND Georgia.
- **#2 RESOLVED as a mapping question** — the dollars were right: the
  app's `S4_*` keys follow the **2024** 600S template; the Rev. 09/11/25
  face renumbered (balance due = face 6 = `S4_5c`, amount due = face 11 =
  `S4_10c`). Transcription map now in `CC_ENTITY_LANE_HANDOFF.md`; seed
  labels annotated; packet-100 regression. ⚠ Ken: a 2025 DOR template
  refresh is the real fix (needs his download).
- **#3 / #9 REFUTED live** — signed L1a persists and sums (L15a =
  10,021); the 50%-business SL F-350 computes 1,543 in all three arms
  under every keying probed. Both pinned; Codex re-runs against current
  prod.
- **#4** K15b joined the restricted federal answer key + closeout.
- **#5** `flow_to: "none"` (**mig 0241**): register-only assets keep the
  4562/Schedule L/UBIA and their K15a/§179 derivations with zero line 14.
- **#6** QBI W-2 wages now include Form 1125-A line 3 cost of labor.
- **#7** `D_AMORT`: the entity amortization register is canonical for
  line 20 (auto component + MeF statement), never line 14. ⚠ the 1065
  D_AMORT seed line is a follow-up (its write skips silently until then).
- **#8** six-decimal ownership percentages (**mig 0240**, widening only).
  ⚠ packet 127's filed K-1 dollars may differ by $1 from our
  last-owner-absorbs residual — the R-K1-ROUND tie-break already on the
  RS agenda, not a new defect.
- **#10** the recovery cap moved to a GROSS-BASIS frame in all three arms
  (prior regular + prior bonus + prior §179), keying-agnostic, still
  coherence-guarded; kills the spurious K15a on a §179-expensed asset.
Gates: 978 green.

### ✅ Done in s212 — RULE STUDIO WO-14: the Form 6765 spec (Ken's go)
Authored in `sherpa-tax-rule-studio` (`d33584c`), **status ⏳ AWAITING KEN
(seed approval)** — `READY_TO_SEED=False` until Ken says "Approve — flip,
seed, export". Sources fetched + read verbatim: f6765 Rev. 12-2024 face,
i6765 Rev. 12-2025 (⚠ label mismatch flagged), i1120ssk box 13 **code M**.
Gate-1 scope walk = RS DECISIONS **D-16** (4 Ken rulings: fixed-base %
preparer-entered · §280C diagnostic-only · Section D deferred+HOLD ·
controlled group HOLD). 24 facts / 7 rules / 50 lines / 10 diagnostics /
6 scenarios / 2 draft flow assertions; all rules cited; SQLite harness
19/0. Packet 227's shape pinned (QREs 53,704 → 4,243; line 21 INFERRED).
⚠ Section G becomes REQUIRED for TY2026 — the spec's staleness boundary.
**Blocks batch-005 #2 / packet 227 until the flip + the app build**
(which must COMPOSE 6765 with 8941 on Schedule K 13g, never stomp).

### 📋 The 1040-lane queue is LEDGERED (s212, Ken's question)
`D:\tax-test-data\CC Code Changes\` (the 1040 lane — SEPARATE from the
1120-S queue) had 7 unique files and no Done folder: it predates the
annex-and-move contract, which only ever existed on the 1120-S side. Both
now match — a `CC Code Changes Done\` folder exists and a verified
per-file ledger lives in that folder's new `README.md`. **They were open,
not un-filed**: 046 is 8/10 (open: Form 1310 · the 8606 IMPORT LANE —
engine complete) · 047's #16-20 still queued · NZ is 4/10 (open: Sch D
QOF answer · 8889/HSA · Sch F · SS lump-sum · 1099-G · multi-state) ·
A-M is blocked on Ken's two asset calls · PULLIAM is 6/7 (open: #7 basis
worksheet). Only BATCH-041 was closed (s198 — the Hodges $2 was a missing
$33 §1250 input, pinned by `test_hodges_sdtw_1250_s198.py`); annexed and
moved. ⚠ Two byte-identical duplicate copies + one superseded earlier
draft await Ken's OK to delete.

### ⚠⚠ s212 LATE — THE LOOP CAUGHT MY OWN REGRESSION (fixed `48ad1dc`)
**batch-006 #10's gross-basis recovery cap was batch-007 #3's bug.** The
gross frame (subtract prior regular + bonus + §179) is valid ONLY under
the R018 split contract. With NO `original_cost`, `cost_basis` is ALREADY
net of a recorded prior bonus — subtracted twice, it HALVED a legitimate
SL deduction (packet 156's Raptor: 11,082 → 6,354, reproduced exactly)
and moved K15a with it. `_recovery_remaining()` is now keying-aware
(gross frame iff original_cost set; else prior bonus is already out but
`sec_179_prior` still subtracts — the handoff's packet-138 keying).
Deployed. Both shapes pinned; batch-006 #10's F-150 still zeroes.
**Also guarded:** batch-006 #7 made the amortization register canonical
for line 20, so a legacy "Amortization" `other_deductions` row DOUBLES it
(13,334 for one 6,667) — staging now warns (a doc was not a guard).

### ▶ 1040 LANE reopened and shipped (`7bba38c`)
Ken's directive 2026-08-05: work BOTH queues. Done: **046 #10 Form 8606
lane** (`form_8606s`, one row per owner — engine complete since mig 0079;
duplicate-owner ERROR, blank-line-6/all-zero WARN; the Thornburg
basis-only shape ties 2=3=14) · **047 #16-#20** (5695 `e5695_*` · 6251
AMT facts · `dispositions`/`installment_sales`/`like_kind_exchanges`).
⚠ `amt_medicare_wages_agg` is Form **8959**, NOT 6251 — the prefix
collides. ⚠ `property_character` = `capital|business_1231|ordinary`.
**NZ #1 was ALREADY built** — the item proposed `schedule_d_qof`; the
shipped field is `schd_qof_disposal` (verify by the SHIPPED name).
The 1040 queue now has the same annex-and-move contract + a verified
ledger (`D:	ax-test-data\CC Code Changes\README.md`); batch-041 filed
to `CC Code Changes Done\`.

### ▶ NEXT: BATCH-007 (10 items, #3 DONE) + the CC-Changes loop
`1120S\CC Changes\CC_CODE_CHANGES_1120S_BATCH-007.md`. **#3 fixed and
deployed.** #2 (line 14 + line 20 double count) is most likely the
amortization-duplicate shape now guarded — a normal page-1 asset does NOT
double count (probed); ask Codex to re-run. Remaining: #1 sold assets
must stay in BEGINNING Schedule L · #4 passenger-auto cap + disposition
convention on a sold listed asset · #5 M1_3c → K16c/M-2/basis · #6 M1_5b
detail → M-1 + the book bridge · #7 GA Schedule 5 allocation inputs ·
#8/#10 SCHED_L_DEPR_TIE (legacy-asset exclusion; regular-vs-AMT arm) ·
#9 the 150DB/MQ 7-year year-2 table rate (20.85%).

### ▶ NEXT: the CC-Changes loop continues
- A background watcher polls `D:\tax-test-data\1120S\CC Changes\` for the
  next Codex batch file (re-arm ~10-minutely). Work it verify-first, ONE
  deploy per batch, annex, move to Done.
- Hold sweep: only state / K-2/K-3 family packets move to `..\Hold\`
  (README contract); lane-capability holds stay in the Inbox as
  `.HOLD.md` companions.
- When the Inbox empties: the 1040 lane resumes at **8606 (046 #10)**,
  then 047 items 16-20, MIXED-PILOT #7 (K-1 basis worksheet), the true
  builds.

### Artifacts left on the shared DB (deliberate, s212)
- Migrations 0237/0238/0239 ride the deploys' build.sh (additive/data-
  release; no early-apply skew risk — verified s190-class safe).
- Read-only probes only on packets 209/218 shells — no data changed.
- The s208/s209 artifacts stand as recorded last session.

### ⛔ KEN DECISIONS OUTSTANDING
- Form 6765 (research credit): NO RS spec — author one or rule the lane's
  scope (batch-005 #2; packet 227 blocked).
- The A–M item-7 asset decisions (#13/#14 in Open items, unchanged).
- States + K-2/K-3 stay on hold pending the RS state-module sessions
  (2026-08-05 →): 101 NC · 187 CA (needs a Lacerte re-export WITH the CA
  pages) · 202 K-2/K-3 · ABE SC.

## ⚠ Open items for Ken
*(carried from s210 — unchanged except as noted)*
1. §179 base components (guaranteed payments / 4797 gain / 1041 K-1s).
2. Rotted-pin surfacing — weekly full-suite cron still wanted.
3. RS `GA600S R001` bonus-only presentation (s197). **NEW s212: the
   GA600S spec's Schedule 1 rule also lacks the PTET gate (batch-005 #9).**
4. RS `R-GA500-DEPR` stale (denies GA §179 conformity).
5. GA §179 real-property carve-out rule wanted.
6. No GA 4562 produced though the 600S says "attach".
7. Other GA-600S returns move on next recompute (s197; now ALSO the
   batch-004 #2 equal-pair suppression and batch-005 #9 S1 gate classes).
8. Lacerte PDF importer doesn't read GA depreciation pages.
9. Federal-bonus-history + no keyed GA prior over-depreciates — diagnostic
   wanted (the batch-004 #4 fallback narrows but does not close this).
10. RS rule for shareholder-side §179 disposition (K-1 GAP 1).
11. GA PTET base on separately stated gain (s195).
12. RS `R-AGG-SUMMARY` spec edit pending.
13-19. Unchanged from s210 (see STATUS_ARCHIVE for detail).
**NEW s212:** RS 6765 spec wanted (batch-005 #2) · MeF 4797 mapper reads
assets only — entity Disposition rows absent from the e-file face ·
Disposition-row AMT deltas don't feed K15b.

## The three K-1 → individual gaps (parked) — unchanged (s210).

## Carried queue — unchanged from s210 (STATUS_ARCHIVE holds the list),
plus: **RS agenda additions (s212):** GA600S S1 PTET gate · 6765 spec ·
1120S_M1 line-6 input ratification (the M1_6b auto-fill removal) ·
RS 8825 22a/22b row-family fact (built ahead of an explicit spec rule).
