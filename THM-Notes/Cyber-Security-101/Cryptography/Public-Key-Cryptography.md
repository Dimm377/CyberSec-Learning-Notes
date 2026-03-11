# TryHackMe: Public Key Cryptography Basics


---

* **Room Link:** [TryHackMe](https://tryhackme.com/room/publickeycrypto)
* **Category:** Cryptography
* **Difficulty:** Easy

---

## Introduction

Room ini membahas **Public Key Cryptography** (Asymmetric Encryption) — pondasi keamanan komunikasi internet modern (HTTPS, SSH, Digital Signatures). Beda sama Symmetric Encryption yang cuma pakai satu kunci, Asymmetric pakai sepasang kunci: **Public Key** dan **Private Key**.

(Jika kamu belum membaca dasar-dasar kriptografi, mulai dulu dari [Cryptography Basic](file:///home/dimm/CyberSec-Learning-Notes/THM-Notes/Cyber-Security-101/Cryptography/Cryptography-Basic.md). Penerapan PKI di protokol jaringan dibahas di [Networking Secure Protocols](file:///home/dimm/CyberSec-Learning-Notes/THM-Notes/Cyber-Security-101/Networking/Networking-Secure-Protocols.md))

*(Konsep dasar cryptography ada di catatan [Cryptography Basic](./Cryptography-Basic.md))*

---

### Symmetric vs Asymmetric

Bayangkan sebuah **kotak pos** (Public Key) yang bisa diakses siapa saja buat memasukkan surat. Tapi cuma orang yang pegang kunci kotak pos itu (Private Key) yang bisa buka dan ambil surat di dalamnya.

| Aspek | Symmetric | Asymmetric |
| ----- | --------- | ---------- |
| **Jumlah Kunci** | 1 (sama buat encrypt/decrypt) | 2 (Public + Private) |
| **Kecepatan** | Cepat, efisien buat data besar | Lebih lambat |
| **Kelemahan** | Susah distribusi kunci aman | Komputasi lebih berat |
| **Use Case** | Enkripsi data bulk (AES) | Key Exchange, Digital Signature |

---

### RSA (Rivest-Shamir-Adleman)

Algoritma asymmetric paling populer — keamanannya tergantung pada **kesulitan memfaktorkan bilangan prima yang sangat besar**:

| Komponen | Rumus / Detail |
| -------- | -------------- |
| **Modulus (n)** | `n = p × q` (p dan q = bilangan prima besar) |
| **Totient ϕ(n)** | `ϕ(n) = (p-1) × (q-1)` |
| **Public Key (e)** | Eksponen publik buat enkripsi (biasanya 65537) |
| **Private Key (d)** | Eksponen privat buat dekripsi — kebalikan `e` modulo `ϕ(n)` |

| Proses | Formula |
| ------ | ------- |
| **Enkripsi** | `Ciphertext = Message^e mod n` |
| **Dekripsi** | `Message = Ciphertext^d mod n` |

---

### Diffie-Hellman Key Exchange

Metode buat dua pihak (Alice dan Bob) menyepakati **Shared Secret Key** lewat jalur yang tidak aman — tanpa pernah mengirim kunci rahasia itu sendiri:

| Langkah | Alice | Bob |
| :-----: | ----- | --- |
| **1** | Hitung `A = g^a mod p`, kirim `A` ke Bob | Hitung `B = g^b mod p`, kirim `B` ke Alice |
| **2** | Terima `B`, hitung `Secret = B^a mod p` | Terima `A`, hitung `Secret = A^b mod p` |
| **3** | Hasilnya **sama** → jadi kunci rahasia bersama | Hasilnya **sama** → jadi kunci rahasia bersama |

> `g` (generator) dan `p` (prime) = angka publik. `a` dan `b` = kunci rahasia masing-masing.

---

### SSH (Secure Shell)

Protokol jaringan yang pakai kriptografi kunci publik buat mengamankan login jarak jauh:

*(Penjelasan SSH juga ada di catatan [Networking Secure Protocols](../Networking/Networking-Secure-Protocols.md))*

| Langkah | Proses |
| :-----: | ------ |
| **1** | Client punya sepasang kunci (Public & Private) |
| **2** | Public Key ditaruh di server (`~/.ssh/authorized_keys`) |
| **3** | Saat login, server menantang client buat membuktikan kepemilikan Private Key |
| **4** | Kalau berhasil, akses diberikan tanpa perlu mengirim password |

> **Header Private Key RSA:** `-----BEGIN RSA PRIVATE KEY-----`

---

### Digital Signatures & Certificates

**Digital Signature** — membuktikan **autentisitas** (pengirim asli) dan **integritas** (pesan tidak berubah):

| Proses | Cara Kerja |
| ------ | ---------- |
| **Signing** | Pengirim membuat hash dari pesan → mengenkripsi hash pakai **Private Key** miliknya |
| **Verifying** | Penerima mendekripsi signature pakai **Public Key** pengirim → membandingkan dengan hash pesan yang diterima |

**Certificates (Sertifikat Digital):** File yang mengikat Public Key sama identitas pemiliknya (seperti website), ditandatangani Certificate Authority (CA). Contoh CA gratis populer: **Let's Encrypt**.

---

### PGP & GPG

**PGP** (Pretty Good Privacy) & **GPG** (GNU Privacy Guard) — standar buat enkripsi file dan email. GPG itu implementasi open-source dari standar OpenPGP.

| Langkah | Command | Fungsi |
| ------- | ------- | ------ |
| **Import Key** | `gpg --import keyfile.asc` | Import kunci publik/privat ke keyring lokal |
| **Encrypt** | `gpg --recipient user --encrypt file.txt` | Enkripsi file buat penerima tertentu |
| **Decrypt** | `gpg --decrypt file.gpg` | Buka file terenkripsi pakai Private Key |

> **Passphrase:** Private Key GPG biasanya dilindungi pakai passphrase tambahan sebagai lapisan keamanan ekstra.
