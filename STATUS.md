# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s333 close, 2026-09-05

**▶▶ START HERE (s333 close, 2026-09-05): BATCH C OF THE RANKED FORTY IS WORKED —
10 of 10 dispositioned: 4 built and deployed, 1 half built (its other half is
Ken's), 2 already built/fixed, 3 spec or Ken questions.** Batches A and B were
worked in s332 (their ledger is in `STATUS_ARCHIVE.md`, s333 entry). Every
build was verify-first, replayed on the item's own staged payload rolled back,
movement-gated where compute changed, deployed and Render-verified; per-item
annexes are on `CC_CODE_CHANGES_1040_BATCH-015.md` / `…BATCH-296.md`. **Ken is
running Codex on the ENTRY of the returns the batches freed.**
⚠⚠ Standing facts every build must know: (1) **the deploy runs `migrate` ONLY —
a new diagnostic code is INERT until `manage.py seed_rules` runs** (none added
this session); (2) **the public mirror's PII guard trips on client SURNAMES —
client numbers only in STATUS / BUILD_ORDER / REVIEW_QUEUE**, and **no real
personal names in checked-in test fixtures** (two s333 fixtures were pushed
with real names for ~20 minutes and corrected in the next commit — the SHAs
`91187646` and `d899e3ae` still carry them; a rewrite is Ken's call, s329
policy); (3) **never start a foreground pytest while a background pytest
runs** — the conftest's connection kill reads as failures in both.

**▶ THE s333 BATCH C LEDGER (item → result → commit):**
| # | item | result | delvio |
|---|---|---|---|
| C1 | 015 #5 §469(g) release, passive non-PTP K-1 | 🏁 built; released losses IN the 8582 MAGI (rentals too); #43's release reaches the per-row face | `a16d18fd` |
| C2 | 015 #9 Form 8615 | half built: lane singleton + face reader + parent name/SSN (mig 0394); client 1969 stages; ⛔ two Ken questions | `d899e3ae` |
| C3 | 015 #11 GA QEE credit rows + line 21 | 🏁 built: the GA-500 Schedule 2 reader; client 2168's page reads whole | `91187646` |
| C4 | 296 #21 partial PTP loss | ⛔ Ken — the vendor allowed a PTP loss against no PTP income; i8582 suspends it | — |
| C5 | 296 #45 filed-split K-1 → 8582 / 8960 | 🏁 built both halves; the s234 8960 4b clamp measured the NET of 4a; client 1410 replays within the payload's own $10 | `a16d18fd` |
| C6 | 296 #46 2441 Part III | ✅ already built s272 | — |
| C7 | 296 #40 AMT passive losses | ⛔ spec-scoped (6251 L2m + 8582 spec RED-defer); RS lane | — |
| C8 | 296 #69 NC Sch S line 17 | ⛔ spec: a federal pull is an RS amendment | — |
| C9 | 296 #70 Form 4361 → Schedule C | 🏁 built (`is_ministerial`, mig 0393; RS R-MIN-4361 amended `e5a7e31`); client 2821 replays to a full TIE | `efc215b2` |
| C10 | 296 #63 commit HTTP 500 | ✅ already fixed `da12fc3` (a JSON-null payer_name in the payload) | — |

Deploys all Render-verified LIVE (the last: `dep-dae2ghf9r02s73eaaigg` for
`01fa75b0`, the fixture scrub; `d899e3ae` went live as `dep-dae2g1gu01pc73cr13qg`). Movement
gate: the 2 filed released-rental returns re-ran rolled back, zero moved.
Migrations 0393 + 0394 were applied on the shared DB by hand before the deploys.

**⛔⛔ KEN — THE PRINT ASK STANDS (ruling 5's retraction is REVERSED).** You do
need to print the full federal + GA for the **59** Gail 1040-lane clients. The
s330 "everyone already has both returns" finding counted any page containing
the string "(Form 1040)" — which appears on Schedule C/SE/A footers and on the
orphan Schedule 1 page a Georgia-only print drops at the end. Measured three
independent ways, all agreeing: **18 of the 61 held clients carry a real Form
1040 page 1; 43 do not**, and the 18 are not enterable either (no source-detail
page in any of the 61). Census: `1040\tmp\s331_gail_masthead.json`. DECISIONS
ruling 5 carries the method and both figures.

**▶ NEXT, in the order that lands the most returns:**
1. **Ken's answers** (Wednesday below) — they unblock more packets than any
   build: the RIE line-10 definition (A10 + 296 #85), the Schedule B ownership
   tag (A2(b), ~26 packets), the former-passive row (B7), the two 8615
   questions (C2), the PTP allowance (C4).
2. **The named TaxWise reader walls by measured count** — 6251 (6 sole, 17
   total; `amt_*` + `compute_6251` exist → reader-only) → the Form 4562 face
   (3; frees A4's two farm registers) → 1116 (5) → sched_line_detail (4) → the
   qualified-tips per-W-2 box-7 split (296 #60's sibling) → the portfolio K-1
   consolidation (DEFERRAL (13)). Drive ONE single-wall packet by hand through
   each before trusting the census (s328).
3. **The OWN UNITS of the triage foot** — 015 #4 Form 3800 (the s331 named
   wall) · 015 #10 Georgia Form 500X lifecycle · 296 #52 Alabama 40NR app
   build · 296 #10 Form 4835 · 296 #56 Form 6252 contingent-payment · the
   IND-CR 212 app build (spec authored s332).
4. **The four fenced clients' filed flag** (2777 · 3630 · 4159 · 4160) and
   ruling 2's penalty acceptance (~10) + ruling 3 (client 3250).
5. **The Lacerte face readers by wall count** — Sch 3 (107) → A (37) → B (32)
   → C (18) (Sch 3's cause is known: Lacerte splits `5a` into `5` + `a`).
6. **⑥c `manage.py merge_client`** (D-044) — first worklist: the 13 all-blank
   duplicate-name groups (DEFERRAL (12)).
7. **The 1065 import** — 95 partnership returns behind it.

**⛔ WAITING ON KEN — WEDNESDAY AGENDA (s332 + s333):** the two season-2026
design questions (what replaces the answer key; how scanned documents get in)
— recommendations in REVIEW_QUEUE, shelf units in BUILD_ORDER · a Form 4136
spec in Rule Studio (296 #48) · the RIE line-10 definition (015 #2 + 296 #85,
one question) · add `manage.py seed_rules` after `migrate` in `build.sh`?
(DEFERRAL_AUDIT (10)) · the Schedule B ownership question (A2(b)) · code L in
`EARLY_CODES` · the former-passive row (B7) · **s333:** the vendor's PTP
allowance (296 #21) · the 8615 pair — parent zero-vs-missing (D_8615_005) and
the QDCGT branch / the vendor's line 9 (015 #9) · the NC line-17 federal pull
(296 #69, an RS amendment) · an AMT-8582 rule set (296 #40, RS authoring) ·
whether to rewrite the two s333 SHAs that briefly carried fixture names.

**⛔ WAITING ON KEN (carried):** the BATCH-013 item-2 RIE placement flag above ·
the packet in tmp/s328_ken_questions.md (GA 500 p1 names a different primary
SSN) · seed ONE client (the …4641 taxpayer in Jenny's book; do NOT edit #4054)
· client #3572's contaminated name (#4514 same shape) · three clients with no
2025 1040 shell · client 2149's filed GA 17a = 2 exemptions on a
single/zero-dependent return · the …4203 W-code question · the asset
METHOD-DERIVATION TABLE review · vendor-name allowlist for the mirror guard? ·
carried: 1071 · 1141 · R-GA500-RIE · 4059 W-2G address · Sch D carryover · GA
RIE L10 · 4081's $169 · standing 1–8 · 2a scope flag · AL 40 · the 4 Tom-book
holds (…8505 · …2276 · …2827 · …8791) · `D_1099MISC_RECON` per-document vs
aggregate (DEFERRAL 19e) · the Schedule C address model gap (DEFERRAL 19a).

**▶ BUILD QUEUE after the named walls:** ④ the three re-raised Lacerte engine
holds (clients 1922, 2386, 3517) · ⑤ the GA 7b military-exclusion engine leg
(8 witnesses; waits on the states-lane spec export) + the 7c/7f DIS
transcription · ④ the shadow-2210 reader · carried: the 8615 parent-first
guard · the zero-activity GA-attach gap · the Schedule C address (DEFERRAL
19a) · the IRS1099NEC document question (19b) · the AMT-1116 twin head, still
unwitnessed in this corpus · the client-1410 Georgia residual (the GA itemized-
deductions adjustment worksheet class, no import surface).

⚠⚠ **ANOTHER CC ACCOUNT WRITES THIS LANE — THE SHARED FILES ARE THE ONLY
PLACE ITS WORK IS VISIBLE.** Do not order events by `updated_at`.
