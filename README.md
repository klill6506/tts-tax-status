# tts-tax-status

Read-only status mirror for the Sherpa tax-prep build (tts-tax-app).
Five planning files are copied here by `scripts/sync_status_mirror.ps1`
at the close of every dev session — **do not edit here**; the source of
truth is the tts-tax-app repo.

| File | What it tells you |
|---|---|
| `SEASON_CHECKLIST.md` | At-a-glance tick-list of the runway to the Jan 2027 season |
| `STATUS.md` | Current state only — resume pointer, active gate, in-flight work |
| `SEASON_PLAN.md` | The authoritative dated plan (months, gates, risks) |
| `SPRINT_SCOPE.md` | The 1040 work queue and quality rules |
| `form_coverage_tracker.md` | Per-form build history, newest first |
