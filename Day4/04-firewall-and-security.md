# Firewall & Security Basics

A firewall controls which ports/traffic are allowed in and out. On servers you'll meet
`ufw` (simple) and `iptables`/`nftables` (low-level). In the cloud, a **separate** layer
exists too.

## `ufw` — Uncomplicated Firewall (Ubuntu/Debian)
```bash
sudo ufw status                 # current rules
sudo ufw enable                 # turn it on (allow SSH FIRST!)
sudo ufw allow 22/tcp           # allow SSH
sudo ufw allow 80,443/tcp       # allow web
sudo ufw allow from 10.0.0.0/24 to any port 5432   # allow a subnet to Postgres
sudo ufw deny 23                # block telnet
sudo ufw delete allow 80        # remove a rule
sudo ufw status numbered        # rules with numbers (to delete by index)
```
> ⚠️ **Allow SSH (22) before enabling ufw on a remote box** or you'll lock yourself out.

---

## `firewalld` (RHEL family)
```bash
sudo firewall-cmd --list-all
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-port=8080/tcp --permanent
sudo firewall-cmd --reload
```

## `iptables` / `nftables` (low-level)
```bash
sudo iptables -L -n -v          # list rules
```
Most teams use `ufw`/`firewalld` (front-ends) rather than raw `iptables`. Containers
(Docker) and Kubernetes manipulate iptables/nftables under the hood.

---

## Two Firewalls in the Cloud
On GCP there are **two independent layers** — both must allow traffic:
1. **VPC firewall rules** (network level, in GCP) — e.g. allow `tcp:80` to instances
   tagged `web`. See [GCP firewall notes](../../GCP/Day4/02-firewall-rules-and-routes.md).
2. **OS firewall** (`ufw`/`firewalld` on the VM itself).

> "I opened port 80 but the site is unreachable" is often because **one** of these two
> layers is still blocking. Check both.

---

## Security Hygiene Checklist
- [ ] Disable password SSH login; use keys only (`PasswordAuthentication no` in `/etc/ssh/sshd_config`).
- [ ] Don't log in as `root`; use `sudo`.
- [ ] Open only the ports you need (least privilege — same idea as
      [GCP IAM least privilege](../../GCP/Day2/03-least-privilege-and-best-practices.md)).
- [ ] Keep packages patched (`apt upgrade`).
- [ ] Protect private keys with `600` perms (Day 2).

---

## TL;DR
- `ufw allow 22/tcp` then `ufw enable` — never lock yourself out of SSH.
- Cloud has **two** firewalls (VPC + OS); both must allow the traffic.
- Harden SSH (keys only, no root), open minimum ports, patch regularly.
