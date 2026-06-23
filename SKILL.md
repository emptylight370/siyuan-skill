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

Construct and run the target command with `-w <workspace-path>` appended (except `workspace` and `serve` subcommands).
Use `-f json` for programmatic output when parsing results.

If the `siyuan` command is not found, try `SiYuan-Kernel` as the alternative command name.
If a command fails, check: (1) workspace path is valid, (2) required flags like `--id` are provided, (3) the subcommand exists in the installed CLI version.

## Global Flags

These flags are available on every subcommand:

| Flag | Description |
|------|-------------|
| `-w, --workspace <path>` | Workspace path (required for all commands except `workspace` and `serve`; note: `serve` only accepts `--workspace` full form, not `-w`, and it is optional as it defaults to the default workspace) |
| `-f, --format <table\|json>` | Output format (default: `table`) |
| `--dry-run` | Validate and print what would happen without making changes |

> **Note:** All flags except `-w` (or `--workspace`) are optional. The CLI will use default values if optional flags are not specified.

## Subcommand Reference

Load `references/commands.md` for full `--help` output of every subcommand.

### Workspace
- `siyuan workspace list` — List registered workspaces
- `siyuan workspace info` — Show current workspace info

### Notebook
- `siyuan notebook list` — List all notebooks
- `siyuan notebook create --name <name>` — Create a notebook (ID auto-generated)
- `siyuan notebook open --id <id>` — Open a notebook
- `siyuan notebook close --id <id>` — Close a notebook
- `siyuan notebook rename --id <id> --name <name>` — Rename a notebook
- `siyuan notebook remove --id <id>` — Remove a notebook

### Document
- `siyuan document list --notebook <id> [--hpath <path>|--path <path>]` — List documents in a notebook
- `siyuan document get --id <id>` — Get document info
- `siyuan document info --id <id>` — Get document info (alias of `get`)
- `siyuan document create --notebook <id> --title <title> [--path <path>] [--markdown <md>]` — Create a document
- `siyuan document rename --id <id> --title <title>` — Rename a document
- `siyuan document move --id <id> --notebook <id> [--hpath <path>|--path <path>]` — Move a document to another notebook
- `siyuan document duplicate --id <id>` — Duplicate a document
- `siyuan document remove --id <id>` — Remove a document
- `siyuan document search <keyword>` — Search documents by keyword

### Block
- `siyuan block get --id <id>` — Get block info
- `siyuan block children --id <id>` — Get child blocks
- `siyuan block insert --parent <id> [--data <md>|--file <path>] [--previous <id>]` — Insert block
- `siyuan block append --parent <id> [--data <md>|--file <path>]` — Append block
- `siyuan block prepend --parent <id> [--data <md>|--file <path>]` — Prepend block
- `siyuan block update --id <id> [--data <md>|--file <path>]` — Update block
- `siyuan block delete --id <id>` — Delete block
- `siyuan block move --id <id> --parent <id> [--previous <id>]` — Move block
- `siyuan block stat --id <id>` — Get block content statistics
- `siyuan block dom --id <id>` — Get block DOM
- `siyuan block kramdown --id <id> [--mode md|textmark]` — Get block kramdown
- `siyuan block breadcrumb --id <id>` — Get block breadcrumb
- `siyuan block batch-get --ids <ids>` — Batch get block info
- `siyuan block batch-kramdown --ids <ids>` — Batch get block kramdown

### SQL
- `siyuan sql "<statement>"` — Execute SQL query (`-l` sets limit)

### Search
- `siyuan search <query>` — Full-text search

### Export
- `siyuan export md --id <id> [--output <path>]` — Export as Markdown (default: stdout)
- `siyuan export html --id <id> [--output <path>]` — Export as HTML (default: stdout)
- `siyuan export docx --id <id> --output <file>` — Export as Word (.docx), --output required
- `siyuan export sy --id <id> [--output <dir>]` — Export as `.sy.zip` (default: print temp path)
- `siyuan export md-zip --id <id> [--output <file>]` — Export as Markdown zip (default: print temp path)
- `siyuan export preview --id <id> [--output <path>]` — Export as preview HTML (default: stdout)
- `siyuan export data [--output <file>]` — Export full workspace data backup

### Import
- `siyuan import md --file <path> --notebook <id> [--hpath <path>|--path <path>]` — Import Markdown file or directory
- `siyuan import sy --file <path> --notebook <id> [--hpath <path>|--path <path>]` — Import `.sy.zip` archive
- `siyuan import data --file <path>` — Import data backup

### Daily Note
- `siyuan dailynote create --notebook <id>` — Create today's daily note
- `siyuan dailynote append --notebook <id> [--data <md>|--file <path>]` — Append block to today's daily note
- `siyuan dailynote prepend --notebook <id> [--data <md>|--file <path>]` — Prepend block to today's daily note

