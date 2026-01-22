--
## What is DNS?
The Domain Name System (DNS) is the phonebook of the Internet. Humans access information online through [domain names](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/), like times.com or espn.com. Web browsers interact through [Internet Protocol (IP)](https://www.cloudflare.com/learning/network-layer/internet-protocol/) addresses. DNS translates domain names to [IP addresses](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/) so browsers can load Internet resources.

---

### How Does DNS Work? (Easy English)

DNS changes a website name into an IP address.
For example:
- Website name: **[www.example.com](http://www.example.com)**
- IP address: **192.168.1.1**    
Every device on the Internet has an IP address.  
An IP address is like a **home address**.  
Without an address, you cannot find a house.  
In the same way, without an IP address, a website cannot be found.

--------

### (4 DNS Servers Used to Load a Webpage)
When a webpage is opened, **four DNS servers** help to find the IP address.

---
### 1. DNS Recursor
The DNS recursor is like a **librarian**.
- It receives the request from your browser.
- The browser asks: _“What is the IP address of this website?”_
- The recursor does the hard work.
- It talks to other DNS servers to find the answer.
- Then it sends the final IP address back to the browser.
---
### 2. Root Name Server
The root server is like a **library index**.
- It is the **first stop** in DNS lookup.
- It does not know the exact IP address.
- It tells where to find the next server.
- It points to the **TLD server**.
---
### 3. TLD Name Server (Top Level Domain)
The TLD server is like a **book rack**.
- It handles the last part of the website name.
- Example:
    - In `example.com` → **.com** is the TLD
- It tells where the **authoritative server** is.
---
### 4. Authoritative Name Server
This is like a **dictionary**.
- It has the **final answer**.
- It knows the real IP address of the website.
- It returns the IP address to the DNS recursor.
- The browser then opens the website.
---
### Simple Flow (Easy to Remember)
Browser  
→ DNS Recursor  
→ Root Server 
→ TLD Server  
→ Authoritative Server  
→ IP Address returned  
→ Website opens


