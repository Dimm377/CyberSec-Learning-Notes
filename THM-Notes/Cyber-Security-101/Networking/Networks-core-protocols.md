# TryHackMe: Networking Core Protocols


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/networkingcoreprotocols)
- **Category:** Networking / Protocols
- **Difficulty:** Easy

---

---

## Overview

Room ini ngebahas protokol-protokol inti yang sering ditemuin di dunia nyata, mulai dari gimana kita nyari alamat (DNS), ngakses web (HTTP), sampai cara kerja email (SMTP/POP3/IMAP). Ngerti protokol ini penting banget buat praktisi keamanan buat liat gimana data ditukar dan celah apa yang mungkin ada di sana.

---

## Task 2: Remembering Addresses (DNS)

DNS nerjemahin nama domain jadi IP address biar perangkat kita bisa berkomunikasi.

- **AAAA Record:** Dipake khusus buat memetakan domain ke alamat **IPv6**.
- **MX Record:** Nentuin mail server yang bertanggung jawab buat domain tersebut.
- **CNAME:** Alias yang ngarahin satu domain ke domain lainnya.

---

## Task 3: WHOIS

WHOIS dipake buat nyari informasi pendaftaran domain, kayak siapa pemiliknya dan kapan domain itu dibuat.

- **Praktik:** Nyari informasi pendaftaran domain populer pake perintah `whois <domain>`.
- **Historical Data:** Berguna di fase pengintaian (Reconnaissance) buat tau umur dan legitimasi sebuah domain.

---

## Task 4: Accessing the Web

Protokol utama buat transmisi data di web. Di room ini, kita pake cara manual (Telnet) buat liat interaksi mentah sama server.

- **Telnet Interaction:**
  1. Hubungin ke IP target: `telnet MACHINE_IP 80`.
  2. Masukin request manual: `GET /flag.html HTTP/1.1`.
  3. Tambahin header Host: `Host: MACHINE_IP` (tekan Enter dua kali).
- **The Flag:** **`THM{TELNET-HTTP}`**.

---

## Task 5: FTP (Transferring Files)

Protokol buat transfer file. Seringkali punya celah kalau dikonfigurasi sebagai _Anonymous Login_.

- **Anonymous Login:** Masuk pake username `anonymous` tanpa perlu password.
- **Commands:**
  - `ls` buat liat file.
  - `get` buat download file (misal: `get flag.txt`).
- **The Flag:** **`THM{FAST-FTP}`**.

---

## Task 6 - 8: Email Protocols (SMTP, POP3, IMAP)

Protokol yang nanganin pengiriman dan pengambilan pesan email.

- **SMTP (Sending):** Protokol pengiriman email. Perintah `DATA` dipake buat mulai nulis isi pesan, dan titik `.` di baris baru buat ngakhirinnya.
- **POP3 (Receiving):** Protokol buat download email dari server ke klien lokal. Server yang umum dipake itu **Dovecot**.
- **The Flag (POP3):** Ngambil pesan ke-4 pake perintah `RETR 4` menghasilkan flag **`THM{TELNET_RETR_EMAIL}`**.
- **IMAP (Synchronizing):** Lebih canggih dari POP3 karena nyinkronisasi email antar perangkat. Perintah buat ngambil pesan ke-4 itu `FETCH 4 body[]`.

> **Tip:** "The flag is in the details."
