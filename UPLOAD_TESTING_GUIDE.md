# Testing Guide - Upload Project & Certificate Fix

## Prerequisites
- Backend server running (`npm start` di folder `/backend`)
- Frontend dev server running (`npm run dev` di folder root)
- Browser DevTools Console terbuka (F12)

## Test Case 1: Upload Project

### Steps:
1. Login ke aplikasi dengan akun user
2. Navigate to **Upload Page** (dari navbar)
3. Tab should be set to **"Project"**
4. **Select an image** untuk project cover:
   - Click drag & drop area atau gunakan file picker
   - Pilih file gambar (PNG, JPG, GIF - max 5MB)
5. **Fill Project Details**:
   - Project Title: `React E-Commerce Dashboard`
   - Description: `A modern e-commerce dashboard built with React and TypeScript featuring real-time analytics and inventory management.`
   - Project Link: `https://github.com/yourusername/ecommerce-dashboard`
   - Tags: `React, TypeScript, Tailwind, Analytics`
6. Click **"Publish Project"** button

### Expected Results:
✅ Success message: "Successfully uploaded! Redirecting..."
✅ Browser console shows:
```
Adding project for user: 1 {title: "React E-Commerce Dashboard", ...}
Project added response: {id: 1234567890, title: "React E-Commerce Dashboard", ...}
Project added successfully
```
✅ Redirect ke Portfolio page dalam 1.5 detik
✅ Project baru muncul di bagian **"Featured Projects"** di Portfolio page
✅ Gambar, title, description, link, dan tags semua tampil dengan benar

### If it fails:
❌ Check browser console untuk error message
❌ Verify user ID di localStorage: 
```javascript
JSON.parse(localStorage.getItem('vinix_user')).id
```
❌ Check Network tab untuk request `/api/projects` - status harus 201

---

## Test Case 2: Upload Certificate

### Steps:
1. Dari Portfolio page atau Upload page, navigate ke **Upload Page**
2. Click tab **"Certificate"**
3. **Select an image** untuk certificate:
   - Click drag & drop area atau gunakan file picker
   - Pilih file gambar sertifikat
4. **Fill Certificate Details**:
   - Certificate Title: `AWS Certified Solutions Architect`
   - Issuer Organization: `Amazon Web Services`
   - Date Issued: `2024-01-15` (gunakan date picker)
5. Click **"Save Certificate"** button

### Expected Results:
✅ Success message: "Successfully uploaded! Redirecting..."
✅ Browser console shows:
```
Adding certificate for user: 1 {title: "AWS Certified Solutions Architect", ...}
Certificate added response: {id: 1234567890, title: "AWS Certified Solutions Architect", ...}
Certificate added successfully
```
✅ Redirect ke Portfolio page dalam 1.5 detik
✅ Certificate baru muncul di bagian **"Certificates"** di Portfolio page
✅ Gambar, title, issuer, dan date semua tampil dengan benar

### If it fails:
❌ Check browser console untuk error message
❌ Verify localStorage untuk certificates data:
```javascript
JSON.parse(localStorage.getItem('vinix_certificates'))
```
❌ Check Network tab untuk request `/api/certificates` - status harus 201

---

## Test Case 3: Multiple Uploads

### Steps:
1. Upload 2-3 projects
2. Upload 2-3 certificates
3. Navigate ke Portfolio page
4. Scroll through projects dan certificates sections

### Expected Results:
✅ Semua uploads tampil di portfolio
✅ Data ditampilkan dalam urutan terbaru di atas (LIFO - Last In First Out)
✅ Edit dan Delete buttons berfungsi untuk masing-masing item

---

## Test Case 4: Edit After Upload

### Steps:
1. Setelah upload project/certificate berhasil
2. Di Portfolio page, hover over project/certificate card
3. Click **Edit** button
4. Ubah beberapa field (title, description, dll)
5. Click **Save**

### Expected Results:
✅ Modal edit terbuka
✅ Form di-populate dengan data lama
✅ Changes disimpan
✅ Portfolio card di-update dengan data baru

---

## Test Case 5: Delete After Upload

### Steps:
1. Setelah upload project/certificate berhasil
2. Di Portfolio page, hover over project/certificate card
3. Click **Delete** button
4. Confirm deletion

### Expected Results:
✅ Item dihapus dari portfolio
✅ List projects/certificates di-update
✅ UI refresh tanpa page reload

---

## Test Case 6: Backend vs Mock Mode Testing

### Test dengan Backend (USE_BACKEND = true)

**Prerequisites:**
- Backend running at `http://localhost:5000`
- MySQL database connected
- `vinixport` database dan tabel sudah dibuat

**Steps:**
1. Set `USE_BACKEND = true` di `services/api.ts`
2. Upload project/certificate
3. Check Database directly:
```bash
mysql -u root vinixport -e "SELECT * FROM projects LIMIT 5;"
mysql -u root vinixport -e "SELECT * FROM certificates LIMIT 5;"
```

**Expected Results:**
✅ Data masuk ke database
✅ Portfolio page menampilkan data dari database
✅ Data persist setelah refresh page

### Test dengan Mock Mode (USE_BACKEND = false)

**Steps:**
1. Set `USE_BACKEND = false` di `services/api.ts`
2. Upload project/certificate
3. Check localStorage:
```javascript
JSON.parse(localStorage.getItem('vinix_projects'))
JSON.parse(localStorage.getItem('vinix_certificates'))
```

**Expected Results:**
✅ Data disimpan ke localStorage
✅ Portfolio page menampilkan data dari localStorage
✅ Data persist setelah refresh (jika localStorage clear)

---

## Console Logs Reference

### Successful Upload Flow:
```
Loading portfolio for user: 1
Portfolio data loaded: {projects: [], certificates: []}
Adding project for user: 1 {title: "...", description: "...", ...}
Project added response: {id: 1234567890, title: "...", ...}
Projects state updated: [Object]
Project added successfully
```

### Error Flow:
```
Adding project for user: 1 {title: "...", ...}
Error adding project: Error: HTTP Error: 500 - Internal Server Error
Error processing file. Please try again.
```

---

## Troubleshooting Matrix

| Symptom | Possible Cause | Solution |
|---------|----------------|----------|
| Upload succeeds but item not showing in portfolio | User ID mismatch | Check `localStorage.getItem('vinix_user').id` |
| "Error processing file" appears immediately | File conversion error | Check file size, format (PNG/JPG/GIF) |
| Item shows briefly then disappears | Wrong dependency array | Verify PortfolioContext uses `[isAuthenticated, user]` |
| Upload hangs for >5 seconds | Backend not responding | Check if backend running, verify API_URL in api.ts |
| Data not persisting after refresh | localStorage cleared | Check browser privacy settings |
| Backend returns 401 | User ID not in header | Verify `x-user-id` header in Network tab |

---

## Performance Notes

- **Upload time**: Depends on file size and network
  - Small images (<1MB): <500ms
  - Large images (2-5MB): 1-3 seconds
- **Redirect delay**: Fixed 1500ms after success message
- **State update**: Instant (optimistic update)

---

## Browser Compatibility

Tested on:
- Chrome 120+
- Firefox 121+
- Safari 17+
- Edge 120+

File API (FileReader) digunakan untuk base64 conversion - supported di semua modern browsers.
