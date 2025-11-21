# Proses Penyimpanan dan Tampilan Data Review Portofolio

## Gambaran Umum
Dokumen ini menjelaskan bagaimana permintaan review portofolio dari mahasiswa disimpan di database dan kemudian ditampilkan di dashboard mentor.

## Urutan Proses

### 1. Pengiriman Permintaan Review oleh Mahasiswa
1. Mahasiswa mengisi form permintaan review di halaman portfolio
2. Form mencakup:
   - Nama mahasiswa (dari profil)
   - Email mahasiswa (dari profil)
   - URL portfolio
   - Catatan tambahan (opsional)
   - Detail pembayaran (nominal, bank, nama akun, bukti transfer)

### 2. Proses di Frontend (PortfolioPage.tsx)
1. Fungsi `handleRequestReview()` dipanggil
2. Data dikumpulkan dari form dan state
3. API `api.createReviewRequest()` dipanggil dengan data berikut:
   ```
   {
     menteeName: user.name,
     menteeEmail: user.email,
     portfolioUrl: window.location.origin + '/#/portfolio',
     notes: paymentNotes,
     paymentAmount: Number(paymentAmount),
     paymentBank,
     paymentAccountName,
     paymentProofImage
   }
   ```

### 3. Proses di Backend (server.js)
1. Endpoint `POST /api/review-requests` menerima data
2. Middleware otentikasi memverifikasi user ID dari header `x-user-id`
3. Sistem memverifikasi bahwa email cocok dengan ID pengguna
4. Data disimpan ke tabel `review_requests` di database MySQL
5. Respons dikembalikan ke frontend

### 4. Struktur Data di Database
Tabel `review_requests` akan berisi:
```
id (AUTO_INCREMENT)
mentee_id (ID pengguna dari tabel users)
mentee_name (Nama pengguna)
mentee_email (Email pengguna)
portfolio_url (URL portfolio yang direview)
notes (Catatan dari mahasiswa)
payment_amount (Nominal pembayaran)
payment_bank (Bank pengirim)
payment_account_name (Nama akun pengirim)
payment_proof_image (URL bukti pembayaran)
payment_status (waiting_verification/approved/rejected)
status (pending/in_progress/completed)
mentor_feedback (Feedback dari mentor - kosong saat awal)
created_at (Timestamp pembuatan)
updated_at (Timestamp update terakhir)
```

### 5. Tampilan di Dashboard Mentor
1. Mentor mengakses `/mentor` atau mengklik "Mentor Dashboard"
2. MentorDashboard.tsx memanggil `api.getReviewRequests()`
3. Backend endpoint `GET /api/review-requests`:
   - Memverifikasi bahwa pengguna adalah mentor (berdasarkan role di tabel users)
   - Jika mentor, kembalikan SEMUA permintaan review
   - Format data sesuai dengan interface ReviewRequest
4. Data ditampilkan dalam daftar dengan informasi lengkap:
   - Nama dan email mahasiswa
   - Tanggal permintaan
   - Catatan dari mahasiswa
   - Status pembayaran
   - Status review
   - Tombol untuk melihat portfolio dan memberikan feedback

## Contoh Alur Data

### Contoh Data Saat Pengiriman:
```
{
  "menteeName": "John Doe",
  "menteeEmail": "john.doe@example.com",
  "portfolioUrl": "http://localhost:5173/#/portfolio",
  "notes": "Mohon feedback tentang desain UI saya",
  "paymentAmount": 150000,
  "paymentBank": "BCA",
  "paymentAccountName": "John Doe",
  "paymentProofImage": "data:image/jpeg;base64,..."
}
```

### Data Tersimpan di Database:
```
INSERT INTO review_requests (
  mentee_id, mentee_name, mentee_email, portfolio_url, notes,
  payment_amount, payment_bank, payment_account_name, payment_proof_image,
  payment_status, status
) VALUES (
  1, 'John Doe', 'john.doe@example.com', 'http://localhost:5173/#/portfolio', 
  'Mohon feedback tentang desain UI saya', 150000, 'BCA', 'John Doe', 
  'data:image/jpeg;base64,...', 'waiting_verification', 'pending'
);
```

### Tampilan di Dashboard Mentor:
- Nama: Mahasiswa: John Doe
- Email: john.doe@example.com
- Catatan: "Mohon feedback tentang desain UI saya"
- Status Pembayaran: Menunggu Verifikasi
- Status Review: Pending
- Tombol: Lihat Portfolio, Setujui Pembayaran, Tandai Selesai & Beri Feedback

## Validasi dan Keamanan
1. Hanya pengguna terotentikasi yang bisa membuat permintaan review
2. Hanya mentor yang bisa melihat semua permintaan review
3. Mentor adalah satu-satunya yang bisa memperbarui status permintaan
4. Data pengguna dipastikan valid melalui verifikasi ID dan email