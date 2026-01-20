
---

## Basic Navigation

|Command|Description|Example|Use-case|
|---|---|---|---|
|`pwd`|Print working directory|`pwd` → `/home/ashar`|Check current location in filesystem|
|`ls`|List files and directories|`ls`|See what’s in the current folder|
|`ls -l`|Long listing (permissions, owner, size, date)|`ls -l`|Check file permissions and owner|
|`ls -a`|Show hidden files|`ls -a`|Find config files (`.bashrc`, `.gitconfig`)|
|`cd <dir>`|Change directory|`cd Documents`|Move into a folder|
|`cd ..`|Move up one directory|`cd ..`|Navigate back|
|`cd ~`|Go to home|`cd ~`|Quick return home|

**Tips:**

- Use `ls -lh` for human-readable sizes.
    
- `cd -` goes to the previous directory.
    

---

## File & Directory Management

|Command|Description|Example|Notes|
|---|---|---|---|
|`touch file`|Create empty file|`touch notes.txt`|Also updates timestamp|
|`mkdir dir`|Create directory|`mkdir projects`|Single folder creation|
|`mkdir -p a/b/c`|Create nested directories|`mkdir -p devops/scripts`|Avoid multiple commands|
|`rm file`|Delete a file|`rm old.txt`|Irreversible!|
|`rm -r dir`|Delete directory recursively|`rm -r old_project`|Deletes directory & all files inside|
|`rm -rf dir`|Force delete|`rm -rf temp`|Dangerous, no confirmation|
|`cp file1 file2`|Copy file|`cp notes.txt backup.txt`|Backup or duplicate file|
|`cp -r dir1 dir2`|Copy directory|`cp -r project project_backup`|Recursively copies folder|
|`mv old new`|Move or rename|`mv report.txt archive/`|Organize or rename files|
|`stat file`|Show file metadata|`stat notes.txt`|Check timestamps, inode, permissions|

**Tip:** Always double-check paths before using `rm -rf`.

---

## Viewing File Content

|Command|Description|Example|Use-case|
|---|---|---|---|
|`cat file`|Display file content|`cat file.txt`|Quick look at small files|
|`less file`|Scrollable viewer|`less large.log`|Efficient for big logs|
|`more file`|Basic paging|`more file.txt`|Simple reading|
|`head file`|Show first 10 lines|`head notes.txt`|Check headers|
|`tail file`|Show last 10 lines|`tail logs.log`|Monitor latest entries|
|`tail -f file`|Real-time log monitoring|`tail -f /var/log/syslog`|Debug live logs|

---

## File Permissions & Ownership

- **Permission symbols:**  
    `r` = read, `w` = write, `x` = execute  
    Order: Owner | Group | Others
    

|Command|Description|Example|Notes|
|---|---|---|---|
|`chmod 755 file`|Set permissions|`chmod 755 script.sh`|Owner full access, others read/execute|
|`chmod u+x file`|Add execute to owner|`chmod u+x deploy.sh`|Run script|
|`chmod o-r file`|Remove read from others|`chmod o-r secret.txt`|Security|
|`chown user file`|Change owner|`chown ashar file.txt`|Ownership control|
|`chown user:group file`|Change owner & group|`chown ashar:devops file.txt`|Team access|

**Tip:** Use `ls -l` to verify permissions.

---

## Users & Groups

|Command|Description|Example|Notes|
|---|---|---|---|
|`useradd username`|Create user|`useradd ash`|Manual, non-interactive|
|`adduser username`|Interactive creation|`adduser ash`|Preferred for beginners|
|`userdel username`|Delete user|`userdel ash`|Cleanup old users|
|`groupadd groupname`|Create group|`groupadd devops`|For permission management|
|`usermod -aG group user`|Add user to group|`usermod -aG devops ash`|Grant group access|
|`id user`|Show UID, GID|`id ash`|Debug permissions|

---

## Processes & System Monitoring

| Command       | Description                | Example        | Use-case           |
| ------------- | -------------------------- | -------------- | ------------------ |
| `top`         | Live process monitor       | `top`          | CPU & memory usage |
| `htop`        | Interactive process viewer | `htop`         | Easier than top    |
| `ps aux`      | List all processes         | `ps aux`       | grep nginx         |
| `kill PID`    | Stop process gracefully    | `kill 1234`    | Proper shutdown    |
| `kill -9 PID` | Force kill                 | `kill -9 1234` | When stuck         |
| `free -h`     | RAM usage                  | `free -h`      | Memory monitoring  |
| `df -h`       | Disk usage                 | `df -h`        | Check free space   |

---


## Editors

|Editor|Use-case|
|---|---|
|`nano`|Beginner-friendly, simple editing|
|`vi` / `vim`|Advanced, server-side editing|

---


# Linux Redirection, Search, Networking & Backup (DevOps Notes)

**Date:** 20/01/2025

---

## 1. Redirection & Pipes (Very Important)

Redirection and pipes are core Linux concepts used heavily in DevOps for logging, automation, and troubleshooting.

### Output Redirection (Overwrite)

Redirect command output to a file (overwrite existing content):

```bash
ls > out.txt
```

### Append Output

Append command output to an existing file:

```bash
ls >> out.txt
```

### Pipes (Connecting Commands)

A pipe (`|`) sends the output of one command as input to another.

```bash
ps aux | grep nginx | wc -l
```

**Explanation:**

- `ps aux` → list all running processes
    
- `grep nginx` → filter nginx-related processes
    
- `wc -l` → count the number of matching lines
    

---

## 2. Search Commands (find vs grep)

### `find` – File Search

Used to search for files and directories.

```bash
find . -name "*.conf"
find /etc -type f -name "*.conf"
```

### `grep` – Text Search

Used to search for text inside files.

```bash
grep "error" file.txt
grep -R "error" /var/log
```

### Difference Between find and grep

|Tool|Purpose|
|---|---|
|find|Find files|
|grep|Find text|

---

## 3. Networking Basics

Common networking commands used in Linux:

```bash
ip a
ping google.com
ss -tulnp
```

**Explanation:**

- `ip a` → show network interfaces and IP addresses
    
- `ping` → test connectivity
    
- `ss -tulnp` → view listening ports and services
    

---

## 4. SSH & Remote Access

### SSH Login

```bash
ssh user@ip_address
```

### Copy Files to Remote Server (SCP)

```bash
scp file.txt user@ip_address:/path
```

---

## 5. Real DevOps Example (Very Important)

### Backup Home Directory with Date

```bash
tar -czvf home_backup_$(date +%F).tar.gz /home/ashar
```

**Output Example:**

```text
home_backup_2026-01-20.tar.gz
```

✅ This is a **professional backup naming and creation style** used in real DevOps environments.

---

## 6. Copy Backup to Another Location

### Copy to USB / External Drive

```bash
cp backup.tar.gz /media/usb/
```

### Copy to Remote Backup Server

```bash
scp backup.tar.gz user@192.168.1.10:/backup/
```

---

## 7. Rsync (Smart Backup Tool)

### What Rsync Does

- Copies **only changed files**
    
- Very fast and efficient
    
- Widely used for backups and synchronization
    

### Rsync Example

```bash
rsync -av /home/ashar/ /backup/ashar/
```

### Rsync Options

|Option|Meaning|
|---|---|
|a|Archive mode|
|v|Verbose output|

---

## 8. Important Differences (Must Remember)

|Tool|Purpose|
|---|---|
|tar|Archive + compression|
|zip|Compression only|
|rsync|Smart backup & synchronization|

---
