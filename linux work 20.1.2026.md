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

