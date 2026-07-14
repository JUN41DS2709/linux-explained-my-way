# Understanding Linux File Permissions

File permissions are crucial in Unix-like operating systems, including Linux, which use several mechanisms to control access to files and directories. These mechanisms include standard permissions, special permissions, and Access Control Lists (ACLs).

---

## Standard Permissions

The primary set of file permissions, commonly known as **standard permissions**, governs the fundamental access levels for files and directories.

These permissions fall into three categories:

* Read (`r`)
* Write (`w`)
* Execute (`x`)

Each permission provides specific capabilities for files and directories:

| Permission    | Effect on Files                                    | Effect on Directories                                        |
| ------------- | -------------------------------------------------- | ------------------------------------------------------------ |
| Read (`r`)    | Allows viewing and reading the contents of files.  | Allows listing the contents of a directory.                  |
| Write (`w`)   | Permits modification or deletion of file contents. | Allows adding, deleting, or renaming files in the directory. |
| Execute (`x`) | Allows running the file as a program or script.    | Allows entering the directory and accessing files within it. |

---

## User Classes

These permissions can be assigned to different classes of users:

* **User (`u`)** → The owner of the file.
* **Group (`g`)** → Users who belong to the file's group.
* **Others (`o`)** → All other users on the system.

The permissions are arranged as follows:

```text
Owner     Group      Others
r w x     r w x      r w x
| | |     | | |      | | |
| | |     | | |      | | +--- Execute
| | |     | | |      | +----- Write
| | |     | | |      +------- Read
| | |     | | |
| | |     | | +--- Execute
| | |     | +----- Write
| | |     +------- Read
| | |
| | +--------- Execute
| +----------- Write
+------------- Read
```

---

## Symbolic File Permissions

Symbolic permissions use specific symbols to represent permissions and who they apply to.

| Symbol | Meaning                                       |
| ------ | --------------------------------------------- |
| `u`    | User (file owner)                             |
| `g`    | Group (members of the file's owning group)    |
| `o`    | Others (users not in the file's owning group) |
| `a`    | All users (`ugo`)                             |
| `r`    | Read                                          |
| `w`    | Write                                         |
| `x`    | Execute                                       |
| `+`    | Add the specified permission                  |
| `-`    | Remove the specified permission               |
| `=`    | Assign the specified permission exactly       |

---

## Numeric Representation

Linux permissions can also be represented numerically:

| Permission    | Value |
| ------------- | ----- |
| Read (`r`)    | `4`   |
| Write (`w`)   | `2`   |
| Execute (`x`) | `1`   |

Examples:

| Permission Combination | Value |
| ---------------------- | ----- |
| `rwx`                  | `7`   |
| `rw-`                  | `6`   |
| `r-x`                  | `5`   |
| `r--`                  | `4`   |

For example:

```bash
chmod 755 script.sh
```

This means:

* Owner → `7` → `rwx`
* Group → `5` → `r-x`
* Others → `5` → `r-x`

---

# Special Permissions: SetUID, SetGID, and Sticky Bit

In addition to standard permissions, Linux provides special permissions known as **SetUID**, **SetGID**, and the **Sticky Bit**.

These permissions provide additional control over files and directories and can improve both functionality and security.

```text
Owner   Group   Others
rws     rws     rwt
|||     |||     |||
|||     |||     ||+---- Sticky Bit
|||     |||     |+----- Execute
|||     |||     +------ Read
|||     |||
|||     ||+------ Set Group ID (SetGID)
|||     |+------- Write
|||     +-------- Read
|||
||+-------- Set User ID (SetUID)
|+--------- Write
+---------- Read
```

---

# SetUID (Set User ID)

## Definition

> **SetUID (Set User ID) is a special Linux file permission that allows an executable program to run with the permissions of the file owner instead of the permissions of the user who executes it.**

### In simple words:

> **When a user runs a SetUID-enabled program, the program temporarily gets the privileges of the file owner while it is running.**

---

## Example

Suppose there is an executable file:

```text
-rwsr-xr-x 1 junaid users myprogram
```

* File owner = `junaid`
* SetUID = Enabled

Now another user named `alice` runs the program.

Normally:

```text
alice
  ↓
Runs myprogram
  ↓
Program runs as alice
```

With SetUID:

```text
alice
  ↓
Runs myprogram
  ↓
Program temporarily runs with junaid's privileges
  ↓
Program exits
  ↓
Privileges return to alice
```

Even though `alice` executed the program, it ran with `junaid`'s permissions because `junaid` owns the file.

---

## How to set the SetUID bit

```bash
chmod u+s /path/to/program
```

This adds the `s` to the owner's execute field.

---

# SetGID (Set Group ID)

## Definition

> **SetGID (Set Group ID) is a special Linux permission that allows an executable program to run with the group permissions of the file instead of the group of the user who executes it.**

When applied to a directory, it ensures that newly created files and subdirectories inherit the directory's group ownership.

---

## Example 1: SetGID on an Executable

Suppose there is an executable file:

```text
-rwxr-sr-x 1 junaid developers myprogram
```

* File owner = `junaid`
* File group = `developers`
* SetGID = Enabled

Now a user named `alice`, whose primary group is `users`, runs the program.

Normally:

```text
User  = alice
Group = users
```

With SetGID:

```text
User  = alice
Group = developers (while the program runs)
```

Even though `alice` executed the program, it runs with the `developers` group because the executable has the SetGID permission enabled.

---

## How to set the SetGID bit

```bash
chmod g+s /path/to/file_or_directory
```

---

# Sticky Bit

## Definition

> **The Sticky Bit is a special Linux directory permission that allows users to create files in a shared directory but prevents them from deleting or renaming files owned by other users.**

Only the file owner, the directory owner, or the root user can delete or rename those files.

---

## Example

Suppose there is a shared directory with the following permissions:

```text
drwxrwxrwt shared
```

Two users, `Alice` and `Bob`, have access to this directory.

Alice creates a file:

```text
notes.txt
```

Bob tries to delete Alice's file:

```bash
rm notes.txt
```

Result:

```text
rm: cannot remove 'notes.txt': Operation not permitted
```

Because the Sticky Bit is enabled, Bob cannot delete Alice's file.

Only:

* The file owner
* The directory owner
* The root user

can delete or rename the file.

---

## How to set the Sticky Bit

```bash
chmod +t /path/to/directory
```
