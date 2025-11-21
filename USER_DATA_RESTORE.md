# User Data Recovery & Login Guide

## Status Database Restore ✅

Database telah berhasil di-restore dengan sample data yang lengkap.

### Data yang Tersedia:
- **6 Users** (5 regular users + 1 mentor)
- **7 Projects** (portofolio dari users)
- **7 Certificates** (sertifikat dari users)
- **5 Review Requests** (permintaan review ke mentor)
- **Assessment Skills** (untuk setiap user)

---

## User Credentials untuk Login

Semua user menggunakan password yang sama: **`password123`**

### Available Users:

| No | Name | Email | Role | Status |
|----|------|-------|------|--------|
| 1 | John Doe | `john.doe@example.com` | User | Regular user dengan 2 projects |
| 2 | Sarah Wilson | `sarah.w@example.com` | User | Designer dengan 2 projects |
| 3 | Michael Chen | `michael.c@example.com` | User | Full Stack Developer dengan 1 project |
| 4 | Emma Rodriguez | `emma.r@example.com` | User | Data Scientist dengan 1 project |
| 5 | Mentor Expert | `mentor@vinixport.com` | Mentor | Mentor untuk review portfolio |
| 6 | Ade Saputra | `ade.saputra@example.com` | User | Junior Developer dengan 1 project |

---

## Testing Login

### Step 1: Start Backend Server
```bash
cd backend
npm start
```
Server akan berjalan di `http://localhost:5000`

### Step 2: Start Frontend Dev Server
```bash
npm run dev
```
Frontend akan berjalan di `http://localhost:5173`

### Step 3: Login dengan Salah Satu Akun

**Option A: Regular User Login**
1. Go to `http://localhost:5173`
2. Click "Login" atau navigate to login page
3. Enter email: `john.doe@example.com`
4. Enter password: `password123`
5. Click "Sign In"

**Expected Results:**
✅ Successfully logged in
✅ Redirected to Portfolio page
✅ Can see all projects dan certificates
✅ Can see assessment scores
✅ Can upload new projects/certificates

**Option B: Mentor Login**
1. Go to login page
2. Enter email: `mentor@vinixport.com`
3. Enter password: `password123`
4. Click "Sign In" (atau jika ada mentor-specific button, gunakan itu)

**Expected Results:**
✅ Successfully logged in as mentor
✅ Redirected to Mentor Dashboard
✅ Can see list of review requests
✅ Can approve payments dan complete reviews

---

## User Profiles dengan Data Lengkap

### John Doe (john.doe@example.com)
- **Role**: User (Regular)
- **Title**: Frontend Developer
- **Bio**: Pengembang web dengan fokus pada antarmuka pengguna dan pengalaman interaktif.
- **Projects**: 
  - E-commerce Dashboard
  - Mobile Banking App
- **Certificates**:
  - React Advanced Certification
  - UI/UX Design Professional
- **Skills**: React, TypeScript, CSS/SCSS, UI/UX Design
- **Review Status**: 1 completed review with feedback from mentor

### Sarah Wilson (sarah.w@example.com)
- **Role**: User (Regular)
- **Title**: UX Designer
- **Bio**: Spesialis desain pengalaman pengguna dengan pendekatan berbasis data dan empati.
- **Projects**:
  - Travel Booking Platform
  - Fitness Tracker UI
- **Certificates**:
  - Figma Design Expert
- **Skills**: UI/UX Design, Figma, Prototyping, User Research
- **Review Status**: 1 in-progress review

### Michael Chen (michael.c@example.com)
- **Role**: User (Regular)
- **Title**: Full Stack Developer
- **Bio**: Pengembang dengan keahlian di backend dan frontend, suka memecahkan masalah kompleks.
- **Projects**:
  - Inventory Management
- **Certificates**:
  - Node.js Backend Development
- **Skills**: JavaScript, Node.js, Express, MongoDB
- **Review Status**: 1 completed review with feedback from mentor

### Emma Rodriguez (emma.r@example.com)
- **Role**: User (Regular)
- **Title**: Data Scientist
- **Bio**: Ilmuwan data dengan fokus pada machine learning dan analisis prediktif.
- **Projects**:
  - Predictive Analytics Tool
- **Certificates**:
  - Machine Learning Specialization
  - Python for Data Science
- **Skills**: Python, Machine Learning, Data Analysis, TensorFlow
- **Review Status**: 1 pending review (waiting verification)

