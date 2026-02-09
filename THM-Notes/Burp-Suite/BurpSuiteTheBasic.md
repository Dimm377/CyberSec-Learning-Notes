# TryHackMe: Burp Suite Basics


---

**Room Link:** [Burp Suite Basics](https://tryhackme.com/room/burpsuitebasics)
**Category:** Web Security / Tools
**Difficulty:** Easy

---

## # Overview

Room ini adalah pondasi utama untuk memahami cara kerja **Burp Suite**, alat wajib bagi setiap _Pentester_ atau _Bug Hunter_. disini bakal belajar gimana cara "berdiri di tengah" antara browser dan server untuk memanipulasi traffic web sesuai keinginan

---

### Understanding the Dashboard

Dashboard adalah pusat kendali Burp Suite. Di versi **Community 2026.1.1** yang sedang ku pake, fitur ini memberikan ringkasan aktivitas sistem:

- **Task Log**: Menampilkan log dari aktivitas yang sedang berjalan.
- **Issue Activity**: Tempat untuk melihat temuan keamanan (manual di versi Community).
- **Shortcut**: `Ctrl + Shift + D` buat langsung loncat ke tab Dashboard.

---

### The Power of Burp Proxy

Proxy adalah fitur paling iconic. Dia bertindak sebagai _Man-in-the-Middle_:

- **Interception**: Menghentikan _request_ sebelum sampai ke server agar bisa dimodifikasi.
- **HTTP History**: Menyimpan log setiap _request_ yang lewat untuk dianalisis nanti.
- **Intercept is on/off**: Tombol untuk menentukan apakah trafik mau ditahan atau dibiarkan lewat begitu saja.

---

### Target Scoping & Site Map

Target Scope adalah fitur di Burp Suite yang digunakan intuk mementukan URL atau domain mana aja yang akan dijadikan target utama / prioritas

- **Site Map**: Memberikan gambaran hierarki struktur folder dan file dari aplikasi web target.
- **Scope**: Memungkinkan kita untuk memfilter traffic agar Burp cuma mencatat domain yang sedang kita serang.

---

### Manual Tweaking with Repeater

Repeater adalah tempat favorit buat yang suka ngulik manual:

- **Function**: Mengirimkan satu _request_ berkali-kali dengan modifikasi parameter yang berbeda tanpa harus _refresh_ browser.
- **Shortcut**: `Ctrl + R` untuk mengirim _request_ dari Proxy ke Repeater secara instan.

---

### Introduction to Intruder

Intruder digunakan untuk serangan otomatisasi skala kecil:

- **Payload Types**: Sniper (satu posisi), Battering Ram, Pitchfork, dan Cluster Bomb.
- **Throttling**: Perlu diingat, di versi **Community**, kecepatan Intruder dibatasi (_throttled_) oleh PortSwigger.

---

### Decoder, Comparer, and Extender

Alat pendukung yang bikin Burp Suite makin lengkap:

- **Decoder**: Digunakan untuk melakukan _encoding/decoding_ data (Base64, URL, Hex) secara cepat.
- **Comparer**: Membandingkan dua _response_ HTTP yang berbeda untuk mencari perbedaan tipis (seperti saat _blind SQL injection_).
- **Extender (BApp Store)**: Tempat memasang ekstensi tambahan seperti **Logger++** atau **Turbo Intruder** untuk meningkatkan kemampuan Burp suite.

> **Note:** Alat hebat nggak bakal berguna di tangan orang yang nggak paham konsep dasarnya. Pahami protokol HTTP, maka akan paham cara merusaknya.
