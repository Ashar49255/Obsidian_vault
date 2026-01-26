# Linux Commands – Concise Practical Guide
## 1. dig – DNS Lookup
# Basic usage
dig google.com A
dig gmail.com MX
dig example.com NS
dig +short google.com
dig @8.8.8.8 google.com
dig -x 8.8.8.8

# Tips:
# A = IP address
# MX = Mail server
# NS = Nameserver
# +short = clean output

---
## 2. find – Search Files
# Search by name
find /home/ashar -name "hello.py"
# Case-insensitive
find /home/ashar -iname "hello.py"
# File type
find /var/www/html -type f -name "index.nginx-debian.html"
# Directory type
find /var/www/html -type d -name "logs"
# Search by size
find /home/ashar -size +10M
# Search by modification time
find /var/log -mtime -7
# Execute command on results
find /tmp -name "*.tmp" -exec rm -f {} \;

---
## 3. locate – Fast File Search
# Simple search
locate hello.py
# Partial name
locate index
# Case-insensitive
locate -i Index
# Limit results
locate -n 10 index
# Update database
sudo updatedb

---
## 4. pkill – Kill Processes by Name
# Kill by process name
pkill nginx
pkill python
# Case-insensitive
pkill -i firefox
# Force kill
pkill -9 node

---
## 5. curl – Test HTTP / APIs
curl http://localhost
curl -I https://google.com

---
## 6. less – View large files
less /var/log/nginx/error.log

---
## 7. ps / top – Monitor processes
ps aux
top

---
## 8. Disk & Memory
df -h         # disk usage
free -h       # memory usage

---
## 9. systemctl – Manage services
systemctl status nginx
systemctl restart docker