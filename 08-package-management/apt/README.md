# Package Management with APT

## APT (Advanced Package Tool)

APT is the package management system used in **Debian-based Linux distributions**, including:

* Ubuntu
* Debian
* Kali Linux
* Linux Mint
* Pop!_OS

APT works with `.deb` packages and uses `dpkg` (**Debian Package Manager**) underneath.

---

## Common APT Commands

| Command                      | Purpose                                              |
| ---------------------------- | ---------------------------------------------------- |
| `sudo apt update`            | Refresh the package list from repositories.          |
| `sudo apt upgrade`           | Upgrade installed packages.                          |
| `sudo apt full-upgrade`      | Upgrade packages and handle dependency changes.      |
| `sudo apt install nginx`     | Install a package.                                   |
| `sudo apt remove nginx`      | Remove a package but keep configuration files.       |
| `sudo apt purge nginx`       | Remove a package along with its configuration files. |
| `sudo apt autoremove`        | Remove unused dependencies.                          |
| `sudo apt search nginx`      | Search repositories for a package.                   |
| `apt show nginx`             | Display package details.                             |
| `apt list --installed`       | List all installed packages.                         |
| `apt list --upgradable`      | Show packages that can be upgraded.                  |
| `sudo apt reinstall nginx`   | Reinstall a package.                                 |
| `sudo apt download nginx`    | Download the `.deb` package without installing it.   |
| `sudo apt clean`             | Remove the downloaded package cache.                 |
| `sudo apt autoclean`         | Remove obsolete package cache files.                 |
| `sudo apt-mark hold nginx`   | Prevent a package from being upgraded.               |
| `sudo apt-mark unhold nginx` | Allow the package to be upgraded again.              |

---

## Checking if a Package is Installed

You can check whether a package is installed using:

```bash id="rpk8p5"
dpkg -l | grep nmap
```

This searches the list of installed packages for `nmap`.

---

## Finding Where a Tool is Installed

### Locate the executable in your PATH

```bash id="q1mmt7"
which nmap
```

Example output:

```text id="ozv0f7"
/usr/bin/nmap
```

---

### List all files installed by a package

```bash id="m5wjlwm"
dpkg -L nmap
```

This displays all files and directories installed by the `nmap` package.

---

## Finding Which Package Owns a File

If you know a file path but do not know which package installed it, use:

```bash id="4h3vzy"
dpkg -S /usr/bin/nmap
```

This searches the package database and displays the package that owns the file.
