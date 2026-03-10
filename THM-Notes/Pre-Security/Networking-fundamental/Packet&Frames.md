# TryHackMe: Packet & Frames


---

- **Room Link:** [Packet&Frames](https://tryhackme.com/room/packetsframes)
- **Category:** Networking / Fundamental
- **Difficulty:** easy

---

## Overview

Room ini membahas bagaimana data dipecah jadi bagian kecil (Packets dan Frames) buat mencegah _bottleneck_ di jaringan, serta cara TCP membangun koneksi lewat Three-way Handshake.

(Konsep layer OSI yang menjadi dasar enkapsulasi ini dijelaskan lebih detail di catatan [OSI Model](file:///home/dimm/CyberSec-Learning-Notes/THM-Notes/Pre-Security/Networking-fundamental/OSI-Model.md))

---

### What are Packets & Frames?

Packets dan Frames itu **potongan-potongan kecil dari data**. Kalau digabungkan, mereka membentuk informasi/pesan yang lebih besar. Keduanya berbeda di OSI Model:

| Konsep | Layer OSI | Analogi Sistem Pos | Isi |
| ------ | :-------: | ------------------ | --- |
| **Packet** | Layer 3 (Network) | **Surat** — berisi pesan dengan alamat tujuan | Data + IP Header (alamat IP sumber & tujuan) |
| **Frame** | Layer 2 (Data Link) | **Amplop** — membungkus surat agar bisa dikirim fisik | Packet + MAC Address (rute lokal) |

**Encapsulation** = proses memasukkan surat ke dalam amplop.

---

### Encapsulation (Proses Pembungkusan Data)

Data dibungkus lapis demi lapis saat bergerak **turun** dari lapisan atas ke bawah:

| Tahap | Layer | Data Unit | Yang Ditambahkan |
| ----- | ----- | --------- | ---------------- |
| 1 | Application → Session | **Data** | Data asli dibuat oleh aplikasi (pesan chat, file gambar) |
| 2 | Transport | **Segment** | Header Transport (nomor port TCP/UDP) |
| 3 | Network | **Packet** | IP Header (alamat IP sumber & tujuan) |
| 4 | Data Link | **Frame** | MAC Address (alamat fisik lokal) |
| 5 | Physical | **Bits** | Diubah jadi sinyal listrik/cahaya (0 dan 1) |

**Kenapa Encapsulation penting?**
- **Standarisasi:** Miliaran perangkat di internet punya aturan yang sama
- **Pencegahan Bottleneck:** Data dipecah jadi potongan kecil, mengurangi kemacetan dibanding kirim satu file besar sekaligus
- **Integrity:** Setiap lapisan bisa mengecek apakah bungkusannya rusak tanpa membongkar seluruh isi

---

### TCP/IP Model

TCP/IP itu versi ringkas dari OSI Model — yang benar-benar diimplementasikan di internet saat ini:

| TCP/IP Layer | Setara OSI Layer | Fungsi |
| ------------ | ---------------- | ------ |
| **Application** | Session + Presentation + Application | Tempat aplikasi beroperasi |
| **Transport** | Transport | TCP/UDP mengelola pengiriman data |
| **Internet** | Network | Routing data pakai alamat IP |
| **Network Interface** | Data Link + Physical | Hubungan fisik dengan perangkat keras |

---

### TCP Packet Headers

| Header | Fungsi |
| :----- | :----- |
| **Source Port** | Port yang dibuka pengirim (dipilih acak dari 0-65535 yang tidak terpakai) |
| **Destination Port** | Port aplikasi/layanan di host tujuan (misal: port 80 buat web server) |
| **Source IP** | Alamat IP perangkat pengirim |
| **Destination IP** | Alamat IP perangkat tujuan |
| **Sequence Number** | Angka acak yang diberikan ke potongan data pertama |
| **Acknowledgement Number** | Sequence Number + 1 — sebagai tanda terima |
| **Checksum** | Kalkulasi matematis buat menjaga integritas data (memastikan tidak korup) |
| **Data** | Isi data aktual yang sedang dikirim |
| **Flag** | Instruksi khusus yang menentukan bagaimana paket ditangani (SYN, ACK, FIN, dll) |

---

### Three-Way Handshake

Three-Way Handshake itu proses **tiga tahap** yang dipakai TCP buat membangun koneksi yang stabil sebelum data asli ditransfer. Analogi: Ibarat **jabat tangan** sebelum mulai bicara — memastikan kedua pihak siap.

| Tahap | Flag | Yang Terjadi |
| ----- | ---- | ------------ |
| 1 | **SYN** | Client mengirim paket dengan sequence number acak — mengajak sinkronisasi |
| 2 | **SYN/ACK** | Server membalas — mengonfirmasi permintaan sinkronisasi diterima |
| 3 | **ACK** | Client mengonfirmasi — koneksi resmi **ESTABLISHED**, transfer data dimulai |

### Komunikasi Lengkap TCP

| Step | Message | Fungsi |
| :--- | :------ | :----- |
| 1 | **SYN** | Pesan awal buat mulai koneksi dan sinkronisasi |
| 2 | **SYN/ACK** | Server mengakui upaya sinkronisasi dari client |
| 3 | **ACK** | Konfirmasi bahwa paket berhasil diterima |
| 4 | **DATA** | Pengiriman data aktual setelah koneksi terjalin |
| 5 | **FIN** | Menutup koneksi secara bersih setelah transfer selesai |
| # | **RST** | Mengakhiri komunikasi secara mendadak (error/masalah sistem) |
