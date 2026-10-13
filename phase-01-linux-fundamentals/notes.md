## Linux Load Average — Simple Interview Answer

**Q: What is Load Average in Linux?**

**Answer:**

Linux Load Average tells us how many processes are running, waiting for CPU time, or waiting for I/O operations on average.

We can check Load Average using:

```bash
uptime
```

**Example output:**

```text
load average: 2.0, 4.0, 6.0
```

* **2.0** → Average load over 1 minute
* **4.0** → Average load over 5 minutes
* **6.0** → Average load over 15 minutes

### How to Calculate Load Average?

First, check the number of CPUs:

```bash
nproc
```

Suppose the server has **4 CPU cores** and Load Average is **6**.

**Formula:**

```text
Normalized Load = (Load Average / CPU Cores) × 100
```

**Calculation:**

```text
(6 / 4) × 100 = 150%
```

This means the load is 1.5 times the number of available CPUs.

If the workload is CPU-intensive, this indicates CPU contention.

### How to Identify High Load?

For a server with 4 CPU cores:

| Load Average | Meaning                        |
| ------------ | ------------------------------ |
| 1            | Low load                       |
| 2            | Moderate load                  |
| 4            | At CPU capacity (if CPU-bound) |
| 6            | High CPU demand                |
| 8            | Very high CPU demand           |

### How to Troubleshoot High Load Average?

I would use these commands:

| Command               | Purpose                               |
| --------------------- | ------------------------------------- |
| `uptime`              | Check Load Average                    |
| `nproc`               | Check CPU count                       |
| `top`                 | Check CPU utilization                 |
| `ps aux --sort=-%cpu` | Find CPU-consuming processes          |
| `vmstat 1 5`          | Check CPU queue and blocked processes |
| `iostat -xz 1 5`      | Check disk I/O performance            |

### Important Interview Question

**Q: Can Load Average be high while CPU utilization is low?**

**Answer:** Yes. If processes are waiting for disk I/O, Load Average can be high even when CPU utilization is low.

---

### ⭐ Final Interview Answer (30–45 Seconds)

"Linux Load Average represents the average number of processes running, waiting for CPU, or waiting in uninterruptible sleep, usually due to I/O.

We can check it using the `uptime` command, which displays the average load for 1, 5, and 15 minutes.

For example, if a server has 4 CPU cores and Load Average is 6, the normalized load is 150%, meaning the load is higher than the available CPU count.

However, high Load Average doesn't always mean high CPU utilization because processes waiting for I/O also contribute to the load.

To troubleshoot, I use `top`, `vmstat`, and `iostat` to identify whether the bottleneck is CPU or disk I/O."

**Remember:** Load Average is not CPU utilization percentage.


----------------------------------------------------------------------

# Linux Performance Troubleshooting — 6 Important Commands for DevOps Engineers

As a DevOps Engineer with around 5 years of experience, you should know not only what these commands do, but also **how to interpret their output and use them to troubleshoot production issues**.

We'll understand all six commands using one real-world scenario.

## Real-Time Production Scenario

Imagine you are managing a Linux server running a Java Spring Boot application.

Users are reporting that the application is responding slowly.

Your server configuration:

```text
Server Name : prod-app-01
Operating System : Ubuntu Linux
CPU           : 4 vCPUs
Memory        : 8 GB
Application   : Java Spring Boot
```

Your task is to determine why the application is slow.

We'll investigate using these commands:

| Command               | Purpose                                |
| --------------------- | -------------------------------------- |
| `uptime`              | Check system uptime and load average   |
| `nproc`               | Check available logical CPUs           |
| `top`                 | Monitor CPU, memory and processes      |
| `ps aux --sort=-%cpu` | Identify CPU-consuming processes       |
| `vmstat 1 5`          | Analyze CPU scheduling, memory and I/O |
| `iostat -xz 1 5`      | Analyze disk I/O performance           |

---

# 1. `uptime` — Check System Load

### What does the `uptime` command do?

The `uptime` command displays:

1. Current system time
2. How long the server has been running
3. Number of logged-in users
4. Linux load averages

### Command

```bash
uptime
```

### Example Output

