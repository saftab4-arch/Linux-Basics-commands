# 🌐 DevOps Linux — Module 2: Networking Commands

> Commands every DevOps / Cloud engineer uses to debug connectivity, inspect what's listening, check firewall rules, and trace where packets go.

---

## 1. `ip addr show` — Show network interfaces and IPs

**What it does:** Lists every network interface on the server with its IP address, MAC address, and state. The modern replacement for the old `ifconfig` command.

```bash
ip addr show
```

**Example output (from my Ubuntu 24.04 on Civo cloud):**
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 state UNKNOWN
    inet 127.0.0.1/8 scope host lo

2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 state UP
    link/ether 16:b4:6f:87:f8:08
    inet 172.30.1.2/24 brd 172.30.1.255 scope global dynamic enp1s0
    inet6 fe80::be31:6ef9:955f:c1ff/64 scope link

3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1454 state DOWN
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
```

**Breaking down each interface:**

| Interface | IP | What it is |
|---|---|---|
| `lo` | 127.0.0.1 | Loopback — internal only, never leaves the machine |
| `enp1s0` | 172.30.1.2 | Real network card — your server's actual IP on the cloud |
| `docker0` | 172.17.0.1 | Docker bridge network — state DOWN = no containers running |

**Interface state flags explained:**
- `UP` — interface is active
- `LOWER_UP` — physical link detected (cable/network connected)
- `NO-CARRIER` — no physical link (docker0 has no containers attached)
- `mtu 1450` — Max Transmission Unit, slightly less than standard 1500 because of cloud overlay networking (Civo uses VXLAN)

> 💡 On cloud VMs you'll see `enp1s0` or `eth0` as the main interface. The IP here (`172.30.1.2`) is your private cloud IP — not your public IP. Public IP is handled by the cloud provider's load balancer/NAT.

---

## 2. `ss -tlnp` — Show all TCP listening ports

**What it does:** Lists every TCP port currently listening on the server, with the process name and PID. The modern replacement for `netstat -tlnp`. Fast and built into the kernel.

```bash
ss -tlnp
```

**Flags explained:**
- `-t` — TCP only
- `-l` — listening sockets only
- `-n` — show port numbers not service names (22 not "ssh")
- `-p` — show process name and PID

**Example output (from my Ubuntu 24.04):**
```
State    Recv-Q  Send-Q  Local Address:Port    Peer Address:Port  Process
LISTEN   0       4096    127.0.0.53%lo:53      0.0.0.0:*          systemd-resolve (pid=1167)
LISTEN   0       4096          0.0.0.0:22      0.0.0.0:*          sshd (pid=736)
LISTEN   0       511           0.0.0.0:40205   0.0.0.0:*          node (pid=1199)
LISTEN   0       128           0.0.0.0:40200   0.0.0.0:*          kc-terminal (pid=1261)
LISTEN   0       4096        127.0.0.1:35483   0.0.0.0:*          containerd (pid=630)
LISTEN   0       4096               *:40300    *:*                runtime-scenari (pid=1189)
LISTEN   0       4096               *:40305    *:*                runtime-info-se (pid=1249)
```

**What each port is:**

| Port | Process | Bound to | Meaning |
|---|---|---|---|
| 53 | systemd-resolve | 127.0.0.53 | DNS resolver — localhost only, normal |
| **22** | **sshd** | **0.0.0.0** | **SSH — open to all IPs** |
| 40205 | node (KillaCode) | 0.0.0.0 | KillaCode IDE web UI |
| 40200 | kc-terminal | 0.0.0.0 | KillaCode terminal |
| 35483 | containerd | 127.0.0.1 | Container runtime — localhost only, safe |
| 40300/40305 | runtime services | * | KillaCode scenario/info services |

**Local address binding matters:**

| Bound to | Meaning |
|---|---|
| `0.0.0.0:22` | Listening on ALL interfaces — anyone can reach it |
| `127.0.0.1:35483` | Localhost only — only processes on this machine |
| `127.0.0.53:53` | Specific loopback alias — internal DNS only |

> 💡 **Security rule:** anything bound to `0.0.0.0` is exposed to the network. Always ask — should this port be public? If not, bind it to `127.0.0.1` instead.

---

## 3. `ss -tulnp` — Show TCP + UDP listening ports

**What it does:** Same as above but adds `-u` for UDP ports. DNS and some monitoring tools use UDP — you miss them with TCP-only.

```bash
ss -tulnp
```

> 💡 DNS (port 53) uses both TCP and UDP. `ss -tlnp` only shows the TCP half. Use `-tulnp` for a complete picture.

---

## 4. `curl -i https://google.com` — Test HTTP endpoint

