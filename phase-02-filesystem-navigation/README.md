# 🐧 Linux for DevOps — Phase 2: Linux Filesystem, Navigation & File Management

**Project:** Linux for DevOps — From Zero to Production Troubleshooting  
**Phase:** 02 — Filesystem & File Management  
**Operating System:** CentOS Stream 9  
**Level:** Beginner  
**Approach:** Concepts → Commands → Hands-on Labs → DevOps Scenarios → Assignments → GitHub

In Phase 1, we learned how to identify a Linux system and inspect CPU, memory, disk, users, and operating system information.

In **Phase 2**, you'll learn how to navigate Linux, create directories, manage files, inspect file contents, and search for files.

These are foundational skills for managing application deployments, configuration files, logs, and scripts.

We'll use your separate CentOS Stream 9 machine for all practical exercises.

---

# 1. Phase 2 — Learning Objectives

By completing this phase, you should understand:

| # | Topic | Commands |
|---|---|---|
| 1 | Linux Filesystem Hierarchy | `/`, `/etc`, `/var`, `/home`, `/opt` |
| 2 | Absolute vs Relative Paths | `pwd`, `cd` |
| 3 | Directory Navigation | `cd`, `ls`, `pwd` |
| 4 | Creating Directories | `mkdir`, `mkdir -p` |
| 5 | Creating Files | `touch`, `echo` |
| 6 | Copying Files | `cp`, `cp -r` |
| 7 | Moving and Renaming | `mv` |
| 8 | Deleting Files and Directories | `rm`, `rmdir` |
| 9 | Viewing File Contents | `cat`, `less`, `head`, `tail` |
| 10 | File Metadata | `stat`, `file` |
| 11 | Searching Files | `find`, `locate` |
| 12 | Searching File Contents | `grep` |
| 13 | Wildcards | `*`, `?`, `[]` |
| 14 | Input/Output Redirection | `>`, `>>`, `2>` |
| 15 | Pipes | `\|` |
| 16 | File Links | Hard links and symbolic links |

We'll complete **10 hands-on labs** and one production-style troubleshooting exercise.

---

# 2. Understanding the Linux Filesystem

## What is a filesystem?

A filesystem is a method used by an operating system to organize, store, and retrieve data.

For example, when you create:

```bash
touch application.log
```

Linux creates a file entry in the filesystem.

The filesystem maintains information such as:

- Filename
- File size
- Ownership
- Permissions
- Timestamps
- Location of file data

## Linux Filesystem Hierarchy

Unlike Windows, which commonly uses drive letters such as `C:` and `D:`, Linux organizes files under a single directory hierarchy.

The starting point is `/`, called the **root directory**.

```text
/
├── boot/
├── dev/
├── etc/
│   ├── hosts
│   ├── passwd
│   └── ssh/
├── home/
│   └── sumanth/
├── opt/
├── proc/
├── root/
├── run/
├── sys/
├── tmp/
├── usr/
└── var/
    ├── log/
    ├── cache/
    └── lib/
```

### Important directories for DevOps

| Directory | Purpose | Real-world DevOps example |
|---|---|---|
| `/` | Root of filesystem hierarchy | Starting point for all paths |
| `/etc` | System configuration | `/etc/ssh/sshd_config` |
| `/var/log` | System/application logs | `/var/log/messages` |
| `/home` | Normal user home directories | `/home/sumanth` |
| `/root` | Root user's home directory | Root-specific files |
| `/opt` | Optional application software | `/opt/myapp` |
| `/tmp` | Temporary files | Temporary deployment files |
| `/usr/bin` | User-space executable programs | `/usr/bin/ls` |
| `/proc` | Process and kernel information | `/proc/meminfo` |
| `/dev` | Device interfaces | `/dev/sda` |
| `/boot` | Boot-related files | Kernel images |
| `/run` | Runtime information | Service sockets and PID files |

### Real-world example

Suppose your company deploys a Spring Boot application on Linux.

You might see:

```text
/opt/gxcapture/
│
├── application.jar
├── config/
│   └── application.properties
└── logs/
    └── application.log
```

