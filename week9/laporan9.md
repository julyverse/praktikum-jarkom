# Laporan Praktikum Modul 9 (Web Server)

Web server merupakan perangkat lunak yang berfungsi menerima permintaan (request) dari client melalui protokol HTTP kemudian mengirimkan respons berupa halaman web atau file lainnya

## Tujuan Praktikum
1. Mahasiswa bisa membuat program web server sederhana berbasis TCP socket programming

## Langkah Percobaan

### Kode Python untuk web server yang saya buat

```python
from socket import *
import sys

# membuat socket server menggunakan TCP untuk komunikasi jaringan
serverSocket = socket(AF_INET, SOCK_STREAM)

# nomor port yang digunakan server
serverPort = 6789

# mengaitkan socket dengan alamat IP dan port
serverSocket.bind(('', serverPort))

# server siap menerima koneksi (maksimal 1 koneksi)
serverSocket.listen(1)

while True:
    # untuk menandakan server sedang aktif dan menunggu permintaan
    print('Ready to serve...')
    
    # menerima koneksi yang masuk dari client (browser)
    connectionSocket, addr = serverSocket.accept()

    try:
        # menerima dan membaca request dari browser dan diubah jadi teks biar bisa dibaca
        message = connectionSocket.recv(1024).decode()

        # mengambil nama file yang diminta dari request yang dikirim browser
        filename = message.split()[1]

        # membuka file HTML yang diminta browser
        f = open(filename[1:])

        # membaca isi file HTML
        outputdata = f.read()
        
        # mengirim response ke browser bahwa permintaan berhasil diproses
        connectionSocket.send("HTTP/1.1 200 OK\r\n\r\n".encode())

        # mengirim isi file HTML ke browser
        for i in range(len(outputdata)):
            connectionSocket.send(outputdata[i].encode())

        # menutup koneksi setelah selesai
        connectionSocket.close()

    except IOError:
        # mengirim status error jika file tidak ditemukan
        connectionSocket.send("HTTP/1.1 404 Not Found\r\n\r\n".encode())

        # menampilkan pesan error di browser
        connectionSocket.send("404 Not Found".encode())

        # menutup koneksi
        connectionSocket.close()
        
serverSocket.close()
sys.exit()
```

### kode html

```html
<html lang="en">
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello World - Julia Firdaus Azzahra</title>
</head>
<body>
    <h1>Hello World!</h1>
    <p> Hai </p>
</body>
</html>
```

### Hasil Percobaan

![9-1](../assets/image/Screenshot%202026-05-16%20230910.png)

## Latihan Tambahan

### Kode Server

```python
from socket import *
from threading import Thread
import sys

def handle_client(connectionSocket):
    try:
        message = connectionSocket.recv(1024).decode()
        filename = message.split()[1]

        f = open(filename[1:], encoding='utf-8')
        outputdata = f.read()

        connectionSocket.send("HTTP/1.1 200 OK\r\n\r\n".encode())

        # kirim seluruh isi file sekaligus
        connectionSocket.send(outputdata.encode('utf-8'))

        connectionSocket.close()

    except IOError:
        connectionSocket.send("HTTP/1.1 404 Not Found\r\n\r\n".encode())
        connectionSocket.send("<h1>404 Not Found</h1>".encode())
        connectionSocket.close()


serverSocket = socket(AF_INET, SOCK_STREAM)
serverPort = 6789

serverSocket.bind(('', serverPort))
serverSocket.listen(5)

print("Server Ready...")

while True:
    connectionSocket, addr = serverSocket.accept()
    print("Connected by:", addr)

    clientThread = Thread(
        target=handle_client,
        args=(connectionSocket,)
    )
    clientThread.start()

serverSocket.close()
sys.exit()
```

### Kode Client

```python
from socket import *
import sys

server_host = sys.argv[1]
server_port = int(sys.argv[2])
filename = sys.argv[3]

clientSocket = socket(AF_INET, SOCK_STREAM)
clientSocket.connect((server_host, server_port))

request = f"GET /{filename} HTTP/1.1\r\nHost: {server_host}\r\n\r\n"
clientSocket.send(request.encode())

response = clientSocket.recv(4096)
print(response.decode())

clientSocket.close()
```

### Kode HTML

```html
<html>
<head>
    <title>hai</title>
</head>

<body>
    <h1>Powered by matcha</h1>
</body>
</html>
```

### Hasil Percobaan

![9-2](../assets/image/Screenshot%202026-05-16%20233932.png)

## Kesimpulan
Dari praktikum ini, dapat disimpulkan bahwa web server dapat dibuat menggunakan TCP socket programming untuk menerima request HTTP, membaca file, dan mengirimkan respons ke client. Dengan penggunaan multithreading, server mampu melayani beberapa client secara bersamaan sehingga proses komunikasi menjadi lebih efisien