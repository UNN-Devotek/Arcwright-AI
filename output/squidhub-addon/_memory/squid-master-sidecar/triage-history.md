# Squid-Master — Triage History

> Rolling log of S/M/L triage decisions. Max 20 entries — oldest evicted during [SV save-session]
> when limit is exceeded. Used for context and pattern recognition across sessions.

```yaml
entries:
  - session_id: "2026-03-20-sc-session-system-f1sc"
    date: "2026-03-20"
    description: "Star Citizen session system — session tracking, crew shares, work orders, OCR screenshot autofill, VOIP hooks. Ported from RegolithCo-Common (GPL-3.0, shutting down June 1 2026). Worktree: D:/SQUID/Squidhub-sc-sessions"
    track: LARGE
    corrected_to: null
    q1_scope: "20+ files — 9 models, migration, equations service, 12 API routes, frontend types/client (pre-built) + session dashboard, work order UI, crew share UI, OCR UI, hooks (remaining)"
    q2_risk: "New 9-table schema, @regolithco/common npm dep, MinIO OCR uploads, mission system FK, VOIP hooks"
    q3_reversibility: "Multi-commit, schema changes — not single-commit reversible"
    branch: "feat/sc-session-system"
    linked_feedback_ids: []
    actual_track: null
    shipped_clean: null

  - session_id: "2026-03-18-game-milestone-engine-squad-integration-g4m3"
    date: "2026-03-18"
    description: "Game Milestone Reward Engine (game-agnostic) + Squad Integration (SquadJS DB, The Unnamed API, BattleMetrics) — admin/user dashboards, ribbon auto-awards, seeding tracking"
    track: LARGE
    corrected_to: null
    q1_scope: "20-30+ files — new external DB connection, 2 external APIs, scheduler jobs, milestone engine core, admin+user dashboards, ribbon auto-grant, schema migrations"
    q2_risk: "New external DB, new API deps, high-frequency polling (5-8s), schema changes, Infisical secrets"
    q3_reversibility: "Multi-commit, new subsystems — not single-commit reversible"
    branch: "feat/game-milestone-engine-squad-integration"
    linked_feedback_ids: []
    actual_track: null
    shipped_clean: null

  - session_id: "2026-03-11-shikaku-daily-mode-007a"
    date: "2026-03-11"
    description: "Add daily Shikaku mode, DRY extract daily game infrastructure from Wordle into shared helpers"
    track: LARGE
    corrected_to: null
    q1_scope: "9+ files — shikaku.py, shikaku_routes.py, models.py, schema_migrations.py (core), daily_helpers.py, wordle_routes.py refactor, base.py, frontend Shikaku component, frontend daily leaderboard"
    q2_risk: "New ShikakuDailyAttempt table, schema_migrations.py (core file) touched"
    q3_reversibility: "Schema change — not single-commit reversible"
    branch: "feat/shikaku-daily-mode"
    linked_feedback_ids: []
    actual_track: null
    shipped_clean: null

  - session_id: "2026-03-17-applications-prd-review-c4f7"
    date: "2026-03-17"
    description: "Full [B] Review Track — applications branch, PRD + plans + full codebase implementation (159 files, 21K+ lines, epics 1-13)"
    track: REVIEW
    corrected_to: null
    q1_scope: "159 files changed — backend models/routes/migrations, frontend components/types/hooks, Discord integration, 50+ story artifacts"
    q2_risk: "N/A — review track, not feature track"
    q3_reversibility: "N/A — review track"
    branch: "applications"
    linked_feedback_ids: []
    actual_track: null
    shipped_clean: null

  - session_id: "2026-03-10-wordle-session-persistence-b4e7"
    date: "2026-03-10"
    description: "Fix Wordle session timeout, add timeout indicator and persistent daily attempt storage"
    track: LARGE
    corrected_to: null
    q1_scope: "3–5 files — Wordle backend session logic, frontend indicator, daily persistence"
    q2_risk: "Schema change required to persist guess data server-side"
    q3_reversibility: "No down migration planned — not cleanly single-commit reversible"
    branch: "fix/fb-0176-wordle-session-persistence"
    linked_feedback_ids: ["FB-0176"]
    actual_track: null
    shipped_clean: null
```
