# SiYuan CLI — Full Command Reference

Auto-generated from `siyuan --help` output (v3.7.0-dev11).

---

## Top-Level Flags

| Flag | Description |
|------|-------------|
| `-f, --format <table\|json>` | Output format (default: `table`) |
| `-h, --help` | Show help |
| `-v, --version` | Print version |
| `-w, --workspace <path>` | Workspace path (required for all commands except `workspace`) |

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
| Command | Description |
|---------|-------------|
| `list` | List all notebooks |
| `create` | Create a notebook |
| `rename` | Rename a notebook |
| `remove` | Remove a notebook |

---

## document — Manage Documents

```
siyuan document [command] -w <path>
```

### Subcommands
| Command | Description |
|---------|-------------|
| `list` | List documents in a notebook |
| `get` | Get document info |
| `create` | Create a document |
| `rename` | Rename a document |
| `move` | Move a document to another notebook |
| `duplicate` | Duplicate a document |
| `remove` | Remove a document |

---

## block — Block Operations

```
siyuan block [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `get` | `--id <id>` | Get block info |
| `children` | `--id <id>` | Get child blocks |
| `insert` | `--id <id>` | Insert block |
| `append` | `--id <id>` | Append block |
| `prepend` | `--id <id>` | Prepend block |
| `stat` | | Get block content statistics |
| `update` | `--id <id>` | Update block |
| `delete` | `--id <id>` | Delete block |
| `move` | `--id <id>` | Move block |
| `dom` | `--id <id>` | Get block DOM |
| `kramdown` | `--id <id>` | Get block kramdown |
| `breadcrumb` | `--id <id>` | Get block breadcrumb |
| `info` | `--id <id>` | Get document info |

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
| `md --id <id> [--output <path>]` | | Export as Markdown (default: stdout) |
| `html` | | Export as HTML |
| `docx` | | Export as Word (.docx) |
| `sy` | | Export as .sy.zip |
| `md-zip` | | Export as Markdown zip |
| `preview` | | Export as preview HTML |
| `data` | | Export full workspace data backup |

---

## import — Import Files

```
siyuan import [command] -w <path>
```

### Subcommands
| Command | Flags | Description |
|---------|-------|-------------|
| `md --file <path> --notebook <id>` | `--hpath <path>` `--path <path>` | Import Markdown file or directory |
| `sy` | | Import .sy.zip archive |
| `data` | | Import data backup |

---

## asset — Manage Assets

```
siyuan asset [command] -w <path>
```

### Subcommands
| Command | Description |
|---------|-------------|
| `unused` | List unused assets |
| `clean` | Clean unused assets |
| `upload` | Upload files to workspace assets |

---

## attr — Manage Block Attributes

```
siyuan attr [command] -w <path>
```

### Subcommands
| Command | Description |
|---------|-------------|
| `get --id <id>` | Get block attributes |
| `set` | Set block attributes |
| `batch-get` | Batch get block attributes |

---

## bookmark — Manage Bookmarks

```
siyuan bookmark [command] -w <path>
```

### Subcommands
| Command | Description |
|---------|-------------|
| `list` | List bookmarks |
| `labels` | List bookmark labels |
| `rename` | Rename a bookmark |
| `remove` | Remove a bookmark |

---

## tag — Manage Tags

```
siyuan tag [command] -w <path>
```

### Subcommands
| Command | Description |
|---------|-------------|
| `list` | List tags |
| `rename` | Rename a tag |
| `remove` | Remove a tag |

---

## history — Data History

```
siyuan history [command] -w <path>
```

### Subcommands
| Command | Description |
|---------|-------------|
| `list` | List all history |
| `get` | Get historical file content |
| `search` | Search history |
| `rollback` | Rollback a document to historical version |
| `clear` | Clear all history |

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
| Command | Description |
|---------|-------------|
| `list` | List snapshots |
| `create` | Create a snapshot |
| `diff` | Diff two snapshots |
| `checkout` | Checkout (rollback to) a snapshot |
| `tag` | Tag a snapshot |
| `untag` | Remove a tag |
| `purge` | Purge old snapshots |
| `search` | Search files in snapshots |
| `file` | File-level snapshot operations (export/get/open/rollback) |

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
| Command | Description |
|---------|-------------|
| `search` | Search databases by name |
| `get` | Get database content |
| `keys` | List database keys (fields) |
| `key add` | Add a key (field) to database |
| `key remove` | Remove a key (field) from database |
| `item add` | Add a row to database |
| `item update` | Update a cell value |
| `item remove` | Remove rows from database |
| `render` | Render database data |
| `unused` | List unused databases |
| `clean` | Clean unused databases |

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
| Command | Description |
|---------|-------------|
| `list` | List directory contents |
| `read` | Read file content |
| `write` | Write file content (stdin or `--file`) |
| `copy` | Copy file or directory |
| `rename` | Rename or move file |
| `delete` | Delete file or directory |

---

## serve — Start Kernel HTTP Server

```
siyuan serve [flags] -w <path>
```

Starts the SiYuan kernel HTTP server for API access.

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