Here:

- `/opt/gxcapture/` contains application files.
- `application.jar` is the deployable Java application.
- `config/` contains application configuration.
- `logs/` contains application logs.

This is an example layout, not a mandatory Linux standard.

---

# 3. LAB 01 — Absolute and Relative Paths

This is one of the most important Linux fundamentals.

## What is an absolute path?

An absolute path starts from the root directory `/`.

Example:

```text
/home/sumanth/projects
```

Regardless of your current location, this path identifies the same directory.

## What is a relative path?

A relative path is interpreted from your current working directory.

Suppose you are inside:

```text
/home/sumanth
```

You can navigate to:

```text
projects
```

using:

```bash
cd projects
```

This is a relative path.

### Comparison

| Absolute Path | Relative Path |
|---|---|
| Starts with `/` | Does not start with `/` |
| Independent of current directory | Depends on current directory |
| `/home/sumanth/projects` | `projects` |
| `/etc/ssh` | `../../etc/ssh` (from `/home/sumanth`) |

## Hands-on

On your CentOS machine, execute:

```bash
pwd
```

Expected:

```text
/home/sumanth
```

Navigate using an absolute path:

```bash
cd /etc
pwd
```

Expected:

```text
/etc
```

Return home:

```bash
cd ~
```

Now execute:

```bash
cd ..
pwd
```

Expected:

```text
/home
```

Return to your home directory:

```bash
cd /home/sumanth
```

### Special directory symbols

| Symbol | Meaning |
|---|---|
| `/` | Root directory |
| `~` | Current user's home directory |
| `.` | Current directory |
| `..` | Parent directory |
| `-` | Previous directory, when used with `cd` |

### Assignment

Navigate from:

```text
/home/sumanth
```

to:

```text
/etc
```

using an absolute path.

Then navigate back to your home directory.

---

# 4. LAB 02 — Create a DevOps Project Directory

Now we'll create a practical directory structure.

We'll use a simulated application called `devops-demo-app`.

**Important:** All exercises will be performed inside your home directory. We won't modify production system directories.

## Step 1 — Navigate home

```bash
cd ~
```

## Step 2 — Create the project directory

```bash
mkdir -p linux-for-devops/phase-02
```

### What is `mkdir`?

`mkdir` means **make directory**.

### What does `-p` do?

The `-p` option creates missing parent directories as needed.

For example:

```bash
mkdir -p project/app/logs
```

Creates:

```text
project/
└── app/
    └── logs/
```

## Step 3 — Navigate into the lab directory

```bash
cd ~/linux-for-devops/phase-02
```

## Step 4 — Create application directories

Execute:

```bash
mkdir -p devops-demo-app/{config,logs,scripts,backup,deployments}
```

### What does `{}` mean?

This is called **brace expansion** in Bash.

It allows you to generate multiple words from a pattern.

The command creates five directories.

Verify:

```bash
ls -lah devops-demo-app
```

Expected structure:

```text
devops-demo-app/
├── config/
├── logs/
├── scripts/
├── backup/
└── deployments/
```

### Production relevance

Application deployments often require separate locations for configuration, logs, scripts, and deployment artifacts.

Organizing files properly makes troubleshooting and maintenance easier.

---

# 5. LAB 03 — Create and Manage Files

## Step 1 — Create an empty file

Navigate into your application directory:

```bash
cd ~/linux-for-devops/phase-02/devops-demo-app
```

Execute:

```bash
touch application.log
```

### What is `touch`?

`touch` updates file timestamps.

If the specified file does not exist, it normally creates an empty file.

Verify:

```bash
ls -l application.log
```

## Step 2 — Create multiple files

```bash
touch config/application.properties
touch logs/app.log
touch scripts/deploy.sh
```

Verify:

```bash
ls -lah config logs scripts
```

## Step 3 — Write content into a file

Execute:

```bash
echo "Application started successfully" > logs/app.log
```

### What is `echo`?

`echo` prints text.

### What is `>`?

`>` redirects output into a file.

**Important:** It overwrites the existing file contents.

Check:

```bash
cat logs/app.log
```

