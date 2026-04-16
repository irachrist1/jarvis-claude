# Jarvis for Claude Code

> Stop re-explaining yourself to Claude. Give it a memory.

Jarvis is a Claude Code skill that discovers your environment, connects to your real tools, and stays in sync across every session. Second run onwards, it shows you what changed since last time.

## What it does

**First run** — scans your machine, asks what it can access, builds a memory:
- Git identity and active repos
- Task managers (TickTick, Todoist, Apple Reminders, Things 3, Taskwarrior, Linear, Microsoft To Do)
- Calendars (Apple Calendar, Google Calendar via gcalcli, Outlook)
- Activity trackers (Daylens, RescueTime, Toggl)
- Browser history — domain-level summary only, no URLs stored
- Tech stack (languages, package managers, frameworks)

**Second run onwards** — shows what changed:
```
Jarvis Update — 14:30 EAT
Last check: 6 hours ago

Repos:
- daylens-windows: 3 new commits on main
- new repo found: side-project at ~/code/side-project

Tasks:
- 2 completed in TickTick/Work
- 1 new reminder in Apple Reminders

Calendar: 2 events today — next: "1:1" at 15:00

Activity since last check: 4.2h VS Code, 1.1h Chrome
```

**Works with zero specific tools** — just git config + any codebase = functional Jarvis. Everything else is a power-up.

## Install

```bash
mkdir -p ~/.claude/skills/jarvis
curl -o ~/.claude/skills/jarvis/SKILL.md \
  https://raw.githubusercontent.com/irachrist1/jarvis-claude/main/SKILL.md
```

Or clone:
```bash
git clone https://github.com/irachrist1/jarvis-claude ~/.claude/skills/jarvis
```

## Use

In any Claude Code session:
```
/jarvis
```

That's it. Jarvis discovers your environment, asks for consent on sensitive data, and builds your memory. Every subsequent `/jarvis` call shows a diff since last time.

## Privacy

Jarvis has three tiers:

| Tier | What | Consent |
|---|---|---|
| 1 | Git config, OS identity, CLI tool existence, repo locations | Automatic |
| 2 | Task titles, calendar names, activity tool metadata | Ask once, remembered |
| 3 | Calendar event content, browser history, activity data | Ask every run |

Browser history is summarized to domain-level counts — no URLs or page titles are ever written to disk.

All data stays on your machine. Nothing is sent anywhere.

## Commands

After setup, say these in any Claude session:

```
Jarvis, refresh tasks
Jarvis, refresh calendar
Jarvis, refresh everything
Jarvis, revoke [tool]       — removes consent + deletes that tool's data
Jarvis, reset               — wipes state and memory, starts fresh
Jarvis, status              — shows tool inventory and last run time
Jarvis, what do you know about me?
```

## Supported tools

| Category | macOS | Windows |
|---|---|---|
| Activity | Daylens, RescueTime, Toggl | Daylens, RescueTime, Toggl |
| Tasks | TickTick, Things 3, Reminders, Todoist, Taskwarrior, Linear | TickTick, Microsoft To Do, Todoist, Taskwarrior, Linear |
| Calendar | Apple Calendar, gcalcli, Outlook | Outlook, gcalcli |
| Browser | Chrome, Firefox, Safari\*, Edge | Chrome, Firefox, Edge |
| Dev | git, gh, node, python, rust, go, brew | git, gh, node, python, rust, go, choco, winget |

\*Safari requires Full Disk Access for Terminal

## Requirements

- Claude Code (any version)
- `sqlite3` CLI for database-backed tools (Daylens, browser history)
  - macOS: `brew install sqlite3`
  - Windows: `winget install SQLite.SQLite`
- Individual tools you want Jarvis to connect to (none required)

## How it works

Jarvis is a Claude Code skill — a markdown file that Claude Code executes when you type `/jarvis`. It uses Claude's built-in Bash and file tools to discover your environment, read data with your consent, and write structured memory files to `~/.claude/skills/jarvis/memory/`. A line is added to `~/.claude/CLAUDE.md` so Claude loads your context automatically in future sessions.

No server. No API keys. No account. Runs entirely on your machine.

---

Built with Claude Code · Designed with Opus 4.6
