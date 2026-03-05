# TryHackMe: Networking Core Protocols


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/networkingcoreprotocols)
- **Category:** Networking / Protocols
- **Difficulty:** Easy

---

## Overview

Room ini membahas protokol-protokol inti yang sering ditemukan di dunia nyata — dari mencari alamat (DNS), mengakses web (HTTP), transfer file (FTP), sampai cara kerja email (SMTP/POP3/IMAP). Mengerti protokol ini penting buat praktisi keamanan buat melihat bagaimana data ditukar dan celah apa yang mungkin ada.

---

### DNS (Remembering Addresses)

DNS menerjemahkan nama domain jadi IP address agar perangkat bisa berkomunikasi:

| Record | Fungsi |
| ------ | ------ |
| **A** | Memetakan domain ke alamat **IPv4** |
| **AAAA** | Memetakan domain ke alamat **IPv6** |
| **MX** | Menentukan mail server yang bertanggung jawab buat domain |
| **CNAME** | Alias yang mengarahkan satu domain ke domain lainnya |

*(Penjelasan lebih lengkap tentang DNS ada di catatan [DNS-In-Details](../../Pre-Security/How-The-Web-Works/DNS-In-Details.md))*

---

### WHOIS

WHOIS dipakai buat mencari informasi pendaftaran domain — siapa pemiliknya, kapan dibuat, dan kapan expired:

| Aspek | Detail |
| ----- | ------ |
| **Command** | `whois <domain>` |
| **Kegunaan di Recon** | Mengetahui umur dan legitimasi sebuah domain pada fase pengintaian (Reconnaissance) |

---

### Accessing the Web (HTTP via Telnet)

Di room ini, kita pakai cara manual (Telnet) buat melihat interaksi mentah dengan server:

```bash
telnet MACHINE_IP 80
GET /flag.html HTTP/1.1
Host: MACHINE_IP
```

*(Tekan Enter dua kali setelah header Host)*

---

### FTP (File Transfer Protocol)

Protokol buat transfer file. Seringkali punya celah kalau dikonfigurasi sebagai _Anonymous Login_:

| Konsep | Detail |
| ------ | ------ |
| **Anonymous Login** | Masuk pakai username `anonymous` tanpa password |
| **`ls`** | Melihat daftar file di server |
| **`get <file>`** | Download file dari server (contoh: `get flag.txt`) |

---

### Email Protocols (SMTP, POP3, IMAP)

Protokol yang menangani pengiriman dan pengambilan pesan email:

| Protokol | Fungsi | Cara Kerja |
| -------- | ------ | ---------- |
| **SMTP** | Kirim email | Perintah `DATA` buat mulai nulis pesan, titik `.` di baris baru buat mengakhiri |
| **POP3** | Download email ke lokal | Server umum: **Dovecot**. Perintah `RETR <nomor>` buat mengambil pesan |
| **IMAP** | Sinkronisasi email antar perangkat | Lebih canggih dari POP3. Perintah `FETCH <nomor> body[]` buat mengambil pesan |

> **Tip:** The flag is in the details.
