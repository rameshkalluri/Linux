# Network Commands

The DevOps toolkit for inspecting and debugging connectivity.

## See Your Own Network Config
```bash
ip addr            # show IP addresses (modern; replaces ifconfig)
ip a               # short form
ip route           # routing table / default gateway
hostname -I        # this host's IP(s)
cat /etc/resolv.conf   # configured DNS servers
```

## `ping` — is the host reachable?
```bash
ping google.com         # continuous (Ctrl+C to stop)
ping -c 4 10.0.0.2      # send just 4 packets
```
Tests basic reachability + latency. No reply ≠ always down (ICMP may be blocked).

## `curl` / `wget` — talk HTTP
```bash
curl https://example.com               # fetch a URL
curl -I https://example.com            # headers only (status code, etc.)
curl -v https://example.com            # verbose: DNS, TLS, request/response
curl -s -o /dev/null -w "%{http_code}\n" https://example.com   # just the status
curl -X POST -d '{"k":"v"}' -H "Content-Type: application/json" url
wget https://example.com/file.tar.gz   # download a file
```
> `curl` is the single most useful tool for testing APIs, health checks, and "is the
> service responding?" questions.

## `ss` / `netstat` — what's listening / connected?
```bash
ss -tuln          # TCP/UDP listening ports, numeric (modern)
ss -tunp          # include the process using each port (needs sudo)
ss -t state established      # current TCP connections
netstat -tuln     # older equivalent of ss -tuln
```
> "Is my app actually listening on 8080?" → `ss -tlnp | grep 8080`.

---

## DNS Lookups
```bash
dig example.com              # full DNS query/answer
dig +short example.com       # just the IP
dig example.com MX           # query a specific record type
nslookup example.com         # simpler, interactive-friendly
host example.com             # quick name → IP
```

## Trace the Path
```bash
traceroute example.com       # hops between you and the host
mtr example.com              # live traceroute + ping (install separately)
```

## `nc` (netcat) — Swiss-army TCP tool
```bash
nc -zv host 22               # test if a port is open (z=scan, v=verbose)
nc -zv host 80-443           # scan a port range
nc -l 9000                   # listen on a port (quick test server)
```
> Great for "can this box reach that database port?" → `nc -zv db-host 5432`.

---

## TL;DR
- `ip a` (my IP), `ping` (reachable?), `dig +short` (DNS), `traceroute` (path).
- `curl -I` / `-v` to test HTTP services and APIs.
- `ss -tlnp` to see what's listening; `nc -zv host port` to test a specific port.
