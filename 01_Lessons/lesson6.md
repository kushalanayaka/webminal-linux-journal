# Lesson 6 – Changing File Attributes

**Platform:** Webminal  
**Status:** Completed  
**Date:** 2026-04-28

---

## Commands Learned

### `dirname` – Extract Directory Path
- `dirname dir2/dir3/dir4/hi.txt` – Strips the non-directory suffix from the pathname
- Output: `dir2/dir3/dir4`
- Useful when you need only the directory part of a file path

### `basename` – Extract Filename
- `basename dir2/dir3/dir4/hi.txt` – Strips directory and suffix from pathname
- Output: `hi.txt`
- Gives only the last entry (filename) from the full path

**Difference between dirname and basename:**
| Command | Input | Output |
|---------|-------|--------|
| `dirname` | `dir2/dir3/dir4/hi.txt` | `dir2/dir3/dir4` |
| `basename` | `dir2/dir3/dir4/hi.txt` | `hi.txt` |

### `chmod` – Change File Permissions
- `chmod -v 666 file1.txt` – Changes permission of `file1.txt` to `666` with verbose output
- Output: `mode of file1.txt changed to 0666 (rw-rw-rw-)`
- `666` means read (4) + write (2) = 6 for owner, group, and others

**Understanding Permission Numbers:**
| Number | Permission |
|--------|------------|
| 4 | Read (r) |
| 2 | Write (w) |
| 1 | Execute (x) |
| 0 | No permission (-) |

**Permission breakdown for 666:**
| Position | Owner | Group | Others |
|----------|-------|-------|--------|
| Value | 6 | 6 | 6 |
| Binary | 4+2 | 4+2 | 4+2 |
| Result | rw- | rw- | rw- |

- `chmod a+rw file1.txt` – Allows read and write for **all** (user, group, others)
  - `a` = all (owner + group + others)
  - `+` = add permission
  - `rw` = read and write
- `chmod a-rw file2.txt` – Denies read and write for **all**
  - `-` = remove permission
  - After this command, no one (not even owner) can read or write the file
- `chmod u+rw file2.txt` – Allows read and write only for **user** (owner)
  - `u` = user/owner
  - Group and others have no permissions

**chmod symbolic mode format:**
| Symbol | Represents |
|--------|------------|
| `u` | User (owner) |
| `g` | Group |
| `o` | Others |
| `a` | All (ugo) |
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set exact permission |

**Examples:**
```bash
chmod u+x file.sh    # Owner can execute
chmod go-w file.txt  # Group and others cannot write
chmod a=r file.txt   # All can only read (exact permissions)

```
- `chmod -R 644 ~/dir` – Recursively changes permission for all files and subdirectories in `~/dir`

    - `644` means: owner = read+write (6), group = read (4), others = read (4)

    - `-R` (recursive) – Applies to all files inside the directory

### `chown` – Change File Owner (Root Only)
- `chown root file1.txt` – Changes the owner of `file1.txt` to `root`

- `chown root:staff file1.txt` – Changes owner to `root` and group to `staff`

- `chown root:staff -R ~/dir2` – Recursively changes owner and group for all files in `~/dir2`

- `chown --from=webminal:webminal root:staff -R ~/dir2` – Changes only files that belong to `webminal` user and `webminal` group to `root:staff`; other files are left unchanged

Note: `chown` can only be used by root user. Regular users will see: `chown: changing ownership of file1.txt: Operation not permitted`


### `chgrp` – Change Group Ownership (Root Only)
- `chgrp root file.txt` – Changes the group of `file.txt` to `root`

- `chgrp -hR root dir2` – Recursively changes group of `dir2` and all subfiles to `root`

    - `-h` (no dereference) – Changes the group of symbolic links themselves, not the files they point to

    - `-R` (recursive) – Applies to all files and subdirectories

Note: `chgrp` can only be used by root user. Regular users will see: `chgrp: changing group of 'file1.txt': Operation not permitted`

**Common permission combinations:**

|Permission	| Octal |	Meaning |
|-----------|-------|---------|
|rw-r--r--	| 644 |	Owner can read/write; others can only read |
|rw-rw-r--	| 664	|Owner and group can read/write; others read only|
|rwxr-xr-x	| 755	|Owner can do everything; others read/execute|
|rwx------	| 700	|Only owner can read/write/execute|
|rw-rw-rw-	| 666	|Everyone can read/write|

### Commands I Typed on Webminal
```bash

dirname dir2/dir3/dir4/hi.txt
basename dir2/dir3/dir4/hi.txt
chmod -v 666 file1.txt
chmod a+rw file1.txt
chmod a-rw file2.txt
chmod u+rw file2.txt
chmod -R 644 ~/dir
chown root file1.txt
chown root:staff file1.txt
chown root:staff -R ~/dir2
chgrp root file.txt
chgrp -hR root dir2
