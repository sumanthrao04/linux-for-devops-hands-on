# Linux Phase 2 — 20 Interview Questions and Answers

**Topic: Linux Filesystem, File Management, Commands, Inodes, Links, and Troubleshooting**

These questions are important for **DevOps Engineers with 3–6 years of experience**.

For each question, I'll explain:

* **Concept:** Understand what it means.
* **Example:** Learn how the command works.
* **Interview Answer:** Explain it confidently to the interviewer.

---

# 1. What Is the Linux Filesystem Hierarchy?

### Concept

The Linux filesystem hierarchy defines how files and directories are organized in Linux.

Unlike Windows, which uses drives such as `C:` and `D:`, Linux organizes files under a single root directory `/`.

The Linux filesystem follows the **Filesystem Hierarchy Standard (FHS)**.

### Linux Filesystem Structure

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
│   ├── user1
│   └── user2
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
    └── log
```

### Important Directories

| Directory | Purpose                                           | Example                    |
| --------- | ------------------------------------------------- | -------------------------- |
| `/`       | Root of the entire filesystem                     | `/`                        |
| `/bin`    | Essential user commands                           | `ls`, `cp`, `mv`           |
| `/sbin`   | System administration commands                    | `fsck`, `reboot`           |
| `/boot`   | Linux boot files                                  | Kernel, GRUB               |
| `/etc`    | System configuration files                        | `/etc/ssh/sshd_config`     |
| `/home`   | Regular users' home directories                   | `/home/ubuntu`             |
| `/root`   | Root user's home directory                        | `/root`                    |
| `/var`    | Variable data                                     | Logs, caches               |
| `/tmp`    | Temporary files                                   | Temporary application data |
| `/usr`    | User applications, libraries, and utilities       | `/usr/bin`                 |
| `/opt`    | Optional third-party software                     | `/opt/myapp`               |
| `/dev`    | Device files                                      | `/dev/sda`                 |
| `/proc`   | Virtual filesystem for process/kernel information | `/proc/cpuinfo`            |
| `/sys`    | Kernel device and subsystem information           | `/sys/class`               |
| `/mnt`    | Temporary filesystem mounting                     | `/mnt/data`                |

**Note:** In modern Linux distributions, `/bin` and `/sbin` are often symbolic links to directories under `/usr`.

### Real-Time DevOps Example

Suppose your Spring Boot application runs on an Ubuntu server.

```text
/opt/myapp/app.jar        → Application
/etc/myapp/config.yml     → Configuration
/var/log/myapp/app.log    → Logs
/home/ubuntu              → User home
```

Understanding the filesystem hierarchy helps you locate application files, configurations, and logs.

### Interview Answer

"The Linux filesystem hierarchy is a directory structure that organizes all files under the root directory `/`. Important directories include `/etc` for configurations, `/var/log` for logs, `/home` for user files, `/boot` for boot files, and `/opt` for third-party applications. As a DevOps Engineer, I use this structure to manage applications, troubleshoot configuration issues, and analyze logs."

---

# 2. What Is the Difference Between Absolute and Relative Paths?

### Concept

A path specifies the location of a file or directory.

Linux supports two types:

1. Absolute path
2. Relative path

### Absolute Path

An absolute path starts from the root directory `/`.

Example:

```bash
cd /var/log
```

It works regardless of your current directory, provided the path exists and you have permission.

### Relative Path

A relative path starts from your current working directory.

Example:

```bash
cd logs
```

Suppose your current directory is:

```text
/home/ubuntu
```

Executing:

```bash
cd projects
```

Moves you to:

```text
/home/ubuntu/projects
```

### Difference

| Absolute Path                    | Relative Path                                    |
| -------------------------------- | ------------------------------------------------ |
| Starts with `/`                  | Usually does not start with `/`                  |
| Independent of current directory | Depends on current directory                     |
| Specifies complete location      | Specifies location relative to current directory |
| Example: `/etc/nginx/nginx.conf` | Example: `config/nginx.conf`                     |

### Useful Commands

```bash
pwd
```

Displays your current directory.

```bash
cd ..
```

Moves to the parent directory.

```bash
cd ~
```

Moves to your home directory.

### Interview Answer

"An absolute path specifies the complete location of a file starting from `/`, whereas a relative path specifies the location based on the current working directory. For example, `/var/log/app.log` is an absolute path, while `logs/app.log` is a relative path."

---

# 3. What Is the Difference Between `/` and `/root`?

This is a frequently asked Linux interview question.

### Concept

Both are directories, but their purposes are completely different.

| `/`                                       | `/root`                                     |
| ----------------------------------------- | ------------------------------------------- |
| Root of the entire Linux filesystem       | Home directory of the root user             |
| Contains all other directories            | Contains the root user's personal files     |
| Starting point of the directory hierarchy | Used for root user files and configurations |
| Example: `/etc`, `/var`, `/home`          | Example: `/root/.bashrc`                    |

### Example

```bash
cd /
pwd
```

Output:

```text
/
```

Now:

```bash
cd /root
pwd
```

Output:

```text
/root
```

Accessing `/root` normally requires root privileges.

### Interview Answer

"`/` is the root directory of the entire Linux filesystem, whereas `/root` is the home directory of the root user. All Linux directories are organized under `/`, while `/root` stores the root user's personal files and configurations."

---

# 4. What Is the Difference Between `mkdir` and `mkdir -p`?

### Concept

`mkdir` stands for **Make Directory**.

It is used to create directories.

### Example 1: `mkdir`

```bash
mkdir project
```

Creates:

```text
project/
```

However:

```bash
mkdir project/backend/logs
```

Fails if the parent directories do not exist.

### Example 2: `mkdir -p`

```bash
mkdir -p project/backend/logs
```

Creates the required parent directories automatically.

Result:

```text
project/
└── backend/
    └── logs/
