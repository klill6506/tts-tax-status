
# Form Coverage Tracker — tts-tax-app

> **2026-08-13 session 259 — RAW-MUTATION SWEEP LEG 1 ✅ (`a8dcaaf`,
> client-only).** Schedule D's row PATCH enlaned + verdict-returning;
> the Slate add guarded (a failed add used to vanish the typed row).
> ⚠⚠ PayerTable's commitSlim DROPPED every verdict — s253's per-cell
> overlays were unreachable on ALL PayerTable screens; now threaded.
> Verified live: lane blocks on failure, edits queue value-kept,
> idempotent retry (no duplicates). 2 tests + 1698 client green.
>
> **2026-08-13 session 258 — THE OUT-OF-SCOPE-STATE NAMED HOLD ✅
> (`0efc148`, no migration).** Backentry gains the `out_of_scope_states`
> payload key (validated; GA refuses — in scope): the staged summary
> echoes the codes and the cleanup closeout records "federal complete;
> CA → the paper preparer" — NEVER a hold. The NZ file's #10
> (multi-state) now has a staging path; non-GA computes stay with the
> paper preparer by name. Ken's 00:45 ruled backlog is fully shipped.
> 8 tests + 623 neighbors green.
>
> **2026-08-13 session 257 — FORM 3800: THE §38(c)(6)(A) MFS THRESHOLD ✅
> (`cb86103`, mig 0321).** Line 13's $25,000 → $12,500 on MFS, with the
> statute's exception built (spouse-has-NO-credit restores $25,000 —
> preparer-asserted `f3800_mfs_spouse_has_credit`, nullable; unanswered
> = $12,500 + D_3800_010, fires only when line 12 > $12,500). One reader
> feeds compute + diagnostic. RS spec amended same pass
> (R-3800-MFS-THRESHOLD verbatim + key excerpt; cache re-verified).
> 13 tests; all four 3800 suites + 526 FAs green.
>
> **2026-08-13 session 256 — FORM 172 LEG 3 ✅: THE NOL UNIT IS
> COMPLETE (`43768c7`, no migration).** ONE data source
> (`form_172_statement_data`, persisted rows) feeds the printed 1040's
> line-8a computation statement page AND MeF's
> NOLCarryforwardDedStatement (IND-368/369 by construction; 8a
> transmits POSITIVE — the XSD is non-negative, the line-9 math
> subtracts; the face keeps the negative). Keyed-8a-no-detail refuses
> e-file by name. ⚠⚠ **HOTFIX in the same deploy: s255's lane missed
> SINGLETON_SECTIONS — every backentry commit 500'd `ca6202c`→now;**
> invariant test added. D_172_80PCT_STATEMENT → info. *Legs:* ✅
> deduction · ✅ generation · ✅ pools lane · ✅ worksheet · ✅ statement
> page · ✅ MeF seam. *Deferred (recorded):* the official Form 172 FACE
> PDF render; D_172_MARITAL_SPLIT (no loss-year filing-status fact).
>
> **2026-08-13 session 255 — FORM 172 LEG 2 ✅: PART I GENERATION
> (`ca6202c`; migs 0319/0320 — Form172 OneToOne + RLS).** Lines 1-24
> off the face (spec T1-T4/T11 pinned; a real 50,000 Sch C loss year →
> line 24 = −50,000); the loss-year vintage opens engine-managed
> (+§461(l) EBL), clears on recompute-to-no-loss, REFUSES on a claimed
> farming carryback; `form_172s` lane + /form-172/ singleton; 4
> diagnostics; D_CFWD_003 carve-out. **The NOL lifecycle is CLOSED
> (generate → deduct → roll → expire).** *Legs:* ✅ deduction · ✅
> generation · ✅ pools lane · ✅ worksheet · ❌ statement page · ❌ MeF
> seam.

> **2026-08-13 session 254 — FORM 172 LEG 1 ✅: THE NOL DEDUCTION
> COMPUTES (`baa5c49`, no migration).** The §172(a)(2) two-tier engine
> off the nol_regular vintage pools (pre-2018 uncapped; post-2017 at 80%
> of the base AFTER pre-2018 reduces it — spec T7 pinned pure + through
> the return); Schedule 1 8a derives NEGATIVE with engagement memory;
> oldest-first MTI absorption writes used/remaining back; **D_CFWD_001
> retires for nol_regular** (nol_amt keeps its red — Ken ruling #3); the
> 20-year expiry fence; 4 diagnostics; FA-1040-NOL-01..05 staged.
> *Legs:* ✅ deduction compute · ✅ pools lane (s235) · ✅ worksheet seed ·
> ❌ Part I generation · ❌ farming-carryback refusal (lane fact) ·
> ❌ statement page · ❌ MeF seam. BATCH-001 #4's park LIFTED.

> **2026-08-13 session 253b — FORM_172 (NOLs) SPEC LEG ✅: authored,
> Gate-1 APPROVED by Ken in-session, seeded (RS 136 forms), exported
> (200), cached (`form_172_spec.json`, verified).** Ken's four rulings
> (DECISIONS.md): BOTH SIDES v1; farming carryback refuses by name;
> ATNOLD preserve-only; the 80% base verbatim — ⚠⚠ incl. the pre-2018
> subtraction the brief's short form dropped (spec T7 pins cap 16,000
> not 40,000). 37 facts / 15 rules all cited / 35 lines / 10 diagnostics
> / 11 scenarios (T9 = the IRS's own EBL example) / FA-1040-NOL-01..05.
> Integrity gate: no shared math; 6-defect negative control all caught.
> **The 404-STOP lifts for NOLs; the app build (legs: compute → 8a
> derive → Part I → diagnostics → statement → FA wiring → MeF seam) is
> NEXT — it retires D_CFWD_001 for `nol_regular` and unblocks BATCH-001
> #4 + BATCH-002's NOL computes.**

> **2026-08-12 session 253 — GUARDED-MUTATION LEG 2: PER-FIELD SAVE
> STATES (BATCH-006 #3/#5 → ✅✅ THE BATCH COMPLETES, all ten, moved to
> Done; `77d7950`, no migration).** Not a form unit: FieldStateInput
> renders is-saving/is-savefailed from its commit's returned verdict
> (latest-commit-wins; new edit supersedes; value kept on failure);
> useTaxpayerFacts.commit resolves per-field verdicts (partial-credit
> honored); W-2 boxes + nested rows return lane verdicts; the 1099-R
> per-field PATCH joins a per-row lane (was: no ok check). Verified
> live through the local 30s abort. Next: the ~95-site raw-mutation
> sweep. No form legs affected.

> **2026-08-12 session 252 — THE GUARDED-MUTATION BUILD LEG 1
> (BATCH-006 #2/#4; `ac04c1b`; client + one view decorator, no
> migration).** Not a form unit: `lib/recordSaves.ts` promotes the W-2
> save-lifecycle archetype to a shared hook (lane + idempotency key per
> intent + lane-derived error + add-doubles-as-retry); dependents
> type-to-add, both rental adds, and Start Return ×2 migrated;
> rental-properties joins @idempotent_create server-side; api.ts routes
> the friendly timeout message into data.error. Verified live: pending →
> visible timeout error with the typed name kept → aborted-but-completed
> create left exactly one row. Leg 2 next: per-field save states (#3) +
> the taxpayer header (#5). No form legs affected.

> **2026-08-12 session 251 — BATCH-006 #2-#5 INVESTIGATION (no code
> change by design; the annex is the spec).** Not a form unit: the four
> UI save-reliability reports reduce to ~103 raw mutation call sites vs
> the s119 saveScope lifecycle only the W-2 archetype rides. Verified
> live: 30s client aborts complete server-side (delayed-duplicate
> window), naked creates fail silently (`DependentsSection.handleAdd`),
> native alert() error surfaces are invisible to automation. The
> guarded-mutation build is next (client-only; no form legs affected).

> **2026-08-12 session 250 — FORM 8959 MULTI-W-2 AGGREGATE ENGAGEMENT
> (BATCH-006 #10; `addc5d7`; mig 0318 additive).** `Taxpayer.
> amt_8959_filed` TRANSCRIBES the packet's filed Form 8959 and engages
> the form when no Who-Must-File arm is evaluable from aggregate-only
> multi-W-2 data (the s227 identity-valve representation class;
> substance-gated on line 4 or 19 nonzero; never a per-employer
> allocation). Bridge-gate repair: 8959 sourcing + engagement now live
> in shared helpers (`resolve_8959_medicare_wages` /
> `resolve_8959_max_single_w2` / `form_8959_engagement`) used by compute
> AND the D_8959_* rules — the rules had been blind to BOTH valves on
> aggregate-sourced returns; D_8959_001 gained a tax-actually-due gate.
> Staging warning points at the remedy; batch-import schema regenerated.
> 11 regressions (`test_batch006_item10_8959_agg.py`). Form 8959 legs
> unchanged (all green since the Topic-8 build) — this is an engagement
> refinement, not a new leg.

> **2026-08-11 session 242t — THE FILED K-1 SPLIT (BATCH-003 #3;
> `823f60a`; mig 0307) — ✅✅ BATCH-003 CLOSES 10/10.** Two nullable
> ScheduleK1 fields (both-or-neither) supersede the single
> material_participation bucket and print BOTH Schedule E columns on one
> row; sum must equal boxes 1+2+3 (both-ends guarded); the MAGI
> partition holds by construction; split rows bypass basis caps and stay
> off the 8582 engine (D_K1_SPLIT_8582 warns on coexisting passive
> losses). ⚠ v1 boundary: a passive-LOSS component refuses (per-component
> 8582 allocation unbuilt). The 1040 lane has now closed batches 003,
> 004 and 005 — thirty items — in three days.

> **2026-08-11 session 242q — FORM 8814 (PARENTS' ELECTION) LEG 1: model +
> compute + all four feeds + lane (BATCH-003 #6; `d2d40fa`; migs
> 0305+0306).** RS export = draft-trap 5th (real rules, ZERO form_lines);
> built from the 2025 face + i8814 live. Feeds via each line's single
> source: L9→3a+3b, L10→the shared 2a source, L12→the composed 8z (5th
> contributor), L15→line 16. Unmatched child REFUSES; ≥$13,500 both-ends
> guarded. ⚠ Found+fixed: Schedule D disengage left its own line-7 write
> stale (blocked line 16 after removing the last capital item).
> *Legs.* ✅ compute · ✅ model · ✅ lane · ✅ render (s242r `f94bd41` — one
> page per child, F8814-001/-002 mirrored, decimal split cells, the 1040
> line-16 box 1, PNG-verified) · ✅ diagnostics (4 D_8814) · ✅ **MeF
> (s242s `4d2199d`)** — per-child IRS8814 docs (max 10); the 1040-side
> pairs at exact XSD positions (pre-hooks); Form8814Ind attrs = ΣL15 +
> doc ids (IND-129/127); the NEW "FORM 8814" OtherIncomeTypeStatement +
> the Sch 1 8z link (IND-234); refusals by name F8814-004 + F8814-003-08
> (NARROWER than the face: >$1,350 AND <$13,500). ⚠ Residual: the
> statement carries only the required FORM 8814 row — the other 8z
> components transmit inside the total without statement rows.
> **FORM 8814 IS COMPLETE (s242q/r/s).**

> **2026-08-11 session 242p — K-1 UNREIMBURSED PARTNERSHIP EXPENSES
> (BATCH-003 #10; `4280b18`; migs 0303+0304).** Linked UPE detail rows on
> the 1065 K-1: the i1040se separate-"UPE"-row on Schedule E col (i),
> exactly-once deduction, SE reduction from RAW box 14A (i1040sse), QBI
> reduction per i8995 (verified live). ⚠ v1 boundary: passive UPE refuses
> at staging AND REDs via D_K1_UPE_PASSIVE (Form 8582 routing unbuilt).
> ⚠ Rows WITHOUT UPE keep the preparer-adjusts-box-14A convention.

> **2026-08-11 session 242o — 1099-DIV AGGREGATE FALLBACKS (BATCH-003 #1;
> `452e0f5`; mig 0302).** `div_qualified_agg` + `div_capgain_dist_agg` on
> Taxpayer, the b009 valve semantics (aggregate only when no payer row
> carries the subtype; detail wins; staging warns). ⚠ Box 2a now has ONE
> source (`capgain_distributions_total`) across the Exception-1 path /
> Schedule D 13 / engagement / SDTW. ⚠ Boxes 2b/2c/2d remain
> per-payer-only (named residual). The FA-1040-INTDIV-03 sniff pins the
> new call site + helper body.

> **2026-08-11 session 242n — FORM 7203: the generic current-year
> charitable deduction (BATCH-003 #9; `57c1886`; mig 0301).** A
> source-preserving line-42 input for packets without K-1 subtype detail:
> exactly-once basis reduction, DETAIL WINS (D_K1_7203_GENCHAR on a
> mismatch), never Schedule A. ⚠ Registry lesson: a diagnostics rules
> module MUST use the dict entry shape — a plain function list crashes
> every `manage.py` command at the system-check stage (the s242k 500X
> rules did, three deploys' worth; prod unaffected, now converted).

> **2026-08-11 session 242m — CLERGY SCHEDULE SE (the MINISTER worksheet):
> the IMPORT LANE now carries it (BATCH-003 #8; `222bd37`; no migration).**
> The engine/render/MeF were complete since Unit 4 — the gap was six W-2
> fields + `clergy_4361_exempt` absent from the lane allowlists (the
> off-switch class, 7th). ⚠ Standing conservative behavior: a packet with
> an allowance but no amount-used/FRV computes a $0 exclusion and
> D_MIN_HOUSING_INC names the gap. The published import schema
> regenerated with the seven fields.

> **2026-08-11 session 242k — GEORGIA FORM 500X (AMENDED RETURN): MODEL +
> COMPUTE + LANE + DIAGNOSTICS SHIPPED (BATCH-004 #1 LEG 2; `31eef58`;
> migs 0299+0300).**
>
> *⚠⚠ THE VERIFY-FIRST FINDING: leg 1's premise was WRONG.* The s242j annex
> claimed GA retired Form 500X for a Form 500 amended checkbox — recall,
> uncorroborated. The live DOR check: **the 2025 Form 500X (Rev. 07/21/25)
> is posted and current, and the 2025 Form 500 face carries NO amended
> checkbox.** The wrong claim never reached code; corrected in every file
> that carried it.
>
> *The face:* a SINGLE-COLUMN re-statement of Form 500 — lines 8-26 print
> the corrected GA-500's own values (one shift: Sch 2B refundable, 500
> line 27 → 500X line 28) + reconciliation lines 27 (paid with original),
> 30 (previous refunds), 29/31/32/33/38/39 (line 39 floors at 0), the
> page-1 IRS-audit checkbox, the page-5 explanation. NO per-line Column A
> — the original-return facts are ASKED. Lines 35-37 (UET/late-penalty/
> interest) are preparer-supplied (payment-date dependent, not computed).
>
> *Legs.* ✅ face (SHA-pinned `fga500x`) · ✅ model (`Form500X` on the
> GA-500 STATE return) · ✅ compute (`compute_500x_result`, single source)
> · ✅ lane (`ga_*` amendment facts; no-GA-500 refuses by name; GA-500 on
> an amended return ALWAYS gets its 500X; explanation falls back to
> federal Part II; state baseline at mark-filed) · ✅ diagnostics (4
> D_500X; GA-only amendments legitimate) · ✅ **render (s242l `9752378`
> — all 5 pages: combs derived from the template's own dividers and
> CALIBRATED against the 500's known-good table; the 27→28 mirror shift
> pinned by a bleed test; audit checkbox both ways; explanation overflow
> → Statement page re-wrapped at the statement's column width; value rows
> visually verified via the fitz→PNG loop and positionally pinned against
> the template's own labels; wired into render_complete's state package)**
> · — MeF: GA has no MeF lane in the app. 20 tests.
> **BATCH-004 CLOSED 10/10 at s242l; the file moved to Done. Amended
> FEDERAL MeF (IRS1040X + AmendedReturnInd) is a standing e-file gap —
> the extract refusal holds the line.**

> **2026-08-11 session 242j — FORM 1040-X: THE IMPORT-LANE AMENDMENT
> LIFECYCLE SHIPPED (BATCH-004 #1 LEG 1; `5f455c5`; no migration).** The
> 2026-06-25 unit built the core (spec/model/compute/render/diagnostics —
> see that entry); the lane could not START an amendment until now.
>
> *The lifecycle:* the `amendment` payload block (Part II explanation
> REQUIRED); the resolver INVERTS for it — the target must be a FILED
> return with its as-filed baseline intact, both refusals with remedies;
> the commit writes `Form1040X` + `is_amended_return` BEFORE compute (one
> pass fills Columns A/B/C); the frozen baseline pinned BYTE-IDENTICAL
> across the commit; the mark-filed sweep skips an amended return BY NAME.
>
> *⚠⚠ THE MeF FINDING (a live defect closed): `extract_return` had no
> return-level amended gate* — an amended return composed as a SECOND
> ORIGINAL transmission (silent double-filing; the s152 "paper-only"
> screen note was never enforced in code). Now refused by name; the 4547
> IND-476 per-form seam pins both layers.
>
> *Legs remaining (item open):* ❌ **GA amended** (Form 500 amended
> checkbox — 500X is retired; verify the 2025 DOR face live first) · ❌
> **amended MeF** (`AmendedReturnInd` + `IRS1040X` document — the s152
> LEG 3 item, now enforced by the extract refusal). 12 tests
> (`test_1040x_amendment_lane_s242j.py`).

> **2026-08-11 session 241w — ✅✅ SCHEDULE H (HOUSEHOLD EMPLOYMENT TAXES) IS
> COMPLETE, ALL SIX LEGS IN ONE SESSION. BATCH-004 #5; migrations 0287+0288;
> BATCH-004 is 9 of 10.**
>
> *⚠⚠ THE SPEC POSITION: the RS `SCHEDULE_H` export answers 200 but is
> `"status": "draft"` covering 7 of ~27 lines — the draft-trap's THIRD
> occurrence.* Built from the 2025 face + 2025 Instructions (thresholds
> fetched live) + `IRS1040ScheduleH.xsd`, per
> `server/specs/_schedule_h_source_brief.md`. RS agenda: re-author per-line.
>
> *⚠⚠ THE RENDER FINDING: lines C and 9 put their "No" checkbox FIRST while
> A/B/10-12/27 put "Yes" first* — caught positionally, pinned by a test that
> re-derives both orientations from the PDF's captions. And C/9 are ONE
> stored fact printed in two places, at most one of which a filed form marks.
>
> *⚠⚠ THE MeF FINDING: the business rules RESHAPED the extract* —
> SH-F1040-005 (exactly one Yes among A/B/C: the skip cascade governs
> emission), SH-F1040-016-01 + S2-F1040-146-02 (a line-9-No form omits lines
> 25/26; Schedule 2 sums its line 8 instead), 008/009/006/022 refusals by
> name.
>
> *Details.* One schedule per spouse (`maxOccurs="2"`); Section B state rows
> iterate (two rate periods per state are legitimate); Schedule 2 line 9
> RECONCILED never written (`D_SH_S2L9`, `!=` ± $1); Worksheets 1/2 NAMED RED
> defers with opposite signs; Section B overflow → Statement page; Part IV +
> paid-preparer unmapped (standalone refused, D_SH_STANDALONE). 86 tests.

> **2026-08-10 session 241v — ✅✅ THE GEORGIA QEE CREDIT + IT-QEE-TP2 IS
> COMPLETE, ALL SIX LEGS (s241s → s241v). BATCH-004 #2. The render leg landed;
> no migration; one deploy.**
>
> *⚠⚠ THE SCOPE FINDING: THE GA SCHEDULE 2 CREDIT GRID IS AN E-FILE
> CONSTRUCT.* The item asks to "render the supporting schedules", plural — but
> the GA-500's own line-21 caption reads *"Total Credits Used from Schedule 2
> Georgia Tax Credits **(must be filed electronically)**"*, no paper grid
> ships in the DOR booklet, and drawing one from scratch is what the
> IRS_FORM_RENDERING rule forbids. **A test pins the caption**, so a future
> GA-500 revision that drops it forces the decision to be re-examined. The
> IT-QEE-TP2 is the paper artifact, and it renders.
>
> *⚠ THE 2024 REVISION IS THE CURRENT FORM FOR TY2025* — its face says *"to be
> used for taxable years beginning on or after January 1, 2024"* and DOR posts
> no 2025 revision as of 2026-08-10. Pinned from the face's own words; the
> manifest note carries the per-season re-check.
>
> *⚠⚠ THE RENDER PRINTS THE FACE'S ARITHMETIC, NEVER THE STATUTE'S.* Section A
> line 3 is, by its own caption, *"The lesser of line 1 or 2"* — THIS form's
> two lines. With expended 9,000 / preapproved 3,000 the face's answer is
> 3,000, and the statutory MFJ cap (5,000) must NOT appear — a printed figure
> the face's own caption cannot produce makes the paper contradict itself. The
> cap lives in `D_GAQEE_CAP`. A test prints exactly that shape and reads the
> line-3 rect.
>
> *⚠ THE FACE CORROBORATES TWO EARLIER CALLS.* It **has an MFS checkbox**
> though §48-7-29.16(b) names no MFS cap — the form contemplates the filer the
> statute does not, which is the exact shape `D_GAQEE_MFS` reports. And its
> two attestations each **bar the credit when false**, so a tick is an
> assertion: the addback attestation prints only when a federally-deducted
> amount is recorded, the SSO1 answer only when an SSO is named (the s241e
> unanswered-is-not-an-answer rule, pinned by reading the empty rect).
>
> *Details.* One form per certificate (two certificates → 4 pages of the
> 2-page DOR PDF, pinned); Section A vs B routed on the pass-through
> assertion, with a test reading BOTH rects; Section B lines 3-6 blank in v1
> (no model home for GA income / the applicable rate — stated, not silent);
> Section C (entities) deliberately unmapped; the positional-sibling
> checkboxes (`Check Box1.0`…) identified by their PRINTED y-rows and pinned
> from the PDF (s236/s241h); an unmodelled code renders nothing.
>
> *Legs, all six.* ✅ brief · ✅ model (migs 0285/0286) · ✅ compute · ✅ lane ·
> ✅ **render** · ✅ diagnostics (8). **No federal effect; no MeF leg** — the
> credit reaches Georgia through the e-filed GA return, not the 1040 MeF
> stream. 11 render tests; an 834-test regression across the QEE suites,
> tts_forms, the acroform filler and flow assertions.

> **2026-08-10 session 241u — GEORGIA QEE CREDIT: the LANE and the DIAGNOSTICS
> (legs 4 + 6 of 6). BATCH-004 #2. No migration; one deploy.**
>
> *Lane.* `georgia_credits`, one record per **CERTIFICATE**, with the
> IT-QEE-TP2 facts as a nested `qee_detail` dict (the `form_7203` OneToOne
> shape) — **REFUSED on any credit code but 125**, because every other
> series-100 credit has its own supporting form. ⚠ **The generator emits the
> nested detail IN THE SAME CHANGE** — it drifted twice before
> (`schedule_fs.other_expenses` s227, `form_4547s.children` s241q), each time
> leaving an author's own validator rejecting payloads the server accepted; a
> test reads the generator's own `defs` to hold the pin.
>
> *Staging refuses:* a blank credit code; **a missing `source_year` whenever
> there is credit to carry** (⚠⚠ asked for, never defaulted — two carryforward
> regimes run at once, so either default is wrong for one population and the
> error surfaces YEARS later); used > generated + carried-in; a transfer on
> code 125 (§48-7-29.16 permits none — but ALLOWED on other codes, which are
> transferable); tentatively-allowed > preapproved (the commissioner may
> PRORATE, and the approval is the ceiling); a contribution PREDATING its
> approval (⚠ a LATE one stages — the 60-day window belongs to the diagnostic,
> not staging); and every face/derived line BY NAME (line 21, S1-5, allowable,
> carryover-out, the statutory limit — each message says where the value
> belongs).
>
> *Eight diagnostics — **every rule RECONCILES, none WRITES** (line 21 and
> S1-5 are `input` in the `500` spec; S1-5 is "Add: OTHER additions", SHARED —
> s230):* `D_GAQEE_CODE` (warning — an unmodelled code NAMED; the amount still
> reduces GA tax as filed) · `_EXPIRED` (error — **the message names WHICH
> regime it applied**, three years or five; silent on an unknown generation
> year, because a confident "expired" throws away a live credit) · `_CAP`
> (error — the $25,000 pass-through arm read as the OVERRIDE it is) · `_MFS`
> (warning — **no cap asserted** for a status the statute does not name;
> "deliberately not guessed") · `_L21` (warning — fires only when BOTH sides
> are non-zero; "detail not entered yet" is not a contradiction) · `_ADDBACK`
> (error — ⚠⚠ compared with **`<`, not `!=`**: S1-5 legitimately carries MORE
> whenever any other GA addition exists, and a `!=` test would fire on every
> such return until people learned to ignore it; sign: UNDERSTATES Georgia
> income — the same dollars taking both a federal deduction and a Georgia
> credit) · `_60DAY` (warning — prompts, never rules; the statute's
> consequence is administrative and unobservable) · `_CERT` (info).
>
> *Teeth proven by injection before being trusted (s232):* the `<`-vs-`!=`
> addback comparison, the both-sides-nonzero line-21 guard, the
> generation-year branch (**a flat three-year constant would expire a LIVE
> 2020 pool**), the 60/61-day boundary, and the pass-through override against
> the joint cap. Every rule also tested QUIET (s241o).
>
> *Legs.* ✅ brief · ✅ model · ✅ compute · ✅ **lane** · ❌ render (the GA
> Schedule 2 grid + IT-QEE-TP2 — neither in `forms_manifest.json`; check for
> AcroForm widgets before choosing the backend, GA-500 itself is
> coordinate-mapped) · ✅ **diagnostics**. **No federal effect at all.**
> 45 lane/diagnostic tests + 34 model/compute + 17 brief.

> **2026-08-10 session 241t — GEORGIA QEE CREDIT: the MODEL and the STATUTE
> (legs 2-3 of 6). BATCH-004 #2. Migrations 0285 (two tables) + 0286 (RLS
> default-deny on both). One deploy.**
>
> *⚠⚠ THE MODEL IS SHAPED BY THE TWO CARRYFORWARD REGIMES.* §48-7-29.16(e) now
> allows an unused credit against *"up to its **succeeding three years'** tax
> liability"*; the pre-amendment text said *"the **succeeding five years'**"*;
> and **2024 Ga. Laws 598 §1-7, eff. 1/1/2025, applies "only to unused tax
> credits generated during taxable years beginning on or after 1/1/2025."** So
> `GeorgiaCredit.source_year` is the discriminator — **never a remaining-years
> countdown**, which could not survive a rollover. Both edges are pinned
> (2024 → 5, 2025 → 3), and so is the counter-intuitive consequence: **the
> OLDER credit outlives the newer one** (2024 → 2029, 2025 → 2028). That
> inversion is exactly what a single constant would smooth over, and the error
> would not surface until a pool expired early or late.
> ⚠ An unknown `source_year` returns **None, not a default** — the sign cuts
> both ways: a confident "not expired" lets a dead pool keep cutting Georgia
> tax; a confident "expired" throws away a live credit.
>
> *ONE ROW PER CERTIFICATE*, which is the item's whole point — Georgia issues a
> numbered certificate per approved contribution and a taxpayer may hold
> several, of different codes and generation years. A **conditional** unique
> constraint on (return, certificate number) refuses the same certificate
> twice; ⚠ the consumer was checked first (the s241/s241b pair) — the compute
> ITERATES and SUMS, so a duplicate **double-counts** rather than vanishing.
> Blank numbers are exempt, because the field is optional.
>
> *⚠ §(b)(3) IS AN OVERRIDE, NOT A FOURTH FILING STATUS.* *"[A]nything to the
> contrary contained in paragraph (1) or (2) … notwithstanding"* — a MARRIED
> COUPLE who are S-corp shareholders get **$25,000**, not $5,000. Reading it as
> a status would cap them at the joint limit: an **UNDERSTATED** credit, the
> direction nobody reports.
>
> *⚠⚠ MFS IS NOT NAMED BY THE STATUTE AND IS REFUSED.* §(b) enumerates *"a
> single individual or a head of household"* and *"a married couple filing a
> joint return"*; neither describes a separate filer. So the cap is **UNKNOWN**
> — not half of MFJ, not equal to single — and `cap_for` returns None so the
> caller refuses. Guessing there is what the 404-STOP gate exists to prevent.
>
> *⚠ THE REPORTED PACKET CANNOT TEST THE CAP*, and that is now a test in its
> own right: MFJ at $5,000 expended sits **exactly AT** the (b)(2) limit, so
> the filed figures agree with an uncapped reading too (s219). Every cap
> assertion uses a case where the cap actually binds.
>
> ⚠ Series-100 codes are an **OPEN ENUM**, so only code 125 computes and every
> other code present is **reported BY NAME** (s235 + s236). ⚠ The
> federally-deducted total is the figure the Schedule 1 addback is
> **RECONCILED** against — never written into `S1-5`, a shared preparer line
> (s230). ⚠ Attached to the **FEDERAL** return like every other document
> family: a certificate exists whether or not a GA-500 is attached, and
> detaching one must not lose it. ⚠ **`compute_ga_qee` WRITES NOTHING** —
> GA-500 line 21 stays preparer-keyed, because the `500` spec types it `input`
> and deriving it means re-authoring that spec (s142).
>
> *Legs.* ✅ brief · ✅ **model** · ✅ **compute** · ❌ lane (⚠ register it in
> the GENERATOR too — that drifted twice, s227/s241q) · ❌ render (⚠ neither
> the GA Schedule 2 grid nor IT-QEE-TP2 is in `forms_manifest.json`) · ❌
> diagnostics. **No federal effect at all.**
> 34 tests; 577 green across the QEE suites and the flow-assertion gate.
> ⚠ Also flipped s241s's deliberately-inverted test (it asserted the model did
> NOT exist so the suite would go red the moment the gap closed — it did).

> **2026-08-10 session 241s — GEORGIA QEE CREDIT + IT-QEE-TP2 OPENED (leg 1 of
> 6: the source brief). BATCH-004 #2. No production code; no migration.**
>
> *⚠ BUILT THROUGH A 404-STOP.* `IT_QEE_TP2`, `IT-QEE-TP2`, `ITQEETP2`,
> `500_SCH2`, `GA_QEE` and `QEE` **all 404**. The `500` spec answers 200 but
> models only the DESTINATION — **line 21 "Total credits used from Schedule 2
> (series-100)", typed `input`** — and says nothing about where the total comes
> from. Built on the s241p three-part test: **O.C.G.A. §48-7-29.16 states every
> limit, the liability cap, both carryforward regimes and the no-double-benefit
> rule verbatim**, so there is nothing to improvise; `lookup/1310/` (s223) and
> `lookup/4547/` (s241p) are the precedents. Both specs on the RS agenda.
>
> *⚠⚠ THE FINDING THAT DECIDES THE MODEL — TY2025 RUNS TWO CARRYFORWARD REGIMES
> SIDE BY SIDE.* Current text: *"succeeding **three** years' tax liability"*.
> Pre-amendment text: *"the succeeding **five** years' tax liability"*. And
> **2024 Ga. Laws 598 §1-7, eff. 1/1/2025, applies "only to unused tax credits
> generated during taxable years beginning on or after 1/1/2025."** So a
> TY2025-generated credit carries **3** years and an older one carries **5**,
> and both are live in the same return. ⚠ A single `CARRYFORWARD_YEARS`
> constant is correct for one population and wrong for the other, and the error
> surfaces **years later** when a pool expires early or late — so **the pool
> keys its GENERATION YEAR**, not a remaining-years counter. Both regimes read
> from primary text (2024 code + 2022 code), not from a summary (s232/s239).
>
> *⚠ VERIFY-FIRST CORRECTED THE ITEM.* GA-500 **line 21** and Schedule 1 **line
> S1-5** are BOTH already seeded as preparer inputs, so the reported packet's
> $5,000 is **enterable today** in flattened form — the return is not wrong.
> The genuine gap is the DOCUMENT DETAIL (credit code, certificate identity,
> generated-vs-used, carryover, the credit↔addback link), which is exactly what
> the item's own second paragraph says. *A build that "fixes" a correct return
> is measuring the wrong thing.*
>
> *⚠⚠ THE SCHEDULE 1 ADDBACK IS A CONDITION ON THE CREDIT, NOT AN ADDITION.*
> §48-7-29.16(h)(1): *"No credit shall be allowed … with respect to any amount
> deducted … as a charitable contribution …"*. And `S1-5` is *"Add: **other**
> additions"* — SHARED and preparer-keyed — so the build **RECONCILES** against
> it and never writes into it (s230: a shared line's first writer must not
> become its owner; the failure mode is a DISAPPEARED number).
>
> *⚠ THE PACKET CANNOT TEST THE CAP.* MFJ with $5,000 expended is **exactly**
> the (b)(2) limit, so it cannot distinguish capped from uncapped, and all five
> of its figures being equal is a coincidence of this packet rather than a rule
> (s219 — a value that cancels by luck). ⚠ **(b)(3) is an OVERRIDE, not a
> fourth filing status**: an LLC member / S-corp shareholder / partner gets
> **$25,000** even when MFJ, and its "portion of the income on which such tax
> was actually paid" proviso needs a KEYED assertion. ⚠ **The pre-approved
> amount is NOT computable** — a statewide first-come-first-served queue against
> a **$120 million** annual cap that the commissioner may **prorate**.
> Transcribed from IT-QEE-TP2, never derived.
>
> *Legs.* ✅ **brief** `_ga_qee_credit_source_brief.md` (a build leg, not a note
> — s222/s223/s241p) · ❌ model (⚠ per CERTIFICATE; series-100 codes are an
> **OPEN ENUM**, so refuse computation for any code but 125 **by name** — s235;
> new tables → RLS default-deny) · ❌ lane · ❌ compute · ❌ render · ❌
> diagnostics. **No federal effect at all** — this is a Georgia credit.
> 17 tests pinning the statute's presence in the brief and the two verify-first
> findings against the seeded DB. ⚠ The content tests **strip markdown** before
> comparing: blockquote markers and emphasis fall INSIDE the quoted phrases, so
> a naive search would pass or fail on reformatting rather than on content.

> **2026-08-10 session 241r — ✅✅ FORM 4547 (Trump Account Election(s)) IS
> COMPLETE, ALL SIX LEGS. BATCH-004 #10. No migration; one deploy.**
>
> *⚠⚠ PRINT AND TRANSMIT HAVE GENUINELY DIFFERENT SHAPES, and that is the whole
> risk of this unit.* The Instructions say *"If you have more than two
> children…attach as many copies of Form 4547 as are needed"* while
> `TrumpAccountChildInfoGrp` is `maxOccurs="100"` in ONE document. So
> **`render_4547` emits ⌈children/2⌉ pages** and **`build_irs4547` emits ONE
> document carrying every child** — and both directions are pinned facing each
> other. A renderer mirroring the face would silently drop children 3+ (nothing
> downstream would notice, because the XML still has them); a builder copying
> the renderer's split would create documents the IRS reads as SEPARATE
> elections. The render test asserts child 3 is in **column 1 of page 2** and
> that page 2 does not repeat child 1 — "present somewhere" would pass for a
> renderer that drifts a column per page.
>
> *⚠ THE COLUMN ASSIGNMENT WAS VERIFIED POSITIONALLY*, not by trusting the
> `_NN` suffix order (the s236 Form-7203 trap, the s241h guard-with-a-hole): a
> test reads every mapped widget's rect from the PDF and asserts, on all 18
> rows, that child 1 is the LEFT widget in x 260-417 and child 2 the RIGHT in
> 419-576, sharing a y.
>
> *⚠ THE SIGN-HERE AND PAID-PREPARER BLOCKS ARE DELIBERATELY UNMAPPED — a
> finding, not an omission.* The taxpayer signature row carries **no widgets at
> all** (hand-signed), and the seven preparer widgets were identified **from
> their own printed labels** (firm's name / EIN / address / phone /
> self-employed) rather than guessed — firm facts this form does not hold
> separately from the 1040's own preparer record. Mapping by inference would
> assert a wrong field on a signed consent.
>
> *MeF.* `IRS4547` sits between `IRS4255` and `IRS4562` in the ReturnData1040
> sequence (after `IRS3800`, before `IRS4835`) — verified across 2025v5.3,
> 2025v5.4 and 2026v1.0 and **pinned by a test that re-derives the position from
> the schema file**, so a TY rollover fails there rather than at transmission.
> The intra-document element order is pinned the same way (s231: an
> `xsd:sequence` emitter can validate BY COINCIDENCE).
> ⚠ Line 5's `xsd:choice` is resolved in the SOURCE dataclass so the builder
> never decides; lines 6/7 are `minOccurs="0"` so an unelected box is **ABSENT,
> never `"false"`** — both directions tested, since the absence test alone
> would pass for a builder that emitted nothing.
> ⚠⚠ **The `IND-476` refusal is BUILT** — a Form 4547 on an amended return is a
> hard reject, and the message carries its remedy (*"can be filed at any
> time"*), so a preparer does not abandon a valid election.
> ⚠ Everything the schema makes mandatory REFUSES rather than substituting
> (s241c). A foreign address refuses for the Form 1310 reason.
>
> *⚠⚠ A TEST THAT WAS PASSING FOR THE WRONG REASON, CAUGHT AND FIXED.* The six
> "missing mandatory fact REFUSES" cases were green before the fixture was
> complete — `extract_return` refuses on the unanswered digital-asset question
> long before reaching Form 4547, so `pytest.raises(UnmappableValue)` was
> satisfied by an unrelated exception (**the s241f passing-by-absence trap**).
> The fixture now answers it and every case asserts the message is OURS. The
> XSD-order test had the same hazard from the other end (an empty regex match
> makes `[] == sorted([])` pass) and now asserts both ends are non-trivial.
>
> *Legs, all six.* ✅ brief · ✅ model (migs 0283/0284) · ✅ lane · ✅
> diagnostics (8) · ✅ **render** · ✅ **MeF**. **No compute leg, deliberately.**
> Form 8879-TA stays print-only and ERO-retained by its own printed
> instruction, with no element anywhere in the MeF schema set.
> 27 tests here (+45 lane/diagnostics, +35 model); a 1,996-test sweep across
> render / e-file / manifest / flow assertions with only the one known
> pre-existing `test_mar30_session4` red.
> ⛔ One item ask still open and NAMED: duplicate child elections across TWO
> elections on one return (the DB constraint is per-election).

> **2026-08-10 session 241q — FORM 4547: the LANE and the DIAGNOSTICS (legs 3
> and 6 of 6). BATCH-004 #10. No migration; one deploy.**
>
> *Lane.* `form_4547s` is a **parent-plus-children** section (the
> `form_1095as`/`allocations` precedent): the parent is one FILED FORM, the
> nested `children` list is Parts II/III, and `form_8879ta` rides as a single
> nested dict (the `form_7203` shape). Ordered after `dependents` because each
> child's `dependent_key` resolves against those rows; an unmatched key commits
> **UNLINKED with a warning** — the election still files (name, SSN, DOB and
> relationship are all on the row), and what is lost is only the pilot
> program's citizenship check.
>
> *⚠ Staging refuses ONLY the untransmittable* — whether a child QUALIFIES is a
> preparer assertion the packet shows on its face, so it is transcribed and the
> diagnostics speak (the s222 doctrine). Refused: line 5's `xsd:choice` violated
> (the same-address indicator beside an address, or domestic beside foreign), a
> US address with **no county** (`CountyNm` is NOT `minOccurs=0` in that
> branch), "not the same address" with no address at all, an election with no
> children, an unnamed responsible party, >100 children, and the Part I
> address/phone choices.
>
> *⚠⚠ FORM 8879-TA PART I IS REFUSED BY NAME.* All six lines are copies of the
> Form 4547 and every caption says so; Part II swears they came from that form.
> Keying them would be the s234 two-sources defect on a signed document (the
> s238 column-(a) doctrine).
>
> *⚠⚠ THE GENERATOR HAD ALREADY DRIFTED — and `children` is REQUIRED.* Staging
> validated both nested families from the moment they were written, but the
> published schema emitted NEITHER, so a payload author could not have
> discovered a field the server insists on and would have had a correct payload
> rejected client-side. **The s227 class exactly**, and the same defect
> `schedule_fs.other_expenses` was (validated since s225, unemitted until
> batch-001 #1). Fixed, and pinned by a test that reads the **generator's own
> `defs`** rather than the written `.json`, so a stale artifact cannot hide it
> either. *The pin is proven by history — it fails against the code as it stood
> an hour earlier.*
>
> *Eight diagnostics, and the shape of the set is that **line 6 and line 7 are
> different tests***: `D_4547_AGE` (error — the ACCOUNT test alone),
> `_PILOT` (error — the birth window, **and its message says the line 6 account
> election may still be correct**), `_CITIZEN` (error — narrower than CTC/ODC),
> `_LINK` (warning, and ONLY when the pilot is elected, because otherwise the
> missing link costs nothing), `_UNKNOWN` (info — the two conditions the app
> CANNOT evaluate, stated rather than guessed), `_AUTH` (error — line 6
> unchecked), `_AMENDED` (error — `IND-476`, **and it says the form "can be
> filed at any time"** so the fix is to file separately, not abandon),
> `_8879TA` (error — an authorization with nothing to authorize).
> ⚠ **Every rule that can be quiet is tested quiet as well as loud** (s241o:
> only the quiet case proves a comparison is real). An unknown DOB and an
> unanswered citizenship are silent everywhere — an unanswered question is not
> a failed test, and the sign runs against the taxpayer.
> ⚠ A test pins the headline case: **a child born in 2020 gets the account and
> NOT the contribution.** A rule conflating them would block a valid election
> AND give the wrong reason.
>
> 45 lane/diagnostic tests + the 35 from s241p; a 1,105-test sweep with only
> the three known `test_backentry_cleanup` reds.
> ⛔ **STILL OPEN on #10: the render leg and the MeF builder.** Also named
> rather than hidden: duplicate child elections are caught within ONE election
> by the DB constraint, but not across two elections on the same return.

> **2026-08-10 session 241p — ★ FORM 4547 (Trump Account Election(s)) + FORM
> 8879-TA — NEW UNIT OPENED, legs 1-2 of 6. BATCH-004 #10. Migrations 0283
> (three tables) + 0284 (RLS default-deny on all three). One deploy.**
>
> *⚠⚠ BUILT THROUGH A 404-STOP, DELIBERATELY.* `lookup/4547/`, `F4547`,
> `FORM_4547`, `8879_TA` and `8879TA` **all return 404** and nothing is cached.
> The gate's own words are *"do NOT improvise the implementation"* — and there
> is nothing here to improvise: `IRS4547.xsd` puts a `<LineNumber>` on every
> element and carries **no amount** except `PriorYearAGIAmt` (a
> signature-authentication input), so **the form computes nothing**, and every
> eligibility test is printed verbatim by the IRS. **The precedent is exact:
> `lookup/1310/` ALSO 404s and s223 built Form 1310 end to end** from the IRS
> form + XSD + business rules with a source brief — and 1310 is a *filed* form,
> so this is not the s222 information-return carve-out being stretched. Both
> forms logged on the RS agenda.
>
> *⚠⚠ LINE 6 AND LINE 7 ARE DIFFERENT TESTS AND THE FORM ASKS THEM
> SEPARATELY.* Line 6 (open the account): under 18 at year end, valid SSN
> issued before the election, no prior election. Line 7 (the $1,000 pilot
> contribution) ADDS: *"born after December 31, 2024, and before January 1,
> 2029"*, *"be a U.S. citizen"*, and anticipated qualifying-child status. **A
> child born in 2020 qualifies for the account and NOT for the contribution.**
> Collapsing them would be the s239 one-sentence-two-tests error.
> ⚠ The FACE states the window as a year list ("born in 2025–2028"), the
> INSTRUCTIONS as open boundaries — the date form is pinned, because a year
> list is where an off-by-one comes from. All four boundary dates tested.
> ⚠ US citizenship here is NARROWER than the CTC/ODC test, which a national or
> a resident alien passes.
> ⚠ Unknown DOB / unanswered citizenship return **None, not False** — the sign:
> reporting "not eligible" for an unanswered question would talk a preparer out
> of a $1,000 contribution (s241c).
>
> *⚠⚠ THE SCHEMA DECIDED TWO DESIGN QUESTIONS TASTE WOULD HAVE GOT WRONG.*
> `IRS4547` is `maxOccurs="unbounded"` on the return and
> `TrumpAccountChildInfoGrp` is `maxOccurs="100"` → the model is REPEATABLE,
> and one document carries up to a hundred children. But the printed face has
> **two child columns** and the Instructions say to attach as many copies as
> needed → **print and transmit have genuinely different shapes**,
> ⌈children/2⌉ pages vs one XML document. A renderer mirroring the face would
> silently drop children 3+.
> ⚠ The relationship enum is the XSD's, **re-derived from the schema file by
> the test** so a TY rollover fails here, not at transmission. What it EXCLUDES
> matters: PARENT / GRANDPARENT / AUNT / UNCLE are valid `Dependent`
> relationships and absent here, so a mapping must REFUSE — falling through to
> OTHER would assert a different fact (s235 verbatim).
>
> *⚠⚠ FORM 8879-TA DOES NOT TRANSMIT, AND THE BATCH ITEM SAYS IT DOES.* Its
> printed header: *"ERO must obtain and retain completed Form 8879-TA."* There
> is **no `IRS8879TA` element anywhere in the MeF schema set** (2025v5.3,
> 2025v5.4 and 2026v1.0 searched). The IRS's own *About* page reads as though it
> transmits — **the face and the schema win** (s239/s241o: a summary is not the
> artifact). Same position the app already takes for Form 8879. Its Part I is a
> **COPY** — all six lines name their own source ("First name of Child 1 *from
> Form 4547, line 1(a)(i)*") and Part II swears they came from that form — so
> they DERIVE and are never keyed; a test asserts the model exposes no field to
> key one into (s234 + the s238 column-(a) ruling).
>
> *⚠⚠ `IND-476` IS A LIVE CONSTRAINT ON A BATCH ITEM NOT YET BUILT.* *"Form
> 4547 must not be present in a post-original return."* **Reject, Active** —
> and the Instructions agree: *"Do not amend Form 1040, 1040-SR, or 1040-NR to
> attach Form 4547."* **BATCH-004 #1 is the 1040-X lifecycle.** Pinned by a test
> that re-reads the business-rules CSV so #1 cannot quietly undo it (s240: the
> reject list is the spec for the seam). `IND-475` is only an Alert and is
> pinned as such.
>
> *Legs.* ✅ **brief** `_4547_source_brief.md` (a build leg, not a note — s222/
> s223) · ✅ **model** `Form4547` + `Form4547Child` + `Form8879TA` + the
> eligibility predicates · ❌ **lane** · ❌ **render** (⚠ the face HAS AcroForm
> widgets, row-named two-per-row; verify columns POSITIONALLY — s236/s241h; and
> `f4547.pdf` is **absent from `forms_manifest.json`**) · ❌ **MeF** · ❌
> **diagnostics**. **No compute leg, deliberately** — nothing on this form
> reaches the 1040, Schedule 1, AGI or tax.
> 35 tests, **teeth proven by injection before being trusted** (s232) —
> including a check that the "no `IRS8879TA` anywhere" assertion reads real
> files rather than passing by absence (s241f); 721-test regression set green
> including flow assertions, plus 127 model/migration/schema tests.

> **2026-08-10 sessions 241l/m/n/o — ★ FORM 1099-PATR (Taxable Distributions
> Received From Cooperatives) — NEW UNIT, COMPLETE. BATCH-004 #9. Migrations
> 0281 + 0282 (RLS default-deny — the new-table rule); one deploy for the
> Georgia + CRUD legs.**
>
> *⚠ NO RENDER LEG AND NO E-FILE LEG — a finding, not a deferral* (the s222
> 1099-MISC position verbatim): an information return received by the taxpayer
> is an INPUT document. No `f1099patr` field map or manifest entry exists for
> ANY 1099, and `IRS1099R` is the only 1099 element in the 1040 MeF schema set.
> The item's "render and transmit the source form" is therefore N/A, stated
> rather than skipped.
>
> *⚠ THE AUTHORITY IS THE FORM.* No RS spec exists for any information return
> (s222), so the 404-STOP gate does not apply and leg 1 was a source brief —
> `server/specs/_1099patr_source_brief.md`, verbatim from **Rev. April 2025**.
>
> *⚠⚠ BOX 1 IS NOT UNCONDITIONALLY INCOME*, which the batch item's "$1,004 →
> 8z" glosses over. *"Any dividends paid on (1) property bought for personal use
> or (2) capital assets or depreciable property used in your business are not
> taxable. However, if (2) applies, reduce the basis of the assets by this
> amount."* A blind feed TAXES A NON-TAXABLE AMOUNT (⚠ overstates income). The
> carve-out is a preparer assertion; the basis reduction is recorded and NOT
> applied, because the app cannot know which assets. And the income set is
> boxes **1, 2, 3 and 5**, not box 1 — while boxes 8/9 are *subsets of* those.
>
> *⚠⚠ THE FIX WAS ONE LINE FROM REPRODUCING THE BUG IT PREVENTS.* Schedule 1
> line 8z has a SINGLE final writer, so PATR had to EXTEND that composition (the
> s230 rule, already learned once on this very line) — but that writer **returns
> early when there is no 1099-MISC**, so a PATR-only return would have had its
> income silently dropped. Both the early return AND the disengage path now
> carry the share, both pinned.
>
> *GEORGIA — the item's claim CONFIRMED, with two qualifications it does not
> state.* Ga. Comp. R. & Regs. r. 560-7-4-.02 fetched verbatim first (s233/s236/
> s239 each found this feed wrong a different way). **(4)(b)1**'s unearned list
> closes with *"and other similar income"* → UNEARNED, worksheet **L10**, which
> mirrors the federal line the income sits on (never L7 — the 1099-DIV pull owns
> that). **(4)(b)1**'s earned list opens with *"net business income earned by an
> individual from any trade or business"* → a Schedule C/F-routed row is EARNED
> and is ALREADY in the base via that schedule's net profit, so **only the 8z
> route contributes**. **(3)**: *"Only retirement income that is included in
> Georgia taxable income"* → the box-1 nontaxable part must not reach Georgia.
> **(2)**: per owner; a 1099-PATR has ONE recipient TIN.
>
> *⚠⚠ THE FEEDER MADE AN EXISTING RULE WRONG — 7th occurrence, and it would have
> opened a NEW silent gap.* The L10 derive is PARTIAL. `D_GA500_017` asked "is
> L10 blank?", so a return with a 1099-PATR *and* other 8-line income would have
> read as complete while short. New trigger: "is L10 still only what the
> software derived, while federal other income is larger?" — reduces to the old
> test with no 1099-PATR, and deliberately NOT a strict `<` because (4)(b)2
> excludes gambling from retirement income, which would false-alarm forever.
>
> *⚠⚠ AND A DEFECT IN MY OWN s241n RULE, caught by the CLIENT TYPECHECK.*
> `qbi_is_patron` is a **per-activity** field on ScheduleC/ScheduleF, never on
> Taxpayer. `D_1099PATR_199A` read it off the taxpayer through a
> `getattr(..., False)`, so it answered "not a patron" on EVERY return and the
> warning fired unconditionally and could not be cleared by doing what it asks.
> *A `getattr` default turns a wrong attribute name into a plausible answer.*
> The firing test had passed either way — only the QUIET case has teeth.
>
> *Legs.* **model** `Form1099PATR` (13 boxes + the two box-1 carve-out fields +
> payer identity + routing) · **compute** `compute_1099patr` (pure; writes
> nothing) + the 8z composition + box 4 → 1040 line 25b · **state** the GA RIE
> L10 pull · **lane** `patr_1099s` (routing + `link_key`, ordered after
> `schedule_cs`/`schedule_fs`; unresolved key → committed UNLINKED with a
> warning) · **diagnostics** five `D_1099PATR_*` + `D_GA500_019` · **client**
> `patr-1099s` CRUD + `SlateForm1099PATRScreen` + its own nav tab, shipped WITH
> the rules (the s241c lesson: a refusal with no input surface is a dead end) ·
> **tests** 65 across two files + the `D_GA500_017` regression pinned in the
> existing RIE pull suite.

> **2026-08-10 session 241j — Schedule 1 alimony (BATCH-004 #3). No migration,
> one deploy.** Lines 19a/19b/19c gain their LANE leg; the form's other legs
> were already green.
> **⚠⚠ THE DEFECT: `D_SCH1_003` checked that the line-19c date was PRESENT and
> never what it SAID.** So a divorce executed in 2020 deducted alimony and
> reduced AGI — **while a rule named "alimony completeness" reported the return
> clean.** ⚠ Sign: OVERSTATES the deduction, understates tax, and moves taxable
> Social Security / the enhanced-senior deduction / the Georgia starting income
> with it. *A rule that validates the SHAPE of a fact reads as though it
> validates the FACT.*
> **`D_SCH1_007`** (error) enforces IRS Topic 452 arm 1 — not deductible for an
> instrument "executed after 2018" (TCJA §11051 repealed IRC §215). Boundaries
> pinned: 12/31/2018 fine, 01/01/2019 not. A blank/unparseable 19c stays with
> `D_SCH1_003` — reading it as pre-2019 would turn a data-entry problem into a
> silent tax answer.
> **`D_SCH1_008`** (info) for arm 2, which the app CANNOT decide: ⚠ **a
> post-2018 modification alone does not end the deduction** — only one that
> *expressly states* the repeal applies — and neither the date nor the election
> is stored, so guessing would DENY a deduction (opposite sign). DEFERRAL_AUDIT.
> ⚠ **Severity PROVED, not asserted**: a two-way compute pins AGI down by
> exactly $8,400 ($98,978 → $90,578, the packet's own figures).

> **2026-08-12 session 246b — ✅ FORM 8582 GAINS THE REP NONPASSIVE ROUTING
> (BATCH-001 #5; mig 0315, one deploy).** Ken un-parked it LIVE, superseding
> both prior scope rulings; spec R-8582-RE-PRO already stated the routing.
> `RentalProperty.material_participation` (tri-state) + ONE predicate
> (`rental_rep_nonpassive`) shared by compute/diagnostics; the bypass mirrors
> the §469(g)-release mechanics and hands the GA RIE per-owner feeder the full
> net through the persisted columns. ⚠ THE R-8582-MAGI TRAP: the REP rental
> loss is a modified-AGI ADD-BACK — pinned (−60,000 REP loss beside a passive
> rental at 149,000 wages: the passive allowance stays $500). ⚠ §469(f)
> former-passive rows REFUSE the routing (D_8582_FPA error); matpart-without-
> REP routes nothing (§469(c)(2), D_8582_MATPART_NO_REP); unanswered-under-REP
> warns. The reported b100 shape reproduces to the dollar (−8,536 fully
> deducted, BLANK 8582 face, RIE owner shares intact, the SS-taxability drop).

> **2026-08-12 session 245b — ✅✅ FORM 1099-Q BUILT, ALL LEGS (BATCH-001 #10;
> migs 0313 + RLS 0314, one deploy).** An information return: no RS spec (s222);
> the governing record is `server/specs/_1099q_source_brief.md` — Pub 970
> (2025) ch. 6 (Coverdell) / ch. 7 (QTP), whose WORKED EXAMPLES are the test
> answer key ($18 QTP / $735 credit-coordination / $25 Coverdell-FMV-derivation,
> each pinned with the Pub's own whole-dollar step rounding). ⚠ ONE CLASSIFIER
> (`compute_1099q.classify_row`) drives compute, diagnostics AND the worksheet:
> skip (trustee-to-trustee / full rollover — "Don't report tax-free
> distributions"), **covered (AQEE ≥ gross — tax-free WITHOUT needing earnings;
> the reported packet's $2,625/$2,625 shape, which must SURVIVE the face tie)**,
> computed, refuse_partial (partial rollover / returned excess — unmodeled
> ordering), refuse_underivable (box 2 blank + no FMV inputs while a taxable
> figure exists). Σ taxable rides the COMPOSED Schedule 1 line 8z as the SIXTH
> contributor ("QTP"/"Coverdell ESA" face literals; add + remove pinned). ⚠ The
> 10% additional tax stays Form 5329 Part II with its preparer-KEYED line 5
> (`Form5329.edu_able_dist` — Ken 2026-06-25); D_1099Q_003 RECONCILES, never
> feeds — and its quiet-case test caught the rule reading a compute-INPUT key
> (`f5329_line5_*`) off the MODEL (AttributeError only when rows exist).
> D_1099Q_004 is the item's double-benefit ask (8863 engaged + an exclusion
> with no credit-used reduction). Render = a statement-page worksheet (no IRS
> 1040-attachment face exists); refusals print as refusals. Lane `q_1099s`
> (all six registries; the generator emits 21 fields) + full browser CRUD.
> 24 regressions + 621 neighbors green. **With #10, every buildable BATCH-001
> item is closed** (#4 NOL-parked, #5 ⛔ KEN).

> **2026-08-12 session 244 — ✅✅ FORM 8862 GOES MULTI-CATEGORY (BATCH-001 #6;
> migration 0312, one deploy `4dd5c40`).** The s241i "complete" verdict held
> for the FORM's legs but every GATE was EIC-only and category-blind: the
> only stored disallowance fact was `eic_disallowed_prior_year`, so a
> CTC-only or AOTC-only prior disallowance rendered NO 8862, fired NO rule,
> and (with no keyed line-1 year) transmitted NO recertification. Two new
> tri-state Taxpayer facts (`ctc/aotc_disallowed_prior_year`) + ONE
> predicate everywhere — **a category engages iff claimed now AND
> previously disallowed** (i8862 Rev. 12-2025 line 2) — at the MeF builder
> (`_f8862_engagement`; named refusal when nothing resolves; attach gate
> rides engagement), the render (per-Part drawing — a CTC-only recert must
> not print sworn EIC answers the XML omits), the diagnostics (D_8862_003
> re-based per category; NEW D_8862_004/005 requirement warnings + NEW
> D_8862_006 unanswered-category), compute_eic's 8867/8862 backfill
> (reaches non-EIC returns, opt-out included; 8867 7a sees all flags) and
> every import surface (+ the lane finally accepts the Part II line 4/11
> derive sources its own refusal message pointed at). ⚠ "CTC claimed"
> includes Schedule 8812 line 12 — ATS Scenario 5 (line 19 zeroed by the
> tax limit, ACTC elected out, box still ticked) pins it. **⚠ HOH REFUTED
> as an 8862 category** (face + XSD + i8862 all three-category; HOH recert
> is Form 8867 due-diligence territory). 24 new regressions + the CTC-only
> print test; scenario5 facts state all three disallowances.

> **2026-08-10 session 241i — ✅✅ FORM 8862 IS COMPLETE (all legs).** Section A
> shipped with its PRINT and its TRANSMISSION in one unit, deliberately: line 6
> and line 8 were in the XSD and unemitted while also unprinted, and splitting
> them is how s241e's paper-vs-XML gap opened.
> **⚠ Line 6 is the Section A/B ROUTER**, so the box and the branch derive from
> ONE list (two derivations of one fact is how they drift — s234).
> **⚠⚠ Line 8 carries a condition the SCHEMA does not state** — "If the child
> was born or died during the year … **Otherwise, skip this line**" — so a
> birth date prints/transmits ONLY for an in-year birth; a date for a child born
> earlier **answers a question the form did not ask**.
> ⚠ The DEATH half is never filled: `Dependent` has no date-of-death field.
> Real EIC case (the residency test has an exception) → DEFERRAL_AUDIT.
> ⚠ Line 7 wants DAYS, the app records MONTHS: 12 → 365, less is left blank,
> because under 183 days BARS the EIC for that child.
> ⚠ The new blank-check reported a false positive (child 2 showing child 1's
> "07") — the rows ABUT at y=672, so the READING was loose, not the map. Inset
> 2pt, then re-proven by dropping the in-year condition (failed with "03").
> **FORM 8862 — all seven legs green** (s241c→s241i): model + migs 0279/0280,
> MeF, print (16 → ~100 map entries), CRUD + lane, `D_8862_002`/`D_8862_003`.

> **2026-08-10 session 241h — Form 8862 Parts III + IV PRINT. No migration, one
> deploy. ⚠ STILL PARTIAL — do NOT tick** (Part II Section A print/emit left).
> They were TRANSMITTED but printed blank, so a preparer reviewing the paper saw
> empty per-child grids while the e-filed version asserted answers — **and the
> paper is what they sign**. Field map 16 → **87** entries. The render feed
> MIRRORS `build_irs8862` (same `classify_dependent_ctc`, same CTC-vs-ODC split,
> same caps, same `citizenship_status` for line 17, same eligibility trio for
> 19a) so paper and XML cannot diverge.
> **⚠⚠ THE WIDGET NUMBERING IS NOT CONTIGUOUS ACROSS LINES.** Line 16's four
> children are `c2_11..c2_14` on one full-width row; its four OTHER DEPENDENTS
> are `c2_15..c2_18` on two half-width rows below; line 17 restarts at `c2_19`.
> "Eight consecutive widgets per line" would put other-dependent 1 into **child
> 4's column** — a wrong Yes/No on a signed form, nothing looking out of place.
> Every index read off the PDF (anchors in the field-map docstring).
> **⚠ THE GUARD ITSELF HAD A HOLE, FOUND BEFORE TRUSTING IT.** The per-column
> check re-derives each box from its printed label — but "Other dependent 1" is
> printed on BOTH line 16's row and line 17's, so a copy-paste between them
> would have passed. Added a row-identity check and **injected exactly that
> copy-paste**: it failed, naming the duplicated row. (An existing
> `test_no_duplicate_widget_targets` caught it too.) *A positional guard is only
> as good as what distinguishes the rows — ask what your own test cannot see.*
> Gates: 601 across the 8862 + flow suites; 1,494-test render/e-file sweep with
> only the two known pre-existing reds. ⚠ MOVEMENT: printed output only — the
> values were already transmitted.

> **2026-08-10 session 241f — Form 8862 DIAGNOSTICS leg. No migration, one
> deploy. ⚠ STILL PARTIAL — do NOT tick this form** (Parts II-A/III/IV print).
> `D_8862_002` (error) — required but INCOMPLETE, naming the missing lines.
> `D_EIC_008` states the requirement as a warning; this states the shortfall,
> because **every unanswered question on this form has an answer that BARS the
> credit**. The MeF builder refuses on the same facts but only at composition,
> so a return reviewed or abandoned before transmission said nothing.
> `D_8862_003` (error) — Part I line 2 vs the credits actually claimed, BOTH
> directions: a ticked box with no claim attaches a certification for a
> re-claim that is not being made; a claimed credit with no ticked box files
> the form without the part that supports it (only the second changes what the
> taxpayer receives). Both no-op on a math-error disallowance.
> 12 tests. ⚠ One caught a trap in its OWN setup — the 8862-row helper was a
> filter-and-update, and those rows are backfilled only on an engaged-EIC
> recompute, so on a fresh fixture it silently did nothing and two tests
> **passed by absence rather than by behaviour**. Now creates and asserts.
> ⚠⚠ **The `inspect.getsource(seed_builtin_rules)` false-red class was
> enumerated in full for the first time: `-k "diagnostic"` = 22 failed / 878
> passed, ALL 22 that class.** Sixth session it has cost time; ~1 hour here just
> to prove none was ours. Promoted to the next unit.

> **2026-08-10 session 241e — Form 8862 RENDER leg extended. No migration, one
> deploy. ⚠ STILL PARTIAL — do NOT tick this form.**
> **⚠⚠ MY OWN EARLIER CHANGE MADE A CORRECT BOUNDARY WRONG.** The field map was
> a deliberate coarse data-map whose docstring said the granular sections were
> *"left blank for the preparer to complete by hand — a faithful data-map, not
> an adjudication."* True while the app held no answers; s241c/s241d gave it
> real ones AND transmitted them, after which a blank printed face beside a
> populated transmission is **the paper and the XML disagreeing about what the
> taxpayer swore**. *Fifth occurrence of "adding support makes an existing
> correct rule wrong" (s225/s233/s238/s240), and the first where the earlier
> change was ours two sessions back. It generalizes past specs and diagnostics
> to RENDERING: a "we deliberately leave this blank" comment is a claim about
> what the app KNOWS, and it expires when the app learns it.*
> Now printed: Part II lines 3/4 and Section B's 9a/9b, 10a/10b, 11a/11b, fed
> from the `Form8862` row plus the derived facts. ⚠ An unanswered question
> leaves BOTH boxes blank — never printed as "No". Section B prints only on the
> childless path, per the face's own instruction.
> **⚠⚠ [0]=Yes / [1]=No VERIFIED POSITIONALLY, and the guard PROVEN by injecting
> the swap** (s236 + s232): `c1_4[0]` x=504.00 y=301.00 with the printed "Yes"
> at x=517.00 on the same row; the test re-derives this from the PDF instead of
> trusting the comment. Also retired a frozen four-key trip-wire that fired on
> legitimate growth rather than on a wrong mapping.
> ⛔ **Open:** the item's diagnostics (next, and smallest); Parts II-A/III/IV
> print (~100 widgets of per-person grid, still transmitted-but-not-printed);
> Section A lines 6 and 8. ⚠ MOVEMENT: printed output moves; no dollars.

> **2026-08-10 session 241d — Form 8862, two more legs. No migration, one
> deploy. ⚠ STILL PARTIAL — do NOT tick this form.** Both found by reading
> **`IRS8862.xsd`**, not the printed face (the s223 rule: the schema decides
> what transmits).
> **Part II SECTION B now transmits** — the childless-EIC path, which is exactly
> the reported packet's shape. `Primary`/`SpouseNoQualifyingChildGrp`, each of
> `NoQualifyingChildGrpType`: day count (9), age (10), other-person-claims (11).
> ⚠ **All three REQUIRED** (no `minOccurs="0"`), so no half-filled group — and
> each carries a caution that BARS the EIC, so a missing member REFUSES rather
> than assuming. Age derives from the DOB; line 11 from the EIC childless Rule
> 12 gate; only the day count is keyed.
> **⚠ ODC PERSONS WERE POSING AS CTC CHILDREN.** `ODCPersonInformationGrp`
> (line 13) existed in the schema and was never emitted — every ODC person
> travelled in `CTCACTCChildInformationGrp` (line 12), asserting
> `QualifyingChildInd` (line 15), a qualifying-CHILD claim an ODC qualifying
> relative cannot make. The schema gives the ODC group no line 14/15 precisely
> because those are child questions. Now split on `ctc_eligible`.
> 33 tests in the file; **full e-file sweep 1,132 passed / 0 failed.**
> ⛔ **Open:** `f8862_2025.py` + `seed_8862.py` still cut to the draft spec's
> collapsed per-Part booleans (the PRINTED form lacks the real answers); the
> item's diagnostics; Section A lines 6 and 8.
> ⚠ MOVEMENT: e-file output moves, conservatively (Section B emits or refuses;
> ODC stops asserting qualifying-child status). No dollars move.

> **2026-08-10 session 241c — 1040 BATCH-004 #8 (Form 8862). Migrations 0279 +
> 0280 (RLS), one deploy. ⚠ PARTIAL — do NOT tick this form.** Form 8862 gains
> an **input model, an MeF leg that is finally truthful, a browser CRUD and a
> lane section**; its RENDER leg and its diagnostics are still open, and Part II
> Section B does not yet emit.
> **⚠⚠ THE DEFECT: the MeF builder FABRICATED FIVE SWORN ANSWERS while its own
> docstring promised it did not** — *"Every per-child/per-student answer derives
> from the SAME model facts the credits computed from (bridge-gate: the 8862
> can't contradict the claim)."* False. Line 3 and line 4 emitted `"false"`,
> lines 16/17 and 19a emitted `"true"`. **Lines 17 and 19a were the worst,
> because the app STORES those facts and USES them to compute the credit being
> certified**: `Dependent.citizenship_status` (five choices, help_text citing
> §152(b)(3)) and the `EducationStudent` AOTC facts. A nonresident-alien
> dependent therefore certified line 17 = "true" against the face's caution
> *"If the answer is 'No' for question 14, 15, 16, or 17, you cannot claim the
> CTC/ACTC/ODC for that child."* ⚠ **Sign: OVERSTATES the credit on a signed
> return.** All five now derive or refuse; a test fails if a literal returns.
> **⚠ THE DESIGN CORRECTION CAME FROM THE FIXTURE, NOT THE TRIAGE.** Line 4 was
> going to be keyed until ATS scenario 5 showed the Taxpayer already carries
> `eic_qualifying_child_of_another` (childless Rule 13) and
> `eic_claimed_as_dependent` (Rule 12) — and line 1 is the CURRENT tax year, so
> lines 4 and 11 ask exactly what the credit already gates on. A keyed copy
> would have been the s234 two-sources-for-one-relationship defect.
> **⚠ Unanswered is REFUSED, not assumed** — line 4 = Yes means the EIC cannot
> be claimed at all, so a fabricated "No" transmits a barred claim. The browser
> CRUD shipped in the same change so the refusal is actionable.
> ⛔ **Open:** Part II Section B (9a/9b, 10a/10b, 11a/11b) stored but not
> emitted — **the reported packet is exactly that path**; `f8862_2025.py` +
> `seed_8862.py` still cut to the draft spec's collapsed per-Part booleans; the
> item's diagnostics unbuilt.
> ⚠ RS: the `8862` spec is the s238 trap a second time — 200, `status: draft`,
> six pseudo-lines for a ~40-line face. **The export's `status` is still checked
> nowhere.** 20 tests; flow 546. ⚠ MOVEMENT: e-file output moves (conservatively).

> **2026-08-10 session 241b — 1040 BATCH-004 #6 (Form 8863 education credits).
> No migration, one deploy.** **Form 8863 gains its INPUT-LANE leg** — the last
> one it was missing. Model (`EducationStudent`, migration 0071; `dependent` FK
> 0093), compute, render, diagnostics, MeF and seed have all been green since
> the Topic-8 build; only `backentry.v1` could not carry a student, so a packet
> with a filed 8863 could not be back-entered without dropping the credit.
> New `education_students` section: one row per Part III STUDENT, who may be the
> taxpayer, the spouse or a dependent.
> **⚠⚠ THE FINDING IS A NEGATIVE ONE AND IT MIRRORS s241's.** Hours earlier s241
> closed a Form 5329 hole where compute reduced its rows to a dict keyed by
> owner, so a duplicate VANISHED — fixed with a DB uniqueness constraint. The
> obvious move here was the same constraint. **It would have been a defect:**
> `compute_8863` ITERATES a list and Parts I/II aggregate across students
> (L1 = Σ line 30, L10 = Σ line 31), so several students per return is the
> normal case and a uniqueness rule would have destroyed the multi-student
> return. *The rule is not "add the constraint" — it is **read what the consumer
> does with the row set, every time**; a dict-by-attribute is a uniqueness
> contract, a list is not, and they are one character apart at the call site.*
> Pinned by a test asserting compute still iterates.
> ⚠ **`student_name` / `student_ssn` are STORED on the row, not derived from the
> dependent link** — the renderer, `rules_8863` and BOTH MeF paths read them
> there, and `read_model` refuses a student without a name and 9 SSN digits, so
> a row missing either is un-transmittable. Both required.
> **Student→dependent link** via the `misc_1099s.link_key` carrier shape
> (`dependent_key`, by name or SSN digits); required for a dependent student,
> rejected for a taxpayer/spouse one. ⚠ An unmatched key commits UNLINKED with a
> warning — the credit is unaffected and `rules_8863` still reports in full;
> only `credit_gates.d_credit_aotc` (which filters `dependent__isnull=False`)
> loses the per-dependent message. Diagnostic specificity, not money.
> Staging also refuses a student carrying BOTH credits (§25A(c)(2)(A)) and every
> computed/aggregate line BY NAME.
> Answer key hand-derived from §25A(b)(1)/(i): $4,000 → **$2,500**, 40%
> refundable **$1,000**, nonrefundable **$1,500** — matches the filed packet;
> tied end to end onto 1040 line 29 and Schedule 3 line 3. 25 tests; flow
> assertions 526. **⚠ MOVEMENT: none.**

> **2026-08-10 session 241 — 1040 BATCH-004 #7 + BATCH-003 #2 (Form 5329 Parts
> III and VII). Migration 0278, one deploy.** **Form 5329 gains its INPUT-LANE
> leg** — the last one it was missing. Its other six legs (spec / compute /
> render / diagnostics / MeF / flow) have been green since the 2026-06-25
> `1040-5329-full-complete` tag; only `backentry.v1` could not carry it, so a
> packet with a filed 5329 could not be back-entered without dropping an excess
> contribution. **Two batch items, ONE section**: #7 asks for Part III
> (traditional-IRA excess) and #2 for Part VII (HSA excess), and they are the
> same `_excess_part()` helper inside one function. The new `form_5329s` section
> carries the preparer-keyed leaves of **all nine parts**, so no other part is
> left un-importable. Also new: the excess **roll-forward** (each part's
> total-excess line → next year's prior-excess line, in the printed face's own
> wording) and **`D_5329_006`**.
> **⚠⚠ THE DEFECT BEHIND IT — A DICT KEYED BY A ROW ATTRIBUTE MAKES DUPLICATES
> VANISH, NOT DOUBLE.** `compute_5329_db` and the diagnostics' `_f5329_state`
> both do `{r.owner: r for r in Form5329.objects.filter(...)}` — an ordinary,
> readable line that silently encodes a uniqueness contract the database never
> enforced. A second row for the same owner does not double-count; the last one
> wins and every earlier row's additional tax is **dropped**, with no error, no
> diagnostic and no rendered form. Measured before the fix: $1,840 of prior
> traditional-IRA excess plus a second taxpayer row with $250 of prior HSA
> excess wrote Schedule 2 line 8 = **$15.00, not $125.40**. *When compute
> reduces a row SET to a dict keyed by one field, that key IS a uniqueness
> constraint — and if the DB lacks it, the overflow is invisible rather than
> wrong.* The contrast is the tell: `Form8606` and `HSAAccount` ITERATE, so a
> duplicate there at least shows as a double count. **⚠ Sign: UNDERSTATES tax.**
> Closed at the DB (0278), the browser POST, the re-owner PATCH and staging.
> ⚠ Independent confirmation nobody had connected: `ReturnData1040.xsd` caps
> **`IRS5329` at maxOccurs 2** — the schema has said "one per owner" all along.
> **⚠ A FORM LINE THAT NAMES ITS OWN SOURCE IS TELLING YOU WHAT TO RECONCILE.**
> Line 44 reads *"2025 distributions from your HSAs from Form 8889, line 16"* —
> the TAXABLE portion, so a fully qualified distribution puts -0- there.
> BATCH-003 #2's packet has a fully qualified $631 distribution, which is exactly
> why its $250 excess survives; keying the gross wipes out the excess and its $15
> tax **with nothing visible changing on the face**. `D_5329_006` runs that
> sentence as a reconciliation, both directions, matched by owner. ⛔ Line 36
> says the same about **Form 8853 line 8 and cannot be reconciled — no
> `Form8853` model exists** (DEFERRAL_AUDIT; a second reason to build 8853).
> ⚠ **Part VIII (ABLE) does NOT roll forward** — its face is line 50 alone with
> no prior-year line, so the form provides no chain; whether §529A taxes an
> uncorrected ABLE excess again is agenda'd for RS, not guessed.
> Answer keys hand-derived from the printed face: #7 → line 16 = 1,840, line 17
> = 110.40 → Sch 2 L8 **110**; #2 → line 48 = 250, line 49 = **15.00**. Both tie
> end to end. 48 tests; flow assertions 526. **⚠ MOVEMENT: none.**
> Also retired three stale hand-counted literals: the two `D_RET_` registration
> counts (RED since s239 added D_RET_011 and never re-pinned them) and the
> proforma key-contract guard, which had never gained `_k1_basis_704d` or
> `_carryforward_attributes` and passed only by their absence.

> **2026-08-09 session 240 — 1040 BATCH-004 item #4 (K-1 §1231 → Form 4797 →
> Schedule 1). No migration, one deploy.** No form gains or loses a leg; the
> reported defect does not exist and two others do.
> **The report is STALE.** `D_K1_SEC1231` was retired 2026-06-30 when the
> §1231 → Form 4797 Part I line 2 feed was built, and the chain ties the filed
> return to the dollar (line 2 detail row, lines 7/11/17/18b, Schedule 1 line 4,
> 1040 line 8, AGI). Partnership and S-corp rows both feed it; the import lane
> already carries `section_1231`; the face already renders a Part I line-2 row
> per K-1. **This is the third BATCH-004 item refuted the same way** — #6 (Form
> 8863) and #7 (Form 5329 Part III) are also fully-built forms whose only gap is
> the `backentry.v1` importer. **⚠ On this queue "the schema has no X" is
> usually true AND usually not the whole story.**
> **⚠⚠ BOTH REAL DEFECTS WERE CREATED BY THAT SAME 2026-06-30 FEED, and neither
> could fail a test.**
> **(1) RETIRING A RED VOIDED A DIFFERENT MODULE'S SAFETY ARGUMENT.** RS
> `R-8582-MULTIFORM` RED-defers Form 8582 Part IX and justifies itself in
> writing: *"the common Part IX triggers (section 1231, 28%-rate) are already
> RED-deferred upstream in the K-1 router, so no NEW silent gap."* True when
> written; false from the day `D_K1_SEC1231` retired. The two guards left
> standing (`D_8582_MULTIFORM`, `D_K1_PTP_LOSS`) both tested `k1_sche_net < 0`
> — K-1 boxes 1/2/3 only — so a K-1 whose ONLY loss is its §1231 amount was
> invisible to both, and a passive $50,000 §1231 loss reduced AGI in full with
> not one diagnostic. A passive activity's §1231 loss IS a passive activity
> deduction; the v1 8582 engine gathers only the Schedule-E net, which is
> exactly what Part IX exists for. New `k1_activity_net()` (= `k1_sche_net` +
> §1231) feeds both triggers as `min(sche, activity) < 0` — a UNION, never a
> narrowing. 28%-rate and §1250 are untouched: `D_K1_SPECIAL_GAIN` still REDs
> those unconditionally, which is why the fix is §1231-only.
> **Sign: OVERSTATES the deduction.**
> **(2) A NEW FEED MADE AN OLD SEAM START REJECTING.** Schedule 1 line 4 is
> written by `compute_4797_db`, so the §1231 feed gave that line a non-zero
> value on returns carrying **no disposition at all** — a retiree with one
> partnership K-1. The 1040 MeF builder has **no `IRS4797` document** (the only
> one in the repo is the 1120-S builder), and `S1-F1040-118-01` (Reject, Active)
> requires Schedule 1's `OtherGainLossAmt` to equal Form 4797's. There is
> nothing to equal, so the return bounces. `_refuse_unattachable_form_4797` now
> refuses by name at composition (the `_refuse_unattachable_form_1116`
> precedent), keyed to a **non-zero line 4** rather than to engagement — a
> §1231 GAIN routes to Schedule D line 11, leaves line 4 at zero, and must still
> transmit. **Sign: a rejected transmission, which no reconciliation catches.**
> The `IRS4797` document itself is UNBUILT and is now a named e-file gap
> alongside `IRS1116` — and the higher-volume of the two.
> Tests: 6 diagnostics + 3 refusal. Flow-assertion gate green (603); e-file
> suite green (543).

> **2026-08-09 session 239 — 1040 BATCH-003 items #5, #7 and #4. No migration, one
> deploy (`4f924ac`).** No form gains a leg; four wrong RULES on already-built forms are
> corrected. **⚠⚠ ALL THREE ITEMS CONCEALED A DEFECT LARGER THAN THE REPORTED
> ONE**, and in each case the reported symptom was the small half.
> **#5 code U** was reported as a missing 1099-R distribution code. Verified
> from the source PDF: i1099-R (2025) Table 1 — "U—Dividends distributed from
> an ESOP under section 404(k)". The real loss is that an unsupported code
> BLANKS THE WHOLE PENSION TAXABLE COLUMN (the no-silent-gap rule), so a $7 ESOP
> dividend suppressed an unrelated $671 code-1 row — the reported $678 out of
> AGI. Deliberately NOT an early code: §72(t)(2)(A)(vi) excepts §404(k)
> dividends, which is why the filed return charged $67 and not $67.70.
> **#7 codes J/T** are now admitted CONDITIONALLY — only when an owner-matched
> Form 8606 reports Part III Roth facts; fail-closed otherwise, so D_RET_003
> still fires with no 8606 and a spouse's 8606 does not admit the taxpayer's.
> **⚠⚠ ADMITTING THEM ALONE WOULD HAVE TAXED THE DISTRIBUTION TWICE.** Two
> instructions disagree on their face: a Roth 1099-R carries the box-7
> IRA/SEP/SIMPLE checkbox FALSE (i1099-R Box 7 — "Do not check the box for a
> distribution from a Roth IRA that is not a Roth SIMPLE IRA") while the 1040
> reports it on lines 4a/4b (i1040 "Lines 4a and 4b" — "an IRA includes a
> traditional IRA ..., Roth IRA ..., and a SIMPLE IRA"), which is also where
> Form 8606 line 25c lands. The gross sat on 5a and the taxable on 4b; and box
> 2a is blank on a Roth BY INSTRUCTION, so the no-basis fallback would have
> taxed $3,600 in full on 5b while the 8606 wrote $0 on 4b. `doc_is_ira_path`
> is now the single source for IRA-vs-pension; `doc_taxable` returns zero for
> any Roth code (a §408A(d)(4) ordering result is never a box-2a fact). The
> §72(t) half is DECLARED, not omitted — new `D_RET_011` (error; the sign
> understates tax) fires only when Part III is actually taxable, because
> feeding Form 5329 line 1 from the blank box 2a would charge 10% of the GROSS,
> a penalty on returned contributions.
> **#4 Georgia RIE — FOUR defects, and the reported one is the smallest.**
> Authority fetched verbatim: Ga. Comp. R. & Regs. r. 560-7-4-.02(4)(b)1.
> **⚠⚠ ONE SENTENCE, TWO DIFFERENT TESTS** — partnership income is earned when
> "subject to Federal FICA tax or Federal self employment tax"; S-corporation
> income is earned when the taxpayer "materially participates". The engine
> applied the S-corp test to both, and they routinely disagree (§1402(a)(13): a
> limited partner's share is outside the SE base however much they
> participate). It costs money because the earned portion is capped at $5,000
> and the unearned portion is not, so $2,252 was absorbed by the cap and
> vanished. The K-1's box 14 code A states the fact. ⚠ **Sign is not
> character** — the test is `14A != guaranteed payments`, not `> 0`, because a
> general partner's SE LOSS is equally SE-subject; that is what reconciles
> s236's filed worksheet (mat.-part. partnership LOSS as earned) with this
> one's (partnership INCOME as unearned). Both are right; different partners.
> Three further feeds were reaching NO RIE line at all: the K-1's own box-5
> interest and box-6a dividends (the reg's unearned list opens with "interest
> income, dividend income", no entity qualifier), guaranteed payments (§1402(a),
> always earned), and — pre-existing — RIE L11 summed box 2a while an engaged
> Form 8606 SUPERSEDES federal 4b, so a basis recovery inflated the base.
> New `D_GA500_018` (warning) on a blank box 14A; ⚠ the default there ENLARGES
> the exclusion and understates Georgia tax, so it is loud.
> ✅ Closed a carried RS item at no cost: `GA_RIE_EARNED_CAP` $5,000 vs the
> reg's $4,000 — **the statute controls**, §48-7-27(a)(5)(E)(i) says $5,000
> twice; the reg's figure is pre-2011-amendment. The code was always right.
> ⚠ Two s236 tests and one flow assertion changed and NEITHER was weakened:
> the s236 fixtures never stated box 14A (they key it now, making them general
> partners, which is what their filed worksheet shows), and FA-1040-RET-07
> asserted a blocked code-J doc blanks 5b — a code-J doc is a Roth and blanks
> 4b, so it had been passing while naming the wrong line.
> ⚠ Retired a stale literal in passing: `test_ga500_diagnostics_leg`'s
> hand-counted `len(codes) == 17` now asserts the DB set equals `RULES_GA500`.
> 18 new tests + 2 in the s236 file. Flow-assertion gate green (526).

> **2026-08-09 session 238 — 1040 BATCH-002 #6, Form 8379 (Injured Spouse
> Allocation). Two migrations (0276 model, 0277 RLS), one deploy.**
> **A NEW FORM GAINS ALL SEVEN LEGS AT ONCE** — model, compute, AcroForm
> render, diagnostics, MeF document, import lane, browser card. Nothing
> existed before: the only trace of Form 8379 anywhere was
> `Form8888.filed_form_8379`, a boolean firing `D_8888_8379`.
> **⚠⚠ THE RS SPEC RETURNED 200 AND WAS STILL NOT THE SPECIFICATION.** The
> 404-STOP gate never fired, which is what made reading it first worth doing:
> the export is `"status": "draft"` with a **4-entry `line_map` for a NINE-line
> Part III grid**, and its `R-8379-ALLOC` states the column constraint for TWO
> of the nine. Building it as written ships **seven unguarded reject
> conditions** — every line carries its own Active rule (13a F8379-015 · 13b
> -004 · 14 -016 · 15 -005 · 16 -024 · 17 -023 · 18 -017 · 19 -018 · 20 -009) —
> plus no exactly-one-injured-spouse pair (-001/-002), no MFJ requirement
> (-013), no foreign/PR/VI address bars (-011, -012-01), no barred-companion
> list (-019-01). The s222/s223 shape: face + `IRS8379.xsd` 2025v5.4 + the 18
> `F8379-*` rules are the real spec, written up in `specs/_8379_source_brief.md`.
> **⚠⚠ COLUMN (a) IS DERIVED, NEVER KEYED** — "Amount shown on joint return"
> restates the return the form rides on, so all nine come from the 1040 and the
> lane refuses an `l*_joint` key BY NAME. Two of the nine are settled by the
> XSD, not the face: **line 13a is 1040 line 1a (both named `WagesAmt`), not
> the 1z total** — 1z also carries tips and household wages, none of them "on
> Form(s) W-2" — and **line 20 is `EstimatedTaxPaymentAmt`, i.e. line 26**, not
> total payments, which would double-count line 19's withholding.
> **⚠⚠ COLUMN (c) IS DELIBERATELY *NOT* DERIVED as `a − b`.** Deriving it
> satisfies all nine balance rules by construction and throws away the filed
> packet's own redundancy; keying both makes `a != b + c` catch a keying slip
> OR an app return that does not match the filed one.
> **⚠⚠ ADDING THE FORM MADE AN EXISTING RULE WRONG, fixed in the same pass** —
> "is a Form 8379 on this return?" now has TWO sources, and `d_8888_8379` /
> `analyze_8888` / `_extract_f8888` all read only the checkbox. A return with a
> genuine Form 8379 and the box unticked would have kept a refund split the IRS
> rejects outright. One shared `form_8379_present()`; both directions pinned.
> **Answer key hand-derived from the packet's own filed 8379**, and line 15's
> $15,750 halves checked against the app's OWN 2025 MFJ constant (31,500):
> 13a 88,080 = 51,360 + 36,720 · 13b 183 = 92 + 91 · 14 175 = 0 + 175 ·
> 15 31,500 = 15,750 + 15,750 · 16 2,700 = 0 + 2,700 · 17 8,060 = 4,030 +
> 4,030 · 19 2,385 = 575 + 1,810.
> ⚠ Boundary stated, not implied: four of the six forms `F8379-019-01` bars
> (8833, 4563, 5074, 8689) are not modeled at all — DEFERRAL_AUDIT.
> ⚠ Retired a stale literal in passing: `test_tts_forms`'s hand-counted
> `len(forms) == 100`; three prior sessions record missing that re-pin.
> 64 new tests (the MeF document validates against the real XSD, which caught
> a hyphenated SSN in the fixture). Regression 230 / 726 / 481 green.

> **2026-08-11 session 242y — ✅ IRS1116: THE OLDEST E-FILE GAP CLOSES.
> No migration, one deploy (`cc5eae3`).** The s237 holding refusal below is
> REPLACED by `_extract_f1116` + `build_irs1116` — a full-path Form 1116
> with a resolvable country now transmits (single country in column A,
> the render's exact line set; `compute_1116_result` the shared single
> source, so print and transmit cannot disagree). The §904(j) election
> paths stay DOCUMENTLESS by law. The refusal's doctrine survives as
> NAMED refusals: no/unresolvable country (the CountryType FIPS enum,
> resolved through the XSD's own table; "RIC" rides the header code),
> and the F1116-012/-013 statement demands (a nonzero line 2 or 3b needs
> an itemized statement the app cannot compose from one total).
> F1116-006-01 (line 33 == line 24 with one form) holds arithmetically —
> pinned; "1099 TAX" replaces the per-country date when the whole tax is
> the 1099 aggregate. 9 MeF tests + the flipped both-ways backentry pins.
> Remaining e-file gap: amended MeF only.

> **2026-08-08 session 237 — 1040 BATCH-002 #4 (the individual foreign tax
> credit). No migration, one deploy.** No form gains a compute or render leg;
> one form becomes IMPORTABLE and one e-file seam becomes HONEST.
> **⚠⚠ TWO THIRDS OF THE REPORTED GAP WAS ALREADY CLOSED.** The item says the
> import lane has "no source-level foreign-tax amount on interest or dividend
> rows" and "no supported direct input for Schedule 3 line 1". Both REFUTED:
> `foreign_tax_paid` has been in `INT_FIELDS` AND `DIV_FIELDS` since the lane's
> first commit (`4926fa4`), and the **§904(j) de minimis AUTO-election** has
> landed `min(foreign tax, regular tax)` on Schedule 3 line 1 — no Form 1116
> engaged, no face — since 2026-07-01. That IS the reported shape (a $97 credit,
> no printed 1116 in 43 pages), so the reported packet imports today unchanged.
> A direct line-1 input would be a defect: the credit is DERIVED, and an
> importable line 1 lets a payload contradict the engine. Both now pinned.
> **Form 1116 (input leg) — the FULL §904 path becomes importable.** The real
> gap: every fact that FORCES the full limitation (tax over the $300/$600
> ceiling, a K-1's foreign tax, a §904(c) carryover, a non-passive category)
> lives on the `Form1116` row, and the lane could not create one — those packets
> were un-importable outright. New `form_1116s`, the lane's **first SINGLETON
> section**. ⚠ The trap the next one will hit: a **OneToOne reverse accessor
> RAISES when absent and has no `.count()`**, so the merge gate, the replace
> pass and the cleanup persistence gate now all read one `_section_qs` helper.
> **⚠⚠ A SECOND GAP THE AUTO-ELECTION HAD OPENED** — `ftc_deminimis_optout` is
> now importable. It was not, and the auto-credit fires on ANY 1099 foreign tax
> under the ceiling, so a packet whose preparer deliberately did NOT claim it
> imported WITH a credit the filed return lacks and nothing could suppress it.
> ⚠ Sign: the lane OVERSTATED the refund, and reconciliation blamed the engine.
> **⚠⚠ Form 1116 (e-file leg) — the missing document is now REFUSED, not
> silently filed.** Schedule 3 line 1 transmits as a bare `ForeignTaxCreditAmt`
> and there is still **no `IRS1116` builder** (the s148 sweep's 4th
> missing-document occurrence). Correct on the §904(j) paths — no Form 1116 is
> due — but a FULL-path return would file a credit with its required form
> missing, and **no MeF business rule catches it** (every `F1116-*` rule in
> `1040_Business_Rules_2025v5.3.csv` presumes the form is present). Now
> `extract_return` refuses by name. A refusal, not a fix: the full path was
> already paper-only in fact and is now paper-only in code. **Building `IRS1116`
> is the follow-up.**
> **Answer key hand-derived, not read back:** single, $90,000 wages, $12,000
> foreign source income, $2,500 foreign tax → L3f 0.1333, L3g 2,099, L7 9,901,
> L19 0.1333, **L20 11,255 — the TAX TABLE's $50-band midpoint, not the bracket
> formula's 11,249** — L21 1,500 = the §904 cap, credit 1,500. That $6 is why
> the key was derived by hand first.
> 14 new tests. Lane suite 181 green, efile+mef 469, flow assertions + the four
> 1116 legs 574.

> **2026-08-08 session 236 — 1040 BATCH-002 #3 and #8. One migration (0275),
> one deploy.** No form gains or loses a leg; two existing legs stop being
> silently wrong, and one form's INPUT leg closes.
> **GA-500 (compute leg) — the RIE worksheet's line-13 K-1 feeder now reads the
> AFTER-§469 amount.** It read each K-1's RAW box amounts, so a passive
> partnership loss that Form 8582 suspended in FULL — never in federal AGI at
> all — still shrank the Georgia exclusion dollar for dollar. Schedule E PAGE 1
> has read the post-§469 result since s199; this is its page-2 twin. New
> `k1_sche_included(k1)` returns what one K-1 actually contributed to Schedule E
> line 32/37, term for term with `schedule_e_p2_totals_from_rows`, and a test
> pins the two against each other so Georgia can never disagree with the federal
> face. Authority: Ga. Comp. R. & Regs. r. 560-7-4-.02, fetched and quoted —
> *"Only retirement income that is included in Georgia taxable income shall be
> included when computing the retirement income exclusion."* ⚠ Sign check: a
> suspended loss leaving the base RAISES the exclusion and LOWERS Georgia tax.
> ⚠⚠ **The reported ANSWER is refuted, and the packet's own forms prove it** —
> filed spouse RIE $7,672 is unreachable (Form 8582 Part VIII: allowed $0;
> Schedule E line 41 is the taxpayer's −$2,948 alone; no $2,948 of spouse
> partnership INCOME exists anywhere). The correct figure is $4,724. **Georgia
> will NOT tie on that packet, by $153, and should not.**
> **Schedule K-1 (input leg) — CLOSED for pass-through charitable
> contributions.** Three fields by §170(b) bucket (1120-S box 12 / 1065 box 13
> codes A, C, E), migration 0275 with `db_default`.
> **Form 7203, 1040-side (compute leg) — Part III line 42 is now FED.** The
> shared engine has mapped 42 to K12a/K12b since s205 and the MeF builder
> already emitted `CharitableContributionAmt`, but `ScheduleK1` had no
> charitable field, so a shareholder's contribution reduced NOTHING and ending
> stock basis ran high by exactly that amount. Reported case reproduces to the
> dollar: 8,058 before deduction items, $700 allowed, ending basis $7,358.
> **Schedule A (compute leg) — lines 11/12 now COMPOSE.** The K-1 buckets ADD to
> the flat facts (different contributions, not a competing measurement of one —
> deliberately NOT the 8283 flat-wins rule); `D_SCHA_012` (info) shows the
> composition. The engagement gate now also fires on a K-1-only gift, which
> previously produced `blank_rows()` and lost the deduction entirely.
> ⛔ **Box 12 codes B, D, F and G still have NO bucket and are REFUSED** at both
> write paths — the same gap `D_SCHA_007` RED-defers on the taxpayer side. ⚠
> Sign: a refused code is NOT deducted at all, so it OVERSTATES tax. Logged in
> DEFERRAL_AUDIT; needs `R-SCHA-CHARITABLE` amended to model all seven.
> ⚠⚠ **Form 7203 (render leg) — the field map's Part III COMMENTS were a row
> off** from line 38 down (`42: Section 179 deductions`, `43: Charitable
> contributions`), contradicting the compute engine beside them. **Nothing ever
> rendered wrong** — read positionally off `f7203.pdf`, every `LineNN[0]` widget
> sits on the row printed NN, and the MeF builder agrees; the AcroForm NAMES
> were right. A fossil of a pre-2022 face that split ST and LT capital losses.
> Comments corrected and a test now anchors each row to the PDF's own printed
> text, proven by injecting the shift.
> 27 new tests; both units proven by revert. 526 flow assertions green.

> **2026-08-08 session 235 — the attribute-preservation layer + BATCH-002 #7.
> ONE new table (`CarryforwardAttribute`), migrations 0272/0273/0274, TWO
> deploys.** No form's compute leg CHANGED status; one new statement page.
> **New — Carryforward Attribute Worksheet (render leg).** No IRS face; a
> supporting statement listing every preserved future-year pool by source year.
> It exists because the defect the five batch items report is INVISIBLE — the
> current-year face ties, and the discarded attribute leaves no wrong number to
> notice, only an absent one.
> **New surface, deliberately NOT a compute leg — `CarryforwardAttribute`.**
> One row per POOL (kind + source year + §170(b)(1) limitation class + owner +
> description), with a back-entry section, browser CRUD, per-vintage
> roll-forward and five `D_CFWD_*` rules. ⚠⚠ **The rows FEED NO TAX LINE** and
> a test pins byte-identical output with and without them: §179 and charitable
> already have an engine field that computes the current year, and a second
> source for one relationship is the s234 Form 8960 line-4b shape.
> `D_CFWD_002` reconciles rows against the engine field; `D_CFWD_001` fires
> **error** for any kind no engine computes.
> ⛔ **Form 172 / regular NOL has NO leg and cannot get one yet — there is no
> Rule Studio spec** (`lookup/172/`, `lookup/NOL/`, `lookup/FORM_172/`,
> `lookup/1045/` all 404). Preservation is built; the computation is BLOCKED at
> the 404-STOP gate. Same for the Schedule A per-vintage charitable ordering,
> whose RS rule models only an aggregate.
> **Form 4562 (input leg) — CLOSED for the §179 carryover.** BATCH-002 #1 was a
> LANE-ONLY gap: compute has owned lines 9/10/11/12/13 since the depreciation
> build (4562 R001/R014/R015) and proforma already rolled line 13 forward, but
> the batch lane could not carry line 10, so a packet's carryover vanished on
> import. `sec_179_carryover_prior` + `taxable_income_limitation` are now
> importable; line 13 stays out as a computed output.
> **Form 1040 dependents (input leg) — the relationship enum now mirrors the
> MeF authority**, `IRS1040.xsd` (2025v5.4) `DependentRelationshipCd`. Seven
> codes had no app value: STEPCHILD, HALF BROTHER, HALF SISTER, PARENT,
> GRANDPARENT, AUNT, UNCLE, plus NONE. `adopted_child` stays unmapped and still
> refuses at compose (no such code; §152(f)(1)(B) makes it a child by blood, so
> the return must say SON or DAUGHTER).
> ⚠⚠ **Schedule 8812 (compute leg) — the CTC relationship test was a BLACKLIST
> and adding those values would have broken it.** `relationship not in ("",
> "other")` was correct only because every value that existed happened to
> satisfy IRC §152(c)(2); a dependent AUNT under 17 would have taken a $2,200
> credit. Now the statute enumerated as a whitelist, read by all THREE copies
> (`compute_8812`, `credit_gates`, `rules_8812`).
> **GA-500 (compute leg) — line 7a is now DERIVED from the federal Dependent
> rows.** It was a bare preparer entry, so every imported Georgia return
> silently carried zero dependents and lost $4,000 of line-14 exemption apiece
> — $207.60 at the 2025 rate, which is exactly the reported $207. O.C.G.A.
> §48-7-26(b)(3)/(a): the exemption is "for each dependent", and "dependent"
> comes from the IRC, so CTC-vs-ODC never enters into it. Written
> unconditionally including back to blank; a typed or batch-sent 7a still wins.
> ⚠ Sign check: this LOWERS Georgia tax — correctly.
> ⛔ **GA-500 (render leg) — line 7d, the per-dependent detail table, is still
> NOT rendered.** The overlay carries only the 7a/7b/7c counts. No tax effect;
> logged in DEFERRAL_AUDIT.
> Statute verified this session (§152(c)(2)/(d)(2)/(f)(1)/(f)(4)). 67 new tests;
> producer, reader and every diagnostic proven by revert or defect injection;
> all 526 flow assertions green.

> **2026-08-08 session 234 — 1040 LANE: BATCH-002 opened; items 2 and 5 built.
> No new form, no migration. One deploy. No form's leg status changed** — the
> 1040 standard-deduction chain and Form 8960 were both already built; this
> session fixed defects inside them.
> **1040 (compute leg) — the dependent standard deduction had a correct chain
> and a starved input.** `R-STD-04` was right; the spec fact
> `dependent_filer_earned_income` is a non-null Decimal defaulting to 0 and
> **nothing anywhere populated it**, so every dependent filer with wages took
> the bare $1,350 minimum. **The authority for the DERIVATION is not the RS
> spec** — R-STD-04 is silent on where worksheet line 1 comes from. The
> **Instructions for Form 1040 (2025), p.35** footnote gives it verbatim:
> earned income is "the total of the amount(s) you reported on Form 1040 or
> 1040-SR, line 1z, and Schedule 1, lines 3, 6, 8r, 8t, and 8u minus the
> amount, if any, on Schedule 1, line 15" (§63(c)(5)(B) is bare; Pub 501 (2025)
> Table 8 supplies the $1,350/$450 worksheet and "If none, enter -0-").
> ⚠ Two more defects fell out of the one fix: line 12 was being written BEFORE
> Schedule 1 existed (now re-settled on the final Schedule 1), and
> `proforma._sr_py_facts` derives `did_itemize` as `line12 > std`, so a starved
> standard deduction made **every dependent filer with wages look like they had
> ITEMIZED** in next year's §111 state-refund worksheet.
> ⚠ Sign check: this LOWERS federal tax on affected returns — correctly.
> **Form 8960 (compute leg) — the reported cause was REFUTED and a worse one
> found.** The form already engaged on interest-only income and already saw K-1
> portfolio interest. The defect: line 4b's auto back-out is derived from the
> K-1/rental MODELS while line 4a is read from Schedule 1 line 5 — two sources
> for one relationship — so when they disagreed the subtraction ran past 4a and
> consumed **portfolio interest**, driving NII negative and DISENGAGING the form
> entirely (no line 17, no Schedule 2 line 12, no NIIT). **Instructions for Form
> 8960 (2025), Line 4b** say it twice: it adjusts "the amounts included on line
> 4a". Now clamped to 4a in BOTH directions.
> ⚠ Sign check: this RAISES tax — correctly; the NIIT was being skipped outright.
> Both fixes proven by revert. 25 new tests; all 526 flow assertions green.

> **2026-08-08 session 233 — 1040 LANE: the CC queue reopened (2 batch files /
> 20 items); BATCH-001 worked 4 of 10. No new form, no migration. One deploy.**
> **No form's leg status changed** — GA-500 and the diagnostics engine were both
> already built; this session fixed defects inside them.
> **GA-500 (compute leg) — three defects in ONE derive, all confirmed in code
> before a line was changed**: the RIE base never received a Form 1041 K-1's
> `other_portfolio` (box 5); RIE line 6 was fed the FULL federal interest, so
> U.S.-obligation interest already subtracted on Schedule 1 line 10 was
> subtracted a second time through the exclusion; and an owner-tagged capital
> LOSS was split 50/50 onto the spouse because the line-9 weighting helper
> skipped `amt <= 0`.
> **⚠⚠ THE AUTHORITY IS NOT THE RS SPEC** — `R-GA500-RIE` covers neither
> question. **Ga. Comp. R. & Regs. r. 560-7-4-.02** settles all three verbatim:
> "Only retirement income that is **included in Georgia taxable income** shall
> be included when computing the retirement income exclusion" and "**One spouse
> may not use any income attributable to the other spouse**". O.C.G.A.
> §48-7-27(a)(5)(A) opens the same way. Both rules are agenda'd for the spec.
> ⚠ **Sign check on the movement**: line 13 grows the exclusion, line 6 SHRINKS
> it (this RAISES Georgia tax — correctly, the dollars were deducted twice), and
> line 9 reallocates on MFJ all-loss years with joint/untagged pinned unmoved.
> **Diagnostics engine — an engine fault was wearing a taxpayer's face.** Twelve
> infrastructure failures were recorded as error-severity findings on a real
> return and the run was still stamped COMPLETED (`RunStatus.FAILED` existed and
> had never been used). Import failures and evaluation failures are now
> distinguished and labelled `details.fault = "engine"`; a run in which any rule
> failed to reach a verdict is FAILED; and a Django system check fails
> `manage.py check` on a rule the build cannot load. ⚠ The check validates the
> CODE-SIDE registry deliberately, NOT the DB rows — the DB is shared with prod,
> so hard-failing startup on a row would let a laptop-seeded rule take
> production down.
> **⚠ Form 8853 Section C is UNCHANGED and still PENDING all four legs** — s233
> did not touch it; the queue took precedence.
>
> **2026-08-08 session 232 — RS LANE: FORM 8853 SECTION C AUTHORED, GATE-1
> APPROVED, SEEDED, EXPORTED, CACHED. No app code, no migration, no deploy.**
> Ken picked spec-first for his last working day before a 10-day absence
> (08-09 → ~08-19), so the day went on the one thing that needs him and the app
> build can now run unattended. Scope is his s224 ruling item 4: **Section C
> only** (long-term care + accelerated death benefits); the Archer / Medicare
> Advantage MSA sections stay out of season-one scope and Form 8889 line 4 stays
> keyed under `D_8889_ARCHER`.
> **⚠⚠ Schedule 1 line 8e is a COMPOSED line, and the IRS's own schema says so** —
> its MeF element is `TotArcherMSAMedcrLTCAmt` (Archer MSA + Medicare Advantage +
> LTC) and the face says "include this amount in the **TOTAL** on line 8e". The
> s230 Schedule-K-13g ruling therefore governs: the writer is a REGISTRY, not
> whichever form arrives first. **⚠⚠ THE STATUTE CORRECTED THE DRAFT** —
> §7702B(d)(2) verbatim makes the limitation "the **excess (if any)** of (A) …
> over (B)", which floors line 25 at zero, and **the face prints no floor there**.
> Drafted unfloored off the face plus a paraphrase fetch that had silently dropped
> the phrase; a second fetch caught it, and the defect was live (line 26 = 15,000
> against line 20 = 10,000 — taxing more than was received). See the Form 8853
> Section C row for the full statement.
>
> **2026-08-08 session 231 — FORM 3800 PART III PASS-THROUGH ROWS 1c / 4h / 4i
> + THE §38(c)(5) ELIGIBLE-SMALL-BUSINESS DETERMINATION. Migration 0271.**
> The unit Ken promoted off the s230 note: one build unblocking BOTH entity-lane
> credits. Form 8941's K-1 box 13 code BA and Form 6765's code M each reached a
> shareholder's 1040 and dead-ended — **the recipient `ScheduleK1` had no credit
> field at all**, so the amount had nowhere to land.
> **Verify-first**: Form 3800 was already substantially built (compute/render/
> diagnostics/seed, 2026-07-04), so this is a GAP unit, not a from-scratch one.
> **Row assignments are FACE-verified** against `resources/irs_forms/2025/
> f3800.pdf` — page 3 line **1c = Form 6765**, page 4 **4h = Form 8941**,
> **4i = Form 6765 (ESB)**. ⚠ The RS 3800 spec is WRONG here and was NOT
> copied: its three newly-exported `line_map` rows `1a`/`1b`/`1c` describe the
> PRE-2023 Part I structure ("General business credits from Part I"), carry no
> facts and no rules, and there is no `4h`/`4i` row or §38(c)(5) rule anywhere
> in it. The destinations came instead from the two source forms' own approved
> DEST rules — R-6765-DEST ("ESB → Form 3800 Part III 4i; others → 1c") and
> R-8941-DEST ("all others (1040/1120) → Form 3800 Part III line 4h") — plus
> the face and the statute. All five spec defects are on the RS agenda.
> **THE LAW (26 U.S.C. §38, read from the statute — it corrected recollection
> twice):** §38(c)(4)(B)(ii) makes a §41 credit a *specified* credit only for
> an eligible small business "as defined in **paragraph (5)(A)** after
> application of the rules of **paragraph (5)(B)**" (not (5)(C)/(5)(D));
> §38(c)(5)(A) sets the ≤$50,000,000 three-year average gross-receipts test
> with §448(c)(2)/(3) rules; and **§38(c)(5)(B) applies that test to the
> SHAREHOLDER, not the S corporation** — which is why it is ONE per-return
> preparer assertion (`Taxpayer.f3800_esb_gross_receipts`), not a per-K-1
> field. §38(c)(4)(B)(viii) makes §45R specified outright, so row 4h takes no
> ESB test — pinned by a test that exercises all three ESB answers so a shared
> router cannot pass by accident.
> **⚠ THE SIGN**: a specified credit has TMT treated as zero
> (§38(c)(4)(A)(ii)(I)), so row 4i is the MORE permissive placement. An
> UNANSWERED ESB determination therefore routes to the REGULAR row 1c —
> guessing can only under-allow, never over-allow. Pinned as an inequality
> (`conservative <= permissive`), not just a value.
> **Legs**: `ScheduleK1.credit_research_41` + `credit_small_employer_health_45r`
> and `Taxpayer.f3800_esb_gross_receipts` (0271, `db_default` on the decimals)
> → `form_3800_k1_credit_rows` returning (routable, deferred) → the rows joined
> `_REGULAR_ROWS`/`_SPECIFIED_ROWS`, which the renderer now IMPORTS rather than
> restating (it kept a private copy — the seam that silently drops a new row)
> → field map Line1c/Line4h/Line4i incl. column (c) pass-through EIN → three
> new `D_3800_007/008/009` (zero code collisions) → serializer + the K-1 box
> registry + the Form 3800 tab's ESB selector.
> **The loop closes**: `k1_import` now carries the s230 synthetic `K13g_M` /
> `K13g_BA` share keys onto the recipient's credit fields, so an entity 6765 /
> 8941 imports straight through to the shareholder's Form 3800.
> **§469 posture reuses the K-1's own `material_participation`** — the same
> question, already answered per activity, and non-null, so a K-1 credit can
> never sit in the D_3800_002 unanswered state.
> **Declared limits, each a RED rather than a silent gap**: `D_3800_007` (ESB
> unanswered → row 1c), `D_3800_008` (a 1065/1041 K-1 credit is EXCLUDED — the
> 1065 box-15 letter is still `[UNVERIFIED]` in the 6765 spec and unnamed in
> the 8941 spec, so it is never placed on a guessed row), `D_3800_009` (two
> entities on one row needs the face's column (a) + Part V, out of scope).
> **⛔ SEPARATE DEFECT FOUND, NOT FIXED — §38(c)(6)(A)**: line 13 uses a flat
> $25,000 threshold, but an MFS filer whose spouse also has a business credit
> gets **$12,500**. A smaller threshold means a LARGER line 13 and LESS credit,
> so the current code OVER-allows in that case. It needs a preparer assertion
> about the SPOUSE's return, so it is its own unit — queued, not folded in.
> Regression: NEW `tests/test_form3800_passthrough_esb.py` (20) + 2 new render-
> leg tests; the row-identity test is anchored to the IRS's own semantic
> subform names AND the printed row labels, because the band test derives its
> expectation from the same map entry it renders with and could only ever prove
> the renderer honours the map. Three negative controls each OBSERVED FAILING
> (ESB routing disabled → 2 exact reds; §45R made ESB-sensitive → 1; row 4i
> pointed at Line4j → the face-anchored test caught it).

> **2026-08-07 session 230 — FORM 6765 BUILT (§41 credit for increasing
> research activities; RS spec approved, Ken ruled BUILD — DECISIONS "Scope +
> gate rulings" item 3). Migrations 0269 + 0270.** The unit is complete on the
> 1120-S: input (`Form6765` singleton + the `form-6765` GET/PATCH endpoint +
> `SlateForm6765Screen` under the `form_6765` tab) → compute
> (`compute_6765.py`, R-6765-METHOD/QRESUM/REG/ASC/280C/DEST/SECTE; all six
> spec pins F6765-T1…T6 matched to the dollar on the first run) → render
> (`f6765` AcroForm, Rev. 12-2024 — a CONTINUOUS-USE form, so the 12-2024
> revision IS the TY2025 face; every page-1/page-2 widget mapped, both pages
> visually verified) → flow (line 30 → Schedule K line 13g → K-1 box 13
> **code M**).
> **⚠⚠ THE ARCHITECTURAL PIECE — K13g IS NOW COMPOSED, NOT OWNED.** Schedule K
> line 13g had exactly one writer (Form 8941's §45R credit, code BA) and a
> boolean bridge-gate (`k13g_is_8941_sourced`) read by the printed K-1, the MeF
> K-1 mapper, and the read model. Adding a second credit form to the same line
> would have STOMPED it — the RS spec's R-6765-DEST says so in as many words.
> New `apps/returns/k13g.py` is the single registry: the entity line is the
> SUM of every engaged source, and **each source is allocated to shareholders
> in its OWN right** through the existing residual-offset allocator (synthetic
> `K13g_BA` / `K13g_M` share keys) — so Σ over shareholders of each code equals
> that form's entity amount EXACTLY, which a proportional split of an
> already-rounded K13g share could not guarantee. Reconcile-or-refuse is
> preserved: once the stored K13g stops equalling Σ sources, NO code is
> emitted and both the print and the MeF mapper refuse, as an un-sourced K13g
> always did.
> **Declared limits, each a RED diagnostic rather than a silent gap**
> (12 rules, `D_6765_*`): Section D payroll tax election (Ken D-16 #3),
> controlled groups (Ken D-16 #4), estate/trust line 31, and — APP-ADDED
> beyond the spec's ten — `D_6765_METHOD_MISSING` (QREs with no method chosen
> computes nothing) and `D_6765_DEST_UNWIRED` (only the 1120-S Schedule K route
> is built; Form 3800 Part III 1c/4i needs the §38(c)(5) ESB determination and
> the 1065 box-15 letter is still UNVERIFIED in the spec). Section G is
> OPTIONAL for tax years beginning before 2026 and REQUIRED after 2025
> (i6765 verbatim) — out of scope, `D_6765_G_TY2026` carries the re-authoring.
> An engaged 6765 REFUSES MeF composition: there is no IRS6765 schema on disk,
> and transmitting a Schedule K credit with its substantiating form silently
> omitted is worse than refusing.
> Regression: `server/tests/test_form_6765.py` (27, incl. the spec's T1–T6
> verbatim and the compose-never-stomp pin) +
> `client/.../slateForm6765Screen.test.tsx` (11, incl. the fixed-base
> percent ⇄ fraction round-trip both ways — a 100× slip there would silently
> move the base amount).
> ⚠ Movement class: **none on existing returns.** Nothing writes K13g unless a
> credit form is engaged, and with only an 8941 engaged the composed sum is
> that 8941's line 16 — byte-identical to the old single-writer behavior
> (pinned by the unchanged 8941/MeF suites, 103 green).
> ⚠ Date note: the s228/s229 entries below are labelled 2026-08-08; their
> commits are both dated **2026-08-07**. The 08-08 labels are wrong.

> **2026-08-08 session 228 — K1_BASIS_704D BUILT (mixed-entity pilot #7, the
> batch's last open item). Deploy `165c972`; migrations 0267 + 0268. No new
> form tick — the partner §704(d) basis limitation is a WORKSHEET on the
> recipient K-1 (Schedule E page 2), deliberately with NO render/MeF leg:
> the spec's R-K1B-CARRY ruling is that the Partner's Instructions make
> basis tracking the partner's own record, never an attachment, so
> storing-and-surviving IS the complete chain.** Leg detail:
> `K1Basis704dWorksheet` (one per 1065 K-1; six preparer-asserted figures) →
> the `max(raw, −allowed)` cap applied ONCE in `k1_sche_net()`
> (materially-participating partners only; a passive basis-limited K-1 keeps
> the 8582 path and warns) → five `D_K1B_*` diagnostics (ARITH = an
> unacknowledgable error; UNASSERTED succeeds the old D_K1_BASIS warning for
> 1065 rows, which now covers only 1120-S/1041) → BOTH lanes (browser panel
> + `basis-704d` upsert endpoint; nested `basis_704d` lane object, schema
> regenerated) → persistence (proforma `_k1_basis_704d` + roll-forward seeds
> a shell K-1 + everything-still-suspended worksheet). FA-1040-K1B-01…05
> active in the flow-assertion gate. Regression:
> `server/tests/test_k1_basis_704d.py` (25) incl. the spec's T1–T7 verbatim
> and the pilot movement pin (line 41 −26,850 → −10,621, Δ +16,229 → Sch 1
> line 5 → AGI; QBI consumes the source −10,621 un-double-limited).
> ⚠ Movement class (intended): only a 1065 K-1 whose worksheet is SAVED
> moves; the flag-only checkbox still moves nothing, now with
> D_K1B_UNASSERTED naming the gap.

> **2026-08-07 session 227 — 1040 CC BATCH-001: all ten items, one deploy
> (`9b9673c`; migration 0266). No new form ticks — every item deepened a form
> already covered.** Leg-level changes worth recording here:
> **Schedule 1 line 24k** is no longer entry-only — it is now ENGINE-FED from
> the new recipient-K-1 §67(e) field (`ScheduleK1.excess_deductions_67e`,
> 1041 box 11 code A, both lanes, the line-18 feeder convention).
> **Form 7203 (1040-side)** Part I line 8a is no longer structurally zero
> (`nondeductible_expenses` → K16c), and the worksheet is now IMPORTABLE as a
> nested `form_7203` object on its S-corp K-1 row — the stated input boundary
> shrinks to line 3k (tax-exempt income). **Form 8959** now engages on a
> single-W-2 aggregate-only packet over the $200k flat arm (derivation, not a
> rule change). **Form 1040 line 36** (applied-forward election) gained its
> first input surface outside the browser (`f1040_fields`), with 35a/36 now
> reconcilable in the lane's answer key. **Form 2210**'s documented-source
> trio + `t2210_prior_full_year` became importable. **GA-500 RIE** line 9 can
> now honor a carryover's T/S/J owner in the carryover-only MFJ case.
> Regressions: `test_1040_batch001_s227.py` (30) + 2 in the RIE file.
> California Form 540 confirmed OUT of scope (SEASON_PLAN locked scope #1).

> **2026-08-07 session 225 — NZ #5 (SCHEDULE F) + NZ #6 (SS LUMP-SUM): both
> import lanes closed. The NZ list goes 7 → 9 of 10. No new form ticks — both
> forms were already covered; this closes their import legs. Commit `18b4db2`;
> NO migration (no model changes).**
>
> *⚠⚠ FOUR CONSECUTIVE NZ ITEMS HAVE NOW BEEN LANE-ONLY GAPS* (#9, #4, #5, #6).
> For Schedule F the models, `compute_schedule_f` (lines 1c/9/33/34 → Schedule 1
> line 6 + Schedule SE line 1a, CRP → SE line 1b, the farm-optional SE method),
> 10 diagnostics, the Slate screen, the render leg and six test files had all
> shipped — `backentry.v1` simply had no `schedule_fs` section. For the lump-sum
> election, `compute_ss_lumpsum` already implements **Pub 915 Worksheets 2 + 4
> verbatim** (RS `1040_RETIREMENT` R-RET-LSE-WS2/WS4/ELECT, reconciled to Pub
> 915's own worked example) with the model, the `ss_lump_sum_election` toggle,
> D_RET_004/008 and a Slate screen; the lane had neither the rows nor the toggle.
> **Both halves of #6 had to travel together** — the election is irrevocable and
> explicit, never inferred from the presence of rows, so importing rows alone
> would have left 1040 line 6b silently unelected on a packet that filed it.
>
> *⚠⚠ NZ #6's TARGET IS COUPLED TO NZ #2 — line 6b = 43,950 exactly cannot be
> reached until the worksheet-rounding item lands.* Pub 915 is a whole-dollar
> form and the engine carries cents (WS2 L21 = 18,571.80 vs the printed 18,572).
> The election machinery is correct to the cent. #2 owns the whole SS worksheet
> family and MOVES existing returns — work #2 and #6 together.
>
> *Repaired in passing:* `_resolve_misc_1099_link` has always matched farms, but
> no farm could ever be imported, so that branch could never resolve;
> `schedule_fs` now commits BEFORE `misc_1099s`, with a test pinning the order.
> *Also:* 3 stale count trip-wires in `test_schedule_f.py`, red on main since
> s213 (`8d022c8`) when the seeder gained two routing lines.
>
> Regression: `server/tests/test_schedule_f_sslumpsum_nz5_nz6_s225.py` — 19
> tests, incl. #5's target verbatim (gross 6,536 / expenses 23,415 / net
> **(16,879)** on Schedule 1 line 6 with no override, plus the Schedule SE leg
> asserted separately) and Worksheet 2 reconstructed and verified line by line.

> **2026-08-07 session 224 (part 2) — FORM 8889 / HSA: the import lane closed,
> and Form 8889 LINE 4 built. NZ #4; the NZ list goes 6 → 7 of 10. No new form
> ticks — 8889 was already covered; this closes its import leg and repairs a
> limit that was computed too high. Commit `804b088`; migration 0265 (one column
> on an existing table).**
>
> *⚠⚠ THE SAME FINDING AS NZ #9, ONE ITEM LATER — and this time the BUILD PLAN
> shows why.* `HSAAccount`, `compute_8889` (Parts I–III → Schedule 1 lines 13
> and 8f, Schedule 2 lines 17c and 17d), eight diagnostics and
> `SlateForm8889Screen` all shipped Phase 2 on 2026-06-14. **The back-entry lane
> had no `hsa_accounts` section**, so no packet carrying a Form 8889 could be
> imported — the form was unreachable from the import path entirely. The source
> brief's own build plan never named it: its `input` leg reads "an 'HSA (8889)'
> tab + HSA CRUD", which is the BROWSER lane. **The two lanes are separate legs,
> so a form's build plan can be fully ticked while the form stays
> un-importable.** Twice in a row on this list now.
>
> *Every editable column is importable* — nothing on this model is
> engine-written. It is all printed source facts, including the RED-deferred
> line 10 (IRA→HSA funding distribution) and line 18 (testing-period failure)
> amounts, so a packet's facts survive even where the math stays manual.
>
> *⚠⚠ THE ONE REAL COMPUTE GAP — FORM 8889 LINE 4, AND THE SIGN IS THE POINT.*
> `owner_lines` carried the literal `line5 = line3  # line 4 (Archer) = 0`: no
> column, no input, no diagnostic. Verified against the **2025** Form 8889
> (`irs.gov/pub/irs-prior/f8889--2025.pdf`) — line 4 is "the amount you and your
> employer contributed to your Archer MSAs for 2025 from Form 8853, lines 1 and
> 2", and **line 5 = line 3 − line 4**. Omitting it left line 5 at the full line
> 3, so the contribution limit came out **too LARGE** and the deduction could
> exceed §223(b)(4)(A). **An omission is only safe when it errs AGAINST the
> taxpayer; this one did not** (the s221 lesson, again). Now
> `archer_msa_contributions`, floored at zero per the face.
>
> *⚠ A NEW PARAMETER HAS TO REACH ITS OTHER CALLER.* `D_8889_EXCESS` calls
> `hsa_deduction` POSITIONALLY to price the 6% excess-contribution warning. Had
> it not been updated it would have kept measuring against the old, too-large
> limit — contributions that are now excess would go unflagged. Pinned by a test
> that ties the two together. `D_8889_ARCHER` (an 8th diagnostic) names **Form
> 8853, which is still not built**, rather than letting silence imply zero.
>
> *⚠ NO MOVEMENT CLASS.* The column defaults to 0 and `hsa_deduction`'s new
> parameter defaults to 0, so every existing return and every existing caller
> computes exactly as before.
>
> *Two staging warnings ride with the new section.* **Line 6 is THREE-STATE and
> its two "empty" values do OPPOSITE things** — omitted/null takes the full line
> 5, an explicit **0** zeroes the whole deduction (`D_8889_FAMILY_ZERO`, the
> $8,550 → $0 swing from s137) — and the diagnostic only fires AFTER commit, so
> a transcriber typing 0 for "no split" loses the deduction silently. And
> **line 9 IS the W-2 box 12 code-W total**: a payload carrying both and
> disagreeing has one of them mistranscribed, and the return can reconcile on
> wages while still deducting the wrong amount.
>
> *Regression target met verbatim* (`test_form8889_nz4_s224.py`, 14 tests):
> family HDHP, $3,250 employer contribution, $4,592 distributed against $4,592
> of qualified medical expenses → a **$5,300** deduction on Schedule 1 line 13,
> **zero** taxable distribution, **zero** HSA additional tax of either kind — and
> the deduction proved above-the-line by reading 1040 line 11, not just line 13.
>
> *⚠ Client note.* The Slate 8889 test fixture is cast `as HSAAccountRow`, so a
> **new required field does not fail the typecheck** — it silently reads
> `undefined`. Kept in step by hand; worth removing the cast someday.

> **2026-08-07 session 224 — FORM 1099-G: the import lane closed, and a THIRD
> GA-500 line-24 roster defect. NZ #9; the NZ list goes 5 → 6 of 10. No new
> form ticks — 1099-G was already covered; this closes its input leg and
> corrects a state-routing error. Migration 0264 (three columns on an existing
> table — no new table, so no RLS migration).**
>
> *⚠⚠ THE ITEM WAS MOSTLY ALREADY BUILT.* "Add 1099-G unemployment documents"
> read like a full unit. Verify-first against shipped code says the model
> `Form1099G`, `compute_1099g` (box 1 → Sch 1 line 7, netting a same-year
> repayment and printing the "Repaid" literal), the box-4 → 1040 line 25b
> roster, five diagnostics and `SlateForm1099GScreen` all shipped in Phase 2 on
> 2026-06-14. **The only missing leg was the LANE** — `backentry.v1` had no
> 1099-G section, so no packet with unemployment could be back-entered, which
> is precisely the blocker the item observed. New section `g_1099s`; schema
> growth only, zero compute change.
>
> *Box 2 is deliberately NOT importable.* It is the STATE_REFUND worksheet's
> input; an importable box 2 would double-count Schedule 1 line 1. Its absence
> is pinned by a test, so nobody "completes" the allowlist later.
>
> *⚠⚠ THE THIRD DEFECT IN THE SAME ROSTER, unreported — a MISSING COLUMN read
> as a MISSING BOX.* `_populate_ga500_from_federal` added **every** 1099-G's
> box-11 state withholding to GA-500 line 24 **ungated**, on a comment in that
> very method asserting the form "has no state-code box". It has one: **box
> 10a State**, on Form 1099-G **Rev. March 2024** — the revision that reports
> CY2025 (verified against `irs.gov/pub/irs-prior/f1099g--2024.pdf`). The
> column had never been stored even though `_1099g_source_brief.md` said boxes
> 10a/10b/11 would all be "stored", and the absent column got mistaken for an
> absent box. Every sibling arm (W-2, 1099-R, 1099-INT/DIV, W-2G, 1099-MISC)
> was already gated on its own state code; this was the lone exception, so an
> out-of-state agency's withholding inflated the Georgia refund. Now stored
> (`box10a_state` / `box10b_state_id` / `payer_tin`) and gated.
>
> *⚠ NO MOVEMENT CLASS — deliberately.* A **blank** box 10a still counts as
> Georgia: every row keyed before s224 has a blank one because the column did
> not exist, and this practice files GA. Closing the hole therefore moves no
> existing return. The new `D_1099G_STATE` warning makes that fallback visible
> rather than silent, at the diagnostic AND at the cell.
>
> *⚠⚠ THE REVISION TRAP, NOW TWICE.* `irs.gov/pub/irs-pdf/f1099g.pdf` today
> serves **Rev. December 2026**, which **renumbers these boxes**: a new
> "Family leave benefits" money box takes 10, and the state trio moves to
> 11a/11b/12. The columns here are TY2025 names and must not be "corrected"
> against that download — pinned by a test that also asserts the TY2026
> renumbering has not been silently adopted. Identical shape to the s222
> 1099-MISC box-14 finding: **on irs.gov, `f<form>.pdf` is the NEXT revision.**
>
> *Legs.* **model** +3 columns (migration 0264, `db_default=""` per the
> deploy-skew rule) · **serializer** the three new fields · **lane**
> `g_1099s` in all four registries · **diagnostics** `D_1099G_STATE` (6th in
> the family) · **state routing** the gated GA-500 arm · **input**
> `SlateForm1099GScreen` box-10a cell (flags itself on withholding-without-
> state) + payer TIN / box 10b in the expansion · **tests** 12 server + 3
> client; the s222 roster test's own premise corrected in place.
>
> *Regression target met verbatim* (`test_form1099g_nz9_s224.py`): $365 → Sch 1
> line 7, $37 → 1040 line 25b, $22 → GA-500 line 24, each exactly once; an
> NC-sourced 1099-G contributes **nothing** to Georgia while its federal income
> and withholding are untouched.

> **2026-08-06 session 223 — ★ FORM 1310 (Statement of Person Claiming Refund
> Due a Deceased Taxpayer) — NEW UNIT, all legs green. BATCH-046 #1; batch 046
> CLOSED, and with 047 closed in s222 BOTH 1040 batches are now closed.
> Commit `658915e`; migrations 0262 (new table) + 0263 (default-deny RLS —
> ⚠ this table carries a claimant SSN).**
>
> *Legs.* **source brief** `server/specs/_1310_source_brief.md` · **model**
> `Form1310` (one per decedent; unique on (return, decedent)) · **gate/logic**
> `apps/returns/form_1310.py` · **diagnostics** `rules_1310.py` (5) ·
> **render** `render_1310` + `field_maps/f1310_2025.py` + the template
> registered in the manifest (98 → 99) · **e-file** `build_irs1310` at
> ReturnData1040 sequence 1123 · **input** CRUD + the `form_1310s`
> `backentry.v1` section + `SlateForm1310Screen` + its nav tab (under Payments,
> because the form gates the REFUND) · **tests** 26 + 9 staging + 3 commit.
>
> *⚠ NO COMPUTE LEG — the form has no arithmetic, and that is why there is NO
> RULE STUDIO SPEC.* `1310`, `F1310` and `FORM_1310` all 404, and so do `8879`,
> `2848`, `9465` and `8888`, while `3115` (which computes a §481(a) adjustment)
> has one. Authority: **Form 1310 (Rev. December 2025)**, Cat. No. 11566B,
> Attachment Sequence No. 87 · `IRS1310.xsd` · the `F1310-*` and
> `IND-298/299/300` MeF business rules.
>
> *⚠⚠ THE BUSINESS RULES ARE THE REAL SPECIFICATION — and they are NARROWER
> THAN THE PRINTED FACE.* Almost every rule on this form is a REJECT rule, so
> the unit is mostly about refusing correctly:
>
> - **BOX A CAN NEVER BE E-FILED.** `IRS1310.xsd` carries the line-A element
>   **commented out** ("not used for e-file at this time") and Part I is a
>   strict `CourtOrPersonalRepGrp` XOR `RefundClaimWithProofOfDeathGrp` choice.
>   It fits: box A asks the IRS to REISSUE a paper check already received in
>   both names, returned marked "VOID" — not a claim made on a return. It
>   prints; composition refuses and `build_irs1310` RAISES. A test re-reads the
>   XSD so a schema drop that enables line A breaks the test.
> - **BOX B is valid only on an AMENDED or SUPERSEDED return** (F1310-024-02 —
>   the face limits it to a claim on Form 1040-X or Form 843) and needs a
>   court-certificate attachment (F1310-023-03). On an ORIGINAL return a
>   court-appointed representative attaches the certificate and files no Form
>   1310 at all.
> - **BOX C has exactly ONE e-fileable answer set**: 2a=No, 2b=No, 3=Yes
>   (F1310-009/010/011). The face permits the others and explains what follows
>   from each, so those are legitimate PAPER returns — printed, and refused at
>   e-file with the rule named. This also settles a schema question: line 2b is
>   REQUIRED by the XSD even though the face says to answer it only when 2a is
>   No.
>
> *⚠⚠ TWO TRAPS BURIED IN SCHEMA COMMENTS.* The claimant's **NAME is not on
> Form 1310 in MeF at all** — "Refund Claimant Name - Use 'InCareOfNm' from the
> Return Header", and F1310-019 makes a missing header value a reject, so the
> document carries only the name CONTROL and the SSN. And
> **`ValidProofOfDeathInd` is REQUIRED inside the box-C group but is NOT A BOX
> ON THE PRINTED FACE** — the face states the requirement only in the Line C
> instructions, so e-file demands an assertion the paper form never collects.
>
> *⚠ THE WHO-MUST-FILE GATE IS CONDITIONED ON A REFUND.* IND-300-01 and
> IND-298-01 both begin "If 'RefundAmt' … has a non-zero value", and the form
> exists to *claim* a refund — so a balance-due decedent return needs no Form
> 1310, and a diagnostic firing there would be an unclearable error on a return
> that owes money (the s199 class). A court certificate discharges it, and a
> surviving spouse filing a JOINT return must not file one at all.
>
> *⚠⚠ TWO GAPS FOUND WHILE BUILDING, neither reported.* **(1)**
> `taxpayer_date_of_death`, `spouse_date_of_death` and `in_care_of` were **not
> in the `backentry.v1` allowlist at all** — a deceased taxpayer's return could
> not be back-entered, making this entire gate unreachable through the lane
> (the dates also drive the printed "Died" literal, MeF's PrimaryDeathDt/
> SpouseDeathDt and IND-424). **(2)** `_in_care_of_name` already RAISED on any
> non-MFJ decedent return — "needs a personal-representative name, which the
> app does not capture yet". The Form 1310 claimant IS that name, so this unit
> **closes a pre-existing e-file refusal**.
>
> *Refuted before building on it:* IND-424 (MFJ + a death date requires
> `SurvivingSpouseInd`) looked unhandled — it is a DIFFERENT element on the 1040
> signature block and `builder.py` already emits it correctly.
>
> *Model + render notes.* The decedent's name, date of death and SSN are
> **DERIVED from the return, never keyed** (F1310-002/004/008 compare them
> against it). Part II answers are **tri-state** — unanswered ≠ No, which is
> what Part II genuinely is on a box-A/B form. The field map is derived
> **positionally** from the template's own geometry; **the Part III signature
> and its Date have NO widget** (wet ink by design — `f1_14` aligns with "Phone
> no. (optional)", not the Date), pinned so nobody invents a mapping. Two
> decedents print as two forms, per the face.
>
> *Not built, flagged.* Form 1040-X / 843 routing of box B · the court
> certificate as a real `BinaryAttachment` (the lane never receives packet PDFs,
> so it is a `ReturnAttachmentReference` and e-file refuses) · a **foreign
> claimant address** (the XSD supports it; no `ForeignAddressType` builder
> exists anywhere in the 1040 mapper) · 1040-NR / 1040-SS.

> **2026-08-06 session 222 — ★ FORM 1099-MISC (Miscellaneous Information) —
> NEW UNIT, all legs green except render/e-file, which DO NOT EXIST for this
> form (see below). BATCH-047 #13; batch 047 CLOSED. Commit `d885ab7`;
> migrations 0260 (new table) + 0261 (its default-deny RLS ALTER).**
>
> *Legs.* **source brief** `server/specs/_1099misc_source_brief.md` ·
> **seed** `seed_form_1099misc` (FORM_1099MISC, 11 output rows; picked up
> automatically by `seed_all`'s name discovery) · **model** `Form1099Misc`
> (payer block, all reportable boxes, the routing choice + three activity FKs) ·
> **compute** `compute_1099misc.py` · **diagnostics** `rules_1099misc.py`
> (10 rules) · **input** CRUD endpoints + the `misc_1099s` `backentry.v1`
> section + `SlateForm1099MiscScreen` + its own nav tab · **tests** 21 + 6
> staging + 5 commit + 17 Slate.
>
> *⚠ NO RENDER LEG AND NO E-FILE LEG — and that is a finding, not a deferral.*
> A 1099-MISC is an INPUT document: there is no IRS form filed with the return
> (the W-2 / 1099-INT / 1099-G position), so nothing is drawn. And **there is no
> `IRS1099MISC` element anywhere in the 1040 MeF schema set** — 2025v5.3 and
> 2025v5.4 were both grepped, and `ReturnData1040.xsd` includes IRS1099R, IRSW2
> and IRSW2G as the only information-return documents. The amounts transmit
> through the lines they compute into (`Form1099WithheldTaxAmt` for 1040 line
> 25b, `OtherIncomeTotalAmt` for Schedule 1 line 8z). No builder is possible and
> none was written.
>
> *⚠ THE AUTHORITY IS THE FORM, NOT A RULE STUDIO SPEC — and that is the house
> rule.* `1099MISC` 404s in Rule Studio, and so do `1099INT`, `1099DIV`,
> `1099R`, `1099NEC`, `1099G` and `W2`. **No information return is specced
> there.** This follows the Form 1099-G precedent: build from the IRS form and
> record the reasoning in a source brief. Source = **Form 1099-MISC Rev. April
> 2025** — the revision that reports calendar year 2025 — plus its own
> "Instructions for Recipient", page 5.
>
> *⚠ THE BOX LAYOUT IS NOT STABLE ACROSS YEARS.* On **Rev. 4-2025 box 14 reads
> "Reserved for future use"**; Rev. 1-2024 had "Excess golden parachute
> payments" there, and Rev. 12-2026 makes 13a/13b/14 Cash tips / TTOC /
> Overtime compensation. Verified by text-extracting all three PDFs —
> "parachute" appears on five pages of the 2024 file and **nowhere** in the 2025
> one. So there is no TY2025 1099-MISC golden-parachute amount (Schedule 2 line
> 17k keeps no feeder), box 14 is refused by the import lane, and the Rev.
> 12-2026 columns exist on the model but are NOT routed (`D_1099MISC_TY26BOX`).
> **Re-verify the box titles when the TY2026 forms post.**
>
> *⚠ ONLY TWO BOXES ARE ADDITIVE ENGINE FEEDERS.* Boxes **3 + 8** → Schedule 1
> line 8z, and box **4** → Form 1040 line 25b. Every other box reports on an
> activity's own schedule, whose income the preparer already keys: Schedule C
> gross receipts already include boxes 1/5/6/10/11, Schedule E lines 3/4 already
> include boxes 1/2, Schedule F line 6a already includes box 9. Feeding those
> would **DOUBLE COUNT**. A row routed to an activity is therefore
> **traceability, not a feeder** — it carries the FK, the payer record survives,
> and `D_1099MISC_RECON` catches the opposite error: an activity keyed for LESS
> than the 1099-MISC routed to it, which nothing on the return caught before.
>
> *⚠ SCHEDULE 1 LINE 8z ALREADY HAD AN OWNER.* `compute_state_refund_db` writes
> **and blanks** it. Line 8z is now COMPOSED by a single final writer —
> `compute_1099misc_db` runs immediately after it and reads the worksheet's
> share back from the `sch1_8z` row the worksheet itself owns. Disengage hands
> the line back rather than blanking it; a displaced direct entry is recorded
> once, on the not-engaged → engaged transition, so `D_1099MISC_8Z` reports it
> and it cannot decay on the next recompute.
>
> *⚠⚠ TWO LIVE DEFECTS FOUND WHILE BUILDING, neither reported*, both in the
> GA-500 line-24 withholding roster this unit had to join anyway.
> **(1)** It read `g.state_withholding`, which does not exist on `Form1099G`
> (the column is `box11_state_withholding`), so `_populate_ga500_from_federal`
> raised `AttributeError` on **any return carrying a 1099-G row** — a 500 on the
> GA-500 federal pull for every Georgia return with unemployment income.
> **(2)** `FormW2G` was missing from that roster entirely, so Georgia
> withholding on a W-2G never reached line 24. ⚠ **Movement class** for (2):
> GA-500 line 24 and the Georgia refund RISE on any existing return with a
> GA-withheld W-2G. Both fixed and pinned by regression.
>
> *Not built, flagged.* Box 15 NQDC and its §409A additional tax (Schedule 2
> line 17h stays direct-entry) · box 12 deferrals stored but not income · a
> second state row (boxes 16–18 print two lines; v1 carries one, matching every
> sibling document model) · routing boxes 13a/13b/14 into the OBBBA Schedule 1-A
> tips/overtime deductions. Each fires its own diagnostic.

> **2026-08-06 session 221 — production support on ONE 1040; three defects
> fixed, two long-standing ⛔ items closed. No form ticks — every change is a
> correction to a form already covered. Commits `a57832c` · `e9fe025` ·
> `e057fbb` · `11d4260` · `1c081ae`. No migrations.**
>
> *Form 8582 line 6 (MAGI)* — now includes **nonpassive and PTP Schedule E
> page-2 income**, guaranteed payments and portfolio income, less nonpassive
> losses and the §179 deduction. §469(i)(3)'s exclusion list is CLOSED and
> i8582 says affirmatively "include any income that's treated as nonpassive
> income". The old "v1 approx" omitted it, which UNDERSTATES MAGI — and a lower
> MAGI buys a LARGER §469(i) special allowance. ⚠ **Movement class**: any 1040
> with rentals AND nonpassive K-1/PTP income loses a special allowance it should
> not have had; the deductible rental loss falls. The direction is always *less*
> deduction, i.e. we were over-deducting. `magi_nonpassive` partitions against
> `passive_k1_income` on the same condition the 8582 gather uses (`material or
> is_ptp`), so no K-1 is counted twice or dropped. **PTPs had been missing from
> MAGI entirely.**
>
> *Form 1040 line 35a* — now settles the line-38 estimated tax penalty:
> `max(0, 34 − 36 − 38)`. IRS line-by-line instructions: "Line 38 is subtracted
> from line 35a or added to line 37." Line 37 already added it; 35a was the
> missing half, so a refund return paid the penalty out to the client. ⚠ The
> mechanical half was SEQUENCING: line 38 is written after the formula passes
> (compute_2210 needs the final lines 24/33) and the post-2210 refresh rewrote
> line 37 only. **Line 34 does not move** — the overpayment is what it is.
>
> *The refund SURFACES* — the printed 1040, Form 8879, the Form 8888
> direct-deposit split and the MeF `RefundAmt` element all read 35a and followed
> automatically. **The Return Manager read line 34** (the OVERPAYMENT) and did
> not; it was also overstating any return that applies part of an overpayment
> forward to next year's estimates. Now 35a, with a test pinning all five
> surfaces to one line.
>
> *Form 4562 — `D_4562_ELECTGAP`* — no longer fires on an asset whose §179
> election consumed its whole basis. §179 comes first and permanently reduces
> basis (§179(a)); §168(k) applies to what remains. A fully expensed asset has
> nothing for bonus to attach to, so its zero bonus is arithmetic and there is
> no §168(k)(7) election-out to make — the warning could not be cleared. New
> shared helper `basis_remaining_after_179()` keeps the over-6,000-lb SUV cap,
> which matters in the other direction: a CAPPED election does leave basis
> behind and the warning should still fire there.
>
> *`D_1040_016`* — REWORKED, not retired. The ordinary refund-with-penalty case
> is now arithmetic. What survives is the case the FACE cannot express: a
> penalty LARGER than the overpayment floors 35a at zero and leaves a real
> balance due with nowhere to print (line 37 computes only when the tax exceeds
> the payments). The IRS bills the difference and nothing on the return says so.
>
> *`D_1040_012`* — the split identity learned the penalty arm:
> `35a = max(0, 34 − 36 − 38)`. The offset fix shipped without it and turned a
> CORRECTED refund into a false error. The floor case is deliberately NOT a 012
> error — the split is not broken there, the penalty is simply larger than the
> money to pay it, which is 016's finding. One fact, one finding.
>
> *1120-S Schedule M-2 (page 8)* — ⛔ **the ATS scenario-5 `M2_3a` question is
> CLOSED and it never needed a ruling.** Tax-exempt interest is an OAA (column
> d) item, never AAA: §1368(e)(1)(A) adjusts AAA "in a manner similar to" §1367
> EXCEPT for income exempt from tax. The IRS key agrees — its own Attachment 9
> is 2,800 + 3,625 = 6,425 with the 486 in column (d). The 5,461 in our test was
> OUR arithmetic, frozen pre-batch-010 #8. ⚠ **And it hid a live e-file
> defect**: batch-010 #8 moved K16a out of the M2_3a FORMULA but left it in
> `_M2_3A_COMPONENTS`, the statement decomposition — so `_stmt_reconcile`
> refused composition on ANY 1120-S with tax-exempt interest plus another AAA
> other-addition, with a message pointing at a sub-schedule that was never the
> problem.
>
> *Form 6252 template* — unchanged on irs.gov (CR-2026-001 was a false
> positive: live page-1 text byte-identical, all 49 AcroForm field names
> identical). The manifest now records `source_sha256`, the RAW download hash,
> alongside the trimmed template's — f6252 is the only derived template of 98,
> and comparing the derived hash against the source URL is what opened the item.

> **2026-08-06 session 220 — CC BATCH-013 PART 2, batch CLOSED (items 2/4/5/8,
> one deploy `0239b11`, migrations 0257–0259). ONE form ticks: Form 6252 gains
> its ENTITY arm; the rest are new surfaces on forms already covered.**
>
> *Form 6252* — **the entity arm now exists.** The model and
> `apps.returns.compute_6252` were already form-agnostic (the 1040 build), so
> this added the SURFACE, not a second engine: an `installment_sales` section in
> the entity allowlist / generated schema / replace-merge handling, an
> *Installment Sale (6252)* tab on both entity editors, and an entity header on
> `render_6252_1040` so the form PRINTS on an 1120-S/1065. Routing: **capital →
> Schedule D by term → K7 / K8a**; business §1231 (>1yr) → the entity 4797 Part
> I sum → K9 / K10; ordinary/≤1yr and the line-25/36 ordinary recapture → Part
> II → page-1 line 4 (1120-S) / line 6 (1065). The §453 gross-profit percentage
> is used **frozen**, never recomputed. ⚠ **§453A interest is a 1040 Schedule 2
> line 15 write with no entity destination** — the two fields store and are
> consumed nowhere on an entity return. e-file: unchanged (no IRS6252 leg).
>
> *Form 4797, entity* — a **BULK SALE** is now one property computed over many
> register rows. `bulk_sale_group` links the aggregate `dispositions` row
> (`is_4797`) to its member assets. The members keep driving the registers, Form
> 4562 and the Schedule L beginning/ending removals — they still carry
> `date_sold` — but their disposal RESULT columns are cleared and they emit no
> gain. The aggregate row's **own keyed basis columns are ignored**: regular AND
> AMT adjusted basis are summed from the linked rows, which is what makes
> **Schedule K line 15b tie** where an aggregate row that cannot see its members
> reports zero. ⚠ a §179 asset may NOT join a group (i4797 sends a §179
> disposition to the K-1, not to Form 4797) — refused at staging and by
> `D_SCHK_BULK_LINK`. ⛔ **e-file refuses a bulk sale loudly** — the aggregate
> 4797 row has never been carried into IRS4797, so emitting the members would
> put zero-proceeds losses in the XML against a recapture gain on the face.
>
> *Schedule D (1120-S) / Form 8949* — an **AGGREGATE net-only row**
> (`net_gain_loss`) for source packets that print only corporation-level capital
> results and supply no transaction detail anywhere. It carries no proceeds or
> basis, prints **no Form 8949 line**, and lands on Schedule D line 7 / line 15
> only. Detail rows stay authoritative per term (staging refuses the
> combination; `D_SCHK_CAPD_AGG` reports the UI-built one). ⛔ **e-file refuses
> it** — IRS8949 is a transaction schedule.
>
> *GA-600S Schedules 7/8* — the depreciation gross pair now has **two source
> presentations**, chosen per return by `ga_depreciation_presentation` (blank ==
> `component_net`, the batch-012 #7 behavior, so nothing stored moves).
> `aggregate_gross` prints BOTH regular-depreciation columns in full and
> excludes only what is not §168 regular depreciation at all — amortization and
> §179. **Net Georgia income (S6_11) is identical under both**, pinned by test:
> only the printed pair moves, and the pair is what reconciles line-for-line
> against the source's own GA-600S. ⛔ a SALE's federal-vs-Georgia gain
> difference still has **no face destination** — `D_SCHK_BULK_GA` reports the
> computed figure and the preparer keys it on Schedule 7/8 "(Attach Schedule)",
> as the source does; the destination is an RS/Ken question.
>
> *Depreciation engine* — reports **`sec_179_in_state_current`**, the Georgia
> twin of the federal `sec_179_in_current` batch-013 #10 introduced. Both
> components now persist on the asset row, so a later consumer subtracts the
> engine's own answer rather than re-deriving the election's §280F and
> remaining-basis caps. An overridden arm reports a §179 component of zero.
>
> *UI* — the bulk-sale label on both the depreciation worksheet (Disposal
> block) and the entity Schedule D row expansion, the aggregate-net field on the
> same expansion, and the GA depreciation-pair selector on entity Client Info.
> Each carries an inline note stating what the setting suppresses.

> **2026-08-06 session 219 — CC BATCH-013 PART 1 (7 of 11 resolved; commits
> `28f8a91` · `da9f85b` · `01b2ca9`, migration 0256). No form ticks — these
> are corrections to forms already covered.**
>
> *Form 4562*: line 16 now has a **second arm**. Batch-012's `D_4562_L16` is
> the register-less SCALAR and keeps its Schedule L exclusion; the new
> **`f4562_bucket`** on a depreciation asset marks a REAL book row for line
> 16. A marked row leaves every MACRS Part III aggregation (line 17, Section
> B and Section C) so it reports exactly once on the face, while still
> driving page-1 line 14, the Schedule L roll-forward and K15a. Line 22 adds
> only the scalar — the rows are already in its register sum. ⚠ a non-MACRS
> life (28.5 SL) has no Pub 946 table, so a line-16 row's current amount must
> come from the batch-009 #10 source override.
>
> *Depreciation engine*: the per-arm current overrides now bind the
> **amortization** path (the §197 branch returned before the override loop
> existed), and the engine reports **`sec_179_in_current` /
> `sec_179_in_amt_current`** so Schedule K line 15a can exclude the election
> explicitly instead of relying on it to cancel between two totals. ⚠ **K15a
> moves** on any 1120-S that overrides exactly one arm of a §179 row, and now
> writes at zero so a stale figure clears.
>
> *Schedule K line 15a, 1065*: ⛔ the write was **unguarded by form code** and
> that line on a 1065 is the **Low-Income Housing Credit (§42(j)(5))**. Now
> 1120-S only — affected 1065s blank on next recompute.
>
> *Schedule L*: an emptied asset class now ends at **exactly zero** rather
> than at the roll-forward's residue (the roll-forward subtracts the
> REGISTER's gross costs from a SOURCE-keyed beginning face value, so it
> could never be relied on to reach zero). Gated on the class having rows.
>
> *Diagnostics*: `OWNER_PCTS` allows one unit in the last permitted decimal
> place per owner (six-decimal K-1 percentages cannot sum to a mathematical
> 100), and gained an explicit negative-percentage error.
>
> *UI*: the three per-arm current overrides — built in batch-009 #10 and
> **never rendered on any screen** until now — are on the Slate depreciation
> worksheet, along with the Form 4562 line selector. The rest of the Georgia
> register (method / life / prior) remains import-only.

> **2026-08-06 session 216 — CC BATCH-012 CLOSED (10 items, one deploy
> `7c5ac63`, migration 0252).**
>
> *Form 4562*: new **line 16** surface — "Other depreciation (including
> ACRS)". No register can derive it (ACRS and the other pre-MACRS §167
> methods have no Pub 946 table), so `D_4562_L16` is a preparer INPUT with
> sub-schedule detail rows. It composes into page-1 line 14 alongside the
> register aggregate and into 4562 line 22, and is deliberately EXCLUDED from
> Schedule L — lines 10a/10b tie to the register's cost/accumulated columns
> and this amount has no rows there. ⚠ the field map's `F4562_16` comment
> said "Add lines 14 and 15"; there is no such line on the 2025 face (the
> AcroForm target was correct, only the comment was wrong).
>
> *Depreciation asset*: **`amt_cost_basis`** — the AMT arm could carry its own
> method, life, prior and current depreciation but never its own BASIS, so
> disposal math reused the regular basis and destroyed any source difference.
> NULL = "same as regular" (every pre-existing row unchanged); an explicit **0
> is meaningful**. Drives the AMT adjusted basis and the K15b component.
> Both UIs also stopped wrapping the disposal table's adjusted-basis row in
> `Math.abs()` — a NEGATIVE adjusted basis is real once the AMT arm has its
> own basis, and it ADDS to the gain.
>
> *GA-600S*: the Schedule 7/8 depreciation **gross pair now nets equal
> federal/Georgia components PER ASSET.** batch-004 #2's whole-register
> suppression was the all-or-nothing version of the same rule and a MIXED
> register defeated it; it is now the degenerate case. ⚠ **the printed pair
> moves on any return whose register mixes equal and unequal assets** (net
> Georgia income is unchanged). GA line count unchanged.
>
> *Schedule L*: new `L24_BOOK_BRIDGE` carries the **current-year** M-1
> book/tax difference into L24d (batch-008 #3 carried only the beginning
> difference). Excludes tax-exempt interest, nondeductible expenses and the
> §179 disposition gain — all already in the M-2 columns. ⛔ a derivation from
> i1120s M-1/M-2 mechanics, not a quoted RS rule: on the RS agenda.
>
> *Diagnostics*: `SCHED_L_DEPR_TIE` error → **warning** (Schedule L is
> "Balance Sheets per Books"; the register is TAX and the model has no book
> columns — and the RS 1120S_SCHL spec has no tie diagnostic at all). Error is
> kept behind a predicate that re-arms once book columns exist.
> `D_4562_METHOD` exempts a row already fully recovered from PRIOR history in
> every arm. `INT_GA_BONUS_ADDBACK` **rewritten**: it read `S1_3`, which on
> the seeded GA-600S is "Total Income (Add Lines 1 and 2)" — so it was both a
> false error when total income was zero AND silently satisfied whenever it
> was not; it now reads the S7_7a/S8_3a pair. ⚠ **NEW HOLD**: `D_4562_BASIS`
> now ERRORS when prior bonus + prior §179 exceeds original cost (previously
> an acknowledgable warning) — recovered basis above what was paid is the same
> impossibility as `original_cost < cost_basis`.
>
> *Entity lane*: `replace_documents` releases a family's derived lines on
> PRESENCE, not only on an explicit empty list; the full federal→GA pull runs
> for an ALREADY-LINKED GA-600S (override-respecting); and
> `entity-shell-bootstrap` gained a guarded `attach` mode that creates only
> the missing return shells on an exactly-matched existing entity.

> **2026-08-06 session 215 — CC BATCH-011 CLOSED (10 items, one deploy
> `ac4a70e`, migration 0251).** *Form 7203*: Part I **line 3m** now has an
> input — `Shareholder.other_stock_basis_increases` (MeF
> `OthItemsIncreaseStockBasisAmt`), the increases twin of batch-010 #4's
> line 13. It is deliberately NOT line 2: a source that reports an other
> increase on 3m reaches the same ending basis through line 2 while printing
> the wrong line. ⚠ the RS 7203 spec's `line_map` carries no 3a–3m at all
> (one calculated line 3) — the letter→meaning map is the app's field map
> plus the MeF element name; on the RS agenda.
>
> *Schedule K-1 (1120-S) item I*: **loans from shareholder now print.** The
> field map's two cells (`sh_loans_boy` / `sh_loans_eoy`) had existed with no
> source since the K-1 was built, so item I was blank on every K-1 ever
> rendered. New `Shareholder.k1_loans_boy` / `k1_loans_eoy` drive it; the
> nested Form 7203 Part II `loans` rows are the fallback when they are zero.
> Item I and 7203 Part II debt basis are DIFFERENT facts — a source can
> report loan balances on the K-1 face while carrying no debt basis at all,
> and the new fields never create a Part II row.
>
> *Form 1125-A line 4*: `A4` joins the sub-schedule surface
> (`LINE_DETAIL_LINES` + `SUBSCHEDULE_CONFIG`) so the "attach schedule"
> §263A detail persists and rolls up; page-1 **line 2** (cost of goods sold)
> joins the entity answer key. A keyed aggregate that disagrees with its own
> rows is now a staging error instead of a silently discarded number.
>
> *Statements (ALL forms)*: single-amount sub-schedule rows now emit a
> **Statement page automatically**. They already reached MeF as
> `Itemized*Schedule` statements, but the print path only ran when a caller
> passed `statements` explicitly — which nothing in the app does — so no
> 1125-A, page-1 line 5 or M-1 detail statement had ever printed.
> ⚠ **page counts change on any return carrying sub-schedule rows.** The
> balance-sheet sub-schedules (L6/L9/L14/L18/L21) still do not print: the
> statement page has one Amount column and they need a beginning/end pair.
>
> *Form 4562 / §280F*: `D_4562_VCLASS` becomes **error** severity when a
> Vehicles-group asset's missing classification actually MOVES the number —
> an unclassified listed vehicle falls back to the §280F passenger-auto
> caps, and an over-6,000-lb vehicle is not a passenger automobile
> (§280F(d)(5)). The finding now prices the fallback in dollars. ⛔ the
> escalation needs Ken's ratification.
>
> *Schedule L*: the `SCHED_L_DEPR_TIE` fully-recovered-subset exception now
> keys on the cost and accumulated lines moving by the SAME amount (bounded
> by the fully-recovered population) instead of demanding that the source
> omit every fully recovered row; and when the ending accumulated gap equals
> the keyed BEGINNING gap, the finding names the beginning balance as the
> origin. The four A/R cells (L2a/L2b gross+allowance beginning, L2d/L2e
> ending) were already seeded and already net into L15a/L15d — no build; an
> unknown line number now names the seeded siblings sharing its stem.

> **2026-08-05 session 214 — FORM 8949 COLUMNS (f)/(g) + THE FORM 2553 IMPORT
> UNIT (CC batches 010 + 009 CLOSED).** *8949*: `Disposition` gains
> `adjustment_code` + signed `adjustment_amount` (**migration 0246**); gain is
> now **proceeds − basis + adjustment** in the model property, in
> `aggregate_schedule_d` (→ K7/K8a) and in the print. Codes validated against
> the i8949 (2025) columns (f)/(g) table fetched live from irs.gov
> (B C D E H L M N O P Q R S T W X Y Z), stored uppercase-alphabetical per
> i8949's own multi-code rule; an amount without a code refuses, as does
> either on a Form 4797 row. **`render_8949` now emits ONE COPY PER BOX** (the
> IRS "check only one box" rule — it previously forced every short-term row
> into box A and every long-term row into box D on a single page), with the
> (f)/(g) columns and their totals. ⚠ the IRS8949 **e-file** adjustment leg is
> NOT built — the read model now REFUSES an adjustment-carrying row rather
> than transmitting a wrong gain (stated boundary). The batch-010 #9 packet is pinned:
> 215,000 − 162,889 + (−17,425) = 34,686 → K7.
>
> *Schedule K 15a*: NEW `PassthroughK1AMTAdjustment` (**0247** + **0248** RLS)
> — post-1986 depreciation adjustments received on K-1s (1065 box 17 code A)
> sum WITH the register's own adjustment into K15a and **derive even with no
> owned depreciable assets** (the aggregate previously early-exited on an
> empty register); a supporting statement names each source. The batch-010 #7 packet is pinned
> at −1, allocated −1/0 across two 50% K-1s.
>
> *Form 2553*: the FORM was already built (s69/s166); the **import lane** was
> not — `return_info.s_election_date` was its only surface, so a packet whose
> forms-needed page lists "2553 (Return)" imported and closed out silently
> missing a required filed form. NEW typed `form_2553` OBJECT section
> (Form2553 is OneToOne) carrying every authorable item plus nested
> `consents` (column J–N) and `qsst` (Part III) rows — both authored WHOLE
> under `replace_documents`, because a partial merge would leave a stale
> signer on a filed election. `form_2553.election_effective_date` vs
> `return_info.s_election_date` is a same-fact contradiction and refuses at
> staging. NEW `ReturnAttachmentReference` (**0249** + **0250** RLS): what the
> filed packet carried, per jurisdiction — deliberately REFERENCES, never
> files (the server never receives the packet). NEW closeout **gate 3b**
> driven by `source.forms_needed`: a declared 2553 must be present, carry
> consent rows, and carry an attachment reference, or the return does not
> file; the report exposes presence + per-jurisdiction attachment counts
> either way, and a declared form with no gate behind it (e.g. 8832) warns
> rather than pretending to verify. The batch-009 #7 packet is pinned end to end.
>
> Also: the guarded entity-shell bootstrap endpoint (new-client packets) and
> the L24 statement's prior-year timing-difference walk. Green: entity lane +
> 2553 suite 64/64 · closeout 35/35 · flow assertions 521/521 · renderer +
> 8949 e-file extract 251/251 · client tsc clean.


> **2026-08-04 session 210 — 1120-S/1065 OTHER DEDUCTIONS: detail rows now
> COMPOSE with computed named deductions (CC batch-003 #1, P0).** The rollup
> pinned face line 20/21 is_overridden with the rows-only sum — erasing
> computed deductible meals — and its post-compute K16c/K18c write (no
> override flag) was erased by ANY recompute, zeroing computed meals-nonded
> whenever rows existed. NEW seeded auto inputs `D_OTHER_ROWS` +
> `D_NONDED_ROWS` (both entity forms, ride seed_all) join the line 20/21 and
> K16c/K18c formulas; the rollup writes subtotals pre-compute and releases
> stale pre-fix pins; the 1120 keeps the legacy path (no composing formula
> there). Packet-124's 294/294 acceptance + recompute stability pinned
> (test_other_deductions_compose.py, 6 tests). Plus batch-002: the self-heal
> reload waits for the ?tab= mirror; entity returns always show the State
> tab; the Create-state lane is timeout/409-honest. NO migration.
> LIABILITY COLUMNS (MIXED-PILOT #2).** The 2025 K-1 item K1 prints Beginning
> AND Ending per §752 bucket; the app modeled ending only. Partner gains the
> three `liability_*_boy` fields (**migration 0236**, db_default=0), the
> renderer feeds all six item K1 AcroForm boxes (the map had them since s125 —
> only the EOY trio was wired; render pin added), both chromes gain the BOY
> column, and NEW `_populate_partners_from_prior_year` proformas partners from
> the prior IN-APP 1065 with prior-EOY→BOY on items J/K1/L (flows zero, special
> allocations not carried). NEW `D_K1_ITEMK_BOY` warning polices BOY vs prior
> in-app EOY. AHEAD of RS `R-K1-ITEM-K` (ending-only; spec edit queued — the
> s204 class, Ken-sequenced). MeF N/A (no 1065 builder). ⚠ a future 1065
> back-entry lane must allowlist the BOY fields. 42 tests + 556 sweep.
> box 1a/box 1 ban is now gated on Schedule B's OWN requirement (Ken's
> Option A ruling, recorded in DECISIONS.md).** A packet-total row may carry
> ordinary dividends / taxable interest exactly while no R-SB-04 trigger
> makes Schedule B required; the adjustments never relax (each is a trigger
> itself). `schedule_b_required` gained the missing dividend-nominee trigger
> (i1040sb verbatim: "interest or ordinary dividends as a nominee") — the
> renderer and MeF attach gates ride the same helper. Serializer +
> `D_INTDIV_012` both gate on `schedule_b_required_db`; D_INTDIV_012
> re-alarms when a later payer row crosses the $1,500 threshold. AHEAD of RS
> `R-AGG-SUMMARY` (spec edit queued). No migration.

> **2026-08-04 session 199 (cont.) — 1099-R CODE W COMPUTES + the D_RET_005
> coverage gate (backlog item 9).** Code W (LTC-rider charge, combined
> arrangement) joins `SUPPORTED_CODES`: §72(e)(11)(A)(ii) excludes the charge
> from gross income by statute, so it rides the code-Q absolute-zero branch —
> a blank box 2a can never tax the gross. AHEAD of RS `R-RET-CODE` (the
> code-6 precedent; RS agenda). `D_RET_005` (IRA deduction + taxable SS
> circular) now requires an employer-plan coverage signal (W-2 box 13 either
> owner / Sch 1 line 16): Pub 590-A applies Appendix B only when covered;
> uncovered = full deduction, no MAGI test, no circle. Registry description
> changed → `seed_rules` on deploy. No migration.

> **2026-08-04 session 199 (cont.) — FORM 8995: the §199A source enumeration
> CONVERGED (backlog item 8).** The reported "stale 8995 state" was refuted —
> the subject return's deduction was correct and engine-computed from a
> QBI-designated rental. The real defect: b011 added rentals as a §199A
> source and only `compute_8995_db` learned; the D_8995_STALE rule (false
> ERROR), the 8995-A diagnostics context, the 8995-A renderer and the MeF
> read model each carried their own frozen source list. All four now delegate
> to one shared `qbi_engaged_db()`. Above-threshold rental-only returns now
> render and transmit their 8995-A. No migration.

> **2026-08-04 session 199 — GA-500 RIE INPUT LEG CLOSED + TWO DIAGNOSTIC
> DEFECTS (Ken's relayed backlog, items 6–7 of ~20).** **GA-500**: the
> Retirement Income Exclusion worksheet's line 13 leaves PREPARER-ENTRY for
> PULLED — the Schedule E page-1 result, attributed per spouse off the NEW
> `RentalProperty.owner` (T/S/J, migration 0235) and split at its post-§469
> ALLOWED amount so a suspended loss never reaches Georgia. The parts
> reconstruct Schedule E line 26 exactly. Ties the filed 36,034 on the
> batch-046 case (engine gave 45,227). `rie()` itself unchanged — it was never
> wrong, it was never fed. Also fixed: `_attribute` dropped half of any
> `joint`-tagged source on a NON-MFJ return, affecting every RIE category.
> **SCHEDULE_L**: `SCHED_L_DEPR_TIE` now runs only on forms that HAVE a
> Schedule L — it was firing two unclearable ERRORs on every 1040 carrying a
> depreciation asset. **FORM_4562 import**: an explicitly-supplied convention
> that contradicts §168(d)(2) on a real-property life is corrected to MM with
> a loud warning (the blank-column case was already handled; the supplied one
> was taken verbatim and computed $0). `RentalProperty.owner` is importable.
> Migration returns.0235.

> **2026-08-03 session 198 — FOUR COMPUTE LEGS MOVED + ONE REFUTED (Ken's
> relayed backlog, items 1–5 of ~20).** **SCH_3** line 11 (excess social
> security) leaves DIRECT-ENTRY for COMPUTED: figured per person from
> same-owner W-2 box 4, 2+ employers required, the person dropped entirely when
> one employer over-withheld; maximum derived from the Schedule SE wage base
> (176,100 × 6.2%). NEW `D_SS_EXCESS_EMPLOYER` / `D_SS_EXCESS_SINGLE`.
> **FORM_4562** line 11 now includes nonpassive K-1 ordinary business income
> (Reg. §1.179-2(c)(6)(ii)-(iii)) via the shared
> `section_179_active_business_income()` — guaranteed payments / 4797 ordinary
> gain / 1041 K-1s stated as boundaries, not guessed. **SS BENEFITS WORKSHEET**
> now rounds LINE BY LINE (the 1040's "round ALL amounts" election, which is
> what TaxWise does) — affects every return with taxable Social Security.
> **Back-entry lane**: `schd_qof_disposal` importable (tri-state preserved).
> **SCHEDULE_D unchanged and VINDICATED** — the reported $2 difference was a
> missing 1099-DIV box 2b, not a worksheet defect. No migration.

> **2026-08-03 session 197 — GA-600S: THE GEORGIA DEPRECIATION PAIR NOW
> REACHES THE FORM (compute + render legs both moved).** s196 taught the
> engine to depreciate Georgia's own basis, but nothing consumed the result —
> the 600S carried the preparer's hand-keyed net difference. Two NEW seeded
> lines carry Ken's ruled GROSS PAIR: **`S7_7a` federal depreciation
> (Schedule 7 addition) · `S8_3a` Georgia depreciation (Schedule 8
> subtraction)**, both joining the `S7_8` / `S8_5` totals. Pulled from the
> FEDERAL asset register (`current_depreciation` / `state_current_depreciation`)
> at state-return creation AND re-pulled after every federal recompute
> (`_auto_sync_ga600s`), override-respecting. **The DOR AcroForm has no
> depreciation widget** — so the dedicated lines print on the preparer/client
> copy (`ga600s_native`) and ROLL INTO `S7_7` / `S8_4` "(Attach Schedule)" on
> the filed copy; both faces carry identical totals. Barcode ties Lacerte:
> federal 37,931 / Georgia 41,509 → GA net income 232,915 · tax 12,088.
> GA-600S line count **82 → 84**. No migration.

> **2026-08-03 session 192 — LANE EXTENSIONS, NO FORM-LEG CHANGE.** The
> STATE_REFUND worksheet (complete since NEXT-UP #9) and Schedule 1
> line 20 became IMPORTABLE through the back-entry lane (`sr_*` taxpayer
> fields · `sch1_fields` "20"); `amt_medicare_wages_agg` (8959 line-1
> fallback) gained a browser-UI edit surface. All legs of the affected
> forms were already green — this session changed input surfaces only
> (backentry allowlists, serializer, two screens; zero compute/render).

> **2026-07-31 session 172 — BACKLOG LEG 2 ITEMS 8 + 16: THE TWO KEN-RULED
> DERIVES (Schedule A §165(d) gambling cap · EIC self-employed).**
> SCHEDULE_A compute leg: `scha_gambling_winnings` now DERIVES from the
> return (Σ W-2G box 1 + `other_gambling_winnings`) unless the NEW
> `scha_gambling_winnings_overridden` (migration 0227 — demo DB applied,
> PROD at deploy); the cap left the engage list (a cap alone is not a
> deduction); a W-2G delete resnaps it; the s137 $0-deduction defect heals.
> 1040_EIC compute leg: an UNANSWERED `eic_self_employed` derives from Sch 1
> L3/L6/K-1 box-14A SE (explicit answer = the override; None stays None so
> `eic_engaged` is untouched); the WS-B default base widened to L3+L6+K-1 SE
> (R-EIC-WSB-SE still says L3-only → RS agenda); the s139 $0-EIC defect
> heals ($4,328 pinned). D_W2G_LOSS_CAP reworded + NEW D_EIC_018 →
> `seed_rules` at deploy (FOUR sessions stacked). ⚠ Also repaired three
> ROTTED s142-era pins (s143's QBI fix + s145's dual-student demotion had
> silently broken test_backlog_leg1_diagnostics_s142.py — it was never
> re-run by those sessions).

> **2026-07-31 session 168 — THE WRAP-UP TABS (Extensions / PY Compare /
> State): SCREEN LEG CONVERTED — THE BUSINESS-ENTITY SLATE LANE IS
> COMPLETE (13/13).** Both entity editors' last three tabs are Slate
> (unit 48; live-proven on both demo returns, settle byte-identical,
> fixpoint twice). Findings: (1) the legacy PY-compare table MISLABELED
> REAL DOLLARS on both entities — 1120-S "K5a/K6 capital gains" showed
> dividends/royalties (ST=K7, LT=K8a per the seeds), a duplicate L15d
> row labeled Total Assets "Accounts Payable" (L16d), L9d/L20d wore
> wrong names, and the 1065 rendered 1120-S-keyed rows throughout ("K11
> §179" = the 1065's OTHER INCOME) — **fixed for BOTH paths** via the
> shared slate/entityPyCompare.ts groups (legacy delegates); (2) the
> legacy Extensions autosave PATCHed /info/ on every tab OPEN (dirty-ref
> fixed, both paths); (3) REVIEW_QUEUE s168: mutations answering with
> TaxReturnSerializer run ~37s unprefetched (retrieve-only prefetches) —
> past the client's 30s bound, so every legacy /info/ save was silently
> ABORTED client-side while the server applied it; the Slate lane widens
> its timeout to 120s until the server fix; (4) REVIEW_QUEUE s168:
> update_info still lacks the row lock its sibling endpoints got.

> **2026-07-31 session 167 — BOUNDARY + FORM 8941: SCREEN LEG CONVERTED;
> the S-corp K-2/K-3 gap CLOSED on the Slate path; the K13g
> disengage-residue ANNOTATED.** The `entity_boundary` tab (both
> entities) and the `form_8941` tab (1120-S) are now Slate (unit 47,
> `SlateEntityBoundaryScreen` / `SlateForm8941Screen` — views over each
> Section's singleton PATCH lane; live-proven on both demo returns,
> settle byte-identical, fixpoint proven twice). Two findings: (1) the
> LEGACY boundary card renders the K-2/K-3 DFE-confirmed checkbox inside
> its 1065-only block while D_EB_K2K3 fires for BOTH entity types (the
> 1120-S indicators landed 2026-07-12) — an S corporation with foreign
> activity had a RED it could not clear from the screen; **fixed on the
> Slate path** (both entities, per-entity indicator wording, warning
> gated on the real foreign-activity read), legacy body untouched per
> the sweep convention. (2) Form 8941's other legs are genuinely full
> (compute → K13g with the override guard, print, MeF IRS8941, six
> diagnostics; both RS spec mirrors diffed current) — but the engine
> only writes K13g while line A is Yes, so DISENGAGING an engaged 8941
> leaves the stale credit on K13g and the MeF K-1 mapper then refuses
> the un-sourced value (REVIEW_QUEUE s167, the s143 zero-residue family;
> the Slate screen states the residue live off the published K13g).

> **2026-07-31 session 166 — ELECTIONS CARDS (2553 / 2848 / 3115): SCREEN
> LEG CONVERTED ON EVERY MOUNT — and the "39/39 1040 screens complete"
> claim was short by two.** The three print-first singleton cards are now
> Slate (unit 46, `SlateForm2553Screen` / `SlateForm2848Screen` /
> `SlateForm3115Screen` — views over each Section container's own lanes;
> the server-derived `analysis` rollups render read-only; row deletes
> gained the two-step arm lane the legacy Del links never had). The
> conversion lives INSIDE the shared Section components, so the entity
> elections tab AND the 1040's form_2848 / form_3115 tabs all convert at
> once — **the 1040 mounts had NO Slate gate while the sweep was recorded
> complete** (the s147 a-tracker-row-is-a-claim lesson, proven again on
> the sweep's own count; fixed by this unit, verified live on the 1040
> demo return). No engine findings: the RS spec mirrors (2553/2848/3115)
> diffed identical to the live exports, render_complete's
> attach_to_return integration for 2553/3115 was verified in-code, and
> these forms are deliberately print-only (no MeF leg).

> **2026-07-31 session 165 — ENTITY SCHEDULE F: SCREEN LEG CONVERTED, two
> header defects + the F14 delete-residue ANNOTATED.** The entity farm tab
> is now Slate for BOTH entities (unit 45, `SlateEntityScheduleFScreen` —
> the seeded sched_f FFV rows over the ONE debounced lane + the B2-3
> manual-PY lane; live-proven on both demo returns, settle byte-identical,
> fixpoint proven twice). Do NOT read the entity Schedule F's render leg
> as green on the header: (1) the accounting-method box (line C) **can
> never print** — `render_schedule_f` maps only a boolean-typed FH_METHOD
> to the Cash box and nothing ever emits FH_METHOD_ACCRUAL, while the
> seeds type it TEXT (REVIEW_QUEUE s165); (2) the FH_1099_RECEIVED seed
> label still asks the pre-2023 "applicable subsidy" question while its
> checkbox prints into the 2025 line F 1099-payments question — a stored
> answer responds to the wrong question (REVIEW_QUEUE s165; the screen
> shows the 2025 face label and warns). Compute residue: the F14
> depreciation feed writes only when nonzero, so deleting the last
> Schedule-F-flowing asset leaves a stale engine F14 in F33/F34 forever
> (REVIEW_QUEUE s165). The at-risk line 36 boxes are a map-only stub (no
> seed/input). E-file: an 1120-S with a nonzero K10 (farm or override)
> deliberately REFUSES to build (box-10 code unmodeled, builder_1120s) —
> the screen states it live; the demo 1120-S's overridden K10 = 10,000
> over an empty farm is the live case.

> **2026-07-31 session 164 — ENTITY SCHEDULE D (DISPOSITIONS): SCREEN LEG
> CONVERTED, the K7/K8a zero-clear + the 1065 wrong-face print ANNOTATED.**
> The entity dispositions tab is now Slate for BOTH entities (unit 44,
> `SlateEntityDispositionsScreen` — PayerTable slim grid, type-to-add, the
> B2-14 capital-only ruling + is_4797 banner/Convert lane carried; live-
> proven on both demo returns, settle byte-identical). Do NOT read entity
> Schedule D coverage as green beyond the 1120-S row lanes: (1)
> `aggregate_schedule_d` zeroes PLAIN K7/K8a before its rows-exist check —
> an imported Schedule K line 7/8a without per-row detail dies on the
> first recompute (REVIEW_QUEUE s164, 0q family); (2) **a 1065 with
> capital dispositions prints the 1120-S Schedule D face** (render 3c2
> unguarded + `f1065sd_2025.py` is an unmapped stub) while its rows feed
> no K line (the deliberate 1065 aggregation guard, DEFERRAL_AUDIT) —
> both REVIEW_QUEUE s164; the Slate screen warns/states live.

> **2026-07-31 session 163 — FORM 8825 (ENTITY SIDE): SCREEN LEG CONVERTED,
> two K2 ownership defects + one input gap ANNOTATED.** The entity rental
> tab is now Slate for BOTH entities (unit 43, `SlateEntityRentalScreen` —
> DocumentTabs per property over the legacy REST CRUD + Schedule A child +
> manual-PY lanes; live-proven on both demo returns, settle byte-identical).
> Do NOT read the 8825's input leg as fully green: **lines 21/22a have no
> input lane anywhere** (engine/print/e-file all read FFVs
> `8825_L21`/`8825_L22a` that no seed creates — REVIEW_QUEUE s163), and the
> K2 write has TWO owners that disagree (`views._rollup_rental_to_k2`
> forces `is_overridden=False` and drops 21/22a; `aggregate_rental_income`
> zeroes a plain K2 on a no-rentals return — the K16d stomp family +
> the 0q family, both REVIEW_QUEUE s163). The Slate screen warns live on a
> published-K2 disagreement and states the 21/22a gap.

> **2026-07-31 session 160 — FORM 1125-A: INPUT/RENDER BOUNDARY ANNOTATION.**
> The 1125-A print field map carries all six line-9a valuation-method
> checkboxes plus 9b/9c/9d/9e (subnormal goods, LIFO adopted, LIFO amounts,
> §263A) — but only A9a (free text) and A9f are seeded, only Cost/LCM/9f
> are renderer-mapped, and no screen offers the rest. A LIFO or §263A
> corporation cannot express its inventory facts end-to-end, and an A9a
> value outside the two supported strings prints line 9a with NO method box
> (the Slate page-1 screen now states the boundary and surfaces
> unrecognized values — unit 40). Do not read the 1125-A as fully covered;
> the fix (seed lines A9b–A9e + inputs + render mapping) is REVIEW_QUEUE
> s160, LEG 3.

> **2026-07-31 session 159 — FORM 7203 (ENTITY SIDE): COMPUTE-LEG DEFECT
> ANNOTATION, screen leg converted.** The entity-side Form 7203 compute
> (`apps/tts_forms/compute_7203.py`, feeds the courtesy print for the
> shareholder's 1040 — no e-file leg by design) carries a **Part I mapping
> defect**: 3g = K7 only, 3h = K8a (prints the LT capital gain on the
> §1231 row), and **K9 is never read — a §1231 gain gives no stock-basis
> increase** (probe-priced: line 10 = 0 vs 30,000 correct + a $15,000
> phantom Schedule-D gain; REVIEW_QUEUE s159, LEG 2 lane). Part III was
> fixed for the symmetric bug 2026-06-30; Part I was left behind. Do not
> read the 7203's compute leg as green until that lands. The screen leg is
> now Slate (unit 39): the NEW read-only `/form-7203-face/<sh_id>/`
> endpoint publishes the engine's dict; loan lines 21/22 and the seven
> suspended-loss carryovers got their FIRST inputs (debt basis and Part
> III columns (b)/(d) were unreachable from the UI before).

> **2026-07-28 session 126g — 1065 FIVE-DECIMAL PARTNER PERCENTAGES — K-1
> ITEM J PRECISION** (QA Batch-001 1065 brief item 9, **THE LAST — the
> brief is COMPLETE 9/9**; **migrations 0222-0226 STAGED, NOT applied**;
> not pushed). **THE MECHANISM:** every partner percentage column stored 4
> decimal places (numeric(7,4)), so the QA return's actual ending capital
> split — **1.00013% / 98.99987%** — was rejected with HTTP 400 "no more
> than 4 decimal places"; the return could not be keyed as Lacerte
> prepared it. **FIX:** mig **0226** widens `Partner`
> profit/loss/capital_pct (+ `_boy`), `PartnerAllocation.percentage`, and
> the `PartnerK1Computed.profit_pct` audit snapshot to numeric(8,5) —
> saves, round-trips EXACTLY, and **prints on K-1 item J** (`_pct` already
> trimmed trailing zeros; proven against the RENDERED bytes by
> template-rect sweep per the brief's PDF-values requirement).
> **D_K1_PCT100 grows column coverage:** ending profit/loss always-on (the
> allocator rides them); ending capital + the three BOY columns (item J)
> validate when IN USE (any nonzero), tolerance 0.01. RS `SCHEDULE_K1_1065`
> re-fetched — NO PCT100 diagnostic there (app-side S-21b Ken-ratified
> rule; no spec conflict); registration description updated →
> **seed_rules rerun BOTH DBs at deploy**. Client: totals + allocation row
> totals display 5 decimals (entry inputs were already free-text).
> **GATES:** NEW server `test_1065_pct_precision.py` **12** (API 400
> repro · exact roundtrip · allocation + snapshot width · item J PDF
> prints · diagnostic column/tolerance cases) · K-1/diagnostics band
> **73** · flow + 1065 band **610** · tsc **52 = baseline** · vitest
> **557 = baseline**.

> **2026-07-28 session 126c — 1065 PARTNER AUTO-SAVE IDENTITY + STABLE
> ORDERING** (QA Batch-001 1065 brief item 5; **migrations 0222-0225 STAGED,
> NOT applied**; not pushed). **THE MECHANISM:** `Partner.Meta` ordered by
> `(sort_order, name)` and `addPartner` never sent a sort_order — two
> just-added partners tied at 0 and ordered by NAME, so each blur-save's
> refresh reordered the cards WHILE the preparer typed a name (empty-name
> ties were Postgres-unstable on top). Separately, two rapid saves put two
> responses in flight and the earlier one landing LAST repainted stale data
> over the newer entry. **FIX:** mig **0225** → ordering
> `(sort_order, created_at)` (edit-independent; AlterModelOptions, no DB
> op) · `addPartner` claims an explicit slot · a save-sequence guard in
> `PartnersSection` lets only the LATEST response apply a refresh (patches
> were already bound to immutable partner ids via closures — now
> test-pinned per call). `views.py` deliberately untouched (parallel
> session owns it). ⚠ The same latent name-ordering exists on
> **Shareholder** (1120-S) — flagged, out of the brief's scope. **GATES:**
> NEW client `partnerAutosaveIdentity.test.tsx` **3** (the brief's
> regression verbatim: two partners rapidly populated with out-of-order
> responses — every PATCH id-correct, no fields crossed, exactly one
> refresh applied) · NEW server `test_partner_identity_item5` **5** ·
> affected band **645** · tsc **52 = baseline** · vitest **546** (+3).

> **2026-07-28 session 126b — 1065 PARTNERSHIP M-2 + K-1 ITEM L ROLL-FORWARD**
> (QA Batch-001 1065 brief item 4; **migrations 0222+0223+0224 STAGED, NOT
> applied**; not pushed). The audit shrank the item AGAIN: the entity M-2 was
> already fully built server-side (FORMULAS_1065 lines 5/8/9, render map, the
> D_M2_3/D_M1_ANALYSIS/EXEMPT diagnostics) — the real defects were the UI and
> the partner layer. **(1) The 1065 screen showed the 1120-S AAA/PTEP/AE&P/
> OAA table** whose M2_1a..M2_8d keys don't exist on a 1065 (every cell
> dead); `BalanceSheetsSection` now branches DATA-DRIVEN (a seeded bare
> `M2_9` ⇒ the partnership Analysis of Partners' Capital Accounts panel,
> lines 1-9). **(2) The partner card DISPLAYED a client-computed EOY but
> persisted 0.00** — every K-1 printed a BLANK item L ending capital (the
> QA's "ending capital cannot be entered"), and `current_year_increase` had
> NO writer anywhere. Per RS `SCHEDULE_K1_1065` R-K1-ITEM-L (§705):
> **mig 0224** adds `capital_other` (the missing item L row 4 — its K-1
> widget `l_other_increase` existed unwired since s125) + a
> `capital_eoy_overridden` flag; `Partner.save()` derives eoy = boy +
> contributed + current-year + other − withdrawals unless overridden
> (update_fields-safe), and 0224 backfills the only two partner rows in the
> shared DB (read-only audited: both eoy 0.00 → 7,894 / 781,403, no
> deliberate entries to preserve). Card: current-year + other editable, EOY
> = YELLOW derived input / GREEN typed override / "↺ derive" reset — the
> house provenance convention. **(3) NEW `D_M2_1`** (info; R-M2-1-TIE) ties
> M-2 line 1 to Σ partner beginning capital (B4-exempt suppressed);
> `D_K1_ITEML` gained the `capital_other` term and now polices OVERRIDES
> only; the M2_3 seed label re-worded to the 2025 face (the old "per Books"
> contradicted the tax-basis M-2 — the COMPUTED tie to M1_9 is queued for
> Ken's adjudication per R-M2-3-TIE). 1065_M2 spec mirror refreshed.
> **GATES:** NEW `test_1065_m2_item4` **13** (§705 roll-forward incl.
> update_fields + override/reset · D_M2_1 four arms · the brief's 789,297
> regression asserted on the RENDERED faces — M-2 line 9 "789,297", both K-1
> item L columns 7,894/781,403) · `test_item_l_breaks` repinned to the
> override contract + a derived-always-ties arm · affected band **663** ·
> diagnostics legs **24** · seed pins **13** · tsc **52 = baseline** ·
> vitest **543**. At deploy: seed_1065 rerun + `seed_rules` BOTH DBs
> (D_M2_1). ⚠ Until 0222/0224 apply, the checked-out code cannot serve 1065
> partner screens against the live DB (missing columns) — live QA follows
> Ken's push.

> **2026-07-28 session 126 — 1065 SCHEDULE B PAGES 2-4 RENDER MAP + SCHEDULE
> B-1** (QA Batch-001 1065 brief item 3; **migrations 0222+0223 STAGED, NOT
> applied**; not pushed). **THE RS SPEC EXISTS — `1065_B`**: s125's "no spec"
> scoping note was wrong because the RS forms LIST endpoint paginates and the
> form hid on a later page; `lookup/1065_B/export/` returns it (6 rules /
> 32-line map / 5 diagnostics / the `b1_entity_type` choice tokens). Mirrored
> to `server/specs/1065_b_spec.json`; the map was reconciled against it —
> line inventory 1-33 matches the seed 1:1 and the B1 tokens are the spec's
> choices VERBATIM (test-pinned). **RENDER leg:** every seeded Schedule B key
> now lands on the face — Yes/No pairs (x=537.6/559.2 on the LAST line of
> each question), B11/B32 single boxes (x=517.6), all inline detail boxes,
> and the PR/DI designation block; B33_PR_TIN deliberately unmapped (e-file
> only, no widget). The §743(b)/§734(b) NEGATIVE boxes sit inside pre-printed
> "$(  )" — the renderer prints the ABSOLUTE value there (test-pinned).
> **B1 is now an ENUMERATED choice** end-to-end: client select
> (`FIELD_CHOICES["1065:B1"]`), renderer token → c2_1[0..5], NEW seed line
> `B1_OTHER` (seed 355→356), migration 0223 normalizing the ONE pre-existing
> free-text value in the shared DB; an unrecognized legacy value prints
> NOTHING rather than guessing a box. **SCHEDULE B-1 (Rev. 8-2019) is a NEW
> FORM**: manifest+template (95→96), `f1065sb1_2025.py` maps ALL 65 widgets
> (type column 7pt — "Exempt organization" overflows 10pt into the country
> box), `render_schedule_b1` generates when an active partner's max ending
> profit/loss/capital pct ≥ 50 (Part I entities / Part II
> individuals+estates; TINs via `partner_display`; instructions page
> stripped), packet-wired before the K-1s + `GENERATED_FORM_RENDERERS`.
> **DELIBERATE GAPS (flagged, not built):** Sch B 3a/3b detail tables have no
> seed lines (entry gap) · Partner has no country field (foreign B-1 country
> blank) · B-1 lists DIRECT owners only (constructive ownership underivable) ·
> the RS spec's 5 diagnostics have NO app runners · RS's "B-1 RED-deferred"
> note in D_B2_B1 is now STALE (REVIEW_QUEUE — needs an RS push). **GATES:**
> NEW `test_1065_schb_render` **23** (map-vs-template validation · the 409
> regression set: Domestic LLC, 2a Yes, 4 No via override, 24 Yes, 33 No, PR
> Floyd Lance · B-1 One Heart 99% Part I / 60% individual Part II · packet +
> manifest parity) · 1065/1041/forms/flow band **1499** (5 trip-wire pins
> re-baselined: seed 356, manifest 96) · targeted modules **590** · tsc **52
> = baseline** · vitest **543**. Pages 2-4 + B-1 also verified VISUALLY
> (checkbox alignment, parens, PR block).

> **2026-07-28 session 125 — SCHEDULE K-1 (FORM 1065) GENERATION** (QA
> Batch-001 1065 brief item 1; app `13ee449`; **migration 0222 STAGED, NOT
> applied**; not pushed). **INPUT leg: was already complete** (partners,
> item J/K/L, GP, distributions all seeded + editable). **RENDER leg: was
> ENTIRELY MISSING and reported as present** — `field_maps/f1065sk1_2025.py`
> was the GENERATED STUB (111 AcroForm names listed for reference, `FIELD_MAP`
> and `HEADER_MAP` both EMPTY). **DISPATCH leg: broken two ways** —
> `render_all_k1s` queried `Shareholder` unconditionally (the reported "No
> active shareholders found for this return" on a partnership) and
> `render_complete_return`'s owner block was scoped `form_code == "1120-S"`, so
> a partnership packet printed pages 1-6 + letter + invoice and nothing else.
> All three legs now green. Field map built from GEOMETRY (template has NO
> tooltips; caption y → widget y is +9pt; the face's own `Line13/14/15/17/18/
> 19/20` subform names identify the code column) — **all 111 widgets mapped, no
> missing / no duplicate targets / no type mismatches, test-pinned.** Box code
> letters transcribed VERBATIM from the RS `SCHEDULE_K1_1065` spec's
> `IRS_2025_I1065SK1` excerpts. **Item 2 rode along** (item 1 needs item I1):
> `PartnerEntityType` 11 choices + `Partner.entity_type` + mig 0222 (backfill
> classifies ONLY `is_individual=True`; False is left BLANK — never invented),
> `partner_display.py` as the one TIN/I1/I2 source, UI dropdown, and the EIN
> fix on BOTH sides (the client's unconditional `formatSSN` was the real cause
> of a partner EIN rendering in SSN punctuation — two-digit prefix regrouped
> as three-two-four). **2 OPEN RULINGS** (REVIEW_QUEUE): box 13
> contribution-type and box 11 character codes cannot be derived from what the
> app stores; documented defaults shipped, exposed by
> `k1_1065_ambiguous_codes()`. **GATES:** NEW `test_1065_k1_package` **17**
> (asserts PDF box values by reading text at each field's template geometry —
> the filler strips widgets, so widget-value reads return nothing) ·
> 1065/1041/forms/flow band **769** · client tsc **52 = baseline** · vitest
> **543**. **STILL OPEN on the 1065:** brief items 3-9 — Schedule B pages 2-4
> render map + Schedule B-1 (template NOT in the repo), partnership M-2,
> partner auto-save identity, Schedule L IRS labels + BOY diagnostic, Form
> 8990, Georgia package, five-decimal percentages.

> **2026-07-27 session 124 — FORM 4562 `D_4562_RECON` AMENDED FOR THE §179
> BUSINESS-INCOME LIMITATION** (RS `6e61341` → R020, seeded/export-verified/
> mirrored; app leg green; **no migration**). Found while settling the 8
> pre-existing suite failures, NOT from a QA report: widening the stale
> `D_4562` family list made the rule visible on the §179 pipeline test, and it
> was raising a **BLOCKING error on a CORRECT return**. The s116 condition was
> unconditional per-destination equality, which cannot survive §179(b)(3)(A) —
> $10,000 of equipment fully elected against $8,000 of Schedule C income
> legitimately puts the ALLOWED 8,000 on Schedule C line 13 and carries 2,000
> to Form 4562 line 13, while the asset module still holds the full election.
> **Ken approved the two-part fix in-session** over downgrading the severity or
> deferring. Now: **(a)** every destination must carry at least its NON-§179
> total (less = ordinary depreciation that failed to route, the original
> silent-skip class, still blocking); **(b)** the §179 that landed across the
> business and farm schedules must equal **line 12**, the amount allowed after
> the limitation. With no §179 on the return the two parts collapse to the
> original strict equality — **strictly stronger** for the ordinary return, not
> a relaxation. RS harness re-implements the PRE-amendment condition as a
> negative control (proves the amendment changes a real verdict); **three
> perturbation controls each observed failing**. Read-only prod scan: 2 tax
> years carry a §179 asset, neither currently tripping it — the defect was
> reproducible but had not yet bitten a stored return. **GATES:**
> `test_section_179_diagnostics.py` **13** (4 new `test_recon_*` = the RS
> scenario oracles) · depreciation/diagnostics band **680** · flow assertions
> **521**. **RATIFICATIONS OPEN** (REVIEW_QUEUE): accrual Schedule F scoped out
> of part (b) · part (b) standing down in the pure-prior-year-carryover shape.

> **2026-07-27 session 124 — SUITE GATE: all 8 pre-existing failures settled**
> (app `931f4a6`; tests + one seeding fix, no product code). Re-run in
> ISOLATION first: all 8 still failed, so none was ordering noise (s108e).
> Five were **stale expectations** — the `D_4562` family list predating
> DEST/BASIS/RECON · the manifest trip-wire at 93 vs 95 (f8879 + f8878 from
> s94) · `TestOfficerCompensationFlow` on the pre-renumber 1120-S page-1
> numbering (the 2025 face is 21 = total deductions, 22 = OBI) · and
> `TestAAANegative`, which pinned "distributions can drive AAA negative" —
> **wrong law** (Reg. §1.1368-2(a)(3)(iii) reduces AAA by distributions but NOT
> BELOW ZERO; RS `1120S_M2` R002 corrected 2026-07-12). That class is
> **RETIRED**, not rewritten, because `test_1120s_spec.py` already pins both
> arms. The sixth was a **real coverage hole**: `test_8915f::TestLandingChain`
> asserts Schedule 2 line 8, whose writer is gated on the 5329 form definition
> AND on SCH_2 being seeded — the module seeded neither, so those assertions
> had never tested anything and only ever passed on seeds leaked from another
> module's `django_db_blocker` fixture. Product path proved correct first
> ($18,000 code-1 → $1,800), then the module made self-sufficient.

> **2026-07-27 session 123 — FORM 2210 RECONCILIATION + PART III FACE (QA
> Batch-001 item 10, second half)** (RS `7bdad04` seeded/verified/mirrored;
> app **NOT PUSHED — migration 0221 staged, Ken's gate**). Item 10's first
> half (the $1-3 penalty deltas) was ALREADY closed by s113's flat-7%
> correction: both QA fact patterns now reproduce the prior software exactly
> (289 and 189). **RENDER leg — was PARTIAL, now COMPLETE:** the field map
> held 5 of 46 fields; Part I lines **1/2/3/6/8** were unmapped, so the face
> printed line 9 as "the smaller of line 5 or line 8" with **line 8 BLANK**
> (the prior-year safe harbor), and the whole Part III worksheet was empty.
> Now Part I 1-9 + Section A lines 10-18 × four columns + line 19.
> **COMPUTE leg — the per-period derivation was computed and DISCARDED**
> (only installments[0] and the SUM of the underpayments were stored); now
> the full Section A grid, the safe-harbor decision, and a per-accrual trace
> (amount · dates · days · rate) are retrievable. **LINE-NUMBER CORRECTION:**
> Part III Section A is lines **10-18** on the 2025 face — the app stored the
> required installment on "18" (the face's OVERPAYMENT) and the underpayment
> on "25" (a line Part III does not have; 25 is Schedule AI). Verified against
> the face text, the widget grid, and the IRS template's own subform names
> (`SectionATable[0].Line10[0]`…`Line18[0]`). "25" is DELETED, not orphaned.
> **SECTION A ALLOCATION FIX:** the old code carried only overpayments
> forward; the face's line 14 makes each column cover the prior column's
> unpaid balance first (diverges only on a LATE catch-up). **No penalty
> changed.** **NEW:** documented source override (2210 line 19 + 1040 line 38
> move together per F2210-006-01) + `D_2210_SRC` / `D_2210_TIE`. **GATES:**
> new `test_2210_reconciliation_item10.py` 23 · existing 2210 suites 54 ·
> flow assertions 521 · client 7 · vitest 543 · tsc 52 = baseline. **STILL
> OPEN on this form:** Part II boxes A/B/D/E unmodeled (so no IRS2210 is
> transmittable; box D is a penalty-REDUCING election) and Schedule AI page 3
> unfilled — both in DEFERRAL_AUDIT.

> **2026-07-26 session 120 — FORM 4562 LEG 4: CONVERSION-SCALE ENTRY (QA
> Batch-001 item 6, approved proposal leg 4 — FINAL LEG)** (app `202559d`;
> no migration; entry tooling only — no engine/tax-law change, no RS
> amendment needed). **Published CSV template + parser**
> (`apps/imports/importers/csv_depr_parser.py`): header aliases ·
> $/comma/parens amounts · M/D/YY dates · per-row errors · EXAMPLE-row
> skip · export-only columns round-trip. **One import path** for template
> CSVs, PASTED spreadsheet rows (tab-separated w/ header), and Lacerte
> TXT: preview shows each row's PROJECTED current-year depreciation +
> fully-depreciated badge + resolved Activity; commit is idempotent
> (X-Idempotency-Key) and runs the FULL recompute (old commit left the
> return stale). **Activity column** resolves to Sch C/E/F pickers
> (C:/E:/F: prefixes; blank = single-activity auto-link, else unassigned
> + D_4562_DEST guides). **Lacerte boundary fixes:** snake-code group
> labels → model display values (engine's Land/Vehicles branches never
> saw the raw codes — a Lacerte land asset would depreciate), and bonus %
> suggested ONLY for year-1 assets (continuing assets got 40% stamped,
> re-reducing remaining basis vs the D_4562_BASIS keying). **NEW GET
> depreciation-csv** (template=1 / round-trip export) + **POST
> depreciation/bulk-update** (activity arm+FK · method · convention ·
> life · group; return-scoped FK checks). **Client:** Paste rows panel ·
> Template/Export CSV buttons · filter bar (search/activity/group/status
> incl. Fully depreciated) · row checkboxes + group select-all + bulk
> toolbar · fully-depreciated row badge. **ACCEPTANCE MET:** 7 active +
> 39 fully-depreciated legacy rows import without changing the
> current-year result (test-pinned; farm depreciation 6,858 unchanged,
> every legacy row $0 across federal/AMT/GA). Gates: NEW
> `test_4562_leg4_bulk_entry.py` **16** · depreciation regression subset
> **97** · lacerte parser **39** · NEW `depreciationLeg4Entry.test.tsx`
> **10** · client vitest **511/511** · tsc 52 baseline. Live demo probe
> green (paste → preview $245 + badge → commit +245 exactly → status
> filter → bulk repoint to the 8825 property (800→1,045) →
> template/export 200s → demo DB restored to baseline). **Item-6
> depreciation rebuild: ALL FOUR LEGS COMPLETE.**

> **2026-07-26 session 118 — FORM 4562 LEG 3: BASIS FIDELITY + §280F
> PARALLEL-ARM CAPS (QA Batch-001 item 6, approved proposal leg 3)** (RS
> `51371ec` — R018/R019 + D_4562_BASIS, seeded + export-verified, mirror
> refreshed; app `bb2935e`; mig 0218 + seed_rules BOTH DBs). **Split basis
> history:** `original_cost` (null ⇒ equals cost_basis — pre-Leg-3 fleet
> byte-identical) + `prior_bonus_depreciation`; cost_basis stays the
> engine's depreciable-basis input (no recompute change — Ken's
> add-fields-only pick). Barn pin: 9,010 / 4,505 prior bonus / 4,505
> depreciable → current still **266**; card shows derived Accum (EOY)
> 5,971 / Adjusted Basis 3,039 (serializer year-aware — no year-1
> §179/bonus double count). **Disposal + §1250-additional math on the
> split fields at every site** (compute / views / rules_4797 / renderer /
> MeF read_model — bridge parity) via the ONE property
> `disposal_cost_basis`. **§280F caps now bind the AMT refigure (same
> table as federal — §280F(a)(1)(A) statutory derivation, i6251 SILENT,
> flagged) and the GA arm (NO-bonus table — §168(k)(2)(F)(i)
> nonconformity; GA was previously UNCAPPED)**, and the cap runs AFTER
> the ≤50% SL recompute (previously escaped it). Both §280F judgment
> calls → REVIEW_QUEUE. **New diagnostic:** D_4562_BASIS (effect-scaled
> error/warning, only when original_cost keyed). Stale-pin repair:
> schedule_e_depreciation_flow 6,812.59 → 6,813 (the cents pin the s116
> repin missed; fails on unmodified HEAD). Gates: NEW
> `test_4562_leg3_basis_fidelity.py` **12** · depr/4797/render **148** ·
> flow **521** · mef_1120s **75** · schF **20** · vitest **469** · tsc 52
> baseline. Live demo probe green (fields render → 9,010 save → Saved ✓ →
> adjusted 8,460 → cleared/restored). ▶ Leg 4 queued (paste grid / CSV /
> bulk assignment / legacy inventory).

> **2026-07-26 session 117 — FORM 4562 LEG 2: ENTRY INTEGRITY (QA Batch-001
> item 6, approved proposal leg 2)** (app `73a9d50`; client-only — no
> migrations, no RS spec change, no compute/render change). **Add Asset is
> now the s107 draft-row convention:** no placeholder POST — the draft
> lives in client state, persists ONCE on the first non-blank description
> carrying everything typed before it, concurrent blurs serialise onto ONE
> create via the in-flight promise ref, and a failed create keeps the card
> + values with the DRF message + Retry. **Per-asset FIFO save queue:**
> every field save chains behind the previous one, so a delayed response
> can never interleave with a later edit (the QA's "delayed autosave
> overwrote a different asset's name/date" class). **Visible
> Saving…/Saved/Not-saved state** in the edit-card header; Add / Edit /
> Del / Import / Close blocked while a save is unresolved. **The
> cross-asset clobber is dead:** the edit card is keyed by asset id, so
> switching rows remounts fresh `defaultValue` inputs instead of
> inheriting the previous asset's DOM text (the s111 unkeyed-card
> defect). Close keeps an unsaved-but-typed draft; an untouched empty
> draft is discarded; Add Asset returns to an existing unsaved draft.
> Gates: NEW `depreciationEntryIntegrity.test.tsx` **10** (single-create
> race guard + remount fix pinned) · vitest **469** · tsc 52 baseline.
> Live demo probe green (Add → zero requests → draft card + unsaved chip
> → description blur → exactly one POST 201 → Saved ✓ → delete restored
> demo). ▶ Legs 3-4 queued (basis fields + §280F parallels · grid/CSV
> bulk).

> **2026-07-26 session 116 — FORM 4562 LEG 1: 1040 DESTINATION ROUTING +
> PER-ASSET ROUNDING + RECONCILIATION (QA Batch-001 item 6 P0, Ken GO +
> 4 ratifications)** (app `3dfb977`+`0961407`; RS `5e6ffa3`+`37f565d`, new
> loader `load_4562_destination_rounding.py`; mig 0217 + seed_rules BOTH
> DBs; deploy verified live `index-BZjYNARY.js` vs 0-hit baseline). **The
> Benkoski silent miss fixed:** the 1040 Flow To dropdown offered the
> ENTITY farm arm (`sched_f`), whose write targets "F14" — a line that
> does not exist on a 1040 — and `_set_field_value` swallowed the miss;
> the module displayed "$4,069 → Schedule F" while Schedule F line 14
> stayed 0 (farm loss short 4,068 · AGI over 6,102). The true 1040 arms
> were UI-unreachable and the serializer NEVER exposed `schedule_f`. Now:
> 1040 Flow To = Sch C/E/F with business/farm/property pickers; entity
> arms never route on a 1040 (incl. page1's transient stamp of the 1040's
> OWN line 14 = std-ded+QBI); per-activity group cards w/ "flows nowhere"
> warnings; addAsset auto-links a single-activity return. **Rounding (RS
> R017, Ken-ratified house convention):** the engine's public boundary now
> returns WHOLE-DOLLAR reported amounts (penny internals in
> `_calculate_asset_depreciation_exact`); destination totals sum rounded
> per-asset amounts (TaxWise parity: Benkoski = 4,068, not
> ROUND(4,069.03) = 4,069). **New diagnostics:** D_4562_DEST (unroutable
> asset; effect-scaled error/warning) + D_4562_RECON (module vs
> destination mismatch = blocking — the permanent guard against the
> silent-skip class). **Mig 0217** (Ken-ratified policy, audited
> before+after BOTH DBs): prod 50 `sched_f` → `schedule_f` ALL auto-linked
> (every return single-farm) · 1 `page1` → `schedule_c` linked · 1 blank
> $0 asset left red; demo 0 rows. **Benkoski PROD recompute exact:** Sch F
> 14 = 4,068 · farm loss 6,642 · AGI 20,729 (the QA expected column). Live
> demo probe green (auto-link → 143 whole → line 14 → delete clears; demo
> restored). Gates: NEW `test_4562_leg1_dest_rounding.py` **7** · engine
> 92 · flow **521** · schF-orch/topic8/schL 86 · vitest 459 · tsc 52
> baseline. FA-4562-DEST-01/ROUND-01 staged in RS (surgical-refresh rule —
> not yet in the app FA export; the pytest leg pins the same flows).
> ▶ Legs 2-4 queued (entry integrity · basis fields+§280F parallels ·
> grid/CSV bulk).

> **2026-07-26 session 115 — FORM 8962 PART IV (shared-policy allocation)
> SHIPPED — the s75 "Parts IV/V unmodeled" boundary HALF-CLOSED** (app
> `9e13f89`; RS `16a5bc4`; mig 0215+0216 + seed_form_8962 42 lines +
> seed_rules on BOTH DBs; deploy verified live `index-q3S2nCYI.js`). QA
> Batch-001 item 9's open half. R-8962-PART4 rewritten to the face's own
> line-34 mechanic (verbatim excerpt; widget dump + IRS8962.xsd agree): the
> 1095-A is entered AS RECEIVED and `_aggregate_1095a` multiplies each
> covered policy-month by the entered percentages (whole-dollar per
> policy-month; blank pct = retain 100%) — ONE aggregation feeds compute,
> print, AND e-file. New Form8962Allocation rows (FK 1095-A) drive line 9,
> the printed 30a-33g grid + line 34 Yes, and the MeF
> SharedPolicyAllocationGrp; lines 9/10 checkboxes now print (never filled
> before). 4 new diagnostics (PART4 EMPTY/OVERLAP/TOO_MANY errors +
> BLANK_PCT warning); the s106e annual trio spec-homed in RS. Gates: part4
> leg 15 · 8962 family 44 · efile sweep 952 · flow **521**
> (FA-1040-8962-07) · vitest 459 · tsc 52 baseline · RS harness ALL PASS
> (7 scenarios incl. T7: 1%-retained → 13/11/11 → repay 132). Live demo
> probe: grid reveal + row round-trip; demo DB restored. **Part V (marriage
> alt) stays flag-only** (DEFERRAL_AUDIT s115; REVIEW_QUEUE recommends
> RED-gating the flag until built).

> **2026-07-25 session 109b — THE TWO 8879 FOLLOW-UPS KEN ORDERED: `D_8879_NEED`
> DROPPED TO INFO (spec-led) + ONE DATE-ENTRY CONTRACT.** No migrations.
> **(1) THE SPEC CHANGE STARTED AT RULE STUDIO, which owns this field** — loader
> amended, harness **79/0**, seeded to RS prod, deployed export verified, and the
> cached mirror `server/specs/8879_spec.json` refreshed from that export (the only
> diff vs the old mirror was that one severity; rules/line_map/facts/tests/metadata
> byte-identical). RS `8a30b6c`. The rule states a FACT — this return requires an
> 8879 — that nothing the preparer can do will clear; an unclearable warning is
> unactionable, and this one did measurable harm (the Batch-001 tester read the
> surviving finding as evidence the record had been lost and filed a P0 on it,
> when it was proof the record still existed). **Enforcement did not move:
> `D_8879_UNSIGNED` stays the ERROR that blocks transmission**, and both the RS
> harness and a new app test pin the pair so it can never go all-non-blocking.
> ⚠ **THE SEVERITY LIVES IN TWO PLACES IN THE APP** — the `_finding()` the rule
> emits AND the `RULES_8879` registration that seeds the DB rule row the
> diagnostics catalogue reads. Moving only the first would have left the UI filing
> the finding under one severity while the catalogue advertised another; a test now
> asserts they agree, and another reads the cached spec mirror so app-vs-spec drift
> fails a test instead of reaching preparers. `seed_rules` re-run + verified on
> BOTH DBs, and confirmed in the running app via `/api/v1/diagnostic-rules/`.
> **(2) THE DATE FIX — and it was worse than reported.** The 8879/8878 were the
> app's only raw `<input type="date">` signature fields (every other date uses the
> shared masked `DateInput`), which is why they took only `2026-07-23` while the
> preparer date took `07/23/2026`. Swapping them onto the shared control exposed
> the MIRROR-IMAGE defect in that component: **it silently mangled ISO input —
> `2026-07-23` displayed as `20/26/0723` and emitted `0723-20-26` (month 20, day
> 26, year 723) to all 17 of its consumers.** Probed and confirmed BEFORE changing
> anything. `DateInput` now normalises a whole ISO date arriving in one change
> (paste/autofill/scripted set) and **never emits a date that does not exist** —
> month 13 and 31 February yield "" (the same path a half-typed date takes, so
> nothing is written) with the field flagged `aria-invalid` + red ring, instead of
> pushing a nonsense date at the server to surface as a generic "Save failed".
> Gates: **vitest 408** (was 396; DateInput 5→12, singletonCardPersistence 7→11) ·
> tsc 0 · `test_8879_8878` 33→**35** · 8879 + flow-assertion bands **560** · core
> diagnostics bands **109**. **Live-verified on the DEMO project**: digits mask as
> typed (0 → 04 → 04/1 → 04/15 → 04/15/2026) · a pasted `2026-07-23` normalises to
> `07/23/2026` and PATCHes `{"primary_signed_date":"2026-07-23"}` · `02/31/2025` is
> flagged and sends NOTHING · correcting it clears the flag and saves; scratch
> record deleted. ⚠ **Checked rather than assumed:** a synthetic `focusout` in the
> browser pane produced TWO identical PATCHes, so the real event path was re-checked
> under RTL — exactly one commit per entry (now pinned; this codebase has been
> bitten by double-commit paths before). Also worth carrying: **React's blur
> delegation listens on `focusout`, not `blur`** — a synthetic `blur` never reaches
> `onBlur` in the hidden pane, which is why s108 believed blur-driven fields were
> unverifiable there. They are verifiable; dispatch `focusout`.

> **2026-07-25 session 109 — FORM 8879 "PERSISTENCE" FIXED — and the SERVER WAS
> RIGHT ALL ALONG (client-only; ZERO server source changed, no migrations, no DB
> writes).** The QA-ranked P0 the external backlog dropped: a signed MFJ 8879 —
> both PINs, both dates, Part I snapshot — came back as "Start Form 8879" with
> every field blank after a trip to the Forms view. **Nothing was ever lost.** The
> QA run's own diagnostics still reported `D_8879_NEED` after the "loss", and
> `d_8879_need` returns `[]` when the Form8879 row is absent — so the row provably
> survived the navigation that appeared to destroy it. The whole defect lived in
> the client seam: a self-managing card seeds `useState` from its `initial` prop
> once and thereafter PATCHes the server directly, never telling FormEditor — so
> `returnData.form_8879` stayed at its page-load `null` for the entire session,
> and the Payments tab is conditionally rendered, so every tab switch remounted
> the card onto that stale seed. **NINE cards shared the shape** (8879 · 8878 ·
> 4868 · 8888 · 9465 · payment vouchers · 2848 · 3115 · 2553) — all nine fixed,
> not just the reported one. Fix: an `onChange` on every card that reports only
> what the SERVER confirmed, and a `setSingleton` in FormEditor that records it
> in place (a key write, not a refetch — none of these cards feeds compute from
> the client, and a refetch would risk stomping unflushed keystrokes).
> **The lesson beyond this form: a "self-managing" card is only safe while it is
> mounted.** Anything conditionally rendered must report its server state upward,
> or its parent's copy becomes a stale seed that silently un-does completed work.
> Gates: NEW `singletonCardPersistence.test.tsx` **7** → vitest **396** (was 389)
> · tsc **0** · NEW `tests/test_8879_persistence.py` **7** — which passed on the
> FIRST run with no server change, the cleanest proof the DB half was never
> broken; it pins the reload payload, the `?fresh_return=1` payload (which
> replaces returnData mid-session and would silently re-stale the seed if it ever
> dropped the key), and get_or_create idempotency · `test_8879_8878` **33/33**
> unchanged. **Live-verified on the DEMO project** (John & Judy Jones MFJ 1040,
> the same return s94 probed): started → Forms → back to Input keeps the card ·
> PINs 54654/16546 + both 07/23/2026 dates + snapshot survive a HARD RELOAD and a
> second Forms round trip ("within tolerance") · Form 8879 present in the
> generated package · scratch record deleted, demo left as found. Two adjacent QA
> findings filed to REVIEW_QUEUE rather than fixed silently: `D_8879_NEED` is an
> unclearable warning (a spec-severity call, and the very thing that misled the
> tester), and the 8879 date fields accept only ISO while preparer dates accept
> `07/23/2026`.

> **2026-07-24 session 107 — SCHEDULE D `+ Add transaction` FIXED (entry layer only;
> no migrations, no compute code, flow band re-ran green).** The button POSTed a
> blank description, the API 400'd it, and no editable row ever appeared — Schedule D
> could not be started at all even though Topic 9 has been six-legs-complete since
> 2026-06-13. **The lesson for the P1 audit: a green leg row is not proof the leg is
> USABLE.** Topic 9's input leg was genuinely built and tested — the tests exercised
> CRUD against rows that already existed, so the empty-list path was never covered.
> Expect the same shape on the other P1 forms: audit ENTRY from an EMPTY form, not
> from a seeded fixture. Fix shape: a client-side draft row held in component state,
> persisted on the first valid required field, with concurrent blurs serialised onto
> one create so fast entry can't split a record. Gates: vitest 342 → 355 ·
> NEW server suite 10 · Schedule D / 8949 / GA-500 / flow bands 582 passed · tsc 0 new.

> **2026-07-19 session 101 — 1065 SCHED_B 2025-FACE RENUMBER (the s99b
> call, Ken's go) — mig returns.0208; flow 518 stands.** The tts 1065
> Schedule B block was a stale pre-face paraphrase (own numbering; face
> Q4 lived on app B6; carried dead questions old-B2 disregarded-entity +
> old-B5 Form 8893/TEFRA; MISSED face Q12, 13a, 14, 15, 17-22, 24-29,
> 30 digital assets, 32, 33 BBA/PR). Now FACE-TRUE: 67 rows = questions
> 1-33 verbatim (f1065.pdf pp. 2-4 + RS 1065_B line map; 10e/13b/31
> reserved, not seeded) + inline detail fields (underscore keys:
> B8_COUNTRY, 10b/10c/10d amounts, B22/B25 amounts, B28 vote/value %,
> B33 B-2 total, the PR designation block; B33_PR_TIN = app-boundary,
> not on the face, kept for MeF). Mig 0208: collision-safe in-place
> renames (FFVs ride the FK; B6→B4 the headline), dead-key deletes,
> the B12b §743(b)/§734(b) one-to-two split ('No' propagates to both,
> 'Yes' to neither — none existed). Re-keyed in lockstep: the Q4
> auto-answer (`_auto_answer_b4_1065_db`), D_L/M1/M2_EXEMPT waiver
> gates, the GATE-SMALL-PTNR FA runner pins, client Auto pill
> (1065,B6)→(1065,B4) + 1099-pair conditional B16a/B16b + the 1120-S-
> only header gate. NEW seed pins: test_schedule_b_face_keys +
> test_schedule_b_face_labels (the s100 label-pin discipline). Gates:
> flow **518** · seed/Q4/L/M suites 31 · 1065 band + registry sweep 110
> · returns 77 · 1120-S schb 27 · tsc 0 · vitest 300 · BOTH DBs
> migrated+reseeded+audited (every carried answer landed face-true) ·
> live demo probe green (face numbering 1-33 DOM-verified; B4 amber
> Auto 'No' on Blue Ridge; 1120-S panel regression clean). Print of
> Sch B answers remains a pre-existing gap (no AcroForm map — DEFERRAL
> s101); the renumber makes the future map's keys equal the face.

> **2026-07-19 session 100 — RENUMBER-STALE-KEY SWEEP (the s98 chip) —
> plumbing unit, no migrations, flow 518 stands; ONE LIVE BUG FIXED.**
> **THE BUG (the label-pin catch): `OTHER_DED_LINE_KEY["1120-S"]` stayed
> "1120S_L19" after the 2025 face inserted line 19 (Form 7205 §179D) —
> the other-deductions rollup wrote its total into the
> ENERGY-EFFICIENT-BUILDINGS box** (MeF face-correct itself, so an
> e-filed TB return would have emitted the total as
> EnergyEffcntCmrclBldgDedAmt; print likewise). Fixed → `1120S_L20`;
> the four test_returns pins that had baked the bug in retargeted; BOTH
> DBs audited — ZERO live returns carried a stale line-19 value.
> **Plus:** 6 DEAD TB-mapping targets (incl. 1065 Guaranteed Payments —
> silently dropped on every TB import) and **14 RESOLVES-WRONG
> balance-sheet targets** (the shifted-by-one class: 1065 land →
> depletable assets, partners' capital → other liabilities; 1120-S
> capital stock → other liabilities, shareholder loans → tax-exempt
> securities) re-keyed to the verified 2025 labels; COGS → the 1125-A
> purchases key (1120-S precedent); GP → face line 10 (mapping key
> added; partner rollup supersedes); 1120-S "Retained Earnings" rule
> DROPPED (engine-owned L24d). SUBSCHEDULE_CONFIG gained per-entry
> `form_codes` gating (bare-key collision class: 1065 line 5 = net farm
> profit). NEW **tests/test_line_key_registry_sweep.py 13/13** — resolves
> + LABEL PINS across other-ded keys, all three TB seeders, sub-schedule
> targets, GA pull maps, and formula-pass targets. Gates: flow 518 ·
> returns/mappings/imports/other-ded 645 · S5/S6 parity + B11 13 · live
> MappingRule audit 166/166 resolvable on BOTH DBs · reseeded BOTH DBs
> (seed_1065 + both mapping seeders) · no client code (tsc/vitest
> untouched). Four convention calls → REVIEW_QUEUE s100.

> **2026-07-19 session 99 — 1065 SCHEDULE B Q4 AUTO-ANSWER (S-21c, the
> s71-ratified queue item) — SPEC-FIRST unit complete; no migrations;
> flow 518 stands.** RS `1065_B` amended IN ITS OWNING LOADER
> (`load_1065_l_b.py`, RS `7a55f57`): NEW **R-B4-AUTO** — the app derives
> Q4 (app row **B6**) as a derived-YELLOW, preparer-overridable answer:
> **4a** = total receipts < $250,000 STRICT per the i1065 (2025) Q4
> VERBATIM sum (p1 1a; p1 4-7; K 3a/5/6a/7; income-or-net-gain K
> 8/9a/10/11; 8825 lines 2/21/22a — positive-only, the R006 interpretive
> mirror) · **4b** = L15d (the item F amount) < $1,000,000 STRICT ·
> **4c** = PRESUMED TRUE (Ken-ratified) · **4d** = derived TRUE under
> 4a·4b (no M-3 support; item-J thresholds unreachable; the
> reportable-entity-partner edge → preparer override — REVIEW_QUEUE s99a).
> B-6/B-7 boundary scenarios seeded; stale "build-gap #3" notes closed
> across R-B4-SMALL / D_B4_SMALL / GATE-SMALL-PTNR-B; FA mirror
> refreshed export-minus-pending (39). tts (`b7f77b6`):
> `_auto_answer_b6_1065_db` after the 1065 formula pass (the B11 clone;
> override-respecting `_set_field_value`); the WAIVER WIRING was already
> live (D_L_EXEMPT/D_M1_EXEMPT/D_M2_EXEMPT suppression, GATE-SMALL-PTNR
> pinned); B6 label re-cut FACE-VERBATIM (four conditions + the waiver
> sentence; ≤500-char column) + seed_1065 reseeded BOTH DBs; the client
> Auto pill generalized to (1120-S, B11) ∪ (1065, B6) by form_code.
> Gates: NEW test_1065_schb_q4 **7/7** (boundaries at exactly
> $250K/$1M, loss exclusion, 8825 rents, override wins) · flow **518** ·
> 1065+SchB band **153** (L/M tie fixtures now answer Q4 'No' by
> override — the auto-answer had made the small fixtures legitimately
> exempt) · tsc 0 · vitest 300 · live demo probe (Blue Ridge 1065:
> B6 auto-'false' off 1a=485,000; amber Auto pill + amber No chip
> DOM-verified on vite+django-demo; probe user cleaned). The tts
> sched_b FACE-RENUMBER (stale paraphrase block) queued → REVIEW_QUEUE
> s99b.

> **2026-07-15 session 94 — FORM 8879 + 8878 (the e-file signature-
> authorization PRINT PAIR; WO-33, the next NEW autonomous item after the
> six-leg set) — ★★ UNIT COMPLETE: input · compute · render · diagnostics ·
> extract-gate · FAs, all legs green. NO MeF document BY DESIGN.** RS specs
> `8879` + `8878` (WO-33 — Ken approved in-session "approve WO-33", four
> seams adopted as recommended; RS seeded `ea28aff`, FAs activated `0d724cb`;
> mirrors 8879_spec.json/8878_spec.json verbatim). **STRUCTURAL HEADLINE:
> NEITHER FORM TRANSMITS** — ERO-retained print artifacts; the Return Header
> PIN block the app already e-files IS the electronic signature. The two NEW
> models (Form8879 + Form8878, migs 0206/0207 + RLS BOTH DBs) are the DB home
> for what was a passed-in `SignatureInfo` dataclass (the SIGNATURE_GAP).
> **INPUT**: Form8879 (method/PIN-entered-by/PINs/ERO EFIN+PIN/firm/signed
> dates/authorizing-return/the signed-at Part I snapshot/SID/prior-year auth/
> the 3 self-select bars) + Form8878 (extension_form/PINs/ERO/the line-7
> snapshot); serializers w/ read-only re-derived `analysis`; form-8879 /
> form-8878 GET/PATCH/DELETE (1040-only; row-locked; NO recompute — print-
> only); Form8879Card + Form8878Card on the Payments tab (the 8879 card has a
> "Snapshot Part I at signing" button); D_8879_/D_8878_ → payments NavScope.
> **COMPUTE**: compute_8879_8878 pinned to RS scenarios 8879-A..H + 8878-A..F
> — the 4-row need chart (**the PP-own-PIN row 4 STILL prints w/ Part III;
> the only skip = self-select + taxpayer's own PIN**), the 5-row 8878 chart
> (**no EFW = no 8878 ever; 2350 stops at Part II**), PIN hygiene (5-digit
> non-zero; EFIN 6 + PIN 5), **seam a: Part I L3 = 1040 line 25d**, **seam d:
> the $50/$14 re-sign tolerance vs the signed-at snapshot**, **seam b: the
> 1040-X arm reads amended col-C (1C/11C/12C/22/20) [verify @ ATS]**, the
> 8878 line-1 = the season Form4868's line 7 (derived FRESH, s88 reverse-
> cache class), the self-select bars. rules_8879 (9) + rules_8878 (7)
> registered in the runner. **RENDER**: f8879 (Rev. 01-2021, continuous-use)
> + f8878 (2025, year-dated) AcroForms — manifest 93→95, hash-verified; ALL
> field-map names probed vs the PDFs (comb PINs=5, EFIN/PIN=11; the shared-
> leaf checkbox pairs keyed by FULL name — s69); **positional render verified
> — every value in its correct box incl. seam-a 25d**; the need-gate IS the
> render gate; NEW "signature" packet tier (the 8879 rides WITH the return at
> (1,2); the 8878 is standalone); render-8879/render-8878 endpoints.
> **EXTRACT GATE (seam c)**: extract_return REFUSES an unsigned or re-sign-
> required 8879 (UnmappableValue; only bites when a card exists — revisit @
> S-17g). **FAs**: FA-8879-NEED/RESIGN + FA-8878-EFW activated + runner
> `_run_8879_8878_assertion` (both ladders); tts mirror 415. Gates: NEW
> test_8879_8878 **33/33** · flow **518** · test_4868 + test_1040v_es green ·
> returns 110 · core 1040 MeF/extract 105 · tsc 0 · vitest 300 · **live demo
> probe** (Jones 1040: both cards render; the 8879 banner painted "required ·
> Parts I,II,III · Part I: AGI 35,492…" from the live return + the unsigned-
> transmit gate w/ MFJ wording; probe row cleaned). WO-33 → ✅ built.

> **2026-07-15 session 89 — FORM 8915-F (Spine S-22b; the LAST of the
> six Gate-1-dispatched tts legs — THE SET IS COMPLETE) — ★★ UNIT
> COMPLETE: input · compute · render · MeF document · FAs, all legs
> green.** RS spec `8915F` (WO-32, approved+seeded s83; mirror
> verbatim-current). **INPUT**: NEW `Form8915F` (mig 0204 + RLS mig
> 0205, BOTH DBs — the model+RLS pair rule) — one row per OWNER per
> item-B DISASTER YEAR (married = a separate form per spouse; unique
> constraint); items B/D + the full Part I-IV face inputs (the lines
> 2-4(b) allocations + line 20 blank = derived, typed wins — the L5
> house rule) + NEW `Form8915FDisaster` child rows (item C FEMA
> number + declaration/begin/END dates + repeat/Part-IV flags; nested
> REPLACE-ALL writes); Form8915FSerializer carries the read-only
> re-derived `analysis`; `forms-8915f` list/create + detail
> PATCH/DELETE (1040-only; PATCH row-locked; every mutation
> recomputes — this card FEEDS compute); "Disaster Dist. (8915-F)"
> Income-group tab; D_8915F_ → form_8915f NavScope. **COMPUTE**: NEW
> `compute_8915f` pinned to all 10 RS scenario oracles — **the
> 179/180-day ONE-day asymmetry pinned BOTH directions against every
> published date example incl. the 12/29/2022 SECURE-floor arm (the
> IRS's own Appendix-D off-by-one class)** · the 1a-1e ladder + the
> single-new-disaster shortcut · the Rev-12-2025 5a/5b redesign
> (5b(b) = min(sum(2-4(a))−5a, 1e)) · sequential-fill (b)-column
> allocation · line 6 waiver / line 7 excess (Part IV overlap reduces
> 7) · the ÷3.0 spread (whole-dollar ROUND_HALF_UP — the flagged
> convention, REVIEW_QUEUE s89) + the 11↔22 matched opt-out boxes ·
> the $22k/$100k per-disaster cap (F8915F-003 by construction) · the
> Part IV [-180d, end+30d] receipt window. **THE LANDING MOVES**: 5b
> −= line 10 += line 15 · 4b −= line 20 += line 26 · line 30/32 route
> by plan type — runs AFTER compute_8606_db; **the 8606 face SPLITS
> 15a/15b/15c + 25a/25b/25c off the 8915-F lines 18/19
> (owner_lines gained the QDD ties — print/XML/compute agree)**;
> **the 5329 line-6 waiver: owner_inputs suppresses the early-tax
> base (line 6 NEVER generates a 5329; line 7 + Part IV 32 keep
> their exposure)**. 15 D_8915F_* code-registered + seeded BOTH DBs.
> **RENDER**: f8915f Rev-12-2025 AcroForm (manifest 92→93,
> hash-verified; **the lines-1-5 two-column table SHADING-PROBED —
> 1a-1e fill column (b), 5a fills column (a), the s71 positional-map
> class**; all widgets label-verified; line-8 No-first vs lines-16/17
> Yes-first checkbox order caught); one 4-page copy per row w/ the
> owner's own name/SSN; >2 disasters check the face box + ride a
> Statement page; joins the packet (seq 915) + `render-8915f`
> endpoint. **MEF**: NEW IRS8915F document (ReturnData1040 slot 2014
> between IRS8912 and IRS8917 — emitted between IRS8911 and its
> ScheduleA loop, maxOccurs=6, per-document PersonNm/SSN); item A/B
> XSD choices (2020/2021 ride the checkbox+attribute forms); the Part
> I FEMA groups (max 20) + Part IV groups (max 10, +end date) fan out
> from the SAME disaster rows; extract bridge-gates on the SAME
> form_lines derivation — refusals name the paper path (EM/pattern/
> missing dates/year enums/opt-out mismatch/>6 forms/Part-I-needs-a-
> dated-disaster); live-XSD valid with TWO documents (the
> separate-spouse shape). **FAs**: FA-8915F-CAP/SPRD/LAND activated
> (RS reseeded, export 413 verified, mirror 412 export-verbatim
> ASCII, `_run_8915f_assertion` in BOTH chains) — **flow gate 512 →
> 515**. Suites: NEW test_8915f **49/49** · flow 515 · 8606/5329/
> retirement seam band 150 · FULL efile/mef/scenario band 966 ·
> tts_forms band 355 (trip-wire re-pinned 92→93) · tsc 0 · vitest
> 300 · live demo probe green (the card CRUD + the banner painting
> 1e/6/7/15 + the per-disaster 179/180 dates live · ORM: 5b 10,300 →
> 3,433 → delete-restores 10,300 · diagnostics fired exactly
> LANDINGS+WAIVER · render 200 %PDF; demo return restored).
> Boundaries → DEFERRAL_AUDIT s89 (10); the rounding ratification →
> REVIEW_QUEUE s89. WO-32 → ✅ DONE in RS.

> **2026-07-15 session 88 — FORM 4868 (Spine S-22b; the FOURTH of the
> six Gate-1-dispatched tts legs) — ★★ UNIT COMPLETE: input · compute ·
> render · MeF (a NEW SUBMISSION FAMILY) · FAs, all legs green.** RS
> spec `4868` (WO-31, approved+seeded s83; mirror verbatim-current).
> **INPUT**: NEW `Form4868` singleton (mig 0202 + RLS mig 0203, BOTH
> DBs — the model+RLS pair rule) — face L4/L5/L7 (blank = derived,
> typed wins) + L8/L9 boxes + fiscal-year dates (paper-only bar) +
> filed date + e-payment confirmation (the no-form path) + the
> extension's OWN EFW election (amount/date, distinct from the s76
> return-side efw_elected) + ride-the-ES-debits flag;
> Form4868Serializer carries the read-only re-derived `analysis`;
> `form-4868` GET/PATCH/DELETE (1040-only, PATCH row-locked from
> birth); Payments-tab card (seq guard); D_4868_ → payments NavScope.
> **UNLIKE the print-only payment trio this card FEEDS COMPUTE** —
> R-4868-CREDIT: line 7 lands on Schedule 3 line 10 as a YELLOW
> feeder in compute_sch_3 (relation-guarded: no Form4868 row → line 10
> stays direct entry; override survives; DELETE clears the
> non-overridden derive — the s66 stale-derive class caught live) →
> L15 → 1040 line 31 → 33. **The L5 derive sums the COMPONENTS
> (25d+26+27..31 − Sch3 L10), never line 33 itself** — subtracting L10
> from a stale/overridden 33 diverges one L10 per recompute (caught by
> the endpoint test). **COMPUTE**: NEW `compute_4868` pinned to all 10
> RS scenario oracles — L6 = max(0, L4−L5) (the face floor) · the
> F4868-001/-002 windows (Apr-15 / Jun-15 with a box; on-or-before at
> equality; after the period end) · extended due Oct-15 / the DERIVED
> line-9 Dec-15 · the 90% two-prong safe harbor (met at equality) ·
> the payment-triggered signature/jurat ladder (R0000-098; the
> two-value enum by PIN type) · the FPYMT-052-02 EFW tie · the
> where-to-file chart BOTH columns as partition-pinned rosters (WITH
> payment: 9 → Charlotte 1302 + 42 → Louisville 931300; WITHOUT:
> 13 Austin / 21 KC / 17 Ogden; foreign 1303/0215) — **the GA
> Charlotte trap is FOUR-way and cross-module-pinned (V 1214 / ES 1300
> / 4868 1302 / foreign 1303)** · efile_blockers = the bridge gate
> (late/fiscal/EFW-mismatch/duplicate-confirmation/EFW prereqs). 16
> D_4868_* code-registered + seeded BOTH DBs (NOATTACH = a structural
> no-op; R0000-195 holds by construction). **RENDER**: f4868.pdf
> (2025, self-contained 4pp) downloaded fresh (manifest 91 → 92);
> f4868_2025 map (17 widgets label-verified — 6 face lines incl. both
> checkboxes on-state "1" + 10 header slots; the page-3 e-pay
> worksheet field deliberately unmapped); render_4868 = face page
> only; **suppression IS the render gate** (a recorded e-payment
> confirmation = the extension already processed; printing would
> duplicate, IND-900); **STANDALONE ONLY — the 4868 NEVER joins the
> return packet** ('Don't attach the 4868 to the return' — the face's
> own instruction; pinned by test); render-4868 endpoint (suppressed →
> the explanation). **MEF — a NEW SUBMISSION FAMILY, not a 1040
> document**: ReturnTypeCd "4868" (Return4868/ReturnHeader4868/
> ReturnData4868, Extensions family, package 2026v1.0 extracted from
> the already-local zip); NEW builder_4868 + read_model_4868 +
> Mapper4868TY2025 registered (2025, "2026v1.0", "4868");
> schema_locator gains family_version_root (the standalone family
> packages nest 3 levels deep; existing families resolve identically —
> regression-pinned); the header emits NO signature elements without a
> payment record and the full PIN + the 4868's own jurat enum with one;
> ReturnData = IRS4868 (6 optional face elements) + ≤1 IRSPayment +
> ≤4 IRSESPayment (the s76 builders reused verbatim); extract
> bridge-gates on the SAME analyze_4868 derivation (refusals name the
> paper path); live-XSD valid BOTH shapes (no-payment / EFW+2 ES).
> **FAs**: FA-4868-L6 / FA-4868-EFW / FA-4868-CREDIT ACTIVATED in RS
> (reseeded, export verified 410 active), 1040 mirror refreshed
> export-verbatim (409 = 410 − the s71-staged 4835-06; ASCII-encoded —
> the cp1252 loader), `_run_4868_assertion` in BOTH dispatch chains —
> **flow gate 509 → 512**. Suites: NEW test_4868 44 (10 spec oracles ·
> chart partitions + the four-way trap · derived defaults · endpoint
> incl. delete-clears · Sch3-L10 feeder ×3 · diagnostics · field-map/
> PDF agreement · render gates + never-in-packet · builder XML shape
> ×3 · live-XSD ×2 · extract refusals + happy path · mapper registry ·
> FA runner pins) · flow 512 · tts_forms+acroform+returns+payment band
> 391 (trip-wire re-pinned 92) · FULL efile/mef/scenario band 966 ·
> tsc 0 · vitest 300 · live demo probe (TX return: no-pay Austin →
> typed L4 5000 → Charlotte 1302 flip painted live; Sch3 L10 4391 →
> L31 4391 → L33 5000 ORM-verified; 6-PATCH concurrent volley all
> landed; render 200 %PDF 1 page; diagnostics fired exactly
> D_4868_ADDR/CREDIT/EPAY; delete → stale-derive caught live → fixed
> (fresh-fetch + DELETE clear) → re-probed clean; one transient pooler
> connection drop mid-recompute, not code). WO-31 → ✅ DONE in RS.

> **2026-07-15 session 87 — the 1040-V / 1040-ES VOUCHER PAIR (Spine
> S-22b; the third of the six Gate-1-dispatched tts legs) — ★★ UNIT
> COMPLETE: input · compute · render · FAs, all legs green. PRINT-ONLY
> BY DESIGN (no MeF documents — the s76 EFW/ES-debit records are the
> electronic siblings; SUPPRESSION is the interlock).** RS specs `1040V`
> + `1040ES` (WO-30, ONE loader/TWO TaxForms, approved+seeded s83;
> mirrors verified verbatim-current at boot). **INPUT**: NEW
> `PaymentVouchers` singleton (mig 0200 + RLS mig 0201, BOTH DBs — the
> model+RLS pair rule) — V section (pay-by-check flag + check amount,
> blank = the line-37 balance; foreign-routing flag) + ES section (4
> paper-check quarter amounts DISTINCT from es_debit_q1-4 + the RAP
> facts + joint bars + Q4-skip plan flags); PaymentVouchersSerializer
> carries the read-only re-derived `analysis`; `payment-vouchers`
> GET/PATCH/DELETE (1040-only, **PATCH row-locked from birth** — the
> s86 lost-update lesson); Payments-tab card (seq guard; V banner +
> RAP/emission banner; debited quarters disable their check inputs);
> D_V_/D_ES_ → payments NavScope. **COMPUTE**: NEW `compute_vouchers`
> pinned to all 10 RS scenario oracles — R-V-USE (balance>0 AND check
> AND NOT efw_elected; suppression reasons efw/online/no_balance) ·
> the $100M check-split (100M exactly → 2) · R-ES-RAP = min(90%
> (66⅔% farmer) expected, 100%/110% prior — 110% iff prior AGI > $150k
> ($75k next-year MFS) and never for farmers; $150k-exactly stays 100%)
> · the $1,000 general-rule gate + the no-prior-liability exception ·
> per-quarter emission (check amount AND not debited) at the fixed
> FPYMT-088-11 calendar · the joint bars (NRA/decree/different-years/
> RDP) · BOTH mailing charts as full partition-pinned rosters (V:
> 9 Charlotte-1214 states + rest Louisville-931000; ES: 29
> Charlotte-1300 + 22 Louisville-931100; foreign → 1303 on both; the
> GA three-way trap pinned by name). Derived defaults (typed wins):
> V amount ← line 37 · expected withholding ← line 25d · prior tax ←
> THIS return's §6654 tax shown (L22+L23−27/28/29, the compute_2210
> chain) · prior AGI ← line 11 · overpayment-credited reads line 36
> (never enters a voucher box). 15 D_V_*/D_ES_* diagnostics
> code-registered + seeded BOTH DBs. **RENDER**: f1040v.pdf (2025) +
> f1040es.pdf (2026 package, 16 pages) downloaded fresh (manifest 89 →
> 91); f1040v_2025 map (15 widgets label-verified; the IRS skipped
> f1_4 — their gap) + f1040es_2025 map (56 widgets = 4 voucher blocks ×
> 14, generated from the verified block layout: V4 alone on PDF p13,
> V3/V2/V1 top-to-bottom on p14); render_1040v = face page only,
> **v_needed is the render gate** (a suppressed voucher cannot reach
> paper — the bridge-gate convention applied to print); render_1040es
> emits ONLY the sheets carrying an emitted quarter (debit-suppressed;
> joint-barred prints taxpayer-only); packet **"voucher" tier sorts
> LAST** (after state returns — tear-off remittances mail separately);
> standalone render-1040v/render-1040es endpoints (suppressed → the
> explanation, not paper). **MEF**: none by design. **FAs**:
> FA-1040V-EFW / FA-ES-RAP / FA-ES-QDEBIT ACTIVATED in RS (reseeded,
> export verified 407 active), 1040 mirror refreshed export-verbatim
> (406 + the s71-staged 4835-06), `_run_1040ves_assertion` in BOTH
> dispatch chains — **flow gate 506 → 509**. Suites: NEW test_1040v_es
> 41 (10 spec oracles · chart partitions · derived defaults · endpoint
> · 15-rule registry + diagnostics · field-map/PDF agreement ×2 ·
> render gates + page selection · packet tier) · flow 509 ·
> tts_forms+acroform+returns 277 (manifest trip-wire re-pinned 91) ·
> tsc 0 · vitest 300 · live demo probe (card banners: KY V→931000 /
> ES→931100 live; 6-PATCH concurrent volley → all fields landed (the
> row lock) + farmer RAP re-derived 16,667; both render endpoints 200
> %PDF; page-map …, 1040-V, 1040-ES(p.1), 1040-ES(p.2) at the very
> back; diagnostics run fired exactly D_V_ADDR/PREP +
> D_ES_ADDR/FARMER/POSTMARK/REQUIRED — probes torn down). Boundaries →
> DEFERRAL_AUDIT s87 (9). WO-30 → ✅ DONE in RS.

> **2026-07-14 session 86 — FORM 8888 FULL UNIT (Spine S-22b; the second
> of the six Gate-1-dispatched tts legs) — ★★ UNIT COMPLETE: input ·
> compute · render · MeF document · FAs, all legs green.** RS spec `8888`
> (WO-29, approved+seeded s83; mirror refreshed post-reseed). **INPUT**:
> NEW `Form8888` singleton (mig 0198 + RLS mig 0199, BOTH DBs — 0199 also
> back-fills the s85-missed returns_form9465 RLS) — 3 account rows
> (amount/routing/type/number) + the 8379 and legacy-bond flags;
> Form8888Serializer carries the read-only re-derived `analysis`;
> `form-8888` GET/PATCH/DELETE (1040-only, **PATCH row-locked** — the
> probe caught the 8941 lost-update class LIVE on concurrent autosaves;
> form-9465 PATCH gained the same lock); Payments-tab card w/ seq guard +
> computed L5/refund-tie banner + live blocker/row-problem readout;
> D_8888_ → payments NavScope. **COMPUTE**: NEW `compute_8888` pinned to
> the RS scenarios — L5 = the account sum (the F8888-001-04 half of the
> two-way tie holds BY CONSTRUCTION), F8888-002-03 refund tie, RTN
> prefix 01-12/21-32, ≤17-char accounts, unique/non-zero numbers
> (015/016), the amended cap (020), the 8379 bar, whole-dollar-only rows;
> `efile_blockers` reads the Active F8888-* set verbatim from the
> TY2025v5.3 CSV. 12 D_8888_* code-registered + seeded BOTH DBs.
> **RENDER**: f8888.pdf Rev. 12-2025 CONTINUOUS USE (manifest 89,
> downloaded fresh); f8888_2025 AcroForm map — 20 widgets ALL
> label-verified (c1_N pairs: on=1 Checking / on=2 Savings = the XSD
> enum); **face page only** (pages 2-3 are the included instructions);
> line 4 'Reserved for future use' deliberately unmapped
> (R-8888-RETIRED); packet tier 4 (sorts after 8867) + standalone
> render-8888 endpoint. **The 1040 face 35-block closed in passing**:
> 35a "Form 8888 attached" checkbox + the never-mapped 35b/c/d DD
> widgets now fill (blank when 8888 rides — IND-084 print parity).
> **MEF**: NEW IRS8888 document (IRS8888.xsd 2025v5.3, Common family;
> ReturnData1040 directly before IRS8889, max 1); `_extract_f8888`
> bridge-gates on the SAME analyze derivation the card/print use —
> refuses on bond asks (program DISCONTINUED), single-account, row gaps,
> and every Active blocker; `build_irs8888` emits DirectDepositInfoGroup
> ×2-3 + TotalAllocationOfRefundAmt, NEVER RefundByCheckAmt (F8888-023);
> the 1040 emits Form8888Ind "X" w/ referenceDocumentId (IND-091/092)
> and suppresses its 35b-d values (IND-084); the ReturnHeader
> RefundDisbursementGrp fans out per account (maxOccurs=3 = the 8888
> limit; R0000-250/251 per group). **FAs**: FA-8888-TIE/SPLIT/NOBOND
> ACTIVATED in RS (`a3fb215`, reseeded, export verified 404 active),
> 1040 mirror refreshed export-verbatim (403 + the s71-staged 4835-06),
> `_run_8888_assertion` in BOTH dispatch chains — **flow gate 503 →
> 506**. Suites: NEW test_8888 37 (oracles · builder order · live-XSD
> full-return w/ IRS8888 + the Form8888Ind/IND-084/header-fan-out pins ·
> extract gate · endpoint · diagnostics · field-map/PDF) · MeF+payment+
> 9465 126 · FULL efile/mef/scenario band 964 · tts_forms+acroform 201 ·
> returns 76 · tsc 0 · vitest 300 · live demo probe (blocker repaint +
> the concurrent-volley lock verification + print/packet/35a-X landing —
> probe rows torn down). Boundaries → DEFERRAL_AUDIT s86 (7 + 2 riders).

> **2026-07-14 session 85 — FORM 9465 FULL UNIT (Spine S-22b; the first of
> the six Gate-1-dispatched tts legs) — ★★ UNIT COMPLETE: input · compute ·
> render · MeF document · FAs, all legs green.** RS spec `9465` (WO-28,
> approved+seeded s83; mirror verified verbatim-current at boot).
> **INPUT**: NEW `Form9465` singleton (mig 0197, BOTH DBs) — header +
> lines 1b-14 + Part II 15-27 + the router attestations; Form9465Serializer
> carries the read-only re-derived `analysis`; `form-9465` GET/PATCH/DELETE
> (1040-only); self-managing Payments-tab card (seq guard) with the
> computed L7/L9/L10/fee/tier banner + live blocker readout; D_9465_ →
> payments NavScope. **COMPUTE**: NEW `compute_9465` pinned to all 10 RS
> scenario oracles (L10 = whole-dollar CEILING of L9/72 — the Gate-1-
> approved convention; guaranteed/streamlined router; the July-2024
> year-keyed fee ladder incl. low-income DDIA-waived/$43; Part II
> three-condition gate; `efile_blockers` = the published Active F9465-*
> set read verbatim from the TY2025v5.3 CSV — the $50k cap and the
> $25k-50k DD band key on 'TotalTaxDueAmt' = LINE 9, the element the
> rules name). 17 D_9465_* diagnostics code-registered + seeded BOTH DBs.
> **RENDER**: f9465.pdf Rev. 9-2020 (manifest 88, downloaded fresh);
> f9465_2025 AcroForm map — 66 fields ALL label-verified in the widget
> y-bands (16a/16b/19/21/25/26 are multi-kid checkbox choices mapped per
> on-state); packet **FRONT tier** (i9465: attach to the FRONT — sorts
> before the 1040; new "front" tier in _packet_sort_key) + standalone
> render-9465 endpoint. **MEF**: NEW IRS9465 document (IRS9465.xsd
> 2025v5.3, InstallmentAgreement family; ReturnData1040 slot between
> IRS9000 and IRSRRB1042S); `_extract_f9465` bridge-gates on the SAME
> analyze derivation the card/print use, refuses on every blocker with
> the paper path named (433-F/2159), F9465-019-02 ties line 8 to the
> ACTUAL IRSPayment record; builder emits the full XSD sequence
> (CheckboxType "X"/BooleanType true-false; the paper-only indicators
> have NO code path). **FAs**: FA-9465-MIN/EFILE/EFW ACTIVATED in RS
> (`7bc1e79`, reseeded, export verified 401 = 400 + the s71-staged
> 4835-06), 1040 mirror refreshed export-verbatim, `_run_9465_assertion`
> in BOTH dispatch chains — **flow gate 500 → 503**. Suites: NEW
> test_9465 36 (10 oracles · FA pins · builder order · extract gate ·
> endpoint · diagnostics · field-map/PDF agreement · live-XSD full-return
> w/ IRS9465) · MeF+payment band 90 · acroform/manifest 200+162 · tsc 0 ·
> vitest 300 · live demo probe (card render + typed-100 blocker
> round-trip + print value landing 8,400×3/117/300 + packet order
> Letter/Invoice/9465/1040 — probe rows torn down). Boundaries →
> DEFERRAL_AUDIT s85; two RS-amendment nits → REVIEW_QUEUE s85.

> **2026-07-14 session 80 — W-2G MeF DOCUMENT LEG (Spine S-22b) — ★ the
> gambling-winnings W-2G now e-files as IRSW2G (mig 0196); the F1040-034-08
> withholding-reconciliation gap is CLOSED.** The form unit (spec FORM_W2G,
> Ken-approved 2026-06-20 · input · compute → Sch 1 8b + the 25c roster ·
> diagnostics) was complete EXCEPT e-file: line 25c transmitted W-2G box-4
> withholding with NO backing document — F1040-034-08 arm (4) sums all
> Forms W-2G 'FederalIncomeTaxWithheldAmt' into 'WithholdingTaxAmt' (a
> Math-Error reject). This unit adds the missing leg per the s72 recipe:
> **NEW FormW2G e-file identity fields** (payer EIN + US address, the
> 1099-R payer-block naming; boxes 13 state/payer-state-ID + 14 state
> winnings joining the existing 15 — mig 0196, BOTH DBs migrated) ·
> serializer + the Misc-Income card gains payer-identity and boxes-13-15
> rows · **`_extract_w2gs`** (recipient identity = the doc's owner,
> FW2G-010 by construction; CalendarYr = TaxYr, FW2G-011; REFUSALS:
> missing payer EIN/name/US-address · box 4 ≥ box 1 FW2G-001-01 · a
> nonzero box 14 without both box-13 halves FW2G-009-01 · all-zero doc ·
> the stale-8b guard re-running the SAME `aggregate_w2g` derivation
> compute uses, FW2G-008-03) · **`build_irsw2g`** (IRSW2GType sequence;
> the W2GStateLocalTaxGrp state row; ReturnData1040 ref 2308, directly
> after IRSW2 2301). EIN-only payer TIN is rule-driven: FW2G-002 rejects
> PayerSSN for individual e-file. Suites: MeF 1040 pure 79→81 (live-XSD
> full-return w/ two IRSW2G docs incl. the state group) · NEW
> test_efile_w2g_extract 13 · efile band 205 + scenario band 161 · flow
> 500 · tsc 0 · vitest 300 · live demo browser+ORM probe (payer EIN typed
> → autosave → ORM-verified; 8b recompute follows box 1; probe row
> deleted, 8b disengaged clean). Boundaries → DEFERRAL_AUDIT s80 (8 —
> boxes 2/3/5-8/10-12 · local 16-18 · corrected-box · foreign addresses ·
> the in-app-diagnostic mirror candidate). No new ratifications — every
> refusal is a published Active reject rule.

> **2026-07-13 session 76 — EFW PAYMENT RECORDS (Spine S-22b Wave 1, the
> payment cluster's buildable half) — ★ balance-due direct debit +
> scheduled quarterly estimate debits now e-file (mig 0195).** The
> S-17b direct-deposit precedent applied to the debit side — payment
> mechanics, no form face, no RS gate: **IRSPayment** (ReturnData1040
> tail slot 5661; all six elements schema-required — the TaxReturn
> bank_* triplet shared with the refund DD, PaymentAmt = the full line-37
> balance due, the preparer-entered requested date, and the NEW
> `Taxpayer.daytime_phone`) + **IRSESPayment** (5668, max 4 — one record
> per nonzero `es_debit_q1-4` amount at the fixed FPYMT-088-11 due dates
> 4/15 · 6/15 · 9/15 · 1/15). NEW INPUTS (mig 0195, both DBs migrated):
> `efw_elected` / `efw_payment_date` / `es_debit_q1-4` on TaxReturn +
> `daytime_phone` on Taxpayer; serializers + the `/info/` allowlist +
> the Payments-tab EFW card (election checkbox · date · phone · four ES
> amounts, riding the existing debounced bank PATCH). REFUSALS: missing/
> invalid bank triplet · missing date · missing/short phone · requested
> date past the due date (FPYMT-072-01 on-time arm; the received-date
> FPYMT arms = transmit-time, DEFERRAL s76). Gates: MeF 1040 pure 79
> (75 + 4, live-XSD w/ IRSPayment + all four ES quarters) · NEW
> test_efile_payment_extract 9 · FULL efile/mef band 408 · flow 500 ·
> tsc 0 · vitest 300 · info-endpoint suites 86 · live browser probe
> (demo: election/date/phone/Q1 typed → autosave → ORM-verified, probe
> data cleared). **The REST of the cluster is RS-gated (404): 8888 ·
> 9465 · the 1040-V/1040-ES vouchers — draft-to-gate plan filed,
> REVIEW_QUEUE s76.**

> **2026-07-13 session 75 — THE COMPUTE-DONE XML ROW (Spine S-22b Wave 1
> item 4) — ★★ SEVEN compute-done 1040 forms now e-file: 5329 · 8606 ·
> 8880 · 8889 · 8959 · 8960 · 8962 (+ the Form 2210 e-file gate).** Each
> form already had input + compute + print; this unit adds the MeF
> document leg per the s72 recipe — extract bridge-gated on the SAME
> derivation the print uses, builder against the XSD sequence, wired at
> the cited ReturnData1040.xsd slot: **IRS5329** (1298, max 2 — per-owner
> `compute_5329_form_lines` + the R-5329-03 `form_5329_generated_for_owner`
> gate; the line-2 exception emits as the zero-padded ReasonCd + amount
> pair) · **IRS8606** (1609 — per-Form8606 `owner_lines` +
> `form_8606_engaged`; name line + SSN schema-required) · **IRS8880**
> (1923 — FORM_8880 rows; line 9 '0.50' normalized to the XSD enum
> '0.5') · **IRS8889** (1965, max 2 — per-owner `compute_8889.owner_lines`;
> REFUSES >1 account per owner, i8889 = one form per individual) ·
> **IRS8959** (2147 — "8959" rows; the face 5/9/15 thresholds collapse to
> FilingStatusThresholdCd; 9/10/20 = XSD-omitted cross-refs; RRTA rows
> REFUSE — RED-deferred compute) · **IRS8960** (2154 — the render_8960 put
> set value-for-value incl. the `schedule_e_non_1411_income` 4b back-out) ·
> **IRS8962** (2161 — FORM_8962 rows + the 1095-A monthly/annual choice via
> the SAME `_aggregate_1095a`/`monthly_ptc` helpers; QSEHRAInd
> required→false, the default-No class; poverty-table cd A/B/C). **Form
> 2210 deliberately has NO document**: F2210-002-02 requires a Part II box
> on any transmitted 2210 and i2210 says don't file when no box applies —
> the penalty rides the 1040's EsPenaltyAmt; the modeled box C
> (t2210_use_annualized) REFUSES pending a Schedule AI compute leg.
> Suites: MeF 1040 pure 64→75 (live-XSD full-return carrying ALL SEVEN
> documents) · NEW test_efile_computedone_extract 15 · FULL efile/mef band
> 395 (was 369; zero scenario blast radius) · flow 500. No compute/render/
> client code touched — extract + builder + tests only; no migrations.
> Boundaries → DEFERRAL_AUDIT s75 (8); 3 ratifications → REVIEW_QUEUE s75
> (2210 policy · 8962 QSEHRA default-No · 8889 multi-account refusal).

> **2026-07-13 session 74 — FORM 7203 BASIS ATTACHMENT (Spine S-22b Wave 1) —
> ★★ the S-corp K-1 basis-limitation form now PRINTS in the 1040 packet AND
> e-files, and Schedule E 28(e) checks — three surfaces, ONE derivation.**
> The 1040-side `IndividualForm7203` (input + compute existed since the K-1
> flow-through unit; the loss cap already fed `k1_sche_net`) gains its
> missing RENDER + MeF legs: NEW `render_7203_1040` (one f7203 face per
> confirmed row whose attach derivation fires; Part I lines 1-15 now
> computed, Part II TOTAL column only, Part III columns (a)-(e); packet
> tier-4 slot) · NEW **IRS7203** document (1004-slot, between Schedule SE
> and IRS1099R; person-name header choice, nested SCorporationName,
> XSD-omitted 11/12/22/30/32/33 cross-refs respected) · Schedule E
> **28(e) BasisComputationRequiredInd** in print (NEW `28{A-D}_e` checkbox
> map entries, position-verified) and XML. Everything bridge-gates through
> NEW `k1_basis_computation` → `basis_attach_required` (Sch E instructions
> Part II verbatim: loss / distribution / disposal / repayment — the first
> two derivable, the last two stated boundaries). COMPUTE FIX (ratification
> queued): the outside stock-basis adjustment folds into Part I line 1
> (§1368 ordering — the old post-line-10 fold overstated basis when
> distributions exceeded line 5; divergence pin in the new suite). NEW
> REFUSAL: S-corp K-1 loss/deduction items without a confirmed 7203 refuse
> at extract (refusal beats fabrication; zero scenario blast radius — no
> ATS 1040 scenario carries recipient K-1s). Suites: MeF 1040 pure 62→64
> (live-XSD full-return w/ IRS7203) · NEW test_efile_7203_extract 7 ·
> render leg 12→14 · 7203 compute suites 32 (all pre-fix pins survived) ·
> flow 500 · FULL efile/mef band 369 · Sch E/K-1/8582/7203 band 274.
> Boundaries → DEFERRAL_AUDIT s74 (7); 3 ratifications → REVIEW_QUEUE s74
> (line-1 fold · the new refusal policy · the partnership-28(e) boundary).

> **2026-07-13 session 73, second act — FORM 8949 DETAIL + SCHEDULE D FULL
> PATH (Spine S-22b Wave 1 item 2, `3b90188`) — ★★ per-lot capital
> transactions now e-file.** NEW **IRS8949** document (2133-slot): one
> document holding every box group (ShortTerm A/B/C/G/H/I ·
> LongTerm D/E/F/J/K/L, the six-flavor checkbox choice per group); rows
> mirror render_8949_1040 (CapitalTransaction box/order + the
> form_7217_8949_rows §731(a)(1) virtual rows; dates as elements or the
> VARIOUS/INHERITED/INH-2010 codes; Exception-2 code-M summary rows keep
> both date columns blank); per-box line-2 totals recombine through the
> SAME aggregate_box_totals + 7217 merge the compute ran — group totals ≡
> the Sch D box columns by construction. **IRS1040ScheduleD** gains the
> 1b/2/3/8b/9/10 CapitalGainAndLossType groups (d/e/h always, g
> nonzero-only) + root referenceDocumentId → IRS8949; the aggregate-only
> refusal is RETIRED, replaced by a stale-box-totals consistency guard.
> Refusals: unparseable date columns · negative basis (NN type) ·
> adjustment codes beyond [A-Z]{0,7}. Suites: MeF 1040 pure 59→62 (live-XSD
> validation of the detail return) · NEW test_efile_8949_schd_extract 7 ·
> schedule_d/8949 band 89 · flow 500 · full efile/mef band 360. Boundaries
> → DEFERRAL_AUDIT s73b (5: word-code columns unmodeled · code-M summaries
> e-file without the broker statement, the 8453 workflow = Wave 4 · 1(a)
> EIN choice + statement refdocs unemitted · 100-char descriptions ·
> the stale guard).

> **2026-07-13 session 73 — SCHEDULE E PARTS I-III + FORM 8582 MeF DOCUMENTS
> (Spine S-22b Wave 1, `a2cbab0`) — ★★ the 1040 rentals e-file gap CLOSED.**
> Serialization-only (no model changes; the one compute touch = the 23c/23d
> zero→sum fix, REVIEW_QUEUE s73): **IRS1040ScheduleE** now emits the FULL
> schedule — Part I PropertyRealEstAndRoyaltyGroup per RentalProperty
> (PropertyDesc enum from the type code, partial OtherUSAddress, expense
> lines 5-18 in schema order, line-19 Desc+Amt detail rows, the aggregate
> line-22 magnitude on the FIRST group = the print's column-A parity),
> Part II/III groups via the SAME `schedule_e_p2_rows` face mirror the print
> renders (bridge-gate; 28(g)/33(c) carry referenceDocumentId=IRS8582),
> totals from the computed rows with 25/31/36 as Pos-type magnitudes;
> Part IV REMIC refuses (no model). **IRS8582** (1581-slot) NEW: Part I-III
> face lines from the FORM_8582 rows (attach gate = rows exist) + Parts
> IV-VIII worksheets recombined through the SAME pure
> `per_activity_allocation` the print uses, including the print's line-C≤0
> gate on Parts VI-VIII; the XSD's stale Part VII(c)/VIII(b) element names
> map by face position. Suites: MeF 1040 pure 56→59 (live-XSD 2025v5.3
> validation of the full Sch E + 8582 return) · NEW
> test_efile_sche_8582_extract 7 · schedule_e/8582 band 127 · flow 500.
> Fix-forward: the s72 8867 refusal had silently broken Scenario 2's IFA
> artifact test (paid-preparer ODC, checklist unanswered) — the scenario now
> attests due diligence, IRS8867 joins its expected document sequence
> (scenario2 suite 29 green). Boundaries → DEFERRAL_AUDIT s73 (8); line-27 +
> print-overflow + 23c/23d nits → REVIEW_QUEUE s73.

> **2026-07-12 session 70 — FORM 3115 PRINT UNIT (Spine S-20d) — ★★ UNIT
> COMPLETE (the tts leg; RS spec WO-23 Gate-1-approved + seeded 2026-07-06;
> mirror cached fresh from the deployed export at kickoff — the s64 rule).**
> Print-only — NO MeF leg by design (the automatic-change original attaches
> to the paper return package + duplicate copy to Ogden; non-automatic =
> National Office + fee) and no tax-line feeds; CRUD skips the recompute
> chokepoint (the s69 print-only recipe, third clone). **INPUT**: Form3115
> (OneToOne singleton per the 19 spec facts + the Gate-1 Q2 direct-entry
> Sch E 7a-7g present/proposed descriptors + Sch A L1 method boxes/2g
> description as print-only fields) — migs 0191 + 0192 (RLS default-deny)
> + 0193; Form3115Serializer (BaseModelSerializer) + the form-3115
> PATCH-creates-lazily singleton endpoint returning rows + the re-derived
> `analysis` (no stored computed columns). **COMPUTE (helpers)**:
> compute_3115 — the DCN-7 depreciation catch-up (taken − allowable at BOY,
> Rev. Proc. 2025-23 §6.01(5)), the Schedule A 2a-2h cash↔accrual netting,
> the §7.03 adjustment-period router (1 neg / 4 pos / de minimis 1 /
> under-exam 2, de-minimis precedence), ratable installments, DCN routing;
> derived-vs-typed: blank dcn/L26 = YELLOW derives, typed wins GREEN;
> cut-off suppresses the whole §481(a) block. **RENDER**: f3115.pdf Rev.
> 12-2022 downloaded fresh + manifest-registered (87, trip-wire bumped) +
> f3115_2025 AcroForm map (294 fields; Yes/No checkbox PAIRS kid[0]=Yes/
> kid[1]=No; c4_3 on-states are the odd '90'/'120'; page-1 header grid +
> applicant-type + change-type boxes + DCN(1) + L6a/L11a/L13 + Part IV
> L25-L29 + Schedule A + the five Sch E face questions) + render_3115
> (1040 taxpayer / entity header routing; **the Rev. 12-2022 face has NO
> fill fields for Sch E L4a/L7 — they print as Statement pages, which also
> satisfies L26's attach-a-computation-summary**; the spread schedule on
> the statement; overall_method checks the page-1 Other box + specify text
> since the face has no overall-method box); standalone render-3115
> endpoint; joins ANY built module's packet when attach_to_return
> (defaults TRUE — the automatic-change original belongs in the filing
> package, unlike the 2553 default). **DIAGNOSTICS**: rules_3115 (8)
> code-registered verbatim + prod seed_rules (D_3115_* 8 live); the
> §481(a)-sign conditions read the EFFECTIVE net (the RS scenarios pin the
> derived path). **UI**: Form3115Section self-managing card + monotonic
> seq guard; mounts on the entity "Elections & POA (2553/2848/3115)" tab
> (both entities) + the NEW 1040 "Method Change (3115)" tab; D_3115_
> nav-mapped in all three scopes. **FAs**: FA-3115-CATCHUP/SPREAD/SCHA
> ACTIVATED on RS prod (status-only reseed) with _run_3115_assertion in
> BOTH dispatch chains (the s69 reconciliation lesson) + all three gate
> mirrors refreshed from the deployed export (1120S 41 verbatim / 1065
> 39 = 43−4 staged / 1040 +3 additive → 382) — **flow gate 475→484**.
> **GATES**: flow 484 · NEW test_3115 39 (the 7 spec oracles pinned) ·
> manifest/acroform 201 · pair 36 · combined sweep 760 · tsc 0 · vitest
> 300 · live ORM probe 14/14 (3115-A end-to-end, diagnostics on live
> rules, packet attach/detach, typed-wins, cut-off; cascade-deleted) ·
> live browser probe (card mounts; real-key 8,000/72,000 → server-painted
> "−64,000 · 1-year period" + derived DCN 7; console clean). Boundaries →
> DEFERRAL_AUDIT s70 (7). OMB-citation nit → REVIEW_QUEUE s70.

> **2026-07-12 session 69 — FORM 2553 + FORM 2848 PRINT-UNIT PAIR (Spine
> S-20b/c) — ★★ BOTH UNITS COMPLETE (the tts legs; RS specs Gate-1-approved
> + seeded s68).** Print-only forms — NO MeF legs by design (2553 =
> paper/fax to KC/Ogden; 2848 = online/fax/mail to the CAF) and no tax-line
> feeds, so CRUD deliberately skips the recompute chokepoint (the
> bulk-assign precedent). **INPUT**: Form2553 (OneToOne + Form2553Consent
> J-N rows + Form2553Qsst Part-III rows) and Form2848 (OneToOne +
> Form2848Representative ×4 + Form2848Matter ×3) — migs 0189 + 0190 (RLS
> default-deny ×6) + firms 0005 (**Preparer gains caf_number + fax**;
> Preparer Manager UI + serializer expose them); all serializers
> BaseModelSerializer; singleton PATCH-creates-lazily endpoints return the
> full serialization + the re-derived `analysis` dict (no stored computed
> columns — the 8283 convention). **COMPUTE (helpers)**: compute_2553 —
> the §1362(b) 2mo15d corresponding-day deadline (the three published
> i2553 examples + Dec-31 + leap-year pinned), timeliness incl.
> invalid-early/preceding-year, min(raw,agg) shareholder gate + item-G,
> the Rev. Proc. 2013-30 relief router (corp/6a-c-alt/entity/PLR $14,500),
> consent scope, part2_basis derived from the face boxes; compute_2848 —
> the receipt-year+3 CAF future-clock, the 45/60-day countersign window,
> modified-CAF (the 08-Jul-2026 Rec. Dev.), filing route, the URP
> four-condition gate, the CAF 9-digit-or-'None' shape. **RENDER**: both
> templates manifest-registered (85/86, hash-verified vs irs.gov — the s61
> unregistered-template class again) + AcroForm field maps (f2553 100
> fields incl. the 7-row consent grid + Part II/III; f2848 92 human-named
> fields incl. the Table_Line3/Table_PartII nesting) + render_2553
> (overflow page-2/page-4 copies; the 2013-30 margin legend as an abs_pos
> literal; wet-ink seams blank) + render_2848 (SSN/EIN TIN-box routing;
> the §1.6012-1(a)(5) prescribed statement at 7pt; e-sign/wet-ink seams);
> standalone render-2553/render-2848 endpoints; 2553 joins the 1120-S
> packet ONLY when attach_to_return (late-election-with-return); 2848 is
> NEVER packeted. **THE APPROVED VALUE-ADD**: form-2848-autofill-rep fills
> an L2 block from the Preparer record (name/firm address/CAF-or-'None'/
> PTIN/phone/fax) marked YELLOW; any manual PATCH clears to GREEN.
> **DIAGNOSTICS**: rules_2553 (19) + rules_2848 (17) code-registered
> verbatim from the specs + prod seed_rules (D_2553_* 19 / D_2848_* 17
> live). **UI**: entity "Elections & POA (2553/2848)" tab (1120-S both
> cards / 1065 2848-only) + the 1040 "Power of Atty (2848)" tab; cards
> self-manage from the singleton endpoints under a monotonic seq guard
> (an out-of-order paint was caught LIVE in the browser probe and fixed);
> D_2553_/D_2848_ nav-mapped in all three scopes. **FAs**: FA-2553-WINDOW/
> COUNT/8832 + FA-2848-FUTURE/SIGN45/CAFFILL ACTIVATED on RS prod with
> runners (_run_2553_assertion/_run_2848_assertion) + all three gate
> mirrors refreshed in one motion (1120S 38 verbatim / 1065 36 = 40−4
> staged / 1040 +3 additive) — **flow gate 463→475**. **GATES**: flow 475 ·
> NEW test_2553_2848_pair 36 · manifest/acroform/packet 206 · tsc 0 ·
> vitest 300 · live ORM probe 21/21 (endpoints, window math, autofill,
> diagnostics, the 1040 refuse; cascade-deleted) · live browser probe
> (cards mount on the entity tab; item-E date → server-painted deadline
> 2026-03-15 → LATE + relief + legend under concurrent saves; autofill
> painted the 9-digit CAF YELLOW). Boundaries → DEFERRAL_AUDIT s69 (7).

> **2026-07-27 session 121 — FORM 8283 1040 RECONCILIATION ARM (QA Batch-001
> item 16) — ★ SHIPPED (app `3ed3c76`, RS `0c8fc7f`, no migration; deploy
> VERIFIED live, `index-BhoKt46x.js`, 3 markers vs 0-hit baseline;
> `seed_rules` BOTH DBs POST-deploy, D_8283 family 17/17).** Audit-first: item
> 16 was ~80% already built (model/compute/render/MeF/16 diagnostics/client
> grid, from the s57 1040 leg + the s65 entity amendment above). **COMPUTE**:
> unchanged — no amount moves. **DIAGNOSTICS**: NEW `D_8283_017`
> (RS `R-8283-RECON`, J8) closes the SILENT-override hole — a flat Schedule A
> line-12 entry beats the 8283 row total by design (R-8283-SCHA12), but
> nothing compared the two, and adding one partial row SILENCED `D_8283_001`
> (which needs zero rows); effect-scaled error/warning with a
> withheld-conservation false-positive guard. `D_8283_005/007` now name the
> SPECIFIC missing facts (cols (e)/(f)/(g), row facts, zero amounts,
> out-of-year dates), not just the offending item. **INPUT/UI**: Schedule A
> line-12 >$500 notice mirroring the server ladder + jump-and-highlight into
> the item grid; `D_8283_` added to the 1040 `RULE_TAB_MAP` (the family had
> NO 1040 route — no tab dot, nothing to click, while both entity scopes had
> one). **RENDER**: unchanged, but Form 8283 REGISTERED in the s112
> generated-form manifest (was absent). **Gates**: NEW test_8283_item16_recon
> 13 · 8283 band 46 · Sch A legs 40 · manifest 10 · flow 521 · NEW
> form8283Item16.test.tsx 14 · vitest 525 · tsc 52 baseline · live demo probe
> green (real runner fired D_8283_017; demo restored). QA acceptance pinned
> verbatim ($1,100 aggregate → blocks + lists facts → completes → clears with
> itemized deductions unchanged). Item 16's conversion/source-summary bullet
> DEFERRED (rides item 15). Boundaries → DEFERRAL_AUDIT s121 (6).

> **2026-07-12 sessions 65-66 — ENTITY FORM 8283 (Spine S-20a) — ★★ UNIT
> COMPLETE, BOTH LEGS — SPEC-FIRST RS round-trip (RS `8b6faca`).** The shared
> 8283 spec's PTE stated-boundary (D_8283_010) closed with an additive
> entity-arm amendment, i8283 Rev. 12-2025 verbatim (R-8283-ENTFILE/ENTSECB/
> ENTFEED/ENTCOPY; **the $5,000 Section-B test reads the ENTITY item amount,
> never per-member** — T15 pins the division-first regression; scenarios
> T14-T16; D_8283_014/015/016). **INPUT**: the NoncashContribution worksheet
> (shared model/compute — return-generic since the 1040 unit) mounts on the
> entity Schedule K tab (both entities; entity-aware feed blurb/hint); CRUD
> already rides the form-agnostic recompute chokepoint. **COMPUTE**: 1120-S
> K12b defaults from the non-withheld rows total (`_derive_schk_inputs_db` —
> the override-respecting B11/K16e pre-pass; typed K12b GREEN wins; stale
> derives self-clear; conservation withholds never leak); **1065 = NO FEED**
> (single combined 13a face line — D_8283_016 coverage warning instead,
> J-E2). **RENDER**: render_8283 opened to 1120-S/1065 (entity legal name +
> EIN header); joins the ENTITY packet block (engagement > $500 self-gated).
> **MeF**: IRS8283 = a DECLARED ReturnData1120S 2025v6.2 document (ref 1228,
> 4797 < 8283 < 8824) — the SHARED build_irs8283 (corporate DonatedProperty-
> Type ≡ IMF's) + K12b refDocId link; Section B rows REFUSE (the J7 wet-ink
> seam); 1065 MeF rides the future mapper. **DIAGNOSTICS**: the row-level
> substantiation checks (D_8283_002-013) now sweep entity rows via
> `_row_returns`; D_SCHK_8283 RETIRED (is_active=False, seeded) → D_8283_014
> error; D_8283_015 copy-to-members info; prod seed_rules run. **FAs**:
> FA-ENT-8283-01/02 staged DRAFT at the RS leg, then ACTIVATED with runners
> (`_run_ent8283_assertion`) + both export-verbatim mirrors refreshed in one
> motion — flow gate 460→463. **GATES**: flow 463 · 8283+schk+pins+diag batch
> 570 · MeF 1120-S + S5/S6 83 · tsc 0 · vitest 300 · NEW test_8283_entity 11
> (T14/T15/T16 oracles) · live ORM probe 15/15 (feed/override/re-derive/
> print/packet/MeF/diagnostics, both entities, cascade-deleted) · live
> browser probe (worksheet on the entity Schedule K tab; typed 3,000 →
> autosave → **K12b server-painted 3,000 YELLOW**). Boundaries →
> DEFERRAL_AUDIT s66 (5 items); J-E1/E2/E3 ratifications queued (built to
> the recommendations).

> **2026-07-12 session 63 — SCH_K INPUT SUB-UNIT (the s56 tail items 4-7) —
> COMPLETE (mig 0188).** The 1120-S Schedule K line-12 family re-keyed to the 2025
> face IN PLACE (K12d→K12e, K12c→K12d, K12b→K12c; FFVs rode the FK — prod had 8
> all-blank rows per renamed line, zero risk; MappingRule.target_line strings
> re-pointed; py_manual/AsFiledBaseline JSON re-keyed — the mig 0187 recipe).
> **(4) Charitable split**: K12a = cash / NEW K12b = noncash (RS SCH_K_1120S R006);
> prints on face 12a/12b (f3_20/f3_21, widget-verified); MeF Cash/Noncash-
> CharitableContriAmt; K-1 box 12 codes A (cash 60%) / C (noncash 50% default) —
> NEW D_SCHK_CHARCODE warning mirrors RS K1 D006 ("verify category; 30% cash = B,
> noncash runs C-G"), NEW D_SCHK_8283 warning (noncash > $500 needs Form 8283,
> not built — S-20a); M1_6b gains K12b; TB "Donations/Charitable" rollup stays
> cash→K12a (noted). **(5) K16e/K16f inputs**: 16e DERIVES bottom-up from the
> Form 7203 ShareholderLoan rows' loan_repayments (override-respecting B11-style
> write, NOT a registry formula — the seeder's newly-computed override-clear
> would stomp typed values; stale derives self-clear when the last loan goes);
> K-1 box 16 code E is PER-RECIPIENT (each owner's own repayments; pro-rata
> fallback when no loans tracked — Σ K-1 == Schedule K either way); 16f enters
> K18 (RS R019 face verbatim: "subtract... 11 through 12e and 16f") and M2_5a
> (i1120s p.50 AAA adjustment 2 verbatim); K-1 code F pro-rata; MeF
> ShareholderLoanRepaymentAmt / TotalForeignTaxesPaidOrAccrAmt; loan CRUD now
> recomputes (chokepoint rule). **(6) K14a/14b**: the pre-K-2-era "Name of
> Country" TEXT row converted to the face's K-2/K-3 CHECKBOXES (booleans; prod
> values were all blank — mig clears defensively); print map c3_7[2]/c3_8[0]
> widget-verified; MeF emits DomesticFilingExceptionInd, and a checked 14a
> REFUSES (K-2 not built — reconcile-or-refuse); D_EB_K2K3/D_EB_DFE_OK extended
> to the 1120-S (indicator: K16f > 0 or 14a checked). **(7) 17c → M-2**: M2_7d
> (the AE&P column — internal letters are legacy: b=OAA/c=STPI/d=AE&P vs the
> face's b=PTEP/c=AE&P/d=OAA, noted for the future M-2 leg) derives from K17c
> (i1120s M-2 col (c) / RS M2 R003/R006), M2_8d follows; NEW D_SCHK_17C_DIV info
> (1099-DIV channel; generation rides sherpa-1099, out of scope). Gates: flow
> 447 · pins (12b/16e/16f y-bands + box 12/16 code tables + K14 checkbox
> targets) · MeF 1120-S suite + S5/S6 byte-stable (scenario-6 fixture re-keyed
> K12d→K12e, same XML) · test_returns 359-line pins · tsc 0 · vitest 300 ·
> **shared DB: mig 0188 APPLIED + seed rerun (359 lines) + seed_rules; live ORM
> probe 14/14 PASS (K18/M2_5a/M2_7d/M2_8d/K16e + per-recipient K-1 + face print
> + diagnostics, cascade-deleted); live BROWSER probe (all 8 new rows render,
> K12b typed → K18/M2_5a server-painted, 17c typed → M-2 grid AE&P col paints
> 12,000, K14a renders as boolean chips)**. NEW `test_schk_input_subunit.py`
> (13) + `rules_1120s_schk.py` (3 code-registered rules, prod-seeded).

> **2026-07-12 session 62 — 1120S_M3 LINE_MAP RENUMBER (audit unit #7), RS LEG
> (+ tts template registration) — UNIT COMPLETE; the audit queue's last standalone
> item (3800 rides the future GBC entity unit).** The M-3 face finally entered the
> repo: **f1120ss3.pdf Rev. December 2019** (the current revision; ⚠ filename trap —
> irs.gov's `f1120sm3.pdf` is the C-corp **1120** M-3, "s" = "schedule") + i1120ss3
> (Rev. 12-2019), both fetched + hash-registered (manifest 84, trip-wire bumped).
> The old RS P1-*/P2-*/P3-* map proved FABRICATED against it (Part III ends at 32 —
> the spec had 33-36; Part I line 1 = the 1a/1b statement questions, not net income;
> P1-FS/P1-RS/P2-DEP/P3-DEP exist on no revision). Rebuilt verbatim: 30 face-keyed
> facts (1a-12d incl. the 4b GAAP/IFRS/tax-basis/other choice + the 12a-d
> asset/liability grid) · R001-R005 · 87 face lines (I-1a..I-12d · II-1..26 incl.
> 21a-g · III-1..32 incl. 23a/b + the Reserved 22) · D001-D007 · 5 scenarios (incl.
> the published Example 1 "$12M FS / $8M Schedule L = not required" pin + P1 L11
> combine + the P3 L32 sign-flip). **NEW tax-law content: R005 — the $50M
> complete-ENTIRELY tier vs the through-Part-I + M-1 option (M-1 L1 must equal P1
> L11) — the tier the pre-s44 "$50M" error had CONFLATED with the $10M filing gate**
> (which reads SCHEDULE L assets, verbatim). The tie chain specced: P1 L11 = P2
> L26(a) (or M-1 L1); **P2 L26(d) = Schedule K line 18** (the same K18 anchor as
> M-1 L8). Excerpts: 5 verbatim (i1120ss3 + face notes; the IRS's own "(Form 1065)"
> typo flagged, not propagated); SCHB's stale "$50M" M-3 link note healed via
> refresh-delete. Harness 165/0 (twice-run pre-polluted); prod stale-deletes exact
> (13 facts + all 20 fabricated lines); idempotent rerun clean; deployed export
> verified (87/5/30/7/5); NEW tts mirror `server/specs/1120s_m3_spec.json`.
> **tts = boundary-RED by design** (no M-3 build leg season one; the three $10M
> threshold constants verified correct, reading Sch L 15d; the "not supported —
> prepare manually" routing unaffected). Gates: manifest 3/3 (84) · flow 447.

> **2026-07-12 session 61 — FORM 6198 2025-FACE RENUMBER (audit unit #6), RS LEG
> (+ tts manifest registration) — UNIT COMPLETE.** The RS 6198 block's line_map was
> FABRICATED (an invented "prior year unallowed losses" line 2, deductible loss on
> 20 vs the face's 21, 13 of 21 face lines missing — matched no published revision).
> Rebuilt verbatim vs f6198.pdf **Rev. November 2025** (a NEW revision, Created
> 9/9/25) + i6198 (Rev. 11-2025, fetched): 21 face-keyed facts (incl. the three
> 15/16/18 a/b checkbox choice facts) · R001-R009 (§465 substance KEPT, re-keyed:
> Part I combine 1-4 · Part II 6-10b · Part III 11-19b w/ the 15b prior-year-19b-
> never-10b caution · L20 larger-of · L21 smaller-of w/ pro-rata carryover · QNF ·
> §465(e) recapture · basis→at-risk→passive ordering · prior-year nondeductible
> amounts ride Part I) · 25 face lines · D001-D006 · 7 scenarios incl. the THREE
> published i6198 L21 examples + the p.3 L5 income-offset example. The paraphrase
> "At-risk computation" excerpt replaced VERBATIM under the same label (fabricated-
> excerpt class) + 5 new verbatim excerpts, mirrored in forms_supporting.py.
> In-loader stale self-heal (9 facts / lines 2+10 / 2 scenarios deleted on prod,
> exact); RuleAuthorityLink refresh-delete (s59 rule). SQLite harness 144/0
> (twice-run, pre-polluted); prod seeded + idempotent rerun clean; deployed export
> verified (25 lines / 9 rules / 21 facts / 6 diag / 7 tests); NEW tts mirror
> `server/specs/form_6198_spec.json`. **tts side: NO drift possible — no 6198
> render/compute/MeF leg exists** (field map is an unmapped stub; 4835's at-risk
> cap matches R005 substance). Housekeeping: f6198.pdf was an UNREGISTERED template
> — hash-verified vs a fresh irs.gov download (SHA match) and registered in
> forms_manifest.json (83 forms; trip-wire bumped). Gates: manifest tests 3/3 ·
> flow 447. The 6198 tts build unit (input/compute/render legs) remains future
> work — the spec is now face-true for it.

> **2026-07-12 session 60 — 1120-S PAGE-1 FFV SEMANTIC RE-KEY (renumber unit #5 tts
> leg, `e4c4ac8` / mig 0187) — UNIT #5 COMPLETE.** Internal keys now equal the 2025
> face (19=7205 NEW input · 20 other · 21 total · 22 OBI · 23a-c tax · 24a-d/z
> payments w/ NEW 24d EPE · 25 penalty · 26/27 owed/overpaid · 28a + NEW computed
> 28b refunded). Mig 0187 renamed FormLine keys in place on the shared DB (278
> FFVs/key verified carried; seed rerun 355 lines, zero stale deletes); py_manual/
> baseline JSON keys re-keyed; imported PY line_values needed no migration (the
> 2024 print already used this numbering — the re-key fixed the live CY-vs-PY
> compare misalignment). **LIVE FIX: owed/overpayment formulas gained the line-25
> penalty term (RS R014).** Print shim retired (map = face 1:1; f1_37/f1_47/f1_53
> widget-verified); MeF + GA-600S pull + 8879-S/8453-S + diagnostics + client lists
> re-keyed; K3c duplicate-widget alias removed. s56-tail 1-3 delivered (7205 input ·
> 24d · 28b); 4-7 = the SCH_K input sub-unit (DEFERRAL_AUDIT s60). Gates: flow 447 ·
> consolidated 583 · S5+S6 8/8 · affected 208+106+36 · tsc 0 · vitest 300 · live ORM
> probe PASS.

> **2026-07-12 session 59 — 1120-S PAGE 1 + M-1 + M-2 2025-FACE RENUMBER (audit unit
> #5), RS LEG ONLY (RS `bb42381`; tts mirrors refreshed — NO tts code; the tts FFV
> re-key leg is the next unit).** PAGE1: rebuilt verbatim vs f1120s.pdf 2025 p.1 —
> NEW line 19 = Form 7205 §179D deduction (the insertion that shifted the old
> 19/20/21 to 20/21/22) + the Tax and Payments block 23a-28e the spec NEVER carried
> (23a ENPI/LIFO · 23b Sch D BIG · 23c sum+4255 · 24a-d/z payments incl. 3800 EPE ·
> 25 penalty/2220 · 26 owed · 27 overpaid · 28a/28b split + DD 28c/d/e); 47 facts /
> 15 rules / 40 lines / 6 diagnostics / 12 scenarios. M-1: the old block was a
> WHOLE-FORM FABRICATION — its "3a guaranteed payments §707(c)" is a 1065 M-1 line;
> real 2025 face: 3a Depreciation / 3b Travel & entertainment itemize line 3,
> 4 = 1+2+3, 5/5a, 6/6a, 7 = 5+6, and line 8 = "Income (loss) (Schedule K, line 18)"
> — never page-1 OBI; guaranteed-payments fact/rule/diagnostic + line 5b deleted on
> prod; R007 applicability (B-Q11 skip / $10M M-3 / 2025 partial option, p.49
> verbatim) + R008 3b composition added. M-2: rows 2/4 → page-1 LINE 22; PTEP (col b)
> + AE&P (col c) columns added (§1375(d) / §1371(c),(d)(3); AE&P dividends = the
> K17c 1099-DIV tie); **R002 AAA distribution cap corrected to the §1368(e)(1)(C)
> without-net-negative-adjustment base** — published i1120s pp.50-51 worksheet
> example + a divergence pin are scenarios (Ken ratification → REVIEW_QUEUE s59).
> 8 fabricated/composite excerpts deleted, 19 verbatim excerpts added (pymupdf).
> SQLite harness 319/0 w/ twice-run self-heal proof; prod seeded + ORM-verified;
> deployed exports verified; tts mirrors (1120s_page1/m1/m2_spec.json) refreshed.
> tts gates on the new mirrors: flow 447 · test_1120s_spec 45 · face pins 18 — green.

> **2026-07-11 session 58 — SCHEDULE L (1120-S) 2025-FACE RENUMBER (audit unit #4),
> spec-first (RS `bfcb95a`; tts pins only — no app fix needed).** The RS 1120S_SCHL
> block ran TWO fabricated numbering systems (facts: total assets at l14, liabilities
> 15-21, l6="other investments"/l7="buildings" vs the face's other-current-assets/
> loans-to-shareholders; line map: a phantom "L22 Total liabilities" shifting equity
> +1 into an invented L28) and its source "excerpts" were fabricated paraphrases
> carrying the same wrong numbering. Rebuilt verbatim vs f1120s.pdf 2025 p.4 +
> i1120s p.49: 65 facts (31 face rows × BOY/EOY + 3 cross-checks) / 8 rules (R001
> total-assets sum now includes lines 4/6/7/8 + the 2/10/11/13 contra pairs; the
> fabricated R002 total-liabilities rule DELETED; R004 = L15==L27; R007 tied to the
> Sch B Q11 verbatim gate; NEW R009 = L15 col (d) → page 1 item F, p.49 verbatim;
> R005/R006/R008 substance kept) / 31 face lines / 7 diagnostics (NEW D007 item-F
> mismatch) / 4 scenarios (NEW contra-pair-netting pin); fabricated excerpts
> replaced with real p.49 text (labels kept — no orphans); stale self-heal deleted
> 26 orphan facts + R002 + L11/L28 on prod. **tts verified FACE-CORRECT end-to-end**
> (seed keys L1a-L27d with a/b/d/e columns; compute L15a/d + L27a/d match R001/R003
> exactly; L24d ← M-2 (R005); L3a BOY populate + 1125-A default (R006/R008); the
> $250K check reads L15d (R007); MeF SCHL_LINE_ORDER carries the full face incl.
> derived contra nets; item F ← L15d in BOTH print header and MeF TotalAssetsAmt
> (R009)) — the drift was RS-only; no app change. NEW `TestScheduleLRowPins` (2
> y-band pins: asset + liability/equity zones incl. the equity-block placement the
> spec had shifted). Gates: pins 18 + flow 447 (465 together) · test_schl_boy_
> inventory 4/4. Export verified on deployed RS; tts mirror refreshed.

> **2026-07-11 session 57 — SCHEDULE K-1 (1120-S) 2025-FACE RENUMBER (audit unit #3),
> spec-first (RS `a0e908c` / tts `69e7e08`).** RS K1_1120S rebuilt verbatim vs
> f1120ssk.pdf 2025 + the i1120s 2025 pp.30-48 code tables: 44 facts (Part I item D +
> Part II E-I incl. F2 responsible party/F3 entity type/item I loans) / 17 rules (NEW
> R012-R016 + R021 code-assignment rules; R017 code-V mechanics; R-K1-ROUND
> parenthetical corrected) / 33 lines (boxes 1-19 — 14 K-3 checkbox, 15 AMT A-F, 18/19
> multi-activity checkboxes — + ItemA-I; full code tables in notes) / 6 diagnostics
> (D004 pre-2023-alphabet error · D005 health-insurance-never-AC · D006 charitable-
> category warning) / 7 scenarios (code-letter pins) / 7 verbatim i1120s excerpts;
> stale fact/line/scenario self-heal guards. **The s37 "codes mirror print tables"
> belief was FALSE — four live tts print/MeF code zones fixed:** box 13 followed the
> PRE-2023 alphabet (LIH §42(j)(5) printed "A" = zero-emission nuclear on the 2025
> table; now 13a→C, 13b→D, 13c→E, 13d→F, 13f→I); K12c §59(e)(2) printed I (= royalty
> deductions; now J); K12d other deductions printed L (= portfolio-other; now ZZ +
> typed statement); >2%-shareholder health insurance printed/emitted code AC (= §448(c)
> gross receipts; i1120s p.17 makes W-2 box 14 the channel — now ZZ + typed statement,
> key K17_AC→K17_HEALTH, Ken ratification in REVIEW_QUEUE); plus **K13e (other rental
> credits) never allocated** (issuer gap — the MeF builder referenced it but box_shares
> never carried it; now allocates, code G both sides). 8941 = BA unchanged-correct.
> Pins: `TestK1CodeLetters2025` (5 — print/MeF table equality + K13e + never-AC).
> Gates: pins 49 · MeF+tts_forms 234 · flow 447 · S5+S6 8/8. Deferrals →
> DEFERRAL_AUDIT s57 (charitable A/B-C-G split · ZZ typed breakdowns · box-10 print
> code · K16e/16f allocation rides the SCH_K input unit · F2/F3/item-I inputs ·
> item-D print blank). Export verified on deployed RS; tts mirror refreshed.

> **2026-07-10 session 48 — Form 8825 REV-12-2025 FACE UNIT + Schedule A (Form 8825),
> spec-first (RS `c4c94bc` / tts `a4435f4`; batch-2 items B2-7a/B2-8/B2-9).** The RS 8825
> spec was ANOTHER early-era drifted block (numbering matched NO published revision) —
> renumbered verbatim to the Dec-2025 face: income split 2a/2b (2b NEW) → 2c; expenses
> 3-17 with 15/16 reserved; **line 17 = the NEW Schedule A (Form 8825) fixed-category
> other-deductions schedule** (A1-A20 + A30 Other; REQUIRED for Schedule M-3 filers per
> i8825 verbatim); 18 = sum(3-17); 19 = 2c − 18; 23 = COMBINE 20a-22a → K2 (the old
> R003 omitted 21/22a). tts (migs 0184+0185 RLS): `other_rental_income` (2b) ·
> `wages_salaries` (13) · `other_info_code` (col (c) A-I, NEW 12-2025 acquisition/
> disposition codes, M-3-only) · NEW `RentalPropertyOtherDeduction` category rows →
> `line17_other_deductions` (the model/MeF/print single source). **FOUR live print bugs
> fixed**: property type printed in the col-(c) A-I column (col (b) empty) ·
> interest_other dropped from line 8 (18 didn't foot) · mgmt/supplies dropped from 17 ·
> 21/22a never printed with 23 omitting them (print diverged from the flowed K2).
> Official f8825sa template (manifest + AcroForm map) appends per 4 properties whenever
> detail rows exist; MeF adds 2b/2c/13/1(c) elements + per-property
> GeneralDependencyMedium detail statements linked from IRS8825 (no declared Schedule A
> doc in 2025v6.2; reconcile-or-refuse; ⚠ MediumExplanationType forbids newlines).
> Diagnostics D_8825_001-005. UI: address block (street/city/StateSelect/ZIP) · A-I
> select · face-ordered expenses incl. wages · "+ Add" category rows · Total Expenses /
> Net Income (Loss) Calculated rows, whole-dollar. Gates: flow 447 (FA008 healed inline)
> · MeF live-XSD · render + the line-1 column x-order pin · S5+S6 8/8 · manifest pin 82
> · tsc 0/vitest 278 · ORM + browser probes (isolated, 361-obj cascades). Deferrals:
> 8825 L21/L22a input path + rental-4797 auto-split · col (c) description statement ·
> Sch E detail-rows UI · B2-7b PY column rides B2-3 (DEFERRAL_AUDIT s48).

> **2026-07-10 session 47 — Form 4562 §168(k)(7) BONUS OPT-OUT ELECTION UNIT, spec-first
> (RS `fdeadfb` / tts s47; the s46 stated follow-on).** Return-level per-class election
> (`bonus_electout_classes`, mig 0183): engine forces bonus 0 for ALL qualified property
> in the class placed in service during the year → §280F Table 2 (RP 2025-16 §2.03(2))
> and the GA §168(k) add-back elimination follow automatically; the i4562-required
> election statement rides the print packet ("Sec. 168(k)(7) Election Statement", after
> the 4562) AND e-files as the DECLARED `ElectNotClaimSpclDeprecAllwnc` document
> (ReturnData1120S ref 1921, position-pinned; 1065 MeF = future-mapper deferral). NEW
> per-asset `bonus_eligible` (qualified-property flag). Diagnostics D_4562_ELECTBONUS
> (bonus inside an elected-out class = error, spec D008) + D_4562_ELECTGAP (bonus zeroed
> without the election = allowed-or-allowable warning, spec D009). UI: 7-class checkbox
> card + Bonus-Eligible checkbox. **⚠⚠ TAX-LAW CORRECTION: the s46 AMT arm reversed —
> a (k)(7) election-out does NOT re-engage the AMT 150DB recompute for post-2015 PIS
> (i6251 2025 2l + i4562 2025 p.7 Note verbatim; eligibility controls); 150DB now bites
> only for never-eligible 200DB property (bonus_eligible=false). Ken ratification
> pending (REVIEW_QUEUE s47).** Gates: engine 63 · flow 447 · MeF 69 · render 25 ·
> S5+S6 8/8 · tsc 0/vitest 278 · ORM + browser probes (isolated, cascade-deleted).
> Deferrals: proforma year-shift bonus_pct zeroing · 40% transitional election ·
> software class token (DEFERRAL_AUDIT s47).

> **2026-07-10 session 46 — Form 4562 DEPRECIATION-METHODS UNIT, spec-first (tts
> `5539168` / RS `d5e4386`; Ken's Lacerte method list).** Method dropdown = Ken's 7 codes
> (200DB/150DB/SL + NEW SL_RES/SL_NONRES/ADS_SL/NONE; passenger auto = a CLASSIFICATION,
> never a method); lives += 30/31.5-legacy/40; NEW `vehicle_classification`
> (under_6000/work_truck_6ft/over_6000 — mig 0182). **TWO LIVE ENGINE BUG CLASSES fixed
> against the verbatim Pub 946 (2025) tables**: the entire 200DB mid-quarter dict was
> derived-wrong (published A-2..A-5 now, all six lives; the old Q4 5-yr column summed to
> 99.00%) and the 150DB 10-yr column switched to SL a year late (A-14 verbatim: 8.74×6
> then 4.37). Luxury-auto caps corrected to Rev. Proc. 2025-16 T1/T2 ($19,600 yr-2 /
> $12,200 no-bonus yr-1 — the old constants miscited "Rev. Proc. 2025-13", the §831(b)
> proc). NEW: 150DB MQ (A-15..A-18) · SL/MM 31.5 (A-7) / ADS 30 (A-13) / ADS 40 (A-13a) ·
> month-aware MM final year · AMT post-1998 matrix (RS R007; i6251 2l — bonus-claimed
> 200DB = NO adjustment, §168(k)(2)(G)) · §179(b)(5)(A) SUV clamp $31,300 + the 6-ft-bed
> exception (R008) · print Section C rows 20a-20e · MeF ADS refuse seam · diagnostics
> D_4562_VCLASS/SUV179/METHOD (refuse-not-silent-zero). Gates: engine 55 (21 new
> published pins) · flow 447 · render 23 · MeF 66 · S5+S6 8/8 · tsc 0/vitest 278 · live
> ORM + browser probes. Deferrals: §168(k)(7) election unit = NEXT · ADS MeF leg ·
> 280F on AMT/state parallels (DEFERRAL_AUDIT s46).

> **2026-07-10 session 45 — Form 4562 RENUMBERED to the 2025 face (RS renumber unit #1)
> + a LIVE print-row fix (tts `4951f41` / RS `e695c1a`).** The 2025 face inserted **19h
> "50-year property"**, shifting residential rental → 19i and nonresidential real → 19j
> (Section C adds 20e; Part IV splits 23a/23b; Part V adds the 24c aircraft question).
> The RS spec block was rebuilt verbatim (26 → 60 line rows; the fabricated 6/7/8 Part-I
> chain fixed; lines 10-13 mirror the Ken-approved carryover loader). **tts print was
> LIVE-WRONG**: the field map keyed residential onto the PDF's Line19h row (= the 50-year
> row) — AcroForm row-group names follow the FACE letters, so names-exist validation
> stayed green while every real-property value printed one row off. Fixed across
> f4562_derivation (27.5→i, 39→j, +50→h) + field map (+L19j) + MeF _GDS_ELEMENTS
> (+GDS50YearPropertyGrp, required-PIS refuse seam); MeF XML for residential/nonres
> UNCHANGED (semantic elements — S5/S6 byte-stable). NEW y-band row-placement pin guards
> the class. Gates: MeF 66 · flow 447 · 4562 render 35 · S5+S6 8/8. Deferral: SL/MM
> 50-year percentage table unsupported (silent 0%, pre-existing → DEFERRAL_AUDIT).

> **2026-07-09 session 42 (S-19 item 12) — 1120-S Schedule B question 11 AUTO-ANSWERED,
> spec-first (tts `94380af` / RS `b7907bc`).** RS 1120S_SCHB RENUMBERED to the verified 2025
> face (verbatim vs f1120s.pdf pp.2-3; the old spec block was a fabricated pre-face numbering —
> its "B11" = an AE&P question that isn't on the face; 7 stale lines + 20 stale facts deleted
> in-loader) + NEW **R006**: B11 (receipts < $250K AND EOY assets < $250K → Sch L/M-1 not
> required) is DERIVED — YELLOW, preparer-overridable; **Schedule L/M-1 computation, printing,
> and diagnostics UNCHANGED** (Ken ruling 2026-07-09). Compute: `_auto_answer_b11_db` after the
> 1120-S formula pass — total receipts per the verbatim i1120s (2025) Q11 definition (p1 1a/4/5 ·
> K3(net)/K4/K5a/K6 · positive-only K7/K8a/K9/K10 · 8825 gross rents + legacy L21/L22a), assets
> = computed L15d, override-respecting write. Client: amber "Auto" pill + amber Yes/No chips on
> the derived answer (1120-S scoped; the 1065's unrelated B11 untouched); a click sets
> is_overridden (GREEN, never recomputed over). Render/MeF unchanged by construction (B11 flows
> from the stored value; `SchLAndSchM1NotRequiredInd` already mapped). Gates: 5 new DB tests
> (`test_schb_q11.py`) · flow 447 · MeF 1120-S 64 · S5+S6 scenario suites 8/8 · parity fixture
> regenerated byte-identical · vitest 278 · tsc 0 · live probe verified, cascade-deleted.
> Follow-ups → REVIEW_QUEUE: 1065 Sch B Q4 sibling (own Ken ruling needed) · K3a-gross capture ·
> SCHB renumber ratification.

> **2026-07-09 session 37 (S6 unit 3) — the 1120-S ATS Scenario-6 BUILD is COMPLETE; the
> s34→s37 S6 lane and the overnight work order are DONE.** `apps/efile/ats/scenario6_1120s.py`
> + `mef_build_ats_1120s_s6` (rollback-txn, seq 22) built through the REAL engine: **all 41
> pinned key lines tie TO THE DOLLAR** (page-1 chain 11,468,259 → 424,118 · COGS 10,061,879 ·
> 4797 14,433 · Sch D/8949 K7 78,649 · §179 62,935 · M-1 back-computed book loss (10,842) ·
> M-2 AAA 1,600,791 · Sch L both years) and the full submission + manifest are live-XSD-valid
> (2025v6.2). **PIN-signed** (`Signature1120SInfo`: practitioner/officer PINs, entered-by ERO,
> jurat facts, IRSResponsiblePrtyInfoCurrInd true, NO 8822-B/binaries — S6's signature
> option, exercised end-to-end for the first time). **NEW mapper channel: K-1 box 10 code**
> — the K-1 XSD's box-10 group REQUIRES a per-item code; the s34 refuse seam now yields to a
> caller-supplied `extract.k10_other_income_code` (the listed_evidence pattern; scenario
> asserts the key's code A), an un-asserted nonzero K10 still refuses → DEFERRAL_AUDIT s37
> (app input leg + the print-side bare-amount box 10). **Documented-quirk overrides (never
> engine changes)**: 8941 chain pins the LAW (K13g 51,014; the key's inverted 12,753 chain
> documented — ⚠⚠ UPLOAD GATED on the e-help ask, REVIEW_QUEUE s34) · the personal-property
> truck 8824 rides `is_real_property=True` at build only (face = key exact: 40,000 deferred /
> 0 recognized, all feeds zero; Ken's hard-RED + override ruling) · M1_6b overridden 0 (key's
> books expensed §179) · 1125-A Att-10's own 540 typo absorbed in the Misc row · Silverado
> depr 24,492 (the key's 24,619 cell contradicts its own 23/25a) · K-1 codes L/BA vs the
> key's quirk A/P · prior-year MACRS 17/20a split rides line 17 as one sum (no ADS channel).
> K-1 odd-dollar splits = key exact (Carrie sort_order 0; residual-to-last). Artifacts →
> docs/mef/ats_out/1120s_scenario6 (built with the documented placeholder originator id —
> pass the real one via `--efin` on the real run; ids in the local README.txt).
> Suites: S6 scenario 4 DB (key pins · K-1 splits · live-XSD XML · capital/8824/8941 chain) ·
> MeF 1120-S mapper 64 · flow gate 447 (no compute changes). ⚠ Test-DB lingering-session
> class: a fresh pytest run right after a completed one can hit "database is being accessed
> by other users" — rerun `--reuse-db`.

> **2026-07-09 session 36 (S6 unit 2) — Form 8824 coverage EXTENDED TO ENTITIES
> (1120-S/1065), ALL LEGS.** Spec-first from `server/specs/form_8824_spec.json`
> (R-8824-ENTROUTE, RS `b4c71b8`; live export re-verified — mirror semantically current).
> **Compute**: `entity_8824_feeds_from_rows` (pure) — L21 + ordinary L22 → entity 4797
> line 16; §1231 L22 → entity 4797 line 5 (`aggregate_dispositions` rolls both into the
> Part I/II sums → K9/K10 + page-1 4/6); capital L22 → Schedule D (1120-S) lines 5/12 →
> line 7/15 nets → K7/K8a (`aggregate_schedule_d`). §1043 rows EXCLUDED at entity
> (categorically ineligible — never silent). **Input**: "Like-Kind (8824)" tab on both
> entity editors (shared section/endpoint — no new serializer). **MeF**: the s34
> LikeKindExchange refuse seam RETIRED — per-exchange IRS8824 documents (ReturnData ref
> 1291, XSD lineNumber-verified), 4797 `GainLossForm8824Amt`/`OrdinaryGainLossForm8824Amt`
> + SchD `ST/LTCapGainLossLikeKindExchAmt` in the sums, reconcile-or-refuse vs the flowed
> FFVs; ⚠ NO refDocId channel exists in the XSD between 8824 and its destinations — ties
> are numeric; extract refuses RED-deferred/§1043/related-party rows (Part II identity
> unmodeled); NN line 19 omits on a realized loss (signed L24 carries it). **Diagnostics**:
> the D_8824_001-010 set now fires on entity returns (generalized `_state`); D_8824_010
> escalates to ERROR on entity §1043; NEW code-registered D_8824_011 (1065 capital — enter
> K8/K9a manually; no Schedule D (1065) aggregation/spec exists) — seed_rules run on prod;
> both spec-silent rulings → REVIEW_QUEUE for Ken. **Render**: `render_8824_entity`
> (shared `_render_8824_copies` core, 1040 output unchanged 8/8) in the entity packet
> after 4797. **FA**: FA-ENT-8824-01 ACTIVATED (RS `a54c406`, Supabase reseeded, deployed
> export 30 actives; pinned mirror spliced → 26) + `_run_ent8824_assertion` — flow gate
> **446→447**. Boundaries → DEFERRAL_AUDIT s36 (1065-capital no-auto-flow · related-party
> MeF refuse · entity-§1043 · NN-line-19). tts `e2cae48`. Suites: 12 (5 pure + 7 DB) ·
> MeF 64 (live-XSD incl. IRS8824 ×2) · 1040-lane 8824 18 + render 8 · tsc 0 / vitest 275.
> ⚠ Pooler degraded window during the session: rotating single-test
> `terminating connection due to administrator command` kills on test_4797_pipeline_leg —
> every test green in at least one run; not a code class.

> **2026-07-09 session 35 (overnight, S6 unit 1) — NEW FORM: Form 8941 (§45R small-employer
> health-insurance credit), ALL LEGS GREEN on the 1120-S.** Spec-first from
> `server/specs/8941_spec.json` (RS `b4c71b8`). **Input**: `Form8941` OneToOne model (migs
> 0179+0180 RLS, prod-applied) + `form-8941` GET/PATCH endpoint (row-locked — concurrent
> focusout autosaves lost fields via stale full-row serializer saves, probe-caught) + the
> "8941 Credit" tab on the 1120-S editor (live-UI probe verified). **Compute**:
> `compute_8941` — STATUTORY chain (WS5/WS6 phaseouts are REDUCTIONS per §45R(d)(3)(A),
> clamped ≥0; WS3 floor-to-$1,000; the face-$33,000 vs WS6-$33,300 self-contradiction
> encoded verbatim with D_8941_005 flagging the band); line 5 preparer-entered (Ken ruling);
> line 16 → Schedule K line 13g after the FFV backfill. Spec pin F8941-T1 green: line 8 =
> **51,014** on the ATS S6 inputs — the key's inverted 12,753 is documented (F8941-T2),
> never built; upload = Ken/e-help. **K-1**: K13g allocates residual-offset; box 13 prints/
> e-files code **BA** (i8941 verbatim; the key's code P = pre-2023 quirk) gated on the
> shared `k13g_is_8941_sourced` bridge-gate; an un-sourced K13g still refuses in MeF; print
> box 13 packs its 5 rows dynamically (6 possible codes). **Render**: f8941 AcroForm map
> (field-map validation test) + manifest entry + `render_8941` + `render-8941` action +
> entity packet block. **MeF**: IRS8941 document (XSD lineNumber-verified), ReturnData ref
> 1697, K13g `OtherCreditsAmt` refDocId link, extract reconcile-or-refuse; live-XSD valid.
> **Diagnostics** D_8941_001–006 seeded (§280C = warning-only per Ken ruling). **FAs**:
> FA-8941-01/02 ACTIVATED (RS `ab3b0ab`) + runners — flow gate **444→446**. ~~⚠ Mirror note:
> the deployed 1120S FA export now carries the s32-drift actives (FA008-012/RC001-variant/
> ENT-BND/4562-179) with no tts runners; the mirror is PINNED to the prior set + 8941 until
> the queued reconciliation pass.~~ **HEALED s64 (2026-07-12): the reconciliation pass ran —
> every drift id has an id-routed runner, both mirrors re-adopted from the deployed export
> (1120S verbatim 30; 1065 = 32 + 4 staged-pending), flow gate 447→460.** Boundaries → DEFERRAL_AUDIT: 1065 K15 / 1040-3800-4h 8941
> lanes; manual-other-credit + 8941 K13g coexistence; lines 17-20 (coop/exempt) never emit.
> tts `9e65aff`. Suites: 14 pure · 10 DB · MeF 59 (live-XSD) · S5 scenario + acroform-K1 16 ·
> tsc 0 / vitest 275.

> **2026-07-08 session 34 (S6 kickoff) — NEW MeF coverage: IRS8949 + Schedule D (1120-S)
> documents + the 1120-S PIN signature; Scenario 6 itself RS-BLOCKED.** The entity capital-
> gain chain now e-files end-to-end: Disposition rows (is_4797=False) → per-box IRS8949
> groups (NEW `form_8949_box` field, mig 0178; blank → C/F derivation; box/term
> contradictions refuse) → IRS1120SScheduleD box totals + line 7/15 nets
> **reconciled-or-refused vs the flowed K7/K8a** → refDocId links (Sch K 7/8a → SchD →
> 8949). First IRS8949 document mapper in the app (the 1040 lane is aggregate-path-only).
> PIN signature: `Signature1120SInfo` (PractitionerPINGrp / SignatureOptionCd / officer
> TaxpayerPIN + jurat facts / IRSResponsiblePrtyInfoCurrInd) — S6's signature option;
> S5's 8453-binary path unchanged. Refuse seams: capital-tx expenses-of-sale (code-E
> adjustment leg unbuilt) · entity LikeKindExchange rows (**8824's RS spec is 1040-only —
> an 1120-S with an exchange cannot e-file until the entity amendment + unit land**).
> ⚠ Scenario 6 BUILD blocked on three RS items (8941 greenfield — no spec exists · 8824
> entity extension · SCHD_1120S pre-2025 line_map renumber):
> `docs/rs_handoff/2026-07-08_s6_rs_gaps.md`. Mapper suite 55 · flow 444.

> **2026-07-08 session 33 (S-17b refund direct deposit) — 1040 MeF + input coverage DEEPENED
> (no new form; the 1040 already ticks).** The 1040 line-35 direct-deposit group now e-files:
> `_extract_direct_deposit` (elected on a 35a refund + BOTH TaxReturn bank fields; partial or
> lexically-invalid data REFUSES — never a silent paper-check downgrade) → IRS1040 35b/c/d
> (RoutingTransitNum/BankAccountTypeCd/DepositorAccountNum directly after RefundAmt; no Form
> 8888 — single account only) + the ReturnHeader `RefundDisbursementGrp` RTN/DAN
> (R0000-250/251). ⚠ `RefundDisbursementCd` enum unpublished: DD emits the reasoned "1"
> (module constant, e-help Q#5); paper check keeps the live-accepted "0". INPUT gap closed:
> the bank fields were entity-editors-only — the 1040 Payments tab now carries a "Refund
> Direct Deposit (lines 35b–35d)" card (debounced `/info/` autosave, inline RTN validation,
> live-UI-verified on an isolated probe). Pure suite 51 incl. live-XSD DD validation; flow
> gate 444 unchanged.

> **2026-07-08 session 32 (§179 pass-through + K-1 rounding + S5 binary attachments) — the
> 4797 (entity lane), K-1 (1120-S), and M-2 coverage DEEPENED; two NEW attachment forms.**
> **§179 pass-through (RS `R-4797-ENTPASS`, Ken-ruled)**: the entity 4797 now EXCLUDES
> §179-passed-through disposals (i4797 verbatim; trigger = `sec_179_elected` + `sec_179_prior`);
> per-owner PRO-RATA facts ride K-1 box 17 code K ("STMT" face + statement page +
> `DisposOfPropWithSect179DedStmt` XML w/ refDocId); corporate-level gain → M-2 3a via the
> seeded `ENT179_GAIN` internal line (formula + MeF statement row); `D_4797_ENTPASS` info;
> MeF refuse-seam retired. ⚠ Stated boundaries: the 1065 box-20-code-L PRINT side rides the
> future partner-K-1-PDF unit (no partner K-1 renderer exists); owner-side 1040 reporting
> from 17K facts = its own unit; casualty/installment §179 disposals unmodeled.
> **K-1 residual-offset rounding (RS `R-K1-ROUND`, Ken-ruled)**: `k1_issuer.allocate_whole`
> — Σ per-shareholder K-1 == Schedule K EXACTLY (residual to the last owner; matches the S5
> and S6 keys). ⚠ 1065/1041 allocators still drift (REVIEW_QUEUE — own spec-first units).
> **NEW attachment forms (manifest 80)**: `f8453corp` (8453-CORP, Rev. 12-2025 — REPLACES
> 8453-S/8453-C; irs.gov filename `f8453crp.pdf`) + `f8822b` — field maps PDF-validated,
> renderers in `apps/tts_forms/attachments.py`, ride e-filed 1120-S submissions as
> BinaryAttachments. Flow gate 440 → 444. GA-501 spec drift CLOSED RS-side (HB 1199 text +
> 9c/settle-block line_map; tts already computed those lines — T6 pin added).

> **2026-07-08 (S-11 Form 1041 fiduciary — leg 8a: the 1041 flow-assertion gate) — ✅ THE 1041
> FULLY TICKS (tag `1041-complete`, commit `06c8946`; full gate 422 → 440).**
> Ken green-lit the FA authoring plan. **Root cause of the "zero 1041 FAs":** the 2026-07-05 S-11 RS
> authoring HAD staged 10 DRAFT FlowAssertions across load_1041_spine / load_1041_schedule_k1 — never
> activated; the export serves active-only. **RS side (`adc4710`):** NEW `load_1041_flow_assertions`
> = the single FA home — 17 active (page-1 chains · Subchapter J DNI/IDD/§662-tier core · §642(b)
> exemption table · §1(e) rates + 0/15/20 stacking pins · ESBT 37% · K-1 character/Σ-reconciliation/
> §642(h) final-year gating · AMT/bankruptcy/grantor defers · the GA-501 state base) + 2 staged
> (FA-1041-NIIT trust-side 8960 · FA-K1041-NIIT box-14H import — activate when built); FA-1041-TAX /
> FA-K1041-RECON disabled as superseded; the drafts tombstoned OUT of the spec loaders so a reseed
> can't regress statuses; SQLite-validated, Supabase-seeded, deployed export = exactly the 17.
> **tts side:** export cached (`server/specs/flow_assertions_1041.json`); `_RUNNERS_1041` = 17 pure
> runners over compute_1041 / rate-schedule / cap-gain worksheet / allocate_k1_boxes / compute_ga501.
> Remaining 1041 work is MeF-lane only (the 4 ATS scenarios — 1041 v5.3 ATS schemas owed by SOR),
> which does not gate the form tick (the 1065 precedent).

> **2026-07-08 (S-11 Form 1041 fiduciary — legs 7 + 8b: beneficiary UI/API + f1041sk1 K-1 PDF + live
> frontend verify) — ⏳ ALL BUILDABLE LEGS GREEN (ticks when the RS-blocked FA gate lands).**
> **Legs 7+8b (session 31 continuation):** `field_maps/f1041sk1_2025.py` (position-correlated IRS f1_NN;
> box 9 = 3 code rows / 11 = 5 / 12 = 5 / 13 = 3 / 14 = 6) + `render_k1_1041`/`render_all_k1s_1041`
> printing the SAME allocator output persisted to `BeneficiaryK1Computed` (box-11 A-F overflow → STMT +
> statement page; grantor refuses; wired into the k1s package + render_complete). Beneficiary INPUT link
> closed: `BeneficiarySerializer` + nested `beneficiaries` (Meta.fields gotcha guarded by test) +
> CRUD endpoints w/ mutation-recompute; `FIDUCIARY_TABS` (the 1041 editor no longer falls through to the
> 1120-S tab set — entity/Sch G/ESBT/K-1-sources/payments now reachable) + Beneficiaries tab +
> `GA501_SECTION_TABS`. **Live UI verify** (dev servers, isolated probe firm, deleted after): interest
> 10,000 → L9/L17 round-trip · Sch B B11/B15=10,000 · beneficiaries 60/40 · GA-501 created from the State
> tab (pull 10,000 / exemption 1,350 / L8 449) · K-1 Package PDF served. 6 new DB tests; tsc 0; vitest 275.
> **REMAINS (externally blocked):** leg 8a flow-assertion gate — RS has ZERO 1041 FAs (REVIEW_QUEUE
> 2026-07-08 s31, authoring plan proposed) · the 4 ATS scenarios (1041 v5.3 ATS schemas still owed by SOR).

> **2026-07-08 (S-11 Form 1041 fiduciary — APP BUILD, leg 6 GA Form 501) — ⏳ IN PROGRESS (does NOT tick).**
> **Leg 6 GA-501 (`370fdb0`): the Georgia fiduciary state return, all four legs in one unit.** Spec-first
> from the live RS `GA501` export (cached `server/specs/ga501_spec.json`). **Compute** `compute_ga501.py`
> (dedicated, GA-500 pattern): federal 1041 ATI (L17) base → Sch 2 §48-7-27 netting → beneficiary share
> removed at L4 (never the federal IDD) → trust $1,350/estate $2,700 exemption → 5.19% (year-keyed, 2026
> 4.99%) → face-verbatim 9c credit cap → the page-2 11c-20 settle block (face arithmetic beyond the spec —
> RS handoff filed to extend the line_map). All 4 RS pins tie (T1 2,525 / T2 1,936 / T3 708 / T4 −3,000);
> 16 pure tests. **Input** `seed_ga501` (3 sections/42 lines, prod-seeded); `FIDUCIARY_STATE_FORM_MAP`
> trust→GA-501; federal pull L17→L1 + FID_TYPE from ENTITY_TYPE (+ fixed the pre-existing GA-700
> refresh-from-federal fall-through); client "Georgia (Form 501)" option on 1041 returns (tsc 0 err).
> **Render** `render_ga501_overlay` on the DOR web-version fillable (semantic names S1L1..S1L20/S2L*,
> template `ga501_2025.pdf` unmodified — the GA-700 recipe); 6a/6b exemption boxes share ONE field name →
> abs_pos "X" literals; residency digit; Sch 3 box-D copy of L4; probe-rendered + visually verified.
> **Diagnostics** `rules_ga501.py` — 8 `D_GA501_*` incl. the part-year/NR Schedule-4 RED-defer; conformity
> text carries the HB 1199 ruling (RS spec text drift → `docs/rs_handoff/2026-07-08_ga501_spec_drift.md`).
> **DB leg 5/5 green (50s — healthy pooler); flow gate 422 unchanged.** v1 boundaries (stated): Schedule 4
> NR allocation RED-defers; Sch 3 per-beneficiary rows render-deferred (L4 total only); Sch 5/5B/6/7 ride
> direct-entry lines; fiduciary name/title boxes blank (no app source — the f1041 boundary). **The 1041
> STILL does not tick** — legs 7 (frontend verify) / 8 (1041 flow-assertion gate) + the f1041sk1 K-1 PDF remain.

> **2026-07-06 (S-11 Form 1041 fiduciary — APP BUILD, leg 5 f1041 render) — ⏳ IN PROGRESS (does NOT tick).**
> **Leg 5 f1041 render (`72b38bb`):** renders onto the official IRS f1041.pdf AcroForm template. Downloaded
> f1041.pdf + f1041sk1.pdf from irs.gov (manifest + `update_irs_forms.py`, hashes recorded); dumped the 173
> AcroForm fields and position-correlated each widget with its line label. `field_maps/f1041_2025.py` (73 fields):
> page-1 L1-24 + payments (25a/25e→Sch G Part II 10/14, 25b/26-30) + Schedule B B1-B15 + Schedule G Part I
> G1a-G9 + entity-type checkboxes (`cb_<type>`, set from ENTITY_TYPE by a `render_tax_return` block) + HEADER_MAP
> (name/EIN/fiduciary/address/final-return). Registered `f1041` in `ACROFORM_FORM_IDS` + `form_code_to_id`.
> **Visually probe-verified** (field-probe render — every mapped box holds its own key; page 1 header/checkboxes/
> income/deductions/tax/payments + page 2 Sch B / Sch G Part I all land correctly). Render test asserts L1 20,000 /
> L23 19,400 / L24 5,165 / L21 600 + header name in the PDF text layer. **The 1041 form STILL does not fully tick**
> — legs 6 (GA 501) / 7 (frontend verify) / 8 (flow gate) + the per-beneficiary K-1 PDF (f1041sk1) remain. Below:
> legs 1/2/3/4.
>
> **2026-07-06 (S-11 Form 1041 fiduciary — APP BUILD, legs 1/2/3/4) — ⏳ IN PROGRESS (does NOT tick).**
> **Leg 3 Schedule K-1 (1041) beneficiary issuance (`99a8943`):** new `Beneficiary` + `BeneficiaryK1Computed`
> models (migs 0175 create / 0176 RLS, applied to prod + test DB). `k1_allocator_1041.py` allocates the entity's
> DNI classes to each beneficiary with CHARACTER RETAINED (§652(b)/§662(b)): `box[c]=round(ent_[c]×dni_pct/100)`,
> boxes 1-8 floored at 0, box 9 directly-apportioned, box 11 final-year (final return only), box 14
> tax-exempt/NIIT/§199A; carry-out ratio = min(1, IDD/taxable-DNI); grantor → no K-1; persisted via
> `persist_all_beneficiary_k1s_db` wired into `compute_return`. Seed grew a `k1_sources` section → 9 sections /
> 83 lines. 6 `D_K1041_*` diagnostics (`rules_1041_k1.py`, reseeded). 6 pure + 3 DB tests. **The K-1 still does
> NOT render** (leg 5). Below: legs 1/2/4.
>
> **2026-07-06 (S-11 Form 1041 fiduciary — legs 1/2/4) — ⏳ IN PROGRESS (does NOT tick).**
> Greenfield estates & trusts entity type (Ken-directed "start 1041"). **Leg 1 seed (`539b204`):** `1041`
> FormDefinition, 8 sections / 72 lines (page-1 L1-25b + Sch B DNI/IDD + Sch G tax + entity/elections + ESBT +
> payments), seeded to prod; `ENTITY_FORM_MAP` `"trust"→"1041"` (EntityType.TRUST + the React frontend already
> existed). **Leg 2 compute (`539b204`):** dedicated `compute_1041.py` (`compute_1041_db` early-return in
> `compute_return`, NOT the FORMULA_REGISTRY) — R-1041-TOTINC/TOTDED/ATI/DNI(§643(a) corpus back-out)/IDD(smaller-of)
> /TIERS(§662)/EXEMPT(§642(b) 600/300/100/5100/0)/TAXINC/TAX(§1(e) sched + 0/15/20 cap-gain wksht)/ESBT(37%)/NIIT/
> TOTTAX; year-guarded TY2025; 13 pure + 1 DB smoke test (persist L23=19400/L24=5165). **Leg 4 diagnostics
> (`d1117d8`):** 11 `D_1041_*` (`rules_1041.py`, registered + reseeded to prod; AMT/bankruptcy RED-defer per
> Gate-1 D-2); 6 DB tests. Flow gate 422. **Form does NOT tick — legs 3 (K-1 `SCHEDULE_K1_1041`) / 5 (f1041
> render — IRS PDF not yet downloaded) / 6 (GA 501) / 7 (frontend verify) / 8 (flow gate) REMAIN.** Detail:
> `.claude` memory `s11-1041-fiduciary-kickoff.md`.

> **2026-07-06 (S-6 PAL/basis deepening) — app build COMPLETE, all 5 R-items → ✅ DONE.** Extends the existing
> 8582 per-activity + Schedule E engine. **R1 self-rental** (§1.469-2(f)(6), `c4cd928`) = the only real compute
> change (Sch E type-7 net income recharacterized non-passive, excluded from the 8582 passive buckets; mig 0172;
> `D_SCHE_SELFRENTAL`). **R2-R5** (`07fb29f`) all diagnostic-only: R2 PTP `D_8582_PTP` (auto-derived from
> `is_ptp`) · R3 REP `D_8582_RE_PRO` RED→info + `_REP_MATLPART`/`_REP_TESTS` · R4 at-risk `D_8582_ATRISK` · R5
> **new Form 461** §461(l) EBL (`compute_461` + `rules_461`, $313k/$626k 2025, diagnostic scope — NO Sch-1
> add-back/NOL). migs 0173/0174. 27 tests; flow 422; tsc 0 / vitest 275. Detail: `.claude` memory
> `s6-pal-basis-deepening.md`.

> **2026-07-06 (S-5 COMPLETE — legs 1 + 2) — entity-return boundary safety net → ✅ DONE, live on prod.** Not a
> form face — the season-one "no silent gap" net (`apps/diagnostics/rules_entity_boundary.py`, 6 `D_EB_*` REDs
> for 1065 + 1120-S) + `EntityBoundaryAssertion` model (migs 0170/0171). **Leg 1 (`50f2874`):** diagnostics;
> M-3 + K-2/K-3 foreign gate auto-derive from data; §704(c)/§754/apportionment read preparer-assertion flags
> (default non-firing); superseded `D_L_M3` deactivated. **Leg 2 (`d74b016`):** React "Boundary" input tab on
> the 1065 + 1120-S editors (`EntityBoundarySection.tsx`) + `entity-boundary` GET/PATCH API action + serializer
> — the preparer-flag arms now fire from the UI. 17 tests (13 diagnostics + 4 endpoint); flow gate 422; `check`
> clean; client tsc 0 err / vitest 275. Detail: `.claude` memory `s5-entity-boundary-diagnostics.md`.

> **2026-07-06 (S-4 follow-on, same session) — Schedule M-1 line-4/line-7 total boxes → ✅ DONE (`c0dbff8`).**
> Render-layer synthesis (`M1_4`=4a+4b+4c, `M1_7`=7a+7b → `f6_132`/`f6_139`) closes the one display gap noted
> in the render leg below; the printed 1065 M-1 is now arithmetically self-consistent. **f1065 render has NO
> known display gaps.** No compute/seed/spec change; flow gate 422. ⚠ compute-vs-spec nuance flagged (M1_5/M1_8
> include 4c/7b; RS 1065_M1 spec lists only 4a/4b + 6a/7a) — a separate compute leg.

> **2026-07-06 (S-4 follow-on) — Form 1065 render recalibration → ✅ DONE. The 1065 form now FULLY TICKS;
> S-4 COMPLETE end-to-end.** The final deferred leg. Root cause: `f1065` was on the legacy coordinate
> overlay (`coordinates/f1065.py`) — never calibrated ("approximate starting points") AND on pre-2025 line
> numbering — while the IRS `f1065.pdf` is a proper 6-page fillable AcroForm (440 fields). Per the
> IRS-rendering rules (AcroForm preferred; the 1120-S / GA-600S precedent), converted the WHOLE form to the
> AcroForm-fill backend: `field_maps/f1065_2025.py` (191 mappings, was an empty stub) covering header +
> page 1 (2025 numbering: line 20 §179D, line 22 total deductions, line 23 ordinary business income →
> Sch K line 1) + Schedule K (K1–K21 + K_ANALYSIS, single-page f5_*) + Schedule L + M-1 + M-2; registered
> `f1065` in `renderer.ACROFORM_FORM_IDS`; marked the coordinate map SUPERSEDED. Field→line map extracted
> from the PDF's OWN label text (not memory); ⚠ seed L-keys are OFFSET from form line numbers (seed L15 =
> form line 14 total assets, seed L8 = form line 7b). Reused `render_tax_return`'s shared Schedule-L
> 4-column translation + contra-net computation + parenthesized-contra negation (same machinery as 1120-S).
> Visually verified a fully-populated 6-page render (every value in the correct box; contra nets right;
> M-1 line 9 = Analysis line 1 = M-2 line 3 ties). 3 DB tests (`test_1065_render_leg.py`). No compute/
> model/migration/spec change → flow gate **422** unchanged; `manage.py check` clean. Documented display-only
> gaps (non-blocking, all totals correct): M-1 line-4 / line-7 total boxes unmapped (itemized sub-amounts
> print inline; totals aren't stored compute keys). Detail: `.claude` memory `f1065-render-acroform-leg.md`.

> **2026-07-06 (S-10, leg 3) — GA Form 700 render → ✅ DONE (`0d59255`). S-10 GA-700 now COMPLETE (all 4 legs);
> the form TICKS.** The deferred blocker (thought to be a viewer-only form) was resolved by the DOR "Print Blank
> Forms" static PDF: `render_ga700_overlay` fills the DOR fillable PDF via the AcroForm text-overlay pipeline
> (`_acroform_fill`), whose semantic field names (S1L1..S8L12, FEI_NUMBER, ...) map ~1:1 to the compute keys —
> so the stored template (`server/pdf_templates/ga700_2025.pdf`) is the DOR PDF unmodified and
> `field_maps/ga700_2025.py` points straight at those names (no AcroForm-Creator injection, unlike GA-600S).
> Ratio S7_2 pre-formatted to a 6-decimal factor (its DB PERCENTAGE type would render "1.0%") + transcribed to
> Sch 2 L4; Total Tax (S1_7) → Sch 3 L1; GA_PTET → CBX_ELECT_TO_PAY checkbox; S8_6 ("other income") → form line
> 7 (S8L7). Visually verified on all 4 schedule pages; 3 DB tests (`test_ga700_render_leg.py`). v1 blanks
> (documented, non-blocking): additive subtotals S1L3/S8L10/S5L8/S6L5 (compute follow-up), Schedule 4 partner
> detail, continuation name/FEIN. Flow gate 422; `manage.py check` clean. **⚠⚠ GA §179 cross-spec conflict
> (GA700 $1.05M/$2.62M vs GA600 $1.25M/$3.13M) still open for Ken** (§179 is separately-stated K-1, not on the
> entity face — did not block render).

> **2026-07-06 (S-10) — GA Form 700 (Georgia partnership + PTET, "GA-700") → legs 1/2/4 DONE; render DEFERRED
> (form did NOT fully tick — SUPERSEDED by the leg-3 note above).** The GA-600S analog for a partnership; attaches to a federal **1065**. Built
> spec-first from RS `GA700` (status `draft` — cached `server/specs/ga700_spec.json`). **Leg 1 compute**
> (`1d7f102`): `FORMULAS_GA700` (`FORMULA_REGISTRY["GA-700"]`) — Sch 8 income (fed ordinary + guaranteed
> payments + other, + §168(k) bonus add-back − GA-4562 depreciation) → Sch 7 **single gross-receipts
> apportionment** (§48-7-31, 6 decimals ROUND_DOWN, **defaults 1.0 / 100% GA** when no everywhere-receipts —
> single-state, avoids a silent $0) → Sch 2 apportioned → Sch 1 GA taxable (NOL/passive-loss floored) → **PTET
> 5.19%** (§48-7-21) when `GA_PTET` elected, else blank (pure pass-through). GA §179 $1,050,000/$2,620,000
> separately-stated K-1 delta (NOT in the entity Sch-8 flow). 9 pure tests (all 5 entity RS pins + §179
> limit/phaseout + edges). **Leg 2 input** (`6c26d72`, prod-seeded): `seed_ga700` (8 sections / 30 lines);
> views.py `PARTNERSHIP_STATE_FORM_MAP` (1065→GA-700) + `create_state_return` dispatch + `GA700_FEDERAL_PULL`
> (federal line 23→S8_1, K4c→S8_5) + `_populate_ga700_from_federal`; FormEditor.tsx `GA_700_SECTION_TABS` +
> `isGa700` + create-state label distinguishes 1065 (Form 700) from 1120-S (Form 600S). 3 DB tests. **Leg 4
> diagnostics** (`f70a9d4`, prod-seeded): `rules_ga700.py` (10 D_GA700_*: 2 warning 179LIMIT/CONFORM, 8 info —
> DEPR/APPORT/NETWORTH/PTET/NRW/COMPOSITE/NOL/UET), runner-registered. 9 DB tests. Flow gate 422; frontend tsc 0.
> **⚠ render leg 3 DEFERRED** — the GA 700 is served via a fillable-forms VIEWER
> (`apps.dor.ga.gov/FillableForms/PDFViewer/Index?form=2025GA700`), NOT a static PDF; acquisition path differs
> from the flat-state recipe. **The GA-700 form ticks only when render lands.** **⚠⚠ GA §179 CROSS-SPEC CONFLICT
> flagged for Ken:** GA700 spec $1.05M/$2.62M vs GA600 $1.25M/$3.13M. **v1 = entity-level PTET return;
> partner-level GA-source allocation + nonresident 4% withholding + PTET owner-side Form 500 subtraction
> DEFERRED** (the app Partner model has no residency field — a future migration; RS spec tests 6-7).

> **2026-07-05 (S-4 1065 core, leg 6) — 1065 flow-assertion gate → ✅ DONE. S-4 CORE COMPLETE.**
> The FIRST flow-assertion gate for the partnership entity (previously `1065_se` was diagnostic-gated only).
> Fetched the Rule Studio export (`/api/flow-assertions/export/?entity_type=1065`, 28 assertions) and split it
> (mirroring the 1040 `_pending` convention — nothing silently dropped): **`server/specs/flow_assertions_1065.json`
> — 22 ACTIVE** (every assertion whose 1065 compute is built in legs 1a-5) + **`flow_assertions_1065_pending.json`
> — 6 STAGED** (`FA-ENT-BND-01/02/03` = ENTITY_BOUNDARY/S-5 not built; `GATE-8990-163J` = Form 8990 not built;
> `GATE-704C-706D-DEFER` = Partner item-M/N §704(c)/§706(d) fields not built; `RECON-M2-CAPITAL` = item-L
> tax-basis capital roll-forward RED-deferred). All runners are in `tests/test_flow_assertions.py`
> (`_run_1065_assertion` / `_RUNNERS_1065`, PURE / no-DB): **RECON-P1-K1/ANALYSIS/L-BALANCE** re-derived through
> the real `FORMULAS_1065`; **RECON-K1-K/9C + INV-GP-DIRECT/CHAR** through the real `k1_allocator` (MagicMock
> partners, `PartnerAllocation` patched); **RECON-14A + INV-SE-BASE** through `compute_1065_se`
> (`entity_14a_bottom_up`, `worksheet_base`); **RECON-M1-ANALYSIS/L21-M2 + GATE-K2K3/SMALL-PTNR** via diagnostic
> source-inspection (the tie/suppression is diagnostic-enforced); **TI001-4** (MACRS), **FA-4562-179-02/03**,
> **RC004** reuse the existing shared runners. A new `gating_check` assertion type was registered; `RECON-*` /
> `INV-*` are routed into the reconciliation / table_invariant runners by id prefix. Gate result: **24 1065
> tests green** (22 assertions + loaded/pending-staged guards); **full flow-assertion suite 423 passed**;
> `manage.py check` clean; no compute/model/migration change. ⚠ **The 1065 FORM still does NOT fully tick** —
> `f1065` page-1 + Schedule K **render recalibration** (2025 coords) is a deferred follow-on leg; S-4 CORE
> (compute + diagnostics + K-1 issue/import + flow gate) is what completed here. **▶ Next: Ken directs** —
> S-10 GA-700 is now UNBLOCKED (depended on the federal 1065 flow), plus S-11 1041 / S-5 / S-6 / S-13/S-14.

> **2026-07-05 (S-4 1065 core, leg 5) — issuer-side K-1 persistence + 1065 → 1040 import → ✅ DONE.**
> The 1120-S `ShareholderK1Computed` / `k1_import.py` mirror for partnerships. **NEW model
> `PartnerK1Computed`** (migrations **0168** create + **0169** RLS default-deny — **applied to the shared prod
> DB** via `migrate returns`; leg 5 is the first S-4 leg with a real schema migration — legs 1–4 were
> FormFieldValue-backed). `box_shares` is keyed by K-1 **box number** ("1"/"4c"/"14a"…), the OUTPUT keys of
> `k1_allocator.allocate_k1`, NOT the entity Schedule K "K1" keys the S-corp twin uses. **Issuer COMPUTE
> already existed** (`allocate_all_k1s`) — this leg added **PERSIST** (`persist_k1_for_partner_db` /
> `persist_all_partner_k1s_db` in `k1_allocator.py`, hooked in `compute.py` immediately after
> `compute_1065_se_db` so each partner's box 14a is final and the persisted row always mirrors the entity's
> current Schedule K). **Import side** (`k1_import.py`): `_PARTNER_BOX_TO_K1_FIELD` (★ the 1065-specific map —
> box **14a → `se_earnings`**, no 1120-S analog since S-corp shareholders have no SE; §199A QBI = box 1,
> guaranteed payments excluded) + `available_partner_k1_offers` / `import_partner_k1_offer` /
> `dismiss_partner_k1_offer`. `available_k1_offers` now MERGES both owner types; every offer carries
> `owner_kind` / `owner_id` / `source_type`; new `import_k1_offer_by_owner` / `dismiss_k1_offer_by_owner`
> dispatch by resolving the id as a Shareholder or Partner. `ScheduleK1.imported_from_partner` FK added;
> `K1ImportDismissal.shareholder` made nullable + `partner` FK + a 2nd unique constraint (Postgres NULLs
> distinct → no cross-owner collision). Views re-key the URL param `shareholder_id`→`owner_id`; serializer
> exposes `imported_from_partner`; frontend `K1ImportOffer` type + import banner use `owner_id`. Tests:
> `test_1065_k1_import_leg.py` — 2 pure (box→field map incl. 14a→se_earnings) + 6 DB (persist 60/40 split +
> box 14a · non-1065 no-op · offer→import · source-update offer · dismiss/resurface · unknown-owner false).
> Shareholder pipeline (`test_k1_import_stage3.py`, 6) + `test_k1_allocator.py` re-verified green; frontend
> `tsc --noEmit` 0 errors. **DEFERRED — STILL OPEN** (leg 5 added a new model, not Partner item-M/N/L-dollar
> fields; each needs a separate Partner-field migration): `D_M2_1`, `D_K1_704C`, `D_K1_706D`. **▶ NEXT leg 6:
> the 1065 flow-assertion gate** (none exists — 1065_se is diagnostic-gated only), then S-4 closes.

> **2026-07-05 (S-4 1065 core, leg 4) — Schedule K-1 allocation reconcile (RECON-K1-K) → ✅ DONE.**
> Built spec-first from RS `SCHEDULE_K1_1065`. **Allocator wiring (`k1_allocator.py`):** promoted the local
> `k_to_box` to a module-level **`K_TO_BOX`** (single source of truth shared with the reconcile diagnostic)
> and wired the leg-1b **`K13c`→box 13c / `K13e`→box 13e** deductions into it + `K_LINES_PRO_RATA`. **New
> `rules_1065_k1.py` (registered):** **`D_K1_RECON`** (error — Σ per-partner K-1 box ≠ entity Schedule K
> line; profit/loss %s must total 100% per category; whole-dollar rounding tolerance = #partners),
> **`D_K1_9C`** (info — entity K9c allocates to box 9c; confirms the pass-through — closes the open box-9c
> verification), **`D_K1_SPECIAL_ALLOC`** (warning — a PartnerAllocation override is applied but §704(b) SEE
> is not tested), **`D_K1_ITEML`** (warning — item L tax-basis capital doesn't roll forward: ending ≠
> beginning + contributed + current-year − withdrawals), **`D_K1_CAPPCT`** (info — an item J capital % is
> reported; nothing allocates by capital_pct — expected). Tests: 3 pure (K_TO_BOX has 13c/13e; 13c/13e
> allocate; 60/40 sums to entity) + 7 DB (recon holds/breaks · 9c · special alloc · item-L ties/breaks ·
> cappct). Flow gate 398, existing allocator suite green. **DEFERRED** (need new Partner boolean fields +
> migration; RED-defer structure, see DEFERRAL_AUDIT): `D_K1_704C` (item M §704(c)) + `D_K1_706D` (§706(d)
> mid-year interest change).

> **2026-07-05 (S-4 1065 core, leg 3) — Schedule M-1 / M-2 reconciliation tie-outs → ✅ DONE
> (diagnostics; the M-1/M-2 sum compute already existed).** Built spec-first from RS `1065_M1` + `1065_M2`.
> This wires the RECON-ANALYSIS chain that leg-1b's `K_ANALYSIS` unblocked (R-M1-9's flagged "tts computes
> NO Analysis line" gap is now closed): **Analysis line (`K_ANALYSIS`) = M-1 line 9 (`M1_9`) = M-2 line 3
> (`M2_3`)**. New `rules_1065_m.py` (registered): **`D_M1_ANALYSIS`** (error — M1_9 ≠ K_ANALYSIS; the
> book→return reconciliation must land on the Schedule K return income), **`D_M2_3`** (warning — M2_3
> data-entry ≠ M1_9; both-zero quiet), **`D_M1_EXEMPT`** / **`D_M2_EXEMPT`** (info — Schedule B Q4 box `B6`).
> ★ B6 exemption **suppresses** the two tie checks (the EXEMPT infos own it). The R-M1-5/8/9 + R-M2-5/8/9 sum
> formulas were already in FORMULAS_1065 (M1_5/M1_8/M1_9, M2_5/M2_8/M2_9) — validation-only, no compute change.
> Tests: 3 pure (spec pins M1-1 M1_9=298000, M2-1 M2_9=728000, tie identity) + 4 DB (tie holds quiet · M1
> mismatch fires · M2_3 off fires · exempt suppression). `check` clean. **DEFERRED** (K-1 leg, see
> DEFERRAL_AUDIT): `D_M2_1` (M-2 line 1 = Σ K-1 item L beginning capital — needs the item-L roll-forward;
> Partner model has only capital %, not tax-basis capital $).

> **2026-07-05 (S-4 1065 core, leg 2) — Schedule L balance check + M-2 tie / M-3 / exempt diagnostics
> → ✅ DONE (diagnostics; the L14/L22 total compute already existed).** Built spec-first from RS `1065_L`.
> Build-gap #2 closed ("tts has NO balance check"). New `rules_1065_l.py` (registered): **`D_L_BALANCE_BOY`**
> / **`D_L_BALANCE_EOY`** (error — face line 14 total assets [app `L15a`/`L15d`] ≠ face line 22 total
> liab+capital [app `L24a`/`L24d`], per column), **`D_L_21_M2_TIE`** (warning — partners' capital EOY [`L23d`]
> ≠ Schedule M-2 line 9 [`M2_9`]; both-zero stays quiet), **`D_L_M3`** (warning — total assets EOY ≥ $10M →
> Schedule M-3 required, RED-defer Decision F), **`D_L_EXEMPT`** (info — Schedule B Q4 small-partnership box
> [app `B6`] set → Schedule L not required). ★ The small-partnership exemption (B6) **suppresses** the balance
> errors + the M-2 tie (D_L_EXEMPT owns it). The R-L-14/R-L-22 total formulas were already in FORMULAS_1065
> (`L15a/d`, `L24a/d`, contra-netted) — this leg is validation-only, no compute change. Tests: 7 DB
> (balanced quiet · OOB BOY/EOY fire · exempt suppression · M-3 threshold · M-2 tie equal/differ). `check`
> clean. RED-defers per spec: R-L-21-TIE math, Schedule M-3 build.

> **2026-07-05 (S-4 1065 core, leg 1b) — Schedule K 2025 renumber + Analysis-of-Net-Income line
> + handoff/page-1 diagnostics → ✅ DONE (compute + input + diagnostics; render still deferred).**
> Built spec-first from RS `SCH_K_1065` + `1065_PAGE1` (cached specs). **Renumber (seed_1065):** the
> app's Schedule K was on PRE-2025 numbering; reconciled to the 2025 face — foreign taxes `K16a` → **line
> 21 (`K21`)**; **line 16 → `K16` "Schedule K-3 is attached" checkbox** (international K-2/K-3 RED-defer,
> Decision A); investment interest `K13d` → **`K13b`**; added **`K13c`** (§59(e)(2)) + **`K13e`** (other
> deductions); new computed **`K_ANALYSIS`** line. **Compute (FORMULAS_1065):** `K_ANALYSIS` = (Σ K
> 1-11) − (Σ K 12-13e + 21) per R-SCHK-ANALYSIS (i1065 verbatim; ties M-1 L9 / M-2 L3 — tie-out enforced
> in the later M-1/M-2 leg). **Allocator (k1_allocator):** carried the K13d→K13b / K16a→K21 renames through
> `K_LINE_CATEGORY` / `K_LINES_PRO_RATA` / the K→box map so existing allocation keeps working. **Diagnostics
> (new `rules_1065_schk.py`, registered):** `D_SCHK_HANDOFF` (error — K1 ≠ page-1 L23), `D_SCHK_K3` (error —
> K-3 attached), `D_SCHK_9C` (info — unrecap §1250), `D_1065P1_4797` (warning — L6 recapture), `D_1065P1_COGS`
> (warning — COGS w/o 1125-A). **Tests:** 7 pure (spec-driven Analysis pin 215000 + renumber assertions) +
> 9 DB (pipeline Analysis persist + 8 diagnostics fire/quiet). Flow gate 398, SE pure 36, `check` clean.
> **DEFERRED (see DEFERRAL_AUDIT):** `D_1065P1_174A` (needs an R&E input line), `D_SCHK_704C` (needs
> item-M/N flags), the f1065 page-1+SchK **render recalibration** (coords stale for 2025), and K13c/K13e
> per-partner K-1 box allocation (the K-1 reconcile leg). ⚠ Prod reseed of `seed_1065` DELETES the old
> `K16a`/`K13d` FFV rows — flag for Ken before reseeding prod (leg-1a precedent).

> **2026-07-05 (seventeenth session) — NC D-400 (North Carolina individual) → ✅ FULLY COMPLETE
> across ALL 4 LEGS (S-9); no migration, FormFieldValue-backed (GA-500/SC1040/AL40 pattern),
> form_code "NC_D400".** Built spec-first from the RS `NC_D400` spec (status `draft`; TY2025).
> **Compute** ✅ `compute_nc_d400` (dispatched in `compute_return` for NC_D400) — federal-AGI start (L6),
> the ★ 85% bonus (L3) + 85% §179-excess (L4) add-back (NC decoupled, $25k/$200k limits, conformity FROZEN
> Jan-1-2023 → OBBBA N/A), AGI-banded child deduction (breakpoint→higher band), restricted NC-itemized
> subset (mort+prop capped $20k / property $10k-$5k MFS / medical less 7.5% AGI), std ded (MFS $0 if spouse
> itemizes), Schedule S Part B subtractions (Bailey/military/SS/US-obligation), Schedule PN proration, and
> the ★ FLAT 4.25% tax (§105-153.7, year-guarded to 2025 — steps to 3.99% after). All 8 RS scenario pins
> verified. **Input** ✅ `seed_nc_d400` (4 sections / 47 lines: face + Schedule S + Schedule A + Schedule
> PN) + NC→NC_D400 wiring (`INDIVIDUAL_STATE_FORM_MAP`, `NC_D400_FEDERAL_PULL` = federal AGI 1040 L11 → D-400
> L6, `_populate_nc_d400_from_federal`, create + refresh dispatch — ⚠ also FIXED a latent AL40
> `refresh_from_federal` bug that fell through to the GA pull) + frontend picker/section-tabs
> (`NC_D400_SECTION_TABS`, isNcD400, SUPPORTED_STATES, D_NCD400_→State tab). **Render** ✅ FACE (flat NCDOR
> "handwritten" template `fnc_d400` — the leading "Do Not Include This Page" instructions cover STRIPPED via
> `delete_page(0)` so the stored 2-page template = the form face; pre-printed "00" anchors → two value
> columns pg0 right-483 / pg1 right-555 + line-12a left box + 20a/21a payment sub-boxes + line-13 Sch PN %
> box, render→PNG→measure-delta + visually verified) + page-0 identity header (name/SSN/address + filing
> circle X at x≈78). **Diagnostics** ✅ 8 `D_NCD400_*` rules (179LIMIT+MFS_STDDED warning; the rest info;
> AMENDED dormant-but-registered for spec parity). Tests: 14 pure compute + 4 state-return DB + 6 render + 8
> diagnostics, all green; flow gate 398. Commits `69cf82b`/`b31d5c7`/`c704f21`/`8358e74`. Template
> `nc_d400.pdf` (NCDOR handwritten, cover-stripped) sha-pinned in the manifest (`fnc_d400`). RS follow-up:
> NC_D400 spec is `draft` → promote to `active`. **v1 boundaries** (RS D_NCD400_*): amended return (L22/L24,
> Sch AM), Form D-422 est-tax interest, NC NOL (Sch S L39), Schedule PN-1 — direct-entry / not computed.

> **2026-07-05 (sixteenth session, part 2) — AL FORM 40 (Alabama individual) → ✅ FULLY COMPLETE
> across ALL LEGS (S-8); no migration, FormFieldValue-backed (GA-500/SC1040 pattern), form_code
> "AL40".** Built spec-first from the RS `AL_FORM_40` spec (status `draft`; TY2025). **Compute** ✅
> `compute_al40` (dispatched in `compute_return` for AL40) — income-from-scratch, the standard-
> deduction sliding scale as a formula, personal + tiered dependent exemptions, 2/4/5% tax
> (§40-18-5), and ★ THE ALABAMA QUIRK: the LIABILITY-based federal-income-tax deduction (L12 =
> max(0, (1040 L22 + 8960 NIIT) − refundable credits), part-year apportioned by AL AGI ÷ fed AGI).
> All 5 RS scenario pins verified. **Input** ✅ `seed_al40` (2 sections / 36 lines) + AL→AL40 wiring
> (`INDIVIDUAL_STATE_FORM_MAP`, create/refresh-from-federal, `_populate_al40_from_federal` pulls the
> FIT-worksheet federal facts SCOPED to the 1040 form) + frontend picker/section-tabs (`AL40_SECTION_TABS`,
> isAl40, SUPPORTED_STATES, D_AL40_→State tab). **Render** ✅ FACE (flat 2-page ALDOR template `fal40`,
> single value column, coordinate overlay, lines 5b–35 + two fidelity mirrors 29/32, visually verified) +
> page-1 identity header (name/SSN/address/filing-status X). **Diagnostics** ✅ 6 `D_AL40_*` info rules
> (FIT/STDDED/EXCLUSIONS/ATP/40NR/NOL; registered + seeded). Tests: 15 pure compute + 3 state-return DB +
> 5 render + 4 diagnostics DB, all green; flow gate 398. Commits `28ceeab`/`3edce72`/`938846c`/`941cd46`/
> `c81bcf2`. Template `al_form_40.pdf` (25f40blk.pdf) downloaded + sha-pinned in the manifest (`fal40`). RS
> follow-up: AL_FORM_40 spec is `draft`. **v1 boundaries** (RS D_AL40_*): Form 40NR nonresident, Schedule
> ATP (L19), Form NOL-85A, Schedule OC credits — direct-entry / not computed.

> **2026-07-05 (sixteenth session) — SC1040 Schedule NR RENDER LEG → ✅ SC1040 FULLY COMPLETE
> (S-7 all legs green); no migration, render-only.** Rendered the SC Schedule NR (part-year/
> nonresident) summary lines 31-48 as a coordinate overlay on the flat 2-page SCDOR template,
> appended behind the SC1040 face when the return is part-year/nonresident (SC1040 "PYNR" set);
> resident returns attach no Schedule NR. **New** `coordinates/fsc_schedule_nr.py`: two-money-column
> map (Col A federal x-anchor 473.4, Col B SC 571.5, value right-edge = anchor − 12.5 = the fsc1040
> face gap) + the two inline proration-% blanks (lines 45/47, right of the pre-printed "%"). Anchors
> auto-extracted from the flat PDF "00"/"%%" glyphs; baselines pinned RL_y = 792 − fitz_y1 + **3.5pt
> font-descent nudge** (measured value-vs-"00" delta −0.04pt after nudge). **renderer.py**:
> `render_sc_schedule_nr()` (PYNR-gated → None for residents; NR-45 fraction ×100 as "NN.NN") +
> register `fsc_schedule_nr` in COORDINATE_REGISTRY + append after the face in `render_sc1040`
> (GA-500 Schedule-1 append precedent). **v1 boundary** (matches `compute_sc1040._schedule_nr`): only
> summary lines 31-48; income-detail lines 1-30 attach blank (preparer enters the AGI totals on line
> 31). RS `SC_SCHEDULE_NR` spec (status `active`) cached to `server/specs/`. Tests **+6** (5 pure
> map/gate + 1 DB part-year 5-page attach: L48=47,800 @ 60.00% proration): render leg **10/10**;
> SC1040 compute 16 + state-return 4 + diagnostics 4 + **flow 398** = **422 passed**. Commit `d9fa2b0`.

> **2026-07-05 (fifteenth session) — SC1040 (South Carolina individual) — RESIDENT RETURN COMPLETE
> across FOUR legs; Schedule NR render is the only remaining sub-leg → S-7 NOT yet fully ticked.**
> New state form on the GA-500 pattern (FormFieldValue-backed, `seed_sc1040`, no migration).
> **Compute** ✅ `compute_sc1040` (dispatched in `compute_return` for form_code SC1040) — SC1040TT tax
> verified 138/138 vs the published SCDOR table (midpoint convention; the RS spec's "$2,533" pin was
> wrong — real = $2,360); §168(k)+§179 add-backs (SC pre-OBBBA $1.25M), retirement/military/age-65
> stack, 44% cap gain, embedded Schedule NR compute. **Input** ✅ `seed_sc1040` (5 sections / 81 lines)
> + `create-state-return`/`refresh-from-federal` SC wiring + frontend picker/section-tabs + the
> RED/YELLOW/GREEN color system (generic editor). **Render** ✅ FACE (3 pages, coordinate overlay,
> visually verified) + page-1 identity header; ⬜ **Schedule NR render remains**. **Diagnostics** ✅ 6
> `D_SC1040_*` info rules (registered + seeded). Tests: 16 pure compute + 4 state-return DB + 5 render
> + 4 diagnostics DB, all green; flow gate 398. Commits `307a810`/`a8cb291`/`81e0809`/`4cb497a`/
> `af2c6a7`/`13443d5`/`118613d`. Also fixed a latent `renewable_facilities` serializer bug (broke ALL
> full-return serialization since the 7-04 Form 8835 unit). RS follow-up: SC1040 spec is `draft` + carries
> the wrong $50k pin.

> **2026-07-08 (twenty-eighth session continuation) — FIRST LIVE ATS SUBMISSIONS (4 rounds), no
> coverage change.** No form leg moved; the e-file TRANSMISSION stack graduated from local-valid to
> live-proven. Rounds: (1) "Invalid mime format" → real MIME multipart transmission file per Pub 5446
> (`4a067f6`); (2) X0000-008 → Return root (`676f3ca`) then the REAL trigger, the envelope root
> (`30ea3b4`); (3) FilingSecurityInformation IND-189…203 (`2c36799`); (4) per-return content rules
> (round-5 queue in `docs/mef/ats_receipts.md` — dependent counts, CapDistribInd, S8 spouse data,
> AdditionalFilerInformation, ⚠ moving-expense/3903 Ken-ruling). MeF computed S5's refund = engine's
> $7,640 exactly. Also: 1120-S doc mappers 1125-A (`e7bebb9`) + 1125-E (`35e3b59`); S2/S3 whole-dollar
> re-pins (`53a229b`). No migration.

> **2026-07-07 (twenty-eighth session) — MeF SOR intake + ATS-active re-stamps + 2 engine fixes, no
> coverage change.** No form leg moved. (1) **MeF** (`199cfff`): 5 SOR packages intaken; 1040 mapper
> re-stamped 2025v5.4→**v5.3** and 1120-S 2025v6.3→**v6.2** (today's ATS-active versions; deltas not
> emitted); new `apps/efile/validation/business_rules.py` loads the BR publications (CSV + XLSX) so an
> ATS reject number resolves to text locally — the 1040 business rules are now IN HAND. (2) **Form 2441
> line 8** — the CDCC applicable percentage (a RATIO) was flattened to "0" by the session-27 whole-dollar
> sweep; fixed (same commit), caught by IRS2441.xsd during the v5.3 revalidation. (3) **Whole-dollar
> zero-clears** (`117cb68`): the s27 sweep left literal "0.00" clears (K2 rental / K7-K8a Sch D /
> disposition K9-K10-K8c-K9c-K15b-4-6) + a K2 cents quantize; fixed. Flow gate 422; pin suite +
> dispositions/4797 green. No migration.

> **2026-07-05 (thirteenth session) — e-file/diagnostics HARDENING, no coverage change.** Two latent
> bugs on already-complete forms, no leg status moved: (1) **Schedule 3 e-file leg** — `build_schedule3`
> now emits the lines 6z + 13z repeating groups (`OtherNonrefundableCreditsGrp` / `OtherRefundableCreditsGrp`
> + totals) that were silently dropped (`5d055e7`); (2) **Form 8867 diagnostics leg** — the AOTC covered-
> credit gate + attestation cascade unified onto one `compute_8863.aotc_claimed` signal (`8783487`). Both
> forms remain ✅ complete; these harden existing legs. No migration.

> **2026-07-04 (ninth session) -- FORM 8835 (Renewable Electricity Production Credit §45) →
> ✅ FORM 8835 COMPLETE (ALL FOUR LEGS TICK) -- migs 0165 (RenewableFacility + 2 Taxpayer
> passthrough facts) + 0166 (RLS), both applied to the SHARED DB.** Built from the cached RS
> `8835_spec.json` + Ken's 2026-07-04 J1-J4 scope rulings. **Model** (`RenewableFacility`,
> one row per §45 facility = one Form 8835 face; J1 multi-facility): field names = the spec
> fact keys; Taxpayer `f8835_passthrough_credit`/`f8835_passthrough_pis_date` (J4).
> **Compute** (`compute_8835.py`): year-keyed 2025 RATE table (Fed. Reg. 2025-09366 — $0.006
> tier-1 / $0.003 tier-2 / hydro-marine post-2022 $0.006 / pre-2022 3.0¢-1.5¢; 2026 unpinned
> → D_8835_RATE_YEAR), the OBBBA before-2025 begin-construction gate (blank date proven by a
> pre-cutoff PIS — the 8936 convention), the Part II chain (2-13 per facility: kWh×rate, bond
> 15% cap, ×5 increased, +10%/+10% bonuses, 90% EPE), `form_8835_state` (THE shared gatherer),
> `form_8835_credit() -> (amount, "1f"|"4e")` (the pinned 3800 wiring, LIVE), `compute_8835_db`
> (FORM_8835 face rows, disengage). Runs BEFORE `compute_3800_db`; lands NOTHING on Sch 3 (all
> via the 3800 §38 limitation). **Routing** (R-8835-ROUTE, J3 binary-per-year): whole year in
> the 4-yr PIS window → 4e; window ended → 1f; PIS-in-year → 4e; window ends mid-year →
> straddle RED-defer. **Diagnostics** (`rules_8835.py`, 9 rules): spec 001-004 + RATE_YEAR +
> the J2/J3/J4 RED-defers **005 mixed / 006 straddle / 007 passthrough-unrouted / 008
> missing-PIS** (all ≤20 chars, error, bridge-gated on `form_8835_state`; escape hatch = the
> 3800 1zz/4z facts). **Render**: f8835.pdf downloaded (seq 835, Cat 14954R, sha pinned) +
> `f8835_2025` LABEL-VERIFIED bijective field map (97 widgets; dead: line-7 reserved pair, 10
> table slivers, line-3 sub-entries) + `render_8835` (ONE FACE PER FACILITY via
> `facility_lines`; lines 14/15 first-face) + 3-page PNG visual pass + packet emit before 3800.
> **UI**: `RenewableFacilitiesSection` tab (Credits group, before 3800) + the two Taxpayer
> passthrough inputs + D_8835_ → form_8835 routing; tsc clean. **GATES**: pure compute
> **19/19** (first run) · DB pipeline **8/8** (S4 4e / T4 1f / all-carries W4 / mixed / straddle
> / passthrough / OBBBA / disengage) · diagnostics **10/10** · render **6/6** · **FA-1040-8835-
> 01..06** in the gate file (transcribed verbatim from the RS canonical set) → **flow gate 397**.
> RS Supabase reseeded (9 diags / 6 FAs); cached `8835_spec.json` refreshed from the live
> deployed export (5→9 diags). Shared DB migrated (0165/0166) + seeded (seed_8835 14 lines,
> seed_rules); test DB migrated through 0166. Tag `1040-form-8835-complete`. **NEXT: the S4
> scenario + mapper leg** (⚠ reconcile the Transfer Election Statement vs 4a=No — a transferred
> credit never lands on Sch 3; the blessed all-carries shape: 8936 personal absorbs the tax →
> 3800 line 38 = 0, Sch 3 6a blank, the whole $13,200 carries).

> **2026-07-04 (eighth session, part 2) -- FORM 3800 (General Business Credit §38) →
> ✅ FORM 3800 COMPLETE (ALL FOUR LEGS TICK) -- the FULL spec-first round-trip in one
> session (mig 0164).** Ken: "should you go ahead and build the 3800?" → the deployed RS
> spec was a thin 1120-S-era draft → **RS 1040-side AMENDMENT authored** (`load_1040_form_
> 3800.py`, AMEND-BY-LOOKUP — entity_types + R001-R003 kept, R004 refreshed, the stale
> 1a/1b/1c sketch deleted; RS commit `5407bb2`), Ken's **scope walk J1-J4** (built-feeder
> Part III rows 1f/1s/1y/1aa + 1zz/4z catch-alls · passive = per-inflow TRI-STATE assertion
> · carryforward = two in-buckets + computed out · §6417/§6418 out) + **review walk W1-W4**
> (incl. the blessed S4 all-carries outcome) → seeded → deployed export verified → canonical
> `3800_spec.json` cached. **tts unit**: Taxpayer `f3800_*` facts (mig 0164) ·
> `compute_3800.py` (Part III inflow gathering bridge-gated to `form_8911_business_credit` +
> the new `form_8936_business_parts`; the 8835 wiring point stubbed for the next unit;
> §38(c)(1) Section A verbatim — line 10b = 1040 L19 + Sch 3 2-4/5a/5b + 6x excl 6a/6b/6k;
> **§38(c)(4)(A) Section C with TMT ZEROED for specified credits**; line 38 = 28+37 → **Sch 3
> line 6a**, computed LAST among the nonrefundable credits) · 6 diagnostics (D_3800_001-006)
> · seed_3800 (42 lines) · **subset field map** over the 9-page face (semantic grid subforms
> Pg3Table.Line1f… verify the rows; all 9 pages print per the face mandate) + render_3800 +
> **carryforward STATEMENT page** + PNG visual pass · Business Credit (3800) UI tab (passive
> selects + catch-alls + CF-ins + escape hatches) · FA-1040-3800-01..05 (authored in the RS
> loader AND the tts gate — no drift). **RETIREMENTS**: D_8911_004 → is_active=False stub
> (8911 line 3 lands on row 1s); D_8936_003 → softened to the spec's info (8936 lines 8/21
> land on 1y/1aa); FA-8911-04/FA-8936-02 runners repinned to the new reality. **FA-DRIFT
> FIX**: the five FA-1040-8936-* tts entries replaced with the RS-canonical definitions (the
> RS loader had authored those ids first). **GATES**: pure compute **9/9** · DB pipeline
> **6/6** (incl. the end-to-end W4 ordering pin: 8936 personal absorbs the tax → the whole
> 13,200 specified credit carries, 6a blank) · render leg **5/5** · **combined flow gate +
> all 3800/8936/8911 legs = 451 passed** (reuse-db 7:04). Shared DB migrated (0164) + seeded
> (seed_3800, seed_rules — retirement/softening verified live); test DB migrated. NEXT: the
> **Form 8835 unit** (spec cached; implements `form_8835_credit() -> (amount, "1f"|"4e")`),
> then the S4 scenario.**

> **2026-07-04 (eighth session) -- FORM 8936 (Clean Vehicle Credits §30D/§25E/§45W) →
> ✅ FORM 8936 COMPLETE (ALL FOUR LEGS TICK) -- migs 0162 (CleanVehicle + 4 Taxpayer facts)
> + 0163 (RLS).** Spec-first from the cached RS `8936_spec.json` + `8936_scha_spec.json`
> (authored 2026-07-04; the S4 arc). **Model**: `CleanVehicle` one-Schedule-A-per-vehicle
> (type new/used/commercial, OBBBA `acquired_date`, transfer election, per-part inputs) +
> Taxpayer `clean_vehicle_magi_prior`/`_prior_filing_status`/`_new_k1_credit`/
> `_commercial_k1_credit`. **Compute** (`compute_8936.py`, TWO-PHASE): phase 1 REPAYMENT —
> a dealer-transferred credit DENIED (MAGI over the cap BOTH years, per-year status caps;
> or 30-day resale) repays on **Schedule 2 lines 1b/1c → 1040 line 17** BEFORE every
> credit-limit worksheet (the 8962 excess-APTC reflow precedent); phase 2 CREDITS after
> compute_5695 (lines 11/16 read Sch 3 5b) and before EIC/8812/8911 (whose CLWs read
> 6f/6m) — Part IV computes BEFORE Part III (line 11 INCLUDES the line-18 used credit,
> the i8936 WALK-ITEM-C asymmetry) → **Sch 3 6f (new personal, line 13) / 6m (used, line
> 18)**; unused personal portion LOST. **OBBBA gate face-verified** (i8936 What's New
> verbatim): no credit for a vehicle ACQUIRED after 9/30/2025 (absolute date, not
> year-keyed — the acquired-before/PIS-later tail claims in its PIS year); blank acquired
> date proven by PIS ≤ cutoff, else excluded + D_8936_001. **FACE-VERIFIED transfer stop**
> (f8936sa 8c/8d + 13b/13c verbatim): a TRANSFERRED credit was received at the point of
> sale — a qualifying transferred vehicle contributes NOTHING to the face (never
> double-claims); RS spec silent → amendment flagged (STATUS). Business (line 8 → 3800
> 1y) + commercial (21 → 1aa) PARK behind **D_8936_003 interim ERROR** (spec info; the
> D_8911_004 pattern — soften when 3800 lands). **Diagnostics** 7 (OBBBA/MAGI/3800/
> VIN-PIS/tax-limited-LOST/used-price/seller-report). **Render**: f8936 + f8936sa (3-page
> SchA) manifest-registered (sha256 vs fresh irs.gov downloads), LABEL-VERIFIED bijective
> maps (31 + 65 widgets; the $4,000 line-16 sliver dead), render_8936 (main + one SchA per
> vehicle, Yes/No chains follow the face's stop logic; 8b/8e/13f/18b/18c auto-Yes only
> when the vehicle proceeds — stated boundary), **rendered-PNG visual pass done** (the S3
> forensics rule). **Input**: CleanVehiclesSection tab (type-driven fields) + prior-year
> MAGI/status + K-1 cards; D_8936_ → form_8936 routing; tsc clean. **GATES**: pure compute
> **18/18** · DB pipeline **8/8** (incl. repayment→line-17, the 6m-squeezes-6f ordering
> pin, disengage, render smoke) · render leg **10/10** · **FA-1040-8936-01..05** in the
> gate file (⚠ need RS loader homes) · **combined flow gate + all four legs = 422 passed**
> (reuse-db 2:54). Shared DB migrated + seeded (seed_8936 25 lines, seed_rules); test DB
> migrated through 0163. NEXT: Form 8835 unit (spec cached) → Form 3800 (⛔ RS spec is a
> thin 1120-S draft — Ken authors) → the S4 scenario.**

> **2026-07-04 (seventh session) -- ATS SCENARIO 3 (Lynette Heather) SCENARIO + MAPPER LEG
> COMPLETE ✅ -- 4 NEW MeF mappers + the Schedule D 1a/8a aggregate compute wire.** No
> migration. **Mappers** (each 1:1 vs its 2025v5.4 XSD, ReturnData positions 934/941/955/1263):
> `IRS1040ScheduleD` (QOF ind + the 1a/8a `TotalST/LTCGL1099BssRptNoAdjGrp` groups + combine
> spine 7/15/16 + face-routing 17-22; **v1 = the aggregate path ONLY** -- 8949 box totals
> raise, IRS8949 doc mapper unbuilt), `IRS1040ScheduleE` (**Part V farm-rental leg only** --
> A/B booleans + 40/41/42/43; Parts I-IV raise), `IRS1040ScheduleF` (identity + cash-method
> income/expense groups; MateriallyParticipatedInd required; accrual/passive raise),
> `IRS4835` (max 4; income/expense + aggregate-"Other" 30a row + Section 263A negative row +
> Section263AIndicatorCd; 34c signed on loss; PAL literal attr). **Compute wire:** the
> SCHEDULE_D 1a/8a rows (seeded since Topic 9, labeled "UNUSED v1") now enter the netting per
> the spec's own R-SCHD-L7-L15 ("L7 = combine 1a..6; L15 = combine 8a..14") -- direct-entry
> (d)/(e), computed (h), engagement extended, field map Row1a f1_3/f1_4/f1_6 + Row8a
> f1_23/f1_24/f1_26 (widget-verified). **Scenario:** `scenario3.py` + `mef_build_ats_scenario3`
> + `test_mef_scenario3_compute.py`; the SE FARM OPTIONAL METHOD verified already emitted
> (4b/15). Key-forensics corrections (rendered-PNG visual pass -- the text decoders missed the
> Sch F fills): 2,970 = Sch F line 26 SEEDS AND PLANTS (not supplies); PEC You checked; Sch E
> A/B both Yes; line 38 blank -> the i1040 IRS-figures-the-penalty election (FFV override --
> compute_2210 has no 6654(e)(2) arm, stated in DEFERRAL_AUDIT). **GATES:** command run ALL
> MATCH (income 72,235 / AGI 71,821 / 13a 567.40 / TI 55,504 / L16 6,328 / SE 827 / owe
> 3,750, every pin engine-verified) / 11-doc submission + manifest live-XSD-valid / pure
> efile 50/50 / combined DB gate (flow assertions + acroform maps + the S3 file) **440
> passed** (16m reuse-db; 1 test-shape fix re-run green). Artifacts UNSIGNED
> (`docs/mef/ats_out/scenario3`) -- Ken signs + uploads. NEXT: S4 needs 8835 + 8936 built
> first (RS specs cached), then the S4 scenario.**

> **2026-07-04 (sixth session) -- FORM 4835 (Farm Rental) RENDER LEG -> ✅ FORM 4835 COMPLETE
> (ALL FOUR LEGS TICK) -- commit `cee838f`.** No migration / no compute change (render-only).
> **Field map** `apps/tts_forms/field_maps/f4835_2025.py`: 63 AcroForm widgets, all on PDF page
> 0, LABEL-VERIFIED bijective vs the official PDF (scratchpad `label_verify_4835.py` — same-row
> printed-text extraction; the map covers every widget exactly once, asserted). Two-column Part
> II decoded: left col (Part2_LeftCol subform) f1_17-30 = lines 8-20, right col f1_31-58 = lines
> 21-30g + totals; 30a-f = desc+amount pairs; 30g §263A = desc f1_53 + amount f1_54; Line A
> Yes/No = c1_1[0]/[1]; 34a/34b = c1_4[0]/[1]; EIN header = Comb[0].f1_03 (comb, 9). **Manifest**
> `f4835` entry added — **sha256 `8ecc6172…` VERIFIED against a fresh irs.gov download**
> (genuine 2025 template, Attachment Seq 37, Cat 13117W). **render_4835()** (renderer.py after
> render_schedule_f_1040): one copy per Form4835 activity, MODEL-DRIVEN through the pure
> `compute_4835_lines` chain (the bridge-gate). Line 7 gross + line 31 total always print; net
> line 32 signed (parens on loss); on a loss the 34a/34b at-risk box + line 34c print; §263A on
> 30g prints parenthesized; **matpart activity -> NO face** (belongs on Schedule F). Registered
> in `ACROFORM_FORM_IDS`; packet emit after Schedule F; `SKIP_PAGES["Form 4835"]={1,2}` strips
> the two instruction pages so each activity = one page. **v1 render boundary:** the model stores
> the six 30a-f "other expenses" as a single aggregate -> printed on line 30a labeled "Other"
> (does not itemize the six specify rows; DEFERRAL_AUDIT). **Header** = the 1040 filer name(s) +
> SSN (the render_8283 pattern). **GATES:** `test_4835_render_leg.py` **11/11** (bijective map +
> map-covers-every-line no-DB trip-wires; income face sweep incl. Line-A box; multi-line L7 sum
> vector; loss parens + 34c + at-risk box; §263A parenthetical; matpart no-copy; two-activity
> two-copy; packet one-page). Combined **flow-assertion gate + all four 4835 legs = 412 passed**
> (reuse-db 15m). NEXT: the S3 (Heather) scenario + mapper leg — all forms now built.**

> **2026-07-04 (fifth session) -- FORM 4835 (Farm Rental) COMPUTE/DIAGNOSTICS/SEED LEGS --
> commit `c7cae44`, mig 0161.** Parallel RS session authored all four blocking specs
> (4835/8835/8936/8936_SCHA export 200); this session built the 4835 unit spec-first.
> **compute_4835.py**: gross L7 = 1+2b+3b+4a+4c+5b+5d+6 (spec includes the L4a CCC-election +
> L5d, resolving my authoring-note [VERIFY]); L31 expenses with §263A on 30g reducing; net L32;
> **FULL loss path** — §465 at-risk (Form 6198) BEFORE §469 passive (Form 8582 $25k active-
> participation allowance with the MAGI>100k phaseout) → line 34c + suspended PAL. Multi-
> instance (T12): Schedule E line 40 = Σ each activity's net/loss. **Model Form4835** (mig 0161,
> fields 1:1 with spec fact keys). **Wiring**: compute_4835_db runs before compute_schedule_e_db;
> a Form4835 now ENGAGES Schedule E (was the S3-flow bug — 4835-only returns disengaged); line
> 42 (gross) added to _SCHE_P2_OUTPUT_LINES (seeded-but-never-written); line 41 folds in line 40
> → Sch 1 L5. **NOTHING to Schedule SE** (§1402(a)(1), the defining invariant). **rules_4835.py**
> 9 diagnostics (MATPART/SE_GUARD/LOSS_LIMITED/CCC_ELECT/CROPINS_DEFER/263A/REPRO/SCHJ/QBI);
> retired the stale `D_SCHE_4835` "not built" RED. **seed_4835** (FORM_4835, 52 lines). QBI is
> preparer-asserted (default NOT QBI — no auto 8995/8995-A feed). **GATES**: pure compute
> **13/13** (spec vectors T1-T12+S3) · integration **2/2** (S3: Sch E 40=11,061 / 42=17,035 /
> Sch 1 L5=11,061, none to SE) · diagnostics **5/5** · flow gate **HELD (401 passed)**.
> ⏳ REMAINING for the form to TICK: render leg (f4835 AcroForm field map — template downloaded,
> 63 fields, not yet committed) + the S3 scenario/mapper. Then S4 (8835/8936/SchA + scenario).**

> **2026-07-04 (fifth session) -- BROKERAGE 1099-B SUMMARY-EXTRACTION SKELETON (8949
> Exception 2) -- commit `c25635f`, mig 0160.** Pivot away from the blocked S3 (Form 4835
> has NO Rule Studio spec — 404, RS server confirmed up via 8283 200; STOPped per the
> mandatory RS rule rather than improvise. S4's 8835/8936 are also 404; only 3800 has a spec.
> Ken ruled "pivot to an unblocked unit"). Delivered the season-one brokerage import boundary
> (SEASON_PLAN item 4). **Leg A** — `compute_schedule_d` now auto-applies code M to every
> `is_summary` row (RS `R-8949-SUMMARY` "code M auto" was UNIMPLEMENTED; this is the single
> source consumed by `render_8949_1040` col (f), the D_8949 diagnostics, and the e-file).
> **Leg B** — new `apps/returns/brokerage_1099b.py::import_brokerage_summary()` maps per-box
> category totals → Exception-2 `CapitalTransaction` summary rows (description "broker — see
> attached statement", blank b/c, code M, statement_attached); idempotent per broker;
> `provenance=IMPORTED` (YELLOW) + `import_confirmed` mandatory-confirm gate (SEASON_PLAN
> item 5). Model + mig 0160 add `provenance`/`import_source`/`import_confirmed` (mirrors the
> existing `DataProvenance`; moved that enum above `CapitalTransaction`). New diagnostic
> **D_8949_006** (warning; trip-wire 16→17). The OCR/AI PDF front-end plugs in ABOVE this seam
> (August "extraction to production"); frontend YELLOW rendering is likewise deferred — the
> skeleton stops at the backend normalized-payload boundary by design. **GATES:** new
> `test_brokerage_summary_skeleton.py` **8/8** (create · idempotent+spares-manual · empty-skip ·
> bad-box/amount reject · flow to Sch D L16 / 1040 L7 · code-M auto · D_8949_006 gate · render
> smoke); flow **381 HELD**; topic9 compute + diagnostics leg GREEN. **Follow-up:** add
> D_8949_006 to the 8949 RS spec on the next Schedule D touch (app-level workflow gate, not
> silently written to the cached JSON).**

> **2026-07-03 (fourth session) -- 8995-A SCHEDULE D DB GATE CLEARED ✅ (unit TICKS) + S2
> (JONES) SCENARIO + MAPPER LEG BUILT -- commits `89a4f88`/`2a151b5`/`c10e51c`/`9b5dcad`,
> mig 0159.** **8995-A DB legs:** three pooler-fought runs (25/20/16 min; 21 connection-kill
> E/F's total) — every test in the five leg files GREEN on current code; 4 real failures were
> ALL seeder/test-side (the spec's 7 SD lines were never in `seed_8995a`'s hardcoded SECTIONS
> list → new f8995a_schd section, 58 lines / 8 sections, reseeded on the SHARED DB with
> `seed_rules`; the patron-delta test missed the §199A income-limit interaction — engine
> 48,013/53,863 hand-verified CORRECT; a title line-wrap; two stale 51-counts). Engine math
> untouched. **S2 scenario leg:** full key forensics (+29 char-shift fills; [l]-path vector
> checks + ✔ glyphs; the key fills INPUTS ONLY — computed lines blank; **Sch A line-18
> itemize-anyway CHECKED**, 27c checked, statutory box checked, dependent col (7) blank =
> fiat #2). **FOUR NEW MAPPERS** 1:1 vs 2025v5.4 XSDs: IRS1040ScheduleA (5a sales ind,
> line-11 `qualifiedContributionsAmt` attr = the dotted "$200" — new Taxpayer field mig 0159
> + abs_pos face literal + FormEditor input; line-12 → IRS8283 ref; `ItmzdDedLessThanStdDedInd`;
> 15/16 guards raise — 4684/misc-statement unbuilt), IRS8283 (Section A complete, one document,
> all rows; **Section B raises** = wet-ink J7 boundary; col (e) → YYYY-MM|VARIOUS), ReturnHeader
> `PaidPreparerInformationGrp` (from PreparerInfo, after Filer), Sch C `AdditionalVehicleInfoGrp`
> (43-47b — boundary retired; 47a/b = the EXISTING has_evidence/evidence_written fields).
> scenario2.py + mef_build_ats_scenario2 + NRA statement PDF (the first live BinaryAttachment
> consumer). **ENACTED PINS ALL MATCH the engine** (hand-computed first): 1a 8,513 (statutory
> wages off 1a) / Sch C 31 = 26,979 no-SE / 12 = 22,201 ELECTED / **13 = 2,658.20** (the
> 8995-A income-limit arm binds — the 1040 line-13 FFV carries CENTS; 15 = 10,632.80 → table
> 1,063) / 19 = 500 ODC / 24 = 563 / 26 = 300 divorcedSpouseSSN / refund 901 / 27 CLEARED.
> The 11-document submission (1040, Sch 1, 8812, Sch A, Sch C, 8283, 8995-A, 8995-A Sch D,
> W-2 ×2, BinaryAttachment) validates vs the LIVE 2025v5.4 XSDs; artifacts in
> docs/mef/ats_out/scenario2. 8995-A pins ride the XML (engine-result-driven, no FFV rows —
> the 7217 pattern). Test file **29/29 GREEN** (--timeout=1800 — the 13-seed fixture outlasts
> the 300s cap; 2 helper-case expectation fixes: name_line1/business_name_line keep entry
> case). Gates: flow 381 · efile 31/31 · acroform 33/33 · tsc 0 · vitest 275. Detail →
> STATUS.md + DEFERRAL_AUDIT (fourth session).**

> **2026-07-03 (third session) -- FORM 8995-A SCHEDULE D (patron reduction §199A(b)(7) + capped
> DPAD §199A(g)) -- ★★ BUILT, ALL PURE GATES GREEN, 🔴 DB LEGS POOLER-BLOCKED (no tick until
> green) -- SPEC-FIRST RS round-trip -- flow gate unchanged 381 (FA-08 amended in place).**
> **Ken RULED (AskUserQuestion): "Is this a MeF test scenario? If so build 8995-A Schedule D"**
> — retired the stated 8995-A patron/DPAD RED-defer for the S2 (Jones) leg. **SPEC** (RS
> `9b4490c`, amend-by-lookup): +`a_business_schd_qbi_alloc`/`_wages_alloc`/`a_dpad_199ag`,
> R-8995A-PATRON reauthored routing→calculation (SD2–SD6 → L14), SCOPE/P2-COMP/P4-LIMIT amended
> (patron → 8995-A at ANY income per i8995a 2025 verbatim; face line-3 skip at/below threshold;
> L15 floor; **L38 cap = L33 − L37 face verbatim**), +7 SD lines, **D_8995A_001/002 REPURPOSED
> error→info (Ken ruled)** + NEW 008/009 guards, T9 amended + T11–T15 HAND-COMPUTED (T13 = the
> S2 below-threshold zero-wage patron — proves the wage limit does NOT zero it; T14 = full
> allocation + zero wages → reduction still $0, the lesser arm); new source IRS_8995A_SCHD_FORM;
> integrity checker extended, 15 scenarios ALL PASS; export drift = exactly the amendment.
> **MODEL** mig 0158 (APPLIED). **COMPUTE** `f10e864`: engine per-column L14/SD chain + skip +
> cap; `form_8995a_patron_engaged` = THE single bridge-gate (router/render/extract/diags);
> `compute_8995a_db` returns the result dict; D_8995_001 retired-dormant. **INPUT** serializers
> `__all__` (auto), FormEditor conditional patron-alloc fields (Sch C + F). **RENDER** f8995ad
> Rev 12-2022 registered (manifest+SHA) — 23-widget map BIJECTIVE; render_8995a fills
> 1{col}_patron + L14/L38 and inserts Sch D copies at page 2 (3 patron columns/copy). **MeF**
> `2c6e8a1`: F8995ASource re-derived via the render gates; IRS8995A + IRS8995AScheduleD
> builders (full XSD-order maps; patron-anchored zeros STATED; SSTB/Agg = XSD CHOICE);
> ReturnData slots after IRS8995 (XSD 2224/2252); efile 31/31 incl. LIVE XSD of the pair.
> **⚠ S2 ENACTED-PIN DIVERGENCE (S13-class):** the key's blank 13a is scenario fiat — enacted
> law computes NONZERO 13a + attaches the QBI documents the key lacks. 🔴 CARRIED: the DB legs
> (3 pooler-sick attempts, zero real failures) + prod `seed_rules` + the RS 8995 sibling
> loader's D_8995_001 retirement note (DEFERRAL_AUDIT). Detail → STATUS.md + RS session_log.

> **2026-07-03 (second session) -- S2 RETURN-LEVEL ARMS (identity/header + EIC opt-out 27c) --
> ★★ BOTH UNITS COMPLETE -- commits `86d4e38` + the 27c unit -- flow gate 380→381.** The
> DEFERRAL_AUDIT item-10 S2 gap list, corrected and closed. **STALE-LIST FINDINGS:** the
> statutory-employee Sch C arm was ALREADY BUILT (W-2 Unit 2, 2026-06-15 — the
> federal-backlog-already-built lesson); the former-spouse SSN needed no statement (the XSD's
> `divorcedSpouseSSN` ATTRIBUTE on line 26). **UNIT A (identity arms, `86d4e38`):**
> deceased-spouse header — c1_3 + the MM/DD/YYYY entry spaces f1_05-10 render from the
> mig-0049 date fields (i1040 "Death of a Taxpayer" verbatim), Pub 4164 §12.5 DECD name line
> (all four cases; S2 = `JOHN A & JUDY DECD<JONES`), the i1040 "Filing as surviving spouse"
> literal via the NEW `AcroField.abs_pos` overlay mechanism (widget-less face text), IRS1040
> DeceasedInd/PrimaryDeathDt/SpouseDeathDt · NRA-spouse §6013(g)/(h) election (mig 0155;
> spine fact was ALREADY SPECCED sort-5) — c1_9 + f1_30 render + NRASpouseTreatedAsResidentGrp
> + TaxpayerInfo checkbox · ReturnHeader IdentityProtectionPIN/SpouseIdentityProtectionPIN ·
> BinaryAttachment document builder + binaryAttachmentCnt (package.py already carried
> /attachment) · line-26 divorcedSpouseSSN. Gates: efile pure 29/29 incl. LIVE XSD validation
> of the full arm set, header-render DB 5/5, map audit, flow 380, tsc, vitest 275. **UNIT B
> (EIC opt-out 27c, spec-first):** **Ken RULED "skip-entirely, ACTC-sibling"**
> (AskUserQuestion) — RS 1040_EIC amended (`e64dbea`: fact `eic_opt_out`, R-EIC-27A gate,
> D_EIC_017 info, scenario EIC-G6, FA-1040-EIC-10 loader-homed; seeded, deployed export
> verified +1/+1/+1 zero-removal drift, canonical eic_spec.json refreshed 16→17 tests) →
> tts mig 0156 + `eic_engaged`/`compute_eic` gates (the EXPLICIT line-27 clear kills the
> stale-value hazard; the legacy eitc_claimed spine write precedes it) + Schedule EIC render
> suppression + c2_13 box + DoNotClaimEICInd + EIC-tab checkbox + D_EIC_017 (seed_rules run).
> Gates: flow **381** (FA-1040-EIC-10 runner arm added) · topic7 compute+diagnostics DB green
> after one 42-min pooler-sick run (15 fixture ERRORs = the documented connection-kill class;
> all re-ran green) + 3 real test-shape fixes (family pin 16→17, the cached-taxpayer flip
> mutation, the `_fires` handler collision) · efile 29/29 · vitest 275 · tsc. NEXT: the S2
> (Jones) scenario + mapper leg — needs the Sch A/C fill forensics (the +29/−29 decode is in
> the scratchpad notes) and ONE Ken question: the ag-co-op-patron "no QBI" scenario note vs
> the engine computing QBI on a statutory-employee Sch C (§199A patron reduction = the 8995-A
> RED-defer). Detail → STATUS.md + DEFERRAL_AUDIT 2026-07-03 (second session) + RS session_log.

> **2026-07-03 -- FORM 8283 (noncash charitable contributions, §170(f)(11)) -- ★★ UNIT
> COMPLETE, ALL FOUR LEGS GREEN -- tag `1040-form-8283-complete` -- SPEC-FIRST RS round-trip --
> flow gate 375→380.** ATS Scenario 2's tax-law form, smallest-first per Ken (S2→S3→S4). The
> SEASON_PLAN appendix-4 OBBBA check ran AT the spec leg: §170(f)(11)/(f)(12) untouched by
> P.L. 119-21; the $500/$5,000/$500,000 tiers statutory + non-indexed; the Rev. 12-2025
> form/instructions current (fetched verbatim). **SPEC**: RS `load_1040_form_8283.py` (RS
> `0748f06`) AMENDS the shared 1120S/1065/1040 stub BY LOOKUP (entity_types preserved; the
> 1120s-era placeholder 10 facts/5 unnamed rules/SA-* lines/D001-D003/2 tests RETIRED) — 51
> facts / 5 rules / 49 lines / 13 diagnostics / 13 HAND-COMPUTED scenarios / 5 loader-homed FA;
> sources USC_26_170F11 (verbatim) + IRS_2025_8283_INSTR refreshed to Rev. 12-2025 verbatim;
> `check_8283_integrity.py` ALL PASS. **Ken's walk (AskUserQuestion): J6 RULED "warn only,
> feed anyway"** (substantiation REDs — no appraisal §170(f)(11)(C), vehicle CWA §170(f)(12),
> attach-tier — fire while the amount STILL FEEDS line 12; the withhold recommendation
> rejected); J2 default-50%-bucket + bind-gated D_8283_008 and J4 conservation RED-defer
> (the ONE feed withhold) as recommended; J1/J3/J5/J7 + stub retirement approved.
> **MODEL** `NoncashContribution` (one row per item or per similar-items GROUP — J5; migs
> 0153 + RLS 0154, APPLIED). **COMPUTE** `compute_8283.py` (`row_analysis`/`noncash_summary`
> = THE bridge-gate); Schedule A line 12 buckets DEFAULT to the row totals (50% ordinary /
> 30% capgain — YELLOW) with the flat facts as per-field GREEN overrides (R-8283-SCHA12,
> the 5a feeder pattern); `charitable_noncash_inputs` shared with the diagnostics;
> D_SCHA_004 repointed to the resolved inputs. **INPUT** serializer + Meta.fields (the
> 7217-hotfix lesson honored) + 2 CRUD actions (recompute-on-mutation) +
> `NoncashContributionsSection` (rows grid + expandable Section-B/appraiser/donee detail)
> on the schedule_a tab; tsc 0 / vitest 275. **RENDER** f8283 Rev-12-2025 registered
> (manifest+SHA256): 117-widget field map LABEL-VERIFIED bijective vs the PDF; `render_8283`
> — Section A 4-row grid w/ overflow copies + ONE Section B copy per item (per-donee-per-item
> rule); packet gated on engaged (total > $500; the feed flows regardless). **13 D_8283_***
> (rules_8283, seed_rules run — 13 registered). **GATES**: 16 pure (13 scenarios + round-trip
> smoke) + flow gate **380** + **DB pipeline 6/6 (101s)** incl. the S2 pin (line 11 250 /
> 12 700 / 14 950), GREEN-override 650, conservation-withhold, sub-$500 feeds-without-form,
> J6 warn-only, and the detail-payload serializer net + `manage.py check` clean. Boundaries →
> DEFERRAL_AUDIT 2026-07-03 (10 items incl. the S2 MAPPER-leg gaps: deceased spouse, NRA
> statement, IP PIN, former-spouse SSN, STATUTORY-EMPLOYEE Sch C, EIC opt-out 27c). NEXT:
> the S2 mapper needs those return-level arms first. Detail → STATUS.md + RS session_log.

> **2026-07-02 (eighth session) -- SCHEDULE SE R-SE-ROUND: per-line whole-dollar rounding --
> ★ UNIT COMPLETE, SPEC-FIRST RS round-trip -- flow gate unchanged 375 (FA-1040-SCHSE-02
> amended in place, zero id drift 353=353).** Resolved the S12 REVIEW_QUEUE item same day:
> **Ken ruled per-line whole-dollar** (the engine cents-chained SE — 12 = 3,437.44 — diverging
> $1 from the ATS key/TaxWise 2,786 + 652 = **3,438** and printing 10+11 ≠ 12). **SPEC**: i1040
> "Rounding Off to Whole Dollars" fetched + quoted VERBATIM (new excerpt on IRS_2025_1040_INSTR;
> the WebFetch paraphrase was wrong again); NEW rule **R-SE-ROUND** (multiplication lines
> 4a/5b/10/11/13 round their product half-up; sum lines add already-rounded entries; L2 sums
> cents → rounds the total); SE-T1..T5 re-pinned (T5 load-bearing: 4,238 vs cents 4,238.87);
> `check_topic8_integrity` per-line ALL PASS; seeded RS Supabase (rules 13→14), deployed export
> verified, canonical `schedule_se_spec.json` refreshed (RS `37f21b3`). **COMPUTE**:
> `compute_schedule_se_lines` per-line `_qd` (entered lines round at entry; the $400/$100 floors
> compare rounded values); minister + the MeF SE extractor ride the same helper automatically;
> Form 7206 consumes the now-whole ½-SE untouched (`_r0` already whole-dollar). **RE-PINS**:
> topic8 compute/input/diagnostics + Schedule F pure + FA runner (SCHSE-01 half-up dollar,
> SCHF-08 source pin) + **S12 re-pinned to the engine** (½-SE 1,719 → AGI 122,445.00 / QBI
> 4,321.80 / tax 17,436.10 / owe 6,430.10 — **every whole-dollar XML value unchanged**: tax
> 17,436 / total 20,874 / owe 6,430; SE line 12 now MATCHES the IRS key). **AUDIT (the ruling's
> companion)**: 7206 already per-line; Schedule C sums-only (no action); **8995 ×20% lines
> (5/9/14) still cents → flagged, and **Ken RULED "build it now" SAME SESSION: R-8995-ROUND
> BUILT as the identical unit** (RS `bfe1d4f`; ×20% lines round their product, entries at
> entry, 1i-1v face rows whole; 8995-T2 re-pinned 9,952; `compute_8995_lines` per-line `_qd`;
> S12 re-pinned AGAIN — QBI **4,322** whole → taxable 102,373.00 / tax 17,436.06 / owe
> 6,430.06, whole-dollar XML still unchanged; 8995-A + 8959 stay cents-chained = stated
> boundaries, DEFERRAL_AUDIT). **GATES**: flow 389 ×2 · efile pure 23/23 ×2 · spec-scenario 16
> · DB 96 passed (+3 pooler connection-kills re-run green) + the 8995-leg DB re-confirm ·
> final `mef_build_ats_scenario12` ALL MATCH + XSD-valid, artifacts rebuilt →
> `docs/mef/ats_out/scenario12/`. **🔧 HOTFIX riding the commit: `partnership_distributions`
> was declared on TaxReturnSerializer but missing from Meta.fields (7217 unit) → every
> return-detail GET 500'd since `b665e55`; caught by this session's DB sweep, one-line fix.**
> Detail → STATUS.md + RS session_log.

> **2026-07-02 (seventh session) -- MeF ATS Scenario 1040-12 (Sam Gardenia) through the REAL
> engine + SIX new e-file document mappers + the W-2 box-12/state groups -- ★★ UNIT COMPLETE --
> flow gate unchanged 375 (pure serialization; the one compute-file touch is the ADDITIVE
> reader `compute_schedule_c.form_7206_rows` — no math change).** The Schedule C family
> scenario (Single, KY; DESIGNER "ENERGY BUILD" sole prop; SEHI 1,000; a no-gain 7217
> distribution). **MAPPER**: read_model + builder grew **IRS1040ScheduleC** (header A-J +
> Parts I/II/III; the compute leg's PERSISTED output columns override the pure chain — carries
> the 8829 reflow; Part IV vehicle answers + line-E address = stated boundaries),
> **IRS1040ScheduleSE** (lines 7/14 are PRE-PRINTED — no XSD elements, never emitted; the
> extractor re-derives line 2 through the SAME helpers compute ran and RAISES on drift vs the
> persisted se_tax), **IRS1040Schedule2** (full SCH2_LINE_ORDER; the 1e/1f/1y/17a repeating
> typed groups map to their TOTAL elements only), **IRS7206** (via the NEW
> `form_7206_rows` bridge-gate reader; extractor RECONCILES Σ line 14 vs Sch 1 L17 — S-corp/
> farm/Medicare-fold SEHI raise UnmappableValue), **IRS8995** (line-1 identity rows from the
> ScheduleC/K-1 models mirroring render_8995; line 15 schema-REQUIRED; the 16/17 carryforward
> zeros emit), **IRS7217** (one per distribution via `distribution_results` — withheld §732(c)
> allocation or gain-with-unasserted-holding-period raise; line 7 emits its figured 0; the
> three Part II line-B totals close the element) — plus IRSW2 **EmployersUseGrp** (box 12 DD)
> + **W2StateLocalTaxGrp** (boxes 15-17 KY). ReturnData1040 positions VERIFIED: 1040 → Sch1 →
> **Sch2** → Sch3 → 8812 → **SchC*** → EIC → **SchSE*** → 1099R* → 2441 → 6251 → **7206*** →
> **7217*** → 8862 → 8863 → 8911* → 8911SchA* → **8995** → W2*. **SCENARIO**:
> `ats/scenario12.py` + `mef_build_ats_scenario12` (rollback default, seq 4) — engine matches
> the hand-computed ENACTED-LAW pins: **43 pins ALL MATCH** (1040 ×19 / Sch1 ×5 / Sch2 ×2 /
> 8995 ×11 / 7217 face ×8): AGI 122,445 / std **15,750** (key stale 15,000) / **QBI 4,322**
> (the key OMITS 8995 entirely) / tax 17,436 (key 18,634 on its stale inputs — reproduced) /
> owe **6,430** (key 7,628); 7217 col (e) **6,000** per §732(a)(2)+(c) (the key's own 4,000
> contradicts its line 10). §6654: scenario silent on prior year, key computes NO penalty →
> `t2210_prior_year_tax=14,444` models the exactly-met 100% harbor (line 38 blank, FORM_2210
> disengaged — documented input modeling). **⚠️ SURFACED — Schedule SE rounding convention
> (REVIEW_QUEUE, Ken to rule):** the engine cents-chains SE (12 = 3,437.44 → 3,437 on Sch 2
> 4/21 + 1040 23); the key/TaxWise round per line (2,786 + 652 = 3,438); the engine's own
> printed SE face shows 10+11 ≠ 12 at whole dollars. Total tax/owe agree either way.
> PRE-EXISTING, documented in the artifact README, never bent. (SE line 9: engine 70,222 is
> CORRECT; the key's 62,722 is a typo.) **GATES**: 42 pure (test_efile_mef_1040 incl. the
> nine-document structural + XSD tests, + scaffold) + **DB scenario suite ALL GREEN**
> (test_mef_scenario12_compute: 19 line pins + Sch1/2/8995 + 7217 face + SE/no-penalty + the
> artifact leg) + XSD-valid first try + `manage.py check` clean + flow gate **375 green**
> (9 getsource-pin FAs flaked when the PARALLEL 4797 session saved compute.py mid-run —
> re-run 9/9 green on the stable tree). Boundaries → DEFERRAL_AUDIT 2026-07-02 (8 items).
> Artifacts → `docs/mef/ats_out/scenario12/` (gitignored). NEXT: S2 (8283) / S3 (4835) / S4
> (3800+8835+8936) each need their tax-law unit first, smallest-first per Ken. Detail →
> STATUS.md.

> **2026-07-02 (sixth session) -- FORM 7217 (§§731/732 partnership property distributions) --
> ★★ UNIT COMPLETE, ALL FOUR LEGS GREEN -- tag `1040-form-7217-complete` -- SPEC-FIRST RS
> round-trip -- flow gate 370→375.** ATS Scenario 12's tax-law form, smallest-first per Ken.
> The SEASON_PLAN appendix-4 OBBBA check ran AT the spec leg: §732 is a basis provision, no
> sunset (amendments end at P.L. 106-170 (1999)); Dec-2024 form/instructions current; ONE
> post-publication development found + encoded (IRS 2026-04-27: TY2025+ K-1 box 19 codes B/C/G →
> line 3, A/D/F → 5a, A/F statement → 5b — sourcing hints, Dec-2026 instructions revision).
> **SPEC**: RS `load_1040_form_7217.py` (RS `de3b2f9`/`c824378`) 31 facts / 5 rules / 23 lines /
> 11 diagnostics / **18 HAND-COMPUTED scenarios** (T2/T3 pin the i7217's OWN Examples 1+2 verbatim
> incl. the 100/440/110 waterfall; every §732(c)(1)(A)/(B)+(c)(2)(A)/(B)+(c)(3)(A)/(B) branch) /
> 5 loader-homed FA; NEW independent gate `check_7217_integrity.py` (statute-direct waterfall,
> self-tested vs Example 2) ALL PASS. **Ken's walk (AskUserQuestion): J1 RULED "wire §731(a)(1)
> gain to Sch D now"** (landing verified verbatim vs the 2025 Partner's Inst. K-1 box 19: "Form
> 8949 and the Schedule D"; NEW source IRS_2025_K1P_INSTR) — encoded as a synthesized no-1099-B
> 8949 row (box C ST / box F LT per the NEW holding-period assertion; proceeds=5c basis=6;
> unasserted → feed WITHHELD + D_7217_011 RED); J2 §731(a)(2)-loss + J3 §737 defers KEPT; J4
> rounding (half-up, residue-to-largest), J5 classification-withhold, J6 securities 5b-feeder
> approved. Seeded RS Supabase; deployed export id-verified (lookup key `FORM_7217`); FA drift =
> EXACTLY the 5 new ids. **MODEL** `PartnershipDistribution` (one Form 7217 per distribution
> date) + `DistributedProperty` children (migs 0150 + RLS 0151, APPLIED) — MULTI-INSTANCE, NO
> FFV face rows. **COMPUTE** `compute_7217.py` pure helpers (Part I chain; the §732(c) waterfall;
> `distribution_results` = THE bridge-gate every consumer reads); the J1 wire injects virtual
> box-C/F rows into compute_schedule_d_db's BOX TOTALS (never the l4/l11 args — double-count
> guard FA-05) + a schedule_d_engaged arm; 11 D_7217_* diagnostics (rules_7217, seed_rules run).
> **INPUT** nested serializers + 4 CRUD actions (recompute-on-mutation) + the two-level
> `PartnershipDistributionsSection` on the form_7217 Income tab; tsc 0 / vitest 275. **RENDER**
> f7217 registered (manifest+SHA256), label-verified map incl. the generated 30-row Part II grid;
> `render_7217` one copy per distribution via distribution_results; the synthesized rows join
> render_8949_1040 (desc truncates at the face's 30 chars). **GATES**: 20 pure compute + 6 render
> + **DB pipeline 4/4** (Gardenia no-gain/render smoke; gain-LT → 8949 box F → Sch D 16 → 1040
> line 7 = 2,000; withheld-until-asserted incl. the assert-flip; money-only + unclassified REDs;
> one pooler connection-kill re-ran green) + flow gate **375** + check clean. ⚠️ S12 answer-key
> notes for the mapper leg: key omits QBI + uses pre-OBBBA 15,000 std ded; its own 7217 Part II
> col (e) 4,000 contradicts its line 10 = 6,000 (engine follows §732 → 6,000 = Example 1); SE
> line 9 key typo 62,722 (176,100−105,878 = 70,222, immaterial). Boundaries → DEFERRAL_AUDIT
> 2026-07-02 (9 items). NEXT: the Scenario-12 mapper needs SIX new document builders (ScheduleC/
> ScheduleSE/Schedule2/7206/8995/7217 — 8995 because ENACTED law computes QBI 4,322 the key
> omits). Detail → STATUS.md + RS session_log.

> **2026-07-02 (fifth session) -- MeF ATS Scenario 1040-13 (William & Nancy Birch, §30C) through
> the REAL engine + 3 new e-file document mappers + a found-and-fixed Schedule 3 mapper gap --
> ★★ UNIT COMPLETE -- flow gate unchanged 370 (pure serialization, no compute/render change).**
> The 8911 engine unit (same day, tag `1040-form-8911-complete`) made this mapper pure
> serialization — no RS spec needed. **MAPPER**: read_model + builder grew **IRS6251**
> (spine-always/add-backs-nonzero emit mirroring render_6251; the SAME `compute_6251_face`
> bridge-gate source; Part III = the render leg's anchors-12/40 v1 boundary; attached when 8911
> engages — the i8911 line-8 "figure the TMT even with no AMT owed" rule, exactly why the
> scenario's form list includes 6251 — or when AMT>0; a RED-deferred 6251 raises UnmappableValue),
> **IRS8911** (FORM_8911 face FFVs; line 8 figured-TMT emitted EVEN AT 0 like the IRS key; zeros
> elsewhere omitted; line 3 business/K-1 > 0 raises UnmappableValue — the Form 3800 gap, mirrors
> D_8911_004; referenceDocumentId links the Schedule A docs), **IRS8911ScheduleA** one per
> RefuelingProperty row (cells via compute's OWN `property_schedule_a` — bridge-gate; the FACE
> stop rules encoded: tract-No stops after 6a, 0% business skips 9-16, 100% business skips Part
> III, non-main-home stops at 17; schema-required desc/dates/6a-indicator validated at extract).
> ReturnData1040 positions VERIFIED: 2441 → 6251 → 8862 → 8863 → 8911* → 8911ScheduleA* → W2*
> (IRS8911 includes from CorporateIncomeTax/Common — the include path confirmed). **🔧 GAP
> FOUND+FIXED: `SCH3_LINE_ORDER` was missing 6i/6j/6k — Schedule 3 line 6j (the 8911 credit
> itself!) was silently DROPPED from every submission** (XSD-valid, totals masked it); added from
> the verified XSD; 6z = a repeating type-text group the app can't fill (stated boundary,
> DEFERRAL_AUDIT); 6e face-Reserved never emitted. **🔧 FILER NAME LINE = Pub 4164 §12.5**
> (fetched verbatim from the local pub): NEW `values.filer_name_line1` — "HENRY A<CARTER", MFJ
> "JOHN A & JANE B<SMITH" (Birch = the first MFJ scenario forced it), different-last
> "CARL A<JONES<& ANGIE MYER", the over-35 shortening ladder; header-ONLY (PersonNameType forbids
> '<'). ⚠️ scenarios 5+8 artifacts predate this — REBUILD before IFA upload. **SCENARIO**:
> `ats/scenario13.py` + `mef_build_ats_scenario13` (rollback default, seq 3) — engine matches the
> hand-computed ENACTED-LAW pins to the dollar (28 pins: 1040 ×18, 8911 face ×7, Sch 3 ×3, + the
> 6251 face ×10): tax 11 / credit 11 / **refund 609** (⚠️ the answer key's pre-OBBBA 30,000 std
> ded gives 162/162 — refund 609 IDENTICAL since the credit fully offsets; the earlier STATUS
> "598" figure was wrong — [[ats-answer-keys-pre-obbba-stale]]); README documents the divergence.
> **GATES**: 21 pure (test_efile_mef_1040 incl. the six-document XSD validation + name-line
> cases) + 19 scaffold + **DB 22/22** (17 line pins + 8911/Sch3 face + 6251 figured-TMT face +
> the artifact leg; two pooler connection-kills re-run green) + flow 370 + check clean.
> Artifacts → `docs/mef/ats_out/scenario13/` (gitignored). NEXT: S12 (7217) needs the Form 7217
> tax-law unit FIRST. Detail → STATUS.md + `.claude` [[mef-line-order-maps-silent-drop]] (new).

> **2026-07-02 (fourth session) -- FORM 8911 + Schedule A (§30C refueling credit) -- ★★ UNIT
> COMPLETE, ALL FOUR LEGS GREEN -- tag `1040-form-8911-complete` -- SPEC-FIRST RS round-trip --
> flow gate 366→370.** ATS Scenario 13's tax-law form, smallest-first per Ken's ruling. The
> SEASON_PLAN appendix-4 **OBBBA sunset check ran AT the spec leg**: §30C(i) verbatim (statute +
> i8911 Rev 12-2025) terminates the credit for property placed in service **after 6/30/2026** —
> TY2025 fully live, **TY2026 HALF-YEAR live (Jan–Jun installs)**, TY2027+ dead → S13 survives,
> no re-rule; the window is EXPLICIT in compute (never a latest-year fallback) + FA-03 pins it.
> **SPEC**: RS `load_1040_form_8911.py` (RS `bd739e1`) 16 facts / 5 rules / 35 lines ("A-"
> prefixed Sch A) / 6 diagnostics / 10 HAND-COMPUTED scenarios / 4 loader-homed FA;
> `check_8911_integrity.py` ALL PASS; Ken approved all four judgment items in-session (the
> line-6b ORDERING reading — its verbatim text is self-referential via Sch 3 line 7; census
> tract = PREPARER ASSERTION v1 with 11-digit GEOID format validation; business path RED-defers
> to the unbuilt Form 3800 D_8911_004; T1 pins ENACTED law — ⚠️ the IRS answer key uses the
> PRE-OBBBA 30,000 std ded → their 162 vs the engine's 11, see the new `.claude`
> [[ats-answer-keys-pre-obbba-stale]]); seeded RS Supabase, deployed export id-verified, FA
> drift-check delta = exactly the 4 new ids. **MODEL** `RefuelingProperty` (one Sch A per row)
> + `Taxpayer.refueling_k1_credit` (migs 0148 + RLS 0149, APPLIED). **COMPUTE** `compute_8911`
> (window/claim-year/tract gates; 30%/$1,000-per-item personal at the main home; 6%/30%-PWA/
> $100,000 business + §179 backout + the 1/29/2023 auto-Yes; L5 = 1040 L16 + Sch2 1z; 6b = L19
> + the pre-8911 Sch 3 credits; **L8 = TMT via compute_6251_result FIGURED ALWAYS** per i8911;
> L10 = min(L4, L9) → Sch 3 6j, excess PERMANENTLY LOST) wired after 8812/the 19-20 sync,
> before the second downstream pass; business line 3 never lands (D_8911_004 RED). **INPUT**
> serializer + `refueling-properties` CRUD (recompute-on-mutation) + `RefuelingPropertiesSection`
> on the new form_8911 tab (Credits nav group; D_8911_ routing) + the K-1 line-2 card; tsc 0 /
> vitest 275. **RENDER** f8911 + f8911sa registered (manifest + SHA256), every widget
> LABEL-VERIFIED; `render_8911` = the face from FFVs + ONE Schedule A per property from the
> model via the SAME `property_schedule_a` helper, merged into one packet doc; the pre-printed
> $100k/$1k cap slivers deliberately unmapped. **GATES**: 18 pure + 7 render + **DB pipeline
> 4/4 in 83s** (Birch 300→11→Sch3 6j→1040 L20; tract-No disengage; business-only face-not-6j;
> render smoke w/ GEOID comb); flow gate **370**; `manage.py check` clean. Boundaries →
> DEFERRAL_AUDIT 2026-07-02 (3800 defer; Appendix A/B not adjudicated; coords/other-owner/7220
> out; TMT-None corner; TY2026 re-pin). NEXT: the Scenario-13 mapper (IRS6251 + IRS8911/SchA
> docs + `scenario13.py`). Detail → STATUS.md + RS session_log.

> **2026-07-02 -- MeF ATS Scenario 1040-5 (Bobby Barker) through the REAL engine + 7 new e-file
> mappers + TWO spec-first engine fixes -- ★★ UNIT COMPLETE -- flow gate 365→366.**
> The refundable-credit-stack scenario (HOH + blind, 2 QC, EIC-after-disallowance/8862, 2441 ×2
> providers, 8863 AOTC-on-self, Sch 8812 with the 2025 line-28 ACTC OPT-OUT, Sch 1 moving
> expenses): `ats/scenario5.py` + `mef_build_ats_scenario5` (rollback default), **22 hand-computed
> 1040 lines match to the dollar** (EIC 5,494 lower-of lookup; line 28 = 0; refundable AOTC 392;
> 25d = 1,754 EXACT). **Fix 1 -- SCH_8812 ACTC opt-out** (RS `c2b778b`, tts `83071ea`): fact
> `actc_opt_out` + R017 `AND NOT actc_opt_out` (Part II-A skipped entirely, CTC/ODC line 19 never
> touched), D_8812_014/D014 info, TS19, FA-1040-CTC-13 (conditional_zero, loader-homed); Taxpayer
> mig 0147 also adds pecf_primary/pecf_spouse/us_home_more_than_half_year/
> mfs_hoh_lived_apart_last_6_months (spine facts; PECF new in RS, other two already specced);
> input = 8812-tab election card + Taxpayer-Info header block; render = five f1040 widgets
> label-verified (c1_5/c1_6/c1_7/c1_32/c2_14). **Fix 2 -- R-8959-ENGAGE = the i8959 Who-Must-File
> conditions** (RS `22f29be`, tts `e25e8d3`): the scenario's box-6 whole-dollar rounding (453 vs
> 452.86) put $0.14 on line 25c via the code's spec-less "line 22 > 0" arm -- REMOVED; the missed
> bullet-1 arm ADDED (any SINGLE W-2 box 5 > $200k FLAT -- one 210k W-2 on an MFJ return must
> file to recover the $90 withheld); `max_single_w2_box5` shared by compute + all three D_8959_*
> (bridge-gate); D_8959_003 engage-gated; scenarios 8959-T6/T7 HAND-COMPUTED; verbatim
> Who-Must-File excerpt on NEW source IRS_2025_8959_INSTR; integrity gate ALL PASS; zero
> sibling-spec drift. **MAPPER**: read_model + builder grew IRS1040Schedule1/Schedule3/
> Schedule8812 (Part II-A omitted under the election; required line-12 Ind emitted)/ScheduleEIC/
> IRS2441 (provider+person groups)/IRS8862 (per-child 365-day + per-student answers derived from
> the SAME model facts the credits computed from)/IRS8863 (per-student 27-30 via
> compute_8863.aotc_student_lines -- new shared helper); IRS1040 gained PECF/MainHome/
> DependentDetail grid/SepdSps/12a-12d block (via std_deduction_checkboxes -- bridge-gate)/
> DoNotClaimACTCInd; IRSW2 boxes 3-6. All NINE documents validate vs the 2025v5.4 XSDs in
> ReturnData1040 sequence order. Stale scaffold pin repointed (ack parser built 6-29).
> Boundaries -> DEFERRAL_AUDIT 2026-07-02 (EIC opt-out unbuilt, months-vs-days granularity,
> business rules still Ken-pull). Detail -> STATUS.md + RS `session_log.md` + `.claude`
> [[mef-efile-kickoff]].

> **2026-07-05 -- GA-500 Schedule 1 (Adjustments to Income) RENDER LEG -- ★★ COMPLETE (9 pure + 4 DB render tests passed in 2:47) -- flow gate 398 held.**
> Closes the DEFERRAL_AUDIT "Schedule 1 DETAIL page never coordinate-mapped" gap. The S1-*
> adjustment lines previously reached the printed return only via the Form-500 line-9 net; now
> the full Schedule 1 renders. Template WASN'T in the repo (`ga500.pdf` = the 5-page main return
> only); downloaded the GA-DOR blank-form-printing packet (27 pages), extracted Schedule 1 =
> source pages 5-7 (p1 Adjustments grid Additions 1-6 / Subtractions 7-14, p2 Retirement Income
> Exclusion worksheet lines 1-17 dual-column, p3 Military Retirement Income Exclusion worksheet)
> → `resources/state_forms/GA/2025/ga500_schedule1.pdf` (flat, registered manifest
> `fga500_schedule1` / `GA-500-SCH1`). Ken ruled (AskUserQuestion): **all 3 pages** + **tips/OT
> fold into printed line 12** (2025 form has no 12a/12b cell → render L12 = S1-12+S1-12a+S1-12b so
> the column foots to L13; exact on the TY2025 bed where tips/OT = 0). Build: NEW
> `coordinates/fga500_schedule1.py` (namespaced P1-*/P2-{TP,SP}-*/P3-{TP,SP}-* + SSN comb) +
> `render_ga500_schedule1()` (maps FFV → keys; 7a←RIE-TP-17, 7b←MIL-TP-3+8, 7d/7e spouse;
> attach only when adjustments exist; p2 only if RIE applies, p3 only if military). Pre-printed
> constants (p2 L4 $5k, p3 L2 $17.5k/L7 $35k) intentionally unmapped. Baseline recipe =
> box-bottom rule + 2pt (get_drawings "re"; the ".00" text anchors skew ~8-10pt low). No compute
> change in the render leg (flow 398 unchanged). ⚠ **Surfaced + then FIXED (Ken's go-ahead) a
> military over-exclusion COMPUTE bug:** `compute_ga500.mil()` was `l3+l8`, over-excluding military
> retirement of $17,501–$34,999; now `min(mret, 35000)` per IT-511 (renderer 7b/7e updated to match;
> T5 re-pinned + NEW `T5b-military-midrange`; 19 pure scenarios + flow 398 green). RS-side spec
> `R-GA500-MIL` + `check_ga500_integrity.py` reconciliation PENDING (`docs/rs_handoff/2026-07-05_…`).
> Boundaries → DEFERRAL_AUDIT 2026-07-05. Detail → `.claude` [[ga500-schedule1-render-leg]] +
> [[ga500-military-exclusion-overexclusion-bug]].

> **2026-07-02 -- GA-500 HB 463 tips/overtime exclusions (§48-7-27(a)(16)/(17)) -- ★★ UNIT COMPLETE (DB-verified, GA suite 48 passed in 9:20) -- tag `ga500-hb463-tips-ot-complete` -- SPEC-FIRST RS round-trip -- flow gate 363→365.**
> Found during the same-day HB 463/AP statute verification: the enacted Act (signed 2026-05-11,
> legis.ga.gov doc 249080) created TY2026-2028 exclusions for cash tips + qualified overtime
> (≤$1,750 each) with NO compute_ga500 handling — an hourly worker's TY2026 GA return overstated
> tax. Ken chose (AskUserQuestion) full unit / **PER-TAXPAYER cap** (MFJ = each spouse separately,
> the "received by" reading — re-verify at the 2026 IT-511, W7) / OT **assertion+auto-feed**
> (unasserted FT-hourly → NO exclusion, penalty-safe + D_GA500_013 nudge) / dedicated
> **S1-12a/12b** sub-lines. RS `load_ga500_form_500.py`: +6 facts, +R-GA500-TIPS/-OT (window-gated;
> bases = RAW federal qualified amounts — **GA has NO phaseout**, never the post-phaseout federal
> deduction; OT = W-2 employee comp ONLY, never 1099), R-GA500-L9-S1 amended (subs L7-L12b),
> +D_GA500_013/014/015, +scenarios T15-T18 (HAND-COMPUTED: per-taxpayer caps 3,500/2,750 → 4,179;
> unasserted-OT → 2,246; TY2025 year-gate → 2,491; under-cap passthrough → 1,672), +FA-GA500-13/14,
> and the **HB 463 authority excerpt CORRECTED to verbatim /AP text** (the prior summary excerpt
> wrongly dated the std-ded increase 2027; the seed deletes the stale row). `check_ga500_integrity`
> re-types the window/caps independently — ALL PASS (18 scenarios). Seeded RS Supabase
> (83f/23r/91L/15d/18t/14FA) → canonical `500_spec.json` re-exported (id-diff: only intended
> changes). tts: seed_ga500 +8 lines (bases + FT-hourly checkboxes + computed 12a/12b; 144→152,
> shared DB re-seeded); compute_ga500 `_tips_ot_cap` EXPLICIT window (TY2029+ = 0, never a
> latest-year fallback) + S1-13 sum; federal pull in `_populate_ga500_from_federal` (per-owner W-2
> box 7 gated on the federal tipped-occupation attestation + other-tips field; OT = the W-2 field
> only; window-gated so D_GA500_015 can't misfire on a pulled value); rules_ga500 +3 (seed_rules
> run); FA merged into the gate file (341→343 assertions). INPUT = the seeded lines on the existing
> Sch 1 tab (StandardSection — zero client change; tsc 0 / vitest 275 untouched). RENDER = correct
> via the line-9 net on the Form 500 face (the Schedule 1 DETAIL page has never been
> coordinate-mapped — pre-existing whole-page gap, stated). GATES: flow **365** (incl. FA-13/14),
> 18 pure scenarios, **DB 48 passed in 9:20** (seed 4 / compute 5 / diagnostics 17 / state-return 5
> incl. both pull tests / + the rest of the GA suite). Boundaries → DEFERRAL_AUDIT 2026-07-02.
> Detail → STATUS.md + RS `session_log.md` + `.claude` [[ga500-kickoff]].

> **2026-07-26 -- s113 QA Batch-001 finishing run (items 7/8/10) -- three shipped legs.**
> (1) **Form 2210 RATE CORRECTION**: the published 2025 i2210 Penalty Worksheet is FLAT 7%
> (x 0.07 in all four rate periods; Rate Period 4 = 1/1-4/15/2026; Table 2 days 365/304/212/90) --
> the June 6% Q2 stub was a pre-publication assumption that UNDERSTATED penalties (the QA $1-3
> TaxWise deltas). Pins 461->466 / 143->145 / 217->219 / 369->372; RS + harness + FA runners
> re-pinned (RS `744ba30` / app `8b927d4`). (2) **Form 7206 PARTNER ARM** -- see the 7206 row
> above (RS `6761b65` / app `7050f93`, mig 0213). (3) **GA-500 RIE diagnostics**: D_GA500_002
> realigned to the spec (DOB-required error), NEW 016 (elected-but-$0) + 017 (not-auto-pulled
> categories) (RS `b2318a8` / app `5f36876`). Detail -> STATUS_ARCHIVE s113.
> **2026-07-01 -- Form 2210 DATED federal payments (§6654 due-date→date-paid accrual) -- ★★ UNIT COMPLETE (DB-verified, 6 pipeline passed in 2:24) -- tag `1040-2210-dated-payments-complete` -- SPEC-FIRST RS round-trip -- flow gate 362→363.**
> Ken chose scope option 1 in-session ("build as designed", `server/specs/2210_dated_penalty_design.md`).
> RS FORM_2210 amended (RS `35413f5`/`9f94b7b`/`0b72b6f`): R-2210-REG reworded to the rule its own
> authority excerpt already stated — every payment applies to the EARLIEST still-underpaid installment
> and accrues **from the installment due date to min(date cured, 4/15/2026)** (7%→6% at 3/31/2026);
> withholding stays ¼-spread ON the due dates (§6654(g) default; actual-date election deferred). NEW
> fact `t2210_payments_dated`, diag `D_2210_DATED` (dated-vs-line-26 reconciliation warning), scenarios
> **P-T7 (mid-year lump = 217)** / **P-T8 (Q4 paid 1/25 = 5)** / P-G2 — all HAND-COMPUTED; P-T1..T6
> pinned unchanged (due-date payments reproduce the fixed-day numbers exactly); FA-02/03 reworded +
> **FA-1040-2210-07** (merged SURGICALLY into the gate file — the deployed RS FA export is missing 21
> accumulated assertions; drift → REVIEW_QUEUE). `check_2210_integrity` re-types the dated algorithm
> independently, ALL PASS. ⚠️ KEY LAW SHAPE: the form has TWO allocations — FACE line 25 nets payments
> per column DATE WINDOW (+overpay carry); the PENALTY worksheet applies earliest-first — never blend
> them (FA-03 caught exactly that). MODEL `FederalEstimatedPayment` (migs 0145 + RLS 0146, APPLIED;
> mirrors StateIncomeTaxPayment, no state_code; creditable kinds = estimate + prior_year_applied,
> PY-applied blank-dated → 4/15 per i2210; serializer requires date or quarter). COMPUTE
> `days_at_rates` + the unified `regular_penalty` (dated rows REPLACE the flat buckets; empty → the
> pre-amendment fallback); `federal_dated_payments`/`-reconciliation` = the single source for compute
> AND diagnostics (bridge-gate). INPUT serializer + `federal-estimated-payments` CRUD
> (recompute-on-mutation) + React `FederalEstimatedPaymentsSection` dated grid on the form_2210 tab.
> RENDER verified unchanged (face 4/5/7/9/19). GATES: flow **363** (incl. FA-07), 389 pure (incl. 16
> 2210 + memo 10); **DB 6 pipeline passed** (incl. dated-late-Q4 = 4.00 → line 38 + the divergence
> warning); tsc 0 / vitest 248; check clean. 1040 line 26 stays flat (spine scope) — boundaries →
> DEFERRAL_AUDIT 2026-07-01. Detail → STATUS_ARCHIVE + DECISIONS.md + `.claude`
> [[form-2210-penalty-due-date-to-date-paid]].

> **2026-07-01 -- 1065 Schedule K / K-1 line 14a SE classification (§1402(a)(13)) -- ★★ UNIT COMPLETE (RECOVERED from an uncommitted dead session, verified, finished) -- tag `1065-se-line14a-complete` -- SPEC-FIRST RS round-trip -- 1040 flow gate unchanged 362.**
> The FIRST RS-specced core-1065 piece. Ken locked the four tax-law calls 2026-07-01
> (`server/specs/1065_se_line14a_spec.md`): (1) undetermined → active/INCLUDE safety net; (2) LLC members
> ride the same active/passive functional-analysis axis as limited partners (Renkemeyer/Soroban — the
> state-law label is not the test; contra-Sirius binds 5th Cir. only, GA/11th unresolved); (3) passive
> capital GP excluded (§1402(a)(13) carve-back is services-only); (4) active capital GP included
> (Reg §1.1402(a)-1(b)). RS form `1065_SE`: 9 rules / 3 diagnostics / 10 tests / 7 authorities (CFR/USC
> quoted VERBATIM; case-law group requires_human_review, re-verify each season) / RS FA 338→341
> (RECON-14A + INV-CHAR active, FLOW-14A-SE disabled=future); canonical export
> `server/specs/1065_se_spec.json`. MODEL `Partner.se_classification` + `se_classification_basis`
> (mig 0144, APPLIED). COMPUTE `resolve_se_classification` + rewritten `compute_self_employment`
> (replaces the silent limited-exclude / LLC-full-include); NEW `compute_1065_se.py` — entity K14a
> derived BOTTOM-UP = Σ per-partner box 14a (replaces `K14a = K1 + K4c`; override kept, D_SE_RECON owns
> the break). DIAGNOSTICS D_SE_UNDET / D_SE_GPCHAR / D_SE_RECON (runner-registered, seeded + active in
> the shared DB; `seed_1065_se` validates spec shape + ensures the K14a line). INPUT PartnersSection
> SE-classification select (general = fixed-active read-only, undetermined hint) + basis note. TESTS
> spec-DRIVEN `test_1065_se_compute_leg.py` (T1–T10 read from the export + INV-CHAR noise + off-form
> no-op + shape pins) + `test_k1_allocator.py` rewritten per spec §11 = **47 passed**; tsc 0 /
> vitest 248. Boundaries (stated → DEFERRAL_AUDIT): SE-base sub-spec (4797 Part II backout + rental
> 1b/1c, coupled w/ 4797 §1245/§1250 verify); 14b/14c; non-individual guard; no 1065 FA gate file yet;
> 1065 K-1 PDF render still stub; no DB pipeline test. Detail → STATUS_ARCHIVE + DECISIONS.md +
> `.claude` [[1065-se-line14a-unit]].

> **2026-07-01 -- State withholding → Schedule A line 5a (auto-total state income tax) -- ★★ UNIT COMPLETE (DB-verified, 28 passed) -- tag `1040-schedule-a-state-5a-complete` -- SPEC-FIRST RS round-trip -- flow gate 360→362.**
> Ken's UX/bug queue ("state withholding → Schedule A"). Ken chose (AskUserQuestion) the FULL scope
> (withholding + estimates + prior-year taxes + prior-year extensions, WITH payment dates) + full RS
> round-trip; sequencing = state now, federal 2210 dates next. Line 5a was a single hand-keyed fact; it
> now AUTO-TOTALS the return's state/local income tax = WITHHELD (documents) + dated payments (YELLOW),
> preparer overrides GREEN, suppressed when general sales taxes are elected. §164 cash-basis date filter
> (a Q4 estimate paid in January lands on next year's 5a). RS `load_1040_schedule_a.py` +4 facts, +rule
> `R-SCHA-5A-STATE` (precedence 2, before SALT — feeds 5d), +2 diags (`D_SCHA_010` transparency /
> `D_SCHA_011` out-of-year), +5 scenarios (T12–T16), +2 FA, +1 authority (`IRS_2025_SCHA_INSTR`, line-5a
> text quoted VERBATIM); `check_schedule_a_integrity` extended + ALL PASS; seeded RS Supabase
> (FlowAssertions 336→338); re-exported `schedule_a_spec.json` (zero content drift). MODEL
> `StateIncomeTaxPayment` (per-return list; kind/amount/date_paid/quarter/tax_year_for/state_code/memo;
> reusable for GA-500 + federal 2210 UET; migs 0142 + RLS 0143, APPLIED). COMPUTE `state_income_tax_withheld`
> (W-2 box17 via `W2StateEntry` rows, else the legacy flat field when no state rows — mig-0084 backfill
> double-count guard; + 1099-R/INT/DIV/G/W-2G) + `state_tax_payments_summary` (dated, in-year) → 5a routing;
> engages on entered payments, NOT on withholding alone. DIAGNOSTICS `D_SCHA_010/011` (re-derive via the
> compute helpers). INPUT serializer + `state-income-tax-payments` CRUD (recompute-on-mutation) + React
> `StateTaxPaymentsSection` (dated grid, in-year total footer, out-of-year amber) + line 5a YELLOW auto-total
> display. Gates: flow **362**, client tsc 0 / vitest 248, 18 pure Sch A GREEN, `manage.py check` clean.
> ✅ **DB-VERIFIED 2026-07-01: `tests/test_schedule_a_compute_leg.py` 28 passed in 13:46** (6 `test_pipeline_5a_*`
> — autofill / override GREEN / sales-tax suppress / out-of-year + D_SCHA_011 / withholding-alone no-engage /
> W-2 no-double-count — + 4 pre-existing pipeline + 18 pure incl. T12–T16). Ran venv-python direct with
> `--timeout=1800 --reuse-db` after dropping a leftover `test_postgres`. **Tagged `1040-schedule-a-state-5a-complete`.**
> Detail → STATUS.md + DECISIONS.md + `.claude` [[schedule-a-5a-state-withholding]] / [[form-2210-penalty-due-date-to-date-paid]].

> **2026-07-01 -- Roth IRA basis tracker (year-over-year, feeds Form 8606 line 22/24) ★★ UNIT COMPLETE -- tag `1040-roth-basis-tracker-complete` -- SPEC-FIRST RS round-trip -- flow gate 359→360.**
> Ken's UX/bug queue ("Roth basis tracker — year-over-year Roth contribution basis; 8606 Part III only
> captures at distribution"). Ken chose (AskUserQuestion) contribution+conversion cumulative scope + the
> full RS round-trip. NEW per-owner `RothIRABasis` model (`contribution_basis`→line 22, `conversion_basis`
> →line 24, `current_year_contributions` for the roll; UniqueConstraint tax_return+owner; migs 0140 + RLS
> 0141, APPLIED). COMPUTE: `_resolve_roth_basis` sources line 22/24 from the tracker (YELLOW) unless the
> Form8606 field is a non-zero override (GREEN); `owner_lines` gained the overrides + emits l22/l24; both
> `compute_8606_db` AND `render_8606` call it (render previously read the doc field directly — would have
> hidden the tracker value on the PDF). DIAGNOSTICS: new `D_8606_ROTHNOBASIS` (warning, tracker-aware) +
> `d_8606_part3` made tracker-aware. INPUT: `RothIRABasisSerializer` + `roth-ira-bases` CRUD (recompute-on-
> mutation, one-per-owner guard) + `RothIRABasisSection` folded into the form_8606 tab + YELLOW carried-hint
> under line 22/24. PROFORMA: `_roth_ira_bases` read side (roll recovers contribution basis first). RS:
> `load_1040_8606.py` (RS `be680d4`) +fact `f8606_roth_cy_contributions` +rule `R-8606-ROTHTRACK` +diag
> +scenario F-G2 +FA-1040-8606-07; `check_8606_integrity` ALL PASS; §408A(d)(4) math unchanged; seeded RS
> Supabase → re-exported `8606_spec.json` (zero drift on existing). Gates: flow **360**, client tsc 0 /
> vitest 248, 14 pure 8606 tests GREEN; DB pipeline/diagnostics tests run separately. Detail → STATUS.md +
> DECISIONS.md + `.claude` [[roth-basis-tracker]].

> **2026-07-01 -- Form 1116 §904(j) de minimis FTC AUTO-path + opt-out -- SPEC-FIRST RS round-trip -- flow gate unchanged 359.**
> Ken's UX/bug queue, next item ("foreign-tax de minimis §904(j)"). The §904(j) election now applies
> AUTOMATICALLY: when the only foreign tax is from 1099-INT/1099-DIV (passive + payee-statement BY DEFINITION),
> total ≤ $300/$600, and NO full Form 1116 is engaged, the credit lands on Schedule 3 line 1 as `min(foreign
> tax, regular tax)` (YELLOW), no Form 1116 face, no carryover. Preparers no longer have to open the FTC tab
> and tick the box for the common retiree-with-an-international-fund case. Law re-verified verbatim vs i1116 +
> 26 USC §904(j) (matches the already-authored spec). Ken chose (AskUserQuestion) auto-apply + opt-out AND the
> full RS round-trip. **RS** `load_1040_form_1116.py`: +fact `f1116_deminimis_optout`, reworded `R-1116-ELECT`
> (auto) + `D_1116_001` (auto-applied), +`D_1116_009` (warning, over-ceiling-no-form nudge — closes a silent
> gap); `check_1116_integrity.py` ALL PASS (math unchanged, facts 19→20 / diags 8→9); seeded RS Supabase →
> re-exported canonical `1116_spec.json` (semantic diff = zero drift). **tts**: COMPUTE helper
> `_deminimis_election_credit` + auto-path in `compute_1116_db` (`form is None`); `compute_1116_result` returns
> the auto dict (render+diagnostics agree). INPUT `Taxpayer.ftc_deminimis_optout` (mig 0139, APPLIED) +
> serializer + opt-out card on `Form1116Section`. DIAGNOSTICS `rules_1116` D_1116_001 auto + D_1116_009 +
> registry synced (9 rules). RENDER unchanged (render_1116 already None for path != full). Opt-out lives on
> Taxpayer NOT Form1116 (the auto-path must work with no form engaged); an engaged Form 1116 stays in the manual
> flow untouched. Gates: flow **359**, client tsc 0 / vitest 248, 12 pure 1116 compute GREEN. +6 new DB tests
> (4 pipeline: auto/MFJ/opt-out/over-ceiling + 2 diagnostics). Detail → STATUS.md + DECISIONS.md + `.claude`
> [[ftc-904j-deminimis-auto-path]].

> **2026-06-30 -- Default-to-No due-diligence questions (digital assets + Sch B 7a/8) -- SPEC-FIRST RS round-trip -- flow gate unchanged 359.**
> Ken's UX/bug batch. The digital-asset header question + Schedule B Part III foreign questions (7a foreign
> account, 8 foreign trust) now DEFAULT to No: the UI pre-selects No shown YELLOW (unconfirmed default; the
> stored field stays null), the face PRINTS No, and the diagnostic drops to INFO ("confirm before filing").
> Explicit Yes/No → GREEN, INFO clears. Because these diagnostics are SPEC-GOVERNED (`D_1040_017` in
> `1040_spine_spec.json`, `D_SCHB_001` in `sch_b_spec.json`, both "implemented strictly from the spec"), CC
> flagged it and Ken chose the proper RS round-trip: amended `load_1040_spine.py` (D_1040_017 warning→info) +
> `load_1040_intdiv_qdcgt.py` (D_SCHB_001 error→info, condition widened to 7a OR line 8 — closing a
> pre-existing spec-vs-code gap where the code only checked 7a; R-SB-05 desc updated), both RS integrity gates
> GREEN, seeded RS Supabase, re-exported both specs to tts (semantic diff = zero drift). RS commit `5d3aac0`.
> tts: render (renderer.py null→No for both), UI (TaxpayerInfoSection select + InterestIncomeSection SchBAnswer,
> default-No YELLOW), rules_1040/rules_schb synced, 4 tests updated. Gates: flow 359, tsc 0 / vitest 248,
> **digital-assets DB 3-passed + Sch B DB 5-passed (confirmed)**. tts `b519377` + fix `cb4a7ad` (the D_SCHB_001
> RULES_SCHB registry severity was left stale at "error" while the function returned info — the clean re-run
> caught it). tts DiagnosticRule rows re-seed on deploy. NOTE: MeF still requires an explicit digital-asset
> answer (stricter e-file posture — intentionally left; e-file track).

> **2026-06-30 -- UX/bug batch: Sch D "V"-for-various + legacy itemizing box removal -- flow gate unchanged 359.**
> Two of Ken's queued UX items. (1) **Sch D "V" shorthand** (`d97d8b1`, frontend-only): Form 8949 date columns
> (b)/(c) now expand "V"→VARIOUS and "I"/"INH"→INHERITED on blur (TaxWise convention). The Sch D model is
> `CapitalTransaction` (free-text CharField dates; the *box* drives ST/LT) — already fully VARIOUS/INHERITED-aware
> across render + the D_8949 diagnostics; only the typing convenience was missing. New pure `lib/capital-tx-date.ts`
> + `dateText` helper. (Corrects the STATUS note that assumed the Disposition `_various` booleans — that's Form 4797,
> which already had its checkbox.) (2) **Legacy itemizing box — FULL CLEAN REMOVAL** (Ken's call): the manual
> Line-12 `elif tp.itemizing` path was redundant (override box + real Schedule A cover it) and a footgun (no
> larger-of-standard guard). Removed the compute.py branch + the `TaxpayerInfoSection` checkbox (→ Schedule A hint)
> + the dead `d_1040_011` branch; **redirected AMT** (`compute_6251`) to the real Line-12==SchA-17 signal, closing
> the `FLAGGED` heuristic (mirrors `compute_section_68_db`). Model columns + serializer kept (additive-only). Gates:
> flow **359**, manage.py check clean, client tsc 0 / vitest 248, **33 DB tests** confirmed (`test_legacy_itemizing_*`
> + `test_011_*` + 30 AMT/§68 no-regression). Detail → STATUS.md.

> **2026-06-30 -- Schedule D line 4/11 face-cell fix (render-only) -- flow gate unchanged 359.**
> Closed the REVIEW_QUEUE 2026-06-29 finding: Sch D lines 4/11 only surfaced the Form 4797 feed at
> render time, not Form 6252 / Form 8824 ST/LT gains, so those amounts printed blank (tax was always
> correct — line 16 → 1040 7a unaffected). RS `SCHEDULE_D` spec fetched first (line_map 4/11 confirmed
> the 6252/8824 feeds belong there). New `schedule_d_face_line()` in `compute_schedule_d.py` — single
> source of truth for the renderer + tests, reusing the existing `form_6252_sch_d_st/lt` /
> `form_8824_sch_d_st/lt` feed functions already used in the netting-input computation. Repointed the
> 4 `test_compute_8824.py` pipeline tests off the un-persisted FormFieldValue row. ⚠️ Those 4 DB tests
> remain UN-CONFIRMED this session — Supabase pooler hung on `test_postgres` creation 3x (cleared via
> `scripts/drop_test_db.py`); re-run next session. Detail → REVIEW_QUEUE.md (Resolved) / STATUS.md.

> **2026-06-30 -- SSA/RRB WITHHOLDING + MEDICARE (Sch A / SEHI) + RAILROAD ★★ UNIT COMPLETE -- tag `1040-ssa-medicare-railroad-complete` -- flow gate 357→359.**
> Ken's Bach-return item 1 (spec-first). RS `load_1040_retirement` amended + seeded (RS Supabase `ylqaejdqwuvwpglxnpgv`):
> +4 facts (`ssa_fed_withheld`→25b, `ssa_medicare_premiums`, `ssa_medicare_destination` schedule_a|sehi, `ssa_is_railroad`
> RRB-1099 SSEB metadata), +2 rules (`R-RET-25B-SS` extends the 25b roster; `R-RET-MEDICARE` → Sch A L1 (Pub 502) OR
> single-proprietor SEHI capped at net SE profit (Form 7206)), +`D_RET_009` RED (Medicare-as-SEHI w/ multiple businesses
> or Marketplace/PTC → manual), +2 scenarios, +`FA-1040-RET-09/10`, +2 authority sources (Pub 502 + Form 7206 instr,
> both verbatim-verified vs the live IRS text — a WebFetch summary had WRONGLY said Medicare isn't deductible).
> `check_retirement_integrity` ALL PASS; canonical `retirement_spec.json` re-exported (39/13/9). tts: Taxpayer +7 fields
> (mig 0131, applied) + serializer; COMPUTE 25b WH (`compute_retirement.py`) / Medicare→Sch A (`compute_schedule_a.py`)
> / Medicare→SEHI folded BEFORE the QBI loop so it also reduces QBI (`compute_schedule_c.py`); DIAGNOSTIC `D_RET_009`
> (routed to social_security tab); RENDER via existing 25b/Sch A maps (`ssa_is_railroad` = label metadata); UI card in
> `SocialSecuritySection` (taxpayer+spouse inputs, Medicare destination select, RRB checkboxes). TESTS 4 pure Sch A
> (`test_ssa_medicare_sehi.py`) + 2 FA. ⚠️ DB pipeline tests (25b/SEHI cap/D_RET_009 firing) deferred (pooler) — covered
> by FA source-checks + integrity. Client gate tsc 0 / vitest 223; manage.py check clean. Detail → STATUS.md + RS session_log.
>
> **2026-06-30 -- Bach UX batch (items 2,3,4,5,6,7,8)** — frontend: #8 pennies (`wholeDollars`), #2 1099-R EIN autofill
> (yellow), #4 Summary two-column + AGI strict-`form_code` fix, #5 Sch C/E/F depreciation links, #7 gambling losses
> single-entry→Sch A, #3 GA "Add a state return" picker, #6 draft-row pre-load (1099-R + 1099-G; W-2 deferred). tsc 0 /
> vitest 223. Detail → STATUS.md.

> **2026-06-29 -- SS LUMP-SUM ELECTION (Pub 915 Worksheets 2+4) ★★ UNIT COMPLETE -- tag `1040-ss-lumpsum-complete` -- all 6 legs + RS seed + DB tests; flow gate 357.**
> Retiree-hardening cluster item 1 (Ken-directed; D_RET_004 first + EXPLICIT-TOGGLE election). LAW verified verbatim
> vs Pub 915 (2025); Terry example reconciles to the dollar (election 2500 < regular 3000). Legs 1-5 prior session
> (SPEC/MODEL/COMPUTE/DIAGNOSTICS/FLOW; tts `3626f1e`..`be9c544`, RS `bcd6c40`). **This session (`bf3c60a`..`1f83aed`):**
> RS SEED (Ken go-ahead) — seeded RS's own Supabase via `load_1040_retirement`, re-exported canonical
> `retirement_spec.json` (lse_* present), re-seeded tts `1040_RETIREMENT` 24→26 lines (+lse_ws2_21/lse_ws4_21 in the
> hardcoded `seed_1040_retirement.py`). RENDER — box 6c = `c1_41[0]` (f1040 HEADER_MAP); `_build_header_data` checks
> it only when `compute_lump_sum_db().applied`; `render_ss_worksheet_statement` appends a WS2/WS4 section. INPUT —
> `SocialSecurityLumpSumSerializer` + `ss-lump-sums` CRUD (recompute-on-mutation) + React
> `SocialSecurityLumpSumSection.tsx` + new `ss_lump_sum` Income tab + RULE_TAB_MAP D_RET_004/008 → ss_lump_sum;
> removed the stale "compute manually" toggle. Client gate tsc 0 / vitest 219 / build clean. DB TESTS —
> `test_ss_lumpsum_pipeline.py` 7 django_db (light seed) on Terry, ALL GREEN (271s). Cluster items 2-3 (D_RET_005,
> D_8606_NO_YEAREND) still open. Detail → STATUS.md + memory.
>
> **2026-06-29 -- ⚠️ NEW CROSS-UNIT FINDING (flagged to Ken, NOT fixed): Schedule D line 11 face/test gap.** The
> long-pooler-blocked 8824 pipeline tests RAN this session; 4 of 5 FAIL because Sch D lines 4/5/11/12 FormFieldValue
> rows are never written by compute (netting INPUTS only) — the cross-form gains show only at RENDER (`dbeef2d`
> `_direct_plus_k1`) and only the 4797 feed (not 6252/8824 LT). Tax correct (line 16→7a); display/test-layer only.
> Detail → DEFERRAL_AUDIT.md + REVIEW_QUEUE.md.

> **2026-06-28 -- Form 8824 (Like-Kind Exchanges §1031 + §1043) ★★ UNIT COMPLETE -- tag `1040-form-8824-complete` -- all 6 legs green; flow gate 347→356.**
> Ken chose "Full incl. Part IV + computed recapture" 2026-06-28. **Closed the last Form 4797 cross-form
> RED-defer `D_4797_004`** (like-kind). SPEC + COMPUTE + RENDER + DIAGNOSTICS + ASSERTIONS (prior session) +
> **INPUT (this session)**. INPUT: `LikeKindExchangeSerializer` (GREEN inputs only, 34 fields) + `like_kind_exchanges`
> / `like_kind_exchange_detail` CRUD `@action`s + return-serializer/prefetch wiring — mirrors `installment_sales`
> exactly. React `LikeKindExchangeSection.tsx` (rows-CRUD, the `InstallmentSalesSection` precedent) +
> `like_kind_exchanges` tab in IndividualNav's **Income** group + `D_8824_`→tab in RULE_TAB_MAP + has-data
> predicate. Client gate: vitest 217/217, vite build clean, **tsc 0 errors** (the old ~84-err baseline is now
> clean; also fixed the nav test's `emptyRd` which was missing `installment_sales`). COMPUTE: `LikeKindExchange`
> model (1 row/exchange, migs 0126/0127) + `compute_8824.py` (no-cache) wired BEFORE `compute_4797_db`; §1231→4797
> L5→Sch D L11 / ordinary→4797 L16→Sch 1 L4 / capital→Sch D / §1043→4797 L10; §1.1031(d)-2 liability netting +
> §1245(b)(4)/§1250(d)(4) recapture + 25a/b/c basis. RENDER: manifest `f8824` + `field_maps/f8824_2025.py` (63
> fields) + `render_8824_1040` after Form 4797. DIAGNOSTICS: `rules_8824.py` 10 `D_8824_*` (RED-defers:
> multi-asset 007 / main-home 008 / personal-property 009). ASSERTIONS: 9 `FA-1040-8824-*` + `_run_8824_assertion`
> → gate 356; FA-09 verifies all 10 diag codes registered. VERIFIED (everything runnable GREEN): 12 pure compute
> + 8 render + flow gate 356 + `manage.py check` + both RS integrity gates + full client gate. ⚠️ The 6 DB
> pipeline tests (5 `test_compute_8824.py` + 4797 `test_diag_002_red_defers`) remain **blocked by the documented
> intermittent Supabase pooler outage** ("terminating connection due to administrator command" mid-seed) — re-run
> next session; logic proven independently. Commits `7d14f69`..`c1350c2`. RS hygiene: re-seed `load_4797` on next
> RS deploy (covers both 6252 + 8824). Detail → STATUS.md / 8824-kickoff memory.

> **2026-06-28 -- Form 6252 (Installment Sale Income) ★★ UNIT COMPLETE -- tag `1040-form-6252-complete` -- all 6 legs green; flow gate 339→347.**
> Ken chose Broad v1 (Parts I-III + §453A) 2026-06-28. SPEC + COMPUTE (prior session) + RENDER + DIAGNOSTICS +
> INPUT + ASSERTIONS (this session). COMPUTE: `InstallmentSale` model (1 row/sale, migs 0124/0125) +
> `compute_6252.py` (pure no-cache + `_db`→Sch 2 L15 §453A + `_face` + feeds) wired BEFORE `compute_4797_db`;
> **fully retired the 4797↔6252 RED-defer** (`D_4797_003` deactivated). RENDER: `render_6252_1040` (compute-driven
> face, one Form 6252/sale via fitz) + f6252 PDF trimmed-to-form + `f6252_2025.py` (49/49). DIAGNOSTICS:
> `rules_6252.py` 8 `D_6252_*` (v1 limit: no §1245-vs-§1250 flag → D_6252_007 heuristic). INPUT:
> `InstallmentSaleSerializer` + CRUD + `InstallmentSalesSection.tsx` + `installment_sales` tab (Income group) +
> removed vestigial `f4797_has_form_6252`; vite build + vitest 217/217. ASSERTIONS: 8 `FA-1040-6252-*` +
> `_run_6252_assertion` → gate 347. VERIFIED: 22/22 `test_compute_6252.py` + 9/9 diag + flow gate 347. Commits
> `0cec206`..`4c1b06b`. RESUME = next unit Form 8824 (§1031). Detail → STATUS.md.
> RENDER leg (2026-06-28): `render_6252_1040` (`renderer.py`) compute-driven face — reads `compute_6252_face`,
> ONE Form 6252 per non-deferred InstallmentSale row, concatenated via fitz (the `render_5329` multi-instance
> precedent; `render_4797_1040`/`render_1116` single-source discipline). f6252 PDF TRIMMED to the form-only page
> (irs.gov bundles 3 instruction pages) + manifest entry (SHA256 of trimmed) + field map `f6252_2025.py` (49/49
> AcroForm fields; line 19 GP% prints as a decimal). `f6252` in `ACROFORM_FORM_IDS`, wired after Form 4797.
> VERIFIED: 22/22 `test_compute_6252.py` (incl. 4 new render), flow gate 339 GREEN.
> Ken chose Broad v1 (Parts I-III + §453A) 2026-06-28. SPEC done prior session (RS, `form_6252_spec.json`).
> COMPUTE leg this session: NEW `InstallmentSale` model (one row/sale, proforma-rollable; migs 0124 + RLS 0125)
> + `compute_6252.py` (pure port of `load_6252.compute_6252`, NO cache, + `_db`/`_face` + cross-form feeds)
> wired into `compute_return` BEFORE `compute_4797_db`. **Closed the Form 4797↔6252 RED-defer** (Ken: "fully
> retire the flag"): `compute_4797` now consumes the 6252 feeds (line 4 §1231 / line 10 ordinary / line 15
> recapture) instead of deferring; `D_4797_003` retired (is_active=False stub); RS `load_4797.py` synced +
> `check_4797_integrity` ALL PASS (11 scenarios); F4797-G2 scenario removed; FA-1040-4797-07 dropped the 6252
> blocker. Routing: capital → Sch D (ST L4 / LT L11); business §1231 → 4797 L4 → Sch D L11; ordinary recapture
> → 4797 L15 → Sch 1 L4; unrecap §1250 → Sch D 25% WS; §453A interest → **Schedule 2 line 15**. Tests
> `test_compute_6252.py`. VERIFIED: 20/20 pure scenarios (6252+4797), RS integrity ALL PASS, **flow gate 339
> GREEN**, SCHD+4797 FAs 19/19, pipeline-t4 closure GREEN (328s under pooler latency). §453A: §6621 rate
> preparer-asserted/row; max ordinary/LTCG year-keyed (2025 37%/20%). Detail → STATUS.md.

> **2026-06-27 -- Form 4797 (Sales of Business Property) ★★ UNIT COMPLETE -- tag `1040-form-4797-complete` -- all legs green; flow gate 332→339.**
> Ken chose "Broad v1" (full Parts I-IV) 2026-06-26. SPEC (RS `29351c2`: `load_4797.py` modern-style +
> `check_4797_integrity.py`; seeded + `form_4797_spec.json`) + COMPUTE (`0e8c0b4`: migration 0123 — 9
> Disposition recapture fields + 7 Taxpayer `f4797_*`; `compute_4797.py` pure port + `compute_4797_result`/
> `_db`/`_face` no-cache, wired BEFORE Sch D netting → Sch 1 line 4 / Sch D line 11 / unrecap §1250→SDTW)
> + RENDER (`361f19c`: `render_4797_1040` compute-driven face, the render_1116 precedent; f4797_2025 map
> comments fixed) + DIAGNOSTICS (`a8d2bff`: `rules_4797.py` 7 `D_4797_*` bridge-gated) + INPUT (`132041b`:
> serializers expose the new fields; React Disposition recapture block + `Form4797ReturnFactsPanel`; tsc 0
> err / vitest 217) + ASSERTIONS (7 `FA-1040-4797-*`, `_run_4797_assertion`; gate 332→339). 34 tests (27
> in test_compute_4797 + 7 FA). v1 boundaries: Sch D line-11 face cell blank (number flows right), ≤4
> props/Part, Part IV net 35a/b only, 4684/6252/8824 RED-defer. The ENTITY-side `render_4797`
> (DepreciationAsset) is unchanged — `render_4797_1040` is the parallel 1040 path.

> **2026-06-26 -- Form 1116 (Foreign Tax Credit §901/§904) ★★ UNIT COMPLETE -- tag `1040-form-1116-complete` -- all 7 legs green.**
> INPUT leg (2026-06-26): `Form1116Section` React tab (Form8615Section singleton precedent) in `FormEditor.tsx` +
> `Form1116Row` + returnData wiring + tab dispatch; `IndividualNav.tsx` tab "Foreign Tax Credit (1116)" placed in the
> **Credits** group (the FTC lands on Schedule 3 line 1 — NOT "Other taxes"; flagged to Ken) + `D_1116_→form_1116`
> RULE_TAB_MAP + singleton has-data dot. GREEN inputs only (no computed model columns; credit shows on Sch 3 L1).
> Gates: tsc 86→84 (0 new; fixed 2 pre-existing — emptyRd `form_5329s` + duplicate `D_5329_` key), vitest 217/217,
> vite build clean; no backend change (flow gate stays 332). tts INPUT commit follows `a1f522a`.
> Ken chose FULL form, "Tighter v1 — Passive-only". SPEC (`load_1040_form_1116.py` + `check_1116_integrity.py` ALL
> PASS; seeded + `1116_spec.json`) + SEED (`Form1116` singleton, migs 0121/0122, `seed_1116` 31 lines, serializer +
> `form-1116` route, manifest `f1116`) + COMPUTE (`compute_1116.py` → Sch 3 line 1, wired FIRST among Sch-3 credits;
> 18/18) + RENDER (`f1116_2025.py` + `render_1116`, map vs 118-widget PDF; 7/7) + DIAGNOSTICS (8 `D_1116_*`; 11/11)
> + ASSERTIONS (7 `FA-1040-1116`; **flow gate 325 → 332**) all GREEN + committed. Two paths → Schedule 3 line 1:
> §904(j) election (≤ $300/$600 → min(tax, regular)) + full Passive §904 limit (L21 = L20 × L17/L18, L24 =
> min(L14,L23), carryforward; L18 = 1040 L15 + Sch 1-A L37 SENIOR ded only; L20 = 1040 L16 + Sch 2 L1z). v1
> RED-defers (8 D_1116_*): non-passive, above-exception QD, Form 2555, carryover→Sch B, exotic. **RESUME = React
> `Form1116Section` tab (Form8615Section precedent) → then tag `1040-form-1116-complete`.** RS `acf4078` / tts
> `9ec9f79`(seed)..`a1f522a`(assertions).
> **SLATE 2026-07-30 (s148, sweep unit 28)** — `SlateForm1116Screen` (the s132 singleton; no server change; the
> engine's 31-row FORM_1116 face published as ƒx cells from `field_values`, form_code-scoped). ⚠⚠ **"ALL 7 LEGS
> GREEN" IS NOT TRUE AND THIS ROW IS THE THIRD PROOF OF THE MISSING-COLUMNS ITEM** (SCH_1A, 8615, now 1116 —
> STATUS open item 31): **(a) NO E-FILE LEG** — no `IRS1116` builder while Schedule 3 line 1 transmits as
> `ForeignTaxCreditAmt`; correct on the §904(j) paths (no 1116 is due), but a FULL-path return is PAPER-ONLY
> (4th occurrence of the missing-document shape). **(b) RENDER LEG INCOMPLETE** — `f1116_2025.py` maps neither
> Part II checkbox (j) Paid/(k) Accrued ("you must check one" on the face), prints nothing in column (l) where
> i1116 directs "1099 taxes" for 1099-reported tax, fills only the (u) total with (q)–(t) blank, and leaves
> header box h "Resident of" empty. **(c) LATENT** — `ADJ_EXCEPTION_TI` falls back to the 2026 table for
> unpinned years and D_1116_007 covers only year==2026 (the year-fallback shape's 4th occurrence → LEG 4 item
> 17). Presentation fix shipped: the §904(j) ceiling copy now honors QSS = $600 (legacy keyed off MFJ alone).
> Constants re-verified vs live i1116 ($20,000; $394,600/$197,300). Live-proven both paths on the demo QA
> return (auto $250; full face 25,000 → L7 20,836 → L19 0.2644 → L24/L35 650 = Sch 3 L1), all 31 on-screen ƒx
> cells ORM-matched, PATCH lane proven (L2 reflow 20,836→20,736), demo REVERTED to zero drift (892/892).

> **2026-06-25 -- Form 5329 FULL (Parts I-IX) ★★ UNIT COMPLETE -- tag `1040-5329-full-complete`** (Ken chose
> FULL form + DUAL taxpayer/spouse). Additional Taxes on Qualified Plans: Part I early-distribution 10%/25%
> (full 01-23/99 exception catalog), Part II education/ABLE (10%), Parts III-VIII excess contributions (6% of
> smaller-of total-excess-or-12/31-value), Part IX excess accumulation / missed RMD (SECURE 2.0 10%/25%). Every
> part → Schedule 2 line 8 = the SUM of both owners. Dedicated `Form5329` model (one row/owner, 37 leaf fields
> + 6 nullable caps) + `RetirementDistribution.owner`; `compute_5329.py` (port of the verified RS `f5329_full`);
> model-driven dual render (one PDF/owner); `Form5329Section` React UI (9 parts); 5 dual `D_5329_*`; 6
> `FA-1040-5329-*` (gate 319→325). All 7 legs green. RS spec = the `F5329_*` blocks in `load_1040_retirement`
> (37/12/58/4/10). Commits RS `e9b042f` / tts `3128e28`..(assertions). v1: no contribution-limit modeling
> (preparer keys the leaves); SIMPLE-25% applies to all of L3 (flagged).



> **2026-06-25 -- STALE-LIST RECONCILE -- Form 2210 (Underpayment of Est. Tax, §6654) ★★ UNIT COMPLETE
> -- tag `1040-form-2210-complete` (built 2026-06-15; this tracker just never recorded it).** Full §6654:
> Part I required annual payment (90% / 100%-110% safe harbor, $1,000 de-minimis, prior-yr AGI >$150k→110%)
> + Part III regular quarterly method (7%/6% §6621 rate periods, four due dates) + Schedule AI annualized
> method → 1040 line 38. Seed `seed_form_2210` (`t2210_*` Taxpayer facts, mig 0082) / `compute_2210.py`
> (wired at END of the 1040 pass, after total tax + payments) / `f2210_2025.py` render / `rules_2210.py`
> (D_2210_NO_PENALTY + D_2210_PRIOR_YEAR) / 6 `FA-1040-2210-01..06` active in the 319 gate. All 6 legs
> verified GREEN today (5 leg files + flow gate, 354 passed). No RS spec (lookup 404) — local
> `2210_spec.json` + `check_2210_integrity.py`. v1 boundaries flagged (waiver/farmer/AI-full-tax deferred).

> **2026-06-25 -- Form 1040-X (Amended Returns) ★★ UNIT COMPLETE -- tag `1040x-complete` (Ken approved W1-W6).**
> Three-column A/B/C delta over a filed 1040 (A=as-filed baseline, C=corrected 1040, B=C−A). RS spec
> `load_1040_form_1040x.py` (seeded; 12 facts/12 rules/61 lines/8 diag/2 scenarios/6 FA; `check_1040x_integrity.py`
> ALL PASS; vs the real Form 1040-X Rev. Dec 2025). `AsFiledBaseline` (frozen Column A snapshot, snapshot-copy,
> auto-captures on mark-as-filed) + `Form1040X` singleton (migs 0117/0118). `compute_1040x` diffs the baseline vs
> the recomputed 1040 (verified map L6←1040 L18/L7←L21/L11←L24/L15←L28+L31; RED-defers carryback/superseding/
> cascade/missing-baseline). `render_1040x` (f1040x AcroForm, appended in the package) + 8 `D_1040X_*` + 6
> `FA-1040X`. Full suite 343 passed; flow gate 313→319. Open: Form1040XSection UI shows placeholders (wire to the
> computed FFVs); Part I deps recap + line 22/23 split = v1 blanks. RS `957a9d6` / tts `460abad`..`6a84ee0`.
> ⚠⚠ **s152 (unit 32, Slate screen) closed the placeholder item and found THREE more:** (1) the container NEVER
> UNWRAPPED the api envelope — `row` was the `{ok,status,data}` wrapper, so the legacy screen showed a permanently
> blank explanation / "Not captured" baseline / unticked flags since ship while every PATCH silently succeeded
> (3rd s146-envelope occurrence; FIXED at the container + pinned by `form1040xEnvelope.test.tsx`); (2) five
> writable amendment facts (lines 6/15/16/18/23) had NO input anywhere — the line-18 default-0 claims the original
> refund AGAIN on any refund-original amendment (FIXED: the Slate screen surfaces all five); (3) DELETE
> `/form-1040x/` leaves ~61 published FFV rows AND `is_amended_return` stuck true (the F8888-020 amended cap keeps
> firing) — proven live on the demo return; REVIEW_QUEUE s152, LEG 2-lane. Also: an amended 1040 has NO
> transmission path at all (MeF accepts e-filed 1040-X; the screen states paper-only) → REVIEW_QUEUE s152, its own
> LEG 3 item, NOT the missing-documents batch. `SlateForm1040XScreen` renders the engine's face; engagement keys
> off the /form-1040x/ ROW, never the FFV rows.

> **2026-06-25 -- GA Form 500 (Georgia Individual) IN PROGRESS** -- the FIRST state individual return.
> RS spec SEEDED + EXPORTED (500_spec.json: 75 facts / 20 rules / 76 lines / 12 diag / 12 tests / 12 FA;
> integrity gate ALL PASS; Ken approved W1-W6). v1 scope MAXIMAL: resident + part-year/nonresident (Sch 3),
> standard + military retirement exclusions, Sch 4 NOL (80% limit), Low Income Credit, IND-CR 202 child-care.
> Constants: rate 5.19% (2025) / 4.99% (2026, HB 463); dependent exemption $4k / $5k. Depreciation
> sec 168(k)/179/OBBBA decoupling = Sch 1 direct-entry (W1).
> **UNIT COMPLETE (v1)** (all 6 legs + create_state_return + UI: seed 9 sec/131 lines, compute byte-for-byte vs gate, render, 12 diagnostics, 12 flow assertions [gate 313], GA-500 data-entry tabs [client tsc 84/vitest 211/build clean]);
> attaches to the 1040 via the state_returns FK (GA-600S precedent).
> **★★ UNIT COMPLETE — tag `ga500-complete` 2026-06-25.** Render follow-ups A/B (`d1cea56`/`33a0ca8`) +
> the "Finish GA-500" pass: (1) line 42-46 reconciliation (`75b118b`/RS `14e4410`) — UET=L42/Interest=L44/
> Amount-Due=L45/Refund=L46 corrected + 10 gift check-offs L32-41 modeled (Ken: MAXIMAL); (2) HB 463 TY2026
> std deduction $15k/$30k (`ff36384`/RS `fd9ebc9`, Ken confirmed TY2026 over conflicting sources); (3)
> header-v2 (`7f97ba3`) — filing status/residency/dependents/DOB. The full printed Form 500 = identity
> header + lines 8-46 + full-package append. GA-500 suite 44/44 + flow gate 313 + RS integrity 14 scenarios.
> Open (low-priority): MI/suffix/DL/part-year-date-range; HB 463 effective-year vs the bill text (REVIEW_QUEUE);
> HB 463 annual step-ups begin TY2027 (not modeled). NEXT = Kens direction.

> **2026-06-24 — Form 8615 (Kiddie Tax §1(g)) UNIT FULLY COMPLETE** — all 6 legs + the QDCGT cap-gain follow-up, tag `1040-8615-complete` (ordinary + qualified-dividend/net-cap-gain).
> Singleton `form-8615` CRUD route + "Kiddie Tax (8615)" tab (Other taxes group); ordinary
> §1(g) core → 1040 line 16; QD/net-cap-gain + SDTW/Sch J/8814 RED-deferred. Flow gate 312
> 9 FA active (FA-1040-8615-01..09; gate 301). QDCGT cap-gain follow-up DONE (mig 0116
> sibling QD/cap-gain fields; Line 5 Worksheets 1/2/3; D_8615_008 retired; FA-06 flipped).
> Resume = Ken's direction. CPA-review care items: WS2/WS3 prorations. See the row + STATUS.md.
> ⚠ s147 ANNOTATION: "all 6 legs" was a CLAIM — no e-file leg exists (no IRS8615 builder → LEG 3),
> the render leg misses the whole parent header (LEG 3), and the compute leg's line-1 sourcing
> missed most unearned income kinds (→ LEG 2 item 7). ✅ s171 (2026-07-31): the line-1 sourcing
> is FIXED (i8615 rule: AGI shortcut / Child's Unearned Income Worksheet; Alternate-Worksheet
> cases RED-defer via NEW D_8615_009 — seed_rules at deploy). E-file + parent header remain open (LEG 3).
*Last updated: 2026-06-24 (**FORM 8829 (Business Use of Home) — ★ UNIT COMPLETE — ALL 6 LEGS GREEN — tag 1040-8829-complete (spec/seed/compute/render/input/diagnostics/assertions; flow gate 261→270 active 1040 FA, full test_flow_assertions.py suite 292/292).** NEW form after the quick-wins bundle (Ken chose Form 8829 home office, then "BROADER: build the Schedule A split"). No prior RS spec (`lookup/8829` → 404), spec-first. Verified vs the actual 2025 f8829.pdf (read directly) + i8829 (Rev. Mar 4 2026) + IRC §280A(c)(5) + §168/Pub 946. The actual-expense home-office engine: Part I business % (area + daycare hours-of-use) → the §280A(c)(5) gross-income limitation in 3 ordered tiers (deductible-anyway 9-14 → operating 16-27 → casualty+depreciation 29-33, each capped at the income remaining; depreciation last → carries over first) → line 36 → Schedule C line 30; Part III 39-yr nonresidential mid-month SL depreciation (Jan 2.461%…Dec 0.107% first year, 2.564% subsequent); Part IV carryover. v1 COMPUTES the RE-tax SALT split (lines 11/17) reusing `compute_schedule_a.salt_line5e` (the OBBBA $40k cap + $500k-MAGI 30% phasedown) incl. the >$500k circular MAGI↔home-office↔AGI iteration (W4 = fixed-point loop); the mortgage Pub-936 split (10/16) stays a preparer fact for itemizers (W3 — matches Sch A; standard-deduction path computed); RED-defers line-35 → Form 4684 + multi-business line-36. RS `load_1040_form_8829.py` (`b933b6e`) = 47 facts/13 rules/44 lines/8 diag/11 scenarios/9 FA/20 links/4 sources; `check_8829_integrity.py` ALL PASS (depreciation table + daycare + SALT + helpers + 11 scenarios re-derived; caught 2 self-authored col-(a)/(b) errors). Ken approved the review walk (W1-W6) → flipped READY_TO_SEED → seeded (Created 8829) → deployed export verified (HTTP 200) → canonical `server/specs/8829_spec.json` + 9 FA staged. LEGS 1-4 (SEED/COMPUTE/RENDER/INPUT) DONE + GREEN — seed `620cbb4`, compute `a572424`, render `372d15d`, input leg (writable Form8829 CRUD route `schedule-c/<sc_id>/forms-8829` mirroring ScheduleCOtherExpense + React Schedule C actual-expense panel, NO migration). LEGS 1-6 DONE + GREEN — UNIT COMPLETE (tag `1040-8829-complete`). LEG 5 (diagnostics, `1426df1`): 8 `D_8829_*` bridge-gated + `D_SC_007` narrowed in the diagnostic (not the helper). LEG 6 (assertions, this commit): merged 9 `FA-1040-8829-*` via NEW `scripts/merge_8829_assertions.py` + a NEW `_run_8829_assertion` (PURE `compute_8829` re-derivation + `inspect.getsource` source-pins); flow gate 261→270 active 1040 FA (full suite 292/292; the predicted "341→350" was stale — verify gate numbers empirically). RESUME = Ken's direction. The simplified $5/sq ft method already exists (`compute_schedule_c.py:252`); actual-expense is RED-deferred today via `D_SC_007`. Decisions: DECISIONS.md 2026-06-24 Form 8829. Prior: **QUICK-WINS BUNDLE — DONE. QW1 Simplified Method joint-survivor = ALREADY BUILT (verified end-to-end; the 2026-06-22 audit was stale — engine has the full Table-2 path, model/serializer/UI all wired, D_SM_003 lifts on survivor age; no work). QW2 Form 2441 claim gate = DONE + GREEN (commit `132d29e`): Ken chose amend-spec + gate-compute — the per-Dependent `claims_dependent_care` checkbox is now the authoritative §21 claim election, so a qualifying-but-unclaimed dependent computes $0 (reconciles `compute_2441` with the `d_credit_dc` diagnostic). Spec-first: `FORM_2441` R-2441-QUALIFYING amended (canonical `form_2441_spec.json` + RS loader, commit `93965c4`, NOT re-seeded — next-deploy TODO); `qualifying_dependents` gated on the flag; new `_is_sec21_qualifying`/`sec21_qualifying_unclaimed`; new `D_2441_007` (info) no-silent-gap nudge; `FA-1040-2441-07` + runner. Flow gate 282→283, 2441 tests 36/36, NO migration. RESUME = Proforma producer (audit #4), deferred to ~2026-06-29 (Ken back in office). Decisions: DECISIONS.md 2026-06-24 Form 2441. Prior: FORM 6251 (AMT engine) — UNIT COMPLETE — ALL 6 LEGS GREEN, tag `1040-6251-complete` (flow gate 273→282). LEG 6 (ASSERTIONS, commit `5b08564`, NO app-code/migration): merged the 9 staged `FA-1040-6251-01..09` via NEW `scripts/merge_6251_assertions.py` (flip staged→active — the W-2G lesson; pending emptied) + a NEW `_run_6251_assertion` runner (FA-1040-6251-* id-prefix in all 3 entry points; PURE `compute_6251` re-derivation + constant pins + `inspect.getsource` source-pins on `compute_6251_db`/`compute_return`): FA-01 L11→Sch 2 L2, FA-02 AMT=max(0,TMT−reg), FA-03 exemption phaseout (2026 OBBBA 50%/$500k), FA-04 TMT 26/28% breakpoint, FA-05 AMTI std add-back + QBI retained (no qbi param), FA-06 Part III reuses QDCGT worksheet, FA-07 constants year-keyed, FA-08 RED-defer no-silent-gap, FA-09 line 10 excludes Schedule J. Flow gate 273→282 (2.4s, pure). The full Form 6251 AMT engine now computes the common case → Sch 2 L2 → total tax (AMTI add-backs/exemption phaseout/26-28 TMT/Part III cap-gains), RED-defers ISO/QSBS/NOL/estate-trust/exotic/AMT-FTC + §1250-28% SDTW, renders the 2-pp face + AMT input card, fires 8 D_6251_*, gated by 9 FA. RESUME = Ken's direction (next DEFERRAL_AUDIT big-ticket). Decisions: DECISIONS.md 2026-06-24 leg 6. Prior: BUILD LEG 5 (DIAGNOSTICS) COMPLETE + GREEN (commit `fbc7805`). NEW `apps/diagnostics/rules_6251.py` wires the 8 `D_6251_*` from `6251_spec.json`, each BRIDGE-GATED to `compute_6251_face` (single source the render reads): `D_6251_001..006` (error) fire EXACTLY when the engine red-defers on that preference (read from the `red_defer` list; §1250/28% Part III SDTW routes under 005), `D_6251_007` (info) when AMT (line 11) > 0, `D_6251_008` (warning) when capital gains drove Part III; `RULES_6251` in `runner.seed_builtin_rules` (self-seeds on deploy). NARROWED `D_AMT_DEFER` (`rules_amt.py`): dropped private-activity-bond interest from `amt_indicators` (the engine now computes it as line 2g → `D_6251_007`; removes the AMT=0 false positive), KEPT K-1 AMT items (engine doesn't auto-import them — flagged v1 gap; the return-level twin of `D_K1_AMT`). Fixed a leg-2 fixture regression: `test_amt_red_defer._set_sch2_line2_amt` now sets `is_overridden=True` (as production does) so the engine preserves a manual Sch 2 L2 escape-hatch entry. Gates: 21/21 diagnostics+AMT, flow gate 273 (unchanged) + 6251 compute/render 35/35 (308 total), check/makemigrations clean, NO migration. RESUME = build leg 6 (assertions — LAST: merge 9 staged FA via `merge_6251_assertions.py` + `_run_6251_assertion`, gate 273→282, tag `1040-6251-complete`). Decisions: DECISIONS.md 2026-06-24 leg 5. Prior: BUILD LEG 4 (INPUT) COMPLETE + GREEN (commit `d9fd4ba`). PURE FRONTEND — the 11 writable `amt_*` Taxpayer facts (model migration 0111 + serializer from leg 1) are now editable in the React FormEditor: NEW `Form6251Section` on a NEW `form_6251` tab ("AMT (6251)") in the "Other taxes" nav group — 5 common-case add-backs as GREEN override inputs with YELLOW "blank = auto from <line>" hints (std L12 / SALT Sch A L7 / senior Sch 1-A L37 / refund Sch 1 L1; depreciation L2l direct) + 6 RED-defer preference items (ISO/QSBS/NOL/estate-trust/other/AMT-FTC) with RED "entering defers to manual" hints; a computed-AMT banner reads Schedule 2 line 2 (computed AMT > 0 / no-AMT note / RED defer warning). Diagnostic→tab routing wired ahead of leg 5: `RULE_TAB_MAP` += `D_6251_`→`form_6251` AND `D_AMT_`→`form_6251`; `IndividualNav.test.tsx` +2 routing cases. Gates: client tsc clean / vitest 205/205 (nav 86/86) / build clean; NO Python, NO migration → flow gate 273 unchanged. RESUME = build leg 5 (diagnostics: `rules_6251.py` wiring the 8 `D_6251_*` bridge-gated to `compute_6251_face` + narrow `D_AMT_DEFER`). Decisions: DECISIONS.md 2026-06-24 leg 4. Prior: BUILD LEG 3 (RENDER) COMPLETE + GREEN. The Form 6251 face prints when AMT > 0. `field_maps/f6251_2025.py` = the 62-widget AcroForm map, every acro_name VERIFIED vs the real 2025 f6251.pdf by position (page 1 f1_1..f1_33 = 2 header + Part I 1a-4 + Part II 5-11; page 2 f2_1..f2_29 = Part III 12-40). NEW `compute_6251.compute_6251_face` (render+diagnostics single source) re-derives via `compute_6251_result` from the persisted 1040 rows + adds Part I 1a/1b — **W1 RESOLVED:** 1a = 1040 L12+L13 (deductions − senior), 1b = AGI(L11) − 1a = taxable income + restored senior; 1b+2a+2b+2g+2l == engine line 4 (AMTI) by construction. `renderer.render_6251` MODEL-DRIVEN, gated on line 11 > 0 (RED-defer/AMT-free → prints nothing; the D_6251_* REDs own the deferred case); fills Part I (1a-4) + Part II (5-11) + Part III anchors 12/40 when the cap-gains worksheet is used; f6251 ∈ ACROFORM_FORM_IDS; appended after Form 8959 (seq 32); page 2 skipped via SKIP_PAGES unless part_iii. `test_6251_render_leg.py` 9/9 (field map vs PDF existence/no-dup/full-62 + face render + 4 DB pipeline incl. rendered L11 == Sch 2 line 2 + 1a/1b reconciliation + no-print gates); flow gate 273 unchanged; NO migration. v1 flagged: Part III intermediate 13-39 not surfaced (0/15/20% breakdown inside the reused QDCGT WS); §1250/28% Part III RED-deferred (leg 2). RESUME = build leg 4 (input: surface the 11 `amt_*` facts in the React FormEditor — a Form 6251 AMT card). Decisions: DECISIONS.md 2026-06-24 leg 3. Prior: BUILD LEG 2 (COMPUTE) COMPLETE + GREEN.  NEW `apps/returns/compute_6251.py` = the AMT engine. Pure `compute_6251` is a Decimal port of the RS loader + gate math (constants + line-5 exemption phaseout [2026 OBBBA $500k/$1M @ 50%] + 26/28% TMT BYTE-FOR-BYTE; the 10 spec scenarios re-derive). AMTI base = taxable income + std-ded/SALT add-back + senior add-back (QBI retained) + PAB 2g + depreciation 2l − refund 2b → exemption → 26/28% TMT (or Part III) → AMT = max(0, TMT − regular tax) → line 11. Part III reuses `compute_qdcgt_worksheet` on the AMT base with the 26/28% rate as the `ordinary_tax_fn` (0/15/20% case computed; §1250/28%-rate SDTW path RED-deferred, W4). DB wrapper `compute_6251_db` gathers the 1040 quantities (taxable income, PRE-Sch-J regular tax for line 10, std/SALT/senior add-backs defaulted from the return + `amt_*` override, PAB aggregate, cap-gain inputs), writes Schedule 2 line 2, reflows via `compute_sch_2` → 1040 line 17 → total tax (the Form 8962 late-feeder precedent); WIRED into `compute_return` after `_compute_1040_tax`, before Schedule J + the first downstream pass. RED-defer guard (6 facts + §1250/28%) BLANKS line 11 (no silent gap; manual escape-hatch override preserved). Common case AMT = 0 → line 2 blank → no reflow → no regression. `test_6251_compute_leg.py` 26/26 (20 pure incl. 10-scenario math gate + 6 DB incl. full-pipeline) + flow gate 273 + 8995-A/Topic 8 regression 70/70 + check/makemigrations clean; NO migration. v1 flagged: Part III §1250/28% deferred, line-10 Sch2-L1z/Sch3-L1/4972 adjustments deferred, itemizing uses the elect flag, Part III intermediate rounding unpinned. RESUME = build leg 3 (render: `f6251` 2-pp face, 62 widgets; PIN 1a/1b vs the real 1040 lines, W1; single-source via `compute_6251_result`). Decisions: DECISIONS.md 2026-06-24 leg 2. Prior: BUILD LEG 1 (SEED) COMPLETE + GREEN.  Migration `0111` adds 11 return-level `Taxpayer` AMT facts (5 common-case add-backs the engine computes — `amt_salt_deduction`/`amt_standard_deduction`/`amt_senior_deduction`/`amt_tax_refund`/`amt_depreciation_adj` — + 6 RED-defer preferences `amt_iso_exercise`/`amt_qsbs`/`amt_nol`/`amt_estate_trust`/`amt_other_preference`/`amt_ftc`), all DecimalField(15,2) default 0, NO RLS migration (scalar cols on the RLS-enabled `returns_taxpayer`, 0110 precedent); `amt_pab_interest` is NOT a field (line 2g derived from the existing `pab_interest` aggregate per the STATUS lock). Manifest `f6251` added + downloaded (2pp, 62 AcroForm widgets, SHA recorded). `seed_6251.py` (the `seed_8995a` template) seeds 38 lines / 3 Parts from canonical `6251_spec.json` line_map (`is_computed` follows line_type; idempotent; build.sh auto-discovers). `TaxpayerSerializer` exposes the 11 facts WRITABLE (GREEN). Stale trip-wire fixed: `test_tts_forms.py` manifest count 25→54. `test_6251_seed_leg.py` 10/10 + check/makemigrations clean + flow gate 273 unchanged. RESUME = build leg 2 (`compute_6251.py` — the engine from the spec rules, byte-for-byte vs `check_6251_integrity.py`; reuse `compute_regular_tax` for line 10 + QDCGT/SDTW for Part III; → Schedule 2 line 2). Decisions: DECISIONS.md 2026-06-24 leg 1. Prior: **FORM 8995-A (above-§199A-threshold QBI) — UNIT COMPLETE — ALL 6 LEGS GREEN — tag `1040-8995a-complete` (flow gate 264→273). Above the §199A threshold the full Form 8995-A now computes the QBI deduction → 1040 line 13 (Parts I-IV + Schedule A SSTB % + Schedule B aggregation + Schedule C loss-netting), renders the 2-pp face + statement pages, takes the per-business inputs, fires the 7 D_8995A_* diagnostics, and is gated by 9 flow assertions. v1 RED-defers the Schedule D patron reduction (L14) + §199A(g) DPAD (L38) → L13 blank + D_8995A_001/002 + the narrowed D_8995_001 (no silent gap). LEG 6 (ASSERTIONS): merged the 9 staged FA-1040-8995A-01..09 via `scripts/merge_8995a_assertions.py` (flips staged→active — the W-2G lesson) + a NEW `_run_8995a_assertion` runner (FA-1040-8995A-* id-prefix dispatch in all 3 `test_flow_assertions.py` entry points, the schf/schj/k1 precedent) — PURE re-derivation through `compute_8995a` for the math (FA-02..06), constants pins (FA-07), `inspect.getsource` source-pins on `compute_8995_db`/`compute_8995a_db` for the cross-form flow + routing + no-silent-gap (FA-01/08/09); flow gate 264→273, no app code change, pending emptied. LEG 5 (DIAGNOSTICS): NEW `apps/diagnostics/rules_8995a.py` wires the 7 positive-validation D_8995A_* (001 patron / 002 DPAD errors fire at ANY income — the no-silent-gap coverage below threshold that the above-threshold-only D_8995_001 misses; 003 aggregation-unverified warn / 004 SSTB-above-ceiling info / 005 no-W2-UBIA warn [reads the POST-aggregation `col_lines`, not raw businesses] / 006 loss-carryforward info / 007 SSTB-in-aggregation error — gate on the above-threshold 8995-A regime so the simplified-8995 D_8995_002..005 own below-threshold), each bridge-gated to the `compute_8995a` helpers; registered via `RULES_8995A` in `runner.seed_builtin_rules` (self-seeds on deploy via `build.sh seed_rules`). `_gather_8995a_businesses` gains a display-only `label` (the pure engine ignores it) so the diagnostics name the EXACT businesses the engine computes — zero drift. D_8995_001 already narrowed in leg 2 (the complementary RED owning the line-13 blank). `test_8995a_diagnostics_leg.py` 14/14 + 8995a compute/render/input regression 31/31 + flow gate 264; NO migration; check/makemigrations clean. GOTCHA: lingering test_postgres → `scripts/drop_test_db.py` between DB-test runs. Decisions: DECISIONS.md 2026-06-23 leg 5. RESUME = leg 6 (merge the 9 staged `flow_assertions_1040_8995a_pending.json`, gate 264→273, tag `1040-8995a-complete`). Prior leg 2 (COMPUTE) — GREEN. NEW `apps/returns/compute_8995a.py` = byte-for-byte port of `check_8995a_integrity.py` (RS): Schedule A SSTB applicable% + Schedule B aggregation combine + Schedule C loss netting + Part II W-2/UBIA limit + Part III phase-in + Part IV REIT/PTP + income limit → L39 → 1040 line 13. `_gather_8995a_businesses` (ScheduleC + non-accrual ScheduleF + each §199A-bearing ScheduleK1) + `compute_8995a_db`→`(red_defer,result)` + `compute_8995a_red_defer`. WIRED into `compute_schedule_c.compute_8995_db` above-threshold branch (net_cap_gain hoisted; → L13 = `_q(L39)` INSTEAD of the old blank; simplified-8995 rows still blanked — 8995-A is the render-leg face). `D_8995_001` NARROWED (coupled to compute — its "line 13 blank/not built" message became false): bridge-gated to `compute_8995a_red_defer`, fires only for the patron/DPAD Schedule D gap. KNOWN v1 GAP (flagged): no patron/DPAD INPUT field → red_defer DORMANT (above-threshold ag-coop patron computes an overstated number) — guard wired, re-arms when the input lands. `test_8995a_compute_leg.py` 16/16 (10-scenario math gate + 4 pipeline + 2 const); flow gate 264; check + makemigrations clean; NO migration. Updated 2 obsolete tests (topic8 above-threshold → L13=0.00 not blank; pure-farm-above-threshold → L13=0.00, D_8995_001 silent). GOTCHA: lingering test_postgres → `DROP DATABASE test_postgres WITH (FORCE)`. Decisions: DECISIONS.md 2026-06-23 leg 2. RESUME = leg 3 (render: un-stub `f8995a_2025.py` Parts I-IV + Sch A/B/C faces, single-source off `compute_8995a`, resolve W7). Prior leg 1 (SEED) `c6a4234`: mig 0108 per-business W-2/UBIA/SSTB/aggregation + `QBIAggregation` + `seed_8995a` + 13/13.** **FORM 8582 PER-ACTIVITY — STAGE B (passive Sch C / Sch F) — UNIT COMPLETE — ALL LEGS GREEN — tag `1040-8582-per-activity-stage-b-complete` (flow gate 264). Extends the per-activity §469 allocation to passive (material_participation is False) Schedule C / Schedule F → Form 8582 Part V, closing the last 2 activity types. PURE ENGINE UNCHANGED (Sch C/F are bucket-V activities); no RS amendment (R-8582-WS-NET already names them), no migration (cols from 0107). compute `93c0208` (`_gather_passive_business` + keymaps dict; MAGI add-back via `form_8582_magi(passive_cf_loss=)` since a passive Sch C/F loss is already in first-pass AGI via Sch 1 L3/L6, unlike rentals/K-1; Sch 1 L3/L6 overwrite = active-net+passive-income−passive-allowed; QBI excludes suspended via `_adjust_passive_cf_qbi`; SE untouched—moot; test_schedule_e_8582_stage_b 7/7 incl. MAGI-addback headline special-allowance $15k-not-$20k). diagnostics `0cadf27` (`d_sf_passive` retired-by-narrow → fires only for un-allocated residual; `d_sc_006`/`d_sf_loss` reworded; test_schedule_f_diagnostics_leg 17/17). input (ScheduleC/F serializers mark allowed/suspended read-only; FormEditor 8582 block on the Sch C/F cards; test_..._stage_b_input 4/4, tsc/vitest 203). render (Sch C/F surface on Part V/VII/VIII with "Sch C"/"Sch F" labels; test_..._stage_b_render 2/2). assertions = flow gate 264 holds, no new FA (FA-05/06/07 type-agnostic). Part IX still RED-deferred (D_8582_MULTIFORM). Known v1 edges flagged (income+prior, QBI vintage, passive-income SE). Decisions: DECISIONS.md 2026-06-23 Stage B. GOTCHA: Sch C/F POST recomputes → YELLOW cols computed post-POST. **STAGE A (rentals + passive K-1):** UNIT COMPLETE — ALL 6 LEGS GREEN — tag `1040-8582-per-activity-complete` (flow gate 264).** Legs 4-6: INPUT (`d071a89`) = serializers expose the 3 cols on RentalProperty+ScheduleK1 (prior-unallowed GREEN/writable; allowed/suspended YELLOW/read-only) + FormEditor.tsx 8582 block on the rental + K-1 cards; test_schedule_e_8582_input_leg 4/4, tsc/vitest 203/build clean. DIAGNOSTICS (`39cdef6`) = `d_k1_passive_loss` NARROWED (bridge-gated on persisted allowed+suspended==0 → fires only for UN-allocated passive losses: PTP/un-engaged; non-PTP now allocated → no RED; d_k1_ptp_loss warning kept) + NEW `d_8582_multiform` RED (Part IX: passive activity allocated by 8582 with a §1231/28%/§1250 component = losses on 2+ forms; interpretation flagged — §1231/§1250-on-passive-loss proxy since the model can't detect same-activity-on-2-forms); 28 tests; self-seeds on deploy. ASSERTIONS (`39cdef6`) = merged FA-1040-8582-05/06/07 (conservation/loss-ratio/Part-IX-RED) via scripts/merge_8582_per_activity_assertions.py + extended `_run_8582_assertion` (per-activity ids dispatch first); flow gate 261→264, pending emptied. **RESUME = STAGE B (passive Sch C/F) or Ken's direction** — extend `_gather_passive_activities` for ScheduleC/F passive rows + Sch 1 L3/L6 add-back (cols exist on Sch C/F from 0107). Stage A legs: SPEC LEG + BUILD LEGS 1 (seed: migration 0107, per-row `prior_year_unallowed_passive` + computed `passive_8582_allowed`/`_suspended` × RentalProperty/ScheduleC/ScheduleF/ScheduleK1) + 2 (compute STAGE A) + 3 (render Parts IV-VIII) DONE + GREEN. LEG 3 (render `78ccded`): Form 8582 Parts IV-VIII now PRINT, model-driven via the shared pure `per_activity_allocation` (extracted as the SINGLE SOURCE OF TRUTH for compute+render; `compute_8582_per_activity` attaches `out["face"]`, per_activity/C/aggregate BYTE-IDENTICAL). Field map 17→**175** fields from the real f8582.pdf widget names (Table_Part4/5/6/7/PartVIII; 5 activity rows + a totals row/part; ratio cols rendered as TEXT). `render_8582._f8582_per_activity` fills Part IV (active rental) / V (other passive incl. passive K-1) / VI (special-allowance alloc, line 9>0) / VII (unallowed alloc) / VIII (allowed loss) + per-col totals + the "form or schedule" reporting-line column (Sch E 22/28/33); VI-VIII only when line C>0; >5 activities/part → supporting statement page (no silent gap); Part IX RED-deferred. test_schedule_e_8582_render_leg **12/12** (shape 175 + single-rental+prior row-1 positions + 2-rental 3:1 ratio split totals + passive-K-1 Part V/VIII + 6-rental overflow statement) + engine 10/10 + flow gate **261** + compute/diagnostics regression 30. NEXT = leg 4 (input: expose `prior_year_unallowed_passive` GREEN + `passive_8582_allowed`/`_suspended` YELLOW on the RentalProperty + ScheduleK1 serializers/UI; Stage A scope) → diagnostics (add `D_8582_MULTIFORM`; RETIRE `d_k1_passive_loss` keep PTP) → assertions (merge 3 staged FA, gate 261→264) → tag `1040-8582-per-activity-complete`. Leg 2: pure `compute_8582_per_activity` engine (10/10 vs PA1/PA2/PA3 + conservation + col-g wiring); `_gather_passive_activities` wires rentals (Part IV/V) + passive NON-PTP K-1 (Part V) through Schedule E p1 line 22 / p2 col (g) → line 41 → Sch 1 L5; `_persist_per_activity` writes the handoff columns; `_inject_legacy_prior` folds the aggregate field pro-rata. Ken-approved scope: PTPs OFF 8582 (§469(k)/i8582), Sch C/F = Stage B fast-follow. `seed_8582` +6 lines (C,P4-P8 → 23). test_schedule_e_8582_compute_leg spec 10 + pipeline 4 (incl. passive-K-1-fed PA2-shape + PTP-off) + K-1 compute/render 58 + flow gate 261. `d_k1_passive_loss` stale until leg-5 retire (flagged). NEXT = render (f8582 Parts IV-VIII model-driven) → input → diagnostics (RETIRE d_k1_passive_loss + add D_8582_MULTIFORM) → assertions → tag. Commits leg2a `11873bd` + leg2b (this). SPEC LEG was DONE + SEEDED + EXPORTED + STAGED (Ken-approved walk).** AUDIT CORRECTION (the Phase-0 lesson): the Schedule E p1 + Form 8582 AGGREGATE unit was ALREADY built + tagged `1040-schedule-e-8582-complete`; the Tier-2 "8582 fed by K-1/Sch C-E-F" item = the per-activity §469 allocation EXTENSION (passive K-1/C/F losses were RED-deferred via `d_k1_passive_loss`, never feeding 8582's Part V). Ken chose FULL per-activity (Parts IV-VIII), Part IX RED-deferred, all 4 activity types feed Part V. Verified vs the real 2025 f8582.pdf (3 pages, 205 widgets — Parts IV-IX FILED) + i8582. RS `load_1040_schedule_e.py` amended (FORM_8582 only, `--only` scoped → SCHEDULE_E untouched): +6 rules (WS-NET/ALLOC-VI/ALLOC-VII/ALLOWED-VIII/CARRYFWD/MULTIFORM) +`f8582_line_c` +6 lines (C,P4-P8) +`D_8582_MULTIFORM` RED +4 scenarios (PA1/PA2/PA3 worked math + PG1) +12 links +Parts IV-IX excerpt +3 FA → FORM_8582 **18/12/23/7/11/20 + 9 FA**; `check_schedule_e_8582_integrity.py` independent per-activity recompute **ALL PASS**; deployed export HTTP 200; canonical `server/specs/form_8582_spec.json` re-committed + 3 FA staged (`flow_assertions_1040_sche_8582_pending.json`); RS `9f34027` (pushed). Core mechanic: line C = (line 3 loss) − line 9; Part VI/VII allocate by loss-ratio (Σ Part VII = line C); Part VIII allowed = loss − unallowed → its schedule; Σ allowed = line 11. NEXT = build leg 1 (per-activity prior-unallowed field on RentalProperty/ScheduleK1/ScheduleC/ScheduleF + migration) → compute → render (f8582 Parts IV-VIII) → input → diagnostics (RETIRE `d_k1_passive_loss`; add `D_8582_MULTIFORM`) → assertions (merge 3 FA) → tag `1040-8582-per-activity-complete`. Brief: `server/specs/_schedule_e_8582_per_activity_brief.md`. Prior: **TIER 1 ITEM 3 LEG 2 (DD-ATTESTATION HARD PRINT-GATE) DONE + GREEN (9/9): `tts_forms/views.py` `due_diligence_print_blockers`+`_enforce_print_gate` hard-block BOTH return-level print endpoints (`render-pdf` AND `render-complete` — closes the bypass; no-ops for entities) for a 1040 when (a) `preparer_id is None` OR (b) a §6695(g) covered credit is claimed with incomplete due diligence; condition (b) BRIDGE-GATES to `d_8867_001_due_diligence_unanswered` (the exact UI diagnostic — can't disagree). Ken: block on BOTH, 1040-only, allow an audit-logged override, HTTP 400. `override:true` (body or `?override=`) bypasses + proceeds. NEW first-class `AuditAction.PRINT` (migration `audit/0002` = choices-only AlterField, SQL `(no-op)`) + `log_print` helper: blocked→`event=blocked`, override→`event=override` (both `{blockers,endpoint}`); clean render logs nothing. `tests/test_print_gate.py` 9/9 (renderer mocked) + combined Item 3 run 30/30. **KEN DECLARED ITEM 3 COVERED** — the office-usability trio (Tier 1 Items 1-3: proforma / "$0-limited" explainer / DD-attestation+preparer-of-record) is COMPLETE; leg 3 (contemporaneous DD notes) is a tracked follow-up. NEXT = Ken's direction on the next tier (DEFERRAL_AUDIT.md: Form 8582 fed by K-1/Sch C-E-F [highest volume], Form 8995-A above §199A threshold, full Form 6251 AMT engine). Prior: **TIER 1 ITEM 3 LEG 1 (PREPARER-OF-RECORD) DONE + GREEN (8/8): preparer-of-record = the existing `TaxReturn.preparer` FK (Ken decision — no new field, no migration). Two genuine gaps shipped (the rest — `_sync_preparer_info`→`PreparerInfo`→signature render + the FormEditor/ReturnManager assign UI — ALREADY EXISTED): (1) ENFORCE — new `apps/diagnostics/rules_preparer.py` `D_PREPARER_001` (WARNING, Ken-chosen: warning now / hard-block at the print-gate; fires when the 1040's `preparer_id is None`; `_ctx_1040`-gated no-op for entities; `RULES_PREPARER` in `runner.py`; self-seeds on deploy via `build.sh seed_rules`); (2) AUDIT ATTRIBUTION — `update_info` saved the model directly, bypassing `AuditViewSetMixin.perform_update`, so the §6695 assignment went unlogged → `snapshot`+`log_update` added; GOTCHA fixed: `audit/service._safe_value` only stringified Models, so a changed `preparer_id`(UUID)/`signature_date`(date)/`tentative_tax`(Decimal) crashed the `changes` JSONField → added UUID/date/datetime/Decimal→str (fixes ALL audited FK/date/Decimal updates, lossless display-only). `tests/test_preparer_of_record.py` 8/8 + `test_audit.py` 13/13; no compute math → no flow-assertion gate. NEXT = LEG 2: DD-attestation HARD print-gate (block `render-pdf` when a §6695(g) covered credit is claimed + not attested AND/OR no preparer-of-record; log the blocked attempt; confirm HTTP code + scope with Ken). Prior: **TIER 1 ITEM 2 (EXPLAINER) LEG 1 — §179 income-limitation diagnostics DONE + GREEN: new `apps/diagnostics/rules_4562.py` wires `D_4562_011` (warning, spec D011 — current-year election line 9 > income limit line 11) + `D_4562_014` (info, spec D014 — carryover line 13 > 0), both BRIDGE-GATED on the persisted Taxpayer §179 fields (the same source `render_4562` reads → can't disagree with the math); line 9 derived = L12+L13−L10; reads `sec_179_business_income_limit` for L11 (the COMPUTED limit, NOT the `taxable_income_limitation` override field). 9/9 `test_section_179_diagnostics.py` (3 no-DB trip-wires + 6 DB incl. 1 end-to-end through `compute_return`). Surfaces via the EXISTING diagnostics path (`run_diagnostics`→`DiagnosticsTab` renders info) — no UI work. Self-seeds in prod on deploy (`build.sh` runs `seed_rules` pass 2) — NOT manually seeded (avoids the dev-shares-prod race window). SCOPING: the CREDIT "$0/limited" explainer already existed (`credit_gates.py` EIC/CTC/ODC/DC/AOTC + the existing D_SC_005/D_8812_004/8582/8962 limitation explainers); §179 was the one genuinely-uncovered surface. Interpretation flagged: read D011's `elected_179` as line 9 (distinct from D014), not `(L9+L10)`. NEXT = Ken's call: Item 2 sufficient → Item 3 (DD-attestation + preparer-of-record), or more non-credit explainers (Sch 1-A ITIN, generic-message strengthening). NOT committed at write time.** Prior: **TIER 1 PROFORMA (item 1) — COMPLETE (read/roll side). Legs 2b+3+4 landed: `_populate_individual_from_prior_year` roll-forward (views.py, hooked into create_return; snapshot-COPY of demographics + dependents + the 5 carryforwards [§179 prior 4562 L13→CY L10] + 8606 basis; `_PROFORMA_*_KEYS` document the 1040 snapshot contract; test_proforma_1040 6/6), §179 serializer surface + UI (provenance banner + Form 4562 §179 card), and the 4 FA-4562-179-* merged into the active gate (257→261). REMAINING: the snapshot PRODUCER (TaxWise importer / app-to-app snapshotter) — flagged in STATUS, not silent. Prior: BUILD LEG 2a (§179 CONSUMPTION COMPUTE + RENDER) DONE + GREEN: NEW 1040-gated `compute_section_179_limitation` (compute.py, wired into compute_return after aggregate_1040_income, before the farm/biz families) computes Form 4562 lines 9-13 per the seeded spec (R001/R011/R014/R015) — L11 = override (`Taxpayer.taxable_income_limitation`) else SUGGESTION (Σ Sch C net + Σ Sch F net BEFORE §179 + 1040 L1a wages, MFJ combined, min L5, floor 0; circularity broken via net-before-§179), L12 = min(L9+L10, L11), L13 = (L9+L10)−L12; L12 distributed PRO-RATA by current §179 elected (fallback positive net) into each business's depreciation column (Sch C L13 / Sch F L14) so AGI/SE/QBI reflect it; `landed<L12` guard = no homeless carryover; common case BYTE-IDENTICAL; entities untouched. Migration 0106 (additive, applied) = 4 Taxpayer fields (`taxable_income_limitation` override + computed `sec_179_business_income_limit`/`sec_179_deduction`/`sec_179_carryover_next`). `render_4562` shows lines 10-13 for individuals from the persisted values (Part I now renders for a pure carryover too). `test_section_179_carryover.py` 12/12 (5 PURE spec scenarios + 6 PIPELINE incl. pro-rata 30:20 split + income-limited net floor + 1 RENDER); regression topic8+schedule_f 71+2 + flow gate 257 + manage.py check clean. ENV gotcha: `poetry run` intermittently hangs >120s — invoke the venv python directly. NEXT = Leg 2b/3 (1040 `_populate_*_from_prior_year` roll-forward [PY L13→CY L10] + UI + serializer override input) then Leg 4 (merge 4 staged FA-4562-179-*, gate 257→261). Prior: §179 RS SPEC LEG DONE + BUILD LEG 1 DONE: the mandatory 4562 spec fetch found it SILENT on Form 4562 line 10 (carryover in)/line 13 (out) + a WRONG line-12 formula → Ken chose "amend RS spec first, then build inline" → NEW `load_4562_section179_carryover.py` AMENDS the existing multi-entity 4562 ADDITIVELY (entity_types untouched): fact section_179_carryover_prior + L10/L13 + FIXED L12 (add 9+10, cap at 11) + enriched L11 (Individuals business-income def incl. W-2 wages 1040 L1a, w/o §179/§164(f)/NOL, MFJ combine) + R014 [L12=min(L9+L10,L11)]/R015 [L13=(L9+L10)−L12] + enriched R011 + D014 (proforma continuity) + 3 verbatim excerpts + 4 staged FA; math gate check_4562_section179_integrity.py ALL PASS; Ken-approved review walk → seeded onto 4562 v1 (DB facts 19→20/rules 10→12/lines 24→26/diags 8→9/tests 9→14) → deployed export HTTP 200 → canonical server/specs/form_4562_spec.json re-committed + flow_assertions_4562_section179_pending.json staged (RS 96a0dd6, tts e3f83b6). Law verified vs 2025 f4562.pdf (pymupdf) + i4562 (irs.gov), NOT memory. BUILD LEG 1 (model/migration): migration 0105 additive — Taxpayer.sec_179_carryover_prior (5th proforma carryforward; other 4 already typed) + TaxReturn.rolled_from_year/rolled_from_source (provenance pointer, snapshot semantics not a live FK); makemigrations --check clean; flow gate 257; tts bc9ead1. 2 BUILD DECISIONS LOCKED (DECISIONS.md): (1) line 11 = compute SUGGESTED (Σ Sch C + Σ Sch F net before §179 + W-2 wages L1a, MFJ combine, floor 0, YELLOW) + preparer OVERRIDE; (2) consumed carryover lands PRO-RATA across all §179 businesses. NEXT = Leg 2a §179 consumption compute+render (renderer.py:1748 + aggregate_depreciation + per-business §179 flow) then roll-forward+UI (Leg 2b/3) then tests+FA merge (Leg 4, gate 257→261). Architecture: snapshot-COPY (Ken-approved). Prior: Schedule J (farm income averaging) — UNIT COMPLETE — ALL 6 LEGS GREEN — tag `1040-schedule-j-complete`: build leg 6 (ASSERTIONS) merged the 8 staged FA-1040-SCHJ-01..08 into the active gate (**249 → 257**) via `merge_schedule_j_assertions.py` + a `_run_schj_assertion` runner (id-prefix dispatch in all 3 `test_flow_assertions.py` entry points, the `_run_schf_assertion` precedent) — PURE chain re-derivation (FA-02/03/07) + source-/constant-pins (FA-01/04/05/06) + no-silent-gap registry+blocker (FA-08); `test_flow_assertions.py` 257 passed, no app code change. Prior leg-5 (DIAGNOSTICS): `rules_schedule_j.py` (NEW) wires the 5 D_SJ_* (3 error/2 warning) + NEW pure `compute_regular_tax` for D_SJ_ELECT_HIGH; 12/12 + 5 rules seeded active. NEXT = Ken's call on the next unit (SPRINT_SCOPE NEXT-UP leads with Schedule C + SE + 8995 + 4562). Still tracked: deferred base-year QDCGT/SDTW statement pages (Sch J render follow-up). See the Schedule J row. Prior leg-1..4: **Schedule J — SPEC LEG COMPLETE — SEEDED + EXPORTED + STAGED (Ken-approved review walk): SCHEDULE_J seeded into RS (DB 65→66 forms, FA 232→240; 42 facts/20 rules/25 lines/5 diagnostics/10 scenarios/23 cited links + 8 FA), canonical `schedule_j_spec.json` committed + 8 FA staged in `flow_assertions_1040_schedule_j_pending.json`; math gate `check_schedule_j_integrity.py` ALL CHECKS PASS (independent SJ-T1 chain L23=31,982 + cell-by-cell constant cross-check); law verified vs the actual 2025 IRS PDFs (23-line chain + base-year rate schedules 2022/23/24 + base-year QDCGT + the identical-47-line Schedule D Tax Worksheet) → brief `_schedule_j_source_brief.md`; walk rulings Q-A reduce/Q-B half-up/Q-C 44-46/Q-D warning. NEXT = build leg 1 (ScheduleJ model + seed + merge base-year schedules into spine TAX_BRACKETS). See the Schedule J row. Prior: **Schedule F (1040 farm) — UNIT COMPLETE — ALL 6 LEGS GREEN — tag `1040-schedule-f-complete`**: leg 6 (assertions) merged the 8 staged FA-1040-SCHF-01..08 into the active gate (**241 → 249**) via `merge_schedule_f_assertions.py` + a new `_run_schf_assertion` runner (id-prefix dispatch in all 3 entry points, the `_run_k1_assertion` precedent) — PURE re-derivation via `compute_schedule_f` helpers for the cash math (FA-01 L34=L9−L33 + dual SE-1a/Sch1-L6 pins; FA-02 line-9 right-column sum=49,250; FA-03 L33 incl. other-expenses + line-14; FA-08 farm-optional min(⅔×gross,$7,240) + 2025 constants + 2026 unsupported), source-pins for the DB feeders (FA-04 depreciation→L14, FA-05 farm QBI→8995 L2 accrual-excluded, FA-06 CRP→SE 1b negative), no-silent-gap registry check FA-07. `test_flow_assertions.py` → **249 passed**; no app code change. **THE UNIT IS DONE — all 6 build legs green (seed/compute/render/input/diagnostics/assertions); tagged `1040-schedule-f-complete`.** NEXT = Schedule J (farm income averaging — its OWN RS spec, spec-first). Prior: **Schedule F (1040 farm) — BUILD LEG 5 (DIAGNOSTICS) COMPLETE + GREEN**: new `rules_schedule_f.py` wires the 8 D_SF_* (ACCRUAL/PASSIVE/ATRISK errors; 461L/CCC_ELECTION/CROPINS_DEFER/WEATHER_DEFER warnings; LOSS info) + the 2 D_SE_FARMOPT (2026 error / INELIG warning) via `RULES_SCHEDULE_F` in `runner.seed_builtin_rules`, each bridge-gated to the `compute_schedule_f` helpers (loss rules re-derive line 34 + skip accrual farms; SE rules gate on `farm_optional_supported`/`farm_optional_eligible`) with offending farm/proprietor names appended; **D_SF_461L = flat $100k per-farm-loss operational reminder (Ken-approved — §461(l) is aggregate)**; **`d_8995_001_above_threshold` made farm-aware** (`farms=bool(qbi_farms)` → no silent gap on a pure-farm-above-threshold return). 17/17 `test_schedule_f_diagnostics_leg.py` + flow gate 241 + Topic 8 + Schedule F regression 80/80 + `manage.py check` clean + 10 rules seeded active in the shared DB. NEXT = leg 6 (assertions — merge 8 staged FA, gate 241→249, tag `1040-schedule-f-complete`). Prior: **Schedule F (1040 farm) — BUILD LEG 4 (INPUT) COMPLETE + GREEN**: un-stubbed the `schedule_f` 1040 tab → `ScheduleF1040Section` per-farm card UI (the ScheduleC precedent) — header A-G (accrual→RED banner, EIN RED-validation, E-G tri-state) + Part I income (1c/9 YELLOW) + Part II expenses (direct-entry lines 10-31; L14 depreciation YELLOW from the 4562 engine) + line-32a-f nested other-expense rows (32/33/34/QBI YELLOW) + at-risk 36a/36b + SEHI/SE-retirement GREEN + the farm-optional-method election card (Schedule SE Part II, per farm-attached proprietor) + Σ-L34 → Sch 1 L6 total; dispatch branches `activeTab==="schedule_f"` on `isIndividual` (1040 model card vs the entity FormFieldValue `ScheduleFSection` — same tab id, separate plumbing, the `f1040sf`/`fschedf` split mirror); IndividualNav un-stub (no 1040 stub tabs left; STUB_COPY emptied) + `schedule_f_farms` collection dot + `D_SF_`/`D_SE_FARMOPT`→`schedule_f` routing wired ahead of leg 5. Pure frontend (CRUD/serializer exist from the seed leg); **tsc clean / vitest 201 (+5 Schedule F nav tests) / build clean.** NEXT = diagnostics leg (`rules_schedule_f.py` — 8 D_SF_* + 2 D_SE_FARMOPT + farm-aware `d_8995_001`). Prior: **Schedule F (1040 farm) — BUILD LEG 3 (RENDER) COMPLETE + GREEN**: `field_maps/f1040sf_2025.py` un-stubbed — the 89 generic AcroForm fields correlated to the cash-method lines by POSITION off the actual 2025 PDF (left-col f1_23-36 = lines 10-22, right-col f1_37-46 = 23-31, f1_47-58 = 32a-f, f1_59/60 = 33/34; comb B/D; radio-by-distinct-name "X"; no pre-printed parens) + `render_schedule_f_1040` (ONE COPY PER FARM, bridge-gated via `compute_schedule_f_lines`; line 14 ← 4562; line-32 a-f itemized rows; loss-only line-36 at-risk box; an ACCRUAL farm renders no face — RED-defer) + `f1040sf` registered in ACROFORM_FORM_IDS + a distinct manifest entry (same PDF as the 1120-S-entity `fschedf`) + packet append after Schedule C + page 2 (accrual Part III) stripped via `SKIP_PAGES["Schedule F"]`. 10/10 `test_schedule_f_render_leg.py` (map-vs-PDF existence/no-dup/full-coverage + face position sweeps incl. resale margin 1c / L14 depreciation / line-32 rows / loss `(25,000)` + at-risk box / header A-G; accrual-skip; 2-farm 2-copies; packet 1-page) + flow gate 241, no regression. NEXT = input leg (un-stub the `schedule_f` React tab). Prior: **Schedule F (1040 farm) — BUILD LEG 2 (COMPUTE) COMPLETE + GREEN**: `compute_schedule_f_family` wired into `compute_return` before the Schedule C family — per-farm cash chain → 5 model cols + accrual zero-gate; Schedule SE feed (Σ L34 → 1a, −Σ CRP → 1b, Σ L9 → farm-optional gross base); Schedule 1 line 6 = Σ farms' L34 (engage-gated + reflow, flipped direct→computed); farms folded into the Schedule C family's QBI pro-rata allocation as additional §199A businesses (½-SE + farm SEHI + farm SE-retirement → `ScheduleF.qbi_amount`; Sch 1 L16/L17 carry farm contributions); Form 8995 line 2 + `qbi_engaged(farms=)`; depreciation `flow_to=schedule_f` → ScheduleF.depreciation line 14 (migration 0102, §179 IN). 12/12 `test_schedule_f_orchestration.py` (FA-01..08 pipeline) + 14/14 pure + flow gate 241 + **Topic 8 regression 54/54 (no value moved)**. Also fixed the prior-session `schedule_se_spec.json` re-export drift — the 4 SE-FARMOPT scenarios it silently added (count trip-wire 5→9, SE scenario loop teaches the farm-optional inputs + null=RED, `compute_schedule_se_lines` emits line 14 = FARM_OPT_MAX). NEXT = render leg (un-stub `f1040sf_2025.py` + `render_schedule_f`). Prior: **Effort #5 — Schedule E page 2 — recipient K-1 full router — COMPLETE (all 6 legs green, tag `1040-k1-router-complete`)**. BUILD LEG 6 (ASSERTIONS) DONE + GREEN: merged the 7 staged FA-1040-K1-01..07 into the active gate (**234 → 241**) + **semantic-repointed FA-1040-SCHE-01 → line 41** via `scripts/merge_schedule_k1_assertions.py`; new `_run_k1_assertion` runner wired into all 3 `test_flow_assertions.py` entry points by an **id-prefix guard** (`FA-1040-K1-*`) — PURE re-derivation through a new pure core `schedule_e_p2_totals_from_rows()` (extracted from `schedule_e_p2_totals`; the DB wrapper delegates) for FA-01/02/03 (line 41 = 26+32+37; line 32 = (h+k)−(i+j) col g = 0; line 37 = (d+f)+(c+e) col c = 0; `_FakeK1` rows), source-pins on the cross-form feeders for FA-04/05/06 (intdiv 2b/3b/3a, Sch D 5/12, Sch SE 1065-only, 8995 2/6), no-silent-gap blocker check for FA-07; flow gate **241** + K-1 compute/render/diagnostics legs **53 passed — no regression** (the from_rows refactor is value-identical); `_FakeK1` bool-coercion gotcha fixed (bool ∈ int — don't Decimal-coerce material_participation). Prior leg: BUILD LEG 5 (DIAGNOSTICS) DONE + GREEN: new `rules_schedule_e_p2.py` wires the 10 D_K1_* (5 RED-defer errors + 3 warnings + 2 info) + 2 D_SCHE_* (REMIC line 39 / Form 4835 line 40) via `RULES_SCHEDULE_E_P2` in `runner.seed_builtin_rules`, each reusing the compute helpers — new shared `k1_sche_net()` in `compute_schedule_k1.py` (passive/PTP-loss bridge-gate refactored from the 3 inline `net=` sites) + `schedule_e_p2_totals` (flow info); offending-entity names appended to each finding; 18/18 `test_schedule_k1_diagnostics_leg.py` + flow gate 234 + K-1 compute leg (251 passed, no regression) + `manage.py check` clean + 12 rules seeded active in the shared DB; client routing `D_K1_`→`schedule_e_p2` already wired in leg 4. Prior leg: BUILD LEG 4 (INPUT) DONE + GREEN: un-stubbed the `schedule_e_p2` React tab + `ScheduleK1Section` (per-entity K-1 cards — source_type 1065/1120-S/1041 drives the IRS box labels + hides inapplicable fields; owner T/S/J; material-participation + PTP toggles; grouped Schedule-E / cross-form / v1-RED-defer money + flags; YELLOW computed-totals preview reading SCHEDULE_E line 32/37/41 from field_values; RED entry-level EIN validation) + `IndividualNav` un-stub (collection dot + `D_K1_`→tab routing) + `ScheduleK1Row` type. Pure frontend (CRUD/serializer exist from the seed leg); tsc clean / vitest 196 / build clean. NEXT = diagnostics leg (`rules_schedule_e_p2.py`). Prior leg: BUILD LEG 3 (RENDER): `f1040se_2025.py` page-2 field map (153 entries, 0 missing/dup vs the actual PDF) + `render_schedule_e` Parts II/III/V (per-entity line 28/33 rows + per-column totals + **line 41** + page-2 header; lines 31/36 abs-injected for the pre-printed parens) + conditional `SKIP_PAGES["Schedule E"]` un-skip via `schedule_e_p2_has_content` + Sch D 5/12 K-1 face (direct + K-1) + 8995 line-1 K-1 §199A QBI rows; new pure `schedule_e_p2_rows()` FACE decomposition reconciles to the compute totals (single source of truth); 18/18 `test_schedule_k1_render_leg.py` + flow gate 234 + K-1 compute/topic8/topic9 (99) green; `manage.py check` clean. Row caps line 28 ×4 / line 33 ×2 / 8995 line 1 ×5 (overflow → totals, flagged). NEXT = input leg (un-stub `schedule_e_p2` UI). Prior leg: RS SPEC + seed + compute (SCHEDULE_K1 seeded; SCHEDULE_E amended page 2; RS DB 63→64, FA 217→224; `schedule_k1_spec.json` + 7 FA staged; **summary is line 41** not 40 vs the real 2025 PDF; §199A 20Z/17V/14I → 8995, SE 1065 14A → Sch SE). Prior: **Effort #4 (Retirement & Misc reorg) COMPLETE — local, NOT pushed. Part A: Social Security split (SSA-1099 card → new `SocialSecuritySection` under the un-stubbed `social_security` tab; pure frontend). Part B: **Form W-2G (certain gambling winnings §61) — ALL 5 LEGS GREEN** (seed `8bfe4c9` / compute `2cf14e7` / diagnostics `a3ff8b6` / assertions `9eaf3d6` / input `16630c1`): box 1 winnings → Sch 1 line 8b (Σ box1 + the non-W-2G `other_gambling_winnings` fact), box 4 fed WH → 1040 line 25c (a ROSTER = Form 8959 line 24 + Σ W-2G box4, NOT 25b); FormW2G doc model (mig 0096/RLS 0097); `MiscIncomeSection` (W-2G doc cards + non-W-2G fact, 8h/8i/8z stay on Schedule 1); no render face (input doc, the 1099-G precedent). Flow gate 234, topic8 49 (no regression), client tsc/vitest 193/build clean. Prior: **Credit-claim Leg B.4 — Taxpayer Info smart SSN + calculated age + IP-PIN (built+reviewed, committed local `568fb33`..`fd0da8d`, NOT pushed): `Taxpayer.identity_protection_pin`/`spouse_identity_protection_pin` (mig 0094, additive) → rendered onto the 1040 signature block (HEADER_MAP `f2_41`/`f2_43`, verified vs the real PDF); read-only `age`/`spouse_age` serializer fields (YELLOW); `TaxpayerInfoSection` extracted from FormEditor.tsx; smart taxpayer/spouse SSN UI (hyphenate/inline-400/ITIN tag) reusing the Leg-0 backend derivation. NO compute/flow-assertion change (IP-PIN+age are metadata; render IS done). vitest 186/186, mig-check clean. Found a PRE-EXISTING Sch 1-A tips-deduction bug (L13=$0 not $5k) — tracked separately, not a B.4 regression. Prior: **Credit-claim Leg B.3 — per-dependent AOTC checkbox: new nullable `EducationStudent.dependent` FK (mig 0093, SET_NULL) auto-links an `owner=dependent` Form 8863 student; `D_CREDIT_AOTC` unified-engine gate (barred/no-expenses→error, MAGI-over-ceiling→info); stale 8863 ❌ row corrected. Prior: UI Batch #2 effort #3 — Interest/Dividend input UI redesigned to a row-per-payer PayerGrid + accordion; additive non-computational `owner` T/S/J field (mig 0092); Sch B fields relocated to the Interest page + `sch_b` tab removed; `D_INTDIV_011` W/H-no-EIN warning. NO change to any form's compute/render/assertion coverage — built+reviewed, committed on `main` locally, awaiting Ken's push. Prior: **W-2 UNIT 3 COMPLETE — Form 7206 SEHI §162(l): 2% S-corp shareholder Box-5 SEHI + the Schedule C ½-SE/SEP limit fix → Schedule 1 line 17; FORM_7206 seeded RS DB 60→61 / FA 200→206; new `compute_7206.py` + `rules_7206.py` (6 diags) + 6 FA; flow gate 217→223; NO migration. Prior: TOPIC 9 SPEC LEG COMPLETE — seeded+exported+staged (Ken approved walk; RS DB 44→46; 12 FA-1040-SCHD staged; sibling D_INTDIV_001/002 + D_8995_002 retired in deployed RS); NEXT = build leg 1 (CapitalTransaction model + seeds). See the Schedule D row — prior: TOPIC 8 BUILD LEG 2 (compute leg) DONE `776ec8b` — NEW `compute_schedule_c.py` (Sch C per-business chain → model columns; Sch SE per-proprietor AUTO-CREATED rows, year-keyed wage base 176,100/184,500, W-2-SS cap; 8995 simplified QBI year-keyed thresholds + pro-rata reductions → 1040 L13, above-threshold BLANKS + D_8995_001; 8959 threshold-reduced-by-wages → Sch 2 L11 + 25c, engage incl. withholding-reconciliation) + `aggregate_depreciation` flow_to=schedule_c (migration 0060, line 13 incl. §179) + feeder repoints Sch 1 L3/15/16/17, Sch 2 L4/11, 1040 L13/25c (engage-gated, override escape hatch) + 7 REDs `rules_schedule_c.py` (D_SC_001/002/003/007, D_SE_003, D_8995_001, D_8959_002); **48/48 `test_topic8_compute_leg.py`** (24 spec scenarios pure + 13 pipeline incl. depreciation→L13 + MFJ-both-SE + clean-W-2-untouched), **flow gate 108**, regression 76 passed; FLAGGED: 8995 L11 vs Sch 1-A 13b (Ken pending) + the 8959 engage widening; **render leg next** — prior: BUILD LEG 1 (seed leg) DONE — models 0058 / RLS 0059 / 4 seeds in build.sh / manifest / CRUD; 33/33 `test_topic8_seed_leg.py`, flow gate 108 — prior: SPEC LEG seeded + exported — spec-first probe found Schedule C/SE absent in RS (404) + Form 8995 a wrong K-1-oriented draft → re-author; Ken-confirmed 4 kickoff scope decisions (COGS Part III in, home office simplified inline, multiple Schedule C, **Form 8959 in**); ALL constants verified vs IRS/SSA (SE wage base 2025 $176,100 / 2026 $184,500; 8959 $250k/$125k/$200k; 8995 thresholds 2025 $394,600/$197,300 & 2026 $403,500/$201,775/$201,750; home office $5×300). **SPEC LEG DONE 2026-06-12:** the 4-form loader `load_1040_schedule_c.py` authored (RS `aac4a38` — SCHEDULE_C 24f/9r/56L/7d/7s + SCHEDULE_SE 17/12/24/4/5 + **8995 RE-AUTHORED** 13/8/21/5/7 [`_retire_stale_8995` drops the wrong stub on seed] + 8959 13/6/24/4/5; 14 flow assertions; 6 new sources + RP 2025-32 §4.26 excerpt; 4 `requires_human_review` walk items), `READY_TO_SEED=False` (guard refuses, **zero DB writes, RS DB still 41 forms**); `check_topic8_integrity.py` **GREEN — ALL CHECKS PASS** (RS `338a373`). **Ken APPROVED the walk in-session → FLIPPED + SEEDED (RS `26e3244`): RS DB 41→44 forms, FlowAssertions 91→105** (SCHEDULE_C/SCHEDULE_SE/8959 created, **8995 RE-AUTHORED** — `_retire_stale_8995` deleted 28 stale stub rows; all rules cited); **deployed exports verified** (HTTP 200 + counts + 8995 re-author pins) → **canonical `server/specs/{schedule_c,schedule_se,8995,8959}_spec.json` committed + 14 assertions staged** in `flow_assertions_1040_topic8_pending.json` (active 1040 gate 86, flow gate 108 — no tts-tax-app code touched). next = **build leg 1 (seed leg)**: the multi-business Schedule C FK model + seed commands + f1040sc/f1040sse/f8959 manifest; then compute→render→input→diagnostics→assertions per form. **Prior — TOPIC 7 (EIC) COMPLETE — tag `1040-eic-complete`** — LEG 5 (diagnostics) wired the 10 deferred rules into `rules_eic.py`/`RULES_EIC` [D_EIC_002/004/008/010/011/014 + D_SEI_001 + D_8867_001/002 + D_8862_001 → **20 EIC-family diagnostics**: 16 D_EIC + 1 D_SEI + 2 D_8867 + 1 D_8862], all reusing the `compute_eic` pure helpers; **D_8867_001 uses the BROAD §6695(g) covered-credit gate** (EIC engaged OR L27>0 / CTC L19·L28 / HOH / AOTC L13) so existing CTC/HOH returns surface the unanswered-DD error; **D_EIC_014 sourced from the seeded 8862 `part_v` row**; **`_sei_age_answers` relocated `renderer.py`→`compute_eic.py`** (bridge-gate for D_SEI_001, render byte-identical); 26/26 `test_topic7_diagnostics_leg.py`, regression 151 passed, dev DB re-seeded. LEG 6 (assertions) merged the 9 staged FA-1040-EIC-01..09 (**gate 77→86**) via `scripts/merge_eic_assertions.py` + new `_run_eic_assertion` (PURE re-derivation + source inspection) wired into all 3 entry points → flow gate **108 passed**. NO migration. Prior — **Topic 7 (EIC) BUILD LEG 4 (input leg) COMPLETE** — `TaxpayerSerializer` +19 EIC facts (**trip-wire 60→79**) + `DependentSerializer` +`eic_qualifying_child`/`is_full_time_student`; **`views.py` dependents handlers now `_recompute_1040`** (latent CTC line-19 bug closed alongside EIC line-27); **`compute_eic.py` backfills the 8867/8862 rows when engaged** (the one gated-file touch — pure row-creation, no math; flow gate stayed 99); client **`eic` tab** (`EICSection` — 16 Yes/No/Unanswered tri-state selects [GREEN once answered, neutral while unanswered; REDs live in the diagnostics leg] + 4 money fields + Dependent EIC-flag table) followed by a `StandardSection` for the 10 seeded 8867/8862 sections. **9/9** in `test_topic7_input_leg.py` (facts round-trip + engage + drive L27a 4,328 through the real routes; dependent PATCH recomputes + Schedule EIC attaches; 8867/8862 rows backfilled + editable via /fields/); **flow gate 99**, compute+render leg 52, `test_1040` 22/22 (alone), `tsc`/`build` clean. NO migration. Next: diagnostics leg. Prior — **Topic 7 (EIC) BUILD LEG 3 (render leg) COMPLETE** — three EIC faces + the 1040_EIC worksheet statement page, strictly from the specs (Topic 5 precedent): **`field_maps/f1040sei_2025`** (Schedule EIC — model-driven per-child columns from the Dependent rows; gate ≥1 FLAGGED QC; 4-digit-year cells; 4a/4b derived via `_sei_age_answers`; page-2 instructions stripped) + **`f8867_2025`** (data-map — 12 seeded three-state answers → `{line}_yes/_no` boxes + DERIVED EIC/CTC/HOH header boxes; gate = engaged OR any answer) + **`f8862_2025`** (data-map — line-1 year + which-credit boxes; gate = disallowed AND not math-error) + **`render_eic_worksheet_statement`** (bespoke ReportLab statement page, `eic_engaged` gate, reached rows only); three form_ids in ACROFORM_FORM_IDS; packet Schedule EIC(43)→EIC worksheet→8862(43A)→[8812]→8867(70). **KEY:** pymupdf `widget.field_name` is DISTINCT per Yes/No kid → each box its own map key (the filler draws "X" when truthy; on-states irrelevant). **28/28** in `test_topic7_render_leg.py`; **flow gate 99**; render regression `test_topic5_render_leg` 18 + schb/sch123/8812/sch1a/f1040-header 214. NO migration / NO compute change. Next: input leg. Prior — **Topic 7 (EIC) BUILD LEG 2 (compute leg) COMPLETE** — **`apps/returns/compute_eic.py`** built from `eic_spec.json`: Step-5/WS-B earned income + `eic_table_lookup` ($50-bracket midpoint ROUND_HALF_UP, year-keyed `EIC_PARAMS` {2025,2026} verbatim — published TY2026 0-QC **664** not 663.42, 3+QC **8231**) + investment-income §32(i) gate + lower-of-AGI → **1040 line 27a** (override-respecting); wired into `compute_return` AFTER the first formula pass + `compute_sch_8812` (AGI/1z final), **"27" added to the 19/28 DB-refresh** so 27a → 32 → 33/34/37. **ENGAGE gate** `eic_engaged` (flagged QC / answered EIC fact / SE-investment amount) — non-engaged returns keep the legacy `eitc_claimed` and fire zero D_EIC (no false REDs / zero regression). **`rules_eic.py`** 10 credit-zeroing+no-silent-gap REDs (D_EIC_001/003/005/006/007/009/012/013/015/016) reuse the compute helpers (bridge-gate); **deferred** the 6 info/warn + D_8867/D_8862/D_SEI to the diagnostics leg. NO migration (0057), NO seed flip (27 already computed). **23/23** in `test_topic7_compute_leg.py` (8 PURE math + 3 edge pins + 8 PIPELINE gates + 3 wiring/engage/registration); **flow gate 99**; regression `test_1040` 22/22 + topic5/8812 green; dev DB re-seeded (10 D_EIC active). **8812 COUPLING REPOINTED (Ken-approved):** `compute_eic` now runs before `compute_sch_8812`; 8812 Part II-B L_24 reads the computed line 27a (face = Sch 3 L11 + 1040 L27), fallback to `eitc_claimed` only when line 27 is blank — regression-safe, `test_8812_part_iib_line24_reads_engine_27a` + TS14b green. Next: render leg (Schedule EIC/8867/8862 faces + 1040_EIC statement page). Prior — **Topic 7 (EIC) BUILD LEG 1 (seed leg) COMPLETE** — **migration 0057** [additive, applied to dev DB: `Dependent` +`eic_qualifying_child`(nullable override)/`is_full_time_student`; `Taxpayer` +20 EIC return-level facts — **`nontaxable_combat_pay` REUSED from the 8812 ACTC block, NOT duplicated** (same W-2 box 12 code Q amount; makemigrations flagged the would-be Alter); the 16 eligibility/election booleans are nullable `default=None` (three-state, fail-safe gates)]; **seeds in build.sh** `seed_1040_eic` [1040_EIC pseudo-form, 18 computed lines, renders as a STATEMENT page] + `seed_schedule_eic` [7 model-driven per-child lines] + `seed_8867` [12 BOOLEAN] + `seed_8862` [6]; **8867/8862 preparer answers are seeded FormFieldValue rows** (Schedule-B-Part-III precedent), NOT Taxpayer fields, so D_8867_001 can detect unanswered; **manifest** +`f1040sei`(2pg)/`f8867`(2pg)/`f8862`(3pg) downloaded + SHA + acroform-verified; **13/13 in `test_topic7_seed_leg.py`** (structure vs each spec line_map + computed sets/field types + idempotency + a spec-driven trip-wire pinning every stored return-level fact to a Taxpayer field); `makemigrations --check` clean; **flow gate 99 passed** (seed-leg precedent — no gated compute/render). `eic_childless_age_qualifies`/`eic_num_qualifying_children` are DERIVED at the compute leg (not stored). Next: compute leg `compute_eic.py` + EIC_PARAMS + the §32 gates. Prior — **Topic 7 (EIC + 8867/8862) SPEC LEG COMPLETE — seeded + exported + staged** [Ken approved review walk in-session]: RS `load_1040_eic.py` (commits `48e1fef` author / `5051f81` math gate / `00a550c` flip+seed) seeded 1040_EIC [computational pseudo-form: Step-5/WS-A/WS-B earned income + EIC-Table $50-midpoint ROUND_HALF_UP lookup + lower-of-AGI rule + Pub 596 Worksheet-1 investment income + Rules-for-Everyone/childless gates → 1040 L27a; **year-keyed `EIC_PARAMS` both years**] + SCHEDULE_EIC (model-driven per-child face) + 8867/8862 (data-map faces) + 9 flow assertions FA-1040-EIC-01..09; `check_eic_integrity.py` **ALL PASS**; **RS DB 37→41 forms, FA 82→91**; canonical `server/specs/{eic,schedule_eic,8867,8862}_spec.json` committed + 9 assertions STAGED in `flow_assertions_1040_eic_pending.json` (active gate stays 77, flow gate **99 passed**). Walk outcome: WS-B SE sourcing = Sch 1 L3 − L15. EIC REUSES `Dependent` (no new doc model). Next: build legs (seed→compute→render→input→diagnostics→assertions). Prior — **Topic 6 (CTC/ACTC, Form 8812) COMPLETE — tag `1040-8812-complete`** [DoD-CLOSEOUT ✅]: verification pass, NO compute/render/migration change — verified all 30 rules (R001-R030) match `sch_8812_spec.json` (RS lookup key `SCH_8812`, not `8812` which 404s); constants $2,200/$1,700 confirmed per-year independently (2025 OBBBA §70104 / 2026 RP 2025-32 §4.05, `_LATEST_VERIFIED_YEAR=2026`); R012 ceil-phaseout correct; **NEW `test_sch_8812_phaseout_boundaries.py`** (6 tests — full / ceil(+$1,+$1k,+$1,001) / partial / R014-STOP / deep-zero, BOTH 2025+2026, BOTH thresholds, through the real `compute_sch_8812`); render verified (3 scenarios); `test_sch_8812_scenarios.py` re-pointed from an outside-repo absolute path to the in-repo canonical spec (byte-identical 18 tests); **flow gate 99 passed** (13 CTC assertions intact: 12 FA-1040-CTC + TI-1040-CTC-A). **8812 diagnostics DEFERRED** (Ken "complete now, track rest" — spec has 12, only D_1040_008 wired; D009 Worksheet-B / D010 Add'l-Medicare / D011 RRTA are the no-silent-gap ones to prioritize). Next topic: Topic 7 (EIC + Form 8867). Prior — **Topic 5 (Retirement Income) COMPLETE — tag `1040-retirement-complete`** [BUILD LEG 6 — ASSERTIONS LEG ✅]: merged the 7 staged FA-1040-RET-01..07 into `flow_assertions_1040.json` (active 70 → 77) via `scripts/merge_retirement_assertions.py`; new `_run_retirement_assertion` in `test_flow_assertions.py` (PURE re-derivation through compute_retirement + source inspection, `_FakeRetDoc`, the `_run_intdiv` precedent) wired into all three entry points keyed on `form in ("1040_RETIREMENT","5329")`; kinds sum_check (4b/5b/25b) / formula_check (5329 L4→Sch2 L8; SS ws_18→6b) / constants_check (§86 base/tier, QSS≠MFJ) / gating_check (6 blockers → registered RED + 5b blank); source-pin gotcha — split-line 4b/5b `_set_field_value` matches `'tax_return, "4b"'` not the call-open form; **`pytest test_flow_assertions.py` → 99 passed** (92 + 7, zero regressions); NO migration/NO compute-render change. **Topic 5 ALL LEGS GREEN.** Next topic: Topic 6 (Form 8812 full DoD — verify + phaseout boundary tests + render). Prior — **Topic 5 BUILD LEG 5 — DIAGNOSTICS LEG ✅**: **D_5329_001** (Form 5329 line 2 exception amount > line 1 early-in-income → error) added to `rules_retirement.py`/`RULES_RETIREMENT` — reuses `RetirementAgg.early_distributions` vs `Taxpayer.exception_amount_5329` (bridge-gate); the retirement family is now 8 active (7 D_RET + 1 D_5329); audit closed (the 7 retirement_spec diags = D_RET_001..007, the 1 5329_spec diag = D_5329_001); 6/6 in `test_topic5_diagnostics_leg.py`, flow gate 92, dev DB re-seeded; NO migration/NO gated files. Resume = build leg 6 (assertions — merge 7 staged 70→77, add a `_run_retirement_assertion` runner kind, tag `1040-retirement-complete`, closes Topic 5). Prior — **Topic 5 (Retirement Income) BUILD LEG 4 — INPUT LEG ✅**: new **Retirement Income** tab (`retirement_income`) → `RetirementIncomeSection` (per-doc 1099-R box CRUD auto-save-on-blur + SSA-1099/5329 assertion card PATCHing the six 0056 facts); `retirement_distributions` added to `TaxReturnSerializer` nested read + the six facts to `TaxpayerSerializer` (trip-wire 54→60); box 2a uses a nullable money input (blank=NULL Simplified-Method gate), the non-null boxes send "0"; 10/10 in `test_topic5_input_leg.py` [CRUD→4a/4b/5a/5b/25b through the real route; blank box 2a blanks 5b; facts→6a/6b 17,000 + Sch 2 L8 2,000/1,200]; flow gate 92, tsc/build clean, serializer regression 16/16; NO migration/NO gated files. Resume = build leg 5 (diagnostics — D_5329_001) then leg 6 (assertions — merge 7 staged 70→77, tag `1040-retirement-complete`). Prior — **Topic 5 (Retirement Income) BUILD LEG 3 — RENDER LEG ✅**: Form 5329 Part I face (`f5329` manifest + `field_maps/f5329_2025.py` + `render_5329`, gated on the `form_5329_generated` compute helper, pages 2-3 stripped) + `render_ss_worksheet_statement` (ws_1..ws_18 reached-lines-only, gate ws_1>0); packet seq 29 (Sch B < 5329 < SS worksheet < 8812); 18/18 in `test_topic5_render_leg.py`, flow gate 92, render regression 192 passed; NO migration / NO compute change. Prior — **Topic 5 (Retirement Income) SPEC LEG COMPLETE — seeded + exported, NOT yet tagged (build legs pending)**: math gate `check_retirement_integrity.py` green [independent recompute of the 18-line SS Benefits Worksheet + 5329 Part I + 1099-R aggregation, RS `a92f253`] → Ken review walk approved [TY2026 §86/5329 constants confirmed non-indexed = NO `_constants_for_year`; R-RET-CODE J/T wording tightened to RED-out-of-v1] → flip + seed [RS `af54dcb`, DB 35→37 forms, FA 75→82] → deployed exports verified → canonical `retirement_spec.json` + `5329_spec.json` + 7 staged assertions committed [tts-tax-app `e45f7c3`]; active 1040 gate untouched at 70, flow gate 92 passed. Next: build leg 1 (RetirementDistribution model + SSA fields + f5329 manifest). Prior — **Topic 4 (Schedule 1-A) COMPLETE — tag `1040-sch1a-complete`**: DoD-closeout, not a fresh build (build landed 2026-06-10). Closed the one open DoD item — senior age disqualification proven COMPUTATIONAL via L_36a/L_36b (not a warning) with 3 new dedicated tests in test_sch_1a_scenarios.py [too-young single → L_35 full $6,000 but L_36a=0; born-before-Jan-2 boundary pair 1961-01-01 vs -02; MFJ spouse-only → L_36b=0/L_36a survives]; `_build_scenario` += optional dob_override/spouse_dob_override; 16/16 in file, flow gate 92; NO compute change. Also: **SUITE_CONTRACT.md** created (repo root) — live DB introspection confirms tax app + portal share clients_client/clients_entity, sherpa-1099 keeps its own filers/recipients/forms_1099 (own `tenants` tenancy, no FK into clients_*), check-in has no client table here; master/snapshot rule + primary-SSN gap documented. Prior — **Topic 3 COMPLETE — ASSERTIONS LEG ✅**: merged the 12 staged INTDIV/SCHB assertions into flow_assertions_1040.json [58→70 active, pending emptied]; new `_run_intdiv_assertion`/`_run_schb_assertion` runners in test_flow_assertions.py [PURE re-derivation through compute_intdiv helpers + source inspection — DB-backed coverage stays in test_intdiv_scenarios.py]; wired into all three runner entry points; flow gate **92 passed**; tagged `1040-intdiv-complete`. Prior — **DIAGNOSTICS LEG ✅**: rules_intdiv.py D_INTDIV_005..010 (qualified>ordinary error; doc-adjustments-exceed-income + liquidation warnings; foreign-tax/PAB/§199A info) + new apps/diagnostics/rules_schb.py D_SCHB_001..007 (Part III three-state chain reusing part_iii_required + seller-financed buyer-SSN error + 8815/foreign-trust/FBAR), registered via RULES_SCHB in runner.seed_builtin_rules; `_DEFERRED_DIAGNOSTICS` emptied so ID-G4/SB-DG1/SB-DG2/SB-DG3 assert through the real pipeline; NO migration (0053 carried every fact); 42/42 (test_topic3_diagnostics_leg.py 20 + test_intdiv_scenarios 22) + flow 80/80; dev DB seeded (10 D_INTDIV + 7 D_SCHB active). Next: assertions leg (merge 12 staged → flow_assertions_1040.json 58→70, new runner kinds, tag 1040-intdiv-complete). Prior — INPUT LEG (`5be74a8`): 1099-DIV tab + Schedule B tab + four 0053 facts, 7/7. COMPUTE LEG (`783e7ea`): compute_intdiv.py + migration 0053 + D_INTDIV_001..004 + bridge retired, 21/21. RENDER LEG (`b7acd95`+`7185547`): f1040sb map + render_sch_b + QDCGT statement + 7b box, 22/22)*

**Form-unit gate (1040 campaign):** a form is DONE only when all four legs are green —
**Input (data-entry UI) → Compute → Render → Flow assertions.**
**As of 2026-06-10, the DoD checklists in `SPRINT_SCOPE.md` (repo root) are the
completion bar per topic** — a form/topic is not marked complete here until every
DoD item is green, regardless of what already exists.

Legend: ✅ green · 🟡 partial · ❌ not built · stub = placeholder only

---

## 1040 Family (campaign focus)

| Form | RS Spec | Input (UI) | Compute | Render | Flow asserts | Notes |
|---|---|---|---|---|---|---|
| **Schedule H (household employment taxes)** | ⚠ RS spec is a DRAFT (7 of ~27 lines; the s238/s241c/s241w trap) — built from the 2025 face + Instructions + `IRS1040ScheduleH.xsd`; brief = `server/specs/_schedule_h_source_brief.md`; RS re-authoring agenda'd | ⚠ import lane only (`schedule_hs` + nested `state_rows`; browser screen a named defer) | ✅ s241w `compute_schedule_h` (year-keyed constants RAISE on unverified year; 3-state FUTA routing; per-row col (g) floor; C-only path zeroes line 25) | ✅ s241w `f1040sh_2025` AcroForm (⚠ C/9 No-first inversion pinned; Section B overflow → Statement) | ✅ s241w 8 D_SH rules + `IRS1040ScheduleH` MeF doc (SH-F1040 cascade + omit-25/26 seams; Worksheets 1/2 RED defers) | s241w — BATCH-004 #5; one per spouse; Sch 2 line 9 reconciled never written |
| **Form 1040 core** | ✅ SPINE seeded 2026-06-10 (45 rules, 72 lines, 16 diagnostics, 33 scenarios; exported as `1040_spine_spec.json`) | ✅ Leg 5 06-10: all 49 Taxpayer input facts serializer-exposed + tabbed UI (Taxpayer Info std-ded block + decedent/HOH, Sch 1-A, Credits & 8812, Payments) + **1040 form-line editor on Tax Summary tab** (Quality Rule 5) with RED/YELLOW/GREEN | ✅ Legs 1+2+3+4 06-10: Tax Table below $100K + TY hard stop + full std-ded chain (migration 0047) + full line plumbing (seed 19→55 lines, FORMULAS_1040 per R-INC/CR/PAY/REF, migration 0048 est payments) + **diagnostics framework** — D_1040_001..016 seeded via `seed_rules` (rules_1040.py), computational bridge gate (3a/7a blanks line 16), migration 0049 diagnostic-fact fields. **Leg 5 fixes: SCH_1A FormFieldValue backfill + form-scoped line lookups** | ✅ **Leg 6 06-10**: all 55 lines mapped (56 position assertions incl. synthesized 11b AGI carry), **HEADER_MAP repaired** (name/SSN/filing-status/occupations were on wrong widgets since the 2025 redesign), 12a-12d boxes + dependents grid + MFS/HOH-QSS names + preparer block render, render-path cross-form collision fixed (`test_f1040_2025_header_render.py` 12 tests) | ✅ **all 16 spine assertions wired (46 total)** | **TOPIC 1 COMPLETE 2026-06-10 (tag `1040-spine-complete`)** — all six legs + DoD residue cleared: digital-asset question (D_1040_017/0050/c1_10), D_1040_004 conservative interim kept, TY2026 constants Ken-verified (RP 2025-32; Tax Table interim accepted, re-pin ~Dec 2026). Per-line status: `line_status_1040.md`. E2E-1..3 + DG-1..6 green spec-driven. |
| **Schedule 8812 (CTC/ACTC/ODC)** | ✅ seeded in RS + exported | ✅ Leg 5 06-10: all 18 fields on the Credits & 8812 tab | ✅ 30 rules | ✅ | ✅ 13 | Compute/render/assertions complete (2026-05-28); input leg closed 2026-06-10 PM #9. Dependents UI ✅. Worksheet B deferred. **TY2026 amounts verified PM #11** (RP 2025-32 §4.05: $2,200/$1,700 — explicit 2026 entry + pins; `_LATEST_VERIFIED_YEAR` fallback). **TOPIC 6 COMPLETE 2026-06-11 (tag `1040-8812-complete`)** — DoD-closeout: 30 rules re-verified vs spec; NEW `test_sch_8812_phaseout_boundaries.py` (full/ceil/partial/STOP/zero × 2025+2026 × both thresholds through real compute); scenario test re-pointed to in-repo canonical spec; 13 CTC flow assertions green (gate 99). **8812 diagnostics leg DEFERRED** (Ken-approved tracked follow-up — spec has 12, 1 wired; prioritize D009/D010/D011). |
| **Schedule 1-A (OBBBA)** | ✅ seeded in RS (Ken approved 2026-06-10) + exported as canonical `sch_1a_spec.json` | ✅ Leg 5 06-10: Sch 1-A tab (tips attestation + amounts, overtime amounts, vehicle row entry w/ VIN validation) | ✅ all 6 parts | ✅ | ✅ 17 | **Part IV (car loan/QPVLI) built 2026-06-10**: CarLoanVehicle model (VIN-validated rows) + ceil-×$200 phaseout + VIN table render + 14 scenarios. All 21 Ken-reviewed RS scenarios run spec-driven (`test_sch_1a_rs_spec_scenarios.py`). **Leg 5 fixed: SCH_1A rows now backfilled on compute (UI-created returns previously never persisted/printed the schedule).** **TY2026 constants verified PM #11** (statutory non-indexed; RP 2025-32 adjusts none of the four parts; age cutoff rolls). **TOPIC 4 COMPLETE 2026-06-11 (tag `1040-sch1a-complete`)** — DoD-closeout: senior age disqualification proven COMPUTATIONAL via L_36a/L_36b with 3 dedicated tests (too-young single L_35=$6,000 but L_36a=0; born-before-Jan-2 boundary pair; MFJ spouse-only L_36b=0); 16/16 in test_sch_1a_scenarios.py; no compute change. **⚠ s140 RE-OPENED — this row's all-green legs hide two whole legs the table has no column for. (a) DIAGNOSTICS: ✅ **LANDED s142** (`2ed5eff`, Ken's 2026-07-30 backlog LEG 1 item 1) — NEW `apps/diagnostics/rules_sch_1a.py` implements all six specced checks (`D_SCH1A_001..006`) plus `D_SCH1A_NO_DOB`, which the spec has no entry for and which carries the money: a valid-SSN filer with NO `date_of_birth` while line 35 > 0, live-proven read-only on the demo QA return at $4,826 forfeited. ⚠ The spec's 001/002 conditions cannot fire as written (compute already zeroes Parts II/III/V for MFS and for a filer without a valid SSN), so every rule is written against the INPUTS; 004 is narrowed to the 12-month near-miss band because Part V is derived, not claimed; two spec facts have no model field (`tips_occupation_on_irs_list` conflated into the one attestation boolean, `tips_multiple_employers_or_occupations` absent and DERIVED from the tipped W-2s). All flagged in REVIEW_QUEUE for the next Rule Studio session — never silently diverged. 29 new tests. (b) E-FILE: ✅ **LANDED s174 2026-08-01** (backlog LEG 3, the "missing MeF documents" batch, form 1 of 4) — NEW `build_schedule1a` emits the `IRS1040Schedule1A` document at its ReturnData1040.xsd sequence position (885, between Schedule 8812 and Schedule A; identical in 2025v5.3/v5.4/2026v1.0), so `TotalAdditionalDeductionsAmt` on 1040 line 13b finally travels with the schedule behind it and these returns are no longer paper-only. Element names + line keys taken VERBATIM from the XSD's own `<LineNumber>` annotations; the app's seeded lines already matched one for one. ⚠ Lines 8/16/25/31 (the printed statutory maximums) have NO schema element and are dropped deliberately — pinned. ⚠ The line-22 vehicle rows come from `CarLoanVehicle`, not FormFieldValues, so they are built as `QlfyPassengerVehicleLoanIntGrp` groups and inserted at their schema position (anchored on every element that FOLLOWS them, so the group lands correctly even when line 23 is absent). ⚠⚠ **The attach gate is line 38, NOT the generic `_any(...)` its sibling schedules use** — `compute_sch_1a` writes Part I MAGI and the filing-status threshold on EVERY return with a taxpayer, so `_any` would have attached an empty Schedule 1-A to essentially every 1040 (revert-proven). ⚠ The printed form's `[:2]` truncation is deliberately NOT reproduced in the XML — the transmitted line 23 foots against the transmitted rows even while the paper copy does not, and the screen now says so. **Proven by LIVE XSD VALIDATION against the real gated 2025v5.3 schemas** (two cases: tips-only, and car-loan with two vehicle groups), revert-proven twice (a swapped element order fails validation; the `_any` gate attaches an empty schedule). 11 new tests in `test_efile_schedule1a_builder.py`. ⚠ **The screen's "not transmitted / file on paper" error was rewritten in the SAME commit** — after this it would send preparers to paper for a reason that no longer exists — as was the vehicle-overflow note's bare "file on paper" instruction (now scoped to paper filing only, since e-file carries every row); both vitest contracts rewritten carrying what they used to assert. **STILL OPEN on this row:** both Part IV attestations defaulting TRUE (LEG 3 item 8, needs a migration), the `[:2]` vehicle PRINT ceiling (LEG 4 overflow-statement item), and the unbuilt R-TIPS-10 SE limit. Also found in the s140 sweep: the age-disqualification tests referenced above cover a too-YOUNG filer but not a filer with NO date of birth, which the engine treats identically to under-65 — a silent 5,700–12,000 loss (now DIAGNOSED by `D_SCH1A_NO_DOB`, s142; compute is unchanged and does not need to change — the fix is the preparer entering the date). Plus the W-2-owner over-inclusion on line 4a (✅ **FIXED s169 2026-07-31**, backlog LEG 2 item 5 — line 4a now filters by `W2Income.owner` against each filer's eligibility, spouse requires MFJ, unknown owner → taxpayer; NEW `D_SCH1A_007` warns on spouse-attributed tips on a non-joint return — `seed_rules` on BOTH DBs at the next deploy; the unit-24 screen's overstatement banner became the exclusion note and its stale "no diagnostics yet" intro was retired), both Part IV attestations defaulting TRUE, the multiple-employer "-0- on 4a/4b" rule, the box-5 > $176,100 case, the `[:2]` vehicle print ceiling, and the unbuilt R-TIPS-10 SE limit. Full write-ups: STATUS.md s140 + REVIEW_QUEUE. This form must NOT be counted complete until (a) and (b) land. |
| Schedule 1 | ✅ seeded 2026-06-10 (`sch_1_spec.json`) | ✅ 06-11 Schedule 1 tab (StandardSection; 70 lines) | ✅ 06-11 compute_sch123 (9/10/25/26 + S1.18 YELLOW feeder ← 1099-INT box 2; L10→1040 L8, L26→L10 computed) | ✅ 06-11 f1040s1 map + render_sch_1 (8a/8d/8s abs-in-parens, 7-repaid box, 19b comb) | ✅ 4 wired 06-11 | **TOPIC 2 COMPLETE (tag `1040-sch123-complete`)**. Diagnostics D_SCH1_001..006 ✅. 14 spec scenarios green (S1-T2 loss-year pin -21,000). S1.18 feeder pinned by compute tests (post-spec Ken-approved computed line — no RS assertion of its own) |
| Schedule 2 | ✅ seeded 2026-06-10 (`sch_2_spec.json`) | ✅ 06-11 Schedule 2 tab (1e/1f source selects; 54 lines) | ✅ 06-11 compute_sch123 (1z/3/7/18/21 **L20 EXCLUDED** — S2-T2 pins 1,884 + S2.13 YELLOW feeder ← W-2 box 12 A/B/M/N; L3→1040 L17, L21→L23) | ✅ 06-11 f1040s2 map (root `form1[0]`) + render_sch_2 (source boxes i-iv, exemption boxes) | ✅ 5 wired 06-11 (FA-SCH2-05 = the L20-exclusion pin) | **COMPLETE**. Diagnostics D_SCH2_001..005 ✅. **8812 L22 re-pointed FACE-VERBATIM ← S2.5/S2.6/S2.13 (NOT S2.4)** |
| Schedule 3 | ✅ seeded 2026-06-10 (`sch_3_spec.json`) | ✅ 06-11 Schedule 3 tab (35 lines) | ✅ 06-11 compute_sch123 (7/8/14/15; 6l signed; L8→1040 L20, L15→L31; CLW-A pre-CTC read for 8812 L13) | ✅ 06-11 f1040s3 map (6z_type = f2-named page-1 widget) + render_sch_3 | ✅ 4 wired 06-11 | **COMPLETE**. Diagnostics D_SCH3_001..003 ✅. ONE page; 6e reserved |
| Schedule B | ✅ seeded 2026-06-11 (`sch_b_spec.json` — Ken approved) | ✅ input leg 06-11: **Schedule B tab** (line 3 8815 + Part III 7a/7b/8 three-state via the default StandardSection branch; lines 1/5/2/4/6 not enterable — payer listings model-driven). DividendIncome model + DIV CRUD from the seed leg (`8e34222`) | ✅ 06-11 compute leg: lines 2/4/6 computed in compute_intdiv_aggregation (L2 = per-doc nets; L4 = L2 − L3(8815) → 1040 2b structural tie; L6 → 3b); R-SB-04/05 gates as pure fns (`schedule_b_required`/`part_iii_required`) for the render/diagnostics legs. **Diagnostics leg 06-11: D_SCHB_001..007 ✅ (rules_schb.py — Part III three-state chain reusing part_iii_required + seller-financed buyer SSN + 8815/foreign-trust/FBAR)** | ✅ 06-11 (`b7acd95`): f1040sb map (model-driven payer slots + Part III three-state boxes) + render_sch_b (seller-first; Nominee/Accrued/ABP rows reconcile to L2; overflow→continuation) + `schedule_b_required` gate + packet seq 08 | ✅ **2 wired 06-11** (FA-1040-SCHB-01 L4=L2−L3 & ==1040 2b structural tie; FA-1040-SCHB-02 required-use/Part III truth table + L6==3b) | **Topic 3 COMPLETE** (tag `1040-intdiv-complete`). All 6 SB spec scenarios green through compute_return (SB-T1 2,875/2,375 tie pinned; SB-DG1/2/3 fire D_SCHB_001/002/006). D_SCHB_001..007 ✅ diagnostics leg |
| QDCGT worksheet + 1099-INT/DIV aggregation (1040_INTDIV) | ✅ seeded 2026-06-11 (`intdiv_spec.json` — Ken approved incl. 6 judgment items) | ✅ input leg 06-11: **1099-DIV tab** (DividendIncomeSection: full box surface 1a-16 + nominee, auto-save-on-blur) + the four 0053 facts on the dividends surface (capital_gain_distributions_only assertion + 3c/7b Form-8814 child-income facts, per-tab Taxpayer PATCH); serializer trip-wire 50 → 54; `dividend_incomes` in detail payload; 7/7 API-path tests | ✅ 06-11 compute leg: `compute_intdiv.py` — R-AGG-2A/2B/3A/3B/7A/25B rosters (supersede spine; 1040 3a/3b now computed feeders, 7 written only on the Exception-1 path) + `compute_qdcgt_worksheet` (WS1-25 pure; WS18/21 HALF-UP; WS22/24 = spine tax method) + `route_line_16` gate INSIDE _compute_1040_tax (**bridge retired**: D_1040_001/R-TAX-07/DG-1/FA-1040-SPINE-15 deleted in RS, new canonical spine spec; D_INTDIV_001..004 live as the blocked-path REDs) + ws-row persist/clear. **Diagnostics leg 06-11: D_INTDIV_005..010 ✅ (rules_intdiv.py — qualified>ordinary error, doc-adjustments + liquidation warnings, foreign-tax/PAB/§199A info)** | ✅ 06-11: render_qdcgt_statement (WS1-25 clean statement page, no faked face; gated on worksheet route) + 1040 7b "Sch D not required" box (`7185547`) | ✅ **10 wired 06-11** (FA-1040-INTDIV-01..10: 2a/2b/3a/3b/25b rosters, Exception-1 truth table, QDCGT breakpoints both years, WS partition identity + WS25=min, WS22/24 source-pinned to compute_tax_line_16, blocker truth table; active gate 58→70) | **Topic 3 COMPLETE** (tag `1040-intdiv-complete`). All 15 spec scenarios green (8 WS pure incl. TY2026 MFJ + MFS boundary pair + ID-Q9 247.50→248 + TCW-cents; 7 pipeline incl. gates; ID-G4 fires D_INTDIV_005); migration 0053 applied. D_INTDIV_001..010 ✅ |
| Retirement: 1099-R agg + SS worksheet (1040_RETIREMENT) | ✅ seeded 2026-06-11 (`retirement_spec.json` — Ken approved at review walk; 25 facts/8 rules/24 lines/7 diags/18 scenarios) | ✅ input leg 06-11: **Retirement Income tab** (`retirement_income`) → `RetirementIncomeSection` — per-doc 1099-R box CRUD (auto-save-on-blur) + SSA-1099/5329 assertion card (six 0056 facts via PATCH /taxpayer/); `retirement_distributions` in detail payload; box 2a uses a nullable money input (blank=NULL Simplified-Method gate); 10/10 in `test_topic5_input_leg.py`. RetirementDistribution model + CRUD from the seed leg | ✅ compute leg 06-11: `compute_retirement.py` — R-RET-CODE routing (J/T always RED; G/H/Q→0; Y reduces 4b via qcd) + R-RET-4AB/5AB (box2a−rollover−qcd floored; IRA→4a/4b vs 5a/5b; **blocking docs blank the taxable column, no silent gap**) + R-RET-25B (full INT+DIV+1099-R box-4 roster) + R-RET-EARLY→5329 + `compute_ss_worksheet` (WS1-18, both STOPs + MFS-with-spouse 85%-from-$1 + 85% cap). Migration 0056 (6 return-level Taxpayer facts). **Diagnostics D_RET_001..007 ✅ (rules_retirement.py)**. 4a-6b flipped to computed feeders. | ✅ render leg 06-11: `render_ss_worksheet_statement` (bespoke ReportLab, ws_1..ws_18 reached-lines-only, gate = ws_1 > 0; never a faked face) — placed after Form 5329 in the packet; 18/18 in `test_topic5_render_leg.py` | ✅ **7 wired 06-11** (FA-1040-RET-01..07: 4b/5b/25b rosters, 5329 L4→Sch2 L8, SS ws_18→6b + 85% cap, §86 constants both years, the 6 unsupported-path blockers; active gate 70→77 via `merge_retirement_assertions.py`; `_run_retirement_assertion` PURE re-derivation) | **Topic 5 COMPLETE (tag `1040-retirement-complete`).** All legs green: spec+seed+compute+render+input+diagnostics+assertions. Compute 26/26; input 10/10; diagnostics 6/6; flow gate **99 passed**. NO `_constants_for_year` (SS §86 + 5329 statutory). |
| Form 5329 Part I (early-distribution tax) | ✅ seeded 2026-06-11 (`5329_spec.json` — 3 facts/3 rules/4 lines/1 diag/3 scenarios) | ✅ input leg 06-11: line-2 exception number + amount + SIMPLE-25% flag entered on the Retirement Income tab's SSA-1099/5329 assertion card (the six 0056 facts) — exception 08 $8k → Sch 2 L8 1,200 proven through the route | ✅ compute leg 06-11: `compute_5329_part_i` (L3=L1−L2, L4=10%/25%×L3) → **Sch 2 line 8** computed feeder (flipped is_computed; override = escape hatch for Parts II-VIII); line 1 fed cross-form by R-RET-EARLY; `form_5329_generated` gate (R-5329-03). **Diagnostics leg 06-11: D_5329_001 ✅** (line 2 exception amount > line 1 early-in-income → error; in `rules_retirement.py`/`RULES_RETIREMENT`, reuses `RetirementAgg.early_distributions`). | ✅ render leg 06-11: `f5329` in manifest (SHA `2ad77023…`) + `field_maps/f5329_2025.py` (Part I lines 1-4 + exception-number box + header) in ACROFORM_FORM_IDS; `render_5329` gates on `form_5329_generated` (one source of truth); pages 2-3 stripped via SKIP_PAGES; packet seq 29 (after Sch B, before 8812) | ✅ FA-1040-RET-04 wired 06-11 (L4 = rate×max(0,L1−L2) → Sch 2 L8) | **Topic 5 COMPLETE (tag `1040-retirement-complete`).** Part I ONLY (10%/25% early-dist tax → Sch 2 line 8); direct-to-Sch-2 shortcut for pure code-1 case (F5329-T1..3 green). Parts II-VIII out of scope. |
| EIC worksheets + Table lookup (1040_EIC) | ✅ seeded 2026-06-11 (`eic_spec.json` — Ken approved at review walk; 33 facts/10 rules/18 lines/16 diags/16 scenarios) | ✅ input leg 06-12: **EIC tab** (`eic`) — Taxpayer EIC-facts card (16 Yes/No/Unanswered tri-state + 4 money incl. shared `nontaxable_combat_pay`) + Dependent EIC-flag table; **serializer trip-wire 60→79** | ✅ compute leg 06-11: `compute_eic.py` — Step-5/WS-B earned income + `eic_table_lookup` ($50-midpoint ROUND_HALF_UP, year-keyed `EIC_PARAMS` both years incl. published TY2026 664/8231) + `investment_income_total` + lower-of-AGI → 1040 L27a (override-respecting); wired after the first formula pass + 8812, "27" added to the downstream refresh. **ENGAGE gate** (`eic_engaged`, no false REDs). **Diagnostics `rules_eic.py` 10 RED gates ✅** (D_EIC_001/003/005/006/007/009/012/013/015/016, reuse compute helpers). 23/23 in `test_topic7_compute_leg.py`, flow gate 99 | ✅ render leg 06-12: `render_eic_worksheet_statement` — bespoke ReportLab STATEMENT page (never a faked face; `eic_engaged` gate, reached rows only) | ✅ **9 wired 06-12** (FA-1040-EIC-01..09 via `_run_eic_assertion` — PURE re-derivation + source inspection; gate **77→86**, flow gate **108 passed**) | **TOPIC 7 COMPLETE 2026-06-12 (tag `1040-eic-complete`)** — diagnostics leg added D_EIC_002/004/008/010/011/014 (now **16 D_EIC**) + D_SEI_001 + D_8867_001/002 + D_8862_001 = **20 EIC-family rules**; D_8867_001 BROAD §6695(g) gate; D_EIC_014 from 8862 part_v; `_sei_age_answers` relocated to compute_eic (bridge-gate). 26/26 `test_topic7_diagnostics_leg.py`. Seed leg: `seed_1040_eic` 18 computed lines; line 27a computed feeder. `nontaxable_combat_pay` REUSED from 8812 ACTC. **SLATE SWEEP UNIT 23 (s139, 2026-07-29, `4c1e51c`, presentation only):** `SlateEicScreen` — singleton screenbar + a bare `.slate-asstable` over the existing Dependent rows + InputRow worksheets + the whole face from the server's OWN 1040_EIC rows as locked ƒx cells. TWELVE defects surfaced at the control that causes them; the two that change filed numbers: **the `eic_self_employed` tri-state is the gate that admits Sch 1 L3 into earned income and UNANSWERED counts as No** (engine-proven CREDIT 4,328 vs 0 for one child, 7,152 vs 0 for two; NO diagnostic covers it) and **the main-home exception label stated the opposite of Pub 596 Rule 14 while `eic_puerto_rico_territory_home` had no input anywhere in the app** (both corrected in the shared const — the legacy screen is fixed by the same lines; `EIC_TAXPAYER_FIELDS` gained the PR fact). Also: Form 2555's EIC kill switch lives on the Credits (8812) screen; Rule 2's valid-SSN fact is a serializer side effect that stays NULL on imported/seeded taxpayers (3 of 7 demo taxpayers with an SSN — `is not True` makes that a $0 credit) and is now answerable here; `step5_1`/`step5_2`/`wsb_6` are seeded but never written → REVIEW_QUEUE. |
| Schedule EIC (qualifying child info) | ✅ seeded 2026-06-11 (`schedule_eic_spec.json` — 7 facts/1 rule/7 lines/1 diag/2 scenarios) | ✅ input leg 06-12: Dependent EIC-flag table on the EIC tab (`eic_qualifying_child` tri-state + `is_full_time_student`); both flags added to `DependentSerializer` | n/a (data-map face, no compute) | ✅ render leg 06-12: `f1040sei_2025` + `render_schedule_eic` — model-driven per-child columns 1/2/3 (name/SSN/4-digit-year cells/4a/4b/rel/months from Dependent); gate ≥1 FLAGGED QC; 4a/4b derived (`_sei_age_answers`, under-19 skip); page-2 instructions stripped | n/a | **TOPIC 7 COMPLETE 2026-06-12**. Diagnostics leg: **D_SEI_001** (flagged QC fails the 4a/4b age test → error; reuses the relocated `_sei_age_answers`). Seed leg: `seed_schedule_eic` 7 model-driven lines. `f1040sei` in manifest + ACROFORM_FORM_IDS. |
| Form 8867 (preparer due diligence) | ✅ re-seeded 2026-07-26 **s114 PER-QUESTION face** (`8867_spec.json` — 25 facts/1 rule/**21 lines**/1 diag/6 scenarios; RS `a0708a5`, D_8867_002 pruned from the catalogue) | ✅ input leg 06-12: Parts I-V Y/N rows via the StandardSection branch on the EIC tab (rows backfilled by `compute_eic` once EIC engages) | n/a (data-map; header boxes derived at render) | ✅ render leg 06-12: `f8867_2025` + `render_8867` — 12 seeded three-state answers → `{line}_yes/_no` boxes; header EIC/CTC/HOH/AOTC DERIVED; gate = `eic_engaged` OR any answer (not every covered credit — that's the diagnostics leg) | n/a | **s114 PER-QUESTION REBUILD 2026-07-26 (`0f397da`+`de31033`; QA Batch-001 item 11, Ken GO)**: the compressed 12-line model → the full Rev. 11-2024 face, one row per printed checkbox (1/2/3/4/4a/4b/5/5_docs/6/7/7a/8/9a/9b/9c/10/11/12/13/14/15). N/A vocabulary ONLY on 2/7/7a/8/9c/11/12 (template widget dump == MeF IRS8867.xsd, line-for-line). Mig 0214 re-keyed stored answers BOTH DBs (8→7a, hoh→14, 9→9a; merged-true→components, merged-false/na→BLANK for re-answer — never invent; verified against the pre-migration audit). Cascade follows the face's own routing (4=No skips 4a/4b; 7a from disallowance flags; 8 yes/na from Sch-C presence; childless EIC skips 9b/9c; 15=certification). E-file now transmits the previously-ABSENT sub-question elements + WorkPaperDocumentNm; certification reads row 15 (no side flag). Print fills all 20 questions + docs list + preparer name/PTIN header. Client: N/A pill on exactly the 7 NA lines + Part VI section. Live demo probe: attest→all applicable filled / un-attest→revert. **TOPIC 7 COMPLETE 2026-06-12**. Diagnostics leg: **D_8867_001** (BROAD §6695(g) covered-credit return with an unanswered DD question → error; s114 = per-question applicability + na-counts-only-where-boxed) + **D_8867_002** (AOTC line 13 answered but 8863 not built → error). Seed leg: `seed_8867` 12 BOOLEAN lines / 5 Parts (FormFieldValue rows). `f8867` in manifest + ACROFORM_FORM_IDS. **LEG C 2026-06-19 (built+reviewed, local, NOT pushed):** new return-level attestation cascades the 8867 answers (late `apply_8867_due_diligence` pass) with a `"na"` value; **render now fills the N/A checkbox on lines 2/7/8 only** (PDF probe — `NA_ELIGIBLE_LINES`); `D_8867_001` accepts `na`; **`D_8867_002` retired (deactivate-in-place, `is_active:False`)**; render gate widened to any covered credit. Frontend: attestation checkbox + `<details>` collapse + simplified EIC exceptions list. All green; mig 0095 (attestation field). |
| Form 8862 (claim after disallowance) | ✅ seeded 2026-06-11 (`8862_spec.json` — 6 facts/1 rule/6 lines/1 diag/2 scenarios) | ✅ input leg 06-12: Parts I-V rows via the StandardSection branch on the EIC tab (backfilled when engaged) | n/a (data-map) | ✅ render leg 06-12: `f8862_2025` + `render_8862` — line-1 year + Part-I which-credit boxes (part_ii/iii/iv); gate = `eic_disallowed_prior_year` AND line "2" math-error ≠ true; all 3 pages kept for the preparer | n/a | **TOPIC 7 COMPLETE 2026-06-12**. Diagnostics leg: **D_8862_001** (math-error disallowance → Form 8862 NOT required; info — fires when `f8862_was_math_error` IS true). Seed leg: `seed_8862` 6 lines / 5 Parts (line 1 TEXT). `f8862` in manifest + ACROFORM_FORM_IDS. |
| Schedule D + 8949 (1040) | ✅ **TOPIC 9 COMPLETE 2026-06-13 (tag `1040-scheduled-complete`)** — all 6 legs. **DIAGNOSTICS** (`b5e690f`): family = 16 rules (7 error + 3 warning + 6 info); `schd_route` re-derived (not persisted); 12/12. **ASSERTIONS** (`2a635b7`): 12 FA-1040-SCHD merged (gate 100→112) via `merge_topic9_assertions.py` + `_run_schd_assertion` runner (PURE re-derivation + source pins, all 3 entry points, form ∈ {SCHEDULE_D, 1040_SCHD_WS, 8949}); **flow gate 134**. | ✅ INPUT LEG DONE 2026-06-13 (`e27383e`) — Schedule D tab (`ScheduleDSection`): CapitalTransaction CRUD (12-box dropdown + i8949 adj-code dropdown w/ definitions [single-select v1; multi-code follow-up] + Exception-2 toggle; computed (h) YELLOW) + the 7 schd_* facts card (QOF three-state, files_4952, carryovers, §1250/28%/§1202 supplements). TaxpayerSerializer +7 facts (trip-wire 86→93); capital_transactions nested read + CRUD already from seed leg. **7/7 `test_topic9_input_leg.py` + trip-wire 93 = 16; tsc/build clean.** NO migration/NO gated files. | ✅ COMPUTE LEG DONE 2026-06-13 (`063194a`) — `compute_schedule_d.py`: pure 8949 col-h + per-box netting (A/G→1b…F/L→10) + `schedule_d_route` (17/20/22→ordinary\|qdcgt\|sdtw) + the four worksheets (`compute_sdtw` 47L w/ skip rules, `compute_carryover_out_worksheet` clc, `compute_28pct_worksheet` w28, `compute_unrecap_1250_worksheet` u1250 — ported from `check_topic9_integrity.py`; SDTW 0/15% breakpoints REUSE `QDCGT_CONSTANTS_BY_YEAR`, `SDTW_BRACKET32_START` new year-keyed; invariant SDTW(18=19=0)==QDCGT) + DB assembly `compute_schedule_d_db` (before formula pass → SCHEDULE_D + 1040_SCHD_WS rows + 1040 L7a) + `schedule_d_line_16` (tax phase, bypasses route_line_16 when engaged). Engage = universal fallback (txns/carryover/4-5-11-12/DIV 2b-2d/unasserted 2a); 4952/2c/2555/8814 BLANK L16. SIBLING: D_INTDIV_001/002 + D_8995_002 RETIRED; 8995 L12 += net cap gain; canonical intdiv/8995 re-exported. `rules_schedule_d.py` 6 REDs. **33/33 `test_topic9_compute_leg.py`, flow gate 122, regression 158.** | ✅ RENDER LEG DONE 2026-06-13 (`49735cd`) — `f1040sd_2025.py` (model-driven face from SCHEDULE_D rows; QOF/17/20/22 Yes-No checkbox pairs; line 21 ABS for pre-printed parens) + `f8949_2025.py` EXTENDED 12-box G/H/I/J/K/L; `render_schedule_d_1040` (gate schedule_d_engaged) + `render_8949_1040` ONE COPY PER BOX (ST→PartI/LT→PartII) + `render_schedule_d_worksheets_statement` (SDTW/28%/§1250/CLC reached sections, statement page); ACROFORM_FORM_IDS +f1040sd; packet seq 12/12A after Sch C. **10/10 `test_topic9_render_leg.py`, flow gate 122, render regression 21.** | ✅ **12 wired (`2a635b7`)** — FA-1040-SCHD-01..12 via `_run_schd_assertion` (gate 100→112, flow gate **134**) | **SEED LEG (29/29):** migration 0061 (`CapitalTransaction` 12-box A–L doc model + 7 schd_* facts) + RLS 0062; seeds `seed_schedule_d`(47L)/`seed_1040_schd_ws`(85L)/`seed_8949`(26L); manifest +f1040sd; CRUD. SPEC (RS DB 44→46, `272aeee`). 192-field 8949 map reusable; Sch D → 1040 line 7a; SDTW line-19 2026 DERIVED (re-pin ~Dec 2026). **RS follow-up: clean the 2 stale pre-Topic-9 ID-G1/G2 scenarios from `load_1040_intdiv` (local canonical already dropped them).** **s107 2026-07-24 — ENTRY-LAYER FIX (all 6 legs stay green; no compute/render/spec change):** `+ Add transaction` POSTed `description: ""` on click → DRF 400 "may not be blank" → NO row ever appeared, so the input leg was UNUSABLE from an empty Schedule D despite being marked done at Topic 9. Now a client-side draft row (`SCHD_DRAFT_ID`) renders on click with zero requests, persists on the first valid description carrying every column typed before it, and serialises concurrent blurs onto ONE create (`creatingRef`) so rapid entry can't split a lot across two 8949 rows; inline validation keeps the row. NEW `scheduleDDraftRow.test.tsx` 13 + `test_schedule_d_draft_row.py` 10 (incl. gain → Sch D 16 → 1040 line 7 → GA-500 line 10). ⚠ The multi-code adj-code follow-up noted at Topic 9 has since shipped (chips); the remaining Schedule D work is the rest of the P1 entry-vs-compute audit. |
| Schedule A | ❌ | ❌ | ❌ | ❌ | ❌ | |
| **Form 4952 (investment interest §163(d))** | ✅ RS spec `4952` v1 fetched + cached 2026-08-06 (`server/specs/4952_spec.json`; 9 facts, 5 rules, 5 line_map entries, 7 diagnostics, 5 test scenarios) | ✅ s218 — 8 `f4952_*` Taxpayer facts (migration 0254, every one `db_default=0`): the Slate Schedule A screen (own section + a live line-3/6/7/8 echo) + the legacy FormEditor group + `backentry.v1` + TaxpayerSerializer | ✅ s218 `compute_4952.py` — the whole face (1, 2, 3, 4a-4h, 5, 6, 7, 8) + the 4g attribution split; **Schedule A line 9 = line 8 whenever the form is keyed** (the derived figure OUTRANKS the flat entry — D_4952_SCHA9 reports the disagreement); **SDTW lines 3/4 now read 4g/4e** (they were hard zeros); line 7 → the proforma snapshot `_f4952_disallowed_cf_prior` → next year's line 2 | ✅ s218 `render_4952` on the official f4952.pdf (manifest + SHA256 recorded; `f4952_2025.py` field map, all 17 widgets, positions asserted against the template's own geometry); packet attachment seq 51. **MeF `build_irs4952`** — element names + order verified against the 2025v5.4 IRS4952.xsd's own `<LineNumber>` annotations, slotted at ReturnData1040 1270 (between IRS4835 and IRS4972) | — (no FA yet; the RS spec's 5 scenarios run as unit tests) | **BATCH-047 #15. 30/30 `test_form_4952_b047.py`** (the 5 RS scenarios verbatim + the packet's 7 deduction / 24 carryforward + every wiring leg + face geometry + MeF). **D_SCHD_001 is REWORKED, not retired**: it no longer says "not built" — it fires only when the form is DECLARED and unkeyed, which is still the condition that blanks 1040 line 16 (a worksheet run at 4g = 4e = 0 would look defensible and be wrong). New `rules_4952.py`: D_4952_4G_CAP (error) · 4G_RATE · SCHA9 · DECLARE · CARRYFWD · NOTREQ. v1 boundaries: ONE form per return (§163(d) is computed on the joint return), and the spec's 1041 routing of line 8 is out of scope. |
| **Form 7206 (SEHI §162(l))** | ✅ FORM_7206 seeded 2026-06-15 (RS DB 60→61, FA 200→206; canonical `7206_spec.json`) | n/a (inputs live on W2Income.two_percent_shareholder_health / ScheduleC.sehi_amount) | ✅ 2026-06-15 `compute_7206.py` — Sch C limit FIX (net profit − apportioned ½-SE-tax − SEP) + the **2% S-corp Box-5 path** → Sch 1 L17 (engages WITHOUT a Schedule C); 17/17 | n/a (line 17 already rendered; no Form 7206 face in v1) | ✅ 6 (FA-1040-7206-01..06 via `_run_7206_assertion`; gate 217→223) | **W-2 UNIT 3 COMPLETE 2026-06-15.** Diagnostics `rules_7206.py` 6 (D_7206_SCORP_NOWAGE/SCORP_LIM/SC_LIM/LTC_AGECAP/2555/PTC). R-SC-SEHI repointed to FORM_7206 in lock-step. NO migration. LTC age-cap + Form 2555 line-12 v1-deferred (no silent gap). | s113 2026-07-26: PARTNER ARM - K-1 box 14A activities (ScheduleK1 13M/13R fields, mig 0213); line 5 pools POSITIVE Sch C + K-1 profits only (2025 face verbatim); form_7206_rows MeF parity; RS SC-T9 + QA acceptance pinned (RS 6761b65 / app 7050f93). **s218 2026-08-06 (BATCH-047 #14):** the item's stated cause is REFUTED — `sehi_amount` is Form 7206 **line 1**, the source premium, and `compute_7206` has always applied the earned-income limit to it (the packet's own 65,765 − 4,646 = 61,119 limit re-derives from the premium, not from an answer). The REAL gap was **line 2**: `compute_7206`'s `ltc` argument had no feeder, so the printed and transmitted line 2 was a structural zero. NEW `sehi_ltc_amount` on ScheduleC / ScheduleF / ScheduleK1 (migration 0255, `db_default=0`), fed through all four call sites (the per-proprietor SEHI block ×2, `form_7206_rows` for the MeF IRS7206 document, and the farm arm), plus lane + serializer + Slate + legacy UI. 9/9 `test_form_7206_ltc_b047.py`. STILL deferred (flagged, not silent): the §213(d)(10) age cap itself (D_7206_LTC_AGECAP — the preparer enters the already-capped figure), the §162(l)(2)(B) subsidised-employer-plan month exclusion, and Form 2555 line 12 (D_7206_2555).
| **Minister / Clergy (§107/§1402)** | ✅ MINISTER seeded 2026-06-16 (RS DB 61→62, FA 206→212; canonical `minister_spec.json`; 4 sources all `requires_human_review`) | ✅ 2026-06-16 — W2Screen clergy section (5 housing fields + Form 4361 checkbox via `onUpdateTaxpayer`); **mig 0088** clergy_* fields on W2Income + `clergy_4361_exempt` Taxpayer fact (trip-wire 93→94); tsc 66 baseline / vitest 34 / build ok | ✅ 2026-06-16 `compute_minister.py` — §107 least-of-three excl; EXCESS → 1040 1h (aggregate_1040_income, override-respecting); clergy SE base (wages+full housing+parsonage−expenses) → Schedule SE line 2 per owner via the existing engine (×0.9235/SS cap/½-SE→Sch1 L15/SE→Sch2 L4); Form 4361 zeroes; **supersedes Topic-8 minister RED-defer**; 18/18 | ✅ `render_minister_statement` (Pub 517 worksheet statement; W-2 = input doc, no IRS face; 4/4) | ✅ 6 (FA-1040-MIN-01..06 via `_run_minister_assertion`; active 201→207, flow gate 223→229) | **W-2 UNIT 4 COMPLETE 2026-06-16 (tag `1040-minister-complete`).** Diagnostics `rules_minister.py` 6 (D_MIN_HOUSING_INC error + EXCESS/4361/SECA/REASONABLE/DEASON info/warn). v1 RED-defers: Sch-C ministerial side income + §265/Deason, reasonable-comp test, retired clergy, 4361 adjudication, mixed two-minister-4361. Brief `server/specs/_minister_source_brief.md`. |
| **Schedule C + SE + 8995 + 8959** | ✅ specs seeded 2026-06-12 | ✅ input leg 2026-06-12 — `schedule_c` tab (per-business cards Parts I-V + nested Part V row CRUD + per-proprietor SE cards + 8995/8959 facts card); TaxpayerSerializer +7 facts (trip-wire 86); nested schedule_c_businesses/schedule_se_forms reads; DepreciationAsset +schedule_c; depreciation POST now full compute_return; DISENGAGE fix (feeders gate on businesses OR se_rows — last-business-deleted reflows L3 to 0); 9/9 input tests + compute 49/49 re-verified + flow gate 108 + tsc/build clean | ✅ compute leg 2026-06-12 (`776ec8b` + 13b amendment `ae9fae7`) — `compute_schedule_c.py`: Sch C per-business chain → model columns; Sch SE auto-created per proprietor (wage base 176,100/184,500, W-2-SS cap, $400 floor); 8995 → 1040 L13 (L11 = AGI−12−13b Ken-ruled; above-threshold blanks + D_8995_001); 8959 → Sch 2 L11 + 25c; feeders Sch 1 L3/15/16/17 + Sch 2 L4/11 computed (engage-gated); depreciation engine → line 13 (0060); 7 REDs `rules_schedule_c.py`; 49/49 + flow gate 108 **s143 2026-07-30 (`4c76624`) COMPUTE AMENDMENT — the stale QBI deduction on 1040 line 13 ($859 understated, backlog LEG 2 item 1):** the not-engaged branch left line 13 alone, so deleting the last section 199A source left the deduction on the return for ever and recomputing never cleared it; now `blank_form_rows() + write_line_13("")`, which pops `values["13"]` so the 14/15 reflow recovers the taxable income. Follows RS `R-8995-L15` (line 13a IS the Form 8995 line-15 figure). The `is_overridden` guard still protects a real direct entry, sound because line 13 is a computed line. **Also an E-FILE defect** — line 13 maps to `QualifiedBusinessIncomeDedAmt` and the builder omits blanks but emits stored values, so the stale figure was TRANSMITTED with the 8995 rows blank. Gates: new `test_backlog_leg2_compute.py` 8 · flow 521 · Topic 8 band 166.| ✅ render leg 2026-06-12 — 4 field maps (f1040sc incl. the IRS 27a/27b widget swap + B/D combs; f1040sse line-7 preprinted unmapped, page 2 stripped; f8995 replaces the old stub, 1i-1v table; f8959 1-24) + `render_schedule_c` (one COPY PER BUSINESS, pure-chain bridge-gate) + `render_schedule_se` (per proprietor; gate L12>0; RED-defer never prints) + `render_8995`/`render_8959` (row-driven engage gates); parens lines abs-injected; packet seq 09/17/55/71; 21/21 `test_topic8_render_leg.py`, flow gate 108, render regression 260 | ✅ **14 wired 06-12** (FA-1040-SCHC-01..04 / SCHSE-01..03 / 8995-01..03 / 8959-01..03 / TOPIC8-01 via `_run_topic8_assertion` — PURE re-derivation + source pins; **active gate 86→100, flow gate 122 passed**) | ✅ spec seeded 2026-06-12 (RS `load_1040_schedule_c`; canonical specs committed; RS DB 44, FA 105). **TOPIC 8 COMPLETE 2026-06-12 (tag `1040-schedulec-complete`)** — diagnostics leg added the 13 info/warn rules (family = **20**: 7 error + 1 warning + 12 info); 8995 L11 = 1040 L11−L12−**L13b** (Ken ruled 06-12, RS amended in lock-step); 8959 engage incl. the withholding-reconciliation case (DECISIONS, reversible). **BUILD LEG 1 (seed leg) DONE 2026-06-12:** migration 0058 (`ScheduleC` multi-business doc model + `ScheduleCOtherExpense` Part V child + thin per-proprietor `ScheduleSE` [Ken's call — W2Income has no owner field so the SS-wage cap needs a per-proprietor home] + 8995/8959 Taxpayer facts) + RLS 0059 (3 tables, rowsecurity=True, 0 policies); seeds `seed_schedule_c`(56L)/`seed_schedule_se`(24L)/`seed_8995`(21L re-author)/`seed_8959`(24L) in build.sh, line_maps + computed sets match specs exactly (trip-wire caught Sch C L13 depreciation = computed/from-4562); manifest f1040sc/f1040sse/f8959 downloaded + f8995 registered (AcroForm-verified); CRUD schedule-c (+ nested other-expenses) + schedule-se (one-per-proprietor guard → 400). **33/33 `test_topic8_seed_leg.py`; flow gate 108 (no gated files touched); makemigrations --check clean.** **8995 RE-AUTHORED** over a wrong K-1 stub; **8959** Decision 4. Build leg 2 (compute) next |
| **Schedule F (farm) + SE farm-optional** | ✅ **SPEC LEG 2026-06-21** — SCHEDULE_F seeded (31f/11r/71L/8d/10s) + SCHEDULE_SE amended additively (Part II farm optional method + line-1a/1b computed feeders + narrowed R-SE-OPTIONAL/D_SE_003); RS DB 64→65, FA 224→232; canonical `schedule_f_spec.json` + re-export `schedule_se_spec.json` + 8 FA staged; math gate ALL PASS (id-length guards added); guard verified (zero DB writes); seed gotcha — `diagnostic_id` varchar(20), `D_SE_FARMOPT_INELIGIBLE`→`D_SE_FARMOPT_INELIG`. Brief `_schedule_f_source_brief.md`. Ken-approved walk. | ✅ **INPUT LEG DONE + GREEN 2026-06-21** — un-stub `schedule_f` 1040 tab → `ScheduleF1040Section` per-farm card UI (the ScheduleC precedent): header A-G (accrual→RED banner, EIN RED-validation, E-G tri-state) + Part I income (1c/9 YELLOW) + Part II expenses (lines 10-31; L14 depreciation YELLOW from 4562) + line-32a-f nested other-expense rows (32/33/34/QBI YELLOW) + at-risk 36a/36b + SEHI/SE-retirement GREEN + farm-optional-method election card (Schedule SE Part II, per farm-attached proprietor) + Σ-L34→Sch1-L6 total; dispatch branches `activeTab==="schedule_f"` on `isIndividual` (1040 model card vs entity FormFieldValue `ScheduleFSection`); IndividualNav un-stub (no 1040 stub tabs left) + `schedule_f_farms` collection dot + `D_SF_`/`D_SE_FARMOPT`→`schedule_f` routing. Pure frontend (CRUD/serializer from seed leg); **tsc clean / vitest 201 (+5) / build clean.** | ✅ **COMPUTE LEG DONE + GREEN 2026-06-21** — `compute_schedule_f_family` (per-farm chain → 5 cols; **accrual zero-gate**, no silent compute; SE feed 1a/1b/gross; **Sch 1 L6 = ΣL34** engage-gated + reflow) wired BEFORE `compute_schedule_c_family`; farms folded into the QBI pro-rata allocation (½-SE + farm SEHI + farm SE-retirement → `ScheduleF.qbi_amount`; Sch 1 L16/L17 carry farm); 8995 L2 + `qbi_engaged(farms=)`; depreciation `flow_to=schedule_f` → L14 (**mig 0102**, §179 IN); Sch 1 L6 flipped direct→computed (`seed_sch_1` is_computed). **12/12 `test_schedule_f_orchestration.py` + 14/14 pure + Topic 8 regression 54/54 (no value moved) + flow gate 241.** Also fixed the `schedule_se_spec.json` re-export drift (SE-FARMOPT-1..4: count 5→9, loop, line-14 emit). | ✅ **RENDER LEG DONE + GREEN 2026-06-21** — `f1040sf_2025.py` un-stubbed (89 generic AcroForm names → cash lines, **every widget correlated to its printed label by position** off the 2025 PDF; comb B/D; radio-by-distinct-name "X"; no pre-printed parens) + `render_schedule_f_1040` (ONE COPY PER FARM, bridge-gated via `compute_schedule_f_lines`; L14←4562; line-32 a-f rows; loss-only at-risk box; **accrual renders no face**) + `f1040sf` in ACROFORM_FORM_IDS + manifest entry + packet append after Sch C + page-2 strip via `SKIP_PAGES["Schedule F"]`. **10/10 `test_schedule_f_render_leg.py`** (map-vs-PDF + face position sweeps + accrual-skip + 2-farm copies + packet 1-page) + flow gate 241, no regression. | ✅ **8 wired 2026-06-21** (FA-1040-SCHF-01..08 via `_run_schf_assertion` — PURE re-derivation for the cash math + source-pins for the depreciation/QBI/CRP feeders + no-silent-gap FA-07; gate **241 → 249**) | **UNIT COMPLETE — ALL 6 LEGS GREEN — tag `1040-schedule-f-complete`; RS SPEC LEG COMPLETE; BUILD LEGS 1 (seed) + 2 (compute) + 3 (render) + 4 (input) + 5 (diagnostics) + 6 (assertions) DONE + GREEN 2026-06-21** (leg 6 assertions = LAST, 8 staged) — diagnostics: `rules_schedule_f.py` 8 D_SF_* + 2 D_SE_FARMOPT (D_SF_461L = flat $100k Ken-approved; loss rules bridge-gate + skip accrual; farm-aware `d_8995_001`); 17/17 + 10 rules seeded active. — migration 0100 (`ScheduleF` per-farm model + `ScheduleFOtherExpense` 32a-f child + 3 farm-optional cols on `ScheduleSE`) + RLS 0101 + `seed_schedule_f` (71 lines/3 sections, build.sh auto-discovers) + serializer/CRUD (`schedule-f` + nested other-expenses) + `schedule_f_farms` nested read; 19/19 `test_schedule_f_seed_leg.py` + flow gate 241 + check/makemigrations clean. Legs 2–6 next. Cash-method v1, **per-farm** (multi-Schedule-F). **Verified vs 2025 IRS PDFs:** L9 = 1c+2+3b+4b+5a+5c+6b+6d+7+8; L34 = L9−L33 → **Sch 1 L6 + Sch SE L1a**; CRP → SE 1b; L14 depr ← 4562; farm net → 8995 QBI. Farm optional SE (2025 $7,240/$10,860/$7,840; 2026 RED). RED-defers (8 D_SF_*/2 D_SE_*): accrual Part III, CCC §77 / crop-ins §451(f) / weather §451(e) elections, passive (8582), at-risk (6198), §461(l). **Schedule J = fast-follow** (own RS spec). |
| **Schedule J (farm income averaging)** | ✅ **SPEC LEG 2026-06-21 — SEEDED + EXPORTED + STAGED (Ken-approved walk; RS `60521bf`).** SCHEDULE_J seeded (42 facts/20 rules/25 lines/5 diagnostics/10 scenarios/23 cited links + 8 FA); RS DB 65→66, FA 232→240; canonical `schedule_j_spec.json` + 8 FA staged in `flow_assertions_1040_schedule_j_pending.json`; math gate `check_schedule_j_integrity.py` ALL CHECKS PASS (independent SJ-T1 chain L23=31,982 + cell-by-cell constant cross-check); guard verified; deployed export HTTP 200. Walk rulings: Q-A line-4 SDTW reduces cy Sch D by 2b/2c floored at 0; Q-B line 6 round-half-up; Q-C SDTW lines 44/46; Q-D D_SJ_ELECT_HIGH warning. Brief `_schedule_j_source_brief.md`. Spec-first probe: NO prior RS spec (404). Law verified vs the actual 2025 IRS PDFs (pymupdf) → `server/specs/_schedule_j_source_brief.md`: 23-line chain (L23→1040 L16); base-year rate schedules 2022/23/24 verbatim; base-year QDCGT (27L) + breakpoints; **Schedule D Tax Worksheet = identical 47-line structure all of 2022/23/24/25** (one year-keyed engine, only breakpoints differ). Both TY2025 (base 22/23/24) + TY2026 (base 23/24/25) buildable now. Ken-locked v1 (AskUserQuestion): base-year amounts direct-entry + optional PriorYearReturn pull; **build SDTW + base-year QDCGT**; RED-defer prior-Sch-J chaining / zero-or-less Taxable-Income worksheets / Form 2555-FEI. KEY: base-year tax (L8/12/16) uses base-year RATE SCHEDULES (never the Tax Table). Stale-cross-ref flag (SDTW lines 44/46, not the instr's 34/36 & 42/44). 4 OPEN requires_human_review Qs (brief §10): Q-A line-4 cap-gain netting (load-bearing), Q-B line-6 rounding, Q-C stale cross-ref, Q-D elect-when-higher. | ✅ **INPUT LEG DONE + GREEN 2026-06-22** — `ScheduleJSection` (FormEditor.tsx) singleton election tab (the Taxpayer pattern): "Elect income averaging" toggle + election card (2a/2b/2c + collapsible current-year preferential detail) + 3 per-base-year cards (filing-status select / taxable income negative-OK / original §1 tax / preferential detail / `used_schedule_j`+`form_2555` tri-states) + computed YELLOW preview (lines 4/6/8/12/16/17/22/23 → 1040 L16, elected-only); GREEN direct entry. Singleton CRUD `PATCH /schedule-j/` (PATCH-creates; uncontrolled money on-blur keyed on `sj.id`). Dispatch `activeTab==="schedule_j"` (1040-only); `TaxReturnData`+`schedule_j`; IndividualNav new tab (Income group after Sch F) + dot predicate + `D_SJ_`→`schedule_j` route staged. **tsc clean / vitest 203 (+2) / build clean**; pure frontend (no backend change → flow gate 249). | ✅ **COMPUTE LEG DONE + GREEN 2026-06-21** — `compute_schedule_j.py`: the 23-line §1301 chain + per-line year+status-keyed tax router (rate schedule / base-year QDCGT / Schedule D Tax Worksheet) + engage/blocker gate + `compute_schedule_j_db` routes line 23 → 1040 line 16 (the `route_line_16` precedent), wired into `compute_return` after `_compute_1040_tax`. REUSES the existing engines (no 47-line re-type): added 2022/23/24 to `compute.TAX_BRACKETS` + `QDCGT_CONSTANTS_BY_YEAR` + `SDTW_BRACKET32_START`, and parameterized `compute_qdcgt_worksheet` / `compute_sdtw` with `ordinary_tax_fn` (default unchanged → Topic 3/9 byte-identical; base years pass the rate schedule). `tax_table_lookup` now gates on `_CONSTANTS_BY_YEAR` (={2025,2026}) not bare `TAX_BRACKETS` (base years are rate-schedule-only). Q-A: line-4 SDTW reduces cy Sch D figures by 2b/2c floored at 0; base-year SDTW adds ⅓ of 2b/2c. **31/31 `test_schedule_j_compute_leg.py`** (SJ-T1 anchor L23=31,982 pure + pipeline + SJ-T2..T10 + engine-constant cross-checks vs the brief + no-2025/26-drift) + **flow gate 249** + spine/intdiv/Topic 9 regression green; check/makemigrations clean (no model change). | ✅ **RENDER LEG DONE + GREEN 2026-06-22** — `field_maps/f1040sj_2025.py` (all 27 PDF widgets mapped by position: page 1 header name/SSN + lines 1/2a/2b/2c/3-17, page 2 lines 18-23; full coverage, no dup) + `render_schedule_j` (ONE face per return, model-driven via `compute_schedule_j_chain`, gated on `schedule_j_engaged` — non-election/blocker prints no face; -0- literal for lines 7/8/12/16 when zero; both pages content, no SKIP_PAGES) + registered in ACROFORM_FORM_IDS + packet append after Schedule F (seq 20). **9/9 `test_schedule_j_render_leg.py`** (map-vs-PDF + full-coverage + SJ-T1 anchor position sweep [L1 180,000 … L23 31,982] + negative-base-year "(5,000)"/"-0-" + engage/blocker gates + packet 2-page) + flow gate 249. **DEFERRED (tracked):** base-year QDCGT/SDTW **statement pages** (brief §11) — the face shows the worksheet-derived taxes; the line-by-line worksheet breakdown needs a `schedule_j_worksheet_details` helper (compute stores only the final columns). | ✅ **8 wired 2026-06-22** (FA-1040-SCHJ-01..08 via `_run_schj_assertion` — PURE chain re-derivation for 02/03/07 + source-/constant-pins for the route + rate-schedule/SDTW/QDCGT engine-reuse 01/04/05/06 + no-silent-gap registry+blocker FA-08; **gate 249 → 257**) | **UNIT COMPLETE — ALL 6 LEGS GREEN — tag `1040-schedule-j-complete`. SPEC LEG COMPLETE + BUILD LEGS 1 (SEED) + 2 (COMPUTE) + 3 (RENDER) + 4 (INPUT) + 5 (DIAGNOSTICS) + 6 (ASSERTIONS) DONE + GREEN 2026-06-22.** Assertions leg (leg 6 = LAST, 8 staged): merged via `merge_schedule_j_assertions.py` + `_run_schj_assertion` (id-prefix dispatch in all 3 entry points), flow gate 257, no app code change. Diagnostics leg: `apps/diagnostics/rules_schedule_j.py` (NEW) wires the 5 D_SJ_* (3 error: 2A_EXCEED/CHAIN/2555; 2 warning: NEG_TI/ELECT_HIGH), each gated on an active election (`sj.elected`); blockers read the `schedule_j_blocker` predicates but evaluated INDEPENDENTLY so they co-fire; D_SJ_ELECT_HIGH gates on `schedule_j_engaged` + NEW pure `compute_regular_tax` (re-derives the regular tax the compute leg overwrote on 1040 L16) vs the chain's line 23; base years dynamic via `base_year_of`. Registered via `RULES_SCHEDULE_J` in `runner.seed_builtin_rules`; **12/12 `test_schedule_j_diagnostics_leg.py` + flow gate 249 + Sch-J compute/render/seed regression 55/55 + check clean + 5 rules seeded active in the shared DB.** NO migration / NO gated-file change. NEXT = build leg 6 (ASSERTIONS — the FINAL leg): merge the 8 staged FA-1040-SCHJ-01..08 (gate 249→257) via `merge_schedule_j_assertions.py` + a `_run_schj_assertion` runner (the `_run_schf_assertion` precedent), tag `1040-schedule-j-complete`. Still tracked: deferred base-year QDCGT/SDTW statement pages (render follow-up). Seed leg: `ScheduleJ` OneToOne(TaxReturn) model (42-field surface, base-year TI negative-OK) + **migration 0103 + RLS 0104 (applied)** + `ScheduleJSerializer` (computed cols read-only) + **singleton CRUD** (`/schedule-j/` GET/PUT/PATCH/DELETE) + nested read `schedule_j` (SerializerMethodField) + `seed_schedule_j` (25 lines) + `f1040sj` manifest/PDF. **15/15 `test_schedule_j_seed_leg.py`** (spec trip-wires + CRUD + negative-TI + nested read) + flow gate 249 (no regression) + check/makemigrations clean. NEXT = build leg 2 (COMPUTE): `compute_schedule_j.py` — 23-line chain + year+status-keyed rate-schedule/QDCGT/**NEW SDTW** engines + `route_line_16` + **merge 2022/23/24 into spine `compute.TAX_BRACKETS`**; then render → input → diagnostics (5 D_SJ_*) → assertions (gate 249→257). QDCGT + `route_line_16` precedent `compute_intdiv.py`. The Schedule F fast-follow. |
| **Schedule E Part I (rentals/royalties) + Form 8582** | ✅ `schedule_e_spec.json` + `form_8582_spec.json` | ✅ "Rental (Sch E)" tab | ✅ `compute_schedule_e.py` + `compute_8582.py` | ✅ `f1040se`/`f8582` | ✅ 6 (SCHE-01/02, 8582-01..04) | **BUILT 2026-06-14** (Ken-approved walk) — the prior ❌ was a stale tracker. Part I net + the $25k §469(i) special allowance; line 26 → Sch 1 L5 (becomes an addend within line 41 at effort #5). |
| **Schedule E page 2 — K-1 router (SCHEDULE_K1)** | ✅ **SPEC LEG 2026-06-21** — SCHEDULE_K1 seeded (31f/11r/21L/10d/12s) + SCHEDULE_E amended page 2 (lines 27-43, +3r/+2d); RS DB 63→64, FA 217→224; canonical `schedule_k1_spec.json` + 7 FA staged; integrity gate ALL PASS; guard verified | ✅ **build leg 4 (input) 2026-06-21** — un-stub `schedule_e_p2`; `ScheduleK1Section` (per-entity cards; source_type drives box labels; owner T/S/J; mat-part/PTP toggles; grouped Sch-E/cross-form/v1-defer fields; YELLOW computed preview; RED EIN validation); nav un-stub + `D_K1_`→tab; tsc/vitest 196/build clean | ✅ **build leg 2 (compute) 2026-06-21** — `compute_schedule_k1.py` (central aggregator) + `compute_schedule_e_db` page 2 / **line-41 repoint** + cross-form addends (intdiv 2b/3b/3a, Sch D 5/12, Sch SE 1065-14A, 8995 2/6) | ✅ **build leg 3 (render) 2026-06-21** — `f1040se_2025.py` page-2 map (153 entries, 0 missing/dup) + `render_schedule_e` Parts II/III/V (per-entity rows + totals + **line 41** + page-2 header; 31/36 abs-injected) + conditional page-2 un-skip + Sch D 5/12 K-1 face + 8995 line-1 K-1 QBI; 18/18 `test_schedule_k1_render_leg.py` | ✅ **build leg 6 (assertions) 2026-06-21** — merged 7 FA-1040-K1-01..07 (gate **234 → 241**) via `merge_schedule_k1_assertions.py` + **repointed FA-1040-SCHE-01 → line 41**; new `_run_k1_assertion` (PURE re-derivation via the extracted `schedule_e_p2_totals_from_rows` core for FA-01/02/03 + source-pins on the cross-form feeders for FA-04/05/06 + no-silent-gap blocker check FA-07), id-prefix dispatch in all 3 entry points; flow gate 241 + K-1 legs 53, no regression. **TAG `1040-k1-router-complete`** | **EFFORT #5 COMPLETE — recipient K-1 full router, all 6 legs green (tag `1040-k1-router-complete`).** Recipient K-1 full router (1065 partner / 1120-S shareholder / 1041 beneficiary). **Verified vs 2025 IRS PDFs:** §199A 20Z/17V/14I → 8995 (2/6); SE 1065 14A → Sch SE; **line 41** (not 40) → Sch 1 L5. v1 RED-defers passive losses / §1231 / AMT / basis / REMIC / 4835 (10 D_K1_* + 2 D_SCHE_*). New per-entity model (NOT `k1_allocator.py`, which is issuer-side). Source brief `_schedule_k1_source_brief.md`. **BUILD LEG 1 (seed) DONE 2026-06-21 (`ef7633a`):** `ScheduleK1` model + mig 0098 / RLS 0099 + serializer/nested/CRUD + `seed_schedule_e` page-2 lines (Sch E now 52L/4 sections); 12/12 seed tests + flow gate 234 (no regression). **BUILD LEG 2 (compute) DONE 2026-06-21:** central `compute_schedule_k1.py` + page-2 Part II/III aggregation + line-41 repoint (Part-I-only line 41 == line 26, FA-1040-SCHE-01 source-string stays green) + cross-form routing (2b/3b/3a, Sch D 5/12, Sch SE 1065-only, 8995 2/6); passive losses RED-deferred (col g/c = 0); 13/13 `test_schedule_k1_compute_leg.py` + flow gate 234 + Sch E/intdiv/topic8/topic9 regressions green. **BUILD LEG 3 (render) DONE 2026-06-21:** `schedule_e_p2_rows()` per-entity FACE decomposition (reconciles to the compute totals) + page-2 field map + `render_schedule_e` page 2 + conditional `SKIP_PAGES` un-skip (`schedule_e_p2_has_content`) + Sch D 5/12 (direct + K-1) + 8995 line-1 K-1 QBI rows; 18/18 render tests + flow gate 234 + compute-leg/topic8/topic9 (99) green. Row caps: line 28 ×4, line 33 ×2, 8995 line 1 ×5 (overflow → totals, flagged). **BUILD LEG 4 (input) DONE 2026-06-21:** un-stubbed the `schedule_e_p2` tab + `ScheduleK1Section` (per-entity cards; source_type 1065/1120-S/1041 drives the box labels + hides inapplicable fields; owner/mat-part/PTP; grouped Sch-E / cross-form / v1-defer money + flags; **YELLOW** computed-totals preview from `field_values`; **RED** EIN entry validation); `IndividualNav` un-stub (collection dot + `D_K1_`→tab); `ScheduleK1Row` type. tsc clean / vitest 196 / build clean (no backend change). **BUILD LEG 5 (diagnostics) DONE 2026-06-21:** new `rules_schedule_e_p2.py` wires the 10 D_K1_* (5 RED-defer errors — passive-loss/§1231/28%-§1250/AMT/other-income; 3 warnings — basis-at-risk/foreign-K3/PTP-loss; 2 info — S-corp-not-SE/line-41-flow) + 2 D_SCHE_* (REMIC line 39 / Form 4835 line 40), registered via `RULES_SCHEDULE_E_P2` in `runner.seed_builtin_rules`; each reuses the compute helpers — new shared **`k1_sche_net()`** in `compute_schedule_k1.py` (passive/PTP-loss bridge-gate, refactored from the 3 inline `net=` sites so the RED can never disagree with the column math) + `schedule_e_p2_totals` (flow info); offending-entity names appended to each finding. 18/18 `test_schedule_k1_diagnostics_leg.py` + **flow gate 234 + K-1 compute leg (251 passed, no regression)** + `manage.py check` clean; 12 rules seeded active in the shared DB (client routing `D_K1_`→`schedule_e_p2` already wired in leg 4). **BUILD LEG 6 (assertions) DONE 2026-06-21:** merged the 7 staged FA-1040-K1-01..07 into the active gate (**234 → 241**) + **semantic-repointed FA-1040-SCHE-01 → line 41** via `merge_schedule_k1_assertions.py`; new `_run_k1_assertion` runner (id-prefix dispatch `FA-1040-K1-*` in all 3 entry points) — PURE re-derivation through the extracted pure core `schedule_e_p2_totals_from_rows()` for FA-01/02/03 (line 41 = 26+32+37; line 32 = (h+k)−(i+j), col g = 0; line 37 = (d+f)+(c+e), col c = 0), source-pins on the cross-form feeders for FA-04/05/06 (intdiv 2b/3b/3a, Sch D 5/12, Sch SE 1065-only, 8995 2/6), no-silent-gap blocker check for FA-07. **flow gate 241** + K-1 compute/render/diagnostics legs **53 passed — no regression** (the from_rows refactor is value-identical). **EFFORT #5 COMPLETE — all 6 legs green, tag `1040-k1-router-complete`.** |
| **Form 8863 (Education — AOTC + LLC §25A)** | ✅ `form_8863_spec.json` (NEXT-UP #8) | ✅ Education (8863) tab (`EducationSection`) + **per-dependent AOTC checkbox (Leg B.3, 2026-06-18 — auto-links an `EducationStudent` owner=dependent via the new `dependent` FK, mig 0093, SET_NULL; check→POST pre-filled, uncheck→confirm-if-detail→DELETE)** | ✅ `compute_8863.py` (AOTC 27-30 → Part I; LLC; Credit Limit Worksheet → Sch 3 L3; refundable AOTC → 1040 L29) **s143 2026-07-30 (`4c76624`) COMPUTE AMENDMENT — the zero-residue fix (backlog LEG 2 item 2):** the nonrefundable credit was written to Sch 3 line 3 UNCONDITIONALLY while the refundable half five lines up already wrote "" at zero, so a student with no allowable credit (MFS-barred / phased out / no qualifying expenses) stored a literal "0" — which then defeated `disengage()`'s `!= ZERO` guard and survived deleting every student. Fixed at BOTH ends: blank at zero on the way in, "is anything stored" on the way out. No dollar moves. Gates: 8863 band 86 · flow 521. 🔴 **STILL OPEN — the dual-student COMPUTE fix (one student taking both credits, $800) is NOT built:** RS `R-8863-LLC` carries the same defect (`L10 = sum of student LLC expenses`, no section 25A(c)(2)(A) exclusion, the rule exists only as a warning diagnostic) and the statute is an ELECTION, so it needs Ken's ruling — see REVIEW_QUEUE. ⚠ RS key is `FORM_8863`, not `8863`.| ✅ `f8863_2025.py` | ✅ FA-1040-8863 (`flow_assertions_1040_8863_pending` merged) | **NEXT-UP #8 COMPLETE.** Diagnostics `rules_8863.py` (8: MFS/dependent bars + dual-student/no-credit/lockout/lockout-NA/AOTC-inelig/TY2026). **s170 2026-07-31 — LEG 2 item 6 RESOLVED BY VERIFICATION:** the line-7 lockout's `any()` is CORRECT (return-wide by law — f8863 face + §25A(i)(5) + IRS8863.xsd; the s138 'per student' recommendation was wrong); shipped the reworded D_8863_LOCKOUT (filer condition), NEW D_8863_LOCKOUT_NA (⚠ seed_rules at deploy), the MeF `RefundableAmerOppCrUnder24Ind` emission (the e-file leg's missing checkbox), and both screens' copy; scope pinned with citations. **Leg B.3** added the claim bridge + **`D_CREDIT_AOTC`** unified-engine gate (barred/no-expenses→error, MAGI-over-ceiling→info; bridge-gated on `compute_8863`). *(EIC, Form 2441, and 8995 are tracked in their own rows/topics — this row replaces the stale catch-all that wrongly read ❌.)* |
| **Form 8615 (Kiddie Tax §1(g))** | ✅ `8615_spec.json` (seeded 2026-06-24 — 31 facts/10 rules/19 lines/7 diag/8 scenarios/9 FA) | ✅ **INPUT LEG 2026-06-24** — singleton `form-8615` CRUD route (the Schedule J pattern) + `Form8615Section` on a new "Kiddie Tax (8615)" tab (Other taxes group): `applies` §1(g) + parent facts (TI L6/tax L10/filing status/QD/net-cap-gain/Σ siblings L7) GREEN, child line-2 detail + 3 RED-defer flags tri-states, YELLOW child amounts + computed preview (L18→1040 L16), RED defer banner | ✅ **COMPUTE LEG 2026-06-24** — `compute_8615.py` ordinary §1(g) core (L2-18 → max(L16,L17) → 1040 L16 late-feeder after Sch J; reuses `compute_tax_line_16`); RED-defers QD/net-cap-gain (`QDCGT_PENDING`), §1250/28% SDTW, Sch J, Form 8814 | ✅ **RENDER LEG 2026-06-24** — `f8615_2025.py` (19 lines + 2 header, verified vs the real PDF) + `render_8615` reads persisted columns | ✅ 9 FA merged 2026-06-24 (FA-1040-8615-01..09 via `merge_8615_assertions.py` + `_run_8615_assertion`; gate 292→301) | **UNIT FULLY COMPLETE — tag `1040-8615-complete`** (all 6 legs + the QDCGT cap-gain follow-up; ordinary + qualified-dividend/net-cap-gain). DIAGNOSTICS: `rules_8615.py` 7 D_8615_* (D_8615_008 RETIRED w/ the QDCGT follow-up); NARROWED D_1040_004 → defer-to-applies + verified $2,700 TY2026 threshold + error→warning. ASSERTIONS: PURE compute_8615 re-derivation + source-pins; FA-06 asserts the compute_qdcgt_worksheet reuse. **QDCGT FOLLOW-UP (mig 0116):** sibling QD/cap-gain fields (Ken: collect per-sibling); `_line5_split` = i8615 Line 5 Worksheets 1/2/3; `_tax_at` routes L9/15/17 through compute_qdcgt_worksheet. Parent + per-sibling data = preparer-asserted. Bugs fixed: `compute_8615_db` + `rules_8615` + `render_8615` all fetch fresh (the reverse-OneToOne cache bit all 3 readers). CPA-review care items: WS2/WS3 prorations; net cap gain from 1040 L7 (no-Sch-D, Sch-D 15/16 netting a future refinement). **SLATE 2026-07-30 (s147, sweep unit 27, `1c0fcc1`)** — `SlateForm8615Screen` (the s132 singleton; no server change). ⚠⚠ **"UNIT FULLY COMPLETE" IS NOT TRUE AND THIS ROW IS THE SECOND PROOF OF THE MISSING-COLUMNS ITEM** (the SCH_1A precedent, STATUS open item 31): **(a) NO E-FILE LEG AT ALL** — no `IRS8615` builder anywhere in `apps/efile/` and no IRS8615 element in the ReturnData1040.xsd sequence, while line 18 overwrites 1040 line 16 which transmits as `TaxAmt`; a kiddie-tax return is PAPER-ONLY (the s140 Schedule 1-A shape, 2nd occurrence). **(b) THE RENDER LEG IS INCOMPLETE** — `f8615_2025.py` maps 19 lines + the CHILD's 2 header fields but NOT box A (parent's name), box B (parent's SSN) or box C (parent's filing status, `c1_1[0..4]`), so the printed form's whole parent header is blank; box C's data IS stored. **(c) COMPUTE — line 1 counts only 1040 2b + 3b + max(0,7)**, while i8615 counts all unearned income (rents, royalties, pensions/annuities, taxable SS, taxable scholarships, unemployment, alimony, TRUST beneficiary income) and directs a child with no earned income to enter AGI; engine-proven 1,767 and 1,217 UNDERSTATED on two returns, and `child_unearned_income` is read-only so there is no override. **(d) LATENT** — `line2_amount_for`/`std_floor_for` fall back to `_LATEST_YEAR` for unpinned years (no dollars move today; 2025 = 2026 = $1,350/$2,700). Presentation fix shipped: the screen no longer claims QD/net-cap-gain defer to manual (built 2026-06-24; `D_8615_008` retired the same day). |
| **Form 5695 (Residential Energy §25D + §25C)** | ✅ `FORM_5695` seeded 2026-06-15, **amended to v2 2026-07-25 (s110)** — the light 2025 face: 21→46 facts / 4→6 rules (+R-5695-GATE, +R-5695-QMPIN) / 15→27 lines / 6→12 diagnostics / 9→18 scenarios / 6→8 FA. Integrity gate `check_5695_integrity.py` re-typed and passing; seeded to RS prod, deployed export verified, mirror `5695_spec.json` refreshed from it. ⚠ first form in the RS DB with two version rows (lookup is `order_by("-version")`; v1 set `archived`). | ✅ **s110** — the Energy Credits tab now carries the whole face: the main home address (keyed once, prints in all four blocks), the 10 tri-state eligibility gates, a QM ID box beside every cost that has one (RED the moment a cost is keyed without it), the doors most-expensive/all-other split, the 25b enabled-property code and the 32a/32b checkboxes. Denied branches collapse with a plain-English note. `form5695LightFace.test.tsx` **19**. | ✅ compute_5695 — v2 adds `credit_25c_parts()` (the ONE place the §25C caps live; render + diagnostics both read it), the gate logic (an explicit No denies exactly the branch the face skips; **21a/21b No takes line 29 with it**; null NEVER denies) and the corrected doors chain 19c/19h. **s173 (2026-07-31) corrected BOTH Credit Limit Worksheets to the 2025 i5695 verbatim** — §25C is figured FIRST (line 31 = 1040 line 18 − Sch 3 [6l,1,2,6d,3,4]) and §25D then subtracts Form 5695 line 32, Sch 3 [6m,6f,6g,6c,6h] and 1040 line 19; the old §25D-first order off a lines-1–4-only limit destroyed up to $3,200 of §25D carryforward and overstated a CTC return's Sch 3 5a. ⚠ **This SPLIT the compute into two phases** (`compute_5695_db` before Schedule 8812, `compute_5695_25d_db` after it — line 14 needs the CTC), and the i5695's Schedule 8812 Worksheet-B footnote is a KNOWN unmodelled path (REVIEW_QUEUE). `test_form5695_compute_leg` **37**. | ✅ `render_5695` — v1 filled Part I + a Part II summary and left the §25C detail blank, which is not a filable form (the QM ID boxes live on those lines). v2 fills the eligibility checkboxes (an unanswered one ticks NEITHER box), all four address blocks, every per-category cost with its PIN split across the template's `Box1-4`/`Box5-17` widgets, 19a-19h, 25b, 27/28/29g/29h/30-32 and 7c/32a/32b. Map 19→125 keys. `test_form5695_render_leg` **12**. | ✅ **FA-1040-5695-01..08** (+07 doors cap, +08 the gates) | **v1 2026-06-15 (Ken: "the very least"); v2 2026-07-25 (Ken: "a light version, good enough to complete the tax return").** ⚠⚠ **v1's doors math OVERSTATED**: the face caps the MOST EXPENSIVE door at $250 on its own before the $500 aggregate, so one replaced $2,000 front door gave $500 where the form gives $250. ⚠⚠ **§25C(h) makes the QM ID number a CONDITION of the credit** for property placed in service after 2024-12-31 — TY2025 is the first year it bites and, thanks to the OBBBA termination, the last. Deferred by design: the per-item PIN slots (arithmetic-neutral — 19f = 19d + 19e), joint-occupancy / fractional-share allocation, the 17e construction split (all three warn). Diagnostics 6→12, `seed_rules` + `seed_form_5695` re-run on BOTH DBs. ⚠⚠ **s151 (unit 31, Slate screen): this row's green legs are NOT the whole form — the two legs the table has no column for both have findings.** (1) **E-FILE: no `build_irs5695` exists** while Schedule 3 lines 5a/5b transmit and `ReturnData1040.xsd` expects `IRS5695` (maxOccurs 2) — the 5th missing-MeF-document occurrence; a claiming return is paper-only (REVIEW_QUEUE s151, LEG 3). (2) **COMPUTE: both Credit Limit Worksheets diverge from the 2025 i5695** — ordering inverted (the form runs §25C FIRST; §25D's line 14 subtracts line 32, the CTC and Sch 3 6m/6f/6g/6c/6h) — priced $1,000 of §25D carryforward destroyed / $2,000 of Sch 3 5a overstated-and-transmitted on a CTC return (REVIEW_QUEUE s151, LEG 2; the s110 "simplified CLW" deferral, now specified verbatim and priced). The Slate screen (`SlateForm5695Screen`, s151) states both at the cells, keeps denied-branch cells VISIBLE (the legacy screen hid them while the stored costs kept driving the engine), and renders the engine's 17-row face as locked ƒx cells. |

| **Form 8853 Section C (long-term care / accelerated death benefits)** | ✅ **SPEC LEG 2026-08-08 (s232) — `8853_SEC_C` AUTHORED, Gate-1 APPROVED, SEEDED, EXPORTED, CACHED** (`8853_sec_c_spec.json` — 23 facts / 10 rules (all cited) / 14 face lines 14a-26 / 12 diagnostics / 14 scenarios / 5 FA; RS `a65ce4f`→`7451a27`, deployed `lookup/8853_SEC_C/export/` = 200). **Section C ONLY** per Ken's s224 ruling; Archer / Medicare Advantage MSA sections deliberately unspecced (`8853_SEC_AB` reserved) and **there is deliberately NO spec under the bare `8853`** — a spec covering half a form while claiming all of it is the s231 Form-3800 defect. 1099-LTC gets no spec (s222 rule for information returns) → `f8853_1099ltc_source_brief.md` in the RS repo. Integrity gate `check_8853_sec_c_integrity.py` shares no math; teeth proven by a 6-defect negative control, all 6 caught. | ⚠ **LEG 1 ✅ s242u (2026-08-11, `ab5ed8e`, migs 0308/0309)** — `Form8853LTC` model (one row per insured × LTC period, RLS default-deny) + lane section `form_8853_ltcs` (order-independent; staging refuses 18 > 17, modified-without-contract, missing insured) + **"8e" joined `SCH1_DIRECT_LINES`** as the keyed A/B residual's carrier; schema regenerated. **LEG 2 ✅ s242v (`3b33982`, migs 0310/0311)** — `Form1099LTC` received-doc model built per the brief (box3_basis explicit "unchecked"; boxes 4/5 nullable — absence never inferred) + `ltc_1099s` lane + **all 12 spec diagnostics** in `rules_8853.py` (dict registry, runner-wired; insured TIN/name matching for the four 1099-LTC cross-checks). Two leg-1 gaps closed: duplicate-insured rows refuse as multi-period; `form_8853_ltcs`/`ltc_1099s` in SECTION_RELATED + the structural LIST_SECTIONS⊆SECTION_RELATED pin. ❌ Still pending: the browser tab/UI. | ✅ **LEG 1 s242u** — `compute_8853.py` per the spec line by line (20 = 18+19; 21 = 420 × days; 23 = MAX(21,22); **25 = MAX(0, 23−24)**; 26 = MAX(0, 20−25)) with the terminally-ill short circuit (§101(g)(1) ADB-only skip), the pre-Aug-1996 unmodified-contract line-24 zeroing (T10), and the four refusals contributing NOTHING (multipayee / line-15 unanswered / multi-period / days out of 1-365 — `REFUSED_*` constants for leg 2). **The composed 8e shipped with it**: `compute_8853_db` delta-adjusts Section C's component around the keyed residual with the FORM_8853 worksheet's `sch1_8e_component` row as engagement memory (zero-movement pinned by T13; add+remove backs out; an overridden 8e wins). Rate $420 = Rev. Proc. 2024-40 §2.62, confirmed three ways (Rev. Proc. verbatim + printed on the 2025 face at line 21 + i8853 Example 1's footnote); `PER_DIEM_RATE` raises on an unverified year — it is INDEXED and must be re-pulled each year. 24 tests transcribe the spec's T1-T14 incl. the three IRS worked examples. | ✅ **LEG 3 s242w (`6952243`)** — f8853 registered + fetched byte-identical to the s232 SHA pin; 19 page-2 widgets caption-verified (both Yes/No pairs YES-first); `render_8853` emits PAGE 2 only, one copy per insured, under the R-8853C-FILING partial populations (out-of-set lines BLANK; refused rows print keyed inputs only; explicit zeros as the face's "-0-" literal); "Form 8853" in GENERATED_FORM_RENDERERS — the 8e attachment requirement is satisfiable while a rows-less keyed 8e (the A/B residual) still warns. ✅ **LEG 4 MeF s242x (`989c7f7`)** — the IRS8853 document (Section C group only; XSD member order verbatim; amounts mirror the render's populations). ⚠ THE XSD HOLDS ONE SECTION C PER RETURN (paper holds many) — a second contributing insured refuses by name. ⚠ A live latent reject closed: a keyed 8e had always transmitted via `TotArcherMSAMedcrLTCAmt` with NO document (S1-F1040-022) — a nonzero 8e without rows, or a nonzero A/B residual beside the component, now refuses by name (paper remedy). Unanswered 15/16 refuse (REQUIRED BooleanType members). **THE UNIT IS COMPLETE across s242u/v/w/x.** | ⚠ **STAGED (s242x, `bb4c4d8`)** — the five FA-1040-8853C export rows carry EMPTY definitions (titles authored s232; runnable definitions never written in RS). Staged in `flow_assertions_1040_pending.json` (with five older stragglers); the T-scenario values they encode are already pinned by `test_form_8853c_s242u.py`. Authoring the RS definitions = the RS-agenda item. | **⚠⚠ THE CENTRAL FINDING — Schedule 1 line 8e is a COMPOSED line and the IRS's own schema says so:** its MeF element is **`TotArcherMSAMedcrLTCAmt`** (Archer MSA + Medicare Advantage + LTC) and the face says "include this amount in the **TOTAL** on line 8e". Per DECISIONS.md (s230, Schedule K 13g) the writer is a **REGISTRY** — the build composes 8e = Section C component + preparer-keyed A/B residual. A single-writer build would silently erase a keyed Archer figure: a DISAPPEARED number, which is why nobody would report it. **⚠ The destination already exists and already fails**: 8e is seeded "Income from Form 8853" as a KEYED line and `form_manifest.py` already declares `AttachmentRequirement("Form 8853")` on it — `test_form_manifest.py` pins the comment *"Form 8853 — never generated"*, which is the build's acceptance criterion to delete. **⚠⚠ THE STATUTE CORRECTED THE DRAFT**: §7702B(d)(2) verbatim makes the limitation "the **excess (if any)** of (A) … over (B)", so **line 25 FLOORS AT ZERO — and the face prints no floor there** (only line 26 carries "-0-"). Drafted unfloored off the face plus a paraphrase fetch that had dropped the phrase; a second fetch from uscode.house.gov caught it. Live defect: line 20 = 10,000 with line 25 = −5,000 gave line 26 = 15,000, taxing more than was received. Pinned by T14 + a structural gate invariant. Ken approved the floor explicitly at Gate 1. **Declared refusals (never silent):** Multiple Payees line 15 = Yes REFUSED because an unshared limitation UNDER-reports income; lines 15/16 three-state so the permissive answer is never a silent default; multi-period, days outside 1-365, and the 1040-NR Sch NEC destination all hold. Scenarios T1-T3 are the IRS's own worked examples verbatim. One `requires_human_review` flag live: §101(g)(3). |

**1040 supporting infrastructure (verified 2026-06-10 PM #9):**
- Models: Taxpayer (incl. migrations 0047 std-ded + 0048 estimated-payments + 0049 diagnostic facts + 0053 Topic 3 facts + **0056 Topic 5 SSA/5329 facts**), W2Income + Box12/Box14 entries, InterestIncome (17-box + FATCA/nominee/accrued/seller-financed block, 0051), DividendIncome (full 1099-DIV surface, 0051, RLS 0052), **RetirementDistribution (full 1099-R box surface, box 2a nullable, 0054, RLS 0055)**, Dependent, CarLoanVehicle, Employer DB (3,828 rows) with EIN autofill + learning loop
- Input tabs (client): Taxpayer Info (incl. 12a-12e std-ded block + decedent/HOH), Dependents, W-2 Income, Interest Income, **Dividend Income (1099-DIV + Form-8814 assertion card, Topic 3)**, **Retirement Income (1099-R box CRUD + SSA-1099/5329 assertion card, Topic 5)**, **EIC (Taxpayer EIC-facts card + Dependent EIC-flag table + 8867/8862 StandardSection, Topic 7)**, **Schedule B (line 3 + Part III three-state, Topic 3)**, Schedule 1/2/3, Sch 1-A (OBBBA), Credits & 8812, Payments, Tax Summary (summary card + editable 1040 form-line list)
- All **79** Taxpayer input facts (migrations 0042/0043/0044/0047/0048/0049/0050/0053/0056/**0057**) serializer-exposed — pinned by `test_taxpayer_input_leg.py` trip-wire (**+ the 2 Dependent EIC flags `eic_qualifying_child`/`is_full_time_student`**)
- Diagnostics: 17 D_1040 rules (rules_1040.py; **001 retired/inactive 06-11** — bridge replaced) + 14 D_SCH1/2/3 rules (rules_sch123.py) + **10 D_INTDIV rules (rules_intdiv.py)** + **7 D_SCHB rules (rules_schb.py)** + **8 retirement-family rules (rules_retirement.py — 7 D_RET + 1 D_5329, Topic 5)** + **20 EIC-family rules (rules_eic.py — 16 D_EIC + 1 D_SEI + 2 D_8867 + 1 D_8862, Topic 7)** on the shared DiagnosticRule machinery, seeded by `seed_rules` (in build.sh); generic EIN/address/dates/income checks gated away from individuals
- `server/specs/flow_assertions_1040.json`: **86 assertions wired** (8812 + Sch 1-A + 15 spine [SPINE-15 retired 06-11] + ALL 13 Sch 1/2/3 + 10 INTDIV + 2 SCHB + 7 RET [Topic 5] + **9 EIC [Topic 7]**); all 1040 pending files now empty staging slots. Flow gate `tests/test_flow_assertions.py` = **108 passed**.
- Field maps: `f1040_2025.py` (audited), `f1040s8_2025.py`, `f1040s1a_2025.py`, `f1040s1_2025.py`, `f1040s2_2025.py`, `f1040s3_2025.py`
- Manifest: 25 IRS PDF templates
- ~~`build.sh` was missing `seed_sch_8812` + `seed_sch_1a`~~ **fixed 2026-06-10**

## Business entities (mature — pre-campaign)

| Package | Status | Notes |
|---|---|---|
| 1120-S + K-1 + GA-600S | ✅ production-grade | 323 seeded lines, 20 flow assertions, full render package (~25 forms incl. 4562/4797/8825/7203/8949/Sched D/L/M-1/M-2) |
| 1065 + K-1 | ✅ | 285 lines, render package |
| 1120 | 🟡 | 172 lines, basic |
| Trust (1041) | ❌ | Entity type enabled, no forms |

## Rule Studio (deployed: sherpa-tax-rule-studio.onrender.com)

- 30 forms seeded in RS DB (mostly 1120-S family + business forms; all status=draft)
- 1040-relevant seeded specs: **SCH_8812** (1,140 facts/rules, exported + implemented), **SCH_1A** (38 rules, 47 lines, 21 scenarios — seeded 2026-06-10, exported + implemented), **1040 SPINE** (45 rules, 91 facts, 72 lines, 16 diagnostics, 33 scenarios — seeded 2026-06-10 PM #5 on Ken's approval; Session-14 stub retired), plus business forms reusable at 1040 level (8995/8995-A/4562/4797/8949/6198/8582/3800/8283)
- RS has no per-form `ready_to_seed` DB field — the sentinel lives in each loader command; form `status` choices are draft/review/approved/archived
