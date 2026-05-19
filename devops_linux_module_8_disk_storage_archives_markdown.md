# 💾 DevOps Linux — Module 8: Disk, Storage & Archives

> Core Linux storage and archive management commands used daily by DevOps and Cloud engineers for backups, disk troubleshooting, storage expansion, compression, and server maintenance.

---

# 1. `df -h` — Check Filesystem Disk Usage

```bash
df -h
```

### What it does
Shows disk usage for mounted filesystems.

### Flags

| Flag | Meaning |
|---|---|
| `-h` | human readable sizes |

### Important Columns

| Column | Meaning |
|---|---|
| `Size` | total disk size |
| `Used` | used space |
| `Avail` | free space |
| `Use%` | disk usage percentage |

### Real World DevOps Use
Critical command used constantly for:
- disk full alerts
- storage troubleshooting
- monitoring servers

---

# 2. `du -sh` — Check Directory Size

```bash
du -sh /var/log
```

### What it does
Shows total directory size.

### Flags

| Flag | Meaning |
|---|---|
| `-s` | summary only |
| `-h` | human readable |

### Real World DevOps Use
Used during:
- disk investigations
- finding huge folders
- log cleanup

---

# 3. `lsblk` — Show Block Devices

```bash
lsblk
```

### What it does
Displays:
- disks
- partitions
- mounted volumes

### Example Output

```text
sda
├─sda1
└─sda2
```

### Real World DevOps Use
Very important in cloud/devops.

Used for:
- EBS volume troubleshooting
- VPS disks
- partition validation
- storage expansion

---

# 4. `mount` — Show Mounted Filesystems

```bash
mount | head
```

### What it does
Displays mounted filesystems.

### Real World DevOps Use
Used constantly for:
- NFS mounts
- Docker mounts
- cloud volumes
- troubleshooting storage

---

# 5. `mount` — Mount Filesystem

```bash
mount /dev/sdb1 /mnt/data
```

### What it does
Attaches filesystem to Linux directory tree.

### Syntax

| Part | Meaning |
|---|---|
| `/dev/sdb1` | disk partition |
| `/mnt/data` | mount point |

### Real World DevOps Use
Used for:
- attaching new disks
- mounting EBS volumes
- storage expansion

---

# 6. `umount` — Unmount Filesystem

```bash
umount /mnt/data
```

### What it does
Safely detaches mounted filesystem.

### Important
⚠️ Never unplug storage before unmounting.

### Real World DevOps Use
Used during:
- maintenance
- migrations
- disk replacements

---

# 7. `tar -cvf` — Create Archive

```bash
tar -cvf backup.tar /var/log
```

### What it does
Creates tar archive file.

### Flags

| Flag | Meaning |
|---|---|
| `-c` | create archive |
| `-v` | verbose |
| `-f` | filename |

### Real World DevOps Use
Used constantly for:
- backups
- migrations
- deployments
- log archiving

---

# 8. `tar -xvf` — Extract Archive

```bash
tar -xvf backup.tar
```

### What it does
Extracts tar archive.

### Flags

| Flag | Meaning |
|---|---|
| `-x` | extract |
| `-v` | verbose |
| `-f` | filename |

### Real World DevOps Use
Used during:
- restores
- deployments
- migrations

---

# 9. `gzip` — Compress File

```bash
gzip largefile.log
```

### What it does
Compresses file using gzip.

### Result

```text
largefile.log.gz
```

### Real World DevOps Use
Used heavily for:
- log compression
- backups
- saving disk space

---

# 10. `gunzip` — Decompress File

```bash
gunzip largefile.log.gz
```

### What it does
Decompresses gzip archive.

### Real World DevOps Use
Used constantly when:
- restoring backups
- reviewing archived logs
- analyzing compressed files

---

# 11. `zip` — Create ZIP Archive

```bash
zip backup.zip test.txt
```

### What it does
Creates ZIP archive.

### Real World DevOps Use
Common for:
- sharing files
- Windows/Linux compatibility
- lightweight backups

---

# 12. `unzip` — Extract ZIP Archive

```bash
unzip backup.zip
```

### What it does
Extracts ZIP file.

### Real World DevOps Use
Used constantly for:
- package extraction
- downloads
- shared archives

---

# 13. `find / -size +100M` — Find Large Files

```bash
find / -size +100M 2>/dev/null
```

### What it does
Finds files larger than 100MB.

### Command Breakdown

| Part | Meaning |
|---|---|
| `+100M` | larger than 100MB |
| `2>/dev/null` | hide permission errors |

### Real World DevOps Use
Very common during:
- disk full incidents
- cleanup operations
- storage troubleshooting

---

# 14. `fdisk -l` — Show Disk Partitions

```bash
fdisk -l
```

### What it does
Lists disks and partitions.

### Real World DevOps Use
Used for:
- disk troubleshooting
- partition validation
- storage provisioning

---

# 15. `free -h` — Check RAM and Swap

```bash
free -h
```

### What it does
Displays:
- RAM usage
- free memory
- swap usage

### Important Columns

| Column | Meaning |
|---|---|
| `total` | total RAM |
| `used` | used RAM |
| `free` | unused RAM |
| `swap` | swap memory |

### Real World DevOps Use
Critical for:
- performance troubleshooting
- memory investigations
- swap analysis

---

# 📌 Core DevOps Commands Learned

| Command | Purpose |
|---|---|
| `df -h` | Check filesystem usage |
| `du -sh` | Check directory size |
| `lsblk` | Show block devices |
| `mount` | Show/mount filesystems |
| `umount` | Unmount filesystems |
| `tar -cvf` | Create archive |
| `tar -xvf` | Extract archive |
| `gzip` | Compress file |
| `gunzip` | Decompress file |
| `zip` | Create ZIP archive |
| `unzip` | Extract ZIP archive |
| `find / -size` | Find large files |
| `fdisk -l` | Show partitions |
| `free -h` | Check RAM and swap |

---

# 🎯 Key DevOps Takeaways

- `df -h` and `du -sh` are essential for disk troubleshooting.
- `lsblk` is critical for cloud storage management.
- `tar` and `gzip` are heavily used for backups and log archives.
- `mount` and `umount` are core Linux storage concepts.
- Large file cleanup is common during production incidents.
- Understanding Linux storage is critical in cloud infrastructure.

