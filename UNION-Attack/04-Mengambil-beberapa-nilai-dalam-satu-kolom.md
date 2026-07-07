' UNION SELECT username || '~' || password FROM users--
Ini menggunakan urutan pipa ganda ||yang merupakan operator penggabungan string pada Oracle. 
Kueri yang disuntikkan menggabungkan nilai-nilai bidang usernamedan password, dipisahkan oleh ~karakter .
