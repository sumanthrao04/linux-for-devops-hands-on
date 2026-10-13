# 🐧 Phase 01 — Linux Fundamentals & System Exploration

## Linux for DevOps — Hands-On Learning

This phase focuses on understanding Linux fundamentals, system architecture,
operating system information, CPU, memory, disk utilization, and filesystem
organization.

The objective is to build a strong Linux foundation for DevOps, Cloud
Engineering, System Administration, and Production Troubleshooting.

---

## 1. Lab Environment

| Component | Details |
|-----------|---------|
| Operating System | CentOS Stream 9 |
| Platform | Local Linux Machine |
| Shell | Bash |
| User | sumanth |
| Hostname | localhost.localdomain |
| Learning Level | Beginner |
| Focus | Linux Administration & DevOps |

---

## 2. Learning Objectives

By completing this phase, I learned:

- Linux architecture and its components
- Linux kernel and shell
- Linux distributions
- Linux users and groups
- Linux system identification commands
- CPU architecture and logical processors
- Linux load average
- Memory management
- Linux swap memory
- Disk utilization
- Linux filesystem hierarchy
- Basic system monitoring commands
- Linux command documentation

---

# 3. Linux Architecture

Linux is an open-source operating system kernel.

A Linux distribution combines the kernel with system utilities,
libraries, package managers, and other software.

### Architecture

```text
             USER
               |
               v
        APPLICATIONS
     Java | Nginx | Docker
               |
               v
        SHELL / UTILITIES
         Bash | Commands
               |
               v
          LINUX KERNEL
       Process Management
       Memory Management
       Filesystem Management
       Network Management
               |
               v
            HARDWARE
        CPU | RAM | Disk
```

### Components

| Component | Description |
|-----------|-------------|
| Hardware | Physical resources such as CPU, RAM, disk, and network devices |
| Kernel | Manages hardware resources and provides operating system services |
| Shell | Interprets user commands |
| Applications | Software running on Linux |
| System Utilities | Programs used to manage and inspect the operating system |

### DevOps Relevance

Linux is commonly used to host:

- Java and Spring Boot applications
- Nginx web servers
- Jenkins automation servers
- Docker containers
- Kubernetes worker nodes
- Monitoring agents
- Cloud workloads

Understanding Linux helps DevOps engineers troubleshoot infrastructure
and application issues.

---

# 4. Linux Distributions

A Linux distribution provides a complete operating system built around
the Linux kernel.

| Distribution | Common Usage |
|--------------|--------------|
| Ubuntu | Cloud servers, development environments |
| Debian | Servers and infrastructure |
| RHEL | Enterprise production environments |
| CentOS Stream | Development platform for upcoming RHEL releases |
| Rocky Linux | RHEL-compatible server environments |
| Amazon Linux | AWS workloads |

### Distribution Used

```bash
cat /etc/os-release
```

Operating system:

```text
CentOS Stream 9
```

### Package Management

CentOS Stream uses DNF for package management.

Example:

```bash
sudo dnf install nginx
```

Ubuntu uses APT:

```bash
sudo apt install nginx
```

---

# 5. Linux Kernel and Shell

## Kernel

The Linux kernel is responsible for:

- Process scheduling
- Memory management
- Device management
- Filesystem operations
- Networking
- Resource allocation

Check kernel version:

```bash
uname -r
```

Check system architecture:

```bash
uname -m
```

## Shell

A shell allows users to interact with the operating system.

Common shells:

- Bash
- Zsh
- Fish
- Sh

Check the configured login shell:

```bash
echo $SHELL
```

### Kernel vs Shell

| Kernel | Shell |
|--------|-------|
| Manages system resources | Interprets commands |
| Communicates with hardware | Provides a command interface |
| Handles processes and memory | Runs commands and scripts |
| Core operating system component | User-space program |

---

# 6. Linux User Identification

Linux supports multiple users.

