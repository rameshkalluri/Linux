# Logs & Troubleshooting

The DevOps superpower isn't memorizing commands — it's a **calm, systematic method** for
finding out what's wrong.

## Where Logs Live
| Location | What |
|---|---|
| `/var/log/syslog` or `/var/log/messages` | general system log |
| `/var/log/auth.log` | logins, sudo, SSH |
| `/var/log/nginx/`, `/var/log/...` | per-application logs |
| `journalctl` | systemd services (Day 3) |
| `dmesg` | kernel ring buffer (hardware, OOM kills) |

```bash
tail -f /var/log/syslog                 # follow live
journalctl -u myapp -f                  # follow a service
journalctl -p err --since "1 hour ago"  # recent errors
dmesg -T | tail                         # kernel messages with timestamps
grep -i error /var/log/syslog | tail    # find errors
```

---

## Debugging Your Own Scripts
```bash
bash -x script.sh        # print each command as it runs
set -x                   # turn on tracing mid-script
set +x                   # turn it off
set -e                   # exit immediately on any error
set -u                   # error on undefined variables
set -o pipefail          # a failed command in a pipe fails the whole pipe
```
> Put `set -euxo pipefail` at the top while developing, then drop `x` for production.

---

## A Troubleshooting Playbook: "The site is down"
Work the layers from the outside in (uses tools from all 5 days):
1. **DNS** — does the name resolve? `dig +short example.com`
2. **Reachability** — is the host up? `ping -c3 <ip>`
3. **Port open?** — `nc -zv <host> 443` or `curl -I https://example.com`
4. **Service running?** — `systemctl status nginx`
5. **Listening?** — `ss -tlnp | grep 443`
6. **Logs** — `journalctl -u nginx -n50` / `tail -f /var/log/nginx/error.log`
7. **Resources** — `df -h` (disk full?), `free -h` (OOM?), `top` (CPU pegged?)
8. **Firewall** — OS (`ufw status`) **and** cloud (GCP VPC rules).

> Most outages are one of: **disk full**, **service crashed/not enabled**, **firewall**,
> **DNS**, or **bad config**. Check those first.

---

## General Method
1. **Reproduce** — what exactly fails, and how?
2. **Read the error** — the message usually names the problem.
3. **Check the obvious** — disk (`df -h`), service (`systemctl status`), logs.
4. **Isolate** — narrow down which layer/component (DNS? app? network?).
5. **Change one thing at a time**, re-test, and note what you did.

---

## TL;DR
- Logs: `/var/log/*`, `journalctl -u <svc> -f`, `dmesg` for kernel/OOM.
- Debug scripts with `bash -x` / `set -euxo pipefail`.
- "Site down" = walk DNS → ping → port → service → listening → logs → resources → firewall.
- Disk-full, crashed service, and firewall are the usual suspects.
