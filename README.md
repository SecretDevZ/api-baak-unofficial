# API BAAK Universitas Gunadarma (Tidak Resmi)

![Preview API Response](https://raw.githubusercontent.com/SecretDevZ/api-baak-unofficial/main/assets/Screenshoot.webp)

> ⚠️ **Ini bukan layanan resmi dari Universitas Gunadarma.**  
> API ini dibuat oleh pihak ketiga dan tidak memiliki kaitan apa pun dengan BAAK atau Universitas Gunadarma.

API ini membantu Anda mengakses data akademik publik dari situs **BAAK Universitas Gunadarma** (https://baak.gunadarma.ac.id) dalam format JSON yang mudah digunakan. Semua data diambil secara otomatis dari halaman yang **terbuka untuk umum**.

**Gunakan API ini di**:  
https://api.500.xx.kg

---

## Apa Itu BAAK?

Menurut situs resminya:

> BAAK (Biro Administrasi Akademik dan Kemahasiswaan) adalah biro yang menangani segala sesuatu yang berkaitan dengan penyelenggaraan kegiatan belajar mengajar dan administrasi akademik bagi seluruh mahasiswa Universitas Gunadarma.

---

## Fitur yang Tersedia

API ini menyediakan tiga fungsi utama:

---

### 1. Cari Data Mahasiswa — `/cari-maba`

Cari mahasiswa berdasarkan **nama** atau **kelas**.

#### Cara Pakai

Anda perlu mengirim dua informasi lewat URL:

- `teks` → kata kunci pencarian (wajib)
- `tipe` → jenis pencarian (opsional, default: `Nama`)

Nilai yang diizinkan untuk `tipe`:
- `Nama` → cari berdasarkan nama mahasiswa
- `Kelas` → cari semua mahasiswa dalam satu kelas

#### Contoh

```bash
# Cari mahasiswa bernama "Budi"
curl "https://api.500.xx.kg/cari-maba?teks=Budi"

# Cari semua mahasiswa di kelas "1IA01"
curl "https://api.500.xx.kg/cari-maba?teks=1IA01&tipe=Kelas"
```

#### Contoh Hasil

```json
{
  "success": true,
  "results": [
    {
      "no": "1",
      "nopend": "12345",
      "nama": "Budi Santoso",
      "npm": "51423123",
      "kelas": "1IA01",
      "keterangan": "Aktif"
    }
  ]
}
```

> Catatan: API otomatis mengambil semua halaman hasil pencarian, jadi Anda tidak perlu repot klik "Next".

---

### 2. Lihat Jadwal Kuliah — `/cari-jadwal`

Lihat jadwal kuliah lengkap untuk suatu **kode kelas** (misal: `1IA01`).

#### Cara Pakai

Cukup kirim:
- `teks` → kode kelas (wajib)

#### Contoh

```bash
curl "https://api.500.xx.kg/cari-jadwal?teks=1IA01"
```

#### Contoh Hasil

```json
{
  "success": true,
  "data": {
    "kelas": "1IA01",
    "semester": "Semester Ganjil 2024/2025",
    "berlaku_mulai": "02 September 2024",
    "jadwal": [
      {
        "kelas": "1IA01",
        "hari": "Senin",
        "mata_kuliah": "Pemrograman Web",
        "waktu": "07:30 – 09:30",
        "ruang": "E501",
        "dosen": "Dr. Budi"
      }
    ]
  }
}
```

> Fitur khusus:  
> - Waktu otomatis diubah dari format seperti `1/2` menjadi `07:30 – 09:30`  
> - Informasi semester dan tanggal berlaku juga ditampilkan

---

### 3. Cari Dosen Wali Kelas — `/cari-wali`

Cari **dosen wali** berdasarkan **kode kelas** atau **nama dosen**.

Data ini berasal dari daftar "Dosen Wali Kelas Semester Ganjil (PTA) 2025/2026" yang tersedia di BAAK.

#### Cara Pakai

Kirim:
- `teks` → kode kelas (misal: `1IA01`) **atau** nama dosen (misal: `OCTARINA BUDI LESTARI`) (wajib)

#### Contoh

```bash
# Cari dosen wali untuk kelas "1IA01"
curl "https://api.500.xx.kg/cari-wali?teks=1IA01"

# Cari kelas yang dibimbing oleh dosen "OCTARINA BUDI LESTARI"
curl "https://api.500.xx.kg/cari-wali?teks=OCTARINA BUDI LESTARI"
```

#### Contoh Hasil

```json
{
  "success": true,
  "query": "OCTARINA BUDI LESTARI",
  "title": "Daftar Dosen Wali Kelas Semester Ganjil (PTA) 2025/2026",
  "total": 1,
  "results": [
    {
      "kelas": "1IA01",
      "dosen": "OCTARINA BUDI LESTARI"
    }
  ]
}
```

> Catatan: Pencarian bersifat eksak (case-insensitive pada sisi server BAAK), dan hanya menampilkan hasil yang cocok persis dengan input.

---

## Aturan & Keamanan

- Hanya tiga alamat yang bisa dipakai:  
  `https://api.500.xx.kg/cari-maba`,  
  `https://api.500.xx.kg/cari-jadwal`, dan  
  `https://api.500.xx.kg/cari-wali`
- Jika Anda mengakses alamat lain, API akan menolak dengan error `404`.
- Semua input divalidasi:
  - `teks` harus diisi
  - `tipe` hanya boleh `Nama` atau `Kelas` (untuk `/cari-maba`)
- Tidak ada data Anda yang disimpan.
- Semua data berasal dari halaman publik — tidak ada akses ke data pribadi.

---

## Yang Perlu Anda Ketahui

1. **Ini bukan API resmi** — jadi bisa berhenti bekerja kapan saja jika situs BAAK berubah.
2. Jika BAAK mengganti tampilan websitenya, API ini mungkin gagal membaca data.
3. Jangan kirim terlalu banyak permintaan dalam waktu singkat — ini bisa membebani server BAAK.
4. Semua data yang ditampilkan sudah tersedia secara terbuka di internet.

---

## Penyangkalan (Disclaimer)

Layanan ini dibuat hanya untuk memudahkan akses informasi publik dari BAAK.  
Pengembang **tidak bertanggung jawab** atas:
- Kesalahan data karena perubahan di situs BAAK
- Penyalahgunaan API
- Masalah hukum yang mungkin timbul

Jika Anda perwakilan resmi Universitas Gunadarma dan keberatan dengan layanan ini, silakan hubungi saya.

---

## Informasi Teknis

- Dibuat dengan: **Cloudflare Workers**
- Bahasa: **JavaScript**
- Metode: **Scraping HTML sederhana** (tanpa browser)
- Sumber data: https://baak.gunadarma.ac.id
- Domain: `api.500.xx.kg`
