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
Fungsi hash adalah algoritma matematika yang mengambil input dengan ukuran berapapun dan menghasilkan output dengan ukuran tetap (fixed-size). Berbeda dengan enkripsi, hashing bersifat **satu arah** (tidak bisa dibalik untuk mendapatkan input asli).

**Karakteristik Utama:**
1.  **Fixed-size output:** Input sebesar apapun (1 kata atau 1 buku) akan menghasilkan hash dengan panjang yang sama.
2.  **Sensitivity:** Perubahan kecil pada input akan mengubah output hash secara drastis (Avalanche Effect).
3.  **Irreversible:** Secara komputasi tidak mungkin mengembalikan hash menjadi data aslinya.

**Hash Collisions:**
Terjadi ketika dua input berbeda menghasilkan hash yang sama. Algoritma lama seperti MD5 dan SHA-1 sudah dianggap tidak aman karena kerentanan terhadap collision.

**Q&A:**
*   **What is the SHA256 hash of the `passport.jpg` file?**
    *   Command: `sha256sum passport.jpg`
    *   Answer: `77148c6f605a8df855f2b764bcc3be749d7db814f5f79134d2aa539a64b61f02`
*   **What is the output size in bytes of the MD5 hash function?**
    *   MD5 output is 128 bits.
    *   1 byte = 8 bits.
    *   128 / 8 = `16` bytes.
*   **If you have an 8-bit hash output, how many possible hash values are there?**
    *   2^8 = `256`
