# TryHackMe: Crack the Hash Level 2 — Writeup

- **Room Link:** [Crack the Hash Level 2](https://tryhackme.com/room/introtohashcracking)
- **Category:** Challenge Room
- **Difficulty:** Medium
- **Tools Used:** Haiti, wordlistctl, Hashcat, John the Ripper
- **Main Techniques:** Hash Identification (CLI-based), Wordlist Management, Offline Hash Cracking

---

## Attack Context

- **Kapan teknik ini dipakai?** Tahap _Credential Access_ — ketika kamu sudah mendapatkan hash password dan perlu mengembalikan plaintext-nya. Room ini berfokus pada penguatan workflow: identifikasi hash secara akurat menggunakan tool CLI, lalu mengelola wordlist dengan efisien sebelum menjalankan proses cracking.
- **Syarat yang dibutuhkan:** Hash target sudah diketahui. Pemahaman dasar tentang hash cracking dari Crack the Hash Level 1 sangat disarankan karena room ini melanjutkan konsep yang sudah dibahas di sana.
- **Tanda keberhasilan:** Kamu bisa mengidentifikasi tipe hash secara akurat, menemukan wordlist yang tepat, dan berhasil memecahkan hash menggunakan Hashcat atau John the Ripper.

---

## Overview

Room ini adalah kelanjutan dari Crack the Hash Level 1. Kalau di Level 1 identifikasi hash masih dilakukan secara manual — mengandalkan panjang karakter dan prefix — di sini kamu akan diperkenalkan dengan tool CLI yang lebih akurat: **Haiti**.

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

![Install-haiti.png]

### Identifying Hash 1: RIPEMD-320

Hash pertama yang diberikan:

```
741ebf5166b9ece4cca88a3868c44871e8370707cf19af3ceaa4a6fba006f224ae03f39153492853
```

Jalankan Haiti terhadap hash ini:

```bash
haiti 741ebf5166b9ece4cca88a3868c44871e8370707cf19af3ceaa4a6fba006f224ae03f39153492853
```

![Check-hash320-with-haiti.png]

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

```bash
haiti 1aec7a56aa08b25b596057e1ccbcb6d768b770eaa0f355ccbd56aee5040e02ee
```

![Kecak-hash256.png]

Outputnya panjang karena hash 64 karakter hex cocok dengan banyak algoritma 256-bit. Yang dicari room ini adalah **Keccak-256**:

| Informasi | Nilai |
| :--- | :--- |
| Hashcat Code | `17800` |
| John the Ripper Format | `raw-keccak-256` |

> **for your information:** **Keccak-256** adalah varian asli dari algoritma yang memenangkan kompetisi standarisasi SHA-3 oleh NIST. Versi finalnya (SHA3-256) dimodifikasi pada parameter padding, sehingga Keccak-256 dan SHA3-256 menghasilkan output berbeda untuk input yang sama — dan Hashcat memperlakukan keduanya sebagai mode yang berbeda.

Haiti langsung menyertakan kode mode Hashcat (`HC: 17800`) dan format JtR (`JtR: raw-keccak-256`) di outputnya — tidak perlu lagi membuka Hashcat Examples Wiki secara terpisah.

**Jawaban Hashcat code:** `17800`
**Jawaban JtR code:** `raw-keccak-256`

![Task2-Hash-identification.png]

---

## Task 3: Wordlists

Punya tool cracking saja tidak cukup — kamu juga butuh kamus tebakan yang tepat. Room ini memperkenalkan tiga sumber utama wordlist dan satu tool pengelola:

- **SecLists** — Koleksi daftar yang dipakai selama security assessment: passwords, usernames, URLs, payloads, dan lainnya.
- **wordlistctl** — Script untuk mencari, mengunduh, dan mengelola arsip wordlist dari berbagai sumber online.
- **Rawsec's CyberSecurity Inventory** — Direktori tool dan resource keamanan siber. Kategori _Cracking_ di sana berguna untuk menemukan wordlist generator.

![Task3-instruction.png]

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

![Using-wordlistctl-find-rockyou.png]

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

![install-rockyou-with-wordlistctl.png]

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

![Wordlist-rockyou-local.png]

**Jawaban path:** `/usr/share/wordlists/passwords/rockyou.txt`

### Help Menu Reference

Kalau kamu tidak yakin flag apa yang tersedia, cek help menu:

```bash
wordlistctl search -h
```

![wordlistctl-local-archives-flag.png]

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

![Wordlistctl-usernames-category.png]

Dari output di atas, wordlist pertama di daftar adalah **CommonAdminBase64**.

**Jawaban:** `CommonAdminBase64`

![Task3-questions.png]

---

## Task 4: Cracking Tools, Modes & Rules

Dua tool cracking yang paling umum dipakai di room ini:

- **Hashcat** — Cracker berbasis GPU, unggul dalam kecepatan raw computation untuk hash sederhana.
- **John the Ripper** (jumbo version) — Cracker berbasis CPU, lebih fleksibel dalam hal format hash yang didukung dan fitur rule mangling.

### Cracking Modes

Ada tiga mode operasi yang tersedia:

| Mode | Cara Kerja | Kapan Dipakai |
| :--- | :--- | :--- |
| **Wordlist** | Mencoba semua kata dari sebuah file dictionary | Password kemungkinan ada di daftar password umum |
| **Incremental** | Mencoba semua kemungkinan kombinasi karakter (brute force murni) | Tidak ada petunjuk tentang password — tapi sangat lambat untuk password panjang |
| **Rule** | Mengambil wordlist lalu mengubah setiap entri dengan pola tertentu (mutasi) | Password kemungkinan adalah variasi dari kata umum — misalnya `password` → `P@ssw0rd123` |

Di Level 1, untuk cracking offline kamu hanya menggunakan Wordlist mode. Rule mode jauh lebih powerful karena mengalikan jumlah kandidat tanpa perlu menyimpan wordlist raksasa — cukup simpan beberapa wordlist inti dan kombinasikan dengan rule yang berbeda-beda.

### Mutation Rules

Rule mode bekerja dengan menerapkan transformasi (mutasi) ke setiap entri wordlist. Ada dua cara menerapkannya: membuat custom wordlist yang sudah dimutasi terlebih dahulu, atau menyuruh tool cracking menerapkan mutasi secara on-the-fly saat proses berjalan. Room ini menggunakan pendekatan kedua — lebih efisien karena tidak perlu menyimpan file berukuran gigabytes yang hanya dipakai sekali.

Jenis-jenis mutasi yang umum:

| Mutation Type | Cara Kerja | Contoh |
| :--- | :--- | :--- |
| **Border** | Menambahkan digit/simbol di awal, akhir, atau keduanya | `password` → `password99` |
| **Freak** | Mengganti huruf dengan simbol yang mirip (leet speak) | `password` → `p@$$w0rd` |
| **Case** | Mengubah variasi uppercase/lowercase | `password` → `Password`, `PASSWORD` |
| **Order** | Membalik urutan karakter | `password` → `drowssap` |
| **Repetition** | Mengulang kelompok karakter | `password` → `passpass` |
| **Vowels** | Menghilangkan atau mengkapitalisasi vokal | `password` → `psswrd` |
| **Strip** | Menghapus satu atau beberapa karakter | `password` → `pasword` |
| **Swap** | Menukar posisi karakter | `password` → `apssword` |
| **Duplicate** | Menduplikasi karakter tertentu | `password` → `ppassword` |
| **Delimiter** | Menambahkan delimiter antar karakter | `password` → `p.a.s.s.w.o.r.d` |

### Creating a Custom Rule in John the Ripper

John menyimpan rule di file konfigurasi. Daripada mengedit file konfigurasi utama, buat file lokal terpisah di direktori yang sama — `/usr/share/john/john-local.conf`:

```bash
sudo nano /usr/share/john/john-local.conf
```

Tambahkan rule berikut:

```
[List.Rules:THM01]
$[0-9]$[0-9]
```

![Rule-john.png]

Bedah per komponen:

| Komponen | Fungsi |
| :--- | :--- |
| `[List.Rules:THM01]` | Nama rule — dipanggil nanti dengan `--rules=THM01` |
| `$[0-9]` | Menambahkan satu digit (0-9) di akhir kata |
| `$[0-9]$[0-9]` | Dua directive `$[0-9]` — menambahkan dua digit di akhir |

Rule ini menerapkan **border mutation**: setiap kata di wordlist akan menghasilkan 100 kandidat baru dengan kombinasi dua digit di belakangnya (00–99). Kata `moonligh` misalnya akan menghasilkan `moonligh00`, `moonligh01`, hingga `moonligh99`.

> **for your information:** Simbol `$` di sintaks rule John bukan karakter variabel shell — melainkan operator yang berarti "append character di akhir kata". `$[0-9]` berarti tambahkan satu karakter dari range 0–9. Ini adalah sintaks khusus John the Ripper, bukan Bash.

### Preparing the Hash File

Hash SHA-1 yang diberikan room:

```
2d5c517a4f7a14dcb38329d228a7d18a3b78ce83
```

Simpan ke file:

```bash
echo '2d5c517a4f7a14dcb38329d228a7d18a3b78ce83' > sha1-john.txt
```

![make-John-sha1-txt.png]

### Cracking with Rule-Based Attack

Jalankan John dengan wordlist `10k-most-common.txt` dari SecLists dan rule `THM01` yang baru saja dibuat:

```bash
john sha1-john.txt --format=raw-sha1 --wordlist=/usr/share/seclists/Passwords/Common-Credentials/10k-most-common.txt --rules=THM01
```

| Komponen | Fungsi |
| :--- | :--- |
| `john` | John the Ripper — tool cracking password |
| `sha1-john.txt` | File input berisi hash target |
| `--format=raw-sha1` | Menentukan tipe hash (SHA-1 tanpa salt) |
| `--wordlist=...` | Path ke wordlist yang digunakan |
| `--rules=THM01` | Menerapkan rule mutasi `THM01` yang sudah dibuat |

![Output-sha1-john-txt.png]

John berhasil memecahkan hash dalam waktu kurang dari 1 detik. Total kandidat yang diproses hanya 1 juta kombinasi (10.000 kata × 100 variasi dua digit) — kecil untuk ukuran GPU modern, tapi rule mode terbukti efektif menemukan password yang tidak akan pernah ada secara harfiah di wordlist manapun.

**Jawaban:** `moonligh56`

> **Common Mistake:** Path wordlist di command di atas menggunakan path SecLists di sistem room TryHackMe. Kalau SecLists kamu terinstal di lokasi yang berbeda (misalnya `/home/dimm/SecLists/...`), sesuaikan path-nya sebelum menjalankan command.

![task4-questions.png]

---

## Task 5: Custom Wordlist Generation

Rule mode di Task 4 menerapkan mutasi secara on-the-fly — efisien untuk wordlist yang hanya dipakai sekali. Tapi ada situasi di mana membuat custom wordlist terlebih dahulu lebih masuk akal:

- Wordlist akan dipakai berulang kali di sesi cracking berbeda — generate sekali lebih hemat dibanding menjalankan rule setiap kali.
- Kamu ingin menggunakan wordlist yang sama di beberapa tool yang berbeda.
- Tool target mendukung wordlist tapi tidak mendukung mangling rules.

Room memberikan hint bahwa password yang dicari berhubungan dengan anjing. Strateginya: unduh wordlist nama-nama ras anjing, terapkan mutasi menggunakan Mentalist, lalu crack hash-nya dengan wordlist hasil generate tersebut.

> **for your information:** **Mentalist** adalah tool berbasis GUI untuk membangun chain mutasi wordlist secara visual. Tersedia di [GitHub: sc0tfree/mentalist](https://github.com/sc0tfree/mentalist) — install sesuai instruksi di repository. Dibanding menulis sintaks rule John secara manual, Mentalist menyediakan antarmuka di mana kamu bisa menambahkan node transformasi (Case, Substitution, Append, dan lainnya) lalu langsung melihat estimasi jumlah output.

### Fetching a Themed Wordlist

Cari wordlist `dogs` melalui wordlistctl:

```bash
wordlistctl search dogs
```

Output menunjukkan satu hasil: `dogs` (2.35 KB). Langsung unduh dan extract:

```bash
sudo wordlistctl fetch dogs -d
```

| Komponen | Fungsi |
| :--- | :--- |
| `fetch` | Sub-command untuk mengunduh wordlist |
| `dogs` | Nama wordlist target |
| `-d` | Decompress — extract arsip setelah download |

File tersimpan di `/usr/share/wordlists/misc/dogs.txt` dengan total 242 entri nama ras anjing.

![wordlist-dogs-txt.png]

### Building Mutations with Mentalist

Buka Mentalist dan susun chain transformasi berikut:

| Node | Transformasi | Penjelasan |
| :--- | :--- | :--- |
| **1. Base Words** | File: `/usr/share/wordlists/misc/dogs.txt` | Memuat 242 nama ras anjing sebagai basis |
| **2. Case** | Toggle: 2nd | Mengubah karakter ke-2 menjadi uppercase — `molossus` → `mOlossus` |
| **3. Substitution** | Replace All: `s` → `$` (One at a time) | Mengganti semua huruf `s` dengan simbol `$` — `mOlossus` → `mOlo$$u$` |

![Create-custom-rules.png]

Mentalist menerapkan semua transformasi secara sequential ke setiap kata — bukan menghasilkan semua kemungkinan varian. Satu kata masuk, satu kata termutasi keluar. Itulah kenapa output-nya tetap 242 entri meskipun ada dua tahap transformasi.

Klik **Process** untuk menghasilkan wordlist. Output disimpan ke file yang kamu tentukan — dalam contoh ini `/home/dimm/wordlist-hashcat.txt`:

![save-rules.png]

### Preparing the Hash File

Hash MD5 yang diberikan room:

```
ed91365105bba79fdab20c376d83d752
```

Simpan ke file:

```bash
echo 'ed91365105bba79fdab20c376d83d752' > MD5-hash.txt
```

![Create-MD5-hash.png]

### Cracking with Hashcat

Jalankan Hashcat dengan mode MD5 dan custom wordlist yang baru dibuat:

```bash
hashcat -m 0 MD5-hash.txt wordlist-hashcat.txt
```

| Komponen | Fungsi |
| :--- | :--- |
| `hashcat` | Tool cracking password berbasis GPU |
| `-m 0` | Mode hash: MD5 |
| `MD5-hash.txt` | File berisi hash target |
| `wordlist-hashcat.txt` | Custom wordlist hasil generate dari Mentalist |

![Crack-With-hashcat-using-custom-rules.png]

![Cracked-MD5-custom.png]

Hashcat memecahkan hash dalam waktu kurang dari 1 detik. Output menunjukkan `Progress: 242/242 (100.00%)` — seluruh 242 kandidat dicoba dan satu cocok. Password aslinya adalah `mOlo$$u$`, yang berasal dari nama ras anjing **Molossus** setelah dua tahap mutasi: toggle karakter ke-2 (`molossus` → `mOlossus`) dan substitusi semua `s` → `$` (`mOlossus` → `mOlo$$u$`).

**Jawaban:** `mOlo$$u$`
