### Domain

`ashar.razesteer.com`

### Routing (Nginx – Path Based)

|URL|Service|
|---|---|
|/app1|Frontend 1 (React – attractive UI)|
|/api1|Backend 1 (Node + MySQL)|
|/app2|Frontend 2 (React – attractive UI)|
|/api2|Backend 2 (Node + MySQL)|

### Tech Stack

- **Frontend:** React JS (build mode, served by Nginx)
    
- **Backend:** Node.js + Express
    
- **Process Manager:** PM2
    
- **Database:** MySQL (2 DBs inside 1 MySQL)
    
- **Web Server:** Nginx
    
- **OS:** Ubuntu (EC2 / VM)
    

---

# 📁 FINAL FOLDER STRUCTURE

`/var/www/  ├── app1-frontend/  │    └── build/  ├── app2-frontend/  │    └── build/   /opt/  ├── api1-backend/  └── api2-backend/`

---

# 🚀 PHASE 1 – BASIC SERVER SETUP (ZERO MACHINE)

### 1️⃣ Update system

`sudo apt update && sudo apt upgrade -y`

### 2️⃣ Install required software

`sudo apt install nginx mysql-server -y`

### 3️⃣ Install Node.js (LTS)

`curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - sudo apt install nodejs -y`

### 4️⃣ Install PM2

`sudo npm install -g pm2`

---

# 🗄️ PHASE 2 – MYSQL SETUP (VERY IMPORTANT)

### 1️⃣ Secure MySQL

`sudo mysql_secure_installation`

### 2️⃣ Login MySQL

`sudo mysql`

### 3️⃣ Create Databases

`CREATE DATABASE app1_db; CREATE DATABASE app2_db;`

### 4️⃣ Create Tables

**App1 (name + designation):**

`USE app1_db; CREATE TABLE users (   id INT AUTO_INCREMENT PRIMARY KEY,   name VARCHAR(100),   designation VARCHAR(100) );`

**App2 (name + education + skill):**

`USE app2_db; CREATE TABLE users (   id INT AUTO_INCREMENT PRIMARY KEY,   name VARCHAR(100),   education VARCHAR(100),   fav_skill VARCHAR(100) );`

Exit MySQL:

`exit;`

---

# ⚙️ PHASE 3 – BACKEND 1 (API1)

### 1️⃣ Create backend folder

`sudo mkdir /opt/api1-backend cd /opt/api1-backend npm init -y npm install express mysql2 cors`

### 2️⃣ `index.js`

`const express = require("express"); const mysql = require("mysql2"); const cors = require("cors");  const app = express(); app.use(cors()); app.use(express.json());  const db = mysql.createConnection({   host: "localhost",   user: "root",   password: "",   database: "app1_db" });  app.post("/api1/add-user", (req, res) => {   const { name, designation } = req.body;   db.query(     "INSERT INTO users (name, designation) VALUES (?,?)",     [name, designation],     () => res.send({ success: true })   ); });  app.get("/api1/users", (req, res) => {   db.query("SELECT * FROM users", (err, data) => res.json(data)); });  app.listen(5001, () => console.log("API1 running on 5001"));`

---

# ⚙️ PHASE 4 – BACKEND 2 (API2)

`sudo mkdir /opt/api2-backend cd /opt/api2-backend npm init -y npm install express mysql2 cors`

### `index.js`

`const express = require("express"); const mysql = require("mysql2"); const cors = require("cors");  const app = express(); app.use(cors()); app.use(express.json());  const db = mysql.createConnection({   host: "localhost",   user: "root",   password: "",   database: "app2_db" });  app.post("/api2/add-user", (req, res) => {   const { name, education, fav_skill } = req.body;   db.query(     "INSERT INTO users VALUES (NULL,?,?,?)",     [name, education, fav_skill],     () => res.send({ success: true })   ); });  app.get("/api2/users", (req, res) => {   db.query("SELECT * FROM users", (err, data) => res.json(data)); });  app.listen(5002, () => console.log("API2 running on 5002"));`

---

# 🔁 PHASE 5 – PM2 (FINAL START)

`pm2 start /opt/api1-backend/index.js --name api1 pm2 start /opt/api2-backend/index.js --name api2  pm2 save pm2 startup`

✅ Backend ab **auto-restart + reboot-safe** hai

---

# 🎨 PHASE 6 – FRONTEND 1 (ATTRACTIVE UI)

`cd /var/www npx create-react-app app1-frontend cd app1-frontend npm install axios`

### `src/App.js`

`import axios from "axios"; import { useState, useEffect } from "react";  export default function App() {   const [name, setName] = useState("");   const [designation, setDesignation] = useState("");   const [users, setUsers] = useState([]);    const load = () =>     axios.get("/api1/users").then(r => setUsers(r.data));    useEffect(load, []);    const submit = () => {     axios.post("/api1/add-user", { name, designation }).then(load);   };    return (     <div style={{padding:40}}>       <h2>👨‍💼 Employee Manager</h2>       <input placeholder="Name" onChange={e=>setName(e.target.value)} />       <input placeholder="Designation" onChange={e=>setDesignation(e.target.value)} />       <button onClick={submit}>Add</button>       <ul>{users.map(u=><li>{u.name} - {u.designation}</li>)}</ul>     </div>   ); }`

`npm run build`

---

# 🎨 PHASE 7 – FRONTEND 2 (ATTRACTIVE UI)

Same steps, different fields.

`npx create-react-app app2-frontend cd app2-frontend npm install axios`

`App.js` → name, education, fav_skill

`npm run build`

---

# 🌐 PHASE 8 – FINAL NGINX CONFIG

`server {   listen 443 ssl;   server_name ashar.razesteer.com;    location /app1/ {     root /var/www/app1-frontend/build;     try_files $uri /index.html;   }    location /api1/ {     proxy_pass http://localhost:5001;   }    location /app2/ {     root /var/www/app2-frontend/build;     try_files $uri /index.html;   }    location /api2/ {     proxy_pass http://localhost:5002;   } }`

`sudo nginx -t sudo systemctl reload nginx`

---

# ✅ FINAL TEST (BROWSER)

|URL|
|---|
|https://ashar.razesteer.com/app1|
|https://ashar.razesteer.com/app2|
|https://ashar.razesteer.com/api1/users|
|https://ashar.razesteer.com/api2/users|