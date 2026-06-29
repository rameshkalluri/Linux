# Text Processing (grep, sed, awk & friends)

Logs, config files, command output — DevOps is mostly text. These tools let you search,
slice, and transform it.

## `grep` — search text
```bash
grep "ERROR" app.log          # lines containing ERROR
grep -i "error" app.log       # ignore case
grep -r "TODO" .              # recursive search in a directory
grep -n "fail" app.log        # show line numbers
grep -v "DEBUG" app.log       # invert: lines WITHOUT DEBUG
grep -c "ERROR" app.log       # count matching lines
grep -E "warn|error" app.log  # extended regex (OR)
grep -A3 -B2 "panic" app.log  # 3 lines After, 2 Before each match
```

## `wc` — count
```bash
wc -l file        # number of lines
wc -w file        # words
wc -c file        # bytes
```

## `cut` — extract columns
```bash
cut -d: -f1 /etc/passwd       # field 1, ':' delimiter → usernames
cut -d, -f2,3 data.csv        # fields 2 and 3 of a CSV
cut -c1-10 file               # characters 1–10 of each line
```

## `sort` & `uniq`
```bash
sort file.txt                 # alphabetical
sort -n nums.txt              # numeric
sort -rn nums.txt             # numeric, reverse (biggest first)
sort -k2 file                 # sort by 2nd column
sort file | uniq              # remove adjacent duplicates (sort first!)
sort file | uniq -c           # count occurrences of each line
sort file | uniq -c | sort -rn   # frequency, most common at top
```
> Classic combo — top 10 IPs hitting a server:
> ```bash
> cut -d' ' -f1 access.log | sort | uniq -c | sort -rn | head
> ```

---

## `awk` — pattern scanning & column processing
Splits each line into fields (`$1`, `$2`, … `$0` = whole line). Syntax: `awk 'pattern {action}' file`.
```bash
awk '{print $1}' access.log              # first field of every line
awk -F: '{print $1}' /etc/passwd         # custom delimiter ':'
awk '{print $1, $3}' file                # multiple fields
awk '$3 > 100 {print $0}' data.txt       # rows where col 3 > 100
awk '/ERROR/ {count++} END {print count}' app.log   # count ERRORs
awk '{sum += $1} END {print sum}' nums   # sum a column
```
Useful built-ins: `NR` (line number), `NF` (number of fields), `FS` (field separator),
and `BEGIN {} ... END {}` blocks for headers/totals.

---

## `sed` — stream editor (find/replace, delete)
```bash
sed 's/old/new/' file          # replace FIRST match per line
sed 's/old/new/g' file         # replace ALL matches (global)
sed -i 's/old/new/g' file      # edit the file IN PLACE
sed -n '2,4p' file             # print only lines 2–4
sed '/^#/d' file               # delete comment lines (start with #)
sed '/^$/d' file               # delete blank lines
```
> `-i` writes changes back to the file — test without it first to preview.

---

## Putting It Together
```bash
# Most frequent error messages in a log
grep ERROR app.log | cut -d' ' -f4- | sort | uniq -c | sort -rn | head

# Memory of each running process, biggest first
ps aux | awk '{print $4, $11}' | sort -rn | head
```

---

## TL;DR
- `grep` finds lines (`-i -r -v -n -c -A/-B`).
- `cut`/`awk` extract columns; `awk` also does math & conditions.
- `sort | uniq -c | sort -rn` = the universal "count & rank" pipeline.
- `sed` does find/replace and line edits; `-i` writes in place.
