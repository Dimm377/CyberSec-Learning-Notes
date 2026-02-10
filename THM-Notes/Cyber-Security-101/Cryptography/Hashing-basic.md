# TryHackMe: Hashing Basic


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/hashingbasics)
- **Category:** Cryptography
- **Difficulty:** Easy

---

## Task 1: Key Terms

**Konsep Dasar:**

- **Plaintext:** Data asli yang belum diubah (text, gambar, dll).
- **Encoding:** Mengubah format data (seperti Base64/Hex). **Bukan enkripsi**, bisa dibalik dengan mudah.
- **Hash:** Output acak dengan panjang tetap (digest) dari fungsi hashing.

**Q&A:**

- **Is Base64 encryption or encoding?** `Encoding`

---

## Task 2: What is a hash function?

**Definisi:**
Fungsi hash berbeda dengan enkripsi. **Tidak ada kunci**, dan dimaksudkan agar **mustahil (atau secara komputasi tidak praktis)** untuk mengembalikan output menjadi input.

Fungsi hash mengambil input data dengan ukuran berapapun dan membuat ringkasan (digest) dari data tersebut. Outputnya memiliki **ukuran tetap (fixed size)**.

- Mudah menghitung output dari input.
- Sangat sulit menentukan input dari output.
- Perubahan kecil pada input (bahkan 1 bit) menyebabkan perubahan drastis pada output.

**Contoh Perubahan 1 Bit:**
Di terminal, kita bisa melihat dua file `file1.txt` (isi "T") dan `file2.txt` (isi "U").

- Huruf **T** (Hex 54) = Binary `01010100`
- Huruf **U** (Hex 55) = Binary `01010101`
  Hanya beda **1 bit** terakhir, tapi hash-nya berubah total:
- MD5 File 1: `b9ece18c950afbfa6b0fdbfa4ff731d3`
- MD5 File 2: `4c614360da93c0a041b22e537de151eb`

**Output Encoding:**
Output asli hash adalah raw bytes, yang kemudian di-encode (biasanya Base64 atau Hexadecimal).

- Contoh: `md5sum` menghasilkan format Hexadecimal (setiap byte direpresentasikan dengan 2 digit hex).

**Why is Hashing Important?**
Hashing bekerja di latar belakang untuk melindungi integritas data & kerahasiaan password.

- **Password:** Server tidak menyimpan password asli, tapi menyimpan **hash value**-nya. Saat login, sistem menghitung hash password yang dimasukkan dan membandingkannya dengan hash yang tersimpan.

**Hash Collision:**
Collision terjadi saat **dua input berbeda menghasilkan output yang sama**.

- **Pigeonhole Effect (Efek Sarang Burung Merpati):** Karena jumlah input tidak terbatas tapi jumlah output terbatas (fixed size), pasti ada input yang "berbagi" output yang sama.
- Contoh: Jika hash cuma 4-bit (16 kemungkinan nilai) tapi ada 21 input, pasti ada collision.

**Insecure Algorithms:**

- **MD5 & SHA1:** Sudah dianggap tidak aman karena collision bisa direkayasa (engineered collision).
- Jangan gunakan keduanya untuk password atau data sensitif.

**Q&A Room:**

- **SHA256 Hash `passport.jpg`:** `77148c6f605a8df855f2b764bcc3be749d7db814f5f79134d2aa539a64b61f02`
- **MD5 Output Size:** 128 bit = **16 bytes**.
- **Possible values 8-bit:** 2^8 = **256**.

---

## Task 3: Insecure Password Storage for Authentication

**Risiko Penyimpanan Password:**

1.  **Plaintext:** Sangat berbahaya. Jika database bocor, attacker langsung dapat password asli.
2.  **Encrypted:** Lebih baik dari plaintext, tapi jika kunci enkripsi (key) dicuri, semua password bisa didekripsi.
3.  **Hashing Murahan (Tanpa Salt):**
    - **Duplicate Passwords:** Jika dua user punya password sama (misal "password123"), hash-nya akan sama.
    - **Rainbow Tables:** Attacker bisa menggunakan tabel hash yang sudah dihitung sebelumnya (rainbow table) untuk memecahkan hash dengan sangat cepat.

---

## Task 4: Using Hashing for Secure Password Storage

**Goal:** Mengamankan password dari serangan Rainbow Table dan Hash Collision.

**Solusi: Salting**
Salting adalah proses menambahkan string acak unik (**Salt**) ke password sebelum di-hash.

- **Proses:** `Hash(Password + Salt) -> Hash Value`
- **Penyimpanan:** Server menyimpan **Username**, **Salt**, dan **Hash Value**.

**Manfaat Salting:**

1.  **Mencegah Rainbow Tables:** Karena setiap password punya salt unik, hash akhirnya akan berbeda-beda meskipun password aslinya sama. Attacker tidak bisa pakai tabel yang sudah ada.
2.  **Mempersulit Brute Force:** Attacker harus memecahkan hash satu per satu untuk setiap user (karena salt-nya beda), tidak bisa massal.

**Analogi:**
Bayangkan dua orang memasak "Sup Ayam" (Password sama).

- Tanpa Salt: Rasanya persis sama.
- Dengan Salt (Bumbu Rahasia): Orang A tambah lada hitam, Orang B tambah cabai. Hasil akhirnya (Hash) jadi sup dengan rasa yang sangat berbeda.

---

## Task 5: Recognising Password Hashes

**Goal:** Identify different password hash formats commonly found in Unix and Windows systems.

### Unix/Linux Hash Formats

