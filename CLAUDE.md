# CLAUDE.md — Arcwright Workshop

AI assistant instructions for the Arcwright agent workflow package.

## What This Repo Is

This is the **Arcwright workshop** — the editing workspace and build pipeline for the `@arcwright-ai/agent-orchestration` npm package. It produces distributable AI agent workflows, skills, and orchestration tools for multiple IDEs.

- **npm org:** `@arcwright-ai`
- **GitHub:** https://github.com/UNN-Devotek/Arcwright-AI
- **Published packages:** `@arcwright-ai/agent-orchestration`, `@arcwright-ai/tmux-setup`

## Publishing

This is the **only** place packages are published from. The Squidhub repo keeps its Squidhub-specific content unpublished — it's just local overlay files, not a separate package.

**First-time setup:**
1. Register `@arcwright-ai` npm org on npmjs.com
2. Add npm auth token: `npm config set //registry.npmjs.org/:_authToken <token>`

**To publish:**
```bash
node build/build.js                                    # build agent-orchestration package
cd packages/agent-orchestration && npm publish --access public
cd packages/tmux-setup && npm publish --access public
```

**What gets published:**
- `@arcwright-ai/agent-orchestration` — single package containing:
  - `_arcwright/{awm,awb,core,_memory}/` — agent/workflow modules
  - `.agents/skills/` — 54+ skills (Claude Code format)
  - `.kiro/skills/` — same skills (Kiro IDE + CLI format)
  - `.kiro/steering/` — Kiro steering docs
  - `.claude/commands/arcwright-track-*.md` — slash commands
  - `tmux/` — tmux setup scripts
- `@arcwright-ai/tmux-setup` — standalone tmux setup package:
  - Catppuccin theme, pane title sync, clipboard integration
  - Agent orchestration scripts, status bar widgets
  - CLI binary: `arcwright-tmux`
- CLI binaries: `arcwright`, `arcwright-install`, `arcwright-tmux`
- Installer supports: Claude Code, Kiro (IDE + CLI), Cursor, Windsurf, Cline, GitHub Copilot, Gemini
- Install modes: project-level (default) or global (`--global`)

## Repo Structure

```
arcwright/
├── _arcwright/              <- Agent/workflow source (editable)
│   ├── awm/                 <- Arcwright Method module (analyst, architect, dev, pm, qa, etc.)
│   ├── awb/                 <- Arcwright Builder module (agent/workflow/module creation)
│   ├── core/                <- Core orchestrator agent + config
│   └── _memory/             <- Sidecar memory + skills
├── .agents/skills/          <- Skill library (54+ skills, SKILL.md format)
├── .claude/commands/        <- Slash commands (arcwright-track-*.md, arcwright-migrate.md)
├── packages/
│   ├── agent-orchestration/ <- npm package (@arcwright-ai/agent-orchestration)
│   │   ├── bin/arcwright.js  <- npx entry point
│   │   ├── lib/              <- Installer, CLI, platform detection (EDIT THESE)
│   │   ├── src/              <- Built output (NEVER edit — wiped on build)
│   │   └── package.json
│   └── tmux-setup/          <- npm package (@arcwright-ai/tmux-setup)
│       ├── bin/arcwright-tmux.js <- npx entry point
│       ├── lib/              <- Installer, CLI (EDIT THESE)
│       ├── src/tmux/         <- tmux config + scripts (EDIT THESE)
│       └── package.json
├── build/
│   └── build.js             <- Workshop packager
```

## Build Pipeline

```bash
node build/build.js
```

Pipeline steps:
1. `buildGenericPackage()` — copies `_arcwright/` modules + `.agents/skills/` → `src/`
2. `buildKiroAssets()` — copies skills → `src/.kiro/skills/`, generates steering docs
3. `bundleTrackCommands()` — copies `arcwright-track-*.md` → `src/.claude/commands/`
4. `bundleTmuxSetup()` — copies tmux scripts → `src/tmux/`
5. `verifyNoLeaks()` — scans for Squidhub content patterns, fails if found

## Key Rules

