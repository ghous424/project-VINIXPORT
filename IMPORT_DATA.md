# Panduan Import Data ke Database VinixPort

## Metode 1: Melalui phpMyAdmin

1. Buka phpMyAdmin di browser (biasanya di http://localhost/phpmyadmin)
2. Pilih database `vinixport` dari daftar database di sebelah kiri
3. Klik tab "SQL" di bagian atas
4. Copy seluruh isi file `backend/sample_data.sql`
5. Paste ke kotak query di phpMyAdmin
6. Klik tombol "Go" untuk mengeksekusi query

## Metode 2: Melalui Command Line MySQL

1. Buka Command Prompt atau Terminal
2. Masuk ke direktori yang berisi file `sample_data.sql`
3. Jalankan perintah berikut:
   ```bash
   mysql -u root -p vinixport < sample_data.sql
   ```
4. Masukkan password MySQL anda (kosongkan jika menggunakan default XAMPP)

## Metode 3: Menggunakan MySQL Workbench atau Tool Lain

1. Buka MySQL Workbench atau tool database yang Anda gunakan
2. Buat koneksi ke database `vinixport`
3. Buka file `backend/sample_data.sql` melalui tool tersebut
4. Eksekusi seluruh query di dalam file

## Data yang Telah Dimasukkan

File `sample_data.sql` berisi:

1. **Users (6 pengguna)**:
   - 5 pengguna biasa (termasuk 1 mentor)
   - 1 pengguna junior developer
   - Termasuk data: nama, email, title, bio, role, dan avatar_url

2. **Projects (7 proyek)**:
   - Berbagai jenis proyek dari pengguna
   - Termasuk: judul, deskripsi, image_url, link, dan tags

3. **Certificates (7 sertifikat)**:
   - Berbagai sertifikasi dari pengguna
   - Termasuk: judul, penerbit, tanggal, dan image_url

4. **Skills (18 keterampilan)**:
   - Keterampilan dari beberapa pengguna
   - Termasuk: nama keterampilan dan level (0-100)

5. **Review Requests (5 permintaan review)**:
   - Permintaan review portofolio dari mahasiswa ke mentor
   - Termasuk berbagai status pembayaran dan review
   - Beberapa sudah memiliki feedback dari mentor

6. **Assessment Questions (18 pertanyaan)**:
   - Termasuk pertanyaan-pertanyaan assessment tambahan
   - Dalam kategori Soft Skills, Digital Skills, dan Workplace Readiness

## Setelah Import Data

Setelah sukses mengimpor data, Anda bisa:

1. Menjalankan backend server:
   ```
   cd backend
   npm start
   ```

2. Menjalankan frontend:
   ```
   npm run dev
   ```

3. Mencoba login dengan salah satu akun contoh:
   - User: john.doe@example.com (tanpa password karena backend belum selesai implementasi)
   - Mentor: mentor@vinixport.com / mentor123

Data contoh ini akan memungkinkan Anda untuk melihat dan menguji fungsi mentor dalam melihat permintaan review dari mahasiswa dan memberikan feedback.