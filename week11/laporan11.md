# Laporan Praktikum Modul 11 (DHCP)

DHCP merupakan protokol jaringan yang digunakan untuk memberikan alamat IP secara otomatis kepada perangkat dalam jaringan. Selain alamat IP, DHCP juga dapat memberikan informasi lain seperti subnet mask, default gateway, DNS server. DHCP bekerja dengan model client-server. Server DHCP akan menyediakan konfigurasi jaringan, sedangkan client akan meminta alamat IP ketika terhubung ke jaringan

## Tujuan Praktikum
1. Dapat menginvestigasi cara kerja protokol DHCP menggunakan Wireshark

## Hasil Capture Wireshark

![11](../assets/image/Screenshot%20(5028).png)

### Penjelasan
Berdasarkan hasil capture pada aplikasi Wireshark, terlihat proses komunikasi protokol DHCP yang terjadi antara client dan server dalam jaringan. Pada kolom Info terlihat empat tahapan utama DHCP yaitu :

1. DHCP Discover
2. DHCP Offer
3. DHCP Request
4. DHCP ACK

Proses dimulai dari client dengan alamat 0.0.0.0 mengirim paket DHCP Discover ke alamat broadcast 255.255.255.255 untuk mencari server DHCP yang tersedia. Setelah itu server DHCP dengan alamat 192.168.1.1 membalas menggunakan DHCP Offer yang berisi penawaran alamat IP kepada client. Selanjutnya client mengirim DHCP Request sebagai permintaan penggunaan alamat IP yang ditawarkan server. Terakhir server mengirim DHCP ACK sebagai tanda bahwa alamat IP telah resmi diberikan dan dapat digunakan oleh client. Pada tampilan detail paket juga terlihat bahwa DHCP menggunakan protokol UDP dengan source port 68 (client) dan destination port 67 (server). Dari hasil capture ini dapat disimpulkan bahwa proses konfigurasi alamat IP secara otomatis menggunakan DHCP berjalan dengan baik melalui mekanisme DORA (Discover, Offer, Request, ACK)

## Kesimpulan
Dari praktikum ini, dapat disimpulkan bahwa protokol DHCP bekerja untuk memberikan alamat IP secara otomatis kepada client melalui empat tahapan utama yaitu Discover, Offer, Request, dan ACK