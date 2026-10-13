# Phase 01 — Linux Fundamentals Assignments

## Linux for DevOps — Hands-On Practice

Operating System: CentOS Stream 9

Objective:

Practice Linux fundamentals and understand how to
inspect and troubleshoot basic system resources.

---

# Assignment 1 — Linux System Identification

## Task

Identify the Linux user, hostname, operating system,
kernel release, and system architecture.

### Commands

```bash
whoami
id
hostname
hostnamectl
cat /etc/os-release
uname -r
uname -m
```

### Questions

1. What is the current username?
2. What is the hostname?
3. Which Linux distribution is installed?
4. What is the kernel release?
5. What is the system architecture?
6. What is the current user's UID?
7. What is the difference between UID and GID?

### My Observations

```text
Username       : sumanth
Hostname       : localhost.localdomain
Operating System: CentOS Stream 9
Kernel Release : <Enter your output>
Architecture   : <Enter your output>
UID            : <Enter your output>
GID            : <Enter your output>
```

### Learning Outcome

- Identified the current Linux user
- Identified operating system information
- Understood kernel release
- Understood Linux system architecture
- Learned basic user and group identification

---

# Assignment 2 — CPU Information

## Task

Inspect CPU architecture, logical processors, and system load.

### Commands

```bash
lscpu
nproc
uptime
```

### Questions

1. How many processing units are available?
2. What is the CPU architecture?
3. What is the CPU model?
4. What does load average represent?
5. What do the three load average values mean?
6. Is load average the same as CPU utilization?
7. Can a server have high load without high CPU utilization?

### My Observations

```text
CPU Architecture : <Enter output>
CPU Model        : <Enter output>
nproc Result     : <Enter output>
System Uptime    : <Enter output>

Load Average:

1 Minute  : <Enter output>
5 Minutes : <Enter output>
15 Minutes: <Enter output>
```

### Learning Outcome

- Understood CPU architecture
- Identified available processors
- Learned the meaning of load average
- Understood the difference between load and CPU usage

---

# Assignment 3 — Linux Memory

## Task

Inspect RAM, memory availability, and swap.

### Commands

```bash
free -h
cat /proc/meminfo
swapon --show
```

### Questions

1. What is total RAM?
2. What is used RAM?
3. What is free RAM?
4. What is available RAM?
5. What is the difference between free and available memory?
6. What is swap?
7. Why does Linux use memory for caching?
8. What can happen when the system experiences severe memory pressure?

### Initial Lab Observations

```text
Total RAM      : 1.7 GiB
Used RAM       : 913 MiB
Free RAM       : 451 MiB
Available RAM  : 793 MiB
Swap Total     : 2.0 GiB
Swap Used      : 0 B
```

Note: These values are from the initial system inspection.
Memory usage changes while the system runs.

### Learning Outcome

- Understood Linux RAM statistics
- Learned about available memory
- Understood Linux caching
- Learned the purpose of swap
- Understood basic memory troubleshooting concepts

---

# Assignment 4 — Disk Utilization

## Task

Inspect filesystem capacity and block devices.

### Commands

```bash
df -h
df -Th
du -sh /home/sumanth
lsblk
```

### Questions

1. What is the root filesystem?
2. What is the root filesystem capacity?
3. How much space is used?
4. How much space is available?
5. What does the mount point `/` represent?
6. What is the difference between df and du?
7. What does lsblk display?
8. What is a filesystem?
9. What is a mount point?

### Initial Lab Observations

```text
Root Filesystem : /dev/mapper/cs-root
Filesystem Size : 17G
Used Space      : 4.9G
Available Space : 13G
Disk Usage      : 29%
Mount Point     : /
```

### Learning Outcome

- Understood filesystem capacity
- Learned how to inspect disk usage
- Understood mount points
- Identified Linux block devices
- Learned the difference between df and du

---

# Assignment 5 — Linux Filesystem Exploration

## Task

Navigate important Linux directories.

### Commands

```bash
cd /
pwd
ls -lah

cd /etc
pwd

cd /var
pwd

cd /home
pwd

cd ~
pwd
```

