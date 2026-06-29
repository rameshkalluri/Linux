# Package Management

How you install software. **The tool depends on the distro** (see Day 1).

## Debian / Ubuntu — `apt`
```bash
sudo apt update                 # refresh package lists (do this first!)
sudo apt upgrade                # upgrade installed packages
sudo apt install nginx          # install a package
sudo apt install -y htop curl   # -y = don't prompt (good in scripts)
sudo apt remove nginx           # remove (keep config)
sudo apt purge nginx            # remove + config
sudo apt autoremove             # clean up unused dependencies
apt search keyword              # search for a package
apt show nginx                  # package details
dpkg -l                         # list installed packages
```
> `apt update` ≠ `apt upgrade`. **update** refreshes the catalog; **upgrade** installs
> newer versions. Always `update` before `install`.

---

## RHEL / Rocky / Amazon Linux — `dnf` (or older `yum`)
```bash
sudo dnf install nginx
sudo dnf remove nginx
sudo dnf update
dnf search keyword
rpm -qa                  # list installed packages
```
`yum` and `dnf` are largely interchangeable; `dnf` is the modern replacement.

## Alpine (containers) — `apk`
```bash
apk add --no-cache curl   # --no-cache keeps image small
apk del curl
```

---

## Installing Outside the Package Manager
- **Download a binary** and put it on your `PATH` (e.g. `/usr/local/bin`):
  ```bash
  sudo mv mytool /usr/local/bin/ && sudo chmod +x /usr/local/bin/mytool
  ```
- **Add a third-party repo** (e.g. Docker, the `gcloud` CLI) then `apt install`.
- **Language managers**: `pip` (Python), `npm` (Node) for libraries/tools.

> **On GCP VMs:** images come with the `gcloud` CLI and package repos preconfigured.
> A typical bootstrap is `apt update && apt install -y nginx` in a startup script (Day 5).

---

## TL;DR
- Ubuntu/Debian → `apt`; RHEL family → `dnf`/`yum`; Alpine → `apk`.
- Always `apt update` before `apt install`; use `-y` in automation.
- Drop standalone binaries into `/usr/local/bin` and `chmod +x` them.
