---
title: Claude Code Skip Permission Confirmation
tags: [claude-code, configuration]
---

# Claude Code Skip Permission Confirmation

## Problem

During Claude Code usage, frequent permission confirmation prompts (e.g., Bash command execution, file editing) require manually selecting "Yes", which interrupts the development workflow.

## Solution

### Temporary (Current Session Only)

```bash
# Option 1: Dedicated flag, same effect as Option 2
claude --dangerously-skip-permissions

# Option 2: General parameter, bypassPermissions is one of the available values
claude --permission-mode bypassPermissions
```

Both options have the same effect — they skip all permission prompts. The only difference is the entry point: one is a shortcut flag, the other is a specific value of a general parameter.

### Persistent (Recommended)

Edit `~/.claude/settings.json` to apply globally across all projects on your machine:

```json
{
  "permissions": {
    "defaultMode": "bypassPermissions"
  }
}
```

### Fine-Grained Pre-Authorization (Compromise)

If you don't want to skip all checks, you can pre-authorize only commonly used tools:

```json
{
  "permissions": {
    "defaultMode": "acceptEdits",
    "allow": [
      "Bash(*)",
      "Read",
      "Write",
      "Edit"
    ]
  }
}
```

`acceptEdits` automatically accepts file edits, and `Bash(*)` covers Bash command confirmations.

## permission-mode Available Values

| Value | Behavior |
|---|---|
| `default` | Default, confirms each action individually |
| `acceptEdits` | Automatically accepts file edits |
| `bypassPermissions` | Skips all permission checks |
| `plan` | Plan only, no execution |
| `dontAsk` | Don't ask, skips when no permission |

## Warning

`bypassPermissions` skips **all** security checks. Only use it when you trust the current environment.
