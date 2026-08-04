# TTS Tax App — STATUS (current state only)

*2026-08-04, cross-repo (Ledger session): "+ New Client" now takes email,
phone and address at creation — optional; only typed fields are sent; a bad
email is refused with nothing created. Normalizing lives in ONE place
(`apps.clients.contact`, shared by the create path and the suite API) so
the two hub write paths cannot drift (D-97). No migration. 55 server tests
(client+suiteapi) + 9 form tests green.*

*Last updated: 2026-08-04, session 208. The MIXED-PILOT batch is
TRIAGED AND MOSTLY WORKED (Ken sequenced it ahead of the 1040 lane, quick
wins first): four items landed (#5 refuted→URL-tab fix · #1 residual
dollar · #4 QBI-wage override · #6 recovery slice), #3 REFUTED as written
(GA-700 already exists end to end), #2 and #7 are true builds NEXT. Then
the 1040 lane resumes at **8606 (046 #10)**. Migration returns.0235 is
latest; s208 adds NO migration.*

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
⚠ NEW (s208): FOUR same-day deploys during a live pilot session produced
the pilot's #5/#6 symptoms (stale-chunk self-heal reloads + restart
windows). Batch pushes when a pilot is actively entering.

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195: **no 2025 returns are being prepared in the app.** Entries exist to
find defects. State the finding and move on.

---

## ▶ RESUME HERE

### ✅ Done in s208 (the MIXED-PILOT batch, quick wins first — Ken's pick)
**#5 State button (REFUTED as a routing bug, real fix shipped):** the route
works — live-verified on the subject 1120-S (State tab → State Returns
screen → created + opened its GA-600S, full Sched 6-8/5/3/1/2/4 rail). The
pilot's "returns to Client Information" = the s200 stale-chunk self-heal
RELOAD landing on the default tab, because the active tab lived only in
component state. FIX: the editor mirrors the active tab to `?tab=` in the
URL (deep links, F5, browser restore, the vite:preloadError reload, and
back/forward all retain the screen; `resolveEditorTab` + 3 pins; the
mirror is gated on returnData.id === route id so cross-editor switches
can't freeze the pre-load default into the URL). Live-verified: reload
retention on federal + GA-600S editors, 1040 home tab intact, Back from
the GA-600S returns to the federal State tab.
**#1 1065 K-1 residual dollar (CONFIRMED, fixed):** per-partner HALF_EVEN
quantizing lost the odd dollar (−53,701 at 50/50 → two −26,850s; Σ K-1 ≠
Schedule K). NEW largest-remainder distribution per K line across the
whole partner set (`_allocate_line_exact` / `_exact_line_shares` →
`allocate_all_k1s`; ties break by partner order; pct sets ≠100% are left
for D_K1_RECON, never force-summed; the WS3a SE base rides the same
corrected set so 14a sums too). Spec authority: RECON-K1-K
(schedule_k1_1065_spec.json, §704). 12 new tests; flow assertions + the
572-test sweep green. NOTE: the 1120-S uses the Ken-ratified R-K1-ROUND
last-owner-absorbs convention — the 1065 mechanism needs its own RS
ruling (agenda).
**#4 Statement A W-2 wages (built as override + diagnostic):** the line
7+8 derivation is RS-pinned (flow assertion) and STAYS the default; the
build is the reviewed-source override the pilot asked for: `QBI_W2_WAGES`
joins a narrow OVERRIDABLE_COMPUTED_LINES allowlist in
SlateStandardSection (Ctrl+Enter opens the override — the old blanket
`noOverride={computed}` premise "the FFV lane has no override write" was
FALSE: update_fields sets is_overridden and compute respects it). State
precedence fixed: an overridden computed line paints OVERRIDDEN, not ƒx.
NEW warning `D_SCHK_QBI_W2_OVERRIDE` names override vs derivation (Rev.
Proc. 2019-11 / Reg. §1.199A-2 cited; equal-to-derivation = "redundant,
clear it"); seed_rules run. Live-verified on the subject 1120-S: the
103,365 override persisted through compute, K-1 share = 103,365, the
diagnostic fires with both figures. The full Rev. Proc. 2019-11 wage
WORKSHEET has no RS spec → RS agenda, not improvised.
**#6 save-timeout recovery (root-caused + slice built):** the pilot's
symptoms line up with the four same-day deploy restarts, not one slow
endpoint — s168's serializer fix is in place (fresh_return is prefetched)
and s119 already provides bounded 30s timeouts, saved_revision acks,
SaveStatusIndicator retry, and idempotency keys on nested creates. BUILT
NOW: explicit failure + Retry states on the three screens that lied or
dangled — Return Manager (both chromes; a failed list fetch showed "No
returns yet"), the editor Preparer tab ("Loading preparers..." forever),
and PreparerManager. REMAINING (next session, design build): idempotency
keys on the taxpayer/fields PATCH lanes + newer-edit-wins arbitration +
per-field unsaved indicator + bootstrap retry.
**#3 GA Form 700 (REFUTED as written):** "the 1065 editor has no
State/Georgia workspace" — GA-700 EXISTS end to end (seed_ga700 S-10,
FORMULAS_GA700, the GA700_FEDERAL_PULL, ga700_2025 AcroForm map,
RULES_GA700, GA_700_SECTION_TABS, create path on the 1065 State tab). The
pilot never found the State tab (see #5; the ?tab fix helps). NEXT: a
verification pass against the pilot's fixture (GA net income −43,621,
zero tax) — real gaps (e.g. the GA 4562 attachment = open item 6) go
through triage.
**Plus: FOUR rotted pins repaired** (red on main since 8f6a78e/74c1bad —
the s198 class): test_returns 1065 seed counts 356→401 / sections 10→11,
and the 1065 render pin still expecting "(102)" after s195c's
the-form-owns-the-parentheses fix.
Gates: server touched-suites + flow assertions sweep green · client
vitest **1,628** (136 files) · tsc 0. NO migration.

### ▶ NEXT: MIXED-PILOT #2 + #7 (true builds), then the 1040 lane
**#2 item K BOY/EOY liability columns** — real build, migration: the app
models ONE share per §752 bucket; the 2025 K-1 item K prints beginning AND
ending for all three classes (DB → Slate → import/export → renderer → MeF
→ roll-forward → diagnostics; next-year BOY defaults from prior EOY).
**#7 K-1 basis/at-risk allowed-loss worksheet (1040 side)** — the
basis-limited checkbox only FLAGS today; the full box-1 loss routes to
Schedule E. Build the worksheet (beginning basis · additions ·
distributions · current loss · allowed · suspended c/f), route only the
allowed loss, keep QBI un-double-limited, persist the suspension.
*(Adjacent to the parked K-1→individual GAP family — consider designing
together.)* Then the 1040 lane resumes: **8606 (046 #10)**, then 047
items 16-20 · the true builds.

### Artifacts left on the shared DB (deliberate, s208)
- The subject 1120-S now HAS its GA-600S (created during #5 verification)
  and carries the 103,365 QBI_W2_WAGES override (matches the source
  return) + its D_SCHK_QBI_W2_OVERRIDE warning awaiting source-verified.
- seed_rules run locally (registers D_SCHK_QBI_W2_OVERRIDE); rides
  build.sh on deploy anyway.

### ⛔ TWO KEN DECISIONS STILL BLOCK A–M ITEM 7 (the 4562 assets)
Unchanged from s207 — see "Open items" #13/#14.

---

## ⚠ Open items for Ken
*(carried from s207 except as noted)*

1. **The §179 base needs a rule for three components I would not guess.**
   Included: nonpassive K-1 ordinary income (Reg. §1.179-2(c)(6)(ii)-(iii)).
   **Excluded pending Ken/RS:** §707(c) guaranteed payments, ordinary 4797
   gain from an active business, 1041 beneficiary K-1s. All three can only
   UNDERSTATE the base. The RS 4562 spec never enumerates.
2. **Rotted pins keep surfacing** — s198 found one, s208 found FOUR more
   (red on main for weeks; scoped sweeps never touch them). The full
   server suite (~6,900) is not run routinely. Worth a weekly full-suite
   cron or a pre-push canary set.
3. **RS `GA600S R001`** still describes the addition as bonus-only — the
   presentation Ken rejected for the gross pair (s197). Fix RS-side.
4. **RS `R-GA500-DEPR` is stale** — denies GA §179 conformity vs HB 1199.
5. **GA §179 does NOT cover certain REAL PROPERTY** (Ken, s196) — needs an
   RS rule; `_calculate_state_ga` applies one flat limit.
6. **No GA 4562 is produced** though both GA-600S copies say "attach GA
   4562" — now ALSO a MIXED-PILOT #3 follow-up (Form 700 packets include one).
7. **Other GA-600S returns will move on their next recompute** (s197).
8. **The Lacerte PDF importer does not read the GA depreciation pages.**
9. **Federal-bonus-history + no keyed GA prior over-depreciates for
   Georgia** — diagnostic wanted, not built.
10. **RS rule for the shareholder-side §179 disposition** — blocks K-1 GAP 1.
11. **GA PTET base on a separately stated gain** — unchanged from s195.
12. ~~046 #8~~ — RULED + BUILT (s199e). **RS `R-AGG-SUMMARY` spec edit**
    still pending on the RS agenda.
13. **The three A–M #2/#3 assets are not linked to an activity** — return A
    has exactly ONE rental (auto-link unambiguous), return B has THREE,
    return C has NONE (its laptop asset probably belongs to Schedule C).
    Auto-link only when exactly one candidate, or preparer pick?
14. **One stored asset still carries convention HY** and computes $0 — but
    the return is recorded as an EXACT TIE, so a repair to MM (1,942/yr)
    could DOUBLE-COUNT. Needs Ken's eyes before any data change.
15. **`SCHED_L_DEPR_TIE` can false-fire on a no-Schedule-L entity return**
    (the B11 gate exists on the s205 D_1120S_L_* family, not this older rule).
16. **Client #2969 duplicate** · **retire `reparent_business_entities`** ·
    **client-delete UI (no path exists)** · **duplicate guard blind to
    entity names**.
17. ~~M-2 AAA distribution cap~~ — RULED + BUILT (s205b). M-2 line-7 FACE
    stays IRS-capped — pin equity/Schedule L, not that face.
18. ~~February 'false' residue~~ — RULED + EXECUTED (s205b); rollback
    snapshot in tax-test-data.
19. **The N-Z #8 8962 target (s204)** — relayed full repayment contradicts
    i8962 Table 5 at 305% FPL. Verify against the actual PDF at entry;
    escalate, don't force the tie.

## The three K-1 → individual gaps (Ken's unit, parked for the backlog)
**GAP 1** shareholder-side §179 disposition — BLOCKED on an RS 4797 rule.
**GAP 2** Georgia Shareholder Summary — buildable; **the Lacerte artifact
is not on disk**, ask Ken to re-send.
**GAP 3** GA individual modifications carryover — needs open item 4 first.
*(MIXED-PILOT #7's basis worksheet is adjacent — consider one design.)*

## Carried queue
**Lane-schema-only (engine-complete)**: 8863 · 5695 · 8606 · 4797 ·
6252 · 7203 · 1116. *(8880 s202 · 2441 s203 · 8962 s204 · 8283 s206.)*
**True builds**: Sch F lane · 8889/HSA · 7206 · 1099-G · 1099-MISC 8z ·
8839 · 8824 · **MIXED-PILOT #2 (item K BOY/EOY, migration) · MIXED-PILOT #7
(basis/at-risk worksheet) · MIXED-PILOT #6 remainder (idempotent field-save
lanes + newer-edit-wins + per-field unsaved indicator + bootstrap retry)**.
**Other queued:** TB default-template Rent/Taxes computed-line fix ·
depreciation-importer prior-split hardening · per-activity QBI carryforward ·
1099-R printed-aggregate fallback · DividendIncome US-obligation field ·
GA payment line from dated payments · packet preflight · TB-import nav
confirm dialog · ⚠ 47 Slate screens / 88 bare checkbox sites + tri-state
selects still lack SlateCheck (DEFERRAL_AUDIT).
**RS agenda:** 8995 rental rows · R-EIC-WSB-SE · 4562 same-year-disposal ·
4797 shareholder-side §179 · GA §179 real-property carve-out ·
R-GA500-DEPR conformity correction · GA600S R001 gross-pair correction ·
§179 active-trade-or-business income enumeration (s198) ·
`D_GA500_017` stale condition (line 13 IS pulled since s199) ·
no RS rule polices the depreciation CONVENTION ·
R-AGG-SUMMARY threshold edit (Option A, s199e) ·
impossible-bonus diagnostic (s205 #4) · RS 7203 Part III map (draft spec) ·
**NEW (s208): ratify the 1065 K-1 residual mechanism (largest-remainder)
— the 1120-S R-K1-ROUND uses last-owner-absorbs; same RECON invariant,
different tie-break** · **NEW (s208): author the Rev. Proc. 2019-11 W-2
wage worksheet spec (MIXED-PILOT #4's full ask) before building**.
