# Linux in 5 Days — DevOps Learning Plan

A focused, hands-on plan to go from zero to **job-ready Linux fundamentals for DevOps**.
Budget **3–4 hours/day** (≈1 hr theory + 2–3 hrs hands-on at the terminal).

> Why Linux for DevOps? Almost every server, container, CI runner, and cloud VM you
> will touch runs Linux. On **GCP**, Compute Engine VMs, GKE nodes, and Cloud Shell are
> all Linux. Master the shell and you can operate, automate, and debug anything.

---

## Prerequisites (Day 0 — Setup)
Pick **one** practice environment — all are free:
- **GCP Compute Engine** — launch a small `e2-micro` VM (free tier) and SSH in. Ties
  directly into the [GCP plan](../GCP/GCP-10-Day-Learning-Plan.md). **Stop/delete when done.**
- **GCP Cloud Shell** — instant Linux terminal in the browser, zero setup.
- **WSL2** on Windows — `wsl --install` then pick Ubuntu.
- **VirtualBox / Multipass** — local Ubuntu VM.

Bookmark: `man7.org`, [ExplainShell](https://explainshell.com), `tldr.sh`.

---

## Day 1 — Linux Fundamentals & The Filesystem
**Theory**
- What Linux is: kernel vs shell vs distro; common distros (Ubuntu, Debian, RHEL, Alpine).
- The Filesystem Hierarchy Standard (FHS): `/`, `/etc`, `/var`, `/home`, `/bin`, `/proc`…
- Absolute vs relative paths; everything is a file.

**Hands-on**
- Navigate the tree: `pwd`, `ls`, `cd`, `tree`.
- Create/inspect files & dirs: `touch`, `mkdir`, `cp`, `mv`, `rm`, `cat`.
- Read help: `man`, `--help`, `tldr`.

📂 [Day 1 notes](Day1/README.md)

---

## Day 2 — Files, Text Processing & Permissions
**Theory**
- Viewing/editing: `cat`, `less`, `head`, `tail`, `nano`, `vim`.
- Permissions & ownership model: `rwx`, octal, `chmod`, `chown`, `umask`.
- Pipes & redirection: `|`, `>`, `>>`, `<`, `2>`, `/dev/null`.

**Hands-on**
- Process logs with `grep`, `sed`, `awk`, `cut`, `sort`, `uniq`, `wc`.
- Find files with `find` and `locate`.
- Fix permissions on a script and run it.

📂 [Day 2 notes](Day2/README.md)

---

## Day 3 — Users, Processes & Packages
**Theory**
- Users & groups, `sudo`, `/etc/passwd`, `/etc/sudoers`.
- Processes & jobs: `ps`, `top`/`htop`, `kill`, `nice`, foreground/background.
- System monitoring: `free`, `df`, `du`, `uptime`, `vmstat`.
- Package managers: `apt` (Debian/Ubuntu), `dnf`/`yum` (RHEL).
- `systemd` services: `systemctl`, `journalctl`.

**Hands-on**
- Create a user & group, grant sudo, switch users.
- Install Nginx, start/enable the service, check status & logs.
- Find and kill a runaway process.

📂 [Day 3 notes](Day3/README.md)

---

## Day 4 — Networking & Remote Access
**Theory**
- TCP/IP, ports, DNS basics.
- Network tools: `ip`, `ping`, `curl`, `wget`, `ss`, `netstat`, `dig`, `nslookup`, `traceroute`, `nc`.
- SSH deep dive: key-based auth, `scp`, `~/.ssh/config`.
- Firewalls: `ufw`, `iptables`/`nftables`.

**Hands-on**
- SSH into a GCP VM with a key pair; copy a file with `scp`.
- Open a port in the firewall and verify with `curl`/`ss`.
- Troubleshoot "site is down" with `dig` + `curl` + `ss`.

📂 [Day 4 notes](Day4/README.md)

---

## Day 5 — Shell Scripting & Automation (DevOps)
**Theory**
- Bash scripting: shebang, variables, args, exit codes.
- Control flow: `if`, `case`, `for`, `while`, functions.
- Scheduling & services: `cron`, `systemd` timers, startup scripts.
- Logs & troubleshooting methodology; tying it all to GCP.

**Hands-on**
- Write a backup script (tar + timestamp) and schedule it with `cron`.
- Write a health-check script that curls an endpoint and exits non-zero on failure.
- Use a **GCE startup script** to bootstrap a VM (install + run a web server).

📂 [Day 5 notes](Day5/README.md)

---

## Recommended Resources
- **The Linux Command Line** (free book by William Shotts) — primary read.
- **OverTheWire: Bandit** — gamified terminal practice.
- **ExplainShell.com** — paste any command to decode it.
- **tldr pages** — practical command examples.
- **Cert path**: *Linux Foundation LFCS* or *RHCSA* (great for DevOps roles).

## Daily Habit Checklist
- [ ] 1 hr concept reading
- [ ] 2–3 hrs hands-on in a real terminal
- [ ] Note every new command + a one-line description
- [ ] Re-type commands from memory (don't copy-paste)
- [ ] **Stop/delete any billable cloud VMs**