### Identify Current User

```bash
whoami
```

Output:

```text
sumanth
```

### Check User and Group IDs

```bash
id
```

### Check Root User

```bash
id root
```

The root user has UID 0.

### Root vs Normal User

| Root User | Normal User |
|-----------|-------------|
| Has administrative privileges | Has limited privileges |
| UID 0 | Usually has a nonzero UID |
| Can manage system resources | Access depends on permissions |
| Used for administrative operations | Used for everyday tasks |

### DevOps Use Case

If Jenkins cannot write to a deployment directory,
verify the user running the operation.

```bash
whoami
id
```

The issue may be related to ownership or permissions.

---

# 7. Linux System Information

### Check Hostname

```bash
hostname
```

Output:

```text
localhost.localdomain
```

### Detailed Host Information

```bash
hostnamectl
```

### Operating System

```bash
cat /etc/os-release
```

### Kernel Version

```bash
uname -r
```

### Complete Kernel Information

```bash
uname -a
```

### System Architecture

```bash
uname -m
```

### DevOps Use Case

Before deploying software, verify the operating system
and architecture to select compatible packages.

---

# 8. CPU Information

CPU information can be inspected using Linux commands.

### Display CPU Details

```bash
lscpu
```

### Check Available Processing Units

```bash
nproc
```

### Check System Uptime and Load

```bash
uptime
```

Example:

```text
up 6 min, 1 user, load average: 0.01, 0.19, 0.13
```

### Understanding Load Average

The three load average values represent:

| Value | Period |
|-------|--------|
| First | 1 minute |
| Second | 5 minutes |
| Third | 15 minutes |

Load average measures the average number of tasks
that are runnable or waiting in uninterruptible sleep.

It is not the same as CPU utilization percentage.

### Production Relevance

High system load may indicate:

- CPU contention
- Heavy processing
- I/O bottlenecks
- Tasks waiting for resources

Additional investigation is required to identify the cause.

---

# 9. Memory Management

Linux manages physical memory and swap.

### Check Memory

```bash
free -h
```

Lab observation:

```text
Total RAM      : 1.7 GiB
Used RAM       : 913 MiB
Free RAM       : 451 MiB
Available RAM  : 793 MiB
Swap           : 2.0 GiB
```

These values were captured during the initial system inspection
and may change over time.

### Important Memory Columns

| Column | Description |
|--------|-------------|
| total | Total usable RAM |
| used | Memory currently used |
| free | Completely unused memory |
| shared | Shared memory usage |
| buff/cache | Memory used for buffers and caches |
| available | Estimated memory available for new applications |

### Important Concept

Free memory and available memory are different.

Linux uses memory for caching to improve performance.

Available memory is often more useful when evaluating
whether the system is under memory pressure.

### Detailed Memory Information

```bash
cat /proc/meminfo
```

---

# 10. Linux Swap

Swap provides disk-backed memory space.

It allows Linux to move eligible memory pages
out of physical RAM when appropriate.

### Check Swap

```bash
swapon --show
```

### Check RAM and Swap Together

```bash
free -h
```

Lab observation:

```text
Swap Total : 2.0 GiB
Swap Used  : 0 B
```

### DevOps Relevance

Excessive swapping may contribute to application performance problems.

During memory troubleshooting, investigate:

- Available RAM
- Swap usage
- Memory-consuming processes
- Application memory requirements
- OOM events

---

# 11. Linux Disk Utilization

Disk monitoring is an important Linux administration task.

### Check Filesystem Usage

```bash
df -h
```

Lab observation:

```text
Filesystem             Size   Used   Avail   Use%
/dev/mapper/cs-root     17G   4.9G    13G    29%
```

### Check Directory Size

```bash
du -sh /home/sumanth
```

### Check Block Devices

```bash
lsblk
```

### Check Filesystem Types

```bash
df -Th
```

### df vs du