```

The `-p` option also avoids reporting an error if the directory already exists.

### Real-Time DevOps Example

During application deployment:

```bash
sudo mkdir -p /opt/myapp/config
sudo mkdir -p /opt/myapp/logs
sudo mkdir -p /opt/myapp/backups
```

### Interview Answer

"`mkdir` creates a directory, while `mkdir -p` creates the directory along with any missing parent directories. I commonly use `mkdir -p` in deployment scripts to create directory structures safely."

---

# 5. What Is the Difference Between `cp` and `mv`?

### Concept

Both commands manage files and directories.

* `cp` = Copy
* `mv` = Move

### `cp` Command

```bash
cp app.conf app-backup.conf
```

Creates a copy.

```text
app.conf
app-backup.conf
```

Both files exist.

To copy a directory recursively:

```bash
cp -r source/ backup/
```

### `mv` Command

```bash
mv app.conf /tmp/
```

Moves the file to another location.

It can also rename files:

```bash
mv old.conf new.conf
```

### Difference

| `cp`                                        | `mv`                                                          |
| ------------------------------------------- | ------------------------------------------------------------- |
| Copies files                                | Moves or renames files                                        |
| Original remains                            | Original path is removed                                      |
| Requires additional storage for copied data | Rename on the same filesystem usually does not copy file data |
| `cp -r` copies directories                  | `mv` moves directories                                        |

### Real-Time DevOps Example

Before updating Nginx configuration:

```bash
sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak
```

This creates a backup.

### Interview Answer

"`cp` copies files or directories while keeping the original, whereas `mv` moves or renames files. In production, I use `cp` to back up configuration files before making changes and `mv` to rename or relocate files."

---

# 6. What Is the Difference Between `rm` and `rmdir`?

### Concept

Both commands remove filesystem entries.

### `rm`

Removes files.

```bash
rm test.txt
```

To remove a directory and its contents:

```bash
rm -r testdir
```

### `rmdir`

Removes empty directories only.

```bash
rmdir testdir
```

If the directory contains files, the command fails.

### Difference

| `rm`                                        | `rmdir`                              |
| ------------------------------------------- | ------------------------------------ |
| Removes files                               | Removes empty directories            |
| Can remove non-empty directories using `-r` | Cannot remove non-empty directories  |
| More flexible                               | Safer for removing empty directories |

### Important Production Warning

Be extremely careful with:

```bash
rm -rf directory/
```

Here:

* `-r` means recursive.
* `-f` means force.

It can permanently remove files without prompting.

### Interview Answer

"`rm` removes files and can remove directories recursively using `-r`, whereas `rmdir` removes only empty directories. In production, I verify the target path before running recursive deletion commands."

---

# 7. What Is the Difference Between `cat`, `less`, `head`, and `tail`?

These commands are commonly used for viewing files.

### Example

Suppose you have:

```text
/var/log/myapp/application.log
```

### `cat` — Display File Content

```bash
cat application.log
```

Displays the entire file.

Useful for small files.

### `less` — View Large Files

```bash
less application.log
```

Allows you to scroll through the file.

Useful shortcuts:

| Key      | Purpose           |
| -------- | ----------------- |
| Space    | Next page         |
| `b`      | Previous page     |
| `/ERROR` | Search for ERROR  |
| `n`      | Next search match |
| `q`      | Quit              |

### `head` — View Beginning of File

```bash
head -n 10 application.log
```

Displays the first 10 lines.

### `tail` — View End of File

```bash
tail -n 20 application.log
```

Displays the last 20 lines.

### Difference

| Command | Purpose                   |
| ------- | ------------------------- |
| `cat`   | Display complete file     |
| `less`  | Read file interactively   |
| `head`  | Display beginning of file |
| `tail`  | Display end of file       |

### Interview Answer

"`cat` displays file contents, `less` allows interactive viewing of large files, `head` displays the beginning of a file, and `tail` displays the end. For production logs, I generally use `less` and `tail` because log files can be large."

---

# 8. What Is the Purpose of `tail -f`?

### Concept

The `tail -f` command monitors a file continuously and displays newly appended lines.

It is useful for monitoring application logs in real time.

### Example

```bash
tail -f /var/log/myapp/application.log
```

Suppose your application generates:

```text
2026-10-14 10:00:01 INFO Application started
2026-10-14 10:00:05 INFO Database connected
2026-10-14 10:00:10 ERROR Database connection timeout
```

New log entries automatically appear in your terminal.

### Real-Time DevOps Scenario

You deploy a new Spring Boot application.

You want to monitor startup logs.

```bash
tail -n 100 -f /var/log/myapp/application.log
```

This displays the last 100 lines and continues monitoring new entries.

For files that may be rotated or recreated, use:

```bash
tail -F /var/log/myapp/application.log
```

`-F` follows the filename and retries if the file becomes temporarily unavailable.

### Interview Answer

"`tail -f` is used to monitor log files in real time. Whenever new log entries are added, they appear automatically. I use it during application deployments, restarts, and production troubleshooting."

---

# 9. What Is the Difference Between `>` and `>>`?

### Concept

Both operators redirect command output into a file.

The main difference is how they handle existing content.

### `>` — Overwrite

```bash
echo "Application Started" > app.log
```

Creates the file if it doesn't exist.

If it already exists, its contents are overwritten.

### `>>` — Append

```bash
echo "Database Connected" >> app.log
```

Adds content to the end of the file.

Existing content remains.

### Example

```bash
echo "Line 1" > test.txt
echo "Line 2" >> test.txt
```

Output:

```text
Line 1
Line 2
```

### Difference

| `>`                         | `>>`                           |
| --------------------------- | ------------------------------ |
| Overwrites existing content | Appends new content            |
| Creates file if missing     | Creates file if missing        |
| Useful for replacing output | Useful for accumulating output |

### Real-Time DevOps Example

```bash
date >> deployment.log
```

Adds the current date to the deployment log.

### Interview Answer

"`>` redirects output to a file and overwrites existing content, whereas `>>` appends output without deleting existing content. I use these operators in shell scripts for saving command output and maintaining logs."

---

# 10. What Is a Linux Pipe?

### Concept

A pipe connects the output of one command to the input of another command.

The pipe operator is:

```bash
|
```

### Example

```bash
ps aux | grep java
```

How it works:

```text
ps aux
   |
   | Output
   v
