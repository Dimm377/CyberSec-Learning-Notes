# TryHackMe: Walking An Application

- **Room Link:** [Walking An Application](https://tryhackme.com/room/walkinganapplication)
- **Category:** Introduction to Web Hacking
- **Difficulty:** Easy

## Walking An Application

---

## Introduction

Walking an application adalah proses navigasi aplikasi web secara manual menggunakan browser untuk memahami struktur, fungsionalitas, dan mencari informasi sensitif yang terekspos. Ini adalah langkah krusial dalam **Web Reconnaissance**.

### Attack Context

* **Kapan teknik ini dipakai?** Tahap **Reconnaissance / Enumeration** — langkah awal untuk memetakan aplikasi web.
* **Syarat apa yang dibutuhkan?** Akses ke browser (Firefox, Chromium) dan target URL.
* **Apa tanda keberhasilannya?** Ditemukannya halaman rahasia, file konfigurasi, atau komentar dev di dalam source code.

---

## Exploring the Application

### 1. Inspecting Source Code
Selalu cek **Page Source code** untuk mencari informasi menarik.
* **Kapan dipakai?** Saat ingin melihat komentar tersembunyi atau link yang tidak muncul di UI.
* **Syarat?** Klik kanan -> `View Page Source`.
* **Tanda keberhasilan?** Menemukan komentar `<!-- TODO: remove this test user -->` atau path folder rahasia.

### 2. Inspecting Network Traffic
Gunakan **Developer Tools** (F12) tab **Network**.
* **Kapan dipakai??** Untuk melihat bagaimana aplikasi berkomunikasi dengan backend.
* **Tanda keberhasilan?** Melihat API request yang membawa data sensitif dalam format JSON.

### 3. Robots.txt & Sitemap.xml
Selalu cek file standar ini.
* **Robots.txt**: Memberi tahu crawler (seperti Google) folder mana yang tidak boleh di-indeks. Attacker justru akan mengecek folder terlarang itu.
* **Sitemap.xml**: Daftar lengkap halaman web yang ada di server.

---

## Real-World Relevance

Meskipun terlihat sederhana, banyak bug bounty hunter menemukan celah besar hanya dengan "jalan-jalan" di aplikasi dan membaca source code yang kelihatannya membosankan. Pengembang seringkali lupa menghapus info debug atau komentar penting sebelum merilis aplikasi ke publik.

### Pertanyaan Singkat

1. Di mana biasanya developer meninggalkan catatan atau komentar di aplikasi web?
2. Apa fungsi dari file `robots.txt`?
3. Mengapa `Developer Tools` sangat berharga bagi seorang web pentester?
