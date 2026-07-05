# CHANGE_REGISTER.md — Tax-law change detection & triage

*Adopted 2026-07-05. The intake funnel for tax-law changes (federal + GA/SC/AL/NC + entity).
Closes the one hole the other planners didn't cover: DETECTION and TRIAGE. Implementation
(RS authoring) was already solved; this feeds it.*

***CANONICAL HOME: the `tts-tax-status` repo** — the shared brain both CC contexts read at
boot, alongside BUILD_ORDER (order), SEASON_PLAN (gates), PRODUCT_MAP (scope).*

## The three-stage model (why this file exists)
1. **Detect** — know a change happened (the feeds below).
2. **Triage** — decide if it touches OUR perimeter, how much, by when (this file's table).
3. **Implement** — RS authoring. Already handled by WORK_ORDERS + BUILD_ORDER.
This file owns stages 1–2 and hands stage 3 off. It does NOT maintain its own build order —
triaged "build it" items become BUILD_ORDER Spine items (or Shelf if externally blocked) and,
where authoring is needed, WORK_ORDERS orders. Same anti-drift rule as WORK_ORDERS.

## The monitoring perimeter (shrinks the problem ~90%)
We monitor ONLY the forms/entities/states we support (see PRODUCT_MAP + the TaxWise
forms-usage report once pulled). A change to a form we don't file is logged "out of perimeter"
and closed. Federal 1040/1120-S/1065/1041 + GA/SC/AL/NC. Not 990/706/709, not other states.

## Detection feeds (all free; subscribe = Ken-lane, this month)
**Federal (IRS):**
- **QuickAlerts** — e-file/schema/business-rule/publication changes. *Already subscribed (MeF).*
- **Guidewire** — advance copies of notices, revenue rulings, revenue procedures, regulations.
- **e-News for Tax Professionals** — weekly (Fridays); the plain-language practitioner digest.
- **IRS Newswire** — releases as they drop (legislation implementation, deadlines).
**States:**
- **GA DOR "Subscribe to Revenue Emails"** (Income & Withholding) — notices, regs, policy
  bulletins. Plus **GA Society of CPAs** for interpretation.
- **SC / AL / NC DOR** email lists + each state CPA society — confirm/subscribe during the
  DOR approval calls (same call).
**Practitioner (interpretation layer):** AICPA Tax Section / The Tax Adviser; NATP.

## Cadence
- **Weekly (Fri, ~15 min):** scan e-News + QuickAlerts; log anything in-perimeter here.
- **Calendared drops (dated gates, not surprises):**
  - Fall — annual inflation **Revenue Procedure** (next year's brackets/limits). *TY2026 = Rev.
    Proc. 2025-32, already out — feeds the Nov TY2026 constants wave. TY2027 = fall 2026.*
  - Summer/fall — IRS draft forms; late fall — final forms.
  - December — standard mileage notice; retirement-plan limit notices.
- **Episodic (can't-miss):** new acts (OBBBA-class), technical corrections, extenders, major
  court decisions. Thin once filtered to perimeter; the big ones are unmissable.

## Triage protocol (three outcomes — mirrors PRODUCT_MAP)
For each detected item, one verdict:
1. **In Core + season one** → new BUILD_ORDER Spine item (+ WORK_ORDERS order if authoring
   needed). If externally blocked → BUILD_ORDER Shelf.
2. **Module / season two** → DEFERRAL_AUDIT entry; note here and close.
3. **Out of perimeter** → log one line, close.

## Two gates, non-negotiable (per the RS-first design)
- A detected change may START an authoring draft. It NEVER crosses a gate unattended.
  **Gate 1** = draft → published spec (Ken approves). **Gate 2** = published → compute
  (existing gated ingest). No auto-publication, no auto-recompute off a detected change.
- **AI/automation may DETECT and TRIAGE; it must NEVER implement.** A misread notice
  auto-written into a spec is the exact silent-error the prime directive forbids. Machine
  surfaces the candidate; Ken rules; RS implements. No exceptions.

## Authority-tagging (the moat — start now, query later)
Each RS spec already cites a primary source. Add two fields going forward: the **authority**
it depends on (statute/reg/form line) + **last-verified-against-source date**. This makes the
corpus a dependency graph: when a caselaw/statute watch fires, query "which specs cite
§1402(a)(13)?" and exactly those light up for re-verification. Don't build the query engine
now — just start tagging so the data exists. Feeds the future Research/AI module.

---

## REGISTER (newest first)
`date · source · what changed · perimeter? · triage verdict · link`

- **2026-07-05 · seed · Rev. Proc. 2025-32 (TY2026 inflation adjustments, OBBBA-amended)** ·
  IN perimeter (all federal modules) · → BUILD_ORDER Shelf item "TY2026 re-cut" already
  covers this; CC verifies EVERY figure against the primary PDF (irs.gov/pub/irs-drop/rp-25-32.pdf),
  not secondary summaries. Watch for post-10/9/2025 corrections.
- **2026-07-05 · seed · OBBBA implementation guidance (ongoing)** · IN perimeter · standing
  watch — technical corrections, IRS notices/regs interpreting OBBBA provisions (SALT cap
  $40,400 phase-out, senior deduction, tips/OT, car-loan interest, §461(l), estate $15M).
  New guidance → triage individually.
- **2026-07-05 · seed · SE limited-partner caselaw (Soroban/Denham, appeals pending)** · IN
  perimeter (1065 SE) · standing watch — already flagged for CASELAW_SE_LP re-verification;
  authority-tag the 1065_SE spec to §1402(a)(13). Re-verify on appeal rulings.
- **2026-07-02 · GA HB 463 · tips/OT exclusions §48-7-27(a)(16)/(17)** · IN perimeter (GA-500) ·
  ✅ DONE (`ga500-hb463-tips-ot-complete`). Shown here as the pattern: state legislation →
  detected → authored spec-first → shipped.

---

## Maintenance
- Canonical in tts-tax-status; both CC boots read it. Triaged build-items flow to BUILD_ORDER
  (order) / WORK_ORDERS (authoring) — this file never holds its own build order.
- Ken-lane July: subscribe to the feeds above (QuickAlerts already done); confirm SC/AL/NC DOR
  + society lists during the approval calls.
- Automated monitor (an RS-orbit agent reading the feeds, filtering to perimeter, drafting a
  triage digest for Ken) is POST-SEASON. The manual weekly scan starts now.
