# Networking Basics

Enough theory to debug real problems.

## IP Addresses
- **IPv4**: four numbers, e.g. `10.128.0.2`. **IPv6**: longer, e.g. `2600:1900::...`.
- **Private ranges** (internal, not routable on the internet):
  `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`.
- **Public IP**: reachable from the internet.
- **CIDR** `/24` = subnet mask; `10.0.0.0/24` holds 256 addresses (`.0`–`.255`).

> On **GCP**, a VPC subnet is defined by a CIDR (e.g. `10.0.0.0/24`). VMs get an
> **internal** IP from it and optionally an **external** IP — exactly these concepts.
> See the [GCP networking notes](../../GCP/Day4/01-vpc-and-subnets.md).

## Ports
A port identifies a specific service on a host (IP:port). Common ones:
| Port | Service |
|---|---|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 53 | DNS |
| 3306 | MySQL |
| 5432 | PostgreSQL |

## DNS — names → IPs
When you hit `example.com`:
1. Resolver checks `/etc/hosts`, then cache.
2. Asks a DNS server (configured in `/etc/resolv.conf`).
3. Gets back an IP (an **A** record for IPv4, **AAAA** for IPv6).
4. Your client connects to that IP:port.

| Record | Meaning |
|---|---|
| A / AAAA | name → IPv4 / IPv6 |
| CNAME | alias to another name |
| MX | mail server |
| TXT | arbitrary text (SPF, verification) |
| NS | authoritative name servers |

## TCP vs UDP
- **TCP** — reliable, ordered, connection-based (web, SSH, databases).
- **UDP** — fast, connectionless, no guarantees (DNS, streaming, some metrics).

---

## TL;DR
- Host is reached via **IP:port**; private vs public IPs; CIDR defines subnets.
- DNS turns names into IPs (`A` records); config in `/etc/resolv.conf` + `/etc/hosts`.
- TCP = reliable, UDP = fast-and-loose. These map 1:1 to GCP VPC/firewall concepts.
