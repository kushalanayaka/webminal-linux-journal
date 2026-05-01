# Lesson 9 – Linux Process Basic Commands

**Platform:** Webminal  
**Status:** Completed  
**Date:** 2026-05-01

---

## What is a Process?

Wiki says: "In computing, a process is an instance of a computer program that is being executed."

**Simple definition:** Process is nothing but a file-content which is residing in RAM.

**Key point:** In Linux, everything is a file.

## `hostname` – Display System Name

```bash
hostname


Displays the name of the system (e.g., fedori).

## Executable File Details
`/bin/hostname` is an ELF 32-bit executable file. When we type an executable on the bash prompt, its content is loaded into RAM, and the Kernel follows those instructions.

```bash
file /bin/hostname
```
**Output:**
```
/bin/hostname: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.18, stripped
```

## Viewing Executable Content
```bash
cat /bin/hostname
```
This shows a combination of readable and unreadable characters. Within that output, instructions tell the Kernel to read a file named `/etc/hosts`, search for entry `127.0.0.1` (which typically points to the host machine name), and display that content.

## Viewing Processes with `ps`
Similar to `ls` (which shows files on disk), `ps` shows files currently in RAM.

```bash
ps
```
**Output example:**
| PID   | TTY      | TIME       | CMD |
|-------|----------|------------|-----|
| 27447 | pts/9    | 00:00:00   | bash |
| 29731 | pts/9    | 00:00:00   | ps |

### Understanding Process Fields:
- **PID**: Process ID – unique number assigned to each process.
- **TTY**: Terminal allocated to the process. If Kernel needs to ask or print something, it uses this terminal.
- **TIME**: CPU time taken to execute the process's instruction, in `[dd-]hh:mm:ss` format.
- **CMD**: Executable file name currently residing in memory.

## Parent and Child Processes
- **Parent process**: Instructs Kernel to load and execute another process (e.g., Bash shell is the manager).
- **Child process**: Created by a parent process.
- Typically, Child PID is higher than Parent PID unless PIDs are reused after running out.\n- The Bash shell acts as the parent process which instructs Kernel to load `hostname` and execute it. The parent has its own unique PID.

```bash
ps
```
**Output:**
| PID   | TTY      | TIME       | CMD |
|-------|----------|------------|-----|
| 27447 | pts/9    | 00:00:00   | bash |
| 31400 | pts/9    | 00:00:00   | bash |
| 31414 | pts/9    | 00:00:00   | ps |
details:
the primary bash shell has PID 27447,
the new Bash shell created has ID 31400,
and `ps` command has ID 31414.

## Showing Parent Process ID (`ppid`)
bash:
```bash
ps -o ppid 31400
```

# Finding Grandparent Process

```bash
ps -o ppid,cmd 27447
```

If you keep repeating this, you will end up on PID 1 – which is the parent of all processes. It is called the **init** process, created by the Kernel.

# Summary of Process Hierarchy

| Level | Process | PID |
|-------|-----------|-----|
| Grandparent | init (created by Kernel) | 1 |
| Parent | Primary Bash shell | 27447 |
| Child | Secondary Bash shell | 31400 |
| Grandchild | ps command | 31414 |

# Key Takeaways
- **Process** = file content residing in RAM
- Kernel follows instructions from processes
- Bash shell acts as manager, assigning tasks to Kernel
- Kernel is the super programmer who decides whether to complete or refuse tasks
- Every process has a unique **PID**
- Every process (except init) has a parent process

# Commands I Typed on Webminal
```bash
hostname
file /bin/hostname
cat /bin/hostname
ps
ps -o ppid 31400
ps -o ppid,cmd 31400
ps -o ppid,cmd 27447```

