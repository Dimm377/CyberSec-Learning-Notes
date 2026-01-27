# TryHackMe: OSI Model

**(My favourite Part)**

**Room Link:** [OSI Model](https://tryhackme.com/room/osimodelzi)
**Category:** Networking / Fundamental
**Difficulty:** easy

---

## Overview

- OSI Model adalah kerangka konsep yang membuat sistem komunikasi di jaringan memiliki bahasa yang sama, Tujuan nya adalah memastikan sistem agar bisa saling berkomunikasi memakai protokol yang sudah disepakati secara global

### What is OSI Model ?

OSI Model (Open System Interconnection Model) adalah kerangka konsep yang membagi jaringan menjadi 7 lapisan: Physical, Data Link, Network, Transport, Session, Presentation, Application

#### 7 layer OSI Model:

<p align="center">
<img src="../../Assets/Images/OSI.png" width="400">
</p>

---

### Penjelasan setiap Layer OSI model:

1. **Physical:** Lapisan Pertama yang bertanggung jawab untuk mentransmisikan data secara fisik melalui media transmisi seperti kabel ethernet, Sinyal listrik, Kabel optic, Modem, dan Antena.

2. **Data Link:** Lapisan kedua yang berfokus pada pengalamatan fisik transmisi data, lapisan ini menerima data dari lapisan jaringan dan menambahkan MAC Address agar data perangkat saling mengenali, simple nya lapisan ini memebantu perangkat yang terhubung ke jaringan agar bisa saling berkomunikasi.

3. **Network:** Lapisan ketiga yang bertugas untuk mengarahkan data dari satu perangkat ke perangkat lainnya di jaringan yang berbeda, menggunakan IP Address untuk menentukan rute terbaik untuk mengirimkan data di jaringan yang kompleks.

Ada 5 protokol dari yang ku pelajari yaitu:

- **IP (Internet Protocol):** Protokol untuk pengalamatan dan pengiriman data di dalam jaringan.
- **ICMP (Internet Control Message Protocol):** Protokol untuk mengirim pesan kontrol dan error antara perangkat jaringan.
- **ARP (Address Resolution Protocol):** Protokol yang menghubungkan / menerjemahkan IP Address ke MAC Address.
- **OSPF (Open Shortest Path First):** Protokol _link-state_ yang menentukan jalur tercepat berdasarkan _bandwidth_ (cost). OSPF sangat efisien karena memiliki peta lengkap topologi jaringan.
- **RIP (Routing Information Protocol):** Protokol _distance-vector_ yang menggunakan jumlah lompatan router (**hop count**) untuk menentukan jalur terbaik. Protokol ini memiliki batas maksimal 15 lompatan (hop).

4. **Transport:** Lapisan keempat yang bertugas untuk mengelola pengiriman data antar perangkat dan memastikan data diterima dengan benar, unit data di lapisan ini disebut segmen bukan lagi packet.

Terdapat 2 protokol di lapisan Transport yaitu:

- **TCP (Transmission Control Protocol):** Protokol yang menyediakan pengiriman data secara handal dan teratur dengan menjamin bahwa data sampai ke tujuan dalam urutan yang benar.
- **UDP (User Datagram Protocol):** Protokol yang lebih cepat dari TCP tapi tidak menjamin pengiriman data yang handal dan urutan data yang teratur.

| Perbedaan             | TCP (Transmission Control Protocol)      | UDP (User Datagram Protocol)             |
| :-------------------- | :--------------------------------------- | :--------------------------------------- |
| **Keandalan**         | Sangat Handal (Reliable)                 | Tidak Handal (Unreliable)                |
| **Koneksi**           | Connection-oriented                      | Connectionless                           |
| **Kecepatan**         | Lebih Lambat (Banyak proses pengecekan)  | Sangat Cepat (Tanpa hambatan)            |
| **Pengiriman Ulang**  | Mengirim ulang data yang hilang/rusak    | Tidak mengirim ulang data yang hilang    |
| **Urutan Data**       | Data diterima sesuai urutan yang dikirim | Data bisa diterima berantakan/tidak urut |
| **Contoh Penggunaan** | Web (HTTP), Email, Transfer File         | Streaming Video, Game Online, VoIP       |

5. **Session:** Lapisan kelima ini fungsinya untuk mengelola sesi antar 2 perangkat, menetapkan, memepertahankan, dan mengakhiri koneksi agar aplikasi berkomunikasi dengan lancar, simple nya di lapisan Session ini membantu menjaga komunikasi yang teratur antara aplikasi yang berbeda.

Protokol yang ada di lapisan ini yaitu:

- **RCP (Remote Procedure Call):** Protokol ini memungkinkan suatu program untuk meminta layanan dari program lain di komputer yang berbeda jaringan, intinya menyederhanakan komunikasi antar aplikasi tanpa harus mengelola detail jaringan.
- **SMB (Server Message Block):** Protokol ini digunakan untuk berbagi file, printer, dan sumber daya lainnya di dalam jaringan, dengan adanya SMB kita dapat mengelola dan mengakses data dengan mudah di jaringan tanpa harus berpindah tempat.

6. **Presentation:** Lapisan keenam ini bertindak sebagai penerjemah data agar dapat dipahami oleh aplikasi / lapisan ke 7 (Application), memastikan bahwa data disajikan dalam format yang benar agar dapat di proses dengan baik, lapisan ini juga menangani enkripsi dan dekripsi untuk keamanan.

7. **Application:** Lapisan ketujuh merupakan lapisan yang paling dekat dengan pengguna, menyediakan layanan jaringan untuk aplikasi perangkat lunak seperti web browser (HTTP/HTTPS) atau email.
