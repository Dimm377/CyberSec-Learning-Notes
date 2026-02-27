# TryHackMe: Cryptography Basic


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/cryptographybasics)
- **Category:** Cryptography / Security
- **Difficulty:** Easy

---

## Introduction

Room ini yang pertama dari tiga room pengantar tentang cryptography. Di sini kita bakal belajar dasar-dasar yang penting sebelum masuk ke penjelasan yang lebih kompleks.

**Materi yang bakal dipelajari:**

- Istilah-istilah penting dalam cryptography
- Pentingnya cryptography di dunia digital
- Mengenal Caesar Cipher dan algoritma historis lainnya
- Standar cipher simetris (AES, DES, 3DES)
- Cipher asimetris umum
- Matematika dasar yang sering dipake di kriptografi (XOR & Modulo)

## Importance of Cryptography

Cryptography itu praktek dan studi tentang teknik komunikasi yang aman dan perlindungan data di mana ada pihak ketiga (lawan). Intinya, lawan tidak boleh sampai melihat (disclose) atau mengubah (alter) isi pesan tersebut.

Cryptography jadi vital di era digital karena jamin tiga hal utama:

1. **Confidentiality (Kerahasiaan):** Cuma orang yang berhak yang bisa baca datanya.
2. **Integrity (Integritas):** Memastikan data tidak berubah pas lagi dikirim.
3. **Authenticity (Keaslian):** Memastikan kalau pengirim data emang beneran orang yang dia klaim.

Selain itu, cryptography juga dipake buat memenuhi standar regulasi internasional.

**Answer the questions below:**

- **Question:** What is the standard required for handling credit card information?
- **Answer:** ?

## Plaintext to Ciphertext

Di sini kita belajar gimana data berubah bentuk dari yang bisa dibaca manusia "plaintext" jadi sebuah kode rahasia "ciphertext".

**Key Value:**

- **Plaintext:** Data asli atau pesan sebelum dienkripsi. Pesan ini masih bisa dibaca sama siapa saja (readable), misalnya teks "hello", foto pribadi, informasi penting, dll.
- **Ciphertext:** Hasil dari proses enkripsi. Pesan ini kelihatan berantakan dan tidak punya arti buat siapa pun yang tidak punya kuncinya (unreadable).
- **Encryption (Enkripsi):** Proses mengubah **Plaintext** jadi **Ciphertext** pakai algoritma dan kunci tertentu.
- **Decryption (Dekripsi):** Proses kebalikannya, yaitu mengubah **Ciphertext** balik jadi **Plaintext** agar bisa dibaca lagi.
- **Key (Kunci):** Informasi yang dipake sama algoritma kriptografi buat membuat proses enkripsi/dekripsi jadi unik.

<p align="center">
<img src="../../Assets/Images/chiper.png" alt="Cipher">
</p>

**Answer the questions below:**

- **Question:** What do you call the encrypted plaintext?
- **Answer:** ?

- **Question:** What is the process of turning plaintext into ciphertext called?
- **Answer:** ?

- **Question:** What do you call the process that returns the plaintext?
- **Answer:** ?

## Historical Cipher

Ini membahas metode enkripsi kuno yang jadi dasar buat cryptography modern. Meskipun sekarang dianggap lemah, mengerti logika di baliknya penting banget.

### 1. Caesar Cipher

Ini salah satu teknik paling tua dan paling simpel.

- **Cara Kerja:** Lakuin pergeseran (shift) setiap huruf dalam alfabet pakai jumlah tertentu.
- **Contoh:** Dengan pergeseran 3 (Shift 3), huruf 'A' bakal jadi 'D', 'B' jadi 'E', dan seterusnya.
- **ROT13:** Varian populer dari Caesar Cipher yang nggeser huruf sebanyak 13 posisi. Karena alfabet ada 26 huruf, jalanin ROT13 dua kali di pesan yang sama bakal balikin ke teks asli.

<p align="center">
<img src="../../Assets/Images/Encryption.png" alt="Encryption">
<img src="../../Assets/Images/Decryption.png" alt="Decryption">
</p>

### 2. Vigenère Cipher

Metode ini lebih canggih karena pakai kata kunci (keyword) buat menentukan jumlah pergeseran.

- **Cara Kerja:** Disebut polyalphabetic substitution karena setiap huruf di plaintext bisa digeser pakai jumlah yang beda-beda tergantung huruf yang sesuai di keyword-nya.
- **Keunggulan:** Lebih susah dipecahin daripada Caesar Cipher karena frekuensi kemunculan hurufnya tidak berpola tetap.

### 3. Enigma Machine

Mesin enkripsi yang dipake tentara Jerman waktu Perang Dunia II.

- **Cara Kerja:** Pakai sistem rotor yang terus berputar setiap kali tombol ditekan, jadi satu huruf yang sama bisa berubah jadi huruf yang beda berkali-kali.
- **Sejarah:** Enigma dianggap mustahil dipecahin sampai akhirnya tim yang dipimpin Alan Turing berhasil mecahinnya, yang kemudian jadi cikal bakal komputer modern.

**Answer the questions below:**

- **Question:** What is the name of the popular Caesar cipher variant used for obscuring text online?
- **Answer:** ?

