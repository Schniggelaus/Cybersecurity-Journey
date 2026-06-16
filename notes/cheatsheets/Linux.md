# Linux Commands Cheatsheet

---

## Navigation & Filesystem

| Command | Description | Example |
|--------|-------------|---------|
| `pwd` | Print current working directory path | `pwd` |
| `ls` | List contents of a directory | `ls`, `ls -la` |
| `cd` | Change directory | `cd /etc`, `cd ..` |

---

## File Operations

| Command | Description | Example |
|--------|-------------|---------|
| `cat` | Display file content (or concatenate multiple files) | `cat file.txt`, `cat file1.txt file2.txt` |
| `find` | Search for files by name or type | see below |
| `grep` | Search for a string inside file(s) | see below |

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

## System Info

| Command | Description | Example |
|--------|-------------|---------|
| `echo` | Output text to the terminal | `echo "Hello World"` |
| `whoami` | Show currently logged-in user | `whoami` |

---

## Shell Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `&` | Run command in the background | `command &` |
| `&&` | Chain commands – second runs only if first succeeds | `command1 && command2` |
| `>` | Redirect output to a file (overwrites) | `echo "hello" > file.txt` |
| `>>` | Redirect output to a file (appends) | `echo "world" >> file.txt` |

---

## Secure Shell Commands

| Command | Description | Example
|---------|-------------|--------|
|`ssh` | Protocol to gain access on another device remotly | `ssh username@10.10.10.10`|
