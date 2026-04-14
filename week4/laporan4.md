# Laporan Praktikum Modul 4 (DNS)

Domain Name System (DNS) merupakan sistem yang berfungsi untuk menerjemahkan nama domain (hostname) menjadi alamat IP. DNS sangat penting dalam infrastruktur internet karena memudahkan pengguna mengakses website tanpa harus mengingat alamat IP. Pada klien, DNS bekerja dengan cara mengirim permintaan ke server DNS lalu menerima balasan berupa alamat IP

## Tujuan Praktikum
1. Dapat menginvestigasi cara kerja DNS menggunakan Wireshark

### A. Nslookup
Nslookup adalah sebuah tools yang digunakan untuk mencari informasi terkait DNS seperti alamat IP atau server DNS

![nslookup1](../assets/image/Screenshot%20(4606).png)

![nslookup2](../assets/image/Screenshot%20(4610).png)

Perintah nslookup www.mit.edu digunakan untuk mengetahui alamat IP dari website tersebut dengan cara meminta informasi ke DNS server. Kemudian, perintah nslookup -type=NS mit.edu digunakan untuk mengetahui server DNS yang mengelola domain mit.edu. Sedangkan nslookup www.mit.edu 8.8.8.8 memiliki fungsi yang sama seperti perintah pertama, yaitu mencari alamat IP, namun menggunakan DNS server milik Google sebagai pembanding. Secara umum, nslookup digunakan untuk memperoleh informasi terkait domain, baik berupa alamat IP maupun server DNS

## Pertanyaan
1. Jalankan nslookup untuk mendapatkan alamat IP dari server web di Asia. Berapa alamat IP
server tersebut?

![a1](../assets/image/Screenshot%20(4899).png)

Alamat IP dari server tersebut adalah 104.16.4.14

2. Jalankan nslookup agar dapat mengetahui server DNS otoritatif untuk universitas di Eropa

![a2](../assets/image/Screenshot%202026-04-15%20000455.png)

3. Jalankan nslookup untuk mencari tahu informasi mengenai server email dari Yahoo! Mail
melalui salah satu server yang didapatkan di pertanyaan nomor 2. Apa alamat IP-nya?

![a3](../assets/image/Screenshot%202026-04-15%20001055.png)

### B. Ipconfig
Perintah ipconfig digunakan untuk melihat informasi konfigurasi jaringan pada komputer. Selain itu, ipconfig juga dapat digunakan untuk melihat apakah komputer sudah terhubung ke jaringan dengan benar. Secara sederhana, ipconfig berfungsi untuk mengecek dan menampilkan informasi jaringan pada perangkat kita

### C. Tracing DNS dengan Wireshark
## Soal 1
1. Cari pesan permintaan DNS dan balasannya. Apakah pesan tersebut dikirimkan melalui UDP
atau TCP?

![b1](../assets/image/Screenshot%20(4629).png)

Protokol yang digunakan adalah UDP

2. Apa port tujuan pada pesan permintaan DNS? Apa port sumber pada pesan balasannya?

![b2](../assets/image/Screenshot%20(4629).png)

port tujuan pada pesan permintaan DNS adalah port 53. Sementara itu, port sumber pada pesan balasan DNS juga menggunakan port 53

3. Pada pesan permintaan DNS, apa alamat IP tujuannya? Apa alamat IP server DNS lokal anda
(gunakan ipconfig untuk mencari tahu)? Apakah kedua alamat IP tersebut sama?

![b3-1](../assets/image/Screenshot%20(4635).png)

![b3-2](../assets/image/Screenshot%20(4636).png)

4. Periksa pesan permintaan DNS. Apa “jenis” atau ”type” dari pesan tersebut? Apakah pesan
permintaan tersebut mengandung ”jawaban” atau ”answers”?

![b4](../assets/image/Screenshot%20(4631).png)

5. Periksa pesan balasan DNS. Berapa banyak ”jawaban” atau ”answers” yang terdapat di
dalamnya? Apa saja isi yang terkandung dalam setiap jawaban tersebut?