- **Question:** Which cipher uses a keyword to decide the alphabet shift?
- **Answer:** ?

- **Question:** What was the name of the machine the Germans used in World War 2 to encrypt messages?
- **Answer:** ?

- **Question:** Knowing that `Xld Hzhz Apntyel dlhte` was encrypted using Caesar Cipher, what is the original plaintext?
- **Answer:** ?
  > (Penjelasan: Bisa pakai tool online seperti [Cryptii](https://cryptii.com/pipes/caesar-cipher). Memasukkan ciphertext `Xld Hzhz Apntyel dlhte` dan pakai **Shift 11** ke arah kanan buat mendapatkan teks aslinya)\_

## Types of Encryption

Dalam kriptografi modern, ada dua kategori utama enkripsi: **Symmetric** (Simetris) dan **Asymmetric** (Asimetris).

### 1. Symmetric Encryption

Pakai **kunci yang sama** buat proses enkripsi dan dekripsi (disebut juga _private key cryptography_).

- **Kelemahan:** Tantangannya itu gimana caranya distribusiin kunci secara aman ke penerima.
- **Algoritma Utama:**
  - **DES (Data Encryption Standard):** Diadopsi tahun 1977, pakai kunci 56-bit. Sudah berhasil di-crack tahun 1999 dalam waktu kurang dari 24 jam.
  - **3DES (Triple DES):** Jalanin DES tiga kali. Kunci 168-bit (efektivitas 112-bit). Sudah mulai ditinggalin sejak 2019.
  - **AES (Advanced Encryption Standard):** Standar global saat ini (diadopsi tahun **2001**). Pakai kunci 128, 192, atau 256 bit. Aman banget dan efisien.

<p align="center">
<img src="../../Assets/Images/Symmetric.png" alt="Symmetric">
</p>

### 2. Asymmetric Encryption

Pakai **sepasang kunci**: _Public Key_ (buat mengenkripsi/mengunci) dan _Private Key_ (buat mendekripsi/membuka).

- **Konsep:** Cuma pemilik _Private Key_ yang bisa buka data yang dikunci pakai _Public Key_-nya.
- **Algoritma Utama:**
  - **RSA:** Pakai problem matematika faktorisasi bilangan prima besar. Rekomendasi kunci minimal 2048-bit.
  - **Diffie-Hellman:** Dipake buat pertukaran kunci secara aman.
  - **ECC (Elliptic Curve Cryptography):** Lebih efisien; kunci 256-bit ECC setara kekuatannya sama RSA 3072-bit.

<p align="center">
<img src="../../Assets/Images/Asymmetric.png" alt="Asymmetric">
</p>

---

**Answer the questions below:**

- **Question:** Should you trust DES? (Yea/Nay)
- **Answer:** ?

  > (Penjelasan: Karena panjang kuncinya cuma 56-bit, DES sudah tidak aman lagi terhadap serangan brute force pakai komputasi modern.)\_

- **Question:** When was AES adopted as an encryption standard?
- **Answer:** ?

- **Question:** What is the name of the cipher that uses 3 keys and 3 execution stages?
- **Answer:** ?

- **Question:** Which encryption standard uses the same key to encrypt and decrypt?
- **Answer:** ?
## Basic Math

Kriptografi modern dibangun di atas fondasi matematika. Task ini membahas dua operasi logika dan aritmatika yang paling sering muncul dalam algoritma keamanan.

### 1. XOR Operation (Exclusive OR)

XOR itu operasi logika biner yang ngebandingin dua bit.

- **Aturan Main:** Ngasilin **1** kalau kedua bit beda, dan **0** kalau kedua bit sama.
- **Simbol:** $\oplus$ atau `^`.

**Truth Table XOR:**
| A | B | A $\oplus$ B |
| :--- | :--- | :--- |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

- **Kegunaan di Kriptografi:** XOR sering dipake sebagai algoritma enkripsi simetris sederhana. Kalau kita punya Plaintext ($P$) dan Secret Key ($K$), maka Ciphertext ($C$) itu $P \oplus K = C$. Buat mendapatkan balik Plaintext, tinggal lakuin $C \oplus K = P$.

### 2. Modulo Operation (%)

Modulo itu operasi buat mencari **sisa bagi** dari sebuah pembagian.

- **Contoh:** $23 \pmod 6 = 5$ (karena 23 dibagi 6 itu 3 dengan sisa 5).
- **Sifat Penting:** Modulo tidak bisa dibalik (_not reversible_). Kalau $x \pmod 5 = 4$, nilai $x$ bisa berupa 4, 9, 14, dan seterusnya sampai tidak terhingga.

---

**Answer the questions below:**

- **Question:** What's 1001 $\oplus$ 1010?
- **Answer:** ?
  _(Cara Hitung: 1$\oplus$1=0, 0$\oplus$0=0, 0$\oplus$1=1, 1$\oplus$0=1)_

- **Question:** What's 118613842 % 9091?
- **Answer:** ?
  _(Cara Hitung: Kamu bisa pakai Python di terminal Kitty kamu: `python3 -c "print(118613842 % 9091)"`)_

- **Question:** What's 60 % 12?
- **Answer:** ?
  _(Cara Hitung: 60 habis dibagi 12, jadi sisanya adalah 0)_
