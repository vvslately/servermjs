# 🚀 Express + MySQL API (ES Module) บน Render

โปรเจกต์นี้คือ API ที่เขียนด้วย **Express.js + MySQL2** รองรับ ES Modules และสามารถ Deploy ได้ฟรีบน [Render](https://render.com) พร้อมเชื่อมกับ React ได้ทันที ✅

---

## 📦 ขั้นตอนการติดตั้งและใช้งาน

### 1. สร้างโฟลเดอร์โปรเจกต์

```bash
mkdir express-mysql-api
cd express-mysql-api
```

---

### 2. สร้าง `package.json`

```bash
npm init -y
```

---

### 3. แก้ไข `package.json`

ให้มีเนื้อหาดังนี้:

```json
{
  "name": "express-mysql-api",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "start": "node server.mjs"
  },
  "dependencies": {}
}
```

---

### 4. ติดตั้ง dependencies

```bash
npm install express mysql2 cors dotenv
```

---

### 5. สร้างไฟล์ `server.mjs`

```js
import express from "express";
import mysql from "mysql2";
import cors from "cors";
import dotenv from "dotenv";

dotenv.config();

const app = express();
app.use(cors());
app.use(express.json());

const connection = mysql.createConnection({
  host: process.env.DB_HOST || "localhost",
  port: process.env.DB_PORT ? Number(process.env.DB_PORT) : 3306,
  user: process.env.DB_USER || "root",
  password: process.env.DB_PASSWORD || "",
  database: process.env.DB_NAME || "test",
});

connection.connect((err) => {
  if (err) {
    console.error("❌ DB connection error:", err);
    process.exit(1);
  }
  console.log("✅ Connected to MySQL");
});

app.get("/api/users", (req, res) => {
  connection.query("SELECT * FROM users", (err, results) => {
    if (err) {
      return res.status(500).json({ error: "Database query error" });
    }
    res.json(results);
  });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`🚀 Server running on port ${PORT}`);
});
```

---

### 6. สร้าง `.gitignore`

```bash
echo "node_modules/" >> .gitignore
echo ".env" >> .gitignore
```

---

### 7. สร้าง `.env` สำหรับการรันในเครื่อง (local only)

```ini
DB_HOST=hopper.proxy.rlwy.net
DB_PORT=51447
DB_USER=root
DB_PASSWORD=HDqPaLfMXZfdVdOMxZbNHVptAPRBojHU
DB_NAME=railway
```

---

### 8. ทดสอบรัน API

```bash
npm start
```

หากเชื่อมต่อสำเร็จ จะเห็น:

```
✅ Connected to MySQL
🚀 Server running on port 3000
```

จากนั้นเข้าที่: [http://localhost:3000/api/users](http://localhost:3000/api/users)

---

## ☁️ Deploy บน Render

### 9. เตรียม Git Repo

```bash
git init
git add .
git commit -m "Initial commit express mysql api"
git remote add origin https://github.com/yourusername/yourrepo.git
git push -u origin main
```

---

### 10. ไปที่ [https://render.com](https://render.com)

- สมัคร / ล็อกอิน
- สร้าง **New Web Service**
- เลือก GitHub repo ของคุณ
- ตั้งค่า:

| Key            | Value                        |
|----------------|------------------------------|
| DB_HOST        | hopper.proxy.rlwy.net        |
| DB_PORT        | 51447                        |
| DB_USER        | root                         |
| DB_PASSWORD    | HDqPaLfMXZfdVdOMxZbNHVptAPRBojHU |
| DB_NAME        | railway                      |
| PORT           | 3000                         |

---

### 11. ตั้งค่า Build/Start Command

- **Build Command**: `npm install`
- **Start Command**: `npm start`

---

### 12. Deploy แล้วรอระบบสร้าง

Render จะสร้างและให้ลิงก์ เช่น:

```
https://your-service-name.onrender.com/api/users
```

คุณสามารถเข้าทดสอบลิงก์นี้ได้ทันที

---

## 🔗 เชื่อมกับ React Frontend

สามารถเรียก API ได้ใน React เช่น:

```jsx
import { useEffect, useState } from 'react';

function App() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("https://your-service-name.onrender.com/api/users")
      .then(res => res.json())
      .then(data => setUsers(data));
  }, []);

  return (
    <div>
      <h1>Users</h1>
      <ul>
        {users.map(u => (
          <li key={u.users_id}>{u.users_name}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

---

## ✅ สรุป

- ใช้ Express.js (ES Module)
- เชื่อมต่อ MySQL ด้วย `mysql2`
- Deploy ฟรีผ่าน Render
- รองรับการเรียกผ่าน React frontend ได้ทันที

---

## 💬 ติดต่อ

หากมีคำถามเพิ่มเติมหรือต้องการให้ผมช่วย setup ทั้ง repo หรือ frontend ด้วย แจ้งได้เลยครับ 🙌
