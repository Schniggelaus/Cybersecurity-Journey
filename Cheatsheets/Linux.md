# Linux Commands Cheatsheet

---

## Navigation & Filesystem

| Command | Description | Example |
|---------|-------------|---------|
| `pwd` | Print current working directory path | `pwd` |
| `ls` | List contents of a directory | `ls`, `ls -la` |
| `cd` | Change directory | `cd /etc`, `cd ..` |

---

## File Operations

| Command | Description | Example |
|---------|-------------|---------|
| `cat` | Display file content (or concatenate multiple files) | `cat file.txt`, `cat file1.txt file2.txt` |
| `find` | Search for files by name or type | see below |
| `grep` | Search for a string inside file(s) | see below |
| `touch` | Create a file | `touch note` |
| `mkdir` | Create a folder (make directory) | `mkdir mydirectory` |
| `cp` | Copy a file or folder | `cp note note2` |
| `mv` | Move (or rename) a file or folder | `mv note2 note3` |
| `rm` | Remove a file or folder | `rm mydirectory` |
| `file` | Determine the type of a file | `file note` |

### `find` – Examples
```bash
find -name file.txt          # Search in current directory by name
find / -name file.txt        # Search from root directory
find . -name file.txt        # Search from current directory
find -name *.txt             # Wildcard: find all .txt files
```

### `grep` – Examples
```bash
grep "searchterm" file.txt         # Search for string in a file
grep -R "searchterm" /etc/         # Recursively search all files in a directory
```

---

## Text Editors

| Command | Description | Example |
|---------|-------------|---------|
| `nano` | Create/edit a file with the Nano text editor | `nano testfile` |

---

## System Info

| Command | Description | Example |
|---------|-------------|---------|
| `echo` | Output text to the terminal | `echo "Hello World"` |
| `whoami` | Show currently logged-in user | `whoami` |
| `man` | Show manual/documentation for a command | `man ls` |

---

## Permissions

Permissions follow the symbolic format `rwxrwxrwx`:
- First 3 chars → **Owner**
- Next 3 chars → **Group**
- Last 3 chars → **Others**

| Letter | Permission | Numeric Value |
|--------|-----------|----------------|
| `r` | Read | 4 |
| `w` | Write | 2 |
| `x` | Execute | 1 |

### Common Examples
| Symbolic | Numeric | Meaning |
|----------|---------|---------|
| `rwxr-xr-x` | 755 | Owner can do anything, others can read and execute |
| `rw-r--r--` | 644 | Owner can read and write, others can only read |
| `rwx------` | 700 | Only the owner has access |

---

## Process Management

| Command | Description | Example |
|---------|-------------|---------|
| `ps` | List running processes with info (PID, status, CPU usage) | `ps` |
| `ps aux` | List processes from all users, including those not attached to a session | `ps aux` |
| `top` | Real-time view of running processes | `top` |
| `kill` | Send a signal to a process (default: terminate) | `kill 1010` |

### Kill Signals
| Term | Description |
|------|-------------|
| `SIGTERM` | Kill the process, but allow it to clean up |
| `SIGKILL` | Kill the process immediately – no cleanup |
| `SIGSTOP` | Stop/suspend a process (can be resumed with `SIGCONT`) |

---

## System Services

| Command | Description | Example |
|---------|-------------|---------|
| `systemctl` | Manage services/processes (start, stop, enable, disable, status) | `systemctl start apache2`, `systemctl enable apache2` |

---

## Automation – Cron

`cron` schedules tasks; `crontab` is used to interact with cron jobs.

| Value | Description |
|-------|-------------|
| MIN | Minute to execute at |
| HOUR | Hour to execute at |
| DOM | Day of the month to execute at |
| MON | Month of the year to execute at |
| DOW | Day of the week to execute at |
| CMD | Command being executed |

**Example:**
```bash
0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/
```
Backs up the Documents folder every 12 hours.

---

## File Transfer & Networking

| Command | Description | Example |
|---------|-------------|---------|
| `wget` | Download a file from a given URL | `wget https://example.com/file.zip` |
| `scp` | Securely copy files between hosts via SSH | `scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt` |
| `python3 -m http.server` | Serve current directory as a simple web server | `python3 -m http.server` |

---

## Secure Shell (SSH)

| Command | Description | Example |
|---------|-------------|---------|
| `ssh` | Protocol for encrypted remote access to another device | `ssh username@10.10.10.10` |

---

## Reconnaissance & Remote Access
 
| Command | Description | Example |
|---------|-------------|---------|
| `ping` | Check if a host is reachable on the network | `ping 10.10.10.10` |
| `nmap` | Scan a host for open ports and running services | `nmap 10.10.10.10` |
| `telnet` | Connect to a remote host/service over an unencrypted connection | `telnet 10.10.10.10 23` |
 
### `nmap` – Common Usage
```bash
nmap 10.10.10.10              # Basic scan of most common ports
nmap -p- 10.10.10.10           # Scan all 65535 ports
nmap -sV 10.10.10.10            # Detect service versions on open ports
```

---

## Shell Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `&` | Run command in the background | `command &` |
| `&&` | Chain commands – second runs only if first succeeds | `command1 && command2` |
| `>` | Redirect output to a file (overwrites) | `echo "hello" > file.txt` |
| `>>` | Redirect output to a file (appends) | `echo "world" >> file.txt` |

---

## Logs

Located in `/var/log/`. Relevant services include `apache2`, `fail2ban` (monitors brute-force attempts), and `UFW` (firewall).

Common log files:
- `access.log` (e.g. `/var/log/apache2/access.log`)
- `error.log` (e.g. `/var/log/apache2/error.log`)

---
