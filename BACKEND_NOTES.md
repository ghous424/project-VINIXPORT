# Catatan Penting untuk Implementasi Backend

## Masalah dengan User ID pada Review Requests

Dalam implementasi awal server, ada beberapa kekurangan penting yang perlu diperbaiki:

### 1. Autentikasi dan User ID
Saat ini, endpoint untuk membuat review request:
```javascript
app.post('/api/review-requests', (req, res) => {
    // ...
    // Using placeholder for mentee_id since we're not authenticating
    const values = [1, menteeName, menteeEmail, portfolioUrl, notes || null, ...];
    // ...
});
```

Ini adalah masalah karena kita menggunakan placeholder ID (1) untuk semua permintaan review, bukan ID pengguna sebenarnya yang login.

### 2. Solusi yang Dibutuhkan

Untuk memperbaiki ini, kita perlu:

1. Implementasi sistem autentikasi yang lengkap (JWT token atau session)
2. Middleware untuk memverifikasi user yang login
3. Memastikan hanya user yang login yang bisa membuat review request

### 3. Perbaikan pada Server.js

Berikut adalah contoh implementasi middleware autentikasi sederhana:

```javascript
const jwt = require('jsonwebtoken');

// Middleware untuk verifikasi token
const authenticateToken = (req, res, next) => {
    const authHeader = req.headers['authorization'];
    const token = authHeader && authHeader.split(' ')[1]; // Bearer TOKEN

    if (!token) {
        return res.status(401).json({ message: 'Access token required' });
    }

    jwt.verify(token, process.env.TOKEN_SECRET || 'default_secret', (err, user) => {
        if (err) {
            return res.status(403).json({ message: 'Invalid or expired token' });
        }
        req.user = user;
        next();
    });
};

// Contoh endpoint yang dilindungi
app.post('/api/review-requests', authenticateToken, (req, res) => {
    const { menteeName, menteeEmail, portfolioUrl, notes, paymentAmount, paymentBank, paymentAccountName, paymentProofImage } = req.body;
    const userId = req.user.id; // ID pengguna dari token
    
    const query = `INSERT INTO review_requests 
                   (mentee_id, mentee_name, mentee_email, portfolio_url, notes, 
                    payment_amount, payment_bank, payment_account_name, payment_proof_image, 
                    payment_status, status) 
                   VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)`;
    
    const values = [userId, menteeName, menteeEmail, portfolioUrl, notes || null,
                    paymentAmount || null, paymentBank || null, paymentAccountName || null, paymentProofImage || null,
                    'waiting_verification', 'pending'];
    
    db.query(query, values, (err, result) => {
        if (err) {
            return res.status(500).json({ message: 'Database error' });
        }
        
        res.status(201).json({ 
            id: result.insertId,
            menteeId: userId, // Sekarang benar mengacu ke user yang login
            mentee_name: menteeName,
            mentee_email: menteeEmail,
            portfolio_url: portfolioUrl,
            // ... dll
        });
    });
});
```

### 4. Implementasi Lengkap yang Dibutuhkan

Untuk implementasi yang lengkap, kita perlu:

1. Install dependensi tambahan:
   ```bash
   npm install jsonwebtoken
   ```

2. Tambahkan TOKEN_SECRET ke .env:
   ```
   TOKEN_SECRET=your-super-secret-jwt-token-here
   ```

3. Implementasi login yang menghasilkan token

4. Middleware untuk semua endpoint yang memerlukan otentikasi

5. Validasi role (user vs mentor) untuk memberi akses yang sesuai

### 5. Perubahan pada Frontend

Frontend juga perlu diperbarui untuk:
- Menyimpan token setelah login
- Mengirim token di header untuk setiap permintaan API
- Menangani error otentikasi

File server.js yang telah kita buat merupakan kerangka awal, dan akan memerlukan perbaikan ini untuk implementasi produksi.