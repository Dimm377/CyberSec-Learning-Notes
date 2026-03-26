# TryHackMe: Crack the hash

- **Room Link:** [Crack the hash](https://tryhackme.com/room/crackthehash)
- **Category:** Challenge Room
- **Difficulty:** Easy
- **Tools Used:** Hashcat examples list, CrackStation, Hashcat, Hashes.com
- **Main Techniques:** Hash identification, online hash cracking, offline hash cracking, brute force

---

## Overview

Room ini melatih skill dasar kriptografi ofensif, yaitu **Password Cracking**. Di Task 1 (Level 1), kita diberikan 5 hash berbeda dan diminta untuk memecahkannya. Tantangan di room ini bukan cuma menjalankan tool, tapi yang lebih penting: _bagaimana cara mengidentifikasi jenis hash tersebut_ sebelum mencoba memecahkannya.

Kita akan menggunakan kombinasi tool online (_CrackStation_, _Hashes.com_) dan mencoba tool offline (_Hashcat_) untuk melihat mana pendekatan yang paling efisien tergantung jenis hash-nya.

![Task 1 Questions](Documentation-assets/Crack-the-hash/Question-Task1.png)

---

## Hash Identification 101

Sebelum masuk ke pemecahan, kamu harus tahu dulu jenis hash apa yang sedang dihadapi. Hash cracking tidak akan berhasil kalau kamu memilih algoritma yang salah di tool cracker.

Ada beberapa cara untuk mengidentifikasi hash:
1.  **Dilihat dari panjang karakternya (Length).**
    - Panjang 32 karakter (Hex) = Biasanya MD5, MD4, atau NTLM.
    - Panjang 40 karakter (Hex) = Biasanya SHA-1.
    - Panjang 64 karakter (Hex) = Biasanya SHA-256.
2.  **Dilihat dari pola / prefix (Signature).**
    - Jika diawali dengan tanda dollar seperti `$2y$`, `$1$`, atau `$6$`, ini adalah format modular crypt format (MCF).
    - Contoh: `$2y$` adalah penanda khas untuk **bcrypt** (Blowfish).

> **for your information:** **Hashcat Examples Wiki** adalah halaman referensi resmi dari Hashcat yang sangat berguna untuk mencocokkan pola hash yang kamu temukan dengan mode (kode angka) yang harus dipakai di Hashcat.

---

## Task 1: Level 1 (Cracking The Hashes)

Dari 5 hash yang diberikan, 4 di antaranya adalah hash standar tanpa _salt_ (tambahan data random untuk mengacak hash). Hash tipe ini paling cepat dan mudah dipecahkan menggunakan database pre-computed online seperti **CrackStation**.

> **for your information:** **CrackStation** adalah layanan online yang menyimpan miliaran hash hasil crack dari berbagai wordlist (seperti Wikipedia dan daftar password bocor). Kalau hash targetmu "umum", CrackStation bisa menemukannya dalam hitungan detik.

### Hash 1: `48bb6e862e54f2a795ffc4e541caed4d`
- **Panjang:** 32 karakter (Hex)
- **Kemungkinan:** MD5
- **Metode:** Masukkan ke CrackStation.
- **Hasil:** Berhasil dikenali sebagai **md5**. Password: `[REDACTED]`

![Crack MD5](Documentation-assets/Crack-the-hash/Crack-Hash-MD5.png)

### Hash 2: `CBFDAC6008F9CAB4083784CBD1874F76618D2A97`
- **Panjang:** 40 karakter (Hex)
- **Kemungkinan:** SHA-1
- **Metode:** Masukkan ke CrackStation.
- **Hasil:** Berhasil dikenali sebagai **sha1**. Password: `[REDACTED]`

![Crack SHA-1](Documentation-assets/Crack-the-hash/Crack-Hash-SHA-1.png)

### Hash 3: `1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032`
- **Panjang:** 64 karakter (Hex)
- **Kemungkinan:** SHA-256
- **Metode:** Masukkan ke CrackStation.
- **Hasil:** Berhasil dikenali sebagai **sha256**. Password: `[REDACTED]`

![Crack SHA-256](Documentation-assets/Crack-the-hash/Crack-Hash-SHA-256.png)

### Hash 5: `279412f945939ba78ce0758d3fd83daa`
- **Panjang:** 32 karakter (Hex)
- **Kemungkinan:** MD4 atau MD5.
- **Metode:** Masukkan ke CrackStation.
- **Hasil:** Berhasil dikenali sebagai **md4**. Password: `[REDACTED]`

![Crack MD4](Documentation-assets/Crack-the-hash/Crack-Hash-MD4.png)

---

## Task 1: The Bcrypt Anomaly (Hash 4)

Hash ke-4 sangat berbeda formatnya dari yang lain:
`$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom`

### 1. Identifikasi Hash

Melihat ada prefix `$2y$`, kita bisa mencocokkannya menggunakan referensi Hashcat Examples List online. 

![View Hash Type](Documentation-assets/Crack-the-hash/View-Type-Hash.png)

Sesuai tabel Hashcat, pola `$2a$`, `$2b$`, `$2x$`, dan `$2y$` adalah **bcrypt** (Blowfish berbasis Unix). Mode Hashcat untuk bcrypt adalah `3200`.

Bcrypt dikenal sebagai algoritma yang sengaja didesain untuk menjadi sangat lambat (*key stretching*), membuatnya sangat tangguh terhadap serangan brute-force.

### 2. Approach 1: Offline Cracking (Hashcat)

Langkah pertama yang bisa dicoba adalah melakukan crack secara offline menggunakan laptop sendiri. Buat file text berisi hash tersebut.

```bash
touch bcrypt.txt
nano bcrypt.txt
# Paste hash ke dalam file lalu simpan
```

![Create Hash File](Documentation-assets/Crack-the-hash/Create-Hash-File.png)

Jalankan Hashcat dengan wordlist yang lebih kecil (`rockyou-75.txt`) agar komputasi tidak terlalu berat.

```bash
hashcat -m 3200 bcrypt.txt /home/dimm/SecLists/Passwords/Leaked-Databases/rockyou-75.txt
```

| Komponen | Fungsi |
| :--- | :--- |
| `hashcat` | Tool password recovery tingkat lanjut |
| `-m 3200` | Memilih jenis hash (3200 = bcrypt) |
| `bcrypt.txt` | File input yang berisi hash target |
| `/home/dimm/.../rockyou-75.txt` | File wordlist yang dipakai untuk menebak dictionary attack |

**Hasil:** Sayangnya proses cracking via Hashcat di laptop gagal.

![Hashcat Fail](Documentation-assets/Crack-the-hash/Crack-with-Hashcat-But-fail.png)

> **Common Mistake:** Di screenshot tertulis *Watchdog: Temperature abort trigger set to 90c*. Hashcat menguras penuh daya GPU/CPU, sehingga mesin laptop akan cepat panas. Bcrypt sangat berat; mencoba meng-crack bcrypt di laptop biasa (terutama dengan wordlist besar) sangat rawan membuat *overheat* (*thermal throttling*) sehingga Hashcat membatalkan proses dengan sendirinya demi keamanan hardware.

### 3. Approach 2: Online Hash Lookup (Hashes.com)

Karena komputasi offline gagal, alternatif berikutnya adalah mencari apakah hash bcrypt ini sudah pernah di-crack oleh orang lain sebelumnya di internet. Berbeda dengan *CrackStation* yang punya database lookup cepat, **Hashes.com** adalah platform yang sering menyimpan hash-hash yang rumit.

Copy hash tersebut dan paste ke fitur pemecah hash di Hashes.com.

![Answer Hash Bcrypt via Hashes.com](Documentation-assets/Crack-the-hash/Answer-Hash-Bcrypt.png)

**Hasil:** _Found!_

Ternyata hash tersebut sudah tersimpan di database Hashes.com dengan password: `[REDACTED]`.

> **for your information:** Memanfaatkan layanan lookup online seperti Hashes.com **seharusnya** selalu menjadi langkah pertama sebelum menyiksa komputermu sendiri dengan Hashcat. Kalau hash tersebut sudah ada di internet, kamu menghemat waktu berjam-jam (atau berhari-hari) komputasi.

---

## Review

- **Identifikasi adalah kunci.** Sebelum meng-crack, pahami bentuk hash-nya lewat panjang karakter (`MD5` = 32, `SHA1` = 40) atau prefix khas (`$2y$` = bcrypt).
- **CrackStation untuk *Quick Wins*.** Untuk hash standar dan unsalted, layanan seperti CrackStation adalah senjata tercepat.
- **Bcrypt itu mematikan.** Algoritma yang didesain secara spesifik agar sangat lambat di-crack. Mencoba brute-force bcrypt dengan wordlist besar di laptop biasa sangat tidak disarankan karena resiko overheat.
- **Work Smart, Not Hard.** Gunakan database pencarian canggih seperti Hashes.com sebagai alternatif pintar saat tool offline tidak sanggup mengangkat beban komputasinya.
