# Phase 02 — Completed Assignments

**Environment:** CentOS Stream 9  
**Status:** Completed (reported by learner)  
**Evidence:** Screenshots referenced in [README.md](README.md). Individual outputs are not transcribed here.

## Assignment 1 — Directory management

**Objective:** Build a production-style directory structure.

```text
production-app/
├── config/
├── logs/
├── scripts/
├── backup/
└── releases/
```

**Tasks completed:** Created parent and child directories; navigated with `cd`; verified paths with `pwd` and listings with `ls`.

**Example commands:**

```bash
cd ~/linux-for-devops/phase-02
mkdir -p production-app/{config,logs,scripts,backup,releases}
cd production-app
pwd
ls -lah
```

## Assignment 2 — File management

**Objective:** Create application configuration, logs and a script; back up configuration.

```bash
touch config/application.properties logs/application.log scripts/deploy.sh
printf 'server.port=8080
spring.application.name=production-app
' > config/application.properties
cp config/application.properties backup/application.properties.bak
ls -lah backup
```

**Verified concepts:** `touch`, stdout redirection, `cp`, directory paths, and backup naming.

## Assignment 3 — Log investigation

**Input:**

```text
INFO Application started
INFO Database connected
WARN CPU usage reached 75%
ERROR Database connection timeout
INFO API request successful
ERROR HTTP 500 Internal Server Error
WARN Memory usage reached 80%
```

**Tasks:** Display full log; first and last three lines; search `ERROR` and `WARN`; count errors; display matching line numbers.

```bash
cat logs/application.log
head -n 3 logs/application.log
tail -n 3 logs/application.log
grep 'ERROR' logs/application.log
grep 'WARN' logs/application.log
grep -c 'ERROR' logs/application.log
grep -n 'ERROR' logs/application.log
```

**Expected error count:** `2` for the sample log above.

## Assignment 4 — File searching

**Tasks:** Locate logs, properties files, directories, regular files and files over 1 MiB; inspect configuration metadata.

```bash
find . -name '*.log'
find . -name '*.properties'
find . -type d
find . -type f
find . -type f -size +1M
stat config/application.properties
```

**Note:** A search that returns no files larger than 1 MiB is valid for small lab data.

## Assignment 5 — Hard and symbolic links

**Tasks:** Create both link types; compare inode numbers; append to original log; verify contents through both links.

Run from inside `production-app`:

```bash
ln logs/application.log logs/application-hardlink.log
ln -s application.log logs/application-symlink.log
ls -li logs/application.log logs/application-hardlink.log
ls -l logs/application-symlink.log
echo 'INFO link verification' >> logs/application.log
tail -n 1 logs/application-hardlink.log
tail -n 1 logs/application-symlink.log
```

**Key observations:** Hard link and original share an inode; symlink stores a target path; both show appended contents while target exists.

## Production scenario — Missing configuration file

**Question:** Deployment reports `config/application.properties` not found. What should be checked?

**Answer:** Verify current working directory (`pwd`), expected path (`ls`), file location (`find`), file contents (`cat`/`grep`), and metadata (`stat`). Next investigate ownership and permissions if the file exists but cannot be read.

## Interview revision — Short answers

1. **Absolute vs relative path:** Absolute starts at `/`; relative depends on the working directory.
2. **`/` vs `/root`:** `/` is the filesystem root; `/root` is root user's home.
3. **`mkdir` vs `mkdir -p`:** `-p` creates missing parents and does not fail for an existing directory.
4. **`cp` vs `mv`:** Copy creates another entry/data copy; move relocates or renames.
5. **`rm` vs `rmdir`:** `rm` removes files; `rmdir` removes empty directories.
6. **`cat`, `less`, `head`, `tail`:** Full display, paginated viewing, first lines, last lines.
7. **`tail -f`:** Follow appended data as a log grows.
8. **`>` vs `>>`:** Overwrite versus append.
9. **Pipe:** Sends stdout of one command into stdin of another.
10. **`find` vs `grep`:** Search filesystem entries versus text content.
11. **Wildcard:** Filename matching pattern such as `*.log`.
12. **Inode:** Filesystem metadata object referenced by directory entries.
13. **Hard vs symbolic link:** Same inode versus path reference.
14. **Delete original hard-link name:** Data remains accessible through another hard link.
15. **Delete symlink target:** Link generally becomes dangling.
16. **Find logs:** `find . -name '*.log'`.
17. **Find errors:** `grep 'ERROR' logs/application.log`.
18. **Count errors:** `grep -c 'ERROR' logs/application.log`.
19. **Missing configuration:** Verify directory, filename, file contents, and permissions.

