# TTS Tax App — STATUS (current state only)

*Last updated: 2026-07-28, session 127 (**SLATE PHASE 1 COMPLETE — Legs 0-7
all shipped on branch `slate-ui`; STOPPED at the Phase 1 gate for Ken**).*

## How this file works (read before editing)
- **Current state only**: resume pointer, active gate, in-flight work. **Overwritten each session.**
- Durable history → `STATUS_ARCHIVE.md`; deferrals → `DEFERRAL_AUDIT.md`; open questions →
  `REVIEW_QUEUE.md`; per-form → `form_coverage_tracker.md`; learnings → `MEMORY.md` / `.claude` auto-memory.
- **Boot planners live in `tts-tax-status`**: `BUILD_ORDER.md` / `SEASON_PLAN.md` / `PRODUCT_MAP.md`.
- **PII rule**: this file mirrors PUBLIC — no client names/SSNs/EFINs.

## ▶ RESUME HERE — ⛔ PHASE 1 GATE: awaiting Ken's review. Do NOT start Phase 2.

**Slate Phase 1 is BUILT and swept.** Branch **`slate-ui`** (pushed, flag
prod-OFF, never merged/deployed): Leg 0 `1c0fc0c` · Leg 1 `c5b213c` · Leg 2
`ff0e571` · Leg 3 `973e0ad` · Leg 4 `b3162c4` · Leg 5 `0c05d28` · Leg 6
`bb5929b` · **Leg 7 (sweep + side-by-sides) `513b92b`**. Four screenshots
delivered to Ken (legacy W-2 vs Slate W-2 archetype on the SAME demo return,
JumpBox, live 1040-p.1 form pane), captured via **headless system Chrome** —
recipe preserved as `scripts/slate_screenshots.mjs` + `scripts/
mint_magic_link.py` (works with the harness pane hidden; trusted keys).

**Acceptance sweep (cc-implementation-prompt.md checklist) — ALL 8 PASS:**
1. Tabular numerals right-aligned ✔ (computed-style probe: `text-align:
   right`, `lining-nums tabular-nums` on currency cells).
2. Keyboard traversal + visible focus ring ✔ (TRUSTED-event Tab trail
   box1→2→3→4 in headless Chrome; focus ring = the spec's
   `rgba(47,102,168,.32) 0 0 0 3px`; Ctrl+J/F3/F6/Enter/Ctrl+Enter cover
   every interaction — no mouse-only paths).
3. WCAG AA ✔ (vendored `slate-tokens.css` payload verified BYTE-IDENTICAL
   to delvio-design `tokens.css` @ v2.0, which passed the 22-check
   contrast gate; yellow-ink discipline in force).
4. Five field states distinct, real-state driven ✔ (entered/suggested ghost/
   computed ƒx/overridden all visible in one shot; estimated built,
   unwired pending a real source — Q6, documented).
5. Diagnostics deep-link focuses the correct field ✔ (Leg 4 live proof +
   claim-or-default tests; F3 cycling).
6. Flag OFF → legacy identical ✔ (flag-off suite green all legs; live:
   no `.slate-root`, zero slate modules execute, Ctrl+J NOT intercepted).
7. No console errors, no new network calls ✔ (headless console clean of
   app errors; the only ≥400s attributed — `/me/` 403 pre-auth on the
   login page + `prior-year/` 404 firing IDENTICALLY in legacy and Slate,
   2 each; expected_box3/5 rides the existing W-2 payload).
8. Screenshot diff posted ✔ (4 PNGs delivered 2026-07-28).

**Gates at close:** vitest **617/617** · tsc **46 = baseline** · server
untouched since Leg 3 (whose 14 new server tests shipped green).

**Next action: NONE until Ken rules on the archetype.** On approval →
Phase 2 in plan §9 order (Return Manager → Diagnostics full view →
entry-paradigm convergence, one screen at a time, each screenshot-reviewed
via `scripts/slate_screenshots.mjs`); rejected/amended → revise per Ken.
The 1040/1065 form queue (BUILD_ORDER Spine) resumes/interleaves on Ken's
direction. Dev QA recipe: preview_start django-demo + vite → `#/` →
Open return → localStorage `delvio-new-ui`=1 → reload.

**Build rules in force:** presentation-only · selective `git add` only —
NEVER `git add .` on this branch (parallel tb_import work unstaged) · no
merge/deploy without Ken · seed_rules BOTH DBs at deploy (D_W2_ family).

## 🔴 Open judgment calls for Ken (REVIEW_QUEUE)
1. **NEW — the Phase 1 gate itself: approve the Slate archetype?** (4
   screenshots delivered; also compare `delvio-design/w2-entry.png`.)
2. RS D_8990_DISALLOW vs D_8990_EBIE conflicting guidance on a 1065 (s126e).
3. Retire MATH_BALANCE_SHEET's 1065 arm? (s126d).
4. RS R-M2-3-TIE adjudication (s126b).
5. K-1 box 13/11 type codes (s125).
6. RS 1065_B stale D_B2_B1 note + 5 unbuilt Sch B diagnostics (s126).
7. s124's `D_4562_RECON` scoping pair.
8. Real One Heart EIN in committed test fixtures (chip `task_f06ee3ed`).
9. *(carried)* Ken's browser pass over 409 Family Holdings (s126g deploy).
10. *(minor)* Legacy "autofilled" yellow → Slate treatment ruling.
11. *(pre-existing)* D_8995_003 + D_8959_001/3/4 NoneType crashes on
    skeleton returns — defensive guard pass wanted (visible in the
    delivered screenshots' dock, incidentally).
12. *(cosmetic, Phase 2)* The legacy floating "Calculating…"/save chip
    overlaps the Slate SummaryBar corner — needs a Slate home.

## Active gates
- **⛔ Phase 1 gate: STOP. Ken reviews the archetype before any Phase 2 work.**
- **Branch discipline:** `slate-ui` checked out; parallel session's
  uncommitted work UNSTAGED (`server/apps/returns/views.py`, `tb_import.py`,
  `test_tb_import.py`). Never stage/stash/`git add .`.
- ⚠ Demo QA return: has a preparer of record ("QA Test Preparer", synthetic
  PTIN) since Leg 5 QA — print gate clear, D_PREPARER_001 silent on it.
- ⚠ FA-1040-4835-06 drift (chip `task_0cf10eac`).
- ⚠ One test DB — never overlap pytest runs.
- ⚠ `server/.venv` repaired s124; use `.venv\Scripts\python.exe` directly.

## ⚡ MISSION (Ken, 2026-07-09): 1040 · 1120-S · 1120 · 1065 · 1041 · 709 by END OF 2026
Unchanged — Slate ran in front of it by Ken's 2026-07-28 directive; with
Phase 1 at the gate, the form queue is available again if Ken wants it next.

## Authoritative files read at boot
- **`tts-tax-status`:** `BUILD_ORDER.md` · `SEASON_PLAN.md` · `PRODUCT_MAP.md`.
- **tax-app root:** `SPRINT_SCOPE.md` · `MASTER_PROMPT.md` · `MEMORY.md`/`DECISIONS.md`/`SUITE_CONTRACT.md` ·
  `REVIEW_QUEUE.md` · `form_coverage_tracker.md` · `USABILITY_QUEUE.md` ·
  `STATUS_ARCHIVE.md` (history) · RS `session_log.md`.
- **Slate lane:** `Design/SLATE_IMPLEMENTATION_PLAN.md` ·
  `D:\dev\delvio-design\SLATE_DESIGN_SYSTEM.md`.
