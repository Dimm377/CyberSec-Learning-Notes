# TryHackMe: Burp Suite Basics


---

- **Room Link:** [Burp Suite Basics](https://tryhackme.com/room/burpsuitebasics)
- **Category:** Web Security / Tools
- **Difficulty:** Easy

---

## # Overview

Room ini pondasi utama buat mengerti cara kerja **Burp Suite**, alat wajib buat setiap _Pentester_ atau _Bug Hunter_. Di sini kita bakal belajar gimana cara "berdiri di tengah" antara browser dan server buat memanipulasi traffic web sesuai keinginan.

---

### Understanding the Dashboard

Dashboard itu pusat kendali Burp Suite. Di versi **Community 2026.1.1** yang lagi aku pakai, fitur ini memberi ringkasan aktivitas sistem:

- **Task Log**: Menampilkan log dari aktivitas yang lagi jalan.
- **Issue Activity**: Tempat buat melihat temuan keamanan (manual di versi Community).
- **Shortcut**: `Ctrl + Shift + D` buat langsung loncat ke tab Dashboard.

---

### The Power of Burp Proxy

Proxy itu fitur paling iconic. Dia bertindak sebagai _Man-in-the-Middle_:

- **Interception**: Ngehentiin _request_ sebelum sampai ke server agar bisa dimodifikasi.
- **HTTP History**: Menyimpan log setiap _request_ yang lewat buat dianalisis nanti.
- **Intercept is on/off**: Tombol buat menentukan apakah trafik mau ditahan atau dibiarin lewat saja.

---

### Target Scoping & Site Map

Target Scope itu fitur di Burp Suite yang dipake buat menentukan URL atau domain mana saja yang bakal dijadiin target utama / prioritas.

- **Site Map**: Memberi gambaran hierarki struktur folder dan file dari aplikasi web target.
- **Scope**: Membuat kita bisa filter traffic agar Burp cuma nyatet domain yang lagi kita serang.

---

### Manual Tweaking with Repeater

Repeater itu tempat favorit buat yang suka ngulik manual:

- **Function**: Mengirim satu _request_ berkali-kali pakai modifikasi parameter yang beda-beda tanpa harus _refresh_ browser.
- **Shortcut**: `Ctrl + R` buat mengirim _request_ dari Proxy ke Repeater secara instan.

---

### Introduction to Intruder

Intruder dipake buat serangan otomatisasi skala kecil:

- **Payload Types**: Sniper (satu posisi), Battering Ram, Pitchfork, dan Cluster Bomb.
- **Throttling**: Perlu diingat, di versi **Community**, kecepatan Intruder dibatasi (_throttled_) sama PortSwigger.

---

### Decoder, Comparer, and Extender

Alat pendukung yang membuat Burp Suite makin lengkap:

- **Decoder**: Dipake buat _encoding/decoding_ data (Base64, URL, Hex) secara cepat.
- **Comparer**: Bandingin dua _response_ HTTP yang beda buat mencari perbedaan tipis (seperti waktu _blind SQL injection_).
- **Extender (BApp Store)**: Tempat masang ekstensi tambahan seperti **Logger++** atau **Turbo Intruder** buat ningkatin kemampuan Burp Suite.


---

## Attack Flow Awareness

Burp Suite berperan utama di tahap **Reconnaissance** dan **Initial Access** untuk web application:

| Tahap | Peran Burp Suite |
| ----- | ---------------- |
| **Reconnaissance** | Site Map memetakan seluruh struktur aplikasi web target, menemukan endpoint tersembunyi |
| **Initial Access** | Proxy + Repeater dipakai untuk menemukan dan mengeksploitasi vulnerability (SQLi, XSS, IDOR, dll) |
| **Post-Exploitation** | Menganalisis session tokens, cookies, dan mekanisme autentikasi untuk escalation |

**Perspektif Blue Team:**
- **WAF (Web Application Firewall):** Bisa mendeteksi pola request yang tidak wajar dari Burp Intruder (request massal ke satu endpoint).
- **Rate Limiting:** Membatasi jumlah request per IP buat memperlambat serangan otomatis.
- **Logging:** Setiap request yang melewati server harus di-log untuk analisis forensik pasca-insiden.

---

## For Real World Relevance

- **Bug Bounty:** Burp Suite adalah senjata utama Bug Hunter. Platform seperti HackerOne dan Bugcrowd dipenuhi laporan yang ditemukan menggunakan Burp Proxy + Repeater.
- **Web App Pentest:** Di engagement profesional, pentester menggunakan Burp Suite Pro untuk melakukan _active scanning_ dan menemukan vulnerability secara otomatis sebelum melakukan verifikasi manual.
- **OWASP Top 10:** Hampir semua vulnerability di OWASP Top 10 (SQLi, XSS, Broken Access Control, dll) bisa ditemukan dan dieksploitasi menggunakan Burp Suite.

---

## Questions

1. Kenapa memahami HTTP protocol adalah prasyarat mutlak sebelum bisa efektif menggunakan Burp Suite?
2. Apa keuntungan strategis menggunakan _Scope_ di Burp Suite saat melakukan pentest terhadap aplikasi web yang besar?
3. Dalam skenario _Blind SQL Injection_, bagaimana fitur **Comparer** di Burp Suite bisa membantu mengonfirmasi keberadaan vulnerability?
4. Jika kamu seorang Blue Teamer, bagaimana cara membedakan traffic normal dari traffic yang dimanipulasi menggunakan Burp Proxy di log server?