### Asset
- `siyuan asset unused` — List unused assets
- `siyuan asset clean [--path <path>]` — Clean unused assets (optional: single unused asset path)
- `siyuan asset stat --path <path>` — Show asset file info (path relative to data dir)
- `siyuan asset upload --id <id> --file <path>` — Upload files to assets (--file repeatable)

### Attribute
- `siyuan attr get --id <id>` — Get block attributes
- `siyuan attr set --id <id> --attr name=value` — Set block attributes (--attr repeatable)
- `siyuan attr batch-get --ids <ids>` — Batch get block attributes

### Bookmark
- `siyuan bookmark list` — List bookmarks
- `siyuan bookmark labels` — List bookmark labels
- `siyuan bookmark rename --old <old> --new <new>` — Rename a bookmark
- `siyuan bookmark remove --label <label>` — Remove a bookmark

### Tag
- `siyuan tag list [--keyword <keyword>]` — List tags (empty keyword = all)
- `siyuan tag rename --old <old-label> --new <new-label>` — Rename a tag
- `siyuan tag remove --label <label>` — Remove a tag

### History
- `siyuan history list [--notebook <id>] [--op <op>] [-p <page>] [-t <type>]` — List all history
- `siyuan history get --path <path>` — Get historical file content
- `siyuan history search <query> [--notebook <id>] [--op <op>] [-p <page>] [-t <type>]` — Search history
- `siyuan history rollback --path <path>` — Rollback a document
- `siyuan history clear` — Clear all history

### Ref
- `siyuan ref backlinks --id <id>` — Get backlinks
- `siyuan ref mentions --id <id>` — Get mentions
- `siyuan ref refresh --id <id>` — Refresh backlinks

### Repo
- `siyuan repo list [-p <page>] [--tag]` — List snapshots (--tag: tagged only)
- `siyuan repo create [--memo <memo>]` — Create a snapshot
- `siyuan repo diff --left <id> --right <id>` — Diff two snapshots
- `siyuan repo checkout --id <id>` — Checkout (rollback to) a snapshot
- `siyuan repo tag --id <id> --name <name>` — Tag a snapshot
- `siyuan repo untag --name <name>` — Remove a tag
- `siyuan repo purge` — Purge old snapshots
- `siyuan repo search <keyword> [-p <page>]` — Search files in snapshots
- `siyuan repo file export --id <fileID>` — Export file from snapshot to temp file
- `siyuan repo file get --id <fileID> [--output <path>]` — Get file content from snapshot (default: stdout)
- `siyuan repo file open --id <fileID>` — Preview file from snapshot
- `siyuan repo file rollback --id <fileID>` — Rollback a file from snapshot

### Sync
- `siyuan sync pull` — Download from cloud
- `siyuan sync push` — Upload to cloud
- `siyuan sync status` — Show sync status

### Database
- `siyuan database search <keyword>` — Search databases by name
- `siyuan database get --av <avID>` — Get database content
- `siyuan database keys --av <avID>` — List database keys
- `siyuan database render --av <avID> [-p <page>] [-s <size>]` — Render database data
- `siyuan database unused` — List unused databases
- `siyuan database clean [--av <avID>]` — Clean unused databases
- `siyuan database key add --av <avID> --name <name> --type <type>` — Add a key (field)
- `siyuan database key remove --av <avID> --key <keyID> [--remove-relation-dest]` — Remove a key (--remove-relation-dest: also remove related data in linked databases)
- `siyuan database item add --av <avID>` — Add a row
- `siyuan database item update --av <avID> --key <keyID> --item <itemID> --value <json>` — Update a cell
- `siyuan database item remove --av <avID> --ids <ids>` — Remove rows

### File
- `siyuan file list <path>` — List directory contents
- `siyuan file read <path>` — Read file content
- `siyuan file write <path> [--file <src>]` — Write file content (stdin or --file)
- `siyuan file copy <src> <dst>` — Copy file or directory
- `siyuan file rename <old> <new>` — Rename/move file
- `siyuan file delete <path>` — Delete file or directory
- `siyuan file find <path> [--include <glob>] [--limit <int>]` — Find files under a path
- `siyuan file grep --pattern <regex> --path <path> [--context <int>]` — Search file contents with regex
- `siyuan file stat <path>` — Show file or directory info

### Outline
- `siyuan outline get --id <id>` — Get document heading tree

### Template
- `siyuan template search [keyword]` — Search/list templates
- `siyuan template get --path <path>` — Read template content
- `siyuan template create --name <name> [--data <md>|--file <path>]` — Create a template from markdown
- `siyuan template save-as --id <id> --name <name>` — Save document as template
- `siyuan template render --path <path> --id <id>` — Render template against a block (preview)
- `siyuan template remove --path <path>` — Remove a template

### System
- `siyuan system current-time` — Show current server time

### Serve
- `siyuan serve` — Start kernel HTTP server

### CLI
- `siyuan completion bash` — Generate bash completion
- `siyuan completion fish` — Generate fish completion
- `siyuan completion powershell` — Generate powershell completion
- `siyuan completion zsh` — Generate zsh completion
