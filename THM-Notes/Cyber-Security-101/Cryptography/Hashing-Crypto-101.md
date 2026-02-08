# TryHackMe: Hashing - Crypto 101

**Room Link:** [TryHackMe](https://tryhackme.com/room/hashingbasics)  
**Category:** Cryptography  
**Difficulty:** Easy  

---

## Task 1: Key Terms

**Konsep Dasar:**
*   **Plaintext:** Data asli yang belum diubah (text, gambar, dll).
*   **Encoding:** Mengubah format data (seperti Base64/Hex). **Bukan enkripsi**, bisa dibalik dengan mudah.
*   **Hash:** Output acak dengan panjang tetap (digest) dari fungsi hashing.

**Q&A:**
*   **Is Base64 encryption or encoding?** `Encoding`

---

## Task 2: What is a hash function?

**Definisi:**
Fungsi hash berbeda dengan enkripsi. **Tidak ada kunci**, dan dimaksudkan agar **mustahil (atau secara komputasi tidak praktis)** untuk mengembalikan output menjadi input.

Fungsi hash mengambil input data dengan ukuran berapapun dan membuat ringkasan (digest) dari data tersebut. Outputnya memiliki **ukuran tetap (fixed size)**.
*   Mudah menghitung output dari input.
*   Sangat sulit menentukan input dari output.
*   Perubahan kecil pada input (bahkan 1 bit) menyebabkan perubahan drastis pada output.

**Contoh Perubahan 1 Bit:**
Di terminal, kita bisa melihat dua file `file1.txt` (isi "T") dan `file2.txt` (isi "U").
*   Huruf **T** (Hex 54) = Binary `01010100`
*   Huruf **U** (Hex 55) = Binary `01010101`
Hanya beda **1 bit** terakhir, tapi hash-nya berubah total:
*   MD5 File 1: `b9ece18c950afbfa6b0fdbfa4ff731d3`
*   MD5 File 2: `4c614360da93c0a041b22e537de151eb`

**Output Encoding:**
Output asli hash adalah raw bytes, yang kemudian di-encode (biasanya Base64 atau Hexadecimal).
*   Contoh: `md5sum` menghasilkan format Hexadecimal (setiap byte direpresentasikan dengan 2 digit hex).

**Why is Hashing Important?**
Hashing bekerja di latar belakang untuk melindungi integritas data & kerahasiaan password.
*   **Password:** Server tidak menyimpan password asli, tapi menyimpan **hash value**-nya. Saat login, sistem menghitung hash password yang dimasukkan dan membandingkannya dengan hash yang tersimpan.

**Hash Collision:**
Collision terjadi saat **dua input berbeda menghasilkan output yang sama**.
*   **Pigeonhole Effect (Efek Sarang Burung Merpati):** Karena jumlah input tidak terbatas tapi jumlah output terbatas (fixed size), pasti ada input yang "berbagi" output yang sama.
*   Contoh: Jika hash cuma 4-bit (16 kemungkinan nilai) tapi ada 21 input, pasti ada collision.

**Insecure Algorithms:**
*   **MD5 & SHA1:** Sudah dianggap tidak aman karena collision bisa direkayasa (engineered collision).
*   Jangan gunakan keduanya untuk password atau data sensitif.

**Q&A Room:**
*   **SHA256 Hash `passport.jpg`:** `77148c6f605a8df855f2b764bcc3be749d7db814f5f79134d2aa539a64b61f02`
*   **MD5 Output Size:** 128 bit = **16 bytes**.
*   **Possible values 8-bit:** 2^8 = **256**.

---

## Task 3: Insecure Password Storage & Hashing Uses

**Pentingnya Hashing untuk Password:**
Menyimpan password dalam bentuk **Plaintext** atau **Encrypted** (yang kuncinya bisa dicuri) sangat berbahaya. Jika database bocor, semua akun user bisa diambil alih.

**Solusi: Hashing**
*   Server hanya menyimpan **Hash** dari password.
*   Attacker yang mencuri database hanya dapat hash acak, bukan password asli.
*   Untuk login, server meng-hash password yang diinput user dan membandingkannya dengan hash di database.

**Kelemahan Hashing Polos (tanpa Salt):**
1.  **Duplicate Passwords:** Jika dua user punya password sama (misal "password123"), hash-nya juga akan sama. Attacker langsung tau mereka pakai password yang sama.
2.  **Rainbow Tables:** Tabel raksasa yang berisi jutaan password umum beserta hash-nya. Attacker tinggal "mencocokkan" hash yang dicuri dengan tabel ini untuk menemukan password asli dalam hitungan detik.

**Solusi Lanjutan: Salting**
Menambahkan string acak unik (**Salt**) ke setiap password sebelum di-hash.
*   User A: Hash("password123" + "SaltA")
*   User B: Hash("password123" + "SaltB")
Hasil hash akan berbeda meskipun password aslinya sama! Ini mematikan serangan Rainbow Table.
