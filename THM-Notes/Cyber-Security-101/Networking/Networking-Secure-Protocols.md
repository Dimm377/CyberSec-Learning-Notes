# TryHackMe: Networking Secure Protocol

**Room Link:** [TryHackMe](https://tryhackme.com/room/networkingsecureprotocols)
**Category:** Cyber Security 101
**Difficulty:** Easy

## Overview

Setelah mempelajari protokol inti (core), room ini membahas versi secure nya dari protokol-protokol tersebut, Fokus utama kita adalah memahami bagaimana enkripsi (SSL/TLS) melindungi data sensitif agar tidak bisa dibaca oleh pihak ketiga saat transit di jaringan

## Task 2: SSH (Secure Shell)

SSH adalah pengganti Telnet yang jauh lebih aman untuk akses via remote terminal

- **Enkripsi:** Berbeda dengan Telnet yang mengirimkan password dalam bentuk _plain-text_, SSH mengenkripsi seluruh sesi communication
- **Port:** Secara default berjalan pada **Port 22**.
- **Autentikasi:** Mendukung penggunaan _Public Key Authentication_ yang jauh lebih aman daripada sekedar password biasa

## Task 3: HTTPS (Hypertext Transfer Protocol Secure)

Versi aman dari HTTP yang digunakan hampir oleh seluruh website modern saat ini

- **SSL/TLS:** HTTPS menggunakan sertifikat SSL/TLS untuk mengenkripsi traffic antara browser dan server
- **Port:** Secara default berjalan pada **Port 443**
- **Integritas:** Menjamin bahwa data yang dikirim tidak dimanipulasi di tengah jalan (Integrity)

## Task 4: SFTP & FTPS (Secure File Transfer)

Dua cara berbeda untuk mengamankan protokol FTP yang sangat tidak aman

- **SFTP (SSH File Transfer Protocol):** Berjalan di atas protokol SSH (Port 22). Sangat populer karena hanya butuh satu port terbuka
- **FTPS (FTP over SSL):** Menggunakan SSL/TLS untuk mengenkripsi koneksi FTP standar (Port 990 atau 21 dengan STARTTLS)

## Task 5: IPsec & VPNs

Protokol yang bekerja di level network (Layer 3) untuk mengamankan seluruh traffic antar dua titik

- **VPN (Virtual Private Network):** Menciptakan tunnel terenkripsi di atas jaringan publik (Internet)
- **IPsec:** Kumpulan protokol yang digunakan untuk autentikasi dan enkripsi paket IP, Sering digunakan dalam koneksi Site-to-Site VPN

## Task 6: Secure Email (SMTPS, IMAPS, POP3S)

Mengamankan komunikasi email dari pengintaian menggunakan enkripsi SSL/TLS.

- **SMTPS (Simple Mail Transfer Protocol Secure):**
  - **Port:** 465.
  - **Fungsi:** Digunakan untuk **pengiriman** email dari client ke server atau antar server secara aman.
  - **Detail:** Menggunakan _Implicit TLS_ untuk mengenkripsi seluruh sesi komunikasi sejak awal koneksi dibuat.
- **IMAPS (Internet Message Access Protocol Secure):**
  - **Port:** 993.
  - **Fungsi:** Digunakan untuk **pengambilan dan sinkronisasi** email dari server secara dua arah.
  - **Detail:** Memungkinkan sinkronisasi email antar perangkat, sehingga pesan tetap tersimpan di server.
- **POP3S (Post Office Protocol v3 Secure):**
  - **Port:** 995.
  - **Fungsi:** Digunakan untuk **mengunduh** email dari server ke perangkat lokal.
  - **Detail:** Secara tradisional menghapus email dari server setelah berhasil diunduh ke client lokal.

> **Tip:** "Encryption is not a wall, it's a puzzle" Hanya karena datanya terenkripsi, bukan berarti sistemnya tidak bisa ditembus
