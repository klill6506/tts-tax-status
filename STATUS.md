# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s332 close, 2026-09-05

**▶▶ START HERE (s332 close, 2026-09-05 late): BATCHES A AND B OF THE RANKED
FORTY ARE WORKED — 20 items: 12 built, 3 half-built (the other half Ken's or a
sized unit), 2 already built, 2 for Ken, 1 waiting on a Rule Studio spec.**
Every build was verify-first, replayed on its own fixture (ties recorded in
each item's annex on `CC_CODE_CHANGES_1040_BATCH-015.md` / `…BATCH-296.md`),
movement-gated where compute changed (rolled-back re-runs; the one $1 mover
was control-proven pre-existing), deployed and Render-verified. Per-item
blocks are in `STATUS_ARCHIVE.md` (s332 evening); the ledger below is the
census. **Ken is running Codex on the ENTRY of the returns BATCH-014 freed.**
⚠⚠ Two things every future build must know: (1) **the deploy runs `migrate`
ONLY — a new diagnostic code is INERT until `manage.py seed_rules` runs** (do
it by hand, note it in the annex; Wednesday asks about build.sh); (2) **the
public mirror's PII guard trips on client SURNAMES — client numbers only in
STATUS / BUILD_ORDER / REVIEW_QUEUE** (it tripped twice this sitting).

**▶ THE s332 BATCH A / B LEDGER (item → result → commit):**
| # | item | result | delvio |
|---|---|---|---|
| A1 | 015 #3 Sch 2 line 8 without 5329 | 🏁 built; 4 of 5 landed | `9bcb61a3` |
| A2 | 015 #17 joint 2a aggregate / Sch B ownership | half built; ownership → Ken (REVIEW_QUEUE) | `acf6efe9` |
| A3 | 015 #1 lump-sum SS: D_1040_014 + the 6c box | 🏁 built; 4 of 8 landed | `ef62bf5e` |
| A4 | 015 #7 land without a convention (+ `LAND` token, `Autos`) | 🏁 built; 91/91 + 42/42 rows | `fabbcb5a` |
| A5 | 015 #18 per-row joint rounding | ✅ already built by 014 #2; pinned | `2099b6d0` |
| A6 | 015 #19 + 296 #60 Sch 2 line-13 source trio | 🏁 built; client 2662 corrected | `58c6e4fe` |
| A7 | 015 #8 GA 1099-G ownerless row | 🏁 built (owner from the federal row) | `cb10f530` |
| A8 | 296 #84 Form 8582 MAGI sign error | 🏁 built (taxable SS SUBTRACTED); 49 re-run, 0 movers | `60f07144` |
| A9 | 015 #13 8829 mortgage under the standard deduction | 🏁 built — on line 16 per i8829, not 10 | `7a58642d` |
| A10 | 015 #2 COD → GA RIE line 10 | ⛔ Ken — one line-10 question with 296 #85 | — |
| B1 | 296 #48 Form 4136 | ⛔ no RS spec (404) — the RS lane first | — |
| B4 | 015 #6 8606 line 25c → 5329 line 1 | 🏁 built; 6 filed re-run, 0 movers | `f90f2a05` |
| B5 | 015 #16 Form 8959 face reader | 🏁 built; 1 landed (client 4655) | `04de3540` |
| B6 | 015 #14 duplicate shells | 🏁 masked identity on the lookup; 13 all-blank groups → D-044 merge | `fd6b779a` |
| B7 | 296 #43 prior passive loss on a gain K-1 | half built; former-passive → Ken (REVIEW_QUEUE) | `7461da55` |
| B8 | 296 #20 Form 7206 >2% shareholder | ✅ already built in s272; fixture ties | — |
| B9 | 015 #12 Form 4952 face reader | half built; portfolio K-1 → DEFERRAL (13) | `3ffaa38d` |
| B10 | 015 #15 §168(k)(7) class election in the lane | 🏁 built; fixture ties | `cd3fb39a` |

Landed this sitting under the standing commit authorization: 4 (A1) + 4 (A3)
+ 1 (B5) returns, every one a TIE; one filed return corrected in place
(client 2662, A6). Deploys all Render-verified LIVE (the last:
`dep-dadrp5favr4c73alv6h0`).

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
1. **Batch C of the ranked forty** (the triage foot of the 015 file) —
   verify-first EACH: three of A/B's twenty were already built, two were
   the vendor's treatment, two were mis-read pages.
2. **The named TaxWise reader walls by measured count** — 6251 (6 sole, 17
   total; `amt_*` + `compute_6251` exist → reader-only) → the Form 4562 face
   (3; blocks the two farm registers of A4) → 1116 (5) → sched_line_detail
   (4) → the qualified-tips per-W-2 box-7 split (296 #60's sibling) → the
   portfolio K-1 consolidation (DEFERRAL (13)). Drive ONE single-wall packet
   by hand through each before trusting the census (s328).
3. **Ken's answers** (Wednesday below): the RIE line-10 definition unblocks
   A10 + 296 #85; the Schedule B ownership answer unblocks ~26 packets
   (A2(b)) and two of A3's residuals; the former-passive answer closes B7.
4. **The four fenced clients' filed flag** (2777 · 3630 · 4159 · 4160) and
   ruling 2's penalty acceptance (~10) + ruling 3 (client 3250).
5. **The Lacerte face readers by wall count** — Sch 3 (107) → A (37) → B (32)
   → C (18) (Sch 3's cause is known: Lacerte splits `5a` into `5` + `a`).
6. **⑥c `manage.py merge_client`** (D-044) — now with a first worklist: the
   13 all-blank duplicate-name groups (DEFERRAL (12)).
7. **The 1065 import** — 95 partnership returns behind it.

**⛔ WAITING ON KEN — WEDNESDAY AGENDA (s332):** the two season-2026 design
questions Ken raised (what replaces the answer key; how scanned documents get
in) — recommendations in REVIEW_QUEUE, shelf units in BUILD_ORDER.
**Also for Wednesday (s332, later):** a Form 4136 spec in Rule Studio (296 #48 is
STOPPED on its absence — the RS lane; the fixture's one fuel / one use bounds leg 1);
the RIE line-10 definition (015 #2 + 296 #85, one question, REVIEW_QUEUE); add
`manage.py seed_rules` after `migrate` in
`build.sh`? — the deploy migrates only, so every NEW diagnostic code is inert
until someone seeds it by hand (DEFERRAL_AUDIT (10)); the Schedule B ownership
question (A2(b), REVIEW_QUEUE); code L in `EARLY_CODES`; 296 #85 (the §108(f)(5)
other-income placement on the GA RIE worksheet — the four Georgia misses still
open on client 2490 after 296 #84's federal fix).

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
unwitnessed in this corpus.

⚠⚠ **ANOTHER CC ACCOUNT WRITES THIS LANE — THE SHARED FILES ARE THE ONLY
PLACE ITS WORK IS VISIBLE.** Do not order events by `updated_at`.
