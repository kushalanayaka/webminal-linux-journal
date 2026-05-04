# Lesson 10 – Foreground and Background Processes

**Platform:** Webminal  
**Status:** Completed  
**Date:** 2026-05-04

---

## Review

- There is a first process named `init` with PID `1`. This is the parent of all processes in the system.
- A process named `bash` interacts with the Kernel on behalf of user requests or commands.
- Every time you log in, a new parent bash is created.

```bash
ps
```
**Output example:**

```text
PID TTY          TIME CMD
5254 pts/1    00:00:00 bash
5336 pts/1    00:00:00 ps
```
In this case, the manager Bash job ID is` 5254`. Each command is a process.

### Blocking Call (Foreground Process)
When bash creates a child process, it uses a blocking call. This means: run the child process and wait for it to complete, then return to the bash prompt.

```bash
sleep 5
```
The shell hangs for 5 seconds and then provides the bash prompt again.

If you run:

```bash
sleep 5
sleep 2
```
The shell runs `sleep 5 `first and waits for it to finish, then runs `sleep 2`. If `sleep 2` is more critical than `sleep 5`, there is an unnecessary delay of 5 seconds.

### Background Process
The shell has an option to run child processes in the background – it will not wait for the child to finish before accepting new user input.

To put any child process in the background, append `&` to the command:

```bash
sleep 5 &
```

**Output:**

```text
[1] 5781
```
- `[1]` – Job number

- `5781` – Background child process ID (PID)

Now you can run another command without waiting:

```bash
sleep 2
```
### Verify Background Process PID
```bash
sleep 5 &
[1] 6095

ps
```
**Output:**

```text
PID TTY          TIME CMD
5254 pts/1    00:00:00 bash
6095 pts/1    00:00:00 sleep
6099 pts/1    00:00:00 ps
```
The `sleep 5` PID matches the `ps` output.

### pstree – Visualize Parent-Child Relationship
`pstree` shows the mapping between child and parent processes.

Note your Bash shell PID (here it is `5254`):

```bash
pstree 5254
```
Output:

```text
bash───pstree
```
This shows Bash has one child process named pstree.

Run a background process:

```bash
sleep 5 &
[1] 6208

pstree 5254
```
Output:

text
```
bash─┬─pstree
     └─sleep
```
Now Bash has two children: pstree and sleep.

### `pstree -p` – Display PIDs

```bash
sleep 5 &
[1] 6272

pstree -p 5254
```
Output:

```text
bash(5254)─┬─pstree(6276)
           └─sleep(6272)
```
The PID from the background process `(6272) `matches the `pstree` output.

### Multiple Background Jobs
Start 4 long-running background jobs (45 seconds each) and 1 very long job (3000 seconds):

```bash
sleep 45 &
[1] 6393

sleep 45 &
[2] 6397

sleep 45 &
[3] 6401

sleep 45 &
[4] 6406

sleep 3000 &
[5] 6557

pstree -p 5254
```
Output:

```text
bash(5254)─┬─pstree(6410)
           ├─sleep(6393)
           ├─sleep(6397)
           ├─sleep(6401)
           ├─sleep(6406)
           └─sleep(6557)
```
### `jobs` – List Background Jobs
`pstree` shows all processes including `pstree` itself. To view only background jobs, use the `jobs `command:

```bash
jobs
```
Output:

```text
[1]   Running                 sleep 45 &
[2]   Running                 sleep 45 &
[3]-  Running                 sleep 45 &
[4]+  Running                 sleep 45 &
[5]+  Running                 sleep 3000 &
```
- `[1], [2],` etc. – Job numbers

- `Running` – Current status

- `&` – Background process

- `+ `– Default job (used if you type `fg` without an argument)

### `fg` – Bring a Process to Foreground

Job `[5]` runs for 3000 seconds – a long time. To bring it to the foreground:

```bash
fg 5
sleep 3000
```
The shell now executes the `sleep` command and hangs (it will wait for 3000 seconds, or 50 minutes, unless interrupted).

### `ctrl+z` – Stop the Current Foreground Process
If you brought a background process to the foreground by mistake and want to put it back in the background:

Press `ctrl+z`:

```text
^Z
[5]+  Stopped                 sleep 3000
```
### Verify Stopped Status

```bash
jobs
```
Output:

```text
[5]+  Stopped                 sleep 3000
```
### `bg` – Restart a Stopped Process in Background

```bash
bg 5
[5]+ sleep 3000 &

jobs
```
Output:

```text
[5]+  Running                 sleep 3000 &
```
### Summary of Job Control Commands
|Command	|Action|
|--------|-------|
|`command &`	| Run command in background |
|`jobs`	| List all background jobs |
|`fg %jobnumber` |	Bring a job to the foreground |
|`ctrl+z`	| Stop the current foreground job |
|`bg %jobnumber` |	Restart a stopped job in the background|

### Process States
|State	| Meaning |
|----------|-------|
|`Running` |	Process is currently executing|
|`Stopped`|	Process is paused (e.g., by ctrl+z)|
|`Terminated` |	Process has finished |

### Commands I Typed on Webminal

```bash
ps
sleep 5
sleep 5 &
sleep 2
sleep 5 &
ps
pstree 5254
sleep 5 &
pstree 5254
sleep 5 &
pstree -p 5254
sleep 45 &
sleep 45 &
sleep 45 &
sleep 45 &
sleep 3000 &
pstree -p 5254
jobs
fg 5
ctrl+z
jobs
bg 5
jobs
Quick Summary
Command	Action
ps
```
