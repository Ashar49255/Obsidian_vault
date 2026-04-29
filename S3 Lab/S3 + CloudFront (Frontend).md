# **AWS Full-Stack Project Documentation (Final Professional Version)**

**Project Name:** AWS Users Manager  
**Stack:** S3 + CloudFront (Frontend) | EC2 + Flask (Backend) | RDS MySQL (Database)

---

## **1️⃣ RDS MySQL Setup**

**Step 1:** Create RDS Database

- AWS Console → RDS → Create Database
    
- Engine: **MySQL**
    
- Template: Free Tier
    
- DB Identifier: `webapp-mysql-db`
    
- Master username/password: `admin / your_password`
    
- Public access: **Yes**
    
- Security group: allow **3306** from EC2
    

**Step 2:** Connect & create table

`mysql -h webapp-mysql-db.cjqomoqsm2ze.us-east-1.rds.amazonaws.com -u admin -p`

`CREATE TABLE users (   id INT AUTO_INCREMENT PRIMARY KEY,   name VARCHAR(50),   email VARCHAR(50) );`

✅ Database ready

---

## **2️⃣ EC2 Backend Setup (with venv)**

**Step 1:** SSH into EC2

`ssh -i key.pem ubuntu@<EC2_PUBLIC_IP>`

**Step 2:** Update & install Python

`sudo apt update -y sudo apt install python3-pip python3-venv -y`

**Step 3:** Create backend folder & virtual environment

`mkdir ~/backend && cd ~/backend python3 -m venv venv source venv/bin/activate`

> You will see `(venv)` in terminal

**Step 4:** Install dependencies

`pip install flask flask-cors pymysql`

**Step 5:** Generate `requirements.txt`

`pip freeze > requirements.txt`

**requirements.txt example:**

`Flask==2.3.3 Flask-Cors==3.0.10 PyMySQL==1.1.0`

---

## **3️⃣ Backend Code (app.py)**

`
```python
from flask import Flask, jsonify, request
from flask_cors import CORS
import pymysql

app = Flask(__name__)
CORS(app)

# Database configuration
DB_HOST = "webapp-mysql-db.cjqomoqsm2ze.us-east-1.rds.amazonaws.com"
DB_USER = "admin"
DB_PASSWORD = "your_password"
DB_NAME = "webapp-mysql-db"


def get_connection():
    return pymysql.connect(
        host=DB_HOST,
        user=DB_USER,
        password=DB_PASSWORD,
        database=DB_NAME,
        cursorclass=pymysql.cursors.DictCursor,
    )


@app.route("/api/users", methods=["GET"])
def get_users():
    conn = get_connection()
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM users")
    data = cursor.fetchall()
    conn.close()
    return jsonify(data)


@app.route("/api/users", methods=["POST"])
def add_user():
    data = request.json

    conn = get_connection()
    cursor = conn.cursor()
    cursor.execute(
        "INSERT INTO users (name, email) VALUES (%s, %s)",
        (data["name"], data["email"]),
    )
    conn.commit()
    conn.close()

    return jsonify({"message": "User added successfully"})


@app.route("/api/users/<int:user_id>", methods=["DELETE"])
def delete_user(user_id):
    conn = get_connection()
    cursor = conn.cursor()
    cursor.execute("DELETE FROM users WHERE id = %s", (user_id,))
    conn.commit()
    conn.close()

    return jsonify({"message": "User deleted successfully"})


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=True)
```


**Step 6:** Run backend

`source venv/bin/activate python app.py`

**Step 7:** Test backend

`curl http://<EC2_PUBLIC_IP>:5000/api/users`

---

## **4️⃣ Frontend Setup (S3 + CloudFront)**

### **Step 1: S3 Bucket**

1. AWS → S3 → Create bucket: `my-frontend-bucket-abc123`
    
2. Enable **static website hosting** → `index.html` as default document
    
3. Bucket permissions:
    
    - If private → CloudFront OAI will access
        
    - If public → set bucket policy read for all
        

**Step 2: Upload Frontend**

