# Perbaikan Masalah Pengiriman Permintaan Review

## Masalah yang Dihadapi
Saat mengirim permintaan review portofolio, pengguna mengalami error: "Gagal mengirim permintaan review. Coba lagi."

## Penyebab Masalah
1. **Kekurangan otentikasi**: Backend tidak menerima user ID saat permintaan review dibuat
2. **Foreign key constraint**: Kolom `mentee_id` di tabel `review_requests` adalah foreign key ke tabel `users`, tapi kita menggunakan placeholder (ID=1) yang mungkin tidak valid
3. **Kekurangan validasi**: Tidak ada verifikasi bahwa pengguna yang membuat permintaan adalah pengguna yang sah

## Solusi yang Diterapkan

### 1. Penambahan Middleware Otentikasi
- Dibuat fungsi `getAuthenticatedUserId()` untuk mendapatkan ID pengguna dari header
- Header yang digunakan: `x-user-id`

### 2. Perbaikan Endpoint POST /api/review-requests
- Sekarang memverifikasi bahwa user terotentikasi
- Memeriksa bahwa data pengguna (email) cocok dengan ID yang dikirim
- Menggunakan ID pengguna yang valid saat membuat permintaan review

### 3. Modifikasi Fungsi Request di Frontend
- Memperbarui fungsi `request()` di `services/api.ts` untuk menyertakan user ID di header
- Fungsi ini mengambil user ID dari localStorage

### 4. Perbaikan Endpoint Lainnya
- GET /api/review-requests: Pengguna biasa hanya bisa melihat review mereka sendiri, mentor bisa melihat semua
- PUT /api/review-requests: Hanya mentor yang bisa memperbarui status
- PUT /api/users/:id: Pengguna hanya bisa mengupdate profil mereka sendiri

### 5. Penyempurnaan Proses Login/Registrasi
- Memastikan ID pengguna disimpan ke localStorage saat login/register
- Memperbarui AuthContext untuk menyimpan user ke localStorage

## Struktur Data yang Dikirim
Sekarang, saat pengguna mengirim permintaan review, data yang dikirim disertai dengan:
- Header `x-user-id` yang berisi ID pengguna terotentikasi
- Validasi bahwa email pengguna cocok dengan ID yang terdaftar

## Testing yang Dianjurkan
1. Pastikan server backend berjalan
2. Import database schema dan sample data
3. Login sebagai pengguna
4. Kirim permintaan review portofolio
5. Verifikasi bahwa permintaan berhasil disimpan di database
6. Login sebagai mentor dan verifikasi bisa melihat permintaan tersebut

## Catatan Penting
- Dalam implementasi produksi, sebaiknya menggunakan JWT token untuk otentikasi alih-alih header sederhana
- Sistem sekarang lebih aman karena hanya pengguna terotentikasi yang bisa membuat permintaan review
- Mentor memiliki akses terbatas hanya untuk memperbarui status permintaan review