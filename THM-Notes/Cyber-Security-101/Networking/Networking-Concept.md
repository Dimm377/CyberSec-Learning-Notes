# TryHackMe: Networking Concepts


---

- **Room Link:** [Network concepts](https://tryhackme.com/room/networkingconcepts)
- **Category:** Cyber Security 101 path
- **Difficulty:** Easy

---

# Overview

## OSI Model

OSI Model adalah kerangka kerja konseptual yang memebagi proses komunikasi jaringan menjadi 7 layer

- **Layer 4 (Transport):** Bertanggung jawab atas komunikasi _end-to-end_ antar aplikasi
- **Layer 3 (Network):** Bertanggung jawab untuk merutekan paket ke jaringan yang tepat menggunakan IP Address
- **Layer 2 (Data Link):** Mengatur perpindahan data antar perangkat di segmen jaringan yang sama (MAC Address).
- **Layer 6 (Presentation):** Menangani encoding, kompresi, dan enkripsi data aplikasi.

> **mnemonic:** "**P**lease **D**o **N**ot **T**hrow **S**pinach **P**izza **A**way", (Physical, Data Link, Network, Transport, Session, Presentation, Application).

## TCP/IP Model

Berbeda dengan OSI Model yang konseptual, TCP/IP adalah model yang benar-benar diimplementasikan di internet saat ini

- **Application Layer:** Gabungan dari layer 5, 6, dan 7 OSI. Protokol seperti HTTP, FTP, dan DNS berada di sini.
- **Internet Layer:** Setara dengan Layer 3 OSI, fokus pada pengalamatan logis (IP).
- **Encapsulation:** Proses di mana setiap layer menambahkan header ke data dari layer di atasnya sebelum dikirim ke layer bawah.

## IP Addresses & Subnets

Identitas unik yang ada di setiap perangkat di jaringan

- **IPv4:** Alamat 32-bit yang dibagi menjadi 4 oktet.
- **Private IP:** Digunakan dalam jaringan lokal (LAN) dan tidak dapat diakses langsung dari internet (misal: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16).
- **IPv4:** Alamat 32-bit yang dibagi menjadi 4 oktet.

## TCP, UDP, and Ports

Protokol transport yang menentukan bagaimana data dikirimkan

- **TCP (Transmission Control Protocol):** Berorientasi koneksi, reliable, dan menggunakan _Three-way Handshake_ (SYN, SYN-ACK, ACK)
- **UDP (User Datagram Protocol):** Tidak berorientasi koneksi, cepat, namun tidak menjamin paket sampai
- **Data Units:** Data pada TCP disebut **Segment**, sedangkan pada UDP disebut **Datagram**.
- **Ports:** Ada sekitar **65.535** port yang tersedia untuk mengarahkan trafik ke layanan spesifik.

## Connecting to a Web Server

Praktek menggunakan perintah dasar untuk berinteraksi dengan tools networking

- **Netcat (nc):** _Swiss-army knife_ untuk jaringan. Digunakan untuk membaca atau menulis data di koneksi jaringan.
- **Telnet:** Protokol lama untuk akses _remote_ teks, sekarang sering digunakan untuk mengecek apakah sebuah port TCP terbuka.
