# SiYuan CLI — Full Command Reference

Auto-generated from `siyuan --help` output (v3.7.0-dev14).

---

## Top-Level Flags

These flags are available on every subcommand (global flags):

| Flag | Description |
|------|-------------|
| `-f, --format <table\|json>` | Output format (default: `table`) |
| `-h, --help` | Show help |
| `-v, --version` | Print version |
| `-w, --workspace <path>` | Workspace path (required for all commands except `workspace`) |
| `--dry-run` | Dry run mode: validate and print what would happen without making changes |

> **Note:** All global flags except `-w` (or `--workspace`) are optional. The CLI will use default values if optional flags are not specified.

---

## workspace — Manage Workspaces

```
siyuan workspace [command]
```

### Subcommands
| Command | Description |
|---------|-------------|
| `list` | List registered workspaces |
| `info` | Show current workspace info |

### Example
```bash
siyuan workspace list
siyuan workspace info
```

---

## notebook — Manage Notebooks

```
siyuan notebook [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `list` | | List all notebooks |
| `create` | `--name <name>` | Create a notebook (ID auto-generated) |
| `open` | `--id <id>` | Open a notebook |
| `close` | `--id <id>` | Close a notebook |
| `rename` | `--id <id> --name <name>` | Rename a notebook |
| `remove` | `--id <id>` | Remove a notebook |

---

## document — Manage Documents

```
siyuan document [command] -w <path>
```

### Subcommands
### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `list --notebook <id>` | `--hpath <path>` `--path <path>` | List documents in a notebook |
| `get --id <id>` | `--id <id>` | Get document info |
| `info --id <id>` | `--id <id>` | Get document info |
| `create --notebook <id> --title <title>` | `--path <path>` `--markdown <md>` | Create a document |
| `rename --id <id> --title <title>` | `--id <id>` `--title <title>` | Rename a document |
| `move --id <id> --notebook <id>` | `--hpath <path>` `--path <path>` | Move a document to another notebook |
| `duplicate --id <id>` | `--id <id>` | Duplicate a document |
| `remove --id <id>` | `--id <id>` | Remove a document |
| `search <keyword>` | | Search documents by keyword |

---

## block — Block Operations

```
siyuan block [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `get --id <id>` | `--id <id>` | Get block info |
| `children --id <id>` | `--id <id>` | Get child blocks |
| `insert --parent <id>` | `--parent <id>` `--data <md>` `--file <path>` `--previous <id>` | Insert block |
| `append --parent <id>` | `--parent <id>` `--data <md>` `--file <path>` | Append block |
| `prepend --parent <id>` | `--parent <id>` `--data <md>` `--file <path>` | Prepend block |
| `stat --id <id>` | `--id <id>` | Get block content statistics |
| `update --id <id>` | `--id <id>` `--data <md>` `--file <path>` | Update block |
| `delete --id <id>` | `--id <id>` | Delete block |
| `move --id <id> --parent <id>` | `--id <id>` `--parent <id>` `--previous <id>` | Move block |
| `dom --id <id>` | `--id <id>` | Get block DOM |
| `kramdown --id <id>` | `--id <id>` `--mode md\|textmark` | Get block kramdown |
| `breadcrumb --id <id>` | `--id <id>` | Get block breadcrumb |
| `batch-get --ids <ids>` | `--ids <id1,id2,...>` | Batch get block info |
| `batch-kramdown --ids <ids>` | `--ids <id1,id2,...>` | Batch get block kramdown |

---

## sql — Execute SQL Query

```
siyuan sql "<statement>" [flags] -w <path>
```

### Flags
| Flag | Description |
|------|-------------|
| `-l, --limit <int>` | Max rows returned (default: 100) |

### Example
```bash
siyuan sql "SELECT * FROM blocks WHERE type='document' LIMIT 10" -w /path -f json
```

---

## search — Full-Text Search

```
siyuan search <query> [flags] -w <path>
```

### Flags
| Flag | Description |
|------|-------------|
| `-m, --method <int>` | 0=keyword 1=query-syntax 2=sql 3=regex 4=fuzzy |
| `-t, --type <stringArray>` | Block type filter (repeatable) |
| `--subtype <stringArray>` | Block subtype filter (repeatable) |
| `-n, --notebook <stringArray>` | Notebook ID filter (repeatable) |
| `--path <stringArray>` | Path prefix filter (repeatable) |
| `-o, --order-by <int>` | 0=type 1=created-asc 2=created-desc 3=updated-asc 4=updated-desc 5=content 6=relevance-asc 7=relevance-desc |
| `-p, --page <int>` | Page number (default: 1) |
| `-s, --page-size <int>` | Results per page (default: 32) |
| `-g, --group-by <int>` | 0=none 1=document |

> **Note:** `-m 4` (fuzzy/vector search) requires an embedding model to be configured in SiYuan settings and embeddings to be generated before use. If results are empty or an error occurs, prompt the user to configure the embedding model first.

### Block Types for `-t`
`document heading paragraph list listItem codeBlock mathBlock table blockquote superBlock htmlBlock embedBlock databaseBlock audioBlock videoBlock iframeBlock widgetBlock callout`

---

## export — Export Documents

```
siyuan export [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `md --id <id>` | `--output <path>` (default: stdout) | Export as Markdown |
| `html --id <id>` | `--output <path>` (default: stdout) | Export as HTML |
| `docx --id <id>` | `--output <file>` (required) | Export as Word (.docx) |
| `sy --id <id>` | `--output <dir>` (default: print temp path) | Export as .sy.zip |
| `md-zip --id <id>` | `--output <file>` (default: print temp path) | Export as Markdown zip |
| `preview --id <id>` | `--output <path>` (default: stdout) | Export as preview HTML |
| `data` | `--output <file>` (default: print temp path) | Export full workspace data backup |

---

## import — Import Files

```
siyuan import [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `md --file <path> --notebook <id>` | `--hpath <path>` `--path <path>` (default: /) | Import Markdown file or directory |
| `sy --file <path> --notebook <id>` | `--hpath <path>` `--path <path>` (default: /) | Import .sy.zip archive |
| `data --file <path>` | | Import data backup |

---

## asset — Manage Assets

```
siyuan asset [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `unused` | | List unused assets |
| `clean` | `--path <path>` (optional, single unused asset path to remove) | Clean unused assets |
| `stat --path <path>` | `--path <path>` (asset path relative to data dir, e.g. assets/image/xxx.png) | Show asset file info |
| `upload --id <id> --file <path>` | `--id <id>` `--file <path>` (repeatable) | Upload files to workspace assets |

---

## attr — Manage Block Attributes

```
siyuan attr [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `get --id <id>` | `--id <id>` | Get block attributes |
| `set --id <id> --attr name=value` | `--id <id>` `--attr name=value` (repeatable) | Set block attributes |
| `batch-get --ids <ids>` | `--ids <id1,id2,...>` | Batch get block attributes |

### Common Attributes for `attr set`
- `icon` - Emoji hex codepoint (e.g. "1f4ca"), emoji character (e.g. "📊"), custom image path (e.g. "1/b3log.png"), or dynamic icon URL
- `title-img` - CSS background-image format (e.g. 'background-image:url("assets/example.jpg")')
- `tags` - Comma-separated tag names

### Examples
```bash
# Set icon attribute
siyuan attr set --id 20260605100657-v080a4j --attr icon=1f4ca -w /path

# Set title image
siyuan attr set --id 20260605100657-v080a4j --attr title-img='background-image:url("assets/example.jpg")' -w /path

# Set tags
siyuan attr set --id 20260605100657-v080a4j --attr tags=tag1,tag2 -w /path
```

---

## bookmark — Manage Bookmarks

```
siyuan bookmark [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `list` | | List bookmarks |
| `labels` | | List bookmark labels |
| `rename --old <old> --new <new>` | `--old <old>` `--new <new>` | Rename a bookmark |
| `remove --label <label>` | `--label <label>` | Remove a bookmark |

---

## tag — Manage Tags

```
siyuan tag [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `list` | `--keyword <keyword>` (empty = all) | List tags |
| `rename --old <old-label> --new <new-label>` | `--old <old-label>` `--new <new-label>` | Rename a tag |
| `remove --label <label>` | `--label <label>` | Remove a tag |

---

## history — Data History

```
siyuan history [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `list` | `--notebook <id>` `--op <op>` `-p, --page <int>` `-t, --type <int>` | List all history |
| `get --path <path>` | `--path <path>` | Get historical file content |
| `search <query>` | `--notebook <id>` `--op <op>` `-p, --page <int>` `-t, --type <int>` | Search history |
| `rollback --path <path>` | `--path <path>` | Rollback a document to historical version |
| `clear` | | Clear all history |

### History Type Values (`-t, --type`)
0=doc-name 1=doc-content 2=asset 3=doc-id 4=database

### Operation Filter (`--op`)
delete, update, create

---

## ref — Backlinks and References

```
siyuan ref [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `backlinks --id <id>` | `--keyword <str>` `--sort <int>` | Get backlinks for a block |
| `mentions --id <id>` | `--keyword <str>` `--sort <int>` | Get mentions for a block |
| `refresh --id <id>` | | Refresh backlinks for a block |

### Sort values for backlinks/mentions
0=updated-desc 1=updated-asc 2=created-desc 3=created-asc 4=name-desc 5=name-asc 6=alphanum-desc 7=alphanum-asc

---

## repo — Data Snapshots

```
siyuan repo [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `list` | `-p, --page <int>` `--tag` (tagged only) | List snapshots |
| `create` | `--memo <string>` | Create a snapshot |
| `diff --left <id> --right <id>` | `--left <id>` `--right <id>` | Diff two snapshots |
| `checkout --id <id>` | `--id <id>` | Checkout (rollback to) a snapshot |
| `tag --id <id> --name <name>` | `--id <id>` `--name <name>` | Tag a snapshot |
| `untag --name <name>` | `--name <name>` | Remove a tag |
| `purge` | | Purge old snapshots |
| `search <keyword>` | `-p, --page <int>` | Search files in snapshots |
| `file` | | File-level snapshot operations |

### repo file — Nested Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `export --id <fileID>` | `--id <fileID>` | Export file from snapshot to temp file |
| `get --id <fileID>` | `--id <fileID>` `--output <path>` (default: stdout) | Get file content from snapshot |
| `open --id <fileID>` | `--id <fileID>` | Preview file content from snapshot |
| `rollback --id <fileID>` | `--id <fileID>` | Rollback a single file from snapshot |

---

## sync — Sync with Cloud

```
siyuan sync [flags] -w <path>
siyuan sync [command] -w <path>
```

### Subcommands
| Command | Description |
|---------|-------------|
| `pull` | Download from cloud |
| `push` | Upload to cloud |
| `status` | Show sync status |

---

## database — Manage Databases (Attribute Views)

```
siyuan database [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `search <keyword>` | | Search databases by name |
| `get --av <avID>` | `--av <avID>` (required) | Get database content |
| `keys --av <avID>` | `--av <avID>` (required) | List database keys (fields) |
| `render --av <avID>` | `--av <avID>` `-p, --page <int>` `-s, --size <int>` `--query <string>` `--view <viewID>` | Render database data |
| `unused` | | List unused databases |
| `clean` | `--av <avID>` (optional, default: clean all) | Clean unused databases |

### database key — Nested Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `key add --av <avID> --name <name> --type <type>` | `--av <avID>` `--name <name>` `--type <type>` `--icon <icon>` `--prev <prevKeyID>` | Add a key (field) to database |
| `key remove --av <avID> --key <keyID>` | `--av <avID>` `--key <keyID>` `--remove-relation-dest` | Remove a key (field) from database |

### Key Types (`--type`)
block/text/number/date/select/mSelect/url/email/phone/mAsset/template/created/updated/checkbox/relation/rollup/lineNumber

### database item — Nested Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `item add --av <avID>` | `--av <avID>` `--block <blockID>` `--content <string>` `--detached` `--group <groupID>` `--previous <prevItemID>` `--view <viewID>` `--ignore-default-fill` | Add a row to database |
| `item update --av <avID> --key <keyID> --item <itemID> --value <json>` | `--av <avID>` `--key <keyID>` `--item <itemID>` `--value <json>` | Update a cell value |
| `item remove --av <avID> --ids <id1,id2,...>` | `--av <avID>` `--ids <ids>` | Remove rows from database |

---

## dailynote — Daily Note Operations

```
siyuan dailynote [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `create --notebook <id>` | | Create today's daily note |
| `append --notebook <id> [--data <md>\|--file <path>]` | `--data` `--file` (`-` for stdin) | Append block to today's daily note |
| `prepend --notebook <id> [--data <md>\|--file <path>]` | `--data` `--file` (`-` for stdin) | Prepend block to today's daily note |

---

## file — Workspace File Operations

```
siyuan file [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `list <path>` | | List directory contents |
| `read <path>` | | Read file content |
| `write <path> [--file <src>]` | `--file <src>` (default: stdin) | Write file content (from stdin or file) |
| `copy <src> <dst>` | | Copy file or directory |
| `rename <old> <new>` | | Rename or move file |
| `delete <path>` | | Delete file or directory |
| `find <path>` | `--include <glob>` `--limit <int>` (default: 200) | Find files under a path |
| `grep --pattern <regex> --path <path>` | `--pattern <regex>` `--path <path>` `--context <int>` `--include <glob>` `--limit <int>` (default: 200) | Search file contents with regex |
| `stat <path>` | | Show file or directory info |

---

## outline — Document Outline

```
siyuan outline [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `get --id <id>` | `--id <id>` | Get document outline (heading tree) |

---

## system — System Information

```
siyuan system [command] -w <path>
```

### Subcommands
| Command | Description |
|---------|-------------|
| `current-time` | Show current server time |

---

## template — Manage Templates

```
siyuan template [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `search [keyword]` | | Search templates; empty keyword lists all |
| `get --path <path>` | `--path <path>` | Read template content |
| `create --name <name>` | `--name <name>` `--data <md>` `--file <path>` (`-` for stdin) `--overwrite` | Create template from markdown content |
| `save-as --id <id> --name <name>` | `--id <id>` `--name <name>` `--overwrite` | Save a document as a template |
| `render --path <path> --id <id>` | `--path <path>` `--id <id>` | Render template against a block (preview) |
| `remove --path <path>` | `--path <path>` | Remove a template |

> `--path` accepts absolute path or path relative to `data/templates/` in the workspace.

---

## serve — Start Kernel HTTP Server

```
siyuan serve [flags]
```

Starts the SiYuan kernel HTTP server for API access.

### Flags
| Flag | Description |
|------|-------------|
| `-w, --workspace <path>` | Workspace path (optional, defaults to default workspace if not specified) |
| `--accessAuthCode <string>` | Access auth code |
| `--attach-ui` | Attach kernel lifecycle to desktop UI process (used by Electron) |
| `--lang <string>` | Language: ar/de/en/es/fr/he/hi/id/it/ja/ko/nl/pl/pt-BR/ru/sk/th/tr/uk/zh-CN/zh-TW |
| `--mode <string>` | Run mode: dev/prod (default "prod") |
| `--port <string>` | Port of the HTTP server (default "0" = auto) |
| `--readonly <string>` | Read-only mode: true/false (default "false") |
| `--ssl` | Enable HTTPS and WSS |
| `--wd <string>` | Working directory of SiYuan |

> **Note:** `serve` does not require `-w` flag; it starts the kernel which then loads the workspace.

---

## completion — Generate Shell Completion

```
siyuan completion [command]
```

### Subcommands
| Command | Description |
|---------|-------------|
| `bash` | Generate autocompletion script for bash |
| `fish` | Generate autocompletion script for fish |
| `powershell` | Generate autocompletion script for powershell |
| `zsh` | Generate autocompletion script for zsh |

Generate autocompletion script for the specified shell.
