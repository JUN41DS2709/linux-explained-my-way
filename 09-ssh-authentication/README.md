# SSH Authentication: Password Authentication vs Key-Based Authentication

In Linux, **SSH (Secure Shell)** allows you to securely connect to another machine over a network. SSH supports two common authentication methods:

1. **Password Authentication**
2. **Public Key Authentication (Key-Based Authentication)**

---

# 1. Password Authentication

This is the simplest authentication method.

## How It Works

1. You run an SSH command.
2. The SSH server asks for your password.
3. If the password matches the account on the remote machine, access is granted.

### Connection Command

```bash id="sshpwd1"
ssh username@192.168.1.10
```

Example:

```bash id="sshpwd2"
ssh junaid@192.168.1.10
```

Output:

```text id="sshpwd3"
junaid@192.168.1.10's password:
```

After entering the correct password:

```text id="sshpwd4"
Welcome to Ubuntu 24.04 LTS
junaid@server:~$
```

---

## Server Configuration

Password authentication is controlled through:

```text id="sshpwd5"
/etc/ssh/sshd_config
```

Check whether it is enabled:

```bash id="sshpwd6"
sudo grep PasswordAuthentication /etc/ssh/sshd_config
```

Example output:

```text id="sshpwd7"
PasswordAuthentication yes
```

To enable password authentication:

```bash id="sshpwd8"
sudo nano /etc/ssh/sshd_config
```

Set:

```text id="sshpwd9"
PasswordAuthentication yes
```

Restart the SSH service.

### Ubuntu/Debian

```bash id="sshpwd10"
sudo systemctl restart ssh
```

### RHEL/CentOS

```bash id="sshpwd11"
sudo systemctl restart sshd
```

---

## Security Risks of Password Authentication

Password authentication is vulnerable to:

* Brute-force attacks
* Password guessing
* Credential stuffing
* Weak passwords
* Password reuse

---

# 2. SSH Key Authentication

This method uses a **key pair**:

* **Private Key** → Stays on your local machine.
* **Public Key** → Copied to the server.

Authentication happens using cryptography instead of passwords.

---

## Step 1: Generate an SSH Key Pair

Generate an Ed25519 key (recommended):

```bash id="sshkey1"
ssh-keygen -t ed25519 -C "junaid@laptop"
```

Older RSA method:

```bash id="sshkey2"
ssh-keygen -t rsa -b 4096 -C "junaid@laptop"
```

Example output:

```text id="sshkey3"
Generating public/private ed25519 key pair.
Enter file in which to save the key:
/home/junaid/.ssh/id_ed25519
```

Press `Enter`.

Resulting files:

```text id="sshkey4"
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

Verify:

```bash id="sshkey5"
ls -l ~/.ssh/
```

Example:

```text id="sshkey6"
-rw------- id_ed25519
-rw-r--r-- id_ed25519.pub
```

---

## Step 2: Copy the Public Key to the Server

Recommended method:

```bash id="sshkey7"
ssh-copy-id username@192.168.1.10
```

Example:

```bash id="sshkey8"
ssh-copy-id junaid@192.168.1.10
```

This copies:

```text id="sshkey9"
~/.ssh/id_ed25519.pub
```

to the server's:

```text id="sshkey10"
~/.ssh/authorized_keys
```

file.

---

## Manual Method

View the public key:

```bash id="sshkey11"
cat ~/.ssh/id_ed25519.pub
```

On the server:

```bash id="sshkey12"
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
```

Paste the public key and save the file.

Set the correct permissions:

```bash id="sshkey13"
chmod 600 ~/.ssh/authorized_keys
```

---

## Step 3: Connect Using Key Authentication

```bash id="sshkey14"
ssh username@192.168.1.10
```

SSH automatically attempts to use your private key:

```text id="sshkey15"
~/.ssh/id_ed25519
```

If authentication succeeds, no password prompt appears.

---

## Using a Specific Private Key

```bash id="sshkey16"
ssh -i ~/.ssh/my_key username@192.168.1.10
```

Example:

```bash id="sshkey17"
ssh -i ~/.ssh/redteam_key kali@10.10.10.5
```

---

## Verbose Mode for Troubleshooting

Basic debugging:

```bash id="sshkey18"
ssh -v username@192.168.1.10
```

More details:

```bash id="sshkey19"
ssh -vv username@192.168.1.10
```

Maximum debugging:

```bash id="sshkey20"
ssh -vvv username@192.168.1.10
```

Example output:

```text id="sshkey21"
debug1: Offering public key:
debug1: Server accepts key:
Authenticated to 192.168.1.10 using "publickey".
```

---

## Server Configuration for Key Authentication

Check the configuration:

```bash id="sshkey22"
sudo grep PubkeyAuthentication /etc/ssh/sshd_config
```

Example output:

```text id="sshkey23"
PubkeyAuthentication yes
```

To enable it:

```text id="sshkey24"
PubkeyAuthentication yes
```

Restart SSH:

```bash id="sshkey25"
sudo systemctl restart ssh
```

---

# Disabling Password Authentication (Recommended)

After verifying that key authentication works correctly:

```bash id="sshkey26"
sudo nano /etc/ssh/sshd_config
```

Set:

```text id="sshkey27"
PasswordAuthentication no
PubkeyAuthentication yes
```

Restart SSH:

```bash id="sshkey28"
sudo systemctl restart ssh
```

Now only users with valid private keys can authenticate.

---

# Authentication Flow Comparison

## Password Authentication

```text id="sshflow1"
Client ---- username/password ----> Server
                     |
              Server verifies password
                     |
                Access granted
```

---

## Key Authentication

```text id="sshflow2"
Client ---- proves possession of private key ----> Server
                     |
          Server verifies matching public key
                     |
                Access granted
```

---

# Comparison Table

| Feature                          | Password Authentication | Key Authentication |
| -------------------------------- | ----------------------- | ------------------ |
| Requires password                | Yes                     | No                 |
| Resistant to brute-force attacks | No                      | Yes                |
| Supports automation              | Poor                    | Excellent          |
| Security level                   | Lower                   | Higher             |
| Recommended for servers          | No                      | Yes                |

---

# Common SSH Commands Cheat Sheet

```bash id="sshcheat1"
# Login using password or key
ssh user@host

# Specify a custom port
ssh -p 2222 user@host

# Generate a key pair
ssh-keygen -t ed25519

# Copy public key to the server
ssh-copy-id user@host

# Use a specific private key
ssh -i ~/.ssh/id_ed25519 user@host

# Debug an SSH connection
ssh -vvv user@host
```

---

## Security Perspective

For penetration testing and red team operations, understanding SSH key authentication is important because compromised private keys can provide persistent access to systems.

From a defensive perspective, many administrators harden servers by disabling password authentication and allowing only key-based authentication.