### Mentor Expert (mentor@vinixport.com)
- **Role**: Mentor
- **Title**: Career Mentor
- **Bio**: Senior industry mentor untuk VinixPort dengan lebih dari 10 tahun pengalaman.
- **Capabilities**:
  - View all portfolio review requests
  - Approve/Reject payments
  - Complete reviews dan provide feedback
  - Manage mentor dashboard

### Ade Saputra (ade.saputra@example.com)
- **Role**: User (Regular)
- **Title**: Junior Developer
- **Bio**: Pemula dalam pengembangan perangkat lunak dengan semangat belajar tinggi.
- **Projects**:
  - Personal Portfolio Website
- **Certificates**:
  - JavaScript Basics
- **Skills**: JavaScript, HTML, CSS (Beginner level)
- **Review Status**: 1 pending review

---

## Database Schema Verification

Verify bahwa semua tabel ada dengan data yang lengkap:

```bash
# Check all tables
mysql -u root vinixport -e "SHOW TABLES;"

# Check user data
mysql -u root vinixport -e "SELECT id, name, email, role FROM users;"

# Check projects per user
mysql -u root vinixport -e "SELECT u.name, COUNT(p.id) as project_count FROM users u LEFT JOIN projects p ON u.id = p.user_id GROUP BY u.id, u.name;"

# Check certificates per user
mysql -u root vinixport -e "SELECT u.name, COUNT(c.id) as cert_count FROM users u LEFT JOIN certificates c ON u.id = c.user_id GROUP BY u.id, u.name;"

# Check review requests
mysql -u root vinixport -e "SELECT menteeName, status, paymentStatus FROM review_requests;"
```

---

## Troubleshooting

### Problem: "Invalid credentials" saat login
**Solution:**
1. Verify email address (case-sensitive di database)
2. Password harus exactly: `password123`
3. Check di database:
   ```bash
   mysql -u root vinixport -e "SELECT id, name, email FROM users WHERE email='john.doe@example.com';"
   ```
4. Jika user tidak ada, re-import sample data

### Problem: Login berhasil tapi portfolio kosong
**Solution:**
1. Check apakah projects dan certificates sudah di-import:
   ```bash
   mysql -u root vinixport -e "SELECT * FROM projects LIMIT 5;"
   ```
2. Verify user_id di projects match dengan user yang login
3. Refresh browser page
4. Check browser console untuk errors

### Problem: Mentor cannot see review requests
**Solution:**
1. Verify user role adalah 'mentor':
   ```bash
   mysql -u root vinixport -e "SELECT name, role FROM users WHERE email='mentor@vinixport.com';"
   ```
2. Check review_requests table:
   ```bash
   mysql -u root vinixport -e "SELECT * FROM review_requests;"
   ```
3. Verify backend is running and accessible

### Problem: Cannot see assessment skills
**Solution:**
1. Assessment questions should be auto-created dari schema
2. Skills untuk each user perlu ditambahkan (ini sudah dilakukan via add_sample_projects.sql)
3. Check if skills table has data:
   ```bash
   mysql -u root vinixport -e "SELECT * FROM skills LIMIT 10;"
   ```

---

## Files Modified/Created

1. **sample_data.sql** - Updated dengan password yang lengkap untuk semua user
2. **add_sample_projects.sql** - New file dengan projects, certificates, skills, review requests
3. **USER_DATA_RESTORE.md** - This documentation file

---

## Next Steps

1. ✅ Database restored dengan sample data
2. ✅ All users memiliki password dan bisa login
3. ✅ Each user memiliki projects dan certificates
4. ✅ Review requests sudah setup
5. 🔄 Test login dengan semua user
6. 🔄 Verify portfolio display
7. 🔄 Test mentor dashboard dan review features
8. 🔄 Test upload new projects/certificates

---

## Command Reference

```bash
# Check backend connection
curl http://localhost:5000/api/debug/status

# Test login API
curl -X POST http://localhost:5000/api/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john.doe@example.com","password":"password123"}'

# Get all users
mysql -u root vinixport -e "SELECT * FROM users \G"

# Count data per user
mysql -u root vinixport -e "SELECT u.id, u.name, COUNT(DISTINCT p.id) as projects, COUNT(DISTINCT c.id) as certs FROM users u LEFT JOIN projects p ON u.id=p.user_id LEFT JOIN certificates c ON u.id=c.user_id GROUP BY u.id, u.name;"
```

---

## Notes

- Password untuk testing: `password123` (same untuk semua user)
- Mentor email: `mentor@vinixport.com`
- Semua user data sudah lengkap dengan projects, certificates, dan skills
- Review requests sudah tersedia untuk testing mentor features
