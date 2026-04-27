# Lesson 1 – Basic Commands to Navigate Directories

**Platform:** Webminal  
**Status:** Completed  
**Date:** 2026-04-27

---

## Commands Learned

### `pwd` – Print Working Directory
- Shows the current directory you are in
- Gives absolute path (e.g., `/home/username/dir1`), not relative

### `mkdir` – Make Directory
- `mkdir -v dir1` – Creates `dir1` with verbose output (shows confirmation)
- `mkdir -vp dir2/dir3/dir4` – Creates parent and child directories in one command
- Without `-p`, if `dir2` doesn't exist, `mkdir dir2/dir3` will fail with error
- With `-p`, no error even if directory already exists

### `ls` – List Directories
- `ls` – Lists directories and files (hidden files starting with `.` are not shown)
- `ls -R` – Lists recursively (includes all child directories and their contents)
- `ls` alone does not show subdirectory contents; `ls -R` does

### `cd` – Change Directory
- `cd dir2` – Navigates into `dir2` (must exist)
- `cd ..` – Goes back one directory (parent directory)
- `cd -` – Goes back to the previous directory you came from
- `cd` or `cd ~` – Goes directly to home directory
- `cd -` only works if you had a previous directory; first `cd -` after login does nothing

---

## Path Types
- **Absolute path:** Starts from root (`/`). Example: `/home/username/dir1`
- **Relative path:** Starts from current location. Example: `dir2/dir3`
- `mkdir -vp dir2/dir3/dir4` uses a **relative path**

---

## Difference Between `ls` and `ls -R`

| Command | Shows |
|---------|-------|
| `ls` | Only current directory contents |
| `ls -R` | Current + all subdirectories |

---

## Difference Between `cd ..` and `cd -`

| Command | Where it goes |
|---------|---------------|
| `cd ..` | One level up (parent) |
| `cd -` | Previous directory you were in (anywhere, not just parent) |

---

## Behind the Scenes
- `cd -` uses `$OLDPWD` environment variable (stores last directory)
- `pwd` reads from `$PWD` environment variable

---

## Commands I Typed on Webminal

```bash
pwd
mkdir -v dir1
mkdir -vp dir2/dir3/dir4
ls
ls -R
cd dir2
cd ..
cd -
cd
