# TryHackMe: CyberChef The Basics

* **Room Link:** [CyberChef The Basics](https://tryhackme.com/room/cyberchefthebasics)
* **Category:** Defensive Security Tooling
* **Difficulty:** Easy

## Introduction

### What is CyberChef?

Bayangkan kamu punya **pisau Swiss Army** — satu alat yang punya puluhan fungsi berbeda dalam satu genggaman. Itulah CyberChef: sebuah aplikasi web yang ringan dan intuitif, dirancang khusus untuk berbagai _cyber operations_ langsung dari browser tanpa perlu install apapun.

CyberChef bekerja dengan konsep **Recipe** — yaitu serangkaian operasi yang dijalankan secara berurutan terhadap sebuah data. Cukup susun operasi-operasi yang kamu butuhkan, lalu CyberChef akan memprosesnya satu per satu secara otomatis sesuai urutan yang kamu buat.

Contoh operasi yang bisa dilakukan:

| Kategori | Contoh Operasi |
| -------- | -------------- |
| **Encoding sederhana** | XOR, Base64, Base85, Morse Code |
| **Kriptografi lanjutan** | AES Encryption, RSA Decryption |
| **Analisis data** | Extract IP Address, parsing teks, regex |

### Learning Objectives

Kita akan mempelajari:
* Apa itu CyberChef dan bagaimana cara kerjanya.
* Cara navigasi antarmuka CyberChef.
* Operasi-operasi umum yang sering dipakai.
* Cara membuat _recipe_ dan memproses data.

---

## Accessing the Tool

Ada dua cara untuk mengakses dan menjalankan CyberChef:

### Online Access

Cara paling mudah — cukup punya **browser** dan **koneksi internet**. Langsung buka lewat link resminya:
[https://gchq.github.io/CyberChef/](https://gchq.github.io/CyberChef/)

### Offline / Local Copy

Jika kamu terjebak menganalisis *malware* di lingkungan terisolasi yang sama sekali tak punya akses internet, jangan cemas. Kamu bisa menginstall CyberChef langsung dari [GitHub repository resmi CyberChef](https://github.com/gchq/CyberChef/releases). Trik operasi tertutup ini wajib kamu pegang demi menjamin kerahasiaan data *client* agar tidak bocor ke luar.

| Metode | Kebutuhan | Kapan Dipakai |
| ------ | --------- | ------------- |
| **Online** | Browser + internet | Penggunaan cepat, belajar, atau data tidak sensitif |
| **Offline** | Install CyberChef | Analisis malware, data rahasia, atau lab terisolasi |

---

## Navigating the Interface

CyberChef terdiri dari 4 area utama:

1. **Operations**: Perpustakaan operasi.
2. **Recipe**: Tempat menyusun langkah-langkah (logika).
3. **Input**: Data mentah yang mau diproses.
4. **Output**: Hasil akhir setelah diproses.

### Operations Area

Ini adalah daftar lengkap semua kemampuan CyberChef. Gunakan fitur **search** jika kamu sudah tahu apa yang dicari. Beberapa operasi penting:

* **From Morse Code**: Mengubah titik-garis jadi teks.
* **URL Decode**: Membersihkan karakter aneh di URL (seperti `%20` jadi spasi).
* **To Base64**: Mengubah teks jadi format Base64 yang sering dipakai untuk menyembunyikan payload.
* **ROT13**: Menggeser huruf (cipher sederhana) untuk mengaburkan teks.

### Recipe Area

Ini adalah **jantung dari CyberChef**. Di sini kamu menentukan urutan kerja. Operasi akan dijalankan dari **atas ke bawah**.

Fitur penting:
* **`BAKE!`**: Tombol eksekusi.
* **`Auto Bake`**: Jika aktif, hasil akan langsung muncul setiap kali ada perubahan di Input atau Recipe.

---

## Attack Flow Awareness

CyberChef adalah alat krusial dalam attack chain dan pertahanan:

1. **Initial Access**: Attacker sering mengirim payload yang di-_encode_ dengan Base64 atau XOR untuk melewati firewall/antivirus. Defender menggunakan CyberChef untuk men-_decode_ payload tersebut dan melihat niat asli attacker.
2. **Exfiltration**: Sebelum mencuri data, attacker mungkin mengompres atau mengenkripsi data tersebut. CyberChef membantu menganalisis format data yang akan keluar dari jaringan.
3. **Reconnaissance**: Menemukan string terenkripsi dalam script JavaScript di sebuah website? CyberChef adalah alat pertama untuk mencoba berbagai cipher umum.

---

## For Real World Relevance

Di dunia nyata, analis SOC (*Security Operations Center*) menggunakan CyberChef setiap hari untuk:
* **De-obfuscation**: Membongkar script PowerShell atau JavaScript jahat yang sengaja dibuat sulit dibaca.
* **Log Analysis**: Membersihkan log dari format URL encoding agar bisa dibaca manusia.
* **Malware Analysis**: Mengekstrak konfigurasi (seperti alamat C2 server) dari file malware yang terenkripsi sederhana.

---

## Questions

1. Mengapa urutan operasi dalam **Recipe Area** sangat krusial?
2. Dalam tahap *Initial Access*, apa kegunaan utama CyberChef bagi seorang Defender?
3. Jika kamu menemukan string yang terlihat seperti Base64 namun gagal di-_decode_ menggunakan operasi `From Base64` standar, apa langkah investigasi selanjutnya yang akan kamu lakukan di CyberChef?
4. Bagaimana cara memitigasi risiko attacker menggunakan teknik *encoding* berlapis untuk menyembunyikan serangan mereka?

---

## Practice, Practice, Practice

Agar instingmu tajam layaknya chef bintang lima saat mengulik data, kamu sangat disarankan untuk mengenali dan menghafalkan **kategori operasi** yang jadi "menu harian". Percayalah, pemahaman ini bakal menyelamatkan kewarasanmu di tengah kompetisi CTF.

### Extractors

Kategori **Extractors** digunakan untuk **menarik informasi spesifik** dari sebuah teks atau data besar. Sangat berguna saat analisis log, email phishing, atau output malware.

| Operasi | Fungsi |
| ------- | ------ |
| `Extract IP addresses` | Mengekstrak semua alamat **IPv4 dan IPv6** yang valid dari input |
| `Extract URLs` | Mengekstrak semua **URL** dari input — protokol (`http`, `ftp`, dll.) wajib ada, kalau tidak akan terlalu banyak _false positive_ |
| `Extract email addresses` | Mengekstrak semua **alamat email** dari input (format: `apapun@domain.com`) |

**Contoh penggunaan `Extract email addresses`:**
Jika input mengandung teks panjang, operasi ini akan otomatis menarik semua string yang berbentuk `sesuatu@domain.com` — contoh: `support@hotmail.com`, `noreply@google.com`, `admin@user.com`.

---

### Date and Time

Kategori **Date / Time** digunakan untuk mengkonversi format waktu, khususnya yang berkaitan dengan **UNIX Timestamp**.

| Operasi | Fungsi |
| ------- | ------ |
| `From UNIX Timestamp` | Mengubah UNIX timestamp menjadi format tanggal/waktu yang bisa dibaca |
| `To UNIX Timestamp` | Mengubah string tanggal/waktu menjadi UNIX timestamp |

**Apa itu UNIX Timestamp?**
Sebuah nilai 32-bit yang merepresentasikan jumlah detik sejak **1 Januari 1970 UTC** (_UNIX epoch_).

**Contoh:**
* `Fri Sep 6 20:30:22 +04 2024` → `To UNIX Timestamp` → `1725654622`
* `1725654622` → `From UNIX Timestamp` → `Fri Sep 6 20:30:22 +04 2024`

### Data Format

Kategori **Data Format** berisi operasi-operasi untuk mengkonversi data antar format encoding. Ini adalah kategori yang **paling sering dipakai** dalam analisis keamanan.

| Operasi | Fungsi | Contoh |
| ------- | ------ | ------ |
| `From Base64` | Decode string Base64 kembali ke data asli | `V2VsY29tZSB0byBUcnlIYWNrTWUh` → `Welcome to tryhackme!` |
| `URL Decode` | Decode karakter _percent-encoded_ URI/URL ke nilai aslinya | `https%3A%2F%2Fgchq.github.io%2FCyberChef%2F` → `https://gchq.github.io/CyberChef/` |
| `From Base85` | Decode string Base85 — lebih efisien dari Base64 | `BOu!rDIj7BEbo7` → `hello world` |
| `From Base58` | Decode Base58 — menghilangkan karakter yang mudah tertukar (`l`, `I`, `0`, `O`) | `AXLU7qR` → `Thm58` |
| `To Base62` | Encode data ke format Base62 — menghasilkan string yang lebih pendek | `Thm62` → `6NiRkOY` |

**Apa itu Base Encodings?**
Operasi seperti `Base(64, 85, 58, 62)` termasuk kategori **base encodings** — yaitu teknik mengubah data biner (string 0 dan 1) menjadi representasi teks menggunakan kumpulan karakter ASCII tertentu. Masing-masing varian punya keunggulan tersendiri tergantung skenario penggunaannya.

---
