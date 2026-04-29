# **Creating Users and SSH Access in a Linux VM**
## **1. Creating Users**
To create two new users inside the VM:
sudo adduser user1
sudo adduser user2
- Set password when prompted.
- Check home directories:
ls /home
---
## **2. Checking SSH Service**
Ensure SSH server is installed and running:
sudo apt install openssh-server -y
sudo systemctl status ssh
If not running:
sudo systemctl start ssh
sudo systemctl enable ssh
---
## **3. SSH Configuration**
Edit the SSH config file:
sudo nano /etc/ssh/sshd_config
Make sure the following lines exist:
PasswordAuthentication yes
AllowUsers ashar user1 user2
- `AllowUsers` restricts SSH login to these users.
- Restart SSH to apply changes:
sudo systemctl restart ssh
---
## **4. Giving Sudo Access (Optional)**
To allow users to run sudo commands:
sudo usermod -aG sudo user1
sudo usermod -aG sudo user2

---

## **5. Testing SSH Access**
### **From the VM itself:**
ssh user1@localhost
ssh user2@localhost
- Enter password → login should work.
### **From the host machine (VM IP):**
ssh user1@VM_IP
ssh user2@VM_IP
- Replace `VM_IP` with the VM’s IP:
hostname -I
---
## **6. Workflow Summary**
adduser → ssh config → restart ssh → test login

---
## **7. Notes / Best Practices**
- Always check the **exact spelling** of usernames.
- Restrict SSH access using **AllowUsers**.
- Disable root login for security (`PermitRootLogin no` in sshd_config).
- Use `sudo` group for controlled administrative privileges.