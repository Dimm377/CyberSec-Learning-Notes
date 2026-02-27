# TryHackMe: Packet & Frames


---

- **Room Link:** [Packet&Frames](https://tryhackme.com/room/packetsframes)
- **Category:** Networking / Fundamental
- **Difficulty:** easy

---

# Overview

Membahas gimana data dipecah jadi bagian kecil (Packets dan Frames) buat mencegah _bottleneck_ di jaringan. Kita juga belajar protokol TCP dan cara dia ngebangun koneksi lewat Three-way Handshake.

### What is Packet & Frames ?

Packet & Frames itu potongan-potongan kecil dari data. Kalau digabungin, mereka bakal jadi informasi / pesan yang lebih besar. Tapi keduanya itu dua hal yang beda di OSI Model:

1. **Packet:** Packet itu unit data dari Lapisan ke-3 OSI Model (Network Layer). Packet berisi informasi header IP.

2. **Frames:** Frames itu unit data yang ada di Lapisan ke-2 (Data Link Layer).

### Analogi Sistem Pos (Packet & Frames)

**Surat (Packet):** Isi pesan yang sudah dikasih alamat tujuan (IP Address).

**Amplop (Frame):** Surat (Packet) harus dibungkus pakai amplop agar bisa dipindahin secara fisik. Di amplop ini ada info buat rute pengiriman (MAC Address).

**Proses membungkus (Encapsulation):** Tindakan memasukkan surat ke dalam amplop, itulah yang disebut Encapsulation.

### Encapsulation

Proses membungkus data dengan informasi kontrol berupa (header dan trailer) waktu data bergerak turun dari lapisan atas ke lapisan bawah.

**Alur encapsulation:**

- **Application to Session Layer:** Data asli dibuat sama aplikasi (misalnya pesan chat atau file gambar)

- **Transport Layer (Segment):** Data dipecah jadi bagian-bagian kecil dan dikasih Header Transport (seperti nomor port TCP/UDP)

- **Network Layer (Packet):** Segmen tadi dibungkus lagi pakai IP Header yang isinya alamat IP sumber dan tujuan. Di tahap ini, unit datanya disebut Packet

- **Data Link Layer (Frame):** Packet dimasukin ke bungkusan terakhir yang berisi MAC Address. Unit data ini disebut Frame

- **Physical Layer (Bits):** Frame diubah jadi sinyal listrik atau cahaya (bit 0 dan 1) buat dikirim lewat kabel

### Why is Encapsulation Necessary?

- **Standarisasi:** Memastikan miliaran perangkat di internet punya aturan yang sama agar tidak terjadi gangguan komunikasi

- **Pencegahan Bottleneck:** Dengan mecah data jadi potongan kecil (Packet/Frame), risiko kemacetan di jaringan jadi lebih rendah dibanding mengirim satu file gede sekaligus

- **Integrity:** Setiap lapisan bisa mengecek apakah bungkusan miliknya rusak atau tidak tanpa harus ngebongkar seluruh isi datanya

### TCP/IP

**TCP/IP:** Protokol ini mirip banget sama OSI Model. Protokol TCP/IP terdiri dari empat lapisan, simpelnya ini seperti versi ringkas dari OSI Model. TCP bersifat Connection-Oriented.

Lapisan-lapisannya:

- **Application:** Lapisan paling atas tempat aplikasi beroperasi

- **Transport:** Tempat TCP ngelola pengiriman data dan memastikan reliabilitasnya

- **Internet:** Tempat perutean (routing) data pakai alamat IP

- **Network Interface:** Lapisan paling bawah yang ngurusin hubungan fisik dengan perangkat keras jaringan

### **TCP Packet Headers**

| Header                     | Description                                                                               |
| :------------------------- | :---------------------------------------------------------------------------------------- |
| **Source Port**            | Port yang dibuka sama pengirim (dipilih acak dari 0-65535 yang tidak terpakai).           |
| **Destination Port**       | Nomor port aplikasi/layanan di host tujuan (misal: port 80 buat web server).             |
| **Source IP**              | Alamat IP dari perangkat yang mengirim paket.                                              |
| **Destination IP**         | Alamat IP dari perangkat tujuan paket tersebut.                                          |
| **Sequence Number**        | Angka acak yang dikasih ke potongan data pertama yang dikirim.                           |
| **Acknowledgement Number** | Angka buat potongan data berikutnya (Sequence Number + 1) sebagai tanda terima.          |
| **Checksum**               | Nilai kalkulasi matematis buat jaga integritas data (memastikan data tidak korup).          |
| **Data**                   | Tempat menyimpan isi data aktual (bytes) dari file yang lagi dikirim.                      |
| **Flag**                   | Instruksi khusus yang menentukan gimana paket harus ditangani selama proses handshake.      |

### Three Way Handshake

Three-way handshake itu proses tiga tahap (SYN, SYN/ACK, ACK) yang dipake TCP buat ngebangun koneksi yang stabil dan sinkron antar dua perangkat sebelum data asli ditransfer.

**SYN (Synchronize):** Client mengirim paket dengan flag SYN yang berisi sequence number acak. Tujuannya memberi tau server dan ngajak sinkronisasi sebelum data dikirim.

**SYN/ACK:** Server bales dengan mengirim paket yang ngaktifin dua flags sekaligus (SYN/ACK). Tujuannya memberi tau client bahwa permintaan sinkronisasinya diterima.

**ACK:** Client mengirim paket terakhir dengan flag ACK. Tujuannya mengonfirmasi bahwa client sudah nerima nomor urut dari server. Begitu tahap ini selesai, status koneksi jadi ESTABLISHED (terjalin) dan transfer data bisa dimulai.

### **Three-Way Handshake & Communication Steps**

| Step  | Message     | Description                                                                               |
| :---- | :---------- | :---------------------------------------------------------------------------------------- |
| **1** | **SYN**     | Pesan awal buat mulai koneksi dan sinkronisasi perangkat.                                |
| **2** | **SYN/ACK** | Paket dari server buat ngakui upaya sinkronisasi dari client.                            |
| **3** | **ACK**     | Paket konfirmasi bahwa pesan/paket sudah berhasil diterima.                               |
| **4** | **DATA**    | Proses pengiriman data aktual setelah koneksi terjalin (Established).                     |
| **5** | **FIN**     | Paket buat nutup koneksi secara bersih setelah transfer data selesai.                    |
| **#** | **RST**     | Paket buat ngakhirin komunikasi secara mendadak kalau terjadi error atau masalah sistem. |
