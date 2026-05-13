# 📚 Dongeng Kita — Panduan Setup Lengkap

Website cerita anak dengan database Supabase.  
Estimasi waktu setup: **15–20 menit**.

---

## Daftar File

| File | Keterangan |
|------|-----------|
| `index.html` | Halaman galeri publik (semua pengunjung bisa lihat) |
| `admin.html` | Panel admin — login, tambah, edit, hapus cerita |
| `supabase-schema.sql` | Script SQL untuk setup database otomatis |
| `SETUP.md` | Panduan ini |

---

## Langkah 1 — Buat Akun & Project Supabase

1. Buka [https://supabase.com](https://supabase.com) dan klik **Start your project**
2. Daftar / login dengan akun GitHub atau email
3. Klik **New Project**
4. Isi:
   - **Name**: `dongeng-kita` (atau nama lain)
   - **Database Password**: buat password yang kuat, **simpan baik-baik**
   - **Region**: pilih `Southeast Asia (Singapore)` agar lebih cepat dari Indonesia
5. Klik **Create new project** — tunggu sekitar 1–2 menit sampai project siap

---

## Langkah 2 — Jalankan SQL Schema

1. Di dashboard Supabase, buka menu **SQL Editor** (ikon database di sidebar kiri)
2. Klik **New query**
3. Buka file `supabase-schema.sql`, **copy semua isinya**
4. Paste ke SQL Editor
5. Klik tombol **Run** (▶️) di pojok kanan bawah
6. Tunggu hingga muncul pesan `Success. No rows returned`

Ini akan otomatis membuat:
- ✅ Tabel `stories`
- ✅ Storage bucket `stories` untuk file HTML
- ✅ Row Level Security (publik bisa baca, hanya admin yang bisa edit)
- ✅ 6 contoh cerita sebagai starter data

---

## Langkah 3 — Ambil Kredensial Supabase

1. Di dashboard Supabase, buka **Project Settings** (ikon gear ⚙️ di sidebar)
2. Klik menu **API**
3. Catat dua nilai berikut:

```
Project URL    : https://xxxxxxxxxxxxxx.supabase.co
anon/public key: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

## Langkah 4 — Isi Kredensial ke File HTML

Buka **`index.html`** dan **`admin.html`** dengan teks editor (Notepad, VS Code, dll).

Di kedua file, cari bagian ini (ada di dekat bawah, di dalam tag `<script>`):

```javascript
const SUPABASE_URL = 'GANTI_DENGAN_SUPABASE_URL';
const SUPABASE_ANON_KEY = 'GANTI_DENGAN_SUPABASE_ANON_KEY';
```

Ganti dengan nilai dari Langkah 3:

```javascript
const SUPABASE_URL = 'https://xxxxxxxxxxxxxx.supabase.co';
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...';
```

> ⚠️ Lakukan ini di **kedua file** — `index.html` dan `admin.html`

---

## Langkah 5 — Buat Akun Admin Pertama

1. Di dashboard Supabase, buka **Authentication** (ikon kunci 🔑 di sidebar)
2. Klik tab **Users**
3. Klik tombol **Add user → Create new user**
4. Isi:
   - **Email**: email kamu (contoh: `admin@gmail.com`)
   - **Password**: password yang kuat
   - Centang **Auto Confirm User**
5. Klik **Create User**

Akun ini digunakan untuk login ke `admin.html`.

---

## Langkah 6 — Hosting di GitHub Pages (Gratis)

### A. Buat Repository GitHub

1. Buka [https://github.com/new](https://github.com/new)
2. Isi **Repository name**: `dongeng-kita`
3. Pilih **Public**
4. Klik **Create repository**

### B. Upload File

1. Di halaman repository yang baru dibuat, klik **uploading an existing file**
2. Upload keempat file:
   - `index.html`
   - `admin.html`
   - `supabase-schema.sql` *(opsional)*
   - `SETUP.md` *(opsional)*
3. Klik **Commit changes**

### C. Aktifkan GitHub Pages

1. Buka tab **Settings** di repository
2. Di sidebar kiri, klik **Pages**
3. Di bagian **Source**, pilih:
   - Branch: `main`
   - Folder: `/ (root)`
4. Klik **Save**
5. Tunggu 1–2 menit

Website kamu akan live di:
```
https://usernamegithub.github.io/dongeng-kita/
```

---

## Langkah 7 — Uji Coba

1. Buka `https://usernamegithub.github.io/dongeng-kita/` — galeri harus tampil dengan 6 cerita contoh
2. Buka `https://usernamegithub.github.io/dongeng-kita/admin.html` — login dengan akun yang dibuat di Langkah 5
3. Coba tambah cerita baru dan upload file HTML

---

## Cara Menambah Cerita Baru

1. Buka halaman **admin.html**
2. Login dengan email & password admin
3. Klik **+ Tambah Cerita**
4. Isi judul, kategori, usia, sinopsis
5. Pilih ikon emoji untuk cover
6. Upload file **`.html`** cerita kamu
7. Klik **🎉 Simpan Cerita**

Cerita langsung muncul di galeri dan bisa diakses dari device mana saja! 🎉

---

## Cara Mengundang Admin/Editor Lain

Karena fitur `invite` memerlukan service role key (tidak aman di browser), cara termudah:

1. Buka **Supabase Dashboard → Authentication → Users**
2. Klik **Add user → Create new user**
3. Isi email dan password orang yang ingin kamu beri akses
4. Bagikan email & password tersebut ke mereka secara pribadi
5. Mereka bisa login di `admin.html`

---

## Opsi Hosting Alternatif

### Netlify (Drag & Drop, Gratis)
1. Buka [https://netlify.com](https://netlify.com) dan daftar
2. Dari dashboard, seret folder berisi `index.html` dan `admin.html` ke area drop
3. Selesai — URL otomatis diberikan (misal: `https://dongeng-kita.netlify.app`)

### Vercel (Gratis)
1. Buka [https://vercel.com](https://vercel.com) dan login dengan GitHub
2. Klik **New Project → Import** repository GitHub kamu
3. Klik **Deploy**

---

## Struktur Database

### Tabel `stories`

| Kolom | Tipe | Keterangan |
|-------|------|-----------|
| `id` | uuid | Primary key, otomatis |
| `title` | text | Judul cerita |
| `description` | text | Sinopsis singkat |
| `category` | text | dongeng / fabel / petualangan / islami / sains |
| `age_range` | text | 3-5 tahun / 6-8 tahun / 9-12 tahun |
| `emoji` | text | Ikon cover |
| `file_path` | text | Path file HTML di Supabase Storage |
| `created_at` | timestamptz | Waktu dibuat |
| `updated_at` | timestamptz | Waktu terakhir diedit |

---

## Troubleshooting

**Galeri kosong / tidak muncul cerita**
- Pastikan `SUPABASE_URL` dan `SUPABASE_ANON_KEY` sudah diisi dengan benar di kedua file HTML
- Buka browser DevTools (F12) → Console, lihat apakah ada error

**Tidak bisa login di admin.html**
- Pastikan akun sudah dibuat di Supabase → Authentication → Users
- Pastikan **Auto Confirm User** dicentang saat membuat akun

**Upload file HTML gagal**
- Pastikan SQL schema sudah dijalankan (Langkah 2) agar storage bucket `stories` terbuat
- Ukuran file maksimal 50MB (lebih dari cukup untuk file HTML)

**Cerita tidak muncul setelah ditambah**
- Refresh halaman `index.html`
- Cek Supabase → Table Editor → stories, pastikan data tersimpan

---

## Pertanyaan?

Hubungi developer atau buka issue di repository GitHub. Selamat bercerita! 🌟