Expected:

```text
Application started successfully
```

## Step 4 — Append content

Execute:

```bash
echo "Database connection established" >> logs/app.log
```

### What is `>>`?

`>>` appends content to the end of a file.

Verify:

```bash
cat logs/app.log
```

Expected:

```text
Application started successfully
Database connection established
```

### DevOps scenario

Suppose an application produces logs.

You need to understand whether a command overwrites or appends data.

| Operator | Purpose |
|---|---|
| `>` | Overwrite output file |
| `>>` | Append output |
| `2>` | Redirect standard error |
| `2>>` | Append standard error |

Be careful using `>` with important configuration or log files.

---

# 6. LAB 04 — Copy, Move, Rename and Delete Files

These commands are used frequently during deployments.

## Part A — Copy files

Create a sample configuration:

```bash
echo "server.port=8080" > config/application.properties
```

Copy it into the backup directory:

```bash
cp config/application.properties backup/
```

### What is `cp`?

`cp` means **copy**.

Verify:

```bash
ls -l backup/
```

You should see:

```text
application.properties
```

### Copy an entire directory

```bash
cp -r config backup/config-backup
```

The `-r` option copies directories recursively.

## Part B — Rename files

Execute:

```bash
mv application.log application-old.log
```

### What is `mv`?

`mv` moves or renames files and directories.

Verify:

```bash
ls -l
```

You should see:

```text
application-old.log
```

## Part C — Move files

Move the renamed file into the logs directory:

```bash
mv application-old.log logs/
```

Verify:

```bash
ls -l logs/
```

## Part D — Delete files

Create a temporary file:

```bash
touch test-delete.txt
```

Delete it:

```bash
rm test-delete.txt
```

### What is `rm`?

`rm` removes files.

### Delete an empty directory

Create an empty directory:

```bash
mkdir test-empty
```

Delete it:

```bash
rmdir test-empty
```

### Important safety rule

Avoid executing commands such as:

```bash
rm -rf /some/path
```

unless you fully understand the target path and consequences.

`rm -rf` recursively and forcibly removes files and directories. It can cause serious data loss.

For now, practice deletion only inside your Phase 2 lab directory.

---

# 7. LAB 05 — View File Contents

DevOps engineers frequently inspect application logs and configuration files.

We'll use:

```text
cat
less
head
tail
```

## Step 1 — Create sample application logs

Execute these commands from `devops-demo-app`:

```bash
echo "INFO Application started" > logs/app.log
echo "INFO Database connected" >> logs/app.log
echo "INFO User login successful" >> logs/app.log
echo "WARN Memory usage increasing" >> logs/app.log
echo "ERROR Database connection timeout" >> logs/app.log
echo "ERROR API request failed" >> logs/app.log
```

## Step 2 — View the complete file

```bash
cat logs/app.log
```

`cat` displays file contents.

## Step 3 — View the beginning

```bash
head -n 3 logs/app.log
```

Expected:

```text
INFO Application started
INFO Database connected
INFO User login successful
```

### Explanation

`head` displays the beginning of a file.

`-n 3` means display three lines.

## Step 4 — View the end

```bash
tail -n 3 logs/app.log
```

Expected:

```text
WARN Memory usage increasing
ERROR Database connection timeout
ERROR API request failed
```

## Step 5 — View logs interactively

```bash
less logs/app.log
```

Use:

```text
Space → Next page
b     → Previous page
q     → Quit
```

## Step 6 — Follow live logs

```bash
tail -f logs/app.log
```

`-f` means **follow**.

The command waits for new content and displays it as the file grows.

To test it, keep this command running in one terminal.

Open another terminal and execute:

```bash
echo "INFO New API request received" >> ~/linux-for-devops/phase-02/devops-demo-app/logs/app.log
```

The new line should appear in the first terminal.

Press `Ctrl + C` to stop following the file.

### Real-world example

For an application writing to a log file:

```bash
tail -f /opt/myapp/logs/application.log
```

This is useful during deployments and troubleshooting.

---

# 8. LAB 06 — Search Files and Log Contents

This is one of the most important labs in Phase 2.

