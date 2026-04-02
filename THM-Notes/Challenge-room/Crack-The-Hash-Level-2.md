# TryHackMe: Crack the Hash Level 2

- **Room Link:** [Crack the Hash Level 2](https://tryhackme.com/room/introtohashcracking)
- **Category:** Challenge Room
- **Difficulty:** Medium
- **Tools Used:** Haiti, wordlistctl, Hashcat, John the Ripper
- **Main Techniques:** Hash Identification (CLI-based), Wordlist Management, Offline Hash Cracking

---

## Attack Context

- **Kapan teknik ini dipakai?** Tahap _Credential Access_ — ketika kamu sudah mendapatkan hash password dan perlu mengembalikan plaintext-nya. Room ini berfokus pada penguatan workflow: identifikasi hash secara akurat menggunakan tool CLI, lalu mengelola wordlist dengan efisien sebelum menjalankan proses cracking.
- **Syarat yang dibutuhkan:** Hash target sudah diketahui. Pemahaman dasar tentang hash cracking dari [Crack the Hash Level 1](Crack-the-hash.md) sangat disarankan karena room ini melanjutkan konsep yang sudah dibahas di sana.
- **Tanda keberhasilan:** Kamu bisa mengidentifikasi tipe hash secara akurat, menemukan wordlist yang tepat, dan berhasil memecahkan hash menggunakan Hashcat atau John the Ripper.

---

## Overview

Room ini adalah kelanjutan dari [Crack the Hash Level 1](Crack-the-hash.md). Kalau di Level 1 identifikasi hash masih dilakukan secara manual — mengandalkan panjang karakter dan prefix — di sini kamu akan diperkenalkan dengan tool CLI yang lebih akurat: **Haiti**.

> **for your information:** **Haiti** adalah tool command-line untuk mengidentifikasi tipe hash. Kelebihannya dibanding menebak manual: Haiti langsung menampilkan kode mode Hashcat (`HC`) dan format John the Ripper (`JtR`) untuk setiap kemungkinan algoritma, sehingga kamu tidak perlu bolak-balik ke Hashcat Examples Wiki.

Selain itu, room ini juga memperkenalkan **wordlistctl** — tool untuk mencari, mengunduh, dan mengelola arsip wordlist dari berbagai sumber.

> **for your information:** **wordlistctl** adalah script dari BlackArch yang bisa mengakses lebih dari 6300 wordlist dari berbagai repository online. Fungsinya mencari wordlist berdasarkan kata kunci, mengunduhnya, dan meng-extract arsip yang terkompresi — semua dari terminal.

---

## Task 1: Introduction

Room ini mencakup tiga area utama: identifikasi hash via Haiti, manajemen wordlist via wordlistctl, dan cracking hash menggunakan Hashcat dan John the Ripper. Tidak ada pertanyaan di task ini.

---

## Task 2: Hash Identification

Di Level 1, kamu mengidentifikasi hash berdasarkan panjang karakter dan prefix — pendekatan yang cukup untuk hash-hash populer seperti MD5, SHA-1, dan SHA-256. Tapi bagaimana kalau hash-nya 80 karakter dan formatnya tidak pernah kamu lihat sebelumnya? Haiti menangani kasus seperti ini dengan lebih akurat.

### Installing Haiti

Haiti diinstal melalui RubyGems:

```bash
gem install haiti-hash
```

| Komponen | Fungsi |
| :--- | :--- |
| `gem` | Package manager untuk bahasa Ruby |
| `install` | Sub-command untuk memasang package |
| `haiti-hash` | Nama package Haiti di registry RubyGems |

![Install Haiti](Documentation-assets/Crack-the-hash-lv2/Install-haiti.png)

### Identifying Hash 1: RIPEMD-320

Hash pertama yang diberikan:

```
741ebf5166b9ece4cca88a3868c44871e8370707cf19af3ceaa4a6fba006f224ae03f39153492853
```

Jalankan Haiti terhadap hash ini:

![Check RIPEMD-320 with Haiti](Documentation-assets/Crack-the-hash-lv2/Check-hash320-with-haiti.png)

Output Haiti menampilkan beberapa kemungkinan:

| Kandidat | JtR Format | Hashcat Code |
| :--- | :--- | :--- |
| **RIPEMD-320** | `dynamic_150` | — |
| IPMI 2.0 RAKP HMAC-SHA1 | — | `7300` |
| Umbraco HMAC-SHA1 | — | `24800` |
| WPA-EAPOL-PBKDF2 | — | `2500` |
| WPA-EAPOL-PMK | — | `2501` |

Kandidat teratas yang paling masuk akal untuk hash sepanjang 80 karakter hex adalah **RIPEMD-320** (_RACE Integrity Primitives Evaluation Message Digest_ - varian 320 bit). Kolom Hashcat Code untuk RIPEMD-320 kosong karena Hashcat tidak mendukung algoritma ini secara native — cracking-nya harus menggunakan John the Ripper dengan format `dynamic_150`. Kandidat lain seperti IPMI dan WPA-EAPOL biasanya punya format khusus dengan separator, bukan hex murni seperti ini.

**Jawaban:** `RIPEMD-320`

### Identifying Hash 2: Keccak-256

Hash kedua:

```
1aec7a56aa08b25b596057e1ccbcb6d768b770eaa0f355ccbd56aee5040e02ee
```

![Keccak-256 Hash Identification](Documentation-assets/Crack-the-hash-lv2/Kecak-hash256.png)

Outputnya panjang karena hash 64 karakter hex cocok dengan banyak algoritma 256-bit. Yang dicari room ini adalah **Keccak-256**:

| Informasi | Nilai |
| :--- | :--- |
| Hashcat Code | `17800` |
| John the Ripper Format | `raw-keccak-256` |

> **for your information:** **Keccak-256** adalah varian asli dari algoritma yang memenangkan kompetisi standarisasi SHA-3 oleh NIST. Versi finalnya (SHA3-256) dimodifikasi pada parameter padding, sehingga Keccak-256 dan SHA3-256 menghasilkan output berbeda untuk input yang sama, dan Hashcat memperlakukan keduanya sebagai mode yang berbeda.

Haiti langsung menyertakan kode mode Hashcat (`HC: 17800`) dan format JtR (`JtR: raw-keccak-256`) di outputnya — tidak perlu lagi membuka Hashcat Examples Wiki.

**Jawaban Hashcat code:** `17800`
**Jawaban JtR code:** `raw-keccak-256`

![Task 2 - Hash Identification Overview](Documentation-assets/Crack-the-hash-lv2/Task2-Hash-identification.png)

---

## Task 3: Wordlists

Punya tool cracking saja tidak cukup, kamu juga butuh kamus tebakan yang tepat. Room ini memperkenalkan tiga sumber utama wordlist dan satu tool pengelola:

- **SecLists** — Koleksi daftar yang dipakai selama security assessment: passwords, usernames, URLs, payloads, dan lainnya.
- **wordlistctl** — Script untuk mencari, mengunduh, dan mengelola arsip wordlist dari berbagai sumber online.
- **Rawsec's CyberSecurity Inventory** — Direktori tool dan resource keamanan siber. Kategori _Cracking_ di sana berguna untuk menemukan wordlist generator.

![Task 3 Instructions](Documentation-assets/Crack-the-hash-lv2/Task3-instruction.png)

### Searching for a Wordlist

**RockYou** adalah wordlist paling terkenal yang berisi jutaan password dari kebocoran data asli, diurutkan berdasarkan frekuensi penggunaan. Untuk mencari wordlist ini melalui wordlistctl:

```bash
wordlistctl search rockyou
```

| Komponen | Fungsi |
| :--- | :--- |
| `wordlistctl` | Tool pengelola wordlist dari BlackArch |
| `search` | Sub-command untuk mencari wordlist berdasarkan kata kunci |
| `rockyou` | Kata kunci pencarian |

![Search Rockyou](Documentation-assets/Crack-the-hash-lv2/Using-wordlistctl-find-rockyou.png)

Output menampilkan 18 varian rockyou dari repository BlackArch, lengkap dengan ukuran file masing-masing — mulai dari `rockyou-05` (104 bytes) hingga `rockyou` versi penuh (53.29 MB).

### Searching Local Archives

Secara default, `wordlistctl search` mencari ke repository remote (online). Untuk mencari wordlist yang sudah terinstal di mesin lokal, tambahkan flag `-l`:

```bash
wordlistctl search -l rockyou
```

| Komponen | Fungsi |
| :--- | :--- |
| `-l` / `--local` | Mencari di arsip lokal, bukan remote |

**Jawaban:** `-l`

### Fetching and Decompressing a Wordlist

Untuk mengunduh wordlist rockyou ke mesin lokal:

```bash
sudo wordlistctl fetch rockyou
```

![Install Rockyou](Documentation-assets/Crack-the-hash-lv2/install-rockyou-with-wordlistctl.png)

> **Common Mistake:** Gunakan `sudo` saat menjalankan `wordlistctl fetch` — tool ini menulis file ke direktori sistem `/usr/share/wordlists/` yang membutuhkan privilege root. Tanpa `sudo`, proses akan gagal dengan error _Permission denied_.

> **Common Mistake:** Saat proses fetch berjalan, kamu mungkin melihat error seperti `[-] Error while downloading rockyou-5.txt.gz: HTTPSConnectionPool ... Connection refused`. Ini terjadi karena salah satu mirror sumber sedang down atau tidak bisa dijangkau. Selama file utama `rockyou.txt` berhasil terunduh dengan status `[+] completed`, proses secara keseluruhan tetap berhasil — error pada mirror lain tidak menggagalkannya.

wordlistctl secara default menyimpan wordlist dalam format terkompresi (`.tar.gz`). Setelah fetch, lokasi file-nya:

```
/usr/share/wordlists/passwords/rockyou.txt.tar.gz
```

Untuk meng-extract arsip tersebut supaya bisa langsung dipakai oleh Hashcat:

```bash
wordlistctl fetch -l rockyou -d
```

| Komponen | Fungsi |
| :--- | :--- |
| `fetch` | Sub-command untuk mengunduh wordlist |
| `-l` | Memilih wordlist dari arsip lokal yang sudah di-fetch sebelumnya |
| `rockyou` | Nama wordlist yang akan di-fetch |
| `-d` | Decompress — extract arsip setelah download |

Setelah dekompresi, jalankan pencarian lokal lagi untuk memverifikasi path akhirnya:

```bash
wordlistctl search -l rockyou
```

![Rockyou Local](Documentation-assets/Crack-the-hash-lv2/Wordlist-rockyou-local.png)

**Jawaban path:** `/usr/share/wordlists/passwords/rockyou.txt`

### Help Menu Reference

Kalau kamu tidak yakin flag apa yang tersedia, cek help menu:

```bash
wordlistctl search -h
```

![Wordlistctl Help](Documentation-assets/Crack-the-hash-lv2/wordlistctl-local-archives-flag.png)

### Browsing Wordlists by Category

wordlistctl juga bisa menampilkan semua wordlist dalam kategori tertentu. Untuk melihat semua wordlist di kategori _usernames_:

```bash
wordlistctl list -g usernames
```

| Komponen | Fungsi |
| :--- | :--- |
| `list` | Sub-command untuk menampilkan daftar wordlist |
| `-g` | Filter berdasarkan group/kategori |
| `usernames` | Nama kategori yang ingin ditampilkan |

![Wordlistctl Usernames](Documentation-assets/Crack-the-hash-lv2/Wordlistctl-usernames-category.png)

Dari output di atas, wordlist pertama di daftar adalah **CommonAdminBase64**.

**Jawaban:** `CommonAdminBase64`

![Task 3 Questions](Documentation-assets/Crack-the-hash-lv2/Task3-questions.png)
