# PRODUCT_MAP.md — What lives where (Core vs. modules)

*Adopted 2026-07-04. Purpose: (1) resolve CC scope questions without a trip to Ken;
(2) define the season-one cut vs. later modules; (3) double as future commercial packaging.
Underlying assumption: **TTS Tax Core covers ~99% of The Tax Shelter's actual book** —
verify against the TaxWise forms-usage report (Ken-lane, still open) and recalibrate.*

***CANONICAL HOME: the `tts-tax-status` repo** — the shared brain both CC contexts (tax-app
AND rule-studio) pull at boot, alongside BUILD_ORDER (order) and SEASON_PLAN (gates).*

## Resolution protocol for CC (read this first)
1. In **Core + season one** → build it (spec-first, four legs, as always).
2. In a **module** or **season two** → do NOT build. Wire the boundary diagnostic (below),
   log to `DEFERRAL_AUDIT.md`, move on.
3. **Not on this map** → `REVIEW_QUEUE.md` for Ken. Never guess scope.

## The governing rule: module boundaries are DIAGNOSTIC boundaries
Core never goes silent at a boundary. When a return crosses into module territory, Core
must DETECT it and throw a loud RED diagnostic naming what the return needs and that this
package doesn't do it. Detection is always Core work THIS season, even when the handling
is a module NEXT season. (Pattern: the 1041 Schedule I AMT RED-defer, ruled 2026-07-04.)

---

## TTS TAX CORE (season one — the product)
Everything the returns in this practice's book require, end to end.

**Compliance:** 1040 / 1120-S / 1065 / 1041 federal, complete with the practice's form set;
GA, SC, AL, NC as licensed states (see States module for the rest).
**Loss limitations (all Core, in statutory order — basis → at-risk → passive → §461(l)):**
- Basis: Form 7203 both sides (entity + 1040), basis provenance gate, K-1 import.
- PAL: Form 8582 per-activity with carryforwards; $25k active-participation allowance.
- At-risk (6198) and §461(l) (Form 461): per Ken's open rulings below.
**Depreciation:** FULL return-side depreciation — 4562, MACRS incl. SL switchover, bonus/§179
with GA nonconformity, AMT conventions, 4797 dispositions with §1245/§1250 character rules,
per-asset schedules. ("Normal depreciation" means everything the RETURN needs; what it
excludes is asset MANAGEMENT — see Asset Pro.)
**Estimates (compliance side):** next-year ES vouchers + safe-harbor calculations (110%/90%,
GA equivalents). These are Core because preparers need them on every balance-due return.
**K-2/K-3 domestic filing exception:** the DFE determination + documentation is CORE and
season one — the software must establish and record WHY K-2/K-3 aren't required. The forms
themselves are Complex Entity Pro.
**Workflow/office:** proforma/rollover, document import (W-2/1099-R/SSA/INT/DIV/1099-B
summary), diagnostics + review workflow, multi-preparer concurrency, printing at volume,
invoices/letters, e-file (MeF A2A), bank-product integration.

### Core boundary diagnostics to wire THIS season (small builds, big protection)
- [ ] M-3 required (asset/receipts thresholds per form instructions — pin in RS spec)
- [ ] K-2/K-3 DFE FAILS → RED: "international reporting required; not supported season one"
- [ ] §704(c) property contributed / special allocations beyond profit-loss-capital %
- [ ] §754 election in effect or §743(b)/§734(b) adjustment indicated
- [ ] Multistate apportionment indicated for an entity (beyond licensed-state scope)
- [ ] 1041 Schedule I AMT indicators (ruled 2026-07-04)
- [ ] §461(l) threshold crossed (pending Ken ruling below)
- [ ] Nonrecourse financing present without at-risk analysis (pending Ken ruling below)

### Open Ken rulings (deeper-PAL/basis items that hide inside the 99%)
| # | Item | Recommended default |
|---|------|---------------------|
| R1 | Self-rental recharacterization (Reg. §1.469-2(f)(6)) | BUILD in Core — common fact pattern (S-corp owner + building LLC); income nonpassive, loss passive |
| R2 | PTP loss segregation in 8582 | BUILD in Core — brokerage-K-1 clients hit it routinely; PTP losses offset only same-PTP income |
| R3 | Real estate professional (§469(c)(7)) | Diagnostic + preparer determination checkbox; no full support |
| R4 | Form 6198 at-risk | Diagnostic when nonrecourse debt present, season one; full form later if usage report demands |
| R5 | Form 461 / §461(l) | RED diagnostic at threshold, season one |

---

## MODULES (season two+ unless noted)

**ASSET PRO — fixed-asset management** (the return-side calc stays in Core)
Bulk transfers/splits/disposals, book-vs-tax multi-basis, rollforward reports, asset import
from client systems, mid-year conventions tooling, property-tax tie-in (companion:
sherpa-property-tax).

**COMPLEX ENTITY PRO**
§704(c) methods (traditional/curative/remedial), §754 + §743(b)/§734(b), §752 debt tiers,
target/waterfall allocations, K-2/K-3 forms, Schedule M-3, tiered passthroughs, advanced
multistate entity apportionment.

**PLANNING PRO**
Multi-year projections, scenario modeling, what-if comparisons, advisory report packages.
(ES vouchers/safe harbor are NOT here — they're Core compliance.)

**STATES**
Per-state or all-state licensing beyond the Core four. Season one: GA/SC/AL/NC only;
~50 returns across 23 states ride the PPR parachute (SEASON_PLAN decision 1).

**INFORMATION RETURNS** — sherpa-1099 (exists; expansion deferred per SEASON_PLAN)
1099 series / IRIS e-filing.

**CLIENT PORTAL** — sherpa-portal (exists)
Client login, document upload (feeds Core's import pipeline), messages, delivery;
e-signature lands here when built (season two).

**FRONT DESK** — sherpa-check-in (exists)
Lobby check-in, desk intake, mail.

**SCHEDULER** — sherpa-scheduler (planned, not started)
Client appointment booking, Calendly-style. Distinct from Front Desk (booking vs. lobby
check-in). Season-two+; not season-one work.

**RESEARCH / AI TAX EXPERT** (future)
The RS-database-grounded query app. Depends on wide source loading in Rule Studio —
explicitly NOT season-one work.

---

## Maintenance
- Lives in repo root + tts-tax-status mirror; CC boot list.
- SEASON_PLAN.md's deferred list defers to THIS file as the single source of truth.
- Scope rulings that change this map are Ken-only; CC records them here with date.
