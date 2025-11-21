# Struktur Halaman Baru Aplikasi VinixPort

## Gambaran Umum
Aplikasi sekarang memiliki dua halaman dashboard yang terpisah:
1. **AnalyticsPage** (pages/AnalyticsPage.tsx) - untuk pengguna biasa
2. **MentorDashboard** (pages/MentorDashboard.tsx) - untuk mentor

## Routing
- `/analytics` - Halaman dashboard untuk pengguna biasa
- `/mentor` - Halaman dashboard khusus untuk mentor

## Perubahan pada Navbar
- Mentor akan diarahkan ke `/mentor` saat klik logo
- Mentor melihat link "Mentor Dashboard" yang mengarah ke `/mentor`
- Pengguna biasa tetap ke `/portfolio` saat klik logo dan melihat `/analytics` untuk analytics

## Fitur MentorDashboard yang Baru
1. **Statistik Lengkap**:
   - Total Requests
   - Pending
   - In Progress
   - Completed

2. **Grafik Status Review**:
   - Distribusi status permintaan review
   - Visualisasi berbentuk chart

3. **Filter Status**:
   - Filter permintaan berdasarkan status
   - Tombol untuk melihat all/pending/in progress/completed

4. **Tampilan Detail**:
   - Avatar mahasiswa
   - Informasi pribadi mahasiswa
   - Catatan dan feedback yang lebih terorganisir

## Perbedaan Utama
| MentorDashboard | AnalyticsPage |
|-----------------|---------------|
| Menampilkan semua permintaan review dari semua mahasiswa | Menampilkan statistik portofolio pengguna sendiri |
| Fokus pada manajemen review | Fokus pada analitik portofolio |
| Fungsi untuk menyetujui pembayaran dan memberikan feedback | Hanya menampilkan informasi pengguna sendiri |
| Filter status permintaan | Tidak ada filter, hanya statistik umum |
| Grafik distribusi status | Grafik distribusi proyek & sertifikat |

## Implementasi Teknis
- Dua file komponen terpisah untuk masing-masing role
- Navigasi di Navbar diarahkan berdasarkan role pengguna
- Data yang dimuat berbeda tergantung role
- Tidak ada kondisi `if/else` yang kompleks dalam satu file