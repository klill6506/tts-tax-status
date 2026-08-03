# TTS Tax App — STATUS (current state only)

*Last updated: 2026-08-03, session 195 (**a full day of Ken TESTING the app
end-to-end on Barcode Supply — nine defects found and shipped**, plus the
GA-600S built with PTET tying Lacerte exactly, and 27 businesses that could
not be invoiced converted to standalone clients). Migration 0234 latest.*

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

## ⚠⚠ STANDING FACT: THIS IS TESTING, NOT FILING
Ken, s195, plainly: **no 2025 returns are being prepared in the app.**
Entries exist to find defects. Do not append compliance cautions to every
number — state the finding and move on.

---

## ▶ RESUME HERE — build the three K-1 → individual gaps

Ken's next unit, agreed at the end of s195. Barcode Supply is the test
vehicle; its 1120-S and GA-600S are both built and tying.

**GAP 1 — the shareholder-side §179 disposition (BLOCKED, needs a spec).**
The ENTITY side is complete and spec-backed (RS 4797 R-4797-ENTPASS): the
disposal is excluded from the entity 4797, box 17 code K + statement
generate per shareholder pro rata, `ENT179_GAIN` feeds M-2 3a and M-1 5b,
`D_4797_ENTPASS` fires. **Nothing on the 1040 side consumes it** — no
17K field, no transfer, no gate. ⚠ **Rule Studio has NO shareholder-side
rule: 0 of the 9 4797 rules mention a shareholder, partner or §179
carryover.** The shareholder's gain depends on their UNUSED §179
carryover, so the same K-1 gives different answers per owner — improvising
it produces a confident wrong number. **Ken must author the RS rule (or
rule) before this is built.** Recipient model is `ScheduleK1`, which
already RED-defers §1231/4797 by design.

**GAP 2 — the Georgia Shareholder Summary (Form 600S).** Buildable now.
Georgia has no "GA K-1"; it has this per-shareholder summary, and the app
produces nothing like it (zero matches in the codebase). Ken's Lacerte
copy is the model: line 1 federal K-1 (lines 1–10), 2b other increases
(federal depreciation), 3b other decreases (Georgia depreciation), 4 GA
passthrough credits (blank under PTET — it is an EXCLUSION, not a credit),
5a/5b GA apportioned income. All the data already exists on the return.

**GAP 3 — the GA individual modifications carryover.** Buildable now.
GA-500 Schedule 1 addition/subtraction lines are preparer-keyed; nothing
carries the entity's federal-vs-GA depreciation pair (+37,931 / −41,509
for Barcode) onto the shareholder's GA-500.

---

## Shipped in s195 (all deployed)

| | |
|---|---|
| `ce807d6` | **`+ New Client` could only ever make a 1040** — entity type hardcoded server-side; the type chips are LIST FILTERS, not creation context. Picker added, seeded from the active tab. |
| `4d3e403` | Depreciation schedule rebuilt **Lacerte-shaped** (asset numbers, category grouping, closing block; ties Barcode's 30 assets to the Lacerte PDF). **The Lacerte importer wrote group labels nothing could read** — 41 assets; migration 0234. **A failed save locked the whole register** — Retry/Discard added. |
| `afad2ed` | The register's **"Cost/basis" column printed the REMAINING basis**, so fully-bonused assets read $0. Now gross; "Prior 179/SDA/depr" carries the write-off. Description opens the worksheet. |
| `74c1bad` | **Schedule L printed `((540,547))`** — the IRS template preprints the parentheses; all four contra pairs, every form on that path. |
| `9efe82e` | **Prior §179 box** — `sec_179_prior` was on the model and read by three computations but exposed on no screen. |
| `8e7b6b8` | **M-1 line 1 was not the books** (back-computed from K18). M1_5b carries `ENT179_GAIN`. Two K-1 statement defects: a year printed as "2,024", and a meaningless Total footed on a fact sheet. |
| `02b1791` | **GA-600S PTET rate was the TY2024 rate** (0.0539) while GA-700 used 0.0519. One `GA_PTET_RATE` constant now. Barcode's GA-600S created with PTET. |
| `936537b` | GA-600S **clean preparer/client copy** in Georgia's software-version style. GA depreciation keyed — **ties Lacerte exactly** (232,915 / 12,088). |
| `c72dfd4` | **26 businesses converted to standalone clients** (#4754–#4779) — they could not be found or invoiced. |
| `a67e24e` | **The Lomax hold closed** — kept the WORKED return, repaired the link. |

---

## ⚠ Open items for Ken

1. **RS rule for the shareholder-side §179 disposition** — blocks GAP 1.
2. **GA PTET base on a separately stated gain** — Ken reads HB 149 as
   taxing it; Lacerte uses federal ordinary income and adds the gain back
   at the shareholder level. §48-7-21(b)(7)(C)(ii) says "net income as
   computed pursuant to Code Section 48-7-21" (an ENTITY concept), which
   supports Ken; shareholder relief is an EXCLUSION limited to "income on
   which tax was actually paid". **No DOR guidance found on separately
   stated items.** The stake is the SALT deduction, not the GA total.
3. **Client #2969** survives holding an empty individual entity that is a
   duplicate of the person at #2970 (Ken confirmed there is only one).
   Which client number survives is his call — the s193 merge recipe.
4. **Retire `reparent_business_entities`** — it encodes the client model
   Ken rejected; re-running it would undo `c72dfd4`.
5. **The app computes no Georgia depreciation** — the GA/federal
   difference is keyed by hand (Barcode: 3,578 as a Schedule 8 other
   subtraction, GA 4562 attached). Diagnostic says so out loud.
6. M1_5b is now COMPUTED — a return with other book-income-not-on-K items
   needs an override carrying the total.

## Carried queue (unchanged from s194 unless noted)
**Lane-schema-only (engine-complete)**: 8880 · 8962 annual · 2441 · 8863 ·
5695 · 8606 · 4797 · 6252 · 7203 · 1116. **True builds**: Sch F lane ·
8889/HSA · 7206 · 1099-G · 1099-MISC 8z · 8839 · 8824.
**Other queued:** TB default-template Rent/Taxes computed-line fix ·
depreciation-importer prior-split hardening · per-activity QBI carryforward ·
1099-R printed-aggregate fallback · RentalProperty `owner` + GA RIE rental
pull · DividendIncome US-obligation field · GA payment line from dated
payments · packet preflight · TB-import nav confirm dialog ·
**client-delete UI (there is NO path to delete a client — the pages that
can are unrouted)** · **duplicate guard is blind to entity names**.
**RS agenda:** 8995 rental rows · R-EIC-WSB-SE · 4562 same-year-disposal ·
**4797 shareholder-side §179 (GAP 1)**.
