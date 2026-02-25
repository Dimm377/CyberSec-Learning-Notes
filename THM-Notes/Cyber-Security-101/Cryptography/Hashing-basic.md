# TryHackMe: Hashing Basic


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/hashingbasics)
- **Category:** Cryptography
- **Difficulty:** Easy

---

## Key Terms

**Konsep Dasar:**

- **Plaintext:** Data asli yang belum diubah (text, gambar, dll).
- **Encoding:** Ngubah format data (kayak Base64/Hex). **Bukan enkripsi**, bisa dibalik gampang banget.
- **Hash:** Output acak pake panjang tetap (digest) dari fungsi hashing.

**Q&A:**

- **Is Base64 encryption or encoding?** `Encoding`

---

## What is a hash function?

**Definisi:**
Fungsi hash beda sama enkripsi. **Gak ada kunci**, dan dimaksudkan biar **mustahil (atau secara komputasi gak praktis)** buat balikin output jadi input.

Fungsi hash ngambil input data berapa pun ukurannya dan bikin ringkasan (digest) dari data itu. Outputnya punya **ukuran tetap (fixed size)**.

- Gampang ngitung output dari input.
- Susah banget nentuin input dari output.
- Perubahan kecil di input (bahkan 1 bit) nyebabin perubahan drastis di output.

**Contoh Perubahan 1 Bit:**
Di terminal, kita bisa liat dua file `file1.txt` (isi "T") dan `file2.txt` (isi "U").

- Huruf **T** (Hex 54) = Binary `01010100`
- Huruf **U** (Hex 55) = Binary `01010101`
  Cuma beda **1 bit** terakhir, tapi hash-nya berubah total:
- MD5 File 1: `b9ece18c950afbfa6b0fdbfa4ff731d3`
- MD5 File 2: `4c614360da93c0a041b22e537de151eb`

**Output Encoding:**
Output asli hash itu raw bytes, yang kemudian di-encode (biasanya Base64 atau Hexadecimal).

- Contoh: `md5sum` ngasilin format Hexadecimal (setiap byte direpresentasiin pake 2 digit hex).

**Why is Hashing Important?**
Hashing bekerja di background buat ngelindungin integritas data & kerahasiaan password.

- **Password:** Server gak nyimpen password asli, tapi nyimpen **hash value**-nya. Waktu login, sistem ngitung hash password yang dimasukin dan ngebandinginnya sama hash yang tersimpan.

**Hash Collision:**
Collision terjadi waktu **dua input beda ngasilin output yang sama**.

- **Pigeonhole Effect (Efek Sarang Burung Merpati):** Karena jumlah input gak terbatas tapi jumlah output terbatas (fixed size), pasti ada input yang "berbagi" output yang sama.
- Contoh: Kalau hash cuma 4-bit (16 kemungkinan nilai) tapi ada 21 input, pasti ada collision.

**Insecure Algorithms:**

- **MD5 & SHA1:** Udah dianggep gak aman karena collision bisa direkayasa (engineered collision).
- Jangan pake keduanya buat password atau data sensitif.

**Q&A Room:**

- **SHA256 Hash `passport.jpg`:** `77148c6f605a8df855f2b764bcc3be749d7db814f5f79134d2aa539a64b61f02`
- **MD5 Output Size:** 128 bit = **16 bytes**.
- **Possible values 8-bit:** 2^8 = **256**.

---

## Insecure Password Storage for Authentication

**Risiko Penyimpanan Password:**

1.  **Plaintext:** Bahaya banget. Kalau database bocor, attacker langsung dapet password asli.
2.  **Encrypted:** Lebih baik dari plaintext, tapi kalau kunci enkripsi (key) dicuri, semua password bisa didekripsi.
3.  **Hashing Murahan (Tanpa Salt):**
    - **Duplicate Passwords:** Kalau dua user punya password sama (misal "password123"), hash-nya bakal sama.
    - **Rainbow Tables:** Attacker bisa pake tabel hash yang udah dihitung sebelumnya (rainbow table) buat mecahin hash super cepet.

---

## Using Hashing for Secure Password Storage

**Goal:** Ngamanin password dari serangan Rainbow Table dan Hash Collision.

**Solusi: Salting**
Salting itu proses nambahin string acak unik (**Salt**) ke password sebelum di-hash.

- **Proses:** `Hash(Password + Salt) -> Hash Value`
- **Penyimpanan:** Server nyimpen **Username**, **Salt**, dan **Hash Value**.

**Manfaat Salting:**

1.  **Nyegah Rainbow Tables:** Karena setiap password punya salt unik, hash akhirnya bakal beda-beda meskipun password aslinya sama. Attacker gak bisa pake tabel yang udah ada.
2.  **Mempersulit Brute Force:** Attacker harus mecahin hash satu per satu buat setiap user (karena salt-nya beda), gak bisa massal.

**Analogi:**
Bayangin dua orang masak "Sup Ayam" (Password sama).

- Tanpa Salt: Rasanya persis sama.
- Dengan Salt (Bumbu Rahasia): Orang A tambahin lada hitam, Orang B tambahin cabai. Hasil akhirnya (Hash) jadi sup pake rasa yang beda banget.

---

## Recognising Password Hashes

**Goal:** Identifikasi format hash password yang umum ditemuin di sistem Unix dan Windows.

### Unix/Linux Hash Formats

