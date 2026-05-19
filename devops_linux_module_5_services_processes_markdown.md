# ⚙️ DevOps Linux — Module 5: Services & Processes

> Core Linux service and process management commands used daily by DevOps and Cloud engineers for troubleshooting, monitoring, deployments, and server operations.

---

# 1. `systemctl status` — Check Service Status

```bash
systemctl status ssh
```

### What it does
Shows:
- service state
- PID
- uptime
- logs
- recent errors

### Command Breakdown

| Part | Meaning |
|---|---|
| `systemctl` | systemd service manager |
| `status` | show service status |
| `ssh` | service name |

### Real World DevOps Use
Used constantly for:
- nginx
- docker
- ssh
- mysql
- redis
- kubernetes services

---

# 2. `systemctl start` — Start Service

```bash
systemctl start ssh
```

### What it does
Starts a stopped service.

### Real World DevOps Use
Used during:
- deployments
- maintenance
- troubleshooting

---

# 3. `systemctl stop` — Stop Service

```bash
systemctl stop ssh
```

### What it does
Stops a running service.

⚠️ Warning:
Stopping SSH remotely can disconnect your session.

### Real World DevOps Use
Used for:
- maintenance
- patching
- restarting applications

---

# 4. `systemctl restart` — Restart Service

```bash
systemctl restart ssh
```

### What it does
Stops then starts service again.

### Real World DevOps Use
One of the most common Linux commands.

Used after:
- config changes
- deployments
- updates

Example:

```bash
systemctl restart nginx
```

---

# 5. `systemctl reload` — Reload Config Without Restart

```bash
systemctl reload ssh
```

### What it does
Reloads configuration without fully restarting process.

### Difference

| Command | Meaning |
|---|---|
| `restart` | full stop/start |
| `reload` | apply config only |

### Real World DevOps Use
Preferred in production because:
- less downtime
- safer reloads
- avoids dropped connections

---

# 6. `systemctl enable` — Start Service on Boot

```bash
systemctl enable ssh
```

### What it does
Automatically starts service during boot.

### Real World DevOps Use
Critical for production systems.

Without enable:
service will not survive reboot.

---

# 7. `systemctl disable` — Disable Auto Start

```bash
systemctl disable ssh
```

### What it does
Prevents service from starting automatically during boot.

---

# 8. `ps aux` — Show Running Processes

```bash
ps aux
```

### What it does
Lists all running processes.

### Flags

| Flag | Meaning |
|---|---|
| `a` | all users |
| `u` | user-oriented output |
| `x` | include background processes |

### Real World DevOps Use
Used constantly to:
- inspect running apps
- investigate suspicious processes
- debug servers

---

# 9. `ps aux | grep ssh` — Find Specific Process

```bash
ps aux | grep ssh
```

### What it does
Searches running processes.

### Important Concept
The pipe:

```text
|
```

sends output from one command into another.

### Real World DevOps Use
Used constantly to verify:
- nginx running
- docker running
- java apps alive
- ssh service active

---

# 10. `top` — Live Process Monitor

```bash
top
```

### What it does
Displays live CPU and RAM usage.

### Important Fields

| Field | Meaning |
|---|---|
| `CPU%` | CPU usage |
| `MEM%` | RAM usage |
| `PID` | Process ID |

### Keyboard Shortcuts

| Key | Action |
|---|---|
| `q` | quit |
| `M` | sort by memory |
| `P` | sort by CPU |

### Real World DevOps Use
Critical during:
- outages
- memory leaks
- high CPU incidents

---

# 11. `htop` — Interactive Process Viewer

```bash
htop
```

### What it does
Enhanced colorful version of:

```bash
top
```

### Install if Missing

```bash
apt install htop -y
```

### Real World DevOps Use
Popular in DevOps because:
- easier navigation
- visual CPU bars
- easier process killing

---

# 12. `kill` — Kill Process by PID

```bash
kill 1234
```

### What it does
Terminates process using PID.

### Find PID First

```bash
ps aux | grep nginx
```

### Real World DevOps Use
Used when:
- app frozen
- process stuck
- runaway CPU process

---

# 13. `kill -9` — Force Kill Process

```bash
kill -9 1234
```

### What it does
Forcefully terminates process immediately.

### Flag Meaning

| Flag | Meaning |
|---|---|
| `-9` | SIGKILL |

### Warning
Very aggressive.

Process cannot shutdown gracefully.

### Real World DevOps Use
Last resort during:
- hung applications
- zombie processes
- runaway CPU incidents

---

# 14. `pkill` — Kill by Process Name

```bash
pkill nginx
```

### What it does
Kills process using process name.

### Real World DevOps Use
Faster than manually finding PID.

---

# 15. `jobs` — Show Background Jobs

```bash
jobs
```

### What it does
Shows background jobs in current shell.

### Real World DevOps Use
Useful when multitasking in terminal sessions.

---

# 16. `bg` — Send Job to Background

```bash
bg
```

### What it does
Continues paused process in background.

### Real World DevOps Use
Allows continued work without stopping running tasks.

---

# 17. `fg` — Bring Job to Foreground

```bash
fg
```

### What it does
Brings background process back to active terminal.

### Real World DevOps Use
Useful when returning to interactive commands.

---

# 📌 Core DevOps Commands Learned

| Command | Purpose |
|---|---|
| `systemctl status` | Check service health |
| `systemctl start` | Start service |
| `systemctl stop` | Stop service |
| `systemctl restart` | Restart service |
| `systemctl reload` | Reload config |
| `systemctl enable` | Enable at boot |
| `ps aux` | Show processes |
| `ps aux | grep` | Search processes |
| `top` | Live CPU/RAM monitoring |
| `htop` | Interactive process viewer |
| `kill` | Kill process by PID |
| `kill -9` | Force kill process |
| `pkill` | Kill by name |
| `jobs` | Show background jobs |
| `bg` | Send job to background |
| `fg` | Bring job to foreground |

---

# 🎯 Key DevOps Takeaways

- `systemctl` manages almost all modern Linux services.
- `top` and `htop` are essential during outages and troubleshooting.
- `ps aux | grep` is one of the most common Linux workflows.
- `kill -9` should only be used as a last resort.
- `enable` ensures services survive reboots.
- Understanding processes and services is critical for DevOps operations.

