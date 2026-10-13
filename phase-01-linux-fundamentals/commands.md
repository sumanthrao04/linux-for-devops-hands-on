# Phase 01 — Linux Commands Reference

## Linux for DevOps

This document contains the Linux commands practiced during
Phase 01 — Linux Fundamentals & System Exploration.

Environment: CentOS Stream 9

---

# 1. User Information Commands

## whoami

Displays the current effective username.

```bash
whoami
```

Example:

```text
sumanth
```

DevOps use case:

Identify which user is executing a deployment command.

---

## id

Displays user ID, group ID, and group memberships.

```bash
id
```

Check another user:

```bash
id root
```

Useful for investigating Linux permissions.

---

## sudo

Executes a command with another user's privileges,
normally root, when authorized.

```bash
sudo whoami
```

Expected output when authorized:

```text
root
```

---

# 2. System Identification Commands

## hostname

Displays the machine hostname.

```bash
hostname
```

Example:

```text
localhost.localdomain
```

---

## hostnamectl

Displays system hostname and operating system information.

```bash
hostnamectl
```

---

## cat /etc/os-release

Displays Linux distribution information.

```bash
cat /etc/os-release
```

Useful when identifying Linux distribution and version.

---

## uname -r

Displays the Linux kernel release.

```bash
uname -r
```

---

## uname -a

Displays detailed kernel and system information.

```bash
uname -a
```

---

## uname -m

Displays machine hardware architecture.

```bash
uname -m
```

Example:

```text
x86_64
```

---

# 3. CPU Commands

## lscpu

Displays CPU architecture and processor information.

```bash
lscpu
```

Important fields:

- Architecture
- CPU(s)
- Model name
- Core(s) per socket
- Thread(s) per core

---

## nproc

Displays the number of processing units available
to the current process.

```bash
nproc
```

---

## uptime

Displays:

- Current time
- System uptime
- Logged-in user count
- Load average

```bash
uptime
```

Example:

```text
load average: 0.01, 0.19, 0.13
```

The values represent:

1 minute, 5 minutes, and 15 minutes.

---

# 4. Memory Commands

## free -h

Displays RAM and swap usage.

```bash
free -h
```

Important fields:

- total
- used
- free
- buff/cache
- available

---

## cat /proc/meminfo

Displays detailed kernel memory statistics.

```bash
cat /proc/meminfo
```

---

## swapon --show

Displays active swap devices or files.

```bash
swapon --show
```

---

# 5. Disk Commands

## df -h

Displays filesystem space usage.

```bash
df -h
```

Important columns:

- Filesystem
- Size
- Used
- Avail
- Use%
- Mounted on

---

## df -Th

Displays filesystem usage and filesystem types.

```bash
df -Th
```

---

## du -sh

Displays summarized directory disk usage.

```bash
du -sh /home/sumanth
```

Options:

-s = summary

-h = human-readable

---

## lsblk

Displays block devices.

```bash
lsblk
```

Useful for identifying:

- Disks
- Partitions
- Logical volumes
- Mount points

---

# 6. Filesystem Navigation Commands

## pwd

Displays the current working directory.

```bash
pwd
```

---

## ls

Lists directory contents.

```bash
ls
```

---

## ls -l

Displays detailed directory listing.

```bash
ls -l
```

---

## ls -a

Displays hidden files.

```bash
ls -a
```

---

## ls -lah

Displays detailed information including hidden files
and human-readable sizes.

```bash
ls -lah
```

Options:

-l = long listing

-a = show hidden files

-h = human-readable sizes

---

## cd /

Moves to the filesystem root.

```bash
cd /
```

---

## cd ~

Moves to the current user's home directory.

```bash
cd ~
```

---

## cd ..

Moves to the parent directory.

```bash
cd ..
```

---

## cd -

Returns to the previous working directory.

```bash
cd -
```

---

# 7. Linux Help Commands

## --help

Displays command usage information.

```bash
ls --help
```

---

## man

Opens the command manual.

```bash
man ls
```

Navigation:

Space = next page

q = quit

/word = search

---

## type

Identifies how a command is interpreted by the shell.

```bash
type cd
```

---

## type -P

Displays the executable path found through PATH.

```bash
type -P ls
```

---

# 8. Quick Command Reference

| Command | Purpose |
|---------|---------|
| whoami | Identify current user |
| id | Display user and group IDs |
| sudo | Execute commands with authorized elevated privileges |
| hostname | Display hostname |
| hostnamectl | Display system details |
| cat /etc/os-release | Check OS distribution |
| uname -r | Check kernel release |
| uname -a | Display detailed kernel information |
| uname -m | Check architecture |
| lscpu | Display CPU details |
| nproc | Check available processing units |
| uptime | Check uptime and load average |
| free -h | Check RAM and swap |
| swapon --show | Check active swap |
| df -h | Check filesystem capacity |
| df -Th | Check filesystem types |
| du -sh | Check directory size |
| lsblk | Display block devices |
| pwd | Display current directory |
| ls | List files |
| ls -lah | Detailed listing |
| cd | Change directory |
| man | Open manual pages |
| type | Identify command type |

---

# 9. Production Troubleshooting Reference

## Scenario 1 — High CPU or Load Alert

Initial commands:

```bash
hostname
uptime
nproc
lscpu
```

Further process investigation will be covered in later phases.

---

## Scenario 2 — Memory Alert

Initial commands:

```bash
free -h
swapon --show
cat /proc/meminfo
```

---

## Scenario 3 — Disk Usage Alert

Initial commands:

```bash
df -h
df -Th
lsblk
```

Check directory size:

```bash
du -sh /home/sumanth
```

---

## Scenario 4 — Identify Linux Server

```bash
whoami
hostname
hostnamectl
cat /etc/os-release
uname -r
```

---

# 10. Key Learning

These commands are useful for:

- Initial server health checks
- System identification
- Infrastructure troubleshooting
- Monitoring verification
- Understanding Linux resource utilization
- Preparing for Linux administration tasks

Next Phase:

Linux Filesystem, Navigation, and File Management.