```text
15:30:10 up 25 days, 4:20, 2 users, load average: 8.50, 6.20, 3.10
```

### Understand Each Value

| Output             | Meaning                                                    |
| ------------------ | ---------------------------------------------------------- |
| `15:30:10`         | Current server time                                        |
| `up 25 days, 4:20` | Server has been running for 25 days and 4 hours 20 minutes |
| `2 users`          | Two logged-in user sessions                                |
| `8.50`             | 1-minute load average                                      |
| `6.20`             | 5-minute load average                                      |
| `3.10`             | 15-minute load average                                     |

### How Do You Analyze This?

Observe:

```text
1-minute load  = 8.50
5-minute load  = 6.20
15-minute load = 3.10
```

The short-term load is higher than the longer-term load.

This suggests that the server load has recently increased.

But is a load average of 8.50 high?

**We cannot determine that without checking the CPU count and the cause of the load.**

For example:

```text
Server A → 4 CPUs  → Load 8.50
Server B → 32 CPUs → Load 8.50
```

If the workload is CPU-bound, Server A is likely experiencing CPU contention, whereas Server B may have spare CPU capacity.

### Interview Explanation

"The `uptime` command helps me check server uptime and load averages over 1, 5 and 15 minutes. If the 1-minute load is higher than the 5-minute and 15-minute averages, it suggests the load has recently increased. I then compare it with the CPU count and investigate whether the load is caused by CPU demand or blocked processes."

---

# 2. `nproc` — Check CPU Count

### What does `nproc` do?

The `nproc` command displays the number of processing units available to the current process.

### Command

```bash
nproc
```

### Example Output

```text
4
```

This means your process has access to four logical CPUs.

### What Is a Logical CPU?

A physical CPU can contain multiple cores.

Some CPUs also support simultaneous multithreading (SMT), commonly called Hyper-Threading on Intel systems.

Example:

```text
Physical CPU
    |
    ├── Core 1
    |     ├── Thread 1
    |     └── Thread 2
    |
    ├── Core 2
    |     ├── Thread 1
    |     └── Thread 2
    |
    ├── Core 3
    |     ├── Thread 1
    |     └── Thread 2
    |
    └── Core 4
          ├── Thread 1
          └── Thread 2
```

In this example:

```text
Physical CPUs = 1
Physical cores = 4
Logical CPUs = 8
```

The operating system can schedule tasks across eight logical CPUs.

### Additional Command

```bash
lscpu
```

Example output:

```text
Architecture:        x86_64
CPU(s):              4
Thread(s) per core:  2
Core(s) per socket:  2
Socket(s):           1
```

Explanation:

| Field              | Meaning                       |
| ------------------ | ----------------------------- |
| Architecture       | CPU architecture              |
| CPU(s)             | Logical CPUs                  |
| Thread(s) per core | Hardware threads per core     |
| Core(s) per socket | Physical cores per CPU socket |
| Socket(s)          | Physical CPU sockets          |

### Real-Time Example

Suppose:

```text
CPU Count = 4
Load Average = 8
```

Normalized load:

$$
\frac{8}{4}\times100=200\%
$$

This means the load is twice the available logical CPU count.

If the load comes from CPU-bound tasks, the server likely has processes waiting for CPU time.

However, if many processes are blocked on I/O, CPU utilization may still be low.

### Interview Explanation

"I use `nproc` to check the available logical CPU count. I compare it with the load average to understand whether the server may be overloaded. I also use `lscpu` to check CPU architecture, cores, sockets and threads."

---

# 3. `top` — Monitor CPU, Memory and Processes

This is one of the most important Linux troubleshooting commands.

### What does `top` do?

The `top` command provides real-time information about:

* CPU utilization
* Memory utilization
* Load average
* Running processes
* CPU-consuming processes
* Memory-consuming processes

### Command

```bash
top
```

### Example Output

