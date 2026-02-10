# **Final User Management App – Complete Project**

---

## **Folder Structure**

``user-app/
``├── backend/
``│   ├── index.js          # Express server with all APIs
`│   ├── db.js             # MySQL connection
`│   ├── package.json      # Backend dependencies
`│   └── Dockerfile        # Backend Dockerfile
`├── frontend/
`│   ├── src/
`│   │   ├── App.jsx       # Main React component
`│   │   └── main.jsx      # Vite entry point
`│   ├── public/
`│   │   └── index.html
`│   ├── package.json      # Frontend dependencies
`│   ├── vite.config.js
`│   └── Dockerfile        # Frontend Dockerfile (Nginx)
`└── README.md             # Project documentation``


---

## **1. Backend Files**

### **db.js** – MySQL connection

`const mysql = require('mysql2/promise');  const db = mysql.createPool({   host: 'mysql-user-app', // Docker container name of MySQL   user: 'root',   password: 'yourpassword', // same as MySQL container env   database: 'user_app', });  module.exports = db;`

---

### **index.js** – Express API

`const express = require('express'); const bodyParser = require('body-parser'); const cors = require('cors'); const db = require('./db');  const app = express(); app.use(cors()); app.use(bodyParser.json());  // Add user app.post('/add-user', async (req, res) => {   const { name, email } = req.body;   try {     const [result] = await db.query(       'INSERT INTO users (name, email) VALUES (?, ?)',       [name, email]     );     res.json({ success: true, id: result.insertId });   } catch (err) {     res.status(500).json({ success: false, error: err.message });   } });  // Fetch all users app.get('/users', async (req, res) => {   try {     const [rows] = await db.query('SELECT * FROM users');     res.json(rows);   } catch (err) {     res.status(500).json({ success: false, error: err.message });   } });  // Delete user app.delete('/delete-user/:id', async (req, res) => {   try {     const { id } = req.params;     await db.query('DELETE FROM users WHERE id = ?', [id]);     res.json({ success: true });   } catch (err) {     res.status(500).json({ success: false, error: err.message });   } });  // Root route app.get('/', (req, res) => {   res.send('Backend running. API ready!'); });  // Start server app.listen(5000, () => console.log('Backend running on port 5000'));`

---

### **package.json** – Backend

`{   "name": "user-backend",   "version": "1.0.0",   "main": "index.js",   "scripts": {     "start": "node index.js"   },   "dependencies": {     "express": "^4.18.2",     "mysql2": "^3.5.2",     "cors": "^2.8.5",     "body-parser": "^1.20.2"   } }`

---

### **Backend Dockerfile**

`# backend/Dockerfile FROM node:20-alpine WORKDIR /app COPY package.json package-lock.json ./ RUN npm install COPY . . EXPOSE 5000 CMD ["node", "index.js"]`

---

## **2. Frontend Files**

### **src/main.jsx**

`import React from "react"; import ReactDOM from "react-dom/client"; import App from "./App";  ReactDOM.createRoot(document.getElementById("root")).render(   <React.StrictMode>     <App />   </React.StrictMode> );`

---

### **src/App.jsx** – User Interface

``import { useState, useEffect } from "react";  export default function App() {   const [name, setName] = useState("");   const [email, setEmail] = useState("");   const [users, setUsers] = useState([]);   const [message, setMessage] = useState("");   const API_URL = "http://54.87.19.100:5000"; // Replace with your EC2 public IP    const fetchUsers = async () => {     try {       const res = await fetch(`${API_URL}/users`);       setUsers(await res.json());     } catch (err) {       console.error(err);     }   };    useEffect(() => { fetchUsers(); }, []);    const handleAdd = async () => {     if (!name || !email) return setMessage("Name and Email required!");     try {       const res = await fetch(`${API_URL}/add-user`, {         method: "POST",         headers: { "Content-Type": "application/json" },         body: JSON.stringify({ name, email }),       });       const data = await res.json();       if (data.success) {         setMessage("User added successfully!");         setName(""); setEmail("");         fetchUsers();       }     } catch (err) { setMessage("Request failed: " + err.message); }   };    const handleDelete = async (id) => {     try {       const res = await fetch(`${API_URL}/delete-user/${id}`, { method: "DELETE" });       const data = await res.json();       if (data.success) {         setMessage("User deleted!");         fetchUsers();       }     } catch (err) { setMessage("Request failed: " + err.message); }   };    return (     <div style={{ maxWidth:600, margin:"50px auto", fontFamily:"Arial", background:"#f4f6f8", padding:20, borderRadius:10, boxShadow:"0 5px 15px rgba(0,0,0,0.1)"}}>       <h1 style={{textAlign:"center", color:"#34495e"}}>Add User</h1>        {message && <div style={{background:"#2ecc71", color:"#fff", padding:10, marginBottom:15, borderRadius:5, textAlign:"center"}}>{message}</div>}        <div style={{display:"flex", gap:10, marginBottom:20}}>         <input style={{flex:1, padding:10, borderRadius:5, border:"1px solid #ccc"}} placeholder="Name" value={name} onChange={e=>setName(e.target.value)}/>         <input style={{flex:1, padding:10, borderRadius:5, border:"1px solid #ccc"}} placeholder="Email" value={email} onChange={e=>setEmail(e.target.value)}/>         <button style={{padding:"10px 20px", background:"#3498db", color:"#fff", border:"none", borderRadius:5, cursor:"pointer"}} onClick={handleAdd}>Submit</button>       </div>        <h2 style={{color:"#34495e"}}>Users List</h2>       <table style={{width:"100%", borderCollapse:"collapse"}}>         <thead><tr>           <th style={{borderBottom:"2px solid #ddd", padding:8}}>Name</th>           <th style={{borderBottom:"2px solid #ddd", padding:8}}>Email</th>           <th style={{borderBottom:"2px solid #ddd", padding:8}}>Actions</th>         </tr></thead>         <tbody>           {users.map(u=>(             <tr key={u.id} style={{background:"#fff", marginBottom:5}}>               <td style={{borderBottom:"1px solid #eee", padding:8}}>{u.name}</td>               <td style={{borderBottom:"1px solid #eee", padding:8}}>{u.email}</td>               <td style={{borderBottom:"1px solid #eee", padding:8}}>                 <button style={{background:"#e74c3c", color:"#fff", border:"none", padding:"5px 10px", borderRadius:3, cursor:"pointer"}} onClick={()=>handleDelete(u.id)}>Delete</button>               </td>             </tr>           ))}           {users.length===0 && <tr><td colSpan={3} style={{textAlign:"center", padding:10}}>No users yet</td></tr>}         </tbody>       </table>     </div>   ); }``

---

### **public/index.html**

`<!DOCTYPE html> <html lang="en">   <head>     <meta charset="UTF-8" />     <meta name="viewport" content="width=device-width, initial-scale=1.0" />     <title>User App</title>   </head>   <body>     <div id="root"></div>     <script type="module" src="/src/main.jsx"></script>   </body> </html>`

---

### **package.json – Frontend**

`{   "name": "frontend",   "version": "1.0.0",   "scripts": {     "dev": "vite",     "build": "vite build",     "preview": "vite preview"   },   "dependencies": {     "react": "^19.2.4",     "react-dom": "^19.2.4"   },   "devDependencies": {     "vite": "^7.3.1",     "@vitejs/plugin-react": "^5.1.4"   } }`

---

### **vite.config.js**

`import { defineConfig } from 'vite' import react from '@vitejs/plugin-react'  export default defineConfig({   plugins: [react()] })`

---

### **Frontend Dockerfile**

`# Build stage FROM node:20-alpine AS build WORKDIR /app COPY package.json package-lock.json vite.config.js ./ RUN npm install COPY . . RUN npm run build  # Production stage FROM nginx:alpine COPY --from=build /app/dist /usr/share/nginx/html EXPOSE 80 CMD ["nginx", "-g", "daemon off;"]`

---

## **3. MySQL Container**

`docker run -d -p 3306:3306 --name mysql-user-app -e MYSQL_ROOT_PASSWORD=yourpassword -e MYSQL_DATABASE=user_app mysql:8`

**Create Table:**

`CREATE TABLE users (   id INT AUTO_INCREMENT PRIMARY KEY,   name VARCHAR(255),   email VARCHAR(255) );`

---

## **4. Running the Project**

### Backend:

`cd backend docker build -t user-backend . docker run -d -p 5000:5000 --name user-backend user-backend`

### Frontend:

`cd frontend docker build -t user-frontend . docker rm -f user-frontend docker run -d -p 80:80 --name user-frontend user-frontend`

- **Frontend:** `http://<EC2-Public-IP>/`
    
- **Backend API:** `http://<EC2-Public-IP>:5000`
    

---

## ✅ Features Working

1. Add user → Success message
    
2. Users list → show all users (without ID column)
    
3. Delete user → updates table & shows message
    
4. Styled table & form
    
5. Fully containerized (frontend + backend + MySQL)