| Prefix                 | Algorithm     | Typical Length   | Example                              |
| ---------------------- | ------------- | ---------------- | ------------------------------------ |
| `$1$`                  | MD5‑crypt     | 34 characters    | `$1$abCDEFGH$K9VYpZ9Fh6K6G/6K7YbWc0` |
| `$2a$`, `$2b$`, `$2y$` | BCrypt        | 60 characters    | `$2y$12$eImiTXuWVxfM37uY4JANjQ==`    |
| `$5$`                  | SHA‑256‑crypt | 43‑44 characters | `$5$rounds=5000$salt$hash`           |
| `$6$`                  | SHA‑512‑crypt | 86‑87 characters | `$6$rounds=5000$salt$hash`           |

**Explanation:**

- The `$` delimiter separates the identifier, optional parameters (e.g., rounds, salt), and the actual hash.
- `rounds` controls the computational cost; higher rounds increase resistance to brute‑force attacks.

### Windows Hash Formats

- **LM Hash** – Legacy LAN Manager hash, 16‑character hexadecimal string (uppercase). Very weak because it splits the password into two 7‑character halves and uses DES. Example: `E52CAC67419A9A224A3B108F3FA6CB6D`.
- **NTLM Hash** – MD4 of the UTF‑16LE encoded password, 32‑character hexadecimal string. Example: `31D6CFE0D16AE931B73C59D7E0C089C0` (hash of an empty password).

### Common Tools & Pitfalls

- **hashid** – Quick identifier for many hash types, but may mis‑detect custom or truncated hashes.
- **hashcat** – Powerful password‑cracking tool; each hash format has a specific mode (e.g., mode 1000 for NTLM, mode 1800 for sha512crypt).
- **John the Ripper** – Another cracking tool with built‑in format detection.
- **Pitfall:** Relying solely on the prefix can be misleading; some applications store raw SHA‑256 or MD5 without a prefix.

### Example Questions (from the room)

1. What is the default number of rounds for `$6$` (sha512crypt) hashes?
2. Identify the length of a Windows NTLM hash.
3. Which hashcat mode is used for Citrix Netscaler hashes?

### Practical Exercise

- Use an online hash identifier (e.g., **hashes.com**) to classify a sample `$6$` hash.
- Capture a Windows NTLM hash from a SAM dump and attempt a dictionary attack with `hashcat -m 1000`.

---

## Task 6: Password Cracking

**Konsep:**
Hashing adalah fungsi satu arah (one-way). Kita tidak bisa "mendekripsi" hash.

- **Cracking = Guessing:** Attacker mengambil daftar kemungkinan password (wordlist), meng-hash satu per satu, lalu mencocokkan hasilnya dengan hash target.

**Tools Populer:**

1.  **Hashcat:** Tool cracking tercepat (berbasis GPU).
    - Contoh Command (SHA256): `hashcat -m 1400 -a 0 hash.txt wordlist.txt`
2.  **John the Ripper (JtR):** Tool klasik, sangat fleksibel (berbasis CPU/GPU).
    - Contoh Command: `john --format=raw-sha256 hash.txt --wordlist=rockyou.txt`

**Wordlists:**

- **rockyou.txt:** Wordlist paling legendaris, berisi 14 juta password yang bocor dari situs RockYou (2009). Hampir semua CTF pemula pakai ini.

**Practical Exercise (Room):**

- **Hash:** `$2a$06$79...` (Identifikasi: `bcrypt`)
- **Hash:** `5e884...` (Identifikasi: `SHA-256`) -> `halloween`

---

## Task 7: Hashing for Integrity Checking

**Goal:** Memastikan file asli dan tidak dimodifikasi (tampered).

**Checksum:**
Nilai hash dari sebuah file disebut **Checksum**.

- Jika download file `installer.exe` dari internet, website biasanya menyertakan checksum (misal SHA256).
- Setelah download, user meng-hash file lokal. Jika hash-nya sama persis, berarti file **aman & tidak corrupt**.
- Beda 1 bit saja di file = Hash berubah total.

**HMAC (Keyed-Hash Message Authentication Code):**
Hashing standar hanya menjamin **Integritas** (file tidak berubah), tapi tidak menjamin **Autentikasi** (siapa pengirimnya).

- **HMAC = Hash + Secret Key.**
- Hanya orang yang punya "Key" yang bisa membuat HMAC yang valid.
- Menjamin **Integrity** AND **Authenticity**.

**Example Question:**

- **HMAC-SHA512 Mode (Hashcat):** `1750`

---

## Task 8: Conclusion

**Ringkasan Perbedaan Utama:**

| Fitur           | Hashing                  | Encoding                    | Encryption                    |
| :-------------- | :----------------------- | :-------------------------- | :---------------------------- |
| **Arah**        | Satu Arah (One-Way)      | Dua Arah (Reversible)       | Dua Arah (Reversible)         |
| **Tujuan**      | Integritas (Integrity)   | Format Data (Usability)     | Kerahasiaan (Confidentiality) |
| **Kunci (Key)** | Tidak Ada (kecuali HMAC) | Tidak Ada                   | Ada (Private/Public Key)      |
| **Output**      | Fixed Size (Digest)      | Variabel (tergantung input) | Variabel (tergantung input)   |

**Contoh Kasus:**

- **Hashing:** Verifikasi password di database, cek file corrupt.
- **Encoding:** Mengirim binary file lewat email (Base64), URL encoding.
- **Encryption:** Mengirim pesan rahasia (WA/Signal), HTTPS.

**Practical Exercise:**

- **Soal:** Decode string `RU5jb2RlREVjb2RlCg==`
- **Answer:** `?`
