# Lesson 5 – Manipulate or Parse File Contents

**Platform:** Webminal  
**Status:** Completed  
**Date:** 2026-04-28

---

## Commands Learned

### `grep` – Search for Patterns in Files
- `grep "linux" hello` – Searches for the word `linux` in file named `hello`
- Shows the entire line where the match is found
- `grep -r 'Hello' .` – Recursively searches for `Hello` in all files inside current directory (`.` means current directory)
- `grep -i 'lINUX' hello` – Ignores case (matches `linux`, `LINUX`, `Linux`, etc.)
- `grep -n 'linux' hello` – Shows line numbers along with matching lines
- `grep -v 'world' hello` – Shows lines that do **not** match the pattern (inverse match)

**Useful grep options:**
| Option | Meaning |
|--------|---------|
| `-r` | Recursive (search directories) |
| `-i` | Ignore case |
| `-n` | Show line numbers |
| `-v` | Invert match (show non-matching lines) |
| `-w` | Match whole words only |

### `wc` – Word Count
- `wc hello` – Counts lines, words, and bytes in a file
- Output format: `lines words bytes filename`
- `wc -L hello` – Shows the length of the **longest line** in the file

**wc options:**
| Option | Meaning |
|--------|---------|
| `-l` | Count lines only |
| `-w` | Count words only |
| `-c` | Count bytes only |
| `-L` | Show length of longest line |

### `cut` – Extract Columns from Files
- `cut -f1 -d' ' new.txt` – Extracts the first column from `new.txt`
- `-f1` (field) – Specifies which column to extract (1 = first column)
- `-d' '` (delimiter) – Specifies what separates the columns (space in this case)
- `cut -f3 -d' ' new.txt` – Extracts the third column

**How cut works:**
```bash
# File content: "col1 col2 col3"
# With -d' ' (space as delimiter):
# -f1 gives "col1"
# -f2 gives "col2"
# -f3 gives "col3"
```
### Common delimiters:

| Delimiter |	Used for |
|-------|-----------|
|-d' '	| Space-separated files|
|-d','	| CSV files (comma-separated)|
|-d':'	| Passwd file or colon-separated data|
|-d$'\t'	| Tab-separated files|

### `paste` – Merge Files Line by Line
- `paste hello new.txt` – Merges `hello` and `new.txt` side by side (line 1 of hello next to line 1 of new.txt)

- `paste -s hello new.txt` – Pastes one file at a time (serially)

   - `-s` (serial) – Pastes all lines of a file into a single line

### `sort` – Sort File Contents
-`sort new.txt` – Sorts the content of `new.txt` in alphabetical order

- By default, sorts line by line in ascending order
  
**Useful sort options:**
  
|Option |	Meaning |
|------|---------|
|-r	| Reverse order |
|-n |	Sort numerically (not alphabetically) |
|-u	| Remove duplicate lines |

### `diff` – Compare Files Line by Line
- `diff hello linux.txt` – Compares two files and shows differences

- Output symbols:

    - `<` – Line from the first file (`hello`)

    - `>` – Line from the second file (`linux.txt`)

- `diff3 hello new.txt linux.txt` – Compares three files at once


### Difference Between grep Options

|Command	 |Effect|
|---------|---------|
|grep "linux" file	| Find lines with "linux" |
|grep -i "LINUX" file	| Find lines with "linux", "LINUX", "Linux", etc.|
|grep -n "linux" file |	Show line numbers |
|grep -v "linux" file	| Show lines without "linux"|
|grep -r "linux" .	| Search all files in current directory|

### Commands I Typed on Webminal

```bash
grep "linux" hello
grep -r 'Hello' .
grep -i 'lINUX' hello
grep -n 'linux' hello
grep -v 'world' hello
wc hello
wc -L hello
echo -e "col1 col2 r1\ncol5 col6 r2\ncol3 col4 r3" >> new.txt
echo -e "Hello\nlinux\nProgrammers paradise" >> linux.txt
cut -f1 -d' ' new.txt
cut -f3 -d' ' new.txt
paste hello new.txt
paste -s hello new.txt
sort new.txt
diff hello linux.txt
diff3 hello new.txt linux.txt