```text
top - 15:45:10 up 25 days, 2 users, load average: 8.50, 6.20, 3.10

Tasks: 250 total, 3 running, 247 sleeping, 0 stopped, 0 zombie

%Cpu(s): 85.0 us, 5.0 sy, 0.0 ni, 5.0 id, 5.0 wa, 0.0 hi, 0.0 si, 0.0 st

MiB Mem : 8192 total, 500 free, 6000 used, 1692 buff/cache

MiB Swap: 2048 total, 2048 free, 0 used

PID    USER     %CPU    %MEM    COMMAND
1234   appuser  180.0   25.0    java
1456   root      40.0    5.0    python
1678   postgres  15.0   10.0    postgres
```

Let's understand each section.

## Section A — Load Average

```text
load average: 8.50, 6.20, 3.10
```

This represents the 1-minute, 5-minute and 15-minute load averages.

## Section B — Tasks

```text
Tasks: 250 total, 3 running, 247 sleeping
```

| Value        | Meaning                             |
| ------------ | ----------------------------------- |
| 250 total    | Total tasks shown                   |
| 3 running    | Tasks currently running or runnable |
| 247 sleeping | Tasks waiting for an event          |
| 0 stopped    | No stopped tasks                    |
| 0 zombie     | No zombie processes                 |

A zombie process has finished execution but its parent has not yet collected its exit status.

## Section C — CPU Utilization

```text
%Cpu(s): 85.0 us, 5.0 sy, 0.0 ni, 5.0 id, 5.0 wa
```

These metrics are particularly important.

| Metric | Full Name          | Meaning                                                 |
| ------ | ------------------ | ------------------------------------------------------- |
| `us`   | User               | CPU time spent executing user-space code                |
| `sy`   | System             | CPU time spent executing kernel code                    |
| `ni`   | Nice               | CPU time spent executing niced user processes           |
| `id`   | Idle               | CPU idle time                                           |
| `wa`   | I/O Wait           | CPU idle time while I/O is outstanding                  |
| `hi`   | Hardware Interrupt | CPU time processing hardware interrupts                 |
| `si`   | Software Interrupt | CPU time processing software interrupts                 |
| `st`   | Steal Time         | CPU time taken by the hypervisor from a virtual machine |

### Scenario 1 — High User CPU

```text
us = 90%
sy = 5%
id = 5%
```

This indicates high CPU consumption by user-space applications.

Possible causes:

* Java application consuming CPU
* Infinite loop
* Excessive application processing
* Too many concurrent requests

Next command:

```bash
ps aux --sort=-%cpu | head -10
```

### Scenario 2 — High I/O Wait

```text
us = 10%
sy = 5%
id = 15%
wa = 70%
```

This indicates substantial CPU idle time associated with outstanding I/O.

Possible causes:

* Slow disk
* Heavy database I/O
* Storage latency
* Excessive file operations

Next command:

```bash
iostat -xz 1 5
```

### Scenario 3 — High Steal Time

```text
st = 25%
```

This can occur in virtualized environments.

It indicates that the virtual CPU was ready to run but the hypervisor was using the physical CPU for other work.

Possible causes include:

* Hypervisor CPU contention
* Oversubscribed physical hosts

In cloud environments, investigate instance performance and CPU scheduling.

## Section D — Memory

```text
MiB Mem : 8192 total, 500 free, 6000 used, 1692 buff/cache
```

| Metric     | Meaning                                              |
| ---------- | ---------------------------------------------------- |
| total      | Total RAM                                            |
| free       | Completely unused RAM                                |
| used       | RAM used excluding reclaimable cache in this display |
| buff/cache | Memory used for buffers and cache                    |

**Important:** Low free memory does not automatically mean a memory problem.

Linux intentionally uses available RAM for filesystem caching.

To investigate memory pressure, use:

```bash
free -h
```

Pay particular attention to the `available` column.

## Section E — Processes

```text
PID    USER     %CPU    %MEM    COMMAND
1234   appuser  180.0   25.0    java
1456   root      40.0    5.0    python
```

### Why Is Java Using 180% CPU?

This is an important interview question.

On a multi-core Linux server, a multithreaded process can consume more than 100% CPU.

For example:

```text
Java Process
    |
    ├── Thread 1 → 100% of one CPU
    └── Thread 2 → 80% of one CPU
```

Total:

```text
Java CPU = 180%
```

In the usual `top` process-level display, 100% represents approximately one fully utilized logical CPU.

