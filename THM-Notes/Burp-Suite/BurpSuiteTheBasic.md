# TryHackMe: Burp Suite Basics

---

- **Room Link:** [Burp Suite Basics](https://tryhackme.com/room/burpsuitebasics)
- **Category:** Web Security / Tools
- **Difficulty:** Easy

---

## Overview

Room ini memberikan pondasi ilmu tentang cara kerja **Burp Suite**, alat yang wajib menyala setiap hari di laptop seorang _Pentester_ atau _Bug Hunter_. Di sini kamu akan belajar bagaimana caranya "berdiri di tengah-tengah" jalur antara _browser_ dan _web server_ — untuk menyadap, menghentikan, dan memanipulasi _traffic_ data sesuai kebutuhan pengujian.

---

## Dashboard

_Dashboard_ adalah pusat kendali (_command center_) di Burp Suite. Kalau kamu membuka versi **Community 2026.1.1**, fitur ini menampilkan ringkasan bagian penting:

- **Task Log**: Ibarat buku absen — menampilkan riwayat _log_ dari _task/crawler_ otomatis yang sedang (atau baru saja selesai) dijalankan.
- **Issue Activity**: Tempat munculnya temuan kerentanan keamanan. Di versi Pro, fitur ini sangat detail; di versi Community, hanya menampilkan temuan minor.
- **Shortcut**: Tekan `Ctrl + Shift + D` untuk langsung melompat ke tab _Dashboard_ kapan pun.

---

## The Burp Proxy

Fitur _Proxy_ adalah inti dari Burp Suite. Dia memerankan posisi _Man-in-the-Middle_:

- **Interception**: Menghadang _request_ HTTP di tengah jalan, menahannya **sebelum** sampai ke _server_. Di sinilah kamu bisa mengedit parameter, header, atau isi _body_ request.
- **HTTP History**: Mencatat semua riwayat paket _request_ yang berlalu-lalang — berguna untuk menyusun analisis.
- **Intercept is on/off**: Toggle sederhana untuk memutuskan apakah _traffic_ ditahan atau dibiarkan menuju server.

---

## Target Scoping & Site Map

Kalau kamu melepas filter proxy, Burp akan mencatat semua _traffic_ latar belakang (seperti _telemetry_ Google atau _update_ Windows). Supaya fokus tidak terpecah, kamu membutuhkan _Scope_.

- **Site Map**: Fitur visual yang menampilkan peta struktur folder dan hierarki file dari web aplikasi target.
- **Scope**: Pembatas yang menyaring _traffic log_ — Burp hanya akan menangkap _traffic_ dari domain/URL yang sudah ditetapkan sebagai target.

---

## Repeater

Kalau _Proxy_ tujuannya untuk menghadang, _Repeater_ adalah bengkel untuk menguji manipulasi parameter secara manual dan teliti:

- **Function**: Mengirim ulang sebuah _request_ secara berulang-ulang dengan modifikasi parameter yang berbeda-beda, tanpa perlu kembali ke browser.
- **Shortcut**: Tekan `Ctrl + R` untuk mengirim _request_ dari _Proxy_ ke _Repeater_ secara cepat.

---

## Intruder

Kalau capek mengirim manual menggunakan _Repeater_, _Intruder_ menangani serangan otomatis di level _brute-force_:

- **Payload Types**: Sniper (menyerang satu titik), Battering Ram, Pitchfork (menyerang beberapa posisi secara bersamaan), dan Cluster Bomb (menguji semua kombinasi dari berbagai _payload_).
- **Throttling**: Di versi **Community Edition**, kecepatan _Intruder_ dibatasi secara drastis (_throttled_) oleh pembuat aplikasi. Versi Pro tidak memiliki batasan ini.

---

## Decoder, Comparer, & BApp Store

Tool pendukung yang jasanya sangat besar:

- **Decoder**: Mengubah encoding/decoding teks (Base64, URL encoding, ASCII Hex) bolak-balik dalam hitungan detik.
- **Comparer**: Mencari perbedaan (_diffing_ visual) dari dua _response_ HTTP. Sangat berguna untuk menganalisis _blind SQL injection_ — mendeteksi perbedaan satu huruf atau durasi respon server.
- **Extender (BApp Store)**: Toko ekstensi untuk menambahkan _plugin_ seperti **Logger++** atau **Turbo Intruder** yang mendongkrak kemampuan Burp Suite.

---

## Attack Flow Awareness

Burp Suite memegang peranan utama saat masuk tahap **Reconnaissance** dan **Initial Access** pada aplikasi web:

| Fase | Peran Burp Suite |
| :--- | :--- |
| **Reconnaissance** | Mengandalkan _Site Map_ untuk memetakan seluruh struktur aplikasi web dan menemukan _endpoint_ tersembunyi |
| **Initial Access** | Menggabungkan _Proxy_ + _Repeater_ untuk memanipulasi parameter, menemukan, dan mengeksploitasi kerentanan (SQLi, XSS, IDOR, dll) |
| **Post-Exploitation** | Membedah _session tokens_ dan _cookies_ untuk mencuri identitas Admin (_Privilege Escalation_) |

**Perspektif Blue Team:**
- **WAF (Web Application Firewall):** Bisa mendeteksi pola _request_ brutal yang tidak wajar dari _Burp Intruder_ (misal: ribuan _request_ ke satu _endpoint login_ dalam hitungan detik).
- **Rate Limiting:** Membatasi jumlah _request_ per IP untuk memperlambat serangan _brute-force_ otomatis.
- **Logging:** Semua pergerakan mencurigakan yang melewati _server_ tercatat secara otomatis sebagai bahan bukti forensik.

---

## Real-World Relevance

- **Bug Bounty:** Burp Suite adalah tool utama para _Bug Hunter_. Di platform seperti HackerOne atau Bugcrowd, sebagian besar laporan kerentanan dihasilkan dari kombinasi _Proxy_ + _Repeater_.
- **Web App Pentest:** Secara profesional, _pentester_ menggunakan Burp Suite Pro untuk menjalankan _active vulnerability scanning_ secara otomatis, lalu memvalidasi temuan secara manual menggunakan _Repeater_.
- **OWASP Top 10:** Hampir seluruh kerentanan di OWASP Top 10 (SQLi, XSS, Broken Access Control, dll) bisa ditemukan dan dieksploitasi menggunakan tool ini.

---

## Questions

1. Kenapa pemahaman dasar protokol HTTP menjadi fondasi mutlak sebelum bisa menggunakan Burp Suite secara efektif?
2. Seberapa penting fitur **Scope** saat kamu ditugaskan menguji aplikasi web berskala besar?
3. Dalam skenario _Blind SQL Injection_ (di mana database hanya merespon lewat jeda waktu), kenapa fitur **Comparer** sangat berguna?
