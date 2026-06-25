# Laporan Praktikum Modul 13 (Ethernet and ARP)

Ethernet adalah teknologi jaringan yang digunakan pada LAN dan bekerja pada lapisan Data Link (Layer 2) dengan menggunakan MAC Address sebagai identitas perangkat. Sementara itu, ARP (Address Resolution Protocol) berfungsi untuk menerjemahkan alamat IP menjadi alamat MAC. Jika alamat MAC tujuan belum diketahui, perangkat akan mengirim ARP Request dan perangkat tujuan akan membalas dengan ARP Reply yang berisi alamat MAC-nya

## Tujuan Praktikum
1. Dapat menginvestigasi cara kerja Ethernet and ARP menggunakan Wireshark

## Hasil Capture Wireshark

![13-1](../assets/image/arp%20(8).png)

Paket ini merupakan permintaan (request). Pada paket ini, perangkat Huawei (192.168.100.1) hendak melakukan komunikasi dengan perangkat beralamat IP 192.168.100.10, namun belum mengetahui alamat MAC-nya. Oleh karena itu, dikirimkanlah ARP Request untuk menanyakan alamat MAC yang berasosiasi dengan IP tersebut

![13-2](../assets/image/arp%20(6).png)

Perangkat Intel (192.168.100.10) mengirimkan ARP Reply secara unicast langsung ke Huawei, dengan isi pesan "192.168.100.10 is at dc:45:46:14:b1:8a". Dengan demikian, Huawei kini telah mengetahui alamat MAC milik Intel

![13-1](../assets/image/arp%20(5).png)

Kali ini perangkat Intel (192.168.100.10) yang mengirimkan ARP Request secara broadcast dengan isi pesan "Who has 192.168.100.1? Tell 192.168.100.10". Permintaan ini muncul karena Intel membutuhkan kembali konfirmasi alamat MAC dari Huawei

![13-1](../assets/image/arp%20(7).png)

Sebagai balasannya, perangkat Huawei (192.168.100.1) mengirimkan ARP Reply secara unicast ke Intel, dengan isi pesan "192.168.100.1 is at 24:46:e4:e4:0b:a9"

## Kesimpulan
Dari praktikum ini, dapat disimpulkan bahwa proses ARP selalu terjadi secara berpasangan: request terlebih dahulu, baru kemudian disusul dengan reply. Pada tahap pertama, Huawei yang memulai dengan mengirim request, sedangkan pada tahap kedua, Intel yang memulai dengan mengirim request