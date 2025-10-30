# SSH Setup Checklist for 3 Hostinger VPS Servers

## Server Information
- **Server 8**: srv1086450.hstgr.cloud (72.60.27.146)
- **Server 2**: srv993275.hstgr.cloud (31.97.131.163)
- **Server 1**: srv990934.hstgr.cloud (69.62.68.46)
- **User**: john
- **Client Machines**: MacBook (first), Mini (second)

---

## Part 1: Setup from MacBook

### Step 1: Generate SSH Key Pair on MacBook
```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_hostinger -C "john@macbook-hostinger"
```
- Press Enter when prompted for passphrase (or add one for extra security)
- This creates `id_ed25519_hostinger` (private key) and `id_ed25519_hostinger.pub` (public key)

### Step 2: Create SSH Config File on MacBook
```bash
nano ~/.ssh/config
```

Add the following configuration:
```
# Hostinger VPS - Server 8
Host vps8
    HostName 72.60.27.146
    User john
    IdentityFile ~/.ssh/id_ed25519_hostinger
    IdentitiesOnly yes

# Hostinger VPS - Server 2
Host vps2
    HostName 31.97.131.163
    User john
    IdentityFile ~/.ssh/id_ed25519_hostinger
    IdentitiesOnly yes

# Hostinger VPS - Server 1
Host vps1
    HostName 69.62.68.46
    User john
    IdentityFile ~/.ssh/id_ed25519_hostinger
    IdentitiesOnly yes
```

Save and exit (Ctrl+O, Enter, Ctrl+X)

Set proper permissions:
```bash
chmod 600 ~/.ssh/config
```

### Step 3: Setup Server 8 (72.60.27.146)
```bash
# SSH in as root
ssh root@72.60.27.146

# Create user john with sudo privileges
adduser john
usermod -aG sudo john

# Setup SSH directory for john
mkdir -p /home/john/.ssh
chmod 700 /home/john/.ssh

# Exit back to MacBook
exit
```

Copy your public key to Server 8:
```bash
ssh-copy-id -i ~/.ssh/id_ed25519_hostinger.pub root@72.60.27.146
```

Then manually add the key to john's authorized_keys:
```bash
ssh root@72.60.27.146

# Copy the authorized key to john's account
cp /root/.ssh/authorized_keys /home/john/.ssh/authorized_keys
chown john:john /home/john/.ssh/authorized_keys
chmod 600 /home/john/.ssh/authorized_keys

# Test that john can sudo
su - john
sudo whoami
# Should output: root

# Exit back to john, then exit to MacBook
exit
exit
```

Test connection as john:
```bash
ssh vps8
# Should connect without password!
```

Disable root login on Server 8:
```bash
ssh vps8
sudo nano /etc/ssh/sshd_config
```

Find and modify this line:
```
PermitRootLogin no
```

Restart SSH service:
```bash
sudo systemctl restart sshd
exit
```

### Step 4: Setup Server 2 (31.97.131.163)
Repeat the same process:
```bash
# SSH in as root
ssh root@31.97.131.163

# Create user john with sudo privileges
adduser john
usermod -aG sudo john

# Setup SSH directory for john
mkdir -p /home/john/.ssh
chmod 700 /home/john/.ssh
exit
```

Copy your public key:
```bash
ssh-copy-id -i ~/.ssh/id_ed25519_hostinger.pub root@31.97.131.163
```

Setup john's authorized_keys:
```bash
ssh root@31.97.131.163

cp /root/.ssh/authorized_keys /home/john/.ssh/authorized_keys
chown john:john /home/john/.ssh/authorized_keys
chmod 600 /home/john/.ssh/authorized_keys

# Test sudo
su - john
sudo whoami
exit
exit
```

Test and disable root:
```bash
# Test connection
ssh vps2

# Disable root login
sudo nano /etc/ssh/sshd_config
# Set: PermitRootLogin no

sudo systemctl restart sshd
exit
```

### Step 5: Setup Server 1 (69.62.68.46)
Repeat once more:
```bash
# SSH in as root
ssh root@69.62.68.46

# Create user john with sudo privileges
adduser john
usermod -aG sudo john

# Setup SSH directory for john
mkdir -p /home/john/.ssh
chmod 700 /home/john/.ssh
exit
```

