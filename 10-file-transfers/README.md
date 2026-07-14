# File Transfers in Linux: `scp`, `rsync`, `wget`, and `curl`

These are some of the most commonly used Linux file transfer tools. Each one is designed for different situations.

| Command | Purpose                             | Works Over Network?          | Best Used For                         |
| ------- | ----------------------------------- | ---------------------------- | ------------------------------------- |
| `scp`   | Securely copy files between systems | Yes (SSH)                    | Quick file transfers between machines |
| `rsync` | Synchronize files and directories   | Yes (SSH, local, and remote) | Backups and syncing large folders     |
| `wget`  | Download files from the internet    | Yes (HTTP/HTTPS/FTP)         | Downloading files and websites        |
| `curl`  | Transfer data using many protocols  | Yes                          | APIs, web requests, downloading files |

---

# 1. `scp` (Secure Copy Protocol)

`scp` securely transfers files between two systems using SSH encryption.

## Syntax

```bash id="scp1"
scp source destination
```

---

## Copy a Local File to a Remote Server

```bash id="scp2"
scp report.pdf user@192.168.1.10:/home/user/
```

This means:

* `report.pdf` → File to send.
* `user` → Username on the remote machine.
* `192.168.1.10` → Remote IP address.
* `/home/user/` → Destination directory.

---

## Copy a File from a Remote Machine to a Local Machine

```bash id="scp3"
scp user@192.168.1.10:/home/user/report.pdf .
```

The `.` represents the current directory.

---

## Copy an Entire Directory

```bash id="scp4"
scp -r project/ user@192.168.1.10:/home/user/
```

`-r` means recursive copy.

---

## Example

Suppose your Kali machine contains:

```text id="scp5"
nmap_scan.txt
```

You want to transfer it to your Ubuntu server:

```bash id="scp6"
scp nmap_scan.txt admin@10.10.10.5:/home/admin/
```

---

# 2. `rsync`

`rsync` is smarter than `scp`.

Instead of copying everything every time, it transfers only the changed portions of files, making it ideal for backups and synchronization.

## Syntax

```bash id="rsync1"
rsync [options] source destination
```

---

## Copy a Directory Locally

```bash id="rsync2"
rsync -av Documents/ Backup/
```

Options:

* `-a` → Archive mode (preserves permissions, timestamps, symbolic links, and ownership when possible).
* `-v` → Verbose output.

---

## Sync Files to a Remote Server

```bash id="rsync3"
rsync -av project/ user@192.168.1.10:/home/user/project/
```

---

## Delete Files on the Destination That No Longer Exist in the Source

```bash id="rsync4"
rsync -av --delete project/ backup/
```

---

## Show Progress During Transfer

```bash id="rsync5"
rsync -av --progress project/ user@192.168.1.10:/backup/
```

---

## Example

Suppose you maintain a GitHub notes repository and only today's notes changed:

```bash id="rsync6"
rsync -av notes/ backup-server:/notes/
```

Only modified files are transferred instead of the entire repository.

---

# `scp` vs `rsync`

| Feature                      | `scp` | `rsync` |
| ---------------------------- | ----- | ------- |
| Secure Transfer              | Yes   | Yes     |
| Transfers only changed files | No    | Yes     |
| Resume interrupted transfer  | No*   | Yes     |
| Best for backups             | No    | Yes     |
| Simple one-time copy         | Yes   | Yes     |

* Traditional `scp` cannot resume interrupted transfers.

For backups and labs, `rsync` is generally preferred.

---

# 3. `wget`

`wget` downloads files from the internet.

## Download a File

```bash id="wget1"
wget https://example.com/file.zip
```

---

## Save with a Different Filename

```bash id="wget2"
wget -O tools.zip https://example.com/file.zip
```

---

## Continue an Interrupted Download

```bash id="wget3"
wget -c https://example.com/kali.iso
```

---

## Download in the Background

```bash id="wget4"
wget -b https://example.com/largefile.iso
```

---

## Download an Entire Website for Offline Viewing

```bash id="wget5"
wget --mirror https://example.com
```

---

## Example

Download a Kali Linux ISO:

```bash id="wget6"
wget https://cdimage.kali.org/kali.iso
```

---

# 4. `curl`

`curl` transfers data using many protocols such as HTTP, HTTPS, FTP, SCP, and many others.

It is heavily used by developers, DevOps engineers, and security professionals.

---

## View Webpage Source Code

```bash id="curl1"
curl https://example.com
```

---

## Download a File

```bash id="curl2"
curl -O https://example.com/file.zip
```

---

## Save with a Custom Name

```bash id="curl3"
curl -o tools.zip https://example.com/file.zip
```

---

## Follow Redirects

```bash id="curl4"
curl -L https://example.com/download
```

---

## Send an HTTP GET Request

```bash id="curl5"
curl https://api.github.com/users/octocat
```

---

## Send JSON Data Using POST

```bash id="curl6"
curl -X POST \
-H "Content-Type: application/json" \
-d '{"username":"junaid","role":"student"}' \
https://example.com/api/users
```

---

## Example for Security Testing

Check HTTP response headers:

```bash id="curl7"
curl -I https://example.com
```

Example output:

```text id="curl8"
HTTP/2 200
server: nginx
content-type: text/html
strict-transport-security: max-age=31536000
```

This technique is frequently used during reconnaissance and web application testing.

---

# Quick Comparison

| Feature                  | `scp` | `rsync` | `wget`  | `curl`   |
| ------------------------ | ----- | ------- | ------- | -------- |
| Uses SSH                 | Yes   | Usually | No      | Optional |
| Downloads from websites  | No    | No      | Yes     | Yes      |
| Supports APIs            | No    | No      | Limited | Yes      |
| Best for backups         | No    | Yes     | No      | No       |
| Supports synchronization | No    | Yes     | No      | No       |
| Common in pentesting     | Yes   | Yes     | Yes     | Yes      |
