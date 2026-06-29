# systemd & Services

`systemd` is the **init system** (PID 1) on most modern distros. It starts, stops, and
supervises background services (daemons) like Nginx, Docker, and SSH.

## `systemctl` — control services
```bash
sudo systemctl start nginx        # start now
sudo systemctl stop nginx         # stop now
sudo systemctl restart nginx      # stop + start
sudo systemctl reload nginx       # reload config without dropping connections
sudo systemctl enable nginx       # start automatically on boot
sudo systemctl disable nginx      # don't start on boot
sudo systemctl enable --now nginx # enable + start in one step
```

## Check status
```bash
systemctl status nginx       # running? enabled? recent log lines
systemctl is-active nginx    # active / inactive
systemctl is-enabled nginx   # enabled / disabled
systemctl list-units --type=service        # all loaded services
systemctl list-units --type=service --state=running
```
> Remember the difference: **start** = now (this boot only); **enable** = on every boot.
> You usually want `enable --now` for a service that should always run.

---

## Logs with `journalctl`
`systemd` captures service logs centrally.
```bash
journalctl -u nginx              # logs for the nginx service
journalctl -u nginx -f           # follow live (like tail -f)
journalctl -u nginx --since "10 min ago"
journalctl -u nginx -n 100       # last 100 lines
journalctl -p err                # only error-priority messages
journalctl -b                    # logs since last boot
journalctl --disk-usage          # how much space logs use
```

---

## Anatomy of a Unit File
Service definitions live in `/etc/systemd/system/` and `/lib/systemd/system/`.
```ini
# /etc/systemd/system/myapp.service
[Unit]
Description=My App
After=network.target

[Service]
ExecStart=/usr/local/bin/myapp
Restart=on-failure
User=appuser

[Install]
WantedBy=multi-user.target
```
After editing unit files:
```bash
sudo systemctl daemon-reload     # reload systemd's config
sudo systemctl enable --now myapp
```
> You'll write a custom unit like this on **Day 5** to keep your own app running.

---

## TL;DR
- `systemctl start|stop|restart|reload` controls a service now.
- `systemctl enable --now <svc>` = run now **and** on every boot.
- `systemctl status` + `journalctl -u <svc> -f` is your go-to debugging combo.