- **Never edit `src/` directories** — wiped on every build
- **`lib/` and `bin/` are permanent** — edit these for installer behavior changes
- **No Squidhub content** — this repo is the generic package. The leak checker enforces this
- **Skills are dual-format** — `.agents/skills/` SKILL.md files work for both Claude Code and Kiro (same frontmatter: `name`, `description`, `allowed-tools`)

## Installer Features

The installer (`lib/installer.js`) supports:

**7 IDE platforms:** Claude Code, Kiro, Cursor, Windsurf, Cline, GitHub Copilot, Gemini

**Install scopes:**
- `npx @arcwright-ai/agent-orchestration` — project-level install (interactive)
- `npx @arcwright-ai/agent-orchestration --global` — global install to `~/` config dirs
- `npx @arcwright-ai/agent-orchestration --tools claude-code,kiro` — multi-IDE install

**Cross-platform:** Windows (WSL2), Linux, macOS. WSL dual-home aware — terminal tools (Claude Code, Gemini) use Linux home, GUI IDEs (Kiro, Cursor, etc.) use Windows home.

**All CLI flags:**

| Flag | Default | What it does |
|------|---------|--------------|
| `--yes`, `-y` | false | Skip interactive prompts, accept all defaults |
| `--global`, `-g` | false | Install to `~/.arcwright` / `~/.claude/` / `~/.kiro/` |
| `--directory <path>`, `-d` | cwd | Target project root |
| `--tools <ids>` | `claude-code` | Comma-sep: `claude-code,kiro,cursor,windsurf,cline,github-copilot,gemini` |
| `--modules <ids>` | `awm,awb,core,_memory` | Which modules to install |
| `--user-name <name>` | prompted | Your name (written into config.yaml) |
| `--output-folder <path>` | `_arcwright-output` | Where Arcwright writes output artifacts |
| `--no-teams` | teams included | Skip the 17 team-* skills and `/team` command |
| `--docker-check` | not installed | Opt in to `/docker-check` and the `docker-type-check` skill |
| `--gitignore <mode>` | prompted | `full` / `skills` / `output-only` / `none` — controls what gets added to `.gitignore` |

**Gitignore modes** (project installs only — global installs skip):
- `output-only` — ignore `_arcwright-output/` only (default / recommended)
- `skills` — also ignore `.agents/skills/` and `.kiro/skills/`
- `full` — ignore all Arcwright dirs, commands, and agent stubs
- `none` — do not modify `.gitignore`

The installer writes a managed `# ─── Arcwright installation ───` block to `.gitignore`. On update, that block is replaced in-place. It never commits to git.

## Module Reference

| Module | Dir | Contents |
|--------|-----|----------|
| AWM (Method) | `_arcwright/awm/` | Specialist agents: analyst, architect, dev, pm, qa, sm, tech-writer, ux-designer |
| AWB (Builder) | `_arcwright/awb/` | Meta-agents for creating new agents, workflows, and modules |
| Core | `_arcwright/core/` | Master orchestrator agent, templates, core workflows |
| Memory | `_arcwright/_memory/` | Sidecar files, skill definitions |

## Relationship to Squidhub

This workshop is the **single publish source** for `@arcwright-ai/agent-orchestration`.

The Squidhub project keeps its own Squidhub-specific overlay files (squid-master persona, MCP templates, etc.) locally. These are **not published** — they sit on top of the installed Arcwright package for Squidhub's own use.

<!-- arcwright-agent-start -->

## Arcwright

Arcwright agents, skills, and workflows are installed in `_arcwright/`.
Key modules: awm, awb, core, _memory

To use an agent, load its `.md` file and follow its activation instructions.
Agent configs are in `_arcwright/{module}/config.yaml`.
Skills are in `.agents/skills/` and `_arcwright/_memory/skills/`.

<!-- arcwright-agent-end -->

<!-- arcwright-tmux-start -->

## Agent Spawning (tmux-aware)

If `$TMUX` is set, load `.agents/skills/tmux-protocol/SKILL.md` before any multi-pane work.

<!-- arcwright-tmux-end -->
