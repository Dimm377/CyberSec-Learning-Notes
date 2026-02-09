# TryHackMe: Networking Core Protocols


---

**Room Link:** [TryHackMe](https://tryhackme.com/room/networkingcoreprotocols)
**Category:** Networking / Protocols
**Difficulty:** Easy

---

---

## Overview

Room ini membahas protokol-protokol inti yang sering ditemui di dunia nyata, mulai dari bagaimana kita mencari alamat (DNS), mengakses web (HTTP), hingga cara kerja email (SMTP/POP3/IMAP). Memahami protokol ini sangat penting bagi praktisi keamanan untuk melihat bagaimana data ditukarkan dan celah apa yang mungkin ada di sana.

---

## Task 2: Remembering Addresses (DNS)

DNS menerjemahkan nama domain menjadi IP address agar perangkat kita bisa berkomunikasi.

- **AAAA Record:** Digunakan khusus untuk memetakan domain ke alamat **IPv6**.
- **MX Record:** Menentukan mail server yang bertanggung jawab untuk domain tersebut.
- **CNAME:** Alias yang mengarahkan satu domain ke domain lainnya.

---

## Task 3: WHOIS

WHOIS digunakan untuk mencari informasi pendaftaran domain, seperti siapa pemiliknya dan kapan domain tersebut dibuat.

- **Praktik:** Mencari informasi pendaftaran domain populer menggunakan perintah `whois <domain>`.
- **Historical Data:** Berguna dalam fase pengintaian (Reconnaissance) untuk mengetahui umur dan legitimasi sebuah domain.

---

## Task 4: Accessing the Web

Protokol utama untuk transmisi data di web. Di room ini, kita menggunakan cara manual (Telnet) untuk melihat interaksi mentah dengan server.

- **Telnet Interaction:**
  1. Hubungkan ke IP target: `telnet MACHINE_IP 80`.
  2. Masukkan request manual: `GET /flag.html HTTP/1.1`.
  3. Tambahkan header Host: `Host: MACHINE_IP` (tekan Enter dua kali).
- **The Flag:** **`THM{TELNET-HTTP}`**.

---

## Task 5: FTP (Transferring Files)

Protokol untuk transfer file. Seringkali memiliki celah jika dikonfigurasi sebagai _Anonymous Login_.

- **Anonymous Login:** Masuk menggunakan username `anonymous` tanpa perlu password.
- **Commands:**
  - `ls` untuk melihat file.
  - `get` untuk mendownload file (misal: `get flag.txt`).
- **The Flag:** **`THM{FAST-FTP}`**.

---

## Task 6 - 8: Email Protocols (SMTP, POP3, IMAP)

Protokol yang menangani pengiriman dan pengambilan pesan email.

- **SMTP (Sending):** Protokol pengiriman email. Perintah `DATA` digunakan untuk mulai menulis isi pesan, dan titik `.` di baris baru untuk mengakhirinya.
- **POP3 (Receiving):** Protokol untuk mendownload email dari server ke klien lokal. Server yang umum digunakan adalah **Dovecot**.
- **The Flag (POP3):** Mengambil pesan ke-4 dengan perintah `RETR 4` menghasilkan flag **`THM{TELNET_RETR_EMAIL}`**.
- **IMAP (Synchronizing):** Lebih canggih dari POP3 karena mensinkronisasi email antar perangkat. Perintah untuk mengambil pesan ke-4 adalah `FETCH 4 body[]`.

> **Tip:** "The flag is in the details."
