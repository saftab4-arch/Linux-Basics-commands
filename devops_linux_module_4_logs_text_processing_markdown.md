# 📜 DevOps Linux — Module 4: Logs & Text Processing

> Core Linux log analysis and text processing commands used daily by DevOps and Cloud engineers for troubleshooting, monitoring, debugging, and automation.

---

# 1. `tail -f /var/log/syslog` — Live Log Monitoring

```bash
tail -f /var/log/syslog
```

### What it does
Streams new log entries live in real time.

### Real World DevOps Use
One of the MOST important troubleshooting commands in Linux.

Used during:
- deployments
- debugging
- service restarts
- monitoring live activity

### Example Output
![tail-f](tail-f-var-log-syslog.png)

---

# 2. `tail -20 /var/log/syslog` — View Recent Logs

```bash
tail -20 /var/log/syslog
```

### What it does
Shows the last 20 lines from the log file.

### Real World DevOps Use
Used constantly to quickly inspect the most recent events and errors.

### Example Output
![tail-20](tail-20-view-recent-logs.png)

---

# 3. `head -20 /etc/services` — View First Lines

```bash
head -20 /etc/services
```

### What it does
Displays the first 20 lines of a file.

### Real World DevOps Use
Useful for quickly checking configs and large files.

### Example Output
![head](head-20-services.png)

---

# 4. `grep ssh /etc/services` — Search for Text

```bash
grep ssh /etc/services
```

### What it does
Searches for lines containing:

```text
ssh
```

### Real World DevOps Use
Used daily to search:
- logs
- configs
- errors
- service entries

### Example Output
![grep](grep-ssh-services.png)

---

# 5. `grep -i error /var/log/syslog` — Case Insensitive Search

```bash
grep -i error /var/log/syslog
```

### What it does
Searches for the word:

```text
error
```

ignoring uppercase/lowercase.

### Flags
- `-i` → ignore case

### Real World DevOps Use
Very common for investigating application or system failures.

### Example Output
![grep-i](grep-i-error.png)

---

# 6. `grep -c ssh /etc/services` — Count Matching Lines

```bash
grep -c ssh /etc/services
```

### What it does
Counts total matching lines.

### Flags
- `-c` → count matches

### Real World DevOps Use
Used for:
- counting errors
- counting requests
- counting log entries

### Example Output
![grep-c](grep-c-ssh.png)

---

# 7. `cat /var/log/syslog | grep systemd` — Filter Logs with Pipe

```bash
cat /var/log/syslog | grep systemd
```

### What it does
Filters logs for lines containing:

```text
systemd
```

### Important Concept
The pipe:

```text
|
```

sends output from one command into another.

### Real World DevOps Use
One of the most important Linux concepts.

Used heavily in:
- automation
- monitoring
- troubleshooting
- scripting

### Example Output
![pipe](cat-syslog-grep-systemd.png)

---

# 8. `awk` — Extract Columns / Parse Logs

```bash
awk '{print $1,$2,$3}' /var/log/syslog | head
```

### What it does
Extracts specific columns from text.

### Column Meaning
| Variable | Meaning |
|---|---|
| `$1` | First column |
| `$2` | Second column |
| `$3` | Third column |

### Real World DevOps Use
Very common for:
- parsing logs
- extracting timestamps
- automation scripts
- monitoring pipelines

### Example Output
![awk](awk-parse-logs.png)

---

# 9. `sed` — Replace Text in Output

```bash
sed 's/ssh/SSH/g' /etc/services | head
```

### What it does
Replaces text in command output.

### Flags Breakdown
| Part | Meaning |
|---|---|
| `s` | substitute |
| `ssh` | original text |
| `SSH` | replacement text |
| `g` | global replace |

### Real World DevOps Use
Used constantly in:
- automation
- config edits
- deployments
- scripting

### Example Output
![sed](sed-replace-text.png)

---

# 10. `sort` — Sort Output Alphabetically

```bash
sort /etc/services | head
```

### What it does
Sorts output alphabetically.

### Useful Flags
| Flag | Meaning |
|---|---|
| `-r` | reverse order |
| `-n` | numeric sort |
| `-u` | unique only |

### Real World DevOps Use
Used for:
- organizing outputs
- log analysis
- finding duplicate entries

### Example Output
![sort](sort-output.png)

---

# 11. `uniq` — Remove Duplicate Lines

```bash
sort test.txt | uniq
```

### What it does
Removes duplicate lines.

### Important
Usually combined with:

```bash
sort file.txt | uniq
```

### Useful Flags
| Flag | Meaning |
|---|---|
| `-c` | count duplicates |
| `-d` | show duplicates only |

### Real World DevOps Use
Used for:
- repeated log analysis
- duplicate IP detection
- repeated error analysis

### Example Output
![uniq](uniq-remove-duplicates.png)

### Additional Example
![uniq-flags](uniq-flags.png)

---

# 12. `wc -l` — Count Lines

```bash
wc -l /etc/services
```

### What it does
Counts total lines in a file.

### Flags
| Flag | Meaning |
|---|---|
| `-l` | count lines |
| `-w` | count words |
| `-c` | count bytes |

### Real World DevOps Use
Used constantly for:
- log counting
- error counting
- validating outputs

### Example Output
![wc](wc-lines.png)

---

# 13. `journalctl -f` — Live Systemd Logs

```bash
journalctl -f
```

### What it does
Streams Linux systemd logs live.

### Flags
- `-f` → follow/live mode

### Real World DevOps Use
Extremely important for:
- service debugging
- daemon troubleshooting
- boot logs
- container/service monitoring

### Example Output
![journalctl-f](journalctl-f.png)

---

# 14. `journalctl -p err -b` — Show Boot Errors

```bash
journalctl -p err -b
```

### What it does
Shows boot-related error logs.

### Flags
| Flag | Meaning |
|---|---|
| `-p err` | errors only |
| `-b` | current boot logs |

### Real World DevOps Use
Used for:
- startup failures
- network issues
- failed services
- boot troubleshooting

### Example Output
![journalctl-errors](journalctl-errors.png)

---

# 15. `journalctl | grep ssh` — Search SSH Logs

```bash
journalctl | grep ssh
```

### What it does
Searches system journal logs for SSH-related entries.

### Real World DevOps Use
Used constantly for:
- failed SSH login investigations
- authentication troubleshooting
- brute force detection
- access auditing

### Example Output
![journalctl-ssh](journalctl-grep-ssh.png)

---

# 📌 Core DevOps Commands Learned

| Command | Purpose |
|---|---|
| `tail -f` | Live log monitoring |
| `tail -20` | Show recent logs |
| `head` | Show first lines |
| `grep` | Search text |
| `grep -i` | Case insensitive search |
| `grep -c` | Count matches |
| `awk` | Extract columns |
| `sed` | Replace text |
| `sort` | Sort output |
| `uniq` | Remove duplicates |
| `wc -l` | Count lines |
| `journalctl` | Systemd logs |

---

# 🎯 Key DevOps Takeaways

- `grep` and `journalctl` are some of the most important Linux troubleshooting tools.
- Pipes (`|`) allow chaining commands together for powerful log filtering.
- `tail -f` and `journalctl -f` are critical during deployments and outages.
- `awk` and `sed` are heavily used in automation and scripting.
- `wc -l` and `uniq -c` help analyze large log datasets quickly.

