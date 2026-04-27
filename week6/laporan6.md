# Laporan Praktikum Modul 6 (TCP)

TCP adalah protokol transport yang digunakan untuk mengirim data secara aman dan teratur. Dalam prosesnya, TCP punya beberapa mekanisme penting. Pertama, three-way handshake (SYN, SYN-ACK, ACK) untuk memulai koneksi. Kedua, ada sequence number dan acknowledgement untuk memastikan data diterima dengan benar. Terakhir, terdapat flow control yang mengatur kecepatan pengiriman sesuai kemampuan penerima, serta congestion control untuk mencegah terjadinya kemacetan pada jaringan

## Tampilan Awal pada Captured Trace
### Pertanyaan
1. Berapa alamat IP dan nomor port TCP yang digunakan oleh komputer klien (sumber) untuk mentransfer file ke gaia.cs.umass.edu? Cara paling mudah menjawab pertanyaan ini adalah dengan memilih sebuah pesan HTTP dan meneliti detail paket TCP yang digunakan untuk membawa pesan HTTP tersebut

![6-1-1-1](../assets/image/Screenshot%20(4755).png)

2. Apa alamat IP dari gaia.cs.umass.edu? Pada nomor port berapa ia mengirim dan menerima segmen TCP untuk koneksi ini?

![6-1-2](../assets/image/Screenshot%20(4761).png)

## Dasar TCP
### Pertanyaan
1. Berapa nomor urut segmen TCP SYN yang digunakan untuk memulai sambungan TCP antara komputer klien dan gaia.cs.umass.edu? Apa yang dimiliki segmen tersebut sehingga teridentifikasi sebagai segmen SYN?

![6-1](../assets/image/Screenshot%20(4782).png)

Nomor urut (sequence number) segmen TCP SYN adalah 0 (relative) dan dikenali sebagai SYN karena memiliki flag SYN = 1

2. Berapa nomor urut segmen SYNACK yang dikirim oleh gaia.cs.umass.edu ke komputer klien sebagai balasan dari SYN? Berapa nilai dari field Acknowledgement pada segmen SYNACK? Bagaimana gaia.cs.umass.edu menentukan nilai tersebut? Apa yang dimiliki oleh segmen sehingga teridentifikasi sebagai segmen SYNACK?

![6-2](../assets/image/Screenshot%20(4785).png)

Segmen SYN-ACK punya sequence number 0 dan acknowledgement 1 (karena membalas SYN klien), dan dikenali dari flag SYN dan ACK yang aktif

3. Berapa nomor urut segmen TCP yang berisi perintah HTTP POST? Perhatikan bahwa untuk menemukan perintah POST, Anda harus menelusuri content field milik paket di bagian bawah jendela Wireshark, kemudian cari segmen yang berisi "POST" di bagian field DATA-nya

![6-3](../assets/image/Screenshot%20(4787).png)

Nomor urut (sequence number) segmen TCP yang berisi perintah HTTP POST adalah 1 (relative), yang diperoleh dari paket yang mengandung data POST pada field DATA di Wireshark

4. Anggap segmen TCP yang berisi HTTP POST sebagai segmen pertama dalam koneksi TCP. Berapa nomor urut dari enam segmen pertama dalam TCP (termasuk segmen yang berisi HTTP POST)? Pada jam berapa setiap segmen dikirim? Kapan ACK untuk setiap segmen diterima? Dengan adanya perbedaan antara kapan setiap segmen TCP dikirim dan kapan acknowledgement-nya diterima, berapakah nilai RTT untuk keenam segmen tersebut? Berapa nilai EstimatedRTT setelah penerimaan setiap ACK? (Catatan: Wireshark memiliki fitur yang memungkinkan Anda untuk memplot RTT untuk setiap segmen TCP yang dikirim. Pilih segmen TCP yang dikirim dari klien ke server gaia.cs.umass.edu pada jendela "daftar paket yang ditangkap". Kemudian pilih: Statistics->TCP Stream Graph- >Round Trip Time Graph)