Copy your public key:
```bash
ssh-copy-id -i ~/.ssh/id_ed25519_hostinger.pub root@69.62.68.46
```

Setup john's authorized_keys:
```bash
ssh root@69.62.68.46

cp /root/.ssh/authorized_keys /home/john/.ssh/authorized_keys
chown john:john /home/john/.ssh/authorized_keys
chmod 600 /home/john/.ssh/authorized_keys

# Test sudo
su - john
sudo whoami
exit
exit
```

Test and disable root:
```bash
# Test connection
ssh vps1

# Disable root login
sudo nano /etc/ssh/sshd_config
# Set: PermitRootLogin no

sudo systemctl restart sshd
exit
```

### Step 6: Verify MacBook Setup
Test all three connections:
```bash
ssh vps8
exit

ssh vps2
exit

ssh vps1
exit
```

All should connect without passwords! ✅

---

## Part 2: Setup Mini (Later This Week)

### Step 1: Copy SSH Key from MacBook to Mini
On MacBook, display your private key:
```bash
cat ~/.ssh/id_ed25519_hostinger
```

On Mini, create the key file:
```bash
nano ~/.ssh/id_ed25519_hostinger
```
Paste the private key content, save and exit.

Copy the public key:
On MacBook:
```bash
cat ~/.ssh/id_ed25519_hostinger.pub
```

On Mini:
```bash
nano ~/.ssh/id_ed25519_hostinger.pub
```
Paste the public key content, save and exit.

Set proper permissions on Mini:
```bash
chmod 600 ~/.ssh/id_ed25519_hostinger
chmod 644 ~/.ssh/id_ed25519_hostinger.pub
```

### Step 2: Create SSH Config on Mini
```bash
nano ~/.ssh/config
```

Add the exact same configuration as MacBook:
```
# Hostinger VPS - Server 8
Host vps8
    HostName 72.60.27.146
    User john
    IdentityFile ~/.ssh/id_ed25519_hostinger
    IdentitiesOnly yes

# Hostinger VPS - Server 2
Host vps2
    HostName 31.97.131.163
    User john
    IdentityFile ~/.ssh/id_ed25519_hostinger
    IdentitiesOnly yes

# Hostinger VPS - Server 1
Host vps1
    HostName 69.62.68.46
    User john
    IdentityFile ~/.ssh/id_ed25519_hostinger
    IdentitiesOnly yes
```

Set proper permissions:
```bash
chmod 600 ~/.ssh/config
```

### Step 3: Test Mini Connections
```bash
ssh vps8
exit

ssh vps2
exit

ssh vps1
exit
```

All should work! ✅

---

## Quick Reference

### Connect to any server:
```bash
ssh vps8    # Server 8
ssh vps2    # Server 2
ssh vps1    # Server 1
```

### Run commands without logging in:
```bash
ssh vps8 'uptime'
ssh vps2 'df -h'
```

### Copy files:
```bash
# To server
scp myfile.txt vps8:/home/john/

# From server
scp vps8:/home/john/myfile.txt ./
```

---

## Security Notes

✅ **Completed:**
- SSH key authentication enabled
- Root login disabled
- User 'john' has sudo privileges
- Single key pair for simplicity

⚠️ **Future Hardening (when ready):**
- Disable password authentication: `PasswordAuthentication no` in `/etc/ssh/sshd_config`
- Change SSH port from 22 to custom port
- Install fail2ban for brute-force protection
- Setup firewall (ufw)

---

## Troubleshooting

**If connection fails:**
```bash
# Check SSH with verbose output
ssh -v vps8

# Verify key permissions
ls -la ~/.ssh/
# id_ed25519_hostinger should be 600
# id_ed25519_hostinger.pub should be 644
# config should be 600
```

**If key not being used:**
```bash
# Test key explicitly
ssh -i ~/.ssh/id_ed25519_hostinger john@72.60.27.146
```

**Fix permissions if needed:**
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519_hostinger
chmod 644 ~/.ssh/id_ed25519_hostinger.pub
chmod 600 ~/.ssh/config
```