**What it does:** Makes an HTTP request and shows response headers + body. Essential for testing if an endpoint is alive, what status code it returns, and what server is behind it.

```bash
curl -i https://google.com
```

**Flags explained:**
- `-i` — include response headers in output
- `-I` — headers only (no body), useful for quick checks
- `-v` — verbose, shows full request + response handshake

**Example output (from my Ubuntu 24.04):**
```
HTTP/2 301
location: https://www.google.com/
content-type: text/html; charset=UTF-8
date: Mon, 18 May 2026 18:32:26 GMT
expires: Wed, 17 Jun 2026 18:32:26 GMT
cache-control: public, max-age=2592000
server: gws
content-length: 220
x-xss-protection: 0
x-frame-options: SAMEORIGIN
alt-svc: h3=":443"; ma=2592000
```

**Reading the headers:**

| Header | Value | Meaning |
|---|---|---|
| `HTTP/2 301` | 301 | Redirect from google.com → www.google.com |
| `location` | https://www.google.com/ | Where it's redirecting to |
| `server` | gws | Google Web Server |
| `cache-control` | max-age=2592000 | Cache for 30 days |
| `x-frame-options` | SAMEORIGIN | Can't be embedded in iframes from other domains |
| `alt-svc` | h3=":443" | Supports HTTP/3 (QUIC) on port 443 |

**Common HTTP status codes you'll see:**

| Code | Meaning | DevOps action |
|---|---|---|
| 200 | OK — all good | ✅ |
| 301/302 | Redirect | Check `location` header |
| 403 | Forbidden | Check auth / firewall |
| 404 | Not found | Wrong URL or app not deployed |
| 502 | Bad gateway | App crashed, nginx can't reach it |
| 503 | Service unavailable | App overloaded or down |
| 000 | No response at all | Port closed or firewall blocking |

> 💡 In prod use `curl -o /dev/null -s -w "%{http_code}"` in health check scripts — returns just the status code, perfect for alerting.

---

## 5. `nc -zv google.com 443` — Test if a TCP port is open

**What it does:** netcat — the simplest way to check if a specific TCP port is reachable on a host. Tests the connection without sending any HTTP data. Great for checking before curl even tries.

```bash
nc -zv google.com 443
```

