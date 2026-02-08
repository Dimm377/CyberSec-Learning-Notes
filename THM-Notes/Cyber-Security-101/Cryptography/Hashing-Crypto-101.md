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
Fungsi hash adalah algoritma matematika yang mengambil input (berapapun ukurannya) dan menghasilkan output dengan ukuran tetap (fixed-size).

**Analogi Utama: Digital Fingerprint** (Sidik Jari Digital)
Sama seperti sidik jari manusia yang unik untuk setiap orang, hash adalah "sidik jari" unik untuk setiap file/data.
*   Jika dua file berbeda 1 bit saja, "sidik jari" (hash)nya akan berubah total.
*   Kita bisa mengenali file dari hash-nya tanpa perlu melihat isi filenya.

**Karakteristik Kunci:**
1.  **Fixed-size:** Input 1 huruf atau 100GB videonya, output hash-nya tetap sama panjangnya (misal MD5 = 32 karakter hex).
2.  **One-way (Irreversible):** Tidak bisa dibalik. Kamu tidak bisa mengambil hash dan mengubahnya kembali menjadi file asli. (Seperti membuat jus buah, tidak bisa dikembalikan jadi buah utuh).
3.  **Avalanche Effect:** Perubahan sangat kecil pada input menghasilkan perubahan drastis pada output hash.
4.  **No Key:** Berbeda dengan enkripsi, hashing tidak menggunakan kunci.

**Kegunaan:**
*   **Integrity Check:** Memastikan file tidak korup/diubah saat didownload.
*   **Password Storage:** Menyimpan password dalam bentuk hash, bukan teks asli.

**Contoh Kasus (Q&A Room):**
*   **SHA256 Hash `passport.jpg`:** Gunakan `sha256sum passport.jpg`.
    *   Result: `77148c6f605a8df855f2b764bcc3be749d7db814f5f79134d2aa539a64b61f02`
*   **MD5 Output Size:** 128 bit = 16 bytes.
*   **Possible values 8-bit:** 2^8 = 256.
