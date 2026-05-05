# Lesson 11 – Linux Process States

**Platform:** Webminal  
**Status:** Completed  
**Date:** 2026-05-05

---

## Process States

According to `man ps`, a process can be in one of the following states:

| State | Description |
|-------|-------------|
| `D` | Uninterruptible sleep (usually I/O) |
| `R` | Running or runnable (on run queue) |
| `S` | Interruptible sleep (waiting for an event to complete) |
| `T` | Stopped, either by a job control signal |
| `X` | Dead (should never be seen) |
| `Z` | Defunct ("zombie") process, terminated but not reaped by its parent |

---

## Viewing Process States

To list existing processes and their states:

```bash
ps -S
```

**Output example:**

```text
PID TTY      STAT   TIME COMMAND
16454 pts/4    Ss     0:03 bash
28682 pts/4    R+     0:00 ps -S
```

- STAT S – Interruptible sleep

- s – Session leader

- + – Runs in foreground

Here, bash (PID 16454) is waiting for its child (28682) to complete. The child is in running state (`R`).

### Stopping a Process
Type `sleep 100` and then press `Ctrl+z`:
```bash
sleep 100
^Z
```

Check the status:
```bash
ps S
```
Output:

```text
PID TTY      STAT   TIME COMMAND
16454 pts/4    Ss     0:05 bash
29796 pts/4    T      0:00 sleep 10
29846 pts/4    R+     0:00 ps S
```

- `STAT` `T` – Stopped (not terminated)

- A stopped process can be resumed later.

### Resuming a Stopped Process
First, create a long-running output:

```bash
seq 1 500000
```

Stop it with `Ctrl+z` (example stopped at 19960). Resume with `fg`:

```bash
fg
```
The process continues from where it left off (19961 to 500000).

### Zombie Process (`Z`)

A zombie is a terminated process that has not been reaped by its parent. The parent has not yet collected the exit status of the child.

**Steps to reproduce:**

Get your session leader PID:

```bash
ps
```
Output:

```text
PID TTY          TIME CMD
2249 pts/1    00:00:00 bash
2294 pts/1    00:00:00 ps
```
Create a subshell:
```bash
bash
ps
```
Output:

```text
PID TTY          TIME CMD
2249 pts/1    00:00:00 bash
2498 pts/1    00:00:00 bash
2540 pts/1    00:00:00 ps
```
From a grandchild shell, stop the subshell (PID 2498):

```
bash
( ( kill -STOP 2498 ) )
```
Output:

```text
[1]+  Stopped                 bash
```

Now check process states:

```
bash
ps S
```

Output:

```text
PID TTY      STAT   TIME COMMAND

2249 pts/1    Ss     0:00 bash
2498 pts/1    T      0:00 bash
2547 pts/1    Z      0:00 [bash] <defunct>
2551 pts/1    R+     0:00 ps S
```
- `T` – Stopped subshell (2498) that has not collected its child's exit status

- `Z` – Zombie child (2547)

View the stopped background job:

```bash
jobs
```
Output:

```text
[1]+  Stopped                 bash
```
Resume the stopped shell to reap the zombie:

```bash
fg
bash
```
Check again:

```bash
ps S
```
Output:

```text
PID TTY      STAT   TIME COMMAND
2249 pts/1    Ss     0:00 bash
2498 pts/1    S      0:00 bash
2561 pts/1    R+     0:00 ps S
```
The zombie process has disappeared.

### Orphaned Process
An orphaned process is a running process whose parent has died without reaping it. Such processes are adopted by `init` (PID 1).

**Steps to reproduce:**

Check current processes:

```bash
ps S
```
Output:

```text
PID TTY      STAT   TIME COMMAND
2249 pts/1    Ss     0:01 bash
3325 pts/1    R+     0:00 ps S
```

Create a subshell:

```bash
bash
ps
```
Output:

```text
PID TTY          TIME CMD
2249 pts/1    00:00:00 bash
3329 pts/1    00:00:00 bash
3371 pts/1    00:00:00 ps
```
From a grandchild, kill the subshell (3329) while its child is still running:

```bash
( sleep 100 & ( kill -9 3329 ))
Killed
```
Verify the subshell is dead:

```bash
ps S
```
Output:

```text
PID TTY      STAT   TIME COMMAND
2249 pts/1    Ss     0:01 bash
3376 pts/1    S      0:00 sleep 100
3381 pts/1    R+     0:00 ps S
```
Check the parent of the orphaned child:

```bash
ps -o ppid 3376
```
Output:

```text
PPID
1
```
The orphaned child has been adopted by init (PID 1).

### Commands I Typed on Webminal
```bash
ps -S
sleep 100
Ctrl+z
ps S
seq 1 500000
Ctrl+z
fg
ps
bash
ps
( ( kill -STOP 2498 ) )
ps S
jobs
fg
bash
ps S
ps S
bash
ps
( sleep 100 & ( kill -9 3329 ))
ps S
ps -o ppid 3376
```


