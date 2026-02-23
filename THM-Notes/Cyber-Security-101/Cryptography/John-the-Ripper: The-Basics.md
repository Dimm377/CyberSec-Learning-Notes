# TryHackMe: John the Ripper - The Basics

---

- **Room Link:** [John the Ripper Basics](https://tryhackme.com/room/johntheripperbasics)
- **Category:** Cryptography / Password Cracking
- **Difficulty:** Easy

---

## Task 1: Introduction

**Apa itu John the Ripper (JtR)?**
John the Ripper itu salah satu tool "password cracker" paling populer dan fleksibel. Tool ini dirancang buat mendeteksi password yang lemah (weak password) dengan cara lakuin proses cracking hash penggunanya.

- **Kecepatan Tinggi:** Mampu lakuin cracking super cepat.
- **Fleksibilitas:** Support ratusan format hash (bukan cuma password user, tapi juga file ZIP terenkripsi, kunci SSH, dll).
- **Open Source:** Gratis dan bisa dimodifikasi.

**Prasyarat:**
Buat pake JtR dengan efektif, perlu paham dasar-dasar:

1.  **Command Line (Linux/Unix):** Navigasi folder, jalanin command.
2.  **Hashing Basics:** Paham bedanya hashing vs enkripsi.

---

## Task 2: Basic Terms

Sebelum mulai cracking, kita harus paham dulu istilah-istilah dasarnya:

1.  **Hash:**
    - Hash itu representasi data (kayak password) yang udah diubah jadi string karakter pake panjang tetap (fixed-length) pake algoritma matematika tertentu (MD5, SHA1, SHA256, dll).
    - Sifat utama hash itu **One-Way Function** (Satu Arah). Kita bisa ubah "password123" jadi hash, tapi SANGAT SULIT (secara komputasi mustahil) buat balikin hash jadi "password123" secara langsung (dekripsi).

2.  **Dictionary Attack:**
    - Karena hash gak bisa didekripsi, cara paling umum buat mecahinnya itu pake **menebak**.
    - Kita punya daftar kata-kata yang mungkin jadi password (**Wordlist** / Dictionary), misal: `rockyou.txt`.
    - JtR bakal ngambil setiap kata dari wordlist -> ngubahnya jadi hash -> ngebandingin sama hash target. Kalau cocok, berarti password ketemu!

3.  **Hash Identifier:**
    - Sebelum nge-crack hash, kita HARUS tau dulu jenis algoritmanya (apakah MD5? SHA256? NTLM?).
    - Tools kayak `hash-identifier` atau layanan online bisa bantuin ngenali jenis hash dari format/panjangnya.

---

## Task 3: Setting Up Your System

Di task ini, kita siapin environment buat pake John the Ripper.

**1. Instalasi John the Ripper:**
Kalau belum terinstall (biasanya udah ada di Kali Linux/Parrot), bisa diinstall pake command:

```bash
sudo apt install john
```

**2. Wordlist RockYou:**
Wordlist yang paling umum dipake buat CTF dan belajar itu `rockyou.txt`.

- **Lokasi:** `/usr/share/wordlists/`
- **Decompress:** Kalau masih bentuk `.gz`, perlu diekstrak dulu:
  ```bash
  sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
  ```

**Pertanyaan:**

- **Which website's breach was the rockyou.txt wordlist created from?**
  - Jawaban: `?`

---

## Task 4: Wordlists & Regex

Di task ini, kita belajar cara pake wordlist dan identifikasi jenis hash.

**1. Identifikasi Hash:**
Sebelum cracking, kita harus tau jenis hash-nya. Bisa pake tool bawaan `hash-identifier` atau script `hash-id.py` yang disediain di soal.

```bash
hash-identifier
# Paste hash-nya
```

**2. Cracking Process:**
Pake command: `john --format=[format] --wordlist=[path_wordlist] [file_hash]`

Berikut jawaban buat latihan hash yang dikasih:

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

Task ini ngebahas cara crack hash otentikasi Windows, atau sering disebut **NTLM**.

**1. Apa itu NTLM?**
NTLM (New Technology LAN Manager) itu protokol otentikasi lama yang dipake Windows. Meski udah ada Kerberos, NTLM masih sering ditemuin.

**2. Identifikasi & Format:**

- Hash NTLM biasanya disimpen di file kayak `ntlm.txt`.
- Format John the Ripper buat NTLM itu `NT`.

**3. Cracking Process:**
Buat latihan ini, kita diminta nge-crack file `ntlm.txt`.

- **Command:**

  ```bash
  john --format=NT --wordlist=/home/dimm/wordlists/rockyou.txt ntlm.txt
  ```

  _(Ganti path wordlist sesuai lokasi rockyou.txt di komputer kamu)_

- **Hasil:**
  Setelah command jalan, password bakal muncul di terminal.

---

## Task 6: Cracking /etc/shadow

Task ini ngajarin cara crack password user di Linux pake gabungan file `/etc/passwd` dan `/etc/shadow`.

**1. Konsep Dasar:**

- `/etc/passwd`: Berisi informasi user (bisa dibaca semua user).
- `/etc/shadow`: Berisi hash password (cuma bisa dibaca root).
- Buat cracking, John butuh kedua file ini digabung biar formatnya dikenali.

**2. Langkah-langkah:**

- **Step 1: Unshadow**
  Kita harus gabungin kedua file itu jadi satu file (misal `unshadowed.txt`) pake command `unshadow`.

  ```bash
  unshadow [path_passwd] [path_shadow] > unshadowed.txt
  ```

  _(Buat latihan ini, pake `local_passwd` dan `local_shadow` yang ada di folder hasil ekstrak tadi)_

- **Step 2: Cracking**
  Setelah jadi satu file, langsung crack pake John (biasanya auto-detect format `sha512crypt` atau sejenisnya).

  ```bash
  john --wordlist=/home/dimm/wordlists/rockyou.txt unshadowed.txt
  ```

- **Step 3: Hasil**
  Password user yang berhasil dicrack bakal muncul.

---

## Task 7: Single Crack Mode

Task ini ngebahas **Single Crack Mode**, fitur JtR buat nge-crack password yang mirip sama username-nya.

**1. Konsep:**

- Berguna kalau password user itu variasi dari nama pengguna (misal: user `admin` punya password `Admin123`).
- Gak butuh wordlist eksternal, JtR bakal manipulasi (mangle) username buat nebak password.

**2. Langkah-langkah:**

- **Step 1: Siapin File**
  Single Crack Mode butuh format `username:hash`.
  File `hash7.txt` isinya cuma hash, jadi kita harus tambahin username di depannya.

  ```bash
  echo "Joker:7bf6d9bb82bed1302f331fc6b816aada" > hash7-siap.txt
  ```

- **Step 2: Cracking**
  Pake flag `--single`. Kita juga perlu kasih tau format hash-nya (MD5).

  ```bash
  john --single --format=raw-md5 hash7-siap.txt
  ```

- **Step 3: Hasil**
  JtR bakal nyoba variasi "Joker" (kayak "Joker1", "joker", dll) sampe ketemu passwordnya.

---

## Task 8: Custom Rules

Task ini ngajarin cara bikin aturan (rules) sendiri buat modifikasi wordlist. Ini berguna kalau kita tau pola password target (misal: "Selalu diawali huruf besar dan diakhiri angka").

**1. Lokasi Config:**
Rules didefinisiin di file konfigurasi John, biasanya di `/etc/john/john.conf`.

**2. Syntax Dasar:**

- `[0-9]`: Nambahin angka 0-9.
- `A0`: Nyisipin sesuatu di awal.
- `Az`: Nyisipin sesuatu di akhir.
- `c`: Capitalize (huruf pertama jadi besar).

**3. Contoh Penggunaan:**
Misal kita mau bikin rule bernama `THMRules` yang nambahin angka di belakang kata.

- Edit `john.conf`, tambahin di bagian `[List.Rules:THMRules]`:
  ```
  [List.Rules:THMRules]
  Az"[0-9]"
  ```
- Jalanin John pake flag `--rule`:
  ```bash
  john --wordlist=wordlist.txt --rule=THMRules hash.txt
  ```

---

## Task 9: Cracking ZIP Password

Task ini ngebahas cara crack file ZIP yang di-password. John gak bisa baca file ZIP langsung, jadi butuh "perantara".

**1. Tool: zip2john**
Kita pake tool bernama `zip2john` buat ngubah file ZIP jadi format hash yang bisa dibaca John.

**2. Langkah-langkah:**

- **Step 1: Convert to Hash**
  Pake `zip2john` dan simpen outputnya ke file teks (misal `zip_hash.txt`).

  ```bash
  zip2john [file_zip] > [output_hash]
  ```

  _Contoh:_

  ```bash
  zip2john secure.zip > zip_hash.txt
  ```

- **Step 2: Cracking**
  Crack file hash itu kayak biasa.

  ```bash
  john --wordlist=/home/dimm/wordlists/rockyou.txt zip_hash.txt
  ```

- **Step 3: Hasil**
  Password file ZIP bakal muncul. Pake password itu buat ekstrak file aslinya (`unzip secure.zip`).

---

## Task 10: Cracking RAR Archive

Mirip sama ZIP, buat file RAR kita butuh tool perantara bernama `rar2john`.

**1. Tool: rar2john**
Ngubah file RAR jadi format hash buat John.

**2. Langkah-langkah:**

- **Step 1: Convert to Hash**
  Pake `rar2john` buat ekstrak hash dari file RAR.

  ```bash
  rar2john [file_rar] > [output_hash]
  ```

  _Contoh:_

  ```bash
  rar2john secure.rar > rar_hash.txt
  ```

- **Step 2: Cracking**
  Crack file hash-nya.

  ```bash
  john --wordlist=/home/dimm/wordlists/rockyou.txt rar_hash.txt
  ```

- **Step 3: Hasil**
  Password file RAR bakal muncul. Pake buat ekstrak (`unrar x secure.rar`).

---

## Task 11: Cracking SSH Keys

Task terakhir ini ngajarin cara crack private key SSH (`id_rsa`) yang diproteksi passphrase.

**1. Tool: ssh2john**
Sama kayak ZIP dan RAR, kita butuh tool converter bernama `ssh2john`.

**2. Langkah-langkah:**

- **Step 1: Convert to Hash**
  Ubah file private key jadi format hash John.

  ```bash
  ssh2john [id_rsa] > [output_hash]
  ```

  _Contoh:_

  ```bash
  ssh2john id_rsa > ssh_hash.txt
  ```

  _(Note: Kalau command `ssh2john` gak ditemuin, coba cari script python-nya pake `locate ssh2john` atau `find / -name ssh2john_`)\*

  > [!TIP]
  > **Biar gak ribet ngetik path panjang, bikin alias aja di terminal:**
  > `echo "alias ssh2john='python ~/JohnTheRipper/run/ssh2john.py'" >> ~/.zshrc`

- **Step 2: Cracking**
  Crack file hash-nya.

  ```bash
  john --wordlist=/home/dimm/wordlists/rockyou.txt ssh_hash.txt
  ```

- **Step 3: Hasil**
  Passphrase buat key SSH itu bakal muncul.