| Prefix                 | Algorithm     | Typical Length   | Example                              |
| ---------------------- | ------------- | ---------------- | ------------------------------------ |
| `$1$`                  | MD5‑crypt     | 34 characters    | `$1$abCDEFGH$K9VYpZ9Fh6K6G/6K7YbWc0` |
| `$2a$`, `$2b$`, `$2y$` | BCrypt        | 60 characters    | `$2y$12$eImiTXuWVxfM37uY4JANjQ==`    |
| `$5$`                  | SHA‑256‑crypt | 43‑44 characters | `$5$rounds=5000$salt$hash`           |
| `$6$`                  | SHA‑512‑crypt | 86‑87 characters | `$6$rounds=5000$salt$hash`           |

**Penjelasan:**

- Delimiter `$` misahin identifier, parameter opsional (kayak rounds, salt), dan hash-nya.
- `rounds` ngontrol biaya komputasi; makin tinggi rounds, makin tahan terhadap serangan brute-force.

### Windows Hash Formats

- **LM Hash** – Hash LAN Manager yang udah jadul, 16-character hexadecimal string (uppercase). Lemah banget karena mecah password jadi dua bagian 7-karakter dan pake DES. Contoh: `E52CAC67419A9A224A3B108F3FA6CB6D`.
- **NTLM Hash** – MD4 dari password yang di-encode UTF-16LE, 32-character hexadecimal string. Contoh: `31D6CFE0D16AE931B73C59D7E0C089C0` (hash dari password kosong).

### Common Tools & Pitfalls

- **hashid** – Identifier cepet buat banyak tipe hash, tapi bisa salah deteksi kalau hash-nya custom atau terpotong.
- **hashcat** – Tool cracking yang powerful; setiap format hash punya mode spesifik (misal mode 1000 buat NTLM, mode 1800 buat sha512crypt).
- **John the Ripper** – Tool cracking lain pake deteksi format bawaan.
- **Pitfall:** Ngandelin prefix doang bisa misleading; beberapa aplikasi nyimpen raw SHA-256 atau MD5 tanpa prefix.

### Example Questions (from the room)

1. Berapa default number of rounds buat hash `$6$` (sha512crypt)?
2. Identifikasi panjang Windows NTLM hash.
3. Hashcat mode apa yang dipake buat Citrix Netscaler hashes?

### Practical Exercise

- Pake online hash identifier (misal **hashes.com**) buat klasifikasi sample hash `$6$`.
- Capture Windows NTLM hash dari SAM dump dan coba dictionary attack pake `hashcat -m 1000`.

---

## Password Cracking

**Konsep:**
Hashing itu fungsi satu arah (one-way). Kita gak bisa "mendekripsi" hash.

- **Cracking = Guessing:** Attacker ngambil daftar kemungkinan password (wordlist), nge-hash satu per satu, terus ncocokin hasilnya sama hash target.

**Tools Populer:**

1.  **Hashcat:** Tool cracking tercepet (berbasis GPU).
    - Contoh Command (SHA256): `hashcat -m 1400 -a 0 hash.txt wordlist.txt`
2.  **John the Ripper (JtR):** Tool klasik, fleksibel banget (berbasis CPU/GPU).
    - Contoh Command: `john --format=raw-sha256 hash.txt --wordlist=rockyou.txt`

**Wordlists:**

- **rockyou.txt:** Wordlist paling legendaris, berisi 14 juta password yang bocor dari situs RockYou (2009). Hampir semua CTF pemula pake ini.

**Practical Exercise (Room):**

- **Hash:** `$2a$06$79...` (Identifikasi: `bcrypt`)
- **Hash:** `5e884...` (Identifikasi: `SHA-256`) -> `halloween`

---

## Hashing for Integrity Checking

**Goal:** Mastiin file asli dan gak dimodifikasi (tampered).

**Checksum:**
Nilai hash dari sebuah file disebut **Checksum**.

- Kalau download file `installer.exe` dari internet, website biasanya nyertain checksum (misal SHA256).
- Setelah download, user nge-hash file lokal. Kalau hash-nya sama persis, berarti file **aman & gak corrupt**.
- Beda 1 bit aja di file = Hash berubah total.

**HMAC (Keyed-Hash Message Authentication Code):**
Hashing standar cuma jamin **Integritas** (file gak berubah), tapi gak jamin **Autentikasi** (siapa pengirimnya).

- **HMAC = Hash + Secret Key.**
- Cuma orang yang punya "Key" yang bisa bikin HMAC yang valid.
- Jamin **Integrity** AND **Authenticity**.

**Example Question:**

- **HMAC-SHA512 Mode (Hashcat):** `1750`

---

## Conclusion

**Ringkasan Perbedaan Utama:**

| Fitur           | Hashing                  | Encoding                    | Encryption                    |
| :-------------- | :----------------------- | :-------------------------- | :---------------------------- |
| **Arah**        | Satu Arah (One-Way)      | Dua Arah (Reversible)       | Dua Arah (Reversible)         |
| **Tujuan**      | Integritas (Integrity)   | Format Data (Usability)     | Kerahasiaan (Confidentiality) |
| **Kunci (Key)** | Tidak Ada (kecuali HMAC) | Tidak Ada                   | Ada (Private/Public Key)      |
| **Output**      | Fixed Size (Digest)      | Variabel (tergantung input) | Variabel (tergantung input)   |

**Contoh Kasus:**

- **Hashing:** Verifikasi password di database, cek file corrupt.
- **Encoding:** Ngirim binary file lewat email (Base64), URL encoding.
- **Encryption:** Ngirim pesan rahasia (WA/Signal), HTTPS.

**Practical Exercise:**

- **Soal:** Decode string `RU5jb2RlREVjb2RlCg==`
- **Answer:** `?`
