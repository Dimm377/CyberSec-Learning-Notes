# TryHackMe: John the Ripper - The Basics

**Room Link:** [John the Ripper Basics](https://tryhackme.com/room/johntheripperbasics)  
**Category:** Cryptography / Password Cracking  
**Difficulty:** Easy  

---

## Task 1: Introduction

**Apa itu John the Ripper (JtR)?**  
John the Ripper adalah salah satu tool "password cracker" paling populer dan fleksibel. Tool ini dirancang untuk mendeteksi password yang lemah (weak password) dengan cara melakukan proses cracking hash penggunanya.
*   **Kecepatan Tinggi:** Mampu melakukan cracking dengan sangat cepat.
*   **Fleksibilitas:** Mendukung ratusan format hash (bukan cuma password user, tapi juga file ZIP terenkripsi, kunci SSH, dll).
*   **Open Source:** Gratis dan bisa dimodifikasi.

**Prasyarat:**
Untuk menggunakan JtR dengan efektif, kita perlu paham dasar-dasar:
1.  **Command Line (Linux/Unix):** Navigasi folder, menjalankan command.
2.  **Hashing Basics:** Paham bedanya hashing vs enkripsi.

---

## Task 2: Basic Terms

Sebelum mulai cracking, kita harus paham dulu istilah-istilah dasarnya:

1.  **Hash:**
    *   Hash adalah representasi data (seperti password) yang telah diubah menjadi string karakter dengan panjang tetap (fixed-length) menggunakan algoritma matematika tertentu (MD5, SHA1, SHA256, dll).
    *   Sifat utama hash adalah **One-Way Function** (Satu Arah). Kita bisa ubah "password123" jadi hash, tapi SANGAT SULIT (secara komputasi mustahil) mengubah hash kembali jadi "password123" secara langsung (dekripsi).

2.  **Dictionary Attack:**
    *   Karena hash tidak bisa didekripsi, cara paling umum untuk memecahkannya adalah dengan **menebak**.
    *   Kita punya daftar kata-kata yang mungkin jadi password (**Wordlist** / Dictionary), misal: `rockyou.txt`.
    *   JtR akan mengambil setiap kata dari wordlist -> mengubahnya jadi hash -> membandingkan dengan hash target. Jika cocok, berarti password ketemu!

3.  **Hash Identifier:**
    *   Sebelum me-crack hash, kita HARUS tahu dulu jenis algoritmanya (apakah MD5? SHA256? NTLM?).
    *   Tools seperti `hash-identifier` atau layanan online bisa membantu mengenali jenis hash dari format/panjangnya.
