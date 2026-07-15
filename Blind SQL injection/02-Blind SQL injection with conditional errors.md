Langkah 1: Memastikan Titik Suntikan (Injection Point) Bisa Memicu Eror
Tambahkan tanda kutip tunggal (') di ujung ID asli kamu, contoh: TrackingId=xyz'.

Jika halaman berubah menjadi 500 Internal Server Error, artinya tanda kutip tersebut sukses membuat sintaks database tidak seimbang dan celah keamanannya positif ada.



Langkah 2: Membangun Saklar Eror Bersyarat (CASE WHEN)
Di database Oracle, kita membuat jebakan eror menggunakan fungsi CASE WHEN dan dipadukan dengan operasi ilegal seperti pembagian dengan nol (1/0). Di Oracle, kita juga wajib menyebutkan tabel bawaan bernama FROM dual.

Tes Kondisi SALAH (Harus Tampil Normal - 200 OK):
Suntikkan perintah yang logikanya salah (misal 1=2):
TrackingId=xyz' || (SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE NULL END FROM dual) || '
Karena 1=2 adalah salah, database akan melompati perintah eror 1/0 dan memilih ELSE NULL (halaman tampil normal).




Tes Kondisi BENAR (Harus Tampil Eror - 500 Internal Server Error):
Ubah kondisinya menjadi benar (misal 1=1):
TrackingId=xyz' || (SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE NULL END FROM dual) || '
Karena 1=1 adalah benar, database terpaksa mengeksekusi THEN TO_CHAR(1/0). Karena angka tidak bisa dibagi nol, database akan mogok kerja dan memicu status eror 500.


(Jangan lupa untuk memblokir mantra tambahanmu lalu tekan Ctrl + U untuk melakukan URL-Encode agar tandanya tidak rusak di jalur Cookie).
