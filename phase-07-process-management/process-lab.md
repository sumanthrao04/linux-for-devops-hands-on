# Phase 05 — Linux Process Management & Troubleshooting

## Complete Commands Reference for GitHub

This is your Phase 05 `commands.md` reference, covering the commands introduced in our process management labs, including their syntax, options, examples, expected behavior, and production DevOps use cases.

Environment: CentOS Stream 9 Level: Beginner to Intermediate Focus: Linux processes, CPU, memory, signals, job control, process priority, and troubleshooting.

Note: This is the complete command set from our Phase 5 lessons, not a verified record of which commands you personally executed on your Linux machine.

## 1. Understanding Linux processes

A process is a running instance of a program.

For example:

```
java -jar application.jar
```

When Java starts, Linux assigns the process a PID (Process ID).

Important process terminology:

| Term | Full form           | Meaning                                       |
| ---- | ------------------- | --------------------------------------------- |
| PID  | Process ID          | Identifies a process                          |
| PPID | Parent Process ID   | Identifies the parent process                 |
| UID  | User ID             | Identifies the user associated with a process |
| TTY  | Teletype terminal   | Terminal associated with the process          |
| RSS  | Resident Set Size   | Resident memory in RAM                        |
| VSZ  | Virtual Memory Size | Virtual address-space size                    |
| STAT | Process Status      | Current process state                         |
| NI   | Nice Value          | Influences scheduling priority                |

## 2. `echo $$` — Find the current shell PID

Purpose: Displays the PID of your current Bash shell.

```
echo $$
```

Example output:

```
2456
```

Explanation:

- `echo` prints text or expanded values.
- `$$` is a special Bash parameter representing the shell PID.

DevOps example: Identify the shell running your deployment commands.

## 3. `ps` — Process Status

Purpose: Displays information about running processes.

### Basic command

```
ps
```

Example:

```
PID    TTY       TIME CMD
2456   pts/0 00:00:00 bash
2501   pts/0 00:00:00 ps
```

| Column | Meaning             |
| ------ | ------------------- |
| PID    | Process ID          |
| TTY    | Associated terminal |
| TIME   | CPU time consumed   |
| CMD    | Command name        |

### Detailed process information

```
ps -f
```

`-f` means full-format listing, including PPID and other information.

### Display all processes

```
ps -ef
```

- `-e` — All processes.
- `-f` — Full-format output.

### Display user-oriented process information

```
ps aux
```

These are BSD-style options:

- `a` — Processes associated with terminals from multiple users.
- `u` — User-oriented display.
- `x` — Include processes without a controlling terminal.

### Inspect a specific PID

```
ps -p 2456
```

`-p` selects a process by PID.

### Customize output columns

```
ps -p 2456 -o pid,ppid,user,stat,cmd
```

`-o` specifies the columns.

DevOps use case: Investigate whether a Java, Jenkins, or Nginx process is running.

## 4. `pstree` — Display the process hierarchy

Purpose: Shows parent-child process relationships in a tree.

```
pstree
```

Display PIDs:

```
pstree -p
```

Display descendants of your current shell:

```
pstree -p $$
```

Example:

```
bash(2456)---sleep(2501)
```

Why is this important?

Suppose a Jenkins job starts a Bash script that launches Java. The process tree helps identify which process started another.

## 5. `top` — Monitor processes in real time

Purpose: Displays live CPU, memory, and process statistics.

```
top
```

Important columns:

| Column  | Meaning                 |
| ------- | ----------------------- |
| PID     | Process ID              |
| USER    | Process owner           |
| PR      | Scheduling priority     |
| NI      | Nice value              |
| VIRT    | Virtual memory          |
| RES     | Resident memory         |
| SHR     | Shared memory           |
| S       | Process state           |
| %CPU    | CPU usage               |
| %MEM    | Memory usage percentage |
| TIME+   | Accumulated CPU time    |
| COMMAND | Command                 |

### Keyboard shortcuts

| Key | Action                   |
| --- | ------------------------ |
| `P` | Sort by CPU              |
| `M` | Sort by memory           |
| `1` | Toggle per-CPU display   |
| `c` | Toggle full command line |
| `k` | Prompt to send a signal  |
| `q` | Quit                     |

### Monitor one process

```
top -p 2456
```

`-p` filters the display to a PID.

DevOps use case: Investigate CPU spikes or application memory consumption.

## 6. `htop` — Interactive process monitoring

`htop` is an interactive process viewer.

```
htop
```

On CentOS Stream 9, installation may require an additional enabled repository:

```
sudo dnf install htop
```

Useful for visually inspecting processes, CPU usage, memory usage, and process trees.

`top` is sufficient if `htop` is unavailable.

## 7. Find high CPU processes

### Sort by CPU usage

