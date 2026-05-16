# Laporan Praktikum Modul 10 (IP)

Internet Protocol (IP) adalah protokol pada jaringan komputer yang digunakan untuk mengirim data dari satu perangkat ke perangkat lain melalui internet. IP bekerja dengan memberikan alamat pada setiap perangkat agar data bisa sampai ke tujuan dengan benar. IP memiliki dua versi, yaitu IPv4 yang menggunakan alamat 32-bit dan IPv6 yang menggunakan alamat 128-bit

## Tujuan Praktikum
1. Dapat menginvestigasi cara kerja protokol IP menggunakan Wireshark

## Hasil Capture Wireshark

### Tracert

![10-1](../assets/image/Screenshot%202026-05-04%20131109.png)

Berdasarkan hasil perintah tracert ke gaia.cs.umass.edu, terlihat bahwa paket data melewati beberapa router (hop) sebelum mencapai tujuan. Setiap hop menunjukkan waktu tempuh (delay) dan alamat IP router yang dilewati. Terdapat beberapa hop yang menampilkan Request timed out yang menunjukkan router tidak memberikan respon. Hasil menunjukkan bahwa paket berhasil mencapai tujuan pada hop ke-28. Proses ini membuktikan cara kerja traceroute yang memanfaatkan nilai TTL dan respon ICMP dari setiap router

### TTL

TTL (Time To Live) merupakan nilai yang digunakan untuk membatasi umur paket dalam jaringan. Setiap paket yang melewati router akan mengalami pengurangan nilai TTL sebesar satu

### ICMP

![10-2](../assets/image/Screenshot%202026-05-04%20132347.png)

ICMP (Internet Control Message Protocol) merupakan protokol yang digunakan untuk mengirim pesan kontrol atau informasi kesalahan dalam jaringan. Terlihat adanya paket ICMP jenis Echo Request yang dikirim oleh komputer sebagai permintaan ke tujuan

![10-3](../assets/image/Screenshot%202026-05-04%20132448.png)

Terlihat paket ICMP jenis Time-to-live exceeded yang dikirim oleh router yang menunjukkan bahwa paket dihentikan karena TTL habis

### IPv4

![10-5](../assets/image/Screenshot%202026-05-04%20132712.png)

Berdasarkan hasil capture Wireshark terlihat bahwa paket yang digunakan adalah Internet Protocol Version 4 (IPv4). Hal ini dapat dilihat pada Gambar yang menampilkan header IPv4 dari paket yang dikirim. Header IPv4 tersebut memuat informasi source port (192.168.100.133) dan destination port (128.119.245.12), panjang header, serta total panjang paket. Selain itu, terdapat field TTL yang menunjukkan batas jumlah router yang dapat dilewati paket. Pada gambar terlihat nilai TTL = 2, yang berarti paket dapat melewati maksimal dua router sebelum dihentikan. Terdapat field Protocol yang menunjukkan bahwa paket membawa protokol ICMP (1). Header IPv4 ini berperan penting dalam proses pengiriman paket karena menentukan bagaimana paket diarahkan dari sumber ke tujuan melalui jaringan

### Fragmentasi

![10-5](../assets/image/Screenshot%202026-05-04%20132712.png)

Fragmentasi adalah proses memecah paket data besar menjadi beberapa bagian kecil supaya bisa lewat di jaringan. Pada gambar tidak terlihat adanya fragmentasi paket IP. Hal ini ditunjukkan dengan nilai Fragment Offset = 0 dan tidak adanya pembagian paket menjadi beberapa bagian yang berarti ukuran paket yang dikirim masih berada di bawah batas Maximum Transmission Unit (MTU) sehingga tidak perlu dipecah. Fragmentasi biasanya terjadi jika ukuran paket terlalu besar, namun pada gambar paket ICMP yang dikirim berukuran kecil sehingga dikirim dalam satu bagian saja

### IPv6

![10-7](../assets/image/Screenshot%202026-05-04%20134208.png)

![10-8](../assets/image/Screenshot%202026-05-04%20134234.png)

Dari gambar tersebut dapat diketahui bahwa paket menggunakan IPv6. Selain itu, alamat IP sumber dan tujuan menggunakan format IPv6 seperti (2001:db8:1::10) dan (2a00:1450:4009:80b::200e) yang bentuknya lebih panjang dan menggunakan tanda titik dua (:), berbeda dengan IPv4 yang biasanya memakai format angka seperti (192.168.1.1)

## Kesimpulan
Dari praktikum ini, dapat disimpulkan bahwa protokol IP berfungsi untuk mengirim data dari sumber ke tujuan pada jaringan komputer, dan melalui Wireshark dapat diamati bagaimana proses kerja IP