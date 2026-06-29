# Cron & Automation

Run scripts automatically — on a schedule or at boot.

## `cron` — scheduled jobs
```bash
crontab -e        # edit YOUR cron jobs
crontab -l        # list them
crontab -r        # remove all (careful)
sudo crontab -e   # root's cron
```

### The 5-field schedule
```
┌── minute (0–59)
│ ┌── hour (0–23)
│ │ ┌── day of month (1–31)
│ │ │ ┌── month (1–12)
│ │ │ │ ┌── day of week (0–7, 0/7 = Sunday)
│ │ │ │ │
* * * * *  command-to-run
```

### Examples
```bash
0 2 * * *      /opt/scripts/backup.sh           # every day at 02:00
*/5 * * * *    /opt/scripts/healthcheck.sh       # every 5 minutes
0 0 * * 0      /opt/scripts/weekly.sh            # Sundays at midnight
30 8 1 * *     /opt/scripts/report.sh            # 08:30 on the 1st of each month
@reboot        /opt/scripts/startup.sh           # once at boot
```
> Use [crontab.guru](https://crontab.guru) to sanity-check schedules.

### Cron gotchas (these bite everyone)
- Cron runs with a **minimal environment** — no `$PATH` like your shell. Use **absolute
  paths** (`/usr/bin/python3`, not `python3`).
- **Capture output** or you'll never see errors:
  ```bash
  0 2 * * * /opt/scripts/backup.sh >> /var/log/backup.log 2>&1
  ```
- Make sure the script is executable (`chmod +x`) and has a shebang.

---

## A Backup Script + Cron (classic DevOps task)
```bash
#!/usr/bin/env bash
set -euo pipefail

SRC="/var/www"
DEST="/backups"
STAMP=$(date +%Y%m%d-%H%M%S)

mkdir -p "$DEST"
tar -czf "$DEST/www-$STAMP.tar.gz" "$SRC"

# keep only the 7 most recent backups
ls -1t "$DEST"/www-*.tar.gz | tail -n +8 | xargs -r rm --

echo "Backup complete: www-$STAMP.tar.gz"
```
Schedule it:
```bash
crontab -e
# add:
0 2 * * * /opt/scripts/backup.sh >> /var/log/backup.log 2>&1
```

---

## `systemd` Timers (modern alternative)
More powerful than cron (logging via journald, dependencies, missed-run handling).
A timer unit pairs with a service unit:
```ini
# /etc/systemd/system/backup.timer
[Timer]
OnCalendar=*-*-* 02:00:00
Persistent=true

[Install]
WantedBy=timers.target
```
```bash
sudo systemctl enable --now backup.timer
systemctl list-timers          # see all timers and next run
```

---

## TL;DR
- `crontab -e`; 5 fields = min hour dom month dow; `*/5` = every 5.
- Cron has a minimal env → use **absolute paths** and redirect output to a log.
- `systemd` timers are the modern, better-logged alternative for important jobs.
