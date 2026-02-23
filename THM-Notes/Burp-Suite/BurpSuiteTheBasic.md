# TryHackMe: Burp Suite Basics


---

- **Room Link:** [Burp Suite Basics](https://tryhackme.com/room/burpsuitebasics)
- **Category:** Web Security / Tools
- **Difficulty:** Easy

---

## # Overview

Room ini pondasi utama buat ngerti cara kerja **Burp Suite**, alat wajib buat setiap _Pentester_ atau _Bug Hunter_. Di sini kita bakal belajar gimana cara "berdiri di tengah" antara browser dan server buat memanipulasi traffic web sesuai keinginan.

---

### Understanding the Dashboard

Dashboard itu pusat kendali Burp Suite. Di versi **Community 2026.1.1** yang lagi aku pake, fitur ini ngasih ringkasan aktivitas sistem:

- **Task Log**: Nampilin log dari aktivitas yang lagi jalan.
- **Issue Activity**: Tempat buat liat temuan keamanan (manual di versi Community).
- **Shortcut**: `Ctrl + Shift + D` buat langsung loncat ke tab Dashboard.

---

### The Power of Burp Proxy

Proxy itu fitur paling iconic. Dia bertindak sebagai _Man-in-the-Middle_:

- **Interception**: Ngehentiin _request_ sebelum sampe ke server biar bisa dimodifikasi.
- **HTTP History**: Nyimpen log setiap _request_ yang lewat buat dianalisis nanti.
- **Intercept is on/off**: Tombol buat nentuin apakah trafik mau ditahan atau dibiarin lewat aja.

---

### Target Scoping & Site Map

Target Scope itu fitur di Burp Suite yang dipake buat nentuin URL atau domain mana aja yang bakal dijadiin target utama / prioritas.

- **Site Map**: Ngasih gambaran hierarki struktur folder dan file dari aplikasi web target.
- **Scope**: Bikin kita bisa filter traffic biar Burp cuma nyatet domain yang lagi kita serang.

---

### Manual Tweaking with Repeater

Repeater itu tempat favorit buat yang suka ngulik manual:

- **Function**: Ngirim satu _request_ berkali-kali pake modifikasi parameter yang beda-beda tanpa harus _refresh_ browser.
- **Shortcut**: `Ctrl + R` buat ngirim _request_ dari Proxy ke Repeater secara instan.

---

### Introduction to Intruder

Intruder dipake buat serangan otomatisasi skala kecil:

- **Payload Types**: Sniper (satu posisi), Battering Ram, Pitchfork, dan Cluster Bomb.
- **Throttling**: Perlu diingat, di versi **Community**, kecepatan Intruder dibatasi (_throttled_) sama PortSwigger.

---

### Decoder, Comparer, and Extender

Alat pendukung yang bikin Burp Suite makin lengkap:

- **Decoder**: Dipake buat _encoding/decoding_ data (Base64, URL, Hex) secara cepet.
- **Comparer**: Bandingin dua _response_ HTTP yang beda buat nyari perbedaan tipis (kayak waktu _blind SQL injection_).
- **Extender (BApp Store)**: Tempat masang ekstensi tambahan kayak **Logger++** atau **Turbo Intruder** buat ningkatin kemampuan Burp Suite.

> **Note:** Alat hebat gak bakal berguna di tangan orang yang gak paham konsep dasarnya. Pahami protokol HTTP, maka bakal paham cara merusaknya.
