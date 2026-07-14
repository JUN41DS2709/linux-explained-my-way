# Process Management in Linux

Processes are running instances of programs. Linux provides various commands to view, monitor, control, and terminate processes.

---

## Viewing Processes

### `ps`

The `ps` command displays information about active processes.

```bash
ps
```

Displays a basic list of processes associated with the current terminal session.

---

### `ps -ef`

Displays detailed information about all running processes in the system.

```bash
ps -ef
```

Common columns include:

* UID → User who owns the process
* PID → Process ID
* PPID → Parent Process ID
* CMD → Command that started the process

---

### `ps -e --format uid,pid,ppid,%cpu,cmd`

Displays selected information about running processes.

```bash
ps -e --format uid,pid,ppid,%cpu,cmd
```

This displays:

* User ID (`UID`)
* Process ID (`PID`)
* Parent Process ID (`PPID`)
* CPU Usage (`%CPU`)
* Command (`CMD`)

---

### `ps aux`

Displays detailed information about all running processes.

```bash
ps aux
```

This is one of the most commonly used variants of `ps`.

---

## Real-Time Process Monitoring

### `top`

Used for real-time process monitoring.

```bash
top
```

It displays:

* CPU usage
* Memory usage
* Running processes
* Load averages
* System uptime

---

## Starting Processes

### `nice`

The `nice` command starts a process with a specified priority.

```bash
nice command
```

Example:

```bash
nice python script.py
```

Processes with higher nice values receive lower scheduling priority.

---

## Terminating Processes

### `kill`

Used to terminate a process using its Process ID (PID).

```bash
kill 1234
```

---

### `pkill`

Used to terminate a process using its process name.

```bash
pkill firefox
```

---

## Finding Process IDs

### Using `grep` with `ps`

A common method of enumerating process IDs is:

```bash
ps -ef | grep firefox
```

This displays processes related to Firefox and can be used to identify its PID.

---

# Background and Foreground Processes

Linux allows processes to run either in the foreground or in the background.

## Starting a Process

```bash
nice <process>
```

---

## Sending a Process to the Background

Press:

```text
Ctrl + Z
```

This pauses the currently running process.

Then use:

```bash
bg
```

to resume it in the background.

---

## Bringing a Process to the Foreground

```bash
fg
```

This brings the background process back to the foreground.

---

## Working with Multiple Jobs

### Resume Job 1 in the Background

```bash
bg %1
```

---

### Bring Job 1 Back to the Foreground

```bash
fg %1
```

---

### View Background Jobs

```bash
jobs
```

---

## Quick Reference

| Need to...                                   | Command     |
| -------------------------------------------- | ----------- |
| Pause the current task and free the terminal | `Ctrl + Z`  |
| Let it continue running in the background    | `bg`        |
| View background jobs                         | `jobs`      |
| Bring the last job to the foreground         | `fg`        |
| Bring a specific job to the foreground       | `fg %3`     |
| Resume a specific job in the background      | `bg %2`     |
| Kill a background job                        | `kill %1`   |
| Kill a process using its PID                 | `kill 3241` |

---

# Process Concepts

## PID (Process ID)

Every process running in Linux has a unique Process ID (`PID`).

Example:

```bash
ps -ef
```

---

## PPID (Parent Process ID)

Every process is created by another process known as its parent process.

The Parent Process ID (`PPID`) identifies which process created the current process.

---

## Foreground Process

A foreground process occupies the terminal and interacts directly with the user.

Examples:

* `nano`
* `vim`
* `top`

---

## Background Process

A background process runs without occupying the terminal, allowing the user to continue executing other commands.

Example:

```bash
python server.py &
```

The `&` symbol starts the process directly in the background.
