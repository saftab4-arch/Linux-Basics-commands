# 🔐 DevOps Linux — Module 7: Users, SSH & Security

> Core Linux user management and SSH security commands used daily by DevOps and Cloud engineers for server access, permissions, authentication, and secure administration.

---

# 1. `whoami` — Show Current User

```bash
whoami
```

### What it does
Displays the currently logged-in user.

### Example Output

```text
ubuntu
```

### Real World DevOps Use
Used constantly before:
- running sudo commands
- modifying configs
- changing permissions

---

# 2. `id` — Show User UID and Groups

```bash
id
```

### What it does
Displays:
- UID
- GID
- user groups

### Example Output

```text
uid=1000(ubuntu) gid=1000(ubuntu) groups=1000(ubuntu),27(sudo)
```

### Real World DevOps Use
Used for:
- permission troubleshooting
- checking sudo access
- validating group memberships

---

# 3. `groups` — Show User Groups

```bash
groups
```

### What it does
Shows groups current user belongs to.

### Real World DevOps Use
Important for:
- Docker permissions
- sudo access
- Kubernetes access
- shared systems

---

# 4. `useradd` — Create User

```bash
sudo useradd devopsuser
```

### What it does
Creates a new Linux user.

### Real World DevOps Use
Used for:
- onboarding engineers
- service accounts
- automation users

---

# 5. `passwd` — Set User Password

```bash
sudo passwd devopsuser
```

### What it does
Sets or changes Linux user password.

### Real World DevOps Use
Used during:
- onboarding
- password resets
- account maintenance

---

# 6. `su -` — Switch User

```bash
su - devopsuser
```

### What it does
Switches into another user account.

### Flags

| Flag | Meaning |
|---|---|
| `-` | full login shell |

### Real World DevOps Use
Used for:
- permission testing
- troubleshooting
- environment validation

---

# 7. `sudo` — Run as Root

```bash
sudo apt update
```

### What it does
Runs command with elevated privileges.

### Real World DevOps Use
One of the MOST used Linux commands.

Required for:
- package installs
- service management
- system administration
- config changes

---

# 8. `sudo -l` — Check Sudo Permissions

```bash
sudo -l
```

### What it does
Displays commands user can run with sudo.

### Real World DevOps Use
Used for:
- RBAC validation
- auditing permissions
- troubleshooting access

---

# 9. `ssh` — Connect to Remote Server

```bash
ssh ubuntu@10.0.0.5
```

### What it does
Securely connects to remote Linux server.

### Syntax

```text
ssh user@server-ip
```

### Real World DevOps Use
CORE DevOps command.

Used constantly for:
- EC2 management
- cloud servers
- VPS administration
- Kubernetes nodes

---

# 10. `scp` — Secure File Copy

```bash
scp file.txt ubuntu@10.0.0.5:/home/ubuntu
```

### What it does
Securely transfers files between systems.

### Command Breakdown

| Part | Meaning |
|---|---|
| `scp` | secure copy |
| `file.txt` | local file |
| `ubuntu@10.0.0.5` | remote server |
| `/home/ubuntu` | destination path |

### Real World DevOps Use
Used constantly for:
- deployments
- config transfers
- backup files
- scripts

---

# 11. `ssh-keygen` — Generate SSH Keys

```bash
ssh-keygen
```

### What it does
Creates SSH public/private key pair.

### Generated Files

| File | Purpose |
|---|---|
| `id_rsa` | private key |
| `id_rsa.pub` | public key |

### Real World DevOps Use
Used for:
- passwordless SSH
- GitHub authentication
- automation
- secure server access

---

# 12. `chmod 600` — Secure Private Key

```bash
chmod 600 ~/.ssh/id_rsa
```

### What it does
Restricts private key permissions.

### Permission Meaning

| Number | Meaning |
|---|---|
| 6 | read + write |
| 0 | no access |
| 0 | no access |

### Real World DevOps Use
SSH refuses insecure private keys.

Critical Linux security practice.

---

# 13. `ssh-copy-id` — Copy SSH Key to Server

```bash
ssh-copy-id ubuntu@10.0.0.5
```

### What it does
Installs SSH public key onto remote server.

### Real World DevOps Use
Used for:
- passwordless login
- automation
- Ansible
- CI/CD pipelines

---

# 14. `last` — Show Login History

```bash
last
```

### What it does
Displays recent Linux login history.

### Real World DevOps Use
Used for:
- access auditing
- suspicious login investigations
- security reviews

---

# 15. `history` — Show Command History

```bash
history | tail
```

### What it does
Shows recent terminal commands.

### Real World DevOps Use
Useful for:
- troubleshooting
- recovering commands
- auditing activity

---

# 📌 Core DevOps Commands Learned

| Command | Purpose |
|---|---|
| `whoami` | Show current user |
| `id` | Show UID/GID/groups |
| `groups` | Show group memberships |
| `useradd` | Create user |
| `passwd` | Set password |
| `su -` | Switch user |
| `sudo` | Run privileged commands |
| `sudo -l` | Check sudo permissions |
| `ssh` | Connect to remote server |
| `scp` | Secure file transfer |
| `ssh-keygen` | Generate SSH keys |
| `chmod 600` | Secure SSH private key |
| `ssh-copy-id` | Copy SSH key to server |
| `last` | Login history |
| `history` | Command history |

---

# 🎯 Key DevOps Takeaways

- SSH is the backbone of Linux server administration.
- SSH keys are far more secure than passwords.
- `sudo` provides controlled privileged access.
- `chmod 600` is mandatory for securing private keys.
- User/group permissions are critical for RBAC and security.
- `scp` and SSH automation are heavily used in DevOps workflows.

