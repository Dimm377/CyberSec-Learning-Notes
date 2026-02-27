# TryHackMe: Networking Concepts


---

- **Room Link:** [Network concepts](https://tryhackme.com/room/networkingconcepts)
- **Category:** Cyber Security 101 path
- **Difficulty:** Easy

---

# Overview

## OSI Model

OSI Model itu kerangka kerja konseptual yang membagi proses komunikasi jaringan jadi 7 layer.

- **Layer 4 (Transport):** Bertanggung jawab buat komunikasi _end-to-end_ antar aplikasi.
- **Layer 3 (Network):** Bertanggung jawab buat merutekan paket ke jaringan yang tepat pakai IP Address.
- **Layer 2 (Data Link):** Ngatur perpindahan data antar perangkat di segmen jaringan yang sama (MAC Address).
- **Layer 6 (Presentation):** Nanganin encoding, kompresi, dan enkripsi data aplikasi.

> **mnemonic:** "**P**lease **D**o **N**ot **T**hrow **S**pinach **P**izza **A**way", (Physical, Data Link, Network, Transport, Session, Presentation, Application).

## TCP/IP Model

Beda sama OSI Model yang konseptual, TCP/IP itu model yang beneran diimplementasiin di internet saat ini.

- **Application Layer:** Gabungan dari layer 5, 6, dan 7 OSI. Protokol seperti HTTP, FTP, dan DNS ada di sini.
- **Internet Layer:** Setara sama Layer 3 OSI, fokus ke pengalamatan logis (IP).
- **Encapsulation:** Proses di mana setiap layer nambahin header ke data dari layer di atasnya sebelum dikirim ke layer bawah.

## IP Addresses & Subnets

Identitas unik yang ada di setiap perangkat di jaringan.

- **IPv4:** Alamat 32-bit yang dibagi jadi 4 oktet.
- **Private IP:** Dipake di jaringan lokal (LAN) dan tidak bisa diakses langsung dari internet (misal: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16).

## TCP, UDP, and Ports

Protokol transport yang menentukan gimana data dikirim.

- **TCP (Transmission Control Protocol):** Berorientasi koneksi, reliable, dan pakai _Three-way Handshake_ (SYN, SYN-ACK, ACK).
- **UDP (User Datagram Protocol):** Tidak berorientasi koneksi, cepat, tapi tidak jamin paket sampai.
- **Data Units:** Data di TCP disebut **Segment**, sedangkan di UDP disebut **Datagram**.
- **Ports:** Ada sekitar **65.535** port yang tersedia buat ngarahin traffic ke layanan spesifik.

## Connecting to a Web Server

Praktik pakai perintah dasar buat berinteraksi sama tools networking.

- **Netcat (nc):** _Swiss-army knife_ buat jaringan. Dipake buat baca atau tulis data di koneksi jaringan.
- **Telnet:** Protokol lama buat akses _remote_ teks, sekarang sering dipake buat mengecek apakah sebuah port TCP terbuka.
