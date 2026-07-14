# User Management in Linux

Linux uses users and groups to control access to files, processes, and system resources. Understanding user management is essential for system administration, security, and penetration testing.

---

## `/etc/passwd` File

The `/etc/passwd` file contains basic information about each user on the system, including:

* **Username** → The name that the user uses for logging in.
* **User ID (UID)** → A unique numeric identifier associated with the user.
* **Group ID (GID)** → A unique numeric identifier for the user's primary group.
* **Home Directory** → The directory where the user is taken after logging in. This is usually where personal files and configurations are stored.
* **Default Shell** → The default program launched when a user logs in, typically a command-line shell such as `bash` or `sh`.

Example:

```bash
cat /etc/passwd
```

---

## `/etc/shadow` File

The `/etc/shadow` file is more security-sensitive because it contains password hashes and other important information related to user authentication, such as:

* **Password Hash** → The user's password in hashed form.
* **Password Expiration** → Information about when the password was last changed and when it will expire.
* **Password Aging Policies** → Information used to enforce password policies.

Because of the sensitive nature of its contents, only the `root` user or users with appropriate privileges can access this file.

Example:

```bash
sudo cat /etc/shadow
```

---

# The Superuser (`root`)

The root user's primary purpose is to perform system administration tasks such as:

* Installing software system-wide.
* Modifying system configurations.
* Managing users and groups.
* Accessing restricted files and directories.

A few important points:

* **Login Name:** Usually `root`.
* **UID:** `0`
* **Privileges:** Unrestricted access to system resources.

### Example

```bash
sudo su
```

or

```bash
su -
```

---

## Important Warning

The immense privileges of the root account require caution.

For example:

```bash
rm -rf /*
```

when executed as root, can delete critical system files and render the system unusable.

---

# Using `sudo`

Instead of logging in directly as root, many Linux users use `sudo`, which allows permitted users to execute commands with superuser privileges.

Example:

```bash
sudo apt update
```

---

## Adding a User to the `sudo` Group

```bash
usermod -aG sudo JD
```

This adds the user `JD` to the `sudo` group.

---

# Switching Between Users

## Using `su`

The `su` command allows switching between users.

```bash
su <user>
```

Example:

```bash
su adam
```

---

## Login Shell with `su -`

By using `-` or `-l` with `su`, you switch to another user while also loading that user's environment variables and shell configuration files.

```bash
su - <user>
```

Example:

```bash
su - adam
```

This provides a login shell and behaves similarly to a full login session for that user.

---

## Passwordless Switching

If you are the root user or have sufficient `sudo` privileges, you can switch to another user without knowing their password.

However, such operations should be performed carefully to maintain system security.

---

# Creating Users

## Add a New User

```bash
useradd adam
```

---

## Common `useradd` Options

### `-m`

Creates a home directory for the user.

Example:

```bash
useradd -m adam
```

---

### `-u`

Specifies a custom User ID (UID).

Example:

```bash
useradd -u 2000 adam
```

---

### `-G`

Adds the user to one or more supplementary groups.

Example:

```bash
useradd -G admins,sudo adam
```

---

## Setting a Password

The `passwd` command allows you to set or change a user's password.

Example:

```bash
passwd adam
```

---

## Creating a User with an Encrypted Password

```bash
useradd -m -p encrypted_password adam
```

---

# Group Management

Groups are used to manage permissions for multiple users collectively.

---

## Viewing Existing Groups

To view all groups present on the system:

```bash
cat /etc/group
```

This displays groups and the users associated with them.

---

## Adding a New Group

```bash
groupadd admins
```

---

## Adding a User to a Group

To associate a user with a group:

```bash
usermod -aG admins adam
```

The `-aG` options mean:

* `-a` → Append the user to additional groups.
* `-G` → Specify supplementary groups.

---

## Removing a User from a Group

```bash
gpasswd -d adam admins
```

This removes `adam` from the `admins` group.

---

# File Ownership

Permissions are closely related to ownership, so understanding ownership management is important.

---

## Changing File Owner

```bash
chown adam file.txt
```

Here, `adam` becomes the new owner of `file.txt`.

---

## Changing Group Ownership

```bash
chgrp admins file.txt
```

Now `file.txt` belongs to the `admins` group.

---

## Changing Both Owner and Group

```bash
chown adam:admins file.txt
```

This changes:

* Owner → `adam`
* Group → `admins`

---

# Understanding UID and GID

In Unix-like systems, each user and group is identified by a unique numerical identifier:

* **UID (User ID)** → Identifies users.
* **GID (Group ID)** → Identifies groups.

These identifiers are fundamental to Linux permissions and ownership.

Examples:

* `chown` uses UIDs and GIDs to determine ownership.
* `chmod` uses ownership information to apply permissions to owners, groups, and others.

---

## The Root User

The root user has:

```text
UID = 0
GID = 0
```

Because of these privileges, root can access and modify nearly every resource on the system.

---

# Retrieving UID and GID

## For a User

The `id` command displays a user's UID, GID, and group memberships.

Example:

```bash
id adam
```

Typical output:

```text
uid=1000(adam) gid=1000(adam) groups=1000(adam),4(adm),24(cdrom),27(sudo),46(plugdev),113(lpadmin),128(sambashare)
```

This displays:

* User UID
* Primary group GID
* Secondary group memberships

---

## For a Group

The `getent` command can be used to view information about a group.

Example:

```bash
getent group admins
```

Typical output:

```text
admins:x:1001:adam
```

The third field represents the GID, while the last field lists group members.

---

# Modifying UID and GID

## Change a User's UID

The `usermod` command combined with `-u` changes a user's UID.

```bash
usermod -u 1001 adam
```

---

## Change a Group's GID

The `groupmod` command combined with `-g` changes a group's GID.

```bash
groupmod -g 1001 admins
```