Therefore, 180% means the Java process is consuming approximately 1.8 logical CPUs.

### Useful `top` Shortcuts

| Key | Purpose                            |
| --- | ---------------------------------- |
| `P` | Sort by CPU usage                  |
| `M` | Sort by memory usage               |
| `1` | Display individual CPU utilization |
| `H` | Show threads                       |
| `c` | Show full command line             |
| `q` | Quit                               |

### Interview Explanation

"I use `top` to monitor CPU, memory and process utilization in real time. I first check user CPU, system CPU, idle time and I/O wait. If CPU usage is high, I identify the process consuming resources. If I/O wait is high, I investigate disk performance."

---

# 4. `ps aux --sort=-%cpu` — Identify CPU-Consuming Processes

### What does `ps` mean?

`ps` stands for **Process Status**.

It displays information about running processes.

### Command

```bash
ps aux --sort=-%cpu | head -10
```

Let's break down the command.

| Component      | Meaning                                                         |
| -------------- | --------------------------------------------------------------- |
| `ps`           | Process Status                                                  |
| `a`            | Show processes associated with terminals belonging to all users |
| `u`            | User-oriented output format                                     |
| `x`            | Include processes without a controlling terminal                |
| `--sort=-%cpu` | Sort by CPU usage in descending order                           |
| `head -10`     | Display the first 10 lines                                      |

### Example Output

```text
USER      PID    %CPU   %MEM   VSZ      RSS      STAT  COMMAND
appuser   1234   180.0  25.0   4000000  2000000  Sl    java
root      1456    40.0   5.0   1000000   400000  S     python
postgres  1678    15.0  10.0   1500000   800000  S     postgres
```

### Important Fields

| Field   | Meaning                           |
| ------- | --------------------------------- |
| USER    | Process owner                     |
| PID     | Process Identifier                |
| %CPU    | CPU usage                         |
| %MEM    | Memory usage                      |
| VSZ     | Virtual memory size               |
| RSS     | Resident memory size              |
| STAT    | Process state                     |
| COMMAND | Command used to start the process |

### Understanding Process States

| State | Meaning               |
| ----- | --------------------- |
| `R`   | Running or runnable   |
| `S`   | Interruptible sleep   |
| `D`   | Uninterruptible sleep |
| `T`   | Stopped               |
| `Z`   | Zombie                |

### Real-Time Example

Your application is slow.

You execute:

```bash
ps aux --sort=-%cpu | head -10
```

You find:

```text
appuser 1234 180.0 25.0 java
```

Now you know that the Java application is consuming significant CPU resources.

The next step is to investigate why.

For example, inspect Java threads:

```bash
top -H -p 1234
```

This displays individual threads belonging to the Java process.

For deeper Java troubleshooting, you can capture a thread dump using an appropriate JDK tool:

```bash
jcmd 1234 Thread.print
```

This can help identify CPU-intensive threads, deadlocks and application-level problems.

**Note:** The `%CPU` reported by `ps` is generally based on CPU time over the process lifetime, not an instantaneous sample. Use `top` or `pidstat` for interval-based CPU measurements.

### Interview Explanation

"I use `ps aux --sort=-%cpu` to identify CPU-intensive processes. Once I identify the PID, I investigate the process further. For a Java application, I can inspect individual threads using `top -H -p <PID>` and capture thread dumps for deeper analysis."

---

# 5. `vmstat 1 5` — Check CPU Queue, Memory and I/O

This is a very important command for senior DevOps interviews.

### What does `vmstat` mean?

`vmstat` stands for **Virtual Memory Statistics**.

It provides information about:

* Processes
* Memory
* Swap
* Disk I/O
* CPU utilization
* Context switches

### Command

```bash
vmstat 1 5
```

### What Do `1` and `5` Mean?

```text
vmstat 1 5
       | |
       | └── Number of reports = 5
       |
       └──── Interval = 1 second
```

It produces five reports, approximately one second apart.

The first report usually represents averages since boot. Focus on subsequent reports for current activity.

### Example Output

```text
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 8  0      0  50000  12000 100000   0    0   100   200  500 1000 90  5  5  0  0
 9  0      0  48000  12000 100000   0    0   120   250  600 1100 92  4  4  0  0
```

