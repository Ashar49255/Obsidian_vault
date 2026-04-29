## 🔹 Frontend ke liye NGINX

👉 User directly is se baat karta hai (browser → nginx)

**Frontend NGINX ka role:**

- HTML, CSS, JS serve karna
    
- React / Angular / Vue build files host karna
    
- Images, static files fast deliver karna
    
- HTTPS (SSL) handle karna
    

**Flow:**

`User Browser      ↓   NGINX      ↓  Static Files (HTML/CSS/JS)`

**Example:**

`server {     listen 80;     server_name example.com;      root /var/www/frontend;     index index.html; }`

📌 **Real-life example:**  
Company website / landing page / dashboard UI

---

## 🔹 Backend ke liye NGINX

👉 User directly backend ko hit nahi karta  
👉 NGINX beech mein **reverse proxy** hota hai

**Backend NGINX ka role:**

- API requests forward karna
    
- Load balancing (multiple backend servers)
    
- Security layer (backend hide karna)
    
- Port management (backend 3000, 5000 etc)
    

**Flow:**

`User Browser      ↓   NGINX      ↓ Backend App (Node / Flask / Django)`

**Example:**

`server {     listen 80;     server_name api.example.com;      location / {         proxy_pass http://localhost:5000;     } }`

📌 **Real-life example:**  
Login API, payments, database access