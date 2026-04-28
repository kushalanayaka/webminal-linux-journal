# Lesson 4 – Basic Process Commands

**Platform:** Webminal  
**Status:** Completed  
**Date:** 2026-04-28

---

## Commands Learned

### `ps` – Process Snapshot
- `ps` – Shows a snapshot of currently running processes
- Output includes:
  - `PID` – Process ID (unique number for each process)
  - `TTY` – Terminal associated with the process
  - `TIME` – CPU time used so far
  - `CMD` – Command/process name
- `ps` only shows processes for the current terminal session
- To see all processes: `ps aux` or `ps -ef`

### `sleep` – Create a Background Process
- `sleep 60 &` – Creates a new process that sleeps for 60 seconds
- `&` (ampersand) – Runs the command in the background
- The terminal shows the process ID immediately after running

### `kill` – Terminate a Process by ID
- `kill 12345` – Sends a termination signal (SIGTERM) to process ID 12345
- Most processes terminate gracefully with this signal
- `kill -9 12345` – Force kills the process (SIGKILL)
- `-9` is used when a normal `kill` does not work (process is frozen or ignoring signals)

**Common kill signals:**
| Signal | Number | Effect |
|--------|--------|--------|
| SIGTERM | 15 (default) | Graceful termination |
| SIGKILL | 9 | Force kill (cannot be ignored) |
| SIGHUP | 1 | Hangup (restart process) |

### `killall` – Terminate Processes by Name
- `killall sleep` – Kills all processes named `sleep`
- Useful when multiple processes have the same name
- `killall -u webminal` – Kills only processes owned by user `webminal`
- `killall -w find` – Waits for all `find` processes to die before returning
  - `-w` (wait) – Checks once per second until all matching processes are terminated
  - May wait forever if a process ignores the signal

### `pidof` – Find Process ID by Name
- `pidof bash` – Returns the process ID(s) of all running `bash` processes
- If multiple instances are running, all PIDs are shown
- `pidof -s bash` – Returns only **one** process ID (single PID)
  - `-s` (single shot) – Useful when you need just one PID

### `nice` – Start a Process with Modified Priority
- `nice -n 19 sleep 30 &` – Runs `sleep 30` with the lowest priority (19)
- Niceness range: `-20` to `19`
  - `-20` – Highest priority (most favorable scheduling)
  - `19` – Lowest priority (least favorable)
- Only root can set negative niceness (higher priority)
- Regular users can only lower priority (set niceness to positive numbers)

### `renice` – Change Priority of Running Process
- `renice -n 19 1234` – Changes priority of process with PID 1234 to 19 (lowest)
- `renice +1 3176` – Increases niceness by 1 (lowers priority)
- `renice +1 -u webminal` – Changes priority of all processes owned by user `webminal`
- Non-root users can only **lower** priority (increase niceness value)
- Non-root users cannot increase priority (cannot make niceness lower/negative)
- Once you lower priority, you cannot raise it back (even if you own the process)

### `top` – Dynamic Real-Time Process Viewer
- `top` – Shows real-time, continuously updating process information
- Displays:
  - System uptime and load average
  - Total processes (running, sleeping, stopped, zombie)
  - CPU and memory usage
  - List of processes sorted by CPU usage
- Press `q` to quit
- Other useful keys in `top`:
  - `h` – Help
  - `k` – Kill a process
  - `r` – Renice a process

### `pstree` – Display Process Tree
- `pstree` – Shows processes in a tree structure (parent-child relationships)
- Makes it easy to see which process started which
- `pstree -p` – Shows process tree with PIDs attached to each process name

### `time` – Measure Command Execution Time
- `time ls -l` – Runs `ls -l` and shows how long it took to complete
- Output includes three timing metrics:

| Metric | Meaning |
|--------|---------|
| `real` | Actual elapsed time from start to finish |
| `user` | CPU time spent in user mode (your program's code) |
| `sys` | CPU time spent in system mode (kernel operations) |

**Example:**
```bash
$ time ls -l
total 8
-rw-r--r-- 1 user user 100 Apr 28 10:00 file.txt

real    0m0.005s
user    0m0.001s
sys     0m0.004s

```
### Commands I Typed on Webminal
```bash
ps
sleep 60 &
ps
kill 12345
ps
kill -9 12345
sleep 30 &
sleep 30 &
ps
killall sleep
killall -u webminal
killall -w find
pidof bash
pidof -s bash
nice -n 19 sleep 30 &
renice -n 19 1234
renice +1 -u webminal
top
pstree
pstree -p
time ls -l
