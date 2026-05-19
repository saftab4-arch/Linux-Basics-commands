# 📦 DevOps Linux — Module 6: Packages & Updates

> Core Linux package management commands used daily by DevOps and Cloud engineers for installing software, patching systems, updating servers, troubleshooting dependencies, and maintaining production environments.

---

# 1. `apt update` — Refresh Package Repository

```bash
apt update
```

### What it does
Downloads the latest package metadata from repositories.

⚠️ Important:
This command does NOT install updates.

### Real World DevOps Use
Always run before:
- installing packages
- upgrades
- security patching

### Important Concept

| Command | Purpose |
|---|---|
| `apt update` | refresh package list |
| `apt upgrade` | install updates |

---

# 2. `apt upgrade -y` — Install Updates

```bash
apt upgrade -y
```

### What it does
Upgrades installed packages to newer versions.

### Flags

| Flag | Meaning |
|---|---|
| `-y` | auto confirm prompts |

### Real World DevOps Use
Used during:
- patching
- maintenance windows
- vulnerability remediation

---

# 3. `apt install` — Install Package

```bash
apt install nginx -y
```

### What it does
Installs software package.

### Real World DevOps Use
Used constantly for:
- nginx
- docker
- git
- htop
- kubernetes tools

---

# 4. `apt remove` — Remove Package

```bash
apt remove nginx -y
```

### What it does
Removes installed package.

### Important
Configuration files may still remain.

### Real World DevOps Use
Used during:
- cleanup
- removing unused software
- troubleshooting

---

# 5. `apt purge` — Remove Package + Configs

```bash
apt purge nginx -y
```

### What it does
Removes:
- package
- configuration files

### Difference

| Command | Meaning |
|---|---|
| `remove` | remove package only |
| `purge` | remove package + configs |

---

# 6. `apt autoremove` — Remove Unused Packages

```bash
apt autoremove -y
```

### What it does
Deletes unused dependency packages.

### Real World DevOps Use
Used to:
- free disk space
- clean servers
- remove orphaned packages

---

# 7. `apt search` — Search Packages

```bash
apt search nginx
```

### What it does
Searches repositories for packages.

### Real World DevOps Use
Used when:
- finding software
- checking package names
- exploring available tools

---

# 8. `dpkg -l` — List Installed Packages

```bash
dpkg -l | head
```

### What it does
Lists installed Debian packages.

### Important Concept

| Tool | Purpose |
|---|---|
| `apt` | high-level package manager |
| `dpkg` | low-level Debian package tool |

### Real World DevOps Use
Used for:
- inventory checks
- audits
- troubleshooting installed software

---

# 9. `dpkg -i` — Install Local `.deb` Package

```bash
dpkg -i package.deb
```

### What it does
Installs local Debian package file.

### Real World DevOps Use
Used for:
- offline installs
- vendor applications
- custom software packages

---

# 10. `snap list` — List Snap Packages

```bash
snap list
```

### What it does
Displays installed Snap packages.

### What is Snap?
Ubuntu package management system for sandboxed applications.

### Real World DevOps Use
Common on Ubuntu environments.

---

# 11. `snap install` — Install Snap Package

```bash
snap install btop
```

### What it does
Installs package using Snap.

### Real World DevOps Use
Used for newer application versions.

---

# 12. `snap remove` — Remove Snap Package

```bash
snap remove btop
```

### What it does
Removes Snap package.

---

# 13. `which` — Locate Binary Path

```bash
which nginx
```

### What it does
Shows executable location.

### Example Output

```text
/usr/sbin/nginx
```

### Real World DevOps Use
Used heavily in:
- scripting
- debugging
- automation

---

# 14. `whereis` — Locate Binary + Docs + Source

```bash
whereis nginx
```

### What it does
Shows:
- binary path
- source path
- man pages

### Difference

| Command | Meaning |
|---|---|
| `which` | executable only |
| `whereis` | binary + docs + source |

---

# 15. `uname -a` — Show Kernel Information

```bash
uname -a
```

### What it does
Displays:
- kernel version
- architecture
- hostname
- OS details

### Flags

| Flag | Meaning |
|---|---|
| `-a` | show all information |

### Real World DevOps Use
Used during:
- troubleshooting
- compatibility checks
- kernel debugging

---

# 📌 Core DevOps Commands Learned

| Command | Purpose |
|---|---|
| `apt update` | Refresh repositories |
| `apt upgrade` | Install updates |
| `apt install` | Install packages |
| `apt remove` | Remove packages |
| `apt purge` | Remove package + configs |
| `apt autoremove` | Cleanup unused packages |
| `apt search` | Search repositories |
| `dpkg -l` | List installed packages |
| `dpkg -i` | Install local .deb package |
| `snap list` | Show Snap packages |
| `snap install` | Install Snap package |
| `snap remove` | Remove Snap package |
| `which` | Locate executable |
| `whereis` | Locate binary + docs |
| `uname -a` | Show kernel/system info |

---

# 🎯 Key DevOps Takeaways

- `apt update` refreshes repositories but does not install updates.
- `apt upgrade` performs actual package upgrades.
- `autoremove` helps keep Linux servers clean.
- `dpkg` works at a lower level than `apt`.
- `which` and `whereis` are heavily used in scripting and debugging.
- Keeping Linux servers patched is critical for security and compliance.

