# Upload Project & Certificate Fix Documentation

## Masalah yang Diperbaiki

### Problem
Upload project dan sertifikat tidak tampil di dalam portfolio user setelah redirect ke halaman portfolio.

### Root Cause
1. **Async/Await tidak digunakan** - UploadPage memanggil `addProject` dan `addCertificate` tanpa menunggu selesai
2. **Dependency Array tidak tepat** - PortfolioContext tidak track perubahan user ID dengan benar
3. **Error handling kurang** - Tidak ada logging untuk debugging

## Solusi yang Diterapkan

### 1. UploadPage.tsx
**File**: `pages/UploadPage.tsx`

```typescript
// SEBELUM (tidak menunggu):
addProject({...});
setUploadStatus(`Successfully uploaded! Redirecting...`);

// SESUDAH (dengan await):
await addProject({...});
setUploadStatus(`Successfully uploaded! Redirecting...`);
```

**Penjelasan**: Dengan `await`, aplikasi akan menunggu sampai data berhasil ditambahkan ke state sebelum redirect ke halaman portfolio.

### 2. PortfolioContext.tsx
**File**: `contexts/PortfolioContext.tsx`

#### Perbaikan A: Dependency Array
```typescript
// SEBELUM:
useEffect(() => {
    if (isAuthenticated) {
        const userId = (user as any)?.id || 1;
        // ... load data
    }
}, [isAuthenticated]); // ❌ Tidak track user ID changes

// SESUDAH:
useEffect(() => {
    if (isAuthenticated && user) {
        const userId = (user as any)?.id || 1;
        console.log('Loading portfolio for user:', userId);
        // ... load data
    }
}, [isAuthenticated, user]); // ✅ Track both auth status dan user
```

**Penjelasan**: Dengan menambahkan `user` ke dependency array, useEffect akan dijalankan kembali jika user berubah, memastikan portfolio yang benar dimuat.

#### Perbaikan B: Error Handling
```typescript
// SEBELUM:
try {
    const newProject = await api.addProject(userId, project);
    setProjects(prev => [newProject, ...prev]);
} catch (err) {
    console.error("Error adding project", err);
} // ❌ Error tidak di-throw

// SESUDAH:
try {
    const newProject = await api.addProject(userId, project);
    console.log('Project added response:', newProject);
    setProjects(prev => [newProject, ...prev]);
} catch (err) {
    console.error("Error adding project", err);
    throw err; // ✅ Re-throw untuk ditangani di UploadPage
}
```

**Penjelasan**: Dengan me-throw error, UploadPage bisa tahu apakah upload gagal dan menampilkan pesan error yang sesuai.

### 3. api.ts
**File**: `services/api.ts`

Ditambahkan implementasi lengkap untuk `updateProject` dan `updateCertificate`:

```typescript
updateCertificate: async (cert: Certificate) => {
    if(!USE_BACKEND) {
        const certs = getStorage<Certificate[]>(KEYS.CERTS, mockCertificates);
        const updatedCerts = certs.map(c => c.id === cert.id ? cert : c);
        setStorage(KEYS.CERTS, updatedCerts);
        return;
    }
    return request(`/certificates/${cert.id}`, {
        method: 'PUT',
        body: JSON.stringify(cert)
    });
},
```

## Alur Kerja yang Benar Setelah Fix

1. **User di Upload Page**
   - Select file image/gambar
   - Isi form (title, description, link, tags, dll)
   - Click "Publish Project" atau "Save Certificate"

2. **UploadPage Processing**
   ```
   handleSubmit() 
   → convertToBase64(file)
   → await addProject() atau await addCertificate()
   → Wait untuk API response
   → Update state di PortfolioContext
   → Show success message
   → Redirect ke /portfolio
   ```

3. **PortfolioContext Reaction**
   ```
   addProject/addCertificate dijalankan
   → State updated: setProjects atau setCertificates
   → Component re-render dengan data baru
   → Portfolio page langsung menampilkan project/cert yang baru diupload
   ```

4. **Portfolio Page Shows Latest Data**
   - Karena PortfolioContext track perubahan, data baru langsung muncul
   - Tidak perlu manual refresh

## Testing Checklist

- [ ] Login sebagai user
- [ ] Go to Upload page
- [ ] Upload project baru
  - [ ] Select image
  - [ ] Fill form (title, description, link, tags)
  - [ ] Click "Publish Project"
  - [ ] Check browser console untuk logs yang benar
  - [ ] Setelah redirect, project baru harus tampil di Portfolio page
- [ ] Upload certificate baru
  - [ ] Select image
  - [ ] Fill form (title, issuer, date)
  - [ ] Click "Save Certificate"
  - [ ] Check browser console untuk logs yang benar
  - [ ] Setelah redirect, certificate baru harus tampil di Portfolio page

## Debug Logs yang Akan Muncul

Saat upload berhasil, Anda akan melihat di browser console:
```
Adding project for user: 1 {title: "...", description: "...", ...}
Project added response: {id: 1234567890, ...}
Project added successfully
```

Atau untuk certificate:
```
Adding certificate for user: 1 {title: "...", issuer: "...", ...}
Certificate added response: {id: 1234567890, ...}
Certificate added successfully
```

## Jika Upload Masih Tidak Bekerja

### Step 1: Check Browser Console
- Open DevTools (F12)
- Go to Console tab
- Lihat error messages yang muncul
- Catat exact error message

### Step 2: Verify User ID
Check di console:
```javascript
JSON.parse(localStorage.getItem('vinix_user'))
// Harus ada property "id" dengan value number
```

### Step 3: Check Network Tab
- Go to Network tab
- Upload project
- Lihat request ke `/api/projects`
- Verify response status adalah 201 (created)

### Step 4: Verify Local Storage
Saat menggunakan mock API (USE_BACKEND = false):
```javascript
JSON.parse(localStorage.getItem('vinix_projects'))
// Harus ada array dengan project baru yang diupload
```

## File yang Dimodifikasi

1. `pages/UploadPage.tsx` - Tambah await untuk async operations
2. `contexts/PortfolioContext.tsx` - Fix dependency array, tambah logging & error handling
3. `services/api.ts` - Lengkapi updateCertificate implementation

## Commit Message untuk Git

```
fix: ensure uploaded projects and certificates display in portfolio

- Add await statements in UploadPage to wait for async operations
- Fix PortfolioContext dependency array to track user ID changes
- Add console logging for debugging
- Complete updateCertificate implementation in API service
- This ensures data is properly saved before page redirect
```
