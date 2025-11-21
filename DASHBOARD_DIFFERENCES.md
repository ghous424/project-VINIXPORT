# Perbedaan Tampilan Mentor dan Mahasiswa di Dashboard

## Tampilan Mentor
Ketika pengguna dengan role 'mentor' login ke sistem, mereka akan melihat tampilan dashboard khusus mentor dengan fitur-fitur berikut:

### Header
- Judul: "Mentor Dashboard"
- Subjudul: "Kelola permintaan review portofolio dari mahasiswa dan berikan feedback profesional"
- Indikator status: "Mentor Mode"

### Statistik (3 kolom)
1. **Total Requests**: Jumlah total permintaan review dari semua mahasiswa
2. **Pending Reviews**: Jumlah permintaan review yang menunggu tindakan mentor
3. **Completed Reviews**: Jumlah review yang sudah diberi feedback oleh mentor

### Daftar Permintaan Review
- Menampilkan **semua** permintaan review dari **semua** mahasiswa
- Informasi yang ditampilkan:
  - Nama mahasiswa
  - Email mahasiswa
  - Tanggal permintaan
  - Catatan dari mahasiswa
  - Feedback yang sudah diberikan oleh mentor
- Aksi yang tersedia:
  - Melihat portfolio mahasiswa
  - Menyetujui pembayaran
  - Menandai selesai & memberikan feedback

## Tampilan Mahasiswa
Ketika pengguna dengan role 'user' login ke sistem, mereka akan melihat tampilan dashboard khusus mahasiswa dengan fitur-fitur berikut:

### Header
- Judul: "Portfolio Analytics"
- Subjudul: "Ringkasan performa dan distribusi konten portofoliomu"
- Indikator status: "Live Data"

### Statistik (3 kolom)
1. **Total Projects**: Jumlah proyek dalam portofolio pengguna
2. **Total Certificates**: Jumlah sertifikat dalam portofolio pengguna
3. **Review Requests**: Jumlah permintaan review yang diajukan oleh pengguna

### Ringkasan Konten
- Grafik perbandingan jumlah proyek dan sertifikat
- Tidak menampilkan daftar permintaan review dari pengguna lain

## Perbedaan Utama
1. **Data yang Ditampilkan**:
   - Mentor: Melihat semua permintaan review dari semua mahasiswa
   - Mahasiswa: Hanya melihat data pribadi (proyek, sertifikat, dan permintaan review mereka sendiri)

2. **Fungsi dan Akses**:
   - Mentor: Bisa menyetujui pembayaran dan memberikan feedback ke mahasiswa
   - Mahasiswa: Hanya bisa melihat statistik portofolio mereka sendiri

3. **Tampilan dan Layout**:
   - Mentor: Fokus pada manajemen permintaan review
   - Mahasiswa: Fokus pada analitik dan statistik portofolio

## Implementasi Teknis
Perbedaan ini diimplementasikan melalui:
- Kondisi `isMentor` berdasarkan role pengguna
- Filter data berbeda dalam useEffect:
  - Mentor: Melihat semua review requests
  - Mahasiswa: Hanya melihat review requests dengan email mereka sendiri
- Tampilan komponen yang berbeda tergantung role