**Flags explained:**
- `-z` — zero I/O mode, just check if port is open (don't send data)
- `-v` — verbose output

**Example output (from my Ubuntu 24.04):**
```
Connection to google.com (64.233.177.138) 443 port [tcp/https] succeeded!
```

**Success vs failure:**
```bash
# Port open
Connection to google.com (64.233.177.138) 443 port [tcp/https] succeeded!

# Port closed / filtered
nc: connect to somehost port 8080 (tcp) failed: Connection refused
```

**Real world uses:**
```bash
nc -zv db-server 5432      # Is Postgres reachable?
nc -zv redis-host 6379     # Is Redis reachable?
nc -zv api-gateway 8080    # Is the API gateway up?
```

> 💡 Always run `nc` before `curl` when debugging. If `nc` fails, the port is blocked — don't waste time debugging the app layer.

---

## 6. `ping -c 5 8.8.8.8` — Test basic connectivity + latency

**What it does:** Sends 5 ICMP packets to Google DNS (8.8.8.8) and measures round-trip time. Tells you if the server can reach the internet and what the baseline latency is.

```bash
ping -c 5 8.8.8.8
```

**Example output (from my Ubuntu 24.04):**
```
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=116 time=12.4 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=116 time=12.0 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=116 time=12.4 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=116 time=12.1 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=116 time=11.9 ms

--- 8.8.8.8 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4005ms
rtt min/avg/max/mdev = 11.9/12.2/12.4/0.2 ms
```

**Reading the output:**

| Field | Value | Meaning |
|---|---|---|
| `time=12.4ms` | 12.4ms | Round-trip latency — excellent for a cloud VM |
| `ttl=116` | 116 | Hops remaining — started at 128, used 12 hops to reach Google |
| `0% packet loss` | 0% | No dropped packets ✅ |
| `mdev=0.2ms` | 0.2ms | Jitter — very stable connection ✅ |

**Latency benchmarks:**

| Latency | Quality | Typical scenario |
|---|---|---|
| < 5ms | Excellent | Same datacenter |
| 5-30ms | Good | Same region cloud |
| 30-100ms | Acceptable | Cross-region |
| > 100ms | Poor | Cross-continent or network issue |
| Packet loss > 0% | Problem | Investigate immediately |

> 💡 Your 12ms to 8.8.8.8 from Civo NYC1 is excellent. `mdev` (jitter) of 0.2ms means the connection is rock stable — good news for any latency-sensitive apps.

---

## 7. `traceroute google.com` — Trace the network path

**What it does:** Shows every network hop (router) between your server and the destination, with latency at each hop. Tells you exactly where packets are going and where delays happen.

```bash
traceroute google.com
```

**Example output (from my Ubuntu 24.04 — Civo NYC1):**
```
traceroute to google.com (142.250.105.139), 30 hops max
 1  172.30.1.1                    0.067 ms   ← Your gateway (Civo hypervisor)
 2  10.8.0.1                     11.484 ms   ← Civo internal network
 3  192.168.1.1                  11.449 ms   ← Civo core router
 4  irb-2.router-01.nyc1.civo.io  11.965 ms  ← Civo NYC1 datacenter router
 5  et-0-0-2.router-02.nyc1.civo.io 11.941ms ← Civo NYC1 uplink router
 6  4.36.9.85                    22.808 ms   ← Internet transit (Level3/Lumen)
 7  * * *                                    ← Router blocking ICMP (normal)
 8  * * *                                    ← Router blocking ICMP (normal)
...
10  108.170.236.90               12.425 ms   ← Google backbone
11  192.178.108.20               22.676 ms   ← Google backbone
...
28  yt-in-f139.1e100.net         32.782 ms   ← Google destination reached
```

**Reading it:**

| Hop | What happened |
|---|---|
| Hop 1 | Your Civo VM gateway — 0.06ms, sub-millisecond ✅ |
| Hops 2-5 | Inside Civo's NYC1 datacenter network |
| Hop 6 | Left Civo, entering public internet transit |
| `* * *` | Routers that block traceroute probes — not packet loss, just firewall |
| Hop 10+ | Inside Google's private backbone network |
| Hop 28 | `1e100.net` = Google's domain, destination reached |

> 💡 `* * *` does NOT mean the connection is broken — it means that router blocks ICMP. The fact that the next hop responds proves packets are passing through. Only worry if ALL remaining hops are `* * *`.

---

## 8. `lsof -i :22` — Which process is using a port

**What it does:** Lists all processes with an open file on a specific network port. "lsof" = List Open Files — in Linux, network sockets are treated as files.

```bash
lsof -i :22
```

**Example output (from my Ubuntu 24.04):**
```
COMMAND  PID  USER  FD   TYPE DEVICE  NODE NAME
systemd    1  root  93u  IPv4  5845    TCP *:ssh (LISTEN)
systemd    1  root  94u  IPv6  5849    TCP *:ssh (LISTEN)
sshd     736  root   3u  IPv4  5845    TCP *:ssh (LISTEN)
sshd     736  root   4u  IPv6  5849    TCP *:ssh (LISTEN)
```

Both `systemd` (PID 1) and `sshd` (PID 736) show on port 22 — this is normal Ubuntu 24.04 socket activation. systemd holds the socket and passes connections to sshd.

**Other useful `lsof -i` examples:**
```bash
lsof -i :80        # Who's on port 80?
lsof -i :8080      # Who's on port 8080?
lsof -i :5432      # Is Postgres actually running?
lsof -i TCP        # All TCP connections
lsof -i -P -n      # All network activity, no DNS resolution (faster)
```

> 💡 Port conflict error when starting a service? Run `lsof -i :<port>` to find the process already using it, then kill it or reconfigure.

---

## 9. `ufw status verbose` — Check firewall rules

**What it does:** Shows the status of UFW (Uncomplicated Firewall) — Ubuntu's default firewall. Shows all rules, allowed ports, and whether the firewall is active.

```bash
ufw status verbose
```

**Example output (from my Ubuntu 24.04):**
```
Status: inactive
```

**UFW is OFF on this machine** — fine for a lab/learning environment, never acceptable in production.

**What a healthy prod firewall looks like:**
```bash
# Enable UFW
ufw enable

# Allow SSH (do this BEFORE enabling or you lock yourself out)
ufw allow 22/tcp

# Allow HTTP and HTTPS
ufw allow 80/tcp
ufw allow 443/tcp

# Check status
ufw status verbose
```

**Expected prod output:**
```
Status: active

To                Action      From
--                ------      ----
22/tcp            ALLOW IN    Anywhere
80/tcp            ALLOW IN    Anywhere
443/tcp           ALLOW IN    Anywhere
22/tcp (v6)       ALLOW IN    Anywhere (v6)
```

**Common UFW commands:**
```bash
ufw enable                    # Turn on firewall
ufw disable                   # Turn off firewall
ufw allow 22/tcp              # Allow SSH
ufw deny 8080                 # Block port 8080
ufw delete allow 80/tcp       # Remove a rule
ufw allow from 10.0.0.0/8    # Allow entire subnet
ufw reset                     # Wipe all rules and start over
```

> 💡 **Golden rule:** Always `ufw allow 22` BEFORE `ufw enable` — or you will lock yourself out of SSH on a remote server permanently.

---

## Quick Reference — Module 2: Networking

| Command | One-liner |
|---|---|
| `ip addr show` | Show all network interfaces and their IPs |
| `ss -tlnp` | Show all TCP listening ports with process names |
| `ss -tulnp` | Same but includes UDP ports too |
| `curl -i https://host` | HTTP request showing headers + status code |
| `curl -I https://host` | Headers only — quick endpoint health check |
| `nc -zv host port` | Test if a TCP port is open on a remote host |
| `ping -c 5 8.8.8.8` | Test connectivity + measure latency |
| `traceroute host` | Trace every network hop to destination |
| `lsof -i :PORT` | Find which process is using a specific port |
| `ufw status verbose` | Show firewall rules and status |

---

## Networking Health Check — What "Good" Looks Like

| Check | Healthy | Red Flag |
|---|---|---|
| `ping` packet loss | 0% | Any loss > 0% |
| `ping` latency (same region) | < 30ms | > 100ms |
| `ping` jitter (mdev) | < 5ms | > 20ms |
| Open ports on `0.0.0.0` | Only intended ones | Unknown ports exposed |
| UFW status (prod) | active | inactive |
| Port 22 bound to | `0.0.0.0` with key auth | `0.0.0.0` with password auth |
| `* * *` in traceroute | Normal mid-route | Every hop from a point onward |

---

## Bonus — Useful One-liners

```bash
# Who is connected to my server right now?
ss -tnp | grep ESTABLISHED

# Count active connections per IP
ss -tn | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -rn

# Test if a URL returns 200
curl -o /dev/null -s -w "%{http_code}" https://myapp.com

# Check DNS resolution
dig google.com +short

# What's my public IP?
curl -s ifconfig.me

# Check if a remote port is open (no nc needed)
timeout 3 bash -c 'cat < /dev/null > /dev/tcp/google.com/443' && echo "open" || echo "closed"
```

---

*Module 3 → Logs: `journalctl`, `tail -f`, `grep`, `awk`, `sed`, `zcat`*
