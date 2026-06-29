# System Monitoring

When a server is slow or full, these tools tell you why. The DevOps mental model:
**CPU, Memory, Disk, Network** — check each in turn.

## Memory (RAM & Swap)
```bash
free            # show memory usage
free -h         # human-readable (Ki/Mi/Gi)
free -mt        # in MB, with a total line
free -s 2 -c 5  # sample every 2s, 5 times
```
Look at the **available** column (not just "free") — Linux uses spare RAM for cache,
which is normal and reclaimable.

## Disk Space
```bash
df              # filesystem disk usage
df -h           # human-readable (powers of 1024, e.g. 1023M)
df -H           # powers of 1000 (e.g. 1.1G)
df -i           # inode usage (can be "full" even when space remains)
```

## Directory Sizes
```bash
du -h file/dir            # space used, human-readable
du -hs .                  # total size of current directory (summary)
du -h --max-depth=1 .     # size of each subdirectory
du -hs * | sort -rh | head    # biggest items here, largest first
```
> "Disk full" drill: `df -h` to find the full mount → `du -hs /var/* | sort -rh` to
> find the culprit → usually old logs in `/var/log`.

---

## CPU & Load
```bash
uptime          # current time, how long up, and load averages (1, 5, 15 min)
top             # live CPU/mem per process
vmstat 1        # CPU/memory/IO stats every second
nproc           # number of CPU cores
```
> **Load average** ≈ number of processes wanting CPU. Rule of thumb: compare to core
> count. Load `4.0` on a 4-core box ≈ fully busy; `8.0` on 4 cores = overloaded.

## Quick Hardware/OS Facts
```bash
uname -a              # kernel & architecture
cat /etc/os-release   # distro name and version
lscpu                 # CPU details
lsblk                 # block devices / disks
```

---

## Watch Something Change
```bash
watch -n 2 df -h          # re-run df every 2 seconds
watch -n 1 'free -h'      # live memory
```

---

## TL;DR
- Memory → `free -h` (watch **available**); Disk → `df -h` then `du -hs * | sort -rh`.
- CPU/Load → `uptime` + `top`; compare load average to `nproc` cores.
- `watch` repeats any command on an interval to see trends.
