### Path-Based Routing kya hoti hai?

Is mein **URL ka path** dekh kar request ko different service ya app ko bheja jata hai.

👉 same domain hota hai, **sirf path change hota hai**

**Example:**

`example.com/api   → API service example.com/app   → Web App example.com/admin → Admin panel`

🧠 **Socho**:  
Domain ek hi ghar hai, lekin ghar ke **different rooms (paths)** hain.

**Real life use:**

- Kubernetes Ingress
    
- Nginx / Load Balancer
    
- Microservices architecture
    

---

### Host-Based Routing kya hoti hai?

Is mein **domain / subdomain (host)** dekh kar request route hoti hai.

👉 **Different domains ya subdomains** hotay hain

**Example:**

`api.example.com     → API service app.example.com     → Web App admin.example.com   → Admin panel`

🧠 **Socho**:  
Yahan **alag alag ghar (subdomains)** hain, har ghar ka apna kaam.

---
### Path vs Host — Simple Comparison

|Cheez|Path-Based|Host-Based|
|---|---|---|
|Domain|Same|Different|
|Routing kis pe|URL path|Host / Subdomain|
|Example|/api, /app|api., app.|
|Use kab|Small / medium apps|Large systems|