![b5](../assets/image/Screenshot%20(4632).png)

6. Perhatikan paket TCP SYN yang selanjutnya dikirimkan oleh host Anda. Apakah alamat IP
pada paket tersebut sesuai dengan alamat IP yang tertera pada pesan balasan DNS?

Menggunakan alamat IP yang sama

7. Halaman web yang sebelumnya anda akses (http://www.ietf.org) memuat beberapa
gambar. Apakah host Anda perlu mengirimkan pesan permintaan DNS baru setiap kali ingin
mengakses suatu gambar?

Tidak memerlukan request DNS baru, karena dapat menggunakan cache DNS yang sudah tersimpan

## Soal 2
1. Apa port tujuan pada pesan permintaan DNS? Apa port sumber pada pesan balasan DNS?

[c1-1](../assets/image/Screenshot%20(4641).png)

[c1-2](../assets/image/Screenshot%20(4642).png)

2. Ke alamat IP manakah pesan permintaan DNS dikirimkan? Apakah alamat IP tersebut
merupakan default alamat IP server DNS lokal Anda?

[c2](../assets/image/Screenshot%20(4639).png)

Ya, alamat tersebut merupakan default alamat IP server DNS lokal pada rekaman paket tersebut

3. Periksa pesan permintaan DNS. Apa ”jenis” atau ”type” dari pesan tersebut? Apakah pesan
tersebut mengandung ”jawaban” atau ”answers”?

[c3-1](../assets/image/Screenshot%20(4639).png)

[c3-2](../assets/image/Screenshot%20(4640).png)

4. Periksa pesan balasan DNS. Berapa banyak ”jawaban” atau “answers” yang terdapat di
dalamnya. Apa saja isi yang terkandung dalam setiap jawaban tersebut?

[c4](../assets/image/Screenshot%20(4648).png)

5. Sertakan hasil tangkapan layar

## Soal 3
1. Ke alamat IP manakah pesan permintaan DNS dikirimkan? Apakah alamat IP tersebut
merupakan default alamat IP server DNS lokal Anda?

[d1-1](../assets/image/Screenshot%20(4645).png)

[d1-2](../assets/image/Screenshot%20(4646).png)

2. Periksa pesan permintaan DNS. Apa ”jenis” atau ”type” dari pesan tersebut? Apakah pesan
tersebut mengandung ”jawaban” atau ”answers”?

[d2](../assets/image/Screenshot%20(4647).png)

3. Periksa pesan balasan DNS. Apa nama server MIT yang diberikan oleh pesan balasan?
Apakah pesan balasan ini juga memberikan alamat IP untuk server MIT tersebut?

[d3](../assets/image/Screenshot%20(4648).png)

4. Sertakan hasil tangkapan layar



## Soal 4
1. Ke alamat IP manakah pesan permintaan DNS dikirimkan? Apakah alamat IP tersebut
merupakan default alamat IP server DNS lokal Anda?

[e1](../assets/image/Screenshot%20(4646).png)

2. Periksa pesan permintaan DNS. Apa ”jenis” atau ”type” dari pesan tersebut? Apakah pesan
tersebut mengandung ”jawaban” atau ”answers”?

[e2-1](../assets/image/Screenshot%20(4647).png)

[e2-2](../assets/image/Screenshot%20(4649).png)

3. Periksa pesan balasan DNS. Berapa banyak ”jawaban” atau “answers” yang terdapat di
dalamnya. Apa saja isi yang terkandung dalam setiap jawaban tersebut?

[e3](../assets/image/Screenshot%20(4648).png)

4. Sertakan hasil tangkapan layar

## Kesimpulan
Dari praktikum ini, dapat disimpulkan bahwa Domain Name System (DNS) berperan penting dalam menerjemahkan nama domain menjadi alamat IP agar dapat diakses oleh komputer. Tools seperti nslookup dan ipconfig membantu dalam melihat serta mengelola informasi DNS