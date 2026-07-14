# Understanding the Linux Filesystem Hierarchy

## 1. Everything is a File

In Linux, almost everything is treated as a file:

* Regular files (documents, scripts)
* Directories (folders)
* Hard drives
* USB devices
* Keyboard and mouse
* Network interfaces
* Running processes (through `/proc`)

Example:

```bash
ls -l /dev/sda
```

`/dev/sda` is a storage device, but Linux treats it like a file.

---

## 2. Linux Filesystem Tree

Unlike Windows, Linux uses a single directory tree.

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── root
├── usr
└── var
```

Everything starts from `/`, which is called the **root directory**.

---

## 3. Important Directories

### `/`

The beginning of the entire filesystem.

Example:

```bash
cd /
```

---

### `/home`

Contains users' personal files and directories.

Example:

```text
/home
├── junaid
├── alice
└── bob
```

A user's personal files are usually stored in locations such as:

```text
/home/junaid/Documents
/home/junaid/Desktop
/home/junaid/Downloads
```

Go to your home directory:

```bash
cd ~
```

or

```bash
cd /home/junaid
```

---

### `/root`

The home directory of the root (administrator) user.

```text
/root
```

Normal users cannot access it without proper permissions.

---

### `/bin`

Contains essential commands used by everyone.

The commands we type are executable files stored in locations such as `/bin`. For example, when we type `ls`, Linux actually executes `/bin/ls` in the background.

Examples:

```text
ls
cp
mv
cat
echo
pwd
rm
```

Example:

```bash
/bin/ls
```

---

### `/sbin`

Contains system administration commands.

Examples:

```text
reboot
shutdown
fdisk
mount
iptables
```

These commands are typically used by the root user or administrators.

---

## `/etc`

The `/etc` directory is an important folder that contains system configuration files.

In Windows, settings are often stored in the Control Panel or Registry. In Linux, many settings are stored as files inside `/etc`.

Because of this, `/etc` is commonly referred to as the **Configuration Directory**.

### Important Files Inside `/etc`

#### `/etc/passwd`

Contains information about all users on the system, such as:

* Username
* User ID (UID)
* Home directory
* Login shell

Passwords are **not** stored here in plain text.

Example:

```bash
cat /etc/passwd
```

---

#### `/etc/shadow`

Contains users' encrypted password hashes.

Only the root user can access this file.

Example:

```bash
sudo cat /etc/shadow
```

---

#### `/etc/hosts`

Used to manually map domain names to IP addresses.

Example:

```text
127.0.0.1 localhost
```

---

#### `/etc/hostname`

Contains the hostname of the computer.

Example:

```bash
cat /etc/hostname
```

---

#### `/etc/ssh`

Contains SSH server configuration files.

If SSH settings need to be modified, changes are usually made here.

---

#### `/etc/fstab`

Contains information about which drives or partitions should be mounted during system boot.

---

### Cybersecurity Perspective

For Ethical Hacking, Penetration Testing, and Red Teaming, `/etc` is extremely important because it can contain useful information such as:

* Existing users on the system (`/etc/passwd`)
* Password hashes (`/etc/shadow`)
* Users with sudo privileges (`/etc/sudoers`)
* Network configuration (`/etc/hosts`)

---

### `/var`

Contains variable data that changes frequently.

Examples include:

* Logs
* Cache
* Mail
* Databases
* Web server files

Examples:

```text
/var/log/
/var/cache/
/var/tmp/
```

Check logs:

```bash
cd /var/log
ls
```

---

### `/tmp`

Contains temporary files.

Programs store temporary data here.

This directory is usually cleared automatically after a reboot.

---

### `/usr`

Contains installed software, libraries, and shared resources.

Examples:

```text
/usr/bin
/usr/lib
/usr/share
```

Most applications are stored here.

---

## Important Directories Inside `/usr`

### `/usr/bin`

Contains commands and programs used by normal users.

Examples:

```text
python3
git
nano
vim
wget
curl
```

When you type:

```bash
ls
```

Linux searches for and executes the command from locations such as `/usr/bin` or `/bin`.

---

### `/usr/sbin`

Contains system administration tools.

Examples:

```text
useradd
groupadd
apache2
```

---

### `/usr/lib`

Contains libraries required for software to run.

These libraries can be thought of as helper files that programs depend on.

---

### `/usr/share`

Contains architecture-independent files such as:

* Documentation
* Icons
* Fonts
* Man pages

---

### `/usr/local`

Software installed manually rather than through the package manager is often installed here.

Example:

```text
/usr/local/bin
```

---

### Cybersecurity Perspective

Pentesters and Red Teamers often inspect `/usr` to determine:

* Which tools are installed
* Which software versions are running
* Whether vulnerable software versions exist

---

### `/dev`

Contains hardware device files.

Examples:

```text
/dev/sda      - Storage device
/dev/null     - A black hole for data. Anything sent here is discarded.
/dev/random   - Generates random numbers.
/dev/urandom  - Generates random data and is more commonly used in practice.
/dev/tty      - Represents the current terminal.
```

Example:

```bash
ls /dev
```

---

### `/proc`

`/proc` is a virtual filesystem.

This means:

* The files inside it are not permanently stored on disk.
* The kernel creates these files dynamically in memory while the system is running.
* These files provide live information about the system and running processes.

You can think of `/proc` as a live dashboard showing the current state of the system.

Examples:

```bash
cat /proc/cpuinfo
```

Memory information:

```bash
cat /proc/meminfo
```

---

### `/sys`

Contains information about hardware and the kernel.

It is commonly used for device and hardware management.

---

### `/boot`

Contains files required to boot Linux.

Examples include:

```text
Kernel
GRUB
initramfs
```

---

### `/mnt`

A temporary mount point used for manually mounted filesystems.

Example:

```bash
sudo mount /dev/sdb1 /mnt
```

---

### `/media`

Contains automatically mounted removable devices such as USB drives and DVDs.

Example:

```text
/media/junaid/USB
```

---

### `/opt`

Contains optional third-party software.

Examples:

```text
Google Chrome
VMware
Burp Suite
```


### File System Hierarchy Example

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
│   └── junaid
│       ├── Documents
│       ├── Downloads
│       └── Desktop
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```
## 4. Types of Files

When you run:

```
ls -l
```

You may see:

```
-rw-r--r-
-drwxr-xr
-xlrwxrwxrwx
```

The first character indicates the file type:

|Symbol|Meaning|
|---|---|
|`-`|Regular file|
|`d`|Directory|
|`l`|Symbolic link|
|`c`|Character device|
|`b`|Block device|
|`p`|Named pipe|
|`s`|Socket|

---

## Absolute vs Relative Paths

### Absolute Path

Starts from `/`.

Example:

```text
/home/junaid/Documents/file.txt
```

Always points to the same location.

---

### Relative Path

Starts from your current directory.

Example:

```text
Documents/file.txt
```

### Why This Matters for Cybersecurity
- **Finding configuration files:** `/etc/ssh/`, `/etc/passwd`, `/etc/shadow`
- **Analyzing logs:** `/var/log/`
- **Looking for web applications:** `/var/www/` (on many systems)
- **Investigating running processes:** `/proc/`
- **Examining mounted drives:** `/mnt/`, `/media/`
- **Checking executables:** `/bin`, `/usr/bin`

