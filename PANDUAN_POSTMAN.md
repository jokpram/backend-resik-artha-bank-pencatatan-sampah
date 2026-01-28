# Panduan Pengujian API Resik Artha - Postman

Dokumen ini berisi langkah-langkah lengkap untuk menguji endpoint backend Resik Artha menggunakan Postman.

## Informasi Dasar
- **Base URL:** `http://localhost:9000/api`
- **Content-Type:** `application/json` (Kecuali untuk upload gambar menggunakan `multipart/form-data`)

---

## 1. Otentikasi (Auth)

### Register Nasabah Baru
- **Endpoint:** `POST /auth/register`
- **Body (JSON):**
```json
{
    "nama": "Nasabah Test",
    "email": "nasabah@test.com",
    "password": "password123",
    "no_hp": "081234567890",
    "alamat": "Jl. Testing No. 123"
}
```

### Login (Nasabah / Petugas)
- **Endpoint:** `POST /auth/login`
- **Body (JSON):**
```json
{
    "email": "nasabah@test.com",
    "password": "password123"
}
```
- **Catatan:** Simpan nilai `token` dari response untuk digunakan pada request berikutnya di bagian **Authorization (Bearer Token)**.

### Cek Profile Saya (Me)
- **Endpoint:** `GET /auth/me`
- **Auth:** Bearer Token

---

## 2. Pengelolaan Sampah (Waste)

### Lihat Jenis Sampah & Harga
- **Endpoint:** `GET /waste/types`
- **Auth:** Bearer Token
- **Tujuan:** Ambil `id` jenis sampah untuk digunakan saat setor sampah.

### Setor Sampah (Nasabah)
- **Endpoint:** `POST /waste`
- **Auth:** Bearer Token
- **Method:** POST
- **Body (form-data):**
  - `id_jenis`: (ID dari Jenis Sampah)
  - `berat`: (Angka, contoh: 2.5)
  - `foto`: (File Gambar - pilih tipe 'File' di Postman)

### Lihat Riwayat Sampah Saya
- **Endpoint:** `GET /waste`
- **Auth:** Bearer Token
- **Tujuan:** Sebagai nasabah, melihat riwayat setoran sendiri.

### Validasi Sampah (Petugas/Officer)
- **Endpoint:** `PATCH /waste/:id/validate`
- **Auth:** Bearer Token (Officer)
- **Tujuan:** Mengubah status sampah menjadi 'valid' dan menambah saldo nasabah.

---

## 3. Penarikan Saldo (Withdrawal)

### Ajukan Penarikan (Nasabah)
- **Endpoint:** `POST /withdrawals`
- **Auth:** Bearer Token
- **Body (JSON):**
```json
{
    "jumlah": 5000
}
```

### Validasi Penarikan (Petugas/Officer)
- **Endpoint:** `PATCH /withdrawals/:id/validate`
- **Auth:** Bearer Token (Officer)

---

## 4. Statistik & Leaderboard

### Lihat Statistik Saya (Nasabah)
- **Endpoint:** `GET /users/stats`
- **Auth:** Bearer Token

### Lihat Leaderboard
- **Endpoint:** `GET /users/leaderboard`
- **Auth:** Bearer Token

---

## Tips Postman
1. **Environment Variables:** Disarankan membuat environment di Postman dengan variabel `baseUrl` and `token`.
2. **Authorization Tab:** Untuk setiap request yang butuh login, buka tab **Authorization**, pilih **Auth Type: Bearer Token**, dan masukkan token hasil login.
