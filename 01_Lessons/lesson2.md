# Lesson 2 – Create Files, Display Contents and Stats

**Platform:** Webminal  
**Status:** Completed  
**Date:** 2026-04-27

---

## Commands Learned

### `touch` – Create Empty File or Update Timestamp
- `touch file1.txt` – Creates a new empty file named `file1.txt`
- If the file already exists, `touch` does not overwrite or change the content
- Instead, it updates the **last modified time** to the current time
- **Example:** Create file → wait 1 minute → `touch` again → modified time changes

### `ls` – List Directory Contents
- `ls` – Lists files and directories (you already learned this in Lesson 1)
- Note: `dir` is not a standard Linux command; `ls` is the correct command

### `clear` – Clear Terminal Screen
- `clear` – Clears all previous output from the terminal screen
- Gives you a fresh, clean screen
- Keyboard shortcut: `Ctrl + L` does the same thing

### `echo` – Display Text or Write to Files
- `echo 'hello'` – Prints `hello` to the terminal screen
- `echo "hello" > hello.txt` – Creates `hello.txt` and writes `hello` inside it
- `>` (single angle bracket) – **Overwrites** the file (deletes old content, writes new)
- `echo "linux" >> hello.txt` – Adds `linux` to the end of `hello.txt`
- `>>` (double angle bracket) – **Appends** (adds new content without deleting old)

### `cat` – View File Content
- `cat hello.txt` – Shows the entire content of `hello.txt` in the terminal
- Short for "concatenate" (can also join multiple files)

### `head` – View First Lines of a File
- `head hello.txt` – Shows first **10 lines** by default
- `head -2 hello.txt` – Shows first **2 lines** only
- `head -n 5 hello.txt` – Another way to show first 5 lines

### `tail` – View Last Lines of a File
- `tail hello.txt` – Shows last **10 lines** by default
- `tail -2 hello.txt` – Shows last **2 lines** only
- `tail -n 5 hello.txt` – Another way to show last 5 lines

### `stat` – Display File Information and Statistics
- `stat hello.txt` – Shows detailed information about the file

**What `stat` shows:**

| Field | Meaning |
|-------|---------|
| File | Name of the file |
| Size | Number of bytes (characters) in the file |
| Blocks | Number of disk blocks used |
| IO Block | Block size of the filesystem |
| Device | Device number where file exists |
| Inode | Unique identifier number of the file |
| Links | Number of hard links to the file |
| Access | File permissions (who can read/write/execute) |
| UID | User ID of the file owner |
| GID | Group ID of the file owner |
| Access time | Last time file was read |
| Modify time | Last time file content was changed |
| Change time | Last time file metadata (permissions, owner) was changed |

**Simple explanation of the three times:**
- **Access time** – When you `cat` or `head` the file
- **Modify time** – When you change content using `echo >` or `>>`
- **Change time** – When you change file permissions or rename the file

---

## Difference Between `>` and `>>`

| Operator | Action | Effect on existing content |
|----------|--------|---------------------------|
| `>` | Overwrite | Deletes everything, writes new |
| `>>` | Append | Keeps everything, adds new at end |

**Example:**
```bash
echo "first" > file.txt   # file contains: first
echo "second" > file.txt  # file contains: second (first is gone)
echo "third" >> file.txt  # file contains: second \n third


````
## Commands I Typed on Webminal

```bash
touch file1.txt
ls
clear
echo 'hello'
echo "hello" > hello.txt
echo "linux" >> hello.txt
cat hello.txt
head -2 hello.txt
head hello.txt
tail -2 hello.txt
stat hello.txt