```
ps aux --sort=-%cpu
```

Explanation:

- `--sort` specifies sorting.
- `-%cpu` sorts CPU percentage in descending order.

### Display the top five processes

```
ps aux --sort=-%cpu | head -n 6
```

Why six lines? One header plus five process rows.

### Custom process report

```
ps -eo pid,ppid,user,%cpu,%mem,stat,comm --sort=-%cpu | head -n 11
```

- `-e` selects all processes.
- `-o` selects output columns.
- `head -n 11` displays the header and ten process rows.

Production scenario: CPU alert at 95%. Use these commands to identify processes consuming CPU before investigating why.

## 8. Find high memory processes

### Check system memory

```
free -h
```

`-h` displays human-readable sizes.

### Sort by memory usage

```
ps aux --sort=-%mem | head -n 11
```

### Display memory details

```
ps -eo pid,user,rss,vsz,%mem,comm --sort=-rss | head -n 11
```

Explanation:

- `rss` — Resident memory, typically displayed in KiB.
- `vsz` — Virtual address-space size, typically displayed in KiB.
- `%mem` — Resident memory as a percentage of physical RAM.
- `--sort=-rss` — Sort by resident memory, descending.

DevOps scenario: Investigate whether a Java application is consuming unusually large amounts of memory.

## 9. `pgrep` — Find processes by name

Purpose: Searches for processes matching specified criteria.

```
pgrep bash
```

Display PID and command line:

```
pgrep -a bash
```

Search processes belonging to the current user:

```
pgrep -u "$USER" -a
```

Important options:

| Option | Meaning                      |
| ------ | ---------------------------- |
| `-a`   | Display PID and command line |
| `-u`   | Match process owner          |
| `-f`   | Match full command line      |
| `-x`   | Match exact process name     |

### DevOps example

```
pgrep -af java
```

Find Java-related process command lines. Review results carefully because full-command matching can include other commands containing the word `java`.

## 10. `pidof` — Find PID by executable name

```
pidof bash
```

Example:

```
2456 2301
```

This indicates multiple Bash processes.

Difference between `pgrep` and `pidof`:

`pgrep` offers flexible pattern-based matching, while `pidof` primarily identifies processes by program name.

## 11. `sleep` — Create a test process

Purpose: Delays execution for a specified duration.

```
sleep 300
```

This waits 300 seconds.

### Run in background

```
sleep 300 &
```

Example output:

```
[1] 2501
```

- `[1]` — Shell job number.
- `2501` — PID.
- `&` — Run in the background.

### Capture the PID

```
sleep 300 &
LAB_PID=$!
echo "$LAB_PID"
```

`$!` expands to the PID of the most recent background command started by the shell.

This is a safe way to manage your own test process.

## 12. `jobs -l` — Display shell jobs and PIDs

Purpose: Displays jobs managed by your current shell.

```
jobs -l
```

Example:

```
[1]+ 2501 Running    sleep 300 &
```

Explanation:

| Component     | Meaning             |
| ------------- | ------------------- |
| `[1]`         | Job number          |
| `+`           | Current/default job |
| `2501`        | PID                 |
| `Running`     | Job status          |
| `sleep 300 &` | Command             |

### Other useful options

```
jobs -r
```

Displays running jobs.

```
jobs -s
```

Displays stopped jobs.

Important: `jobs` is a Bash built-in and only reports jobs known to that shell. It does not display every Linux process.

## 13. `fg` — Bring a job to the foreground

```
fg %1
```

Explanation:

- `fg` — Foreground.
- `%1` — Job number 1.

The terminal becomes occupied by the selected job until it finishes, stops, or is interrupted.

## 14. `bg` — Resume a stopped job in the background

Start:

```
sleep 300
```

Press `Ctrl+Z` to suspend the job.

Then:

```
bg %1
```

This resumes job 1 in the background.

### Keyboard shortcuts

| Shortcut | Meaning                                       |
| -------- | --------------------------------------------- |
| `Ctrl+C` | Sends SIGINT to the foreground process group  |
| `Ctrl+Z` | Sends SIGTSTP to the foreground process group |
| `Ctrl+D` | Sends end-of-file input; may exit a shell     |
| `&`      | Starts a command in the background            |

Difference: `Ctrl+C` normally requests interruption, while `Ctrl+Z` suspends execution.

## 15. `kill` — Send signals to processes

Purpose: Sends a signal to a process.

Despite its name, `kill` is not limited to terminating processes.

### Default signal

```
kill 2501
```

Normally sends SIGTERM.

### Explicit SIGTERM

```
kill -TERM 2501
```

Requests graceful termination.

### SIGKILL

```
kill -KILL 2501
```

Forcibly terminates the process without allowing it to handle the signal.

### List signals

```
kill -l
```

