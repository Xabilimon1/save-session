# save-session

Claude Code skill — saves compressed session summaries to the x-brain Obsidian vault.

## Install

```bash
/plugin marketplace add Xabilimon1/save-session
/plugin install save-session@Xabilimon1
```

## Usage

Say `save session`, `guarda la sesión`, or `fin de sesión` at the end of a session.

## What it does

1. Generates compressed summary from current session
2. Writes to `Claude/Sessions/YYYY-MM-DD.md` in x-brain vault
3. Updates `Claude/Context.md` if major context changed

## Requirements

- Obsidian running with x-brain vault open
- `obsidian` CLI available (`obsidian help`)
