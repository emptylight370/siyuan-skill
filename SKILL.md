---
name: siyuan-skill
description: >
  Operate SiYuan Note CLI (siyuan) to manage workspaces, notebooks, documents, blocks, SQL queries,
  search, export/import, assets, attributes, bookmarks, tags, history, refs, repo snapshots, sync, serve,
  database attribute views, templates, outlines, and system info. Use this skill when the user mentions
  SiYuan, 思源笔记, or asks to query/manipulate SiYuan data via CLI. The skill enforces the correct
  workflow: discover workspaces first, then target a specific workspace for all subsequent commands.
agent_created: true
---

# SiYuan Note CLI Skill

## Overview

SiYuan Note provides a CLI (`siyuan`) to manage workspaces, notebooks, documents, blocks, and more.
The binary must be available on `PATH` (verify with `siyuan --version`).

> **CLI Version:** v3.7.0-rc.1. Commands shown below reflect this version.

> **Note:** If `siyuan` is not found on `PATH`, try `SiYuan-Kernel` as the alternative command name.

## Workflow

Follow this sequence for every user request:

### 1. Discover Available Workspaces

Run `siyuan workspace list` (no `-w` flag needed) to obtain the list of registered workspaces.
If the user has not specified a workspace, present the list and ask them to choose.

```
siyuan workspace list
siyuan workspace info          # show current default workspace
```

### 2. Lock in the Workspace Path

After the user selects a workspace (or provides a path directly), store the workspace path and use it
as the `-w` argument for **all subsequent commands** (except `workspace` subcommands).

### 3. Execute the Requested Command

Construct and run the target command with `-w <workspace-path>` appended (except `workspace` subcommand).
Use `-f json` for programmatic output when parsing results.

If a command fails, check: (1) workspace path is valid, (2) required flags like `--id` are provided, (3) the subcommand exists in the installed CLI version.

## Common Patterns

### Obtaining Block/Document IDs

Many commands require a block or document ID (`--id`). Obtain IDs via:
- `siyuan sql "SELECT id, content FROM blocks WHERE type='d'" -w <path> -f json` — list document IDs
- `siyuan document list --notebook <id> -w <path> -f json` — list documents in a notebook
- `siyuan search <keyword> -w <path> -f json` — search and get IDs from results

### Dry-Run for Destructive Operations

Before running destructive commands (`remove`, `delete`, `clean`, `clear`, `purge`), prepend `--dry-run` to preview the effect without making changes:
```
siyuan notebook remove --id <id> --dry-run -w <path>
```

### JSON Output Parsing

Use `-f json` for programmatic parsing. The output is a JSON array of objects; field names match the SiYuan kernel API response schema. Pipe to `jq` or similar tools for filtering.

### Workspace Lock Errors

If a command exits with code 24 and reports `lock workspace [...] failed`, the workspace is already in use by another SiYuan process (e.g., the desktop app). Either close the other process or target a different workspace.

## Global Flags

These flags are available on every subcommand:

| Flag | Description |
|------|-------------|
| `-w, --workspace <path>` | Workspace path (required for all commands except `workspace`; optional for `serve`, which defaults to the default workspace). |
| `-f, --format <table\|json>` | Output format (default: `table`) |
| `--dry-run` | Validate and print what would happen without making changes |

> **Note:** All flags except `-w` (or `--workspace`) are optional. The CLI will use default values if optional flags are not specified.

## Subcommand Reference

> **For detailed flags and examples of each command, load `references/commands.md`.**

| Command | Subcommands |
|---------|-------------|
| `siyuan workspace` | `list`, `info` |
| `siyuan notebook` | `list`, `create`, `open`, `close`, `rename`, `remove`, `set-icon`, `random-icon` |
| `siyuan document` | `list`, `get`/`info`, `create`, `rename`, `move`, `duplicate`, `remove`, `search` |
| `siyuan block` | `get`, `children`, `insert`, `append`, `prepend`, `update`, `delete`, `move`, `stat`, `dom`, `kramdown`, `breadcrumb`, `batch-get`, `batch-kramdown` |
| `siyuan sql "<stmt>"` | (direct) |
| `siyuan search <query>` | (direct, with flags) |
| `siyuan export` | `md`, `html`, `docx`, `sy`, `md-zip`, `preview`, `data` |
| `siyuan import` | `md`, `sy`, `data` |
| `siyuan dailynote` | `create`, `append`, `prepend` |
| `siyuan asset` | `unused`, `clean`, `stat`, `upload` |
| `siyuan attr` | `get`, `set`, `batch-get` |
| `siyuan bookmark` | `list`, `labels`, `rename`, `remove` |
| `siyuan tag` | `list`, `rename`, `remove` |
| `siyuan history` | `list`, `get`, `search`, `rollback`, `clear` |
| `siyuan ref` | `backlinks`, `mentions`, `refresh` |
| `siyuan repo` | `list`, `create`, `diff`, `checkout`, `tag`, `untag`, `purge`, `search`, `file` |
| `siyuan sync` | `pull`, `push`, `status` |
| `siyuan database` | `search`, `get`, `keys`, `render`, `unused`, `clean`, `key`, `item` |
| `siyuan file` | `list`, `read`, `write`, `copy`, `rename`, `delete`, `find`, `grep`, `stat` |
| `siyuan outline` | `get` |
| `siyuan template` | `search`, `get`, `create`, `save-as`, `render`, `remove` |
| `siyuan system` | `current-time` |
| `siyuan serve` | (direct, with flags) |
| `siyuan completion` | `bash`, `fish`, `powershell`, `zsh` |

> **Note:** `serve` accepts all global flags plus its own flags (`--accessAuthCode`, `--attach-ui`, `--lang`, `--mode`, `--port`, `--readonly`, `--ssl`, `--safe-mode`, `--wd`). It does not require `-w`; see `references/commands.md` for the full flag list.
