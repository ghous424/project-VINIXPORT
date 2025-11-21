# Panduan Data Dashboard Mentor vs Mahasiswa

## Perbedaan Data yang Ditampilkan

### Mentor Dashboard (MentorDashboard.tsx)
File: `pages/MentorDashboard.tsx`

**Data yang Ditampilkan:**
- Semua permintaan review dari semua mahasiswa
- Tidak ada filter berdasarkan email pengguna
- Statistik: Total Requests, Pending, In Progress, Completed (dari semua mahasiswa)

**Proses Pengambilan Data:**
```javascript
api.getReviewRequests()
    .then(requests => {
        setReviewRequests(requests);  // Semua permintaan
        setLoading(false);
    })
```

### Analytics Page (AnalyticsPage.tsx)  
File: `pages/AnalyticsPage.tsx`

**Data yang Ditampilkan:**
- Hanya permintaan review yang dikirim oleh pengguna saat ini (berdasarkan email)
- Statistik: Total Projects, Total Certificates, Review Requests (milik pengguna saat ini)

**Proses Pengambilan Data:**
```javascript
api.getReviewRequests()
    .then(requests => {
        const myRequests = requests.filter(req => req.menteeEmail === user.email);
        setReviewRequests(myRequests);  // Hanya permintaan pengguna ini
    })
```

## Pengalihan Otomatis
- Mentor yang mencoba mengakses `/analytics` akan diarahkan ke `/mentor`
- Mahasiswa yang mencoba mengakses `/mentor` akan diarahkan ke `/analytics`

## Cek Akses
- MentorDashboard.tsx memiliki validasi: `if (!user || user?.role !== 'mentor')`
- AnalyticsPage.tsx memiliki validasi: `if (user?.role === 'mentor')`

## Troubleshooting Jika Tampilan Masih Sama

1. **Pastikan server backend berjalan** dan database memiliki data permintaan review dari beberapa mahasiswa
2. **Periksa apakah fungsi api.getReviewRequests() mengembalikan data yang benar**
3. **Verifikasi bahwa role pengguna disimpan dengan benar** di auth context
4. **Pastikan tidak ada cache yang menyebabkan data tidak diperbarui**

## Contoh Data yang Seharusnya Ditampilkan

### Untuk Mentor:
- John Doe (john@example.com) - Permintaan Review
- Sarah Smith (sarah@example.com) - Permintaan Review  
- Mike Johnson (mike@example.com) - Permintaan Review

### Untuk Mahasiswa (misalnya John Doe):
- Hanya permintaan dari John Doe (john@example.com) - jika ada