# What is Linux?

**Linux** is a free, open-source, Unix-like **operating system kernel**. In everyday use,
"Linux" refers to a full OS made of the kernel + GNU tools + a shell + packages — bundled
together as a **distribution (distro)**.

## Kernel vs Shell vs Distro
| Layer | What it does | Examples |
|---|---|---|
| **Kernel** | Talks to hardware: CPU, memory, disks, network, processes | The "Linux" in Linux |
| **Shell** | Command interpreter you type into | `bash`, `zsh`, `sh`, `fish` |
| **Distro** | Kernel + shell + tools + package manager, packaged together | Ubuntu, Debian, RHEL, Alpine |

> Analogy: the **kernel** is the engine, the **shell** is the steering wheel, and the
> **distro** is the whole car.

---

## Common Distros for DevOps
| Distro | Package manager | Where you'll see it |
|---|---|---|
| **Ubuntu / Debian** | `apt` | Default for most cloud VMs, GCP Compute Engine |
| **RHEL / Rocky / CentOS** | `dnf` / `yum` | Enterprise servers |
| **Alpine** | `apk` | Tiny container base images (Docker) |
| **Amazon Linux / COS** | `yum` / read-only | AWS / GKE nodes |

---

## Why DevOps Runs on Linux
- **Servers & cloud VMs** are overwhelmingly Linux (cheap, stable, scriptable).
- **Containers** (Docker) are built on Linux kernel features (namespaces, cgroups).
- **Kubernetes** nodes run Linux.
- **CI/CD runners**, automation, and Infrastructure-as-Code all assume a Linux shell.
- On **GCP**, Compute Engine, GKE nodes, and Cloud Shell are Linux — the same skills apply.

---

## Everything is a File
A core Unix philosophy: devices, processes, and even kernel settings are exposed as files.
- A disk → `/dev/sda`
- Process info → `/proc/<pid>/`
- Kernel tunables → `/proc/sys/`, `/sys/`

This is why the same small tools (`cat`, `grep`, `>`) work on almost everything.

---

## TL;DR
- Linux = open-source kernel; a **distro** packages it into a usable OS.
- Know your distro because it decides your **package manager** (`apt` vs `dnf`).
- DevOps lives on Linux: servers, containers, K8s, CI, and GCP VMs.
- "Everything is a file" → a few simple tools compose to do almost anything.
