
---

## **1. What is NGINX?**

- NGINX (pronounced "Engine-X") is a **web server**.
    
- It can also be used as a **reverse proxy, load balancer, and cache server**.
    
- Basically, it helps your website or web app handle **lots of visitors** efficiently.
    

---

## **2. Why we use NGINX?**

- **Fast:** Handles thousands of connections at the same time.
    
- **Lightweight:** Doesn’t use too much memory.
    
- **Flexible:** Can act as a web server, reverse proxy, or load balancer.
    
- **Reliable:** Often used in big websites like Netflix, Google, and Airbnb.
    

---

## **3. How NGINX works (simple version)**

Imagine your website is a **restaurant**:

1. Visitors are **customers**.
    
2. NGINX is the **reception**: it takes requests from customers.
    
3. It either:
    
    - **Serves the food itself** (like serving static files: HTML, CSS, images), or
        
    - **Passes the request to the kitchen** (like sending dynamic requests to your app, e.g., Python Flask, Node.js).
        

This is why it’s called a **reverse proxy** when passing requests to your app server.

---

## **4. NGINX vs Apache (easy comparison)**

|Feature|NGINX|Apache|
|---|---|---|
|Performance|Very fast|Slower under heavy traffic|
|Handling traffic|Event-driven (many users at once)|Process-based (uses more memory)|
|Static files|Fast|Slower|
|Dynamic files|Pass to another server|Can handle itself|

---

## **5. Basic NGINX Usage**
1. **Install NGINX**
`sudo apt update sudo apt install nginx`
2. **Start NGINX**
`sudo systemctl start nginx`
3. **Check status**
`sudo systemctl status nginx`
4. **Visit in browser**
- Go to `http://localhost`
- You will see the default NGINX page.
1. **Stop NGINX**
`sudo systemctl stop nginx`

---
## **6. Where NGINX stores files**
- **Configuration files:** `/etc/nginx/`
- **Web files (HTML, images):** `/var/www/html/`
---
## **7. NGINX is used for**
- Hosting websites
- Serving static files fast
- Acting as a reverse proxy (sending requests to app servers)
- Load balancing (sharing traffic across multiple servers)
---
## **1. Forward Proxy (Client Side Proxy)**
### **What is it?**
- Forward Proxy sits **in front of users (clients)**.
- It hides the **client** from the internet.
### **Simple Example**
Imagine:
- You (user) want to visit **google.com**
- You are not allowed directly
- You send request to a **proxy server**
- Proxy server sends request to Google **on your behalf**
👉 Google **does NOT know who you are**, it only sees the proxy.
### **Flow**
`User → Forward Proxy → Internet/Website`
### **Why use Forward Proxy?**
- Hide user identity (privacy)
    - Control internet usage (schools, offices)
    - Block websites
    - Cache websites to save bandwidth
    ### **Real-life Example**
- Office internet restrictions
- VPN (mostly works like forward proxy)
---
## **2. Reverse Proxy (Server Side Proxy)**
### **What is it?**
- Reverse Proxy sits **in front of servers**.
- It hides the **backend servers** from users.
### **Simple Example**
Imagine:
- User opens **your website**
- Request first goes to **NGINX**
- NGINX forwards it to:
    - App server
     - Backend server    
    - Database server
👉 User **never knows** which server handled the request.
### **Flow**
`User → Reverse Proxy (NGINX) → Backend Servers`
### **Why use Reverse Proxy?**
- Load balancing
- Security (hide backend servers)
- SSL handling (HTTPS)
- Better performance
- Caching
### **Real-life Example**
- NGINX
- HAProxy
- AWS Load Balancer
---
## **3️⃣ Reverse Proxy vs Forward Proxy**

| Feature           | Reverse Proxy                            | Forward Proxy                                      |
| ----------------- | ---------------------------------------- | -------------------------------------------------- |
| **Location**      | In front of **servers**                  | In front of **clients**                            |
| **Purpose**       | Protect and manage servers               | Protect and manage clients                         |
| **Visibility**    | Client sees proxy as the server          | Server sees proxy as the client                    |
| **Common Use**    | Load balancing, caching, SSL termination | Anonymity, internet filtering, bypass restrictions |
| **Who it serves** | Backend servers                          | Clients                                            |

---
💡 **Easy way to remember:**
- **Reverse Proxy** = Protects the **server side**.
- **Forward Proxy** = Protects the **client side**.
-----
# **What is the different between the Apache web server and Nginx**
### **Key Differences Explained**
1. **Architecture & Performance**
    - Apache: Every new connection creates a new **process or thread** → memory heavy under high traffic.
    - Nginx: Uses **event-driven asynchronous model** → can handle **10,000+ connections** with minimal memory.
2. **Serving Static vs Dynamic Content**
    - Apache: Can serve static and dynamic content directly using modules like mod_php.
    - Nginx: Super fast for static content, but for dynamic content, it forwards requests to backend servers (Python, PHP, Node.js).
3. **Configuration & Flexibility**
    - Apache: `.htaccess` allows directory-level config changes → good for shared hosting.
    - Nginx: Centralized config only → more efficient and faster, but no per-directory overrides.
4. **Use Cases in Industry**
    - Apache: Good for **legacy apps, shared hosting, PHP-heavy websites**.
    - Nginx: Best for **high traffic websites, microservices, cloud apps, load balancing, reverse proxy setups**
-------------------------
- **Apache** = Old style, slower under high traffic, can run dynamic apps directly.
- **Nginx** = Modern, super fast, handles static content efficiently, uses backend for dynamic apps.