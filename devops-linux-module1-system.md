# 🖥️ DevOps Linux — Module 1: System Commands

> Commands every DevOps / Cloud engineer runs daily to check server health, hunt processes, and understand what a machine is doing.

---

## 1. `uptime` — Server uptime + load averages

**What it does:** Shows how long the server has been running and load averages over the last 1, 5, and 15 minutes. Load avg should stay **below your CPU core count** — if it exceeds it, the server is struggling.

```bash
uptime
```

**Example output (from my Ubuntu 24.04):**
```
17:45:35 up 13 min,  0 users,  load average: 0.00, 0.02, 0.05
```

| Field | Value | Meaning |
|---|---|---|
| up 13 min | 13 minutes | Server rebooted recently |
| 0 users | 0 | No interactive users logged in |
| load avg | 0.00, 0.02, 0.05 | Virtually idle — well below 1 CPU core limit |

> 💡 **Rule of thumb:** Load avg > number of CPU cores = server is overloaded. Check cores with `nproc`.

---

## 2. `nproc` — Number of CPU cores

**What it does:** Prints the number of available CPU cores. Use this alongside `uptime` to interpret load averages.

```bash
nproc
```

**Example output:**
```
1
```

> 💡 A 1-core server with load avg of 1.5 is struggling. A 4-core server with 1.5 is totally fine.

---

## 3. `free -h` — RAM and swap usage

**What it does:** Shows memory usage in human-readable format. The `available` column is what actually matters — not `free`. Linux deliberately uses spare RAM for disk caching.

```bash
free -h
```

**Example output (from my Ubuntu 24.04):**
```
               total        used        free      shared  buff/cache   available
Mem:           1.9Gi       427Mi       771Mi       1.1Mi       872Mi       1.4Gi
Swap:          1.0Gi          0B       1.0Gi
```

| Column | Value | Meaning |
|---|---|---|
| total | 1.9Gi | Total RAM on the server |
| used | 427Mi | RAM used by actual processes |
| buff/cache | 872Mi | RAM borrowed for disk cache (normal) |
| **available** | **1.4Gi** | **Real free RAM — use this number** |
| Swap used | 0B | Zero swap = healthy ✅ |

> 💡 **Red flag:** If swap used is non-zero on a prod server, RAM is exhausted. The server is using disk as slow RAM — investigate immediately.

---

## 4. `df -h` — Disk space usage

**What it does:** Shows disk space across all mounted filesystems. Watch the `Use%` column — above 85% is a warning, 95%+ is an emergency.

```bash
df -h
```

**Example output (from my Ubuntu 24.04):**
```
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           191M  1.0M  190M   1% /run
/dev/vda1        19G  5.4G   13G  30% /
tmpfs           952M   84K  952M   1% /dev/shm
/dev/vda16      881M  117M  703M  15% /boot
/dev/vda15      105M  6.2M   99M   6% /boot/efi
```

| Filesystem | What it is | Use% |
|---|---|---|
| /dev/vda1 | Main OS disk (the one that matters) | 30% ✅ |
| /dev/vda16 | Boot partition | 15% ✅ |
| tmpfs | RAM-based virtual filesystem — ignore | N/A |

> 💡 `tmpfs` entries are not real disk — they live in RAM. Focus on `/dev/vda*` or `/dev/sda*` entries.

---

## 5. `top` — Live process monitor

**What it does:** Real-time view of all running processes, CPU, and memory. Updates every 3 seconds. Every Linux server has it — no install needed.

```bash
top
```

**Keyboard shortcuts inside top:**

| Key | Action |
|---|---|
| `1` | Toggle per-core CPU breakdown |
| `M` | Sort by memory usage |
| `P` | Sort by CPU usage |
| `k` | Kill a process (enter PID) |
| `q` | Quit |

> 💡 Press `1` immediately after opening top to see per-core CPU — essential on multi-core servers.

---

## 6. `htop` — Interactive process monitor (better top)

**What it does:** Visual, colorful, interactive version of `top`. Shows CPU bars, memory bars, and lets you navigate/kill processes with arrow keys and function keys. Not installed by default.

```bash
# Install first (Ubuntu/Debian)
sudo apt install htop -y

# Run it
htop
```

**What you see (from my Ubuntu 24.04):**
```
Mem[||||||||||||||||||||||||  289M/1.86G]   Load average: 0.00 0.00 0.00
Swp[                           0K/1024M]   Uptime: 00:24:44

  PID USER    PRI  NI   VIRT   RES  SHR  CPU% MEM%   TIME+  Command
 1707 root     20   0   5520  4560 3396   0.0  0.2  0:00.04 htop
    1 root     20   0  22104 13300 9508   0.0  0.7  0:02.41 /sbin/init
  630 root     20   0  1750M 46440 32672  0.0  2.4  0:00.31 /usr/bin/containerd
```

**Key columns:**

| Column | Meaning |
|---|---|
| PID | Process ID |
| RES | Actual RAM used (Resident Set Size) |
| CPU% | CPU percentage this process is using |
| MEM% | RAM percentage this process is using |
| Command | What is running |

**Keyboard shortcuts:**

| Key | Action |
|---|---|
| `F6` | Sort by column |
| `F9` | Kill selected process |
| `F10` | Quit |
| `t` | Toggle tree view (see parent/child relationships) |

> 💡 Always install `htop` as the first thing on any new server. It's the command you'll live in.