Let's understand the important columns.

## A. Process Information

```text
r  b
8  0
```

| Column | Meaning                                                   |
| ------ | --------------------------------------------------------- |
| `r`    | Runnable processes, including running and waiting for CPU |
| `b`    | Processes blocked in uninterruptible sleep                |

### Example

```text
CPU Cores = 4

r = 8
```

This suggests more runnable tasks than available logical CPUs.

If this continues, CPU contention is likely.

Now consider:

```text
r = 1
b = 10
```

This indicates many blocked tasks.

Investigate storage and other sources of uninterruptible sleep.

## B. Memory Information

```text
swpd free buff cache
```

| Column  | Meaning                     |
| ------- | --------------------------- |
| `swpd`  | Virtual memory used in swap |
| `free`  | Unused memory               |
| `buff`  | Buffer memory               |
| `cache` | Cache memory                |

## C. Swap Information

```text
si so
0  0
```

| Column | Meaning                                |
| ------ | -------------------------------------- |
| `si`   | Memory swapped in from disk per second |
| `so`   | Memory swapped out to disk per second  |

### Example

```text
si = 5000
so = 8000
```

If swap activity remains high, the system may be experiencing memory pressure.

This can cause application performance problems.

## D. Disk I/O

```text
bi bo
```

| Column | Meaning                                       |
| ------ | --------------------------------------------- |
| `bi`   | Blocks received from block devices per second |
| `bo`   | Blocks sent to block devices per second       |

These provide an overview of block I/O activity.

## E. CPU Information

```text
us sy id wa st
```

These represent user CPU, system CPU, idle time, I/O wait and steal time.

### Interview Scenario

**Interviewer: Your server has 4 CPUs and Load Average is 12. How will you investigate?**

You execute:

```bash
vmstat 1 5
```

Scenario A:

```text
r = 12
b = 0
us = 95
```

Likely issue:

**CPU saturation.**

Scenario B:

```text
r = 1
b = 12
wa = 65
```

Likely issue:

**I/O bottleneck or blocked tasks.**

Next, investigate disk latency and D-state processes.

### Interview Explanation

"I use `vmstat` to analyze runnable processes, blocked processes, memory, swapping and CPU utilization. If the runnable process count is consistently higher than the available CPUs, I investigate CPU contention. If blocked processes and I/O wait are high, I investigate storage performance."

---

# 6. `iostat -xz 1 5` — Check Disk I/O Performance

### What does `iostat` mean?

`iostat` stands for **Input/Output Statistics**.

It helps monitor CPU statistics and block-device performance.

The command is commonly provided by the `sysstat` package.

### Command

```bash
iostat -xz 1 5
```

### Command Breakdown

| Option | Meaning                       |
| ------ | ----------------------------- |
| `-x`   | Extended disk statistics      |
| `-z`   | Hide devices with no activity |
| `1`    | One-second interval           |
| `5`    | Five reports                  |

As with `vmstat`, the first report generally contains averages since boot. Focus on subsequent reports for current activity.

### Example Output

```text
Device   r/s   w/s   r_await   w_await   aqu-sz   %util
sda      150   200      25.0      45.0      12.5    98.0
```

### Important Metrics

| Metric    | Meaning                                           |
| --------- | ------------------------------------------------- |
| `r/s`     | Read operations per second                        |
| `w/s`     | Write operations per second                       |
| `r_await` | Average read request latency in milliseconds      |
| `w_await` | Average write request latency in milliseconds     |
| `aqu-sz`  | Average I/O queue length                          |
| `%util`   | Percentage of time the device had I/O in progress |

### How to Interpret the Output

Consider:

```text
r_await = 25 ms
w_await = 45 ms
aqu-sz = 12.5
%util = 98%
```

This suggests the storage device is busy and requests are experiencing latency.

However, whether these values are problematic depends on:

* Storage type (HDD, SSD, NVMe)
* Application latency requirements
* Normal baseline performance
* Device concurrency capabilities

For example, 98% utilization can indicate saturation on a traditional serial disk, but it does not necessarily indicate saturation on a modern parallel SSD or NVMe device.

### Real-Time Scenario

