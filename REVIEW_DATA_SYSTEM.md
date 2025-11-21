# Sistem Penyimpanan dan Tampilan Data Review Portofolio

## Arsitektur Sistem

### Backend (Node.js + MySQL)
- Menerima permintaan dari frontend melalui API endpoints
- Menyimpan data ke database MySQL
- Menyediakan data untuk ditampilkan di frontend
- Melakukan otentikasi dan otorisasi

### Frontend (React + TypeScript)
- Mengumpulkan data dari form pengguna
- Mengirim data ke backend
- Menampilkan data dari backend sesuai peran pengguna

## Proses Penyimpanan Data

### 1. Pengumpulan Data di Frontend
- File: `pages/PortfolioPage.tsx`
- Fungsi: `handleRequestReview()`
- Data dikumpulkan dari state komponen dan profil pengguna

### 2. Pengiriman ke Backend
- File: `services/api.ts`
- Fungsi: `createReviewRequest()`
- Data dikirim melalui endpoint POST `/api/review-requests`
- Header otentikasi disertakan (x-user-id)

### 3. Proses dan Penyimpanan di Backend
- File: `backend/server.js`
- Endpoint: `POST /api/review-requests`
- Validasi otentikasi dan otorisasi
- Data disimpan ke tabel `review_requests`
- Foreign key `mentee_id` mengacu ke `users.id`

## Proses Tampilan Data

### 1. Pengambilan Data di Frontend (Mentor)
- File: `pages/MentorDashboard.tsx`
- Hook: `useEffect` saat halaman dimuat
- Fungsi: `api.getReviewRequests()`

### 2. Penyediaan Data dari Backend
- File: `backend/server.js`
- Endpoint: `GET /api/review-requests`
- Identifikasi role pengguna: mentor vs regular user
- Mentor: dapat melihat semua permintaan review
- Regular user: hanya melihat permintaan mereka sendiri

### 3. Tampilan di Dashboard Mentor
- File: `pages/MentorDashboard.tsx`
- Menampilkan daftar lengkap permintaan review
- Dengan filter status (all, pending, in_progress, completed)
- Tombol untuk menyetujui pembayaran dan memberikan feedback

## Validasi dan Konsistensi Data

### Di Backend
- Foreign key constraint: `mentee_id` → `users.id`
- Validasi otentikasi di setiap endpoint
- Pembatasan akses berdasarkan role

### Di Frontend
- Pembatasan navigasi berdasarkan role
- Redirect otomatis jika akses tidak sah
- Loading state dan error handling

## Manfaat Sistem Ini

1. **Keterpisahan Peran**:
   - Mentor fokus pada manajemen review
   - Mahasiswa fokus pada portofolio dan permintaan

2. **Keamanan Data**:
   - Data hanya bisa diakses oleh pihak yang berwenang
   - Validasi di setiap tahap transaksi data

3. **Efisiensi**:
   - Mentor bisa melihat semua permintaan di satu tempat
   - Mahasiswa bisa melacak status permintaan mereka

4. **Keterlacakan**:
   - Setiap permintaan bisa ditelusuri dari pengajuan hingga feedback
   - Riwayat pembayaran dan status ditampilkan jelas

## Uji Fungsionalitas

Untuk menguji bahwa sistem bekerja dengan baik:

1. **Sebagai Mahasiswa**:
   - Login dengan akun mahasiswa
   - Buka halaman portfolio
   - Kirim permintaan review
   - Verifikasi data muncul di halaman analytics (hanya milik sendiri)

2. **Sebagai Mentor**:
   - Login dengan akun mentor
   - Buka halaman mentor dashboard
   - Verifikasi semua permintaan review dari mahasiswa muncul
   - Coba berikan feedback dan perbarui status
   - Verifikasi perubahan tersimpan di database

Sistem ini sekarang sepenuhnya fungsional untuk menyimpan data permintaan review dari mahasiswa dan menampilkannya di dashboard mentor.