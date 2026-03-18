# TryHackMe: Client-Server Basics

- **Room Link:** [Client-Server Basics](https://tryhackme.com/room/client-serverbasics)
- **Category:** Pre-Security
- **Difficulty:** Easy

## Introduction

Bayangkan sebuah dunia di mana setiap orang hidup sendirian di pulau terpencil. Mereka punya makanan sendiri, alat sendiri, dan tidak pernah berbicara dengan siapa pun. Itulah gambaran komputer di masa awal: mereka bekerja sendiri, menyimpan file sendiri, dan tidak bisa berkomunikasi dengan mesin lain.

Namun, dunia berubah saat organisasi-organisasi mulai berpikir: *"Bagaimana kalau kita sambungkan pulau-pulau ini supaya kita bisa tukar makanan dan alat tanpa peduli jarak?"* Dari sinilah cikal bakal **Internet** lahir melalui jaringan legendaris seperti **ARPANET**, **CYCLADES**, dan **NSFNET**.

Sama seperti masyarakat manusia di mana setiap orang punya keahlian khusus dan menawarkan jasa (seperti dokter, koki, atau montir), sistem komputer yang saling terhubung juga mulai **berespesialisasi**. Ada yang jago menyimpan data, ada yang jago melayani permintaan kita. 

Di room ini, kamu akan mempelajari bagaimana client-server bekerja.

### Learning Objectives

Setelah menyelesaikan room ini, kamu akan memahami:
*   Model **Client-Server** (dasar dari hampir semua interaksi di internet).
*   Konsep-konsep dasar seperti **DNS**, **Client**, **Server**, **Port**, **Protocol**, dan **Network**.

> **for your information:**
> **ARPANET** (_Advanced Research Projects Agency Network_) — Jaringan komputer pertama di dunia yang menggunakan protokol paket data, dikembangkan oleh Departemen Pertahanan AS sebagai fondasi internet yang kita kenal sekarang.
> **CYCLADES** — Jaringan riset Prancis tahun 1970-an yang menjadi pelopor dalam membebankan tanggung jawab pengiriman data pada pengirim/penerima (host), bukan pada jaringan itu sendiri.
> **NSFNET** (_National Science Foundation Network_) — Jaringan tulang punggung (*backbone*) di AS yang dibuat pada pertengahan 1980-an untuk menghubungkan pusat-pusat superkomputer dan menjadi pondasi infrastruktur internet modern.

## Pizza Delivery

Cara termudah untuk memahami bagaimana sistem komputer memberikan layanan (*service*) ke sistem lain adalah dengan menggunakan analogi dunia nyata. Mari kita bayangkan proses memesan **Pizza Luigi**.

Malam Sabtu tiba, Alice dan Bob ingin makan pizza.
1.  **Menu**: Alice melihat menu Pizza Luigi dan memilih pizza yang dia mau.
2.  **GPS**: Bob masuk ke mobil, memasukkan alamat Luigi ke GPS, dan mulai menyetir.
3.  **Order**: Begitu sampai, Bob masuk ke toko dan memesan: *"Satu pepperoni pizza ukuran besar dan satu cola."*
4.  **Acknowledge**: Karyawan toko mencatat pesanan dan mulai membuat pizzanya.
5.  **Enjoy**: Setelah pesanan siap, Bob pulang dan menikmati malam pizza yang indah bersama Alice.

Proses ini terlihat sangat sederhana dan otomatis bagi kita. Tapi jangan khawatir, karena kita sudah terbiasa memesan makanan, kita sering tidak menyadari langkah-langkah detail di baliknya. Mari kita bedah setiap langkah tersebut dan hubungkan dengan bagaimana sistem komputer berinteraksi.

---

### Mapping Pizza to Computers

Dalam dunia *Cyber Security* dan *Networking*, setiap langkah di atas punya penjelasan teknisnya:

![Model Client-Server](file:///home/dimm/CyberSec-Learning-Notes/THM-Notes/Assets/Images/Client-server.png)

| Langkah memesan Pizza | Istilah Komputer | Penjelasan |
| :--- | :--- | :--- |
| **Alamat Luigi** | **IP Address** | Lokasi unik di jaringan agar paket data tahu harus ke mana. |
| **GPS** | **DNS** | Mengubah nama yang manusia mengerti (Pizza Luigi) menjadi alamat yang komputer mengerti (IP). |
| **Menu** | **Service/Content** | Apa yang tersedia untuk diminta oleh user. |
| **Pemesanan** | **Request** | Pesan yang dikirim oleh Client untuk mendapatkan layanan tertentu. |
| **Konfirmasi** | **Response** | Jawaban dari Server yang menyatakan permintaan diterima atau ditolak. |
| **Makan Pizza** | **Data Transfer** | Hasil akhir yang diterima oleh user (halaman web, file, dll). |

---

Setelah memahami analoginya, mari kita masuk ke mekanisme teknis bagaimana komponen-komputer ini bekerja secara nyata.

#### 1. Client & Server
Dua peran utama dalam komunikasi jaringan:
*   **Client**: Perangkat atau software yang **menginisiasi** permintaan (misal: Browser di laptopmu).
*   **Server**: Sistem yang **mendengarkan** (*listening*) dan melayani permintaan tersebut (misal: Web Server THM).

#### 2. Request & Response
Mekanisme pertukaran data yang harus terstruktur:
*   **Request**: Harus diformat dengan benar agar Server paham sumber daya mana yang diminta.
*   **Response**: Jawaban dari Server yang bisa berupa data produk (Sukses) atau pesan kesalahan (Error) jika sumber daya tidak ditemukan atau permintaan salah format.

#### 3. Protocol: Rules of Communication
Kumpulan aturan yang mendefinisikan bagaimana Client dan Server berbicara. **Protocol** menentukan:
*   Sintaks dan perintah yang dimengerti (misal: `GET`, `POST`).
*   Bagaimana pesan harus disusun (urutan data).
*   Bagaimana menangani kesalahan jika komunikasi terputus.

#### 4. Port: Identifying Services
Sebuah angka identitas unik (0-65535) yang digunakan untuk menentukan layanan mana yang ingin diakses pada sebuah server. 
*   Satu Server bisa menjalankan banyak layanan sekaligus (Web di port 80/443, Email di port 25, dll).
*   Client harus menghubungkan ke **Port** yang benar agar sampai ke layanan yang tepat.

#### 5. DNS: Domain Name System
Sistem yang memetakan nama domain yang mudah diingat manusia menjadi **IP Address** yang dimengerti mesin. Tanpa DNS, kamu harus menghafal angka seperti `104.26.11.210` untuk membuka sebuah website.

> **for your information:**
> **IP Address** (_Internet Protocol Address_) — Label numerik unik yang diberikan kepada setiap perangkat di jaringan untuk identifikasi dan lokasi komunikasi.

---

### Role in Cyber Security (Real-World Relevance)

Memahami model ini sangat krusial karena setiap komponen adalah titik serangan potensial:
*   **DNS Spoofing**: Penyerang memanipulasi DNS agar mengarahkan user ke server palsu (seperti mengarahkan Bob ke toko pizza palsu).
*   **DoS (Denial of Service)**: Membanjiri server dengan jutaan *Request* palsu hingga server tidak sanggup merespons pengguna asli.
*   **Port Scanning**: Hacker mencoba mengetuk semua "pintu" (Port) di server untuk mencari layanan yang punya celah keamanan terbuka.
*   **MitM (Man-in-the-Middle)**: Penyerang mencegat komunikasi antara Client dan Server untuk mencuri data sensitif di tengah jalan.
