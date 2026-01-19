
---

## 1. Basic Navigation Commands

### `pwd`

**What:** Shows the current working directory  
**Why:** To know exactly where you are in the filesystem

### `ls`

**What:** Lists files and directories  
**Why:** To see what exists in the current folder

### `ls -l`

**What:** Long listing (permissions, owner, size, date)  
**Why:** To check file details and permissions

### `ls -a`

**What:** Shows hidden files (starting with `.`)  
**Why:** Config files are often hidden

### `cd <dir>`

**What:** Changes directory  
**Why:** To move into another folder

### `cd ..`

**What:** Moves one directory up  
**Why:** To go back in directory structure

### `cd ~`

**What:** Goes to home directory  
**Why:** Quick way to return home

---

## 2. File & Directory Management

### `touch file`

**What:** Creates an empty file  
**Why:** Used to create files or update timestamps

### `mkdir dir`

**What:** Creates a directory  
**Why:** To organize files

### `mkdir -p a/b/c`

**What:** Creates nested directories  
**Why:** Saves time when creating deep folder structures

### `rm file`

**What:** Deletes a file  
**Why:** To remove unwanted files

### `rm -r dir`

**What:** Deletes directory recursively  
**Why:** To remove folders with files inside

### `rm -rf dir`

**What:** Force delete without confirmation  
**Why:** Used carefully when cleanup is required

### `cp file1 file2`

**What:** Copies file  
**Why:** To create backups or duplicates

### `cp -r dir1 dir2`

**What:** Copies directory  
**Why:** To duplicate entire folders

### `mv old new`

**What:** Moves or renames files  
**Why:** File organization or renaming

### `stat file`

**What:** Shows detailed file metadata  
**Why:** For inode, permissions, timestamps info

---

## 3. Viewing File Content

### `cat file`

**What:** Displays file content  
**Why:** Quick view of small files

### `less file`

**What:** Scrollable file viewer  
**Why:** Best for large files

### `more file`

**What:** Basic paging view  
**Why:** Simple viewing without editing

### `head file`

**What:** Shows first 10 lines  
**Why:** Useful for checking headers

### `tail file`

**What:** Shows last 10 lines  
**Why:** Logs usually grow at the end

### `tail -f file`

**What:** Live log monitoring  
**Why:** Real-time debugging (very common in DevOps)

---

## 4. File Permissions & Ownership

### Permission Meaning

`r` = read, `w` = write, `x` = execute  
Order: **Owner | Group | Others**

### `chmod 755 file`

**What:** Sets permission  
**Why:** Common for scripts and applications

### `chmod u+x file`

**What:** Adds execute permission to owner  
**Why:** To run scripts

### `chmod o-r file`

**What:** Removes read from others  
**Why:** For security

### `chown user file`

**What:** Changes file owner  
**Why:** Ownership control

### `chown user:group file`

**What:** Changes owner and group  
**Why:** Team-based access

---

## 5. Users & Groups

### `useradd username`

**What:** Creates a user  
**Why:** Manual user creation

### `adduser username`

**What:** Interactive user creation  
**Why:** Easier and safer

### `userdel username`

**What:** Deletes user  
**Why:** Cleanup old users

### `groupadd groupname`

**What:** Creates group  
**Why:** Permission management

### `usermod -aG group user`

**What:** Adds user to group  
**Why:** Grant group access

### `id user`

**What:** Shows UID, GID  
**Why:** Debug permission issues

---

## 6. Process & System Monitoring

### `top`

**What:** Live system processes  
**Why:** CPU & memory monitoring

### `htop`

**What:** Improved top  
**Why:** Easier process management

### `ps aux`

**What:** Lists all processes  
**Why:** Find running services

### `kill PID`

**What:** Stops process gracefully  
**Why:** Proper shutdown

### `kill -9 PID`

**What:** Force kill  
**Why:** When process is stuck

### `free -h`

**What:** Memory usage  
**Why:** RAM monitoring

### `df -h`

**What:** Disk usage  
**Why:** Check disk space

---

## 7. Networking Commands

### `ip a`

**What:** Shows IP info  
**Why:** Network troubleshooting

### `ping google.com`

**What:** Checks connectivity  
**Why:** Internet/network test

### `ss -tulnp`

**What:** Shows open ports  
**Why:** Service verification

### `scp`

**What:** Secure file transfer  
**Why:** Copy files between servers

### `rsync -av`

**What:** Sync files efficiently  
**Why:** Backup & deployment

---

## 8. Archive & Compression

### `tar`

**What:** Archive files  
**Why:** Backup & transfer

### `zip / unzip`

**What:** Compress files  
**Why:** Reduce size

---

## 9. Editors

### `nano`

**Why:** Beginner-friendly editor

### `vi`

**Why:** Powerful editor used in servers

---

## 10. Redirection & Pipes

### `>`

**Why:** Save output to file

### `>>`

**Why:** Append output

### `|`

**Why:** Chain commands together

---

## 11. Search & Find

### `find`

**Why:** Search files anywhere

### `grep`

**Why:** Search text inside files

---

## 12. Links

### `ln`

**Why:** Create file references

---

## 13. Mounting

### `/etc/fstab`

**Why:** Permanent disk mounts

---

## 14. Package Management

### `apt install`

**Why:** Install software