### Check whether a process exists and is signalable

```
kill -0 2501
```

This sends no signal, but checks process existence and permissions. A successful result doesn't prove the process is healthy.

### Safe lab

```
sleep 600 &
LAB_PID=$!

kill -TERM "$LAB_PID"
wait "$LAB_PID"
```

The `wait` command may return a nonzero exit status because the process was terminated by a signal.

### Important signals

| Signal  | Common Linux number    | Purpose             |
| ------- | ---------------------- | ------------------- |
| SIGHUP  | 1                      | Hangup              |
| SIGINT  | 2                      | Interrupt           |
| SIGQUIT | 3                      | Quit                |
| SIGKILL | 9                      | Forced termination  |
| SIGTERM | 15                     | Termination request |
| SIGSTOP | Architecture-dependent | Stop process        |
| SIGCONT | Architecture-dependent | Continue process    |

Prefer signal names because some signal numbers differ by architecture.

## 16. `pkill` — Send signals by process name

```
pkill -TERM -x sleep
```

This targets processes whose names exactly match `sleep` and that you have permission to signal.

Warning: It could terminate multiple test processes. Prefer targeting a known PID during practice.

## 17. `nice` — Start a process with a scheduling nice value

Purpose: Starts a process with an adjusted nice value.

```
nice -n 10 sleep 300 &
```

Explanation:

- `nice` starts a command with a specified nice value.
- `-n 10` sets nice value 10.
- `&` runs it in the background.

Normal nice values range from `-20` to `19`.

| Value | Meaning                    |
| ----- | -------------------------- |
| -20   | Higher scheduling priority |
| 0     | Default nice value         |
| 19    | Lower scheduling priority  |

Lower nice values favor a process when competing for CPU under normal scheduling policies.

## 18. `renice` — Change priority of an existing process

Start a process:

```
nice -n 10 sleep 300 &
LAB_PID=$!
```

Check:

```
ps -p "$LAB_PID" -o pid,ni,pri,cmd
```

Change nice value:

```
renice -n 15 -p "$LAB_PID"
```

Explanation:

- `-n 15` — Set nice value to 15.
- `-p` — Target a process PID.

Verify:

```
ps -p "$LAB_PID" -o pid,ni,pri,cmd
```

Terminate:

```
kill "$LAB_PID"
```

DevOps use: Influence CPU scheduling for noncritical batch processes. It does not replace resource limits or proper capacity planning.

## 19. `nohup` — Run a process ignoring hangup signals

Full form: No Hangup.

```
nohup sleep 600 > logs/nohup-test.log 2>&1 &
```

Explanation:

| Part        | Meaning                                                  |
| ----------- | -------------------------------------------------------- |
| `nohup`     | Ignore SIGHUP                                            |
| `sleep 600` | Run test process                                         |
| `>`         | Redirect standard output                                 |
| `2>&1`      | Redirect standard error to standard output's destination |
| `&`         | Run in background                                        |

### Capture PID

```
LAB_PID=$!
```

### Verify

```
ps -p "$LAB_PID" -o pid,stat,cmd
```

### Stop

```
kill "$LAB_PID"
```

DevOps note: `nohup` is useful for simple temporary tasks. For production applications, use systemd or another suitable service manager.

## 20. `disown` — Remove a job from shell job management

Start:

```
sleep 600 &
```

Then:

```
disown
```

This removes the current job from Bash's job table.

An alternative:

```
disown -h %1
```

This marks job 1 so Bash won't send it SIGHUP when Bash exits, while retaining it in the job table.

Difference: `nohup` changes SIGHUP handling for a command at startup; `disown` changes how Bash manages an existing job.

## 21. Linux process states

Inspect:

```
ps -eo pid,ppid,stat,comm
```

Common states:

| State | Meaning               |
| ----- | --------------------- |
| R     | Running or runnable   |
| S     | Interruptible sleep   |
| D     | Uninterruptible sleep |
| T     | Stopped or traced     |
| Z     | Zombie                |
| I     | Idle kernel thread    |

### Count processes by state

```
ps -eo stat= | cut -c1 | sort | uniq -c
```

Explanation:

1. `ps -eo stat=` displays process states without a header.
2. `cut -c1` extracts the first character.
3. `sort` groups identical states.
4. `uniq -c` counts occurrences.

## 22. Zombie process investigation

A zombie process has exited, but its parent has not yet collected its termination status.

Search:

```
ps -eo pid,ppid,stat,comm | awk '$3 ~ /^Z/'
```

Explanation:

- `$3` — Process status column.
- `~` — Regex match operator in awk.
- `/^Z/` — Status beginning with Z.

Important: You cannot kill an already-exited zombie process. Investigate the parent process and whether it is reaping children.

## 23. `vmstat` — Virtual Memory Statistics

