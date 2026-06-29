# Permissions & Ownership

Linux is multi-user, so every file has an **owner**, a **group**, and **permissions**
controlling who can read/write/execute it. This is a classic interview topic.

## Reading `ls -l` Output
```
-rwxr-xr--  1  ramesh  devops  4096  Jun 29 10:00  deploy.sh
│└┬┘└┬┘└┬┘     └──┬──┘ └──┬──┘
│ │  │  │         │       └ group
│ │  │  │         └ owner (user)
│ │  │  └ others' permissions (r--)
│ │  └ group's permissions (r-x)
│ └ owner's permissions (rwx)
└ file type (- file, d dir, l link)
```

## The Three Permissions
| Symbol | Octal | On a file | On a directory |
|---|---|---|---|
| `r` read | 4 | read contents | list contents (`ls`) |
| `w` write | 2 | modify file | create/delete files inside |
| `x` execute | 1 | run as program | enter/`cd` into it |

Three groups of these apply to: **owner (u)**, **group (g)**, **others (o)**.

---

## Octal Notation (add the numbers)
- `rwx` = 4+2+1 = **7**
- `rw-` = 4+2   = **6**
- `r-x` = 4+1   = **5**
- `r--` = 4     = **4**

| Mode | Meaning | Common use |
|---|---|---|
| `755` | owner rwx, group/others r-x | scripts, executables, dirs |
| `644` | owner rw, group/others r | normal files |
| `600` | owner rw only | secrets, **SSH private keys** |
| `700` | owner rwx only | private dirs (`~/.ssh`) |

---

## `chmod` — change permissions
```bash
chmod 755 deploy.sh         # octal
chmod +x deploy.sh          # add execute (so you can ./run it)
chmod -x file               # remove execute
chmod u+w,g-w file          # symbolic: owner +write, group -write
chmod -R 755 mydir          # recursive
```

## `chown` / `chgrp` — change owner / group
```bash
sudo chown ramesh file.txt          # change owner
sudo chown ramesh:devops file.txt   # change owner AND group
sudo chgrp devops file.txt          # change group only
sudo chown -R www-data:www-data /var/www   # recursive (typical web setup)
```

---

## `umask` — default permissions for new files
```bash
umask          # show current mask (often 022)
```
New files start from `666` and dirs from `777`, then the umask is subtracted:
- umask `022` → files `644`, dirs `755`.

---

## `sudo` and Root
- `root` (UID 0) bypasses all permission checks.
- Use `sudo <cmd>` to run a single command as root instead of logging in as root.
- Covered more on [Day 3](../Day3/01-users-and-groups.md).

---

## Real DevOps Gotchas
- **SSH keys must be `600`** (or `700` on `~/.ssh`) or SSH refuses them.
- A script won't run until it has `+x` → `chmod +x script.sh && ./script.sh`.
- "Permission denied" writing to `/etc` or `/var` → you need `sudo`.

---

## TL;DR
- Permissions = `rwx` for **owner / group / others**; octal `r=4 w=2 x=1`.
- `chmod 755` scripts, `644` files, `600` secrets; `chmod +x` to make runnable.
- `chown user:group` changes ownership; `sudo` for system files.
