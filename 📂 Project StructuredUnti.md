## Plaintext

```
user-app/
│
├── backend/
│   ├── index.js          # Express server APIs
│   ├── db.js             # MySQL connection pool
│   ├── package.json      # Node dependencies
│   └── Dockerfile        # Backend container config
│
├── frontend/
│   ├── src/
│   │   ├── App.jsx       # Main React UI
│   │   └── main.jsx      # Vite entry point
│   ├── public/
│   │   └── index.html
│   ├── package.json      # Frontend dependencies
│   ├── vite.config.js
│   └── Dockerfile        # Nginx production build
│
└── README.md             # Documentation
```

---

## 🛠️ 1. Backend Configuration

### `db.js` – Database Connection

Is file mein MySQL pool configuration hai.

> [!IMPORTANT] `host` mein Docker container ka naam (`mysql-user-app`) istemal kiya gaya hai.

JavaScript

```
const mysql = require('mysql2/promise');

const db = mysql.createPool({
  host: 'mysql-user-app', 
  user: 'root',
  password: 'yourpassword', 
  database: 'user_app',
});

module.exports = db;
```

### `index.js` – Express API

Yahan teen main routes hain: `POST /add-user`, `GET /users`, aur `DELETE /delete-user/:id`.

JavaScript

```
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const db = require('./db');
const app = express();

app.use(cors());
app.use(bodyParser.json());

// Add user
app.post('/add-user', async (req, res) => {
  const { name, email } = req.body;
  try {
    const [result] = await db.query(
      'INSERT INTO users (name, email) VALUES (?, ?)',
      [name, email]
    );
    res.json({ success: true, id: result.insertId });
  } catch (err) {
    res.status(500).json({ success: false, error: err.message });
  }
});

// Fetch all users
app.get('/users', async (req, res) => {
  try {
    const [rows] = await db.query('SELECT * FROM users');
    res.json(rows);
  } catch (err) {
    res.status(500).json({ success: false, error: err.message });
  }
});

// Delete user
app.delete('/delete-user/:id', async (req, res) => {
  try {
    const { id } = req.params;
    await db.query('DELETE FROM users WHERE id = ?', [id]);
    res.json({ success: true });
  } catch (err) {
    res.status(500).json({ success: false, error: err.message });
  }
});

app.listen(5000, () => console.log('Backend running on port 5000'));
```

---

## 💻 2. Frontend Configuration (React + Vite)

### `App.jsx` – User Interface

UI ko clean banane ke liye inline CSS ka use kiya gaya hai.

> [!NOTE] `API_URL` ko apne EC2 ya server IP se replace karein.

JavaScript

```
import { useState, useEffect } from "react";

export default function App() {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [users, setUsers] = useState([]);
  const [message, setMessage] = useState("");
  const API_URL = "http://YOUR_EC2_IP:5000"; 

  const fetchUsers = async () => {
    try {
      const res = await fetch(`${API_URL}/users`);
      setUsers(await res.json());
    } catch (err) { console.error(err); }
  };

  useEffect(() => { fetchUsers(); }, []);

  const handleAdd = async () => {
    if (!name || !email) return setMessage("Name and Email required!");
    const res = await fetch(`${API_URL}/add-user`, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ name, email }),
    });
    const data = await res.json();
    if (data.success) {
      setMessage("User added successfully!");
      setName(""); setEmail("");
      fetchUsers();
    }
  };

  const handleDelete = async (id) => {
    const res = await fetch(`${API_URL}/delete-user/${id}`, { method: "DELETE" });
    const data = await res.json();
    if (data.success) {
      setMessage("User deleted!");
      fetchUsers();
    }
  };

  return (
    <div style={{ maxWidth:600, margin:"50px auto", fontFamily:"Arial", background:"#f4f6f8", padding:20, borderRadius:10, boxShadow:"0 5px 15px rgba(0,0,0,0.1)"}}>
      <h1 style={{textAlign:"center", color:"#34495e"}}>User Management</h1>
      
      {message && <div style={{background:"#2ecc71", color:"#fff", padding:10, marginBottom:15, borderRadius:5, textAlign:"center"}}>{message}</div>}

      <div style={{display:"flex", gap:10, marginBottom:20}}>
        <input style={{flex:1, padding:10, borderRadius:5, border:"1px solid #ccc"}} placeholder="Name" value={name} onChange={e=>setName(e.target.value)}/>
        <input style={{flex:1, padding:10, borderRadius:5, border:"1px solid #ccc"}} placeholder="Email" value={email} onChange={e=>setEmail(e.target.value)}/>
        <button style={{padding:"10px 20px", background:"#3498db", color:"#fff", border:"none", borderRadius:5, cursor:"pointer"}} onClick={handleAdd}>Add</button>
      </div>

      <table style={{width:"100%", borderCollapse:"collapse"}}>
        <thead>
          <tr style={{background:"#eee"}}>
            <th style={{padding:10}}>Name</th>
            <th style={{padding:10}}>Email</th>
            <th style={{padding:10}}>Action</th>
          </tr>
        </thead>
        <tbody>
          {users.map(u=>(
            <tr key={u.id}>
              <td style={{padding:10, borderBottom:"1px solid #ddd"}}>{u.name}</td>
              <td style={{padding:10, borderBottom:"1px solid #ddd"}}>{u.email}</td>
              <td style={{padding:10, borderBottom:"1px solid #ddd"}}>
                <button style={{background:"#e74c3c", color:"#fff", border:"none", padding:"5px 10px", borderRadius:3}} onClick={()=>handleDelete(u.id)}>Delete</button>
              </td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}
```

---

## 🐳 3. Docker Deployment

### Step 1: MySQL Database Setup

Pehle database container run karein aur table create karein.

Bash

```
# Run MySQL Container
docker run -d -p 3306:3306 --name mysql-user-app \
  -e MYSQL_ROOT_PASSWORD=yourpassword \
  -e MYSQL_DATABASE=user_app \
  mysql:8

# Create Table (Execute inside container)
docker exec -it mysql-user-app mysql -u root -pyourpassword -e "
USE user_app;
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255),
  email VARCHAR(255)
);"
```

### Step 2: Build & Run Backend

Bash

```
cd backend
docker build -t user-backend .
docker run -d -p 5000:5000 --name user-backend --link mysql-user-app user-backend
```

### Step 3: Build & Run Frontend

Bash

```
cd frontend
docker build -t user-frontend .
docker run -d -p 80:80 --name user-frontend user-frontend
```

---

## ✅ Final Checklist

- [x] **Frontend:** `http://<EC2-IP>/`
    
- [x] **Backend API:** `http://<EC2-IP>:5000/users`
    
- [x] **Database:** MySQL port 3306 open (optional for local access)
    
- [x] **Security Groups:** Inbound rules mein port 80 aur 5000 allow hone chahiye.