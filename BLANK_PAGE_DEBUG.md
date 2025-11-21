# Panduan Mengatasi Halaman Putih Kosong Setelah Login

## Penyebab Umum
1. **User tidak terotentikasi dengan benar**
2. **Port aplikasi yang salah**
3. **Error JavaScript tidak tertangani**
4. **Masalah dengan AuthContext**

## Solusi Langkah-demi-Langkah

### 1. Cek Port yang Digunakan
- Pastikan frontend berjalan di port yang benar (biasanya 5173 bukan 3000)
- Secara default Vite berjalan di http://localhost:5173
- URL yang benar mungkin: `http://localhost:5173/#/portfolio`

### 2. Periksa Browser Console
- Tekan F12 untuk membuka Developer Tools
- Buka tab Console
- Lihat error apa yang muncul saat mengakses halaman

### 3. Periksa Data User di localStorage
- Di Developer Tools, buka tab Application (di Chrome) atau Storage (di Firefox)
- Cek apakah ada item 'vinix_user' di Local Storage
- Jika tidak ada, maka login mungkin tidak berhasil menyimpan data user

### 4. Coba Login Lagi
- Logout dari aplikasi
- Coba login kembali
- Pastikan login berhasil (tidak ada error di console)

### 5. Gunakan File Uji Login
Gunakan file `Test_Login_Function.html` yang telah dibuat untuk:
- Memastikan server backend berjalan
- Mengecek apakah ada user dalam database
- Membuat user demo jika tidak ada
- Menguji fungsi login

### 6. Verifikasi Data User
Pastikan user memiliki data yang lengkap termasuk ID:
- ID user harus disimpan di AuthContext
- User data harus tersimpan di localStorage
- Fungsi otentikasi harus mengembalikan ID pengguna

## Debugging Spesifik

### Jika di Browser Console muncul error:
- Salin error tersebut
- Pastikan backend server berjalan di port 5000
- Pastikan database XAMPP (MySQL) berjalan

### Jika halaman tetap kosong:
- Coba refresh halaman
- Coba clear cache browser
- Coba buka halaman di incognito mode
- Pastikan semua dependensi terinstal (jalankan `npm install`)

## Penanganan Otentikasi yang Ditingkatkan

File `AuthContext.tsx` telah diperbarui untuk:
- Mengambil ID user dari data yang tersimpan di localStorage
- Memastikan user data lengkap sebelum menandai sebagai terotentikasi
- Menyimpan user data ke localStorage setelah login/register/update

## Endpoint yang Dapat Diuji

Setelah login, Anda bisa menguji endpoint berikut:
- http://localhost:5000/api/debug/auth (untuk melihat status otentikasi)
- http://localhost:5000/api/debug/status (untuk melihat status server dan database)

## Jika Masih Bermasalah

1. Pastikan semua server berjalan:
   - XAMPP (MySQL)
   - Backend server (`npm start` di folder backend)
   - Frontend server (`npm run dev`)

2. Pastikan database terisi:
   - Import `backend/schema.sql` ke phpMyAdmin
   - Tambahkan user demo melalui `Test_Login_Function.html`

3. Gunakan file `Form_Input_Database.html` untuk mengisi data secara langsung ke database jika diperlukan

Dengan mengikuti panduan ini, masalah halaman putih kosong setelah login seharusnya dapat diidentifikasi dan diperbaiki.