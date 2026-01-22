Architecture (Simple)

**Client → NGINX → PHP-FPM → MySQL → WordPress**

**Prerequisites**
**Application:** WordPress  
**Server:** Ubuntu 24.04 LTS (AWS EC2)  
**Web Server:** NGINX  
**PHP Version:** 8.3 FPM  
**Database:** MySQL 8.0



![[Screenshot 2026-01-21 165117.png]]


<center>Step 1: system update & upgrade with install Nginx</center>


![[Pasted image 20260121170347.png]]
<center>Step 2: Install MySQL and create WordPress DB & User</center>

![[Pasted image 20260121170513.png]]

<center>Step:3 Install PHP & PHP8.3-FPM and Download Wordpress and file permission</center>


![[Pasted image 20260121171125.png]]


<center>step 4: NGINX configuration &  site Enable</center>

![[Pasted image 20260121171448.png]]

<center>step 5: Configure WordPress</center>


![[Pasted image 20260121182356.png]]


<center> Finally WordPress Installed </center>