## Part A — Find files

### What is `find`?

`find` searches directory trees using criteria such as filename, type, and size.

Find all `.log` files:

```bash
find . -name "*.log"
```

### Explanation

| Component | Meaning |
|---|---|
| `find` | Search command |
| `.` | Current directory |
| `-name` | Match filename |
| `"*.log"` | Files ending in `.log` |

Find a specific configuration file:

```bash
find . -name "application.properties"
```

Find directories:

```bash
find . -type d
```

Find regular files:

```bash
find . -type f
```

Find files larger than 1 MB:

```bash
find . -type f -size +1M
```

You may get no output because our lab files are small. That's normal.

## Part B — Search inside files using grep

### What is `grep`?

`grep` searches text for matching patterns.

Find errors:

```bash
grep "ERROR" logs/app.log
```

Expected:

```text
ERROR Database connection timeout
ERROR API request failed
```

Find warnings:

```bash
grep "WARN" logs/app.log
```

Search without case sensitivity:

```bash
grep -i "error" logs/app.log
```

Show line numbers:

```bash
grep -n "ERROR" logs/app.log
```

Count matching lines:

```bash
grep -c "ERROR" logs/app.log
```

Expected:

```text
2
```

### Production scenario

A developer reports:

> The Spring Boot application is returning HTTP 500 errors.

You might start by searching application logs:

```bash
grep -i "error" logs/app.log
```

For large logs, use targeted searches and timestamps to narrow the investigation.

---

# 9. LAB 07 — Understand Pipes

## What is a pipe?

A pipe connects the standard output of one command to the standard input of another command.

The pipe symbol is:

```text
|
```

### Example 1

```bash
cat logs/app.log | grep "ERROR"
```

How it works:

```text
cat logs/app.log
       |
       v
Displays file contents
       |
       v
      PIPE
       |
       v
grep "ERROR"
       |
       v
Displays matching lines
```

A simpler equivalent is:

```bash
grep "ERROR" logs/app.log
```

### Example 2 — Count error lines

```bash
grep "ERROR" logs/app.log | wc -l
```

Expected:

```text
2
```

### What is `wc`?

`wc` means **word count**.

The `-l` option counts lines.

### Example 3 — Filter system processes

```bash
ps aux | grep bash
```

This displays process-list lines containing `bash`.

We'll study process management in detail in a later phase.

---

# 10. LAB 08 — Understand Wildcards

Wildcards help match multiple filenames.

### Common wildcards

| Wildcard | Meaning |
|---|---|
| `*` | Matches zero or more characters |
| `?` | Matches exactly one character |
| `[abc]` | Matches one listed character |
| `[0-9]` | Matches one digit |

## Hands-on

Create files:

```bash
touch test1.txt test2.txt test3.txt
touch app1.log app2.log
```

List all text files:

```bash
ls *.txt
```

List all log files in the current directory:

```bash
ls *.log
```

Match numbered text files:

```bash
ls test?.txt
```

Match files containing one digit after `test`:

```bash
ls test[0-9].txt
```

### DevOps relevance

Wildcards are useful when working with:

- Log files
- Backup files
- Deployment artifacts
- Configuration files
- Script collections

---

# 11. LAB 09 — Inspect File Metadata

Linux stores metadata about files.

## Step 1 — Inspect a file

```bash
stat logs/app.log
```

Example fields:

```text
File
Size
Blocks
IO Block
Device
Inode
Links
Access
Modify
Change
Birth
```

### Important concepts

| Field | Meaning |
|---|---|
| Size | File size |
| Inode | Filesystem identifier for the file |
| Access | Last access time |
| Modify | Last content modification |
| Change | Last metadata/status change |
| Birth | Creation time, when supported |

### What is an inode?

An inode stores metadata about a filesystem object.

It contains information such as:

- Ownership
- Permissions
- File size
- Timestamps
- References to data blocks

A filename is stored in a directory entry that refers to an inode.

We'll study inodes more deeply in the storage phase.

## Step 2 — Identify file type

```bash
file logs/app.log
```

The `file` command attempts to identify the type of data contained in a file.

