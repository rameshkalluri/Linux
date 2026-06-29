# Navigation & File Basics

The handful of commands you'll use thousands of times.

## Looking Around
```bash
pwd                 # where am I (print working directory)
ls                  # list files
ls -l               # long listing: permissions, owner, size, date
ls -la              # also show hidden files (those starting with .)
ls -lh              # human-readable sizes (K, M, G)
ls -lt              # sort by modification time (newest first)
ls -ltr             # ...reversed (oldest first) — great for newest log at bottom
tree                # visual directory tree (may need: apt install tree)
```
> `-r` reverses sort order, `-t` sorts by time — combine flags like `ls -ltr`.

## Moving Around
```bash
cd /path/to/dir     # go to a directory
cd ..               # up one level
cd ~                # home directory
cd -                # previous directory
```

---

## Creating Things
```bash
touch ramesh        # create an empty file (or update its timestamp)
mkdir logs          # create a directory
mkdir -p a/b/c      # create nested dirs, no error if they exist
```

## Copying, Moving, Renaming
```bash
cp file.txt backup.txt      # copy a file
cp -r dir1 dir2             # copy a directory recursively
mv old.txt new.txt          # rename
mv file.txt /tmp/           # move into another directory
```

## Deleting (careful!)
```bash
rm file.txt          # delete a file
rm -r dir            # delete a directory and its contents
rm -rf dir           # force, no prompts — ⚠️ irreversible, no recycle bin
```
> ⚠️ There is **no undo**. Double-check the path before `rm -rf`. Never run
> `rm -rf /` or `rm -rf $VAR/` where `$VAR` could be empty.

---

## Peeking at File Contents
```bash
cat file.txt         # print whole file
less file.txt        # scrollable viewer (q to quit, / to search)
head file.txt        # first 10 lines
head -n 20 file.txt  # first 20 lines
tail file.txt        # last 10 lines
tail -f app.log      # follow a log live (Ctrl+C to stop) — essential for DevOps
```

---

## Wildcards (Globbing)
```bash
ls *.log             # everything ending in .log
ls app-?.txt         # single-character match: app-1.txt, app-a.txt
ls log[1-3].txt      # ranges: log1.txt, log2.txt, log3.txt
cp *.conf /backup/   # operate on many files at once
```

---

## TL;DR
- `pwd` / `ls -ltr` / `cd` to orient and move.
- `touch`, `mkdir -p`, `cp -r`, `mv`, `rm -r` to manage files.
- `tail -f` to watch logs live; `less` to read big files.
- Wildcards (`*`, `?`, `[ ]`) let one command hit many files.
