# TryHackMe: Hashing - Crypto 101

**Room Link:** [TryHackMe](https://tryhackme.com/room/hashingbasics)  
**Category:** Cryptography  
**Difficulty:** Easy  

---

## Task 1: Key Terms

**Konsep Dasar:**
Sebelum masuk ke hashing, penting untuk memahami beberapa istilah kunci:

*   **Plaintext:** Data dalam format aslinya yang belum dienkripsi atau di-hash (bisa berupa teks, gambar, dll).
*   **Encoding:** Metode representasi data (seperti Base64 atau Hex). Ini **BUKAN** enkripsi dan bisa dibalik dengan mudah.
*   **Hash:** Output berukuran tetap (digest) dari fungsi hash. "Hashing" adalah proses menghasilkan nilai hash tersebut.

**Q&A:**
*   **Is Base64 encryption or encoding?** `Encoding`

---

## Task 2: What is a hash function?

**Definisi:**
Fungsi hash adalah algoritma matematika yang mengambil input dengan ukuran berapapun dan menghasilkan output dengan ukuran tetap (fixed-size). Berbeda dengan enkripsi, hashing bersifat **satu arah** (tidak bisa dibalik untuk mendapatkan input asli). Tidak ada "kunci" dalam hashing standar.

**Karakteristik Utama:**
1.  **Fixed-size output:** Input sebesar apapun (1 kata atau 1 buku) akan menghasilkan hash dengan panjang yang sama.
    *   Contoh: MD5 selalu 128-bit (16 bytes), SHA-256 selalu 256-bit (32 bytes).
2.  **Sensitivity (Avalanche Effect):** Perubahan kecil pada input akan mengubah output hash secara drastis. Ini berguna untuk cek integritas data (file corrupted atau tidak).
3.  **Irreversible (One-way):** Secara komputasi tidak mungkin mengembalikan hash menjadi data aslinya.
4.  **Collision Resistance:** Sangat sulit (idealnya tidak mungkin) menemukan dua input berbeda yang menghasilkan hash yang sama.

**Output Encoding:**
Output asli hash adalah raw bytes, tapi biasanya ditampilkan dalam format:
*   **Hexadecimal:** Paling umum (contoh: output `md5sum`, `sha256sum`).
*   **Base64:** Kadang digunakan unuk menghemat tempat.

**Contoh Kasus (Q&A Room):**

1.  **SHA256 Hash of File:**
    *   Soal meminta hash dari file `passport.jpg` di folder `~/Hashing-Basics/Task-2`.
    *   Command: `sha256sum passport.jpg`
    *   Answer: `77148c6f605a8df855f2b764bcc3be749d7db814f5f79134d2aa539a64b61f02`

2.  **MD5 Output Size:**
    *   MD5 menghasilkan output 128-bit.
    *   Konversi ke bytes: 128 bits / 8 = **16 bytes**.
    *   (Note: Jika ditampilkan dalam Hex, panjangnya jadi 32 karakter karena 1 byte = 2 hex chars).

3.  **Possible Hash Values:**
    *   Jika output hash adalah **8-bit**, berapa kemungkinan nilainya?
    *   Rumus: `2^n` (dimana n adalah jumlah bit).
    *   Hitung: `2^8` = **256** kemungkinan nilai.
    *   Ini menunjukkan probabilitas collision; semakin kecil bit-nya, semakin besar kemungkinan collision (Pigeonhole Principle).
