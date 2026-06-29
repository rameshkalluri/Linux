# Users & Groups

Linux is multi-user. Each user has a UID; each group has a GID. Permissions (Day 2)
hang off these identities.

## Who am I?
```bash
whoami         # current username
id             # uid, gid, and all groups
groups         # groups I belong to
who            # who is logged in
last           # recent login history
```

## Key Files
| File | Contains |
|---|---|
| `/etc/passwd` | One line per user: `name:x:UID:GID:comment:home:shell` |
| `/etc/shadow` | Encrypted passwords (root-only) |
| `/etc/group` | Groups and their members |
| `/etc/sudoers` | Who can use `sudo` (edit with `visudo`) |

```bash
cut -d: -f1 /etc/passwd        # list all usernames
```

---

## Managing Users
```bash
sudo useradd -m -s /bin/bash ramesh   # create user with home dir + bash shell
sudo passwd ramesh                    # set their password
sudo usermod -aG sudo ramesh          # add to 'sudo' group (-aG = append!)
sudo userdel -r ramesh                # delete user and home dir
```
> ⚠️ Always use `-aG` (append) with `usermod`. Plain `-G` **replaces** all groups and
> can lock a user out of `sudo`.

## Managing Groups
```bash
sudo groupadd devops          # create group
sudo usermod -aG devops ramesh
sudo gpasswd -d ramesh devops # remove user from group
```

---

## `sudo` and Switching Users
```bash
sudo command           # run one command as root
sudo -i                # interactive root shell
su - ramesh            # switch to another user (need their password)
sudo su -              # become root via sudo
```
- `sudo` logs who did what (`/var/log/auth.log`) — better than logging in as root.
- Membership in the `sudo` group (Debian/Ubuntu) or `wheel` (RHEL) grants `sudo`.

> **On GCP:** when you SSH into a Compute Engine VM, your account is auto-provisioned and
> usually already has passwordless `sudo`. The OS user maps to your Google identity via
> OS Login or SSH keys in instance metadata.

---

## TL;DR
- `id` / `whoami` / `groups` tell you who you are and your access.
- Create users with `useradd -m`, grant admin with `usermod -aG sudo`.
- Prefer `sudo <cmd>` over logging in as root; it's safer and audited.
- User info lives in `/etc/passwd`; group info in `/etc/group`.
