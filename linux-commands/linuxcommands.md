# 🐧 Linux Terminal Commands

> A reference guide covering essential Linux terminal commands — from basic navigation to networking and system monitoring.

---

## 📁 File & Directory Navigation

| Command | Description |
|--------|-------------|
| `pwd` | **P**rint **W**orking **D**irectory — shows your current location in the filesystem |
| `ls` | List all files and folders in the current directory |
| `ls -a` | List **all** files including hidden ones (files starting with `.`) |
| `ls -l` | Long listing format — shows permissions, owner, size, and date |
| `ls -R` | Recursively list files in the current directory and all subdirectories |

> 💡 You can combine flags: `ls -la` lists all files (including hidden) with details.

---

## 📄 File Operations

| Command | Description |
|--------|-------------|
| `touch <filename>` | Create a new empty file |
| `cat <filename>` | Display the full contents of a file |
| `cp <filename> <path>` | Copy a file to the given path |
| `mv <filename> <path>` | Move (or rename) a file to the given path |
| `rm <filename>` | Remove/delete a file |
| `cp -R <foldername> <path>` | Copy an entire folder (recursive) to a path |
| `rmdir <foldername>` | Remove an empty directory |
| `mkdir <foldername>` | Create a new folder |

> ⚠️ `rm` is permanent — there's no trash bin in the terminal. Be careful!

---

## 🔗 Pipes & Redirection

Pipes (`|`) and redirects (`>`) are some of the most powerful features in the Linux terminal.

### Example: Convert text to uppercase

```bash
cat file.txt | tr a-z A-Z > upper.txt
```

**Step-by-step breakdown:**

1. `cat file.txt` — reads the contents of the file
2. `|` — **pipe**: passes the output as input to the next command (via a buffer)
3. `tr a-z A-Z` — **translate**: converts all lowercase letters to uppercase inside the buffer
4. `> upper.txt` — **redirect**: writes the final output into `upper.txt` (creates it if it doesn't exist)

> 💡 `>` overwrites the file. Use `>>` to **append** to an existing file instead.

---

## 📊 Disk & Storage

| Command | Description |
|--------|-------------|
| `df` | Show disk usage of all mounted file systems |
| `du <foldername>` | Show space used by each file inside a folder |
| `du -sh <foldername>` | Show **summary** total space used (`-s`) in **human-readable** format (`-h`) e.g. `4.2G` |

---

## 👀 Viewing File Contents

| Command | Description |
|--------|-------------|
| `head <filename>` | Show the first **10 lines** of a file (default) |
| `head -n <N> <filename>` | Show the first N lines of a file |
| `tail <filename>` | Show the last **10 lines** of a file (default) |
| `tail -n <N> <filename>` | Show the last N lines of a file |
| `tail -f <filename>` | **Follow** — show real-time updates as the file grows (great for log files!) |

> 💡 `tail -f` is extremely useful for monitoring live log files on a server.

---

## 🔍 Finding Files

| Command | Description |
|--------|-------------|
| `locate <filename>` | Quickly find a file using a pre-built database (may not reflect very recent changes) |
| `find . -name "<filename>"` | Search for a file by name in the current directory (`.`) |
| `find . -iname "<filename>"` | Same as above but **case-insensitive** |
| `find . -type f` | Find only **files** |
| `find . -type d` | Find only **directories** |
| `find . -size +10M` | Find files **larger than** 10 MB |

> 💡 `find` is real-time and always accurate; `locate` is faster but relies on a database updated by `updatedb`.

---

## 🔎 Searching Inside Files — `grep`

`grep` searches for a string pattern inside files.

| Command | Description |
|--------|-------------|
| `grep "<string>" <filename>` | Search for a string in a file |
| `grep -w "<string>" <filename>` | Match **whole word** only |
| `grep -i "<string>" <filename>` | Case-**insensitive** search |
| `grep -n "<string>" <filename>` | Show matching lines with **line numbers** |
| `grep -r "<string>" .` | **Recursively** search in all files in the directory |
| `grep -c "<string>" <filename>` | Show **count** of matching lines |
| `grep -l "hello" *.txt` | List only the **filenames** that contain the match |

---

## ↕️ Sorting & Comparing

| Command | Description |
|--------|-------------|
| `sort <filename>` | Print the contents of a file in sorted (alphabetical) order |
| `diff <file1> <file2>` | Compare two files line by line |
| `diff -u <file1> <file2>` | Unified format — cleaner, shows context lines |
| `diff -y <file1> <file2>` | Side-by-side comparison |

---

## 🔐 Permissions & Ownership

Every file in Linux has read (`r`), write (`w`), and execute (`x`) permissions.

| Command | Description |
|--------|-------------|
| `chmod +x <filename>` | Add **execute** permission to a file (make it runnable) |
| `sudo chown user <filename>` | Change the **owner** of a file to `user` |

> 💡 `sudo` = **superuser do** — runs a command with admin/root privileges.

---

## 📦 Compression

| Command | Description |
|--------|-------------|
| `zip <output.zip> <files>` | Compress files into a zip archive |
| `unzip <file.zip>` | Extract a zip archive |

---

## 📡 Networking

| Command | Description |
|--------|-------------|
| `ping <url>` | Continuously send packets to a URL and report response time (press `Ctrl+C` to stop) |
| `hostname` | Show the machine's hostname |
| `hostname -I` | Show the machine's IP address |
| `hostname -f` | Show the fully qualified domain name (FQDN) |
| `nslookup <domain>` | Look up the IP address of a domain |
| `netstat` | Show all active network connections and ports |

---

## 🖥️ System Information & Monitoring

| Command | Description |
|--------|-------------|
| `uname` | Show the kernel name |
| `lscpu` | Detailed CPU information (cores, architecture, speed, etc.) |
| `free` | Show used and available RAM |
| `vmstat` | Virtual memory statistics |
| `top` | Real-time view of running processes and resource usage |
| `htop` | Enhanced, colorful, interactive version of `top` |
| `btop` | Beautiful, modern resource monitor with graphs 🔥 |

> 💡 `top`, `htop`, and `btop` all show live CPU/memory usage. Install `htop` or `btop` if not already available — they're much more readable than `top`.

---

## 🧰 Miscellaneous Utilities

| Command | Description |
|--------|-------------|
| `echo <text>` | Print a string or variable value to the terminal |
| `man <command>` | Open the **manual** (documentation) for any command. E.g. `man ls` |
| `history` | Show the list of recently used commands |
| `sudo apt update` | Update the package list (for Debian/Ubuntu-based systems) |

> 💡 `man` is your best friend. When in doubt: `man <command>` gives you the full official documentation.

---

## 🧠 Quick Reference Cheatsheet

```
Navigation      : pwd, ls, ls -a, ls -l, ls -R
Files           : touch, cat, cp, mv, rm, mkdir, rmdir
View Contents   : head, tail, tail -f
Find Files      : find, locate
Search in Files : grep, grep -r, grep -i, grep -n
Compare         : diff, diff -u, diff -y
Disk            : df, du, du -sh
Permissions     : chmod, chown
Network         : ping, hostname, nslookup, netstat
System Info     : uname, lscpu, free, vmstat
Monitoring      : top, htop, btop
Misc            : echo, man, history, zip, unzip, sort
```
