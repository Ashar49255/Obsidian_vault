
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
    

5. **Stop NGINX**
    

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

# Reverse Proxy

## Definition:

A reverse proxy sits **in front of backend servers**, accepting requests from clients and forwarding them to appropriate backend servers.

## Key Uses:

1. **Load Distribution**: Distributes traffic across multiple servers
    
2. **SSL Termination**: Handles HTTPS encryption
    
3. **Caching**: Stores static content to reduce backend load
    
4. **Security**: Hides backend server details, provides DDoS protection

## Example Flow:

text

Client → Reverse Proxy → [Server1, Server2, Server3]
           (public IP)      (private IPs, hidden)

---

# Forward Proxy

## Definition:

A forward proxy sits **in front of clients**, acting as an intermediary between clients and the internet.

## Key Uses:

1. **Access Control**: Restricts which websites users can visit
    
2. **Privacy/Anonymity**: Hides client IP addresses
    
3. **Caching**: Stores frequently accessed content locally
    

## Example Flow:

text

[Client1, Client2, Client3] → Forward Proxy → Internet
   (internal network)           (gateway)

