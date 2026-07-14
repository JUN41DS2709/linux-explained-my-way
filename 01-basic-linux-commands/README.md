# Basic Linux Commands

Linux provides a large number of commands for interacting with the system. These are some of the basic and frequently used commands that every Linux user should know.

## User and System Information Commands

### `pwd`

Prints the current working directory.

### `whoami`

Displays the current username associated with the user.

### `w`

The `w` command gives us important information about:

* Who is currently using the computer.
* How much the computer is being used.
* What programs are currently running.

It is a handy tool for people who take care of computer systems, as it helps them keep an eye on what users are doing, how much of the computer's resources are being used, and how to make everything run smoothly.

### `who`

The `who` command is used to obtain information about the users currently logged into the system.

### `date`

The `date` command displays the current date and time.

---

## Terminal Management Commands

### `clear`

Used to clear the terminal screen.

### `exit`

Used to exit a process or to get back to a normal user if working as `root`.

### `logout`

Logs out the current user and prompts the terminal session to close after logging out.

---

## User Management Commands

### `passwd`

Used to change the user's password.

---

## Navigation Commands

### `cd`

Used to change directories.

### `ls`

Lists files and directories.

---

## File and Directory Management Commands

### `cp`

Stands for **copy** and is used for copying files and their contents.

### `mv`

Used to change a file name and also used for moving files across directories or to a specific location.

### `rm`

Removes or deletes a file from the system.

### `rmdir`

Used for removing a directory, but the directory must be empty.

### `mkdir`

Creates a directory.

### `rm -rf`

Used for the forceful removal of directories or files.

---

## Command Reference and History

### `history`

Displays the command-line history.

### `man`

`man` stands for **manual** and is used to view the manual pages for tools and commands.

For example, if you do not know about `nmap`, you can use:

```bash
man nmap
```

to view its usage manual.

---

## System Management Commands

### `shutdown`

Used for shutting down the system.

* `-c` is used for cancelling a scheduled shutdown.
* The shutdown can also be scheduled for a specific time.

---

## Disk Usage Commands

### `df -h`

Shows the disk usage of mounted filesystems in a human-readable format.

### `du -sh`

Shows the size of a file or directory in a human-readable format.
