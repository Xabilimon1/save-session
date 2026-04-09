---
name: save-session
description: Use when finishing a Claude Code session or when user says "save session", "guarda la sesión", "fin de sesión", or invokes /save-session. Saves compressed session summary to x-brain Obsidian vault.
---

# Save Session

Save compressed session summary to x-brain vault (`~/x-brain`). Requires Obsidian open.

## Steps (run in order)

**1. Collect facts from current conversation:**
- What was done (actions taken, files changed, commands run)
- Key decisions and why (only non-obvious)
- Pending actionable items

**2. Check if today's session file exists:**
```bash
obsidian vault="x-brain" read path="Claude/Sessions/$(date +%Y-%m-%d).md" 2>/dev/null && echo EXISTS || echo NEW
```

**3a. If NEW — create file:**
```bash
obsidian vault="x-brain" create name="Claude/Sessions/$(date +%Y-%m-%d)" content="---\ndate: $(date +%Y-%m-%d)\ntags: [session]\n---\n\n# Sesión $(date +%Y-%m-%d)\n\n## $(date +%H:%M) — <TOPIC>\n<SUMMARY>" silent
```

**3b. If EXISTS — append block:**
```bash
obsidian vault="x-brain" append path="Claude/Sessions/$(date +%Y-%m-%d).md" content="\n## $(date +%H:%M) — <TOPIC>\n<SUMMARY>"
```

**4. Update Context.md** — ONLY if: new project added, critical decision, new tool/plugin, major preference change.
```bash
obsidian vault="x-brain" append file="Context" content="\n- [[Claude/Sessions/$(date +%Y-%m-%d)]] — <one line summary>"
```

## Summary format

```
## HH:MM — <project or topic>
- Done: item1, item2, item3
- Decision: <what> — <why> (skip obvious ones)
- Pending: [ ] actionable task
```

## Compression rules

| Include | Skip |
|---|---|
| Decisions + rationale | Setup/install steps |
| New projects/tools added | Trivial edits |
| Blockers found | "Explored X" without outcome |
| Pending actions | Intermediate debugging steps |
| Config/preference changes | Things that got reverted |

- Max 80 words per block
- One line per item, no prose
- Squash related items on one line with commas
- If nothing meaningful happened: write `- Done: /` and skip context update