| Command | Purpose |
|---------|---------|
| df | Displays filesystem capacity and available space |
| du | Displays disk usage of files and directories |
| lsblk | Displays block devices and their relationships |

### Production Scenario

Alert:

```text
Server: prod-app-01
Disk Usage: 95%
```

Initial investigation:

```bash
df -h
```

Identify large directories:

```bash
sudo du -xhd1 /var
```

Further investigation should identify the actual source of disk usage
before deleting or modifying files.

---

# 12. Linux Filesystem Hierarchy

Linux uses a single filesystem hierarchy starting at `/`.

### Filesystem Structure

```text
/
├── boot
├── dev
├── etc
├── home
├── opt
├── proc
├── root
├── run
├── sys
├── tmp
├── usr
└── var
```

### Important Directories

| Directory | Purpose |
|-----------|---------|
| / | Root of the filesystem hierarchy |
| /boot | Boot-related files |
| /dev | Device interfaces |
| /etc | System configuration |
| /home | Normal user home directories |
| /opt | Optional software |
| /proc | Process and kernel information |
| /root | Root user's home directory |
| /run | Runtime information |
| /sys | Kernel device and system information |
| /tmp | Temporary files |
| /usr | Programs, libraries, and shared resources |
| /var | Variable system and application data |

### Navigation Commands

```bash
cd /
pwd
ls
```

Navigate to configuration directory:

```bash
cd /etc
pwd
```

Navigate to variable data directory:

```bash
cd /var
pwd
```

Return to home directory:

```bash
cd ~
pwd
```

---

# 13. Linux Help and Documentation

Linux provides built-in documentation.

### Command Help

```bash
ls --help
```

### Manual Pages

```bash
man ls
```

### Identify Command Type

```bash
type cd
```

### Find Executable Path

```bash
type -P ls
```

These commands help Linux administrators understand
commands without relying entirely on external documentation.

---

# 14. Real-World DevOps Scenario

## Scenario: Application Responding Slowly

A production Spring Boot application is experiencing high response times.

Server:

```text
prod-app-01
```

### Initial Investigation

Step 1 — Identify the server:

```bash
hostname
```

Step 2 — Check system uptime and load:

```bash
uptime
```

Step 3 — Check CPU information:

```bash
nproc
```

Step 4 — Check memory:

```bash
free -h
```

Step 5 — Check disk capacity:

```bash
df -h
```

Step 6 — Check operating system:

```bash
cat /etc/os-release
```

### Learning

These commands provide an initial understanding of system health.

They do not independently identify the root cause of application latency.

Further troubleshooting may require process inspection,
application logs, network diagnostics, and application metrics.

---

# 15. Hands-On Lab Screenshots

## Linux System Information

Commands:

```bash
whoami
id
hostnamectl
cat /etc/os-release
uname -r
```



---

## CPU Information

Commands:

```bash
lscpu
nproc
uptime
```


---

## Memory Information

Commands:

```bash
free -h
swapon --show
```


---

## Disk Information

Commands:

```bash
df -h
df -Th
lsblk
```



---

## Linux Filesystem Exploration

Commands:

```bash
cd /
pwd
ls -lah
cd /etc
cd /var
cd ~
```


---

# 16. Skills Developed

After completing this phase, I gained foundational knowledge of:

- Linux architecture
- Linux system administration fundamentals
- Linux user identification
- Operating system and kernel identification
- CPU and load average interpretation
- Linux memory management
- Swap concepts
- Disk utilization monitoring
- Filesystem hierarchy
- Linux navigation
- Basic system troubleshooting

---

# 17. Next Phase

## Phase 02 — Linux Filesystem & Navigation

Topics:

- Absolute and relative paths
- Linux filesystem hierarchy
- Directory navigation
- Hidden files
- File and directory creation
- File copying and moving
- File deletion
- File metadata
- File searching

The next phase will include hands-on file management
and practical DevOps filesystem scenarios.