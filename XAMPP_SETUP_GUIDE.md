# Panduan Lengkap untuk Menjalankan Sistem dengan Database XAMPP

## Prasyarat Sistem
- XAMPP (dengan Apache dan MySQL)
- Node.js dan npm
- Git (opsional)

## Langkah 1: Menyiapkan Database (XAMPP)

### A. Jalankan XAMPP
1. Buka XAMPP Control Panel
2. Start Apache dan MySQL
3. Pastikan status kedua layanan menunjukkan "Running"

### B. Siapkan Database di phpMyAdmin
1. Buka browser dan akses http://localhost/phpmyadmin
2. Pada panel kiri, klik "New" untuk membuat database baru
3. Masukkan nama database: `vinixport`
4. Klik "Create"

### C. Import Schema Database
1. Di halaman database `vinixport`, klik tab "SQL"
2. Copy dan paste isi dari file `backend/schema.sql`
3. Klik "Go" untuk mengeksekusi

### D. (Opsional) Import Data Contoh
1. Di tab yang sama, paste isi dari file `backend/sample_data.sql`
2. Klik "Go" untuk mengeksekusi

## Langkah 2: Menyiapkan Backend Server

### A. Install Dependencies
```bash
cd backend
npm install
```

### B. Konfigurasi Environment
File `.env` sudah disiapkan dengan konfigurasi default:
```
DB_HOST=localhost
DB_USER=root
DB_PASS=  # Kosongkan jika menggunakan XAMPP default
DB_NAME=vinixport
PORT=5000
```

### C. Jalankan Server Backend
```bash
npm start
```
Server seharusnya berjalan di http://localhost:5000

## Langkah 3: Menyiapkan Frontend

### A. Install Dependencies
```bash
npm install
```

### B. Jalankan Aplikasi
```bash
npm run dev
```

## Uji Fungsionalitas

### A. Uji Koneksi Database
Anda bisa menjalankan skrip uji:
```bash
node backend/test_db_connection.js
```

### B. Uji Fungsionalitas Pengiriman Review
1. Register atau login sebagai mahasiswa
2. Masuk ke halaman portfolio
3. Klik tombol "Review Sekarang"
4. Isi form permintaan review
5. Klik "Review Sekarang"

## Troubleshooting Umum

### 1. "Gagal mengirim permintaan review. Coba lagi."
**Penyebab kemungkinan:**
- MySQL tidak berjalan
- Database `vinixport` belum dibuat
- Schema belum diimport
- Backend server tidak berjalan

**Solusi:**
- Pastikan XAMPP MySQL berjalan
- Pastikan database dan tabel dibuat dengan benar
- Pastikan backend server berjalan

### 2. Error Koneksi ke Database
**Penyebab kemungkinan:**
- Konfigurasi database tidak benar
- Port MySQL berbeda dari default

**Solusi:**
- Periksa file `.env` untuk konfigurasi yang benar
- Pastikan port MySQL adalah 3306 (default)

### 3. Tabel Tidak Ditemukan
**Penyebab kemungkinan:**
- Schema database belum diimport

**Solusi:**
- Import file `backend/schema.sql` ke database `vinixport`

## Cek Status Sistem

### 1. Cek apakah MySQL berjalan
- Di XAMPP Control Panel, status MySQL harus "Running"

### 2. Cek apakah database siap
- Di phpMyAdmin, database `vinixport` harus ada
- Tabel-tabel berikut harus ada:
  - users
  - projects
  - certificates
  - skills
  - review_requests
  - assessment_questions

### 3. Cek apakah backend server aktif
- Akses http://localhost:5000 (seharusnya ada response)
- Server harus menunjukkan "Connected to MySQL database" di console

### 4. Cek apakah frontend bisa akses backend
- Di browser developer tools (F12), cek tab Network
- Lihat apakah permintaan ke localhost:5000 berhasil

## Verifikasi Data Tersimpan

Setelah mengirim permintaan review:
1. Buka phpMyAdmin
2. Buka database vinixport
3. Pilih tabel review_requests
4. Klik tab Browse
5. Anda seharusnya melihat data permintaan review baru

Dengan mengikuti panduan ini, sistem pengiriman dan tampilan permintaan review portofolio diharapkan dapat berjalan dengan baik.