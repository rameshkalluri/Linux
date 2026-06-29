# Processes & Jobs

Every running program is a **process** with a PID (process ID) and a parent (PPID).

## Listing Processes
```bash
ps                       # processes in current shell
ps -ef                   # ALL processes, full format (parent-child via PPID)
ps aux                   # ALL processes, BSD style (with %CPU, %MEM)
ps -ef | grep nginx      # find a specific process
pgrep -a nginx           # find PIDs by name
pstree                   # process tree visual
```
> `ps aux` columns you care about: **PID**, **%CPU**, **%MEM**, **STAT**, **COMMAND**.

## Live Monitoring
```bash
top                      # live process view (q to quit)
htop                     # nicer, colored, scrollable (install: apt install htop)
```
In `top`: press `M` sort by memory, `P` sort by CPU, `k` kill a PID.

---

## Killing Processes
```bash
kill 1234                # ask process 1234 to stop (SIGTERM, graceful)
kill -9 1234             # force kill (SIGKILL, last resort)
kill -15 1234            # explicit SIGTERM (default)
pkill nginx              # kill by name
killall python3          # kill all matching processes
```
> Try `kill` (15, graceful) first so the app can clean up. Use `kill -9` only if it
> won't die — it can't flush/save and may leave things in a bad state.

### Common Signals
| Signal | Number | Meaning |
|---|---|---|
| SIGTERM | 15 | polite "please stop" (default) |
| SIGKILL | 9 | forceful, cannot be caught |
| SIGHUP | 1 | hang up — many daemons reload config on this |
| SIGINT | 2 | what `Ctrl+C` sends |

---

## Foreground & Background Jobs
```bash
./long-task.sh           # runs in foreground (blocks your shell)
./long-task.sh &         # run in background
Ctrl+Z                   # suspend the foreground job
bg                       # resume suspended job in background
fg                       # bring background job to foreground
jobs                     # list this shell's jobs
nohup ./task.sh &        # keep running after you log out
disown -h %1             # detach a job from the shell
```
> For long-running remote tasks, prefer **`tmux`** or **`screen`** — they survive SSH
> disconnects far better than `nohup`.

## Priority (`nice`)
```bash
nice -n 10 ./batch.sh    # start with lower priority (nicer to others)
renice -n 5 -p 1234      # change priority of a running process
```

---

## TL;DR
- `ps -ef` / `ps aux` to list, `top`/`htop` to watch live.
- `kill <pid>` (graceful) → `kill -9 <pid>` (force) as last resort.
- `&`, `Ctrl+Z`, `bg`, `fg`, `jobs` manage foreground/background; `tmux` for remote.
