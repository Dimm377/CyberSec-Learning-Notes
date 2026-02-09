# TryHackMe: Hashing Basic

<<<<<<< HEAD
**Room Link:** [Hashing Basic](https://tryhackme.com/room/hashingbasics)
**Category:** Cryptography
**Difficulty:** Easy
=======
**Room Link:** [Hashing Basic](https://tryhackme.com/room/hashingbasics)  
**Category:** Cryptography  
**Difficulty:** Easy  
>>>>>>> 33e2f66c071245c6584bbaee9f3246366b32fdf6

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

## Task 3: Insecure Password Storage & Hashing Uses

**Pentingnya Hashing untuk Password:**
Menyimpan password dalam bentuk **Plaintext** atau **Encrypted** (yang kuncinya bisa dicuri) sangat berbahaya. Jika database bocor, semua akun user bisa diambil alih.

**Solusi: Hashing**

- Server hanya menyimpan **Hash** dari password.
- Attacker yang mencuri database hanya dapat hash acak, bukan password asli.
- Untuk login, server meng-hash password yang diinput user dan membandingkannya dengan hash di database.

**Kelemahan Hashing Polos (tanpa Salt):**

1.  **Duplicate Passwords:** Jika dua user punya password sama (misal "password123"), hash-nya juga akan sama. Attacker langsung tau mereka pakai password yang sama.
2.  **Rainbow Tables:** Tabel raksasa yang berisi jutaan password umum beserta hash-nya. Attacker tinggal "mencocokkan" hash yang dicuri dengan tabel ini untuk menemukan password asli dalam hitungan detik.

**Solusi Lanjutan: Salting**
Menambahkan string acak unik (**Salt**) ke setiap password sebelum di-hash.

- User A: Hash("password123" + "SaltA")
- User B: Hash("password123" + "SaltB")
  Hasil hash akan berbeda meskipun password aslinya sama! Ini mematikan serangan Rainbow Table.

---

## Task 4: Recognising Password Hashes

**Goal:** Identify and differentiate common password hash formats found on Unix/Linux and Windows systems, understand their structure, and know how to work with them using typical tools.

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

**Why Salting Matters:** Both LM and NTLM hashes are unsalted, making them vulnerable to pre‑computed rainbow tables. Adding a unique salt before hashing (e.g., using bcrypt or scrypt) mitigates this risk.

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
- Compare the cracked result against a rainbow table to see the impact of salting.

> **Note:** Always add a unique **salt** to passwords before hashing to prevent rainbow‑table attacks.

---
