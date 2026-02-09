# TryHackMeL Public Key Cryptography Basics


---

**Room Link:** [TryHackMe](https://tryhackme.com/room/publickeycrypto)
**Category:** Cryptography
**Difficulty:** Easy

---

---

## Task 1: Introduction

**Konsep Dasar:**
Room ini membahas tentang **Public Key Cryptography** (atau Asymmetric Encryption), yang menjadi fondasi keamanan komunikasi internet saat ini (seperti HTTPS dan SSH). Berbeda dengan Symmetric Encryption yang hanya menggunakan satu kunci (kunci yang sama untuk encrypt dan decrypt), Asymmetric Encryption menggunakan sepasang kunci: **Public Key** dan **Private Key**.

---

## Task 2: Common Use of Asymmetric Encryption

**Analogi Kunci:**
Bayangkan sebuah kotak pos (public key) yang bisa diakses siapa saja untuk memasukkan surat. Namun, hanya orang yang memegang kunci kotak pos tersebut (private key) yang bisa membuka dan mengambil surat di dalamnya.

**Perbedaan Utama:**

- **Symmetric Encryption:** Cepat, efisien untuk data besar, tapi sulit dalam distribusi kunci (key distribution problem).
- **Asymmetric Encryption:** Lebih lambat, tapi sangat aman untuk pertukaran kunci awal (Key Exchange) dan verifikasi identitas (Digital Signature).

---

## Task 3: RSA

**RSA (Rivest-Shamir-Adleman):**
Algoritma asymmetric paling populer yang keamanannya bergantung pada **kesulitan memfaktorkan bilangan prima yang sangat besar**.

**Komponen Utama:**

1.  **Modulus (n):** Hasil perkalian dua bilangan prima besar (`p` dan `q`).
    - Rumus: `n = p * q`
2.  **Public Key (e):** Eksponen publik untuk enkripsi (biasanya 65537).
3.  **Private Key (d):** Eksponen privat untuk dekripsi, dihitung dari kebalikan `e` modulo `ϕ(n)`.
4.  **Totient (ϕ(n)):** Jumlah bilangan yang relatif prima terhadap `n`.
    - Rumus: `ϕ(n) = (p-1) * (q-1)`

**Cara Kerja:**

- **Enkripsi:** `Ciphertext = Message^e mod n`
- **Dekripsi:** `Message = Ciphertext^d mod n`

---

## Task 4: Diffie-Hellman Key Exchange

**Konsep:**
Metode untuk dua pihak (Alice dan Bob) menyepakati sebuah **Shared Secret Key** melalui jalur komunikasi yang tidak aman, tanpa pernah mengirimkan kunci rahasia itu sendiri.

**Variabel:**

- `g` (generator) dan `p` (prime): Angka publik yang diketahui semua orang.
- `a`: Kunci rahasia Alice.
- `b`: Kunci rahasia Bob.
- `A`: Kunci publik Alice (`g^a mod p`).
- `B`: Kunci publik Bob (`g^b mod p`).

**Proses Pertukaran:**

1.  Alice mengirim `A` ke Bob.
2.  Bob mengirim `B` ke Alice.
3.  Alice menghitung `Shared Secret = B^a mod p`.
4.  Bob menghitung `Shared Secret = A^b mod p`.
5.  Hasilnya pasti sama, dan nilai inilah yang menjadi kunci rahasia bersama.

---

## Task 5: SSH

**Secure Shell (SSH):**
Protokol jaringan yang menggunakan kriptografi kunci publik untuk mengamankan login jarak jauh dan layanan jaringan lainnya.

**Implementasi:**
Saat menggunakan SSH key authentication:

1.  Client punya sepasang kunci (Public & Private).
2.  Public Key diletakkan di server (dalam file `~/.ssh/authorized_keys`).
3.  Saat login, server menantang client untuk membuktikan kepemilikan Private Key yang sesuai.
4.  Jika berhasil, akses diberikan tanpa perlu mengirim password.

**Header Private Key:**
File Private Key RSA biasanya diawali dengan header:
`-----BEGIN RSA PRIVATE KEY-----`

---

## Task 6: Digital Signatures and Certificates

**Digital Signature (Tanda Tangan Digital):**
Digunakan untuk membuktikan **autentisitas** (pengirim asli) dan **integritas** (pesan tidak berubah) dari suatu pesan atau dokumen.

**Cara Kerja:**

- **Signing:** Pengirim membuat hash dari pesan, lalu meng-enkripsi hash tersebut dengan **Private Key** miliknya.
- **Verifying:** Penerima men-dekripsi signature dengan **Public Key** pengirim untuk mendapatkan hash asli, lalu membandingkannya dengan hash pesan yang diterima.

**Certificates (Sertifikat Digital):**
File yang mengikat Public Key dengan identitas pemiliknya (seperti website), ditandatangani oleh otoritas tepercaya (Certificate Authority/CA). Contoh CA gratis populer adalah **Let's Encrypt**.

---

## Task 7: PGP and GPG

**PGP (Pretty Good Privacy) & GPG (GNU Privacy Guard):**
Standar untuk enkripsi file dan email. GPG adalah implementasi open-source dari standar OpenPGP.

**Workflow Dasar:**

1.  **Import Key:** Mengimpor kunci publik/privat orang lain ke keyring lokal.
    - Command: `gpg --import keyfile.asc`
2.  **Encrypt:** Mengenkripsi file untuk penerima tertentu.
    - Command: `gpg --recipient user --encrypt file.txt`
3.  **Decrypt:** Membuka file terenkripsi menggunakan Private Key kita.
    - Command: `gpg --decrypt file.gpg`

**Passphrase:**
Private Key GPG biasanya dilindungi oleh passphrase tambahan sebagai lapisan keamanan ekstra.