grep java
   |
   v
Displays matching processes
```

### Another Example

```bash
cat application.log | grep ERROR
```

This displays lines containing `ERROR`.

A simpler equivalent is:

```bash
grep ERROR application.log
```

### Real-Time DevOps Example

Find the five highest CPU-consuming processes:

```bash
ps aux --sort=-%cpu | head -n 6
```

The output includes one header line and five process entries.

### Interview Answer

"A pipe is used to pass the standard output of one command as the standard input of another command. It allows us to combine multiple Linux commands. For example, `ps aux | grep java` helps identify Java processes."

---

# 11. What Is the Difference Between `find` and `grep`?

### Concept

Both commands are used for searching, but they search different things.

### `find` — Search Files and Directories

Example:

```bash
find /opt -name "application.log"
```

Searches for a file named `application.log` under `/opt`.

### `grep` — Search Text Patterns

Example:

```bash
grep "ERROR" application.log
```

Searches for lines containing `ERROR`.

### Difference

| `find`                                          | `grep`                        |
| ----------------------------------------------- | ----------------------------- |
| Searches filesystem entries                     | Searches text patterns        |
| Finds files and directories                     | Finds matching lines          |
| Supports filename, size, time and other filters | Supports regular expressions  |
| Example: `find / -name "*.log"`                 | Example: `grep ERROR app.log` |

### Real-Time DevOps Scenario

You need to find an application log and check its errors.

Step 1:

```bash
find /opt/myapp -name "*.log"
```

Step 2:

```bash
grep "ERROR" /opt/myapp/logs/application.log
```

### Interview Answer

"`find` searches for files and directories based on criteria such as name, size, or modification time. `grep` searches for text patterns inside files. I use `find` to locate application files and `grep` to identify errors in application logs."

---

# 12. What Are Linux Wildcards?

### Concept

Wildcards are special characters used to match filenames.

They are commonly used with commands such as:

```bash
ls
cp
rm
find
```

### Common Wildcards

| Wildcard | Meaning                         | Example         |
| -------- | ------------------------------- | --------------- |
| `*`      | Matches zero or more characters | `*.log`         |
| `?`      | Matches exactly one character   | `file?.txt`     |
| `[abc]`  | Matches one specified character | `file[123].txt` |
| `[0-9]`  | Matches one digit               | `log[0-9].txt`  |

### Example 1

```bash
ls *.log
```

Lists `.log` files in the current directory.

### Example 2

```bash
ls app?.log
```

Matches:

```text
app1.log
app2.log
```

But not:

```text
app10.log
```

### Example 3

```bash
ls app[123].log
```

Matches:

```text
app1.log
app2.log
app3.log
```

### Interview Answer

"Linux wildcards are special characters used to match filenames. The most common are `*`, which matches multiple characters, `?`, which matches a single character, and square brackets, which match a specified set or range of characters."

---

# 13. What Is an Inode?

This is an important Linux interview question.

### Concept

An **inode (index node)** is a filesystem data structure that stores metadata about a file.

An inode stores information such as:

* File permissions
* File owner
* File size
* Timestamps
* Link count
* References to file data blocks

**An inode does not store the filename.**

The directory stores the mapping between a filename and its inode number.

### Example

Create a file:

```bash
touch test.txt
```

Check its inode:

```bash
ls -i test.txt
```

Output:

```text
123456 test.txt
```

Here:

```text
123456 = Inode number
```

View detailed inode information:

```bash
stat test.txt
```

### Real-Time DevOps Scenario

Sometimes a server reports:

```text
No space left on device
```

But disk usage is not full.

Why?

The filesystem may have exhausted its available inodes.

Check disk usage:

```bash
df -h
```

Check inode usage:

```bash
df -i
```

If inode usage is 100%, the filesystem may be unable to create new files.

This commonly happens when an application creates excessive numbers of small files.

### Interview Answer

"An inode is a filesystem data structure that stores file metadata such as permissions, ownership, size, timestamps, and data block references. It does not store the filename. I can check inode numbers using `ls -i` and inode utilization using `df -i`. In production, inode exhaustion can cause 'No space left on device' errors even when disk capacity is available."

---

# 14. What Is the Difference Between a Hard Link and a Symbolic Link?

### Concept

Linux supports two common types of links:

1. Hard links
2. Symbolic links (soft links)

### Hard Link

A hard link is another directory entry pointing to the same inode as the original file.

Create a hard link:

```bash
ln original.txt hardlink.txt
```

Both filenames reference the same file data.

### Symbolic Link

A symbolic link is a separate file containing a path to another file or directory.

Create a symbolic link:

```bash
ln -s original.txt softlink.txt
```

### Difference

| Hard Link                                      | Symbolic Link                        |
| ---------------------------------------------- | ------------------------------------ |
| References the same inode                      | Stores a path to another file        |
| Shares file data                               | Points to a target path              |
| Usually cannot cross filesystems               | Can cross filesystems                |
| Normally cannot link directories               | Can link directories                 |
| Remains usable if original filename is deleted | Breaks if its target path disappears |
| Created using `ln`                             | Created using `ln -s`                |

### Verify Links

```bash
ls -li
```

Example:

```text
12345 -rw-r--r-- 2 user user 100 original.txt
12345 -rw-r--r-- 2 user user 100 hardlink.txt
12346 lrwxrwxrwx 1 user user  12 softlink.txt -> original.txt
```

Notice:

* Original and hard link have the same inode.
* Symbolic link has a different inode.

### Interview Answer

"A hard link is another filename pointing to the same inode, whereas a symbolic link stores a path to another file. Hard links usually cannot cross filesystems, but symbolic links can. If the original filename is deleted, the hard link remains usable, while a symbolic link may become broken."

---

# 15. What Happens When the Original File Is Deleted but a Hard Link Exists?

### Concept

A hard link and the original filename reference the same inode.

Example:

```bash
echo "Hello Linux" > original.txt
```

Create a hard link:

```bash
ln original.txt hardlink.txt
```

Delete the original filename:

```bash
rm original.txt
```

Now:

```bash
cat hardlink.txt
```

Output:

```text
Hello Linux
```

### Why?

Because the hard link still references the same inode and file data.

The data remains accessible as long as a hard link exists or an open file reference keeps the inode alive.

### Interview Answer

"If the original file is deleted but a hard link exists, the file data remains accessible through the hard link because both filenames reference the same inode. Deleting one filename does not remove the underlying file data while other references remain."

---

# 16. What Happens When a Symbolic Link's Target Is Deleted?

### Concept

A symbolic link references a target path.

Example:

```bash
echo "Hello DevOps" > original.txt
```

Create a symbolic link:

```bash
ln -s original.txt softlink.txt
```

Delete the original:

```bash
rm original.txt
```

Now:

```bash
cat softlink.txt
```

Output:

```text
cat: softlink.txt: No such file or directory
```

The symbolic link still exists, but its target is missing.

This is called a **broken or dangling symbolic link**.

### Check Symbolic Link

```bash
ls -l softlink.txt
```

Output:

```text
softlink.txt -> original.txt
```

### Interview Answer

"If the target file of a symbolic link is deleted, the symbolic link becomes broken because it points to a path that no longer exists. The link itself remains until it is deleted."

---

# 17. How Do You Find All `.log` Files in a Directory?

### Concept

Use the `find` command.

### Command

```bash
find /var/log -type f -name "*.log"
```

Explanation:

| Option          | Meaning                          |
| --------------- | -------------------------------- |
| `/var/log`      | Search directory                 |
| `-type f`       | Search regular files             |
| `-name "*.log"` | Match filenames ending in `.log` |

### Find Logs Recursively

```bash
find /opt/myapp -type f -name "*.log"
```

### Find Logs Modified in the Last 24 Hours

```bash
find /var/log -type f -name "*.log" -mtime -1
```

### Find Logs Larger Than 100 MB

```bash
find /var/log -type f -name "*.log" -size +100M
```

### Interview Answer

"I use the `find` command to locate log files. For example, `find /var/log -type f -name '*.log'` searches recursively for regular files ending with `.log`. I can also filter files based on size or modification time."

---

# 18. How Do You Find Errors in an Application Log?

### Concept

Use `grep` to search for errors.

Suppose your application log is:

```text
/var/log/myapp/application.log
```

### Search for ERROR

```bash
grep "ERROR" /var/log/myapp/application.log
```

### Case-Insensitive Search

```bash
grep -i "error" /var/log/myapp/application.log
```

Matches:

```text
ERROR
Error
error
```

### Search With Line Numbers

```bash
grep -n "ERROR" /var/log/myapp/application.log
```

### Search With Context

```bash
grep -C 5 "ERROR" /var/log/myapp/application.log
```

Displays five lines before and after each matching line.

This is useful for understanding what happened around an error.

### Real-Time DevOps Example

You receive an alert:

```text
HTTP 500 Internal Server Error
```

You check application logs:

```bash
grep -n -C 5 "ERROR" /var/log/myapp/application.log
```

Possible output:

```text
2026-10-14 10:30:01 ERROR Database connection failed
2026-10-14 10:30:01 ERROR Connection timeout
```

Now you can investigate database connectivity.

### Interview Answer

"I use `grep` to search for errors in application logs. For example, `grep -i ERROR application.log` finds error messages regardless of case. I also use `grep -n` for line numbers and `grep -C` to view surrounding log entries for troubleshooting."

---

# 19. How Do You Count the Number of Error Entries?

### Concept

Use `grep` with `-c`.

### Command

```bash
grep -c "ERROR" application.log
```

Example output:

```text
25
```

This means 25 lines contain `ERROR`.

### Case-Insensitive Count

```bash
grep -ic "error" application.log
```

### Alternative Using Pipe

```bash
grep "ERROR" application.log | wc -l
```

Explanation:

| Command        | Purpose             |
| -------------- | ------------------- |
| `grep "ERROR"` | Find matching lines |
| `wc -l`        | Count lines         |

### Count Errors Across Multiple Log Files

```bash
grep -h "ERROR" /var/log/myapp/*.log | wc -l
```

The `-h` option prevents filename prefixes in matching output.

**Important:** These commands count matching lines, not necessarily unique incidents. A single exception may generate multiple error lines.

### Interview Answer

"I use `grep -c ERROR application.log` to count the number of lines containing errors. Alternatively, I can combine `grep` with `wc -l`. This is useful for checking whether application errors increased after a deployment."

---

# 20. How Do You Troubleshoot a Missing Application Configuration File?

This is one of the most practical questions for DevOps interviews.

## Real-Time Scenario

You deployed a Spring Boot application on Linux.

The application fails to start.

Logs show:

```text
ERROR: Configuration file not found
/etc/myapp/application.yml
```

How will you troubleshoot?

### Step 1 — Check Whether the File Exists

```bash
ls -l /etc/myapp/application.yml
```

If the file is missing:

```text
No such file or directory
```

### Step 2 — Search for the Configuration File

```bash
sudo find /etc /opt -type f -name "application.yml" 2>/dev/null
```

This searches common configuration and application directories.

### Step 3 — Check the Application Startup Configuration

If the application runs as a systemd service:

```bash
systemctl cat myapp.service
```

Check whether the application is configured to use the expected file path.

For example:

```text
ExecStart=/usr/bin/java -jar /opt/myapp/app.jar --spring.config.location=file:/etc/myapp/application.yml
```

### Step 4 — Check File Permissions

```bash
ls -l /etc/myapp/application.yml
```

Example:

```text
-rw------- 1 root root 1024 application.yml
```

If the application runs as `appuser`, it may not have permission to read the file.

Check the service user:

```bash
systemctl show myapp.service -p User
```

Also check parent-directory permissions:

```bash
namei -l /etc/myapp/application.yml
```

### Step 5 — Check for Symbolic Links

```bash
ls -l /etc/myapp/application.yml
```

If the configuration file is a symbolic link, verify that its target exists.

```bash
readlink -f /etc/myapp/application.yml
```

### Step 6 — Check Application Logs

```bash
journalctl -u myapp.service -n 100 --no-pager
```

Look for errors such as:

```text
FileNotFoundException
Permission denied
Configuration file not found
```

### Step 7 — Restore the Configuration

If the file is genuinely missing, restore it from an approved source such as:

* Version-controlled configuration
* Deployment artifact
* Configuration management system
* Verified backup

Avoid creating an arbitrary configuration file in production.

### Step 8 — Restart and Verify

After correcting the issue:

```bash
sudo systemctl restart myapp.service
```

Check service status:

```bash
systemctl status myapp.service
```

Check logs:

```bash
journalctl -u myapp.service -f
```

Finally, verify the application's health endpoint.

For example:

```bash
curl -f http://localhost:8080/actuator/health
```

This assumes Spring Boot Actuator is enabled and the endpoint is accessible.

### Interview Answer

"If an application configuration file is missing, I first verify the expected file path using `ls`. Then I use `find` to check whether the file exists in another directory.

Next, I inspect the application startup configuration and verify file permissions and ownership.

If the application runs as a systemd service, I use `systemctl cat` to check its configuration and `journalctl` to investigate startup errors.

Once I identify the issue, I restore the configuration from the approved source, restart the service if required, and verify the application health."

---

# Quick Revision — All 20 Questions

Use this table before your Linux interview.

| #  | Question                       | Key Point to Remember                                           |
| -- | ------------------------------ | --------------------------------------------------------------- |
| 1  | Linux filesystem hierarchy     | All files are organized under `/`                               |
| 2  | Absolute vs relative path      | Absolute starts from `/`; relative depends on current directory |
| 3  | `/` vs `/root`                 | Filesystem root vs root user's home                             |
| 4  | `mkdir` vs `mkdir -p`          | Create directory vs create missing parents too                  |
| 5  | `cp` vs `mv`                   | Copy vs move/rename                                             |
| 6  | `rm` vs `rmdir`                | Remove files vs empty directories                               |
| 7  | `cat`, `less`, `head`, `tail`  | Display, browse, beginning, end                                 |
| 8  | `tail -f`                      | Monitor appended log entries                                    |
| 9  | `>` vs `>>`                    | Overwrite vs append                                             |
| 10 | Linux pipe                     | Connect command output to another command's input               |
| 11 | `find` vs `grep`               | Search files vs search text                                     |
| 12 | Wildcards                      | Filename matching using `*`, `?`, `[]`                          |
| 13 | Inode                          | Stores file metadata and data references                        |
| 14 | Hard vs symbolic link          | Same inode vs target path                                       |
| 15 | Delete original with hard link | Data remains accessible                                         |
| 16 | Delete symbolic link target    | Link becomes broken                                             |
| 17 | Find `.log` files              | `find /var/log -type f -name "*.log"`                           |
| 18 | Find errors in logs            | `grep -i ERROR app.log`                                         |
| 19 | Count error entries            | `grep -c ERROR app.log`                                         |
| 20 | Missing configuration file     | Check path, permissions, service configuration and logs         |

## Important for DevOps Interviews

For a DevOps Engineer with around five years of experience, interviewers often extend basic Linux questions into practical scenarios.

For example:

**Scenario 1:** A Linux server reports "No space left on device," but `df -h` shows available space. What will you check?

**Answer:** I would check inode utilization using `df -i`, because the filesystem may have exhausted its available inodes.

**Scenario 2:** Your Spring Boot application stopped working after a configuration deployment. How would you investigate?

**Answer:** I would check the configuration file, verify permissions, inspect the systemd service configuration, and review logs using `journalctl`.

**Scenario 3:** Your application log is 10 GB. How would you find errors without opening the entire file?

**Answer:** I would use `grep`, `tail`, and `less` to search and inspect the relevant log entries.

**Scenario 4:** A symbolic link exists, but accessing the file returns "No such file or directory." Why?

**Answer:** The symbolic link may be broken because its target file was deleted or moved.

**Scenario 5:** You need to identify all log files larger than 500 MB. Which command would you use?

```bash
find /var/log -type f -name "*.log" -size +500M
```

**My recommendation:** Practice these 20 questions directly on your Linux machine. For interview preparation, focus especially on **inodes, hard links vs symbolic links, `find`, `grep`, log analysis, and missing configuration troubleshooting**. These topics connect basic Linux knowledge with real production DevOps responsibilities.