For example, it may report:

```text
logs/app.log: ASCII text
```

---

# 12. LAB 10 — Hard Links and Symbolic Links

This is an important Linux interview topic.

Linux supports two commonly discussed link types:

1. Hard links
2. Symbolic links

## What is a hard link?

A hard link is another filename referring to the same inode.

Create one:

```bash
ln logs/app.log logs/app-hardlink.log
```

Check inode numbers:

```bash
ls -li logs/app.log logs/app-hardlink.log
```

Both should have the same inode number.

### Important

Changes made through either hard link affect the same underlying file contents.

Hard links generally cannot cross filesystem boundaries.

## What is a symbolic link?

A symbolic link is a filesystem object containing a path to another file or directory.

Create one:

```bash
ln -s app.log logs/app-symlink.log
```

Because the link is created inside `logs`, the relative target `app.log` points to `logs/app.log`.

Verify:

```bash
ls -l logs/
```

You should see something similar to:

```text
app-symlink.log -> app.log
```

### Hard Link vs Symbolic Link

| Hard Link | Symbolic Link |
|---|---|
| References the same inode | Stores a path to another file |
| Usually cannot cross filesystems | Can cross filesystems |
| Normally cannot link directories | Can link directories |
| Remains usable if another filename is deleted | Can become broken if its target path disappears |

### DevOps example

Applications sometimes use symbolic links to switch between deployment versions.

Example:

```text
/opt/myapp/
├── releases/
│   ├── v1/
│   └── v2/
└── current -> releases/v2/
```

This pattern can help manage application releases.

---

# 13. Production-Style Troubleshooting Lab

## Scenario — Application Deployment Failed

Imagine your deployment pipeline reports:

```text
Deployment failed.

Error:
Application configuration file not found.
```

Expected configuration:

```text
devops-demo-app/config/application.properties
```

### Step 1 — Confirm your location

```bash
pwd
```

### Step 2 — List application files

```bash
ls -lah
```

### Step 3 — Find the configuration file

```bash
find . -name "application.properties"
```

### Step 4 — Check file contents

```bash
cat config/application.properties
```

### Step 5 — Inspect metadata

```bash
stat config/application.properties
```

### Step 6 — Verify configuration

```bash
grep "server.port" config/application.properties
```

Expected:

```text
server.port=8080
```

### What did you learn?

You learned how to investigate:

- Missing files
- Incorrect file paths
- Incorrect working directories
- Configuration contents
- File metadata

These are common issues in Linux-based application deployments.

---

# 14. Phase 2 — Practical Assignment

Now complete these exercises without copying every command from the lesson.

## Assignment 1 — Directory Management

Create this structure:

```text
production-app/
├── config/
├── logs/
├── scripts/
├── backup/
└── releases/
```

**Tasks:**

1. Create the main directory.
2. Create all subdirectories.
3. Navigate into the application directory.
4. Display the current directory.
5. List all directories.

---

## Assignment 2 — File Management

Inside `production-app`, create:

```text
config/application.properties
logs/application.log
scripts/deploy.sh
```

Add this configuration:

```properties
server.port=8080
spring.application.name=production-app
```

**Tasks:**

1. Create the files.
2. Add configuration values.
3. Copy `application.properties` into `backup`.
4. Rename the copied file to `application.properties.bak`.
5. Verify the backup exists.

---

## Assignment 3 — Log Investigation

Add these entries to `logs/application.log`:

```text
INFO Application started
INFO Database connected
WARN CPU usage reached 75%
ERROR Database connection timeout
INFO API request successful
ERROR HTTP 500 Internal Server Error
WARN Memory usage reached 80%
```

**Tasks:**

1. Display the complete log.
2. Display the first three lines.
3. Display the last three lines.
4. Search for `ERROR`.
5. Search for `WARN`.
6. Count the number of errors.
7. Display error line numbers.

---

## Assignment 4 — File Searching

**Tasks:**

1. Find all `.log` files.
2. Find all `.properties` files.
3. Find all directories.
4. Find all regular files.
5. Find files larger than 1 MB.
6. Display the metadata of `application.properties`.

