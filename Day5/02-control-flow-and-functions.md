# Control Flow & Functions

Add logic so scripts can make decisions and repeat work.

## `if` / `else`
```bash
if [ "$ENV" = "prod" ]; then
    echo "Deploying to production"
elif [ "$ENV" = "staging" ]; then
    echo "Deploying to staging"
else
    echo "Unknown environment" >&2
    exit 1
fi
```

### Test operators
| Numbers | Strings | Files |
|---|---|---|
| `-eq` equal | `=` equal | `-f` is a file |
| `-ne` not equal | `!=` not equal | `-d` is a directory |
| `-gt` greater | `-z` empty | `-e` exists |
| `-lt` less | `-n` not empty | `-r`/`-w`/`-x` readable/writable/exec |

```bash
if [ -f /etc/nginx/nginx.conf ]; then echo "config exists"; fi
if [ "$count" -gt 100 ]; then echo "too many"; fi
[[ "$name" == ramesh* ]] && echo "match"   # [[ ]] supports patterns & &&/||
```
> Prefer `[[ ... ]]` in bash — it's safer with unquoted vars and supports `&&`, `||`, `==` patterns.

---

## `case` — cleaner multi-branch
```bash
case "$1" in
    start)   echo "starting" ;;
    stop)    echo "stopping" ;;
    restart) echo "restarting" ;;
    *)       echo "usage: $0 {start|stop|restart}"; exit 1 ;;
esac
```

---

## Loops
```bash
# for over a list
for env in dev staging prod; do
    echo "Processing $env"
done

# for over files
for f in /var/log/*.log; do
    echo "Found $f"
done

# C-style
for i in {1..5}; do echo "$i"; done

# while
count=0
while [ "$count" -lt 3 ]; do
    echo "count=$count"
    count=$((count + 1))
done

# read a file line by line
while IFS= read -r line; do
    echo "Line: $line"
done < input.txt
```
> `$(( ... ))` does integer arithmetic. Use `break` to exit a loop, `continue` to skip.

---

## Functions
```bash
log() {
    echo "[$(date +%H:%M:%S)] $*"
}

deploy() {
    local target="$1"          # 'local' keeps the var inside the function
    log "Deploying to $target"
    return 0                    # return an exit code
}

log "starting"
deploy "prod"
```
- Args inside a function are `$1`, `$2`… just like a script.
- Use `local` for function variables to avoid clobbering globals.

---

## A Real Mini-Script: health check
```bash
#!/usr/bin/env bash
set -euo pipefail

URL="${1:-http://localhost:8080/health}"

code=$(curl -s -o /dev/null -w "%{http_code}" "$URL")
if [ "$code" = "200" ]; then
    echo "OK ($code)"
    exit 0
else
    echo "UNHEALTHY ($code)" >&2
    exit 1
fi
```

---

## TL;DR
- `if [ ... ]` with `-eq/-gt` (numbers), `=`/`-z` (strings), `-f/-d` (files); prefer `[[ ]]`.
- `case` for multi-branch; `for`/`while` for loops; `$(( ))` for math.
- Functions take `$1 $2…`; use `local`; `return` an exit code.
