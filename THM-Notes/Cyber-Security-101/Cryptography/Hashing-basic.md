# TryHackMe: Hashing Basic


---

* **Room Link:** [TryHackMe](https://tryhackme.com/room/hashingbasics)
* **Category:** Cryptography
* **Difficulty:** Easy

---

## Key Terms

(Catatan ini adalah bagian dari seri Cryptography. Untuk fondasi kriptografi, cek [Cryptography Basic](file:///home/dimm/CyberSec-Learning-Notes/THM-Notes/Cyber-Security-101/Cryptography/Cryptography-Basic.md). Cara meng-crack hash yang kamu temukan ada di [John the Ripper](file:///home/dimm/CyberSec-Learning-Notes/THM-Notes/Cyber-Security-101/Cryptography/John-the-Ripper: The-Basics.md))

**Konsep Dasar:**

* **Plaintext:** Data asli yang belum diubah (text, gambar, dll).
* **Encoding:** Mengubah format data (seperti Base64/Hex). **Bukan enkripsi**, bisa dibalik gampang sekali.
* **Hash:** Output acak pakai panjang tetap (digest) dari fungsi hashing.

**Q&A:**

* **Is Base64 encryption or encoding?** `Encoding`

---

## What is a hash function?

**Definisi:**
Fungsi hash beda sama enkripsi. **Tidak ada kunci**, dan dimaksudkan agar **mustahil (atau secara komputasi tidak praktis)** buat balikin output jadi input.

Fungsi hash mengambil input data berapa pun ukurannya dan membuat ringkasan (digest) dari data itu. Outputnya punya **ukuran tetap (fixed size)**.

* Mudah menghitung output dari input.
* Susah sekali menentukan input dari output.
* Perubahan kecil di input (bahkan 1 bit) menyebabkan perubahan drastis di output.

**Contoh Perubahan 1 Bit:**
Di terminal, kita bisa melihat dua file `file1.txt` (isi "T") dan `file2.txt` (isi "U").

* Huruf **T** (Hex 54) = Binary `01010100`
* Huruf **U** (Hex 55) = Binary `01010101`
  Cuma beda **1 bit** terakhir, tapi hash-nya berubah total:
* MD5 File 1: `b9ece18c950afbfa6b0fdbfa4ff731d3`
* MD5 File 2: `4c614360da93c0a041b22e537de151eb`

**Output Encoding:**
Output asli hash itu raw bytes, yang kemudian di-encode (biasanya Base64 atau Hexadecimal).

* Contoh: `md5sum` menghasilkan format Hexadecimal (setiap byte direpresentasiin pakai 2 digit hex).

**Why is Hashing Important?**
Hashing bekerja di background buat melindungi integritas data & kerahasiaan password.

* **Password:** Server tidak menyimpan password asli, tapi menyimpan **hash value**-nya. Waktu login, sistem menghitung hash password yang dimasukkan dan membandingkannya dengan hash yang tersimpan.

**Hash Collision:**
Collision terjadi waktu **dua input beda menghasilkan output yang sama**.

* **Pigeonhole Effect (Efek Sarang Burung Merpati):** Karena jumlah input tidak terbatas tapi jumlah output terbatas (fixed size), pasti ada input yang "berbagi" output yang sama.
* Contoh: Kalau hash cuma 4-bit (16 kemungkinan nilai) tapi ada 21 input, pasti ada collision.

**Insecure Algorithms:**

* **MD5 & SHA1:** Sudah dianggap tidak aman karena collision bisa direkayasa (engineered collision).
* Jangan pakai keduanya buat password atau data sensitif.

---

## Insecure Password Storage for Authentication

**Risiko Penyimpanan Password:**

1.  **Plaintext:** Bahaya sekali. Kalau database bocor, attacker langsung dapat password asli.
2.  **Encrypted:** Lebih baik dari plaintext, tapi kalau kunci enkripsi (key) dicuri, semua password bisa didekripsi.
3.  **Hashing Murahan (Tanpa Salt):**
* **Duplicate Passwords:** Kalau dua user punya password sama (misal "password123"), hash-nya akan sama.
* **Rainbow Tables:** Attacker bisa pakai tabel hash yang sudah dihitung sebelumnya (rainbow table) buat memecahkan hash super cepat.

---

## Using Hashing for Secure Password Storage

**Goal:** Mengamankan password dari serangan Rainbow Table dan Hash Collision.

**Solusi: Salting**
Salting itu proses menambahkan string acak unik (**Salt**) ke password sebelum di hash.

* **Proses:** `Hash(Password + Salt) : Hash Value`
* **Penyimpanan:** Server menyimpan **Username**, **Salt**, dan **Hash Value**.

**Manfaat Salting:**

1.  **Mencegah Rainbow Tables:** Karena setiap password punya salt unik, hash akhirnya akan beda-beda meskipun password aslinya sama. Attacker tidak bisa pakai tabel yang sudah ada.
2.  **Mempersulit Brute Force:** Attacker harus memecahkan hash satu per satu buat setiap user (karena salt-nya beda), tidak bisa massal.

bayangkan dua orang sedang memasak Sup Ayam (Password sama).

* Tanpa Salt: Rasanya persis sama.
* Dengan Salt (Bumbu Rahasia): Orang A tambahin lada hitam, Orang B tambahin cabai. Hasil akhirnya (Hash) jadi sup pakai rasa yang beda sekali.

---

## Recognising Password Hashes

**Goal:** Identifikasi format hash password yang umum ditemukan pada sistem Unix dan Windows.

### Unix/Linux Hash Formats

| Prefix                 | Algorithm     | Typical Length   | Example                              |
| ---------------------- | ------------- | ---------------- | ------------------------------------ |
| `$1$`                  | MD5‑crypt     | 34 characters    | `$1$abCDEFGH$K9VYpZ9Fh6K6G/6K7YbWc0` |
| `$2a$`, `$2b$`, `$2y$` | BCrypt        | 60 characters    | `$2y$12$eImiTXuWVxfM37uY4JANjQ==`    |
| `$5$`                  | SHA‑256‑crypt | 43‑44 characters | `$5$rounds=5000$salt$hash`           |
| `$6$`                  | SHA‑512‑crypt | 86‑87 characters | `$6$rounds=5000$salt$hash`           |

**Penjelasan:**

* Delimiter `$` memisahkan identifier, parameter opsional (seperti rounds, salt), dan hash nya.
* `rounds` mengontrol biaya komputasi; makin tinggi rounds, makin tahan terhadap serangan brute force.

### Windows Hash Formats

* **LM Hash** – Hash LAN Manager yang sudah kuno, 16-character hexadecimal string (uppercase). Sangat lemah karena memecah password jadi dua bagian 7-karakter dan pakai DES. Contoh: `E52CAC67419A9A224A3B108F3FA6CB6D`.
* **NTLM Hash** – MD4 dari password yang di encode UTF-16LE, 32-character hexadecimal string. Contoh: `31D6CFE0D16AE931B73C59D7E0C089C0` (hash dari password kosong).

### Common Tools & Pitfalls

* **hashid** – Identifier cepat buat banyak tipe hash, tapi bisa salah deteksi kalau hash nya custom atau terpotong.
* **hashcat** – Tool cracking yang powerful, setiap format hash punya mode spesifik (misal mode 1000 buat NTLM, mode 1800 buat sha512crypt).
* **John the Ripper** – Tool cracking lain pakai deteksi format bawaan.
* **Pitfall:** Mengandalkan prefix saja bisa misleading, beberapa aplikasi menyimpan raw SHA-256 atau MD5 tanpa prefix.

---

## Password Cracking

**Konsep:**
Hashing itu fungsi satu arah (one-way). Kita tidak bisa "mendekripsi" hash.

* **Cracking = Guessing:** Attacker mengambil daftar kemungkinan password (wordlist), menghitung hash satu per satu, terus membandingkan hasilnya dengan hash target.

**Tools Populer:**

1.  **Hashcat:** Tool cracking tercepet (berbasis GPU).
* Contoh Command (SHA256): `hashcat -m 1400 -a 0 hash.txt wordlist.txt`
2.  **John the Ripper (JtR):** Tool klasik, fleksibel (berbasis CPU/GPU).
* Contoh Command: `john --format=raw-sha256 hash.txt --wordlist=rockyou.txt`

**Wordlists:**

* **rockyou.txt:** Wordlist paling legendaris, berisi 14 juta password yang bocor dari situs RockYou (2009). Hampir semua CTF pemula pakai ini.

* **SecLists:** Kumpulan wordlist terbesar dan terlengkap, berisi jutaan password, username, dan data sensitif lainnya yang bocor dari berbagai sumber.

---

## Hashing for Integrity Checking

**Goal:** Memastikan file asli dan tidak dimodifikasi (tampered).

**Checksum:**
Nilai hash dari sebuah file disebut **Checksum**.

* Kalau download file `installer.exe` dari internet, website biasanya mencantumkan checksum (misal SHA256).
* Setelah download, user menghash file lokal. Kalau hash-nya sama persis, berarti file **aman & tidak corrupt**.
* Beda 1 bit saja di file = Hash berubah total.

**HMAC (Keyed-Hash Message Authentication Code):**
Hashing standar cuma menjamin **Integritas** (file tidak berubah), tapi tidak menjamin **Autentikasi** (siapa pengirimnya).

* **HMAC = Hash + Secret Key.**
* Cuma orang yang punya Key yang bisa membuat HMAC yang valid.
* Menjamin **Integrity** DAN **Authenticity**.

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

* **Hashing:** Verifikasi password di database, cek file corrupt.
* **Encoding:** Mengirim binary file lewat email (Base64), URL encoding.
* **Encryption:** Mengirim pesan rahasia (WA/Signal), HTTPS.

---

## Attack Flow Awareness

Hashing muncul di banyak titik dalam _attack chain_, baik dari sisi attacker maupun defender:

| Tahap | Peran Hashing |
| ----- | ------------- |
| **Post-Exploitation** | Setelah masuk ke sistem, attacker mencari file password (contoh: `/etc/shadow` di Linux, `SAM` di Windows) yang berisi hash credentials |
| **Credential Access** | Attacker meng-_crack_ hash yang didapat untuk mendapatkan password plaintext |
| **Lateral Movement** | Password yang berhasil di-crack bisa dipakai untuk login ke sistem lain di jaringan (password reuse) |
| **Defense (Integrity)** | Blue Team menggunakan hash checksum untuk mendeteksi file yang dimodifikasi oleh malware |

**Teknik Attacker:**
* **Pass-the-Hash (PtH):** Di Windows, attacker bisa menggunakan NTLM hash **tanpa perlu** meng-crack-nya jadi plaintext. Cukup hash-nya saja untuk melakukan autentikasi.
* **Kerberoasting:** Meng-request service ticket dari Active Directory lalu meng-_crack_ hash-nya secara offline.

---

## For Real World Relevance

* **Data Breaches:** Hampir setiap data breach besar melibatkan hash cracking. Jika organisasi menggunakan hashing yang lemah (MD5 tanpa salt), jutaan password bisa di-crack dalam hitungan jam menggunakan GPU modern.
* **Forensics:** Investigator menggunakan hash checksum untuk memverifikasi integritas barang bukti digital. Satu perubahan kecil di file bukti akan mengubah hash-nya dan membatalkan bukti di pengadilan.
* **Password Policy:** Standar industri sekarang mengharuskan penggunaan **bcrypt** atau **Argon2** (bukan MD5/SHA1) karena keduanya sengaja dirancang lambat untuk mempersulit brute force.

---
---

## Questions

1. Apa perbedaan utama antara **Hashing** dan **Encoding** dalam hal arah prosesnya (reversible)?
2. Kenapa penggunaan **Salt** sangat krusial untuk melindungi password di database?
3. Di tahap mana (*attack chain*) seorang penyerang biasanya melakukan *hash cracking*?
