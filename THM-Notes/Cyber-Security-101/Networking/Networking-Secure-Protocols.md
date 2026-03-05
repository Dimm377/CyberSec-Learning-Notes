# TryHackMe: Networking Secure Protocol


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/networkingsecureprotocols)
- **Category:** Cyber Security 101
- **Difficulty:** Easy

---

## Overview

Setelah belajar protokol inti (core), room ini membahas **versi secure-nya**. Fokus utamanya mengerti bagaimana enkripsi (SSL/TLS) melindungi data sensitif agar tidak bisa dibaca pihak ketiga saat transit di jaringan.

---

### Protokol Insecure vs Secure

| Protokol Lama | Masalah | Versi Secure | Port |
| ------------- | ------- | ------------ | :--: |
| Telnet | Password dikirim plain-text | **SSH** | 22 |
| HTTP | Data tidak dienkripsi | **HTTPS** | 443 |
| FTP | Credential dan data terbuka | **SFTP** / **FTPS** | 22 / 990 |
| SMTP | Email bisa disadap | **SMTPS** | 465 |
| IMAP | Email bisa disadap | **IMAPS** | 993 |
| POP3 | Email bisa disadap | **POP3S** | 995 |

---

### SSH (Secure Shell)

Pengganti Telnet yang jauh lebih aman buat akses remote terminal:

| Aspek | Detail |
| ----- | ------ |
| **Enkripsi** | Seluruh sesi komunikasi dienkripsi (beda dengan Telnet yang plain-text) |
| **Port** | Default **Port 22** |
| **Autentikasi** | Support _Public Key Authentication_ — lebih aman daripada password biasa |

---

### HTTPS (Hypertext Transfer Protocol Secure)

Versi aman dari HTTP yang dipakai hampir seluruh website modern:

| Aspek | Detail |
| ----- | ------ |
| **SSL/TLS** | Mengenkripsi traffic antara browser dan server pakai sertifikat |
| **Port** | Default **Port 443** |
| **Integritas** | Menjamin data tidak dimanipulasi di tengah jalan |

---

### Secure File Transfer (SFTP vs FTPS)

Dua cara berbeda buat mengamankan FTP yang pada dasarnya sangat tidak aman:

| Protokol | Cara Kerja | Port | Keunggulan |
| -------- | ---------- | :--: | ---------- |
| **SFTP** (SSH File Transfer Protocol) | Berjalan di atas protokol SSH | 22 | Cuma butuh satu port terbuka — populer |
| **FTPS** (FTP over SSL) | Pakai SSL/TLS buat enkripsi koneksi FTP | 990 / 21 (STARTTLS) | Kompatibel dengan infrastruktur FTP lama |

---

### IPsec & VPNs

Protokol yang bekerja di level network (Layer 3) buat mengamankan seluruh traffic antar dua titik:

| Teknologi | Fungsi |
| --------- | ------ |
| **VPN** | Membuat tunnel terenkripsi di atas jaringan publik (Internet) |
| **IPsec** | Kumpulan protokol buat autentikasi dan enkripsi paket IP — sering dipakai di Site-to-Site VPN |

---

### Secure Email Protocols

Mengamankan komunikasi email dari pengintaian pakai enkripsi SSL/TLS:

| Protokol | Port | Fungsi |
| -------- | :--: | ------ |
| **SMTPS** | 465 | **Pengiriman** email dari client ke server — pakai _Implicit TLS_ sejak awal koneksi |
| **IMAPS** | 993 | **Sinkronisasi** email dua arah antar perangkat — pesan tetap tersimpan di server |
| **POP3S** | 995 | **Download** email ke perangkat lokal — secara tradisional menghapus email dari server setelah download |

> **Tip:** Encryption is not a wall, it's a puzzle. Cuma karena datanya terenkripsi, bukan berarti sistemnya tidak bisa ditembus.
