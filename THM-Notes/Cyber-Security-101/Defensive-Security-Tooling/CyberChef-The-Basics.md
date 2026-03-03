# TryHackMe: CyberChef The Basics

- **Room Link:** [CyberChef The Basics](https://tryhackme.com/room/cyberchefthebasics)
- **Category:** Defensive Security Tooling
- **Difficulty:** Easy

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
- Apa itu CyberChef dan bagaimana cara kerjanya.
- Cara navigasi antarmuka CyberChef.
- Operasi-operasi umum yang sering dipakai.
- Cara membuat _recipe_ dan memproses data.

---

## Accessing the Tool

Ada dua cara untuk mengakses dan menjalankan CyberChef:

### Online Access

Cara paling mudah — cukup punya **browser** dan **koneksi internet**. Langsung buka lewat link resminya:
[https://gchq.github.io/CyberChef/](https://gchq.github.io/CyberChef/)

### Offline / Local Copy

Jika kamu harus bekerja di lingkungan yang terisolasi (tanpa internet), kamu bisa mendownload file rilisnya langsung dari [GitHub repository CyberChef](https://github.com/gchq/CyberChef/releases). Ini penting untuk menjaga kerahasiaan data sensitif agar tidak keluar ke internet.

| Metode | Kebutuhan | Kapan Dipakai |
| ------ | --------- | ------------- |
| **Online** | Browser + internet | Penggunaan cepat, belajar, atau data tidak sensitif |
| **Offline** | Download file rilis | Analisis malware, data rahasia, atau lab terisolasi |

---

## Navigating the Interface

CyberChef terdiri dari 4 area utama:

1. **Operations**: Perpustakaan operasi.
2. **Recipe**: Tempat menyusun langkah-langkah (logika).
3. **Input**: Data mentah yang mau diproses.
4. **Output**: Hasil akhir setelah diproses.

### Operations Area

Ini adalah daftar lengkap semua kemampuan CyberChef. Gunakan fitur **search** jika kamu sudah tahu apa yang dicari. Beberapa operasi penting:

- **From Morse Code**: Mengubah titik-garis jadi teks.
- **URL Decode**: Membersihkan karakter aneh di URL (seperti `%20` jadi spasi).
- **To Base64**: Mengubah teks jadi format Base64 yang sering dipakai untuk menyembunyikan payload.
- **ROT13**: Menggeser huruf (cipher sederhana) untuk mengaburkan teks.

### Recipe Area

Ini adalah **jantung dari CyberChef**. Di sini kamu menentukan urutan kerja. Operasi akan dijalankan dari **atas ke bawah**.

Fitur penting:
- **`BAKE!`**: Tombol eksekusi.
- **`Auto Bake`**: Jika aktif, hasil akan langsung muncul setiap kali ada perubahan di Input atau Recipe.

---

## Attack Flow Awareness

CyberChef adalah alat krusial dalam attack chain dan pertahanan:

1. **Initial Access**: Attacker sering mengirim payload yang di-_encode_ dengan Base64 atau XOR untuk melewati firewall/antivirus. Defender menggunakan CyberChef untuk men-_decode_ payload tersebut dan melihat niat asli attacker.
2. **Exfiltration**: Sebelum mencuri data, attacker mungkin mengompres atau mengenkripsi data tersebut. CyberChef membantu menganalisis format data yang akan keluar dari jaringan.
3. **Reconnaissance**: Menemukan string terenkripsi dalam script JavaScript di sebuah website? CyberChef adalah alat pertama untuk mencoba berbagai cipher umum.

---

## For Real World Relevance

Di dunia nyata, analis SOC (*Security Operations Center*) menggunakan CyberChef setiap hari untuk:
- **De-obfuscation**: Membongkar script PowerShell atau JavaScript jahat yang sengaja dibuat sulit dibaca.
- **Log Analysis**: Membersihkan log dari format URL encoding agar bisa dibaca manusia.
- **Malware Analysis**: Mengekstrak konfigurasi (seperti alamat C2 server) dari file malware yang terenkripsi sederhana.

---

## Questions

1. Mengapa urutan operasi dalam **Recipe Area** sangat krusial?
2. Dalam tahap *Initial Access*, apa kegunaan utama CyberChef bagi seorang Defender?
3. Jika kamu menemukan string yang terlihat seperti Base64 namun gagal di-_decode_ menggunakan operasi `From Base64` standar, apa langkah investigasi selanjutnya yang akan kamu lakukan di CyberChef?
4. Bagaimana cara memitigasi risiko attacker menggunakan teknik *encoding* berlapis untuk menyembunyikan serangan mereka?

---
