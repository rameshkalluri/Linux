# SSH & Remote Access

**SSH** (Secure Shell) is how you log into and operate remote Linux servers. It's the
single most-used remote tool in DevOps.

## Connecting
```bash
ssh user@host                 # connect (prompts for password or uses a key)
ssh user@host -p 2222         # custom port
ssh -i ~/.ssh/mykey user@host # use a specific private key
ssh user@host 'df -h'         # run one command remotely and exit
```

---

## Key-Based Authentication (the right way)
Passwords are weak and noisy; keys are standard for servers and required for cloud.

```bash
ssh-keygen -t ed25519 -C "ramesh@laptop"   # generate a key pair
# creates ~/.ssh/id_ed25519 (PRIVATE — keep secret) and id_ed25519.pub (PUBLIC)

ssh-copy-id user@host          # install your public key on the server
# now: ssh user@host  → logs in with no password
```

**The model:** the **public** key goes on the server (`~/.ssh/authorized_keys`); the
**private** key stays on your machine and never leaves it.

> ⚠️ Permissions matter (Day 2): `chmod 700 ~/.ssh` and `chmod 600 ~/.ssh/id_ed25519`
> or SSH will refuse the key.

---

## `~/.ssh/config` — stop typing long commands
```ssh-config
# ~/.ssh/config
Host gcp-web
    HostName 34.123.45.67
    User ramesh
    IdentityFile ~/.ssh/id_ed25519
    Port 22
```
Now just: `ssh gcp-web`.

---

## Copying Files
```bash
scp file.txt user@host:/tmp/             # local → remote
scp user@host:/var/log/app.log .         # remote → local
scp -r mydir user@host:/opt/             # whole directory
rsync -avz mydir/ user@host:/opt/mydir/  # efficient sync (only changed parts)
```
> `rsync` beats `scp` for large/repeated transfers — it only sends differences and can resume.

---

## SSH on GCP
GCP makes SSH easy and key-managed:
```bash
gcloud compute ssh my-vm --zone=us-central1-a    # handles keys for you
gcloud compute scp file.txt my-vm:~ --zone=us-central1-a
```
- `gcloud compute ssh` auto-generates a key and pushes it via metadata/OS Login.
- Or click **SSH** in the Cloud Console for a browser terminal.
- The VM's firewall must allow **tcp:22** from your IP — see
  [GCP firewall notes](../../GCP/Day4/02-firewall-rules-and-routes.md).

---

## Keep Sessions Alive: `tmux`
```bash
tmux              # start a session
# Ctrl+b d        → detach (keeps running)
tmux attach       # reattach later
```
Run long jobs inside `tmux` so an SSH drop doesn't kill them.

---

## TL;DR
- `ssh user@host`; prefer **key auth** (`ssh-keygen` + `ssh-copy-id`).
- Private key stays local (`600`), public key goes on the server.
- Use `~/.ssh/config` for shortcuts; `scp`/`rsync` to copy files.
- On GCP: `gcloud compute ssh <vm>` manages keys for you; allow tcp:22.
