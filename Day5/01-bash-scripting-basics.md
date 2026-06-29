# Bash Scripting Basics

A shell script is just a file of commands you'd type anyway — saved and made repeatable.

## Anatomy of a Script
```bash
#!/usr/bin/env bash          # shebang: run this with bash
set -euo pipefail            # safety: exit on error, unset vars, pipe failures

echo "Hello, $(whoami)!"     # command substitution
```
Make it executable and run it (Day 2 permissions):
```bash
chmod +x hello.sh
./hello.sh
```
> The **shebang** (`#!`) on line 1 tells the OS which interpreter to use.
> `set -euo pipefail` at the top is a near-universal best practice — it makes scripts
> fail fast instead of silently continuing after an error.

---

## Variables
```bash
name="Ramesh"           # no spaces around =
echo "$name"            # use with $ (quote it!)
echo "${name}_suffix"   # braces when concatenating

count=$(ls | wc -l)     # capture command output
readonly PI=3.14        # constant
```
- Always **quote** variables (`"$var"`) to handle spaces/empties safely.
- `$(...)` runs a command and substitutes its output.

## Arguments & Special Variables
```bash
./deploy.sh prod v2
```
| Var | Meaning |
|---|---|
| `$0` | script name |
| `$1`, `$2`… | first, second argument (`prod`, `v2`) |
| `$#` | number of arguments |
| `$@` | all arguments |
| `$?` | exit code of the last command |
| `$$` | current process PID |

```bash
ENV="$1"
VERSION="${2:-latest}"   # default to 'latest' if $2 is unset
echo "Deploying $VERSION to $ENV"
```

---

## Exit Codes
Every command returns a code: **0 = success**, non-zero = failure.
```bash
mkdir /tmp/x
echo $?            # 0 if it worked

exit 1             # end the script with a failure code
```
Exit codes are how automation (cron, CI, GCP health checks) knows if your script
succeeded. Always `exit` non-zero on failure.

---

## Input
```bash
read -p "Enter name: " name
read -sp "Password: " pass    # -s hides input
```

---

## TL;DR
- Start scripts with `#!/usr/bin/env bash` and `set -euo pipefail`.
- Assign with `var=value` (no spaces); use `"$var"`; capture with `$(cmd)`.
- Args are `$1 $2 …`, count `$#`, last exit code `$?`.
- Return `0` on success, non-zero on failure — automation depends on it.