### Questions

1. What does `/` represent?
2. What is the purpose of `/etc`?
3. What is stored in `/var`?
4. What is the purpose of `/home`?
5. What is the difference between `/` and `/root`?
6. What does `~` represent?
7. What does `cd ..` do?
8. What does `cd -` do?
9. What is a hidden file?
10. What does `ls -lah` display?

### Learning Outcome

- Navigated the Linux filesystem
- Understood important directories
- Learned basic navigation commands
- Understood hidden files
- Practiced filesystem exploration

---

# Assignment 6 — Linux Help Commands

## Task

Learn how to access Linux command documentation.

### Commands

```bash
ls --help
man ls
type cd
type -P ls
```

### Questions

1. What is the purpose of man?
2. What does --help do?
3. What is a shell built-in command?
4. What does type do?
5. How do you exit a manual page?

### Learning Outcome

- Learned to use Linux documentation
- Understood command help
- Identified shell built-ins
- Learned how to find executable paths

---

# Assignment 7 — Real-World DevOps Scenario

## Scenario: Application Responding Slowly

A production application is experiencing high latency.

Application:

```text
Spring Boot
```

Server:

```text
prod-app-01
```

### Task

Perform an initial Linux system health check.

### Commands

```bash
hostname
uptime
nproc
free -h
df -h
cat /etc/os-release
```

### Questions

1. How do you confirm the correct server?
2. How do you check system uptime?
3. How do you inspect system load?
4. How do you check memory availability?
5. How do you check disk utilization?
6. Can these commands identify the complete root cause of latency?
7. What additional information might be required?

### Expected Approach

Step 1:

Verify the hostname.

Step 2:

Check uptime and load average.

Step 3:

Check available CPU resources.

Step 4:

Check memory availability and swap.

Step 5:

Check filesystem utilization.

Step 6:

Determine whether additional investigation is needed.

### Important Note

These commands are preliminary health checks.

They do not prove the root cause of application latency.

Further investigation may require:

- Process monitoring
- Application logs
- Network diagnostics
- Database monitoring
- Application performance metrics

---

# Assignment 8 — Linux Interview Questions

Answer these questions in your own words.

## Linux Fundamentals

1. What is Linux?
2. What is the Linux kernel?
3. What is a shell?
4. What is the difference between a kernel and a shell?
5. What is a Linux distribution?
6. What is the difference between CentOS and Ubuntu?

## Linux Users

7. What is the root user?
8. What is UID?
9. What is GID?
10. What is sudo?

## System Information

11. How do you check the Linux kernel version?
12. How do you identify the Linux distribution?
13. How do you check the hostname?
14. How do you check CPU architecture?

## CPU and Memory

15. What is load average?
16. What is the difference between CPU usage and load average?
17. What is swap?
18. What is the difference between free and available memory?
19. What is the Linux OOM killer?

## Filesystem

20. What is the Linux root directory?
21. What is the purpose of /etc?
22. What is the purpose of /var?
23. What is the difference between /root and /home?
24. What is a mount point?
25. What is the difference between df and du?

---

# Assignment Completion Checklist

| Assignment | Status |
|------------|--------|
| System Identification | Completed |
| CPU Information | Completed |
| Memory Inspection | Completed |
| Disk Inspection | Completed |
| Filesystem Exploration | Completed |
| Linux Help Commands | Completed |
| DevOps Scenario | Completed |
| Interview Questions | Completed |

Update the status according to the tasks actually completed.

---

# Phase 01 — Final Learning Summary

Topics covered:

- Linux architecture
- Linux kernel and shell
- Linux distributions
- Linux users and groups
- System identification
- CPU architecture
- Load average
- Memory management
- Swap
- Disk utilization
- Filesystem hierarchy
- Linux command documentation
- Basic production troubleshooting

## Next Phase

Phase 02 — Linux Filesystem & Navigation

Topics:

- Absolute and relative paths
- File and directory operations
- Hidden files
- File metadata
- File searching
- Filesystem navigation
- Practical Linux administration labs