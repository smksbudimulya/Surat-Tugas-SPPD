# E-Surat & SPPD SMKS Budi Mulya

Aplikasi administrasi **Surat Tugas & Surat Perintah Perjalanan Dinas (SPPD)** untuk SMKS Budi Mulya.

## Arsitektur

- `index.html` → frontend statis, dapat di-host di GitHub Pages.
- `Code.gs` → backend Google Apps Script.
- Google Spreadsheet → database.
- Tidak lagi bergantung pada Blogger.

## Fitur yang dipertahankan

- Dashboard jumlah surat dan guru.
- Pembuatan/edit/hapus Surat Tugas & SPPD.
- Master Data Guru.
- Login Administrator.
- Pengaturan identitas Kepala Sekolah dan sekolah.
- Preview dokumen 2 halaman.
- Cetak A4 dan F4.
- Logo dan kop surat dari pengaturan.
- Pencarian arsip surat.
- Autofill data guru.

## 1. Siapkan Google Spreadsheet

Buat satu Google Spreadsheet khusus untuk aplikasi ini.

Kemudian:
**Extensions → Apps Script**

Hapus kode lama dan masukkan isi `Code.gs`.

Jalankan fungsi:

`setupDatabase()`

Izinkan akses yang diminta Google.

## 2. Deploy Backend

Di Apps Script:

**Deploy → New deployment → Web app**

Gunakan:
- Execute as: **Me**
- Who has access: **Anyone**

Salin URL Web App.

## 3. Hubungkan GitHub Pages

Buka `index.html`.

Cari:

`const GAS_URL = "PASTE_URL_WEB_APP_APPS_SCRIPT_DI_SINI";`

Ganti dengan URL Web App Apps Script.

Contoh:

`const GAS_URL = "https://script.google.com/macros/s/XXXX/exec";`

Jangan memasukkan username/password admin ke `index.html`.

## 4. Upload ke GitHub

Struktur repository:

```text
e-surat-sppd-smks-budi-mulya/
├── index.html
├── Code.gs
├── README.md
└── .gitignore
```

Untuk GitHub Pages:
1. Buat repository baru.
2. Upload `index.html` dan file pendukung.
3. Buka **Settings → Pages**.
4. Source: **Deploy from a branch**.
5. Branch: `main`.
6. Folder: `/ (root)`.
7. Simpan.

File `Code.gs` boleh disimpan sebagai referensi source backend, tetapi backend tetap dijalankan dari Apps Script.

## Akun awal

Database awal membuat akun administrator:

- Username: `admin`
- Password: `admin2026`

**Segera ubah password pada sheet `Users` setelah instalasi.**

Setelah login, backend membuat **token sesi**. Token dikirim dari frontend ke backend untuk operasi simpan, edit, hapus, dan pengaturan. Token tidak ditulis sebagai kredensial tetap di GitHub.

## Sheet yang dibuat

- `Users`
- `Settings`
- `Data_Guru`
- `Data_Surat`

## Catatan keamanan

Frontend GitHub adalah kode publik. Karena itu:
- Jangan menyimpan password di HTML.
- Jangan menyimpan API key rahasia di HTML.
- Database tetap berada di Google Spreadsheet.
- Akses perubahan data harus melalui backend Apps Script.

## Catatan migrasi

Kode frontend awal berasal dari versi Blogger. Pada versi GitHub:
- dependensi layout Blogger dihapus,
- mode demo dihapus dari penggunaan normal,
- branding menjadi **E-Surat & SPPD SMKS Budi Mulya**,
- URL GAS dipusatkan di satu konfigurasi,
- error backend diberi pesan yang lebih jelas,
- frontend dibuat statis sehingga cocok untuk GitHub Pages.



## Struktur GitHub
- `index.html` — frontend aplikasi
- `config.js` — **satu-satunya file untuk URL Web App GAS**
- `Code.gs` — backend Apps Script, ditempatkan di Apps Script yang terhubung ke Spreadsheet

## Role
- **Admin**: seluruh fitur + pengaturan sekolah + pengelolaan pengguna.
- **Operator**: wajib login; dapat membuat, mengedit, menghapus, melihat arsip, master guru, dan mencetak surat/SPPD.
- Pengunjung tanpa login hanya melihat halaman login dan tidak dapat membaca database.

## Akun awal
- Admin: `admin` / `admin2026`
- Operator: `operator` / `operator2026`
Segera ganti password setelah instalasi dengan mengubah data pada sheet `Users`.

## Konfigurasi GitHub
Edit hanya `config.js`:
```js
window.ESURAT_CONFIG = {
  APP_NAME: "E-Surat & SPPD SMKS Budi Mulya",
  GAS_URL: "URL_WEB_APP_GAS_ANDA",
  DEFAULT_PAPER_SIZE: "a4"
};
```
Jangan menaruh password, API key, atau rahasia lain di `config.js` karena GitHub Pages bersifat publik.

## Cetak
Preview mendukung A4 dan F4/Folio. Ukuran `@page`, area kertas, dan tata letak dokumen mengikuti ukuran yang dipilih sehingga satu data surat yang sudah tersimpan dapat dicetak ulang kapan saja.