Your PostgreSQL database is responding slowly.

You observe:

```text
CPU Utilization = 20%
Load Average = 15
```

You execute:

```bash
vmstat 1 5
```

Output:

```text
r = 1
b = 12
wa = 65
```

Then:

```bash
iostat -xz 1 5
```

Output:

```text
Device  r_await  w_await  aqu-sz  %util
sda       80       120      15     99
```

### What Does This Indicate?

The combination of:

* Many blocked processes
* High I/O wait
* High disk request latency
* Sustained disk activity

strongly suggests a storage performance bottleneck.

### Possible Solutions

Investigate:

1. Slow database queries
2. Excessive disk reads and writes
3. Storage capacity and performance limits
4. Database indexing
5. Cloud disk IOPS and throughput limits
6. Background jobs causing excessive I/O

Do not immediately increase disk capacity or IOPS without verifying the bottleneck.

### Interview Explanation

"I use `iostat -xz 1 5` to check disk performance. I focus on read/write latency, I/O queue length and device utilization. If latency is high and many processes are blocked, I investigate whether storage is causing the application slowdown."

---

# 7. Complete Production Troubleshooting Flow

Now let's connect all six commands.

**Scenario: Users report that the Spring Boot application is slow.**

```text
              Users Report Slowness
                       |
                       v
                 Check uptime
                       |
                       v
              Load Average = 12
                       |
                       v
                  Run nproc
                       |
                       v
                   CPUs = 4
                       |
                       v
                  Execute top
                       |
             +---------+---------+
             |                   |
             v                   v
        High CPU             Low CPU
             |                   |
             v                   v
      ps aux --sort=-%cpu     vmstat 1 5
             |                   |
             v                   v
      Identify Process     Check r, b, wa
             |                   |
             v                   v
      Investigate App       High Blocked Tasks
                                 |
                                 v
                           iostat -xz 1 5
                                 |
                                 v
                         Investigate Disk I/O
```

This is a practical troubleshooting approach.

---

# 8. Interview Questions Based on These Commands

| Interview Question                                        | Command               |
| --------------------------------------------------------- | --------------------- |
| How do you check Linux Load Average?                      | `uptime`              |
| How do you check CPU count?                               | `nproc`               |
| How do you monitor CPU utilization?                       | `top`                 |
| How do you identify high CPU-consuming processes?         | `ps aux --sort=-%cpu` |
| How do you check the CPU run queue?                       | `vmstat 1 5`          |
| How do you check blocked processes?                       | `vmstat 1 5`          |
| How do you identify disk latency?                         | `iostat -xz 1 5`      |
| How do you check swapping activity?                       | `vmstat 1 5`          |
| How do you identify high I/O wait?                        | `top` or `vmstat`     |
| How do you troubleshoot a CPU-intensive Java application? | `top -H -p <PID>`     |

---

# 9. Final Interview Answer — Production Troubleshooting

**Interviewer: Your Linux production server is running slowly. How do you troubleshoot it?**

You can explain:

"I would start by checking the server's load average using `uptime`.

Next, I would check the number of available CPUs using `nproc` to understand whether the load is high relative to CPU capacity.

Then I would use `top` to monitor CPU utilization, memory utilization and running processes.

If CPU utilization is high, I would use `ps aux --sort=-%cpu` to identify CPU-intensive processes.

I would also use `vmstat 1 5` to check the CPU run queue, blocked processes, memory pressure and swapping activity.

If I notice high I/O wait or blocked processes, I would use `iostat -xz 1 5` to investigate disk latency, I/O queue length and device utilization.

Based on these findings, I would identify whether the performance issue is related to CPU, memory, application processing or disk I/O before taking corrective action."

---

## 10. Hands-On Assignment on Your Linux Machine

Run these commands on your Linux system:

```bash
uptime
```

```bash
nproc
```

```bash
top
```

Press `q` to exit.

```bash
ps aux --sort=-%cpu | head -10
```

```bash
vmstat 1 5
```

```bash
iostat -xz 1 5
```

If `vmstat` or `iostat` is not installed on Ubuntu, install the relevant packages:

```bash
sudo apt update
sudo apt install procps sysstat
```

