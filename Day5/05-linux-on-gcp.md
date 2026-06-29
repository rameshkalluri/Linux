# Linux on GCP — Tying It All Together

Everything from Days 1–5 applies directly when you run Linux on Google Cloud. This page
connects the two learning tracks.

## Where Linux Shows Up in GCP
| GCP service | Linux connection |
|---|---|
| **Compute Engine** | Linux VMs you SSH into and operate (Days 1–4) |
| **Cloud Shell** | A free Linux terminal in the browser |
| **GKE nodes** | Linux hosts running your containers |
| **Cloud Run / Functions** | Linux containers under the hood |
| **Startup scripts** | Bash that bootstraps a VM (Day 5) |

See the [GCP Compute Engine notes](../../GCP/Day3/01-compute-engine-basics.md).

---

## Launch & Connect to a Linux VM
```bash
# create a small free-tier-friendly VM
gcloud compute instances create web-1 \
    --zone=us-central1-a \
    --machine-type=e2-micro \
    --image-family=debian-12 --image-project=debian-cloud

# SSH in (gcloud manages your keys — see Day 4)
gcloud compute ssh web-1 --zone=us-central1-a

# ...do Linux things...
gcloud compute instances delete web-1 --zone=us-central1-a   # ⚠️ clean up!
```

---

## Startup Scripts (Bootstrapping with Bash)
A **startup script** runs as root on first boot — pure Day 5 Bash, automating Day 3
package installs and Day 3 service management.

```bash
gcloud compute instances create web-1 \
    --zone=us-central1-a \
    --machine-type=e2-micro \
    --image-family=debian-12 --image-project=debian-cloud \
    --tags=http-server \
    --metadata=startup-script='#!/usr/bin/env bash
set -euo pipefail
apt update
apt install -y nginx
systemctl enable --now nginx
echo "<h1>Hello from $(hostname)</h1>" > /var/www/html/index.html'
```
Then allow HTTP at the **VPC firewall** (Day 4 — two firewalls!):
```bash
gcloud compute firewall-rules create allow-http \
    --allow=tcp:80 --target-tags=http-server
```
> This single command exercises: Bash (`set -euo pipefail`), `apt` (Day 3), `systemctl`
> (Day 3), file redirection (Day 2), and firewalls (Day 4).

---

## Debugging a GCP VM (Day 4 + Day 5 playbook)
- **Can't SSH?** Check the VPC firewall allows `tcp:22`, the VM is running, and your key
  is in metadata/OS Login.
- **Web app unreachable?** Two firewalls (VPC + OS `ufw`), is `nginx` active
  (`systemctl status nginx`), is it listening (`ss -tlnp`)?
- **Startup script didn't work?** Read its output:
  ```bash
  sudo journalctl -u google-startup-scripts.service
  sudo cat /var/log/syslog | grep startup-script
  ```
- **Disk full / slow?** `df -h`, `free -h`, `top` (Day 3).

---

## Your Capstone (do this!)
1. Create a Linux VM with a **startup script** that installs and serves a web page.
2. Open `tcp:80` in the VPC firewall and confirm with `curl http://<external-ip>`.
3. SSH in with **key auth**, find the Nginx access log, and `tail -f` it while you curl.
4. Write a **health-check script** (Day 5) and schedule it with **cron** (Day 5).
5. **Delete the VM and firewall rule** to avoid charges.

---

## TL;DR
- GCP runs Linux everywhere: Compute Engine, GKE, Cloud Shell, Cloud Run.
- Startup scripts = Day 5 Bash automating Day 3 installs/services on boot.
- Remember the **two firewalls** (VPC + OS) when something's unreachable.
- Cross-reference the [GCP 10-Day Plan](../../GCP/GCP-10-Day-Learning-Plan.md) and
  **always delete billable resources** when done.
