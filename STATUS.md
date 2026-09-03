# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s328, 2026-09-03

**▶ THE LACERTE-LAYOUT EXTRACTOR PASS, LEG 2, SHIPPED (`26e70113` +
`c2a3276b`; 276 green across the four extractor suites).** BUILD_ORDER ⑥
stays OPEN. Leg 2 = the Federal Worksheets document readers
(`lacerte1040/worksheets.py`), the GA 500 income-statement block
(`ga500_stmts.py`), the emit assembly with the TaxWise face identities
mirrored, and the Schedule 1-A logic factored into
`taxwise1040/sch_1a_emit.py` so both vendors share it.

- **Lacerte prints no W-2 / 1099 facsimiles.** The documents live in the
  Federal Worksheets tables: Wage Schedule → `w2s` (no EIN / box 3 / 5 /
  12 printed); Pension + IRA schedules → `r_1099s` (no code; a blank
  Taxable is 0 — the Grand Total proves it; `***` = 8606-computed →
  refuse by name); Interest / Dividend lists → `int_1099s` / `div_1099s`
  (printed ONLY when no Schedule B is filed — 126 Sch-B packets ∩ 0
  lists); Pub 915 line 1 → `ssa_box5_net_benefits` (joint total on the
  taxpayer — no per-owner box 5 exists, no lever); SSA withholding = 25b
  − Σ 1099-R W/H. Payer FEIN + GA withholding come ONLY from the GA 500
  INCOME STATEMENT DETAILS block (statements A–F, type by the X row),
  matched to documents by GA withholding (+ income on a tie).
- **Owners on MFJ lists:** the MFJ-vs-MFS comparison page (per-spouse
  income columns; joint = half each) → the s326 RIE rule → joint if no
  spouse is RIE-age → refuse by name.
- **Three leg-1 defects found by driving #1313 through by hand:** Lacerte
  prints every X ~10pt LEFT of its label — p2 aged/blind `born` anchor,
  the 12a claimed boxes, the digital-asset Yes/No (#1313's "No" had read
  as "Yes"). Parametrized in `taxwise1040/f1040.py`, pinned in
  `lacerte1040/layout.py` on a corpus census (Yes-X 491 ×9, No-X 528
  ×245, aged 167 ×118, You-claimed 216 ×2; blind / spouse-claimed
  windows extrapolated — no witness yet). Sch 1-A: only the subline
  gutter differs (`SCH1A_SUB_X`).
- **Verified against the entry lane's answer keys:** #1313, #1522,
  #1570 match section for section (w2s / r_1099s / int / div /
  dependents / 8867 / car loans / taxpayer fields / expected). Only
  differences: Lacerte rows carry MORE (payer_ein, state_id_number,
  state_distribution); the entry lane hand-keyed `ga500_fields["5"]`
  (GA filing-status letter — neither pipeline emits it) and expected
  zeros on 36/37/38 (the TaxWise pipeline omits them too).
- **Runs:** `lacerte-s328` (before the Sch 1-A / X fixes): 255 refused,
  0 emitted, refusal census = leg 1's (the coverage gate refuses
  upstream — a corpus run measures nothing about a reader it never
  reaches). `lacerte-s328b` (fixed code, log
  `tmp\lacerte_extract_s328b.txt`, batch `lc328b-001.batch.json`): **255
  packets → 3 EMITTED (#1522, #1570, #3251), 252 refused.**
  Single-wall packets (the cheapest unlocks): Sch B 2 · Sch 2 1 · 8606 1
  · Federal Statements 1 · Maryland 1 · one non-Lacerte packet · one
  shell already filled (1313) · **the packet named in tmp/s328_ken_questions.md: the GA 500 page 1 lists a
  DIFFERENT primary SSN (…7699) with the 1040's filer (…5844) as the
  spouse — a swapped-spouse or mismatched GA return; ⛔ KEN.** Walls per
  packet: 9 single · 10 double · 8 triple · the median ~9. ⚠⚠ **s328b
  ran on the STALE s327 shells index; the index was regenerated from the
  DB at the end of the session (2,978 shells; 1522 / 1570 are FILED with
  documents by the entry lane, so #1522 / #1570 are duplicates of
  hand-keyed landings).** `tmp\commit_s328_lacerte.py` re-fences every
  emit against the fresh index (skips filled / non-draft shells by name)
  and runs the standing-authorization contract; `--dry-run` rolls back
  everything. **🏁 THE FIRST LACERTE-PIPELINE LANDING: #3251 (client
  3251) — dry-run TIE, then COMMITTED under the standing authorization
  (batch `s328-lacerte-commit-001`, `tmp\commit_s328_lacerte.txt`);
  verified by the DATA: 2025 1040 FILED with its W-2 row, GA-500 FILED.**
  #1522 / #1570 correctly FENCED (filed shells).
