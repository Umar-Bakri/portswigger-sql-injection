Apa itu injeksi SQL buta?
Injeksi SQL buta terjadi ketika sebuah aplikasi rentan terhadap injeksi SQL, tetapi respons HTTP-nya tidak berisi hasil kueri 
SQL yang relevan atau detail kesalahan basis data apa pun.
Banyak teknik seperti UNIONserangan tidak efektif terhadap kerentanan injeksi SQL buta. Hal ini karena teknik tersebut 
bergantung pada kemampuan untuk melihat hasil kueri yang disuntikkan 
dalam respons aplikasi. Meskipun masih dimungkinkan untuk mengeksploitasi injeksi SQL buta untuk mengakses data yang tidak sah, teknik yang digunakan harus berbeda
