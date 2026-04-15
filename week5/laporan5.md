# Laporan Praktikum Modul 5 (UDP)

User Datagram Protocol (UDP) merupakan salah satu protokol pada layer transport yang bersifat sederhana dan tidak memiliki mekanisme kontrol seperti TCP. UDP tidak melakukan pengecekan koneksi, tidak menjamin pengiriman data, serta tidak melakukan retransmisi jika terjadi kehilangan paket. Karena sifatnya yang ringan dan cepat, UDP sering digunakan pada aplikasi yang membutuhkan kecepatan tinggi seperti streaming

## Tujuan Praktikum
1. Mahasiswa dapat menginvestigasi cara kerja protokol UDP menggunakan Wireshark.

## Pertanyaan
1. Pilih satu paket UDP yang terdapat pada trace Anda. Dari paket tersebut, berapa banyak “field” yang terdapat pada header UDP? Sebutkan nama-nama field yang Anda temukan!

![5-1](../assets/image/Screenshot%20(4658).png)

Terdapat 4 field utama pada header UDP yaitu Source Port, Destination Port, Length, dan Checksum

2. Perhatikan informasi “content field” pada paket yang Anda pilih di pertanyaan 1. Berapa panjang (dalam satuan byte) masing-masing “field” yang terdapat pada header UDP?

![5-2-1](../assets/image/Screenshot%20(4660).png)

![5-2-2](../assets/image/Screenshot%20(4661).png)

![5-2-3](../assets/image/Screenshot%20(4662).png)

![5-2-4](../assets/image/Screenshot%20(4663).png)

Setiap field pada header UDP memiliki ukuran sebagai berikut:
- Source Port = 2 byte
- Destination Port = 2 byte
- Length = 2 byte
- Checksum = 2 byte
Sehingga total panjang header UDP adalah 8 byte

3. Nilai yang tertera pada ”Length” menyatakan nilai apa? Verfikasi jawaban Anda melalui paket UDP pada trace

![5-3](../assets/image/Screenshot%20(4665).png)

Nilai Length pada paket UDP adalah 58 byte, sedangkan ukuran payload adalah 50 byte. Hal ini sesuai dengan struktur UDP, di mana panjang total terdiri dari header sebesar 8 byte dan payload sebesar 50 byte. Dengan demikian, nilai Length menunjukkan total ukuran keseluruhan segmen UDP

4. Berapa jumlah maksimum byte yang dapat disertakan dalam payload UDP? (Petunjuk: jawaban untuk pertanyaan ini dapat ditentukan dari jawaban Anda untuk pertanyaan 2)

Berdasarkan teori, ukuran maksimum paket IP adalah 65535 byte karena field length menggunakan 16-bit. Setelah dikurangi header IP (20 byte) dan header UDP (8 byte), maka maksimum payload UDP adalah 65507 byte

5. Berapa nomor port terbesar yang dapat menjadi port sumber? (Petunjuk: lihat petunjuk pada pertanyaan 4)

Karena port menggunakan 16-bit, maka nomor port maksimum adalah 65535

6. Berapa nomor protokol untuk UDP? Berikan jawaban Anda dalam notasi heksadesimal dan desimal. Untuk menjawab pertanyaan ini, Anda harus melihat ke bagian ”Protocol” pada datagram IP yang mengandung segmen UDP

![5-6](../assets/image/Screenshot%202026-04-15%20015410.png)

7. Periksa pasangan paket UDP di mana host Anda mengirimkan paket UDP pertama dan paket UDP kedua merupakan balasan dari paket UDP yang pertama. (Petunjuk: agar paket kedua merupakan balasan dari paket pertama, pengirim paket pertama harus menjadi tujuan dari paket kedua). Jelaskan hubungan antara nomor port pada kedua paket tersebut!

![5-7-1](../assets/image/Screenshot%20(4669).png)

![5-7-2](../assets/image/Screenshot%20(4668).png)

Terjadi hubungan sebagai berikut:
1. Paket pertama (request):
- Source Port = port client
- Destination Port = port server
2. Paket kedua (response):
- Source Port = port server
- Destination Port = port client

Hal ini menunjukkan bahwa terjadi pertukaran (swap) port antara pengirim dan penerima, sehingga data dapat dikirim kembali ke client yang benar

## Kesimpulan
Dari praktikum ini, dapat disimpulkan bahwa UDP adalah protokol yang sederhana dan cepat karena tidak menggunakan koneksi dan tidak menjamin pengiriman data seperti TCP. Header UDP terdiri dari beberapa bagian penting seperti port sumber, port tujuan, panjang data, dan checksum. Selain itu, komunikasi UDP terjadi dengan cara saling bertukar nomor port antara pengirim dan penerima. Karena sifatnya yang ringan, UDP cocok digunakan untuk layanan yang membutuhkan kecepatan tinggi, meskipun ada risiko data tidak sampai atau hilang