# TryHackMe: Networking Secure Protocol


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/networkingsecureprotocols)
- **Category:** Cyber Security 101
- **Difficulty:** Easy

---

## Overview

Setelah belajar protokol inti (core), room ini ngebahas versi secure-nya dari protokol-protokol tersebut. Fokus utama kita itu ngerti gimana enkripsi (SSL/TLS) ngelindungin data sensitif biar gak bisa dibaca sama pihak ketiga waktu transit di jaringan.

## Task 2: SSH (Secure Shell)

SSH itu pengganti Telnet yang jauh lebih aman buat akses remote terminal.

- **Enkripsi:** Beda sama Telnet yang ngirim password dalam bentuk _plain-text_, SSH mengenkripsi seluruh sesi komunikasi.
- **Port:** Secara default jalan di **Port 22**.
- **Autentikasi:** Support _Public Key Authentication_ yang jauh lebih aman daripada sekadar password biasa.

## Task 3: HTTPS (Hypertext Transfer Protocol Secure)

Versi aman dari HTTP yang dipake hampir seluruh website modern sekarang.

- **SSL/TLS:** HTTPS pake sertifikat SSL/TLS buat mengenkripsi traffic antara browser dan server.
- **Port:** Secara default jalan di **Port 443**.
- **Integritas:** Jamin bahwa data yang dikirim gak dimanipulasi di tengah jalan (Integrity).

## Task 4: SFTP & FTPS (Secure File Transfer)

Dua cara beda buat ngamanin protokol FTP yang sebenernya sangat gak aman.

- **SFTP (SSH File Transfer Protocol):** Jalan di atas protokol SSH (Port 22). Populer banget karena cuma butuh satu port terbuka.
- **FTPS (FTP over SSL):** Pake SSL/TLS buat mengenkripsi koneksi FTP standar (Port 990 atau 21 dengan STARTTLS).

## Task 5: IPsec & VPNs

Protokol yang bekerja di level network (Layer 3) buat ngamanin seluruh traffic antar dua titik.

- **VPN (Virtual Private Network):** Bikin tunnel terenkripsi di atas jaringan publik (Internet).
- **IPsec:** Kumpulan protokol yang dipake buat autentikasi dan enkripsi paket IP. Sering dipake di koneksi Site-to-Site VPN.

## Task 6: Secure Email (SMTPS, IMAPS, POP3S)

Ngamanin komunikasi email dari pengintaian pake enkripsi SSL/TLS.

- **SMTPS (Simple Mail Transfer Protocol Secure):**
  - **Port:** 465.
  - **Fungsi:** Dipake buat **pengiriman** email dari client ke server atau antar server secara aman.
  - **Detail:** Pake _Implicit TLS_ buat mengenkripsi seluruh sesi komunikasi sejak awal koneksi dibuat.
- **IMAPS (Internet Message Access Protocol Secure):**
  - **Port:** 993.
  - **Fungsi:** Dipake buat **pengambilan dan sinkronisasi** email dari server secara dua arah.
  - **Detail:** Bikin sinkronisasi email antar perangkat jadi mungkin, jadi pesan tetap tersimpan di server.
- **POP3S (Post Office Protocol v3 Secure):**
  - **Port:** 995.
  - **Fungsi:** Dipake buat **download** email dari server ke perangkat lokal.
  - **Detail:** Secara tradisional menghapus email dari server setelah berhasil didownload ke client lokal.

> **Tip:** "Encryption is not a wall, it's a puzzle." Cuma karena datanya terenkripsi, bukan berarti sistemnya gak bisa ditembus.
