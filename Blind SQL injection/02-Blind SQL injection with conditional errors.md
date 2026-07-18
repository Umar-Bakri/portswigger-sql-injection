# 📘 Catatan Belajar: Blind SQL Injection (Oracle) via Cookie

Ringkasan tahapan eksploitasi Blind SQL Injection menggunakan teknik *conditional error-based* pada database Oracle, dari deteksi awal sampai ekstraksi password admin.

---

## 🏛️ Fase 1: Tes Apakah Pintu Gudang Bisa Dirusak (Deteksi Awal)

**Langkah:**
Mengubah `TrackingId=xyz` menjadi `xyz'` (Error), lalu menjadi `xyz''` (Error hilang).

**Penjelasan:**
- Memasukkan satu tanda kutip (`'`) untuk merusak struktur kueri database asli di aplikasi. Ketika halaman memunculkan error, itu bukti bahwa input cookie langsung ditempel ke kueri SQL.
- Memasukkan dua tanda kutip (`''`). Di dalam SQL, dua tanda kutip berurutan artinya "mengabaikan tanda kutip" (*escaping*). Karena kuerinya jadi seimbang lagi, errornya hilang (`200 OK`).
- ✅ Ini memastikan **100% ada celah SQL Injection**.
- 
---

## 🔍 Fase 2: Menebak Jenis Mesin Database (Oracle Detection)

**Langkah:**
Mencoba `SELECT ''` (tetap error), lalu mencoba `SELECT '' FROM dual` (lancar / `200 OK`).

**Penjelasan:**
Setiap mesin database punya aturan bahasa (*syntax*) berbeda:

- Di **MySQL/SQL Server**, perintah `SELECT ''` itu sah-sah saja.
- Di **Oracle**, perintah `SELECT` wajib menyebutkan nama tabelnya (`FROM nama_tabel`). Oracle menyediakan tabel tiruan bawaan bernama `dual` untuk keperluan tes.
- ' || (SELECT '' ) || ' # error 500
- ' || (SELECT '' FROM DUAL) || ' # 200 ok

Karena rumus yang pakai `FROM dual` berhasil membuat errornya hilang, didapat info berharga:
> 🎯 **Target lab ini menggunakan database Oracle!**

---

## 📦 Fase 3: Memastikan Tabel `users` Itu Nyata

**Langkah:**
Mencoba `FROM not-a-real-table` (error), lalu mencoba `FROM users WHERE ROWNUM = 1` (lancar).

**Penjelasan:**
- Menembak nama tabel asal-asalan → database langsung memicu error karena tabelnya tidak ada.
- Ganti menjadi `FROM users` → halaman kembali lancar (`200 OK`). Ini membuktikan bahwa tabel bernama `users` benar-benar ada di dalam database tersebut.
- ' || (select '' from users where rownum =1) || '
- ' || (select '' from users where username= 'administrator') || ' #konfirmasi adminitrator

> **Catatan:** Kondisi `WHERE ROWNUM = 1` dipakai karena hanya butuh database mengembalikan 1 baris saja, agar tidak merusak baris gabungan teks cookie-nya.

---

## 🧠 Fase 4: Membuat Saklar Error Bersyarat (`CASE WHEN`)

**Langkah:**
Mencoba `CASE WHEN (1=1) THEN TO_CHAR(1/0)...` (Error 500), lalu ganti `CASE WHEN (1=2)...` (Lancar 200 OK).
' || (select CASE WHEN (1=0) THEN TO_CHAIR(1/0) ELSE '' END FROM dual) || '
**Penjelasan:**
Di sinilah logika utama lab ini bekerja — membuat sistem logika otomatis di dalam database:

| Kondisi | Aksi Database | Hasil |
|---|---|---|
| **BENAR** (`1=1`) | Eksekusi perintah setelah `THEN`, yaitu `TO_CHAR(1/0)` (pembagian dengan nol) | Database crash → **`500 Internal Server Error`** |
| **SALAH** (`1=2`) | Lompati perintah pembagian nol, eksekusi `ELSE ''` | Halaman lancar → **`200 OK`** |

---

## 🎯 Fase 5: Interogasi Akun Admin dan Panjang Password

**Langkah:**
Menguji `username='administrator'` (hasil: Error 500 → admin ada). Lalu menguji `LENGTH(password)>1`, `>2`, dst. menggunakan Repeater sampai errornya hilang.

**Penjelasan:**
- Memasukkan perintah cek panjang password: `LENGTH(password)>20`.
- Saat angka dinaikkan bertahap, status akan terus `500 Internal Server Error` (artinya kondisi "lebih besar dari" bernilai **BENAR**).
- Begitu tebakan angka menyentuh batasnya, kondisi menjadi **SALAH**, pembagian nol dilewati, halaman kembali `200 OK`.

> 📏 Dari situ diketahui **panjang password adalah 20 karakter**.

---

## 🏁 Fase 6: Eksekusi Massal dengan Burp Intruder

**Langkah:**
Menggunakan rumus `SUBSTR(password,1,1)='§a§'`, mengatur payload `a-z` dan `0-9`, lalu mencari status `500`.

**Penjelasan:**
- Karena menebak 20 karakter dengan kombinasi huruf dan angka secara manual itu melelahkan, request dipindahkan ke **Burp Intruder**.
- Fungsi `SUBSTR(password,1,1)` artinya: *"Ambil huruf ke-1, sebanyak 1 karakter"*.
- Huruf tebakan ditandai dengan simbol `§a§` sebagai target acak Intruder.

**Cara membaca hasil:**
Saat serangan Intruder berjalan, fokus melihat kolom **Status**. Jika ada baris yang memunculkan status `500`, artinya karakter pada baris itulah yang **BENAR** (karena memicu kondisi pembagian nol).

Setelah huruf posisi ke-1 ketemu, rumus diubah menjadi `SUBSTR(password,2,1)` untuk mencari huruf ke-2, dan seterusnya sampai ke-20 karakter kata sandi terkumpul lengkap untuk login sebagai administrator.

---

> 📌 *Catatan pribadi belajar keamanan web — Blind SQL Injection (Oracle, error-based via cookie).*
