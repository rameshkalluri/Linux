# Finding Files

## `find` — search the filesystem live (most powerful)
```bash
find / -name "Foo.txt"            # by exact name, from root
find . -iname "*.log"             # current dir, case-insensitive, wildcard
find /var -type f -name "*.gz"    # only files (f); -type d for directories
find . -type d -name "node_modules"   # find directories
```

### Filter by size, time, owner
```bash
find / -size +100M                # files larger than 100 MB
find . -mtime -1                  # modified in the last 24 hours
find . -mtime +7                  # not modified in over 7 days
find /home -user ramesh           # owned by a user
find . -perm 777                  # files with specific permissions
```

### Act on results with `-exec` or `-delete`
```bash
find . -name "*.tmp" -delete                      # delete matches
find /var/log -name "*.log" -mtime +30 -delete    # clean old logs
find . -name "*.sh" -exec chmod +x {} \;          # chmod each match
find . -name "*.txt" -exec grep -l "TODO" {} \;   # files containing TODO
```
> `{}` is each found path; `\;` ends the `-exec`. This is a daily DevOps tool for
> cleanup and bulk operations.

---

## `locate` — instant search via a prebuilt index
```bash
sudo updatedb          # refresh the index (run once / via cron)
locate nginx.conf      # near-instant results
```
> Faster than `find` but uses a database that can be **stale**. Use `find` for live
> accuracy, `locate` for speed.

## `which` / `type` — find a command's binary
```bash
which python3          # /usr/bin/python3
type -a ls             # all locations + whether it's an alias
```

---

## TL;DR
- `find` = live, flexible search by name/size/time/owner, with `-exec`/`-delete`.
- `locate` = fast but indexed (run `updatedb`); `which` finds command binaries.
- Memorize: `find . -name "*.log" -mtime +7 -delete` for log cleanup.