![6-4](../assets/image/Screenshot%20(4790).png)

Waktu kirim dan ACK dilihat di kolom Time, dengan RTT hasil selisih keduanya yang berkisar 100–250 ms dan digunakan untuk menghitung Estimated RTT.

5. Berapa panjang setiap enam segmen TCP pertama?

![6-5](../assets/image/Screenshot%20(4794).png)

Panjang setiap enam segmen TCP pertama adalah sekitar 1460 byte per segmen, sesuai dengan nilai Maximum Segment Size

6. Berapa jumlah minimum ruang buffer tersedia yang disarankan kepada penerima dan diterima untuk seluruh trace? Apakah kurangnya ruang buffer penerima pernah menghambat pengiriman?

![6-6](../assets/image/Screenshot%20(4796).png)

Jumlah minimum ruang buffer yang tersedia (window size) adalah sekitar 5840 byte, dan selama proses pengiriman tidak terjadi hambatan karena buffer penerima masih mencukupi

7. Apakah ada segmen yang ditransmisikan ulang dalam file trace? Apa yang anda periksa (di dalam file trace) untuk menjawab pertanyaan ini?

![6-7](../assets/image/Screenshot%20(4798).png)

Tidak ditemukan adanya segmen yang ditransmisikan ulang (retransmission)

8. Berapa banyak data yang biasanya diakui oleh penerima dalam ACK? Dapatkah anda mengidentifikasi kasus-kasus di mana penerima melakukan ACK untuk setiap segmen yang diterima?

![6-8](../assets/image/Screenshot%202026-04-28%20001413.png)

Penerima biasanya mengakui sekitar 1460 byte per ACK, dan tidak selalu mengirim ACK untuk setiap segmen karena adanya mekanisme delayed ACK

9. Berapa throughput (byte yang ditransfer per satuan waktu) untuk sambungan TCP? Jelaskan bagaimana Anda menghitung nilai ini

![6-9](../assets/image/Screenshot%20(4801).png)

Throughput merupakan banyaknya data yang berhasil dikirim dalam setiap satuan waktu. Dari grafik yang diamati, throughput TCP berada di sekitar ±1.3–1.5 Mbps ketika sudah mencapai kondisi stabil. Hal ini mencerminkan laju transfer data efektif setelah koneksi TCP melewati fase awal

## Congestion Control pada TCP
### Pertanyaan
1. Gunakan alat plotting Time-Sequence-Graph (Stevens) untuk melihat grafik nomor urut berbanding waktu dari segmen yang dikirim oleh klien ke server gaia.cs.umass.edu. Dapatkah Anda mengidentifikasi di mana fase “slow start” TCP dimulai dan berakhir, dan pada bagian mana algoritma ”congestion avoidance” mengambil alih? Berikan komentar tentang bagaimana data yang diukur berbeda dari perilaku ideal TCP yang telah kita pelajari

![6-1-1](../assets/image/Screenshot%20(4803).png)

Grafik menunjukkan slow start di awal (kenaikan eksponensial), lalu beralih ke congestion avoidance (kenaikan linear), dengan hasil nyata yang bisa berbeda karena delay dan packet loss

## Kesimpulan
Dari praktikum ini, dapat disimpulkan bahwa TCP merupakan protokol yang mampu menjamin pengiriman data secara andal dan teratur melalui mekanisme koneksi yang terstruktur. Proses seperti three-way handshake, penggunaan sequence number dan acknowledgement, serta pengaturan flow control dan congestion control berperan penting dalam menjaga keutuhan dan kelancaran pengiriman data. Dari hasil analisis menggunakan Wireshark, dapat diamati bahwa setiap proses dalam TCP berjalan sesuai dengan konsep teorinya, sehingga data dapat dikirim dengan baik tanpa kehilangan maupun kesalahan