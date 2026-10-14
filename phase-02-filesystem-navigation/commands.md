# Phase 02 — Linux Commands Reference

Environment: **CentOS Stream 9 / Bash**. Paths below assume a practice directory at `~/linux-for-devops/phase-02/devops-demo-app`.

## Navigation and directories

| Command | Purpose | Example |
|---|---|---|
| `pwd` | Print current working directory | `pwd` |
| `cd` | Change directory | `cd /etc` |
| `cd ~` | Go home | `cd ~` |
| `cd ..` | Go to parent | `cd ..` |
| `cd -` | Return to previous directory | `cd -` |
| `ls` | List entries | `ls` |
| `ls -lah` | Long listing, hidden entries, readable sizes | `ls -lah` |
| `mkdir` | Create directory | `mkdir logs` |
| `mkdir -p` | Create parent directories as needed | `mkdir -p app/{config,logs}` |

**Paths:** `/home/sumanth/project` is absolute; `project` is relative. `.` means current directory and `..` means parent directory.

## Create, copy, move and delete

| Command | Purpose | Example |
|---|---|---|
| `touch` | Create empty file if absent; otherwise update timestamps | `touch logs/app.log` |
| `echo` | Print text | `echo 'INFO started'` |
| `cp` | Copy file | `cp config/application.properties backup/` |
| `cp -r` | Copy directory recursively | `cp -r config backup/config-copy` |
| `mv` | Move or rename | `mv old.log logs/old.log` |
| `rm` | Remove file | `rm test-delete.txt` |
| `rmdir` | Remove empty directory | `rmdir test-empty` |

**Caution:** `rm` generally does not provide an undo facility. Always verify paths and avoid destructive commands outside your lab.

## Reading logs and files

| Command | Purpose | Example |
|---|---|---|
| `cat` | Display complete file | `cat logs/app.log` |
| `less` | Scroll through file (`q` quits) | `less logs/app.log` |
| `head -n 3` | First three lines | `head -n 3 logs/app.log` |
| `tail -n 3` | Last three lines | `tail -n 3 logs/app.log` |
| `tail -f` | Follow appended log lines | `tail -f logs/app.log` |

## Find and grep

```bash
find . -name '*.log'               # Match names
find . -type f                    # Regular files
find . -type d                    # Directories
find . -type f -size +1M          # Files larger than 1 MiB
find . -name 'application.properties'
grep 'ERROR' logs/app.log          # Matching lines
grep -i 'error' logs/app.log       # Case-insensitive
grep -n 'ERROR' logs/app.log       # Line numbers
grep -c 'ERROR' logs/app.log       # Count matching lines
```

`find` searches filesystem entries. `grep` searches content within text.

## Redirection and pipes

| Syntax | Meaning | Example |
|---|---|---|
| `>` | Redirect stdout, overwrite file | `echo 'started' > logs/app.log` |
| `>>` | Redirect stdout, append | `echo 'ready' >> logs/app.log` |
| `2>` | Redirect stderr, overwrite | `ls missing 2> errors.txt` |
| `2>>` | Redirect stderr, append | `ls missing 2>> errors.txt` |
| `\|` | Pipe stdout to next command | `grep ERROR logs/app.log \| wc -l` |

The command `grep -c 'ERROR' logs/app.log` also counts matching lines directly.

## Wildcards

```bash
ls *.txt         # Names ending .txt
ls test?.txt     # Exactly one character between test and .txt
ls test[0-9].txt # One digit between test and .txt
```

Shell globs match filenames. Quoted `*.log` in `find -name '*.log'` is interpreted by `find`, not expanded by the shell.

## Metadata and links

```bash
stat logs/app.log                   # Metadata, timestamps, inode
file logs/app.log                   # Inspect file content type
ls -li logs/app.log                 # Show inode number
ln logs/app.log logs/app-hardlink.log
ln -s app.log logs/app-symlink.log  # Target is relative to link location
ls -l logs/
```

**Hard link:** Another name for the same inode; normally cannot cross filesystems.  
**Symbolic link:** Stores a path to its target; may become broken if that path no longer resolves.

## Troubleshooting quick reference

```bash
pwd
ls -lah
find . -name 'application.properties'
cat config/application.properties
stat config/application.properties
grep 'server.port' config/application.properties
```

