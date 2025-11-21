# Panduan Debugging Error "Gagal mengirim permintaan review. Coba lagi."

## Langkah-langkah untuk Mengidentifikasi Masalah

### 1. Pastikan Backend Server Berjalan
- Buka terminal/command prompt
- Arahkan ke folder `backend`
- Jalankan: `npm start`
- Pastikan Anda melihat pesan: `Server is running on port 5000`
- Pastikan juga pesan: `Connected to MySQL database`

### 2. Cek Browser Console
- Buka aplikasi di browser
- Tekan F12 untuk membuka Developer Tools
- Buka tab Console
- Coba kirim permintaan review
- Lihat error apa yang muncul di console

### 3. Cek Browser Network Tab
- Tetap di Developer Tools
- Buka tab Network
- Coba kirim permintaan review
- Lihat request ke `http://localhost:5000/api/review-requests`
- Periksa status response dan detail error

### 4. Cek Log Backend
- Di terminal tempat Anda menjalankan backend
- Lihat apakah muncul log saat permintaan datang
- Periksa apakah muncul error log

## Penyebab Umum dan Solusi

### A. Backend Server Tidak Berjalan
- Solusi: Jalankan `npm start` di folder backend

### B. Database Tidak Terhubung
- Gejala: Pesan "Connected to MySQL database" tidak muncul
- Solusi: Pastikan XAMPP (MySQL) berjalan dan konfigurasi di .env benar

### C. CORS Error
- Gejala: Error "Access to fetch at 'http://localhost:5000/...' from origin 'http://localhost:5173' has been blocked by CORS policy"
- Solusi: Pastikan konfigurasi CORS di backend benar

### D. User Tidak Terotentikasi
- Gejala: Error "Authentication required"
- Solusi: Pastikan sudah login dan user ID disimpan di localStorage

### E. Tabel Database Tidak Ada
- Gejala: SQL Error karena tabel tidak ditemukan
- Solusi: Import `backend/schema.sql` ke database

## Cek File Konfigurasi

### Pastikan .env berisi:
```
DB_HOST=localhost
DB_USER=root
DB_PASS=
DB_NAME=vinixport
PORT=5000
```

### Pastikan USE_BACKEND = true di api.ts
- File: `services/api.ts`
- Baris: `const USE_BACKEND = true;`

## Jalankan Pengujian

1. Buka XAMPP dan pastikan Apache & MySQL berjalan
2. Import database menggunakan phpMyAdmin
3. Jalankan backend: `cd backend && npm start`
4. Jalankan frontend: `npm run dev`
5. Buka browser dan cek console (F12)
6. Coba kirim permintaan review
7. Perhatikan error di console

## Jika Masih Bermasalah

File `Form_Input_Database.html` bisa digunakan untuk mengisi data secara langsung ke database tanpa menggunakan aplikasi utama. Ini akan membantu memastikan bahwa database bisa diakses dan struktur tabel benar.

## Endpoint yang Digunakan
- Frontend: Mengirim ke `http://localhost:5000/api/review-requests`
- Backend: Menerima di `POST /api/review-requests` dengan otentikasi x-user-id