---

## 7. `vmstat 1 5` — System stats snapshot

**What it does:** Prints a system performance snapshot every N seconds, M times. Shows CPU, memory, disk I/O, and swap activity all in one view. Great for spotting bottlenecks.

```bash
vmstat 1 5
```

**Example output (from my Ubuntu 24.04):**
```
procs -----------memory---------- ---swap-- -----io---- -system-- -------cpu-------
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st gu
 1  0      0 755656  43888 855488    0    0   534   218  124    3  1  1 98  0  0  0
 0  0      0 755656  43888 855528    0    0     0     0   87  254  0  0 100  0  0  0
 0  0      0 755656  43888 855528    0    0     0     0  103  266  0  0  99  0  1  0
 0  0      0 755656  43888 855528    0    0     0     0   82  255  0  1  99  0  0  0
 0  0      0 755656  43888 855528    0    0     0     0   89  288  1  0  99  0  0  0
```

**Key columns to watch:**

| Column | What it means | Red flag |
|---|---|---|
| `r` | Processes waiting for CPU | Consistently > CPU cores = overloaded |
| `b` | Processes in uninterruptible sleep | > 0 = I/O bottleneck |
| `si` / `so` | Swap in / swap out (pages/sec) | Any non-zero = RAM exhausted |
| `bi` / `bo` | Disk reads / writes (blocks/sec) | Spikes = heavy disk activity |
| `wa` | CPU time waiting for I/O | > 10% = disk is your bottleneck |
| `id` | CPU idle percentage | Should be high on a healthy server |
| `us` | User-space CPU usage | Your apps |
| `sy` | Kernel CPU usage | OS overhead |

**Reading my output:**
- `r=1` on first row = vmstat itself waiting (normal), drops to 0 after ✅
- `swpd=0`, `si=0`, `so=0` = zero swap activity ✅
- `wa=0` = no I/O wait, disk is fast ✅
- `id=98-100` = CPU sitting idle 98-100% of the time ✅

> 💡 `vmstat` is the first tool to run when a server "feels slow" but you don't know why. Check `wa` for disk, `si/so` for RAM, `r` for CPU.

---

## 8. `ps aux --sort=-%cpu | head -10` — Top CPU processes

**What it does:** A static snapshot of the top 10 CPU-hungry processes. Unlike `top`, it's not live — but it's scriptable and great for logging or alerting.

```bash
ps aux --sort=-%cpu | head -10
```

**Flags explained:**
- `a` — show processes from all users
- `u` — user-oriented format (shows CPU%, MEM%, command)
- `x` — include processes not attached to a terminal
- `--sort=-%cpu` — sort by CPU descending (highest first)
- `head -10` — show only top 10

---

## 9. `ps aux --sort=-%mem | head -10` — Top memory processes

**What it does:** Same as above but sorted by memory. Perfect for hunting memory leaks.

```bash
ps aux --sort=-%mem | head -10
```

**Example output (from my Ubuntu 24.04):**
```
USER         PID %CPU %MEM    VSZ   RSS TTY  STAT START  TIME COMMAND
root        1199  0.1  5.1 11770228 100752 ?  SNl  17:32  0:02 /opt/theia/node ... (KillaCode IDE)
root         858  0.0  3.9 1761524  76960 ?   Ssl  17:32  0:00 /usr/bin/dockerd
root        1229  0.1  2.8 1243432  54640 ?   SNl  17:32  0:02 /opt/theia/node ... (KillaCode worker)
root         630  0.0  2.3 1792748  46440 ?   Ssl  17:32  0:01 /usr/bin/containerd
root         697  0.0  1.1 110016   22988 ?   Ssl  17:32  0:00 /usr/bin/python3 (unattended-upgrades)
```

**Key columns:**

| Column | Meaning |
|---|---|
| %CPU | CPU usage at this moment |
| %MEM | RAM percentage used |
| VSZ | Virtual memory size (includes shared libs — often misleading) |
| RSS | Actual physical RAM used — the real number |
| STAT | Process state: S=sleeping, R=running, Z=zombie |

> 💡 `VSZ` looks scary but is mostly virtual/mapped memory. Always focus on `RSS` for actual RAM consumption.

---

## Quick Reference — Module 1: System

| Command | One-liner |
|---|---|
| `uptime` | Server uptime + load averages |
| `nproc` | Number of CPU cores |
| `free -h` | RAM and swap usage |
| `df -h` | Disk space per filesystem |
| `top` | Live process monitor (built-in) |
| `htop` | Interactive process monitor (install first) |
| `vmstat 1 5` | CPU / memory / disk / swap stats every 1s |
| `ps aux --sort=-%cpu \| head -10` | Top 10 CPU processes (snapshot) |
| `ps aux --sort=-%mem \| head -10` | Top 10 memory processes (snapshot) |

---

## Health Check — What "Good" Looks Like

| Metric | Healthy | Warning | Critical |
|---|---|---|---|
| Load avg | < nproc value | = nproc value | > nproc value |
| Swap used | 0 | > 0 | > 50% of total |
| Disk Use% | < 70% | 70-85% | > 85% |
| CPU idle (`id`) | > 80% | 50-80% | < 50% |
| I/O wait (`wa`) | < 5% | 5-15% | > 15% |

---

*Module 2 → Networking: `ss`, `curl`, `nc`, `lsof`, `ufw`, `ip addr`, `traceroute`*
