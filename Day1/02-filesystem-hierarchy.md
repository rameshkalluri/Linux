# The Linux Filesystem Hierarchy (FHS)

Linux has a **single tree** starting at the root `/` (there are no `C:\` drives). Other
disks/partitions are *mounted* into this tree at a directory (mount point).

## The Tree at a Glance
| Path | Purpose |
|---|---|
| `/` | Root — the start of the entire directory tree. |
| `/bin` | Essential user command binaries (`ls`, `cp`) needed even in single-user mode. |
| `/sbin` | System binaries for root/admin tasks (`reboot`, `iptables`). |
| `/boot` | Kernel images and bootloader files needed to start the machine. |
| `/dev` | Device files (disks `/dev/sda`, null `/dev/null`). |
| `/etc` | **System-wide configuration files** — the central hub for service/app config. |
| `/home` | Personal directories for normal users (`/home/ramesh`). |
| `/root` | Home directory for the **root** (superuser). Not the same as `/`. |
| `/lib`, `/lib64` | Shared libraries needed by binaries in `/bin` and `/sbin`. |
| `/media` | Auto-mounted removable media (USB, CD). |
| `/mnt` | Manual/temporary mount point. |
| `/opt` | Optional third-party software. |
| `/usr` | User system resources: `/usr/bin`, `/usr/lib`, installed programs. |
| `/var` | **Variable data**: logs (`/var/log`), caches, spools, databases. |
| `/tmp` | Temporary files (often cleared on reboot). |
| `/proc` | Virtual filesystem exposing **process & kernel** info (not on disk). |
| `/sys` | Virtual filesystem exposing devices & kernel objects. |

---

## The Directories DevOps Engineers Touch Most
- **`/etc`** — config lives here. e.g. `/etc/nginx/nginx.conf`, `/etc/ssh/sshd_config`,
  `/etc/hosts`, `/etc/fstab`, `/etc/crontab`.
- **`/var/log`** — where you go when something breaks: `/var/log/syslog`,
  `/var/log/nginx/`, `journalctl`.
- **`/home/<user>`** and **`/root`** — your working/config dotfiles (`~/.ssh`, `~/.bashrc`).
- **`/proc` & `/sys`** — live system state (CPU, memory, kernel tunables).

> Memory hook: **config in `/etc`, logs in `/var/log`, your stuff in `/home`.**

---

## Paths: Absolute vs Relative
- **Absolute** starts at root: `/etc/nginx/nginx.conf` (works from anywhere).
- **Relative** is from your current directory: `nginx/nginx.conf`.
- Shortcuts:
  - `.` → current directory
  - `..` → parent directory
  - `~` → your home directory
  - `-` → previous directory (with `cd -`)

```bash
pwd                 # print working directory (where am I?)
cd /var/log         # absolute
cd ../              # up one level
cd ~                # home
cd -                # jump back to previous dir
```

---

## TL;DR
- One tree from `/`; disks mount into it (no drive letters).
- **`/etc`** = config, **`/var/log`** = logs, **`/home`** = users, **`/proc`** = live state.
- Use absolute paths for reliability; `.`, `..`, `~`, `-` for speed.
