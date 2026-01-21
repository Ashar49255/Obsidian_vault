
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



