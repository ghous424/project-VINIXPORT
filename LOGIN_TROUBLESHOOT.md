# Panduan Memperbaiki Masalah Login

## Masalah Umum dan Solusi

### 1. Backend Server Tidak Berjalan
**Gejala:** Tidak bisa login karena koneksi ke server gagal
**Solusi:**
- Pastikan XAMPP (MySQL) sudah berjalan
- Pastikan Node.js sudah terinstal
- Jalankan server backend:
  ```bash
  cd backend
  npm install
  npm start
  ```
- Server seharusnya berjalan di http://localhost:5000

### 2. Database Belum Diimpor
**Gejala:** Login gagal karena pengguna tidak ditemukan
**Solusi:**
- Pastikan database `vinixport` sudah dibuat di MySQL
- Import skema database:
  ```sql
  -- Import backend/schema.sql ke database vinixport
  ```
- (Opsional) Import data contoh:
  ```sql
  -- Import backend/sample_data.sql ke database vinixport
  ```

### 3. Konfigurasi Frontend Belum Benar
**Gejala:** Frontend tetap menggunakan mode mock
**Solusi:**
- Pastikan file `.env` berisi konfigurasi yang benar:
  ```
  DB_HOST=localhost
  DB_USER=root
  DB_PASS=
  DB_NAME=vinixport
  PORT=5000
  ```
- Pastikan `USE_BACKEND` di `services/api.ts` sudah diubah menjadi `true`

### 4. Masalah Koneksi Database
**Gejala:** Error koneksi ke database
**Solusi:**
- Pastikan MySQL di XAMPP berjalan
- Cek konfigurasi database di `.env`
- Jika menggunakan password pada MySQL, pastikan diisi di `.env`:
  ```
  DB_PASS=katasandi
  ```

## Login untuk Pengguna Biasa
- Jika Anda telah mengimpor data contoh, Anda bisa login dengan:
  - Email: `john.doe@example.com`
  - Password: (bebas, karena sistem demo ini tidak memvalidasi password)
  
## Login untuk Mentor
- Email: `mentor@vinixport.com`
- Password: `mentor123`

## Uji Login
1. Pastikan server backend berjalan (port 5000)
2. Jalankan frontend (port 5173 biasanya)
3. Buka halaman login di frontend
4. Coba login dengan akun contoh

## Troubleshooting Tambahan

### Jika muncul error CORS
Pastikan server backend mengizinkan permintaan dari frontend dengan menambahkan middleware CORS:
```javascript
app.use(cors({
  origin: ["http://localhost:5173", "http://localhost:3000"], // Sesuaikan dengan port frontend
  credentials: true
}));
```

### Jika login tetap tidak berfungsi
1. Periksa konsol browser untuk error network
2. Periksa terminal server backend untuk error
3. Pastikan semua dependensi telah diinstall
4. Coba restart XAMPP dan server backend

## Untuk Pengembangan Lebih Lanjut
Sistem ini masih memerlukan perbaikan untuk produksi:
- Implementasi validasi password yang benar
- Implementasi sistem otentikasi JWT
- Penanganan error yang lebih baik
- Validasi input dari form