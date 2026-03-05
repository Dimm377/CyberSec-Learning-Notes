# TryHackMe: Wireshark: The Basics


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/wiresharkthebasics)
- **Category:** Networking / Analysis tool
- **Difficulty:** Easy

---

## Task 1: Introduction

**Wireshark** adalah alat analisis paket jaringan (*open-source*) yang paling populer di dunia. Ibarat sebuah **mikroskop** atau **mesin X-Ray** buat jaringan — dia membuat kita bisa melihat apa yang sebenarnya terjadi di dalam jaringan atau udara (Wi-Fi), membedah setiap bit data yang lewat.

**Kegunaan Utama:**
- **Sniffing:** Mengintip traffic yang sedang jalan secara *live*.
- **Investigating PCAP:** Membedah rekaman traffic (file `.pcap`) yang sudah diambil sebelumnya.

### Learning Objectives (Tujuan Kita):
Di room ini, kita bakal fokus menguasai:
1. Navigasi dan konfigurasi dasar Wireshark.
2. Membedah paket buat mencari informasi di setiap lapisan **TCP/IP**.
3. Cara pakai **Display Filters** buat memfilter data yang kita butuhkan.

---

## Task 2: Tool Overview

### Use Cases (Kapan Kita Pakai Wireshark?)
Wireshark bisa dipakai buat banyak hal:
1. **Troubleshooting Jaringan:** Mendeteksi titik kegagalan (*failure points*) atau kemacetan (*congestion*) di jaringan.
2. **Mendeteksi Anomali Keamanan:** Mencari *host* mencurigakan, penggunaan *port* yang tidak biasa, atau *traffic* nakal.
3. **Mempelajari Protokol:** Menginvestigasi detail protokol seperti *response code* HTTP atau isi *payload* data.

> **Catatan Penting (Anti-Miskonsepsi):** Wireshark **BUKAN** *Intrusion Detection System* (IDS). Dia tidak bisa mencegah serangan atau memberi peringatan otomatis. Dia hanya mengizinkan kita membedah paket secara mendalam dan dia **tidak memodifikasi** paket, cuma membacanya. Jadi, mendeteksi anomali itu bergantung penuh pada wawasan si analyst.

### GUI and Data
Tampilan antarmuka utama Wireshark dibagi menjadi 5 bagian penting:

| Bagian GUI | Penjelasan Fungsinya |
| ---------- | -------------------- |
| **Toolbar** | Menu utama yang berisi *shortcuts* untuk *packet sniffing*, *filtering*, menyortir, meringkas, mengekspor, atau menggabungkan file `.pcap`. |
| **Display Filter Bar** | Kolom utama buat mengetik *query filter* (menyaring data yang lagi dilihat). |
| **Recent Files** | Daftar file yang barusan dibuka/dianalisis. Bisa diklik ganda buat buka lagi. |
| **Capture Filter and Interfaces** | Tempat mengatur *capture filter* (filter yang jalan **sebelum** merekam) dan memilih titik tangkap (misal: antarmuka jaringan seperti `eth0`, `lo`, atau `ens33`). |
| **Status Bar** | Baris paling bawah yang menunjukkan status *tool*, profil, dan jumlah paket. |

---

---
