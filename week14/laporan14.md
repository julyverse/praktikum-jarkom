# Laporan Praktikum Modul 14 (802.11 WiFi)

IEEE 802.11 adalah standar jaringan WiFi yang memungkinkan komunikasi data tanpa kabel. Dalam jaringan ini, Access Point (AP) mengirimkan Beacon Frame untuk mengumumkan keberadaan jaringan, sedangkan Data Frame digunakan untuk mengirim data pengguna. Sebelum dapat berkomunikasi, perangkat harus melakukan proses Association dengan AP, dan koneksi dapat diakhiri melalui proses Disassociation.

## Tujuan Praktikum
1. Dapat menginvestigasi cara kerja WiFi menggunakan Wireshark

## Analisis Beacon Frame

![14-1](../assets/image/Screenshot%20(5133).png)

Setelah filter wlan.fc.type_subtype == 8 diterapkan, terlihat bahwa Access Point "CiscoLinksys_f7:1d:51" secara rutin memancarkan beacon frame dengan SSID "30 Munroe St". Selain itu pada list yang sama juga muncul beacon dari Access Point lain yaitu "LinksysGroup_67:22" dengan SSID "linksys_ses_24086" dan "linksys12", yang merupakan AP tetangga (bukan AP rumah yang sedang diobservasi)

![14-2](../assets/image/Screenshot%20(5131).png)

Pada frame ini, alamat penerima (Receiver Address) dan alamat tujuan (Destination Address) bernilai Broadcast (ff:ff:ff:ff:ff:ff), karena beacon memang ditujukan untuk semua perangkat di sekitar AP, bukan untuk satu perangkat tertentu. Transmitter Address dan Source Address bernilai CiscoLinksys_f7:1d:51, yaitu alamat MAC dari Access Point pengirim beacon

![14-3](../assets/image/Screenshot%20(5126).png)


![14-4](../assets/image/Screenshot%20(5127).png)

Informasi ini menunjukkan bahwa AP "30 Munroe St" beroperasi pada channel 6

![14-5](../assets/image/Screenshot%20(5128).png)

Bagian "Tagged parameters" pada beacon frame berisi sejumlah informasi tambahan yang disiarkan oleh AP. Pada beacon frame yang diamati, ditemukan beberapa tag penting, antara lain:
1. Tag: SSID parameter set, bernilai "30 Munroe St" -> ini adalah nama jaringan WiFi yang ditampilkan kepada pengguna
2. Tag: Supported Rates 1(B), 2(B), 5.5(B), 11(B) [Mbit/sec] -> kecepatan transmisi data yang didukung oleh AP
3. Tag: DS Parameter set: Current Channel: 6 -> menunjukkan channel operasi AP, sesuai dengan informasi radio sebelumnya
dll

## Analisis Transfer Data

![14-6](../assets/image/Screenshot%20(5134).png)

Sebelum data HTTP dapat dikirimkan, terlebih dahulu dilakukan proses pembentukan koneksi TCP (three-way handshake) antara host dengan server gaia.cs.umass.edu. Urutan paketnya adalah sebagai berikut:
1. Paket No. 474: host mengirim segmen [SYN] dengan Seq=0 kepada server
2. Paket No. 476: server membalas dengan segmen [SYN, ACK] dengan Seq=0, Ack=1
3. Paket No. 478: host membalas dengan segmen [ACK] Seq=1, Ack=1, sehingga koneksi TCP resmi terbentuk

Setelah handshake selesai, pada paket No. 480 barulah host mengirimkan permintaan HTTP GET menuju path /wireshark-labs/alice.txt

![14-7](../assets/image/Screenshot%20(5136).png)

Saat layer Transmission Control Protocol pada paket No. 480 diperluas, terlihat informasi bahwa segmen ini dikirim dari Source Port 2538 menuju Destination Port 80 dengan Sequence number = 1, Acknowledgment number = 1, dan panjang data (Len) = 435 byte, yaitu panjang dari permintaan HTTP GET beserta header-headernya (Host, User-Agent, Accept, dll)

![14-8](../assets/image/Screenshot%20(5137).png)

![14-9](../assets/image/Screenshot%20(5138).png)

Salah satu hal unik pada frame 802.11 adalah digunakannya hingga empat alamat MAC tergantung konfigurasi jaringan . Hal ini menunjukkan bahwa AP "30 Munroe St" berfungsi sebagai perantara (relay): frame dikirim secara nirkabel dari host menuju AP (Receiver Address = AP), kemudian diteruskan oleh AP melalui jaringan kabel menuju gateway (Destination Address = CiscoLinksys_f4:eb:a8), sehingga total terdapat dua pasang alamat yang berbeda dalam satu frame

## Analisis Association

![14-10](../assets/image/Screenshot%20(5140).png)

Association Request adalah "permintaan bergabung" yang dikirim oleh perangkat (host) ke Access Point (AP) saat ingin terhubung ke jaringan WiFi

![14-11](../assets/image/Screenshot%20(5142).png)

Association Response adalah "jawaban" dari Access Point (AP) atas permintaan bergabung (Association Request) yang dikirim oleh host. Hasil filter menunjukkan satu frame Association Response, dikirim dari AP CiscoLinksys menuju host Intel. Frame Control Field pada paket ini bernilai 0x1000, dengan Type = Management frame (0) dan Subtype = 1 (Association Response), yang mengonfirmasi bahwa permintaan asosiasi host telah diterima dan disetujui oleh AP "30 Munroe St"

## Kesimpulan
Dari praktikum ini, dapat disimpulkan bahwa komunikasi pada jaringan WiFi (802.11) diawali dengan AP yang menyiarkan beacon frame berisi informasi SSID dan channel, kemudian host yang ingin terhubung akan mengirim Association Request dan dibalas dengan Association Response oleh AP