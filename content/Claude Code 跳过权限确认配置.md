---
title: Claude Code 跳过权限确认配置
tags: [claude-code, configuration]
---

# Claude Code 跳过权限确认配置

## 问题

Claude Code 运行过程中频繁弹出权限确认（如 Bash 命令执行、文件编辑等），需要手动选择 "Yes"，打断开发节奏。

## 解决方案

### 临时生效（当次会话）

```bash
# 方式一：专用 flag，效果等同方式二
claude --dangerously-skip-permissions

# 方式二：通用参数，bypassPermissions 是可选值之一
claude --permission-mode bypassPermissions
```

两者效果相同，都跳过所有权限提示。区别仅是入口不同：一个是快捷 flag，一个是通用参数的某个取值。

### 永久生效（推荐）

编辑 `~/.claude/settings.json`，对本机所有项目全局生效：

```json
{
  "permissions": {
    "defaultMode": "bypassPermissions"
  }
}
```

### 精细化预授权（折中方案）

不想完全跳过所有检查时，可只预授权常用工具：

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

`acceptEdits` 自动接受文件编辑，`Bash(*)` 覆盖 Bash 命令确认。

## permission-mode 可选值

| 值 | 行为 |
|---|---|
| `default` | 默认，逐项确认 |
| `acceptEdits` | 自动接受文件编辑 |
| `bypassPermissions` | 跳过所有权限检查 |
| `plan` | 仅规划不执行 |
| `dontAsk` | 不询问，无权限时跳过 |

## 注意

`bypassPermissions` 会跳过**所有**安全检查，仅在信任当前环境时使用。
