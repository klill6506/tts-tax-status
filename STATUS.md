# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-24 (s283 — the entity-packet polish, Ken-directed:
our partnership packet vs the Lacerte-filed original).*

*⚠⚠ RESUME POINT — **s283 SHIPPED THE PACKET-POLISH UNIT** (`66d0d09`,
deploy verification in flight at close — CHECK RENDER STATUS FIRST if this
file is stale; the two morning deploys `d5fbea1` + `3134bf7` are LIVE and
Render-API-verified). Ken supplied the Lacerte-filed partnership packet for
client 3444 (numbers tie; the from-scratch entry was ChatGPT's) and asked
for the appearance/completeness gaps. Five mechanisms:*

*① **PREPARER PLUMBING — a two-writers/one-reader mismatch.** Every print
surface (1065/1120-S Paid Preparer block, letter letterhead + signature,
invoice firm header, GA-700 p2 preparer block) reads the `PreparerInfo`
SNAPSHOT; the import lane's `_assign_preparer` set only the FK — so a
lane-entered return showed a preparer in Return Manager and printed a blank
signature block everywhere. New `apps/returns/preparer_sync.py`: ONE sync
(viewset + lane) + `effective_preparer_block()` with roster-FK fallback,
consumed by all four surfaces.*

*② **NAME + EIN ON EVERY PAGE (the Lacerte convention).** Lacerte prints
them on all 29 pages; ours had ~12 of 35. `render_complete_return` now
stamps the top margin of every entity-packet page whose text+widgets lack
the EIN — self-maintaining (native faces never double-print; new forms join
automatically; letter/invoice exempt; **1040 packets deliberately excluded —
that stamp would be name+SSN, Ken's privacy call, not made**).*

*③ **THE SHARED-NAME ACROFORM DEFECT (acroform_filler).** `widget_positions`
was keyed by field name — a field with widgets on SEVERAL pages kept only
the LAST one, so GA-700's NAME_COPY/FEIN_COPY (one shared name, pages 2-8)
printed on one page and vanished from six. Now a positions LIST drawn at
every widget (the AcroForm semantic). ⚠ **The ga700 map's v1 note said
exactly this and I dismissed it as wrong — it blamed the TEMPLATE but was
right about the FILLER.** The s282 claims-lesson replayed same-day: verify
the component a claim names before ruling the claim wrong. GA-700 also
gained its p2 preparer block + continuation headers; 1065 p1 gained line I
(K-1 count), the discuss-Yes boxes, and the full firm address.*

*④ **1065 LETTER/INVOICE VOCABULARY** — 8879-PE / GA-8453 P (were
8879-CORP / GA-8453 S on partnerships), partner K-1 paragraph, partnership
forms list incl. Schedule B-1 (the packet's own gate) + GEORGIA FORMS
(Form 700 + 8453-P — the section used to vanish), client NUMBER, Lacerte
boxed sections, and ⚠ the Amount-Due derivation now beats a stored ZERO
total (the seeded blank printed "Preparation Fee $765 / Amount Due $0.00").*

*⑤ **DATA (client 3444, matches the filed face)**: preparer = the roster
entry the filed 1065 prints; entity activity; date business started;
invoice fee. **Evidence**: 9 new tests (stamp full-page audit; GA-700
every-page headers; lane sync; fallback; vocabulary; stored-zero total),
teeth by two injections (both red, both reverted); 195 render/filler/print
regressions + 34 preparer-adjacent green.*

*▶ NEXT (fresh session): verify the `66d0d09` deploy reached LIVE (poller
was running at close). Then the carried queue: ⛔ #3 entity transport
(Ken), batch items as posted. **NEW NAMED GAPS vs Lacerte, not built (Ken
to rank)**: (a) **Form 8879-PE render** — the packet has NO federal e-file
authorization page (the 1040 side has 8879; entities have none; grep
"neither built yet" in renderer); (b) **7004 in the packet assembly** (the
render exists, package="extension" — it just never joins the full packet);
(c) 1065 preparer signature/date cells (no AcroForm fields on the IRS
template — needs abs_pos literals if wanted).*

*⚠ **PROCESS FLAG FOR KEN (s283, against myself)**: the client's BUSINESS
NAME went into the `66d0d09` commit message (the repo is PRIVATE and the
public mirror carries only planning files — those are clean — but the queue
rule says commit messages carry synthetic identifiers only). History on
main is never rewritten, so it stands unless you direct otherwise; the rule
is re-noted here so the next session greps its commit message before
pushing.*

*▶ AWAITING (carried from s282): entry-lane token (Ken, in their session) →
client-4569 restage + the 40NR packet through the variant path; the
`GA_OCGA_48_7` authority-row ownership call (Ken) → unblocks the approved
GA-500 S3-9 seed; 1040 BATCH-012 / BATCH-296 stay in `CC Changes`.*

*⛔ KEN — outstanding (carried): the entity second-state-face transport
(#3, REVIEW_QUEUE); state-face override-honor convention
(`OVERRIDE_HONORED_STATE_LINES` now 2 members); client-2961 AL 40NR
source-defect disposition; the 146-packet re-export; NC/CA/SC linked-state
reopens; #8 GA-700 Sch 4; #6 1065X/AAR; #68 optimizer; s274 PII items; RS
8990 re-authoring gate; Form 6765 Section G; client-4545 D_8606_BASIS_ONLY;
per-rule cleanup acknowledgment; **1065 BATCH-004 #4 blocked on Codex's
Box-2 arithmetic**.*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names, no packet codenames, no SSNs/EFINs.

---

## ⚠⚠ STANDING FACT: PUSHING TO `main` DEPLOYS (WHEN BUILDS RUN)
Render auto-deploys from `main`: prod (prep.delviotax.com) = service
`delvio-tax` (srv-d6geloa4d50c73el2trg); demo = `tts-tax-demo`. ⚠ **A push
is not a deploy — CHECK THE DEPLOY STATUS after pushing** (API key in
`D:\dev\Passwords & Secrets\render-api-key.txt`). **Standing push
authorization (Ken 2026-08-23): push at own judgment; verify every deploy;
hold only for a named reason.**
⚠⚠ **ORDERING (s279/s282): push → deploy LIVE → seed → verify — and the
deploy ITSELF seeds: `build.sh seed_all` auto-discovers every `seed_*`
command at BUILD time.** A new seeder goes live WITH its deploy; the manual
post-deploy seed is the idempotent VERIFY; `check_rule_paths` is one
command.

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist
to find defects. State the finding and move on.

## ▶ SINGLE-CHANNEL RULE — STANDING (Ken, 2026-08-22)
**The build session MANAGES the sibling Claude sessions.** ListAgents +
SendMessage — ⚠ the channel DROPPED a morning both ways (s277): anything
load-bearing goes in batch-file annexes too. **Never relay tokens through
the message channel.** ONE delvio-tax tree holder; ONE pytest/test_postgres
holder (⚠ s283 hit this again: two pytest runs collided on test_postgres —
wait for the background batch before launching another). Peers stage; Ken
decides.

## ⚠ Known red / rotted — THE ONE LIST (post-s283)
- **The quintet** (s225/s258 files) — seed_builtin_rules leakage;
  `--create-db` = reset AND non-implication proof.
- **`test_1040.py` — 6 pipeline tests** (s234, reuse-db only) ·
  **`test_mappings.py` — 7 setup ERRORS** (s239) · `test_4868.py` (4, ⛔ KEN s217).
- ⚠ Re-diagnose before inheriting (the s281 topic7 lesson).
- **Client typecheck**: green (s281). No client changes s282/s283.

### ⚠ Test-run hazards (standing)
🌐 = campaign-wide · 🔧 = this repo only (scope marking, s281).
- 🔧 One shared `test_postgres` (RS suite included). Long runs DETACHED;
  never pipe pytest through `Select-Object`; `poetry run` only from
  `server\` (the Bash tool prints NOTHING for poetry one-liners — use the
  PowerShell tool).
- 🌐 ⚠⚠ PS5.1 encoding traps: regex-replace file rewrites BANNED; Edit tool
  or `[IO.File]` BOM-less UTF8; grep the DIFF for mojibake after any shell
  touch. ⚠ Embedded double-quote in a here-string arg to a NATIVE exe
  SPLITS the argument — long native args via a FILE (`git commit -F`).
- 🌐 ⚠⚠ `Measure-Object -Line` skips blank lines — `ReadAllLines().Count`.
- 🌐 ⚠⚠ A bare HTTP 400 (no `error` body) = the body never parsed —
  suspect ENCODING (UTF8.GetBytes + charset).
- 🌐 Staging answers 201 even for an invalid payload — the verdict is
  `row["status"]`; return CRUD routes carry the trailing slash.
- 🔧 ⚠ pytest-randomly NOT installed (s281). Test order is not randomized.
- 🌐 ⚠ (s283) **pymupdf `get_text()` does NOT reliably surface AcroForm
  widget VALUES** — a filled-forms coverage scan must also read
  `page.widgets()` or it under-reports (my first name/EIN scan called
  GA-700 pages empty when page 1 was filled).

## 🔎 Carried for triage — NOT claims
- (s283) **Entity packets have NO federal e-file authorization page**
  (8879-PE unbuilt, 7004 not in the packet assembly) — named gaps above,
  Ken to rank.
- (s283) The stamp excludes 1040 packets (name+SSN privacy call — Ken).
- (s282) `OVERRIDE_HONORED_STATE_LINES` (AL40-12/AL_40NR-14) hand-synced
  with compute; third member ⇒ derive both sides from one declaration.
- (s281) Out-of-scope-state line-18 prompt-shaped diagnostic — specified,
  not built. · Stage allowlists `schd_fields` keys, `ga500_fields` not at
  all (commit refuses atomically). · `D_8812_015` fires on 1 live row —
  CORRECT, not a regression. · ⭐ evidence is only about what it could have
  observed; a sweep reports its own coverage.
- (s280) regression fixtures insensitive to their rule — Gate-1 approved
  2026-08-24, execution with the states lane.
- (s279 late) cleanup `source_verified` all-or-nothing; client-4545 ⛔ KEN.
- (s268) 1,604 queries/run + memoization candidates.
- (s241/s281) `Form8606` unique-constraint candidate (DB clean; Ken's
  call). 🔴 `HSAAccount` half CLOSED — multiple HSAs legitimate.
- (s275/s281) `.first()`-on-per-form-rules sweep — narrowed (62/71 seeders
  guarded); remainder: multi-instance forms + 9 named out-of-reach seeders.

## ⛔ KEN DECISIONS OUTSTANDING — carried (see STATUS_ARCHIVE for detail)
- Form 6765 Section G (TY2026+) · 1040 v5.4 business rules · 1120-S
  Inbox: 180 / 214 / pre-incorporation trailer · 17a / 17d.

## RS AGENDA — carried:
Everything from s277–s282 stands: the AL_FORM_40NR diagnostic-condition
amendments (undeclared fact / engine-output condition); the GA-500 S3-9
seed BLOCKED on `GA_OCGA_48_7` ownership (D-31 member four, live);
R-AL-TAX mechanism amendment staged; D-36 reads for TN_FAE170 / SC1120 in
progress on the states lane. Nothing new from s283 (render-only; no spec
touched — the RS gate's spec-fetch was skipped WITH REASON: no computation
or line-mapping change).
