# Quick Start Guide - Database Restore & Login

## 🚀 Quick Setup (Copy & Paste Commands)

### Prerequisite
- MySQL/XAMPP running
- Backend tidak perlu running saat ini

### 1️⃣ Reset Database (if needed)
```bash
cd C:\xampp\mysql\bin
.\mysql.exe -u root -e "DROP DATABASE IF EXISTS vinixport; CREATE DATABASE vinixport;"
```

### 2️⃣ Import Schema
```bash
cd C:\xampp\mysql\bin
Get-Content "c:\Users\bagus\Downloads\YYYYYYYY\YYYYYYYY\backend\schema.sql" | .\mysql.exe -u root
```

### 3️⃣ Import Sample Data
```bash
cd C:\xampp\mysql\bin
Get-Content "c:\Users\bagus\Downloads\YYYYYYYY\YYYYYYYY\backend\sample_data.sql" | .\mysql.exe -u root
```

### 4️⃣ Import Projects & Certificates Data
```bash
cd C:\xampp\mysql\bin
Get-Content "c:\Users\bagus\Downloads\YYYYYYYY\YYYYYYYY\backend\add_sample_projects.sql" | .\mysql.exe -u root
```

### 5️⃣ Verify Data
```bash
cd C:\xampp\mysql\bin
.\mysql.exe -u root vinixport -e "SELECT COUNT(*) as total_users FROM users; SELECT COUNT(*) as total_projects FROM projects; SELECT COUNT(*) as total_certificates FROM certificates;"
```

Expected output:
```
total_users: 6
total_projects: 7
total_certificates: 7
```

---

## 🔐 Test User Accounts

Copy salah satu dari bawah:

### User 1: Frontend Developer
```
Email: john.doe@example.com
Password: password123
```

### User 2: UX Designer
```
Email: sarah.w@example.com
Password: password123
```

### User 3: Full Stack Developer
```
Email: michael.c@example.com
Password: password123
```

### User 4: Data Scientist
```
Email: emma.r@example.com
Password: password123
```

### User 5: Junior Developer
```
Email: ade.saputra@example.com
Password: password123
```

### Mentor
```
Email: mentor@vinixport.com
Password: password123
```

---

## 🧪 Quick Login Test

### Option A: Command Line Test
```bash
# Test login John Doe
curl -X POST http://localhost:5000/api/login \
  -H "Content-Type: application/json" \
  -d "{\"email\":\"john.doe@example.com\",\"password\":\"password123\"}"
```

Expected: `{"user":{"id":3,"name":"John Doe",...}}`

### Option B: Browser Test
1. Start backend: `cd backend && npm start`
2. Start frontend: `npm run dev`
3. Go to http://localhost:5173
4. Click Login
5. Enter: `john.doe@example.com` / `password123`
6. Click Sign In
7. Should see portfolio with 2 projects

---

## 📊 Database Verify Commands

```bash
# Show all users
mysql -u root vinixport -e "SELECT id, name, email, role FROM users;"

# Show all projects
mysql -u root vinixport -e "SELECT u.name as user, p.title as project, COUNT(*) FROM projects p JOIN users u ON p.user_id=u.id GROUP BY p.user_id, p.title;"

# Show all certificates
mysql -u root vinixport -e "SELECT u.name as user, c.title as certificate FROM certificates c JOIN users u ON c.user_id=u.id;"

# Show review requests
mysql -u root vinixport -e "SELECT menteeName, status, paymentStatus FROM review_requests;"

# Count everything
mysql -u root vinixport -e "SELECT 'users' as type, COUNT(*) as count FROM users UNION SELECT 'projects', COUNT(*) FROM projects UNION SELECT 'certificates', COUNT(*) FROM certificates UNION SELECT 'skills', COUNT(*) FROM skills UNION SELECT 'reviews', COUNT(*) FROM review_requests;"
```

---

## ✅ Success Checklist

After running all commands above, verify:

- [ ] Database `vinixport` exists
- [ ] All 6 users created with data
- [ ] All 7 projects visible
- [ ] All 7 certificates visible
- [ ] All 5 review requests visible
- [ ] Can login dengan email & password
- [ ] Portfolio shows projects & certificates
- [ ] Mentor dashboard shows review requests

---

## 📂 File Locations

All files referenced:
```
c:\Users\bagus\Downloads\YYYYYYYY\YYYYYYYY\
├── backend\
│   ├── schema.sql (database structure)
│   ├── sample_data.sql (users with passwords)
│   └── add_sample_projects.sql (projects, certs, skills)
├── USER_DATA_RESTORE.md (detailed guide)
├── DATA_ISSUE_EXPLANATION.md (what happened & why)
└── QUICK_START_COMMANDS.md (this file)
```

---

## ⚠️ Common Issues & Quick Fixes

### Issue: "Database does not exist"
```bash
# Run this first:
.\mysql.exe -u root -e "CREATE DATABASE vinixport;"
# Then import schema.sql
```

### Issue: "Foreign key constraint fails"
Make sure you:
1. ✅ Import schema.sql FIRST
2. ✅ Import sample_data.sql SECOND
3. ✅ Import add_sample_projects.sql THIRD

(Order matters because of foreign keys!)

### Issue: "Login fails with Invalid credentials"
Check:
1. Email harus exactly: `john.doe@example.com`
2. Password harus exactly: `password123`
3. Verify user exists:
   ```bash
   mysql -u root vinixport -e "SELECT * FROM users WHERE email='john.doe@example.com';"
   ```

### Issue: "Portfolio is empty"
Check projects exist:
```bash
mysql -u root vinixport -e "SELECT * FROM projects WHERE user_id=3;"
```

If empty, re-import `add_sample_projects.sql`

---

## 🎯 Next Steps

1. ✅ Run all 5 commands above to restore database
2. ✅ Run verify command to check data
3. ✅ Test login dengan salah satu user account
4. ✅ Explore portfolio, projects, certificates
5. ✅ Test mentor features (if applicable)
6. 🔄 Read `USER_DATA_RESTORE.md` untuk detail lebih lanjut

---

## 📞 Support

If you encounter any issues:

1. Check the error message carefully
2. Verify MySQL is running: `.\mysql.exe -u root -e "SELECT 1;"`
3. Check database exists: `.\mysql.exe -u root -e "SHOW DATABASES;"`
4. Read `DATA_ISSUE_EXPLANATION.md` untuk context
5. Review `USER_DATA_RESTORE.md` untuk troubleshooting section

---

**Catatan Penting:**
- Semua commands di atas menggunakan PowerShell (Windows)
- Untuk Linux/Mac, ganti `.\mysql.exe` dengan `mysql`
- Database credentials: user=`root`, password=`` (empty)
