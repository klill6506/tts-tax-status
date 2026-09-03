# TTS Tax App — STATUS (current state only)

## ▶▶ RESUME POINTER — s329, 2026-09-02 night (the s328 close was mislabeled 09-03; the clock says 09-02)

**🏁 THE SECOND PII HISTORY REWRITE IS EXECUTED (Ken's go, narrow scope,
2026-09-02 night).** `main` was force-pushed once from a filtered mirror
clone: `8f87ed1b` → **`75de3a2d`**, 2,720 commits, 902 SHAs changed, first
changed commit dated 2026-03-22. All four gates passed BEFORE the push:
V1 tip TREE hash identical (`15a2a45c…`) · V2 commit count unchanged ·
V3 zero residual hits across all 11,593 reachable text blobs and every
commit message **with a positive control — the same 41 patterns hit the
untouched backup 41/41 in blobs and 11/11 in messages** · V4 commit map.

- **Scope (Ken: "push the narrow scope only for now"):** the s327 test
  fixture's real identifiers (2 SSNs + 5 name/street words) and **17
  Lacerte packet codes** that lived in history and were absent at tip —
  each now the client number, or `[client]` where no number is known.
- **⏳ DEFERRED to a later rewrite (Ken's "for now"):** ~86 TaxWise packet
  surnames that survive only in old `STATUS.md` / `STATUS_ARCHIVE.md`
  blobs (every one was fixed at tip when the mirror guard caught it; only
  history still carries them), plus the s297 tier-3 tokens present at tip.
  Tracked in REVIEW_QUEUE as **REWRITE #3**.
- **Two live tip catches were scrubbed by ordinary commits first** (a
  mapped token must be absent at tip or the filter rewrites current code):
  a packet code in four files (`67d5ac25`) and a client surname in the Form
  8962 BATCH-296 item-9 test docstring (`21ea4025`) — the tenth PII catch.
- **Backup mirror (KEEP):** `D:\tax-test-data\repo-backups\delvio-tax-pre-rewrite-2026-09-02.git`
  holds the full pre-rewrite history at `8f87ed1b`. Destroying it is a
  separate Ken call, as with the s297d backup.
- **Old→new SHA map:** `docs/history/rewrite-2026-09-02-commit-map.txt`
  (2,721 rows). 180 hash citations across STATUS / archives / DECISIONS /
  REVIEW_QUEUE / BUILD_ORDER / the auto-memory were repaired from it, none
  ambiguous. **⚠⚠ EVERY delvio-tax SHA quoted before 2026-09-02 night is
  STALE — resolve it through the map, never from memory.**
- **⛔ PEER LANES MUST RE-CLONE** (the other account, the entity/entry
  lanes, Codex). A lane that pulls onto old history will re-introduce it.
- The public `tts-tax-status` mirror never carried these values and was
  NOT rewritten; its SHAs stand.

**▶ BOOT TASK ① DONE — the "59 never committed" overnight job finished
(`tmp\commit_s328_uncommitted.txt`, batch `s328-uncommitted-commit-001`):
31 landed · 7 fenced (already carried rows — correct) · 17 no_tie · 4
staging errors.** Then the holds were RE-EXTRACTED with current code (the
stale-payload rule) — `tmp\s328-rie-refix-src` → `PipelineOut\s328-rie-refix`
→ `tmp\commit_s328_rie_refix.py` (batch `s328-rie-refix-commit-001`): **11
GA holds → 9 emitted → 8 TIE and LANDED** (clients 3815 · 3825 — the
11,038-vs-8,596 RIE case — · 4419 · 2019 · 1794 · 2228 · 4751 · 1810;
verified by the data: federal filed + GA-500 filed; six carry document
rows and **clients 2228 and 4419 correctly carry none — both are sole
proprietors whose only income is Schedule C** (AGI 3,833 / 15,517, SE tax
582 / 2,447), so zero document rows is the right shape, not a coverage
gap), **1 real hold: client 4081** (GA S1-7 / RIE-TP-17 43,756 vs
43,925 — the carried "$169" item, still Ken's), **2 refused by name:
clients 4429 and 3871** (GA 7b unborn-dependent exemption — the s323
class, an ENGINE leg nobody has built; witnesses now 8). Still held from
the overnight job, by class: **§6654 federal 37/38 penalty deltas —
clients 1938 · 3680 · 1219 · 3514 · 2774 · 4093** (Ken's family, six more
witnesses) · **`r_1099s.distribution_codes` 3-char — clients 2793 · 3010 ·
3160 · 4589** (build queue ②, now 8 packets).

**▶ FRESH DB CENSUS AT THE s329 CLOSE (a count is a timestamp — re-run it,
never quote this one later):** 2025 federal 1040 shells **2,978 — filed
1,258** (was 1,218 at the s327 close: +40 tonight), draft 1,716, in
progress 4. GA-500 under a filed federal: 1,224 — **1,187 filed, 37 draft**
(the same 37 the entry lane owes a face check, listed below).

**▶ BATCH-296 items 54–59 triaged by the DATA (annex appended to the
file):** 54 / 56 / 58 / 59 are FILED (58 + 59 = the s326 owner-witness
family); **55 (client 4502) and 57 (client 4547) stay DRAFT — re-extracted
tonight, both now REFUSE BY NAME ("joint return with ownerless
documents")** — the s326 rule refusing to guess an owner; they need an
owner witness or hand-keying. No build in 54–59. `/bugs`: no open reports.

**▶ NEXT (after Ken's rewrite decision):** ② the 3-char
`distribution_codes` model gap (migration; 8 packets) · ③ the Lacerte face
readers by wall count (Sch 1 → Sch B → Sch 2/3 → Sch D+8949 → Sch A → Sch
C; 8995 via `taxwise1040/f8995.py`) — for each, drive ONE packet whose
only wall is that face before trusting the census · ④ the shadow-2210
reader (face line 38) · ⑤ the GA 7b unborn-dependent engine leg (8
witnesses; spec with the states lane).

**▶ THE LACERTE-LAYOUT EXTRACTOR PASS (BUILD_ORDER ⑥, OPEN) — state at
the s328 close, unchanged tonight:** leg 2 shipped (`5173e51f` +
`c5101c8c`; 276 green across the four extractor suites): the Federal
Worksheets document readers (`lacerte1040/worksheets.py`), the GA 500
income-statement block (`ga500_stmts.py`), the shared Schedule 1-A emitter
(`taxwise1040/sch_1a_emit.py`). Lacerte prints no W-2 / 1099 facsimiles —
Wage Schedule → `w2s`; Pension + IRA schedules → `r_1099s` (blank Taxable
= 0; `***` = 8606-computed → refuse); Interest / Dividend lists →
`int_1099s` / `div_1099s` (printed ONLY when no Sch B is filed); Pub 915
line 1 → `ssa_box5_net_benefits`; payer FEIN + GA withholding ONLY from
the GA 500 INCOME STATEMENT DETAILS block. Owners on MFJ: the MFJ-vs-MFS
comparison page → the s326 RIE rule → refuse. Lacerte prints every X
~10pt LEFT of its label (pinned on a corpus census in
`lacerte1040/layout.py`). The s328b corpus run: 255 packets → 3 emitted,
252 refused; **🏁 client 3251 = the first Lacerte-pipeline landing** (tie,
committed, data-verified); #1522 / #1570 fenced (filed by the entry lane).
Wall list (faces gate first): Sch 1 181 · 8995 172 · Sch D 131 · Sch B
126 · Sch 2 119 · Sch 3 113 · 8949 109 · 1116 95 · 4562 84 · Sch A 76 ·
Sch 1-A 75 (READ) · Sch C 75 · Sch E p2 75 · GA 4562 72 · 8582 65 · Sch E
63 · 7203 62 …. Single-wall packets: Sch B 2 · Sch 2 1 · 8606 1 · Federal
Statements 1 · Maryland 1 · one non-Lacerte · one filled shell · the
packet in `tmp/s328_ken_questions.md` (GA 500 p1 names a different primary
SSN — ⛔ KEN). **Rule: never write a Lacerte packet code outside
tax-test-data** (codes ARE surnames; map `1040\tmp\s328_code_to_client.json`).

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
