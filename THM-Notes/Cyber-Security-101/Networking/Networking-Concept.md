# TryHackMe: Networking Concepts


---

- **Room Link:** [Network concepts](https://tryhackme.com/room/networkingconcepts)
- **Category:** Cyber Security 101 path
- **Difficulty:** Easy

---

## Overview

### OSI Model

**OSI Model** itu kerangka kerja konseptual yang membagi proses komunikasi jaringan jadi **7 layer**,Bayangkan proses **mengirim surat lewat kantor pos** — setiap layer punya tugas spesifik, mulai dari menulis isi surat sampai kurir mengantarkannya ke pintu rumah.

| Layer | Nama | Analogi Surat | Fungsi |
| :---: | ---- | ------------- | ------ |
| 7 | **Application** | Isi surat yang kamu tulis | Interface langsung ke pengguna (HTTP, FTP, DNS) |
| 6 | **Presentation** | Bahasa surat (diterjemahkan/dienkripsi) | Encoding, kompresi, dan enkripsi data |
| 5 | **Session** | Percakapan telepon (buka-tutup sambungan) | Mengelola sesi komunikasi antar aplikasi |
| 4 | **Transport** | Pilih layanan pengiriman (kilat/reguler) | Komunikasi _end-to-end_ antar aplikasi (TCP/UDP) |
| 3 | **Network** | Alamat tujuan (kota, kode pos) | Merutekan paket ke jaringan yang tepat (IP Address) |
| 2 | **Data Link** | Alamat RT/RW setempat | Perpindahan data antar perangkat di segmen jaringan yang sama (MAC Address) |
| 1 | **Physical** | Kurir, jalan raya, dan truk | Transmisi bit mentah lewat media fisik (kabel, wifi) |

> **Mnemonic buat menghafal urutan dari bawah ke atas:** **P**lease **D**o **N**ot **T**hrow **S**pinach **P**izza **A**way (Physical, Data Link, Network, Transport, Session, Presentation, Application)

(Penjelasan lebih detail per-layer ada di catatan [OSI Model](file:///home/dimm/CyberSec-Learning-Notes/THM-Notes/Pre-Security/Networking-fundamental/OSI-Model.md) dan alur resolusi nama domain di [DNS In Details](file:///home/dimm/CyberSec-Learning-Notes/THM-Notes/Pre-Security/How-The-Web-Works/DNS-In-Details.md))


---

### TCP/IP Model

Beda sama OSI Model yang sifatnya konseptual (teori), **TCP/IP** itu model yang **benar-benar diimplementasikan** di internet saat ini. TCP/IP menyederhanakan 7 layer OSI jadi 4 layer saja:

| TCP/IP Layer | Setara OSI Layer | Contoh Protokol |
| ------------ | ---------------- | --------------- |
| **Application** | Session + Presentation + Application | HTTP, FTP, DNS, SMTP |
| **Transport** | Transport | TCP, UDP |
| **Internet** | Network | IP, ICMP |
| **Network Access** | Data Link + Physical | Ethernet, Wi-Fi |

**Encapsulation:** Setiap layer menambahkan header ke data dari layer di atasnya sebelum dikirim ke layer bawah — ibarat surat yang dimasukkan amplop, lalu amplop dimasukkan ke dalam paket, lalu paket dimasukkan ke dalam kontainer.

---

### IP Addresses & Subnets

Setiap perangkat di jaringan butuh **identitas unik** — ibarat alamat rumah. Tanpa alamat, data ga tau harus dikirim kemana.

| Tipe | Penjelasan |
| ---- | ---------- |
| **IPv4** | Alamat 32-bit yang dibagi jadi 4 oktet (contoh: `192.168.1.1`) |
| **Private IP** | Alamat yang hanya dipakai di jaringan lokal (LAN), tidak bisa diakses langsung dari internet |

**Range Private IP yang wajib diingat:**

| Range | CIDR | Jumlah Alamat |
| ----- | ---- | ------------- |
| `10.0.0.0` – `10.255.255.255` | `/8` | ~16 juta |
| `172.16.0.0` – `172.31.255.255` | `/12` | ~1 juta |
| `192.168.0.0` – `192.168.255.255` | `/16` | ~65 ribu |

---

### TCP, UDP, and Ports

Dua protokol transport utama yang menentukan **bagaimana data dikirim**:

| Aspek | **TCP** | **UDP** |
| ----- | ------- | ------- |
| **Analogi** | Telepon (harus tersambung dulu baru bicara) | Walkie-talkie (langsung bicara tanpa peduli didengar atau ga) |
| **Orientasi** | Connection-oriented (wajib handshake) | Connectionless (langsung kirim) |
| **Reliabilitas** | Reliable — dijamin sampai | Unreliable — ga dijamin sampai |
| **Handshake** | Three-way: SYN → SYN-ACK → ACK | Tidak ada |
| **Data Unit** | **Segment** | **Datagram** |
| **Use Case** | Web browsing, email, file transfer | Streaming video, gaming, DNS query |

**Ports** — ibarat **nomor kamar di apartemen**. Satu gedung (IP Address) bisa punya banyak kamar (port) yang masing-masing menampung layanan berbeda. Total port tersedia: **65.535**.

---

### Connecting to a Web Server

Tools dasar buat berinteraksi dengan server:

| Tool | Analogi | Fungsi |
| ---- | ------- | ------ |
| **Netcat (nc)** | _Swiss-army knife_ jaringan | Membaca atau menulis data di koneksi jaringan — sangat fleksibel |
| **Telnet** | Telepon jadul | Protokol lama buat akses _remote_ berbasis teks, sekarang sering dipakai untuk mengecek apakah sebuah port TCP terbuka |
