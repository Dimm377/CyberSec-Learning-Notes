# TryHackMe: Public Key Cryptography

**Room Link:** [TryHackMe](https://tryhackme.com/room/publickeycrypto)
**Category:** Cryptography / Security
**Difficulty:** Easy

## Task 1: Introduction

Di room sebelumnya (Cryptography Basics), kita udah belajar tentang enkripsi simetris (Symmetric Encryption), di mana satu kunci yang sama dipake buat ngunci (encrypt) dan buka (decrypt) data.

Tapi, enkripsi simetris punya masalah besar: **Gimana cara kita ngasih kunci rahasia itu ke temen kita tanpa ketauan orang lain?** Kalau kita kirim kuncinya lewat internet biasa, bisa aja ada _hacker_ yang ngintip dan nyolong kuncinya. Begitu kuncinya bocor, semua komunikasi kita jadi nggak aman lagi.

Nah, di room ini kita bakal belajar solusi cerdas buat masalah itu, yaitu **Public Key Cryptography** (atau Asymmetric Encryption). Kita bakal bahas algoritma sakti kayak **Diffie-Hellman Key Exchange** dan **RSA**, yang jadi pondasi keamanan internet modern (termasuk HTTPS yang lu pake sekarang).

_(No Answer Needed)_

---

## Task 2: Common Use of Asymmetric Encryption

Task ini menjelaskan kegunaan utama dari enkripsi asimetris dalam komunikasi modern.

### Konsep Utama:

1. **Kecepatan:** Enkripsi asimetris jauh lebih **lambat** dibandingkan enkripsi simetris.
2. **Kegunaan Utama:** Karena lambat, kita nggak pake asimetris buat kirim data berukuran besar. Kita pake asimetris buat **bernegosiasi dan menyepakati kunci simetris** (Key Exchange) yang bakal dipake buat sesi komunikasi selanjutnya.
3. **Analogi Pertemuan Bisnis:** Room ini pake analogi meeting buat jelasin 4 pilar keamanan:
   - **Authentication:** Memastikan identitas lawan bicara.
   - **Authenticity:** Memastikan pesan bener-bener dari pengirim yang sah.
   - **Integrity:** Memastikan pesan nggak diubah di tengah jalan.
   - **Confidentiality:** Memastikan cuma pihak berwenang yang bisa denger/baca.

### Analogi Gembok:

Kalo lu mau temen lu kirim pesan rahasia, lu kasih dia sebuah **gembok** yang terbuka. Cuma lu yang pegang kuncinya. Temen lu masukin pesan ke kotak, lalu dikunci pake gembok itu

## Task 3: RSA

RSA (Rivest-Shamir-Adleman) adalah algoritma asimetris yang keamanannya bergantung pada kesulitan memfaktorkan perkalian dua bilangan prima besar.

### Komponen Utama:

- **$p$ & $q$**: Dua bilangan prima rahasia.
- **$n$ (Modulus)**: Dihitung dengan $n = p \times q$.
- **$\phi(n)$ (Euler's Totient)**: Dihitung dengan $\phi(n) = (p - 1) \times (q - 1)$.
- **$e$ (Public Exponent)**: Bagian dari kunci publik untuk enkripsi.
- **$d$ (Private Exponent)**: Bagian dari kunci privat untuk dekripsi.

## Task 4: Diffie-Hellman Key Exchange

Diffie-Hellman adalah metode untuk bertukar kunci kriptografi secara aman melalui saluran publik. Algoritma ini tidak digunakan untuk enkripsi data secara langsung, melainkan untuk menyepakati sebuah kunci rahasia bersama.

### Parameter Utama:

- **p**: Bilangan prima besar (modulus).
- **g**: Generator (bilangan dasar).
- **a & b**: Kunci rahasia milik Alice dan Bob.
- **A & B**: Nilai publik yang dikirimkan satu sama lain.

### Perhitungan Langkah demi Langkah:

1. **Menghitung Nilai Publik Alice (A):**
   $A = g^a \pmod p$
2. **Menghitung Nilai Publik Bob (B):**
   $B = g^b \pmod p$
3. **Menghitung Kunci Rahasia Bersama:**
   - Alice menghitung: $Key = B^a \pmod p$
   - Bob menghitung: $Key = A^b \pmod p$
     _(Hasil akhirnya harus sama)_
