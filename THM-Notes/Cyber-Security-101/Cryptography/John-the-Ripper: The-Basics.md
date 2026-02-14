# TryHackMe: John the Ripper - The Basics

---

- **Room Link:** [John the Ripper Basics](https://tryhackme.com/room/johntheripperbasics)
- **Category:** Cryptography / Password Cracking
- **Difficulty:** Easy

---

## Task 1: Introduction

**Apa itu John the Ripper (JtR)?**
John the Ripper adalah salah satu tool "password cracker" paling populer dan fleksibel. Tool ini dirancang untuk mendeteksi password yang lemah (weak password) dengan cara melakukan proses cracking hash penggunanya.

- **Kecepatan Tinggi:** Mampu melakukan cracking dengan sangat cepat.
- **Fleksibilitas:** Mendukung ratusan format hash (bukan cuma password user, tapi juga file ZIP terenkripsi, kunci SSH, dll).
- **Open Source:** Gratis dan bisa dimodifikasi.

**Prasyarat:**
Untuk menggunakan JtR dengan efektif, perlu memahami dasar-dasar:

1.  **Command Line (Linux/Unix):** Navigasi folder, menjalankan command.
2.  **Hashing Basics:** Paham bedanya hashing vs enkripsi.

---

## Task 2: Basic Terms

Sebelum memulai cracking, kita harus paham dulu istilah-istilah dasarnya:

1.  **Hash:**
    - Hash adalah representasi data (seperti password) yang telah diubah menjadi string karakter dengan panjang tetap (fixed-length) menggunakan algoritma matematika tertentu (MD5, SHA1, SHA256, dll).
    - Sifat utama hash adalah **One-Way Function** (Satu Arah). Kita bisa ubah "password123" jadi hash, tapi SANGAT SULIT (secara komputasi mustahil) mengubah hash kembali jadi "password123" secara langsung (dekripsi).

2.  **Dictionary Attack:**
    - Karena hash tidak bisa didekripsi, cara paling umum untuk memecahkannya adalah dengan **menebak**.
    - Kita punya daftar kata-kata yang mungkin jadi password (**Wordlist** / Dictionary), misal: `rockyou.txt`.
    - JtR akan mengambil setiap kata dari wordlist -> mengubahnya jadi hash -> membandingkan dengan hash target. Jika cocok, berarti password ketemu!

3.  **Hash Identifier:**
    - Sebelum me-crack hash, kita HARUS tahu dulu jenis algoritmanya (apakah MD5? SHA256? NTLM?).
    - Tools seperti `hash-identifier` atau layanan online bisa membantu mengenali jenis hash dari format/panjangnya.

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

- **Lokasi:** `/usr/share/wordlists/`
- **Decompress:** Jika masih dalam bentuk `.gz`, perlu diekstrak dulu:
  ```bash
  sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
  ```

**Pertanyaan:**

- **Which website's breach was the rockyou.txt wordlist created from?**
  - Jawaban: `?`

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

- **Hash 1 (MD5):**
  - Identifikasi: Hash `1A1DC91C907325C69271DDF0C944BC72` terdeteksi sebagai **?**.
  - Command: `john --format=raw-md5 --wordlist=/home/dimm/wordlists/rockyou.txt hash1.txt`

- **Hash 2 (SHA1):**
  - Identifikasi: Hash terdeteksi sebagai **?**.
  - Command: `john --format=raw-sha1 --wordlist=/home/dimm/wordlists/rockyou.txt hash2.txt`

- **Hash 3 (SHA256):**
  - Identifikasi: Hash terdeteksi sebagai **?**.
  - Command: `john --format=raw-sha256 --wordlist=/home/dimm/wordlists/rockyou.txt hash3.txt`

- **Hash 4 (Whirlpool):**
  - Identifikasi: Hash terdeteksi sebagai **?**.
  - Command: `john --format=whirlpool --wordlist=/home/dimm/wordlists/rockyou.txt hash4.txt`

---

## Task 5: Cracking Windows Authentication

Task ini membahas cara crack hash otentikasi Windows, atau sering disebut **NTLM**.

**1. Apa itu NTLM?**
NTLM (New Technology LAN Manager) adalah protokol otentikasi lama yang digunakan oleh Windows. Meski sudah ada Kerberos, NTLM masih sering ditemukan.

**2. Identifikasi & Format:**

- Hash NTLM biasanya disimpan di file seperti `ntlm.txt`.
- Format John the Ripper untuk NTLM adalah `NT`.

**3. Cracking Process:**
Untuk latihan ini, kita diminta men-crack file `ntlm.txt`.

- **Command:**

  ```bash
  john --format=NT --wordlist=/home/dimm/wordlists/rockyou.txt ntlm.txt
  ```

  _(Ganti path wordlist sesuai lokasi rockyou.txt di komputer kamu)_

- **Hasil:**
  Setelah command jalan, password akan muncul di terminal.

---

## Task 6: Cracking /etc/shadow

Task ini mengajarkan cara crack password user di Linux dengan menggabungkan file `/etc/passwd` dan `/etc/shadow`.

**1. Konsep Dasar:**

- `/etc/passwd`: Berisi informasi user (bisa dibaca semua user).
- `/etc/shadow`: Berisi hash password (hanya bisa dibaca root).
- Untuk cracking, John butuh kedua file ini digabung agar formatnya dikenali.

**2. Langkah-langkah:**

- **Step 1: Unshadow**
  Kita harus menggabungkan kedua file tersebut menjadi satu file (misal `unshadowed.txt`) menggunakan command `unshadow`.

  ```bash
  unshadow [path_passwd] [path_shadow] > unshadowed.txt
  ```

  _(Untuk latihan ini, gunakan `local_passwd` dan `local_shadow` yang ada di folder hasil ekstrak tadi)_

- **Step 2: Cracking**
  Setelah jadi satu file, langsung crack pakai John (biasanya auto-detect format `sha512crypt` atau sejenisnya).

  ```bash
  john --wordlist=/home/dimm/wordlists/rockyou.txt unshadowed.txt
  ```

- **Step 3: Hasil**
  Password user yang berhasil dicrack akan muncul.

---

## Task 7: Single Crack Mode

Task ini membahas **Single Crack Mode**, fitur JtR untuk men-crack password yang mirip dengan username-nya.

**1. Konsep:**

- Berguna jika password user adalah variasi dari nama pengguna (misal: user `admin` punya password `Admin123`).
- Tidak butuh wordlist eksternal, JtR akan memanipulasi (mangle) username untuk menebak password.

**2. Langkah-langkah:**

- **Step 1: Siapkan File**
  Single Crack Mode butuh format `username:hash`.
  File `hash7.txt` isinya cuma hash, jadi kita harus tambah username di depannya.

  ```bash
  echo "Joker:7bf6d9bb82bed1302f331fc6b816aada" > hash7-siap.txt
  ```

- **Step 2: Cracking**
  Gunakan flag `--single`. Kita juga perlu kasih tahu format hash-nya (MD5).

  ```bash
  john --single --format=raw-md5 hash7-siap.txt
  ```

- **Step 3: Hasil**
  JtR akan mencoba variasi "Joker" (seperti "Joker1", "joker", dll) sampai ketemu passwordnya.

---

## Task 8: Custom Rules

Task ini mengajarkan cara membuat aturan (rules) sendiri untuk memodifikasi wordlist. Ini berguna kalau kita tahu pola password target (misal: "Selalu diawali huruf besar dan diakhiri angka").

**1. Lokasi Config:**
Rules didefinisikan di file konfigurasi John, biasanya di `/etc/john/john.conf`.

**2. Syntax Dasar:**

- `[0-9]`: Menambahkan angka 0-9.
- `A0`: Menyisipkan sesuatu di awal.
- `Az`: Menyisipkan sesuatu di akhir.
- `c`: Capitalize (huruf pertama jadi besar).

**3. Contoh Penggunaan:**
Misal kita mau bikin rule bernama `THMRules` yang menambahkan angka di belakang kata.

- Edit `john.conf`, tambahkan di bagian `[List.Rules:THMRules]`:
  ```
  [List.Rules:THMRules]
  Az"[0-9]"
  ```
- Jalankan John dengan flag `--rule`:
  ```bash
  john --wordlist=wordlist.txt --rule=THMRules hash.txt
  ```

---

## Task 9: Cracking ZIP Password

Task ini membahas cara crack file ZIP yang di-password. John tidak bisa baca file ZIP langsung, jadi butuh "perantara".

**1. Tool: zip2john**
Kita pakai tool bernama `zip2john` untuk mengubah file ZIP menjadi format hash yang bisa dibaca John.

**2. Langkah-langkah:**

- **Step 1: Convert to Hash**
  Gunakan `zip2john` dan simpan outputnya ke file teks (misal `zip_hash.txt`).

  ```bash
  zip2john [file_zip] > [output_hash]
  ```

  _Contoh:_

  ```bash
  zip2john secure.zip > zip_hash.txt
  ```

- **Step 2: Cracking**
  Crack file hash tersebut seperti biasa.

  ```bash
  john --wordlist=/home/dimm/wordlists/rockyou.txt zip_hash.txt
  ```

- **Step 3: Hasil**
  Password file ZIP akan muncul. Gunakan password itu untuk ekstrak file aslinya (`unzip secure.zip`).

---

## Task 10: Cracking RAR Archive

Mirip dengan ZIP, untuk file RAR kita butuh tool perantara bernama `rar2john`.

**1. Tool: rar2john**
Mengubah file RAR jadi format hash untuk John.

**2. Langkah-langkah:**

- **Step 1: Convert to Hash**
  Gunakan `rar2john` untuk ekstrak hash dari file RAR.

  ```bash
  rar2john [file_rar] > [output_hash]
  ```

  _Contoh:_

  ```bash
  rar2john secure.rar > rar_hash.txt
  ```

- **Step 2: Cracking**
  Crack file hash tersebut.

  ```bash
  john --wordlist=/home/dimm/wordlists/rockyou.txt rar_hash.txt
  ```

- **Step 3: Hasil**
  Password file RAR akan muncul. Gunakan untuk ekstrak (`unrar x secure.rar`).

---

## Task 11: Cracking SSH Keys

Task terakhir ini mengajarkan cara crack private key SSH (`id_rsa`) yang diproteksi passphrase.

**1. Tool: ssh2john**
Sama seperti ZIP dan RAR, kita butuh tool converter bernama `ssh2john`.

**2. Langkah-langkah:**

- **Step 1: Convert to Hash**
  Ubah file private key menjadi format hash John.

  ```bash
  ssh2john [id_rsa] > [output_hash]
  ```

  _Contoh:_

  ```bash
  ssh2john id_rsa > ssh_hash.txt
  ```

  _(Note: Jika command `ssh2john` tidak ditemukan, coba cari script python-nya pakai `locate ssh2john` atau `find / -name ssh2john_`)\*

  > [!TIP]
  > **Biar aku gak ribet ngetik path panjang, ku bikin alias aja di terminal ku:**
  > `echo "alias ssh2john='python ~/JohnTheRipper/run/ssh2john.py'" >> ~/.zshrc`

- **Step 2: Cracking**
  Crack file hash tersebut.

  ```bash
  john --wordlist=/home/dimm/wordlists/rockyou.txt ssh_hash.txt
  ```

- **Step 3: Hasil**
  Passphrase untuk key SSH tersebut akan muncul.