---

## Assignment 5 — Linux Links

**Tasks:**

1. Create a hard link to `application.log`.
2. Create a symbolic link to `application.log`.
3. Compare inode numbers.
4. Modify the original log file.
5. Verify that the changes are visible through both links.

---

# 15. Phase 2 — Interview Questions

Try answering these questions in your own words.

| # | Question |
|---|---|
| 1 | What is the Linux filesystem hierarchy? |
| 2 | What is the difference between absolute and relative paths? |
| 3 | What is the difference between `/` and `/root`? |
| 4 | What is the difference between `mkdir` and `mkdir -p`? |
| 5 | What is the difference between `cp` and `mv`? |
| 6 | What is the difference between `rm` and `rmdir`? |
| 7 | What is the difference between `cat`, `less`, `head`, and `tail`? |
| 8 | What is the purpose of `tail -f`? |
| 9 | What is the difference between `>` and `>>`? |
| 10 | What is a Linux pipe? |
| 11 | What is the difference between `find` and `grep`? |
| 12 | What are Linux wildcards? |
| 13 | What is an inode? |
| 14 | What is the difference between a hard link and a symbolic link? |
| 15 | What happens when the original file is deleted but a hard link exists? |
| 16 | What happens when a symbolic link's target is deleted? |
| 17 | How do you find all `.log` files in a directory? |
| 18 | How do you find errors in an application log? |
| 19 | How do you count the number of error entries? |
| 20 | How do you troubleshoot a missing application configuration file? |

---

# 16. GitHub Repository — Phase 2

Once you complete the labs, add this directory to your existing GitHub repository.

```text
linux-for-devops-hands-on/
│
├── README.md
│
├── phase-01-linux-fundamentals/
│   ├── README.md
│   ├── commands.md
│   ├── assignment.md
│   └── screenshots/
│
└── phase-02-filesystem-navigation/
    │
    ├── README.md
    ├── commands.md
    ├── assignment.md
    │
    └── screenshots/

```

### Recommended screenshots

| Screenshot | What to Capture |
|---|---|
| 01 | Absolute and relative path navigation |
| 02 | Application directory structure |
| 03 | File creation, copying, moving |
| 04 | Log filtering using `grep`, `head`, `tail` |
| 05 | File searching using `find` |
| 06 | File metadata using `stat` |
| 07 | Hard links and symbolic links |
| 08 | Missing configuration troubleshooting |

### Suggested Git commit

```bash
git add .
git commit -m "Phase 02: Linux filesystem navigation and file management"
git push
```

---

# 17. Phase 2 Completion Checklist

| Task | Status |
|---|---|
| Understand filesystem hierarchy | ⏳ |
| Understand absolute and relative paths | ⏳ |
| Navigate Linux directories | ⏳ |
| Create directories | ⏳ |
| Create and edit files | ⏳ |
| Copy and move files | ⏳ |
| Delete files safely | ⏳ |
| View file contents | ⏳ |
| Search files using `find` | ⏳ |
| Search logs using `grep` | ⏳ |
| Understand pipes and redirection | ⏳ |
| Understand wildcards | ⏳ |
| Inspect file metadata | ⏳ |
| Understand hard and symbolic links | ⏳ |
| Complete production troubleshooting lab | ⏳ |
| Complete assignments | ⏳ |
| Upload GitHub documentation | ⏳ |

---

## What You Should Do Next

Start with **Labs 01–05** on your separate CentOS Stream 9 system.

After completing them, share screenshots or terminal outputs showing:

```bash
pwd
ls -lah
ls -lah devops-demo-app
cat devops-demo-app/logs/app.log
```

Run the last two commands from `~/linux-for-devops/phase-02`.

I'll review your results before you continue with Labs 06–10.

Once all ten labs and assignments are complete, we'll prepare your **Phase 2 GitHub README.md, commands.md, assignment.md, and LinkedIn project update**.

**Next phase after this: Phase 3 — Linux Users, Groups, Permissions & Ownership**, where we'll begin learning Linux administration and troubleshooting permission-related deployment failures.