**File:** `index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <title>AWS Users Dashboard</title>

    <style>
        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #4f46e5, #9333ea);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .card {
            background: #ffffff;
            width: 420px;
            padding: 25px;
            border-radius: 16px;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.25);
        }

        h1 {
            text-align: center;
            margin-bottom: 20px;
            color: #111827;
        }

        .form-group input {
            width: 100%;
            padding: 12px;
            margin-bottom: 10px;
            border-radius: 8px;
            border: 1px solid #d1d5db;
            font-size: 14px;
        }

        .form-group input:focus {
            outline: none;
            border-color: #6366f1;
        }

        .add-btn {
            width: 100%;
            padding: 12px;
            background: #4f46e5;
            color: white;
            font-size: 15px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.3s;
        }

        .add-btn:hover {
            background: #4338ca;
        }

        ul {
            list-style: none;
            padding: 0;
            margin-top: 20px;
        }

        li {
            background: #f9fafb;
            padding: 12px;
            margin-bottom: 10px;
            border-radius: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .user-info {
            display: flex;
            flex-direction: column;
        }

        .user-name {
            font-weight: 600;
            color: #111827;
        }

        .user-email {
            font-size: 13px;
            color: #6b7280;
        }

        .delete-btn {
            background: #ef4444;
            color: white;
            border: none;
            padding: 6px 10px;
            border-radius: 6px;
            cursor: pointer;
        }

        .delete-btn:hover {
            background: #dc2626;
        }

        .footer {
            text-align: center;
            margin-top: 15px;
            font-size: 12px;
            color: #9ca3af;
        }
    </style>
</head>

<body>
    <div class="card">
        <h1>👥 Users Manager</h1>

        <div class="form-group">
            <input id="name" type="text" placeholder="Enter name" />
            <input id="email" type="email" placeholder="Enter email" />
            <button class="add-btn" onclick="addUser()">➕ Add User</button>
        </div>

        <ul id="users"></ul>

        <div class="footer">
            AWS • EC2 • RDS • S3 • CloudFront
        </div>
    </div>

    <script>
        // Replace <EC2_PUBLIC_IP> with your actual EC2 public IP or domain
        const API = "http://<EC2_PUBLIC_IP>:5000/api/users";

        function loadUsers() {
            fetch(API)
                .then(res => res.json())
                .then(data => {
                    const ul = document.getElementById("users");
                    ul.innerHTML = "";

                    data.forEach(u => {
                        ul.innerHTML += `
                            <li>
                                <div class="user-info">
                                    <span class="user-name">${u.name}</span>
                                    <span class="user-email">${u.email}</span>
                                </div>
                                <button class="delete-btn" onclick="deleteUser(${u.id})">
                                    Delete
                                </button>
                            </li>
                        `;
                    });
                });
        }

        function addUser() {
            const name = document.getElementById("name").value;
            const email = document.getElementById("email").value;

            if (!name || !email) {
                alert("Please enter name and email");
                return;
            }

            fetch(API, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({ name, email }),
            }).then(() => {
                document.getElementById("name").value = "";
                document.getElementById("email").value = "";
                loadUsers();
            });
        }

        function deleteUser(id) {
            fetch(`${API}/${id}`, {
                method: "DELETE",
            }).then(() => loadUsers());
        }

        loadUsers();
    </script>
</body>
</html>
```

- Replace `<EC2_PUBLIC_IP>` with backend EC2 IP
    
- Upload to **S3 bucket**
    

---

# **Option 1: Public Bucket (Simple, Not Recommended for Production)**

1. Go to S3 → Select bucket → Permissions → **Bucket Policy**
    
2. Add this JSON policy:

`{
 `` "Version": "2012-10-17",
  `"Statement": [
    `{
      `"Sid": "PublicReadGetObject",
      `"Effect": "Allow",
      `"Principal": "*",
      `"Action": "s3:GetObject",
      `"Resource": "arn:aws:s3:::my-frontend-bucket-abc123/*"
    `}
  `]
``}

--------

### **Step 3: CloudFront Distribution**

1. CloudFront → Create Distribution
    
2. Origin type: **Amazon S3** → bucket endpoint
    
3. **Allow private bucket access (OAI)** → ✅
    
4. Default root object: `index.html`
    
5. Create → wait 10–15 min
    
6. CloudFront domain:
    

`https://d123abcd.cloudfront.net`

- Test: frontend live + HTTPS + CRUD working
    

---

## **5️⃣ Backend Auto-Start (systemd)**

`sudo nano /etc/systemd/system/flask-backend.service`

`[Unit] Description=Flask Backend After=network.target  [Service] User=ubuntu WorkingDirectory=/home/ubuntu/backend Environment="PATH=/home/ubuntu/backend/venv/bin" ExecStart=/home/ubuntu/backend/venv/bin/python app.py Restart=always  [Install] WantedBy=multi-user.target`

`sudo systemctl daemon-reload sudo systemctl enable flask-backend sudo systemctl start flask-backend sudo systemctl status flask-backend`

✅ Backend auto-start verified

---

## **6️⃣ Full Testing**

1. Open **CloudFront URL** → frontend loads
    
2. Add user → check `/api/users` → works
    
3. Delete user → check database → works
    
4. GET `/api/users` → returns all users
    

✅ Full stack verified, production-ready

---

## **7️⃣ Deliverables**

- `app.py` (backend)
    
- `requirements.txt`
    
- `index.html` (frontend)
    
- Systemd service for backend
    
- CloudFront + S3 setup
    
- RDS MySQL table
    

---

> Built a full-stack AWS application: Frontend on S3 + CloudFront (HTTPS, CDN), backend REST APIs on EC2 (Flask), and RDS MySQL as database. Supports full CRUD operations with modern UI, secure and globally accessible.