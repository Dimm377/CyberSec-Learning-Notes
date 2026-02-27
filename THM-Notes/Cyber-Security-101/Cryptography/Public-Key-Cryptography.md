# TryHackMe: Public Key Cryptography Basics


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/publickeycrypto)
- **Category:** Cryptography
- **Difficulty:** Easy

---

---

## Introduction

**Konsep Dasar:**
Room ini membahas **Public Key Cryptography** (atau Asymmetric Encryption), yang jadi fondasi keamanan komunikasi internet sekarang (seperti HTTPS dan SSH). Beda sama Symmetric Encryption yang cuma pakai satu kunci, Asymmetric Encryption pakai sepasang kunci: **Public Key** dan **Private Key**.

---

## Common Use of Asymmetric Encryption

**Analogi Kunci:**
Bayangin sebuah kotak pos (public key) yang bisa diakses siapa saja buat memasukkan surat. Tapi, cuma orang yang megang kunci kotak pos itu (private key) yang bisa buka dan ambil surat di dalamnya.

**Perbedaan Utama:**

- **Symmetric Encryption:** Cepat, efisien buat data besar, tapi susah di distribusi kunci (key distribution problem).
- **Asymmetric Encryption:** Lebih lambat, tapi aman banget buat pertukaran kunci awal (Key Exchange) dan verifikasi identitas (Digital Signature).

---

## RSA

**RSA (Rivest-Shamir-Adleman):**
Algoritma asymmetric paling populer yang keamanannya tergantung sama **kesulitan memfaktorkan bilangan prima yang sangat besar**.

**Komponen Utama:**

1.  **Modulus (n):** Hasil perkalian dua bilangan prima besar (`p` dan `q`).
    - Rumus: `n = p * q`
2.  **Public Key (e):** Eksponen publik buat enkripsi (biasanya 65537).
3.  **Private Key (d):** Eksponen privat buat dekripsi, dihitung dari kebalikan `e` modulo `ϕ(n)`.
4.  **Totient (ϕ(n)):** Jumlah bilangan yang relatif prima terhadap `n`.
    - Rumus: `ϕ(n) = (p-1) * (q-1)`

**Cara Kerja:**

- **Enkripsi:** `Ciphertext = Message^e mod n`
- **Dekripsi:** `Message = Ciphertext^d mod n`

---

## Diffie-Hellman Key Exchange

**Konsep:**
Metode buat dua pihak (Alice dan Bob) menyepakati sebuah **Shared Secret Key** lewat jalur komunikasi yang tidak aman, tanpa pernah mengirim kunci rahasia itu sendiri.

**Variabel:**

- `g` (generator) dan `p` (prime): Angka publik yang diketahui semua orang.
- `a`: Kunci rahasia Alice.
- `b`: Kunci rahasia Bob.
- `A`: Kunci publik Alice (`g^a mod p`).
- `B`: Kunci publik Bob (`g^b mod p`).

**Proses Pertukaran:**

1.  Alice mengirim `A` ke Bob.
2.  Bob mengirim `B` ke Alice.
3.  Alice ngitung `Shared Secret = B^a mod p`.
4.  Bob ngitung `Shared Secret = A^b mod p`.
5.  Hasilnya pasti sama, dan nilai inilah yang jadi kunci rahasia bersama.

---

## SSH

**Secure Shell (SSH):**
Protokol jaringan yang pakai kriptografi kunci publik buat ngamanin login jarak jauh dan layanan jaringan lainnya.

**Implementasi:**
Waktu pakai SSH key authentication:

1.  Client punya sepasang kunci (Public & Private).
2.  Public Key ditaruh di server (dalam file `~/.ssh/authorized_keys`).
3.  Waktu login, server nantang client buat buktiin kepemilikan Private Key yang sesuai.
4.  Kalau berhasil, akses dikasih tanpa perlu mengirim password.

**Header Private Key:**
File Private Key RSA biasanya diawali pakai header:
`-----BEGIN RSA PRIVATE KEY-----`

---

## Digital Signatures and Certificates

**Digital Signature (Tanda Tangan Digital):**
Dipake buat buktiin **autentisitas** (pengirim asli) dan **integritas** (pesan tidak berubah) dari suatu pesan atau dokumen.

**Cara Kerja:**

- **Signing:** Pengirim membuat hash dari pesan, terus meng-enkripsi hash itu pakai **Private Key** miliknya.
- **Verifying:** Penerima men-dekripsi signature pakai **Public Key** pengirim buat mendapatkan hash asli, terus ngebandinginnya sama hash pesan yang diterima.

**Certificates (Sertifikat Digital):**
File yang ngikat Public Key sama identitas pemiliknya (seperti website), ditandatangani sama otoritas tepercaya (Certificate Authority/CA). Contoh CA gratis populer itu **Let's Encrypt**.

---

## PGP and GPG

**PGP (Pretty Good Privacy) & GPG (GNU Privacy Guard):**
Standar buat enkripsi file dan email. GPG itu implementasi open-source dari standar OpenPGP.

**Workflow Dasar:**

1.  **Import Key:** Import kunci publik/privat orang lain ke keyring lokal.
    - Command: `gpg --import keyfile.asc`
2.  **Encrypt:** Enkripsi file buat penerima tertentu.
    - Command: `gpg --recipient user --encrypt file.txt`
3.  **Decrypt:** Buka file terenkripsi pakai Private Key kita.
    - Command: `gpg --decrypt file.gpg`

**Passphrase:**
Private Key GPG biasanya dilindungi pakai passphrase tambahan sebagai lapisan keamanan ekstra.
