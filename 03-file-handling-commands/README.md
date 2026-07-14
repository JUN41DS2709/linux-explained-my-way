# File Handling Commands

File handling commands are used to navigate directories, manage files, view file contents, change permissions, and work with archives and compressed files.

## Directory Navigation and Management

### `cd`

Used to change directories.

### `pwd`

Prints the current working directory.

### `mkdir`

Creates a directory.

### `rmdir`

Used for removing a directory, but the directory must be empty.

### `rm -rf`

Used for the forceful removal of directories or files.

---

## Listing Files and Directories

### `ls`

Lists files and directories.

### `ls -l`

Lists files and directories with more information such as:

* Permissions
* Owner
* Group
* File size
* Modification date and time

### `ls -a`

Also lists hidden files and directories.

---

## File Creation and Viewing

### `touch`

Creates an empty file.

### `cat`

Displays the contents of a file.

### `head`

Displays the first few lines of a file.

### `tail`

Displays the last few lines of a file.

### `more`

The `more` command shows the contents of a file, pausing after each screenful.

### `less`

The `less` command is a more advanced and flexible version of `more`, allowing both forward and backward navigation through the file.

---

## Searching Files and Directories

### `find`

Used to search for files and directories.

---

## File Permissions

### `chmod`

Used to change file permissions.

Permission values:

* **Read (`4`)** → Permission to read the contents of a file.
* **Write (`2`)** → Permission to modify or write contents to a file.
* **Execute (`1`)** → Permission to execute the file if it is executable.

For example:

```bash id="e6k4kq"
chmod 777 cat.txt
```

The first `7` is for the owner of the file, the second `7` is for the group, and the last `7` is for other users.

This means all types of users can now:

* Read
* Write
* Execute

Since:

```text
4 + 2 + 1 = 7
```

---

## File Ownership

### `chown`

Used to change file ownership.

Example:

```bash id="2f5vza"
chown JD cat.txt
```

---

## Archiving and Compression

### `tar`

Used to archive files.

Example:

```bash id="6nrf5v"
tar -cvf archive.tar file1.txt file2.txt
tar -xvf archive.tar
```

### `gzip` and `gunzip`

Used to compress and decompress files.

Example:

```bash id="h17h2u"
gzip file1.txt
gunzip file1.txt.gz
```

---

## Monitoring Files

### `tail -f`

Used to monitor changes to a file in real time.

This is commonly used for monitoring log files.

---

## Identifying File Types

### `file`

The `file` command in Linux is a powerful tool that helps identify and classify file types by analyzing their contents and structure rather than relying only on file extensions.