- **The wall list is unchanged by design** (faces gate first): Sch 1 181
  · 8995 172 · Sch D 131 · Sch B 126 · Sch 2 119 · Sch 3 113 · 8949 109
  · 1116 95 · 4562 84 · Sch A 76 · Sch 1-A 75 (now READ) · Sch C 75 ·
  Sch E p2 75 · GA 4562 72 · 8582 65 · Sch E 63 · 7203 62 ….

⚠⚠ **PII CATCH (ninth, s328 close — the mirror guard tripped on a packet
CODE):** Lacerte packet codes ARE surnames. Every code this session wrote
into repo / planning / memory files is now a client NUMBER (map:
`1040\tmp\s328_code_to_client.json`; Ken-facing codes only in
`1040\tmp\s328_ken_questions.md`). Found on the way: the s327 fixture in
`server/tests/test_lacerte1040.py` carried a REAL couple's names + both
SSNs as "synthetic" words (now synthetic), and three s328 commit
messages carry surname codes (`26e70113`, `c2a3276b`, `62c76c9b`) — the
tree is clean, the HISTORY is not → the supervised rewrite is SCHEDULED
(Ken's go 2026-09-03; NEXT ⓪; REVIEW_QUEUE "REWRITE #2"). s327's layout.py comments
still carry codes (HITTC/CALVO… → now numbers too). **Rule going
forward: never write a Lacerte packet code outside tax-test-data.**

**▶ NEXT:** ⓪ **THE SECOND PII HISTORY REWRITE — Ken's go 2026-09-03 ("Sure you can schedule the rewrite"; DECISIONS). Run it FIRST, in a dedicated sitting with Ken present and every peer lane off the tree, per the s297 recipe in REVIEW_QUEUE (fresh mirror clone → `git filter-repo` → force-push → commit map under docs/history → repair hash citations); scope = `server/tests/test_lacerte1040.py`'s pre-`7dc2d044` versions (real names + SSNs) and the messages of `26e70113` / `c2a3276b` / `62c76c9b` (surname packet codes).** ① at boot: read `tmp\commit_s328_uncommitted.txt`'s GRAND
SUMMARY (the overnight job); re-extract any GA-RIE hold from its TaxWise
packet. ② The face
readers by wall count through the TaxWise parsers with measured Lacerte
bands (Sch 1 → Sch B → Sch 2/3 → Sch D+8949 → Sch A → Sch C; 8995 via
`taxwise1040/f8995.py`) — for each, find ONE packet whose only wall is
that face and drive it through by hand before trusting the census (the
s328 lesson). ③ the shadow-2210 reader (face line 38).

**▶ BOOT TASK RUN THIS SESSION — the "58 never committed" payloads:**
audited by the DATA (`tmp\s328_uncommitted_audit.json`: 242 payloads in
r51 / gail-s325 / georgianna-g2 / g4 / shellfix-rerun; **59 sit on a
2025 shell with ZERO document rows**), then the tie-gated commit
(`tmp\commit_s328_uncommitted.py` → `.txt`, batch
`s328-uncommitted-commit-001`, keys suffixed `-s328`; one transaction
per return, tie commits + bookkeeping, else rollback) was LAUNCHED and
runs ~4 min/return (~4 h) — **read its GRAND SUMMARY at boot**. At close:
13 processed, 8 tie/landed, 4 no_tie held — clients 1938 and 3680 are
the §6654 penalty shape (federal 37/38 deltas, Ken's class); client
3815 is the s325 GA RIE family (S1-7 / RIE-TP-17 expected 11,038 vs
8,596) — ⚠ these r51 / early-book payloads PREDATE the s326 owner-witness
fix, so a GA RIE hold here means RE-EXTRACT the TaxWise packet with
current code, never commit the stale payload.

**▶ KEN'S 6252 RULING (relayed by the entity lane as an OPTION SELECTION,
2026-09-02 night; DECISIONS "Form 6252 line 19"):** round the gross
profit percentage to four decimals as Lacerte prints. Verified: the 2025
instructions say "rounded to at least 4 digits" — a FLOOR, both methods
comply, a permitted vendor accommodation Ken chose for dollar
reconciliation (not an engine error). DEFERRAL_AUDIT (17) ruled; build
stays in the auto-dealer unit (rounding MODE = a convention to pin on a
witness). #3773 was closed out FILED by the entity lane on Ken's
"file as is and note the credit"; the shareholder 1040 (batch ee7d11b9,
key i-skoglund-4000-d) still waits on the auto-dealer unit (items 2–4).

**▶ KEN'S RULING, 2026-09-02 (s327 sitting) — A TIE IS A FILED RETURN, and
the Filed count is the practice's true count.** `commit_staged_return`
marks filed on a tie (federal + attached states). **Federal 1040 Filed
1,218 of 2,978 at the s327 close (all tie-verified); GA-500 under filed
federals 1,147 filed / 37 draft.** ⛔ ENTRY LANE: 37 GA-500s under
hand-filed federals were never tie-verified and stay Draft — clients
1019 1033 1034 1035 1056 1075 1076 1081 1089 1090 1094 1136 1164 1165
1215 1216 1218 1254 1259 1262 1273 1274 1281 1307 1315 1367 1372 1411
1587 1588 1600 1601 1609 1842 1857 1858 1891 — verify the GA face and
mark, or record why no GA return applies. 8 returns are filed with zero
document rows (clients 1400 1106 1200 1974 4036 3878 1598 3264) —
verify one before assuming SSA-only/hand-keyed.

**▶ ENTITY LANE (other account):** five Skoglund S-corps FILED (#1153,
#2920, #3103, #3773, #4460); 1065: 2 of 69 filed, 66 packets in
`1065\Inbox` — the partnership import is the next entity job (BUILD_ORDER
Ken-directed block). Carried entity findings: DEFERRAL_AUDIT (9)–(18).

**▶ NEW SUITE MODULE — delvio-research:** doors built
(`apps/suiteapi/research.py`); **⛔ KEN: set `RESEARCH_SERVICE_TOKEN` on
BOTH Render services** — both doors are inert (503) until it exists.

**▶ DISREGARDED ENTITY TYPES + THREE CLIENT-RECORD RULINGS (s327):** built
and live (`clients.0015`/`0016`); follow-ups queued in BUILD_ORDER
(owner field on disregarded entities; the due-date calendar).

**▶ CLIENT-BASE RULINGS relayed to the CRM (s327 night):** done; still
open: one bookkeeping client's monthly-vs-quarterly.

⚠⚠ **ANOTHER CC ACCOUNT WRITES THIS LANE — THE SHARED FILES ARE THE ONLY
PLACE ITS WORK IS VISIBLE.** Do not order events by `updated_at`.

**▶ BUILD QUEUE after the Lacerte legs:** ② the 3-char
`distribution_codes` model gap (migration; 4 packets since s324) · ③ the
TaxWise extractor walls by measured count (f6251 = 13 ·
sched_line_detail = 6+ · f5329 · the classifier patch, GA part-year
detector first) · ④ the three re-raised Lacerte engine holds (clients
1922, 2386, 3517) · ⑤ the GA 7b military-exclusion engine leg (7
witnesses; waits on the states-lane spec export) + the 7c/7f DIS
transcription · ⑦ the §6654 family decision (Ken) · ⑧ BATCH-013 (posted,
UNWORKED, 10 Tom-lane product gaps; ⚠ item 5's premise is refuted —
Schedule C has no business_address on the MODEL, a migration not a sync)
· carried: the 8615 parent-first guard · out_of_scope_states · the
zero-activity GA-attach gap.

**⛔ WAITING ON KEN:** may the Lacerte pipeline file a packet whose
manifest carries a non-Georgia state return federal + GA only (47
packets refuse on it — states on hold)? · the packet named in tmp/s328_ken_questions.md (Lacerte): the GA 500
page 1 names a different primary filer (SSN …7699) with the 1040's
filer (…5844) as the spouse — swapped spouses on the GA return, or the
wrong GA return in the packet? · the §6654 family (…0500/…0534/…7701/
…7044 + clients 1938 and 3680 tonight — line 37/38 penalty deltas) · seed ONE client (the
…4641 taxpayer in Jenny's book; ⚠ do NOT edit #4054) · client #3572's
contaminated name (#4514 same shape) · three clients with no 2025 1040
shell · does the standing commit authorization extend to ENTRY-LANE
HAND-KEYED commits? (client 3250 staged, dry-run TIE) · client-2149's
filed GA 17a = 2 exemptions on a single/zero-dependent return · the 61
Gail federal reprints (`tmp\GAIL-TRIAGE-2026-08-31.md`) · the …4203
W-code question · one Georgianna reprint + five named reprints · the
asset METHOD-DERIVATION TABLE review (annex) · vendor-name allowlist for
the mirror guard? · carried: 1071 · 1141 · R-GA500-RIE · 4059 W-2G
address · Sch D carryover · GA RIE L10 · 4081's $169 · standing 1–8 · 2a
scope flag · AL 40 · the 4 Tom-book holds (…8505 · …2276 · …2827 · …8791).

**▶ OFF-SPINE, SHIPPED 2026-09-01 (Bob lane):** `GET
/api/v1/suite/clients/by-phone/` (`apps/suiteapi/caller.py`, 8 tests).
