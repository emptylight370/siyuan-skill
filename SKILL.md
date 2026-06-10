---
name: siyuan_skill
description: >
  Operate SiYuan Note CLI (siyuan) to manage workspaces, notebooks, documents, blocks, SQL queries,
  search, export/import, assets, attributes, bookmarks, tags, history, refs, repo snapshots, sync, serve,
  and database attribute views. Use this skill when the user mentions SiYuan, 思源笔记, or asks to
  query/manipulate SiYuan data via CLI. The skill enforces the correct workflow: discover workspaces first,
  then target a specific workspace for all subsequent commands.
agent_created: true
---

# SiYuan Note CLI Skill

## Overview

SiYuan Note provides a CLI (`siyuan`) to manage workspaces, notebooks, documents, blocks, and more.
The binary must be available on `PATH` (verify with `siyuan --version`).

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

Append `-w <workspace-path>` to every command (except `workspace` subcommands).
Use `-f json` for programmatic output when parsing results.

```
siyuan <subcommand> [args] -w /path/to/workspace -f json
```

## Global Flags

| Flag | Description |
|------|-------------|
| `-w, --workspace <path>` | Workspace path (required for all commands except `workspace`) |
| `-f, --format <table\|json>` | Output format (default: `table`) |
| `-h, --help` | Show help for any command |

## Subcommand Quick Reference

Load `references/commands.md` for full `--help` output of every subcommand.

| Group | Subcommand | Purpose |
|-------|------------|---------|
| **Workspace** | `workspace list` | List registered workspaces |
| | `workspace info` | Show current workspace info |
| **Notebook** | `notebook list` | List all notebooks |
| | `notebook create` | Create a notebook |
| | `notebook rename` | Rename a notebook |
| | `notebook remove` | Remove a notebook |
| **Document** | `document list` | List documents in a notebook |
| | `document get` | Get document info |
| | `document create` | Create a document |
| | `document rename` | Rename a document |
| | `document move` | Move a document |
| | `document duplicate` | Duplicate a document |
| | `document remove` | Remove a document |
| **Block** | `block get` | Get block info (`--id`) |
| | `block children` | Get child blocks |
| | `block insert` | Insert block |
| | `block append` | Append block |
| | `block prepend` | Prepend block |
| | `block update` | Update block |
| | `block delete` | Delete block |
| | `block move` | Move block |
| | `block stat` | Get block content statistics |
| | `block dom` | Get block DOM |
| | `block kramdown` | Get block kramdown |
| | `block breadcrumb` | Get block breadcrumb |
| | `block info` | Get document info |
| **SQL** | `sql "<statement>"` | Execute SQL query (`-l` sets limit) |
| **Search** | `search <query>` | Full-text search |
| **Export** | `export md --id <id>` | Export as Markdown |
| | `export html` | Export as HTML |
| | `export docx` | Export as Word |
| | `export sy` | Export as `.sy.zip` |
| | `export md-zip` | Export as Markdown zip |
| | `export preview` | Export as preview HTML |
| | `export data` | Export full workspace backup |
| **Import** | `import md --file <path> --notebook <id>` | Import Markdown |
| | `import sy` | Import `.sy.zip` |
| | `import data` | Import data backup |
| **Daily Note** | `dailynote create --notebook <id>` | Create today's daily note |
| | `dailynote append --notebook <id> [--data <md>|--file <path>]` | Append block to today's daily note |
| | `dailynote prepend --notebook <id> [--data <md>|--file <path>]` | Prepend block to today's daily note |
| **Asset** | `asset unused` | List unused assets |
| | `asset clean` | Clean unused assets |
| | `asset upload` | Upload files to assets |
| **Attribute** | `attr get --id <id>` | Get block attributes |
| | `attr set` | Set block attributes |
| | `attr batch-get` | Batch get attributes |
| **Bookmark** | `bookmark list` | List bookmarks |
| | `bookmark labels` | List bookmark labels |
| | `bookmark rename` | Rename a bookmark |
| | `bookmark remove` | Remove a bookmark |
| **Tag** | `tag list` | List tags |
| | `tag rename` | Rename a tag |
| | `tag remove` | Remove a tag |
| **History** | `history list` | List all history |
| | `history get` | Get historical file content |
| | `history search` | Search history |
| | `history rollback` | Rollback a document |
| | `history clear` | Clear all history |
| **Ref** | `ref backlinks --id <id>` | Get backlinks |
| | `ref mentions --id <id>` | Get mentions |
| | `ref refresh --id <id>` | Refresh backlinks |
| **Repo** | `repo list` | List snapshots |
| | `repo create` | Create a snapshot |
| | `repo diff` | Diff two snapshots |
| | `repo checkout` | Checkout a snapshot |
| | `repo tag` / `untag` | Tag/untag snapshot |
| | `repo purge` | Purge old snapshots |
| | `repo search` | Search files in snapshots |
| | `repo file` | File-level snapshot ops |
| **Sync** | `sync pull` | Download from cloud |
| | `sync push` | Upload to cloud |
| | `sync status` | Show sync status |
| **Database** | `database search` | Search databases by name |
| | `database get` | Get database content |
| | `database keys` | List database keys |
| | `database key add` | Add a key (field) |
| | `database key remove` | Remove a key |
| | `database item add` | Add a row |
| | `database item update` | Update a cell |
| | `database item remove` | Remove rows |
| | `database render` | Render database data |
| | `database unused` | List unused databases |
| | `database clean` | Clean unused databases |
| **File** | `file list` | List directory contents |
| | `file read` | Read file content |
| | `file write` | Write file content |
| | `file copy` | Copy file or directory |
| | `file rename` | Rename/move file |
| | `file delete` | Delete file or directory |
| **Serve** | `serve` | Start kernel HTTP server |

## Key Usage Patterns

### SQL Queries

```bash
# Query blocks
siyuan sql "SELECT * FROM blocks WHERE type='document' LIMIT 10" -w /path -f json

# Count blocks by type
siyuan sql "SELECT type, COUNT(*) as cnt FROM blocks GROUP BY type" -w /path -f json
```

### Search

```bash
siyuan search "keyword" -w /path -f json
# Flags: -m (method), -t (type filter), -n (notebook filter), -o (order), -p (page), -s (page-size)
```

**Note:** `-m 4` (fuzzy/vector search) requires an embedding model to be configured and embeddings to be generated first. If fuzzy search returns no results or an error, remind the user to configure the embedding model in SiYuan settings and run embedding.

### Export a Document

```bash
siyuan export md --id <block-id> --output /tmp/doc.md -w /path
```

### Import Markdown

```bash
siyuan import md --file ./notes/ --notebook <notebook-id> -w /path
```

## Notes

- Always run `siyuan workspace list` first if no workspace is known.
- Never omit `-w` for non-workspace subcommands; the CLI will target the default workspace otherwise.
- Use `-f json` when the output needs to be parsed programmatically.
- Block IDs are required for most `block`, `attr`, `ref` operations (`--id` flag).
- Load `references/commands.md` whenever a subcommand's detailed flags are needed.
