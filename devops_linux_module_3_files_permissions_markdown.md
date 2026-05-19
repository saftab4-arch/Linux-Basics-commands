# 🐧 DevOps Linux — Module 3: Files & Permissions

> Core Linux file management and permissions commands used daily by DevOps and Cloud engineers for troubleshooting, deployments, configuration management, and server administration.

---

# 1. `pwd` — Show Current Working Directory

```bash
pwd
```

### What it does
Displays your current directory location in Linux.

### Real World DevOps Use
Before editing or deleting files on production servers, engineers always verify their location first.

### Example Output
![pwd](pwd.png)

---

# 2. `ls -lah` — List Files with Permissions

```bash
ls -lah
```

### What it does
Lists:
- hidden files
- permissions
- owners
- timestamps
- human-readable file sizes

### Flags
- `-l` → long format
- `-a` → show hidden files
- `-h` → human readable sizes

### Real World DevOps Use
One of the most used Linux commands for checking deployment files, configs, and permissions.

### Example Output
![ls -lah](ls-lah.png)

---

# 3. `cd /var/log` — Move into Logs Directory

```bash
cd /var/log
```

### What it does
Changes current directory to Linux system logs.

### Real World DevOps Use
Most troubleshooting starts inside:

```text
/var/log
```

### Example Output
![cd](cd.png)

---

# 4. `ls -lah` Inside `/var/log`

```bash
ls -lah
```

### What it does
Lists Linux log files and directories.

### Real World DevOps Use
Used to identify:
- auth logs
- kernel logs
- package logs
- syslog
- application logs

### Example Output
![var log](var-log-ls.png)

---

# 5. `less /etc/services` — Scroll Through Large Files

```bash
less /etc/services
```

### What it does
Opens a scrollable read-only file viewer.

### Real World DevOps Use
Used constantly for:
- huge log files
- configs
- service mappings
- debugging outputs

### Example Output
![less](less-services.png)

---

# 6. `tail -f /var/log/syslog` — Live Log Monitoring

```bash
tail -f /var/log/syslog
```

### What it does
Streams new log entries live in real time.

### Real World DevOps Use
Critical during:
- deployments
- troubleshooting
- service restarts
- debugging production issues

### Example Output
![tail](tail-f-syslog.png)

---

# 7. `touch test.txt` — Create Empty File

```bash
touch test.txt
```

### What it does
Creates an empty file.

### Real World DevOps Use
Used for:
- testing
- script creation
- placeholders
- configs

---

# 8. `cp` — Copy Files

```bash
cp test.txt backup.txt
```

### What it does
Copies files.

### Real World DevOps Use
Engineers create backups before modifying configs.

### Example Output
![cp](cp-file.png)

---

# 9. `mv` — Move or Rename Files

```bash
mv backup.txt backup-old.txt
```

### What it does
Moves or renames files.

### Real World DevOps Use
Used during:
- deployments
- release management
- backups
- log rotation

### Example Output
![mv](mv-file.png)

---

# 10. `chmod 644` + `chmod +x` — Change Permissions

```bash
chmod 644 test.txt
chmod +x script.sh
```

### What it does
Changes Linux file permissions.

### Real World DevOps Use
Used constantly for:
- deployment scripts
- automation
- security hardening
- executable scripts

### Example Output
![chmod](chmod.png)

---

# 11. `find` — Search Filesystem

```bash
find /var/log -name "*.log"
```

### What it does
Searches Linux filesystem for matching files.

### Real World DevOps Use
Used daily to locate:
- logs
- configs
- certificates
- backups

### Example Output
![find](find-logs.png)

---

# 12. `du -sh` — Check Folder Size

```bash
du -sh /var/log
```

### What it does
Shows total directory size.

### Real World DevOps Use
Critical for:
- disk full investigations
- log cleanup
- storage troubleshooting

### Example Output
![du](du-sh.png)

---

# 📌 Core DevOps Commands Learned

| Command | Purpose |
|---|---|
| `pwd` | Show current directory |
| `ls -lah` | List files with permissions |
| `cd` | Change directories |
| `less` | Read large files |
| `tail -f` | Live log monitoring |
| `touch` | Create files |
| `cp` | Copy files |
| `mv` | Move/rename files |
| `chmod` | Change permissions |
| `find` | Search filesystem |
| `du -sh` | Check directory size |

---

# 🎯 Key DevOps Takeaways

- Linux permissions are critical for security and deployments.
- `tail -f` is one of the most important troubleshooting commands.
- `find` helps quickly locate logs and configs across large systems.
- `chmod +x` is required before running deployment scripts.
- `du -sh` is heavily used during disk space incidents.

