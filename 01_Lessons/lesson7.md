# Lesson 7 – Locate File and Its Type

**Platform:** Webminal  
**Status:** Completed  
**Date:** 2026-04-29

---

## Commands Learned

### `file` – Determine File Type
- `file linux.txt` – Determines the type of a file
- Output for text file: `linux.txt: ASCII text`
- `file /dev/null` – Shows `/dev/null: character special` (character device)
- Useful when you don't know what kind of file you are dealing with

**Common file types detected by `file`:**
| Output | Meaning |
|--------|---------|
| `ASCII text` | Regular text file |
| `directory` | Directory |
| `symbolic link` | Link to another file |
| `executable` | Binary program |
| `character special` | Device file (character-based) |
| `block special` | Device file (block-based) |

**For system devices (root only):**
- `file -s /dev/sda2` – Shows filesystem details of a special device
- `-s` (special files) – Reads and identifies device files
- Example output: `/dev/sda2: x86 boot sector, code offset 0x52, OEM-ID "NTFS "`
- **Note:** Regular users will get `permission denied` error for this command

### `whereis` – Locate Binary, Source, and Manual Files
- `whereis ls` – Finds the location of the `ls` command
- Output: `ls: /bin/ls /usr/share/man/man1/ls.1.gz`
- Shows binary file, source files, and manual pages
- `whereis stdio.h` – Finds header file location
- Output: `stdio: /usr/include/stdio.h /usr/share/man/man3/stdio.3.gz`

**What whereis searches:**
| Type | Description |
|------|-------------|
| Binary | Executable files (like `/bin/ls`) |
| Source | Source code files |
| Manual | Man pages (`.gz` files) |

### `which` – Locate Binary File Being Executed
- `which php` – Shows which version of PHP will run when you type `php`
- Output: `/usr/bin/php` (or similar path)
- Searches only in directories listed in `$PATH` environment variable
- Useful when multiple versions of a program are installed

**Difference between whereis and which:**
| Command | Searches | Shows |
|---------|----------|-------|
| `whereis` | Binary, source, man pages | All locations found |
| `which` | Only `$PATH` directories | Only the binary that will execute |

**Understanding `$PATH`:**
```bash
echo $PATH
# Output: /usr/local/bin:/usr/bin:/bin:/home/user/bin

```
- which only looks in these directories

- First matching binary found is the one that runs

### `find` – Search for Files in Directory Hierarchy

- `find ~ -name "linux.txt"` – Searches for file named `linux.txt` starting from home directory (~)

- `-name` – Matches by filename (case-sensitive)

Useful find options:

|Option	|Meaning|
|-------|--------|
|-name "pattern"	| Search by filename (case-sensitive) |
|-iname "pattern"	| Search by filename (case-insensitive) |
|-type f	| Search only regular files |
|-type d	| Search only directories |
|-size +20c |	Files larger than 20 bytes }|
|-exec command {} \;	| Execute command on found files |

### Finding files and running file command:

```bash
find . -type f -exec file '{}' \;

```
- `.` – Current directory

- `-type f` – Regular files only

- `-exec file '{}' \;` – Run `file` command on each found file

- `{}` – Placeholder for each filename found

- `\;` – End of the exec command

### Finding files and displaying attributes:

```bash
find . -type f -exec ls -l '{}' \;
```
- Runs `ls -l` on each regular file found

### Finding files larger than 20 bytes:

```bash
find ~ -type f -size +20c -exec ls -hl {} \;
```
- `~` – Start from home directory

- `-type f` – Regular files only

- `-size +20c` – Size greater than 20 bytes (`c` = bytes)

- `-exec ls -hl {} \;` – List each file with human-readable sizes

**Size units for find:**

|Unit	|Meaning|
|-----|-------|
|`c	`|Bytes|
|`k	`|Kilobytes (1024 bytes)|
|`M`	|Megabytes|
|`G`	|Gigabytes|

**Example size syntax:**

|Expression	| Meaning|
|-----------|---------|
|`+20c`	|Greater than 20 bytes |
|`-20c` |	Less than 20 bytes |
|`20c`	|Exactly 20 bytes |

**Copying found files to another directory:**

```bash
find ~ -type f -size +20c -exec cp {} dir1 \;
```

- Copies all files larger than 20 bytes from home directory to dir1

### Find Command Examples Summary

|Command	| Action |
|---------|---------|
|`find ~ -name "file.txt"` |	Find file by name |
|`find . -type f`	| Find all regular files |
|`find . -type d` |	Find all directories |
|`find ~ -size +100k`	| Find files larger than 100KB |
|`find . -name "*.txt" -exec file {} \;` | Find all text files and show their type |
|`find ~ -type f -size +20c -exec ls -lh {} \;`	 | Find large files and list them |
|`find ~ -type f -size +20c -exec cp {} backup/ \;`	 |Find large files and copy to backup |

### Commands I Typed on Webminal
```bash
file linux.txt
file /dev/null
file -s /dev/sda2
whereis ls
whereis stdio.h
which php
echo $PATH
find ~ -name "linux.txt"
find . -type f -exec file '{}' \;
find . -type f -exec ls -l '{}' \;
find ~ -type f -size +20c -exec ls -hl {} \;
find ~ -type f -size +20c -exec cp {} dir1 \;
```
