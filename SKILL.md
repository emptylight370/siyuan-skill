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

> **CLI Version:** v3.7.0-dev15 (shown as v3.6.5). Commands shown below reflect this version.

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
# For most commands
siyuan <subcommand> [args] -w /path/to/workspace -f json

# For serve command (optional)
siyuan serve [flags] --workspace /path/to/workspace
```

## Global Flags

These flags are available on every subcommand:

| Flag | Description |
|------|-------------|
| `-w, --workspace <path>` | Workspace path (required for all commands except `workspace` and `serve`; note: `serve` only accepts `--workspace` full form, not `-w`, and it is optional as it defaults to the default workspace) |
| `-f, --format <table\|json>` | Output format (default: `table`) |
| `--dry-run` | Validate and print what would happen without making changes |
| `-h, --help` | Show help for any command |

> **Note:** All flags except `-w` (or `--workspace`) are optional. The CLI will use default values if optional flags are not specified.

## Subcommand Quick Reference

Load `references/commands.md` for full `--help` output of every subcommand.

| Group | Subcommand | Purpose |
|-------|------------|---------|
| **Workspace** | `workspace list` | List registered workspaces |
| | `workspace info` | Show current workspace info |
| **Notebook** | `notebook list` | List all notebooks |
| | `notebook create --name <name>` | Create a notebook (ID auto-generated) |
| | `notebook open --id <id>` | Open a notebook |
| | `notebook close --id <id>` | Close a notebook |
| | `notebook rename --id <id> --name <name>` | Rename a notebook |
| | `notebook remove --id <id>` | Remove a notebook |
| **Document** | `document list --notebook <id> [--hpath <path>\|--path <path>]` | List documents in a notebook |
| | `document get --id <id>` | Get document info |
| | `document info --id <id>` | Get document info |
| | `document create --notebook <id> --title <title> [--path <path>] [--markdown <md>]` | Create a document |
| | `document rename --id <id> --title <title>` | Rename a document |
| | `document move --id <id> --notebook <id> [--hpath <path>\|--path <path>]` | Move a document to another notebook |
| | `document duplicate --id <id>` | Duplicate a document |
| | `document remove --id <id>` | Remove a document |
| | `document search <keyword>` | Search documents by keyword |
| **Block** | `block get --id <id>` | Get block info |
| | `block children --id <id>` | Get child blocks |
| | `block insert --parent <id> [--data <md>\|--file <path>] [--previous <id>]` | Insert block |
| | `block append --parent <id> [--data <md>\|--file <path>]` | Append block |
| | `block prepend --parent <id> [--data <md>\|--file <path>]` | Prepend block |
| | `block update --id <id> [--data <md>\|--file <path>]` | Update block |
| | `block delete --id <id>` | Delete block |
| | `block move --id <id> --parent <id> [--previous <id>]` | Move block |
| | `block stat --id <id>` | Get block content statistics |
| | `block dom --id <id>` | Get block DOM |
| | `block kramdown --id <id> [--mode md\|textmark]` | Get block kramdown |
| | `block breadcrumb --id <id>` | Get block breadcrumb |
| | `block batch-get --ids <ids>` | Batch get block info |
| | `block batch-kramdown --ids <ids>` | Batch get block kramdown |
| **SQL** | `sql "<statement>"` | Execute SQL query (`-l` sets limit) |
| **Search** | `search <query>` | Full-text search |
| **Export** | `export md --id <id> [--output <path>]` | Export as Markdown (default: stdout) |
| | `export html --id <id> [--output <path>]` | Export as HTML (default: stdout) |
| | `export docx --id <id> --output <file>` | Export as Word (.docx), --output required |
| | `export sy --id <id> [--output <dir>]` | Export as `.sy.zip` (default: print temp path) |
| | `export md-zip --id <id> [--output <file>]` | Export as Markdown zip (default: print temp path) |
| | `export preview --id <id> [--output <path>]` | Export as preview HTML (default: stdout) |
| | `export data [--output <file>]` | Export full workspace data backup |
| **Import** | `import md --file <path> --notebook <id> [--hpath <path>\|--path <path>]` | Import Markdown file or directory |
| | `import sy --file <path> --notebook <id> [--hpath <path>\|--path <path>]` | Import `.sy.zip` archive |
| | `import data --file <path>` | Import data backup |
| **Daily Note** | `dailynote create --notebook <id>` | Create today's daily note |
| | `dailynote append --notebook <id> [--data <md>\|--file <path>]` | Append block to today's daily note |
| | `dailynote prepend --notebook <id> [--data <md>\|--file <path>]` | Prepend block to today's daily note |
| **Asset** | `asset unused` | List unused assets |
| | `asset clean [--path <path>]` | Clean unused assets (optional: single unused asset path) |
| | `asset stat --path <path>` | Show asset file info (path relative to data dir) |
| | `asset upload --id <id> --file <path>` | Upload files to assets (--file repeatable) |
| **Attribute** | `attr get --id <id>` | Get block attributes |
| | `attr set --id <id> --attr name=value` | Set block attributes (--attr repeatable) |
| | `attr batch-get --ids <ids>` | Batch get block attributes |
| **Bookmark** | `bookmark list` | List bookmarks |
| | `bookmark labels` | List bookmark labels |
| | `bookmark rename --old <old> --new <new>` | Rename a bookmark |
| | `bookmark remove --label <label>` | Remove a bookmark |
| **Tag** | `tag list [--keyword <keyword>]` | List tags (empty keyword = all) |
| | `tag rename --old <old-label> --new <new-label>` | Rename a tag |
| | `tag remove --label <label>` | Remove a tag |
| **History** | `history list [--notebook <id>] [--op <op>] [-p <page>] [-t <type>]` | List all history |
| | `history get --path <path>` | Get historical file content |
| | `history search <query> [--notebook <id>] [--op <op>] [-p <page>] [-t <type>]` | Search history |
| | `history rollback --path <path>` | Rollback a document |
| | `history clear` | Clear all history |
| **Ref** | `ref backlinks --id <id>` | Get backlinks |
| | `ref mentions --id <id>` | Get mentions |
| | `ref refresh --id <id>` | Refresh backlinks |
| **Repo** | `repo list [-p <page>] [--tag]` | List snapshots (--tag: tagged only) |
| | `repo create [--memo <memo>]` | Create a snapshot |
| | `repo diff --left <id> --right <id>` | Diff two snapshots |
| | `repo checkout --id <id>` | Checkout (rollback to) a snapshot |
| | `repo tag --id <id> --name <name>` | Tag a snapshot |
| | `repo untag --name <name>` | Remove a tag |
| | `repo purge` | Purge old snapshots |
| | `repo search <keyword> [-p <page>]` | Search files in snapshots |
| | `repo file export --snapshot <id> --file <path>` | Export file from snapshot |
| | `repo file get --snapshot <id> --file <path>` | Get file content from snapshot |
| | `repo file open --snapshot <id> --file <path>` | Preview file from snapshot |
| | `repo file rollback --snapshot <id> --file <path>` | Rollback a file from snapshot |
| **Sync** | `sync pull` | Download from cloud |
| | `sync push` | Upload to cloud |
| | `sync status` | Show sync status |
| **Database** | `database search <keyword>` | Search databases by name |
| | `database get --av <avID>` | Get database content |
| | `database keys --av <avID>` | List database keys |
| | `database render --av <avID> [-p <page>] [-s <size>]` | Render database data |
| | `database unused` | List unused databases |
| | `database clean [--av <avID>]` | Clean unused databases |
| | `database key add --av <avID> --name <name> --type <type>` | Add a key (field) |
| | `database key remove --av <avID> --key <keyID>` | Remove a key |
| | `database item add --av <avID>` | Add a row |
| | `database item update --av <avID> --key <keyID> --item <itemID> --value <json>` | Update a cell |
| | `database item remove --av <avID> --ids <ids>` | Remove rows |
| **File** | `file list <path>` | List directory contents |
| | `file read <path>` | Read file content |
| | `file write <path> [--file <src>]` | Write file content (stdin or --file) |
| | `file copy <src> <dst>` | Copy file or directory |
| | `file rename <old> <new>` | Rename/move file |
| | `file delete <path>` | Delete file or directory |
| | `file find <path> [--include <glob>] [--limit <int>]` | Find files under a path |
| | `file grep --pattern <regex> --path <path> [--context <int>]` | Search file contents with regex |
| | `file stat <path>` | Show file or directory info |
| **Outline** | `outline get --id <id>` | Get document heading tree |
| **Template** | `template search [keyword]` | Search/list templates |
| | `template get --path <path>` | Read template content |
| | `template create --name <name> [--data <md>\|--file <path>]` | Create a template from markdown |
| | `template save-as --id <id> --name <name>` | Save document as template |
| | `template render --path <path> --id <id>` | Render template against a block (preview) |
| | `template remove --path <path>` | Remove a template |
| **System** | `system current-time` | Show current server time |
| **Serve** | `serve` | Start kernel HTTP server |
| **CLI** | `completion bash` | Generate bash completion |
| | `completion fish` | Generate fish completion |
| | `completion powershell` | Generate powershell completion |
| | `completion zsh` | Generate zsh completion |

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

> **Note:** `-m 4` (fuzzy/vector search) requires an embedding model to be configured and embeddings to be generated first. If fuzzy search returns no results or an error, remind the user to configure the embedding model in SiYuan settings and run embedding.

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
- Use `--dry-run` to validate a command without making changes (useful for destructive operations like `block delete`, `file delete`, `notebook remove`).
- Load `references/commands.md` whenever a subcommand's detailed flags are needed.
