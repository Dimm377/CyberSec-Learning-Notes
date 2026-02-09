# TryHackMe: Packet & Frames


---

- **Room Link:** [Packet&Frames](https://tryhackme.com/room/packetsframes)
- **Category:** Networking / Fundamental
- **Difficulty:** easy

---

# Overview

membahas tentang bagaimana data dipecah menjadi bagian kecil (Packets dan Frames) untuk mencegah bottleneck di jaringan. Kita juga mempelajari protokol bernama TCP dan cara ia membangun koneksi melalui Three-way Handshake.

### What is Packet & Frames ?

Paket & Frames adalah bagian kecil dari sebuah data yang mana ketika digabungkan akan membentuk suatu informasi / pesan yang lebih besar, tapi keduanya merupakan dua hal yang berbeda di dalam OSI Model

1. **Packet:** Packet adalah bagian data dari Lapisan ke 3 OSI Model (Network Layer), Packet berisi tentang informasi header IP,

2. **Frames:** Frames adalah unit data yang ada di lapisan ke 3 (Data link layer)

### Analogi Sistem pos (packet & frames)

**Surat (Packet):** Isi pesan yang sudah diberi alamat tujuan (IP Address)

**Amplop (Frame):** isi surat (Packet) harus dibungkus menggunakan amplop agar bisa dipindahkan secara fisik, di amplop ini punya informasi untuk rute pengiriman (MAC Address)

**Proses membungkus (Encapsulation):** Tindakan memasukkan surat ke dalam amplop yang disebut Encapsulation

### Encapsulation

Proses membungkus data dengan informasi kontrol berupa (header dan trailer) pada saat data tersebut bergerak turun dari lapisan atas ke lapisan bawah

**alur encapsulation:**

- **Application to Session Layer:** Data asli dibuat oleh aplikasi (misalnya pesan chat atau file gambar)

- **Transport Layer (Segment):** Data dipecah menjadi bagian-bagian kecil dan diberikan Header Transport (seperti nomor port TCP/UDP)

- **Network Layer (Packet):** Segmen tadi dibungkus lagi dengan IP Header yang berisi alamat IP sumber dan tujuan. Di tahap ini, unit datanya disebut Packet

- **Data Link Layer (Frame):** Packet dimasukkan ke dalam bungkusan terakhir yang berisi MAC Address,Unit data ini disebut Frame

- **Physical Layer (Bits):** Frame diubah menjadi sinyal listrik atau cahaya (bit 0 dan 1) untuk dikirim lewat kabel.

### Why is Encapsulation Necessary?

- **Standarisasi:** Memastikan miliaran perangkat di internet punya aturan yang sama agar tidak terjadi gangguan komunikasi

- **Pencegahan Bottleneck:** Dengan memecah data jadi potongan kecil (Packet/Frame), risiko kemacetan di jaringan jadi jauh lebih rendah dibanding mengriim satu file besar sekaligus

- **Integrity:** Setiap lapisan bisa ngecek apakah bungkusan miliknya rusak atau nggak tanpa harus ngebongkar seluruh isi datanya

### TCP/IP

**TCP/IP:** Protokol ini sangat mirip dengan OSI model, Protokol TCP/IP terdiri dari empat lapisan, simple nya ini kayak versi ringkasan dari OSI Model, TCP bersifat Connection-Oriented

Lapisan-lapisan nya terdiri dari:

- **Application:** Lapisan paling atas tempat aplikasi beroperasi

- **Transport:** Tempat di mana TCP mengelola pengiriman data dan memastikan reliabilitasnya

- **Internet:** Tempat perutean (routing) data menggunakan alamat IP

- **Network Interface:** Lapisan paling bawah yang menangani hubungan fisik dengan perangkat keras jaringan

### **TCP Packet Headers**

| Header                     | Description                                                                               |
| :------------------------- | :---------------------------------------------------------------------------------------- |
| **Source Port**            | Port yang dibuka oleh pengirim (dipilih acak dari 0-65535 yang tidak terpakai).           |
| **Destination Port**       | Nomor port aplikasi/layanan di host tujuan (misal: port 80 untuk web server).             |
| **Source IP**              | Alamat IP dari perangkat yang mengirimkan paket.                                          |
| **Destination IP**         | Alamat IP dari perangkat yang dituju oleh paket tersebut.                                 |
| **Sequence Number**        | Angka acak yang diberikan pada potongan data pertama yang dikirimkan.                     |
| **Acknowledgement Number** | Angka untuk potongan data berikutnya (Sequence Number + 1) sebagai tanda terima.          |
| **Checksum**               | Nilai kalkulasi matematis untuk menjaga integritas data (memastikan data tidak korup).    |
| **Data**                   | Tempat penyimpanan isi data aktual (bytes) dari file yang sedang dikirim.                 |
| **Flag**                   | Instruksi khusus yang menentukan bagaimana paket harus ditangani selama proses handshake. |

### Three Way handshake

three way handshake adalah proses tiga tahap (SYN, SYN/ACK, ACK) yang digunakan oleh TCP untuk membangun koneksi yang stabil dan sinkron antara kedua perangkat sebelum data asli di transfer

**SYN (Synchronize):** Client mengirim dengan flag SYN yang berisi sequence number acak
tujuannya memeberitahu server dan mengajak sinkronisasi sebelum data dikirim

**SYN/ACK:** Servere membalas dengan mengirim paket yang mengaktifkan dua flags sekaligus (SYN/ACK), Tujuan nya yaitu mmeberitahu client bahwa permintaan sinkronisasi nya diterima

**ACK:** Client mengirim packet terakhir dengan flags ACK, tujuannya mengonfirmasi bahwa client sudah menerima nomor urut dari server. Begitu tahap ini selesai, status koneksi resmi menjadi ESTABLISHED (terjalin) dan transfer data asli bisa dimulai

### **Three-Way Handshake & Communication Steps**

| Step  | Message     | Description                                                                               |
| :---- | :---------- | :---------------------------------------------------------------------------------------- |
| **1** | **SYN**     | Pesan awal untuk memulai koneksi dan menyinkronkan perangkat.                             |
| **2** | **SYN/ACK** | Paket dari server untuk mengakui upaya sinkronisasi dari client.                          |
| **3** | **ACK**     | Paket konfirmasi bahwa pesan/paket telah berhasil diterima.                               |
| **4** | **DATA**    | Proses pengiriman data aktual setelah koneksi terjalin (Established).                     |
| **5** | **FIN**     | Paket untuk menutup koneksi secara bersih setelah transfer data selesai.                  |
| **#** | **RST**     | Paket untuk mengakhiri komunikasi secara mendadak jika terjadi error atau masalah sistem. |
