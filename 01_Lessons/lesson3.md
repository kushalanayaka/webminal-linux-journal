# Lesson 3 – Copy, Rename, Delete Files

**Platform:** Webminal  
**Status:** Completed  
**Date:** 2026-04-27

---

## Commands Learned

### `du` – Disk Usage
- `du` – Displays the disk usage of the current directory
- `du -xh ~` – Shows disk usage of home directory
  - `-h` (human readable) – Shows sizes as KB, MB, GB instead of bytes
  - `-x` (exclude) – Stays on one filesystem, does not enter other filesystems
  - `~` – Your home directory
- `du --max-depth 3 ~` – Limits the output to 3 levels deep (shows only directories up to 3 levels down)
- Disk usage increases when you copy files into a directory

### `cp` – Copy Files and Directories
- `cp -v hello.txt dir2` – Copies `hello.txt` into `dir2` directory with verbose output
- `cp -v hello.txt dir2/file2.txt` – Copies `hello.txt` into `dir2` and renames it to `file2.txt` in one command
- `cp -vr dir2/*.txt dir2/dir3` – Copies all files ending with `.txt` from `dir2` into `dir2/dir3`
  - `-r` (recursive) – Needed for copying directories
  - `*` (wildcard) – Matches anything
  - `*.txt` – Matches all files ending with `.txt`
- `cp -vr dir2/dir3 .` – Copies directory `dir3` to the current directory (`.` means current location)
- `cp` leaves the original file untouched – you end up with two copies

### `md5sum` – Calculate File Checksum
- `md5sum hello.txt` – Calculates a unique checksum (hash) for the file
- Example output: `b8d5079c5d6a9dbb3294b31d318d74c0`
- Used to verify file integrity when copying or downloading files
- If two files have the same `md5sum`, they are identical
- If a file gets corrupted, the `md5sum` changes
- To verify a copied file is identical: `md5sum hello.txt` and `md5sum dir2/hello.txt` should show the same hash

### `mv` – Move or Rename Files and Directories
- `mv hello.txt dir2/dir3/dir4/hi.txt` – Moves `hello.txt` into `dir4` and renames it to `hi.txt`
- After `mv`, the original file no longer exists in the source location (`ls` will not show `hello.txt`)
- `mv dir2/*.txt dir5` – Moves all `.txt` files from `dir2` into `dir5`
- `mv dir5 dir50` – Renames directory `dir5` to `dir50`
- Difference between `cp` and `mv`:
  - `cp` = copy-paste (two copies exist)
  - `mv` = cut-paste (one copy exists, original is gone)
- `mv` does not need `-r` flag for directories (unlike `cp` and `rm`)

### `ln` – Create Links
- `ln dir2/dir3/dir4/hi.txt hello` – Creates a hard link named `hello` pointing to `hi.txt`

**Hard Links:**
- Multiple names pointing to the same inode (same data on disk)
- Both `stat hello` and `stat dir2/dir3/dir4/hi.txt` show the same inode number
- Link count shows `2` because two names point to the same data
- Deleting one link does not delete the data until the last link is removed

**Soft Links (Symbolic Links):**
- `ln -s dir2/dir3/dir4/hi.txt softlink` – Creates a soft link (like a shortcut)
- `stat softlink` shows a new inode (different from the original)
- Link count remains `1`
- If you delete the original file, the soft link becomes broken (points to nothing)

**Understanding Inode:**
- An inode is a unique number assigned to each file or directory
- It stores metadata (permissions, timestamps, location on disk) but not the filename
- Hard links share the same inode; soft links have their own inode

### `rm` – Remove Files
- `rm -i file2.txt` – Removes `file2.txt` with confirmation prompt (`y` to confirm)
- `rm -ri dir50/*` – Recursively removes all contents of `dir50` with confirmation for each file
  - `-r` (recursive) – Needed to remove directories and their contents
  - `-i` (interactive) – Asks for confirmation before each removal
- `rm -rf junk/*` – Force removes all contents of `junk` without any confirmation
  - `-r` (recursive) – Removes directories and their contents
  - `-f` (force) – Ignores warnings and never prompts
  - ⚠️ **Extremely dangerous** – `rm -rf` can delete important files by mistake. Always double-check before running.

### `rmdir` – Remove Empty Directory
- `rmdir dir50` – Removes `dir50` only if it is completely empty
- If the directory has any files or subdirectories, `rmdir` will fail (use `rm -r` instead)

---

## Difference Between `cp` and `mv`

| Command | Action | Original file remains? |
|---------|--------|----------------------|
| `cp` | Copy | Yes (two copies exist) |
| `mv` | Move | No (moved to new location) |

---

## Difference Between Hard Links and Soft Links

| Feature | Hard Link | Soft Link (Symbolic Link) |
|---------|-----------|---------------------------|
| Command | `ln target linkname` | `ln -s target linkname` |
| Inode | Same as target | New inode |
| Link count | Increases | Stays 1 |
| Works if target is deleted | Yes (data still exists) | No (becomes broken) |
| Can link to directory | No (usually) | Yes |
| Can cross filesystems | No | Yes |

---

## Commands I Typed on Webminal

```bash
du
du -xh ~
du --max-depth 3 ~
cp -v hello.txt dir2
cp -v hello.txt dir2/file2.txt
cp -vr dir2/*.txt dir2/dir3
cp -vr dir2/dir3 .
ls
md5sum hello.txt
md5sum dir2/hello.txt
mv hello.txt dir2/dir3/dir4/hi.txt
mkdir dir5
mv dir2/*.txt dir5
mv dir5 dir50
ln dir2/dir3/dir4/hi.txt hello
stat hello
stat dir2/dir3/dir4/hi.txt
ln -s dir2/dir3/dir4/hi.txt softlink
stat softlink
rm -i file2.txt
rm -ri dir50/*
rm -rf junk/*
rmdir dir50