Purpose: Displays system statistics about processes, memory, paging, block I/O, interrupts, and CPU.

```
vmstat 1 5
```

Explanation:

- `1` — Sampling interval of one second.
- `5` — Five reports.

Important fields:

| Field | Meaning                                |
| ----- | -------------------------------------- |
| r     | Runnable tasks                         |
| b     | Tasks blocked in uninterruptible sleep |
| swpd  | Swap used                              |
| free  | Free memory                            |
| buff  | Buffer memory                          |
| cache | Cache memory                           |
| si    | Swap in                                |
| so    | Swap out                               |
| bi    | Blocks received from block devices     |
| bo    | Blocks sent to block devices           |
| us    | User CPU                               |
| sy    | System CPU                             |
| id    | Idle CPU                               |
| wa    | I/O wait                               |
| st    | Steal time                             |

The first report usually represents averages since boot; subsequent reports describe the sampling intervals.

### DevOps use

Investigate CPU contention, memory pressure, swapping, and possible I/O bottlenecks.

## 24. Inspect threads of a process

```
ps -L -p 2456 -o pid,tid,pcpu,stat,comm
```

Explanation:

- `-L` — Display threads.
- `-p` — Select PID.
- `tid` — Thread ID.
- `pcpu` — CPU usage.

DevOps example: Investigate threads consuming CPU in a Java application.

## 25. Check OOM events using journalctl

OOM: Out Of Memory.

```
sudo journalctl -k --no-pager | grep -i -E 'out of memory|oom|killed process'
```

Explanation:

- `journalctl` — Query systemd journal.
- `-k` — Kernel messages.
- `--no-pager` — Print directly without an interactive pager.
- `grep -i` — Ignore case.
- `grep -E` — Extended regular expressions.

No matching output does not necessarily prove that no OOM event occurred; logs may have rotated or not been retained.

# 26. Complete Production Troubleshooting Scenarios

## Scenario 1 — High CPU Usage

Alert: CPU utilization 95%, application response time increased.

Initial investigation:

```
hostname
uptime
nproc
top
ps aux --sort=-%cpu | head -n 11
vmstat 1 5
```

Inspect a suspicious process:

```
ps -p PID -o pid,ppid,user,%cpu,%mem,etime,stat,cmd
```

Investigate threads:

```
ps -L -p PID -o pid,tid,pcpu,stat,comm
```

Potential causes include high request traffic, CPU-heavy operations, garbage collection, or inefficient application code.

## Scenario 2 — High Memory Usage

Alert: Memory utilization 95%.

```
free -h
ps aux --sort=-%mem | head -n 11
swapon --show
vmstat 1 5
```

Inspect kernel OOM messages:

```
sudo journalctl -k --no-pager | grep -i -E 'out of memory|oom|killed process'
```

Investigate whether the issue involves memory pressure, swap activity, application heap settings, or memory growth.

## Scenario 3 — Application Process Not Responding

First identify the process:

```
pgrep -af java
```

Inspect the specific PID:

```
ps -p PID -o pid,ppid,user,stat,%cpu,%mem,etime,cmd
```

Inspect its threads:

```
ps -L -p PID -o pid,tid,pcpu,stat,comm
```

Review application logs and service status before deciding whether to restart the process.

# 27. Quick Command Cheat Sheet

| Command               | Purpose                           |
| --------------------- | --------------------------------- |
| `echo $$`             | Current Bash shell PID            |
| `ps`                  | Display processes                 |
| `ps -f`               | Full process listing              |
| `ps -ef`              | All processes in full format      |
| `ps aux`              | User-oriented process listing     |
| `pstree -p`           | Process hierarchy with PIDs       |
| `top`                 | Live process monitoring           |
| `htop`                | Interactive process monitor       |
| `ps aux --sort=-%cpu` | Sort by CPU usage                 |
| `ps aux --sort=-%mem` | Sort by memory usage              |
| `pgrep`               | Search process IDs                |
| `pidof`               | Find PIDs by executable name      |
| `sleep`               | Delay execution                   |
| `jobs -l`             | Shell jobs with PIDs              |
| `fg`                  | Bring job to foreground           |
| `bg`                  | Resume job in background          |
| `kill`                | Send signal by PID                |
| `pkill`               | Send signal by matching process   |
| `kill -l`             | List signals                      |
| `nice`                | Start with a nice value           |
| `renice`              | Change nice value                 |
| `nohup`               | Run ignoring SIGHUP               |
| `disown`              | Remove/mark shell job             |
| `free -h`             | Check RAM and swap                |
| `vmstat`              | System activity statistics        |
| `ps -L`               | Display process threads           |
| `journalctl -k`       | View kernel journal messages      |
| `wait`                | Wait for child process completion |
