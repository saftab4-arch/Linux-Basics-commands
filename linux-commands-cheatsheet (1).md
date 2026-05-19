# 🐧 Linux Commands Cheat Sheet & Lab Practice

> A hands-on Linux fundamentals reference built while practicing on Ubuntu inside Docker Desktop.
> Includes Docker setup, command reference, real-world examples, and practiced outputs.

---

## 📋 Table of Contents

1. [Lab Setup — Docker Desktop + Ubuntu Container](#-lab-setup--docker-desktop--ubuntu-container)
2. [Linux Boot Sequence](#-linux-boot-sequence)
3. [Filesystem Basics](#-filesystem-basics)
4. [Core Command Reference](#-core-command-reference)
5. [systemctl — Service Management](#-systemctl--service-management)
6. [Networking with `ip a`](#-networking-with-ip-a)
7. [20 Commands Practice (with outputs)](#-20-commands-practice-with-outputs)
8. [Helpful Tips](#-helpful-tips)

---

## 🐳 Lab Setup — Docker Desktop + Ubuntu Container

### Step 1 — Install Docker Desktop
Download and install Docker Desktop from [docker.com](https://www.docker.com/products/docker-desktop/).

### Step 2 — Run an Ubuntu container

```bash
# Create + start container in background, keep it running
docker run -itd --name linux-lab ubuntu bash

# Step inside the container's Linux terminal
docker exec -it linux-lab bash

# When done, exit and delete it
exit
docker rm -f linux-lab
```

### Common Docker commands

| Command | Description | Example |
|---------|-------------|---------|
| `docker ps -a` | Lists all containers (running + stopped) | `docker ps -aq` → only IDs |
| `docker rm -f <id>` | Force-removes a container even if running | `docker rm -f linux-lab` |
| `docker stop <id>` | Stops a running container gracefully | `docker stop 3c7351b6b272` |
| `docker exec -it <name> bash` | Opens a shell inside running container | `docker exec -it linux-lab bash` |

---

## 🔄 Linux Boot Sequence

```
Power ON
   ↓
BIOS / UEFI firmware
   ↓
Bootloader (GRUB) → loads the kernel
   ↓
Kernel boots
   ↓
systemd (PID 1) starts
   ↓
Services start (sshd, docker, k8s, etc.)
```

---

## 📁 Filesystem Basics

| Path | What it contains |
|------|------------------|
| `/bin` | Essential user binaries (ls, cat, cp, mv) |
| `/sbin` | System binaries (reboot, shutdown, fdisk) |
| `/etc` | Configuration files |
| `/etc/fstab` | Filesystem table — mount points at boot |
| `/etc/os-release` | OS distribution info |
| `/var/log` | System and application logs |
| `/home` | User home directories |
| `/tmp` | Temporary files |

---

## 📘 Core Command Reference

### Basic essentials

| Command | What it does | Example |
|---------|--------------|---------|
| `ls` | List files and directories | `ls -lah` |
| `cd` | Change directory | `cd /var/log` |
| `pwd` | Show current directory | `pwd` |
| `mkdir` | Create new directory | `mkdir -p sandbox` |
| `rm` | Delete files/directories | `rm -rf old_folder` |
| `cp` | Copy files/directories | `cp file.txt /tmp/` |
| `mv` | Move or rename files | `mv old.txt new.txt` |
| `touch` | Create empty file | `touch notes.txt` |
| `cat` | Display file contents | `cat /etc/os-release` |
| `nano` | Edit files in terminal | `nano config.txt` |
| `clear` | Clear terminal screen | `clear` |
| `history` | Show command history | `history` |

### System info

| Command | What it does | Example |
|---------|--------------|---------|
| `uname -r` | Linux kernel release version | `uname -a` (all info) |
| `cat /etc/os-release` | Displays Linux distribution info | shows Ubuntu version |
| `date` | Current date and time | `date` |
| `uptime` | How long machine has been running | `uptime` |
| `hostname` | Show hostname | `hostname -I` (IPs) |

### Processes & monitoring

| Command | What it does | Example |
|---------|--------------|---------|
| `ps aux` | Snapshot of all running processes | `ps aux --sort=-%cpu` |
| `top` | Real-time process viewer | `top -u root` |
| `htop` | Interactive, colorful process viewer | `htop -u syed` |
| `nohup` | Run process in background, ignore hangup | `nohup script.sh &` |

### Disk & memory

| Command | What it does | Example |
|---------|--------------|---------|
| `df -h` | Disk space usage (human-readable) | `df -hT` (with FS type) |
| `du -sh` | Size of a directory | `du -sh /var/log` |
| `free -h` | RAM and swap usage | `free -m` (megabytes) |

### Files & text

| Command | What it does | Example |
|---------|--------------|---------|
| `head` | Print first N lines of a file | `head -n 1 li.txt` |
| `tail` | Print last N lines (or follow live) | `tail -f /var/log/syslog` |
| `wc -l` | Word count by lines | `wc -l file.txt` |
| `grep` | Search for text in files/output | `ps aux \| grep nginx` |
| `chmod` | Change file permissions | `chmod +x script.sh` |
| `chown` | Change file ownership | `chown syed:syed file.txt` |

### Admin & packages

| Command | What it does | Example |
|---------|--------------|---------|
| `sudo` | Run command as administrator | `sudo apt update` |
| `apt update` | Refresh package list | `sudo apt update` |
| `apt install` | Install software | `sudo apt install htop` |

### Help yourself

| Command | What it does |
|---------|--------------|
| `man <command>` | Full manual page |
| `<command> --help` | Quick usage info |
| `info <command>` | Detailed info page |

---

## ⚙️ systemctl — Service Management

| Command | What it does |
|---------|--------------|
| `systemctl status ssh` | Check if service is running |
| `systemctl start ssh` | Start the service now |
| `systemctl stop ssh` | Stop the service now |
| `systemctl restart ssh` | Stop + start (full restart) |
| `systemctl reload ssh` | Reload config without restarting |
| `systemctl enable ssh` | Start automatically on boot |
| `systemctl disable ssh` | Don't start on boot |
| `systemctl is-active ssh` | Returns "active" or "inactive" |
| `systemctl is-enabled ssh` | Returns "enabled" or "disabled" |
| `journalctl -u ssh` | Full logs for the service |
| `systemctl list-units --type=service` | List all loaded services |
| `systemctl --failed` | Show only failed services |

> **Note for WSL users:** WSL2 doesn't run systemd by default. Use `service ssh status` instead, OR enable systemd in `/etc/wsl.conf`:
> ```ini
> [boot]
> systemd=true
> ```

---

## 🌐 Networking with `ip a`

The `ip a` command shows all network interfaces with their IPs, MACs, and status. Modern replacement for `ifconfig`.

### Example output breakdown

#### 1. `lo` — Loopback (localhost)
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536
   inet 127.0.0.1/8 scope host lo
   inet6 ::1/128 scope host
```
- `127.0.0.1` = classic "localhost"
- MTU 65536 is huge — packets just bounce inside the kernel
- `scope host` = only usable on this machine

#### 2. `enp1s0` — Real network interface ⭐
```
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450
   link/ether ae:3f:92:8a:d2:d2
   inet 172.30.1.2/24 brd 172.30.1.255 scope global dynamic
   valid_lft 86306390sec preferred_lft 75517190sec
```
- Modern naming (en = ethernet, p1s0 = PCI bus)
- MAC address: hardware identifier
- `/24` = 254 usable hosts in subnet
- `dynamic` = assigned via DHCP
- MTU 1450 suggests tunnel/overlay (VM/cloud)

#### 3. `docker0` — Docker bridge (idle)
```
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1454 state DOWN
   inet 172.17.0.1/16
```
- Auto-created when Docker installed
- `172.17.0.0/16` = Docker's default subnet (65,534 IPs)
- `DOWN` + `NO-CARRIER` = no containers running

### Useful variations
```bash
ip a                # all interfaces
ip -br a            # brief one-line view (super clean!)
ip -4 a             # IPv4 only
ip route            # routing table
ip neigh            # ARP table
```

---

## 🧪 20 Commands Practice (with outputs)

> Below are 20 commands practiced inside the `linux-lab` Ubuntu container. Replace with your real outputs when you run them.

### 1. `pwd`
```bash
root@ubuntu:/home/devops$ pwd
/home/devops
```

### 2. `whoami`
```bash
root@ubuntu:/home/devops$ whoami
root
```

### 3. `date`
```bash
root@ubuntu:/home/devops$ date
Sun May 17 15:47:29 UTC 2026
```

### 4. `uptime`
```bash
root@ubuntu:/home/devops$ uptime
 15:48:12 up 1:30, 1 user, load average: 0.05, 0.07, 0.09
```

### 5. `uname -r`
```bash
root@ubuntu:/home/devops$ uname -r
6.6.114.1-microsoft-standard-WSL2
```

### 6. `cat /etc/os-release`
```bash
root@ubuntu:/home/devops$ cat /etc/os-release
NAME="Ubuntu"
VERSION="24.04.1 LTS (Noble Numbat)"
ID=ubuntu
PRETTY_NAME="Ubuntu 24.04.1 LTS"
```

### 7. `ls -lah`
```bash
root@ubuntu:/home/devops$ ls -lah
total 12K
drwxr-xr-x 2 root root 4.0K May 17 14:20 .
drwxr-xr-x 3 root root 4.0K May 17 14:20 ..
-rw-r--r-- 1 root root   45 May 17 15:00 li.txt
```

### 8. `mkdir -p sandbox`
```bash
root@ubuntu:/home/devops$ mkdir -p sandbox
root@ubuntu:/home/devops$ ls
li.txt  sandbox
```

### 9. `touch notes.txt`
```bash
root@ubuntu:/home/devops$ touch notes.txt
root@ubuntu:/home/devops$ ls
li.txt  notes.txt  sandbox
```

### 10. `cat li.txt`
```bash
root@ubuntu:/home/devops$ cat li.txt
hello sir
this is devops path 2025
```

### 11. `head -n 1 li.txt`
```bash
root@ubuntu:/home/devops$ head li.txt -n 1
hello sir
```

### 12. `tail -n 1 li.txt`
```bash
root@ubuntu:/home/devops$ tail li.txt -n 1
this is devops path 2025
```

### 13. `wc -l li.txt`
```bash
root@ubuntu:/home/devops$ wc -l li.txt
2 li.txt
```

### 14. `df -h`
```bash
root@ubuntu:/home/devops$ df -h
Filesystem      Size  Used Avail Use% Mounted on
overlay         100G   45G   55G  45% /
tmpfs            64M     0   64M   0% /dev
```

### 15. `free -h`
```bash
root@ubuntu:/home/devops$ free -h
              total        used        free      shared  buff/cache   available
Mem:           16Gi       4.2Gi       8.1Gi       250Mi       3.7Gi        11Gi
Swap:         2.0Gi          0B       2.0Gi
```

### 16. `ps aux`
```bash
root@ubuntu:/home/devops$ ps aux | head -5
USER       PID  %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1   0.0  0.1  22568  4892 ?        Ss   14:20   0:01 /sbin/init
root       123   0.0  0.2  45120  8456 ?        S    14:20   0:00 /usr/sbin/sshd
```

### 17. `ip a` (brief view)
```bash
root@ubuntu:/home/devops$ ip -br a
lo               UNKNOWN        127.0.0.1/8 ::1/128
enp1s0           UP             172.30.1.2/24 fe80::ac3f:92ff:fe8a:d2d2/64
docker0          DOWN           172.17.0.1/16
```

### 18. `grep`
```bash
root@ubuntu:/home/devops$ grep "devops" li.txt
this is devops path 2025
```

### 19. `history`
```bash
root@ubuntu:/home/devops$ history | tail -5
   45  ls -lah
   46  cat li.txt
   47  df -h
   48  free -h
   49  history | tail -5
```

### 20. `clear`
```bash
root@ubuntu:/home/devops$ clear
# (terminal screen cleared)
```

---

## 💡 Helpful Tips

### The `;` operator — run multiple commands
```bash
mkdir test ; cd test ; touch file.txt
# Runs all three regardless of success
```

### The `&&` operator — run only if previous succeeds
```bash
mkdir test && cd test
# Only cd if mkdir succeeded
```

### Background processes
```bash
nohup long_script.sh &
# Runs in background, survives terminal close
```

### Get help on any command
```bash
man mkdir          # Full manual
mkdir --help       # Quick options
info mkdir         # Detailed info
```

### Practice tip
```bash
cd /bin && ls
# Browse and try out any of the 20+ commands you find!
```

---

## 🎯 Learning Path

- [x] Set up Docker Desktop + Ubuntu container
- [x] Practice 20 core Linux commands
- [x] Understand boot sequence & systemd
- [x] Explore filesystem structure
- [x] Inspect network interfaces with `ip a`
- [ ] Move on to file permissions deep-dive
- [ ] Learn shell scripting basics
- [ ] Explore networking commands (netstat, ss, nslookup)

---

**Author:** Syed
**Date:** May 2026
**Lab Environment:** Docker Desktop + Ubuntu container on WSL2
