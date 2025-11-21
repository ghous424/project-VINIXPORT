# Panduan Setup VinixPort dengan Database MySQL (XAMPP)

## Persyaratan Sistem
- XAMPP (sudah termasuk Apache & MySQL)
- Node.js dan npm
- Git (opsional)

## Instalasi dan Konfigurasi

### 1. Setup Database
1. Jalankan XAMPP Control Panel
2. Start Apache dan MySQL
3. Buka phpMyAdmin (biasanya di http://localhost/phpmyadmin)
4. Buat database baru dengan nama `vinixport`
5. Import file `backend/schema.sql` ke database tersebut

### 2. Setup Backend
1. Buka terminal/command prompt di folder proyek
2. Pindah ke folder backend:
   ```
   cd backend
   ```
3. Install dependensi:
   ```
   npm install
   ```
4. Jalankan server backend:
   ```
   npm start
   ```

### 3. Setup Frontend
1. Di folder utama proyek, install dependensi jika belum:
   ```
   npm install
   ```
2. Jalankan aplikasi frontend:
   ```
   npm run dev
   ```

## Konfigurasi Database
File `.env` sudah disiapkan dengan konfigurasi default:
```
DB_HOST=localhost
DB_USER=root
DB_PASS=  # Kosongkan jika menggunakan XAMPP default
DB_NAME=vinixport
PORT=5000
```

Ubah sesuai kebutuhan jika Anda mengganti pengaturan default XAMPP.

## Struktur Database
Database ini mencakup tabel-tabel berikut:
- `users`: Untuk menyimpan data pengguna dan mentor
- `projects`: Untuk menyimpan proyek portofolio
- `certificates`: Untuk menyimpan sertifikat
- `skills`: Untuk menyimpan data keterampilan
- `review_requests`: Untuk menyimpan permintaan review dari mahasiswa ke mentor
- `assessment_questions`: Untuk menyimpan pertanyaan penilaian

## API Endpoints
Backend menyediakan endpoint berikut:
- POST /api/login - Login pengguna
- POST /api/register - Registrasi pengguna
- POST /api/mentors/login - Login mentor
- GET /api/portfolio/:userId - Ambil data portofolio
- POST/PUT/DELETE /api/projects - Manajemen proyek
- POST/PUT/DELETE /api/certificates - Manajemen sertifikat
- POST/GET/PUT /api/review-requests - Manajemen permintaan review

## Pengujian Fungsionalitas
1. Pastikan server backend berjalan di http://localhost:5000
2. Jalankan frontend
3. Register dan login sebagai pengguna
4. Submit permintaan review portofolio
5. Login sebagai mentor (mentor@vinixport.com / mentor123)
6. Lihat dan proses permintaan review di halaman Mentor Dashboard

## Mengisi Database dengan Data Contoh
Untuk pengujian dan demonstrasi, Anda bisa mengisi database dengan data contoh:

1. Import skema database dari file `backend/schema.sql`
2. Import data contoh dari file `backend/sample_data.sql` (lihat file `IMPORT_DATA.md` untuk petunjuk detail)
3. Data contoh mencakup:
   - 6 pengguna (termasuk 1 mentor)
   - 7 proyek portofolio
   - 7 sertifikat
   - 18 keterampilan
   - 5 permintaan review dari mahasiswa ke mentor
   - 18 pertanyaan assessment

## Catatan Penting
Lihat file `BACKEND_NOTES.md` untuk catatan implementasi penting, khususnya terkait dengan otentikasi dan penanganan ID pengguna di backend. Implementasi yang ada sekarang adalah kerangka dasar dan perlu perbaikan untuk produksi, terutama dalam penanganan otentikasi dan keamanan.

## API Endpoints Saat Ini
Backend menyediakan endpoint berikut:
- POST /api/login - Login pengguna
- POST /api/register - Registrasi pengguna
- POST /api/mentors/login - Login mentor
- GET /api/portfolio/:userId - Ambil data portofolio
- POST/PUT/DELETE /api/projects - Manajemen proyek
- POST/PUT/DELETE /api/certificates - Manajemen sertifikat
- POST/GET/PUT /api/review-requests - Manajemen permintaan review
- PUT /api/users/:id - Memperbarui profil pengguna