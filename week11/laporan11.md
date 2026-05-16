# Laporan Praktikum Modul 11 (DHCP)

DHCP merupakan protokol jaringan yang digunakan untuk memberikan alamat IP secara otomatis kepada perangkat dalam jaringan. Selain alamat IP, DHCP juga dapat memberikan informasi lain seperti subnet mask, default gateway, DNS server. DHCP bekerja dengan model client-server. Server DHCP akan menyediakan konfigurasi jaringan, sedangkan client akan meminta alamat IP ketika terhubung ke jaringan

## Tujuan Praktikum
1. Dapat menginvestigasi cara kerja protokol DHCP menggunakan Wireshark.

## Langkah Percobaan
### Percobaan UDP
1. Membuat program client (udp-client.py)

```python
# * import semua method yang ada di socket
from socket import *

#  kampus, kost, rumah
serverName = "localhost"
serverPort = 12000 # port koneksi

# AF_INET = ip addr v4 | SOCK_DGRAM = UDP
clientSocket = socket(AF_INET, SOCK_DGRAM)

# selama running = True, program akan berjalan
running = True
while running:
    message = input("> ") # input dari user

    # EXIT, Exit, eXIT
    if message.lower() == "exit":
        clientSocket.sendto(
            message.encode(),
            (serverName, serverPort)
        )

        print("[SYSTEM] Keluar dari program")
        running = False
        continue

    # abc, Namaku, ayam, keonho
    clientSocket.sendto(
        # jarkom = 10101010101010101
        message.encode(),
        # (x,y)
        (serverName, serverPort)
    )

    # menerima pesan
    modifiedMessage, serverAdress = clientSocket.recvfrom(2048)

    print("[SYSTEM] Pesan telah diterima dari: ", serverAdress)
    print(modifiedMessage.decode()) # kalau ngirim atau menerima data janlup di decode atau incode gtu gtu

# indentasi 0
clientSocket.close()
print("[SYSTEM] Koneksi telah ditutup")
```

2. Membuat program server (udp-server.py)

```python
from socket import *

# membuat socket untuk srever
serverPort = 12000
serverSocket = socket(AF_INET, SOCK_DGRAM)

# menghubungkan (bind)
serverSocket.bind(
    # tuple
    ('' ,serverPort)
)

print("[SERVER] server siap digunakan")

# dijalankan, selama running bernilai true
running = True
while running:
    message, clientAddress = serverSocket.recvfrom(2048)
    # message yang diterima = 10101010101
    decodeMessage = message.decode()

    # jika pesan = 'exit'
    if decodeMessage.lower() == "exit":
        print("[SYSTEM] server telah diberhentikan")
        running = True
        continue

    # mengcapslock
    modifiedMessage = decodeMessage.upper()
    print("[SYSTEM] diterima dari ", clientAddress, " message: ", decodeMessage)

    # mengirim ke klien
    serverSocket.sendto(
        modifiedMessage.encode(),
        clientAddress
    )

serverSocket.close()
print("[SYSTEM] socket server telah ditutup")
```

### Percobaan TCP
1. Membuat program client (tcpClient.py)

```python
from socket import *

# server port
serverName = 'localhost'
serverPort = 8080

clientSocket = socket(AF_INET, SOCK_STREAM)

clientSocket.connect((serverName, serverPort))

sentence = input("input lowercase sentence: ")

clientSocket.send(sentence.encode())

modifiedSentence = clientSocket.recv(2048)

print('From server: ', modifiedSentence.decode())

clientSocket.close()
```

2. Membuat program server (tcpServer.py)

```python
from socket import *

serverPort = 8080

serverSocket = socket(AF_INET, SOCK_STREAM)

serverSocket.bind(('', serverPort))

serverSocket.listen(5)
print('Server siap menerima koneksi client...')

try:
    while True:
        try:
            connectionSocket, addr = serverSocket.accept()
            print('Koneksi diterima dari: ', addr)

            sentence = connectionSocket.recv(2048).decode()

            print('Pesan diterima: ', sentence)

            modifiedSentence = sentence.upper()

            connectionSocket.send(modifiedSentence.encode())

            connectionSocket.close()

        except timeout:
            continue
        
except KeyboardInterrupt:
    print('[SYSTEM] server dihentikan oleh pengguna')

finally:
    serverSocket.close()
    print('[SYSTEM] socket server telah ditutup')
```

### Penjelasan Singkat
- socket(AF_INET, SOCK_STREAM) : membuat socket TCP
- connect() : menghubungkan ke server
- send() : mengirim data ke server
- recv() : menerima balasan dari server
- close() : menutup koneksi

## Hasil Percobaan
1. Hasil UDP

![7-1](../assets/image/Screenshot%20(4872).png)

2. Hasil TCP

![7-2](../assets/image/Screenshot%20(4876).png)

## Kesimpulan
Dari praktikum ini, dapat disimpulkan bahwa socket programming memungkinkan komunikasi antara client dan server dalam jaringan menggunakan protokol UDP dan TCP, di mana UDP bersifat cepat tanpa koneksi dan tidak menjamin pengiriman data, sedangkan TCP lebih andal karena menggunakan koneksi dan menjamin data sampai dengan urutan yang benar. Praktikum ini membantu memahami konsep dasar komunikasi jaringan secara langsung melalui implementasi program

![11](../assets/image/Screenshot%20(5028).png)