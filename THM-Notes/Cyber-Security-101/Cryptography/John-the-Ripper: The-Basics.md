# TryHackMe: John the Ripper - The Basics


---

- **Room Link:** [John the Ripper Basics](https://tryhackme.com/room/johntheripperbasics)
- **Category:** Cryptography / Password Cracking
- **Difficulty:** Easy

---

## Task 1: Introduction

**Apa itu John the Ripper (JtR)?**  
John the Ripper adalah salah satu tool "password cracker" paling populer dan fleksibel. Tool ini dirancang untuk mendeteksi password yang lemah (weak password) dengan cara melakukan proses cracking hash penggunanya.
*   **Kecepatan Tinggi:** Mampu melakukan cracking dengan sangat cepat.
*   **Fleksibilitas:** Mendukung ratusan format hash (bukan cuma password user, tapi juga file ZIP terenkripsi, kunci SSH, dll).
*   **Open Source:** Gratis dan bisa dimodifikasi.

**Prasyarat:**
Untuk menggunakan JtR dengan efektif, perlu memahami dasar-dasar:
1.  **Command Line (Linux/Unix):** Navigasi folder, menjalankan command.
2.  **Hashing Basics:** Paham bedanya hashing vs enkripsi.

---

## Task 2: Basic Terms

Sebelum memulai cracking, kita harus paham dulu istilah-istilah dasarnya:

1.  **Hash:**
    *   Hash adalah representasi data (seperti password) yang telah diubah menjadi string karakter dengan panjang tetap (fixed-length) menggunakan algoritma matematika tertentu (MD5, SHA1, SHA256, dll).
    *   Sifat utama hash adalah **One-Way Function** (Satu Arah). Kita bisa ubah "password123" jadi hash, tapi SANGAT SULIT (secara komputasi mustahil) mengubah hash kembali jadi "password123" secara langsung (dekripsi).

2.  **Dictionary Attack:**
    *   Karena hash tidak bisa didekripsi, cara paling umum untuk memecahkannya adalah dengan **menebak**.
    *   Kita punya daftar kata-kata yang mungkin jadi password (**Wordlist** / Dictionary), misal: `rockyou.txt`.
    *   JtR akan mengambil setiap kata dari wordlist -> mengubahnya jadi hash -> membandingkan dengan hash target. Jika cocok, berarti password ketemu!

3.  **Hash Identifier:**
    *   Sebelum me-crack hash, kita HARUS tahu dulu jenis algoritmanya (apakah MD5? SHA256? NTLM?).
    *   Tools seperti `hash-identifier` atau layanan online bisa membantu mengenali jenis hash dari format/panjangnya.

---

## Task 3: Setting Up Your System

Pada task ini, kita akan mempersiapkan environment untuk menggunakan John the Ripper.

**1. Instalasi John the Ripper:**
Jika belum terinstall (biasanya sudah ada di Kali Linux/Parrot), bisa diinstall dengan command:
```bash
sudo apt install john
```

**2. Wordlist RockYou:**
Wordlist yang paling umum digunakan untuk CTF dan belajar adalah `rockyou.txt`.
*   **Lokasi:** `/usr/share/wordlists/`
*   **Decompress:** Jika masih dalam bentuk `.gz`, perlu diekstrak dulu:
    ```bash
    sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
    ```

**Pertanyaan:**
*   **Which website's breach was the rockyou.txt wordlist created from?**
    *   Jawaban: `rockyou.com` (Wordlist ini berasal dari data breach situs sosial media RockYou pada tahun 2009).

---

## Task 4: Wordlists & Regex

Pada task ini, kita belajar cara menggunakan wordlist dan mengidentifikasi jenis hash.

**1. Identifikasi Hash:**
Sebelum cracking, kita harus tahu jenis hash-nya. Bisa pakai tool bawaan `hash-identifier` atau script `hash-id.py` yang disediakan di soal.
```bash
hash-identifier
# Paste hash-nya
```

**2. Cracking Process:**
Gunakan command: `john --format=[format] --wordlist=[path_wordlist] [file_hash]`

Berikut adalah jawaban untuk latihan hash yang diberikan:

*   **Hash 1 (MD5):**
    *   Identifikasi: Hash `1A1DC91C907325C69271DDF0C944BC72` terdeteksi sebagai **MD5**.
    *   Command: `john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt`

*   **Hash 2 (SHA1):**
    *   Identifikasi: Hash terdeteksi sebagai **SHA1**.
    *   Command: `john --format=raw-sha1 --wordlist=/usr/share/wordlists/rockyou.txt hash2.txt`

*   **Hash 3 (SHA256):**
    *   Identifikasi: Hash terdeteksi sebagai **SHA256**.
    *   Command: `john --format=raw-sha256 --wordlist=/usr/share/wordlists/rockyou.txt hash3.txt`

*   **Hash 4 (Whirlpool):**
    *   Identifikasi: Hash terdeteksi sebagai **Whirlpool**.
    *   Command: `john --format=whirlpool --wordlist=/usr/share/wordlists/rockyou.txt hash4.txt`
